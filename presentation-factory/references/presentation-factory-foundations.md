# Presentation Factory — Foundations

Single source of truth for the plugin. Every skill cites this. Do not re-explain — link here.

## The pipeline

```
Frame → Draft → Compose → Ship
```

| Stage | Output | Pause-for-review |
|---|---|---|
| **Frame** | `00_Intake_Brief.md` in `WIP/[deck-name]/` | Yes — before drafting |
| **Draft** | `10_Content.md` (slide-by-slide content + asset prompts + speaker notes) | Yes — before compose |
| **Compose** | `slides/01-*.html` … `slides/NN-*.html` (one HTML file per slide, plus `deck.html` index) | Yes — before export |
| **Ship** | `deck.pdf` → `deck.pptx` (editable text, movable assets); promoted to chosen destination | — |

Single-operator factory. Pause points are self-review, not multi-reviewer ceremony.

## The two styles

| Style | Use for | Density | Image-to-text ratio |
|---|---|---|---|
| **Informational** | Proposals, training, internal decks, leave-behinds, working sessions | Higher text — designed to read, not present | ~30% text / ~50% structure / ~20% imagery |
| **Presentational** | Conference talks, oral pitches, kickoffs, executive pitches | Lower text — designed to *back* the speaker | ~10% text / ~30% structure / ~60% imagery |

A single deck is one style end-to-end. Mixing styles is the most common failure mode — informational slides in a presentational deck look text-bloated; presentational slides in an informational deck look hollow on the page. Pick once at Frame.

## The three brands

| Brand | When to use | Primary color | Display font |
|---|---|---|---|
| **Slalom** (default) | Almost everything — pursuits, internal, client-facing where Slalom is the named lead | Slalom Blue `#0C62FB` on white surfaces, Slalom Ink `#000A25` on dark | Lora italic for emphasis, Slalom Sans for everything else |
| **Composable DXP** | Practice-led pursuits, microsites, content marketing for the Composable Platforms practice | Lavender `#8A7CE0` primary, Cobalt `#0A25FF` secondary | Lora 700 (non-italic) for display, Inter for body |
| **Client** (custom) | Co-branded or client-led decks (client wants their brand on the deck Slalom built) | Ingested from client styleguide PDF or `.com` crawl | Ingested |

Switching brand changes:
1. Tokens file (loaded into every slide)
2. Logo treatment in the chrome (footer / corner)
3. Eyebrow color
4. Gradient accent palette
5. Body font (Slalom Sans for Slalom, Inter for Composable DXP, ingested font for Client)

Switching brand does **not** change template structure. The 25 canonical templates are brand-agnostic.

## Typography rules (v0.2)

| Element | Family | Weight / style | Treatment |
|---|---|---|---|
| **Title** (slide H1) | **Lora** | **Italic 400** (whole title) | Large. The entire title is Lora italic. `<em>` inside the title bumps weight to italic 600 and recolors to brand-primary (or brand-cyan on dark backgrounds) for phrase emphasis. |
| **Subtitle** | **Avenir Next** | **Bold 700** | Medium, sentence case. Supporting line under the title. |
| **Kicker / Sub-heading** | **Avenir Next** | **Bold 700** | Sentence case, medium-large. Section sub-headings. |
| **Eyebrow** | **Avenir Next** | **Bold 700** | UPPERCASE, letter-spacing 0.16em, small (~16px on slide), brand-primary color. |
| **Body** | **Slalom Sans** | Regular 400 | Generous line-height (1.55). Inter for Composable DXP brand. |
| **Pull quote / callout** | **Lora** | Italic 500 | Larger than body. No quotation marks if visually distinct. |
| **Stat / callout number** | **Lora** | **Italic 600-700 (bold)** | Huge (160-220px on slide). Always bold-italic — never light weight. |
| **Stat unit suffix** (k, M, %) | **Lora** | Italic 600 | 0.45em of the number. Brand-primary color. |
| **Stat label** | **Slalom Sans** | Regular 400 | Small, below stat. |
| **Footer chrome / page numbers** | **Avenir Next** | Regular 400 | Tabular-nums for numerals. |

### The signature Slalom move (v0.2)

Whole titles are Lora italic. Within them, a `<em>` wrapper around the key emphasis phrase bumps the weight to semibold and recolors to brand primary (or brand cyan on dark backgrounds). Examples:

