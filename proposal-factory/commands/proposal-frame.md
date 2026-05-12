---
description: Run only Stage 1 (Frame) — intake walkthrough + auto-run RFP parser if an RFP/RFQ PDF is attached. Output is a filled 00_Intake_Brief.md in WIP/. AskUserQuestion-driven for discrete fields; conversational for free-text.
---

Invoke the `proposal-factory-frame` skill. The user wants to run only the intake stage — capture variant + brand + companion flags + Win Theme + buyer state + posture + buyers + scope + pricing + team. If an RFP/RFQ PDF is in the WIP folder, the skill auto-runs the `proposal-factory-rfp-parser` agent first; its output (`01_RFP_Parsed.md`) pre-fills several intake fields.

The skill walks 7 prompt rounds (varies based on RFP presence and variant):

1. Client name + Pursuit descriptor + variant + brand
2. RFP attached? If yes, run `proposal-factory-rfp-parser` agent; pause to confirm parsed output before continuing
3. Companion flags (tentative — POC / video / microsite / none)
4. The discrete required calls (Buyer State / Posture / Capability)
5. Win Theme + Deb Oler answer (free-text, conversational)
6. Client problem + scope + buyers + competitors
7. Pricing model + indicative total + team + schedule + open questions

Calls the `proposal-factory-scaffolder` agent after Round 1 to set up the WIP folder.

Output: `WIP/[ClientName_Descriptor]/00_Intake_Brief.md` filled, with the readiness check at the bottom. Plus `01_RFP_Parsed.md` if an RFP was attached.

If the user wants to continue to Draft afterwards, hand off to the `proposal-factory-create-flow` orchestrator.
