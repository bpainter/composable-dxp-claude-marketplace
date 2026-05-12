---
description: Run only Stage 4 (Ship) — promote finished artifact to 20_Proposals/Active/[Pursuit]/Final/, update pursuit _Brief.md and Decision_Log.md, archive WIP.
---

Invoke the `proposal-factory-ship` skill. The user wants to promote the polished proposal to the pursuit folder. The skill:

1. **Verify polish complete.** Confirm PPTX/PDF files exist at the top level of WIP. Confirm any confirmed companions have their deployment URLs / passwords captured in `_secrets.md`.

2. **Final self-review.** Walk a pre-ship checklist with the user:
   - Win Theme consistent across sections?
   - Scope ↔ Pricing ↔ Schedule ↔ Team consistent?
   - Compliance matrix complete (Large variants with RFP)?
   - Companions tested and deployment URLs working?
   - Cover letter (Large-Document) signed and present?
   - Brand applied correctly?

3. **Promote via `proposal-factory-promoter` agent.** The agent:
   - Copies finished files to `20_Proposals/Active/[ClientName_Descriptor]/Final/`
   - Naming: `[ClientName]_Proposal_[YYYY-MM-DD].pptx` + `.pdf` (or `.docx` + `.pdf` for Large-Document)
   - Companions: `Final/companions/poc/`, `Final/companions/video/`, `Final/companions/microsite/` with URLs + passwords
   - Updates `20_Proposals/Active/[ClientName_Descriptor]/_Brief.md` with submission details
   - Updates `20_Proposals/Active/[ClientName_Descriptor]/Decision_Log.md` with pivotal proposal decisions

4. **Archive WIP.** Move `WIP/[ClientName_Descriptor]/` to `99_Archive/Proposal_Factory_WIP/[ClientName_Descriptor]_[YYYY-MM-DD]/`. Or delete if the user prefers (canonical artifact is now in `20_Proposals/Active/...`).

5. **Tag in Salesforce** per `50_Knowledge/Frameworks/Pursuit_Excellence/`.

Output: pursuit's `Final/` populated; `_Brief.md` + `Decision_Log.md` updated; WIP archived.

Post-decision (separate flow — not part of Ship): when the client decides, the pursuit folder moves to `20_Proposals/Won/` or `20_Proposals/Lost/`. Win-loss review goes in `20_Proposals/Win_Loss_Reviews/`.
