---
type: reference
project: skills-library
scope: plugin
plugin: bynder
tags: [type/reference, plugin/bynder, scope/foundational, topic/sdk]
status: active
---
# bynder-js-sdk cheat sheet

Quick reference for [`@bynder/bynder-js-sdk`](https://github.com/Bynder/bynder-js-sdk) — the official JavaScript / TypeScript SDK. Skills cite this file rather than restating boilerplate.

Install: `pnpm add @bynder/bynder-js-sdk` (or `npm`, `yarn`).

The SDK wraps the Asset Bank API and the OAuth2 token lifecycle. It does not yet wrap Workflow, Analytics, or Brand Guidelines — for those, fall back to plain `fetch`.

## Initializing the client

### Permanent token (one-off scripts, dev)

```ts
import Bynder from '@bynder/bynder-js-sdk';

const bynder = new Bynder({
  baseURL: 'https://your-portal.bynder.com/api/',
  permanentToken: process.env.BYNDER_PERMANENT_TOKEN!,
});
```

### Client Credentials (server-to-server)

```ts
import Bynder from '@bynder/bynder-js-sdk';

const bynder = new Bynder({
  baseURL: 'https://your-portal.bynder.com/api/',
  clientId: process.env.BYNDER_CLIENT_ID!,
  clientSecret: process.env.BYNDER_CLIENT_SECRET!,
  // SDK handles token request and refresh internally
});
```

### Authorization Code (user-in-the-loop)

```ts
import Bynder from '@bynder/bynder-js-sdk';

const bynder = new Bynder({
  baseURL: 'https://your-portal.bynder.com/api/',
  clientId: process.env.BYNDER_CLIENT_ID!,
  clientSecret: process.env.BYNDER_CLIENT_SECRET!,
  redirectUri: 'https://your-app.example.com/oauth/callback',
});

// 1. Build the authorize URL and redirect the user
const url = bynder.makeAuthorizationURL(
  'csrf-token',
  'offline asset:read meta.assetbank:read'
);

// 2. After redirect-back, exchange the code
const token = await bynder.getToken(req.query.code);

// 3. Subsequent calls use the stored token; SDK refreshes automatically when offline scope is requested
```

## Listing and searching assets

```ts
// Basic list
const { items, page, total } = await bynder.getMediaList({
  limit: 50,
  page: 1,
  type: 'image',
});

// Search by metaproperty
const heroes = await bynder.getMediaList({
  propertyOptionId: ['{metaproperty-option-uuid}'],
  type: 'image',
  limit: 100,
});

// Free-text search
const results = await bynder.getMediaList({
  keyword: 'composable hero',
  limit: 50,
});

// Get a single asset with derivative URLs included
const asset = await bynder.getMediaInfo({
  id: '5KsDBWseXY6QegucYAoacS',
  versions: 1, // include derivative URL set
});
```

## Uploading an asset

The SDK abstracts the multi-step upload (init → chunks → finalize):

```ts
import { readFileSync } from 'node:fs';

const fileBuffer = readFileSync('./hero.jpg');

const result = await bynder.uploadFile({
  filename: 'Hero_Image_Composable_2026.jpg',
  body: fileBuffer,
  contentType: 'image/jpeg',
  data: {
    brandId: '{brand-uuid}',
    name: 'Hero Image — Composable 2026',
    description: 'Campaign hero, web + social use, photographer credit attached',
    tags: ['hero', 'campaign-2026'],
    metaproperty_AssetCategory: ['{option-uuid-marketing}'],
    metaproperty_Channel: ['{option-uuid-web}', '{option-uuid-social}'],
  },
});

console.log(result.mediaid);
```

For very large files (video, hi-res print), the SDK handles chunking under the hood. Watch for memory pressure if you're uploading from a Node process that's already memory-constrained — stream via `body` rather than loading the full buffer when possible.

## Updating metadata

```ts
await bynder.editMedia({
  id: '5KsDBWseXY6QegucYAoacS',
  name: 'Hero Image — Composable 2026 (v2)',
  description: 'Updated copy',
  metaproperty_BrandPillar: ['{option-uuid-composable}'],
});
```

The metadata format mirrors the upload `data` object — `metaproperty_{Name}` keys with arrays of option UUIDs.

## Listing and managing metaproperties

```ts
// All metaproperties
const props = await bynder.getMetaproperties();

// One metaproperty with options
const prop = await bynder.getMetaproperty({ id: '{metaproperty-uuid}' });

// Create a new metaproperty
await bynder.saveNewMetaproperty({
  name: 'Channel',
  label: 'Channel',
  type: 'select2',
  isMultiselect: true,
  isFilterable: true,
  isRequired: false,
});

// Add an option to an existing metaproperty
await bynder.saveNewMetapropertyOption({
  id: '{metaproperty-uuid}',
  name: 'Out-of-home',
  label: 'Out-of-home',
});
```

For complex metaproperty work (parent/child hierarchies, bulk option creation), the SDK gets terse — fall back to direct API calls via the SDK's `_request` escape hatch or plain `fetch` if needed.

## Tags and collections

```ts
const tags = await bynder.getTags({ limit: 1000 });
const collections = await bynder.getCollections({ limit: 200 });
const collection = await bynder.getCollection({ id: '{collection-uuid}' });
```

## Brand Guidelines, Workflow, Analytics

Not in the SDK as of writing. Use plain `fetch` against the documented endpoints. Skeleton:

```ts
async function bynderRequest<T>(path: string, init: RequestInit = {}): Promise<T> {
  const res = await fetch(`https://your-portal.bynder.com${path}`, {
    ...init,
    headers: {
      ...init.headers,
      'Authorization': `Bearer ${await getAccessToken()}`,
      'Content-Type': 'application/json',
    },
  });
  if (!res.ok) {
    const body = await res.text();
    throw new Error(`Bynder ${res.status}: ${body}`);
  }
  return res.json() as Promise<T>;
}

