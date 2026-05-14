---
name: pf-slide-composer
description: >
  The Compose stage's core skill. Reads 10_Content.md and produces one HTML file
  per slide in WIP/[deck]/slides/, plus a deck.html index that loads every slide
  for review. For each slide: loads the right template, inlines the brand
  tokens.css, substitutes every data-slot="..." element with the content from
  the markdown, and writes the final HTML.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/specialty, topic/html-composition]
status: active
---

## When to invoke

Called from the Compose stage orchestrator after `10_Content.md` is reviewed.

## Required context

Templates: [`../../references/presentation-factory-templates.md`](../../references/presentation-factory-templates.md). Foundations: [`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md). The slot contract (in templates.md) defines every data-slot the templates use.

## What this skill does

### Stage 1: Read inputs

1. Read `WIP/[deck]/10_Content.md` — parse the slide-by-slide content.
2. Read `WIP/[deck]/brand/tokens.css` — the brand-specific tokens.
3. Confirm the assets directory `WIP/[deck]/assets/` has been populated (images, infographics, icons).
4. Create `WIP/[deck]/slides/` if it doesn't exist.

### Stage 2: For each slide block in the content file

1. **Find the template**: parse the `template: NN-filename.html` line. Load `resources/templates/NN-filename.html`.
2. **Inline the brand tokens**:
   - Find the `<link rel="stylesheet" href="../tokens/slalom.css">` line
   - Replace with `<style>` block containing the actual contents of `WIP/[deck]/brand/tokens.css`
   - The result: each slide is fully standalone (no external CSS dependency)
3. **Replace logo paths**:
   - Find `src="../assets/slalom-logo.svg"` (and the white variant)
   - Replace with the path to the deck's brand logo: relative path `../brand/logo.svg` (or `logo-white.svg`)
4. **Substitute every `data-slot` element**:
   - Iterate `<element data-slot="slot-name">existing</element>` elements
   - For text slots: replace text content with the corresponding field from the content markdown
   - For `<img data-slot="image-src" src="...">`: replace `src` with the asset path from the content markdown (e.g. `../assets/images/slide-04-image.png`)
   - For `data-slot="logo"` `<img>` elements: replace `src` with the brand logo path
   - For `data-slot="slide-number"`: write "NN / TT" where TT is total slide count
   - For the italic emphasis: the content markdown uses `*emphasis*` — convert to `<em>emphasis</em>` in the title element
5. **Write the slide**: save to `WIP/[deck]/slides/NN-template-name.html`

### Stage 3: Build the deck index

After all slides are composed, build `WIP/[deck]/slides/deck.html` — a single page that lists every slide for review. Each slide rendered at scale in an iframe with a metadata caption.

Use the same pattern as `resources/templates/index.html` — a 2-column grid of scaled iframes.

### Stage 4: Self-validate

Quick sanity check on the output:

1. **Every slide present?** Count of HTML files in `slides/` should match slide count in `10_Content.md` + 1 (for `deck.html`).
2. **Token-substituted?** Grep one slide for `var(--slalom-blue)` — should be present if Slalom brand. If links to external tokens still exist, the inlining failed.
3. **Slot-substituted?** Grep for `data-slot=` — slots should remain (they're useful for the PPTX converter), but the *content* inside them should be the user's content, not the template default.
4. **Asset paths resolve?** For each `<img src="...">`, the file should exist on disk in `WIP/[deck]/`.

Surface any failures in a compose log: `WIP/[deck]/compose.log`.

### Stage 5: Open the deck for review

Output to the user:
- Total slide count
- Path to `deck.html` for browser preview
- Any validation flags from the compose log

Pause for user review via AskUserQuestion:

```
Question: "[N] slides composed. How does the deck look?"
Options:
1. Ship it — convert to PDF and PPTX — Recommended
2. Re-compose specific slides (I'll tell you which)
3. Go back to content — revise 10_Content.md and recompose
```

## Implementation pattern (pseudocode)

The actual implementation can be a Bash + Python script in the WIP folder, or executed step-by-step by the orchestrator:

```python
import re, os, pathlib

deck_dir = pathlib.Path("WIP/[deck]")
templates_dir = pathlib.Path("../../resources/templates")
slides_dir = deck_dir / "slides"
slides_dir.mkdir(exist_ok=True)

tokens = (deck_dir / "brand" / "tokens.css").read_text()
content_blocks = parse_content_md(deck_dir / "10_Content.md")

for i, block in enumerate(content_blocks, start=1):
    template = (templates_dir / block.template).read_text()
    # Inline tokens
    out = template.replace(
        '<link rel="stylesheet" href="../tokens/slalom.css">',
        f'<style>{tokens}</style>'
    )
    # Replace logo paths
    out = out.replace('src="../assets/slalom-logo.svg"', 'src="../brand/logo.svg"')
    out = out.replace('src="../assets/slalom-logo-white.svg"', 'src="../brand/logo-white.svg"')
    # Substitute slots
    for slot, value in block.slots.items():
        # Convert markdown *emphasis* to <em>
        value = re.sub(r'\*(.+?)\*', r'<em>\1</em>', value)
        out = substitute_slot(out, slot, value)
    # Slide number
    out = out.replace(
        '<span class="slide-number" data-slot="slide-number">01 / 24</span>',
        f'<span class="slide-number" data-slot="slide-number">{i:02d} / {len(content_blocks):02d}</span>'
    )
    out_path = slides_dir / f"{i:02d}-{block.template_name_short}.html"
    out_path.write_text(out)

build_deck_index(slides_dir, content_blocks)
```

The skill can either invoke this script via Bash, or perform the substitutions directly with Edit operations on copies of the template files — whichever is more reliable.

## Anti-patterns

- **Don't** rewrite the template's CSS — only swap tokens, never structural styles. If the template's layout is wrong, fix the template (and bump the version), don't fix at compose time.
- **Don't** strip the `data-slot` attributes after substitution — the PPTX converter needs them to identify text frames.
- **Don't** silently fail if a slot can't be matched — log it and surface as a validation warning.
- **Don't** inline images as base64 in the slide HTML unless explicitly required (it bloats the file and slows the PDF render). Use relative paths.
- **Don't** apply brand tokens manually — always read the brand's `tokens.css` and inline it. Manual token application drifts.