- "*Build* better tomorrows for everyone" — whole headline Lora italic; `<em>better tomorrows</em>` in cyan-semibold on the dark title hero
- "*AI that drives* real impact" — whole headline Lora italic; `<em>real impact</em>` in blue-semibold on white
- "*Three principles that shape* how we work" — whole headline Lora italic; `<em>how we work</em>` in blue-semibold

This is the brand's visual signature. Don't break the pattern unless the brand is set to Client and the client doesn't use serif-italic display type.

### Why these fonts

- **Lora italic** for display gives the brand warmth without sacrificing authority. It's the calligraphic counterweight to the "fiercely human" voice. Italic at display sizes reads as confident, not decorative.
- **Avenir Next bold** for sub-heads gives crisp, modern definition — readable at small sizes (eyebrows), feels architectural at medium sizes (subtitles). It's available on every Mac and well-supported on Windows; falls back to Helvetica Neue / Inter cleanly.
- **Slalom Sans regular** for body keeps the body copy distinctively Slalom. Body text is where the brand's identity sits at length — Slalom Sans there, Avenir Next at structure, Lora at headline.

## The gradient design element (v0.2)

The legacy 5-bar of Slalom branded colors stacked on the right edge is **retired**. In its place: a **single smooth multi-color gradient** that sweeps through the brand accent palette.

**Slalom default gradient stops**:
```
#DEFF4D → #C7B9FF → #FF4D5F → #1BE1F2 → #0C62FB
(lime  →  lavender →   coral   →  cyan  →  blue)
```

### Where it appears (v0.2: strip-only by default)

