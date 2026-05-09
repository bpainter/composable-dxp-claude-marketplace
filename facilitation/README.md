---
type: readme
project: skills-library
scope: plugin
plugin: facilitation
tags: [type/readme, plugin/facilitation]
status: active
---
# facilitation

**Facilitation craft — a senior facilitator persona, plus specialists for meeting design, multi-day workshop design, design thinking, decision-making, group dynamics, virtual/hybrid, strategic visualization, and a library of collaboration patterns.**

## Skills (9)

| Skill | What it does |
|---|---|
| [[facilitation-facilitator]] | Senior facilitator persona — designs and leads sessions that turn conversations into decisions; sequences across the rest of the plugin. |
| [[facilitation-meeting-architect]] | Hoffman-grounded meeting design — agendas built from ideas/people/time, the cognitive constraint of meetings, beginning/middle/end structures. |
| [[facilitation-workshop-designer]] | Multi-day workshop design — scoping, sponsor design teams, Scan/Focus/Act backbone, agenda assembly, 3-day / 2-day / 1-day / 90-min arcs. |
| [[facilitation-collaboration-pattern-library]] | Reference to the Collaboration Code patterns (100s–300s) — physical environment, energetic environment, preparation, participants, workshop design, virtual sessions, outcomes, learning and mastering facilitation. |
| [[facilitation-design-thinking-coach]] | Design Thinking process facilitator — problem statement, empathy, focus, ideation, prototype, test; transforming organizations; designing the future (systems thinking, ecosystems, lean business model). |
| [[facilitation-strategic-visualizer]] | Visual strategy and innovation canvases — Team Charter, 5 Bold Steps Vision, Cover Story Vision, Design Criteria, Storytelling, Context, Business Model, Value Proposition. |
| [[facilitation-decision-architect]] | Decision-making methods — when to vote, when to consent, when to consult, when the leader decides; multi-voting, dot voting, fist of five, RAPID, integrative decisions, trade-off matrices. |
| [[facilitation-group-dynamics-coach]] | Psychological safety, dominance/silence, conflict navigation, equity practices, tactical empathy, neurodiversity inclusion, difficult participants. |
| [[facilitation-virtual-hybrid-specialist]] | Remote and hybrid facilitation — Miro/FigJam/Mural choreography, attention budget, async pre-work, hybrid-fairness rules, virtual openings/closings, energy management. |
| [[facilitation-gamestorming-coach]] | Gray, Brown, and Macanufo's catalog of named facilitation games — Empathy Map, Cover Story, 3-12-3 Brainstorm, Pre-Mortem, Speed Boat, Dot Voting, NUF Test, etc. ~120-game deep catalog with object of play, players, duration, how-to-play, strategy. |

## Agent

**`facilitation-agent`** — Senior facilitator who knows when to use each skill in this plugin and how to sequence them. Use this agent (instead of invoking skills directly) when a task spans scoping → design → delivery → value capture, requires sequencing across multiple specialists, or needs senior judgment about which skill applies.

## Slash commands

Six commands covering the facilitation lifecycle:

| Command | What it does |
|---|---|
| `/scope` | Job 1 — clarify outcomes with sponsor; produce scoping brief naming outcome, out-of-scope, participants, political dynamics |
| `/design-session` | Walk the 5 Ps + Scan/Focus/Act; pick the arc; sequence blocks with named games/modules; populate session-design template |
| `/pick-game` | Quick game selection from the 123-game catalog — input is outcome + time + people, output is 1–3 named games with delivery notes |
| `/triage-meeting` | Diagnose a recurring meeting that's stopped producing value — recommend kill / halve / restructure / leave |
| `/decision-rule` | Recommend a decision-making method (consent, RAPID, leader-after-consultation, etc.) plus the moment-of-decision script |
| `/recap` | Generate within-24h sponsor-signed recap from session notes — decisions, owners, dates, artifacts, next checkpoint |

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

**Foundations**
- **`facilitation-foundations.md`** — synthesis of cross-cutting principles: the 5 Ps, divergence/insight/convergence, Scan/Focus/Act, the difference between facilitation and chairing, the difference between meetings and workshops, when to facilitate vs. when to consult.
- **`bermon-context.md`** — pointer to the consulting plugin's Bermon profile for tone, depth, and recommendations.

