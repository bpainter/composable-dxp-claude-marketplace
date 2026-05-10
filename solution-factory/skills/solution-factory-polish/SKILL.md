---
name: solution-factory-polish
description: >
  Stage 3 orchestrator for the Solution Factory — Polish. Generates visuals using
  the canonical toolchain (Napkin, Figma+Mermaid, PowerPoint native, higgsfield
  with nano banana / chatgpt 2 / Seedance, Flaticon). Formats markdown to output
  (16:9 PPTX via PowerPoint MCP, 8.5×11 PDF via Word MCP, optional Composable DXP
  microsite via Vercel). Applies brand (Slalom default vs. Composable DXP
  secondary). Output: PPTX/PDF/HTML files at the top level of WIP. Also known as:
  visual + format + brand stage, polish runner.

type: skill
project: skills-library
plugin: solution-factory
aliases: [solution-factory-polish]
tags: [type/skill, plugin/solution-factory, scope/orchestrator, topic/polish, stage/polish]
status: active
---

## Goal

Take 8 markdown deliverables and convert them to formatted output (PPTX / PDF / HTML), with visuals generated and brand applied. Files sit at the top level of `WIP/[solution-name]/` alongside the markdown sources — *not* in a `final/` subfolder.

## When to invoke

- Stage 3 of the full `solution-factory-create-flow` pipeline (auto-routed)
- User runs `/solution-polish` directly
- User says "polish the kit" / "convert to PPTX" / "generate the visuals"

## Required context

For the canonical toolchain, see `../../references/solution-factory-toolchain.md` — every visual decision routes through it. For brand assets, the canonical paths are:

- Slalom default: `70_Templates/Slalom_Brand_Assets/Slalom_PowerPoint_16x9_Aug2025.potx` + `Slalom_Brand_Guide_Feb2025.pdf` + `slalom-design-system/`
- Composable DXP secondary: `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/` (CSS tokens, JSX, marks)

## The polish sequence

### Step 1 — Visual generation (extract all `[VISUAL: ...]` tags)

Walk every markdown deliverable in WIP. For each `[VISUAL: ...]` tag, route per the toolchain:

- **`[VISUAL: Diagram — Napkin, ...]`** → call Napkin via web/MCP. Style: `corporate` for Slalom default, `corporate` or `clean` for Composable DXP. Save as PNG to `assets/diagrams/`.
- **`[VISUAL: Diagram — Figma, ...]`** → author Mermaid syntax → render via Figma → export. Save to `assets/diagrams/`.
- **`[VISUAL: Chart — PowerPoint, ...]`** → defer to Step 2. PowerPoint native charts are inserted at slide composition.
- **`[VISUAL: Image — higgsfield, nano banana]`** → call `mcp__higgsfield__balance` to confirm credits, then `mcp__higgsfield__generate_image`. If nano banana fails or output is off-brand, fall back to chatgpt image 2. Save to `assets/images/`.
- **`[VISUAL: Video — higgsfield, seedance]`** → call `mcp__higgsfield__generate_video`. Save to `assets/videos/`.
- **`[VISUAL: Icon — Flaticon, ...]`** → source from Flaticon. Constraint: lined OR colored, no hand-drawn. Same style across the kit. Save to `assets/icons/`.
- **`[VISUAL: Stock — placeholder]`** → check Slalom Creative Library first. Higgsfield only as fallback.

**Pre-check:** before any heavy higgsfield session, call `mcp__higgsfield__balance`. If credits are low, AskUserQuestion: *"Higgsfield credits at X. Proceed, top up first, or skip image generation?"*

**Visual review:** before Step 2, walk the visual review checklist from the toolchain reference. Same icon style? Same chart style? Diagrams matching brand? No hand-drawn icons surviving?

### Step 2 — Format to output

For each deliverable, format to its output type using the right MCP:

| Deliverable | Output | MCP |
|---|---|---|
| 01 One-Pager | 16:9 PPTX (1 slide) + PDF export | PowerPoint MCP, then `export_pdf` |
| 02 Sales Enablement Kit | 16:9 PPTX (~30–40 slides) | PowerPoint MCP |
| 03 Discovery Workshop | 16:9 PPTX (4–6 slides) + 8.5×11 PDF facilitator guide | PowerPoint MCP + Word MCP |
| 04 First Call Deck | 16:9 PPTX (20–40 slides) + PDF for client share | PowerPoint MCP, then `export_pdf` |
| 05 Case Study | 16:9 PPTX (1–2 slides) + 8.5×11 PDF (long form) | PowerPoint MCP + Word MCP |
| 06 Email Templates | Markdown source + `.txt` export per template | Markdown |
| 07 Handoff Packet | Markdown only | Markdown |
| 08 Marketing Handoff | Markdown only | Markdown |

**PowerPoint workflow:**

1. Open `Slalom_PowerPoint_16x9_Aug2025.potx` as the base via `mcp__PowerPoint__By_Anthropic___open_presentation`.
2. Save as `[deliverable-name].pptx` in the WIP folder via `save_presentation`.
3. For each markdown section that maps to a slide, call `add_slide` + `set_slide_title` + `add_text_to_slide` + `insert_image` (for visuals from `assets/`).
4. For PowerPoint-native charts, use the chart insertion at slide composition time.
5. For First Call Deck: every slide gets speaker notes per the template guidance.
6. Apply theme overrides for Composable DXP if applicable (see Step 3).
7. Final save. Optionally `export_pdf`.

**Word workflow:**

