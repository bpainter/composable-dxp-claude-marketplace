---
name: brand-guidelines-composer
description: >
  Assemble a brand guidelines document — the deliverable that consolidates strategy, naming, voice, identity, and applications into a single reference. Owns the structure (foundation, verbal, logo, color, type, imagery, motion, applications, governance), the level of detail per section, and the format-specific execution (one-page vs. enterprise multi-chapter). Hands off to docx, pdf, or web-publish skills for execution. Use this skill any time the user is producing a brand guidelines deliverable.

# Project context
type: skill
project: skills-library
plugin: brand
aliases: [brand-guidelines-composer, brand-guidelines, style-guide, brand-book]
tags: [type/skill, plugin/brand, topic/brand, topic/guidelines]
status: active
---

# Brand Guidelines Composer

A brand guidelines document consolidates everything the brand has decided into a single reference. Most brand guidelines fail in one of two ways: they're too thin (a one-pager that doesn't actually constrain anything), or too bloated (a 90-page tome nobody opens after launch).

This skill chooses the right level of detail for the brand's actual context, structures the content so it's findable, and assembles it for the right output format (docx, pdf, web-published, or a Notion/Confluence page).

Pair with [[brand-strategist]], [[brand-naming]], [[brand-identity-system]], [[brand-voice-tone]] for the source material; [[design-document]] for the design system; [[docx]] / [[pdf]] for execution.

## When to use this skill

- Producing a brand guidelines document for a launching brand.
- Updating brand guidelines after a refresh.
- Auditing existing guidelines for completeness.
- Creating internal-only vs. external-publish versions.
- Re-formatting guidelines from one output (Word) to another (web-published).

## What this skill is NOT

- Not the strategy / naming / voice / identity skills. Those *produce* the content; this skill *assembles* it.
- Not the document-design skill — see [[design-document]] for the visual layout.
- Not the file-execution skill — `docx` and `pdf` skills generate the files.

## The standard structure

When asked to produce brand guidelines, default to this 9-chapter structure. Adjust depth per scale (one-pager vs. enterprise tome).

### 1. Brand foundation
- **Mission, vision, values** — one paragraph each, plain English.
- **Personality and voice attributes** (preview of voice chapter).
- **Audiences** — who we serve, with specifics.
- **Positioning summary** — pulled from [[brand-strategist]] output.

### 2. Verbal identity
- **Tagline / positioning statement.**
- **Voice** — 4 attributes with do/don't pairs (full content from [[brand-voice-tone]]).
- **Tone register** — situation-to-tone matrix.
- **Naming conventions** — for products and features.
- **Glossary of terms we use / avoid.**
- **Filler-words ban list** — specific to this brand.

### 3. Logo
- **Primary mark** — full color, monochrome, knockout.
- **Lockups** — logo + tagline, vertical, horizontal.
- **Monogram / favicon** — the small-size variant.
- **Clearspace** — minimum margin around the mark.
- **Minimum size** — smallest acceptable usage.
- **On-light / on-dark / on-color rules.**
- **Misuse examples** — show what's banned (don't stretch, don't recolor, don't add effects, don't crop).

### 4. Color
- **Primary palette** — semantic naming, HEX, RGB, HSL.
- **Secondary palette.**
- **Neutrals** — surface and text.
- **Functional palette** — success, warning, error, info.
- **Tokens and contrast pairings** — WCAG AA / AAA verified.
- **Dark-mode rules** — separate values where needed.

### 5. Typography
- **Display, text, mono families.**
- **Type scale** — modular, with px and line-height per step.
- **Pairing and weight rules** — which face to use where.
- **License coverage** — what use cases are licensed.
- **Fallback stacks** for digital surfaces.

### 6. Imagery and illustration
- **Photography direction** — subject, lighting, composition, palette, treatment, style.
- **Do/don't mood references.**
- **Illustration style** — if commissioned.
- **Iconography style** — family, stroke weight, fill rule, sizing.

### 7. Motion (if applicable)
- **Easing curves** — custom defaults.
- **Duration scale.**
- **Reduced-motion behavior.**
- See [[motion-and-transitions]] for the full motion-system reference.

### 8. Applications
Per surface, show how the brand applies:
- **Web** (with screenshots / spec).
- **Mobile** (with screenshots / spec).
- **Deck templates** (16:9 master slides — see [[surface-deck-16x9]]).
- **Document templates** (8.5×11 — see [[surface-document-letter]]).
- **Social** (LinkedIn / Instagram — see [[surface-social-linkedin]] and [[surface-social-instagram]]).
- **Email** (signature, header, footer).
- **Swag and environmental** (if applicable).

