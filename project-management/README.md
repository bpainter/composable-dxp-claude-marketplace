---
type: readme
project: skills-library
scope: plugin
plugin: project-management
tags: [type/readme, plugin/project-management]
status: active
---
# project-management

**Project management and delivery — a senior PM persona, task-focused workflows, plus flow and methodology specialists**

## Skills (9)

| Skill | What it does |
|---|---|
| [[project-management-project-manager]] | Senior project-manager persona — delivery leader who owns end-to-end digital project execution, blending Waterfall rigor with Agile velocity. |
| [[project-management-requirements-discovery]] | Turns unstructured stakeholder input into structured, prioritized, traceable requirements with assumptions, constraints, and risks. |
| [[project-management-user-story]] | Turns loose feature requests into well-formed agile user stories with Gherkin acceptance criteria, INVEST-checked scope, and INVEST-aware splitting. |
| [[project-management-sprint-planning]] | Plans sprints the team can actually deliver — sprint goal, capacity, story selection, estimation, dependencies, working agreements. |
| [[project-management-raid-log]] | Creates and maintains a project RAID log with Risk Up Front methodology — Risks, Assumptions, Issues, Dependencies — with familiarity scoring and continuous review. |
| [[project-management-retro-facilitator]] | Runs retrospectives the team actually learns from — right format for the moment, real signal, committed actions. |
| [[project-management-facilitator]] | **Pointer** to the dedicated [`facilitation`](../facilitation/README.md) plugin. Workshop and meeting facilitation moved into its own plugin (9 specialists) given the depth of canonical sources. |
| [[project-management-flow-engineer]] | Reinertsen-grounded flow optimization: Cost of Delay, CD3, Little's Law (cycle time = WIP ÷ throughput), batch sizing, WIP constraints, cadence design. |
| [[project-management-methodology-advisor]] | Gothelf-grounded methodology choice: when to use Lean Startup vs Agile vs Design Thinking; sequenced workflows for new product engagements. |

## Agent

**`project-management-agent`** — Senior PM who knows when to use each skill in this plugin and how to sequence them. Use this agent when a task spans discovery → planning → delivery → retro, requires sequencing, or needs senior judgment about which skill applies.

## Shared references

**Book digests**
- **`book-product-development-flow.md`** — Reinertsen (2009): 175 economic principles for managing product development under uncertainty
- **`book-lean-agile-design-thinking.md`** — Gothelf (~2017): the three-questions framing for picking methodology
- **`book-risk-up-front.md`** — Josephs & Rubenstein (2018): proactive risk inventory at kickoff

The plugin also pairs with the product-management plugin's `book-escaping-the-build-trap.md` (Perri).

**Templates**
- **`template-flow-metrics.md`** — Cost of Delay, CD3, Little's Law, WIP recommendations, cadence design
- **`template-methodology-selector.md`** — DT vs. Lean vs. Agile decision worksheet
- **`template-risk-up-front.md`** — RUF kickoff inventory, RAP-sheet structure, continuous risk management

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files (where present).
- Tone across all skills: direct, practical, builder-oriented.

## Pairs with

- **`facilitation`** — workshop and meeting design lives here now (Collaboration Code, Hoffman, Klein/Newman, Lewrick, *Design a Better Business*); PM keeps retro, sprint planning, and requirements-discovery as PM-cadence work
- **`consulting`** — `consulting-management-consultant` and `consulting-change-management-advisor` for the strategic side of engagements
- **`leadership`** — `leadership-people-leader` for the human side of running teams
- **`software-engineering`** — sprint and user-story outputs feed engineering execution

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
