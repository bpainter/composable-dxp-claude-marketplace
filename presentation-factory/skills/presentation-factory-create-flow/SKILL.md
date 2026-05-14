---
name: presentation-factory-create-flow
description: >
  The main orchestrator for the Slalom Presentation Factory. Walks the user
  through the Frame → Draft → Compose → Ship pipeline end-to-end. Uses
  AskUserQuestion for discrete decisions; conversational input for free-text.
  Routes through stage orchestrators and pulls specialty skills from
  consulting, design, marketing, brand, and the factory's own pf-* skills.
  Auto-activates when a user runs /presentation-create.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/orchestrator, topic/pipeline]
status: active
---

## Goal

Take the user from "I need a deck on [topic]" to a finished, branded, editable PowerPoint in the right folder. End-to-end. Cowork-native — AskUserQuestion at every discrete decision.

## When to invoke

- User runs `/presentation-create`
- User starts a new chat asking for a deck and the intent is unambiguous
- User asks any version of: "build a deck for [audience]," "I need slides for [event]," "make a pitch for [client]"

For partial workflows, route to the stage orchestrator directly via the matching slash command — `/presentation-frame`, `/presentation-draft`, `/presentation-compose`, `/presentation-ship`.

## Required context

For pipeline mechanics, the two styles, the three brands, the 25 templates, anti-patterns:  
[`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md).

For the toolchain (Higgsfield, Napkin, Flaticon, Chromium, PowerPoint MCP):  
[`../../references/presentation-factory-toolchain.md`](../../references/presentation-factory-toolchain.md).

For Slalom brand patterns from the May 2026 crawl:  
[`../../references/slalom-brand-patterns.md`](../../references/slalom-brand-patterns.md).

**Do not re-explain the pipeline** — cite the foundations file.

## Stages

### Stage 0 — Diagnose intent (30 sec)

Before invoking any stage, confirm what the user wants. Use **AskUserQuestion**:

```
Question: "What would you like to do?"
Options:
1. Create a new presentation from scratch (full pipeline) — Recommended
2. Update an existing deck (refresh content, re-render, or change brand)
3. Run only one stage (frame / draft / compose / ship)
4. Check status of decks in WIP
```

- **New from scratch**: proceed to Stage 1.
- **Update existing**: ask for the deck name. Load the existing `WIP/[deck]/` (or restore from `Archive/`). Skip to the stage the user wants to begin from.
- **Single stage**: route to the matching orchestrator skill.
- **Status**: list `WIP/` decks and which stage each is at, then stop.

### Stage 1 — Frame

Hand off to **`presentation-factory-frame`**. That skill:

1. Walks the intake via AskUserQuestion:
   - Style (informational | presentational)
   - Brand (Slalom | Composable DXP | Client)
   - If Client → routes to `pf-brand-router` for brand ingestion
   - Audience, runtime, purpose, context
2. Calls the `presentation-factory-scaffolder` agent to create `WIP/[deck]/`.
3. Calls `pf-brand-router` to populate `WIP/[deck]/brand/`.
4. Writes `00_Intake_Brief.md`.

Returns when intake is complete and brand assets are in place.

**Pause point** — confirm intake before drafting:

```
Question: "Intake complete. Ready to draft the content?"
Options:
1. Yes, start drafting — Recommended
2. Yes, but draft only the title + first 3 slides so I can review the framing
3. Pause — let me add more context first
```

### Stage 2 — Draft

Hand off to **`presentation-factory-draft`**. That skill:

1. Calls `pf-content-author` to:
   - Lock the narrative arc
   - Map slides to templates (the 25-template catalog)
   - Draft each slide's content with italic-emphasis applied per brand
   - Specify image / infographic / icon needs as prompts in the content file
2. Calls `pf-speaker-notes` to ensure every slide has 3-5 sentence speaker notes.
3. Calls specialty skills:
   - `consulting:consulting-management-consultant` for POV alignment
   - `marketing:marketing-copywriter` for body copy
   - `brand:brand-voice-tone` for register
   - `consulting:consulting-humanize` for AI-tell removal (mandatory final pass)
   - `design:design-taste` for the four checks

Returns when `WIP/[deck]/10_Content.md` is complete.

**Pause point** — review the content before composing:

```
Question: "Content draft complete ([N] slides). Ready to compose into HTML?"
Options:
1. Yes, compose all slides — Recommended
2. Yes, but I want to refine slides X, Y, Z first
3. Pause — let me read 10_Content.md and edit directly
```

### Stage 3 — Compose

Hand off to **`presentation-factory-compose`**. That skill:

1. Calls `pf-asset-orchestrator` to generate all images / infographics / icons.
   - Images via Higgsfield MCP (nano banana 2 / chatgpt image 2 for stills; seedance 2 for video)
   - Infographics via Napkin.ai (manual or browser MCP)
   - Icons via Flaticon (one family per deck — held consistent)
2. Calls `pf-slide-composer` to:
   - Inline brand tokens into each template
   - Substitute every `data-slot` element with content from `10_Content.md`
   - Write one HTML file per slide + a `deck.html` review index

Returns when `WIP/[deck]/slides/` is populated.

**Pause point** — review the rendered slides:

```
Question: "Slides composed ([N] HTML files). How does the deck look?"
Options:
1. Ship it — convert to PDF and PPTX — Recommended
2. Re-compose specific slides (I'll tell you which)
3. Go back to content and revise
```

### Stage 4 — Ship

Hand off to **`presentation-factory-ship`**. That skill:

1. Calls `pf-html-to-pdf` to render each slide HTML to PDF and merge into `deck.pdf`
2. Calls `pf-pdf-to-pptx` to convert into editable PowerPoint (`deck.pptx`)
3. Calls `presentation-factory-promoter` agent to move the deck to its canonical home based on intent (library / pursuit / talk / solution / client-one-off)
4. Archives WIP

Returns when promotion is complete.

## Done — surface the deliverables

```
Deck complete.

[View PowerPoint](computer://[full-path-to].pptx)
[View PDF](computer://[full-path-to].pdf)
[Source content](computer://[full-path-to-archive]/10_Content.md)
```

Offer to schedule a quarterly review if the deck is in the internal library (likely to get reused).

## Anti-patterns (orchestrator-level)

- **Don't** skip the intent diagnosis — assuming "new from scratch" when the user wanted a refresh wastes a lot of work
- **Don't** auto-advance past pause points — every pause is a chance for the user to catch a wrong assumption cheaply
- **Don't** invoke specialty skills out of order — content must come before assets must come before composition
- **Don't** let the brand router get skipped on Client decks — a "Slalom-branded" client deck is a credibility hit
- **Don't** ship without running `consulting:consulting-humanize` on the final content — this is the AI-tell filter
