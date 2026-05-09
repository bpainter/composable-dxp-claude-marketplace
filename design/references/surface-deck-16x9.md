---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/presentation, topic/surface]
status: active
---

# Surface: 16:9 Deck

The format-spec reference for 16:9 presentation decks. Loaded by [[design-presentation]] as the canonical surface reference. Pairs with the `pptx` skill for execution.

## Format basics

| Attribute | Value |
|---|---|
| Aspect ratio | 16:9 |
| Pixel dimensions | 1920×1080 (PowerPoint default), or 2560×1440 for Retina prep |
| Inch dimensions | 13.333" × 7.5" |
| Safe area margin | 60px on all sides (1800×960 usable) |
| Resolution | 96 DPI for screen, 150 DPI for export-to-print |
| Color mode | RGB (sRGB) for screen-projected; CMYK only if printing |

**Why 16:9 over 4:3:** 16:9 is the modern projector and Zoom default. 4:3 only persists on legacy academic conference setups. Always check before building — but 16:9 is right 95% of the time.

## Master grid options

Pick one based on stylistic register (see [[stylistic-vocabulary]]):

### 12-column grid (default — flexible)
- 1800px usable width / 12 columns = 150px per column
- 32px gutter (between columns)
- Effective column with gutter ≈ 118px
- Use: most decks. Plays well with 1/2, 1/3, 1/4, 1/6, 1/12 splits.

### 6-column grid (editorial / editorial-tech)
- 1800px / 6 = 300px per column
- 48px gutter
- Use: editorial register, deck-as-magazine, long-form pitch.

### 8-column grid (premium-restrained)
- 1800px / 8 = 225px per column
- 40px gutter
- Use: executive-audience proposals, premium positioning.

### Asymmetric anchor (high variance)
- 60/40 or 38/62 (golden ratio) split
- Use: when DESIGN_VARIANCE > 6 and the brief calls for editorial moments.

## Vertical zones

```
┌──────────────────────────────────────────────────┐ <- 60px safe area top
│  Footer-corner (logo, page #) — top OR bottom    │
│                                                   │
│  TITLE ZONE (~270px tall)                         │
│  - H1: 44pt                                       │
│  - Optional H2 subtitle: 28pt                     │
│                                                   │
├──────────────────────────────────────────────────┤
│                                                   │
│  BODY ZONE (~540px tall)                          │
│  - Content varies by archetype                    │
│  - Body type 18–24pt                              │
│                                                   │
├──────────────────────────────────────────────────┤
│  FOOTER ZONE (~135px tall)                        │
│  - Footer-corner alternative                      │
│  - Page #, doc title, version                     │
└──────────────────────────────────────────────────┘ <- 60px safe area bottom
```

Total: 270 + 540 + 135 = 945px (with safe area = 1080px)

## Type scale (projection-readable)

For 16:9 decks projected at presentation distance, body type lives at 18–24pt minimum.

| Role | Size | Weight | Line-height | Tracking |
|---|---|---|---|---|
| Display (cover hero) | 96–120pt | Bold/Black | 0.95 | -2% |
| H1 (slide title) | 40–48pt | Semibold | 1.1 | -1% |
| H2 (subtitle) | 28–32pt | Medium | 1.2 | 0 |
| H3 (in-slide section) | 22–26pt | Medium | 1.3 | 0 |
| Body | 18–24pt | Regular | 1.4 | 0 |
| Caption / source | 14–16pt | Regular | 1.4 | +1% |
| Mono / data | 16–20pt | Mono Regular | 1.3 | 0 |

**Minimum body type:** 18pt for executive audiences (older eyes, projector at distance). 16pt absolute floor — go below and content stops being readable from the back row.

## Color tokens (deck-default starting point)

For consulting decks at executive register, lean **premium-restrained**.

### Slalom-instance decks (default for Bermon's practice)

