---
name: brand-identity-system
description: >
  Author the visual identity system for a brand — logo, wordmark, marks, color systems, typography, photography direction, and iconography style as one coherent authoring skill. Direct (not execute) logo and mark design; specify color palettes with semantic naming and contrast pairings; build type systems that pair correctly and license correctly; ensure dark-mode and accessibility compliance from the start. Inherits the named bans from design-taste (LILA, Inter, pure-black, two-warring-primaries, mixed-stroke icons, geometric circle logo). Use this skill any time a brand identity is being designed, refreshed, or applied.

# Project context
type: skill
project: skills-library
plugin: brand
aliases: [brand-identity-system, brand-identity, visual-identity, identity-system]
tags: [type/skill, plugin/brand, topic/brand, topic/identity, topic/visual-design]
status: active
---

# Brand Identity System

This skill is the visual-identity authoring layer of brand work. Logo direction, wordmark, color system, type system, photography direction, and iconography style — all as one coherent system. The output is a complete identity that holds across surfaces and time.

You usually aren't drawing the logo yourself; you're directing it, evaluating it, or briefing a designer. Same for type and photography. The job is to make the visual decisions strategic and coherent — not "I like this one."

Pair with [[brand-strategist]] (positioning that informs identity), [[brand-naming]] (the wordmark's source), [[brand-voice-tone]] (verbal register that visual register must match), [[design-taste]] (named visual-tells the identity must avoid), [[stylistic-vocabulary]] (the register family the identity lives in).

## When to use this skill

- Designing a new brand identity from scratch.
- Refreshing an existing brand.
- Evaluating logo concepts (concepts, refinements, "is this on the right track").
- Building a color palette grounded in brand strategy.
- Designing a type system with proper licensing for the brand's actual use cases.
- Specifying photography direction and iconography style.
- Auditing an existing identity for consistency or evolving it.
- Applying an existing brand to specific touchpoints.

## What this skill is NOT

- Not the naming skill (see [[brand-naming]]).
- Not the voice/tone skill (see [[brand-voice-tone]]).
- Not the brand strategy skill (see [[brand-strategist]]).
- Not the design-execution skill (logos and color palettes are *briefed*; designers execute).
- Not the production-system skill — that's [[design-screen]], [[design-presentation]], [[design-document]] for surface-specific application.

## Logo and mark direction

A strong mark is:

- **Simple.** Works at favicon size (16×16).
- **Distinctive.** Not a stock idea. Avoid generic swooshes, lightbulb-as-idea, globe-with-arrow tropes. (The "geometric circle logo" ban — see [[ai-tells-forbidden-patterns]].)
- **Memorable.** Recognizable after one exposure.
- **Scalable.** Works monochrome, on a phone, on a billboard.
- **Appropriate.** Fits the audience and category. A founder-facing legal product is not a meme brand.
- **Ownable.** Trademark-clearable at screening, distinct from competitors in the category.

### Logo brief template

```
# Logo brief: {Brand}

## Strategic context
- What the brand stands for (3 words):
- Who it serves:
- What it needs to feel (3 adjectives):
- What it must not feel (2 adjectives):
- Competitors to differentiate from:
- Stylistic register from `stylistic-vocabulary`:

## Constraints
- Required formats: SVG (primary), PNG, ICO
- Required variants: full color, monochrome, single-color, knockout (for dark backgrounds)
- Required minimum size: 16px favicon
- Color palette: directional or fixed?
- Type pairing: directional or fixed?

## Concept directions (2–4)
For each:
- Direction name
- One-sentence rationale
- What it earns the brand
- What it risks
- Reference visual cue

## Anti-bland gates
- No generic swooshes.
- No globe-with-arrow.
- No lightbulb-as-idea.
- No abstract-geometric-circle-on-pastel-gradient.
- No "AI Purple/Blue" gradient backgrounds.
```

### Evaluating a logo

When reviewing concepts, comment on each axis. Avoid taste-based feedback ("I prefer the second one"). Tie comments to the brief.

| Axis | Question |
|---|---|
| **Strategic fit** | Does it match the brief's three adjectives? |
| **Distinctiveness** | How does it stand next to competitors? |
| **Versatility** | Does it scale, work monochrome, on dark, on light? |
| **Construction** | Type, geometry, balance, kerning. |
| **Risks** | Legibility at small sizes, similarity to known marks, cultural read. |
| **What I want next** | Specific direction for the next round. |

## Color systems

A brand color system has four jobs:

1. **Identity** — recognizable. The brand owns specific colors.
2. **Function** — UI states, data encoding, status communication.
3. **Accessibility** — contrast pairings for readability.
4. **Atmosphere** — the emotional register.

### Building a palette

```
Primary (1–2 colors):
  - The colors the brand owns.
  - High impact, used sparingly.

Secondary / supporting (2–4):
  - Expand expressive range.
  - Used in editorial moments, accent treatments.

Neutrals (4–6):
  - Grays from near-white to near-black.
  - Often slightly tinted (warm or cool).

Functional (4):
  - Success, warning, error, info.
  - Each accessible against neutrals.

Surface and text:
  - Derived from neutrals.
  - Named for role, not hue.
```

### Per-color spec

For each color, document:

- **Name (semantic, not literal):** `accent` not `blue-500`. `ink` not `near-black`. `paper` not `off-white`.
- **HEX, RGB, HSL.**
- **HSL channel form** (`220 90% 56%`) for CSS variables (shadcn/Tailwind format).
- **WCAG contrast pairings:** which text colors meet AA / AAA on this background.
- **Usage rules:** where it can and cannot be used.

### Named bans (from [[design-taste]])

- **The LILA ban** — no AI purple/blue gradients. No oversaturated indigo glows.
- **Pure-black ban** — no `#000000`. Use Off-Black, Zinc-950, Charcoal.
- **Two-warring-primaries ban** — one primary owns the moments. Other primaries become functional.
- **Oversaturated-accents ban** — desaturate accents to <60% saturation; blend with neutrals.

### Common color failures

- Two primary colors that fight for attention (no clear hierarchy).
- A palette that fails AA contrast on the brand's own UI.
- Colors named after their hue, then later changed (debt forever).
- Too many neutrals; team can't decide which gray to use.
- No dark-mode strategy at the start; bolted on later.

For digital, prefer **HSL channel triplets** (`220 90% 56%`) for source values, since shadcn/ui and most design-token systems wrap them.

## Typography systems

Type does more brand work than color. It is also where most brand systems quietly fail — wrong license tier, too many weights, fonts that fight each other.

### Building a type system

```
Display: for headlines and brand moments.
  - Distinctive, can be expensive.
  - Often the brand's signature face.

Text: body and UI.
  - Must be highly legible at 14–18px.
  - Often a workhorse face that can do everything.

Mono (optional): code, data, numerals.
  - For technical brands or data-heavy applications.
```

### Per-face spec

For each typeface, document:

- **Family** (the font name).
- **License:** web, app, marketing, embedded — verify each. **License the use cases the brand actually has, not just the cheapest tier.**
- **Weights used:** typically 3–4 (e.g., Regular, Medium, Semibold, Bold). Don't include weights you won't use — performance penalty.
- **Type scale:** modular scale or custom; document px and line-height per step.
- **Pairing rules:** which face to use where.
- **Fallback stack** for digital surfaces.

### Pair criteria

- **Contrast:** a sturdy text face with a more characterful display face works almost always.
- **Era / origin compatibility:** don't pair two faces that were both designed to be characterful — they fight.
- **Practical license cost** across the brand's likely surfaces.

### Named bans

- **The Inter ban** for "premium" or "creative" briefs — use Geist, Outfit, Cabinet Grotesk, Satoshi, Söhne, Migra, GT Walsheim. Inter is fine for pure utility UI.
- **Serif on dashboards** — banned. Editorial yes; cockpit no.
- **Oversized H1s** — control hierarchy with weight and color, not just massive scale.
- **Modular scale required** — no arbitrary 15/17/19; use a documented scale.

### Common type failures

- Picking a beautiful display face with a free license; finding out later it isn't licensed for the app.
- Too many weights; performance penalty and inconsistent use.
- A display face that does not have a usable italic or bold.
- Pairing two trendy faces; the brand looks like every brand.

## Photography and illustration

Photography and illustration extend the brand's visual register beyond logo/color/type.

### Direction (what to specify)

- **Subject category:** people in environment / objects / abstract / mixed.
- **Lighting signature:** golden hour / overcast / studio-soft / dramatic.
- **Composition signature:** wide / medium / tight; centered / asymmetric.
- **Palette:** colors that thread through all photography — usually pulling from the brand color system.
- **Treatment:** color / black-and-white / duotone / muted color.
- **Style:** documentary / commercial / editorial / illustrative.

For full art-direction language, see [[design-imagery]] in the design plugin.

### Stock vs. commissioned vs. generated

For brand identity work specifically:
- **Commission flagship photography** — the brand's "who we are" hero imagery.
- **Generate (higgsfield)** for supporting imagery when commissioning isn't budgeted.
- **Stock only as last resort** — and curated heavily, never default Unsplash.

## Iconography style

Pick one icon family for the brand. Hold it across all surfaces.

For brand-side icon work, see [[design-iconography]] in the design plugin. Decision tree:

- **Product UI:** Lucide (shadcn default), Phosphor (variation), Tabler (breadth).
- **Decks, documents, social:** Flaticon (with discipline) or commissioned.
- **Brand-statement (icon set as identity element):** commissioned.

Specify in the brand identity system:
- **Family.**
- **Stroke weight** (1.5px or 2px).
- **Fill-vs-outline rule.**
- **Sizing scale.**
- **Color treatment.**

## Brand identity authoring workflow

When asked to develop a brand identity:

1. **Read the strategic context.** [[brand-strategist]] output — positioning, audience, competitors. If missing, generate the strategy first.
2. **Read the verbal register.** [[brand-voice-tone]] output — voice attributes, tone register. Visual register must match.
3. **Pick a stylistic register** from [[stylistic-vocabulary]] (premium-restrained / swiss-modern / editorial / brutalist / etc.).
4. **Direct the logo.** Brief, evaluate concepts, recommend.
5. **Build the color system.** Primary, secondary, neutrals, functional. Named semantically. Contrast-paired. Dark-mode planned.
6. **Build the type system.** Display, text, mono. Licensed. Modular scale.
7. **Specify photography direction.**
8. **Specify iconography style.**
9. **Document everything** for [[brand-guidelines-composer]] to assemble.

## How to engage

Ask:
- Is there an existing brand strategy from [[brand-strategist]]?
- Is this a new identity, refresh, or extension?
- What's the stylistic register from [[stylistic-vocabulary]]?
- What surfaces need to apply this identity (web, app, deck, document, social, environmental)?
- What's the timeline?
- Is there a budget for commissioned photography? For commissioned illustration?
- Are display fonts licensed? Or do we need to license?
- Dark-mode required?
- Accessibility level (WCAG AA minimum, AAA aspirational)?

## Common identity-system failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Geometric circle logo** | Abstract circle/swoosh that could belong to any tech company | Brief grounded in the brand's actual story; reject generic shapes |
| **AI Purple/Blue palette** | Indigo gradient backgrounds, neon glows | Desaturated neutrals, one accent at <60% saturation |
| **Two warring primaries** | Two equally weighted "primary" colors fighting | One primary; others become functional |
| **Inter as premium signal** | Inter on a luxury/creative brief | Geist, Cabinet Grotesk, Söhne, Migra |
| **Too many fonts** | Display + text + accent + display-2 + script | Three faces max (display, text, mono optional) |
| **Unlicensed display font** | Beautiful free font that turns out unlicensed for app embed | License every face for every use case before committing |
| **No dark-mode strategy** | Bolted-on dark mode that fails contrast | Plan dark mode from start; test contrast in both modes |
| **Five-color brand palette** | Five equally weighted colors with no hierarchy | Two-color foundational palette + functional + neutrals |
| **Personality-trait grid in guidelines** | "Bold. Friendly. Trustworthy. Modern." in stock-photo boxes | Voice attributes prove themselves in their own writing |
| **Logo too detailed for favicon** | Logo unreadable at 16×16 | Test at 16×16 every concept |
| **Mixed-stroke icons across surfaces** | UI uses 1.5px Lucide, deck uses 2px Phosphor, doc uses Flaticon outline | One family, one stroke weight, holdable across surfaces |

## Hand-offs

- **[[brand-strategist]]** — for positioning context.
- **[[brand-naming]]** — for the wordmark source.
- **[[brand-voice-tone]]** — for verbal register matching.
- **[[brand-guidelines-composer]]** — for compiling the identity into a guidelines document.
- **[[design-iconography]]** — for icon family selection and discipline.
- **[[design-imagery]]** — for photography direction.
- **`software-engineering-tailwind-tokens`** — for token implementation in code.
- **`software-engineering-shadcn-component`** — for shadcn theme customization.
- **External design execution** — final logo design, custom typeface design, illustration commissions go to external designers; this skill briefs them.

## Constraints

- Don't pick fonts you have not licensed for the brand's actual use cases.
- Don't promise trademark clearance for logo concepts. Refer to legal counsel.
- Don't produce taste-based logo critique. Tie all feedback to the brief.
- Don't assume Western reading order, color symbolism, or naming conventions for global brands.
- Don't ship an identity without a dark-mode strategy.
- Don't use color as the only encoding — pair with shape, position, or label.
- For Slalom Composable DXP work: default register is **premium-restrained** for executive-audience brands, **editorial-tech** for technical brands.
