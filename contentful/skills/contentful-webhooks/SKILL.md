---
name: contentful-webhooks
description: Configure and consume Contentful webhooks correctly — content events, environment scoping, HMAC signing, header customization, retries, idempotent receivers, and the canonical revalidation handler for a Next.js front end. Use this skill any time webhooks are being set up or fixed; when "I published in Contentful and the site didn't update" comes up; when secret-signing is missing; when a third-party integration (Algolia, search, downstream cache) needs to react to publish events; or when the team is debating webhooks vs. polling vs. scheduled rebuilds. Trigger whenever the question is how Contentful tells the rest of the world that something changed.

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-webhooks]
tags: [type/skill, plugin/contentful, topic/contentful, topic/webhooks]
status: active
---

# Contentful Webhooks

Webhooks are Contentful's primary outbound integration mechanism. Every Slalom Composable DXP build assumes signed, environment-scoped webhooks driving on-demand revalidation, downstream sync, and audit trails. Time-based revalidation is the safety net, not the strategy.

This skill owns webhook configuration and consumption. Pair with `contentful-react-wrapper` (the revalidation handler), `contentful-space-architect` (per-environment configuration), `contentful-delivery-optimization` (cache strategy), and `contentful-migrations` (webhooks frequently change alongside schema).

## When to use this skill

- Standing up webhooks for a new Contentful environment.
- The front-end isn't revalidating after publish.
- Adding a downstream consumer (Algolia indexer, search warmup, audit log).
- Securing an existing unsigned webhook.
- Debugging retries / failed deliveries.
- Adjusting filters so a noisy webhook calms down.

## Core posture

- **Sign every webhook.** Anyone can hit a public revalidation endpoint and trigger work. HMAC verify on receipt; reject everything else.
- **Filter aggressively at the source.** Webhook traffic is cheap until it isn't; firing on every save event cascades into rebuild storms. Filter on content type and event.
- **Idempotent receivers.** Retries are normal. Your handler must produce the same outcome on a duplicate delivery.
- **One webhook per concern.** Don't multiplex — a "do everything" handler is harder to debug than three small ones.
- **Per-environment.** A webhook configured in one environment does not automatically follow when you create a new release env. Verify on every alias swap.

## Anatomy of a webhook

In Contentful, a webhook has:

- **Name** — human-readable identifier.
- **URL** — your handler endpoint.
- **Triggers** — which content events fire (`Entry.publish`, `Entry.unpublish`, `Asset.delete`, etc.).
- **Filters** — content type, environment, tags, custom expressions over the payload.
- **HTTP headers** — custom static headers, plus a built-in HMAC signature option.
- **Payload** — default Contentful entry envelope, or a custom transformer.
- **Active** — toggle.

## Content events