1. Call `mcp__Word__By_Anthropic___create_document` with the deliverable name.
2. Insert content section by section via `insert_text` + `format_text`.
3. Insert images from `assets/` for visuals.
4. Save and optionally `export_pdf`.

### Step 3 — Apply brand

**Slalom default:**

- Logo from `70_Templates/Slalom_Brand_Assets/slalom-design-system/project/assets/`
- Body type: **Avenir Next LT Pro** (already pre-set in the .potx; do *not* use embedded Slalom Sans)
- Color palette per `Slalom_Brand_Guide_Feb2025.pdf` (or the framework distillation at `50_Knowledge/Frameworks/Slalom_Brand/Color_System`)
- Replace embedded illustrated icons with refined Flaticon set per `50_Knowledge/Frameworks/Slalom_Brand/Iconography` practice override
- Delete the "PowerPoint Tips and Tricks" and "Visual Design System" sections from the .potx before saving
- Footer: standard Slalom boilerplate + slide numbers

**Composable DXP secondary:**

- Wordmark + mark from `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/assets/`
- Voice rules: **em-dash banned** in TFIC copy. No marketing-speak verbs. Literary-confident tone.
- Color tokens from `project/colors_and_type.css` (apply to PPTX theme)
- Display type: bold Lora, no italic (do NOT use the parent's italicized Lora signature move)
- For HTML / microsite output: use the JSX components from `project/ui_kits/microsite/`
- Note: TFIC is a sub-brand. Slalom firm voice, boilerplate, and footer remain.

### Step 4 — Optional microsite (Composable DXP only)

If the kit is Composable DXP secondary AND tier is Complex, AskUserQuestion:

```
Question: "This is a Complex Composable DXP solution. Want to generate an encapsulated microsite as an additional output?"
Options:
1. Yes, deploy to Vercel as password-protected static site (Recommended for Complex tier)
2. Yes, but generate for SharePoint deployment (less interactive)
3. No, PPT/PDF outputs are sufficient
```

If yes (Option 1):

1. Author static HTML using the TFIC design system (CSS tokens + JSX components from `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/`)
2. Map markdown content to microsite sections (`/index`, `/pitch`, `/discovery`, `/case-studies`, `/pricing`, `/contact`, `/assets`)
3. Deploy via `mcp__vercel__deploy_to_vercel`. Password protect.
4. Save the deployment URL into the WIP folder.

If yes (Option 2): generate static HTML; package for SharePoint upload. User uploads manually.

If no: skip.

For non-Complex tiers OR Slalom default brand: skip Step 4 entirely.

## Pause-and-review

After Step 2, before Ship, AskUserQuestion:

```
Question: "Polish complete. Review the visual output?"
Options:
1. Yes, walk me through the visual review checklist
2. Yes, open the One-Pager + First Call Deck for direct visual review
3. Skip review and proceed to Ship
```

## Failure modes

- **Higgsfield credits low.** Surface and ask before generation.
- **Napkin output doesn't match brand.** Re-prompt with stronger brand guidance, or fall back to Figma.
- **PowerPoint MCP fails to insert image.** Verify image path; verify aspect ratio; surface error to user.
- **TFIC voice rule violated** (em-dash in TFIC copy, italicized Lora display, marketing-speak verbs). Auto-flag and re-route through `brand:brand-voice-tone` or `consulting:consulting-humanize`.
- **The .potx has embedded illustrated icons that survived into the polished deck.** Replace them via the Iconography practice override. Don't ship a Slalom default deck with the embedded illustration set.
- **Microsite deployment fails on Vercel** (password setup, domain config). Surface error; offer SharePoint fallback.

## Collaborates with

- `design:design-imagery` (advice for image direction before higgsfield prompting)
- `design:design-visualization` (advice for diagram direction before Napkin/Figma)
- `design:design-presentation` (advice for slide composition)
- `design:design-iconography` (advice for icon family selection)
- `brand:brand-voice-tone` (advice for voice adherence)
- `consulting:consulting-humanize` (humanization pass)
- `vercel:vercel-deploy-pipeline` (microsite deployment)
- `software-engineering:software-engineering-shadcn-component` (microsite component composition for Composable DXP)
- `anthropic-skills:pptx` (PPTX authoring)
- `anthropic-skills:docx` (Word authoring)

## Token discipline

- Toolchain decisions live in `../../references/solution-factory-toolchain.md`. Cite, don't repeat.
- Brand application is opinionated. Don't ask the user "which color?" — the brand frameworks have answers.
- Pre-check higgsfield credits ONCE per session, not per generation.

## Example prompts

1. *"Polish the kit for `composable-dxp-migration`."* → run all 4 steps.
2. *"Just generate the visuals — I'll handle the PPTX in Keynote."* → run Step 1 only.
3. *"Build the One-Pager PPTX from the markdown."* → run Step 2 for 01 only.
4. *"This is a Complex DXP solution — generate the microsite too."* → run all 4 steps including Step 4 with Vercel deploy.
5. *"Apply the TFIC brand to the existing PPTX."* → run Step 3 in retrofit mode on already-formatted output.
6. *"Higgsfield is failing — fall back to chatgpt 2 for everything."* → switch model default for the session.
7. *"Replace all the embedded illustrated icons with the Flaticon practice override."* → run icon replacement only.
8. *"Polish done — what's next?"* → AskUserQuestion about Ship readiness.

## See also

- `../../references/solution-factory-foundations.md`
- `../../references/solution-factory-toolchain.md` — the canonical toolchain reference
- `60_Digital_Manufacturing/Solution_Factory/Pipeline.md`
- `../solution-factory-create-flow/SKILL.md`
- `../solution-factory-ship/SKILL.md` — next stage
