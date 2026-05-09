---
name: algolia-index-design
description: >
  Index and record design for Algolia — designing the JSON record shape that powers search, deciding searchable vs. faceted vs. displayed attributes, choosing per-type indices vs. one federated index, structuring distinct/grouping for variant collapse, and architecting replicas (standard vs. virtual) for sort orders. Use this skill any time the user is starting a new Algolia integration, adding a new content type to an existing index, refactoring records that have gotten bloated, deciding between one index or many, or planning sort orders. The schema decisions here propagate into every other skill — get them right and ranking, faceting, and indexing pipelines fall into place.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-index-design]
tags: [type/skill, plugin/algolia, topic/algolia, topic/information-architecture]
status: active
---

This skill puts you in the role of a senior search engineer designing the record shape and index topology before anyone tunes a single setting. Default posture: **records are flat denormalized projections of the source content shaped for search, not copies of the source.**

For Slalom Composable DXP work, this is where Phase 1 of every search workstream concentrates. A bad record shape forces every other skill to fight upstream — relevance tuning compensates for missing attributes, indexing pipelines bloat with unnecessary fields, the front end does runtime joins it shouldn't.

Pair with `algolia-relevance-tuning` for what to do once the records exist, `algolia-indexing-pipeline` for how to populate them, `algolia-contentful-integration` if Contentful is the source, and `cx-information-architect` for the upstream IA decisions on facets and refinement.

## When to use this skill

- New Algolia integration — what records to create, what indices, what attributes.
- Adding a new content type to an existing index, or deciding it deserves its own index.
- Records have gotten too big (>10 KB free / >100 KB paid) — flattening strategy.
- Choosing between **one index with `type` faceting** vs. **per-type indices**.
- Designing sort orders — when to use virtual replicas, when standard.
- Planning collapse / `distinct` for products with variants, or articles with multi-locale.
- Reviewing someone's proposed schema for relevance, faceting, and operational health.
- Migrating from a legacy search index where the record shape no longer fits.

## Core posture

- **Records are search projections, not source copies.** Re-shape for search. Strip what won't be queried, faceted, or displayed.
- **Per-type indices by default.** Federate at query time, not at storage time.
- **Faceting is a first-class design concern.** A facet is a feature; it doesn't appear by accident.
- **Flat over nested.** Algolia ranks against attributes, not arbitrarily nested structures. Flatten.
- **Idempotent objectIDs.** Reindex must be a no-op when nothing has changed. Use a stable source ID.
- **Plan for change.** Adding an attribute is cheap. Removing is cheap. Renaming requires a migration. Pick names you won't regret.

## The three lenses on every attribute

For each attribute on the record, decide three things explicitly:

| Lens | Question | Setting |
|---|---|---|
| **Search** | Should this attribute be matched against the query? | `searchableAttributes` |
| **Filter / Facet** | Should users / the front end filter by this? | `attributesForFaceting` |
| **Display** | Will this be rendered in the hit / refinement / detail? | included on the record (else strip it) |

An attribute that fails all three doesn't belong on the record.

## Record shape — patterns

### Topic-style record (article, glossary term, person, event)

```json
{
  "objectID": "article-2026-composable-dxp-talk",
  "type": "article",

  // Searchable, in priority order
  "title": "What 'composable' really means in 2026",
  "summary": "Three years in, the patterns have hardened. Here's what we ship.",
  "body_plaintext": "Composable architectures decouple…", // truncated, no HTML

  // Faceted
  "topics": ["composable", "dxp", "architecture"],
  "audience": ["technology-leader"],
  "stage": ["awareness", "consideration"],
  "author_id": "person-bermon-painter",
  "publishedAt_unix": 1730000000,
  "locale": "en-US",

  // Displayed
  "slug": "what-composable-really-means-in-2026",
  "url": "/articles/what-composable-really-means-in-2026",
  "image": "https://images.ctfassets.net/.../hero.jpg?w=600&q=80",
  "author_name": "Bermon Painter",

  // Custom ranking
  "popularity": 142,
  "freshness_score": 0.84
}
```

Notes on what's *not* there:
- No HTML in `body_plaintext`. Strip tags upstream.
- No giant `body`. Truncate at ~5 KB; long-tail relevance comes from title and summary.
- No nested `author: { name, role, bio }`. Flatten to `author_id` (filter), `author_name` (display).
- No raw `publishedAt` ISO string in addition to `publishedAt_unix`. Pick one. Unix int is faster to filter and sort.

### Commerce-style record (product, service, package)

