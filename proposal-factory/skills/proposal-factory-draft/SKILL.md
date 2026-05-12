---
name: proposal-factory-draft
description: >
  Stage 2 orchestrator for the Proposal Factory — Draft. Variant-aware markdown
  drafting. For SPRO/Small: drafts the proposal as one or two files. For
  Large-Slides/Large-Document: drafts master file + standalone section files
  using the Templates/Sections/ snippets. Authors companion drafts (POC brief,
  walkthrough video script, microsite plan) in parallel if flagged at intake.
  Calls specialist skills from consulting, marketing, brand, project-management
  plugins. Pauses for self-review at the end of each section (or per user
  preference). Output: completed markdown files in WIP/. Also known as: proposal
  drafting, variant authoring, draft stage runner.

type: skill
project: skills-library
plugin: proposal-factory
aliases: [proposal-factory-draft]
tags: [type/skill, plugin/proposal-factory, scope/orchestrator, topic/drafting, topic/draft]
status: active
---

## Goal

Produce a draft proposal in markdown — substance right, formatting deferred. Variant-aware.

## When to invoke

- User runs `/proposal-draft`
- Called by `proposal-factory-create-flow` at Stage 2
- User says: "let's draft," "start the proposal," "write the [section]"

**Prerequisite:** `00_Intake_Brief.md` exists and passes the readiness check. If not, route back to `proposal-factory-frame`.

## Required context

- The pursuit's intake — `WIP/[ClientName_Descriptor]/00_Intake_Brief.md`
- The pursuit's RFP parse (if applicable) — `WIP/[ClientName_Descriptor]/01_RFP_Parsed.md`
- The variant template — `60_Digital_Manufacturing/Proposal_Factory/Templates/0X_*.md` per variant
- The section snippets — `60_Digital_Manufacturing/Proposal_Factory/Templates/Sections/*.md`
- Foundation: [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md)

## Variant-aware flow

### SPRO

1. Open `Templates/01_SPRO.md` as the structure guide.
2. Author the six blocks directly into `WIP/[Pursuit]/01_SPRO.md`:
   - Title + Win Theme
   - Client problem
   - Approach (3 bullets)
   - Team
   - Investment + Timeline
   - Next step
3. Self-review.

### Small

1. Open `Templates/02_Small_Proposal.md` as the structure guide.
2. Author the deck as one markdown file `WIP/[Pursuit]/02_Small_Proposal.md`.
3. For each section (Executive Summary, Win Theme, Approach, Scope, Team, Schedule, Pricing, References, Risks/Assumptions, Next Steps):
   - Pull guidance from the matching snippet in `Templates/Sections/`.
   - Author the section content inline in the master file.
4. Self-review.

### Large-Slides + Large-Document

1. Open the master variant template (`03_Large_Proposal_Slides.md` or `04_Large_Proposal_Document.md`) as the structure guide.
2. Author the master file with section references.
3. For each section, author a standalone file in the WIP folder using the matching snippet from `Templates/Sections/`:
   - `Executive_Summary.md`
   - `Win_Theme.md`
   - `Approach.md`
   - `Scope.md`
   - `Team_Bios.md`
   - `Pricing.md`
   - `Schedule.md`
   - `Risks_Assumptions.md`
   - `References.md`
   - `Compliance_Matrix.md` (RFP-driven)
4. Track per-section length against budget (slides for Large-Slides; pages for Large-Document).
5. Self-review.

## Draft order (recommended)

1. **Win Theme** first — it's the thread through everything else.
2. **Executive Summary** — anchors the proposal in the Win Theme.
3. **Approach** — the substance of the work.
4. **Scope** — what's in / out, derived from Approach.
5. **Schedule** — derived from Approach phases.
6. **Pricing** — derived from Scope + Schedule + Team.
7. **Team Bios** — derived from Approach.
8. **References** — pulled from `20_Proposals/Won/`.
9. **Risks + Assumptions** — derived from Approach + Schedule.
10. **Compliance Matrix** (if RFP) — last; references all prior section locations.

## Specialist skill routing

| Section | Skill |
|---|---|
| Win Theme | `marketing:marketing-copywriter` + `brand:brand-voice-tone` + `consulting:consulting-humanize` |
| Executive Summary | `marketing:marketing-copywriter` + `consulting:consulting-humanize` |
| Approach | `consulting:consulting-management-consultant` + `project-management:project-management-project-manager` |
| Scope | `consulting:consulting-sow-risk-advisor` + `consulting:consulting-management-consultant` |
| Pricing | `consulting:consulting-negotiation-coach` + `consulting:consulting-sow-risk-advisor` |
| Team Bios | (pulls from team-finder; minimal LLM authoring) |
| Schedule | `project-management:project-management-project-manager` |
| References | `consulting:consulting-management-consultant` (Reference Story pattern) |
| Risks/Assumptions | `consulting:consulting-sow-risk-advisor` + `project-management:project-management-raid-log` |
| Compliance Matrix | `proposal-factory-rfp-parser` agent output → adapt |

## Solution alignment (when applicable)

If the pursuit aligns to a known solution kit (named in intake), pull from the solution kit:

- One-Pager → Win Theme + Executive Summary
- Sales Enablement Kit Battlecard → Why Slalom differentiators
- Approach pattern → Approach
- Pricing pattern → Pricing
- Case Study → References

Don't copy verbatim. Adapt for this pursuit's specific buyer + scope + Win Theme.

## Companion drafts (parallel)

If flagged at intake:

- **POC:** author `05_POC_Brief.md` per `Templates/05_POC_Brief.md` — personas, journeys, key flows, target stack, success criteria.
- **Walkthrough video:** author `06_Walkthrough_Video_Script.md` per `Templates/06_Walkthrough_Video_Script.md` — scene-by-scene with voiceover prompts.
- **Microsite:** author `07_Microsite_Plan.md` per `Templates/07_Microsite_Plan.md` — page structure, navigation, deployment plan.

Author these in parallel with the proposal sections — they pull from the same source content.

## Pause points

After each major section:

```
Question: "Section [X] drafted. Continue to [next], pause for review, or revise [X]?"
Options:
1. Continue to [next section] (Recommended)
2. Pause — I want to review what's drafted so far
3. Revise [section X]
```

After all sections drafted, full proposal self-review:

```
Question: "Draft complete. Move to Polish?"
Options:
1. Yes, generate visuals + format (Recommended)
2. Yes, but the [section] needs another pass first
3. Pause — I'll come back to this
```

## Outputs

For Small / Large: a set of markdown files in the WIP folder. Plus companion files if flagged.

For SPRO: a single markdown file in the WIP folder.

## Skill discipline

- Markdown only — no PPTX / PDF generation. That's Polish.
- Pull section content from the snippets — don't re-author the snippets themselves.
- Apply brand voice at draft time (no em-dashes in TFIC; no marketing-speak verbs).
- Cross-reference downstream sections (Approach phases → Schedule durations → Pricing → Team).
- Don't generate companions before proposal substance is settled — but DO author the companion-brief drafts in parallel.

## See also

- [`../proposal-factory-create-flow/SKILL.md`](../proposal-factory-create-flow/SKILL.md)
- [`../proposal-factory-polish/SKILL.md`](../proposal-factory-polish/SKILL.md)
- [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md)
- [Templates/](../../../../60_Digital_Manufacturing/Proposal_Factory/Templates/) — variant templates + section snippets
