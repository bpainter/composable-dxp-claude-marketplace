---
description: Show status of solutions in the Solution Factory — what's in WIP, at which stage, what's blocked, what's stale, and (optionally) what's been promoted recently. Read-only.
---

Invoke the `solution-factory-status` skill. The user wants a glance at Solution Factory state — no modifications.

The skill inspects `60_Digital_Manufacturing/Solution_Factory/WIP/` and reports:
- Active solutions and their stage (Frame / Draft / Polish / Ready to Ship / Ship blocked)
- Brand and Tier per solution
- Started date + last touched + lead
- Stale solutions (untouched > 30 days)
- Quality Gates blocked (presence of `QUALITY_GATES_FAILED.md`)
- (Optionally) recently promoted solutions in `10_Practice/Offerings/Solutions/` and `40_Library/Solution_Briefs/`

Output is a concise tabular report — single message, glanceable. Read-only — does not modify any files.
