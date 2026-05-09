---
type: reference
project: skills-library
scope: plugin
plugin: algolia
tags: [type/reference, plugin/algolia, scope/foundational, topic/slalom]
status: active
---
# Slalom context — where Algolia fits

## The practice

Algolia work happens inside Slalom's **Composable DXP** practice (Enterprise capability). The default stack is **Next.js + Contentful + Algolia + Vercel**, with the search layer increasingly carrying personalization and AI-assisted discovery on top of structured content.

We position Algolia as the **discovery surface** in front of the **structured-content surface** (Contentful). The split:

- Contentful holds the source of truth for editorial and commerce content.
- Algolia holds the index that powers search, autocomplete, recommendations, and category browsing.
- The pipeline between them is governance: we decide what to index, what to denormalize, and how often to refresh.

A composable site without good search degrades to navigation-only browsing. A site with *only* good search misses people who can't articulate what they want. Both layers earn their place; this plugin is opinionated about the search half.

## Where Algolia shows up in our engagements

- **Discovery** — site search, faceted browse, autocomplete on the global header, listing pages backed by InstantSearch.
- **Recommendations** — Related, Frequently Bought Together (or read-alike for content), Trending, Looking Similar on detail pages.
- **Personalization** — Algolia Personalization for affinity-based reranking; opt-in NeuralSearch for semantic + keyword hybrid.
- **Merchandising** — query rules, pinning, redirects, banners — the levers business stakeholders ask for first and engineering teams forget to budget for.
- **Analytics** — search-side instrumentation feeding the same product-analytics pipeline as the rest of the site.

## Pursuit and delivery posture

- In **Pursuit Excellence Stage 3 (Develop)** scoping, we use the [[#The default architecture]] below as the reference design unless the client has reasons to deviate.
- In **Slalom Summit Discover phase**, search becomes its own workstream by RG2 — record design, faceting, relevance baselines, A/B plan.
- For **GTM Zero Legacy** modernization campaigns, Algolia is the canonical replacement for legacy SharePoint / on-prem Solr / SQL-LIKE search.

## Partner relationship

- Algolia is a **Slalom partner**. The partner team can help with discounts, access to early features (NeuralSearch, AI Browse), and joint-account engagement.
- We have access to Algolia's **partner portal** for sandbox apps and credits.
- **Algolia Plus / Premium plans** unlock NeuralSearch, Personalization, Recommend, and the Ingestion API source connectors. Validate the plan tier early in scoping — capability availability is plan-gated.

## The default architecture

```
Contentful (source of truth)
    │
    │  on publish/unpublish webhook
    ▼
Indexer (Vercel Function / Worker)
    │  flatten Topics & Assemblies → Algolia records
    │  hold per-type indices (articles, glossary, products, people, ...)
    │  + replicas for sort orders
    ▼
Algolia (discovery layer)
    │
    ├── Search Client → InstantSearch React (search page, listing pages)
    ├── Search Client → Autocomplete UI library (global header)
    ├── Recommend → Related / FBT / Trending widgets on detail pages
    ├── Insights events ← clicks, conversions, view events from the front end
    ├── Personalization ← user profiles built from Insights events
    └── A/B Testing ← settings / rule variants
```

Variations:
- **Per-tenant content** in B2B SaaS: same architecture + secured API keys with tenant filters.
- **Commerce on Shopify or SFCC**: use Algolia's first-party connector instead of building the indexer.
- **Heavy editorial workflow** with preview: index from Preview API entries into a separate `*_preview` index; route the preview environment to that.

## Anti-patterns we don't ship

- **Indexing entire Contentful entries with all linked references inlined**. That blows up record size, makes relevance tuning brittle, and burns operations. Flatten deliberately.
- **Building a custom indexer when the Marketplace Contentful app would do**. Use the Marketplace app for the common case; build custom only when the mapping needs editorial logic the app can't express.
- **Using one giant index with `type` faceting for everything**. Federate intentionally; per-type indices are easier to tune and replace.
- **Skipping Insights**. Without click and conversion events the Personalization and Recommend models can't train, and you can't run honest A/B tests.
- **Putting an admin / write API key in the browser**. Search-only keys exist; secured API keys exist; never ship anything else client-side.
