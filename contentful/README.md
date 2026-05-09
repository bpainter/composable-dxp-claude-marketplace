---
type: readme
project: skills-library
scope: plugin
plugin: contentful
tags: [type/readme, plugin/contentful]
status: active
---
# contentful

**Platform-implementation skills for Contentful-based composable DXPs**

Sits adjacent to `software-engineering`. The software-engineering plugin covers the language, framework, and architecture work; this plugin covers the platform-specific Contentful implementation surface. Most engagements pull from both.

## Skills (12)

### Information architecture and modeling

| Skill | What it does |
|---|---|
| [[contentful-content-model]] | Designs modular, governance-friendly content types and relationships using the Topics & Assemblies pattern. Where every Contentful project starts. |
| [[contentful-localization]] | Locale strategy, fallback chains, field-level localization, translation workflow, locale-aware querying. |
| [[contentful-rich-text]] | Renders Contentful Rich Text into React with embedded entries/assets, custom marks, and design-system mapping. |

### Implementation

| Skill | What it does |
|---|---|
| [[contentful-react-wrapper]] | Wires React components to Contentful across preview and production — typed clients, GraphQL fragments, on-demand revalidation. |
| [[contentful-graphql]] | Designs GraphQL Content API queries — fragments, polymorphic unions for assemblies, complexity budget, codegen. |
| [[contentful-delivery-optimization]] | Performance work — Delivery vs. Preview, advanced caching, the Sync API, the Images API, CDN-aware fetch patterns. |

### Platform operations

| Skill | What it does |
|---|---|
| [[contentful-space-architect]] | Spaces, environments, environment aliases, cross-space references, deployment-pipeline best practices. |
| [[contentful-migrations]] | Reproducible CMA migrations using the Contentful CLI / contentful-migration. Dry runs, env promotion, rollback. |
| [[contentful-webhooks]] | Webhook configuration, content events, HMAC signing, filters and transformations, retry behavior. |

### Extensibility, personalization, agentic workflows

| Skill | What it does |
|---|---|
| [[contentful-app-framework]] | Builds custom UI extensions — app definitions, locations, App SDK, hosting, distribution. |
| [[contentful-personalization]] | Contentful Personalization (former Ninetailed) — audiences, experiences, A/B, edge personalization patterns. |
| [[contentful-mcp-cli]] | Practical Contentful MCP server and CLI workflows — when to reach for which, agent-led modeling, scripted ops, safety rails. |

## Agent

**`contentful-agent`** — Senior platform engineer who knows when to use each skill in this plugin and how to sequence them across model, implementation, ops, and extensibility. Use this agent when a task spans multiple skills (e.g., new content type → model → migrate → wire → revalidate), requires sequencing, or needs senior judgment about which skill applies.

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

- **`slalom-context.md`** — Slalom organizational context, Composable DXP practice focus, partner relationship with Contentful
- **`contentful-foundations.md`** — Domain model, data model, links, environments, locales — the concepts that show up in every skill
- **`api-surface.md`** — One-page map of all eight APIs (Delivery, Preview, Management, GraphQL, Images, User Management, SCIM, Sync) with when-to-use guidance
- **`cli-cheat-sheet.md`** — Contentful CLI commands grouped by workflow (auth, space/env management, content export/import, migration scripts)
- **`mcp-cheat-sheet.md`** — Contentful MCP server tool surface and the high-leverage agent prompts that map to it
- **`common-content-models.md`** — Live reference of all 44 content types in the Slalom Demo 2.0 space, with patterns worth copying
- **`marketplace-apps.md`** — Curated map of OOTB Contentful Marketplace apps and webhook templates (DAM, TMS, search, personalization, AI, deploy hooks, etc.) for Slalom defaults

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files where the topic warrants depth.
- Tone across all skills: direct, builder-oriented, opinionated. Match Bermon's `preferences.md`.
- All skills assume **Slalom's Composable DXP practice** is the operating context — Next.js + Contentful + Vercel is the default stack; alternatives only when called out.

## Pairs with

- **`software-engineering`** — `software-engineering-composable-architect` sets system shape; `software-engineering-frontend-developer`, `-nextjs-scaffold`, `-shadcn-component`, and `-pro-blocks-composer` consume Contentful via this plugin's wrapper / GraphQL / rich-text skills
- **`design`** — `design-handoff` and `design-screen` outputs feed into the wrapper layer
- **`cx`** — `cx-information-architect` outputs feed `contentful-content-model`
- **`project-management`** — sprint and user-story outputs drive engineering execution against Contentful

## Part of the second-brain marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