### 9. Governance
- **Who owns the brand?** (specific role / team).
- **How to request approvals?**
- **Where do brand assets live?** (Figma library, internal SharePoint, brand portal).
- **How to flag brand drift?**
- **Refresh cadence** — when does the brand get re-audited?

## Sizing the guidelines to the brand

### One-pager (small / scrappy brand)
A single page covering:
- 3-attribute voice + 1 tagline.
- Logo + 3 colors + 2 fonts.
- One "do this not that" example.
- One contact for questions.

**Use when:** small startup, internal-only brand, fast turnaround. Don't over-engineer.

### Standard (most B2B brands)
6–12 pages or screens covering all 9 chapters at moderate depth.

**Use when:** small-to-mid companies, internal use plus partner/agency reference.

### Enterprise (large brand, multi-product)
30–60 pages or a full web-published brand portal. Each chapter at full depth, with template files and asset downloads.

**Use when:** enterprise organization, multiple sub-brands, broad agency network, regulatory requirements.

## Format selection

| Format | When |
|---|---|
| **PDF** (8.5×11 or A4) | Default for distribution. Use [[design-document]] + [[pdf]] skill for production. |
| **Web-published** (HTML / Notion / Confluence / brand portal) | Living document. Easier to update. Use when the brand will evolve. |
| **Figma file** | Designer-facing companion to the document. Always paired with one of the above. |
| **Mixed** | Web for living rules, PDF for partner/agency distribution, Figma for design execution. Most enterprise brands. |

## Authoring workflow

When asked to compose brand guidelines:

1. **Inventory inputs.** What's done? What's missing?
   - Strategy from [[brand-strategist]]?
   - Naming from [[brand-naming]]?
   - Voice from [[brand-voice-tone]]?
   - Identity system from [[brand-identity-system]]?
   - Photography / illustration direction?
2. **Fill gaps.** If anything is missing, route back to the relevant skill. Don't compose guidelines around incomplete content.
3. **Pick scale** — one-pager / standard / enterprise — based on brand context and team size.
4. **Pick format** — PDF / web / mixed.
5. **Outline the document** following the standard 9-chapter structure (or adapted version for one-pager).
6. **Compose each chapter** by lifting content from the source skills. Don't rewrite from scratch — reference the source content.
7. **Add governance.** Brand decisions need an owner. Document who.
8. **Hand off to design execution** — [[design-document]] for visual layout, [[docx]] / [[pdf]] for file generation, or web-publish workflow for living docs.
9. **Review with the brand owner** before publishing.

## Common guidelines failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Tome that nobody reads** | 90-page enterprise guidelines, never consulted | Right-size to brand context; add a 1-page summary at front |
| **One-pager that doesn't constrain** | Single page with 5 colors and a logo; doesn't answer real questions | Add do/don't examples, ban lists, application examples |
| **Vague voice section** | "Bold. Friendly. Modern." with no examples | Route to [[brand-voice-tone]]; rebuild with attributes + do/don't pairs |
| **No governance** | Document doesn't say who owns the brand | Add governance chapter with named owner |
| **Outdated colors / type** | Guidelines reference brand assets that have evolved | Audit and update; or move to web-published format for living updates |
| **Missing application examples** | Logo and colors documented, but no surface-specific application shown | Add Applications chapter with screenshots / spec |
| **No motion guidelines** | Brand has digital surfaces but no motion direction | Add motion chapter; route to [[motion-and-transitions]] |
| **Inconsistent file structure** | PDF guidelines, but design assets scattered across email and Slack | Centralize asset storage; document location in governance |
| **No dark-mode rules** | Brand has digital surfaces but only light-mode color values documented | Add dark-mode rules to color chapter |

## Hand-offs

- **[[brand-strategist]]**, **[[brand-naming]]**, **[[brand-voice-tone]]**, **[[brand-identity-system]]** — source content.
- **[[design-document]]** — for visual layout of PDF/Word guidelines.
- **`docx`** and **`pdf`** skills — for file generation.
- **[[design-imagery]]** — for photography direction within guidelines.
- **[[design-iconography]]** — for icon family documentation within guidelines.
- **`consulting-humanize`** — for cleanup of guidelines prose.

## Constraints

- Don't compose guidelines around incomplete content. Fill source gaps first.
- Don't bloat one-pager brands into enterprise tomes — over-engineering wastes the team's time.
- Don't skip governance. A brand without an owner drifts.
- Don't reference brand assets without documenting where they live.
- Don't produce guidelines without surface-specific application examples.
- Don't ship guidelines with a "Bold. Friendly. Modern." voice section. Voice needs do/don't pairs.
- For Slalom client work, brand guidelines often go through Slalom's IP team for legal review — coordinate the path before publishing externally.