const campaigns = await bynderRequest('/workflow/campaigns/');
```

## Error handling

The SDK throws on non-2xx responses; the error carries the HTTP body. Wrap every call you care about:

```ts
try {
  await bynder.editMedia({ id, name });
} catch (err) {
  if (err.status === 401) {
    // token expired or scope insufficient; re-auth
  } else if (err.status === 429) {
    // rate-limited; backoff
  } else if (err.status === 404) {
    // asset deleted between fetch and edit
  } else {
    throw err; // genuinely unexpected
  }
}
```

## Rate limiting and retry

The SDK does not implement retry/backoff for you. Wrap with `p-retry` or similar:

```ts
import pRetry from 'p-retry';

await pRetry(() => bynder.uploadFile({ /* ... */ }), {
  retries: 5,
  factor: 2,
  minTimeout: 500,
  onFailedAttempt: (e) => {
    if (e.response?.status === 429) return; // expected
    if (e.response?.status >= 500) return; // expected
    throw e; // don't retry 4xx other than 429
  },
});
```

For migrations, throttle yourself to ~5 req/sec — the API will start 429-ing well before that on Asset Bank writes.

## Pagination patterns

```ts
async function* allAssets(filter: Record<string, unknown>) {
  let page = 1;
  while (true) {
    const list = await bynder.getMediaList({ ...filter, limit: 1000, page });
    if (!list || list.length === 0) return;
    yield* list;
    if (list.length < 1000) return;
    page++;
  }
}

for await (const asset of allAssets({ type: 'image' })) {
  // process
}
```

The `getMediaList` shape varies — sometimes the SDK returns `items + page + total`, sometimes just an array. Inspect the response shape on your specific SDK version and account.

## Common pitfalls

- **`metaproperty_*` keys take option UUIDs, not display names.** Resolve names → UUIDs once, cache the lookup, use UUIDs in writes.
- **`brandId` is required on upload** when the account has multiple brand IDs configured. Get it from `getBrands()` once.
- **`type` filter only accepts a single value.** To search both image + video, do two queries or use the GraphQL Asset API where available.
- **Default page size is 50.** Raise to `limit: 1000` for migrations.
- **Token expiry on long-running scripts** — the SDK refreshes tokens for Authorization Code with `offline` scope, but Client Credentials tokens don't refresh; renew explicitly when expiry is near.
- **Webhook receivers re-fetch via SDK** — but they need their own credentials; don't share the editing-app's user-bound token with the webhook receiver.

## CI usage patterns

```yaml
# .github/workflows/bynder-sync.yml
- name: Sync metaproperties from code
  env:
    BYNDER_CLIENT_ID: ${{ secrets.BYNDER_CLIENT_ID }}
    BYNDER_CLIENT_SECRET: ${{ secrets.BYNDER_CLIENT_SECRET }}
    BYNDER_PORTAL: ${{ vars.BYNDER_PORTAL }}
  run: pnpm tsx ./scripts/sync-metaproperties.ts
```

Pin SDK version. The package has had breaking changes; lock to a known-good version and upgrade deliberately.

## Source

- SDK repo: https://github.com/Bynder/bynder-js-sdk
- API reference: https://api.bynder.com/docs/
- Authorization: https://api.bynder.com/docs/getting-started#authorization
