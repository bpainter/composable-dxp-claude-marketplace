---
name: design-document
description: >
  Design 8.5×11 long-form documents — whitepapers, POVs, briefs, playbooks, executive summaries, case studies. Editorial design for printable/PDF deliverables — cover system, section opens, pull quotes, callouts, infographic placement, footer/header system, citation discipline. Owns the document format end-to-end, not paragraph-by-paragraph layout. Use this skill any time the user is making a 8.5×11 document — whitepaper, POV, solution brief, playbook, case study — even when they don't say "design the document." Trigger before opening Word so the document has a system before content goes in. Hands off to docx and pdf skills for execution.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-document, document-design, long-form-design]
tags: [type/skill, plugin/design, topic/design, topic/document]
status: active
---

# Design Document (8.5×11 long-form)

This skill owns the 8.5×11 long-form document format. Deliverables are .docx or .pdf files — whitepapers, POVs, solution briefs, playbooks, executive summaries, case studies. The work isn't paragraph-by-paragraph layout — it's the **editorial system** every page pulls from: master grid, type scale, color palette, section archetypes, callout system, citation style.

A document with a system reads like a publication. A document without one reads like a Word doc someone exported.

Pair with [[design-taste]] (load first), [[design-process]] (Phase-0 calibrate), the [[docx]] skill or [[pdf]] skill (execution), and [[stylistic-vocabulary]] (register selection — usually editorial-tech for thought leadership, premium-restrained for executive briefings).

## When to use this skill

- Whitepaper, POV, or thought-leadership document.
- Solution brief or proposal short-form (4–8 pages).
- Playbook (internal or client-facing).
- Executive summary as standalone document.
- Case study (1–4 pages typically).
- Refactoring an existing document that "reads OK but looks like a Word doc."

## What this skill is NOT

- Not the writing skill. Narrative and argument are upstream — the writer hands you the manuscript, you design the page system around it.
- Not the slide deck skill (see [[design-presentation]] for 16:9).
- Not the social asset skill (see [[design-social-asset]] for short-form web posts).
- Not the execution skill. File generation is `docx` and `pdf`.

## The document system (build first)

### 1. Format and grid
- **Page size:** 8.5×11 inches (US letter). For European audiences, A4 (8.27×11.69) — note the difference.
- **Orientation:** portrait by default. Landscape only for specific data-heavy spreads.
- **Margins:** asymmetric is common in editorial work. Default: 1" top, 1" bottom, 1.25" outside, 0.75" inside (for binding) — or symmetric 1" all around for screen-only PDFs.
- **Column grid:** single column for prose-heavy work; 2-column for technical reference; 3-column for newsletter-style.
- **Baseline grid:** 12-point baseline for body type. Helps text columns align across spreads.

### 2. Type scale (print-grade, not screen-default)

For 8.5×11 documents, body type lives at **10–12pt** (smaller than screen). 14pt body looks juvenile in print.

| Role | Size | Weight | Line-height | Use |
|---|---|---|---|---|
| Display (cover hero) | 48–72pt | Bold/Black | 1.0 | Cover only |
| H1 (chapter / section) | 24–36pt | Semibold | 1.1 | Chapter opens |
| H2 (subsection) | 18–22pt | Medium | 1.2 | Subsections |
| H3 (sub-subsection) | 14–16pt | Semibold | 1.3 | Use sparingly |
| Body | 10–12pt | Regular | 1.45 | Default body |
| Caption / footnote | 8–9pt | Regular | 1.4 | Sources, captions |
| Pull quote | 18–24pt | Italic or weighted | 1.3 | Editorial moments |
| Callout body | 11pt | Medium | 1.4 | Sidebars, notes |

**Print-vs-screen note:** if the document is screen-only (PDF distributed digitally), bump body to 11–12pt. If print-output, 10–11pt is fine.

For typeface bans (no Inter as a "premium" choice on editorial work, no serif on technical-cockpit pages), see [[ai-tells-forbidden-patterns]].

### 3. Color system
For editorial-tech and premium-restrained registers:
- **Ink** (body text): near-black, not pure black. `#0A0A0A` or warmer `#1A1714`.
- **Paper** (background): off-white. `#FAFAFA` for screen, true white for print.
- **Accent** (chapter heads, pull quotes, callout backgrounds): one brand color.
- **Functional** (success, warning, error): rare in long-form documents — usually only in playbooks with status callouts.

Use color sparingly. A whitepaper with three colors used carefully reads more authoritative than one with seven.

### 4. Section archetypes (define your set)

