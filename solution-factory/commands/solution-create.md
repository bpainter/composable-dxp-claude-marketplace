---
description: Start a new solution end-to-end (Frame → Draft → Polish → Ship). Default for new chats in the Solution Factory Cowork project.
---

Invoke the `solution-factory-create-flow` skill. The user wants to create a new solution from scratch. The skill:

1. Diagnoses intent at Stage 0 with AskUserQuestion (new / refresh / single-stage / status)
2. Routes to `solution-factory-frame` for Stage 1 intake
3. Routes to `solution-factory-draft` for Stage 2 markdown drafting of all 8 deliverables
4. Routes to `solution-factory-polish` for Stage 3 visuals + format + brand
5. Routes to `solution-factory-ship` for Stage 4 Quality Gates + dual-routed promotion
6. Pauses for review at the end of each stage

Use AskUserQuestion at every discrete decision. Use conversational input for free-text (Reframe, problem statement, listen-fors, etc.). Capture into `00_Intake_Brief.md` as you go.

For pipeline mechanics, the `solution-factory-create-flow` skill cites `references/solution-factory-foundations.md`. Don't re-explain the pipeline.
