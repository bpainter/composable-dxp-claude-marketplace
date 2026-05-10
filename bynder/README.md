---
type: readme
project: skills-library
scope: plugin
plugin: bynder
tags: [type/readme, plugin/bynder]
status: active
---
# bynder

**Platform-implementation skills for Bynder DAM-based composable DXPs**

Sits adjacent to `contentful` and `software-engineering`. The software-engineering plugin covers language, framework, and architecture work; the contentful plugin covers headless-CMS implementation; this plugin covers the platform-specific Bynder DAM surface. Most of our composable engagements pull Bynder + Contentful together; some pursuits are Bynder-only (greenfield DAM stand-up or optimization of an existing Bynder implementation).

## Skills (12)

### Information architecture and asset modeling

| Skill | What it does |
|---|---|
| [[bynder-asset-model]] | Designs the metaproperty taxonomy, asset structure, and naming conventions that make assets findable, governable, and integration-ready. Where every Bynder project starts. |
| [[bynder-brand-guidelines]] | Brand Guidelines module setup — sections, do/don't, downloadables, governance over guideline updates. |
| [[bynder-derivatives]] | Derivative template strategy — what sizes, formats, and crops to pre-render vs. generate on demand via the on-the-fly transformations. |

### Implementation

| Skill | What it does |
|---|---|
| [[bynder-js-sdk]] | Practical patterns with `@bynder/bynder-js-sdk` — auth flows, asset queries, uploads, bulk operations, error handling, retry. |
| [[bynder-compact-view]] | Embeds the Universal Compact View (UCV) asset picker into custom apps, Contentful, Salesforce, and bespoke editors. |
| [[bynder-contentful-pairing]] | The most common Slalom pattern — Bynder DAM + Contentful CMS. Asset references, the OOTB connector, custom field controls, derivative selection at the CMS layer. |

### Platform operations

| Skill | What it does |
|---|---|
| [[bynder-portal-architect]] | Portal setup, sub-portals, multi-brand, custom domains, environments, and the dev/test/prod split when a client has multiple Bynder accounts. |
| [[bynder-permissions-workflow]] | Permission profiles, asset-level security, workflow stages, review/approval, scheduled publishing, the asset lifecycle. |
| [[bynder-webhooks-events]] | Webhook configuration for asset events (added, updated, deleted, metadata changed), HMAC signing, retry behavior, downstream cache invalidation. |

### Extensibility, migration, optimization

| Skill | What it does |
|---|---|
| [[bynder-marketplace-connectors]] | OOTB Bynder Marketplace connectors — Contentful, Adobe Creative Cloud, Salesforce, Sitecore, WordPress, Drupal, Hootsuite, Workfront. When to use OOTB vs. custom. |
| [[bynder-migration]] | Migrating into Bynder from another DAM (or from "shared drive chaos"). Bulk upload via API, metadata mapping, rollback strategy, editor coordination. |
| [[bynder-optimization-audit]] | The optimization-engagement pattern — auditing an existing Bynder implementation against a health rubric (taxonomy, derivatives, permissions, integrations, adoption) and producing a remediation plan. |

## Agent

**`bynder-agent`** — Senior DAM platform engineer who knows when to use each skill in this plugin and how to sequence them across model, implementation, ops, integration, and optimization. Use this agent when a task spans multiple skills (e.g., greenfield Bynder + Contentful stand-up, or an optimization audit that touches taxonomy, derivatives, and permissions), requires sequencing, or needs senior judgment about which skill applies.

## Commands (slash entry points)

Fast routes into the most-used workflows. Each command invokes the right skill or skill sequence; see the file in `commands/` for the full prompt.

| Command | What it does |
|---|---|
| `/audit` | Run the eight-metric optimization audit on an existing Bynder implementation. The headline command for optimization engagements. |
| `/bootstrap` | Greenfield stand-up — portal architecture, asset model, brand guidelines, derivatives, permissions, integrations, webhooks. |
| `/pair` | Wire up Bynder + Contentful pairing — install OOTB app, configure both sides, design the asset-reference shape, plan webhook-driven sync. |
| `/sweep` | Quarterly hygiene pass on a Slalom-built deployment — run the rubric, fix drift, record the delta. |
| `/migrate` | Plan and execute a migration into Bynder from another DAM or shared-drive chaos — extraction, mapping, throttled bulk import, downstream rewriting. |
| `/connector-check` | Audit Marketplace connector coverage — what's installed, what's actively used, what Tier 1 adoption drivers are missing. |

Use `/audit` first when the engagement shape is unclear or the user says any version of "we have Bynder but it isn't working." Use `/bootstrap` for greenfield. Use `/sweep` for ongoing health on a deployment that already has a baseline.

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

- **`slalom-context.md`** — Slalom organizational context, Composable DXP practice focus, partner relationship with Bynder, the two engagement shapes (greenfield vs. optimization)
- **`bynder-foundations.md`** — Domain model, data model, asset/metaproperty/taxonomy/derivative concepts — the things every skill assumes
- **`api-surface.md`** — One-page map of the Bynder API surfaces (Asset Bank, Workflow, Analytics, Brand Guidelines, Compact View) with when-to-use guidance and OAuth2 specifics
- **`sdk-cheat-sheet.md`** — `bynder-js-sdk` quick-reference — auth, list/get/upload/update/delete, error handling, the patterns we reach for first
- **`marketplace-connectors.md`** — Catalog of Bynder Marketplace OOTB connectors with use cases, gotchas, and the build-vs-buy decision

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files where the topic warrants depth.
- Tone across all skills: direct, builder-oriented, opinionated. Match Bermon's `preferences.md`.
- All skills assume **Slalom's Composable DXP practice** is the operating context — Bynder + Contentful + Next.js + Vercel is the default stack; the optimization-engagement pattern is treated as a first-class shape, not a corner case.

## Engagement shapes

Three distinct shapes show up; the agent and skills explicitly recognize each:

1. **Bynder-with-Contentful (greenfield)** — most common. Bynder is the DAM, Contentful is the CMS, the OOTB Bynder–Contentful connector or custom Compact View bridges them. Sequence runs through `bynder-portal-architect` → `bynder-asset-model` → `bynder-derivatives` → `bynder-contentful-pairing`.
2. **Bynder-only implementation (greenfield)** — Bynder stands up as DAM ahead of (or independent of) the headless CMS choice. Same upstream sequence; downstream goes through `bynder-js-sdk` for whatever channel needs assets.
3. **Bynder optimization (existing implementation)** — Bynder is in place but underperforming. `bynder-optimization-audit` runs first and routes findings to the relevant remediation skills (`bynder-asset-model` for taxonomy decay, `bynder-derivatives` for output sprawl, `bynder-permissions-workflow` for access drift, `bynder-marketplace-connectors` for connector misconfig).

## Pairs with

- **`contentful`** — most engagements pair the two; `bynder-contentful-pairing` is the bridge skill
- **`software-engineering`** — `software-engineering-composable-architect` sets the system shape; `software-engineering-frontend-developer` consumes Bynder assets via `bynder-js-sdk` and `bynder-compact-view`
- **`design`** — `design-handoff` outputs feed derivative-template decisions
- **`cx`** — `cx-information-architect` outputs feed asset-model and metaproperty design upstream
- **`project-management`** — adoption metrics from `bynder-optimization-audit` feed practice-level engagement tracking

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
