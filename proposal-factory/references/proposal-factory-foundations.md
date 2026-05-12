---
type: reference
project: skills-library
scope: plugin
plugin: proposal-factory
tags: [type/reference, plugin/proposal-factory, scope/foundational, topic/proposal-factory]
status: active
---

# Proposal Factory — Foundations

Plugin-wide shared reference. Cited by every skill in the `proposal-factory` plugin. Don't re-explain this content inline — link or quote sparingly.

## What the Proposal Factory is

The Slalom Proposal Factory produces pursuit-ready proposals from intake to submitted artifact. It lives at [`60_Digital_Manufacturing/Proposal_Factory/`](../../../60_Digital_Manufacturing/Proposal_Factory/).

**Single-operator factory.** Bermon is the sole user and reviewer. No multi-reviewer Quality Gates ceremony. Pause points between stages are self-review.

## Output variants — pick one at intake

| Variant | Length | Format | When |
|---|---|---|---|
| **SPRO** | 1 slide | 16:9 PPTX | Direct sell, trust-based, Advisory or small Simple scope |
| **Small** | 12–18 slides | 16:9 PPTX | Middle ground, informal evaluation |
| **Large-Slides** | 50–150 slides | 16:9 PPTX | RFP with slide-based evaluation |
| **Large-Document** | 8.5×11 paged | PDF (Word source) | RFP with document-based evaluation |

Full decision rules in [`60_Digital_Manufacturing/Proposal_Factory/Output_Variants.md`](../../../60_Digital_Manufacturing/Proposal_Factory/Output_Variants.md).

## Companion pieces — flagged at intake, confirmed at Polish

| Companion | What it is | Tooling |
|---|---|---|
| **POC** | Clickable prototype walking through personas + journeys + key flows | Website Factory plugin + Vercel MCP |
| **Walkthrough video** | Recorded screen + Elevenlabs voiceover | higgsfield + Elevenlabs |
| **Microsite** | Static HTML/CSS/JS proposal companion with quick navigation | Vercel MCP + Claude Design / Claude Code |

Full decision rules in [`60_Digital_Manufacturing/Proposal_Factory/Companions.md`](../../../60_Digital_Manufacturing/Proposal_Factory/Companions.md).

## The four-stage pipeline

```
Frame  →  Draft  →  Polish  →  Ship
~15-45m  ~hours   ~hours    ~30min
```

- **Frame** — intake. Variant + brand + companion flags + Win Theme + buyers + scope + pricing + team + competitors + schedule. RFP auto-parse runs as a step inside Frame if an RFP/RFQ PDF is in the WIP folder. Output: `00_Intake_Brief.md` (+ optionally `01_RFP_Parsed.md`).
- **Draft** — variant-aware markdown drafting. Pulls section snippets from `Templates/Sections/`. Companion drafts (POC brief, video script, microsite plan) authored in parallel if flagged. Output: markdown files.
- **Polish** — visuals via the toolchain; format to output (PPTX or PDF); apply brand; confirm companions and run companion sub-flows. Output: finalized files + companion artifacts.
- **Ship** — promote to `20_Proposals/Active/[Pursuit]/Final/`. Update pursuit `_Brief.md` and `Decision_Log.md`. Archive WIP. Output: canonical artifact at the pursuit folder.

## Required intake calls

These determine downstream decisions. MUST be captured before drafting begins:

1. **Variant** — SPRO / Small / Large-Slides / Large-Document.
2. **Brand** — Slalom default or Composable DXP secondary.
3. **Companion flags (tentative)** — POC / video / microsite / none.
4. **Win Theme** — one-line contrarian claim. Read aloud, produces *"huh, I never thought of it that way"* — not *"yes, exactly."*
5. **Buyer State** — Latent / Admitted / Vision / Evaluation (Collaborative Sale).
6. **Posture** — TO / FOR / WITH / BY (Mabee). Default: WITH them.
7. **Key buyers** — at minimum Economic Buyer + Coach named or explicit-TBD.

Plus the discrete fields: deadline, indicative pricing, capability, scope-in / scope-out, competitive landscape, team mix.

## Brand selection

If the pursuit's anchor capability includes any of {Composable, Headless, DXP, MACH, Contentful, Vercel, Algolia, Bynder, modern commerce}, recommend **Composable DXP secondary**. Otherwise **Slalom default**.

For Composable DXP secondary (TFIC sub-brand), critical voice rules:
- **Em-dash banned** in TFIC copy. Use commas, parentheses, or sentence breaks.
- **No marketing-speak verbs** ("elevate," "leverage," "seamless," "unleash," "next-gen").
- Voice is "literary-confident," not "tech-startup-bold."
- Display type: bold Lora, no italic.

## RFP auto-parse

If an RFP / RFQ PDF is present in the WIP folder when Frame starts, the `proposal-factory-rfp-parser` agent runs as a step inside Frame and produces `01_RFP_Parsed.md` with:

- All extracted mandatory requirements (shall / must / required)
- Suggested compliance matrix structure (rows: requirement → location)
- Page limit (if specified)
- Evaluation criteria + weighting (if specified)
- Submission format (PDF / PPTX / portal / email)
- Key questions to address

Frame uses the parsed output to pre-fill intake fields where possible.

## Promotion routing

Single target. Mirror the pursuit folder.

```
At Ship, copy finished artifact + companions to:

20_Proposals/Active/[ClientName_Descriptor]/Final/
├── [ClientName]_Proposal_[YYYY-MM-DD].pptx (or .docx for Large-Document)
├── [ClientName]_Proposal_[YYYY-MM-DD].pdf
└── companions/
    ├── poc/         # POC URL + password
    ├── video/       # MP4 + link
    └── microsite/   # microsite URL + password
```