The multi-color gradient is **ONLY** used as:
1. **Right-edge accent strip** — 28px wide (50% of v0.1's 56px). This is the primary use.
2. **Top or bottom band** — 56px tall, used sparingly as a divider treatment between deck sections.

### Where it does NOT appear

**Never** use the multi-color gradient as a hero / full-bleed background. For hero backgrounds (title slide, section divider, thank-you, image-led), use one of:

- **Flat color**: solid Slalom navy `#0F1C41` or Slalom ink `#000A25` for dark heroes; solid white for light heroes.
- **Same-family subtle gradient**: navy → ink linear, or ink → navy radial. Pre-defined as `--gradient-hero-navy`, `--gradient-hero-ink`, `--gradient-hero-blue`. All single-color-family — no rainbow.
- **Full-canvas background image**: a photograph or abstract motif as the dominant visual, with an overlay that darkens the corner where text sits — defined as `--image-overlay-bottom` / `--image-overlay-left`. The overlay color is a darker hue of the image (or brand ink) fading to transparent.

The multi-color gradient is a brand-accent element. The hero is supposed to feel grounded; rainbow backgrounds feel busy and consumer-y, not the Slalom register.

**Composable DXP gradient**:
```
#8A7CE0 → #5A4FC4 → #0A25FF → #061AC8
(lavender → lavender-deep → cobalt → cobalt-deep)
```

**Client gradient**: derived from ingested brand at Frame stage. If the client has a multi-color system, sweep through it. If single-color, use a single-hue gradient (light to dark of the brand primary).

## Imagery, video, illustrations

**Source**: Higgsfield MCP (nano banana 2 / chatgpt image 2 for stills; seedance 2 for video).

**Direction language** in the markdown content file uses this shape:
```markdown
> **Image** (1920×1080, full-bleed): Wide shot of a diverse team in a modern open office,
> mid-conversation around a table with laptops. Natural daylight, warm-cool color contrast.
> Documentary photography style, slight motion blur on background. No screens visible.
> Mood: collaborative, focused, optimistic. Avoid stock-photo clichés (handshakes, suited men pointing).
```

Asset prompts MUST include:
- Dimensions
- Composition (wide / medium / portrait)
- Mood and color direction
- Style (documentary / editorial / abstract / illustrative)
- What to avoid (no stock-photo clichés)

## Infographics, frameworks, process diagrams

**Source**: Napkin.ai. Generate visuals from the diagram description in the markdown content file. Use for: process flows, frameworks (3-stage, 4-quadrant), org diagrams, value chains.

**Description language**:
```markdown
> **Infographic** (Napkin, square): 4-quadrant matrix showing "Effort vs Impact"
> with axes labeled, 4 named items placed in quadrants, brand-aligned color
> per quadrant. Use as decision framework slide visual.
```

## Icons

**Source**: Flaticon (via web — find the right pack and license).

**Style rule**: pick ONE icon family per deck and hold the line. Common picks:
- Lucide / Phosphor (geometric, single-stroke, 1.5-2px) — for informational decks
- Flaticon "Linear" or "Filled" sets — for presentational decks
- Slalom's own brand icons are explicitly **banned** by user mandate — they're inconsistent

## Data viz

Inline SVG generated by vanilla JS, or pre-built chart libraries (Chart.js / Recharts equivalents inlined as SVG). Brand-aligned palette derived from tokens.

## The 25 canonical templates

See `references/presentation-factory-templates.md` for the full catalog and where each template lives.

| Family | Count | Templates |
|---|---|---|
| **Openers & closers** | 5 | title, section-divider, agenda, thank-you, contact |
| **Content layouts** | 9 | one-col-text, two-col-text-image, three-col, image-led, pull-quote, big-stat, eyebrow-headline-body, list-with-icons, two-col-text-text |
| **Data & diagrams** | 6 | chart, table, framework-2x2, process-3-stage, comparison, timeline |
| **Specialty** | 5 | team-bios, case-study, pricing, roadmap, before-after |

Each template is:
- Single self-contained HTML file
- 1920×1080 viewport (16:9)
- Inline CSS — no external stylesheets
- Inline assets (base64 images) or external URLs (Higgsfield-generated)
- Vanilla JS only — no frameworks
- Brand tokens injected via CSS custom properties at the top of the `<style>` block
- Slot pattern: every text slot is `data-slot="slot-name"` so the composer can substitute

## The toolchain

| Step | Tool | Notes |
|---|---|---|
| Brand ingestion (client) | WebFetch crawl of `.com` + LLM parse of styleguide PDF | Output: client tokens file in `WIP/[deck]/brand/` |
| Content authoring | `consulting:consulting-management-consultant` + `consulting:consulting-humanize` + `design:design-taste` | Deck POV alignment, jargon removal, taste check |
| Image generation | Higgsfield MCP (`generate_image`, `generate_video`) | nano banana 2 = stills; seedance 2 = video |
| Infographic generation | Napkin.ai (manual or browser MCP) | Always store generated PNG/SVG in `WIP/[deck]/assets/infographics/` |
| Icons | Flaticon (web) | License per use. Cache to `WIP/[deck]/assets/icons/` |
| HTML composition | `pf-slide-composer` skill | Reads templates from `resources/templates/` |
| HTML → PDF | Headless Chromium (via Bash) | Print to PDF with `--no-margin --paper-size=16:9` |
| PDF → editable PPTX | `pf-pdf-to-pptx` skill | Uses PowerPoint MCP; reconstructs editable text frames |

## Anti-patterns to actively reject

1. **Mixing styles** within one deck — pick informational or presentational at Frame
2. **The 5-color bar on the right** — retired. Use the gradient instead
3. **Inter as the body font** when in Slalom default brand — use Slalom Sans
4. **Slalom's own illustration-style icons** — banned by user mandate
5. **Stock photography clichés** — handshakes, suited men pointing, fake diversity tableaux
6. **Three-column-card layouts as the default** — fine for content layouts where it serves, but the AI default that emerges is "feature card grid for everything." Refuse.
7. **All-caps for body text** — eyebrows only
8. **Slide titles without italic Lora emphasis** when in Slalom default brand — the brand's signature move is the italic-emphasis pattern; not using it is a tell
9. **AI-generated copy** that hasn't been through `consulting:consulting-humanize`
10. **Default consultant filler words** — "elevate," "leverage," "seamless," "unleash," "next-gen," "best-in-class"

## File layout in WIP

```
60_Digital_Manufacturing/Presentation_Factory/WIP/[deck-name]/
  00_Intake_Brief.md          ← Frame output
  10_Content.md               ← Draft output (slide-by-slide content)
  brand/
    tokens.css                ← copied/derived brand tokens
    logo.svg
    (client-specific assets)
  assets/
    images/                   ← Higgsfield-generated
    infographics/             ← Napkin-generated
    icons/                    ← Flaticon-sourced
  slides/
    01-title.html
    02-agenda.html
    …
    NN-thank-you.html
    deck.html                 ← index page that loads all slides for review
  deck.pdf                    ← exported PDF
  deck.pptx                   ← final editable PowerPoint
```

## See also

- `references/presentation-factory-templates.md` — the 25-template catalog
- `references/presentation-factory-toolchain.md` — detailed toolchain steps with example commands
- `references/slalom-brand-patterns.md` — the brand crawl synthesis
- `resources/tokens/` — brand token CSS files (slalom.css, composable-dxp.css, client-template.css)
- `resources/templates/` — the 25 HTML template files
