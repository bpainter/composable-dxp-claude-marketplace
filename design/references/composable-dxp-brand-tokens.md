---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/composable-dxp, topic/brand-tokens, topic/sub-brand]
status: active
---

# Composable DXP brand tokens (TFIC sub-brand)

Working tokens for The Future Is Composable (TFIC) sub-brand work — the practice-level brand sitting under Slalom parent. Load this for any TFIC microsite, roadshow material, playbook, insights article, or other practice-owned externally-branded surface.

> **Canonical reference:** `50_Knowledge/Frameworks/Composable_DXP_Brand/`. This file is the working set.
> **Source-of-truth bundle:** `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/`.
> **For Slalom parent work** (proposals, firm decks, whitepapers): use [[slalom-brand-tokens]] instead.

## Which brand applies — quick decision

| Surface | Use |
|---|---|
| TFIC microsite (`thefutureiscomposable.com`), Composable Roadshow, CMO Playbook, TFIC Insights | **This file (TFIC sub-brand)** |
| Slalom proposal, Slalom whitepaper / POV, Slalom-branded deck, slalom.com | [[slalom-brand-tokens|Slalom parent tokens]] |
| Internal practice comms, 1:1 logs, engagement charters, status reports | Neutral (no brand styling needed) |

## The non-negotiables

- **No em-dashes** (`—`) or en-dashes (`–`) in body copy. Use commas, periods, parentheses.
- **No italics in display headings.** Lora 700 bold serif carries the weight.
- **No marketing-speak verbs:** unlock, revolutionize, transform, supercharge, empower, leverage, harness.
- **No "not X but Y" comparative scaffolding.**
- **No emoji. No exclamation marks** (except thank-you confirmations).
- **Sentence case** for headings and buttons.
- **Lavender (`#8a7ce0`) leads.** Cobalt (`#0a25ff`) anchors back to parent — sparing.

## Color tokens

### Brand

| Token | HEX | Use |
|---|---|---|
| `--color-primary` | `#8a7ce0` | Slalom Lavender — primary CTAs, eyebrows, accent rules |
| `--color-primary-deep` | `#5a4fc4` | Hover, pressed, focus ring |
| `--color-primary-soft` | `#ebe7f8` | Tinted backgrounds |
| `--color-secondary` | `#0a25ff` | Slalom Cobalt — sparing, parent-brand anchor |
| `--color-secondary-deep` | `#061ac8` | Cobalt hover |
| `--color-secondary-soft` | `#e0e4ff` | Cobalt tinted bg |

### Foreground

| Token | HEX | Use |
|---|---|---|
| `--color-fg` | `#0e0e1a` | Body text, headings |
| `--color-fg-muted` | `#5a5a6e` | Captions, metadata |
| `--color-fg-subtle` | `#8a8a9c` | Tertiary metadata, disabled |

### Surfaces (paper)

| Token | HEX | Use |
|---|---|---|
| `--color-paper` | `#ffffff` | Default surface |
| `--color-paper-soft` | `#faf9fc` | Emphasis sections (faint lavender tint) |
| `--color-paper-muted` | `#f0eef7` | Cards, inset blocks |
| `--color-paper-deep` | `#ebe7f8` | Strong emphasis |

### Rules

| Token | HEX |
|---|---|
| `--color-rule` | `#e0dceb` |
| `--color-rule-strong` | `#c8c2db` |

### Accents (one per screen)

| Token | HEX | Soft variant |
|---|---|---|
| `--color-accent-warm` | `#f7c948` | `#fef6db` |
| `--color-accent-cool` | `#2dd4bf` | `#d8f5f0` |
| `--color-signal` | `#d12f3a` | `#fbe2e4` |
| `--color-success` | `#18a672` | `#d6f0e4` |

Full palette and usage rules: [[../../../50_Knowledge/Frameworks/Composable_DXP_Brand/Color_System|Color System]].

## Type tokens

- **Display:** `--font-display` → Lora 700. **No italics.**
- **Body:** `--font-body` → Inter 400/500/600/700.
- **Mono:** `--font-mono` → JetBrains Mono 400/500.

### Scale (responsive `clamp()`)

