---
type: readme
project: skills-library
scope: plugin
plugin: product-management
tags: [type/readme, plugin/product-management]
status: active
---
# product-management

**Product management as a distinct discipline from project management — outcomes over outputs.**

Where project management plans, sequences, and ships defined work, **product management** decides *what* should be built, *whether* it should be built, and *what success means once it ships*. The two adjacent disciplines are best treated as separate plugins so neither's vocabulary erodes the other.

## Skills (1)

| Skill | What it does |
|---|---|
| [[product-management-product-manager]] | Senior product manager grounded in Perri's *Escaping the Build Trap*. Covers product strategy (Vision → Strategic Intents → Product Initiatives → Options), the Product Kata, outcomes vs. outputs, product discovery, the project-to-product org shift, and the four product roles (PM, PO, Product Lead, CPO). |

## Agent

**`product-management-agent`** — Senior product practitioner who knows when to use the product-manager skill and how to sequence it with adjacent skills (project-management, behavioral-economics, cx, design). Use when starting a product engagement or when a project-management approach is producing the build trap.

## Roadmap

Likely additions:

- `product-management-discovery` — generative research, opportunity assessment, solution design, validation experiments
- `product-management-product-leadership` — operating model, hiring, performance, the CPO role
- `product-management-product-analytics` — measurement plans, north-star metric, HEART, Pirate Metrics
- `product-management-product-operations` — Marty Cagan's product-ops practice; tooling and rituals that scale product orgs

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

**Book digests**
- **`book-escaping-the-build-trap.md`** — Perri (2018): the build trap, the four product roles, the strategy framework, the Product Kata, outcomes vs. outputs, project-vs-product org transition
- **`book-product-leadership.md`** — Banfield, Eriksson, Walkingshaw (2017): the leadership cascade (principles → vision → strategy → roadmap), three stages of product leadership style (startup / emerging / enterprise), partner ecology, hiring timing. Shared with the cx plugin.
- **`book-product-roadmaps-relaunched.md`** — Lombardo, McCarthy, Ryan, Connors (2017): theme-driven roadmap structure, components (vision, objectives, themes, timeframes, disclaimer), stakeholder filter, prioritization frameworks (Kano, DFV, ROI scorecard, MoSCoW, Cost of Delay). Shared with the cx plugin.

The plugin also pairs deeply with the project-management plugin's `book-product-development-flow.md` (Reinertsen) and `book-lean-agile-design-thinking.md` (Gothelf).

**Templates**
- **`template-product-strategy.md`** — Vision → Strategic Intents → Product Initiatives → Options; outcome-statement template; Pirate Metrics measurement plan

## Conventions

- Skills are prefixed with `product-management-` to match the plugin name
- Each skill has a `SKILL.md` with YAML frontmatter and the standard sections (Role, Methodology, Engage, Deliverables, Boundaries, Examples)
- Each skill has a `references/` folder with skill-specific deep references

## Pairs with

- **`project-management`** — sister plugin: project-management ships defined work; product-management decides what work should be defined
- **`cx`** — `cx-customer-research`, `cx-jobs-to-be-done`, `cx-journey-mapper` feed product discovery
- **`behavioral-economics`** — Slalom Behavioral Design Model integrates well with product discovery
- **`design`** — `design-product-designer` is the product-design counterpart; PM and product-design partner closely
- **`software-engineering`** — engineering executes the discovery and delivery

## Installing

```bash
# Cowork (Claude Desktop) — recommended
# Drag Plugins/product-management.plugin into Settings → Customize → Personal Plugins

# Or marketplace install (Claude Code CLI)
claude plugin install product-management@composable-dxp
```

## License

MIT.
