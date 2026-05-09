---
name: algolia-search-client
description: >
  Direct usage of the Algolia search client (`algoliasearch` and the lite `@algolia/client-search`) — when InstantSearch and Autocomplete are the wrong shape and you need raw queries. Covers RSC search, server actions, edge runtime, multi-index queries, browse, search params, locale-aware querying, error handling, and the Slalom defaults for module boundaries (where the client lives, who instantiates it, how secrets are scoped). Use this skill when building a custom search UI, when calling Algolia from a Next.js Server Component or Route Handler, when running search at the edge, or when InstantSearch overhead isn't justified.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-search-client]
tags: [type/skill, plugin/algolia, topic/algolia, topic/sdk]
status: active
---

This skill puts you in the role of an engineer reaching for raw Algolia clients when the higher-level libraries don't fit. Default posture: **InstantSearch and Autocomplete handle the common cases; the search client handles everything else.**

The 2024+ JavaScript clients are a single `algoliasearch` package with a search-and-management entry, plus a lite `@algolia/client-search` for browser bundles. The older `algoliasearch` v4 had separate `searchClient.initIndex(name).search(...)` calls; the v5+ API is flatter — everything goes through the client directly.

Pair with `algolia-instantsearch-react` (when the page is search-shaped), `algolia-autocomplete` (when the UX is typeahead-shaped), `algolia-indexing-pipeline` (the same client writes records), and `algolia-api-keys-security` (the keys passed to the client).

## When to use this skill

- Server Component or Route Handler doing a search and rendering plain HTML (no client-side refinement).
- A custom search UI that doesn't fit InstantSearch's widget model.
- A Server Action that issues a search in response to a form submission.
- Edge functions / middleware that pre-filter or redirect based on a search.
- A backend service consuming Algolia (Node, Python, Go).
- A one-off script — exporting an index, computing aggregations, validating records.
- Anywhere the InstantSearch state machine is overkill for a one-shot query.

## Core posture

- **Pick the right client variant.** `algoliasearch` for full control (read + write); `@algolia/client-search` for read-only browser bundles.
- **Server vs. client matters.** Server: Node SDK + admin or scoped key in env. Client: lite SDK + search-only key (or secured API key).
- **One client per process.** Reuse the instance; don't recreate it per request.
- **Always supply `objectID` on writes.** Idempotency.
- **Use `clickAnalytics: true`** on every search, regardless of UI library, so Insights can attribute downstream.
- **Errors return JSON with details.** Log the whole envelope.

## Install

```bash
# Full client — server-side or full-control client
pnpm add algoliasearch

# Lite client — browser bundles, read-only
pnpm add @algolia/client-search
```

Note: the v5+ JavaScript client uses ESM-first exports. For Vercel Edge Runtime, the lite client works; the full client may need configuration depending on bundler.

## Client setup — server-side

```ts
// lib/algolia/server.ts
import { algoliasearch } from 'algoliasearch';

let _client: ReturnType<typeof algoliasearch> | null = null;

export function getServerClient() {
  if (!_client) {
    const appId = process.env.ALGOLIA_APP_ID;
    const apiKey = process.env.ALGOLIA_ADMIN_KEY; // server-only
    if (!appId || !apiKey) throw new Error('Missing Algolia server env');
    _client = algoliasearch(appId, apiKey);
  }
  return _client;
}
```

Server keys belong only to server contexts. Never bundle `ALGOLIA_ADMIN_KEY` into client code; never prefix it with `NEXT_PUBLIC_`.

## Client setup — browser

```ts
// lib/algolia/browser.ts
import { liteClient as algoliasearch } from 'algoliasearch/lite';

let _client: ReturnType<typeof algoliasearch> | null = null;

export function getBrowserClient() {
  if (!_client) {
    const appId = process.env.NEXT_PUBLIC_ALGOLIA_APP_ID;
    const apiKey = process.env.NEXT_PUBLIC_ALGOLIA_SEARCH_KEY;
    if (!appId || !apiKey) throw new Error('Missing Algolia client env');
    _client = algoliasearch(appId, apiKey);
  }
  return _client;
}
```

The lite client only does search and Insights — no settings, no writes, no key management. Bundle is much smaller. Use it everywhere browser-side.

## Single-index search

```ts
import { getServerClient } from '@/lib/algolia/server';

export async function searchArticles(query: string, page = 0) {
  const client = getServerClient();
  const { results } = await client.search<ArticleHit>({
    requests: [{
      indexName: 'articles',
      query,
      hitsPerPage: 20,
      page,
      clickAnalytics: true,
      attributesToRetrieve: ['title', 'summary', 'url', 'image', 'author_name'],
      filters: 'locale:en-US',
    }],
  });
  return results[0];
}
```

The `requests` array is plural because the v5+ API treats single-index search as a special case of multi-index. The result is `results[0]`.

`attributesToRetrieve` shrinks the payload to only the fields the UI uses. For RSC and Route Handler use, this is meaningful — payloads compound across requests.

