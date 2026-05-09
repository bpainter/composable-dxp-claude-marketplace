---
name: design-presentation
description: >
  Design 16:9 presentation decks (PowerPoint, Keynote, Gamma, Google Slides) with deck-system thinking, hierarchy, density discipline, and brand application across slides. Owns the presentation format end-to-end — not slide-by-slide layout, but the master grid, type scale, color system, and slide archetypes that make a deck feel intentional rather than templated. Use this skill any time the user is making a 16:9 deck — proposal, pitch, all-hands, board update, conference talk, training — even when they don't say "design the deck." Trigger before opening PowerPoint or Keynote so the deck has a system before content goes on slides. Hands off to the pptx skill for execution.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-presentation, deck-design]
tags: [type/skill, plugin/design, topic/design, topic/presentation]
status: active
---

# Design Presentation (16:9)

This skill owns the 16:9 deck format. The deliverables are PowerPoint or Keynote files, but the work isn't slide-by-slide layout — it's the **master system** that every slide pulls from: grid, type scale, color palette, slide archetypes, density rules, transitions. A deck without a system feels like 47 slides built by 47 different people. With a system, even a quick build feels intentional.

Pair with [[design-taste]] (load first — sets dials, picks register, names defaults to reject), [[design-process]] (the Phase-0 calibrate then Phase-1 define), and the [[pptx]] skill (the execution layer once the system is set).

## When to use this skill

- Starting any 16:9 deck — proposal, pitch, board update, conference talk, training, all-hands.
- Refactoring an existing deck that "feels off" but you can't say why.
- Picking a deck template starting point and customizing it to the brief.
- Reviewing a deck for system drift across slides.
- Building a deck-master template the team will reuse across pursuits.

If the task is a one-off slide insertion into an existing deck where the system is already set, skip this skill — go straight to `pptx` for execution.

## What this skill is NOT

- Not the layout-each-slide skill. That's slide-by-slide composition inside the system.
- Not the writing skill. UX copy and narrative belong to the proposal-brief, story, or whitepaper skills upstream.
- Not the execution skill. File generation is `pptx`'s job.

## The deck system (build this first)

Before any slide gets content, define and document:

### 1. Master grid
- **Slide dimensions:** 16:9 at 1920×1080 (PowerPoint default) or 13.33×7.5 inches.
- **Safe area:** 60px margin on all sides (slides projected at angles or cropped on Zoom).
- **Column grid:** 12-column for flexibility, or 6-column for editorial-tech register, or 8-column for premium-restrained.
- **Gutter:** 32–48px between columns.
- **Vertical baseline grid:** 8pt (16:9 plays well with 8 multiples — 1080/8 = 135).
- **Title zone, body zone, footer zone:** define heights once. Title zone usually top 1/4, body zone middle 1/2, footer bottom 1/8.

### 2. Type scale
For 16:9 at projection distance, body type lives at 18–24pt minimum. Smaller is unreadable from row five.

| Role | Size | Weight | Use |
|---|---|---|---|
| Display (cover) | 80–120pt | Bold/Black | Title slide moment |
| H1 (slide title) | 40–48pt | Semibold | Most slide titles |
| H2 (slide subtitle) | 28–32pt | Medium | When the title needs context |
| H3 (section) | 22–26pt | Medium | In-slide section breaks |
| Body | 18–24pt | Regular | Default body type |
| Caption | 14–16pt | Regular/Medium | Footnotes, sources, attribution |
| Mono / data | 16–20pt | Mono Regular | Numbers, code, data labels |

For the typeface ban list (no Inter on premium briefs, no serif on technical decks), see [[ai-tells-forbidden-patterns]].

### 3. Color system
Apply the brand's color tokens — primary, secondary, neutrals, functional. For consulting decks at the executive register, lean **premium-restrained** (see [[stylistic-vocabulary]]):
- One ink (deep neutral)
- One paper (off-white or near-white)
- One brand accent (used sparingly)
- One functional palette (success, warning, error, info — calibrated, not Bootstrap defaults)

The deck should announce the brief, not announce PowerPoint's default theme.

### 4. Slide archetypes (define your set; use them)
Most decks need 6–10 archetypes. Define each once, then use them throughout. Don't invent a new layout per slide.

| Archetype | When |
|---|---|
| **Cover** | Open. Title, subtitle, audience, date. Earns attention. |
| **Section divider** | Between major sections. Acts as a chapter break. |
| **Big claim** | One sentence. The core argument of a section. |
| **Headline + supporting visual** | Default content slide. Asymmetric (split-screen or 60/40). |
| **Comparison** | Two columns. Us vs. them, before vs. after. |
| **Process / sequence** | 3–5 steps left-to-right or top-to-bottom. |
| **Quote / pull quote** | A specific quote earned by the surrounding context. |
| **Data viz** | One chart per slide. See [[shadcn-chart-inventory]] for chart selection. |
| **Architecture / system diagram** | Boxes-and-arrows. Use Mermaid or commissioned diagram. |
| **Q&A / discussion** | A specific question, not "Questions?". |
| **Closing commitment** | A specific commitment, number, or next step. Not "Thank you." |

For the bans on cover slides, closing slides, headshot grids, and five-pillar slides, see [[ai-tells-forbidden-patterns]] under "Surface-specific tells: presentations."

### 5. Density discipline
Per [[design-taste]]'s VISUAL_DENSITY dial:
- **Density 1–3 (premium):** one claim per slide. Body 24pt. Generous whitespace. ~30 words max.
- **Density 4–7 (standard):** default consulting register. Body 18–22pt. ~50 words max.
- **Density 8–10 (technical/data):** dense data displays. Mono numerals. Tabular alignment. Up to 100 words.

