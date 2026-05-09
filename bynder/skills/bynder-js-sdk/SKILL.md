---
name: bynder-js-sdk
description: Practical patterns with @bynder/bynder-js-sdk — auth flow selection, asset list/search/get, chunked uploads, metadata updates, metaproperty management, error handling, retry, pagination, and falling back to plain fetch for surfaces the SDK doesn't yet wrap (Workflow, Analytics, Brand Guidelines). Use this skill any time the user is wiring a Node / TypeScript backend to Bynder, building a migration script, integrating Bynder with a CMS or PIM via API, or debugging an SDK behavior — including OAuth2 token refresh, rate-limit-driven retries, and shape-of-response inconsistencies between SDK versions.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-js-sdk]
tags: [type/skill, plugin/bynder, topic/bynder, topic/sdk, topic/api]
status: active
---

# Bynder JS SDK (backend integration patterns)

This skill puts you in the role of a senior backend engineer who's wired Bynder into half a dozen production stacks and knows where the SDK's edges are. Default posture: prefer the SDK for the surfaces it covers (Asset Bank), fall back to plain `fetch` for the surfaces it doesn't (Workflow, Analytics, Brand Guidelines), and wrap everything in retry / backoff / typed-error handling.

The official SDK is `@bynder/bynder-js-sdk` at https://github.com/Bynder/bynder-js-sdk. It wraps the OAuth2 token lifecycle, abstracts the multi-step chunked upload, and provides ergonomic helpers for the most-used Asset Bank endpoints. It does *not* wrap Workflow, Analytics, or Brand Guidelines as of writing — for those, hand-roll requests against the documented endpoints.

Pair with `bynder-asset-model` for the metaproperty schema your code reads/writes, `bynder-contentful-pairing` for the most common downstream consumer, `bynder-migration` for bulk import patterns, and `bynder-webhooks-events` for receivers that re-fetch via SDK.

For the quick-reference patterns, see `references/sdk-cheat-sheet.md`. This skill goes deeper on architecture and the failure modes.

## When to use this skill

- Wiring a Node / TypeScript backend to Bynder for the first time.
- Building a migration script that bulk-imports or bulk-updates assets.
- Integrating Bynder with a CMS, PIM, or commerce backend via API.
- Debugging SDK behavior — token refresh, upload chunking, response-shape variance.
- Implementing webhook receivers that re-fetch asset state.
- Picking between auth flows for a specific integration shape.

## Core posture

- **Pick the auth flow first; everything else follows.** Authorization Code for user-in-the-loop, Client Credentials for server-to-server, Permanent Token only for one-off scripts.
- **Wrap every SDK call.** Retry on 429 + 5xx, hard-fail on 4xx (except 429), never retry blindly on 401 (token problem, not network problem).
- **Treat the SDK as one tool in the box.** Workflow, Analytics, Brand Guidelines need plain `fetch`. Don't twist the SDK into something it isn't.
- **Stream what you can.** Large uploads buffered into memory destroy Node processes; use streaming where the SDK supports it.
- **Pin the version.** The SDK has had breaking changes; lock and upgrade deliberately.
- **Type everything.** The SDK's TypeScript types are partial; declare your own response interfaces for anything you depend on.

## Auth flow selection

