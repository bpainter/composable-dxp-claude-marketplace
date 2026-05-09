---
name: bynder-webhooks-events
description: Bynder webhook configuration and asset-event handling — subscribing to asset.created/updated/deleted/archived and metaproperty events, HMAC signature verification, retry behavior, payload patterns (lookup-then-fetch since payloads are minimal), idempotent receivers, and the downstream-cache invalidation patterns that make Bynder + Contentful and Bynder + custom-front-end feel real-time. Use this skill when wiring up webhook receivers for a new Bynder integration, debugging webhook deliveries that aren't arriving or aren't being processed, designing the cache-invalidation strategy for a downstream system (CMS, search index, CDN), or fixing an existing webhook setup that's flaky.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-webhooks-events]
tags: [type/skill, plugin/bynder, topic/bynder, topic/webhooks, topic/events, topic/sync]
status: active
---

# Webhooks and Asset Events (Bynder downstream sync)

This skill puts you in the role of the engineer who designs and debugs the eventing layer between Bynder and everything downstream. Default posture: webhooks are how you make the integration feel real-time. The OOTB Marketplace connectors don't wire them for you (mostly); you do.

Bynder webhooks fire on asset and metaproperty events. The payload is intentionally minimal (just IDs + event type) — the receiver is expected to re-fetch current state via API. The eventing model is HMAC-signed, retried on failure, and configurable per environment. That's the whole substrate; the discipline is the patterns that turn it into reliable downstream behavior.

