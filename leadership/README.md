---
type: readme
project: skills-library
scope: plugin
plugin: leadership
tags: [type/readme, plugin/leadership]
status: active
---
# leadership

**Leadership coaching, people-management, hiring, meetings, goals, transitions, and perspective.**

Seven specialized skills, one orchestrating agent, deep reference files distilled from canonical leadership texts.

## Skills (7)

| Skill | What it does |
|---|---|
| [[leadership-executive-coach]] | Specialized executive coach for neurodivergent leaders (ADHD, autism, Level 1 autism) navigating high-stakes, fast-paced leadership roles. Helps leaders slow down, evaluate before reacting, and lead authentically without masking. |
| [[leadership-people-leader]] | Coaches teams and direct reports through clarity, feedback, and real conversation — without sacrificing humanity or accountability. The everyday craft: 1:1s, feedback, performance conversations, hiring, team development. |
| [[leadership-hiring]] | Performance-based hiring end to end. Performance Profiles, the two-question interview, evidence-based assessment, scorecards, debriefs, structured loops, sourcing, closing. Built on Adler's *Hire With Your Head*, Grosse & Loftesness's *Scaling Teams*, and DeMarco & Lister's *Peopleware*. |
| [[leadership-meetings-and-cadences]] | Designs the meeting and cadence architecture that makes a team run. Beginning/middle/end meeting types, weekly staff, 1:1 architecture, skip-levels, decision meetings, retrospectives, and the Manager Tools cadence. Built on Hoffman, Horstman, Fournier, Lopp. |
| [[leadership-onboarding-and-transitions]] | The First 90 Days as an actual playbook. STARS diagnosis, breakeven, win matrix, the five conversations with your boss, taking over a team, leaving a role cleanly, onboarding direct reports. Built on Watkins. |
| [[leadership-goals-and-okrs]] | OKR design, weekly P1/P2/P3 priorities, the Monday-commit / Friday-celebrate cadence, motivation diagnosis, Q2 prioritization, hedgehog/level-5/flywheel mental models. Built on Wodtke, Pink, Covey, Collins. |
| [[leadership-stoic-perspective]] | The leader's operating system for perspective, equanimity, and burnout prevention. Negative visualization, dichotomy of control, voluntary discomfort, identity-vs-role separation, perspective rituals. Built on Irvine and the Nightingales' *How F*cked Up Is Your Management*. |

## Agent

**`leadership-agent`** — Senior leadership coach who knows when to use each skill in this plugin. Use this agent when a leadership question spans multiple lenses (executive + day-to-day, hiring + onboarding, goals + cadence + perspective), requires sequencing, or needs senior judgment about which to pull on.

## Slash commands (8)

Tight, opinionated wrappers over the most-used workflows. Each command routes through the right skill(s) with a specific structure.

| Command | What it does | Cadence |
|---|---|---|
| [/1on1](commands/1on1.md) | Prep for or log a 1:1 — opens the running log, surfaces open commitments, suggests topics | Daily / weekly |
| [/feedback](commands/feedback.md) | Coach me through delivering specific feedback (SBI, Radical Candor calibration) | As needed |
| [/hire-loop](commands/hire-loop.md) | Hiring workflow end-to-end (Performance Profile → loop → debrief → close) | Per req |
| [/decision](commands/decision.md) | Walk a material decision through the full structure with pre-mortem and decision log entry | Per material decision |
| [/cadence-audit](commands/cadence-audit.md) | Audit calendar / cadence; produce a prioritized prune list | Quarterly |
| [/perspective](commands/perspective.md) | Stoic perspective check (dichotomy of control, view from above, the reactivity gap) | Situational |
| [/okr-set](commands/okr-set.md) | Set, sharpen, or review quarterly OKRs (Wodtke format with stretch calibration) | Quarterly + mid-Q |
| [/transition](commands/transition.md) | Build a 90-day transition plan (yours or a direct's) | Per role change / hire |

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections (Role & Identity, Core Methodology, How to Engage, Key Deliverables, Domain Expertise, Boundaries & Escalation, Example Prompts).
- Each skill has a `references/` folder with deep, source-cited reference files. References load on demand from inside the skill.
- Tone across all skills: direct, empathetic, builder-oriented. The executive coach speaks specifically to neurodivergent leaders; the others are universal but tuned for senior consulting/practice leadership.
- Skill-internal templates (mini scorecards, mini agendas) live inside the skill's `references/` for the agent to reach for in conversation. Canonical, Bermon-facing templates live in `70_Templates/` (T_Interview_Scorecard.md, T_1on1_Log.md, etc.).

## Pairs with

- **`behavioral-economics`** — `behavioral-economics-organizational-behavior-specialist` for diagnosing team dynamics
- **`consulting`** — `consulting-management-consultant` and `consulting-change-management-advisor` for organizational change
- **`project-management`** — `project-management-facilitator` for leading workshops

## Where templates live

| Template | Vault location | Used by |
|---|---|---|
| T_1on1_Log.md | `70_Templates/` | leadership-people-leader, leadership-meetings-and-cadences |
| T_Profile.md | `70_Templates/` | leadership-people-leader |
| T_Interview_Scorecard.md | `70_Templates/` | leadership-hiring |
| T_Interview_Debrief.md | `70_Templates/` | leadership-hiring |
| T_Hiring_Role_Brief.md | `70_Templates/` | leadership-hiring |
| T_Performance_Review.md | `70_Templates/` | leadership-people-leader |
| T_OKR_Worksheet.md | `70_Templates/` | leadership-goals-and-okrs |
| T_30_60_90.md | `70_Templates/` | leadership-onboarding-and-transitions |
| T_Decision_Log.md | `70_Templates/` | leadership-executive-coach, leadership-people-leader |
| T_Coaching_Notes.md | `70_Templates/` | leadership-people-leader, leadership-executive-coach |

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