## Multi-index search (federation)

```ts
const { results } = await client.search<ArticleHit | PersonHit | GlossaryHit>({
  requests: [
    { indexName: 'articles', query, hitsPerPage: 5, clickAnalytics: true },
    { indexName: 'people', query, hitsPerPage: 3 },
    { indexName: 'glossary_terms', query, hitsPerPage: 3 },
  ],
});

const [articles, people, glossary] = results;
```

One round trip; one shared `query` across requests. Each result has its own `nbHits`, pagination, etc.

## Browse — paginating the entire index

For exports, sitemaps, scheduled jobs:

```ts
import { browseObjects } from 'algoliasearch';

const all: ArticleHit[] = [];
await client.browseObjects<ArticleHit>({
  indexName: 'articles',
  browseParams: { filters: 'locale:en-US' },
  aggregator: (response) => {
    all.push(...response.hits);
  },
});
```

Browse skips ranking and pagination — fast for full scans, no `nbHits` cap.

Use for:
- Sitemap generation.
- Building secondary indices (`my-index_top-100`).
- Scheduled jobs comparing source vs. index records.

Don't use for runtime search.

## Server Component pattern (Next.js App Router)

```tsx
// app/articles/page.tsx
import { searchArticles } from '@/lib/algolia/articles';

export default async function ArticlesPage({
  searchParams,
}: {
  searchParams: Promise<{ q?: string; page?: string }>;
}) {
  const { q = '', page = '0' } = await searchParams;
  const result = await searchArticles(q, Number(page));

  return (
    <main>
      <SearchForm initialQuery={q} />
      <p>{result.nbHits} results</p>
      <ol>
        {result.hits.map((hit) => (
          <li key={hit.objectID}>
            <a href={hit.url}>{hit.title}</a>
          </li>
        ))}
      </ol>
      <Pagination total={result.nbPages} current={result.page} />
    </main>
  );
}
```

This pattern wins when:
- You don't need client-side refinement (no facets, no toggles).
- The page is bookmarkable / linkable / SEO-relevant.
- The result list is the main thing on the page.

When you need refinement, switch to `algolia-instantsearch-react`.

## Server Action — search from a form

```tsx
'use server';
import { searchArticles } from '@/lib/algolia/articles';
import { redirect } from 'next/navigation';

export async function submitSearch(formData: FormData) {
  const q = String(formData.get('q') ?? '');
  redirect(`/articles?q=${encodeURIComponent(q)}`);
}
```

For "submit and go to a results page," redirect is simpler than running the search in the action and passing data. Let the page fetch its own data in its `page.tsx`.

## Edge / middleware pattern

```ts
// middleware.ts (Vercel Edge)
import { liteClient as algoliasearch } from 'algoliasearch/lite';
import { NextResponse } from 'next/server';

const client = algoliasearch(
  process.env.NEXT_PUBLIC_ALGOLIA_APP_ID!,
  process.env.NEXT_PUBLIC_ALGOLIA_SEARCH_KEY!
);

export async function middleware(req) {
  const { pathname } = new URL(req.url);
  // Example: redirect /go/{query} → /search?q={query} only if results exist
  // ...
}
```

Edge has timing constraints (≤25ms typical CPU budget on Vercel). The lite client is fine, but be mindful of total round-trip time including the Algolia call.

## Locale-aware querying

If your records are fanned out one-per-locale (recommended), filter at query time:

```ts
client.search({
  requests: [{
    indexName: 'articles',
    query,
    filters: `locale:${locale}`,
  }],
});
```

If your records carry a `_locale` array (one record covers multiple locales — rarer pattern), use:

```ts
filters: `_locale:${locale}`
```

For multi-locale dashboards (one search across all locales), drop the filter and add `attributesForFaceting` on `locale` so the UI can show locale-specific counts.

## Search params — the ones you actually use

| Param | What |
|---|---|
| `query` | The query string |
| `hitsPerPage` | 1–1000; default 20 |
| `page` | 0-based |
| `filters` | Boolean filter expression — `'topic:dxp AND publishedAt_unix > 1700000000'` |
| `facetFilters` | Array form of filters; cleaner for refinement UI |
| `numericFilters` | Array — `['price >= 1000', 'price <= 5000']` |
| `facets` | Which facets to count |
| `maxValuesPerFacet` | Default 10; bump for big facets |
| `attributesToRetrieve` | Limits the payload |
| `attributesToHighlight` | Wraps matches in `<em>` |
| `attributesToSnippet` | Truncates around matches |
| `clickAnalytics` | true → returns `queryID` for Insights attribution |
| `enablePersonalization` | true + `userToken` → applies Personalization |
| `userToken` | Per-user identifier for Personalization |
| `aroundLatLng` / `aroundRadius` | Geosearch |
| `mode` | `'neuralSearch'` to enable hybrid (Premium plan) |

Pass them per-request inside `requests`. Don't set them globally on the client (a stale anti-pattern).

## Error handling