Pair with `bynder-js-sdk` (the receiver re-fetches via SDK), `bynder-contentful-pairing` (the most common downstream consumer), `bynder-asset-model` (metaproperty events), and `bynder-optimization-audit` (broken webhook plumbing is one of the audit's primary findings).

## When to use this skill

- Setting up webhook receivers for a new Bynder integration.
- Debugging webhooks that aren't arriving or aren't being processed.
- Designing cache-invalidation strategy for a downstream system (CMS, search index, CDN).
- Fixing a flaky existing webhook setup.
- Adding a new event subscription (e.g., metaproperty changes that need to cascade).
- Designing idempotent receivers that handle retries correctly.

## Core posture

- **Webhooks make the integration feel real-time.** Without them, downstream systems serve stale assets until manual refresh.
- **Payloads are minimal; receivers re-fetch.** Don't try to reconstruct asset state from the payload — fetch via API.
- **Verify HMAC every time.** Spoofable otherwise. No exceptions.
- **Make receivers idempotent.** Bynder retries; same event will arrive multiple times.
- **Acknowledge fast; process async.** Bynder's retry timer is short. Receive → enqueue → respond 200 in milliseconds; do the work in a background job.
- **Subscribe narrowly.** Only events you actually act on. A webhook subscribing to every asset event in a 100K-asset bank is a noise-and-cost generator.
- **Document the event → action map.** Future-you thanks present-you when something breaks.

## Events to know

| Event | When it fires | Common downstream action |
|---|---|---|
| `asset.created` | New asset uploaded and saved | Add to search index, add to CMS asset cache |
| `asset.updated` | Metadata changed, file replaced, derivative re-rendered | Invalidate caches in Contentful entries, CDN, search index |
| `asset.deleted` | Asset removed permanently | Remove from caches; surface clearly to editors who reference it |
| `asset.archived` | Asset archived (soft-delete) | Remove from active caches but retain reference for restoration |
| `asset.published` | Asset moved to public state (in plans that support it) | Promote to public CDN paths |
| `asset.unpublished` | Asset moved back to private | Demote / invalidate public references |
| `metaproperty.created` | New metaproperty added | Refresh metaproperty cache in any custom integration |
| `metaproperty.updated` | Metaproperty config changed (rename, scope, options) | Re-fetch affected assets; refresh integration UIs |
| `metaproperty.deleted` | Metaproperty removed | Remove from integration UIs; alert if active integrations depend on it |
| `metaproperty_option.created` | New option added (e.g., new campaign value) | Refresh option-list caches |
| `metaproperty_option.updated` | Option renamed | Cascade to any cached option name |
| `metaproperty_option.deleted` | Option removed | Cascade to filter UIs; flag assets referencing it |
| `collection.created` / `collection.updated` / `collection.deleted` | Collection lifecycle | Refresh collection-driven caches |

For the canonical list, check https://developers.bynder.com/webhooks/.

## The receiver pattern

### Skeleton (Next.js / Node)

```ts
// app/api/webhooks/bynder/route.ts
import { verifyBynderSignature } from '@/lib/bynder/webhook';
import { enqueueJob } from '@/lib/queue';

export async function POST(req: Request) {
  const body = await req.text();
  const signature = req.headers.get('x-bynder-signature');

  // 1. Verify HMAC (always, no exceptions)
  if (!verifyBynderSignature(body, signature, process.env.BYNDER_WEBHOOK_SECRET!)) {
    return new Response('invalid signature', { status: 401 });
  }

  // 2. Parse and acknowledge fast
  const event = JSON.parse(body);

  // 3. Enqueue async work
  await enqueueJob('bynder-event', event);

  // 4. Respond immediately
  return new Response('ok', { status: 200 });
}
```

The receiver does three things: verify, enqueue, ack. The processing happens in a worker.

### HMAC verification

```ts
import { createHmac, timingSafeEqual } from 'node:crypto';

export function verifyBynderSignature(
  body: string,
  signature: string | null,
  secret: string,
): boolean {
  if (!signature) return false;
  const computed = createHmac('sha256', secret).update(body).digest('hex');
  // timingSafeEqual prevents timing attacks
  try {
    return timingSafeEqual(Buffer.from(computed), Buffer.from(signature));
  } catch {
    return false;
  }
}
```

The exact header name (`x-bynder-signature` vs. `x-bynder-hmac` vs. similar) and algorithm depend on Bynder's webhook config. Confirm against current docs; the verification *pattern* is universal.

### The worker

```ts
// workers/bynder-event-worker.ts
import { bynder } from '@/lib/bynder/client';

export async function processBynderEvent(event: BynderEvent) {
  const seen = await haveISeenThisEvent(event.eventId);
  if (seen) return; // idempotent — drop duplicates

  switch (event.event) {
    case 'asset.created':
    case 'asset.updated': {
      const asset = await bynder.getAsset(event.assetId);
      await refreshAssetCache(asset); // CMS, search, CDN
      break;
    }
    case 'asset.deleted':
    case 'asset.archived': {
      await invalidateAssetCache(event.assetId);
      await flagAffectedEntries(event.assetId);
      break;
    }
    case 'metaproperty_option.updated': {
      const optionId = event.metapropertyOptionId;
      const affectedAssets = await bynder.listAssets({ propertyOptionId: [optionId] });
      for (const a of affectedAssets) {
        await refreshAssetCache(a);
      }
      break;
    }
    // ...
  }

  await markEventSeen(event.eventId);
}
```

Two patterns to internalize:

1. **Re-fetch, don't trust the payload.** Bynder events carry IDs; current state requires an API call.
2. **Idempotent dedup.** Bynder retries on failure. Same event ID will arrive multiple times. Track seen events (Redis set, DB unique constraint) to drop duplicates.

### Failure handling

```ts
try {
  await processBynderEvent(event);
} catch (err) {
  if (isTransient(err)) {
    // throw to let the queue retry (typically 5–15 attempts with exponential backoff)
    throw err;
  } else {
    // permanent failure — log, alert, dead-letter
    await deadLetter(event, err);
  }
}
```

Distinguish transient (network, 5xx, 429) from permanent (bad data, asset deleted, schema mismatch). Don't infinitely retry permanent failures.

## Configuring webhooks in Bynder

Two places:

1. **Portal admin UI** — most common. Settings → Integrations → Webhooks. Configure URL, events, signing secret, environment scope.
2. **API** — programmatic; useful for IaC. Less commonly used.

Per webhook configuration:

| Setting | Notes |
|---|---|
| URL | The receiver endpoint. HTTPS only; Bynder won't deliver to HTTP. |
| Events | Pick narrowly. `asset.*` is too broad if you only care about updates. |
| Filter | Where supported, scope by metaproperty value (e.g., only assets with `property_Brand=Acme`). |
| Signing secret | Stored in receiver as `BYNDER_WEBHOOK_SECRET`. Rotate quarterly. |
| Active flag | Toggle on/off without deleting — useful for maintenance windows. |

### Secret rotation

```
1. Generate new secret in Bynder portal.
2. Receiver must accept BOTH old and new secret for the rotation window.
3. Cut over Bynder to the new secret.
4. Confirm receiver is processing under new secret.
5. Remove old secret from receiver.
```

Rotate quarterly or on staff-change.

## Cache invalidation patterns

The most common downstream consumer of Bynder webhooks is a CMS or front-end cache. Patterns:

### Pattern 1: Direct invalidation

Receiver hits the CMS / front-end revalidate endpoint:

```ts
// asset.updated
await fetch(`${FRONTEND_URL}/api/revalidate?tag=asset:${event.assetId}`, {
  method: 'POST',
  headers: { Authorization: `Bearer ${REVALIDATE_TOKEN}` },
});
```

The front end (Next.js with `revalidateTag`) handles re-fetching on next request.

### Pattern 2: External index lookup, then invalidate

Receiver looks up which CMS entries reference the asset, then invalidates those:

```ts
// asset.updated
const entryIds = await assetIndex.lookup(event.assetId);
for (const entryId of entryIds) {
  await fetch(`${FRONTEND_URL}/api/revalidate?tag=entry:${entryId}`, { /* ... */ });
}
```

The external index is maintained from Contentful publish events plus a one-time scan.

### Pattern 3: CMS update + revalidate

Receiver fetches updated asset, updates the cached derivative URLs in the CMS entry's JSON field, saves the entry (don't publish — let editors review):

