---
name: pf-pdf-to-pptx
description: >
  Converts the deck (HTML slides + PDF) into an editable PowerPoint file
  where text is editable text frames and assets are movable images. Uses the
  PowerPoint MCP to create each slide, re-parses the source HTML to extract
  text + bounding boxes + asset references, and adds them as PPTX elements
  at the right coordinates. Outputs deck.pptx.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/specialty, topic/pptx-export]
status: active
---

## When to invoke

Called from the Ship stage orchestrator after PDF export is complete.

## Required context

Toolchain: [`../../references/presentation-factory-toolchain.md`](../../references/presentation-factory-toolchain.md). The PowerPoint MCP tools are loaded as deferred tools in this session.

## Why this approach

The user's hard requirement: **text must be editable** and **assets must be movable** in PowerPoint. A "PDF-as-image-per-slide" hack fails both. We rebuild each slide as native PowerPoint objects:

- Text → editable text frames at the right coordinates
- Images → movable picture shapes
- Gradient elements → PowerPoint shapes with gradient fills
- Decorative SVG (chart, matrix lines) → fallback to an image of the original SVG, with editable text labels overlaid where they appeared

This is more complex than PDF-as-image, but produces a deck the user can actually edit downstream.

## What this skill does

### Stage 1: Create the presentation

```
mcp__PowerPoint___create_presentation
  filename: WIP/[deck]/deck.pptx
  slide_size: 16:9  (or set width=13.333" height=7.5" — equivalent to 1920×1080 at 144 DPI)
```

### Stage 2: For each composed slide HTML

Process slides in order. For each `slides/NN-template.html`:

#### 2a. Parse the HTML

Extract:
- All `<element data-slot="...">text</element>` items — text content + class (gives layout coordinates)
- All `<img src="...">` items — image source + class
- Gradient/decorative elements (`.gradient-strip`, `.gradient-band`, `.gradient-hero`) — for re-creation as PPTX shapes

The bounding box for each element comes from the template's CSS — since we know the template, we know the layout. Maintain a JSON lookup at `resources/templates/_layouts.json` (see Stage 4 below) that maps template name + slot name → bounding box (left, top, width, height in inches).

#### 2b. Add a slide

```
mcp__PowerPoint___add_slide
  layout_index: 6  (blank layout)
```

#### 2c. Add the gradient background (if present)

For templates that use `.gradient-strip` (right-edge), `.gradient-band-top`, `.gradient-band-bottom`, `.gradient-hero`:

The PowerPoint MCP `add_text_to_slide` may not directly support gradient shapes — fall back to:
1. Render the gradient as a high-resolution PNG once per template (pre-built in `resources/gradient-stages/`)
2. Insert as a background image at the right position

Alternative: insert an image of the slide's gradient layer as a backdrop. Lower-fidelity but works in all PowerPoint installations.

#### 2d. Add text frames

For each text slot:

```
mcp__PowerPoint___add_text_to_slide
  text: [content from the slide HTML]
  left, top, width, height: [from layout map for this template + slot]
  font_size: [from template CSS — convert px to pt: 1px ≈ 0.75pt]
  font_name: 'Slalom Sans' or 'Lora' depending on slot
  bold: true|false
  italic: true|false  (preserve <em> tags as italic runs)
  color: '#000A25' (Slalom Ink) or the brand foreground
  align: left|center|right
```

For titles with `<em>` emphasis, the text frame needs **runs** — one run regular, one run italic Lora, one run regular. The PowerPoint MCP may need to be invoked multiple times or with runs param if supported. Fallback: add the whole title as a single italic-mixed text frame using inline runs in the MCP's `text` parameter format (e.g. `"Build [italic]better tomorrows[/italic] for everyone"`).

#### 2e. Add images

```
mcp__PowerPoint___insert_image
  image_path: [resolved absolute path from the slide HTML's <img src>]
  left, top, width, height: [from layout map]
```

#### 2f. Add speaker notes

