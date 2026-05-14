# Slalom Brand Patterns — Synthesis from slalom.com (May 2026)

Crawled: homepage (`/us/en`), AI services (`/services/artificial-intelligence`), PUMA case study (`/customer-stories/puma-ai-creator`). Cross-referenced against `70_Templates/Slalom_Brand_Assets/Slalom_Brand_Guide_Feb2025.pdf` (the official brand guide) and the existing design system at `70_Templates/Slalom_Brand_Assets/slalom-design-system/`.

## The signature move

**Italic Lora emphasis within sans-serif headlines.** This is the brand's strongest, most consistent visual move and the easiest way to make a slide read as Slalom.

Examples observed:
- "Meet your customers in the *moments that matter*"
- "Legacy modernization. Done right."
- "AI that drives *real impact*"
- "Industry *know-how*"
- "Exponential *impact*"
- "End-to-end *services*"
- "Let's solve *together.*"
- "AI consulting services with our *technology partners*"
- "We combine our AI expertise with other services to deliver on *your unique vision.*"
- "*You may also like*"
- "Let's talk about your *next big project*."

Frequency: roughly 90% of H2-level section headers on slalom.com use this pattern.

**Mechanic**: sans-serif headline, with 1-3 emphasis words in italic Lora. The emphasis word is usually a noun or short phrase that carries the meaning ("impact," "services," "together," "your unique vision"). The italic carries the warmth — counterbalancing the corporate efficiency of the sans-serif.

## Tagline & positioning line

- **Footer tagline**: "Fiercely Human Consulting"
- **Above-fold positioning** (homepage): "Slalom is a fiercely human business and technology company that leads with outcomes and partners with leaders—bringing more together."
- **Purpose statement** (homepage): "Our purpose is to help people and organizations dream bigger, move faster, and *build better tomorrows for all.*"

## Voice & tone

- **Outcome-driven**: "AI consulting services that turn ambition into measurable outcomes." Not "leverage" / "enable" / "unlock" — explicit outcomes.
- **Plainspoken**: short declarative sentences. "Legacy modernization. Done right." beats "Comprehensive legacy modernization solutions designed to..."
- **Warm authority**: confident but not loud. The italic-Lora carries the warmth.
- **First-plural ("we")** is standard. Client referred to as "you" or by name.
- **Specifics over abstractions**: 180k jersey designs, 1.7M ratings, 54k users from 206 countries — Slalom case studies lead with hard numbers.

## Photography

- **People in workspaces**: real consultants and clients (named in captions), not stock models
- **Documentary-style**: candid, conversational, slight motion in some frames
- **Mid-shots and wide shots**: rarely tight portraits (those are reserved for team-bio thumbnails)
- **Color**: natural daylight dominates. Warm-cool contrast common.
- **Subjects**: 2-4 people in frame is typical. Singles are reserved for portraits.
- **No**: cliché handshakes, suited-men-pointing-at-laptops, fake-diversity tableaux, shutterstock-aesthetic

## Geometric / abstract visuals

Slalom mixes photography with abstract geometric motifs:
- **Dot patterns** on deep navy — concentric circles in yellow/cyan as accent colors
- **Repeating grid of dashed circles** — used on AI services page as a "Thumbnail 520×490"
- **Gradient fields** on dark navy backgrounds — subtle depth, never busy

These geometric visuals are the visual language for "AI / data / abstract concept" sections where a photograph would feel forced.

## Color usage in practice

Observed on the live site:

| Surface | Color use |
|---|---|
| **Hero (light)** | White / Neutral-50 background. Slalom Ink type. Slalom Blue for CTAs. |
| **Hero (dark)** | Slalom Ink or Navy background. White type. Lime or cyan as 1 accent dot in a geometric pattern. |
| **Body sections** | White surface, generous spacing. Single accent color per section (rarely two). |
| **CTA buttons** | Slalom Blue fill, white text. |
| **Eyebrow** | Slalom Blue, small uppercase. |
| **Stats / numbers** | Slalom Ink (light bg) or White (dark bg). NOT colored. |
| **Customer logos** | Grayscale, low contrast — don't compete with the headline. |

The 5-bar of colors stacked on the right edge **does not appear on slalom.com**. The brand has evolved away from it. The official template (`Slalom_PowerPoint_16x9_Aug2025.potx`) still uses it but the brand language has moved on. Our gradient replacement is faithful to where the brand actually is in May 2026.

## Layout patterns observed

| Pattern | Where used | Adapt to slide |
|---|---|---|
| **Big italic-emphasis headline + single CTA on dark hero** | Homepage hero, section opens | → `title.html`, `section-divider.html` |
| **Eyebrow + italic-emphasis headline + lead paragraph + body** | Service overview sections | → `eyebrow-headline-body.html`, `one-col-text.html` |
| **Numbered or labeled grid of capabilities (3-4 across)** | "AI consulting capabilities" on services pages | → `three-col.html`, `list-with-icons.html` |
| **Pull quote with attribution + portrait** | Customer testimonials, case study quotes | → `pull-quote.html` |
| **Stat block: huge number + small label** | Case study impact sections (180k, 1.7M, 81k) | → `big-stat.html` |
| **Card grid: image + tag + title + read-time** | Insights / case study browse | → `case-study.html` (single), partial repeats for browse |
| **Process / blueprint diagram (full-width image)** | AI blueprint section | → `process-3-stage.html` |
| **Two-column text-text** (paragraph + paragraph with subhead) | Case study "Vision / Solution" sections | → `two-col-text-text.html` |
| **Image-left + text-right (or reversed)** | Services with photo + capability description | → `two-col-text-image.html` |
| **Team carousel with name + title + quote + headshot** | Case study "Our Slalom Team" | → `team-bios.html` |

## What the templates should embody

To make a deck feel Slalom in May 2026, the templates need to:

1. **Lead with italic-Lora emphasis on every title slide and most section dividers** — non-negotiable signature
2. **Use the smooth gradient instead of the 5-bar** — right-edge strip, top/bottom band, or hero field
3. **Hold white space generously** — Slalom's web sections breathe; cramped slides feel off-brand
4. **One accent color per slide** — restraint, not the rainbow
5. **Photography or abstract geometric motifs** — not stock illustrations of corporate scenes
6. **Stats as large numerals with quiet labels** — let the number do the work
7. **Plainspoken copy** — short sentences, specific verbs, no consultant filler

## Brand assets in the workspace

| Asset | Location |
|---|---|
| Slalom Brand Guide (PDF) | `70_Templates/Slalom_Brand_Assets/Slalom_Brand_Guide_Feb2025.pdf` |
| Official PowerPoint template | `70_Templates/Slalom_Brand_Assets/Slalom_PowerPoint_16x9_Aug2025.potx` |
| Slalom logos (SVG) | `70_Templates/Slalom_Brand_Assets/slalom-design-system/project/assets/` |
| Composable DXP logos | `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/assets/` |
| Slalom Sans (woff/woff2) | `/Users/bermon.painter/Downloads/Slalom Sans V1.017/Slalom Sans/` |
| Lora (ttf) | `/Users/bermon.painter/Downloads/Lora/Lora/` |

Fonts are embedded in the token CSS via `@font-face` from these absolute paths (for local rendering) or substituted with Google Fonts Lora + a system-sans fallback (for distribution).
