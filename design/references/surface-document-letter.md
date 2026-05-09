---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/document, topic/surface]
status: active
---

# Surface: 8.5×11 Document

The format-spec reference for 8.5×11 long-form documents (US letter). Loaded by [[design-document]]. Pairs with `docx` and `pdf` skills for execution.

For European audiences, A4 (8.27×11.69) substitutes — note the proportional differences when designing for both markets.

## Format basics

| Attribute | Value |
|---|---|
| Page size | 8.5 × 11 inches (US letter) |
| A4 alternative | 8.27 × 11.69 inches |
| Pixel dimensions @ 150 DPI | 1275 × 1650 |
| Pixel dimensions @ 300 DPI | 2550 × 3300 |
| Color mode | sRGB for screen-only PDF; CMYK for print |
| Bleed | 0.125" if printing with bleed; 0 for screen-only |

## Margin systems

### Symmetric (default for screen-only)
- 1" all four sides
- Usable area: 6.5" × 9"
- Use: most consulting deliverables, screen-only PDFs.

### Asymmetric (editorial / premium)
- Top: 1"
- Bottom: 1"
- Outside: 1.25"
- Inside: 0.75" (for binding gutter)
- Use: print-output, editorial register, heavy use of pull quotes in margin.

### Editorial-narrow (single-column long-form)
- Top: 1.25"
- Bottom: 1.25"
- Outside: 1.5"
- Inside: 1.0"
- Use: long-form prose where line length needs to be tight (60–75 characters).

## Type scale (print-grade)

For 8.5×11 documents, body type sits at 10–12pt. 14pt body is too large for the format.

| Role | Size | Weight | Line-height | Tracking |
|---|---|---|---|---|
| Display (cover hero) | 48–72pt | Bold/Black | 1.0 | -2% |
| H1 (chapter / section) | 24–36pt | Semibold | 1.1 | -1% |
| H2 (subsection) | 18–22pt | Medium | 1.2 | 0 |
| H3 (sub-subsection) | 14–16pt | Semibold | 1.3 | 0 |
| Body | 10–12pt | Regular | 1.45 | 0 |
| Drop cap | 36–48pt | Display weight | spans 3 lines | -2% |
| Caption / footnote | 8–9pt | Regular | 1.4 | +1% |
| Pull quote | 18–24pt | Italic or Medium | 1.3 | 0 |
| Callout body | 11pt | Medium | 1.4 | 0 |
| Mono / code | 10pt | Mono Regular | 1.4 | 0 |

**Print vs. screen:**
- Print output: 10–11pt body works.
- Screen-only PDF: 11–12pt body recommended (no zoom-in luxury at presentation distance).

## Line-length discipline

For body prose:
- **Optimal:** 60–75 characters per line.
- **Max:** 90 characters (gets unreadable).
- **Min:** 45 characters (gets choppy).

A 6.5" wide column at 11pt body = ~78 characters. Right in the sweet spot. A full 8.5" wide page (no margin) at 11pt = ~120 characters. Unreadable. Always set a max-width.

## Color tokens

For editorial-tech / premium-restrained registers (Slalom defaults):

```
--ink:              #0A0A0A   (body text)
--ink-warm:         #1A1714   (warmer alternative)
--ink-muted:        #4A4A4A   (secondary text, captions)
--paper:            #FAFAFA   (screen background)
--paper-print:      #FFFFFF   (true white for print)
--paper-warm:       #F5F1EB   (warm alternative)
--accent:           (one brand color)
--accent-tint-08:   (accent at 8% opacity — callout backgrounds)
--rule:             #E5E5E5   (separator lines, light)
--rule-strong:      #C4C4C4   (separator lines, emphasis)
```

Use color sparingly. A whitepaper with three colors used carefully reads more authoritative than seven.

## Section archetypes (full set)

15 archetypes that cover ~95% of consulting documents.

### Cover
Title, subtitle, author/firm, date, version. Sometimes a hero visual or typographic cover.
- **Banned:** full-bleed Unsplash hands-on-keyboard cover. (See [[ai-tells-forbidden-patterns]].)

### Executive summary
Page 2 typically. **Makes a claim**, doesn't list contents.
- **Length:** 200–400 words.
- **Banned:** "This document covers X, Y, and Z" structure.

### Table of contents
For documents > 12 pages. Skip for shorter ones.
- Page numbers right-aligned. Section numbers left.

### Section open
Each major section. Big chapter title, optional epigraph or one-sentence intro.
- **Type:** H1 at 28–36pt. Optional H2 subtitle at 18pt.
- **Layout:** generous whitespace. The break should feel like a chapter break.

