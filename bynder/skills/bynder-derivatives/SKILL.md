---
name: bynder-derivatives
description: Design and govern Bynder derivative templates and on-the-fly transformations — what sizes, formats, focal-points, and quality settings to pre-render vs. generate dynamically, how to align derivatives to the design system's responsive breakpoints, and how to keep the derivative library from sprawling. Covers pre-rendered derivatives, the dynamic asset transformation engine (URL-parameter-driven), CDN caching implications, and the operational discipline of retiring unused derivatives. Use this skill when designing derivative templates for a new deployment, auditing derivative output for an existing implementation, planning image / video output for a design system, or reconciling between Bynder derivatives, Next.js Image optimization, and Vercel's image pipeline.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-derivatives]
tags: [type/skill, plugin/bynder, topic/bynder, topic/derivatives, topic/performance]
status: active
---

# Derivatives (Bynder output formats)

This skill is about getting the right pixels to the right surface at the right cost. Bynder derivatives are the output formats your assets render in — the web hero crop, the social card, the print master, the email-safe thumbnail. The default behavior in Bynder is to pre-render every derivative on every upload. The default behavior of brand teams is to ask for "just one more" derivative every quarter. The default behavior of front-end teams is to use them inconsistently.

The discipline this skill enforces: model derivatives like an API contract. Limited, named, design-system-aligned, and audited.

For Slalom Composable DXP work, derivatives sit between brand operations and front-end engineering. Get them right and `next/image` does the rest of the work. Get them wrong and you end up with a CDN cache full of one-off transformations that nobody can reason about.

Pair with `bynder-asset-model` for the metaproperties that drive derivative selection logic, `bynder-contentful-pairing` for how derivatives surface in CMS field controls, `bynder-js-sdk` for programmatic derivative URL resolution, and `bynder-optimization-audit` if the engagement is fixing a derivative-sprawl problem.

## When to use this skill

- Designing the derivative library for a new Bynder deployment.
- Auditing existing derivatives — which are used, which are sprawl.
- Adding a new output channel (a redesigned web experience, a new email template, a new social platform).
- Reconciling between Bynder derivatives, Next.js Image optimization, and Vercel.
- Diagnosing performance problems where Bynder is the upstream image source.
- Planning an enablement of the dynamic asset transformation engine.
- Deprecating derivatives during a brand refresh.

## Core posture

- **Derivatives are an API contract**, not an asset-bank decoration. Limit them, name them, document them.
- **Default to a small pre-rendered set + dynamic transformations on top.** Pre-rendered for the canonical output sizes the design system uses; dynamic for the long tail.
- **Align to the design system breakpoints.** A derivative that doesn't match a known responsive breakpoint produces awkward crops at the edge cases.
- **Pre-render only what's worth pre-rendering.** Every pre-rendered derivative is a re-render cost on every upload. Print masters, video proxies, and the canonical web hero set — yes. The two-pixel-off variant — no.
- **Treat the dynamic transformation engine like a CDN URL contract.** Lock the parameters used by the front end behind constants; don't sprinkle URL-params across the codebase.
- **Audit quarterly.** Use Analytics to retire unused derivatives.

## Derivative families — what to pre-render

Most engagements end up with a recognizable set:

### Web (responsive image system)

Aligned to Tailwind / design-system breakpoints. Default set:

| Name | Width | Use |
|---|---|---|
| `web_thumbnail` | 320 | Mobile-first thumbnails, lazy-load placeholders |
| `web_card_sm` | 640 | Mobile card images |
| `web_card_md` | 1024 | Tablet card images |
| `web_card_lg` | 1440 | Desktop card images |
| `web_hero_lg` | 1920 | Desktop hero, full-bleed |
| `web_hero_xl` | 2560 | Retina/HiDPI desktop hero |
| `web_inline` | 1024 | Article-body inline image |

All in `webp` and `avif`, with `jpeg` as a fallback. The dynamic transformation engine generates these too, but pre-rendering the most-used sizes saves CDN cache misses.

### Social

Platform-specific aspect ratios are mandatory; sizes evolve.