| Archetype | When |
|---|---|
| **Cover** | Page 1. Title, subtitle, author/firm, date, version. Sometimes a hero visual. |
| **Executive summary** | Page 2 (often). Makes a claim. Earns the rest of the document. |
| **Table of contents** | For documents > 12 pages. Skip for shorter ones. |
| **Section open** | Each major section. Big chapter title plus optional epigraph. |
| **Body prose** | Default content page. Single column or 2-column. |
| **Pull quote** | Editorial highlight from the surrounding text. One per spread max. |
| **Callout / sidebar** | Tip, note, warning, key insight. Visually distinct from body. |
| **Figure / chart** | Data viz with caption, source, figure number. |
| **Process diagram** | Boxes-and-arrows or sequence diagram. |
| **Comparison table** | Multi-column comparison. |
| **Case-in-point box** | A specific example illustrating the surrounding argument. |
| **About / authors** | Bio + photo (one per page max). |
| **Closing / call-to-action** | Specific next step. |
| **Citations / endnotes** | Source references. |
| **Back cover** | Logo, tagline, contact, version. |

For the bans on full-bleed-stock-image covers, "executive summary that summarizes," "our approach" five-pillar pages, headshot grids, and pillar-checkmarks pages, see [[ai-tells-forbidden-patterns]] under "Surface-specific tells: documents."

### 5. Header / footer system
Define once, hold across all pages:

- **Running header:** document title (left or center) + section name (right) — or both. Suppress on cover and section opens.
- **Running footer:** page number + author/firm short name — or just page number. Suppress on cover.
- **Page numbers:** `iv` for front matter, `1` from main content. Right page numbers on right pages, left on left, for two-page spreads.
- **Footnote vs. endnote:** pick one. Footnotes at bottom of page (academic, technical). Endnotes at end of section/document (consulting standard).

### 6. Pull quotes and callouts

Pull quotes lift a sentence from the surrounding text and present it as an editorial moment.
- **Type:** larger than body (18–24pt), often italic or in the accent color.
- **Layout:** in a column gutter, breaking into the text column.
- **Frequency:** one per spread max. More dilutes them.

Callouts are sidebar content — tips, notes, warnings, key insights.
- **Visual treatment:** icon + heading + body in a tinted box, or accent-color rule on the left edge with body in default style.
- **Length:** 30–60 words.
- **Type:** one or two callout-archetypes per document, used consistently.

### 7. Figures and charts

Every figure has:
- **Figure number** (Figure 1, Figure 2.3 if numbered by section).
- **Title** (descriptive, not "Chart").
- **Caption** (1–2 sentences explaining what the figure shows).
- **Source** ("Source: Slalom client research, 2026" or external citation).

For chart selection see [[shadcn-chart-inventory]]. For image generation, see [[design-imagery]].

### 8. Citations
Pick one citation style and hold it:
- **APA** (academic, social sciences).
- **Chicago footnote** (long-form, editorial).
- **Numbered endnotes** (consulting whitepapers — most common for our work).
- **Inline parenthetical** ((Author, 2026)) — for shorter pieces.

Citations on hyperlinks: full URL in the citation, hyperlinked text in the body. Don't bury the URL.

## Document-system file

Before any content layout, produce `document-system.md` in the WIP folder:

```
# Document system: {Project name}

## Audience and intent
- Audience: {specific person, context}
- Intent: {what they should do/decide after reading}
- Stylistic register: {editorial-tech / premium-restrained / etc.}
- Length target: {N pages including cover, TOC, back}

## Format
- Size: 8.5×11 (US letter) | A4 | other
- Output: print-ready / screen-only PDF / docx
- Margins: {top/bottom/inside/outside}
- Columns: single | 2-col | mixed by section

## Type scale
- Display: {font} 60pt Bold (cover only)
- H1: {font} 28pt Semibold
- H2: {font} 18pt Medium
- Body: {font} 11pt Regular, 1.45 line-height
- Caption: {font} 9pt Regular
- Pull quote: {font} 22pt Italic Medium
- Mono / data: {font} 10pt

## Color tokens
- ink: #0A0A0A
- paper: #FAFAFA (screen) / #FFFFFF (print)
- accent: {brand color}
- callout backgrounds: tinted accent at 8% opacity

## Section archetypes in use
- Cover, Exec summary, TOC, Section open, Body prose, Pull quote, Callout, Figure, Citations, Back cover.

## Anti-bland gates
- Cover not a stock-photo full bleed.
- Exec summary makes a claim, doesn't list contents.
- No "our approach" five-pillar page.
- No headshot grid on about page.
- No checkmark grid for capabilities.

## Citation style: {APA | Chicago | numbered endnotes | inline}
- Endnote count: {N} expected.
```

