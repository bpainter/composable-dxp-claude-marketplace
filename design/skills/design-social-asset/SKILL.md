---
name: design-social-asset
description: >
  Design social assets for LinkedIn and Instagram — single posts, carousels, story templates, link previews. Owns the format-specific composition (1200×627, 1200×1200, 1080×1080, 1080×1350, 1080×1920) plus brand consistency across a series. Scroll-stopping composition over template clichés. Use this skill any time the user is making LinkedIn or Instagram content — single image post, carousel, story, link preview, ad creative — even when they don't say "design the asset." Trigger before opening Canva or Figma so the asset has system thinking before pixels.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-social-asset, social-design, linkedin-design, instagram-design]
tags: [type/skill, plugin/design, topic/design, topic/social, topic/marketing]
status: active
---

# Design Social Asset

This skill owns LinkedIn and Instagram surface design. Deliverables are PNG/JPG images at platform-native aspect ratios, plus carousel sets where multiple images compose a single post.

The work is small but the rules are tight: scroll-stopping composition, thumb-stopping in 0.4 seconds, brand consistency across a series, type readable at thumbnail size. Most "we need a LinkedIn post" defaults to teal-quote-card or hands-on-keyboard imagery. Reject those (see [[ai-tells-forbidden-patterns]]).

Pair with [[design-taste]] (load first), [[design-iconography]] (when icons are involved), [[design-imagery]] (when the asset needs commissioned/generated imagery).

## When to use this skill

- Single LinkedIn post (image accompanying text post).
- LinkedIn link preview (when the post includes a URL).
- LinkedIn carousel (multi-page swipeable post — high engagement).
- Instagram single post (image with caption).
- Instagram carousel.
- Instagram story (vertical, 24-hour ephemeral).
- LinkedIn or Instagram ad creative (paid).
- Series/campaign work — multiple posts that share a system.

## What this skill is NOT

- Not the writing skill. Caption text and hooks are upstream.
- Not the brand-identity skill (see brand plugin).
- Not the imagery generation skill (see [[design-imagery]] for higgsfield workflows).
- Not the campaign-strategy skill (when to post, A/B testing, etc. — see marketing plugin).

## The surface specs (memorize these)

### LinkedIn

| Surface | Dimensions | Aspect | Use |
|---|---|---|---|
| **Single image post** | 1200×627 | 1.91:1 | Default image post. Native LinkedIn UI optimal size. |
| **Single image post (square)** | 1200×1200 | 1:1 | Higher engagement than 1.91:1. Often preferred. |
| **Carousel page** | 1080×1080 | 1:1 | Each page of a swipeable carousel. |
| **Carousel page (vertical)** | 1080×1350 | 4:5 | Vertical carousel pages — higher reach in feed. |
| **Link preview** | 1200×627 | 1.91:1 | Auto-generated when URL is in post; designed when controlled. |
| **Article header** | 1280×720 | 16:9 | LinkedIn article cover. |
| **Profile cover** | 1584×396 | 4:1 | Personal profile background. |
| **Company page cover** | 1192×220 | 5.4:1 | Company page background. |

### Instagram

| Surface | Dimensions | Aspect | Use |
|---|---|---|---|
| **Single post (square)** | 1080×1080 | 1:1 | Classic Instagram post. |
| **Single post (portrait)** | 1080×1350 | 4:5 | Vertical post — takes more feed real estate, higher engagement. |
| **Single post (landscape)** | 1080×566 | 1.91:1 | Landscape post. Less engagement than square or portrait. |
| **Carousel page** | 1080×1080 or 1080×1350 | 1:1 or 4:5 | Multi-page swipeable post. Match all pages to one aspect. |
| **Story / Reels** | 1080×1920 | 9:16 | Vertical full-screen, 24-hour ephemeral. |

For most B2B Slalom Composable DXP work, the relevant surfaces are:
- **LinkedIn 1200×1200 single post** (highest engagement for B2B).
- **LinkedIn carousel at 1080×1350** (vertical carousels outperform square in 2025+).
- **LinkedIn link preview at 1200×627** (when distributing whitepapers, articles).

Instagram is rare for our work but might apply for employer-brand content or events.

## The asset system (build first for any series)

A one-off post is ad-hoc. A series (3+ related posts) earns a system.

### 1. Aspect ratio committed
Pick one (or two) per campaign. Don't mix square + vertical + landscape across a series.

### 2. Type system
Type at thumbnail size is the constraint:
- **Headline:** must read at ~250×250px (Instagram thumbnail). Test this.
- **Body:** must read at full feed size (~600px wide).
- **Captions / disclaimers:** can be smaller, but legible.

For 1080×1080 LinkedIn carousel:

| Role | Size | Weight |
|---|---|---|
| Hero headline | 80–120pt | Bold/Black |
| Sub-headline | 40–60pt | Semibold |
| Body | 28–36pt | Regular/Medium |
| Caption / source | 18–24pt | Regular |
| CTA / button | 28–36pt | Semibold |

Bigger than a typical web. Social is read-while-scrolling — type has to win in 0.4 seconds.

### 3. Color system
Brand-consistent across the series. Don't change palette per post.
- Background: usually one tone (light or dark) per series.
- Foreground type: high-contrast against background.
- Accent: one color used sparingly for emphasis.

### 4. Composition rules

**Scroll-stopping discipline:**
- **First fold (top 40% of asset):** hook lives here. The big claim, the surprising number, the question.
- **Second fold (next 40%):** support — body, evidence, context.
- **Third fold (bottom 20%):** brand mark, attribution, CTA, swipe indicator.

