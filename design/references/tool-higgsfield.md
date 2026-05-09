---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/imagery, topic/tool, topic/ai-imagery]
status: active
---

# Tool: higgsfield

The workflow protocol for [higgsfield.ai](https://higgsfield.ai/mcp). Loaded by [[design-imagery]] when the project needs generated stills or video.

higgsfield is an AI image and video generation platform with multiple models and an MCP available for direct integration. It's the **default for original imagery** when commissioning isn't budgeted and stock doesn't fit the brief.

## Models and defaults

### Stills
- **Default model: nano banana 2.** Fast, photoreal, brand-safe. Best for B2B consulting deliverables.
- **Alternative: chatgpt image 2.** Better at complex compositions, abstract concepts, illustrative styles. Slower than nano banana 2.
- Other models: check higgsfield's model catalog as it evolves.

### Video
- **Default model: seedance 2.** Strong on motion, lighting, scene composition.
- **Use cases:** social video, presentation hero loops, deliverable video assets.

## MCP integration

higgsfield offers an MCP server. **Verify whether it's installed in the current Claude environment** before assuming MCP-based generation is available. If not installed, the workflow uses the higgsfield web UI directly.

When MCP is available:
- Tools follow `mcp__higgsfield__*` naming.
- Generation, status checking, and download can happen programmatically.
- Verify the tool list at start of any imagery task.

When MCP is not available:
- Workflow is web-based: open higgsfield.ai, generate, download, paste into deliverable.
- Document this in the run log.

## When to use higgsfield

- **Cover imagery** for decks, documents, social.
- **Hero visuals** for marquee deliverables.
- **Section dividers** needing visual punctuation.
- **Brand-statement visuals** when commissioned photography isn't budgeted.
- **Social asset hero imagery.**
- **Video for social and event use** (seedance 2).

## When NOT to use higgsfield

- **Photography of real people, places, or events** — needs real photography.
- **Brand identity marquee** — commission custom.
- **Fast-iterating utility imagery** — stock is faster if license allows.
- **Code-generated imagery** (data viz, diagrams) — use [[design-visualization]] tools.

## Prompt protocol

higgsfield prompt quality follows the same 7-element art direction brief from [[design-imagery]]. Don't reduce a prompt to the subject alone — that's how you get hands-on-keyboard.

### The full prompt structure

```
Subject: {specific subject — be detailed}
Setting: {location, time of day, environment, specific details}
Mood: {sensory adjectives, emotional register}
Lighting: {natural / studio / dramatic / soft / harsh; specific direction and color}
Composition: {wide / medium / tight; symmetry; rule-of-thirds; subject placement}
Palette: {3–5 colors that should appear}
Style: {photoreal / illustrative / editorial / documentary / cinematic}
Negative prompt: {what NOT to include — actively reject AI defaults}
```

### Prompt example (good)

```
Subject: A retail merchandiser in late 30s walking through a back-of-house warehouse aisle, holding a tablet, slightly turned toward camera but not posing.

Setting: Back-of-house of a mid-tier retail store, Tuesday afternoon, 2026. Industrial shelving with apparel boxes, one loading-dock door open behind subject letting in natural light. Concrete floor, fluorescent overhead lights, slight dust in the air.

Mood: Quiet authority, late-shift focus, slightly tired but capable. Documentary-grade. Not stock-pose.

Lighting: Side-lit by warm fluorescent (3000K), natural cool light (5500K) from loading-dock door creating a gentle backlight on subject's hair and shoulders. Slight shadow detail on the right side of frame. No flash.

Composition: Wide medium shot. Subject right-third of frame. Environment left two-thirds. Rule-of-thirds. Slight off-axis angle.

Palette: Warm amber from fluorescent, cool blue-gray from outside, deep charcoal from machinery, muted earth tones from boxes.

Style: Photoreal documentary photography. Editorial register, not commercial-stock register.

Negative prompt: Not stock-pose. No fake smiling at camera. No clean-warehouse-cliché. No hands-on-keyboard. No back-of-laptop. No abstract-tech-blob. No flat lighting. No purple-blue gradient.
```

### Prompt example (bad — what to avoid)

```
A retail worker in a warehouse using a tablet.
```

This produces AI-default output: stock-pose, flat lighting, generic composition, "warehouse" generic.

## Iteration protocol

### Generation 1 — calibrate
- Generate the first batch with the full prompt.
- Look for: which elements landed correctly? Which missed?
- Note specifically what's wrong (lighting, mood, subject pose, composition).

### Generation 2 — refine
- Adjust the prompt based on what missed.
- Add stronger negative prompts for AI defaults that crept in.
- Often: stronger lighting direction, more specific subject pose, tighter composition spec.

### Generation 3 — committed
- The generation that ships.
- If still drifting, escalate: switch model (nano banana 2 → chatgpt image 2), or escalate to commission.

**Don't ship the first generation.** Iterate at least twice. AI defaults appear strongly in first generations.

## Model selection guidance

### nano banana 2 (default for B2B consulting work)
- Strong on photoreal subjects, environments, lighting.
- Brand-safe — less likely to produce uncanny faces or weird hand artifacts than older models.
- Fast — usable iterations in seconds.
- **Best for:** documentary-style imagery, business contexts, real-world subjects.

### chatgpt image 2 (alternative)
- Stronger on complex compositions, abstract concepts, illustrative styles.
- Slower than nano banana 2.
- **Best for:** conceptual hero visuals, illustrative whitepaper covers, brand-statement abstracts.

### seedance 2 (video)
- Motion, scene transitions, lighting changes over time.
- **Best for:** social video, presentation hero loops, deliverable video.
- Note: video is more expensive (compute + iteration time) than stills.

## Quality gates before shipping

- [ ] **Three generations minimum.** Don't ship the first.
- [ ] **Subject accurate** — labels, products, environments are correct.
- [ ] **No AI artifacts** — extra fingers, melted faces, illegible text. Common in 2025-era models; less common in 2026 nano banana 2 / seedance 2 but still possible.
- [ ] **Composition matches deliverable surface** — cover image at right aspect ratio, hero at right resolution.
- [ ] **Lighting and mood match imagery system** — consistent across imagery family.
- [ ] **Caption / source written** — "Generated via higgsfield, nano banana 2, prompt date {date}" or commissioned-style attribution.
- [ ] **Alt text drafted** for accessibility.
- [ ] **License confirmed** — higgsfield generation rights for commercial use.

## Brand-consistency across an imagery family

When generating multiple images for one deliverable (cover + section dividers + supporting visuals), maintain consistency:

- **Lock the lighting signature** — all images use the same lighting direction, color temperature, intensity.
- **Lock the composition signature** — all images use the same approach (wide / medium / tight, symmetric / asymmetric).
- **Lock the palette** — all images thread the same 3–5 colors.
- **Lock the subject category** — if the cover is people-in-environment, the section dividers should also be people-in-environment, not abstract.
- **Re-use the prompt template** — don't write a fresh prompt per image; modify one core template per image.

This is how a deliverable feels like one art director directed all the imagery, not like AI generated random images.

## Common higgsfield failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Generic stock-pose** | Subject smiling at camera with arms crossed | Add stronger pose direction in prompt + stronger negative prompt |
| **Hands-on-keyboard** | Generic close-up of hands typing | Cut from prompt entirely; use real subject and environment |
| **Mixed lighting across family** | Cover golden-hour, section dividers studio | Lock lighting signature in imagery system |
| **AI artifact in face/hands** | Extra fingers, melted face, weird eyes | Regenerate; if persistent, switch model |
| **Text in image is illegible** | Generated text on signs, badges, computers | AI models still struggle here; either edit out in post or regenerate |
| **Lighting too flat** | No shadows, no depth | Add stronger lighting direction (e.g., "side-lit, key light from left") |
| **Composition too centered** | Subject dead center, no breathing room | Add asymmetric composition spec |
| **Palette drift across family** | Image 1 warm, image 2 cool | Lock palette in imagery system |
| **AI Unsplash aesthetic** | Default-AI-stock look | Add stronger negative prompts; iterate |
| **Wrong aspect ratio** | Generated 1:1 when needed 16:9 | Specify aspect in higgsfield settings |

## Higgsfield + curation

Higgsfield is a tool. Curation is the skill. Generating 100 images is fast; picking the 5 that ship — and iterating to make them right — is the work. Budget more time for curation than for generation.

## Cross-references

- **Skill:** [[design-imagery]] (the workflow that uses this tool).
- **Surface contexts:** [[design-presentation]], [[design-document]], [[design-social-asset]].
- **Anti-bland:** [[ai-tells-forbidden-patterns]] — Unsplash defaults ban, AI artifacts, hands-on-keyboard ban.
- **Pre-delivery:** [[pre-delivery-checklist]].