| Name | Dimensions | Use |
|---|---|---|
| `social_og_1200x630` | 1200×630 | OG / Facebook / LinkedIn share |
| `social_x_1600x900` | 1600×900 | X (Twitter) summary card large |
| `social_instagram_square` | 1080×1080 | Instagram square |
| `social_instagram_portrait` | 1080×1350 | Instagram 4:5 portrait |
| `social_instagram_story` | 1080×1920 | Story / Reels vertical |

Re-evaluate annually; platform specs drift.

### Email

Email clients (especially Outlook) hate creative resizing. Pre-render explicit widths.

| Name | Width | Use |
|---|---|---|
| `email_hero_600` | 600 | Standard email hero |
| `email_inline_400` | 400 | Email inline image |
| `email_thumbnail_200` | 200 | Email thumbnail / button |

JPEG only — many email clients don't render webp.

### Print / production

| Name | Spec | Use |
|---|---|---|
| `print_300dpi` | 300 DPI master, full size | Print production master |
| `print_proof` | 100 DPI watermarked | Print proof |

Print masters are heavy; pre-render once, gate behind permissions.

### Video

| Name | Spec | Use |
|---|---|---|
| `video_web_1080p_h264` | 1080p, H.264 baseline | Web playback |
| `video_web_720p_h264` | 720p, H.264 | Lower-bandwidth fallback |
| `video_social_vertical_1080p` | 1080×1920 | Social vertical |
| `video_thumbnail_jpg` | First-frame JPEG | Poster image |

### Document / brand-asset bundles

Often these aren't true "derivatives" but Bynder collections / bundles — a logo collection includes the primary mark in mono, reversed, knockout, RGB, CMYK, vector. Bundle, don't pre-render every variant separately.

## On-the-fly transformations (dynamic asset transformation)

When enabled on the account, Bynder serves transformations via URL parameters. The substrate looks roughly like:

```
https://d3p7zgxmpk5ezh.cloudfront.net/m/{transform-id}/{asset-id}/{filename}?w=1200&h=630&fit=crop&fm=webp&q=80
```

Rules of thumb:

- **Lock parameters in code.** Front-end constants that emit URLs, not free-form `src` strings sprinkled across components. One `bynderUrl({ id, preset: 'webHeroLg' })` helper used everywhere.
- **Cap the parameter combinations.** A `presets` map with ~10 named presets, not 1000 unique URLs in the wild.
- **Use focal-point crops.** Bynder respects asset focal points set in the asset bank. `?fit=crop&crop=focalpoint` is the pattern.
- **Format negotiation.** `?fm=webp` for modern browsers, `?fm=avif` for next-gen. Let the front end (Next.js) decide based on `Accept` header rather than hardcoding.
- **Quality.** `?q=80` for hero / inline, `?q=70` for thumbnails, `?q=90` for downloadable masters.

### Reconciling with Next.js Image

`next/image` does its own resizing on top of whatever URL you give it. You have two options:

1. **Bynder pre-rendered URL → `next/image`.** Bynder serves `webHeroLg` (1920×); Next resizes again for actual viewport. Two layers of resize → potential quality loss + double caching.
2. **Bynder dynamic URL with the exact target width → `next/image` with `unoptimized: true` or with the loader configured to bypass.** Avoids double-resize.

Slalom default: option 2 for hero/feature images where pixel quality matters; option 1 (Bynder pre-rendered + Next) for thumbnails and grids where simplicity beats pixel-perfection.

A custom Next.js image loader is the cleanest split:

