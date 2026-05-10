---
type: readme
project: skills-library
scope: plugin
plugin: brand
tags: [type/readme, plugin/brand]
status: active
---
# brand

**Brand strategy, naming, identity, voice, guidelines, and audit.**

## Skills (6)

### Strategy

| Skill | What it does |
|---|---|
| [[brand-strategist]] | Strategic lead for brand work — positioning, narrative, audience, brand architecture, rollout. Orchestrates the specialty skills below. |

### Specialties

| Skill | What it does |
|---|---|
| [[brand-naming]] | Naming products, companies, features, offerings. Five-category framework, criteria, six-step process from brief to finalists. |
| [[brand-identity-system]] | Visual identity authoring — logo direction, color systems, typography, photography direction, iconography style. One coherent system. |
| [[brand-voice-tone]] | Voice attributes (with not-X pairings, do/don't pairs), tone register matrix, filler-words bans, AI-tic rejection. |
| [[brand-guidelines-composer]] | Assembles the deliverable guidelines document — 9 standard chapters, sized to brand (one-pager / standard / enterprise). Hands off to docx / pdf. |
| [[brand-audit]] | Read-only audit of existing brand applications — drift detection, prioritized fix list across identity, voice, surface application, governance. |

### Retired

- [[brand-designer]] — split into the four specialty skills above (naming, identity-system, voice-tone, guidelines-composer) plus the new brand-audit. Retained as a redirect for backwards compatibility.

## Agent

**`brand-agent`** — Senior brand practitioner who orchestrates the specialty skills. Use this agent when a task spans multiple specialties, requires sequencing, or needs senior judgment about which skill applies.

## Pairs with

- **`design`** — for visual execution. Brand outputs (identity system, color tokens, type system) become inputs to design-system-engineer, design-screen, and surface skills.
- **`marketing`** — for positioning and voice that surface in copy, social, SEO.
- **`consulting`** — for client-facing brand work inside engagements.

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- The specialty skills cite [[design-taste]] (in the design plugin) for the named visual-tells (LILA ban, Inter ban, etc.) so brand and design share one source of truth.
- The voice skill cites [[ai-tells-forbidden-patterns]] for the filler-words ban list.
- Tone across all skills: direct, opinionated, builder-oriented.

## Cross-plugin references

The brand plugin's specialty skills cite design-plugin references so the two plugins share one source of truth:

- [[stylistic-vocabulary]] (design plugin) — register selection.
- [[ai-tells-forbidden-patterns]] (design plugin) — visual and verbal bans.
- [[motion-and-transitions]] (design plugin) — for motion in brand identity systems.
- [[design-imagery]] (design plugin) — for photography direction.
- [[design-iconography]] (design plugin) — for icon family selection.

## Slalom-instance reference

For Slalom-branded work, the canonical reference lives at `50_Knowledge/Frameworks/Slalom_Brand/`:

- [[../../50_Knowledge/Frameworks/Slalom_Brand/Brand_Foundations|Brand Foundations]] — fiercely human essence, creative lens.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/Voice_and_Tone|Voice and Tone]] — voice rules feed `brand-voice-tone`.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/Color_System|Color System]] — palette feeds `brand-identity-system`.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/Typography|Typography]] — fonts feed `brand-identity-system`.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/Logo_and_Lockups|Logo and Lockups]] — logo rules feed `brand-identity-system` and `brand-audit`.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/Iconography|Iconography]] — icon system feeds `brand-identity-system`; includes the practice's Flaticon override.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/Imagery_and_Graphics|Imagery and Graphics]] — photo + Slalom-line + color-bar rules.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/PowerPoint_Template_Inventory|PowerPoint Template Inventory]] — official PowerPoint scaffolding.
- [[../../50_Knowledge/Frameworks/Slalom_Brand/Web_Design_System|Web Design System]] — slalom.com tokens and patterns.

Binary assets (Brand Guide PDF, .potx, design-system bundle) live at `70_Templates/Slalom_Brand_Assets/`.

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
