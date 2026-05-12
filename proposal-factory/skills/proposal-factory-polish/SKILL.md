---
name: proposal-factory-polish
description: >
  Stage 3 orchestrator for the Proposal Factory — Polish. Generates visuals via the
  canonical toolchain (Napkin / Figma+Mermaid / PowerPoint native / higgsfield /
  Flaticon), formats markdown to PPTX (slides variants) or PDF via Word
  (Large-Document), applies brand (Slalom default vs. Composable DXP secondary),
  confirms companions tentatively flagged at intake, and calls
  proposal-factory-companion-router to produce confirmed companions. Generates
  speaker notes for slide variants. Output: finalized files at top level of WIP
  alongside markdown sources, plus companion outputs in assets/companions/. Also
  known as: visual + format + brand stage, polish runner.

type: skill
project: skills-library
plugin: proposal-factory
aliases: [proposal-factory-polish]
tags: [type/skill, plugin/proposal-factory, scope/orchestrator, topic/polish]
status: active
---

## Goal

Convert finished markdown into formatted output (PPTX / PDF) with brand applied and visuals generated. Confirm + produce companions.

## When to invoke

- User runs `/proposal-polish`
- Called by `proposal-factory-create-flow` at Stage 3
- User says: "polish the proposal," "make the slides," "format the document"

**Prerequisite:** Draft is complete. Markdown files exist in WIP folder. If not, route back to `proposal-factory-draft`.

## Required context

See [`../../references/proposal-factory-toolchain.md`](../../references/proposal-factory-toolchain.md) for full toolchain routing. See [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md) for variant rules + brand rules.

## Steps

### Step 1 — Visual generation

For each `[VISUAL: ...]` tag in the markdown, route to the right tool:

- **Diagram — Napkin, corporate** → call Napkin.ai. Save SVG/PNG to `WIP/[Pursuit]/assets/diagrams/`.
- **Diagram — Figma, flowchart** → render Mermaid via Figma, export. Save to `assets/diagrams/`.
- **Chart — PowerPoint, [type]** → keep as a placeholder; the PowerPoint MCP will render at slide assembly.
- **Image — higgsfield, nano banana** → call `mcp__79eb9064-...__generate_image`. Save to `assets/images/`.
- **Video — higgsfield, seedance** → call `mcp__79eb9064-...__generate_video`. Save to `assets/video/`.
- **Icon — Flaticon, lined/colored** → check `50_Knowledge/Frameworks/Slalom_Brand/Iconography` for refined Flaticon set; fall back to Flaticon search. Save to `assets/icons/`.

For higgsfield image generation:
1. Call `mcp__79eb9064-...__balance` first to confirm credits.
2. Call `mcp__79eb9064-...__generate_image` with prompt + aspect ratio.
3. Call `mcp__79eb9064-...__media_confirm` if needed for review.

### Step 2 — Format to output

By variant:

#### SPRO / Small / Large-Slides → 16:9 PPTX

1. Open base .potx via PowerPoint MCP:
   - Slalom default: `70_Templates/Slalom_Brand_Assets/Slalom_PowerPoint_16x9_Aug2025.potx`
   - Composable DXP: same base + TFIC tokens
2. Create presentation: `mcp__PowerPoint__By_Anthropic___create_presentation`.
3. For each slide / section in the markdown:
   - `add_slide` with appropriate layout
   - `set_slide_title`
   - `add_text_to_slide` for body content
   - `insert_image` for visuals from `assets/`
   - For charts: use the master's chart placeholder layouts (PowerPoint native)
4. `save_presentation` to `WIP/[Pursuit]/0X_[VariantName].pptx`.
5. `export_pdf` to `WIP/[Pursuit]/0X_[VariantName].pdf`.

#### Large-Document → 8.5×11 PDF via Word

1. `mcp__Word__By_Anthropic___create_document` with brand-aligned styles.
2. For each chapter:
   - Apply heading style
   - `insert_text` for body prose
   - `format_text` for callouts / sidebars
   - Embed visuals from `assets/`
