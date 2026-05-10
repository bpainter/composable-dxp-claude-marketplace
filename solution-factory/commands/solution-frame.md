---
description: Run only Stage 1 (Frame) — the intake walkthrough. Output is a filled 00_Intake_Brief.md in WIP/. AskUserQuestion-driven for discrete fields; conversational for free-text.
---

Invoke the `solution-factory-frame` skill. The user wants to run only the intake stage — capture the 5 required calls (Reframe / Deb Oler / Buyer State / Posture / Tier) plus problem statement, components, capabilities, industries, buyer personas, and listen-fors.

The skill walks 6 prompt rounds:
1. Solution name + Brand
2. The 4 discrete required calls (Buyer State / Posture / Tier / Capability)
3. Reframe + Deb Oler answer (free-text, conversational)
4. Problem + components + industries
5. Buyer personas + listen-fors
6. Pull-forward optional fields + tracking metadata

Calls the `solution-factory-scaffolder` agent after Round 1 to set up the WIP folder.

Output: `WIP/[solution-name]/00_Intake_Brief.md` filled, with a 13-item readiness check at the bottom.

If the user wants to continue to Draft afterwards, hand off to the `solution-factory-create-flow` orchestrator.
