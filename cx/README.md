---
type: readme
project: skills-library
scope: plugin
plugin: cx
tags: [type/readme, plugin/cx]
status: active
---
# cx

**Strategic customer experience — research, jobs, personas, journeys, service design, IA, and personalization**

## Skills (9)

| Skill | What it does |
|---|---|
| [[cx-customer-research]] | Senior research expertise across qualitative and quantitative methods — interviews, focus groups, ethnography, surveys, and mixed-methods. Designs the study, writes screeners and discussion guides, runs the work, synthesizes findings. |
| [[cx-customer-feedback-synthesizer]] | Feedback analyst who transforms raw customer data — interviews, support tickets, surveys, NPS — into clear, actionable insights that drive product, design, and marketing decisions. |
| [[cx-jobs-to-be-done]] | Frames product, service, and content decisions through Jobs-to-be-Done — what the user is trying to get done, the situation they're in, the progress they want, and what's blocking them. |
| [[cx-persona-developer]] | Builds research-grounded personas — goals, frustrations, behaviors, decision contexts, and the verbatim quotes that anchor each one. Usable as inputs to product, design, content, and marketing decisions. |
| [[cx-journey-mapper]] | Builds current-state and future-state journey maps showing what the user does, thinks, and feels across stages — with pain points, moments of truth, opportunities, and channels. |
| [[cx-service-designer]] | Customer journey and service design expert who maps end-to-end experiences across digital, physical, and human touchpoints. Service blueprints and orchestration. |
| [[cx-information-architect]] | Information architect and content modeling expert. Designs taxonomies, navigation, and content models that scale across channels. Outputs feed Contentful modeling in software-engineering. |
| [[cx-product-trend-researcher]] | Trend researcher who identifies emerging signals shaping product strategy, design, and market positioning. |
| [[cx-personalization-strategist]] | Demand generation strategist obsessed with turning anonymous visitors into qualified pipeline. Personalization, segmentation, and lifecycle strategy. |

## Agent

**`cx-agent`** — Senior CX practitioner who knows when to use each skill in this plugin and how to sequence them. Use this agent when a task spans research → synthesis → mapping → service design, requires sequencing, or needs senior judgment about which skill applies.

## Shared references

Cross-skill resources live in `cx/references/`. Reach for them before duplicating content into a skill's own folder.

- [[references/cx-canon-reading-list|cx-canon-reading-list]] — the books this plugin treats as authoritative
- [[references/experience-principles|experience-principles]] — writing principles that screen ideas (Risdon, Stickdorn)
- [[references/ecosystem-mapping|ecosystem-mapping]] — actors, channels, value flows (Risdon)
- [[references/managing-complexity|managing-complexity]] — Norman's six tools for navigable complexity
- [[references/sense-and-respond-loop|sense-and-respond-loop]] — the operating model behind continuous CX (Gothelf)

## Cross-plugin references

Shared with the `product-management` plugin (canonical digests live there):

- [[book-product-leadership]] — Banfield et al.: leadership cascade, three-stage leadership style, partner ecology
- [[book-product-roadmaps-relaunched]] — Lombardo et al.: theme-driven roadmap craft, prioritization frameworks

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files (where present).
- Plugin-shared references live at `cx/references/`.
- Tone across all skills: direct, practical, builder-oriented.

## Pairs with

- **`design`** — for visual translation of journey, IA, and service design outputs
- **`software-engineering`** — `cx-information-architect` outputs feed `software-engineering-contentful-model`
- **`marketing`** — for personalization and lifecycle execution

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
