---
description: Author a visual identity system — logo direction, color, typography, photography, iconography style — as one coherent system

# Project context
type: command
project: skills-library
plugin: brand
tags: [type/command, plugin/brand]
status: active
---

Author the visual identity system for a brand — new, refresh, or extension.

Steps:

1. **Confirm strategic context** from [[brand-strategist]]: positioning, audience, competitors. If missing, route there first.

2. **Confirm verbal register** from [[brand-voice-tone]]: voice attributes and tone register. Visual register must match.

3. **Pick a stylistic register** from [[stylistic-vocabulary]] (premium-restrained / swiss-modern / editorial-tech / brutalist / minimal-soft / etc.). Default for Slalom Composable DXP work: **premium-restrained** for executive brands, **editorial-tech** for technical brands.

4. **Direct the logo** via [[brand-identity-system]] logo brief template:
   - Strategic context (3 brand-stand-for words, audience, 3 feel adjectives, 2 must-not-feel adjectives, competitors to differentiate from).
   - Constraints (formats, variants, minimum size, color palette directional or fixed).
   - 2–4 concept directions with rationale per direction.
   - Anti-bland gates (no geometric circle, no globe-with-arrow, no lightbulb-as-idea, no AI-Purple/Blue gradient).

5. **Build the color system:**
   - Primary (1–2 colors), secondary (2–4), neutrals (4–6), functional (success/warning/error/info).
   - Per color: semantic name, HEX/RGB/HSL, HSL channel form, WCAG contrast pairings, usage rules.
   - Cite [[ai-tells-forbidden-patterns]] color tells: LILA ban, pure-black ban, two-warring-primaries ban, oversaturated-accents ban.
   - Plan dark mode from start; verify contrast in both modes.

6. **Build the type system:**
   - Display, text, mono families.
   - Per face: family, license (web/app/marketing/embedded), weights used, modular type scale, pairing rules, fallback stack.
   - Cite [[ai-tells-forbidden-patterns]] type tells: Inter ban (for premium briefs), serif-on-dashboard ban, oversized-H1 ban, modular-scale required.
   - License every face for actual use cases before committing.

7. **Specify photography direction** via [[design-imagery]] 7-element art-direction brief.

8. **Specify iconography style** via [[design-iconography]] (family, stroke weight, fill-vs-outline rule, sizing scale).

9. **Document the system.** Hand off to [[brand-guidelines-composer]] for assembly into the deliverable guidelines document.

Output format:

```
# Identity system: {Brand}

## Stylistic register
- Family from stylistic-vocabulary: {register}
- Reason: {why this register fits}

## Logo
- Concept direction selected: {name}
- Rationale: {why}
- File formats: SVG (primary), PNG, ICO
- Variants: full color, monochrome, single-color, knockout
- Minimum size: 16px
- Clearspace: {minimum margin}
- Misuse examples: {what's banned}

## Color system
- Primary: {semantic name + HEX/RGB/HSL + contrast pairings}
- Secondary: ...
- Neutrals: ...
- Functional: ...
- Dark-mode rules: ...

## Type system
- Display: {family + license + weights + scale + pairing}
- Text: ...
- Mono (optional): ...
- Modular scale: {12, 14, 16, 18, 24, 32, 48 — or modular ratio}

## Photography direction
- Subject category: {people / objects / abstract}
- Lighting signature: {warm / cool / studio / dramatic}
- Composition signature: {wide / medium / asymmetric}
- Palette: {3–5 colors}
- Treatment: {color / B&W / duotone / muted}
- Style: {documentary / commercial / editorial / illustrative}

## Iconography style
- Family: {Lucide / Phosphor / commissioned}
- Stroke weight: {1.5px or 2px}
- Fill rule: {default outline; filled = active}
- Sizing scale: xs/sm/md/lg/xl
```
