---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/slalom, topic/brand-tokens]
status: active
---

# Slalom brand tokens

Working tokens for Slalom-branded design work. **Loaded by every Slalom-instance design task** — pair with the more comprehensive context in `slalom-context.md`.

> **Canonical reference:** the full brand documentation lives at `50_Knowledge/Frameworks/Slalom_Brand/`. This file is the working set.
> **Source-of-truth files:** `70_Templates/Slalom_Brand_Assets/` (Brand Guide PDF, PowerPoint template, web design system bundle).
> **For the practice's TFIC sub-brand** (microsite, roadshow, playbook, insights): use [[composable-dxp-brand-tokens]] instead. The two brands diverge intentionally — em-dash, italic display, primary color all differ.

## The non-negotiables

- **Slalom Blue (`#0C62FB`) MUST appear in every composition.**
- **Sentence case everywhere** — headlines, subheads, body, buttons, nav. Never Title Case (except Medium / wired press releases).
- **No emoji.** No decorative unicode. No exclamation points (except inside the value name "Smile!").
- **The signature italic move** (web only): italicize 1–3 Lora-italic words inside an Inter/Slalom Sans headline. Carries the emotional/aspirational core. Don't italicize full sentences or connective words.
- **Logo:** Slalom Blue, Slalom Dark Blue, White, or Black. Never any other color. One global logo — no sub-brand-as-new-logo.

## Color tokens

### Primary core

| Name | HEX | Use |
|---|---|---|
| Slalom Blue | `#0C62FB` | THE primary — buttons, links, focus, headlines |
| Slalom Dark Blue | `#002FAF` | Hover, deep emphasis |

### Secondary core (one at a time per surface)

| Name | HEX |
|---|---|
| Cyan | `#1BE1F2` |
| Coral Red | `#FF4D5F` |
| Purple | `#C7B9FF` |
| Chartreuse | `#DEFF4D` (brand) / `#CDE848` (web) |

### Web extended (slalom.com only)

| Name | HEX |
|---|---|
| `--slalom-blue-hover` | `#0A4EC9` |
| `--slalom-blue-deep` | `#002FAF` |
| `--slalom-navy` | `#0F1C41` |
| `--slalom-ink` | `#000A25` |
| `--neutral-50` (page bg) | `#F5F5F5` |
| `--neutral-200` (border) | `#E6E6E6` |
| Semantic danger | `#DA052B` |

### Neutrals

| Name | HEX |
|---|---|
| White | `#FFFFFF` |
| Light Gray | `#E6E6E6` |
| Dark Gray (text only) | `#666666` |
| Black | `#000000` |

Full palette including dark tones and light tints: see [[../../../50_Knowledge/Frameworks/Slalom_Brand/Color_System|Color System]].

## Type tokens

### Brand-controlled surfaces (print, web, social, video, signage)

- **Primary:** Slalom Sans (Thin, Light, Regular, Bold, Black + italics)
- **Supporting:** Lora (Regular, Medium, Semibold, Bold + italics) — optional

### PowerPoint slide decks

- **Avenir Next LT Pro** (Light, Regular, Demi, Bold + italics) — embedded in the official template. **Don't substitute Slalom Sans in PowerPoint.**

### Avenir Next LT Pro outside PowerPoint (only these three)

- MS Outlook email default settings (external)
- MS Power BI dashboards
- MS Word docs editable and shared with clients

### Web (slalom.com — Slalom Sans isn't publicly licensed)

- Sans: **Inter** 300–700 (substitute for Slalom Sans)
- Serif: **Lora** italic 400–500
- Mono: **JetBrains Mono**

### Web type scale (rem, 16px root)

| Token | Size | Weight |
|---|---|---|
| `--fs-display-xl` | 4.5rem | 300 |
| `--fs-display-l` | 3.5rem | 400 |
| `--fs-display-m` | 2.75rem | 500 |
| `--fs-h1` | 2.25rem | 600 |
| `--fs-h2` | 1.875rem | 600 |
| `--fs-h3` | 1.5rem | 600 |
| `--fs-h4` | 1.25rem | 600 |
| `--fs-body-l` | 1.125rem | 400 |
| `--fs-body` | 1rem | 400 |
| `--fs-body-s` | 0.875rem | 400 |
| `--fs-caption` | 0.75rem | 500 (uppercase, tracked) |
| `--fs-quote` | 1.75rem | 400 italic Lora |

