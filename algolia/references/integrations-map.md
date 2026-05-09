---
type: reference
project: skills-library
scope: plugin
plugin: algolia
tags: [type/reference, plugin/algolia, scope/foundational, topic/algolia, topic/integrations]
status: active
---
# Algolia integrations map — what's OOTB, what we build

Curated list of source connectors, integrations, and Marketplace surfaces. Goal: avoid building what already exists, and know which Slalom defaults we lean on.

Source: https://www.algolia.com/doc/integration

## Decision tree

```
Need to index data from {system}?
├── Algolia Ingestion has an OOTB source connector? ─→ Use it. Build only the transformation.
├── The {system} side has an Algolia plugin / app / extension? ─→ Use it. Prefer official.
└── Neither? ─→ Build a custom indexer. See `algolia-indexing-pipeline`.
```

The vast majority of what teams build by hand is already a connector. **Check first.**

## Source connectors (Ingestion API)

Configured in the Algolia Dashboard under "Data sources." Most are scheduled (cron) plus webhook-triggered for incremental changes.

| Source | Pattern | Notes |
|---|---|---|
| **Contentful** | Marketplace app + webhook | Slalom default. See `algolia-contentful-integration`. |
| **Sanity** | Plugin + webhook | Lower-volume engagements. |
| **Strapi** | Plugin + webhook | Less mature than Contentful or Sanity. |
| **Drupal** | Module | Legacy migration support. |
| **WordPress** | Plugin | Editorial-heavy sites. |
| **Shopify** | First-party connector | Storefronts. Uses Shopify webhooks for incremental sync. |
| **Shopify Plus** | First-party connector + Markets support | For multi-region storefronts. |
| **Salesforce Commerce Cloud** | LINK cartridge | Enterprise B2C commerce. |
| **Adobe Commerce (Magento)** | Extension | Enterprise B2C commerce. |
| **commercetools** | Source connector | Composable commerce. |
| **BigCommerce** | App | SMB/mid-market commerce. |
| **BigQuery** | Source connector | Analytical data warehouse → search. |
| **Snowflake** | Source connector | Same. |
| **PostgreSQL** | Source connector | Direct DB → index. |
| **MongoDB** | Source connector | NoSQL → index. |
| **Amazon S3** | Source connector | Files (CSV/JSON/NDJSON) → index. |
| **Google Drive / SharePoint** | Source connector | Document search use cases. |
| **Salesforce (CRM)** | Source connector | Knowledge / case search. |
| **Zendesk** | Source connector | Help-center search. |
| **Custom API source** | Webhook + SDK | When none of the above fit. Build per `algolia-indexing-pipeline`. |

## Front-end integrations

| Surface | Library | Default in Slalom stack |
|---|---|---|
| Site search page (composable Next.js) | `react-instantsearch` (Hooks) | ✅ Default |
| Header autocomplete | `@algolia/autocomplete-js` + plugins | ✅ Default |
| Vanilla JS site (no framework) | `instantsearch.js` | Rare for us |
| Vue / Nuxt | `vue-instantsearch` | Per-engagement |
| Angular | `angular-instantsearch` | Rare for us |
| iOS / Swift | `InstantSearch iOS` | Mobile-app engagements |
| Android / Kotlin | `InstantSearch Android` | Mobile-app engagements |
| React Native | `react-instantsearch-native` | Cross-platform mobile |

## AI / personalization integrations

| Capability | How it integrates |
|---|---|
| **NeuralSearch** | Toggle on the index settings; pass `mode: "neuralSearch"` per query. Premium plan. |
| **Personalization** | Send Insights events with `userToken`; pass `enablePersonalization: true` + `userToken` on search. |
| **Recommend** | Send Insights events; configure models in Dashboard; query `/1/indexes/*/recommendations`. |
| **Query Categorization** | Configured in Dashboard; surfaces as a search response field for routing. |
| **Vector / embeddings (OpenAI, Vertex, Bedrock)** | NeuralSearch handles the common case OOTB. For custom embeddings, use `objectAttributes` of type `vector` (private beta — check availability) or build hybrid retrieval at the application layer. |

## Analytics and observability

| Tool | How it integrates |
|---|---|
| **Algolia Analytics (built-in)** | Free/included; no setup beyond Insights events. |
| **GA4** | Send GA4 events parallel to Insights events; correlate by `queryID`. |
| **Segment** | Algolia destination + source; or a server-side translator. |
| **Datadog / New Relic** | Monitoring API; alerting on app health and latency. |
| **PagerDuty** | Webhook from Monitoring API → PD service. |

## Deployment / DevOps

| Tool | How it integrates |
|---|---|
| **GitHub Actions** | Algolia CLI for settings/rules/synonyms-as-code. |
| **GitLab CI / CircleCI / Jenkins** | Same — CLI or Node SDK. |
| **Vercel** | Serverless functions for indexers and webhooks; Edge Runtime supports `@algolia/client-search` (lite client). |
| **Cloudflare Workers** | Lite client works; bundle size matters. |
| **AWS Lambda** | Standard Node SDK. |

## What we don't recommend

- **Building a Contentful → Algolia indexer from scratch when the Marketplace app fits.** It only fits when the mapping is straightforward (1 entry → 1 record per locale). Anything more goes custom.
- **Using `algoliasearch` (full client) in the browser.** Always the lite client (`@algolia/client-search`) on the front end.
- **Indexing draft / preview content into the production index.** Use a separate `*_preview` index, route the preview environment to it.
- **One index for everything via a `type` facet.** Default to per-type indices. Federation happens at the query layer (multi-index search), not the storage layer.

## Slalom partner-portal access

The Slalom partner team has access to an Algolia partner portal with sandbox apps and credits. For new engagements:

1. Reach out to the Algolia partner team via the Slalom partner manager for a partner sandbox.
2. The sandbox covers Premium-plan features (NeuralSearch, Personalization, Recommend) without billing the engagement during prototyping.
3. Promote to a client-paid app at the end of Discover, before any production data lands.
