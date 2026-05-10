---
type: readme
project: skills-library
scope: plugin
plugin: consulting
tags: [type/readme, plugin/consulting]
status: active
---
# consulting

**Management consulting, digital strategy, and engagement delivery**

## Skills (8)

| Skill | What it does |
|---|---|
| [[consulting-management-consultant]] | Seasoned management consultant who diagnoses complex organizational problems, structures engagements for success, and navigates stakeholder dynamics with surgical precision. |
| [[consulting-digital-strategist]] | Digital strategy advisor who frames ambiguous customer and business problems into clear, executable product and platform strategies. Cross-references the digital-transformation books in the `innovation` plugin. |
| [[consulting-change-management-advisor]] | OCM specialist guiding digital transformation and organizational change. Stakeholder readiness, resistance management, adoption strategy, and sustaining change post-go-live. |
| [[consulting-proposal-strategist]] | Senior RFP strategist and proposal expert who transforms complex requirements into winning, evaluator-friendly proposals. |
| [[consulting-sow-risk-advisor]] | Contract risk expert who hardens Statements of Work to eliminate ambiguity, quantify scope, and control change. |
| [[consulting-sales-bd-strategist]] | Strategic sales and business development leader who builds enterprise pipelines, structures complex land-and-expand plays, and converts pursuit opportunities into closed deals. |
| [[consulting-negotiation-coach]] | Negotiations specialist who coaches on high-stakes deal conversations, contract terms, and interpersonal bargaining. |
| [[consulting-humanize]] | Rewrites consultant-jargon and AI-generated prose into clear, direct, professionally friendly English at roughly a high-school reading level. |

**Note:** the previous `consulting-innovation-strategist` has been split out into a dedicated [[../innovation/README|`innovation` plugin]] with eleven specialty skills (strategist, portfolio-architect, value-engineer, discovery-coach, method-validator, typologist, monetization-strategist, lab-architect, disruption-analyst, leadership-coach, digital-transformation-advisor). The previous skill location is now a redirect.

## Agent

**`consulting-agent`** — Senior consultant who knows when to use each skill in this plugin and how to sequence them. Use this agent when a task spans multiple skills (e.g., proposal + SOW + change), requires sequencing, or needs senior-level judgment about which skill to apply.

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

**Personal context**
- **`bermon-context.md`** — Personal/professional profile shaping tone, depth, and recommendations
- **`slalom-context.md`** — Slalom organizational context, service pillars, MACH/composable priorities
- **`consulting-foundations.md`** — Engagement phases, stakeholder management, scope control, risk frameworks

**Book digests** (the canonical texts every skill reaches for)
- **`book-flawless-consulting.md`** — Block (2011): the 5-phase engagement, contracting meeting, 5-layer discovery, 13 forms of resistance, expert/pair-of-hands/collaborative roles
- **`book-management-consulting-kubr.md`** — Kubr (2002): the encyclopedic ILO/UN reference; 5-phase consulting process, diagnostic decomposition, proposal structure, fee-setting, termination protocols
- **`book-essential-tools-burtonshaw.md`** — Burtonshaw-Gunn (2010): ~32 management-consulting tools, Mendelow stakeholder grid, 4Ts Risk Response, the Five Change Strategies
- **`book-consulting-discipline-mabee.md`** — Mabee (2009): Four Postures of Influence (TO/FOR/WITH/BY Them), Three Core Questions, the Cairn of habits, One Good Hour
- **`book-essentials-of-organizational-behavior.md`** — Robbins & Judge (2018): Lewin, Kotter, ADKAR, resistance sources, Big Five, organizational justice, conflict modes
- **`book-never-split-the-difference.md`** — Voss (2016): tactical empathy, the 9 negotiation tactics, Ackerman model, 7-38-55 communication rule
- **`book-challenger-sale.md`** — Dixon & Adamson (2011): Teach-Tailor-Take Control, Commercial Insight design, the 6-step Reframe pitch, Mobilizer/stakeholder map

**Templates**
- **`template-engagement-scoping.md`** — Block contracting meeting + Mendelow stakeholder grid + Block's 5-layer discovery
- **`template-negotiation-prep.md`** — Voss tactical empathy + classical BATNA/ZOPA
- **`template-challenger-sale.md`** — Pursuit qualification + Commercial Insight design + Mobilizer map
- **`template-change-readiness-discovery.md`** — Lewin + Kotter + ADKAR + Robbins/Judge resistance sources + Block discovery interview

The synthesized engagement view lives as a Solution Brief at [`/40_Library/Solution_Briefs/2026-05_Consulting-Engagement-Lifecycle.md`](../40_Library/Solution_Briefs/2026-05_Consulting-Engagement-Lifecycle.md).

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files.
- Tone across all skills: direct, practical, builder-oriented.

## Pairs with

- **`project-management`** — for the operational PM and delivery side of engagements
- **`cx`** — for customer research, journey, and personalization inputs feeding strategy
- **`brand`** — for client-facing brand work inside engagements

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture: skills (atomic), category agents (sequencing), orchestrators (cross-domain coordination).

## License

MIT.
