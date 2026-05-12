---
name: proposal-factory-status
description: >
  Advice skill for the Proposal Factory. Inspects the WIP/ folder at
  60_Digital_Manufacturing/Proposal_Factory/WIP/ and reports on active pursuits —
  which are at which stage (Frame / Draft / Polish / Ship), how complete each is,
  what's blocked, what's stale. Optionally inspects the pursuit folders at
  20_Proposals/Active/ to report on submitted-but-pending pursuits. Optionally
  inspects 20_Proposals/Won/ and Lost/ for recent activity. Read-only — does not
  modify anything. Also known as: WIP status, proposal status, proposal factory
  dashboard.

type: skill
project: skills-library
plugin: proposal-factory
aliases: [proposal-factory-status]
tags: [type/skill, plugin/proposal-factory, scope/advice, topic/status]
status: active
---

## Goal

Give the user a read-only status report on active proposals.

## When to invoke

- User runs `/proposal-status`
- Called by `proposal-factory-create-flow` at Stage 0 if user picks "Check status"
- User says: "what's in flight," "show me my pursuits," "any proposals that need attention," "what's stale"

## Required context

None specific. Reads from filesystem.

## What to inspect

### Primary — WIP folders

`60_Digital_Manufacturing/Proposal_Factory/WIP/`

For each subfolder, read:

- `00_Intake_Brief.md` if present → variant, brand, deadline, Win Theme, companion flags
- Top-level markdown files → infer Draft progress
- Top-level `.pptx` / `.pdf` files → infer Polish progress
- `assets/companions/` subfolders → infer companion progress
- File mtimes → staleness

### Optional — Submitted pursuits

`20_Proposals/Active/[Pursuit]/Final/`

For pursuits where Final/ has been populated, list them with submission date (from `_Brief.md` or file mtime).

### Optional — Recent decisions

`20_Proposals/Won/` and `20_Proposals/Lost/` — list pursuits with mtime in last 30 days.

## Report format

Plain markdown report, or optionally a Cowork artifact via `create_artifact` so the user can re-open it.

### Active in WIP

For each pursuit:

| Pursuit | Variant | Brand | Stage | Deadline | Stale? | Companions |
|---|---|---|---|---|---|---|
| [Name] | [Variant] | [Brand] | [Frame/Draft/Polish/Ship-Ready] | [YYYY-MM-DD] | [days since touch] | [POC/Video/Microsite/None] |

### Stage inference rules

- **Frame** — `00_Intake_Brief.md` exists but readiness check has unchecked items
- **Draft** — readiness check passes, markdown files exist, no PPTX/PDF yet
- **Polish** — PPTX/PDF exists, companions in progress
- **Ship-Ready** — PPTX/PDF + all confirmed companions complete, no Final/ yet in pursuit folder

### Stale flag

- ≥7 days since last touch: flag as ⚠️ stale
- ≥14 days: 🚨 critical stale

### Submitted in Active/ (pending decision)

| Pursuit | Variant | Submitted | Days since | Anticipated decision |
|---|---|---|---|---|

### Recent Won / Lost (last 30 days)

| Pursuit | Outcome | Submitted | Decided |
|---|---|---|---|

## Cowork artifact (optional)

Offer to render the status as a live artifact:

> *"Want this as a live status page you can re-open? It'll refresh from the filesystem each time."*

If yes, call `create_artifact` with an HTML page that uses `window.cowork.callMcpTool` (workspace bash + read) to refresh the status on load.

Note: the artifact reads markdown files; it doesn't author or modify them.

## Skill discipline

- Read-only. Do not modify any files.
- Don't make assumptions about variant or stage if files are missing — surface "Cannot determine" rather than guess.
- Don't reveal information that's not in WIP / Active / Won / Lost.
- Be concise — the report is for the user's mental model, not a comprehensive audit.

## See also

- [`../proposal-factory-create-flow/SKILL.md`](../proposal-factory-create-flow/SKILL.md)
- [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md)
- [WIP/README.md](../../../../60_Digital_Manufacturing/Proposal_Factory/WIP/README.md)
- [20_Proposals/](../../../../20_Proposals/)