| Token | Range | Use |
|---|---|---|
| `--text-display-xl` | 56–88px | h1 hero |
| `--text-display-lg` | 40–64px | h2 page header |
| `--text-display-md` | 28–40px | h3 card title |
| `--text-display-sm` | 20–26px | h4 small heading |
| `--text-body-lg` | 18px | Lead, h5 |
| `--text-body` | 16px | Default, h6 |
| `--text-body-sm` | 14px | Captions |
| `--text-caption` | 12px | Metadata, mono labels |

### Letter-spacing

- `--tracking-hero` `-0.01em` (h1 only)
- `--tracking-display` `-0.005em` (h2–h4)
- `--tracking-eyebrow` `0.12em` (mono uppercase eyebrows)

### Line-height

- `--leading-display` `1.12`
- `--leading-tight` `1.25`
- `--leading-body` `1.55`
- `--leading-loose` `1.7` (long-form `.prose`)

Full type spec: [[../../../50_Knowledge/Frameworks/Composable_DXP_Brand/Typography|Typography]].

## Spacing — 4px base scale

`4 / 8 / 12 / 16 / 20 / 24 / 32 / 40 / 48 / 64 / 80 / 96`

Generous whitespace is the rule. Density signals tech-startup; we want editorial.

## Radius

| Token | Value | Use |
|---|---|---|
| `--radius-sm` | 4px | Inputs, tags, mono pills |
| `--radius-md` | 8px | Buttons, cards |
| `--radius-lg` | 16px | Large feature cards, sheets, dialogs |
| `--radius-pill` | 999px | Mono tags only |

**No "blob" radii.**

## Shadows (no colored shadows, no glow)

| Token | Use |
|---|---|
| `--shadow-sm` | Cards at rest |
| `--shadow-md` | Hovered cards, dropdowns, popovers |
| `--shadow-lg` | Dialogs, sheets only |

Cards: 1px `--color-rule` border **OR** `--shadow-sm` — never both.

## Motion

- Easing: `cubic-bezier(0.16, 1, 0.3, 1)` (`--ease-out`).
- `--duration-micro` 150ms · `--duration-macro` 250ms.
- **No springs. No bounces. No physics. No parallax. No scroll-jacking.**
- Vocabulary: subtle scale (1 → 1.02) and opacity (0 → 1). That's it.
- Press = opacity drops to 0.85, no scale-down.
- `prefers-reduced-motion: reduce` honored.

## Layout

| Token | Value |
|---|---|
| `--max-content` | 1440px |
| `--max-fullbleed` | 1920px |
| `--max-prose` | 720px |
| `--gutter` | clamp(24px, 4vw, 48px) |
| `--navbar-h` | 64px |

## Iconography

- **Lucide** at 1.5px stroke, rounded line caps.
- Sizes: 16 (rare inline) · 20 (buttons, form) · 24 (nav, default) · 32+ (feature illustrations).
- Color: inherits `currentColor`. Default `--color-fg`; `--color-primary-deep` for active.
- **No emoji. No unicode icons. No filled variants. No icon-only buttons without `aria-label`.**

## Voice (working set)

- **Five anchors:** plain language, honest about trade-offs, literary not breathless, first-person plural ("we"), second-person reader ("you").
- **Em dash banned** in body copy.
- **Sentence case** for headings and buttons. Title Case only for proper nouns / named programs ("Composable Roadshow," "CMO Playbook"). UPPERCASE only for eyebrow labels.
- **Trailing CTA arrow:** Lucide `arrow-right`, not unicode "→".

Full voice rules: [[../../../50_Knowledge/Frameworks/Composable_DXP_Brand/Voice_and_Tone|Voice and Tone]].

## Reuse from Slalom parent

The sub-brand inherits from Slalom parent:

- "Fiercely human" essence and creative lens
- Sentence case discipline
- No marketing-speak verbs (parent and sub-brand agree)
- No emoji discipline
- Customer-first framing

The sub-brand **does not** inherit:

- Em-dash as house punctuation (sub-brand bans it)
- Signature italic move (sub-brand bans italics in display)
- Slalom Blue primary (sub-brand uses Lavender)
- The official Slalom illustrated icon set (sub-brand uses Lucide for now)

## Cross-references

- [[../../../50_Knowledge/Frameworks/Composable_DXP_Brand/README|Composable DXP Brand framework]] — full canonical reference.
- [[slalom-brand-tokens]] — Slalom parent tokens (for non-TFIC work).
- [[slalom-context]] — practice and firm organizational context.
- `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/colors_and_type.css` — canonical CSS.
- `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/ui_kits/microsite/` — JSX components.