Display weight philosophy: light (300–400). UI: 500–600. Heading tracking `-0.02em`. Eyebrows uppercase + wider tracking.

Full type spec including pairing rules, deck type styles, justification: see [[../../../50_Knowledge/Frameworks/Slalom_Brand/Typography|Typography]].

## Spacing, radii, shadows (web)

- **Spacing scale:** 8-pt rhythm — `4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 96 / 128`.
- **Section vertical:** 96–128px desktop, 64px mobile.
- **Container max:** 1280px wide, 720px editorial-narrow.
- **Radii:** 4 (chips) / 8 (buttons, inputs, cards default) / 16 (large cards, modals) / 24 (feature hero) / full (pills only — badges, avatars, icon buttons).
- **Shadows:** ink-blue tinted, `rgba(0, 10, 37, 0.05–0.16)`. `sm` rest, `md` hover, `lg/xl` for popovers/modals only. **No neumorphism, no glow.**
- **Focus ring:** `0 0 0 3px rgba(12, 98, 251, 0.35)` — always visible.

## Motion

- Default `200ms cubic-bezier(0.22, 1, 0.36, 1)` (ease-out-quart). `300ms` for larger surfaces.
- Hover lift: `translate-y(-2px)` on cards, `shadow-sm → shadow-md`.
- **No bounces. No springs. No scale-on-press.** Confidence = restraint.
- `prefers-reduced-motion` respected.

## Voice (working set)

- **Five attributes:** people-first, uplifting, friendly, real, innovative.
- **Sentence case** for everything. **No emoji.** **No exclamation points** (except "Smile!").
- **Em dash (—)** is house punctuation. Closed (no spaces) per brand guide; web design system permits spaces.
- **"We"** for Slalom. **"You"** for the client. **Rarely "I."**
- **Slalomers** for the community (informal). **Slalom team members** (formal).
- **Customers**, not clients.
- Trailing CTA arrow: Lucide `arrow-right` SVG, not the unicode "→" character.

Full voice rules including capitalization edges, em dash rules, percent rules, inclusive language: see [[../../../50_Knowledge/Frameworks/Slalom_Brand/Voice_and_Tone|Voice and Tone]].

## Iconography (working set)

| Surface | Default icon family |
|---|---|
| Slalom.com / web app | **Lucide** at `strokeWidth={1.75}`, single-color, currentColor |
| Internal Slalom comms / ERG | Slalom Icon Set 1 (illustrated) — from Creative Library |
| PowerPoint default | Slalom Icon Set 1 (already embedded) |
| **Customer-facing decks** (refined) | **Flaticon outlined, black or Slalom brand colors** (practice override) |
| Whitepaper / POV / article | **Flaticon outlined** (practice override) |

The practice override applies to all polished customer-facing work. Skip the kindergarten-style illustrated set. See [[../../../50_Knowledge/Frameworks/Slalom_Brand/Iconography|Iconography]] and `tool-flaticon.md`.

## Imagery (working set)

- Candid photography of real consultants + real clients.
- **No stock for Slalom employees.** Real people only, with signed releases.
- **No 3D renders. No mesh gradients. No AI stock.**
- Add brand-color pops via wardrobe, accessories, props.
- Documentary case-study photos: don't radically alter colors.

Full imagery rules: see [[../../../50_Knowledge/Frameworks/Slalom_Brand/Imagery_and_Graphics|Imagery and Graphics]].

## Cross-references

- [[../../../50_Knowledge/Frameworks/Slalom_Brand/README|Slalom Brand framework]] — full canonical reference.
- [[slalom-context|slalom-context.md]] — firm/practice organizational context (parallel reference).
- [[surface-deck-16x9|surface-deck-16x9.md]] — generic 16:9 deck spec; uses these tokens.
- [[surface-document-letter|surface-document-letter.md]] — generic 8.5×11 spec; uses these tokens.
- [[tool-flaticon|tool-flaticon.md]] — Flaticon protocol with Slalom kindergarten override.
- `70_Templates/Slalom_Brand_Assets/slalom-design-system/project/colors_and_type.css` — canonical web token CSS.