```json
{
  "objectID": "sku-12345-color-blue-size-l",
  "type": "product",

  "name": "Cloud-Build engagement starter pack",
  "description": "...",
  "brand": "Slalom",

  "categories": ["services", "cloud-build"],
  "categories_lvl0": "services",
  "categories_lvl1": "services > cloud-build",
  "tags": ["aws", "azure", "discovery"],

  "color": "blue",
  "size": "L",
  "price": 49500,            // integer cents to avoid float quirks
  "price_currency": "USD",
  "in_stock": true,
  "rating": 4.6,
  "rating_count": 23,

  "image": "...",
  "url": "/products/sku-12345",

  "popularity": 1250,
  "_distinct_key": "sku-12345"  // collapses color/size variants into one hit
}
```

Notes:
- Hierarchical facets via `categories_lvl{N}` (Algolia's standard pattern).
- `_distinct_key` plus `attributeForDistinct: '_distinct_key'` in settings collapses variants. Set `distinct: 1` (or `distinct: 3` if you want up to 3 variants shown).
- Currency stored as integer cents.

### People / directory record

```json
{
  "objectID": "person-bermon-painter",
  "type": "person",
  "name": "Bermon Painter",
  "name_search": "Bermon Painter Senior Director Composable DXP",  // synonyms / role for typo-friendly search
  "role": "Senior Director, Market Solutions",
  "capabilities": ["enterprise"],
  "practice": "Composable DXP",
  "city": "Charlotte",
  "skills": ["next-js", "contentful", "vercel", "design-systems"],
  "image": "...",
  "url": "/people/bermon-painter"
}
```

## Per-type indices vs. one index

Default: **per-type indices**. `articles`, `glossary_terms`, `people`, `products`. Why:

- Each can have its own `searchableAttributes` priority. Articles want title-first; products want SKU-first.
- Each can have its own `customRanking`. Articles → `desc(publishedAt_unix)`. Products → `desc(popularity)`.
- Replacing one type's records doesn't risk the others.
- A failed indexer for one type can't take down the whole search.

Use **one index** only when:

- The volume per type is tiny (<5k records total) and the operational simplicity wins.
- Cross-type ranking is the explicit feature (e.g., a unified "answer" search where the answer might be a glossary term, an article, or an FAQ — all ranked together).
- A specific Marketplace integration locks you into a single-index pattern.

For per-type indices, the front end uses **multi-index search** (`<Index>` widgets in InstantSearch, or `multipleQueries` directly) for federation.

## Indices per environment

- `articles` (prod), `articles_staging`, `articles_dev` — or
- separate Algolia *applications* per environment (`slalom-prod`, `slalom-staging`).

**Prefer separate applications.** Per-environment indices in one app share rate limits, share keys, share analytics, and share the blast radius of a `clearIndex` mistake. Separate applications cost a tiny bit more in plan slots, but the isolation is worth it.

For preview environments (Contentful Preview API draft content), use a separate `*_preview` index inside the staging app, populated from drafts.

## Replicas — virtual by default

Sort orders need a replica because the primary index ranks by relevance.

```json
{
  "replicas": [
    "virtual(articles_publishedAt_desc)",
    "virtual(articles_popularity_desc)"
  ]
}
```

Then on the replica:

```json
{
  "customRanking": ["desc(publishedAt_unix)"]
}
```

Standard replicas (just the bare name in the array) are full physical copies — they multiply your record count by N. Only reach for them when the replica needs different `searchableAttributes` or independent settings beyond `customRanking`.

## Distinct and grouping

`distinct` collapses near-duplicate records into one hit. Use it for:

- Product variants (color/size of the same SKU).
- Multi-locale records of the same content.
- Article series (one hit per series instead of one per part).

Setup:

```json
{
  "attributeForDistinct": "_distinct_key",
  "distinct": 1
}
```

`distinct: 3` shows up to 3 variants per group, useful for products where you want to surface multiple sizes. `distinct: true` is equivalent to `1`.

The grouping is post-ranking, so the *best* record per `_distinct_key` is kept.

## Locales — fan out, don't squeeze

For multi-locale content, **one record per (entry, locale)**. `objectID` becomes `{entryId}-{locale}`, and a `locale` attribute holds `en-US` / `fr-FR` / etc. Then the front end filters `locale:en-US` on every query.

Don't try to put all locales into one record with localized fields nested. The relevance engine treats them all as the same document; English queries match French content; ranking gets weird. Fan out.

For very large locale sets (>10), consider one index per locale (`articles_en_us`, `articles_fr_fr`). The trade-off is operational: more indices to manage, but each is smaller and faster.

## Attribute taxonomy

A common attribute-naming convention to enforce consistency:

| Pattern | Use |
|---|---|
| `name`, `title`, `summary`, `description` | searchable text content |
| `*_plaintext` | stripped-HTML version of rich text |
| `*_unix` | numeric Unix timestamp — for filtering / sorting |
| `*_lvl0`, `*_lvl1`, `*_lvl2` | hierarchical facet levels |
| `*_id` | reference IDs (filter only, usually) |
| `*_count` / `popularity` / `rating` | numeric custom-ranking signals |
| `image`, `url`, `slug` | display only |
| `_distinct_key`, `_tags` | Algolia conventions |

## Sizing and limits

- **Record size**: <10 KB free, <100 KB paid. Truncate body text aggressively.
- **Records per index**: practically unlimited (millions are fine), but each search scans the whole index — keep records lean.
- **Attributes**: no hard limit, but >50 is a smell. Audit.
- **`searchableAttributes` priority list**: keep it tight. The further down the list, the lower the boost.

## Output formats

### Index design proposal

```
# Algolia Index Design Proposal: [Project]

## Indices (per environment)
For each:
- Name (e.g., articles, glossary_terms)
- Source content type
- Approximate record count
- Records-per-day churn (for capacity planning)

## Record shape (per index)
For each:
- objectID derivation
- Full JSON example
- Attribute table: name | type | search? | facet? | display? | custom-rank? | source field

## Replicas
For each replica:
- Name | virtual/standard | customRanking

## Distinct / grouping
- attributeForDistinct, distinct value, rationale

## Locale strategy
- Fan-out approach, locale facet

## Migration plan (if replacing existing search)
- Backfill source, atomic-replace cadence

## Open questions
- Any non-obvious choices that need stakeholder sign-off
```

### Attribute matrix (for the kickoff doc)

```
| Attribute       | Type    | Searchable | Facet | Display | Custom Rank | Source                     |
|-----------------|---------|------------|-------|---------|-------------|----------------------------|
| objectID        | string  | —          | —     | —       | —           | derived: {sys.id}-{locale} |
| title           | string  | ordered=1  | —     | yes     | —           | fields.title               |
| summary         | string  | ordered=2  | —     | yes     | —           | fields.summary             |
| body_plaintext  | string  | ordered=3  | —     | —       | —           | strip(fields.body)         |
| topics          | array   | unordered  | yes   | yes     | —           | fields.topics[]            |
| publishedAt_unix| number  | —          | —     | yes     | desc        | epoch(fields.publishedAt)  |
| popularity      | number  | —          | —     | —       | desc        | analytics                  |
| locale          | string  | —          | yes (filterOnly) | yes | —    | sys.locale                 |
```

## Common anti-patterns

- **Mirroring source structure 1:1.** Records that look like the CMS entry are not search records.
- **Including HTML in indexed text.** Strip tags upstream; `<em>` highlighting comes from Algolia, not from your content.
- **Storing the entire body.** Truncate. Title + summary + first ~5 KB of body is plenty for relevance.
- **Free-text tags.** Will fragment to dozens of variants of the same tag in a quarter. Use a controlled vocabulary upstream.
- **Forgetting `_distinct_key`.** Then variants flood the SERP.
- **Per-environment indices in one app.** Prefer separate apps.
- **Standard replica when virtual would do.** You're paying for nothing.
- **`searchableAttributes: []` (default).** Means *everything is searchable equally*, which is rarely what you want.

## Constraints

- Don't design records before you've seen real source content. Sketch on paper, then validate against actual entries.
- Don't introduce attributes the front end won't use. Each one is operational cost.
- Don't put PII or auth-restricted content in records intended for a search-only key. If access control is needed, plan secured API keys (`algolia-api-keys-security`) up front.
- Don't skip the `objectID` design conversation. Bad IDs = unsafe reindexes.

## How this skill relates to others

- The settings that operate on this record shape → `algolia-relevance-tuning`.
- Getting the records into the index → `algolia-indexing-pipeline`.
- The Contentful → Algolia mapping when the source is Contentful → `algolia-contentful-integration`.
- The front-end consumption of this index → `algolia-instantsearch-react`, `algolia-search-client`, `algolia-autocomplete`.
- Multi-tenant filtering on top of these records → `algolia-api-keys-security`.

## Source material

- Records and indices: https://www.algolia.com/doc/guides/sending-and-managing-data/prepare-your-data/in-depth/what-is-in-a-record/
- Searchable attributes: https://www.algolia.com/doc/guides/managing-results/must-do/searchable-attributes/
- Hierarchical facets: https://www.algolia.com/doc/guides/managing-results/refine-results/faceting/in-depth/facet-hierarchical/
- Replicas: https://www.algolia.com/doc/guides/managing-results/refine-results/sorting/in-depth/replicas/
- Distinct: https://www.algolia.com/doc/guides/managing-results/refine-results/grouping/
- Plugin reference: `../../references/algolia-foundations.md`
- Plugin reference: `../../references/api-surface.md`
