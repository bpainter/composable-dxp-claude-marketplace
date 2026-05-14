---
name: pf-content-author
description: >
  The Draft stage's authoring skill. Takes the intake brief + research + any
  source materials and produces 10_Content.md — slide-by-slide content with
  template assignment, eyebrow/title/body slots, asset prompts (image / infographic
  / icon), and the italic-Lora emphasis applied where appropriate. Sequences calls
  to consulting + design + humanize + brand-voice skills.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/specialty, topic/content-authoring]
status: active
---

## When to invoke

Called from the Draft stage orchestrator after Frame is complete. The intake brief and any research materials should be in `WIP/[deck]/`.

## Required context

Foundations: [`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md). Templates: [`../../references/presentation-factory-templates.md`](../../references/presentation-factory-templates.md). Brand patterns: [`../../references/slalom-brand-patterns.md`](../../references/slalom-brand-patterns.md).

## What this skill does

### Stage 1: Outline the deck arc (5 min)

Before writing slide content, choose the narrative arc. For most decks one of:

| Arc | Slide count | Use when |
|---|---|---|
| **Problem → Approach → Result → Ask** | 8-12 | Pitch, pursuit, kickoff |
| **Context → Insight → Implication → Recommendation → Plan** | 10-16 | Strategy POV, board update |
| **Welcome → Agenda → Sections → Working time → Synthesis** | 10-20 | Workshop, working session |
| **Hook → Story → Lesson → Application** | 8-15 | Conference talk, keynote |
| **Section per topic → Summary** | 15-30 | Training, playbook (informational) |

Propose the arc to the user via AskUserQuestion if not specified in intake. Lock the arc before writing.

### Stage 2: Map slides to templates (10 min)

For each slide in the arc, pick a template from the 25-template catalog. Reference [`../../references/presentation-factory-templates.md`](../../references/presentation-factory-templates.md).

Output a deck outline like:

```
Slide 01 — Title (01-title.html)
Slide 02 — Agenda (03-agenda.html)
Slide 03 — Section Divider: Problem (02-section-divider.html)
Slide 04 — Stat-driven problem (13-big-stat.html — 1-stat variant)
Slide 05 — Three forces converging (10-three-col.html)
Slide 06 — Section Divider: Approach (02-section-divider.html)
…
```

Sanity-check: style alignment. If the user picked **presentational** but the outline is heavy on table/list-with-icons templates, propose alternatives. If **informational**, lean toward dense layouts.

Pause for review: confirm the outline before drafting slide content.

### Stage 3: Draft each slide's content (the bulk of the work)

For each slide in the outline, write a section of `10_Content.md` following this shape:

```markdown
## Slide NN — [Template name] — template: NN-filename.html

**Eyebrow**: [optional]
**Title**: [headline with *italic emphasis* on 1-3 emphasis words]
**Subtitle / Body**: [as the template needs]
**Slot details**: [for any complex template, the per-slot content]

**Image prompt** (Higgsfield, 1920×1080):
> [Full prompt as documented in the toolchain reference]

**Speaker notes**: [3-5 sentences for the presenter]

---
```

#### Authoring sequence per slide

1. **Frame the message**: what's the ONE thing this slide must communicate? Write that as a single sentence in your scratch space first.
2. **Cite consulting POV**: invoke `consulting:consulting-management-consultant` (or `consulting:consulting-digital-strategist` for digital topics) to validate the framing against Slalom's POV as a digital consultancy. The slide should sound like Slalom — not like a generic AI deck.
3. **Draft the copy**: invoke `marketing:marketing-copywriter` for the actual words. Aim for plain, declarative, specific.
4. **Apply the italic-emphasis pattern** (if brand = Slalom):
   - Pick 1-3 words from the title that carry meaning (usually a noun or short phrase)
   - Wrap them in `*...*` markdown (or `<em>` in HTML) so the composer renders them in italic Lora
   - Examples: "AI that drives *real impact*", "Build *better tomorrows* for everyone"
5. **Specify imagery / infographic / icon needs**:
   - For image slots: write a Higgsfield-ready prompt per the toolchain reference
   - For infographic slots: write a Napkin-ready description
   - For icon slots: write a Flaticon search query, hold the family consistent across the deck
6. **Apply brand voice**: invoke `brand:brand-voice-tone` to align with the brand's register
7. **Strip AI tells**: invoke `consulting:consulting-humanize` to rewrite anything that sounds like consultant filler ("elevate," "leverage," "seamless," "unleash," "next-gen," "best-in-class")
8. **Write speaker notes**: 3-5 sentences. What does the presenter say to back up this slide? Speaker notes are NOT the body copy read aloud — they're the framing the body copy doesn't include.

### Stage 4: Taste check (5 min)

When the full content file is drafted, invoke `design:design-taste` against the deck as a whole. Apply the four checks:

1. **Swap** — could every italic-emphasis title belong to a generic AI consulting deck, or does it sound like Slalom?
2. **Squint** — does the deck have visual variance (mix of templates), or is every slide a three-col-card?
3. **Signature** — does the italic-Lora emphasis show up where it should?
4. **Token** — are colors / gradient described where the design expects them?

Fix anything that fails before pausing for user review.

### Stage 5: Pause for user review

After `10_Content.md` is complete, surface it for the user. Use AskUserQuestion:

```
Question: "Content draft complete (24 slides). Ready to compose into HTML?"
Options:
1. Yes, compose all slides — Recommended
2. Yes, but I want to refine the opening 3 slides first
3. Pause — I want to review the full content file
```

## The italic-Lora emphasis pattern — applying it

When `Italic-emphasis: on` in the brand brief (Slalom default):

- **Every title slide** should have 1-3 emphasis words
- **Most section dividers** should have an emphasis
- **About 70% of body slide titles** should have an emphasis
- **Pull quotes** don't need emphasis — the whole thing is italic Lora already
- **Don't overdo it** — if every slide uses emphasis it loses signal

Picking emphasis words:
- Prefer the noun or short phrase that carries the meaning
- Avoid emphasizing function words ("the," "and," "of")
- Avoid emphasizing the longest word — usually the brand-noun, not the modifier
- Examples:
  - ✓ "Build *better tomorrows* for everyone"
  - ✗ "*Build* better tomorrows for everyone"
  - ✓ "AI that drives *real impact*"
  - ✗ "AI that *drives* real impact"

When `Italic-emphasis: off` (Composable DXP, most Client brands):

- Skip the inline `*...*` markdown
- Use Lora 700 (non-italic) for the whole title — applied by the tokens
- The title still wraps balance + tracks tight; just no italic mid-line

## Output

Single file: `WIP/[deck]/10_Content.md`. Slide-by-slide. Includes:
- Deck-level metadata header (style, brand, audience, runtime)
- Slide-by-slide blocks (template + slots + asset prompts + speaker notes)
- A simple deck-table-of-contents at the top

## Anti-patterns

- **Don't** write slide content without first locking the arc and the template-per-slide map
- **Don't** generate asset prompts that say "professional business person in office" — write specific, scene-aware prompts
- **Don't** skip `consulting:consulting-humanize` — every Slalom-brand deck needs this pass
- **Don't** apply italic-emphasis to a brand where it doesn't belong
- **Don't** write speaker notes that just repeat the slide body
- **Don't** write 11-bullet lists. If a list-with-icons template is being used, hold to 3-5 items max
