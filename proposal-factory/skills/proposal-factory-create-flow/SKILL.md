---
name: proposal-factory-create-flow
description: >
  The main orchestrator for the Slalom Proposal Factory. Auto-activates when a user
  starts a new chat in the Proposal Factory Cowork project. Diagnoses intent (new
  proposal / refresh / single-stage / status), routes to the right stage
  orchestrator, and walks the user through the four-stage pipeline (Frame → Draft →
  Polish → Ship) end-to-end. Uses AskUserQuestion for discrete decisions;
  conversational input for free-text fields. Sequences other Proposal Factory
  orchestrators (frame, draft, polish, ship) and pulls specialty skills from
  consulting, design, marketing, brand, vercel, website-factory plugins where
  needed. Also known as: proposal factory orchestrator, proposal-create entry
  point, full pipeline runner.

type: skill
project: skills-library
plugin: proposal-factory
aliases: [proposal-factory-create-flow]
tags: [type/skill, plugin/proposal-factory, scope/orchestrator, topic/pipeline]
status: active
---

## Goal

Take the user from "I want to create a proposal for [client]" to a promoted, ready-to-submit proposal in `20_Proposals/Active/[Pursuit]/Final/`. End-to-end. Cowork-native — uses AskUserQuestion at every discrete decision.

## When to invoke

- User starts a new chat in the Proposal Factory Cowork project (default)
- User runs `/proposal-create`
- User asks any version of: "create a new proposal," "draft a proposal for [client]," "we need to respond to [RFP]," "let's start the [client] pursuit"

For partial workflows (just intake, just polish, etc.), route to the stage orchestrator directly via the matching slash command — `/proposal-frame`, `/proposal-draft`, `/proposal-polish`, `/proposal-ship`.

## Required context

For pipeline mechanics, variants, companion rules, and anti-patterns, see [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md). For visual / output / deployment toolchain, see [`../../references/proposal-factory-toolchain.md`](../../references/proposal-factory-toolchain.md). **Do not re-explain the pipeline** — cite the foundations file.

## Stages

### Stage 0 — Diagnose intent (30 sec)

Before invoking any stage orchestrator, confirm what the user wants. Use **AskUserQuestion**:

```
Question: "What would you like to do?"
Options:
1. Create a new proposal from scratch (full pipeline) — Recommended
2. Update or refresh an existing proposal
3. Run only one stage (frame / draft / polish / ship)
4. Check the status of proposals currently in WIP
```

**If "Create new":** proceed to Stage 1.

**If "Update or refresh":** ask for the existing pursuit name. Pull the prior `00_Intake_Brief.md` if found. Skip to the stage where revisions begin (usually Draft).

**If "Run only one stage":** route to the matching orchestrator skill (`proposal-factory-frame`, `-draft`, `-polish`, or `-ship`) without running the full flow.

**If "Check status":** invoke `proposal-factory-status` and stop.

### Stage 1 — Frame

Hand off to **`proposal-factory-frame`**. That skill:

1. Walks the intake using AskUserQuestion for discrete fields (variant / brand / companions / buyer state / posture / capability) and conversational input for free-text (Win Theme / Deb Oler answer / problem / scope / pricing).
2. Auto-runs the `proposal-factory-rfp-parser` agent if an RFP/RFQ PDF is attached.
3. Calls the `proposal-factory-scaffolder` agent to create the WIP folder.

Returns when `00_Intake_Brief.md` (and optionally `01_RFP_Parsed.md`) exists in `WIP/[ClientName_Descriptor]/` and the readiness check passes.

**Pause point:** confirm intake before drafting. Use AskUserQuestion:

```
Question: "Intake complete. Ready to start drafting?"
Options:
1. Yes, start drafting (Recommended)
2. Yes, but draft only the Win Theme + Executive Summary first so I can review the framing
3. Pause — I want to refine the intake first
```

