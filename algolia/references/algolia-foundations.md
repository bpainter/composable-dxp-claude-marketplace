---
type: reference
project: skills-library
scope: plugin
plugin: algolia
tags: [type/reference, plugin/algolia, scope/foundational, topic/algolia]
status: active
---
# Algolia foundations — the concepts behind every skill

The vocabulary that recurs in every other skill in this plugin. Get these right and the rest of the surface is just composition.

## Domain hierarchy

```
Account
└── Application (one per environment, usually)
    ├── Index ── Settings (searchable, faceting, ranking, typo, …)
    │   ├── Records (objects)
    │   ├── Synonyms (one-way / multi-way / placeholders / alt-corrections)
    │   ├── Rules (query → action)
    │   └── Replicas (standard / virtual)
    ├── API Keys (admin / search / monitoring / analytics / scoped / secured)
    ├── Users (Personalization profiles)
    └── Cluster (the underlying compute; multi-region available)
```

### Application

The unit of authentication and billing. Has its own `appId`, its own keys, its own indices. **One application per environment** is the rule of thumb — `slalom-prod`, `slalom-staging`, `slalom-dev`. Don't mingle staging records with prod traffic.

### Index

The fundamental search container. Settings live on the index, not on individual records. Indices are flat; you don't get sub-indices, but you get **replicas** that share the underlying records and only differ by sort order.

### Record (object)

A flat JSON document. Algolia is schemaless at the index level — there's no required shape — but **uniformity matters for relevance tuning**. Each record has an `objectID` (you supply it; if you don't, Algolia generates one, but always supply it so reindexes are idempotent). Recommended max record size: **10 KB** (free), 100 KB (paid plans). Bigger records → worse relevance and higher cost.

### Settings

Per-index configuration. Some live forever once set, others change frequently. The ones you'll touch:

- `searchableAttributes` — which attributes are queried, and in what priority order.
- `attributesForFaceting` — which attributes can be filtered/faceted (`searchable`, `filterOnly`, or default).
- `customRanking` — the tiebreaker after textual relevance: `desc(popularity)`, `asc(price)`.
- `ranking` — the master ranking formula. Default works for ~95% of cases; touch with care.
- `attributesToHighlight` / `attributesToSnippet` — what gets the `<em>` and the truncation.
- `typoTolerance`, `minWordSizefor1Typo`, `minWordSizefor2Typos` — typo behavior.
- `distinct` + `attributeForDistinct` — collapse duplicates (e.g., one product across colors).
- `relevancyStrictness` — Personalization / Dynamic Re-Ranking knob.

### Replicas — standard vs. virtual

Sort orders need a replica because Algolia ranks by relevance first. Replicas come in two flavors:

| | Standard | Virtual |
|---|---|---|
| Storage | Full copy of records | Shares records with primary |
| Cost | Counted as separate records (×N) | Free of record-count impact |
| Settings | Independent | Inherited (only `customRanking` differs) |
| Use when | You need different `searchableAttributes` or full setting independence | You only need a different sort order |

**Default to virtual.** Standard replicas are an old pattern and rarely worth the cost.

### Records — sourcing, freshness, idempotency

- `objectID` is your idempotency key. Reuse it across runs.
- **Atomic reindexing** uses `replaceAllObjects`: writes to a temporary index, swaps. No empty-index window, no partial state.
- **Partial updates** with `partialUpdateObject` change one or more attributes without re-sending the whole record. Cheaper than full updates.
- **Indexing operations are eventually consistent** — typically <1s, occasionally longer. Don't read-after-write within the same request.

## Search: the request lifecycle

```
Client →  POST https://{appId}-{dsn}.algolia.net/1/indexes/{index}/query
       ↳  appId + apiKey in headers (or query string)
       ↳  body: { query, hitsPerPage, filters, facets, page, ... }
       ↳  optional: clickAnalytics: true, userToken (for Personalization)
Algolia → { hits, nbHits, page, queryID, processingTimeMS, ... }
```

- **DSN routing** picks the closest replica cluster automatically.
- **`queryID`** is critical: it's what you pass back when you send `click` and `conversion` events to attribute results to a specific search.
- **Latency budget**: target <50ms server-side at p95. Algolia itself is sub-50ms in-region in nearly all cases; the hop from your edge to Algolia is the variable.

## Insights events — the analytics spine

Every measurable thing — clicks, conversions, A/B tests, Personalization, NeuralSearch evaluation, Recommend training — depends on **Insights events**. Three event types:

- `view` — fired when a record is shown.
- `click` — fired when a user clicks a record.
- `conversion` — fired when a user does the meaningful thing (purchase, signup, request a meeting, download).

Each event optionally carries `queryID` (for search-attributed events) and `userToken` (for Personalization). Without `queryID`, the click can't be attributed to the search; without `userToken`, Personalization has no user to learn from.

## Rate limits — what the SDKs hide from you

- **Search rate limits** are generous and per app. You're more likely to hit them via misuse (a single front-end query that triggers 12 searches via tabbed UI) than via traffic.
- **Indexing rate limits** are tighter. Use **batching** (`saveObjects` / `partialUpdateObjects`) instead of one-by-one. Batches up to 1000 records or 10 MB at a time.
- **Recommend / Personalization training** runs nightly. You don't trigger it; you provide enough events for it to work.

## Multi-tenant and access control

The model is: **secured API keys**. You start with a search-only API key, then on the server you add filters (`filters: 'tenantId:42'`), an expiry (`validUntil`), and any IP / index restrictions, and `generateSecuredApiKey` returns an opaque token your front end can use safely. The token is HMAC-signed; users can't tamper with the embedded filters. See the `algolia-api-keys-security` skill.

## Plans and feature gating

Many Algolia features are **plan-gated**:

| Feature | Plan |
|---|---|
| Search, Indexing, Synonyms, Rules | All paid plans (Build / Grow / Premium) |
| Personalization | Grow and above |
| Recommend | Grow and above (extra cost per request on some models) |
| NeuralSearch / AI Search | Premium |
| Query Suggestions | All paid plans |
| Insights API | All paid plans |
| Ingestion API source connectors | Grow and above |
| Multi-region clusters / DSN | Premium |

Check the active plan in the Dashboard before promising a capability.

## SDKs and client libraries

- **JavaScript**: `algoliasearch` (the fully-featured client) and `@algolia/client-search` (the lite client, search-only, smaller bundle). Default to the lite client for the front end.
- **InstantSearch**: `react-instantsearch` (current; replaces `react-instantsearch-dom`), `instantsearch.js`, `vue-instantsearch`, `angular-instantsearch`.
- **Autocomplete**: `@algolia/autocomplete-js`, `@algolia/autocomplete-plugin-*`.
- **Insights**: `search-insights` (browser) or call the REST endpoint directly server-side.
- **Backend**: official clients in Node, Python, PHP, Ruby, Java, Go, .NET, Swift, Kotlin, Scala — all generated from the OpenAPI specs in `algolia/api-clients-automation`.

## Source material

- Algolia REST API: https://www.algolia.com/doc/rest-api
- Get started guides: https://www.algolia.com/doc/guides/get-started
- Libraries and tools: https://www.algolia.com/doc/libraries
- API clients automation (the OpenAPI source): https://github.com/algolia/api-clients-automation
