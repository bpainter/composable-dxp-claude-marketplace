---
name: design-taste
description: >
  The anti-bland foundation for every design task. Three calibration dials (variance, motion, density), an intent-first protocol that forces specific answers before any pixel, named AI default patterns to actively reject (the LILA ban, Inter ban, Jane Doe effect, 3-column-card ban, filler-words ban, and more), and four pre-show checks (swap, squint, signature, token). Loaded by every other design execution skill in this plugin. Use this skill any time the user is producing visual design work — screen, deck, document, social asset, identity application — even when they don't say "taste." Trigger whenever you're about to commit to a stylistic direction so the work emerges from intent and constraint rather than the statistical center of AI training.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-taste]
tags: [type/skill, plugin/design, topic/design, topic/taste]
status: active
---

# Design Taste

This skill exists because LLM-generated design defaults to its statistical center. Inter font, AI purple, three equal cards, centered hero, generic placeholder names, "elevate your workflow." All of it predictable, all of it forgettable. This skill is the override layer — three dials that calibrate the work to the brief, an intent-first protocol that forces specific answers before generation, a named list of AI tells to actively reject, and four checks before showing the user.

Load this skill at the start of any visual design task. It does not replace `design-process`, `visual-design-foundations`, or `interaction-patterns` — it sits on top of them and prevents the most common failure mode in AI-assisted design: bland output that looks like every other AI-assisted design.

## When to use this skill

- Starting any new visual design work — screen, deck, document, social asset.
- Reviewing a design that "feels generic but you can't say why."
- Picking a color palette, typography pairing, or stylistic direction.
- Pushing back on a first draft that defaulted to the AI center.
- Pairing with `design-presentation`, `design-document`, `design-social-asset`, `design-screen`, or any execution skill — taste loads first.

## The three dials

Every design task gets calibrated on three axes. The defaults below are the *Slalom Composable DXP house* baseline. Adjust per project brief; the user can override any dial in their prompt.

| Dial | Default | What it controls |
|---|---|---|
| **DESIGN_VARIANCE** | 6 | How experimental the layout is. 1 = perfect symmetry, centered grids. 10 = asymmetric, masonry, fractional grids, large empty zones. |
| **MOTION_INTENSITY** | 4 | How much animation. 1 = no movement, hover-only. 10 = scroll-triggered choreography, magnetic physics. |
| **VISUAL_DENSITY** | 5 | How packed the surface is. 1 = art gallery, huge whitespace. 10 = pilot cockpit, dense data. |

