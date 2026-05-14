# Presentation Factory — Template Catalog

The 25 canonical slide templates. Each is brand-agnostic — tokens are injected at compose time.

All templates live in `resources/templates/`. All tokens live in `resources/tokens/`.

## Slot contract

Every template uses `data-slot="slot-name"` attributes. The composer substitutes by slot name. Common slots:

| Slot | Where | Used in |
|---|---|---|
| `eyebrow` | small uppercase text above title | most templates |
| `title` | the headline (supports inline `<em>` for italic-Lora emphasis) | most templates |
| `subtitle` | optional support line under title | title, section-divider |
| `body` | the lead paragraph or body copy | content layouts |
| `footer` | bottom-of-slide chrome (date, presenter, audience) | most templates |
| `slide-number` | "01 / 24" style indicator | most non-title templates |
| `image-1`, `image-2`, … | image src attributes | image-led, two-col, case-study |
| `image-1-alt`, etc. | alt text | accompanying every image slot |
| `stat-1-number`, `stat-1-label` | big-number slides | big-stat, case-study |
| `col-1-title`, `col-1-body` | column slots | two-col, three-col |
| `quote-text`, `quote-attribution`, `quote-role` | pull-quote attribution | pull-quote |
| `item-N-icon`, `item-N-title`, `item-N-body` | list with icons | list-with-icons |

## The 27 templates (v0.2)

> **v0.2 typography note**: titles are always Lora italic; subtitles, sub-headings, eyebrows, and footer chrome use Avenir Next bold; body text uses Slalom Sans (or Inter for Composable DXP). Stat numbers are Lora italic semibold/bold. See `references/presentation-factory-foundations.md` for the full type contract.

## The templates

### Openers & closers (5)

| # | Template | Filename | Style | Notes |
|---|---|---|---|---|
| 01 | **Title** | `title.html` | both | Hero opener. Large italic-Lora-emphasis headline. Gradient or full-bleed image. Logo bottom-left. Date / presenter bottom-right. |
| 02 | **Section Divider** | `section-divider.html` | both | Major section open. Eyebrow "SECTION N" + italic-Lora-emphasis headline. Gradient field. |
| 03 | **Agenda** | `agenda.html` | both | Numbered list of section titles. Eyebrow + title + numbered items. |
| 04 | **Thank You** | `thank-you.html` | both | Closing slide. Large "*Thank you*" or "*Let's solve together*" with contact info. |
| 05 | **Contact** | `contact.html` | both | Names, roles, emails, phone — for handoff or follow-up. Grid of 2-4 people. |

### Content layouts (9)

| # | Template | Filename | Style | Notes |
|---|---|---|---|---|
| 06 | **One-Col Text** | `one-col-text.html` | informational | Single column of body. Eyebrow + title + lead + body paragraphs. Generous margins. |
| 07 | **Eyebrow + Headline + Body** | `eyebrow-headline-body.html` | both | Small eyebrow, large italic-Lora headline, single body paragraph. Generous whitespace. |
| 08 | **Two-Col Text + Image** | `two-col-text-image.html` | both | 50/50 split. Text left, image right (or reversed). |
| 09 | **Two-Col Text + Text** | `two-col-text-text.html` | informational | Side-by-side text columns. "Vision / Solution" pattern. |
| 10 | **Three-Col** | `three-col.html` | both | 3 equal columns. Each: small icon + title + body. |
| 11 | **Image-Led** | `image-led.html` | presentational | Full-bleed image. Headline overlaid bottom-left. Subtle gradient bar for legibility. |
| 12 | **Pull Quote** | `pull-quote.html` | both | Italic-Lora quote, large. Attribution + role + company logo. Optional portrait. |
| 13 | **Big Stat** | `big-stat.html` | both | Huge number (300px+). Label below. Optional 2-stat or 3-stat variants via slot. |
| 14 | **List with Icons** | `list-with-icons.html` | informational | 3-6 items. Each: icon + title + 1-line body. Vertical or horizontal layout. |

### Data & diagrams (6)

| # | Template | Filename | Style | Notes |
|---|---|---|---|---|
| 15 | **Chart** | `chart.html` | both | Container for SVG chart. Chart rendered by vanilla JS at slide load. Brand palette. |
| 16 | **Table** | `table.html` | informational | Data table. Brand-styled header row. Zebra stripes optional. |
| 17 | **Framework 2×2** | `framework-2x2.html` | both | 2x2 matrix. Axis labels + 4 quadrants. Each quadrant: title + 1-2 items. |
| 18 | **Process 3-Stage** | `process-3-stage.html` | both | 3-stage horizontal flow. Stage badge + title + body for each. Connector arrows. |
| 19 | **Comparison** | `comparison.html` | both | Side-by-side comparison. 2-col table-style or visual A-vs-B layout. |
| 20 | **Timeline** | `timeline.html` | both | Horizontal timeline. Date markers + milestone titles + brief body. |

### Specialty (5)

| # | Template | Filename | Style | Notes |
|---|---|---|---|---|
| 21 | **Team Bios** | `team-bios.html` | both | 3-5 team members. Each: portrait + name + role + 1-2 sentence quote. |
| 22 | **Case Study** | `case-study.html` | informational | Single case study. Client logo + headline + 3-stat strip + body + image. |
| 23 | **Pricing** | `pricing.html` | informational | Pricing table or tiered cards. Brand-aligned. Not the SaaS-cliché 3-tier. |
| 24 | **Roadmap** | `roadmap.html` | both | Phased roadmap. Quarters or stages with workstream rows. Brand swatches by phase. |
| 25 | **Before-After** | `before-after.html` | both | Side-by-side. Before state + intervention + after state. |

### Extended (added in v0.2 from agentic-commerce reference deck) (2)

| # | Template | Filename | Style | Notes |
|---|---|---|---|---|
| 26 | **Readiness Ladder** | `26-readiness-ladder.html` | both | 4-tier maturity model. Each tier: colored top-rail + tier label + headline + body + outcome/risk value line. Use for capability ladders, maturity assessments, tiered readiness. |
| 27 | **Journey Map** | `27-journey-map.html` | both | Multi-surface horizontal flow with secondary outcome/risk node grid below. Use for customer journey, agent journey, multi-stage workflows where each stage has both an action and a downstream consequence. |

## Style alignment notes

- **Informational style** decks lean on templates 06, 09, 10, 14, 16, 18, 22, 23 — denser layouts that read on the page.
- **Presentational style** decks lean on templates 01, 02, 11, 12, 13, 17 — image-led, big-number, sparse-text layouts that back a speaker.
- Both styles use the openers/closers and most data templates.

The composer should warn (not block) if a user is trying to use a clearly-mismatched template for the chosen style (e.g. `table.html` in a presentational deck — usually wrong but not always).

## Brand alignment

Each template is brand-agnostic in structure. The brand expression is via:

1. **Tokens injected at compose time** (colors, fonts, spacing)
2. **Logo asset path** (in footer area; differs per brand)
3. **Gradient palette** (the right-edge strip or background gradient pulls from brand-defined stops)
4. **Italic-Lora emphasis pattern** (used by default for Slalom; toggleable per brand — Composable DXP uses Lora 700 non-italic)
