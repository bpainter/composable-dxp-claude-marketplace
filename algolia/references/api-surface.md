---
type: reference
project: skills-library
scope: plugin
plugin: algolia
tags: [type/reference, plugin/algolia, scope/foundational, topic/algolia, topic/api]
status: active
---
# Algolia API surface — when to reach for which

Nine production APIs plus the Ingestion API for source connectors. Each skill in this plugin tells you which one to use; this reference tells you why.

## Decision matrix

| If you need to... | Use | Skill |
|---|---|---|
| Run a search query from front end or back end | Search API | `algolia-search-client`, `algolia-instantsearch-react`, `algolia-autocomplete` |
| Index, update, delete records | Search API (`saveObjects`, `partialUpdateObjects`, `deleteObjects`) | `algolia-indexing-pipeline` |
| Manage settings, synonyms, rules | Search API | `algolia-relevance-tuning` |
| Manage API keys | Search API (`addApiKey`, `updateApiKey`) | `algolia-api-keys-security` |
| Send click / view / conversion events | Insights API | `algolia-analytics-events` |
| Get Related, FBT, Trending, Looking Similar | Recommend API | `algolia-recommend` |
| Query Personalization profiles, train Personalization | Personalization API | `algolia-personalization-ai` |
| Set up A/B tests | A/B Testing API | `algolia-relevance-tuning`, `algolia-analytics-events` |
| Get search analytics (top queries, no-result, click position) | Analytics API | `algolia-analytics-events` |
| Watch app health and performance | Monitoring API | (ops) |
| Pull data from a managed source (Contentful, Shopify, BigQuery, S3) | Ingestion API | `algolia-indexing-pipeline`, `algolia-contentful-integration` |

## Search API

The everything-API for search and indexing. One endpoint host — `{appId}-dsn.algolia.net` (search) and `{appId}.algolia.net` (writes). Auth: app ID + API key in the headers.

- **Search** (`POST /1/indexes/{index}/query`) — query records.
- **Multi-index search** (`POST /1/indexes/*/queries`) — federated, single round trip.
- **Browse** (`POST /1/indexes/{index}/browse`) — paginate the entire index without ranking; use for export.
- **Indexing** (`POST /1/indexes/{index}/batch`) — batched writes.
- **Settings** (`PUT /1/indexes/{index}/settings`) — update or replace.
- **Rules** (`PUT /1/indexes/{index}/rules/{objectID}`) — query rules CRUD.
- **Synonyms** (`PUT /1/indexes/{index}/synonyms/{objectID}`) — synonyms CRUD.
- **Replace All Objects** — atomic reindex via `operations` API.
- **API Keys** (`POST /1/keys`, etc.) — add / update / delete keys.

Rate limits: search is high (per-plan, but typically thousands of QPS); writes are batched (1000 ops or 10 MB at a time).

Docs: https://www.algolia.com/doc/rest-api/search/

## Insights API

Where every event goes that downstream features (Personalization, Recommend, A/B, Analytics) need to learn from. Endpoint: `https://insights.algolia.io/1/events`. Auth: app ID + search-only key (yes, search-only — Insights is intentionally low-trust because the events are event sources for the user, not authoritative state).

- `POST /1/events` — array of events. Up to 1000 per call.
- Event types: `click`, `view`, `conversion` (and `addToCart` / `purchase` flavors that include subtype + price).
- Required fields: `eventType`, `eventName`, `index`, `userToken`, `objectIDs` (or `filters`).
- Optional but high-value: `queryID` (attribution), `positions` (CTR-by-position), `currency` + `objectData` (revenue attribution).

**If you skip Insights, you lose Personalization, Recommend models, A/B testing, and Search Analytics enrichment.** Wire it on day one.

Docs: https://www.algolia.com/doc/rest-api/insights/

## Recommend API

Pre-trained recommendation models served as a separate API. Endpoint: `POST /1/indexes/*/recommendations`. Auth: app ID + search-only key.

