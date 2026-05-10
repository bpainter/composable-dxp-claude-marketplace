---
type: readme
project: skills-library
scope: plugin
plugin: behavioral-economics
tags: [type/readme, plugin/behavioral-economics]
status: active
---
# behavioral-economics

**Behavioral economics, decision science, choice architecture, and research methodology**

## Skills (7)

| Skill | What it does |
|---|---|
| [[behavioral-economics-behavioral-economist]] | Applies validated behavioral science to real-world decisions in policy, business, product, and organizational settings. Diagnoses irrational decision-making with prospect theory, dual-process thinking, and bias frameworks; designs evidence-based interventions. |
| [[behavioral-economics-behavioral-economist-social-cognition]] | Behavioral economist specializing in social cognition and persuasive messaging — how identity, framing, and narrative shape what people hear, believe, and act on. |
| [[behavioral-economics-choice-architect]] | Designs decision environments that guide people toward better choices while preserving autonomy. Defaults, salience, friction, framing, sequencing, and reminder design. |
| [[behavioral-economics-organizational-behavior-specialist]] | Diagnoses and improves individual, team, and organizational performance using evidence-based psychology and organizational science. Motivation, team dynamics, conflict, and culture. |
| [[behavioral-economics-pricing-strategist]] | Applies behavioral economics to monetization and value capture — anchoring, decoys, reference prices, bundling, loss aversion, and willingness-to-pay research. |
| [[behavioral-economics-research-methods]] | Research methods expert for qualitative and quantitative studies. Designs interviews, surveys, experiments, and mixed-methods studies; ensures the design answers the question. |
| [[behavioral-economics-statistician]] | Inferential statistics, data analysis, and evidence-based organizational insights. Hypothesis tests, regression, power analysis, A/B test design and analysis. |

## Agent

**`behavioral-economics-agent`** — Senior behavioral scientist who knows when to use each skill in this plugin and how to sequence them. Use this agent (instead of invoking skills directly) when a task spans multiple skills, requires sequencing, or needs expert-level judgment about which skill to apply.

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

**Personal context**
- **`bermon-context.md`** — Personal/professional profile shaping tone, depth, and recommendations
- **`behavioral-foundations.md`** — Dual-process theory, prospect theory, 15 core biases, ethical frameworks

**Book digests** (the canonical texts every skill reaches for)
- **`book-thinking-fast-slow.md`** — Kahneman (2011): System 1/2, prospect theory, anchoring, availability, two selves, ~25-bias catalog with page references
- **`book-nudge.md`** — Thaler & Sunstein (2008/2021): NUDGES mnemonic, libertarian paternalism, defaults, sludge, Save More Tomorrow, ethics of nudging
- **`book-predictably-irrational.md`** — Ariely (2008): decoy effect, arbitrary coherence, zero price, social vs. market norms, endowment, 13 phenomena with experiments
- **`book-designing-for-behavior-change.md`** — Wendel (2013): the **CREATE Action Funnel**, four-stage design process, intervention catalog by funnel gate, behavioral product workflow

**External catalogs**
- **`external-catalogs.md`** — The Decision Lab (biases index, ~100+ biases) and BeSci (tactics catalog) for coverage beyond the books

**Templates** (Slalom's Behavioral Design Model, plus extensions)
- **`template-behavioral-profile.md`** — segment definition with motivations, social influences, environment, barriers (Slalom Stage 2)
- **`template-choice-architecture.md`** — current-state and future-state flow design with the 5 core levers (Slalom Stage 3 part 1)
- **`template-nudges-biases.md`** — 8 common biases plus the extended catalog (Slalom Stage 3 part 2)
- **`template-intervention-design.md`** — 8 behavior-change tactics plus the BeSci catalog (Slalom Stage 3 part 3)
- **`template-use-case-scoring.md`** — Customer Value × Business Value scoring grid (Slalom Stage 3 prioritization)
- **`template-use-case-validation.md`** — hypothesis-driven testing template plus the 5-question ethics gate (Slalom Stages 4 + 5)

The Slalom Behavioral Design Model is also documented as a Solution Brief at [`/40_Library/Solution_Briefs/2026-05_Slalom-Behavioral-Design-Model.md`](../40_Library/Solution_Briefs/2026-05_Slalom-Behavioral-Design-Model.md).

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files.
- Tone across all skills: direct, practical, builder-oriented.

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture: skills (atomic), category agents (sequencing), orchestrators (cross-domain coordination).

## License

MIT.
