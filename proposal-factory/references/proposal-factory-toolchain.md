---
type: reference
project: skills-library
scope: plugin
plugin: proposal-factory
tags: [type/reference, plugin/proposal-factory, scope/toolchain, topic/polish]
status: active
---

# Proposal Factory — Visual + Output + Deployment Toolchain

Plugin-wide shared reference for Stage 3 Polish. Cited by `proposal-factory-polish` and `proposal-factory-companion-router`.

Same canonical routing as Solution Factory plus Elevenlabs (voiceover) and Website Factory handoff for POC.

## Visual generation

### Diagrams + infographics → Napkin.ai

Architecture diagrams, framework overviews, process flows, comparison layouts, concept maps.

**Style:** `corporate` for Slalom default; `corporate` or `clean` for Composable DXP — *never* `vibrant` or `flat`.

```
[VISUAL: Diagram — Napkin, corporate]
[Description of what the diagram shows]
```

### Flowcharts + decision trees + sequence diagrams → Figma + Mermaid

Tighter branding control, anything that may be edited later by design, decision trees, sequences, swim lanes.

```
[VISUAL: Diagram — Figma, flowchart]
[Mermaid-style description]
```

### Charts + graphs → PowerPoint native

Bar / line / pie / comparison charts inside slides. Editable, themed. Avoid Python-rendered chart images.

```
[VISUAL: Chart — PowerPoint, bar]
Data: [headers + rows]
Source: [citation]
```

### Photographic + illustrative imagery → higgsfield MCP

Hero images, mood imagery, illustrative scenes, AI-generated images.

- **nano banana** (Google) — default model
- **chatgpt image 2** — fallback when nano banana doesn't fit the brief

```
[VISUAL: Image — higgsfield, nano banana]
Prompt: [the image prompt]
Aspect: [16:9 | 4:3 | 1:1 | 9:16]
Mood: [a few descriptors]
Brand alignment: [Slalom default | Composable DXP secondary]
```

Stock-first via the Slalom Creative Library (brand.slalom.com) before generating.

**Tools:** `mcp__79eb9064-...__generate_image`, `mcp__79eb9064-...__balance`, `mcp__79eb9064-...__media_confirm`.

### Animations + video segments → higgsfield MCP (Seedance)

Animated explainers, looping backgrounds, micro-animations, B-roll for the walkthrough video companion.

```
[VISUAL: Video — higgsfield, seedance]
Prompt: [the motion prompt]
Duration: [seconds]
Aspect: [16:9 | 9:16]
Looping: [yes | no]
```

**Tool:** `mcp__79eb9064-...__generate_video`.

### Icons → Flaticon

- **Lined** OR **colored flat** — one consistent style across the proposal.
- **No hand-drawn.** No isometric / 3D unless specifically called for.

```
[VISUAL: Icon — Flaticon, lined]
Subject: [what the icon represents]
Color: [from token palette]
```

For Slalom default, prefer the practice override at `50_Knowledge/Frameworks/Slalom_Brand/Iconography` (refined Flaticon set).

## Output formatting

### 16:9 PPTX → PowerPoint MCP

For variants: SPRO, Small, Large-Slides.

**Base file:**
- Slalom default: `70_Templates/Slalom_Brand_Assets/Slalom_PowerPoint_16x9_Aug2025.potx`
- Composable DXP: same base + TFIC token + typography overrides per `50_Knowledge/Frameworks/Composable_DXP_Brand/`

**Tools:** `mcp__PowerPoint__By_Anthropic___create_presentation`, `add_slide`, `set_slide_title`, `add_text_to_slide`, `insert_image`, `save_presentation`, `export_pdf`.

### 8.5×11 Word + PDF → Word MCP

For variant: Large-Document.

**Tools:** `mcp__Word__By_Anthropic___create_document`, `format_text`, `insert_text`, `replace_text`, `export_pdf`.

**Per-RFP formatting:** if the RFP specifies font / spacing / margins (federal often does), apply at document creation.

## Companion production

### POC → Website Factory plugin → Vercel deploy

1. Hand off `05_POC_Brief.md` to the `website-factory` plugin.
2. Authoring tool per the POC brief (Claude Design default; v0 alternative; Figma Make alternative).
3. Website Factory plugin produces the prototype.
4. Deploy via Vercel MCP `deploy_to_vercel` with Deployment Protection (password).
5. URL + password come back to the Proposal Factory and get inserted into the proposal cover note.

### Walkthrough video → higgsfield + Elevenlabs

1. Generate B-roll / motion via higgsfield Seedance.
2. Record screen capture of slides + POC if applicable.
3. Generate voiceover via Elevenlabs:
   - Voice ID matched to brand (Slalom default = warm-confident; Composable DXP = literary-confident, no marketing-speak)
   - Pace ~140–160 wpm for Slalom default; ~130–150 wpm for TFIC
4. Edit + assemble (editing tool out of scope — Descript / ScreenFlow / Premiere).
5. Export MP4 (1080p min, 4K if projecting).
6. Host: Vimeo unlisted password-protected, or embed in microsite if one exists.

### Proposal microsite → design + frontend skills → Vercel deploy

1. Author using Claude Design (default) for fast HTML/CSS/JS, or Claude Code for elaborate Next.js.
2. Use design system source:
   - Slalom default: `70_Templates/Slalom_Brand_Assets/slalom-design-system/`
   - Composable DXP: `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/`
3. Deploy via Vercel MCP `deploy_to_vercel` with Deployment Protection (password).
4. URL + password into the cover note.
5. Verify mobile + tablet + desktop layouts before sharing.

## Stage 3 Polish visual-review checklist

Before moving to format-to-output:

- Same icon style across the proposal (lined OR colored — pick one)
- Same chart style across the proposal (PowerPoint native consistently — no mixed image-chart and native-chart)
- Diagrams generated for the dominant brand (Napkin `corporate` for Slalom default; `corporate` or `clean` for Composable DXP)
- No hand-drawn icons (if the .potx came with embedded illustrated icons, replace them)
- Imagery is real (no placeholder text "stock photo here" surviving into the polished output)

## Output format routing — by variant

| Variant | Output | Tool |
|---|---|---|
| SPRO | 16:9 PPTX (1 slide) + PDF export | PowerPoint MCP |
| Small | 16:9 PPTX (12–18 slides) + PDF export | PowerPoint MCP |
| Large-Slides | 16:9 PPTX (50–150 slides) + PDF export | PowerPoint MCP |
| Large-Document | 8.5×11 PDF (Word source) | Word MCP |

## Companion routing — by companion type

| Companion | Authoring tool | Hosting |
|---|---|---|
| POC | Website Factory plugin (Claude Design / v0 / Figma Make) | Vercel + password |
| Walkthrough video | higgsfield (visuals) + Elevenlabs (voiceover) + editing tool | Vimeo unlisted / microsite embed |
| Microsite | Claude Design (default) / Claude Code (elaborate) | Vercel + password |

## See also

- [`proposal-factory-foundations.md`](proposal-factory-foundations.md)
- [Pipeline.md → Visual toolchain](../../../60_Digital_Manufacturing/Proposal_Factory/Pipeline.md)
- [Companions.md](../../../60_Digital_Manufacturing/Proposal_Factory/Companions.md)
- [Website Factory plugin](../../website-factory/) — POC handoff target
- [Solution Factory toolchain](../../solution-factory/references/solution-factory-toolchain.md) — sibling toolchain reference