**Book digests** (the canonical texts every skill reaches for)
- **`book-collaboration-code-patterns.md`** — Evans (2020/EY wavespace ed. 2021): orientation map for the pattern catalog (100s Physical Environment → 320s Mastering Facilitation). **Deep reference**: [`pattern-catalog.md`](skills/facilitation-collaboration-pattern-library/references/pattern-catalog.md) has per-pattern digests for all 91 patterns with the source's specific arguments and citations.
- **`book-collaboration-code-tools.md`** — Evans (2019/EY wavespace ed. 2021): orientation map for the tool catalog by Scan/Focus/Act phase. **Deep reference**: [`tools-catalog.md`](skills/facilitation-workshop-designer/references/tools-catalog.md) has per-tool digests for all ~110 tools with how-it-works steps.
- **`book-collaboration-code-models.md`** — Taylor, Evans, Bird (2019/EY wavespace ed. 2021): orientation map for the MG Taylor Modeling Language. **Deep reference**: [`models-catalog.md`](skills/facilitation-workshop-designer/references/models-catalog.md) has per-model digests for all 33 models with origin stories, components, and use.
- **`book-collaboration-code-casebook.md`** — Evans (curator, 2019): orientation map for the case studies. **Deep reference**: [`casebook-cases.md`](skills/facilitation-workshop-designer/references/casebook-cases.md) has per-case digests for all 19 cases with full agenda spines and practitioner reflections.
- **`book-facilitating-collaboration.md`** — Klein & Newman (2016, Value Web): the **Six Jobs** of the facilitator — Scoping, Sponsors, Preparation, Designing, Delivery, Value Capture (+ Satisfaction).
- **`book-design-thinking-playbook.md`** — Lewrick, Link, Leifer (Wiley, 2018): DT cycle (problem → empathy → focus → ideate → prototype → test), transforming organizations, designing ecosystems and the digital future.
- **`book-meeting-design.md`** — Hoffman (Two Waves Books, 2018): meeting as designed object — the cognitive constraint, agendas built from ideas/people/time, conflict via facilitation, beginning/middle/end meetings.
- **`book-design-a-better-business.md`** — van der Pijl, Lokitz, Solomon (Wiley, 2016): the canvas-driven strategy/innovation process — Prepare → Point of View → Understand → Ideate → Prototype → Validate → Scale.
- **`book-gamestorming.md`** — Gray, Brown, Macanufo (O'Reilly, 2010; v2.0 Gray & Brown 2024): the canonical playbook of named facilitation games. **Deep reference**: [`gamestorming-catalog.md`](skills/facilitation-gamestorming-coach/references/gamestorming-catalog.md) has per-game digests for all ~120 games with object of play, players, duration, how-to-play, strategy.

**Templates** (drop-in scaffolds for real sessions)
- **`template-session-design.md`** — 5 Ps + Scan/Focus/Act backbone with a slot-by-slot agenda
- **`template-pre-work-brief.md`** — what participants need to receive before a session
- **`template-decision-record.md`** — what was decided, by whom, why, owners, next checkpoint
- **`template-collaboration-pattern-selector.md`** — quick chooser across the Collaboration Code 100s–300s
- **`template-workshop-arc.md`** — 3-day, 2-day, 1-day, half-day, 90-minute arcs
- **`template-virtual-meeting-checklist.md`** — pre-, during-, post-session checklist for remote/hybrid
- **`template-workshop-recap.md`** — within-24h recap structure
- **`template-business-model-canvas.md`** — 9-block Osterwalder canvas with prompt guidance
- **`template-strategy-sketch.md`** — 5 Bold Steps Vision canvas variant

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files.
- Tone across all skills: direct, practical, builder-oriented. Facilitation as discipline, not magic.
- Plugin sources cite primary authors. The four Collaboration Code books are personally licensed to Bermon (EY wavespace edition) — digests reference structure and frameworks for personal study, not redistribution.

## Pairs with

- **`project-management`** — `project-management-retro-facilitator` for retros (still lives in PM); `project-management-facilitator` is now a thin pointer to this plugin.
- **`consulting`** — `consulting-management-consultant` and `consulting-negotiation-coach` for the strategic and negotiation work that often surrounds a workshop engagement (Block, Voss).
- **`behavioral-economics`** — `behavioral-economics-choice-architect` for the decision environments inside the room.
- **`design`** — `design-process` for the discovery → define → develop → deliver framing on the design side.
- **`leadership`** — `leadership-people-leader` for the people-leader side of running a team's meeting cadence.

## Part of the second-brain marketplace

See the [marketplace README](../README.md) for the full architecture: skills (atomic), category agents (sequencing), orchestrators (cross-domain coordination).

## License

MIT.
