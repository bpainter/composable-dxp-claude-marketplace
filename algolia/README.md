---
type: readme
project: skills-library
scope: plugin
plugin: algolia
tags: [type/readme, plugin/algolia]
status: active
---
# algolia

**Platform-implementation skills for Algolia-powered search experiences in composable DXPs**

Sits adjacent to `contentful` and `software-engineering`. The contentful plugin owns the content surface; this plugin owns the search surface. On almost every Composable DXP engagement the two ship together — Contentful holds the source of truth, Algolia powers the discovery layer.

## Skills (12)

### Index design and relevance

| Skill | What it does |
|---|---|
| [[algolia-index-design]] | Designs the record shape, searchable/faceted/displayed attributes, distinct/grouping, replicas, and virtual replicas. The schema decisions that everything else inherits. |
| [[algolia-relevance-tuning]] | Ranking and custom ranking, typo tolerance, synonyms, query rules (merchandising, redirects, pinning), A/B testing, NeuralSearch hybrid relevance. |

### Implementation

| Skill | What it does |
|---|---|
| [[algolia-instantsearch-react]] | React InstantSearch and Next.js Hooks — server-rendered search pages, routing, refinement UI, infinite scroll, Federated Search. The default front-end skill. |
| [[algolia-autocomplete]] | The Algolia Autocomplete UI library — query suggestions, multi-source autocomplete, recent searches, redirects. Distinct from InstantSearch. |
| [[algolia-search-client]] | Direct search-client usage when InstantSearch isn't the right fit — RSC search, server actions, edge handlers, custom UIs, multi-index queries. |

### Indexing and ops

| Skill | What it does |
|---|---|
| [[algolia-indexing-pipeline]] | Getting data in — full reindex vs. partial updates, atomic reindex with `replaceAllObjects`, streaming pipelines, the Contentful → Algolia path. |
| [[algolia-api-keys-security]] | API-key strategy, secured API keys for multi-tenant filtering, IP restrictions, rate limits, token rotation. The thing that bites teams in production. |
| [[algolia-mcp-cli]] | Algolia MCP server and CLI workflows — when to reach for which, agent-led indexing, scripted ops, safety rails. |

### AI, recommendations, analytics

| Skill | What it does |
|---|---|
| [[algolia-recommend]] | The Recommend API — Related Items, Frequently Bought Together, Trending, Looking Similar. Models, training data, fallbacks. |
| [[algolia-personalization-ai]] | Personalization, NeuralSearch (vector + keyword hybrid), AI Browse, Query Categorization. The AI surface and what it costs. |
| [[algolia-analytics-events]] | Insights API events (click, view, conversion), event taxonomy, A/B test setup, Search Analytics, attribution to revenue. |

### Integrations and extensibility

| Skill | What it does |
|---|---|
| [[algolia-contentful-integration]] | The Contentful ↔ Algolia integration patterns we ship by default — Marketplace app vs. webhook vs. custom pipeline, record shape from Topics & Assemblies, locale and preview handling. |

## Agent

**`algolia-agent`** — Senior platform engineer who knows when to use each skill in this plugin and how to sequence them across index → relevance → implementation → indexing → ops → AI/personalization. Use this agent when a task spans multiple skills (e.g., new search experience → index design → indexer → InstantSearch UI → events → A/B), requires sequencing, or needs senior judgment about which skill applies.

## Slash commands

Five commands for the canonical engagement lifecycle. Each delegates to the agent and sequences the right skills behind the scenes.

| Command | When to run |
|---|---|
| `/algolia-bootstrap` | First Algolia integration on a new engagement — apps, indices, keys, settings-as-code, indexer, Insights baseline |
| `/algolia-audit` | ~30 days post-bootstrap, then quarterly — health check across Insights, settings drift, key hygiene, plan-tier fit |
| `/algolia-relevance-review` | Quarterly per index, or on-demand when stakeholders report "search is bad" — produces synonym/rule/settings proposals + A/B plan |
| `/algolia-reindex` | Schema migrations, drift repair, legacy-search migrations — guided atomic reindex with snapshot, non-prod validation, rollback |
| `/algolia-keys-rotate` | Staff offboarding, suspected leak, scheduled rotation, post-incident — zero-downtime key rotation runbook |

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

- **`slalom-context.md`** — Slalom organizational context, Composable DXP practice focus, where Algolia shows up in our engagements, and partner relationship
- **`algolia-foundations.md`** — Domain model, applications, indices, records, settings, replicas — the concepts that show up in every skill
- **`api-surface.md`** — One-page map of all Algolia APIs (Search, Insights, Recommend, Analytics, Monitoring, Ingestion, Personalization, A/B Testing, Query Suggestions) with when-to-use guidance
- **`cli-cheat-sheet.md`** — Algolia CLI commands grouped by workflow (auth, index ops, settings, rules, dictionaries, secured keys)
- **`mcp-cheat-sheet.md`** — Algolia MCP server tool surface and the high-leverage agent prompts that map to it
- **`integrations-map.md`** — Curated map of Algolia integrations, source connectors, and Marketplace surfaces (Contentful, Sanity, Shopify, Salesforce Commerce, Adobe Commerce, Vertex/Bedrock, etc.) for Slalom defaults

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files where the topic warrants depth.
- Tone across all skills: direct, builder-oriented, opinionated. Match Bermon's `preferences.md`.
- All skills assume **Slalom's Composable DXP practice** is the operating context — Next.js + Contentful + Algolia + Vercel is the default stack; alternatives only when called out.

## Pairs with

- **`contentful`** — `contentful-content-model` defines the source of truth; `algolia-contentful-integration` and `algolia-indexing-pipeline` move it into search; `contentful-webhooks` triggers reindex
- **`software-engineering`** — `software-engineering-frontend-developer`, `-nextjs-routing`, `-nextjs-cache`, `-nextjs-server-client` consume Algolia via this plugin's InstantSearch / Autocomplete / search-client skills
- **`design`** — `design-handoff` outputs feed the InstantSearch UI components
- **`marketing`** — SEO/GEO and analytics pair with `algolia-analytics-events` for full-funnel attribution
- **`cx`** — `cx-information-architect` outputs feed `algolia-index-design` (faceting and refinement design)

## Part of the second-brain marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
