---
type: readme
project: skills-library
scope: plugin
plugin: innovation
tags: [type/readme, plugin/innovation]
status: active
---
# innovation

**Innovation strategy, portfolio governance, value engineering, discovery, validation, monetization, lab design, disruption, leadership, and digital transformation**

## Skills (11)

| Skill | What it does |
|---|---|
| [[innovation-strategist]] | Top-of-funnel architect — mandate, ambition mix across Three Horizons, operating model (line vs. lab, stage-gate vs. discovery-driven), cadence, top-line metrics. The 18-month roadmap. |
| [[innovation-portfolio-architect]] | Translates the strategist's mandate into working governance — gates, allocation logic, kill discipline, learning vs. outcome metrics, calendar, dashboard. |
| [[innovation-value-engineer]] | Quantifies value across horizons — NPV/DCF for H1, expected value for H2, real options for H3. Kill economics, learning ROI, portfolio value roll-up. The math the CFO needs. |
| [[innovation-discovery-coach]] | Front-of-funnel — JTBD switch interviews, observation, the five Innovator's DNA discovery skills. Produces validated job stories and the Gate 1 brief. |
| [[innovation-method-validator]] | Runs the four-stage validation cycle (Insight → Problem → Solution → Business Model). Assumption mapping, pretotyping, build-measure-learn cadence. |
| [[innovation-typologist]] | Doblin Ten Types diagnostic and generative. Surfaces single-type product-only patterns; designs 3+ type combinations for sustained advantage. |
| [[innovation-monetization-strategist]] | Willingness-to-pay before product. Diagnoses the four monetization failure modes (feature shocks, minivations, hidden gems, undeads); designs WTP studies, packaging, pricing models. |
| [[innovation-lab-architect]] | Designs labs that survive year two. Charter, archetype, operating model, KPIs, Ten Faces role design, survival mechanics, sunset criteria. |
| [[innovation-disruption-analyst]] | Christensen disruption diagnostic — applies the test rigorously, maps incumbent vs. disruptor positions, recommends response patterns (esp. the separate-unit pattern). |
| [[innovation-leadership-coach]] | Culture + capital. Catmull's Brain Trust and candor mechanisms; Dyer/Furr/Lefrandt's four sources of innovation capital (human, social, reputation, impression amplifiers). |
| [[innovation-digital-transformation-advisor]] | Reframes DT as portfolio, not program. Failure-pattern diagnosis (70–80% fail rate); Rogers's five domains; Technology Fallacy maturity stages; CDO trap. Cross-references with `consulting-digital-strategist`. |

## Agent

**`innovation-agent`** — Senior innovation architect who knows when to use each skill in this plugin and how to sequence them. Use this agent (instead of invoking skills directly) when a task spans multiple skills, requires sequencing, or needs senior-level judgment about which skill to apply. Tagline: Innovation end-to-end — strategy, portfolio, validation, value, and the leadership that holds it together.

## Slash commands (8)

High-leverage recurring operations. Each command sequences the right skills and produces a structured deliverable.

| Command | What it does | Layer(s) |
|---|---|---|
| `/innovation:audit` | Full Innovation OS audit across all 7 layers, prioritized fix plan | All |
| `/innovation:strategy` | Design a new innovation strategy on a page (mandate, ambition mix, operating model, cadence, metrics) | Layer 1 |
| `/innovation:portfolio` | Design or audit portfolio governance (gates, allocation, kill discipline, dashboard) | Layer 2 |
| `/innovation:discover` | Run a JTBD discovery sprint (4–6 weeks, switch interviews, observation, Gate 1 brief) | Layer 3 |
| `/innovation:validate` | Run the four-stage validation cycle on a bet (Insight → Problem → Solution → Business Model) | Layer 4 |
| `/innovation:value-case` | Build a horizon-aware value case (NPV / EV / real options + kill economics) | Layer 5 |
| `/innovation:charter` | Design or recharter an innovation lab (8-section charter + Ten Faces role map + survival mechanics) | Layer 6 |
| `/innovation:disruption-test` | Apply Christensen's disruption test rigorously and recommend response patterns | Cross-layer |

See [[../../40_Library/Solution_Briefs/2026-05_Innovation-Operating-System|the Innovation OS Solution Brief]] for the seven-layer model these commands operate within.

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

**Personal context**
- **`bermon-context.md`** — Builder, not researcher; behavioral-economics lens native; Composable DXP practice context; runs at $100M+ scale
- **`slalom-context.md`** — Where innovation work lives inside Slalom; the firm-wide framework stack (PE, DE, Summit) intersection
- **`innovation-foundations.md`** — Three Horizons; Ambition Matrix; JTBD; stage-gate vs. discovery-driven; Doblin Ten Types; lab vs. line; the five-question diagnostic spine