### Body prose
Default content page.
- **Layout:** single column 5–6.5" wide, or 2-column with 0.25" gutter for technical reference.
- **Type:** body at 10–12pt, 1.45 line-height.

### Pull quote
Editorial highlight from surrounding text.
- **Layout:** in column gutter, breaking into the text column. Or full-width between paragraphs.
- **Type:** 18–24pt, italic or weighted.
- **Frequency:** one per spread max.

### Callout / sidebar
Tip, note, warning, key insight. Visually distinct from body.
- **Treatment options:** tinted background box (8% accent tint), accent rule on left edge, icon plus heading plus body.
- **Length:** 30–60 words.
- **Frequency:** 0–2 per spread.

### Figure / chart
Data viz, diagram, or image with caption.
- Required: figure number, title, caption, source.
- **Format:** "Figure 3.2 — Composable DXP adoption curve. Source: Slalom client research, 2026."

### Process / sequence diagram
Boxes-and-arrows or sequenced visual.
- **Caption** every diagram (don't make readers do work).

### Comparison table
Multi-column side-by-side.
- **Type:** body 10pt; column headers 11pt semibold.
- **Rule weight:** thin horizontal rules between rows; no vertical rules.

### Case-in-point box
Specific example illustrating the surrounding argument.
- **Layout:** distinct from body — accent rule or tinted background.
- **Length:** 100–200 words.

### About / authors
Bio + photo (one per page max).
- **Banned:** headshot grid. (See [[ai-tells-forbidden-patterns]].)
- **Format:** photo (160px square), name, title, 2–3 sentence bio.

### Closing / call-to-action
Specific next step. Not "Thank you for reading."

### Citations / endnotes
Numbered or alphabetic depending on style.
- **Hyperlinks:** full URL in citation, hyperlink the title text in body.

### Back cover
Logo, tagline, contact info, document version.
- **Don't** put closing argument here — too easy to miss.

## Header / footer system

Define once, hold across all pages.

### Running header (suppressed on cover and section opens)
- Left: document title (short form).
- Right: section name (current).
- **OR** just one of these.

### Running footer (suppressed on cover)
- Left or center: document title + version.
- Right: page number.
- **OR** just page number.

### Page numbers
- `i, ii, iii, iv` for front matter (cover, exec summary, TOC).
- `1, 2, 3, ...` from main content.
- Right page numbers on right pages, left on left, for two-page spreads.

## Citation styles

| Style | Use for |
|---|---|
| **APA** | Academic, social sciences |
| **Chicago footnote** | Long-form editorial, history |
| **Numbered endnotes** | **Consulting whitepaper default** |
| **Inline parenthetical** | Shorter pieces, journalism style |
| **Bibliography only** | Books, technical references |

Pick one. Hold it across the document. Mixed styles read as careless.

## Length conventions (Slalom Composable DXP)

| Document type | Length |
|---|---|
| Whitepaper | 8–15 pages |
| POV | 3–5 pages |
| Solution brief | 4–8 pages |
| Playbook | 12–30 pages |
| Case study | 1–4 pages |
| Executive summary (standalone) | 1–2 pages |

## Pre-delivery checklist (document-specific)

Run [[pre-delivery-checklist]] plus these document-specific gates:

- [ ] **Margins consistent** across all pages.
- [ ] **Running header / footer** disciplined — page numbers, doc title, version, date present and correct on all body pages.
- [ ] **Cover earns the document** — no full-bleed Unsplash defaults.
- [ ] **TOC accurate** — page numbers match.
- [ ] **Pull quotes used sparingly** — one per spread, max.
- [ ] **Captions on every figure** — figure number, title, source.
- [ ] **Citation style consistent** — one style throughout.
- [ ] **Hyperlinks tested** — every URL resolves.
- [ ] **Body type ≥ 10pt for print, ≥ 11pt for screen.**
- [ ] **Line length 60–75 characters** for body prose.
- [ ] **Accessible PDF tags** if exporting for web (proper heading structure, alt text, reading order).
- [ ] **Front matter pagination** uses roman numerals, main content uses arabic.

## Cross-references

- **Skill:** [[design-document]] (the workflow that uses this surface).
- **Execution:** `docx` and `pdf` skills.
- **Anti-bland:** [[ai-tells-forbidden-patterns]] section "Surface-specific tells: documents."
- **Stylistic registers:** [[stylistic-vocabulary]] (editorial-tech, premium-restrained, editorial).
- **Imagery:** [[design-imagery]] for cover and figure imagery.
- **Visualization:** [[design-visualization]] for diagrams and charts.
- **Pre-delivery:** [[pre-delivery-checklist]].
