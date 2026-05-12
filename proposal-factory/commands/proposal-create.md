---
description: Start a new proposal end-to-end (Frame → Draft → Polish → Ship). Default for new chats in the Proposal Factory Cowork project.
---

Invoke the `proposal-factory-create-flow` skill. The user wants to create a new proposal from scratch. The skill:

1. Diagnoses intent at Stage 0 with AskUserQuestion (new / refresh / single-stage / status)
2. Routes to `proposal-factory-frame` for Stage 1 intake (variant + brand + companions + RFP auto-parse + Win Theme + buyers + scope + pricing + team)
3. Routes to `proposal-factory-draft` for Stage 2 variant-aware markdown drafting
4. Routes to `proposal-factory-polish` for Stage 3 visuals + format + brand + confirmed companion production
5. Routes to `proposal-factory-ship` for Stage 4 promote to `20_Proposals/Active/[Pursuit]/Final/`
6. Pauses for self-review at the end of each stage

Use AskUserQuestion at every discrete decision (variant, brand, companion flags, buyer state, posture). Use conversational input for free-text (Win Theme, problem statement, Deb Oler answer). Capture into `00_Intake_Brief.md` as you go.

For pipeline mechanics, the `proposal-factory-create-flow` skill cites `references/proposal-factory-foundations.md`. Don't re-explain the pipeline.
