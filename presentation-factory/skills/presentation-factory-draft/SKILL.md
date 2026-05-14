---
name: presentation-factory-draft
description: >
  Stage 2 orchestrator for the Presentation Factory — Draft. Calls pf-content-author
  to write the slide-by-slide content; pf-speaker-notes for notes; consulting +
  marketing + brand + humanize + design-taste skills for content quality. Pauses
  for user review at the end. Output: 10_Content.md.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/orchestrator, topic/draft]
status: active
---

## Goal

Produce `WIP/[deck]/10_Content.md` — every slide's content + asset prompts + speaker notes, validated against brand voice and stripped of AI tells.

## When to invoke

Called from `presentation-factory-create-flow` after Frame is complete, or directly via `/presentation-draft`.

## Pre-flight

Verify:
- `WIP/[deck]/00_Intake_Brief.md` exists and has all required sections
- `WIP/[deck]/brand/tokens.css` and `logo.svg` exist

If anything's missing, route back to Frame and stop.

## Steps

### Step 1 — Hand off to pf-content-author

Invoke the `pf-content-author` skill. It walks its own 5-stage process:
1. Outline the deck arc
2. Map slides to templates
3. Draft each slide's content with italic-emphasis applied
4. Specify image / infographic / icon prompts
5. Run consulting + humanize + brand-voice + taste-check passes

The skill returns when `10_Content.md` is drafted (without speaker notes if user opted to defer them).

### Step 2 — Apply speaker notes

Invoke `pf-speaker-notes` to ensure every slide block has a `**Speaker notes**:` section. This skill either drafts notes that are missing or refines weak existing notes.

### Step 3 — Final humanize pass on the full deck

Invoke `consulting:consulting-humanize` once more against the complete `10_Content.md` file. This catches any AI-tell phrases that slipped through per-slide passes (looking at headlines holistically across the deck — e.g. "elevate" appearing in three different slide titles is something you only catch reading them together).

### Step 4 — Taste-check the deck

Invoke `design:design-taste` against the deck-level question: does this deck have **variance**? If 60%+ of slides use the same template, push back via user prompt to redistribute. If every title uses italic-emphasis on the same word ("impact" 8 times), suggest alternatives.

### Step 5 — Generate the deck-table-of-contents

At the top of `10_Content.md`, write a simple TOC:
```markdown
# Deck: [name]
**Style**: [informational | presentational]
**Brand**: [slalom | composable-dxp | client:NAME]
**Audience**: [from intake]
**Runtime**: [from intake]
**Slide count**: [actual]

## Table of contents
1. Title — Build *better tomorrows* for ACME
2. Agenda — What we'll *cover*
3. Section: The problem
4. The market is *shifting* faster
...
```

### Step 6 — Surface for review

Return path to `10_Content.md` for user review. Tell the user:
- Total slide count
- Approx. asset count (images + infographics + icons that need to be generated)
- Estimated compose time

Pause via the main orchestrator's pause point.

## Output

`WIP/[deck]/10_Content.md` — slide-by-slide content, asset prompts, speaker notes, deck metadata header.

## Anti-patterns

- **Don't** skip the deck-level humanize pass — per-slide passes miss cross-slide patterns
- **Don't** generate assets at this stage — that happens in Compose. Content first.
- **Don't** decide the slide count before the arc — arc decides count
- **Don't** write content that doesn't map to a specific template — if a slide doesn't fit any of the 25, the content has the wrong shape (split it or merge it)
