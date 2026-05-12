---
description: Run only Stage 2 (Draft) — variant-aware markdown drafting of the proposal + companion drafts if flagged. Pre-requires a completed 00_Intake_Brief.md.
---

Invoke the `proposal-factory-draft` skill. The user wants to draft the proposal in markdown. The skill is variant-aware:

- **SPRO** — drafts `01_SPRO.md` (one slide of content)
- **Small** — drafts `02_Small_Proposal.md` (12–18 slides) using section snippets inline
- **Large-Slides** — drafts master file + standalone section files for each section (Executive_Summary, Win_Theme, Approach, Scope, Team_Bios, Pricing, Schedule, Risks_Assumptions, References, Compliance_Matrix)
- **Large-Document** — same as Large-Slides but with document-shape (longer prose, fewer bullets, page-limit tracking)

If companions were flagged at intake, drafts companion files in parallel:
- `05_POC_Brief.md` — handoff brief for the Website Factory
- `06_Walkthrough_Video_Script.md` — scene-by-scene script
- `07_Microsite_Plan.md` — page structure + deployment plan

The skill pulls section content from `60_Digital_Manufacturing/Proposal_Factory/Templates/Sections/`. It does NOT re-author the snippets; it adapts them for this specific pursuit using the intake fields as source.

Calls specialist skills from other plugins as needed:
- `consulting:consulting-management-consultant` for Approach, Scope, Pricing rationale
- `marketing:marketing-copywriter` for Executive Summary + Win Theme prose
- `brand:brand-voice-tone` for voice alignment (Slalom default warm-confident vs. Composable DXP literary-confident)
- `consulting:consulting-humanize` for final-pass cleanup of AI-flavored prose
- `consulting:consulting-sow-risk-advisor` for Scope + Risks/Assumptions framing
- `solution-factory` plugin if pursuit aligns to a known solution kit

Output: completed markdown files in `WIP/[ClientName_Descriptor]/`. Self-review pause point at end before Polish.
