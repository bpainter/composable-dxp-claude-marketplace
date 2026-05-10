---
type: reference
project: skills-library
scope: plugin
plugin: solution-factory
tags: [type/reference, plugin/solution-factory, scope/foundational, topic/toolchain, topic/polish]
status: active
---

# Solution Factory — Toolchain

The canonical visual / output / deployment toolchain for Stage 3 Polish. Every `[VISUAL: ...]` tag in a markdown deliverable routes to one of the tools below.

## Routing decision tree

```
Need a visual?
│
├── Diagram / infographic / framework overview / process flow
│   └── → Napkin.ai (style: corporate)
│
├── Flowchart / decision tree / sequence diagram
│   └── → Figma + Mermaid (export PNG/SVG)
│
├── Chart (bar / line / pie / comparison) — going into a deck
│   └── → PowerPoint native chart (editable, themed)
│
├── Photographic or illustrative imagery
│   └── → higgsfield MCP (model: nano banana default; chatgpt 2 fallback)
│       (After Slalom Creative Library check first)
│
├── Animation / video
│   └── → higgsfield MCP (Seedance)
│
├── Icons
│   └── → Flaticon.com (lined OR colored — pick one, never mix; no hand-drawn)
│
└── Stock photography
    └── → Slalom Creative Library first, higgsfield fallback
```

## Diagrams and infographics → Napkin.ai

**Use for:** architecture diagrams, framework overviews, process flows where structure is the point, comparison layouts, concept maps, "infographic" feel.

