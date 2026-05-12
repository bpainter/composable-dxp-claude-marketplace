---
name: proposal-factory-ship
description: >
  Stage 4 orchestrator for the Proposal Factory — Ship. Walks a self-review
  checklist, then promotes the finished artifact to
  20_Proposals/Active/[ClientName_Descriptor]/Final/, updates the pursuit's
  _Brief.md and Decision_Log.md, archives WIP. Calls the proposal-factory-promoter
  agent for the file-system work. No multi-reviewer Quality Gates ceremony —
  single-operator. Output: pursuit's Final/ populated with proposal + companion
  artifacts; WIP archived. Also known as: promote, ship runner, proposal
  finalization.

type: skill
project: skills-library
plugin: proposal-factory
aliases: [proposal-factory-ship]
tags: [type/skill, plugin/proposal-factory, scope/orchestrator, topic/ship, topic/promotion]
status: active
---

## Goal

Promote the polished proposal + companions to the pursuit folder. Update pursuit records. Archive WIP.

## When to invoke

- User runs `/proposal-ship`
- Called by `proposal-factory-create-flow` at Stage 4
- User says: "ship it," "promote the proposal," "we're ready to send"

**Prerequisite:** Polish complete. Final PPTX/PDF files exist at the top level of WIP. If not, route back to `proposal-factory-polish`.

## Required context

See [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md) for promotion routing logic.

## Steps

### Step 1 — Pre-ship self-review checklist

Walk the user through the checklist via AskUserQuestion (one yes/no/skip per item):

Cross-document consistency:
- [ ] Win Theme identical across cover, Executive Summary, Win Theme section, microsite hero (if applicable)
- [ ] Scope ↔ Pricing — pricing covers exactly what's in scope
- [ ] Approach phases ↔ Schedule durations — match
- [ ] Pricing ↔ Schedule — phase pricing matches phase timing
- [ ] Team ↔ Approach — every phase has named team members from the team roster

RFP compliance (Large variants):
- [ ] Compliance matrix complete — every mandatory requirement has a row
- [ ] Slide / page references updated to actual locations (not placeholders)
- [ ] Page / slide limit respected
- [ ] Submission format matches RFP (PDF / PPTX / portal / email)

Brand:
- [ ] Brand applied correctly throughout (Slalom default OR Composable DXP secondary)
- [ ] No em-dashes in TFIC copy (Composable DXP only)
- [ ] No marketing-speak verbs (especially TFIC)
- [ ] Logo placement correct

Content quality:
- [ ] Cover letter signed (Large-Document only)
- [ ] Speaker notes complete (slide variants)
- [ ] No "Lorem ipsum" / placeholder content remaining
- [ ] Spelling: client name + product names + buyer names verified
- [ ] References named (with permission verified)

Companions (if applicable):
- [ ] POC URL works and is password-protected
- [ ] POC tested in incognito with the password
- [ ] Walkthrough video plays and is hosted (Vimeo unlisted / microsite)
- [ ] Microsite URL works on mobile + desktop, password-protected

If any checklist item fails, AskUserQuestion to route the user back to the failing stage:

```
Question: "[Failing item]. Route back to which stage?"
Options:
1. Polish — fix the visual / format / brand issue
2. Draft — fix the substance issue
3. Frame — re-check the intake
4. I'll fix it in place, then continue
```

### Step 2 — Call the promoter agent

Once the checklist passes, call `proposal-factory-promoter` with:

- Pursuit name (`[ClientName_Descriptor]`)
- Variant
- WIP folder path
- Final artifact filename(s)
- Companion outputs paths (if any)
- Pursuit folder target (`20_Proposals/Active/[ClientName_Descriptor]/`)

The agent:
1. Verifies `20_Proposals/Active/[ClientName_Descriptor]/` exists. If not, prompts the user to confirm folder name (sometimes the WIP name and the pursuit folder name diverge).
2. Creates `20_Proposals/Active/[ClientName_Descriptor]/Final/` if it doesn't exist.
3. Copies finished files:
   - Slide variants: `[ClientName]_Proposal_[YYYY-MM-DD].pptx` + `.pdf`
   - Document variant: `[ClientName]_Proposal_[YYYY-MM-DD].docx` + `.pdf`
4. Copies companions:
   - `Final/companions/poc/URL.md` (with URL + password)
   - `Final/companions/video/[Pursuit]_Walkthrough.mp4` + `URL.md`
   - `Final/companions/microsite/URL.md` (with URL + password)
5. Updates `20_Proposals/Active/[ClientName_Descriptor]/_Brief.md` — appends submission details:
   - Submission date
   - Variant
   - Investment
   - Key Win Theme
   - Companions
6. Appends to `20_Proposals/Active/[ClientName_Descriptor]/Decision_Log.md` — pivotal proposal decisions:
   - Why this variant
   - Why this pricing model
   - Why this team
   - Any concessions made during drafting

### Step 3 — Archive WIP

AskUserQuestion:

```
Question: "Proposal promoted. Archive WIP folder?"
Options:
1. Yes — move to 99_Archive/Proposal_Factory_WIP/ (Recommended)
2. Yes — delete WIP (disk is tight)
3. No — keep WIP for now
```

If archive: move `WIP/[ClientName_Descriptor]/` to `99_Archive/Proposal_Factory_WIP/[ClientName_Descriptor]_[YYYY-MM-DD]/`.

### Step 4 — Salesforce reminder

Remind the user:

> *"Don't forget to tag in Salesforce per `50_Knowledge/Frameworks/Pursuit_Excellence/`. The pursuit is now in Submitted state. Update the opportunity record + add the Solution tag(s) used in this proposal."*

### Step 5 — Post-decision instructions (future)

Remind the user of the post-decision flow:

> *"When the client decides:*
> - *Won → move `20_Proposals/Active/[ClientName_Descriptor]/` to `20_Proposals/Won/`. Then create `30_Engagements/Active/[ClientName_Descriptor]/_Charter.md` with a backlink.*
> - *Lost → move to `20_Proposals/Lost/`. Capture lessons in `20_Proposals/Win_Loss_Reviews/`.*
>
> *The pursuit folder lives wherever it ends up — the proposal artifact stays with it as the historical record."*

## Outputs

- `20_Proposals/Active/[ClientName_Descriptor]/Final/` populated with proposal + companions
- Pursuit's `_Brief.md` updated with submission details
- Pursuit's `Decision_Log.md` updated with pivotal proposal decisions
- WIP archived or kept per user choice

## Skill discipline

- Don't promote a proposal that hasn't been polished.
- Don't promote if any checklist item fails without an explicit user override.
- Don't silently overwrite an existing Final/ artifact — version (`_v2`) or replace explicitly.
- Don't forget to update `_Brief.md` and `Decision_Log.md` — they're load-bearing for pursuit accountability.

## See also

- [`../proposal-factory-polish/SKILL.md`](../proposal-factory-polish/SKILL.md)
- [`../../agents/proposal-factory-promoter.md`](../../agents/proposal-factory-promoter.md)
- [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md)
- [20_Proposals/Active/README.md](../../../../20_Proposals/Active/README.md)
