---
name: solution-factory-status
description: >
  Advice skill for the Solution Factory. Inspects the WIP/ folder at
  60_Digital_Manufacturing/Solution_Factory/ and reports on active solutions —
  which are at which stage (Frame / Draft / Polish / Ship), how complete each is,
  what's blocking, what's stale. Optionally inspects the canonical promotion
  targets (10_Practice/Offerings/Solutions/ and 40_Library/Solution_Briefs/) to
  report on recently shipped solutions. Read-only — does not modify anything.
  Also known as: WIP status, solution status, solution factory dashboard.

type: skill
project: skills-library
plugin: solution-factory
aliases: [solution-factory-status]
tags: [type/skill, plugin/solution-factory, scope/advice, topic/status]
status: active
---

## Goal

Tell the user what's happening in the Solution Factory — without modifying anything. Read-only.

## When to invoke

- User runs `/solution-status`
- User says "what's in WIP?" / "what solutions are active?" / "show me solution factory status"
- Stage 0 of `solution-factory-create-flow` when user picks "Check status"

## What to inspect

### 1. WIP folders (active work)

For each folder in `60_Digital_Manufacturing/Solution_Factory/WIP/`:

- Look for `00_Intake_Brief.md`
  - If present: extract Brand, Tier, dominant Buyer State, Reframe headline, date created, lead
  - Walk the readiness check. If 13/13 → Frame complete. If <13 → Frame in progress.
- Look for `01_One_Pager.md` through `08_Marketing_Handoff.md`
  - Count how many exist. Note which are missing.
  - For each that exists, check: is the Reframe filled in? Are placeholders still present (`[...]` markers)?
- Look for PPTX/PDF/HTML at top level
  - If present: Polish has run for that deliverable
- Look for `QUALITY_GATES_FAILED.md`
  - If present: kit failed Quality Gates; surface what failed

### 2. Stage classification

Based on what's present:

| Files present | Stage |
|---|---|
| Only `00_Intake_Brief.md` (incomplete or complete) | **Frame** |
| Intake + at least one `0X_*.md` deliverable but no PPTX | **Draft** |
| Intake + multiple deliverables + at least one PPTX/PDF | **Polish** |
| All 8 markdown deliverables + all expected PPTX/PDF + no `QUALITY_GATES_FAILED.md` | **Ready to Ship** |
| All of above + `QUALITY_GATES_FAILED.md` | **Ship blocked — revisions needed** |

### 3. Staleness

If the most recent file modification is > 30 days old:
- Mark as **stale** — possibly abandoned
- Surface to user: *"This solution hasn't been touched in N days. Resume, archive, or delete?"*

### 4. Promoted solutions (optional)

If user asks for shipped solutions:

- List recent additions in `10_Practice/Offerings/Solutions/{Advisory,Simple,Complex}/` (look for folders matching `*-[YYYY-MM]-Slalom`)
- List recent additions in `40_Library/Solution_Briefs/` (folders matching same pattern)
- For each: report tier, brand, date

## Output format

Single-message status report:

```markdown
## Solution Factory — Status

### Active in WIP/ (N solutions)

| Solution | Stage | Brand | Tier | Started | Last touched | Lead |
|---|---|---|---|---|---|---|
| `composable-dxp-migration` | Polish | Composable DXP | Simple | 2026-05-08 | today | Bermon |
| `ai-transformation-fsi` | Frame (8/13) | Slalom default | Complex | 2026-05-09 | 2 days ago | TBD |

### Stale solutions
- `cms-lift-and-shift` — last touched 2026-04-01 (45 days ago). Resume / archive / delete?

### Quality Gates blocked
- (none currently)

### Recently shipped (last 30 days, optional)
- `cms-platform-migration-2026-05-Slalom` — Composable DXP / Simple → 10_Practice/Offerings/Solutions/Simple/
```

## Failure modes

- **WIP folder empty.** Report: *"No active solutions in WIP. Want to start a new one? `/solution-create`."*
- **A WIP folder is malformed** (missing intake, but deliverables present). Surface to user: *"`folder-name` has deliverable drafts but no `00_Intake_Brief.md`. Recover the intake or treat as orphaned?"*
- **Folder name doesn't match conventions** (not kebab-case, has spaces). Surface but don't auto-fix.

## Token discipline

- Read-only. Don't modify anything.
- Be concise — a status report is a glance, not an essay. The Bermon-style tabular summary is the goal.
- Don't re-explain the pipeline. The user knows.

## Example prompts

1. *"What's in WIP?"* → run the inspection, output the table.
2. *"Show me solution factory status."* → same.
3. *"Anything stale?"* → filter to stale-only solutions.
4. *"What did I ship recently?"* → list promoted folders from the last 30 days.
5. *"Is `composable-dxp-migration` ready to ship?"* → focused inspection on that one solution.
6. *"What's blocked?"* → filter to solutions with `QUALITY_GATES_FAILED.md` or other blockers.
7. *"Status report for the team meeting."* → full output, all sections.
8. *"Just the active ones."* → skip stale + promoted sections.

## See also

- `../../references/solution-factory-foundations.md`
- `60_Digital_Manufacturing/Solution_Factory/WIP/README.md` — WIP folder shape
- `60_Digital_Manufacturing/Solution_Factory/Pipeline.md` — stage definitions
