---
name: presentation-factory-ship
description: >
  Stage 4 orchestrator for the Presentation Factory — Ship. Calls pf-html-to-pdf
  to merge slides into deck.pdf, then pf-pdf-to-pptx to produce an editable
  deck.pptx. Calls presentation-factory-promoter agent to move the deck to its
  canonical home. Archives WIP.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/orchestrator, topic/ship]
status: active
---

## Goal

Turn the composed slide HTMLs into a final, branded, **editable** PowerPoint deck, and put it in its canonical home for the user to use.

## When to invoke

Called from `presentation-factory-create-flow` after Compose is complete, or directly via `/presentation-ship`.

## Pre-flight

Verify:
- `WIP/[deck]/slides/01-*.html` … files exist (count matches `10_Content.md`)
- `WIP/[deck]/slides/deck.html` exists (review index)
- User has signed off on the composed slides

If user hasn't reviewed `deck.html`, prompt them to before continuing.

## Steps

### Step 1 — Render PDF

Invoke `pf-html-to-pdf`. It:
1. Verifies Chromium/Chrome is installed
2. Renders each slide HTML → single-page PDF at 1920×1080
3. Merges into `WIP/[deck]/deck.pdf`
4. Validates page count matches slide count
5. Optionally extracts a slide-1 preview thumbnail

Surface the PDF preview to the user for a quick sanity check before proceeding to PPTX (slower).

### Step 2 — Convert to editable PPTX

Invoke `pf-pdf-to-pptx`. It:
1. Creates a new PowerPoint via the MCP
2. For each slide HTML: parses text + image references, looks up layout positions, adds text frames + images to the slide
3. Preserves italic-emphasis via mixed text runs (regular + italic Lora)
4. Adds speaker notes (where MCP supports it; otherwise writes to `notes.md` fallback)
5. Saves to `WIP/[deck]/deck.pptx`

**Note**: first time this skill is run, the `_layouts.json` file in `resources/templates/` may not exist. The skill must build it via the `scripts/build-layouts.js` Puppeteer script. Subsequent runs use the cached file. If a template is modified, the layouts file should be regenerated.

### Step 3 — Validation pass

Spot-check the output by:
1. Opening one slide in the MCP via `get_slide_content` — confirm text is editable
2. Confirming slide count matches expected
3. Listing any slides that fell back to image-fallback path

### Step 4 — Pick the destination

```
AskUserQuestion: "Where should this deck land?"
Options:
1. Internal library — 40_Library/Decks/
2. Pursuit / proposal folder — 20_Proposals/Active/[Pursuit]/Final/
3. Conference talk archive — 30_Talks/[YYYY]/[Event]/
4. Solution / practice deck — 10_Practice/Offerings/[Solution]/Decks/
5. Client-facing one-off — 20_Proposals/Active/[Client]/Decks/
```

For options 2-5, ask for the pursuit / event / solution / client name if not yet specified.

### Step 5 — Promote

Invoke `presentation-factory-promoter` agent with:
- `wip_path`: full path to `WIP/[deck]/`
- `intent`: matching the user's choice above
- `destination_details`: pursuit name / event / etc.

The agent copies `deck.pptx` + `deck.pdf` to the destination, updates any relevant index files, and archives WIP.

### Step 6 — Surface deliverables to user

```
Deck shipped.

[View PowerPoint](computer://path-to.pptx)
[View PDF](computer://path-to.pdf)
[Source markdown](computer://path-to-archive/10_Content.md)
```

Optionally suggest:
- Schedule a quarterly refresh check (if internal library / solution deck — these tend to age)
- Add a link in `40_Library/Decks/README.md` (if internal library)
- Update the parent pursuit's `_Brief.md` (if pursuit)

## Output

| File | Path |
|---|---|
| Editable PowerPoint | `[destination]/[deck-name].pptx` |
| PDF | `[destination]/[deck-name].pdf` |
| Archived WIP | `60_Digital_Manufacturing/Presentation_Factory/Archive/[deck-name]-[date]/` |

## Anti-patterns

- **Don't** skip the PDF validation step — if PDF is broken, PPTX will inherit the issue
- **Don't** ship without confirming the destination — promoting to the wrong folder is a real cleanup
- **Don't** delete the WIP — always archive. The source-of-truth markdown is in there.
- **Don't** silently fall back to image-only PPTX without telling the user — they need to know which slides aren't fully editable
- **Don't** auto-promote on success — the user picks the destination via AskUserQuestion
