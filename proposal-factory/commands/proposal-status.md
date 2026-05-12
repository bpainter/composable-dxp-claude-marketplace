---
description: Show status of pursuits in WIP/ — what's at which stage, what's blocked, what's stale, and (optionally) what's been promoted to Active/Final/ recently. Read-only.
---

Invoke the `proposal-factory-status` skill. The user wants a read-only status report on active pursuits. The skill:

1. Inspects `60_Digital_Manufacturing/Proposal_Factory/WIP/` and reports on each folder:
   - **Pursuit name** — `[ClientName_Descriptor]`
   - **Variant** — SPRO / Small / Large-Slides / Large-Document
   - **Brand** — Slalom default / Composable DXP secondary
   - **Stage** — Frame / Draft / Polish / Ship-Ready
   - **Completeness** — what's done, what's missing
   - **Companions flagged** — POC / video / microsite
   - **Submission deadline** — from intake
   - **Staleness** — last modified date; flags anything ≥7 days untouched

2. Optionally inspects `20_Proposals/Active/` to report on pursuits whose proposals have been submitted but the decision is pending.

3. Optionally inspects `20_Proposals/Won/` and `Lost/` for recent activity (past 30 days).

Output: a structured status report. Does not modify anything.

For Cowork rendering, consider offering to render the status as a live artifact (via `create_artifact`) that the user can re-open to refresh.
