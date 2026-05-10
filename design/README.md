---
type: readme
project: skills-library
scope: plugin
plugin: design
tags: [type/readme, plugin/design]
status: active
---
# design

**UI / UX / web / mobile design — consumes brand outputs, feeds engineering**

## Skills (15)

### Always-on (load on every task)

| Skill | What it does |
|---|---|
| [[design-taste]] | The anti-bland foundation. Three dials (variance, motion, density), intent-first protocol, named AI tells to reject, four pre-show checks. **Loaded by every other execution skill.** |
| [[design-process]] | Always-on design discipline — nine behavioral rules plus the five-phase workflow (Define → Explore → Design → Review → Handoff) preceded by a Phase-0 calibrate step that loads design-taste. |

### Product UI authoring

| Skill | What it does |
|---|---|
| [[design-product-designer]] | Product designer persona — UX/UI specialist, design-systems architect, Figma expert. The senior generalist for product design work. |
| [[design-web-designer]] | Designs visually compelling, conversion-optimized web pages using modern CSS, Tailwind, and component-based layouts. |
| [[design-screen]] | Composes a new screen or updates an existing screen by reusing the project's design system. |
| [[design-motion-interaction-designer]] | UI animations, micro-interactions, page transitions. Loads `motion-and-transitions`; defaults to Sonner / Vaul. |
| [[design-accessibility-specialist]] | WCAG 2.2 compliance, ARIA authoring, automated testing, inclusive design. |

### Surface-specific authoring (new in v0.4)

| Skill | What it does |
|---|---|
| [[design-presentation]] | 16:9 deck design (PowerPoint, Keynote, Gamma, Google Slides). Slide-system thinking, density discipline. Hands off to `pptx`. |
| [[design-document]] | 8.5×11 long-form (whitepaper, POV, brief, playbook, case study). Editorial design — cover system, pull quotes, callouts, citations. Hands off to `docx` / `pdf`. |
| [[design-social-asset]] | LinkedIn (1200×627, 1200×1200, 1080×1080, 1080×1350) and Instagram (1080×1080, 1080×1350, 1080×1920) posts, carousels, stories. |
| [[design-iconography]] | Icon family selection (Lucide, Heroicons, Phosphor, Tabler, Flaticon), stroke discipline, fill-vs-outline rules, sizing scale. |
| [[design-visualization]] | Diagrams (Mermaid, napkin.ai), charts (shadcn / Recharts), commissioned visuals — routes by job. |
| [[design-imagery]] | Generated (higgsfield: nano banana 2 stills, seedance 2 video), commissioned, stock; art-direction language. |

### Operational

| Skill | What it does |
|---|---|
| [[design-audit]] | Read-only audit of a screen, component, or file for design-system drift. |
| [[design-handoff]] | Generates complete, dev-ready handoff annotations. |

## Agent

**`design-agent`** — Senior design practitioner who knows when to use each skill in this plugin and how to sequence them. Use this agent when a task spans multiple skills (audit → screen → handoff), requires sequencing, or needs senior judgment about which skill applies.

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

**Foundations:**
- **`slalom-context.md`** — Slalom organizational context, service pillars, MACH/composable priorities, Composable DXP practice context.
- **`slalom-brand-tokens.md`** — Working set for Slalom-branded design tasks: color tokens, type, voice, motion, iconography defaults. Always-on for Slalom-instance design work. Canonical reference: `50_Knowledge/Frameworks/Slalom_Brand/`.
- **`composable-dxp-brand-tokens.md`** — Working set for the practice's TFIC sub-brand (`thefutureiscomposable.com`, Composable Roadshow, CMO Playbook, Insights). Diverges intentionally from Slalom parent — em-dash banned, italics-in-display banned, lavender primary. Canonical reference: `50_Knowledge/Frameworks/Composable_DXP_Brand/`.
- **`visual-design-foundations.md`** — Static-side principles: hierarchy, color, spacing, proximity, golden ratio, border radius, depth, typography, and design-system thinking.
- **`interaction-patterns.md`** — Dynamic-side principles: state design, focus management, dismissal patterns, loading states, motion and easing, touch targets, empty states, persuasion vs. dark patterns.

**Taste layer (anti-bland):**
- **`stylistic-vocabulary.md`** — Named visual languages with what each lands and when to reach for it.
- **`ai-tells-forbidden-patterns.md`** — Named AI default patterns to reject across surfaces.
- **`pre-delivery-checklist.md`** — Priority-ordered named checks every artifact passes before shipping.

**Motion (added in Session 1.5):**
- **`motion-and-transitions.md`** — The named-pattern catalog for interaction motion. Animation Decision Framework, named bans (scale(0), ease-in, transition: all, keyframe-on-rapid-trigger), component-specific patterns (Sonner, Vaul, modals, tabs, skeletons), CSS transform mastery, springs, gestures, performance. Adapted from Emil Kowalski and Hyperframes.

**Surface specs (added in Session 2):**
- **`surface-deck-16x9.md`** — 16:9 presentation deck format spec.
- **`surface-document-letter.md`** — 8.5×11 document format spec.
- **`surface-social-linkedin.md`** — LinkedIn surface inventory and engagement priorities.
- **`surface-social-instagram.md`** — Instagram surface inventory and format specs.

**Tool protocols (added in Session 2):**
- **`tool-flaticon.md`** — Flaticon sourcing protocol for non-UI iconography.
- **`tool-napkin-ai.md`** — napkin.ai workflow for generative concept diagrams.
- **`tool-higgsfield.md`** — higgsfield image/video generation workflow (nano banana 2, seedance 2).

**shadcn ecosystem inventories (single source of truth, also cited by software-engineering plugin):**
- **`shadcn-component-inventory.md`** — Complete catalog of shadcn/ui primitives, grouped by job.
- **`shadcn-block-inventory.md`** — Free official shadcn blocks.
- **`shadcn-chart-inventory.md`** — All seven shadcn chart categories with consulting-deliverable defaults.
- **`shadcndesign-pro-blocks-inventory.md`** — The paid 343-block catalog from shadcndesign.com.

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files (where present).
- The five operational task skills (`design-taste`, `design-process`, `design-screen`, `design-audit`, `design-handoff`) are workflow-shaped — load them before doing the corresponding task. `design-taste` and `design-process` load *together* on every task.
- Tone across all skills: direct, practical, builder-oriented.

## Pending expansion

Session 3 (queued separately): brand plugin restructure — see `../Brand_and_Design_Expansion_Plan.md`.

## Pairs with

- **`brand`** — consumes brand strategy and identity outputs
- **`software-engineering`** — `design-handoff` and `design-screen` outputs feed `software-engineering-design-implementation` and `software-engineering-frontend-developer`
- **`cx`** — journey maps, personas, and IA inputs from cx land here

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
