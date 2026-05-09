---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/social, topic/linkedin, topic/surface]
status: active
---

# Surface: LinkedIn

The format-spec reference for LinkedIn assets. Loaded by [[design-social-asset]] for B2B social work — the primary social channel for Slalom Composable DXP content.

## Surface inventory

| Surface | Dimensions | Aspect | Notes |
|---|---|---|---|
| **Single image post (square)** | 1200×1200 | 1:1 | **Default for B2B**. Higher engagement than 1.91:1. |
| **Single image post (landscape)** | 1200×627 | 1.91:1 | Native LinkedIn UI optimal but lower engagement than square. |
| **Carousel page (square)** | 1080×1080 | 1:1 | Each page of swipeable carousel. |
| **Carousel page (vertical)** | 1080×1350 | 4:5 | **Higher reach in 2025+** — vertical takes more feed space. |
| **Link preview** | 1200×627 | 1.91:1 | Auto-generated when URL is in post. Design when you control the destination's OG image. |
| **Article header** | 1280×720 | 16:9 | LinkedIn Article cover. |
| **Profile cover** | 1584×396 | 4:1 | Personal profile background. |
| **Company page cover** | 1192×220 | 5.4:1 | Company page background. |
| **Document post (PDF)** | 1080×1080 typical | varies | Native PDF carousel — high-engagement format. |

## Engagement priorities (B2B 2025+)

Ordered by typical engagement rate (descending):
1. **Carousel** (image carousel or document/PDF post) — highest reach.
2. **Vertical 4:5 single post.**
3. **Square 1:1 single post.**
4. Landscape 1.91:1 single post.
5. Plain text post (no image).

For Slalom Composable DXP work, default to carousel for substantive content, square or vertical single for quick-take posts.

## Type-at-thumbnail discipline

LinkedIn feed renders posts at varying sizes. The thumbnail in mobile feed is ~280px wide. The same asset must work at thumbnail and full size.

For 1080×1080 carousel page:

| Role | Min size | Use |
|---|---|---|
| Hero headline | 80pt | First fold of cover page |
| Sub-headline | 40pt | Supporting line |
| Body | 28pt | Page-N body content |
| Caption / source | 18pt | Footnotes, sources |
| CTA / button | 32pt | Closing-page CTAs |
| Page number | 18pt | "1/7" indicator |

Rule of thumb: the hero on the cover should read clearly at 280px (mobile thumbnail). Test by exporting and viewing at 25% zoom.

## Layout grid

For 1080×1080 carousel:
- **Margin:** 80px on all sides (usable 920×920).
- **Title zone:** top 30% (270px).
- **Body zone:** middle 50% (450px).
- **Footer zone:** bottom 20% (180px) — page number, brand mark, swipe-indicator.

For 1200×1200 single post:
- **Margin:** 100px on all sides.
- Same proportional zones.

## Carousel best practices

### Cover (page 1)
- **Earns the swipe.** Specific question, surprising claim, or count-with-tease ("7 patterns that will surprise you" — only if they will actually surprise).
- **Hero headline dominates** — top 50% of cover.
- **Subhead optional** — when the headline needs context.
- **Page indicator** (1/7 or "Swipe →") in footer.
- **Banned:** generic "5 lessons learned," motivational quotes, stock-graphic backgrounds.

### Pages 2 to N-1
- **One idea per page.** Don't cram.
- **Visual consistency** with cover (type, color, layout grid).
- **Page numbers** in footer.
- **Optional:** progressive narrative (each page builds on the previous) or parallel structure (each page presents a parallel idea).

### Closing page (page N)
- **Specific commitment** — link to whitepaper, specific question to answer in comments, specific hashtag, follow CTA with reason.
- **Banned:** "Follow for more," "Like and share," generic CTAs.
- **Brand mark present** — same placement as other pages.

### Length sweet spot
- 7–10 pages. Algorithm favors 7+; below 5 reads thin, above 12 reads bait.
- Document/PDF carousels can go longer (20+ pages OK because the format implies long-form).

## Document post (PDF carousel) specifics

LinkedIn's native PDF post format uploads a multi-page PDF that scrolls in the feed. Strong B2B reach. Specs:

- **Page size:** 1080×1080 or 1080×1350 (matches image carousel) or 8.5×11 (treated as document).
- **File format:** PDF.
- **Length:** 10–25 pages typical; supports up to 300.
- **Use:** more substantive content than image carousels — small whitepaper format, highlight reels, compiled insights.

## Link preview discipline

When the post includes a URL, LinkedIn pulls the destination's OG image (1200×627). Designer controls the OG image, so:

- **Treat OG image as a separate asset** designed to the 1.91:1 spec.
- **Title and description** also matter (set in the page's `<title>` and `<meta name="description">` tags).
- **Logo discipline** — brand mark on the OG image, but small (~8% of width).
- **No CTA button rendering** in the OG image (LinkedIn provides its own).

## Profile and company page covers

Less frequent but worth getting right.

### Personal profile cover (1584×396, 4:1)
- **Banned:** stock photos.
- **Good:** typographic statement of focus area, abstract pattern in brand colors, professional landscape relevant to your work.
- **Account for cropping:** mobile crops differently from desktop. Keep critical content centered.

### Company page cover (1192×220, 5.4:1)
- Even more cropped than personal. Center content tightly.
- **Logo position:** profile image overlaps left side — design around it.
- **Treatment:** brand statement, value prop in 5 words, abstract brand pattern.

## Pre-delivery checklist (LinkedIn-specific)

Run [[pre-delivery-checklist]] plus these:

- [ ] **Aspect ratio matches platform spec.**
- [ ] **Hero readable at 280px thumbnail.**
- [ ] **Brand mark on every carousel page.**
- [ ] **Page numbers (1/N) on every carousel page.**
- [ ] **Cover page earns the swipe** — not a generic listicle headline.
- [ ] **Closing page commits** — not "follow for more."
- [ ] **Visual consistency across carousel** (type, color, grid).
- [ ] **Alt text drafted** for every image.
- [ ] **Suggested post copy** drafted (the visual designer hands the writer a hook).
- [ ] **Link destination's OG image** also designed if the post links to a URL.

## Cross-references

- **Skill:** [[design-social-asset]].
- **Anti-bland:** [[ai-tells-forbidden-patterns]] section "Surface-specific tells: social."
- **Imagery:** [[design-imagery]] for higgsfield-generated visuals.
- **Iconography:** [[design-iconography]] when icons are used.
- **Marketing strategy:** `marketing-social-media-marketer` skill in marketing plugin.
