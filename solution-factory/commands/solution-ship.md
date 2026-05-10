---
description: Run Stage 4 (Ship) — Quality Gates checklist, cross-document consistency check, dual-routed promotion (Composable DXP → 10_Practice; Slalom default → 40_Library), reference stub creation, practice catalog update.
---

Invoke the `solution-factory-ship` skill. The user wants to quality-gate and promote a polished solution kit.

The skill runs 7 steps:

1. **Quality Gates checklist** — walks the 15 unified anti-patterns from the Slalom OS plus ~10 SF-specific gates (Deb Oler answered, Reframe produces "huh," cross-document consistency, speaker notes complete, brand applied, visuals real, tier selected, routing correct, hybrid README format, practice catalog updated, toolchain followed, output formats correct, Marketing Handoff complete, microsite call documented).

2. **Cross-document consistency check** — verifies solution name / Reframe / key components / proof points / differentiators / stakeholders match across all 8 deliverables.

3. **Determine promotion target** based on Brand from intake:
   - Composable DXP secondary → `10_Practice/Offerings/Solutions/{Tier}/[Solution-Name]-[YYYY-MM]-Slalom/` + reference stub at `40_Library/Solution_Briefs/`
   - Slalom default → `40_Library/Solution_Briefs/[Solution-Name]-[YYYY-MM]-Slalom/`

4. **Promote via the agent** — calls `solution-factory-promoter` to do the file-system work and write the hybrid promoted folder README.md (practice metadata frontmatter + One-Pager content + kit index).

5. **Update practice catalog** (Composable DXP only) — replaces `_to be authored_` with link in `10_Practice/Offerings/Solutions/README.md`.

6. **Tag in Salesforce** — surfaces SF Solution tag from intake; reminds user to apply.

7. **Final report** + WIP archive option.

Quality Gates failures (2+) block promotion. The skill writes `QUALITY_GATES_FAILED.md` and routes back to the right earlier stage.