**The 0.4-second test:** can the first fold be understood in 0.4 seconds at thumbnail size? If not, rework.

### 5. Carousel-specific rules

Carousels are the highest-engagement format on LinkedIn. They earn that engagement only when:

- **Page 1 (cover) earns the swipe.** A specific question, a surprising claim, a "you'll be surprised by [N]." Generic "5 lessons" headlines don't.
- **Pages 2 to N-1 deliver one idea each.** Don't cram.
- **Last page commits.** Specific CTA, specific link, specific question. Not "follow for more."
- **Visual consistency** — same type system, same color system, same layout grid across all pages.
- **Page numbers** in the footer (1/7, 2/7, etc.) so readers know where they are.

**Length:** 5–10 pages optimal for LinkedIn (algorithm favors 7+). Below 5 reads as thin; above 10 reads as bait.

### 6. Brand mark placement

Every asset has the brand mark — Slalom or sub-brand wordmark/logo:
- **Single post:** bottom-right or bottom-left corner. ~5% of asset width.
- **Carousel:** mark on every page. Bottom-right is conventional.
- **Story:** bottom-center, accounting for swipe-up gesture overlay.

Don't make the mark dominant. Make it discoverable.

## How to engage

When asked to design a social asset:

1. **Calibrate** ([[design-taste]]): single asset or series? Audience? Stylistic register? Default for Slalom B2B is **premium-restrained** or **editorial-tech**, not minimal-soft.
2. **Confirm the surface and aspect ratio** before any composition.
3. **Build the system** if it's a series (≥ 3 posts). Document `asset-system.md` in the WIP folder.
4. **Compose** following the system.
5. **Run the four checks** ([[design-taste]] — swap, squint, signature, token) plus the **0.4-second test**.
6. **Pre-delivery checklist** ([[pre-delivery-checklist]]) — contrast at thumbnail, type readable at thumbnail, brand mark present and proportional, alt text written for the post.
7. **Hand off** the file plus alt text plus suggested caption-text-for-post (the actual social post copy is upstream, but the visual designer often supplies suggested hooks).

## Asset-system file (for series)

```
# Asset system: {Campaign name}

## Audience and intent
- Audience: {LinkedIn B2B / Instagram consumer / both}
- Intent: {what we want them to do — click link, save, share, comment}
- Stylistic register: {premium-restrained / editorial-tech / minimal-soft}

## Surface
- Platform(s): {LinkedIn / Instagram / both}
- Aspect ratio(s): {1:1 single / 4:5 carousel / 9:16 story}
- Page count (if carousel): {N pages}

## Type system
- Hero: {font} 100pt Bold
- Sub: {font} 50pt Semibold
- Body: {font} 32pt Regular
- Caption: {font} 22pt
- CTA: {font} 32pt Semibold

## Color system
- Background: {hex}
- Foreground: {hex}
- Accent: {hex}
- Logo treatment: {white-knockout / on-brand / mono}

## Layout grid
- Margins: 80px on all sides at 1080×1080
- Title zone: top 30%
- Body zone: middle 50%
- Footer / brand zone: bottom 20%

## Series consistency rules
- All assets share: {type system / color / margins / brand mark placement}
- Variation across assets: {only the headline + accent visual}
```

## Surface-specific cross-references

For LinkedIn-specific specs and engagement patterns: [[surface-social-linkedin]].
For Instagram-specific specs: [[surface-social-instagram]].

## Common social-asset failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Teal quote card** | White text on teal box, headshot corner, logo bottom | Type-driven moment; drop the box |
| **"10 things you didn't know"** | Listicle headlines that bait without delivering | Genuine, specific, surprising claims |
| **"Lessons learned"** | Generic platitude carousels | Specific lessons that name names and dates |
| **Pointing-at-camera headshot** | Stock-pose author photo | Real-context photo or no author photo at all |
| **Motivational quote on branded background** | Steve Jobs quotes etc. | Don't post motivational quotes |
| **Type unreadable at thumbnail** | 28pt headline on 1080×1080 | Hero ≥ 80pt; test at 250×250 thumbnail |
| **Aspect ratio mismatch in carousel** | Page 1 square, page 2 vertical | Lock one aspect, hold across the carousel |
| **No brand mark** | Asset that could belong to anyone | Logo bottom-corner, ~5% of width |
| **Hero buried below fold** | Headline center-vertically, hidden in feed crop | Headline in top 40% of asset |
| **Generic "swipe →" indicator** | Default chevron with no character | Match the indicator to the design system |

## Hand-offs

- **`pptx` skill** — sometimes used to produce social assets at fast turnaround (PowerPoint exports to PNG cleanly).
- **Canva or Figma** — preferred design tools; export from there.
- **[[design-imagery]]** — when the asset needs commissioned imagery via higgsfield.
- **[[design-iconography]]** — when the asset uses icons.
- **Marketing plugin** (`marketing-social-media-marketer`) — for caption text, hashtags, post strategy, A/B testing.

## Constraints

- Don't accept "make it pop" as direction. Define the stylistic register and density first.
- Don't recycle brand decks into LinkedIn carousels — the type scale, density, and aspect ratio are different.
- Don't ship a carousel without page numbers (1/7, 2/7).
- Don't ship without an alt-text draft.
- Don't ship a single-asset format when a carousel earns more engagement (B2B LinkedIn data favors carousels).
- For Slalom Composable DXP work, default register is **editorial-tech** for technical content, **premium-restrained** for executive content. Avoid minimal-soft (it's the AI default for SaaS social).