3. Generate Table of Contents.
4. Apply per-RFP formatting if specified (font / spacing / margins).
5. `save_document` to `WIP/[Pursuit]/04_Large_Proposal_Document.docx`.
6. `export_pdf` to `WIP/[Pursuit]/04_Large_Proposal_Document.pdf`.

### Step 3 — Apply brand

#### Slalom default
- Avenir Next LT Pro typography (already in .potx)
- Slalom color system
- Slalom logo bottom-right corner of cover
- Replace any embedded illustrated icons with refined Flaticon icons
- Delete the .potx's "PowerPoint Tips and Tricks" + "Visual Design System" sections before delivery

#### Composable DXP secondary
- Same base .potx + TFIC overlay
- Lora display type (bold, no italic)
- TFIC color tokens from `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/colors_and_type.css`
- TFIC mark + wordmark from same path
- **Em-dash banned in all copy** — verify before finalizing
- **No marketing-speak verbs** — verify before finalizing

### Step 4 — Confirm companions

For each companion flagged at intake, AskUserQuestion to confirm:

```
Question: "POC was flagged at intake. Confirm production?"
Options:
1. Yes — produce the POC
2. No — drop it (and update the proposal to remove POC references)
3. Defer — decide post-submit
```

Repeat for video + microsite.

### Step 5 — Companion production (call companion-router)

For each confirmed companion, hand off to `proposal-factory-companion-router`. The router:

- **POC:** loads `05_POC_Brief.md`, calls Website Factory plugin, deploys via Vercel MCP.
- **Walkthrough video:** loads `06_Walkthrough_Video_Script.md`, generates visuals via higgsfield, voiceover via Elevenlabs, prompts user to assemble + export.
- **Microsite:** loads `07_Microsite_Plan.md`, calls design + frontend skills, deploys via Vercel MCP.

Return: URLs + passwords for any deployed companions.

### Step 6 — Speaker notes (slide variants only)

For each slide in the PPTX, generate speaker notes:

- Talk track (2–4 sentences of what to say)
- Key claim (the slide's load-bearing point)
- Pushback line (anticipated objection + response — Challenger-style)
- Per-buyer Win-Result tie-in (Strategic Selling)

Embed via PowerPoint MCP into the slide notes section.

### Step 7 — Visual review

Walk the visual-review checklist:

- Same icon style across the proposal (lined OR colored — pick one)
- Same chart style (PowerPoint native consistently)
- Diagrams generated for the dominant brand (Napkin corporate; never vibrant)
- No hand-drawn icons
- Imagery is real (no placeholder text surviving)

## Outputs

In `WIP/[ClientName_Descriptor]/`:
- Finalized PPTX (slide variants) or DOCX + PDF (document variant)
- PDF exports of slide variants
- `assets/` populated with diagrams / charts / icons / images / video
- `assets/companions/poc/` with URL + password (if POC confirmed)
- `assets/companions/video/[Pursuit]_Walkthrough.mp4` (if video confirmed)
- `assets/companions/microsite/` with URL + password (if microsite confirmed)
- `_secrets.md` with all passwords

## Pause point

After visuals + format + companions:

```
Question: "Polish complete. Move to Ship?"
Options:
1. Yes — promote to pursuit folder (Recommended)
2. Wait — I want to revise [specific element]
3. The compliance matrix needs another pass (Large variants)
```

## Skill discipline

- Don't start Polish if the markdown isn't approved.
- Don't produce companions if the proposal substance isn't settled.
- Don't ship em-dashes or marketing-speak verbs in TFIC copy.
- Don't use hand-drawn icons.
- Verify pricing + scope + schedule consistency before generating speaker notes.

## See also

- [`../proposal-factory-draft/SKILL.md`](../proposal-factory-draft/SKILL.md)
- [`../proposal-factory-ship/SKILL.md`](../proposal-factory-ship/SKILL.md)
- [`../../agents/proposal-factory-companion-router.md`](../../agents/proposal-factory-companion-router.md)
- [`../../references/proposal-factory-toolchain.md`](../../references/proposal-factory-toolchain.md)