Models (specify `model` per request):
- `related-products` — items similar to a given object (cosine on co-events + content).
- `bought-together` — items frequently bought / consumed with the given object.
- `trending-items` / `trending-facets` — popular over a recent window.
- `looking-similar` — image-similarity (requires image URLs in records, opt-in).

Each model needs minimum events to train (typically thousands of events over the past 30 days); the model trains nightly. The API returns a list of recommended objectIDs scored.

Reach for it instead of building your own related-items logic. The lift over a hand-rolled "same category, sort by views" widget is real.

Docs: https://www.algolia.com/doc/rest-api/recommend/

## Personalization API

Read and write user profiles, fetch the events backing a profile, and trigger Personalization strategy updates. Endpoint: `https://personalization.algolia.com`.

- `GET /1/profiles/personalization/{userToken}` — read a profile.
- `DELETE /1/profiles/{userToken}` — GDPR delete.
- `GET /1/strategies/personalization` / `POST` — read/write the Personalization strategy (event weights, facets to consider).

Personalization is mostly *configured* in the Dashboard and *consumed* by passing `userToken` + `enablePersonalization: true` on search calls. This API is for inspection, debugging, and right-to-be-forgotten.

Docs: https://www.algolia.com/doc/rest-api/personalization/

## A/B Testing API

Define and observe A/B tests. Endpoint: `https://analytics.algolia.com/2/abtests`.

- Variants are defined by index (one variant per index) or by `customSearchParameters` overrides.
- Traffic split is set per test.
- Significance is computed by Algolia's internal stats engine.
- Tests need ≥ 30 days of clicks/conversions to be meaningful for most search experiences.

A/B tests live alongside Personalization and NeuralSearch, so you can A/B them too.

Docs: https://www.algolia.com/doc/rest-api/ab-test/

## Analytics API

Read the analytics surface — top searches, top searches with no results, top clicked, no-click rate, click position, conversions, top filters. Endpoint: `https://analytics.algolia.com/2/`.

The Dashboard shows all of this; the API is for piping it into your own observability or reporting layer.

Docs: https://www.algolia.com/doc/rest-api/analytics/

## Monitoring API

App health, latency, status, infrastructure events. Endpoint: `https://status.algolia.com/1/`.

Reach for it when wiring uptime to PagerDuty, Datadog, or Slack. Day-to-day delivery rarely uses it.

Docs: https://www.algolia.com/doc/rest-api/monitoring/

## Ingestion API

Managed source connectors and ETL inside Algolia. Endpoint: `https://data.{region}.algolia.com/1/`. Region-bound (`us`, `eu`).

Concepts:
- **Source** — where data comes from (Contentful, Shopify, BigQuery, Snowflake, S3, custom).
- **Destination** — the Algolia index that receives it.
- **Task** — a scheduled or webhook-triggered run.
- **Transformation** — a JS function (run inside Algolia) that maps source records to Algolia records.

When this fits, prefer it to a hand-rolled indexer. When the transformation logic is too complex (cross-record lookups, asset processing, locale fan-out), build the indexer in your own runtime.

Docs: https://www.algolia.com/doc/rest-api/ingestion/

## Query Suggestions API

A managed index of query suggestions, generated nightly from your traffic. You don't write to it directly; you configure a source index and Algolia generates the suggestions. The suggestions index is then queryable like any other index — typical use: power Autocomplete with `useQuerySuggestionsPlugin`.

Docs: https://www.algolia.com/doc/guides/building-search-ui/ui-and-ux-patterns/query-suggestions/

## Cross-cutting concerns

- **Tokens belong in env vars, never in code or repos.** Rotate on staff changes.
- **Rate limits are per app + per key.** If a build-time process and a runtime process share a key, throttling one starves the other. Partition.
- **Errors return JSON with `status` + `message` + sometimes `details.items[]`.** Log the whole envelope; the items array is where validation errors live.
- **DSN routing** is automatic for search; for writes, all traffic goes to the primary cluster.
- **Versioning**: the Search API is stable v1; major changes are introduced as new endpoints, not breaking ones. The Ingestion API is newer and evolves more.