**The slide-as-document failure mode:** when a slide has more than 50 words of body text, it's no longer a slide. Either split it across two slides, or cut it from the deck and put it in a paired document (see [[design-document]]).

### 6. Transitions
Default to **none** for consulting decks (slide cuts feel professional and don't distract). If transitions are used:
- One transition style across the whole deck (no mixing fade with push with cube).
- Duration ≤ 300ms.
- No "exciting" defaults (cube, dissolve, dissolve-particle, peel-off). Cuts and crossfades only.
- See [[motion-and-transitions]] for the full motion ban list.

### 7. Speaker notes
Every slide that isn't self-explanatory has speaker notes. Speaker notes are the script — they're also the document the deck pairs with for asynchronous viewers.

## Slide-system file (always create this)

Before any slide content, produce a `deck-system.md` in the WIP run folder:

```
# Deck system: {Project name}

## Audience and intent
- Audience: {specific person, context}
- Intent: {what they should do/decide after}
- Stylistic register: {from stylistic-vocabulary}
- Dials: variance {N} / motion {N} / density {N}

## Master grid
- Dimensions: 1920×1080 (16:9)
- Safe area: 60px margin
- Columns: 12-col, 32px gutter
- Baseline: 8pt grid

## Type scale
- Display: {font} 96pt Bold
- H1: {font} 44pt Semibold
- Body: {font} 20pt Regular
- Caption: {font} 14pt Regular
- Mono: {font} 18pt Regular

## Color tokens
- ink: #0A0A0A
- paper: #FAFAFA
- accent: #4F2C8E (Slalom indigo at 60% saturation)
- success/warn/error/info: ...

## Slide archetypes in use
- Cover, Section, Big claim, Headline+visual, Comparison, Process, Quote, Data viz, Closing commitment.
- Each defined as a master slide in pptx.

## Anti-bland gates
- No agenda slide → opens with claim.
- No closing "Questions?" slide → specific question or commitment.
- No 3-column feature row → 2-column zigzag or asymmetric.
- No headshot grid.
- No five-pillar methodology page.

## Speaker note discipline
- Every non-self-explanatory slide has notes.
- Notes are the asynchronous-viewer script.
```

## How to engage

When asked to design a deck:

1. **Calibrate** (Phase 0). Read [[design-taste]], answer the three intent questions, set the dials, pick a register from [[stylistic-vocabulary]].
2. **Define the deck system.** Build the seven elements above. Document in `deck-system.md`.
3. **Build the master slides** in pptx (one per archetype). Hand off to `pptx` skill for the actual file generation.
4. **Compose the slides** using the masters. Don't invent new layouts mid-deck.
5. **Run the four checks** ([[design-taste]] — swap, squint, signature, token). Flag anything that defaulted.
6. **Pre-delivery checklist** ([[pre-delivery-checklist]]) — accessibility, contrast, type sizes, mobile preview (yes — check at thumbnail size).
7. **Speaker notes pass.** Every slide that isn't self-explanatory.

When asked to fix an existing deck:

1. **Audit for system absence.** Most "this deck feels off" complaints are systemic — no master grid, drifted type scale, mixed icon sets, inconsistent slide archetypes.
2. **Rebuild the system** before fixing slides individually. A coherent system applied across slides fixes 80% of "feels off" faster than per-slide tweaks.
3. **Re-template the slides** to the new masters.

## Common deck failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Template-aesthetic** | Looks like every consulting deck | Pick a register from [[stylistic-vocabulary]] and commit |
| **Slide-as-document** | Body text > 50 words | Split or move to paired document |
| **Drift across slides** | Slide 7 looks nothing like slide 3 | Define archetypes, use master slides |
| **Default theme** | PowerPoint Office theme colors | Custom color tokens |
| **Headshot grid on team page** | 8 same-size headshots in a grid | One headshot at a time with a sentence each, or cut |
| **Five-pillar methodology slide** | 5 identical icon-headline-blurb columns | Pick the *one* pillar that matters; structure around it |
| **"Questions?" closing slide** | Final slide just says "Questions?" | Specific commitment or specific question we want answered |
| **Microscopic body type** | 12pt body, 14pt caption | 18–24pt body minimum for projection |
| **Color in low contrast** | Brand color text on light gray background | Run contrast through pre-delivery-checklist |
| **Dense-text slide** | 200-word paragraph | Split, summarize, or move to speaker notes / document |

## Hand-offs

- **`pptx` skill** — for actual file generation, master slide setup, content placement.
- **[[design-document]]** — when content is too dense for a slide and belongs in a paired document.
- **[[design-imagery]]** — when slides need commissioned imagery.
- **[[design-visualization]]** — when slides need diagrams or data viz.
- **[[design-handoff]]** — when handing the deck to a presenter who didn't make it.

## Surface-specific cross-reference

For full slide-format specs (dimensions, type scale, master grid options, slide archetype detail), see [[surface-deck-16x9]] in `design/references/`.

## Constraints

- Don't open PowerPoint before the deck system is defined. The system precedes the slides.
- Don't accept "PowerPoint default theme" as a starting point. Custom tokens or use a previous Slalom-house template.
- Don't do per-slide creative when the deck has 20+ slides. Master slides exist for a reason.
- Don't skip the cover. The cover earns the next 10 minutes of attention.
- Don't end on "Thank you." or "Questions?". The closer says something specific.
- For Slalom client work, default register is **premium-restrained** for executive audiences, **swiss-modern** for technical, **editorial-tech** for thought-leadership. See [[stylistic-vocabulary]].