Slalom defaults skew slightly toward variance (we're a digital practice — not a bank), motion-light (consulting work has to read clearly in static screenshots), and mid-density (we present to executives — not data analysts).

### Dial reference

**DESIGN_VARIANCE**
- 1–3: predictable. `justify-center`, strict 12-column grids, equal paddings.
- 4–7: offset. Negative-margin overlapping, varied aspect ratios, asymmetric headers.
- 8–10: asymmetric. Masonry, fractional CSS Grid, large empty zones (`padding-left: 20vw`).
- **Mobile override:** any layout above level 4 must collapse cleanly to a single-column mobile layout. No horizontal scroll.

**MOTION_INTENSITY**
- 1–3: static. No automatic animations, only `:hover` and `:active`.
- 4–7: fluid CSS. `transition: transform 250ms ease-out`, animation-delay cascades, `transform` and `opacity` only.
- 8–10: choreographed. Scroll-triggered reveals, parallax, Framer Motion hooks, GSAP for scrolltelling.

**VISUAL_DENSITY**
- 1–3: art gallery. Huge section gaps, one element per screen, expensive feel.
- 4–7: daily app. Normal spacing.
- 8–10: cockpit. Tiny paddings, no card containers, 1px lines for separation, monospace numerals.

## Intent-first protocol

**Before producing any pixel, deck slide, document spread, or social tile, answer these three questions out loud — in writing, in the file, or in the prompt response.** Not in your head.

### 1. Who is this human?

Not "users." Not "stakeholders." Not "the audience." A specific person. Where are they when they encounter this? What's on their mind? What did they do five minutes before, what will they do five minutes after?

> **Bad:** "Senior decision-makers at enterprise companies."
> **Good:** "A CIO at a $4B retailer reading this on a delayed flight at 11pm, three days before her board meeting where she has to defend a $40M digital investment."

### 2. What must they accomplish?

Not "use the dashboard." Not "understand the offering." A verb the person will perform. The answer determines what leads, what follows, what hides.

> **Bad:** "Learn about composable DXP."
> **Good:** "Decide whether to short-list us for the architecture phase before her 3pm calendar block ends."

### 3. What should this feel like?

Say it in words that mean something. *Clean and modern* means nothing — every AI says it. Reach for words that constrain.

> **Bad:** "Professional, modern, clean."
> **Good:** "Confident like a clinical study, restrained like a Swiss editorial, breathing like a museum wall text."

If you can't answer all three with specifics, **stop**. Ask the user. Do not guess. Do not default.

### Intent must be systemic

Saying "warm" and using cold colors is not following through. Intent is a constraint that shapes every decision. If the intent is *warm*, the surfaces, text, borders, accents, semantic colors, and typography are all warm. If the intent is *dense*, spacing, type size, and information architecture are all dense. Check the output against the stated intent: does every choice reinforce it?

## Required outputs before proposing a direction

After answering the three intent questions and **before proposing any visual direction**, produce four things:

1. **Domain.** Five or more concepts, metaphors, vocabulary from this product's actual world. Not features — territory. For a retail CIO deck: not "buy box, cart, checkout" but "back-of-house, replenishment, planogram, shrink, peak day."
2. **Color world.** Five or more colors that exist naturally in this domain. If this product were a physical space, what would you see? What materials, what light? List them. Not "warm and cool" — actual colors that *belong*.
3. **Signature.** One element — visual, structural, or interaction — that could only exist for THIS audience. If you can't name one, keep exploring.
4. **Defaults to reject.** Three obvious choices for this surface type that you are *not* going to make. You can't avoid patterns you haven't named.

The user reads your direction. Could they identify what this is for if you removed the project name? If not, it's generic. Explore deeper.

## AI tells — named patterns to reject

Default LLM design output has signatures. Reject these unless the brief explicitly calls for them. The bans are named so we can cite them.

For the surface-extended list, see `references/ai-tells-forbidden-patterns.md`.

### Color
- **The LILA ban.** AI purple/blue gradients, neon glows, oversaturated accents. Use desaturated neutrals (Zinc, Slate, Stone) with one high-contrast singular accent (Emerald, Electric Blue, Deep Rose, Burnt Orange).
- **Pure black banned.** No `#000000`. Use Zinc-950, Charcoal, or Off-Black.
- **No gradient text on headers.** Save gradient text for one moment per surface, max.
- **No multiple primary colors fighting.** One accent earns its place.

### Typography
- **The Inter ban.** Banned for "premium" or "creative" briefs. Reach for Geist, Outfit, Cabinet Grotesk, Satoshi, Söhne, Neue Haas Grotesk, or a quality serif for editorial work. Inter is fine for pure utility UI; it's banned as a brand statement.
- **Serif on dashboards banned.** Editorial yes; cockpit no.
- **No oversized H1s.** Don't scream. Hierarchy is built from weight, color, and surrounding space — not just size.
- **Modular type scale.** No arbitrary 15px / 17px / 19px. Use a scale (12, 14, 16, 18, 24, 32, 48) or a modular ratio (1.25× or 1.333×).

### Layout
- **Centered-hero ban above DESIGN_VARIANCE 4.** No centered headline-over-image-with-button. Force split-screen, asymmetric, left-aligned content with right-aligned asset.
- **The 3-column card-row ban.** Generic "three equal feature cards in a row" is forbidden. Use 2-column zig-zag, asymmetric grid, bento, or horizontal scroll instead.
- **Anti-card-overuse.** Above VISUAL_DENSITY 7, drop card containers. Use `border-t`, `divide-y`, or pure negative space for grouping. Cards earn their place when elevation communicates real hierarchy.

### Content and data
- **The Jane Doe effect.** Banned: "John Doe," "Sarah Chan," "Jack Su," "Acme Corp," "Nexus," "SmartFlow." Use realistic, contextually believable names. For Slalom work, use brands and people that actually exist in the deck context if possible.
- **Fake-numbers ban.** No `99.99%`, `50%`, `1234567`. Use organic data (`47.2%`, `+1 (312) 847-1928`, `$3.4M ARR`).
- **Filler-words ban.** Cut: "elevate," "seamless," "unleash," "next-gen," "leverage," "synergy," "innovative," "best-in-class," "unlock," "empower." Replace with concrete verbs that name what actually happens.
- **No generic avatars.** SVG egg icons banned. Lucide user icons banned for placeholder avatars. Use real, contextually believable photo placeholders or specific styling.

### Stock and placeholders
- **No Unsplash defaults.** The "AI Unsplash aesthetic" (back-of-someone's-laptop, hands-on-keyboard, abstract-tech-blob) is banned. If you must placeholder, use `https://picsum.photos/seed/{specific-string}/W/H` or commission via higgsfield (see `references/tool-higgsfield.md`).
- **shadcn defaults banned.** Using shadcn/ui in default state — default radius, default neutrals, default shadows — is banned. Always customize radii, colors, and shadow stack to match the brief.

### Surface-specific tells

**Decks (16:9):**
- No "agenda → context → solution → benefits → next steps" template.
- No three-column "what we'll cover" slides.
- No "questions?" closer slide. The closer says something.

**Documents (8.5×11):**
- No "executive summary" that summarizes the entire document. The exec summary makes a claim.
- No headshot grid on the about page.
- No "our approach" slide-as-document with five identical pillar boxes.

**Social (LinkedIn / Instagram):**
- No "lessons learned" carousel with white text on a teal box.
- No "10 things you didn't know" listicle structure unless the list is genuinely surprising.
- No quote-card-with-headshot when the quote isn't memorable.

## Subtle layering

This is the backbone of craft regardless of style direction. You should barely notice the system working. When the user looks at a Vercel dashboard, they don't think "nice borders" — they just understand the structure. Craft is invisible.

### Surface elevation
Surfaces stack. A dropdown sits above a card which sits above the page. Build a numbered scale — base, then increasing levels. In dark mode, higher elevation = slightly lighter. In light mode, higher elevation = slightly lighter (or uses subtle shadow).

Each jump is only a few percentage points of lightness. You can barely see the difference in isolation. When surfaces stack, the hierarchy emerges. Whisper-quiet shifts.

- **Sidebars:** same background as canvas, not different. Different colors fragment the visual space. A subtle border is enough separation.
- **Dropdowns:** one level above their parent. If both share the same level, the dropdown blends in.
- **Inputs:** slightly *darker* than surroundings, not lighter. Inputs are inset; they receive content.

### Borders that disappear

Borders should disappear when you're not looking for them and be findable when you need structure. Low-opacity rgba blends with the background — it defines edges without demanding attention. Solid hex borders look harsh in comparison.

Build a progression: standard borders, softer separation, emphasis borders, focus rings. Match intensity to importance.

### "Color lives somewhere"

Every product, every audience, every brief lives in a world. That world has colors. Before reaching for a palette, spend time in the brief's world. What would you see if you walked into the physical version of this space? What materials, what light, what objects?

Your palette should feel like it came FROM somewhere — not like it was applied TO something.

## The four checks (run before showing the user)

Before presenting any output, run these. If a check fails, iterate before showing.

1. **The swap test.** If you swapped your typeface for the AI default (Inter), would anyone notice? If you swapped the layout for a standard centered-hero template, would it feel different? The places where swapping wouldn't matter are the places you defaulted.

2. **The squint test.** Blur your eyes. Can you still perceive hierarchy? Is anything jumping out harshly? Craft whispers. If something shouts, it's probably wrong.

3. **The signature test.** Can you point to five specific elements where your direction's signature appears? Not "the overall feel" — actual components, type choices, color moves, layout decisions. A signature you can't locate doesn't exist.

4. **The token test.** Read your tokens out loud — color names, type styles, spacing scale labels. Do they sound like they belong to this brief's world, or could they belong to any project? `--ink` and `--parchment` evoke a world. `--gray-700` and `--surface-2` evoke a template.

## Before producing each artifact

State this at the top of your output (in a comment block, in the prompt response, in the design file metadata). It's the audit trail.

```
Intent: [who is this human, what must they accomplish, how should it feel]
Domain: [5+ concepts from this brief's world]
Color world: [5+ colors that exist in this domain]
Signature: [one element unique to this audience]
Rejecting: [default 1] → [alternative], [default 2] → [alternative], [default 3] → [alternative]
Dials: variance [N] / motion [N] / density [N]
```

This is not ceremony. It's the connection between the brief and the output. If you can't fill this in, you're defaulting.

## Sameness is failure

If another AI-assisted designer, given a similar prompt, would produce substantially the same output — you have failed. This is not about being different for the sake of it. It's about the design emerging from the specific brief, the specific audience, the specific context. When you design from intent, sameness becomes impossible because no two intents are identical. When you design from defaults, everything looks the same because defaults are shared.

## How this skill relates to others

- **Always-on design discipline (the workflow side):** [[design-process]] — load alongside this skill.
- **Static visual fundamentals:** [[visual-design-foundations]] in `design/references/` — the *what readers see first* layer.
- **Behavior and motion fundamentals:** [[interaction-patterns]] in `design/references/` — the dynamic layer.
- **Surface-extended forbidden patterns:** [[ai-tells-forbidden-patterns]] in `design/references/`.
- **Stylistic vocabulary catalog:** [[stylistic-vocabulary]] in `design/references/`.
- **Pre-delivery checks:** [[pre-delivery-checklist]] in `design/references/`.
- **Surface-specific execution:** `design-presentation` (16:9), `design-document` (8.5×11), `design-social-asset` (LinkedIn / Instagram), `design-screen` (product UI).
- **Brand identity application:** [[brand-strategist]] and [[brand-designer]] (the latter being split — see `Brand_and_Design_Expansion_Plan.md`).
- **Front-end implementation:** `software-engineering-design-implementation`, `software-engineering-frontend-developer`, `software-engineering-shadcn-component`.

## Constraints

- Do not skip the intent-first protocol because "the brief seems clear." The brief is never clear enough to override your defaults without the three questions.
- Do not soften the named bans into suggestions. The bans are non-negotiable defaults. Override them only with explicit user direction.
- Do not produce shadcn/ui in default state. Customize.
- Do not invent fake names, fake companies, or fake numbers. Either use real ones (with permission) or use labels that announce their placeholder status (`{Client Name}`, `{Industry}`, `{$Revenue}`).
- Do not run the four checks performatively after the work is done. They're a gate before showing — if they fail, iterate.

## Credits

The dials, the AI Tells naming, and the Bento-2.0 motion paradigm are adapted from [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) under MIT license — sharpened for our consulting context (less frontend-specific, more surface-portable).

The intent-first protocol, the four required outputs, the four checks, and the "where defaults hide" framing are adapted from [Dammyjay93/interface-design](https://github.com/Dammyjay93/interface-design) — extended to non-product-UI surfaces.

The named-rules architecture (priority categorization, citation-backed UX rules) is influenced by [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill); the rule content lives in `references/pre-delivery-checklist.md`.

The skill-vs-command split owes to [Owl-Listener/designer-skills](https://github.com/Owl-Listener/designer-skills).

Slalom-house defaults (variance 6 / motion 4 / density 5; no consultant filler vocabulary; one-accent palette) are ours.