| Integration shape | Use |
|---|---|
| Custom marketing app a marketer logs into | Authorization Code (3-legged) with `offline` scope for refresh |
| Server-to-server integration with Contentful, a PIM, or a backend | Client Credentials (2-legged) |
| Migration script run once during a launch | Permanent Token (rotate after) or Client Credentials |
| Webhook receiver that re-fetches | Client Credentials |
| Local dev / exploratory script | Permanent Token (your own user's PAT) |

The discipline: production never runs on a Permanent Token bound to a person. The integration outlives the person.

## Architecture pattern: the bynder client wrapper

Don't sprinkle SDK calls across application code. Wrap the SDK in a small adapter that:

1. Owns the auth flow + token lifecycle.
2. Adds retry + backoff.
3. Adds typed responses.
4. Provides escape hatches for surfaces the SDK doesn't cover.

```ts
// lib/bynder/client.ts
import Bynder from '@bynder/bynder-js-sdk';
import pRetry from 'p-retry';
import type { BynderAsset, BynderMetaproperty } from './types';

export class BynderClient {
  private sdk: Bynder;
  private accessToken?: string;
  private accessTokenExpires = 0;

  constructor(private opts: BynderClientOptions) {
    this.sdk = new Bynder({
      baseURL: opts.baseURL,
      clientId: opts.clientId,
      clientSecret: opts.clientSecret,
    });
  }

  /** Auth: client-credentials with cached token */
  private async ensureToken(): Promise<string> {
    if (this.accessToken && Date.now() < this.accessTokenExpires - 60_000) {
      return this.accessToken;
    }
    const res = await fetch(`${this.opts.baseURL}v6/authentication/oauth2/token`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
      body: new URLSearchParams({
        grant_type: 'client_credentials',
        client_id: this.opts.clientId,
        client_secret: this.opts.clientSecret,
        scope: this.opts.scopes.join(' '),
      }),
    });
    if (!res.ok) throw new Error(`Bynder auth failed: ${res.status}`);
    const json = await res.json();
    this.accessToken = json.access_token;
    this.accessTokenExpires = Date.now() + json.expires_in * 1000;
    return this.accessToken!;
  }

  /** Generic retry-wrapped call */
  private async call<T>(fn: () => Promise<T>): Promise<T> {
    return pRetry(fn, {
      retries: 5,
      factor: 2,
      minTimeout: 500,
      onFailedAttempt: (err: any) => {
        const status = err?.response?.status ?? err?.status;
        // retry only on 429 + 5xx
        if (status === 429 || (status >= 500 && status < 600)) return;
        throw err;
      },
    });
  }

  async listAssets(filter: AssetFilter = {}): Promise<BynderAsset[]> {
    return this.call(() => this.sdk.getMediaList({
      limit: 1000,
      ...filter,
    }));
  }

  async getAsset(id: string): Promise<BynderAsset> {
    return this.call(() => this.sdk.getMediaInfo({ id, versions: 1 }));
  }

  async uploadAsset(opts: UploadAssetOptions): Promise<{ mediaid: string }> {
    return this.call(() => this.sdk.uploadFile(opts));
  }

  /** Fallback to plain fetch for un-SDK'd surfaces */
  async rawRequest<T>(path: string, init: RequestInit = {}): Promise<T> {
    const token = await this.ensureToken();
    const res = await fetch(`${this.opts.baseURL.replace(/\/api\/$/, '')}${path}`, {
      ...init,
      headers: {
        ...init.headers,
        Authorization: `Bearer ${token}`,
      },
    });
    if (!res.ok) {
      const body = await res.text();
      const err: any = new Error(`Bynder ${res.status}: ${body}`);
      err.status = res.status;
      throw err;
    }
    return res.json() as Promise<T>;
  }

  /** Workflow API helper */
  async listCampaigns() {
    return this.rawRequest('/workflow/campaigns/');
  }
}
```

Application code now calls `bynder.listAssets({ propertyOptionId: [...] })` and never touches the SDK directly. This makes the SDK swappable, makes retry uniform, and makes the auth-flow choice explicit at construction time.

## Pagination patterns

Asset Bank list endpoints paginate by `limit + page`. Default `limit` is small (~50). For migrations and exports, raise to 1000 and async-iterate:

```ts
async function* allAssets(client: BynderClient, filter: AssetFilter = {}) {
  let page = 1;
  while (true) {
    const list = await client.listAssets({ ...filter, limit: 1000, page });
    if (!list || list.length === 0) return;
    yield* list;
    if (list.length < 1000) return;
    page++;
  }
}

for await (const asset of allAssets(client, { type: 'image' })) {
  // process
}
```

Some endpoints (especially `/api/v4/media/`) have variant response shapes — sometimes `{ items, page, total }`, sometimes a bare array. Inspect what your specific SDK + account actually returns and handle both cases.

## Upload patterns

Single-file:

```ts
const result = await client.uploadAsset({
  filename: 'Hero.jpg',
  body: fileBuffer,
  contentType: 'image/jpeg',
  data: {
    brandId: BRAND_ID,
    name: 'Hero — Composable 2026',
    metaproperty_AssetCategory: [METAPROP_OPTIONS.AssetCategory.Marketing],
    metaproperty_Channel: [METAPROP_OPTIONS.Channel.Web, METAPROP_OPTIONS.Channel.Social],
  },
});
```

Bulk migration upload (with throttling):

```ts
import pLimit from 'p-limit';

const limit = pLimit(5); // max 5 concurrent
const results = await Promise.allSettled(
  files.map(file => limit(() => client.uploadAsset({
    filename: file.name,
    body: file.stream,
    contentType: file.mime,
    data: mapMetadata(file),
  })))
);
```

Concurrency = 5 keeps you under the API write rate-limit headroom on most account tiers. Tune by watching for 429s.

## Metaproperty management

Metaproperty operations are slow and rate-limit-sensitive. Don't call them on every asset upload — cache option lookups.

```ts
// Build a name → UUID lookup once at startup
class MetapropertyCache {
  private cache = new Map<string, Map<string, string>>(); // metaprop name → option name → option UUID

  constructor(private client: BynderClient) {}

  async load() {
    const props = await this.client.rawRequest<BynderMetaproperty[]>('/api/v4/metaproperties/');
    for (const prop of props) {
      const optMap = new Map<string, string>();
      for (const opt of prop.options ?? []) {
        optMap.set(opt.name, opt.id);
      }
      this.cache.set(prop.name, optMap);
    }
  }

  optionId(metapropName: string, optionName: string): string {
    const m = this.cache.get(metapropName);
    if (!m) throw new Error(`Unknown metaproperty: ${metapropName}`);
    const id = m.get(optionName);
    if (!id) throw new Error(`Unknown option ${optionName} on ${metapropName}`);
    return id;
  }
}
```

Now upload code uses `metaCache.optionId('Channel', 'Web')` instead of looking it up repeatedly.

## Error handling

The SDK throws on non-2xx; the error object varies. Treat status codes as the contract:

| Status | Meaning | Action |
|---|---|---|
| 200/201/204 | Success | Continue |
| 400 | Bad request — usually a metaproperty option that doesn't exist or a missing required field | Hard fail with the request body for debugging |
| 401 | Token expired or insufficient scope | Refresh token (Authorization Code) or re-authenticate (Client Credentials) — once. Don't loop. |
| 403 | Permission profile blocks the operation | Hard fail; this is a permissions design problem |
| 404 | Resource doesn't exist | Could be deleted between fetch and update — handle gracefully |
| 409 | Conflict (rare on Bynder, more common on Contentful) | Re-fetch, retry once |
| 429 | Rate-limited | Backoff + retry (handled by `p-retry`) |
| 5xx | Server error | Backoff + retry |

Never retry 400/403/404 blindly — they indicate a logic problem, not a transient one.

## Webhook-receiver pattern

The receiver re-fetches asset state via SDK. Pattern:

```ts
// app/api/webhooks/bynder/route.ts
import { verifyBynderSignature } from '@/lib/bynder/webhook';
import { bynder } from '@/lib/bynder/client';

export async function POST(req: Request) {
  const body = await req.text();
  const signature = req.headers.get('x-bynder-signature');
  if (!verifyBynderSignature(body, signature, process.env.BYNDER_WEBHOOK_SECRET!)) {
    return new Response('invalid signature', { status: 401 });
  }
  const event = JSON.parse(body);

  switch (event.event) {
    case 'asset.updated':
    case 'asset.created':
      const asset = await bynder.getAsset(event.assetId);
      await refreshDownstreamCache(asset);
      break;
    case 'asset.deleted':
      await invalidateDownstreamCache(event.assetId);
      break;
    case 'metaproperty.updated':
    case 'metaproperty_option.updated':
      // potentially cascading; re-fetch all assets that reference this option
      await refreshAffectedAssets(event);
      break;
  }
  return new Response('ok');
}
```

Always verify HMAC before doing work. See `bynder-webhooks-events`.

## Falling back to plain fetch

For Workflow, Analytics, Brand Guidelines — anything the SDK doesn't wrap — use `client.rawRequest`:

```ts
const campaigns = await client.rawRequest<{ campaigns: Campaign[] }>('/workflow/campaigns/');
const analytics = await client.rawRequest<UsageReport>('/api/v4/analytics/assets/?from=2026-01-01&to=2026-03-31');
const brandGuide = await client.rawRequest<BrandGuidelinesSection[]>('/brandguidelines/sections/');
```

Type the responses yourself; don't expect the SDK to evolve to cover them on your timeline.

## Output formats

### Integration plan

```
# Bynder Integration Plan: [System]

## Auth flow
[Authorization Code | Client Credentials | Permanent Token — and why.]

## Operations
- Read: [list endpoints, pagination strategy]
- Write: [list endpoints, throttling strategy]

## Metaproperty cache strategy
[Build at startup | per-request | none.]

## Retry policy
[On which statuses, with what backoff, with what max attempts.]

## Error escalation
[What user-visible error vs. silent log vs. dead-letter queue.]

## Webhook integration (if any)
[Events subscribed, downstream effects, idempotency strategy.]

## Token rotation plan
[How keys are stored, rotated, revoked.]
```

## Common anti-patterns to call out

- **SDK calls scattered across application code.** Wrap.
- **Permanent Token in production.** Use Client Credentials.
- **Optimistic-no-retry.** Every Bynder call needs retry on 429 + 5xx; without it, transient failures look like outages.
- **Loading entire asset bank into memory for one operation.** Async iterate.
- **Looking up metaproperty UUIDs on every write.** Cache.
- **Trusting SDK response shapes without typing them.** Declare interfaces.
- **Retrying 401s.** That's a token problem; refresh once, fail otherwise.
- **Retrying 400s.** That's a logic problem; fail loudly.
- **Webhook handler that doesn't verify HMAC.** Trivially spoofable; always verify.

## Constraints

- Do not recommend the SDK for surfaces it doesn't wrap (Workflow / Analytics / Brand Guidelines) — recommend `rawRequest` or plain `fetch`.
- Do not assume the SDK's response types are complete; declare your own interfaces.
- Do not recommend Permanent Tokens for production integrations.
- Do not skip the retry wrapper.
- Do not load full asset banks into memory; iterate.

## How this skill relates to others

- The metaproperty schema this code reads/writes → `bynder-asset-model`.
- The downstream system the integration serves → `bynder-contentful-pairing`, `bynder-marketplace-connectors`.
- Bulk-import use of this client → `bynder-migration`.
- Webhook receivers that re-fetch via this client → `bynder-webhooks-events`.
- Compact View flows that may share token strategy → `bynder-compact-view`.

## Source material

- SDK repo: https://github.com/Bynder/bynder-js-sdk
- API reference: https://api.bynder.com/docs/
- OAuth2 reference: https://api.bynder.com/reference/get_v6-authentication-oauth2-auth
- Plugin reference: `../../references/sdk-cheat-sheet.md` (quick-reference patterns)
- Plugin reference: `../../references/api-surface.md`
- Plugin reference: `../../references/bynder-foundations.md`