Then update `_Brief.md` and `Decision_Log.md` in the pursuit folder. Archive WIP.

Post-decision (not part of Ship), the pursuit folder moves to `20_Proposals/Won/` or `Lost/` per the [20_Proposals README](../../../20_Proposals/README.md).

## Foundation knowledge the factory consumes

The Proposal Factory cites these — does not duplicate. Templates and skills point at them.

### Pursuit theory
- [Slalom Operating System](../../../50_Knowledge/Frameworks/Slalom_Operating_System/00_Master_Operating_System.md) — canonical OS, vocabulary, four postures, anti-patterns
- [Sales Theory Playbook](../../../50_Knowledge/Frameworks/Sales_Theory/00_Sales_Theory_Playbook.md) — cross-cutting cookbook; phrasing index
- [Strategic Selling](../../../50_Knowledge/Frameworks/Sales_Theory/01_The_New_Strategic_Selling.md) — Buying Influences, Win-Results, PPVVCC
- [Solution Selling Fieldbook](../../../50_Knowledge/Frameworks/Sales_Theory/02_The_Solution_Selling_Fieldbook.md) — Pain Chain, 9-Block, Sponsor Letter, Reference Story
- [Challenger Sale](../../../50_Knowledge/Frameworks/Sales_Theory/03_The_Challenger_Sale.md) — Commercial Teaching, Reframe (spine of Win Theme)
- [Collaborative Sale](../../../50_Knowledge/Frameworks/Sales_Theory/04_The_Collaborative_Sale.md) — Buyer States, Collaboration Plan, PPVVCC
- [Pursuit Excellence](../../../50_Knowledge/Frameworks/Pursuit_Excellence/) — pursuit operations + Salesforce tagging
- [Pursuit to Delivery handoff](../../../50_Knowledge/Frameworks/Pursuit_to_Delivery/00_Sales_to_Engagement_Handoff.md) — informs Approach + Schedule + post-award handoff

### Engagement theory (referenced for proposal Scope, Approach, Pricing)
- `consulting/references/book-flawless-consulting.md` — Block; contracting
- `consulting/references/book-management-consulting-kubr.md` — Kubr; six benefit categories
- `consulting/references/book-consulting-discipline-mabee.md` — Mabee; Four Postures
- `consulting/references/book-never-split-the-difference.md` — Voss; tactical empathy

### Slalom context
- [Slalom Capabilities](../../../50_Knowledge/Frameworks/Slalom_Capabilities/README.md) — the 7 capability areas
- [PE & DE Roles](../../../50_Knowledge/Frameworks/PE_DE_Roles/) — Principal Engineer + Delivery Engineer roles for Team_Bios

### Brand
- [Slalom Brand](../../../50_Knowledge/Frameworks/Slalom_Brand/) — default brand framework
- [Composable DXP Brand](../../../50_Knowledge/Frameworks/Composable_DXP_Brand/) — secondary brand framework
- [Slalom Brand Assets](../../../70_Templates/Slalom_Brand_Assets/) — .potx, brand guide, design system
- [Composable DXP Brand Assets](../../../70_Templates/Composable_DXP_Brand_Assets/) — TFIC design system

### Sibling factories
- [Solution Factory](../../../60_Digital_Manufacturing/Solution_Factory/) — when a pursuit aligns to a known solution, pull from the kit
- [Website Factory](../../../60_Digital_Manufacturing/Website_Factory/) — POC companion handoff target

### Past won proposals
- [`20_Proposals/Won/`](../../../20_Proposals/Won/) — Reference Story source

## Anti-patterns the factory refuses

Pulled from the Slalom OS anti-patterns. The most-violated in proposal work:

1. **Lead with Slalom** — logo / services bar in first slides. Lead with the Win Theme instead.
2. **Generic "we transform" framing** — no Reframe, no quantified pain. Reject.
3. **Capability brochure case studies** — bullets instead of narrative arcs. Reference Stories require six elements.
4. **Single-stakeholder assumption** — *"the customer"* without naming Economic / User / Technical / Coach.
5. **Win without Win** — closing without naming the personal stake of each buyer (Strategic Selling).
6. **Boundary failure** — proposal Approach uses different vocabulary than the eventual engagement.
7. **Free consulting** — insight that doesn't terminate at a Slalom unique strength.
8. **Open-ended discovery** — *"what's keeping you up at night?"* in the proposal pitch.
9. **Pitching Collins's eleven companies as proof** — several have failed since *Good to Great* published. Use his language, not his case studies.
10. **Marketing-speak verbs** — *"elevate / leverage / seamless / unleash / next-gen / transform."* Reject. Especially banned in TFIC copy.

## See also

- [`../README.md`](../README.md) — plugin overview
- [`proposal-factory-toolchain.md`](proposal-factory-toolchain.md) — visual + output + deployment toolchain
- [Pipeline.md](../../../60_Digital_Manufacturing/Proposal_Factory/Pipeline.md) — full workflow
- [Intake_Brief.md](../../../60_Digital_Manufacturing/Proposal_Factory/Intake_Brief.md) — intake template
- [Output_Variants.md](../../../60_Digital_Manufacturing/Proposal_Factory/Output_Variants.md) — variant rules
- [Companions.md](../../../60_Digital_Manufacturing/Proposal_Factory/Companions.md) — companion rules
- [Templates/](../../../60_Digital_Manufacturing/Proposal_Factory/Templates/) — variant + section + companion templates
- [instructions-claude-cowork.md](../../../60_Digital_Manufacturing/Proposal_Factory/instructions-claude-cowork.md) — Cowork project Instructions content