## How to engage

1. **Calibrate** ([[design-taste]]): intent, register, dials. Default editorial-tech for technical thought leadership; premium-restrained for executive briefings.
2. **Read the manuscript end-to-end** if it's already written. Note where pull quotes, callouts, and figures naturally fit.
3. **Define the document system.** Document in `document-system.md`.
4. **Build the document master** in docx or InDesign or pdf-skill scaffold. One template covering all archetypes.
5. **Lay out the content** using the masters.
6. **Run the four checks** ([[design-taste]] — swap, squint, signature, token).
7. **Pre-delivery checklist** ([[pre-delivery-checklist]]): contrast, type sizes, margin consistency, citation style, page numbering, alt text on figures, accessible PDF tags if exporting.

## Length conventions (Slalom Composable DXP defaults)

Per [[preferences]] in the memory layer:

| Document type | Length |
|---|---|
| Whitepaper | 8–15 pages |
| POV | 3–5 pages |
| Solution brief | 4–8 pages |
| Playbook | 12–30 pages |
| Case study | 1–4 pages |
| Executive summary | 1–2 pages standalone |

Going over by 50% is usually a signal the document is two documents. Going under by 50% is usually a signal it's a slide.

## Document-archetype cheat sheets

### Whitepaper structure (default)
1. Cover
2. Executive summary (claim + 3-4 supporting points)
3. Table of contents
4. Introduction / context
5. Argument (3–5 sections)
6. Implications / what to do about it
7. Case examples
8. Conclusion / specific next step
9. About authors
10. Citations / references
11. Back cover

### POV structure (sharper, shorter)
1. Cover
2. The claim (one paragraph, on the cover or page 2)
3. The argument (2–3 sections)
4. What this means for [audience]
5. Closing commitment
6. Citations
7. Back cover

### Solution brief
1. Cover (with one-line value prop)
2. The problem (the audience experiences)
3. The solution (what we do)
4. Why us / proof
5. Engagement model
6. Closing call-to-action
7. Back cover

### Playbook
1. Cover
2. How to use this playbook (1 page)
3. Methodology overview
4. Per-stage chapters (1 chapter per stage)
5. Per-deliverable templates
6. Per-role checklists
7. Glossary
8. Citations / further reading

## Common document failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Word-doc-aesthetic** | Reads like a Calibri Word export | Custom type scale, custom margins, designed cover |
| **Stock-photo cover** | Full-bleed Unsplash hands-on-keyboard with title overlay | Typographic cover or commissioned image |
| **Five-pillar page** | "Our approach" with 5 identical icon-headline blurbs | Methodology by example, or skip the page |
| **Exec summary that summarizes** | Page 2 lists contents | Exec summary makes a claim |
| **Headshot grid** | "About the team" with 8 cropped headshots | Cut, or one headshot at a time with one sentence each |
| **Citations as afterthought** | Inconsistent citation style, broken URLs | Pick one style; verify every URL |
| **No pull quotes / callouts** | Wall of body text for 12 pages | Add 1 pull quote per spread; 1–2 callouts per chapter |
| **Microscopic body type on print** | 9pt body | 10–11pt body for print, 11–12pt for screen |
| **Inconsistent header/footer** | Page numbers in different positions | Define once, hold across all pages |
| **Figures without captions** | Charts inserted without figure number/source | Number, title, caption, source for every figure |

## Hand-offs

- **`docx` skill** — for Word document execution.
- **`pdf` skill** — for PDF generation, accessibility tagging, form fields if applicable.
- **[[design-imagery]]** — when documents need commissioned imagery or higgsfield-generated visuals.
- **[[design-visualization]]** — when documents need diagrams or data viz.
- **[[design-iconography]]** — when callouts need icons.
- **[[design-presentation]]** — when content is too thin for a document and should be a deck instead.

## Surface-specific cross-reference

For full format specs, see [[surface-document-letter]] in `design/references/`.

## Constraints

- Don't open Word before the document system is defined.
- Don't accept Calibri 11pt as the type scale. Custom typeface, custom scale.
- Don't ship a document without endnotes/citations if the document references external sources.
- Don't reuse a Slalom template's defaults — they were tuned for someone else's brief.
- Default register for thought-leadership: **editorial-tech**. For executive briefings: **premium-restrained**.