```ts
try {
  const { results } = await client.search({ requests: [{ indexName: 'articles', query }] });
  return results[0];
} catch (err) {
  if (err && typeof err === 'object' && 'status' in err) {
    // Algolia error: { status, message, name, code? }
    console.error('Algolia error', err);
    if ((err as any).status === 404) return { hits: [], nbHits: 0 };
    throw err;
  }
  throw err;
}
```

Specific cases:
- **Index not found (404)** — return empty results gracefully on first deploy or if a tenant has no content yet.
- **Invalid API key (403)** — fail fast; log loud. Means the key was rotated and the env is stale.
- **Rate limited (429)** — back off; consider partitioning keys per workload.
- **Validation error (400)** — usually means a malformed query or bad filter expression. Log the request body.

## Caching

Algolia CDN-caches search responses by default. Don't try to cache them yourself in your runtime.

For RSC, you *can* layer Next's `unstable_cache` for non-personalized queries that change rarely (e.g., a "trending" sidebar). Set a short TTL (60s) and tag for revalidation. Don't cache personalized queries.

## Testing

```ts
// __tests__/articles.test.ts
import { describe, it, expect, vi } from 'vitest';
import { searchArticles } from '@/lib/algolia/articles';

vi.mock('@/lib/algolia/server', () => ({
  getServerClient: () => ({
    search: vi.fn().mockResolvedValue({
      results: [{ hits: [{ objectID: 'a1', title: 'Test' }], nbHits: 1, page: 0, nbPages: 1 }],
    }),
  }),
}));

describe('searchArticles', () => {
  it('returns hits', async () => {
    const result = await searchArticles('test');
    expect(result.hits).toHaveLength(1);
  });
});
```

For integration tests against a real Algolia, use a separate test app (`slalom-test`) and reset state per run. Don't run integration tests against staging or prod.

## Common patterns

### Conditional Personalization

```ts
const userToken = await getUserToken(); // null if anonymous
const { results } = await client.search({
  requests: [{
    indexName: 'articles',
    query,
    ...(userToken ? { enablePersonalization: true, userToken } : {}),
    clickAnalytics: true,
  }],
});
```

### Secured-key-per-request

```ts
import { generateSecuredApiKey } from 'algoliasearch';

const securedKey = generateSecuredApiKey(SEARCH_KEY, {
  filters: `tenantId:${tenantId}`,
  validUntil: Math.floor(Date.now() / 1000) + 3600,
});

// pass `securedKey` to the front end; instantiate the lite client with it
```

See `algolia-api-keys-security` for the deeper treatment.

### Pre-warmed first paint

```tsx
// Server Component
const initial = await searchArticles(q);

return (
  <ArticlesClient initialResult={initial} />  // Client Component takes over for refinement
);
```

Hand the server-fetched result to a Client Component as initial state; let the client take over for further interaction. Same pattern as InstantSearch's SSR, just hand-rolled.

## Common anti-patterns

- **Recreating the client per request.** Slow; defeats the SDK's connection pooling.
- **Putting an admin or write key in `NEXT_PUBLIC_*`.** Catastrophic; rotate immediately.
- **Calling `client.search` from a Client Component with a server-only key.** Won't work; the bundle would need the key, which you don't ship.
- **Skipping `clickAnalytics: true`.** Insights can't attribute clicks to the search.
- **No `attributesToRetrieve`.** Pulls every field on every hit; payload blows up over time.
- **Hand-rolling pagination state across renders.** Either keep it in the URL, or use InstantSearch.
- **Using the full `algoliasearch` package on the browser.** Use the lite client.
- **Catching every error silently.** A 403 should fail loudly so the env gets fixed.

## Constraints

- Don't ship a custom search UI that has refinement when InstantSearch would do. Maintenance debt.
- Don't bypass Insights. Even hand-rolled UIs should fire `view` and `click` events.
- Don't run the same search server-side and client-side for the same render. Pick one — usually server, with client takeover for refinement.
- Don't put SDK calls in a tight loop. Batch via multi-index search instead.

## How this skill relates to others

- The widget-based React UI on top of this client → `algolia-instantsearch-react`.
- The typeahead UI on top of this client → `algolia-autocomplete`.
- Indexing writes (using the same client, different methods) → `algolia-indexing-pipeline`.
- Key strategy and secured keys → `algolia-api-keys-security`.
- Insights events → `algolia-analytics-events`.

## Source material

- JavaScript clients (v5+): https://www.algolia.com/doc/libraries/javascript/v5/
- Search params reference: https://www.algolia.com/doc/api-reference/api-parameters/
- Lite client: https://www.npmjs.com/package/algoliasearch (see `/lite` entry)
- Multi-index queries: https://www.algolia.com/doc/api-reference/api-methods/multiple-queries/
- Browse: https://www.algolia.com/doc/api-reference/api-methods/browse/
- API clients automation (the OpenAPI source): https://github.com/algolia/api-clients-automation
- Plugin reference: `../../references/api-surface.md`