When the deck is for Slalom (proposal, whitepaper supporting deck, executive brief), use the Slalom palette directly. The official PowerPoint template (`70_Templates/Slalom_Brand_Assets/Slalom_PowerPoint_16x9_Aug2025.potx`) is pre-loaded with these tokens — start there. See [[slalom-brand-tokens]] for the working set.

```
--slalom-blue:        #0C62FB   (THE primary — must appear in every comp)
--slalom-dark-blue:   #002FAF   (deep emphasis, hover-state equivalent)
--slalom-ink:         #000A25   (primary text on light)
--slalom-navy:        #0F1C41   (dark surfaces, full-bleed)
--neutral-50:         #F5F5F5   (light page bg, warmer than white)
--neutral-200:        #E6E6E6   (borders, dividers)
--accent:             ONE of: #1BE1F2 (cyan), #FF4D5F (coral), #C7B9FF (purple), #DEFF4D (chartreuse)
```

Pair Slalom Blue with **one** secondary core color per composition. Never mix all secondary colors except in the official Slalom color bar (5-color order: Blue → Cyan → Coral → Purple → Chartreuse).

Type in PowerPoint is **Avenir Next LT Pro** — already pre-set in the template. Don't substitute Slalom Sans (font compatibility issues on customer systems).

### Generic decks (when not for Slalom)

For non-Slalom client work, use a premium-restrained neutral palette:

```
--ink:          #0A0A0A   (primary text, dark backgrounds)
--ink-muted:    #4A4A4A   (secondary text)
--paper:        #FAFAFA   (primary background, light backgrounds)
--paper-warm:   #F5F1EB   (warm alternative paper)
--accent:       (one brand color at <60% saturation)
--success:      #2D7D32
--warning:      #C77800
--error:        #C03A2B
--info:         #1B5E97
```

Replace the accent and re-tune the functional palette to match the brief. Don't ship PowerPoint default theme colors.

## Slide archetypes (full set)

10 archetypes that cover ~95% of consulting deck slides. Define each as a master slide in pptx; reuse across the deck.

### 1. Cover
Title slide. Earns the next 10 minutes.
- **Layout:** Asymmetric. Title left-aligned, accent visual right. Or full-bleed cover image with title overlay.
- **Type:** Display 96–120pt. Subtitle 28pt. Audience/date in caption.
- **Banned:** centered everything. The agency-template cover with center-stacked logo / title / subtitle / date is the AI-default. Reject.

### 2. Section divider
Chapter break between major sections.
- **Layout:** Big number (01, 02, 03) plus section title. Generous whitespace.
- **Type:** Number at 200pt+ (decorative), section title at 48pt.
- **Use:** between 3+ major sections. Skip for 2-section decks.

### 3. Big claim
The argument of a section. One sentence per slide.
- **Layout:** Single block of large type. Centered or left-aligned per register.
- **Type:** 60–80pt. Generous letting.
- **Use:** open or close a section with the argument it makes.

### 4. Headline + supporting visual
Default content slide. ~70% of slides should be this archetype.
- **Layout:** 50/50 or 60/40 split. Headline + body left, image/diagram right.
- **Or:** headline top, full-width body below.
- **Avoid:** centered headline floating above centered body. (Centered-hero ban above DESIGN_VARIANCE 4.)

### 5. Comparison
Us vs. them, before vs. after, option A vs. option B.
- **Layout:** Two columns of equal weight. Sometimes a Δ row showing differences.
- **Type:** Same scale across columns; let the content do the differentiation.
- **Avoid:** colored "us" column vs. neutral "them" column on a sales-pitch deck. Reads as defensive.

### 6. Process / sequence
3–5 steps, left-to-right (preferred for English audiences) or top-to-bottom.
- **Layout:** Equally weighted boxes connected by arrows. Or numbered horizontal flow without boxes.
- **Avoid:** five-pillar grid masquerading as a sequence. (See ai-tells-forbidden-patterns: the five-pillar ban.)