Each slide block in `10_Content.md` has a `**Speaker notes**:` block. The PowerPoint MCP doesn't appear to have a `set_speaker_notes` tool directly — workaround:
- Add a "Notes" text frame at the bottom of the slide that's hidden from the slide view (placed off-canvas at y > 7.5"); or
- Post-process the PPTX file with python-pptx to set `slide.notes_slide.notes_text_frame.text`

A simpler fallback: write the notes into a separate `WIP/[deck]/notes.md` file for the user to paste manually if the MCP doesn't support notes.

### Stage 3: Save

```
mcp__PowerPoint___save_presentation
  filename: WIP/[deck]/deck.pptx
```

### Stage 4: The layout map

The composer needs a JSON file that, for each template and slot, gives the pixel position. Author this once at `resources/templates/_layouts.json`:

```json
{
  "01-title": {
    "eyebrow":   { "left": 120,  "top": 140, "width": 1400, "height": 30,  "px_px": true },
    "title":     { "left": 120,  "top": 240, "width": 1500, "height": 400, "font_px": 120 },
    "subtitle":  { "left": 120,  "top": 680, "width": 1400, "height": 200, "font_px": 36 },
    "logo":      { "left": 120,  "top": 980, "width": 200,  "height": 48 },
    "footer-meta": { "left": 1600, "top": 990, "width": 200, "height": 40 }
  },
  "07-eyebrow-headline-body": {
    "eyebrow":   { ... },
    "title":     { ... },
    "body":      { ... },
    ...
  }
  // ... for all 25 templates
}
```

This file is read by the converter to know where each slot's text/image lives. Pixel positions convert to inches at 144 DPI: `inches = px / 144`.

**TODO at first use**: the conversion script generates an initial `_layouts.json` by:
1. Loading each template's HTML in headless Chromium
2. Using `getBoundingClientRect()` on each `data-slot` element
3. Writing the coordinates to JSON
4. Manual review and adjustment for templates where the auto-extracted positions look off

A `scripts/build-layouts.js` script is implied; commit the resulting JSON.

### Stage 5: Validate the PPTX

After save, sanity check:
- Slide count matches input
- Open the PPTX via the MCP `get_slide_content` and confirm a sample slide has the expected text
- Surface the file to the user with a "spot-check this slide" prompt

## Fallback path

If the layout map is incomplete or the MCP-based reconstruction fails for a specific complex template (e.g. the chart slide with SVG):

1. Render the source slide HTML to a high-res PNG (use chromium with `--screenshot` instead of `--print-to-pdf`)
2. Insert the PNG as a full-bleed image on the PowerPoint slide
3. Overlay the most editable text elements (title, eyebrow, key body) as text frames at their known coordinates
4. Mark the slide as "image fallback — full edit not possible" in the render log

This sacrifices "everything editable" for that one slide but keeps the deck shipable.

## Output

| File | Path |
|---|---|
| Editable PowerPoint | `WIP/[deck]/deck.pptx` |
| Layout extraction (cached) | `resources/templates/_layouts.json` |
| Notes file (fallback) | `WIP/[deck]/notes.md` (if notes can't be embedded) |
| Conversion log | `WIP/[deck]/pptx-convert.log` |

## Anti-patterns

- **Don't** insert each slide as a single image with no editable text — that fails the user's requirement
- **Don't** rely on the layout map being 100% accurate on first try — surface mis-aligned slots and let the user adjust the master layout file
- **Don't** try to reconstruct decorative SVG elements as PowerPoint primitives (curves, gradients in non-trivial paths) — use pre-rendered PNGs and trust the master layout
- **Don't** lose speaker notes silently — if the MCP can't embed them, write to `notes.md` and tell the user
- **Don't** edit `resources/templates/_layouts.json` manually slide-by-slide — regenerate via the build script when template positions change

## First-use note

The `_layouts.json` file does not exist yet. The first run of this skill MUST also create it. Strategy:

1. Build it from `getBoundingClientRect()` measurements via a Puppeteer script (`scripts/build-layouts.js`)
2. Once authored, check it in alongside the templates so subsequent runs use the cached file
3. When a template is updated, re-run the build script
