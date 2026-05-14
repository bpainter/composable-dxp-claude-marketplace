---
name: pf-speaker-notes
description: >
  Generates or refines speaker notes for every slide in 10_Content.md. Each
  slide gets 3-5 sentences the presenter can use as a teleprompter — not the
  body copy read aloud, but the framing the body copy doesn't include.
  Runs after content drafting and before composition.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/specialty, topic/speaker-notes]
status: active
---

## When to invoke

Called from the Draft stage orchestrator after the slide content is drafted. Speaker notes are part of `10_Content.md` from initial draft, but this skill exists to:
1. Generate notes if they were skipped during initial drafting
2. Refine notes that are too thin / too verbose / too repetitive of the slide body

Also re-runs at compose time to ensure every slide has a notes block (since the PPTX export needs them).

## Required context

Foundations: [`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md). Brand patterns: [`../../references/slalom-brand-patterns.md`](../../references/slalom-brand-patterns.md).

## What good speaker notes look like

Speaker notes are NOT:
- The slide title read aloud
- The body copy read aloud
- A bullet list of "what to say"
- A monologue (>200 words) the presenter has to memorize

Speaker notes ARE:
- 3-5 sentences a presenter can glance at and recover the thread
- The *framing* — why this slide is here, what the audience should leave knowing
- A transition to the next slide (often the last sentence)
- Specific evidence the presenter can cite when asked

### Structure per slide

```markdown
**Speaker notes**:
[1-2 sentences: what this slide is doing in the deck's arc — why now?]
[1-2 sentences: the specific point — backed by a number, name, or evidence if relevant]
[1 sentence: transition to next slide — "Which raises the question we're going to spend the next ten minutes on…"]
```

Total length target: 60-120 words. Under 50 is thin; over 150 is a monologue.

### Style

- **First person plural** ("we"): same as the slide voice
- **Plainspoken**: no consultant filler ("elevate," "leverage," "unlock," "seamless")
- **Specific**: name the company, the number, the date. Anchor every claim.
- **Conversational**: would feel natural said aloud at a normal pace

## Generation pattern

For each slide in `10_Content.md`:

1. Read the slide's title, body, and any other content slots
2. Identify the slide's purpose from the arc position (opener, context-setter, evidence, ask, etc.)
3. Draft notes following the 3-part structure above
4. Apply `consulting:consulting-humanize` to strip any AI tells
5. Apply `brand:brand-voice-tone` to ensure register matches brand voice

## Per-template guidance

| Template | Notes emphasis |
|---|---|
| **01-title** | The mission/objective for this conversation. Why we're here. One sentence on what success looks like for the next 30/60 min. |
| **02-section-divider** | What this section is going to do. Transition from the last section. |
| **03-agenda** | Time check. Acknowledge what we'll cover and what we won't. |
| **04-thank-you** | Restate the ask. Offer a clear next step. |
| **06-one-col-text** | The body is dense — notes guide the presenter to which parts to lean on vs. which to skip if pressed for time. |
| **12-pull-quote** | Context for the quote. Who said it, when, why it lands. The quote itself doesn't need re-reading. |
| **13-big-stat** | The stat is the slide. Notes explain what it means and what it doesn't mean. |
| **17-framework-2x2** | Walk the four quadrants briefly. Land on where the recommendation falls and why. |
| **22-case-study** | Tell the story. What changed. What the team did. What the stats represent. |
| **23-pricing** | Frame the investment as a function of the outcome. Anticipate the pushback. |
| **24-roadmap** | Where you are. Where you're going. The risks at each phase. |

## Output

Inline into `10_Content.md` — each slide block ends with a `**Speaker notes**:` block.

## Anti-patterns

- **Don't** restate the title in the notes
- **Don't** use bullet lists in notes — sentences only (presenters glance, they don't read)
- **Don't** write notes longer than 150 words — if it needs that much, the slide is doing too little
- **Don't** include "say:" or "remember to:" — write declarative notes the presenter just absorbs
- **Don't** use jargon the audience won't recognize without the slide visible