**Recommendation logic:** if the readiness check failed any required field, default to option 3.

### Stage 2 — Draft

Hand off to **`proposal-factory-draft`**. That skill drafts the proposal per variant:

- **SPRO:** drafts the single-slide markdown
- **Small:** drafts the deck markdown using inline section snippets
- **Large-Slides:** drafts master file + standalone section files (Executive_Summary, Win_Theme, Approach, Scope, Team_Bios, Pricing, Schedule, Risks_Assumptions, References, Compliance_Matrix)
- **Large-Document:** same as Large-Slides but with document-shape (longer prose, page-budget tracking)

Companion drafts authored in parallel if flagged at intake.

Pulls specialist skills:
- `consulting:consulting-management-consultant` — Approach, Scope, Pricing rationale
- `marketing:marketing-copywriter` — Executive Summary + Win Theme prose
- `brand:brand-voice-tone` — voice alignment per brand
- `consulting:consulting-humanize` — final-pass cleanup of AI-flavored prose
- `consulting:consulting-sow-risk-advisor` — Scope + Risks framing
- `solution-factory` plugin if pursuit aligns to a known solution kit

**Pause point:** self-review all markdown. Use AskUserQuestion:

```
Question: "Draft complete. Ready to Polish?"
Options:
1. Yes, generate visuals + format to PPTX/PDF (Recommended)
2. Yes, but I want to revise the Win Theme / Approach first
3. Pause — need to think about [specific section]
```

### Stage 3 — Polish

Hand off to **`proposal-factory-polish`**. That skill:

1. Generates visuals via the canonical toolchain (Napkin / Figma+Mermaid / PowerPoint native / higgsfield / Flaticon).
2. Formats to output (PPTX for slide variants; PDF via Word for document variant).
3. Applies brand (Slalom default or Composable DXP secondary).
4. Confirms companions with AskUserQuestion for each flagged at intake.
5. For confirmed companions, calls `proposal-factory-companion-router` which runs the companion sub-flows.
6. Generates speaker notes for slide variants.

**Pause point:** visual review. Use AskUserQuestion:

```
Question: "Polish complete. Ready to Ship?"
Options:
1. Yes, promote to the pursuit folder (Recommended)
2. Wait — I want to revise [specific element]
3. Compliance matrix needs another pass (Large only)
```

### Stage 4 — Ship

Hand off to **`proposal-factory-ship`**. That skill:

1. Walks the pre-ship self-review checklist.
2. Calls the `proposal-factory-promoter` agent to copy finished files to `20_Proposals/Active/[ClientName_Descriptor]/Final/`.
3. Updates the pursuit's `_Brief.md` and `Decision_Log.md`.
4. Archives WIP.
5. Reminds the user to tag in Salesforce per Pursuit Excellence.

Output: pursuit's `Final/` populated.

## Skill discipline

- Don't bulldoze through stages. Pause at every stage boundary.
- Don't re-explain the pipeline. Cite the foundations file.
- Don't author content without an intake. If the user says "draft a proposal" without an intake, route to Frame first.
- Don't generate visuals before markdown is approved. Polish runs after Draft is signed off.
- Don't generate companions before proposal substance is settled. Companion production happens at Polish.

## See also

- [`../../README.md`](../../README.md) — plugin overview
- [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md)
- [`../../references/proposal-factory-toolchain.md`](../../references/proposal-factory-toolchain.md)
- [`../proposal-factory-frame/SKILL.md`](../proposal-factory-frame/SKILL.md)
- [`../proposal-factory-draft/SKILL.md`](../proposal-factory-draft/SKILL.md)
- [`../proposal-factory-polish/SKILL.md`](../proposal-factory-polish/SKILL.md)
- [`../proposal-factory-ship/SKILL.md`](../proposal-factory-ship/SKILL.md)
- [Pipeline.md](../../../../60_Digital_Manufacturing/Proposal_Factory/Pipeline.md)