The full event surface ([source](https://www.contentful.com/developers/docs/webhooks/content-events/)):

| Group | Events |
|---|---|
| Entry | `Entry.create`, `Entry.save`, `Entry.auto_save`, `Entry.publish`, `Entry.unpublish`, `Entry.archive`, `Entry.unarchive`, `Entry.delete` |
| Asset | `Asset.create`, `Asset.save`, `Asset.auto_save`, `Asset.publish`, `Asset.unpublish`, `Asset.archive`, `Asset.unarchive`, `Asset.delete` |
| ContentType | `ContentType.create`, `ContentType.save`, `ContentType.publish`, `ContentType.unpublish`, `ContentType.delete` |
| Tag | `Tag.create`, `Tag.save`, `Tag.delete` |
| Comment | `Comment.create`, `Comment.delete` |
| Task | `Task.create`, `Task.save`, `Task.delete` |
| Release | `Release.create`, `Release.save`, `Release.execute`, `Release.delete` |
| AppEvent | `AppEvent.create` (App Framework) |

For revalidation you typically subscribe to `Entry.publish`, `Entry.unpublish`, `Asset.publish`, `Asset.unpublish`, and (if you render content type metadata) `ContentType.publish`. `Entry.auto_save` fires on every keystroke pause; do not subscribe unless you have a specific reason.

## Filters

Filters narrow which events make it past the source. Common patterns:

- **Filter by environment.** `sys.environment.sys.id == "master"`. Critical when you have multiple environments and don't want dev/staging events polluting production handlers.
- **Filter by content type.** `sys.contentType.sys.id == "article"`.
- **Filter by tag.** `metadata.tags.sys.id in ["public"]`.
- **Filter by custom field.** `fields.publishStatus.en-US == "approved"` (when using a custom workflow).
- **Combine filters with `and` / `or`.** Available in the UI and CMA.

For Slalom defaults: every production-targeted webhook filters on `sys.environment.sys.id`. A webhook configured for the `master` alias should not fire when staging publishes.

## Signing webhooks (HMAC)

Contentful supports built-in HMAC signing. Configure a secret in the webhook config; Contentful signs each request with it.

Signature headers:

- `x-contentful-signature` — HMAC-SHA256 of the body.
- `x-contentful-signed-headers` — comma-separated list of headers included in the signature.
- `x-contentful-timestamp` — request timestamp.

Verification (Node, with timestamp window check):

```ts
import crypto from "node:crypto";

function verifyContentfulSignature(
  rawBody: string,
  signature: string | null,
  timestamp: string | null,
  signedHeaders: string | null,
  request: Request,
  secret: string,
  toleranceSeconds = 300,
): boolean {
  if (!signature || !timestamp || !signedHeaders) return false;

  // Replay protection
  const now = Math.floor(Date.now() / 1000);
  const ts = Number(timestamp);
  if (!Number.isFinite(ts) || Math.abs(now - ts) > toleranceSeconds) return false;

  // Reconstruct the canonical signed string
  const headers = signedHeaders.split(",").map((h) => h.trim());
  const headerLines = headers.map((h) => `${h}:${request.headers.get(h) ?? ""}`).join("\n");
  const canonical = [headerLines, rawBody, timestamp].join("\n");

  const expected = crypto.createHmac("sha256", secret).update(canonical).digest("hex");
  return crypto.timingSafeEqual(Buffer.from(expected), Buffer.from(signature));
}
```

Always use `timingSafeEqual`. Plain `===` leaks via timing.

If a project predates the built-in HMAC, you can add a custom header containing your own HMAC by using the webhook's "Custom headers" feature — sign client-side with a shared secret. Same idea, slightly more setup.

## The canonical revalidation handler (Next.js)

```ts
// app/api/contentful/revalidate/route.ts
import { revalidateTag } from "next/cache";
import { NextRequest, NextResponse } from "next/server";
import { verifyContentfulSignature } from "@/lib/contentful/verify-webhook";

export async function POST(req: NextRequest) {
  const rawBody = await req.text();
  const ok = verifyContentfulSignature(
    rawBody,
    req.headers.get("x-contentful-signature"),
    req.headers.get("x-contentful-timestamp"),
    req.headers.get("x-contentful-signed-headers"),
    req,
    process.env.CONTENTFUL_REVALIDATE_SECRET!,
  );
  if (!ok) return NextResponse.json({ error: "invalid signature" }, { status: 401 });

  const payload = JSON.parse(rawBody);
  const contentType = payload.sys?.contentType?.sys?.id;
  const id = payload.sys?.id;
  const topic = req.headers.get("x-contentful-topic") ?? ""; // e.g., "ContentManagement.Entry.publish"

  if (contentType && id) {
    revalidateTag(`${contentType}:${id}`);
    revalidateTag(contentType); // aggregate
  }

  // Optional: route to downstream consumers
  await Promise.allSettled([
    enqueueAlgoliaSync({ contentType, id, topic, payload }),
    enqueueAuditLog({ topic, payload }),
  ]);

  return NextResponse.json({ ok: true });
}
```

Things to internalize:

- Read the body as **text first**, then JSON-parse; the signature covers the raw body.
- **Always 200/4xx fast.** Long-running work goes to a queue (Vercel Queue, BullMQ, SQS). Contentful retries on 5xx.
- Use **`Promise.allSettled`** for fan-out so one failed downstream doesn't fail the whole webhook.

## Retry behavior

Contentful retries failed deliveries with exponential backoff for ~24 hours. Implications:

- A 5xx from your handler triggers a retry. Make sure the receiver is idempotent.
- A 401 from signature verification *also* triggers retry. If the secret rotates, every queued event arrives 24h later when the new secret is in place. Plan secret rotation alongside webhook updates.
- The Webhook Calls log in the Contentful UI shows status, response, and retry count. Use it.

## Common webhook patterns

### Cache invalidation (the canonical one)

Above. Tag-based revalidation in Next.js, idempotent, signed.

### Search index sync (Algolia, Elasticsearch, Typesense)

```ts
// pseudocode
on Entry.publish:
  fetch full record from CDA via GraphQL (the webhook payload may not contain everything you index)
  upsert into search index
on Entry.unpublish, Entry.delete:
  delete from search index
on Asset events:
  usually no-op for search
```

Always re-fetch from the API rather than relying on the webhook payload to be complete. The payload is the published version *as published*; if your search record needs resolved references, you need to fetch.

### Static site rebuild trigger (alternative to revalidation)

If you're on a pure static-export front end (no Vercel ISR), webhook hits a build hook:

```yaml
# Contentful webhook
URL: https://api.vercel.com/v1/integrations/deploy/{deploy-hook-id}
Method: POST
Triggers: Entry.publish, Entry.unpublish, Asset.publish, Asset.unpublish
Filters: sys.environment.sys.id == "master"
```

Less granular than revalidation but bulletproof.

### Audit log

```ts
on any *.publish, *.unpublish, *.delete:
  append { timestamp, actor, contentType, id, topic } to audit store
```

Pairs with enterprise observability requirements.

### Personalization re-evaluation

When using `contentful-personalization`, audience or experience changes can warrant re-evaluating cached variant decisions. Webhook fires; edge cache is invalidated for affected paths.

## Configuring webhooks

Three ways:

1. **UI** — fastest for a one-off. Settings → Webhooks → Add webhook. Use for exploration.
2. **CMA via the `contentful-management` SDK** — for scripted, repeatable setup. Pairs with the migration discipline; webhook config lives in the repo.
3. **A migration script** — `contentful-migration` does not directly support webhooks, but a small Node script using `contentful-management` SDK works and can be checked in alongside migrations.

```ts
// scripts/setup-webhooks.ts
import { createClient } from "contentful-management";

const client = createClient({ accessToken: process.env.CONTENTFUL_MANAGEMENT_TOKEN! });
const space = await client.getSpace(process.env.CONTENTFUL_SPACE_ID!);

await space.createWebhookWithId("revalidate-master", {
  name: "revalidate (master)",
  url: "https://www.example.com/api/contentful/revalidate",
  topics: ["Entry.publish", "Entry.unpublish", "Asset.publish", "Asset.unpublish"],
  filters: [
    { equals: [{ doc: "sys.environment.sys.id" }, "master"] },
  ],
  headers: [],
  // HMAC signing
  transformation: undefined,
  secret: process.env.CONTENTFUL_REVALIDATE_SECRET!,
});
```

## Custom payload transformations

Contentful supports custom payload templates via the `transformation` field. Useful when:

- The downstream API expects a specific shape (a Slack incoming-webhook expects `{ text: "..." }`).
- You want to drop most of the body to keep the receiver lean.

For Slack notifications on publish:

```js
// transformation
{
  contentType: "application/json",
  body: JSON.stringify({
    text: `Published: ${'{ /payload/sys/contentType/sys/id }'} ${'{ /payload/sys/id }'}`,
  })
}
```

For most cache and search use cases, leave the default payload alone and parse on the receiver.

## Failure modes

- **Unsigned production webhook.** A trivial DOS vector and a leaky integration. Always sign.
- **No environment filter.** Dev edits revalidate prod. Always filter on `sys.environment.sys.id`.
- **Sync work in the handler.** A handler that writes to Algolia synchronously stalls 4–6 seconds; Contentful retries; fan-out cascades. Queue it.
- **Reading the body as JSON before verifying.** The signature covers the raw bytes; once you `req.json()` you can't recover them. Always `req.text()` first.
- **Non-idempotent receivers.** Retries duplicate work. Index upserts must be idempotent.
- **Webhook count creep.** Spaces hit configured webhook limits at scale; ten near-duplicates exist because nobody cleans up. Quarterly hygiene: list, delete dead webhooks.
- **Secret rotation without coordination.** Rotate the secret in Contentful before deploying the new one to the receiver, and you have 24h of failed deliveries. Deploy the receiver first (accept old AND new secret), then rotate, then retire the old.
- **Custom transformations that swallow useful fields.** When debugging, the standard envelope is much easier to read than your custom shape. Default to standard.

## How this skill relates to others

- The handler that consumes most webhooks → `contentful-react-wrapper`.
- Per-environment configuration → `contentful-space-architect`.
- Cache strategy webhooks support → `contentful-delivery-optimization`.
- Migration sequences that change webhook config alongside schema → `contentful-migrations`.
- App Framework AppEvents → `contentful-app-framework`.
- Personalization re-evaluation triggers → `contentful-personalization`.

## Source material

- Webhooks overview: https://www.contentful.com/developers/docs/webhooks/overview/
- Content events: https://www.contentful.com/developers/docs/webhooks/content-events/
- Configure webhook: https://www.contentful.com/developers/docs/webhooks/configure-webhook/
- contentful-management SDK: https://github.com/contentful/contentful-management.js
- Plugin reference: `../../references/api-surface.md`
