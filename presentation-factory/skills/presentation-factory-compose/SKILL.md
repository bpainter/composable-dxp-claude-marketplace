---
name: presentation-factory-compose
description: >
  Stage 3 orchestrator for the Presentation Factory — Compose. Calls
  pf-asset-orchestrator to generate all images / infographics / icons, then
  pf-slide-composer to inline brand tokens into each template and substitute
  data-slots with content. Output: WIP/[deck]/slides/NN-*.html + deck.html.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/orchestrator, topic/compose]
status: active
---

## Goal

Produce one HTML file per slide in `WIP/[deck]/slides/` plus a `deck.html` review index. Every slide is self-contained: tokens inlined, slots substituted, asset paths resolved.

## When to invoke

Called from `presentation-factory-create-flow` after Draft is complete, or directly via `/presentation-compose`.

## Pre-flight

Verify:
- `WIP/[deck]/10_Content.md` exists and has slide blocks
- `WIP/[deck]/brand/tokens.css` exists
- Empty `WIP/[deck]/assets/` subfolders exist

If `10_Content.md` is missing, route to Draft and stop.

## Steps

### Step 1 — Generate assets

Invoke `pf-asset-orchestrator`. It:
1. Parses asset prompts from `10_Content.md`
2. Generates images via Higgsfield MCP
3. Generates infographics via Napkin.ai (manual or browser-driven)
4. Sources icons via Flaticon
5. Updates `10_Content.md` with local asset paths
6. Reports any failures

If assets fail and the user wants to skip / retry / placeholder, handle that conversation before proceeding.

### Step 2 — Confirm asset readiness

Verify every asset referenced in `10_Content.md` exists on disk in `WIP/[deck]/assets/`. List any gaps.

If gaps exist:
```
AskUserQuestion: "Some assets are missing. How to proceed?"
Options:
1. Retry the failed assets — Recommended if it was a transient issue
2. Skip — compose with placeholder boxes and we'll fix in PowerPoint
3. Pause — I'll source manually and resume
```

### Step 3 — Compose slides

Invoke `pf-slide-composer`. It:
1. Reads `10_Content.md`
2. For each slide block: loads the right template, inlines tokens, substitutes slots, writes to `slides/NN-*.html`
3. Builds `slides/deck.html` index page
4. Self-validates

### Step 4 — Open the deck.html for user review

Return the path to `WIP/[deck]/slides/deck.html` so the user can open it in a browser to spot-check.

Pause via the main orchestrator. Common feedback patterns and how to handle:

- "Slide N's title is too long" → user edits `10_Content.md`, re-run `pf-slide-composer` for that slide only
- "I don't like image on slide N" → re-run `pf-asset-orchestrator` for that slide's image prompt
- "The whole arc feels off" → route back to Draft

## Output

- `WIP/[deck]/slides/01-*.html` … `slides/NN-*.html`
- `WIP/[deck]/slides/deck.html` (index for review)
- `WIP/[deck]/compose.log` (validation results)

## Anti-patterns

- **Don't** generate assets if content isn't user-approved — wasted Higgsfield credits
- **Don't** compose without the assets in place — the slides will have broken image links
- **Don't** silently swap a missing asset for a generic placeholder — surface the gap
- **Don't** skip the deck.html review — the user catches things in the browser preview they won't catch in the markdown
