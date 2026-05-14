---
name: pf-asset-orchestrator
description: >
  Reads asset prompts from 10_Content.md, routes each to the right generator
  (Higgsfield for images/video, Napkin.ai for infographics, Flaticon for icons),
  saves outputs to WIP/[deck]/assets/, and updates 10_Content.md with the
  resolved local paths. Polls long-running jobs and reports back.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/specialty, topic/asset-generation]
status: active
---

## When to invoke

Called from the Compose stage orchestrator before the slide composer runs. Assets need to exist before slides are composed because the composer needs resolvable image paths.

## Required context

Toolchain: [`../../references/presentation-factory-toolchain.md`](../../references/presentation-factory-toolchain.md). Foundations: [`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md).

## What this skill does

### Stage 1: Parse asset prompts from content file

Scan `WIP/[deck]/10_Content.md` for asset blocks. Three types:

- **Image prompt** blocks (Higgsfield) → output PNG/MP4 to `assets/images/`
- **Infographic** blocks (Napkin) → output PNG/SVG to `assets/infographics/`
- **Icon** blocks (Flaticon search) → output SVG/PNG to `assets/icons/`

Each prompt is associated with a slide number (the section header above it).

### Stage 2: Image generation (Higgsfield)

For each image prompt, call the Higgsfield MCP `generate_image` tool with the prompt text. Default settings:

- Model: `nano banana 2` for stills (general-purpose, fast)
- Alternative: `chatgpt image 2` for more photorealistic
- Aspect ratio: 16:9 for full-bleed, 4:3 for two-col layouts, 3:4 for portraits
- Resolution: 1920×1080 for hero, 1280×720 for inline

For video prompts (rare; only for openers or specific story slides), use `seedance 2`.

**Workflow**:
1. Submit prompt → returns job ID
2. Poll `job_display` until ready (or use the listener pattern if the MCP supports it)
3. Download the result via `show_medias` or direct URL
4. Save to `WIP/[deck]/assets/images/slide-NN-image.png` (or `.mp4` for video)
5. Update `10_Content.md`: replace the `**Image prompt**` block's path with `**Image**: ../assets/images/slide-NN-image.png`

**If generation fails** (rate limit, content policy, etc.):
- Log the failure with the slide number and reason
- Don't block the rest of the asset generation — continue with others
- Surface failures to user at the end with options: retry with adjusted prompt, manually source, or use a placeholder

### Stage 3: Infographic generation (Napkin)

Napkin.ai does not have an MCP. Two workflows:

**Path A — Manual (default)**:
1. For each infographic block, write the Napkin-ready description to `WIP/[deck]/assets/infographics/slide-NN-prompt.md`
2. Tell the user: "Open Napkin.ai, paste these prompts, save results to the infographics folder."
3. After user signals done, verify each expected file exists

**Path B — Browser MCP** (if `mcp__Claude_in_Chrome` available and user wants to skip manual):
1. Use the browser MCP to drive Napkin.ai
2. Submit each prompt, screenshot result, save to disk
3. This path is slower and more error-prone — only use if user explicitly opts in

### Stage 4: Icon sourcing (Flaticon)

For each icon block:
1. The block contains a Flaticon search query and a style preference
2. Either: (a) drive a browser search via `mcp__Claude_in_Chrome` and download the chosen icon, or (b) write the search query to `WIP/[deck]/assets/icons/slide-NN-needed.md` and ask user to source manually
3. Save SVGs to `WIP/[deck]/assets/icons/`
4. Update `10_Content.md` with the file paths

Pick ONE icon family for the whole deck. Don't mix Lucide / Phosphor / Flaticon styles within a deck — `design:design-iconography` covers this.

### Stage 5: Validation

After all generation runs:
- Count expected assets (parsed from content) vs. actual assets on disk
- Surface any missing assets to the user
- For images that came out wrong (visible to LLM via small thumbnail check): offer re-generation

### Stage 6: Update content file

Rewrite `10_Content.md` so every asset reference points at a local path. The composer skill needs concrete file paths to substitute into the templates.

## Higgsfield prompt patterns that work

From the `design:design-imagery` skill — these are the principles to follow:

- **Specific scene, not generic**: "wide shot of a healthcare nurse at a workstation, mid-conversation with a colleague, natural light, slight motion blur on background" — NOT "professional healthcare worker"
- **Mood line**: always include a mood ("collaborative," "focused," "warm") — single word per direction
- **Style line**: documentary / editorial / abstract / illustrative — pick one and hold
- **Light direction**: "natural daylight from left," "warm low-angle morning light" — light is the easiest brand differentiator
- **Avoid clichés**: explicitly negate ("no handshakes, no pointing-at-screens, no fake diversity tableaux")
- **Color palette**: "warm-cool contrast" or "muted earth tones with one accent" — guide the color story
- **Resolution**: always specify (1920×1080 for hero, 1280×720 for inline)

## Anti-patterns

- **Don't** generate generic stock-photo-style imagery (suited men pointing, fake diversity tableaux). Every image gets a specific scene description.
- **Don't** mix icon families across a deck.
- **Don't** silently swap a failed generation with a placeholder without telling the user.
- **Don't** generate assets if the user hasn't signed off on `10_Content.md` — image generation has cost. Confirm content first.
- **Don't** request more than ~12 unique images per deck — that's a strong signal the deck is over-decorated. Push back via user prompt.