### 7. Quote / pull quote
A specific quote earned by surrounding context.
- **Layout:** Large quote, attribution caption.
- **Type:** Quote 40–60pt italic or upright depending on register. Attribution 18–22pt.
- **Banned:** stock quotes from Steve Jobs / Marie Curie / generic CEOs. The motivational-quote-graphic ban.

### 8. Data viz
One chart per slide. The chart and its caption are the slide.
- **Layout:** Chart center or left-aligned. Caption below or right.
- **See [[shadcn-chart-inventory]]** for chart type selection.
- **Banned:** pie charts with 5+ slices. Vertical bars with cramped category labels.

### 9. Architecture / system diagram
Boxes-and-arrows, system topology, data flow.
- **Default tool:** Mermaid (rendered to image) or commissioned diagram (Figma → png export).
- **Caption every diagram** — diagrams without captions force the audience to do work.
- **Banned:** the AWS/GCP "exploded clouds" diagram cliché. Custom diagram earns more.

### 10. Closing commitment
Final slide. Specific. Banned: "Thank you." or "Questions?".
- **Layout:** Single block. Commitment, question we want answered, or number to remember.
- **Use:** "We'll deliver the architecture diagram by Tuesday EOD" or "What would have to be true for you to short-list us?" or "$3.4M, 6 months, two SKUs."

## The deck-system file

Always create `deck-system.md` in the WIP run folder before any slide content. Template in [[design-presentation]].

## Slalom-instance shortcut

If the deck is Slalom-branded, you don't need to reinvent layouts. Use the official template's 59 layouts and 16 sections — the layout-to-archetype map is in [[../../../50_Knowledge/Frameworks/Slalom_Brand/PowerPoint_Template_Inventory|PowerPoint Template Inventory]]. Replace the embedded illustrated icons with refined Flaticon icons per the [[../../../50_Knowledge/Frameworks/Slalom_Brand/Iconography#our-practice-override-refined-alternative|practice override]].

## Density calibration (per [[design-taste]] VISUAL_DENSITY dial)

| Density | Body words/slide | Body size | Whitespace |
|---|---|---|---|
| 1–3 (premium) | ~30 | 24pt | Generous (40%+ slide is empty) |
| 4–7 (standard) | ~50 | 18–22pt | Balanced (~25% slide whitespace) |
| 8–10 (technical) | ~100 | 18pt + mono numerals | Dense; tabular alignment |

## Pre-delivery checklist (deck-specific)

Before a deck ships, run [[pre-delivery-checklist]] plus these deck-specific gates:

- [ ] **Cover earns the next 10 minutes.** Not a template default.
- [ ] **Closing slide says something specific.** Not "Thank you."
- [ ] **No agenda → context → solution → benefits → next steps template structure.**
- [ ] **No five-pillar methodology slide.**
- [ ] **No headshot grid.**
- [ ] **Body type ≥ 18pt across the deck.**
- [ ] **One archetype per kind of slide (no per-slide creative).**
- [ ] **Speaker notes on every non-self-explanatory slide.**
- [ ] **Outline view passes** — slide titles alone tell the story.
- [ ] **Mobile/thumbnail preview** — slides legible at 25% zoom (Zoom screen-share quality).
- [ ] **Brand consistency** — logo placement, color usage, type-pair correct on every slide.

## Cross-references

- **Skill:** [[design-presentation]] (the workflow that uses this surface).
- **Execution:** `pptx` skill (file generation).
- **Anti-bland:** [[ai-tells-forbidden-patterns]] section "Surface-specific tells: presentations."
- **Stylistic registers:** [[stylistic-vocabulary]] (premium-restrained, swiss-modern, editorial-tech for consulting work).
- **Charts:** [[shadcn-chart-inventory]] for chart type guidance.
- **Pre-delivery:** [[pre-delivery-checklist]].