**Style parameter:**
- Slalom default brand → `corporate`
- Composable DXP secondary → `corporate` or `clean` (NEVER `vibrant` or `flat` — clashes with TFIC's literary-confident voice)

**Output:** PNG, ready to insert into PPTX or HTML.

**Tag format in markdown:**
```
[VISUAL: Diagram — Napkin, corporate]
[Description of what the diagram shows]
```

## Flowcharts, decision trees, sequence diagrams → Figma + Mermaid

**Use for:** anything that needs tighter branding control, anything that will be edited later by the design team, decision trees, sequence flows, swim-lane diagrams.

**Workflow:** author as Mermaid → render via Figma's Mermaid plugin → export as PNG/SVG.

**Tag format:**
```
[VISUAL: Diagram — Figma, flowchart]
[Mermaid-style description]
```

## Charts and graphs → PowerPoint native

**Use for:** bar charts, line charts, pie charts, comparison charts inside a deck. Native charts are editable by the end user, scale cleanly, and match the template's color theme.

**Tool:** PowerPoint MCP — `mcp__PowerPoint__By_Anthropic___*`. Use chart insertion at the slide level.

**Don't do:** Python-rendered chart images. They can't be edited and don't theme.

**Tag format:**
```
[VISUAL: Chart — PowerPoint, bar]
Data: [headers + rows]
Source: [citation]
```

## Photographic and illustrative imagery → higgsfield MCP

**Use for:** hero images, mood / lifestyle imagery, illustrative scenes, conceptual photography, AI-generated image content.

**Slalom default model selection:**
- **nano banana** (Google) — default. Best general-purpose photographic and illustrative output.
- **chatgpt image 2** — fallback when nano banana refuses, fails the brand check, or doesn't capture the brief. Different aesthetic — useful for stylized / poster-like outputs.

**MCP tools:**
- `mcp__higgsfield__generate_image` — produce an image
- `mcp__higgsfield__balance` — check credits before a heavy generation session
- `mcp__higgsfield__media_confirm` — confirm rendered output for use
- `mcp__higgsfield__media_upload` — upload existing media to higgsfield workspace

**Stock-first principle:** check the Slalom Creative Library at brand.slalom.com FIRST. Only generate when stock doesn't have what's needed.

**Tag format:**
```
[VISUAL: Image — higgsfield, nano banana]
Prompt: [the image prompt]
Aspect: [16:9 | 4:3 | 1:1 | 9:16]
Mood: [a few descriptors]
Brand alignment: [Slalom default | Composable DXP secondary]
Fallback: [chatgpt image 2 with adjusted prompt if nano banana fails]
```

## Animations and video → higgsfield MCP (Seedance)

**Use for:** animated explainers, looping background animations, micro-animations on a microsite or first-call deck close, short B-roll for marketing handoffs.

**Tool:** `mcp__higgsfield__generate_video` (uses Seedance by default).

**Tag format:**
```
[VISUAL: Video — higgsfield, seedance]
Prompt: [the motion prompt]
Duration: [seconds]
Aspect: [16:9 | 9:16]
Looping: [yes | no]
```

## Icons → Flaticon.com

**Use for:** any iconography — section markers, navigation, callouts, list bullets where icons add value.

**Style constraints (mandatory):**
- **Lined** OR **colored flat** — pick one and use consistently across the kit. Never mix.
- **No hand-drawn icons.** They clash with both Slalom default and Composable DXP secondary brands.
- **No isometric / 3D** unless specifically calling for it (rare).

**For Slalom default:** prefer the practice override at `50_Knowledge/Frameworks/Slalom_Brand/Iconography` (refined Flaticon set) — replaces the embedded illustrated icons in the .potx template.

**Tag format:**
```
[VISUAL: Icon — Flaticon, lined]
Subject: [what the icon represents]
Color: [from token palette — name the token, not the hex]
```

## Output formats — by deliverable

| Deliverable | Output format(s) | Authoring tool |
|---|---|---|
| 01 One-Pager | 16:9 PPTX (1 slide) + PDF export | PowerPoint MCP |
| 02 Sales Enablement Kit | 16:9 PPTX (~30–40 slides) | PowerPoint MCP |
| 03 Discovery Workshop | 16:9 PPTX (4–6 slides) + facilitator's guide as 8.5×11 PDF | PowerPoint MCP + Word MCP |
| 04 First Call Deck | 16:9 PPTX (20–40 slides) + PDF for client share | PowerPoint MCP |
| 05 Case Study | 16:9 PPTX (1–2 slides) + 8.5×11 PDF (longer form) | PowerPoint MCP + Word MCP |
| 06 Email Templates | Markdown source + `.txt` files | Markdown |
| 07 Handoff Packet | Markdown only (internal artifact) | Markdown |
| 08 Marketing Handoff | Markdown source + (eventual) Marketing_Factory inputs | Markdown |

## Tool MCPs

### PowerPoint
- `mcp__PowerPoint__By_Anthropic___create_presentation`
- `mcp__PowerPoint__By_Anthropic___add_slide`
- `mcp__PowerPoint__By_Anthropic___set_slide_title`
- `mcp__PowerPoint__By_Anthropic___add_text_to_slide`
- `mcp__PowerPoint__By_Anthropic___insert_image`
- `mcp__PowerPoint__By_Anthropic___save_presentation`
- `mcp__PowerPoint__By_Anthropic___export_pdf`
- `mcp__PowerPoint__By_Anthropic___open_presentation`

**Base file:** Always start from `70_Templates/Slalom_Brand_Assets/Slalom_PowerPoint_16x9_Aug2025.potx`. For Composable DXP solutions, apply TFIC design tokens / typography overrides per `50_Knowledge/Frameworks/Composable_DXP_Brand/`.

### Word
- `mcp__Word__By_Anthropic___create_document`
- `mcp__Word__By_Anthropic___format_text`
- `mcp__Word__By_Anthropic___insert_text`
- `mcp__Word__By_Anthropic___replace_text`
- `mcp__Word__By_Anthropic___export_pdf`

### Higgsfield
- `mcp__higgsfield__generate_image`
- `mcp__higgsfield__generate_video`
- `mcp__higgsfield__balance`
- `mcp__higgsfield__media_confirm`
- `mcp__higgsfield__media_upload`

### Vercel (microsite, optional)
- `mcp__vercel__deploy_to_vercel`
- `mcp__vercel__check_domain_availability_and_price`
- `mcp__vercel__get_deployment`
- `mcp__vercel__list_projects`

## Optional microsite (Composable DXP solutions only)

Status: **debating — not yet a default deliverable.**

**Use case:** Composable DXP solutions sell *with* a composable architecture. Showing the architecture in action — as a working microsite — is itself the proof.

**Authoring:** static site built from markdown sources + JSX components from `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/ui_kits/microsite/`.

**Hosting options:**
- **Option A — Vercel password-protected static site.** Deploy via `mcp__vercel__deploy_to_vercel`. URL pattern: `[solution-name].slalom-composable-dxp.com` or similar.
- **Option B — Internal SharePoint site.** Less interactive, but lives where partners already work.

**When to make this call:**
- **Complex tier** Composable DXP solutions — usually justified.
- **Simple tier** — usually overkill.
- **Advisory tier** — context-dependent.

Decision happens at end of Polish, not at intake. Default is no microsite; opt-in if the kit warrants it.

## Visual review (before format-to-output)

Before converting markdown to PPTX/PDF/HTML, walk this review:

- [ ] Same icon style across the kit (lined OR colored — picked one)
- [ ] Same chart style across the kit (PowerPoint native consistently — no mixed image-chart and native-chart)
- [ ] Diagrams generated for the dominant brand (Napkin `corporate` for Slalom default; `corporate` or `clean` for Composable DXP)
- [ ] No hand-drawn icons (if the .potx came with embedded illustrated icons, replace them per the Slalom Iconography practice override)
- [ ] Imagery is real (no Lorem-ipsum-style placeholders surviving into polished output)
- [ ] Stock photos sourced from Slalom Creative Library where possible (higgsfield only when stock fails)
- [ ] Aspect ratios correct for output (16:9 for slides, 1:1 / 1.91:1 / 9:16 for social handoff)