```ts
// next.config.ts
images: {
  loader: 'custom',
  loaderFile: './lib/bynder-image-loader.ts',
}

// lib/bynder-image-loader.ts
export default function bynderLoader({ src, width, quality }: ImageLoaderProps) {
  // src is the Bynder asset ID (or a stable reference)
  return `https://d3p7zgxmpk5ezh.cloudfront.net/m/${TRANSFORM_ID}/${src}?w=${width}&q=${quality ?? 80}&fm=webp`;
}
```

Now `<Image src={asset.id} width={1920} height={1080} />` produces a Bynder-served, correctly-sized URL.

## Operational discipline

### Quarterly derivative audit

```
1. Pull derivative usage from the Analytics API.
2. Bucket: heavily used (>1000 views/quarter), used (1–1000), unused.
3. For each unused derivative: confirm it's not in the Compact View / connector defaults.
4. Schedule deprecation. Notify integration owners.
5. Disable on the account; monitor for 30 days; delete.
```

The unused derivatives have a real cost — re-render budget on every upload, plus CDN cache pollution. Be willing to retire them.

### Naming convention

```
{family}_{purpose}_{spec}
```

Examples: `web_hero_xl_2560`, `social_og_1200x630`, `email_inline_400`, `print_300dpi_master`. Predictable, scannable, sortable.

### Format strategy

| Format | Use |
|---|---|
| `webp` | Web default. ~30% smaller than JPEG. Universal browser support. |
| `avif` | Web next-gen. ~50% smaller than JPEG. Most browsers support; fallback to webp. |
| `jpeg` | Fallback for legacy + email. |
| `png` | Logos with transparency, UI elements. Don't use for photography. |
| `mp4 (H.264)` | Web video baseline. |
| `mov (H.265)` | Higher-quality web/print. Browser support patchier. |

### Quality vs. size

Don't ship 95% quality JPEGs to the web. The visible difference between `q=80` and `q=95` is negligible at typical viewing distances; the file-size difference is 2–3×.

### Focal points

Every brand-photography asset should have a focal point set in the asset bank. The dynamic transformation engine uses it for `fit=crop&crop=focalpoint` cropping. Editors who set focal points on upload save the design team from awkward crops at every responsive breakpoint.

## Output formats

### Derivative library proposal

```
# Bynder Derivative Library: [Project / Brand]

## Goals
[Channels in scope, design-system breakpoints, performance targets.]

## Pre-rendered derivatives
| Name | Spec | Format(s) | Use |
| ... | ... | ... | ... |

## Dynamic transformation presets
| Preset name (in code) | URL params | Use |
| ... | ... | ... |

## Format strategy
[webp / avif / jpeg fallbacks; video formats per channel.]

## Quality strategy
[Per family.]

## Reconciliation with front-end
[Next.js Image custom loader plan; preset → Bynder URL helper.]

## Audit cadence
[Quarterly review process; ownership.]

## Deprecation path for unused derivatives
[Process for retiring.]
```

## Common anti-patterns to call out

- **One derivative per visual variant the design has ever shipped.** Catalog of 80 derivatives. Most unused. Audit and prune.
- **Hand-rolled URLs across the codebase.** Every component constructs Bynder URLs differently. Centralize behind a helper.
- **Pre-rendering things that should be dynamic.** Rarely-used aspect-ratio variants don't deserve re-render budget on every upload.
- **Dynamic-everything.** Conversely, the most-used hero sizes deserve pre-render — no reason to take a CDN cache miss on every first load.
- **Format hardcoded to JPEG.** Every modern browser supports webp; many support avif. Negotiate format.
- **No focal points.** Every photographic asset cropped to viewport without a focal point produces unflattering results.
- **Quality always 95%.** File sizes 2–3× larger than they need to be.
- **Email derivatives in webp.** Many email clients (Outlook) won't render. Stick to JPEG for email.
- **Print masters publicly accessible.** Permission-gate them; they're heavy and rights-sensitive.

## Constraints

- Do not propose more than ~15–20 pre-rendered derivatives without explicit justification — past that, audit pressure exceeds value.
- Do not propose dynamic-only without confirming the on-the-fly transformation engine is enabled on the account (not all plans include it).
- Do not assume design-system breakpoints; confirm with the design lead.
- Do not skip the audit cadence — derivative sprawl is a leading indicator of optimization-engagement work.

## How this skill relates to others

- The metaproperties that gate which assets get which derivative permissions → `bynder-asset-model`.
- The CMS field controls that surface derivative selection → `bynder-contentful-pairing`.
- The SDK helpers that produce derivative URLs → `bynder-js-sdk`.
- The Compact View configuration that returns derivatives to the host → `bynder-compact-view`.
- The audit rubric that finds derivative sprawl → `bynder-optimization-audit`.

## Source material

- Bynder dynamic asset transformation docs: https://developers.bynder.com/dynamic-asset-transformation
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
- Web performance canon: web.dev image optimization, https://web.dev/learn/images/.