**Book digests** (the canonical texts every skill reaches for)

*Strategy & disruption*
- **`book-innovators-dilemma.md`** — Christensen (1997): five principles of disruptive innovation, RPV framework, separate-unit response pattern
- **`book-driving-digital-strategy.md`** — Rogers (2016): five domains (Customers, Competition, Data, Innovation, Value), Disruptive Business Model Map
- **`book-why-digital-transformations-fail.md`** — five-stage maturity, seven failure patterns, five disciplines
- **`book-technology-fallacy.md`** — Kane et al. (2019): four-stage maturity, strategy/leadership/talent/culture as differentiators
- **`book-hbr-must-reads-innovation.md`** — Drucker's seven sources, Govindarajan/Trimble on dedicated teams, Day's three questions, Discovery-Driven Planning

*Discovery & validation*
- **`book-innovators-dna.md`** — Dyer/Gregersen/Christensen (2011/2019): five discovery skills (associating, questioning, observing, networking, experimenting)
- **`book-innovators-method.md`** — Furr & Dyer (2014): four-stage validation (Insight → Problem → Solution → Business Model)
- **`book-design-a-better-business.md`** — van der Pijl/Lokitz/Solomon (2016): visual tool catalog for discovery, ideation, validation

*Typology & monetization*
- **`book-ten-types-of-innovation.md`** — Keeley et al. / Doblin (2013): the ten types, three-type-combination empirical finding
- **`book-monetizing-innovation.md`** — Ramanujam & Tacke (2016): four failure modes (feature shocks, minivations, hidden gems, undeads), WTP-first principle

*Labs & leadership*
- **`book-innovation-lab-excellence.md`** — six lab archetypes, 8-section charter, year-2 collapse pattern, survival mechanics
- **`book-ten-faces.md`** — Kelley (2005): ten roles for innovation teams (Anthropologist, Experimenter, Cross-Pollinator, Hurdler, Collaborator, Director, Experience Architect, Set Designer, Caregiver, Storyteller)
- **`book-creativity-inc.md`** — Catmull (2014): Brain Trust, candor norms, separating idea from person, mechanisms not principles
- **`book-innovation-capital.md`** — Dyer/Furr/Lefrandt (2019): four sources of innovation capital (human, social, reputation, impression amplifiers)

**Templates** (six ready-to-use canvases)
- **`template-three-horizons-canvas.md`** — time-based portfolio map; default 70/20/10 capital allocation
- **`template-ambition-matrix.md`** — risk-based portfolio map (Nagji & Tuff)
- **`template-jtbd-interview-guide.md`** — switch interview structure with Four Forces synthesis
- **`template-ten-types-diagnostic.md`** — investment-intensity heatmap + type-combination canvas
- **`template-value-engineering-canvas.md`** — horizon-aware value case (NPV / EV / real options) with kill economics
- **`template-innovation-charter.md`** — the 8-section lab charter

**Carry-overs from prior consulting plugin**
- **`jobs_to_be_done_discovery.md`** — JTBD discovery framework (full reference; deep complement to the interview guide template)
- **`portfolio_governance_template.md`** — gate-by-gate criteria reference

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections (Role & Identity, Core Methodology, How to Engage, Key Deliverables, Source Frameworks, Templates, Boundaries, Example Prompts).
- Tone across all skills: direct, opinionated, builder-oriented. Bermon doesn't need basics re-explained.
- Always tag the horizon (H1/H2/H3) — H1 governance applied to H3 work is the most common failure mode in enterprise innovation.

## Pairs with

- **`consulting`** — `consulting-digital-strategist` cross-references the same DT books for engagement-shaping; `consulting-change-management-advisor` owns OCM mechanics for innovation change work
- **`behavioral-economics`** — `behavioral-economics-pricing-strategist` provides cognitive-science depth on pricing; `behavioral-economics-behavioral-economist` provides decision-making depth for adoption design
- **`cx`** — `cx-jtbd`, `cx-persona-developer`, `cx-journey-mapper` provide CX-specific extensions of discovery work
- **`brand`** — Doblin Type 9 (Brand) work flows to brand plugin
- **`project-management`** — `project-management-flow-engineer` optimizes throughput inside labs

## Scope changes from the prior consulting plugin

- The previous `consulting-innovation-strategist` skill is replaced by this 11-skill plugin and now exists as a redirect.
- The previous skill's two references (`portfolio_governance_template.md` and `jobs_to_be_done_discovery.md`) have moved to this plugin's `references/`.
- Cross-plugin DT references — three books on digital transformation live here, with `consulting-digital-strategist` cross-referencing them.

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture: skills (atomic), category agents (sequencing), orchestrators (cross-domain coordination).

## License

MIT.
