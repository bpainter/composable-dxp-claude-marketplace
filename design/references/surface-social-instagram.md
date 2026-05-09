---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/social, topic/instagram, topic/surface]
status: active
---

# Surface: Instagram

The format-spec reference for Instagram assets. Loaded by [[design-social-asset]].

For Slalom Composable DXP work, Instagram is **rare** — most B2B audiences live on LinkedIn. Instagram applies for: employer-brand content, event coverage, recruiting visibility, occasional consumer-DTC client work. When it does apply, the format rules are tight.

## Surface inventory

| Surface | Dimensions | Aspect | Notes |
|---|---|---|---|
| **Single post (square)** | 1080×1080 | 1:1 | Classic Instagram format. |
| **Single post (portrait)** | 1080×1350 | 4:5 | **Higher engagement** — takes more feed real estate. |
| **Single post (landscape)** | 1080×566 | 1.91:1 | Lowest engagement of the three. |
| **Carousel page** | 1080×1080 or 1080×1350 | 1:1 or 4:5 | Match all pages to one aspect. |
| **Story** | 1080×1920 | 9:16 | Vertical full-screen, 24-hour. |
| **Reel cover** | 1080×1920 | 9:16 | Reel thumbnail. |
| **Reel feed-grid display** | 1080×1080 portion | 1:1 effective | Reels show as cropped square in profile grid — design for this. |
| **Profile picture** | 320×320 | 1:1 | Brand or personal avatar. |
| **Highlights cover** | 1080×1920 (cropped to 1:1 circle) | 9:16 | Story highlights covers. |

## Engagement priorities (consumer / employer-brand 2025+)

Ordered by typical engagement:
1. **Reels** (short-form video) — algorithm priority.
2. **Carousels** (4:5 vertical preferred over 1:1).
3. **Vertical 4:5 single post.**
4. **Square 1:1 single post.**
5. **Stories** — high engagement but ephemeral.
6. Landscape post — avoid.

## Type-at-thumbnail discipline

Same principle as LinkedIn: posts render small in feed (~360px wide on phone). For 1080×1080:

| Role | Min size | Use |
|---|---|---|
| Hero headline | 90pt | Cover hero |
| Sub-headline | 48pt | Supporting line |
| Body | 32pt | Page body |
| Caption / source | 22pt | Source attribution |
| CTA | 32pt | Closing CTAs |

Instagram audiences are even more scroll-heavy than LinkedIn. **Type discipline is even tighter.** Hero readable at thumbnail = non-negotiable.

## Layout grid

For 1080×1080:
- **Safe area:** 100px on all sides (1080 - 200 = 880 usable).
- **Critical content** lives in center 1000×1000 — Instagram crops slightly different on different devices.

For 1080×1350 (portrait):
- **Safe area:** 100px on sides, 130px top/bottom.
- **Title zone:** top 30% (~400px).
- **Body zone:** middle 50% (~675px).
- **Footer zone:** bottom 20% (~270px).

For 1080×1920 (story / Reel cover):
- **Top 250px:** account name overlay zone — keep critical content out of this area.
- **Bottom 250px:** sticker/swipe-up overlay zone — keep critical content out of this.
- **Effective safe area:** 1080×1420 in the middle.
- **Type-safe zone:** 1080×1200 to be conservative.

## Carousel best practices

Same principles as LinkedIn carousels (see [[surface-social-linkedin]]):
- Cover earns the swipe.
- One idea per page.
- Closing page commits.
- Page numbers (1/N).
- 7–10 pages sweet spot.

**Instagram-specific differences from LinkedIn:**
- **Vertical 4:5 outperforms square 1:1** — switch to 1080×1350 by default.
- **Type can be slightly smaller** than LinkedIn carousels because Instagram audiences zoom into posts more often. But still test at thumbnail.
- **Visual style can be more expressive** — Instagram audiences expect more visual punch than B2B LinkedIn.

## Story / Reel cover specifics

### Stories (1080×1920)
- 24-hour ephemeral. Less precious — can be more experimental.
- **Lower critical-content zone:** middle 1080×1420.
- **Account name appears top-left** — design around it.
- **Swipe-up / "see more" appears bottom** — design around it.
- **Stickers, polls, links overlay** in middle zone.

### Reel covers (1080×1920)
- This is the *thumbnail* on the user's profile grid (cropped to 1:1) and in the feed (full 9:16).
- **Design for both crops:** the center 1080×1080 must work as a thumbnail, the full 1080×1920 must work as a feed Reel cover.
- **Brand mark / hook** in the center 1080×1080 zone.

## Story / Reel content structure

For multi-part Stories:
- **Frame 1 (hook):** The question, the surprising claim, the "wait, what?"
- **Frames 2 to N-1:** the content.
- **Last frame:** the CTA — swipe-up to whitepaper, DM us, follow.
- **Pacing:** users tap through quickly. Each frame should land in 2 seconds.

## Profile picture and highlights

### Profile picture (320×320, displays as circle)
- Brand mark only. No tagline.
- For personal: face centered, shoulders visible. Test the circular crop.

### Highlights covers (1080×1920, displayed as 1:1 circle)
- One per topic ("Events," "Speaking," "Whitepapers").
- **Visual consistency** across all highlight covers.
- **Treatment:** simple icon or letter on brand-color background.

## Pre-delivery checklist (Instagram-specific)

Run [[pre-delivery-checklist]] plus:

- [ ] **Aspect ratio matches platform.**
- [ ] **Critical content in safe zones** (account for top/bottom overlays in stories).
- [ ] **Hero readable at thumbnail** (~360px for feed).
- [ ] **Reel cover works as both 9:16 and 1:1 crop.**
- [ ] **Carousel: one aspect, all pages.** Don't mix.
- [ ] **Page numbers** on carousel pages.
- [ ] **Brand mark present and consistent.**
- [ ] **Alt text drafted** for accessibility.
- [ ] **Hashtag-suggestion list** drafted (the writer adds hashtags but the designer often supplies relevance).

## When NOT to design for Instagram

For most Slalom Composable DXP work, ask first: does this audience actually live on Instagram? If yes (consumer brand, employer-brand recruiting), proceed. If no, redirect the energy to LinkedIn.

Slalom-internal content (events, off-sites) is the most common legitimate Instagram use case. Client thought-leadership rarely fits Instagram.

## Cross-references

- **Skill:** [[design-social-asset]].
- **LinkedIn:** [[surface-social-linkedin]].
- **Anti-bland:** [[ai-tells-forbidden-patterns]] section "Surface-specific tells: social."
- **Imagery:** [[design-imagery]] for higgsfield-generated visuals.