```ts
const asset = await bynder.getAsset(event.assetId);
const entries = await contentful.searchEntries({ /* by assetId in any json field */ });
for (const entry of entries) {
  entry.fields.heroAsset['en-US'].derivatives = {
    hero: asset.thumbnails.web_hero_lg,
    card: asset.thumbnails.web_card_md,
  };
  await contentful.updateEntry(entry); // saves draft, doesn't publish
  await revalidate(`entry:${entry.sys.id}`);
}
```

This preserves the editor workflow — they see the updated asset in preview, decide whether to re-publish.

Slalom default for Bynder + Contentful: pattern 2 + pattern 3 in combination. Pattern 2 keeps front-end cache fresh; pattern 3 keeps the CMS entry's stored derivatives current so editors never see "this URL doesn't work anymore."

## Output formats

### Webhook integration plan

```
# Bynder Webhook Integration: [System / Use case]

## Events subscribed
| Event | Why | Downstream action |
| ... | ... | ... |

## Receiver architecture
- Endpoint: [URL]
- Hosting: [Vercel function / Lambda / container]
- Queue: [Vercel Queue / SQS / Inngest / etc.]
- Worker: [hosted where]
- Idempotency store: [Redis / DB unique constraint]

## HMAC verification
- Secret storage: [secret manager]
- Rotation cadence: [quarterly / semi-annual]
- Old/new acceptance window: [days]

## Cache invalidation pattern
[Direct invalidation | external index | CMS update + revalidate | combination]

## Error handling
- Transient retries: [N attempts, exponential backoff]
- Permanent failure: [dead-letter to where; alert to whom]
- Replay: [how a dead-letter is replayed manually]

## Audit / observability
- Logging: [where event logs live]
- Metrics: [delivery rate, processing time, error rate]
- Alerts: [thresholds, on-call routing]

## Test plan
- Synthetic event: [how to fire one]
- Real event: [how to verify end-to-end]
- HMAC failure: [verified rejection]
```

## Common anti-patterns to call out

- **No HMAC verification.** Spoofable; treat as a security incident if found.
- **Synchronous processing in the receiver.** Long-running work in the request handler causes timeouts → Bynder retries → duplicate processing. Receive → enqueue → 200.
- **Trusting the payload.** Bynder payloads are minimal; receiver must re-fetch.
- **No idempotency.** Retries duplicate work; bad if the work is not idempotent (e.g., metric increments, outbound notifications).
- **Subscribing to everything.** Noise; cost; debugging hell.
- **Single environment for webhooks.** Production webhook firing into staging receiver, or vice versa. Confirm environment per webhook.
- **No secret rotation.** Compromise risk grows over time.
- **Webhook receiver in the same process as the front-end cache.** Coupled; a bad event can affect rendering.
- **No dead-letter for permanent failures.** Errors silently retried forever; nobody sees them.
- **No observability.** When delivery fails, nobody knows until editors notice stale assets.

## Constraints

- Do not skip HMAC verification.
- Do not process synchronously in the request handler.
- Do not subscribe to events the receiver doesn't act on.
- Do not store webhook secrets in code or repos.
- Do not use HTTP endpoints — Bynder requires HTTPS.
- Do not assume one webhook per integration; many integrations need ≥ 2 (asset events + metaproperty events).
- Do not replay events without confirming idempotency works.

## How this skill relates to others

- The SDK calls the receiver makes to re-fetch → `bynder-js-sdk`.
- The downstream Contentful integration that consumes events → `bynder-contentful-pairing`.
- The metaproperty events that signal taxonomy changes → `bynder-asset-model`.
- The optimization-audit signal "asset updates don't propagate" → `bynder-optimization-audit`.
- Multi-environment webhook routing → `bynder-portal-architect`.

## Source material

- Bynder webhooks docs: https://developers.bynder.com/webhooks/
- API reference (webhook config): https://api.bynder.com/docs/ (Webhooks section)
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
- Sister plugin: `../contentful/skills/contentful-webhooks/SKILL.md`
