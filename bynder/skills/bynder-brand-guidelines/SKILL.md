---
name: bynder-brand-guidelines
description: Stand up, populate, and govern the Bynder Brand Guidelines module — sections, pages, content blocks, downloadables, embedded assets, and the publish/draft workflow. Covers IA principles for brand documentation, content patterns (do/don't, color, typography, voice), Bynder mechanics (block types, asset embeds, permissions), and the API for syncing guidelines with a code-based design system. Use this skill when setting up Brand Guidelines for the first time, restructuring an existing guideline set, syncing guidelines with a design-system source of truth, planning a brand launch / refresh in Bynder, or driving Brand Guidelines adoption as part of an optimization engagement.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-brand-guidelines]
tags: [type/skill, plugin/bynder, topic/bynder, topic/brand-guidelines]
status: active
---

# Brand Guidelines (Bynder module)

This skill puts you in the role of someone who runs Brand Guidelines as a real product — structured, governable, in sync with the design system, and adopted by the people who need it. Default posture: Brand Guidelines is the front door for non-technical contributors, and a Bynder implementation that doesn't activate it is leaving most of the platform's value on the table.

The Brand Guidelines module is a first-class Bynder surface, not a CMS knockoff. It has its own permission model, its own publish/draft state, its own URL surface, and direct embedding from the asset bank — meaning logos, color samples, photography examples, and downloadables stay automatically in sync with source-of-truth assets.

Pair with `bynder-asset-model` for the metaproperties that drive embedded-asset selection, `bynder-permissions-workflow` for who can edit guidelines vs. consume them, and `bynder-portal-architect` if multi-brand requires guideline-set duplication. For an optimization engagement where Brand Guidelines is under-utilized, route through `bynder-optimization-audit` first.

## When to use this skill

- Standing up Brand Guidelines for a new Bynder deployment.
- Restructuring an existing guidelines set that's grown unwieldy.
- Migrating brand-book PDFs into the Brand Guidelines module.
- Syncing guideline content (color tokens, type scale, spacing) with a code-based design system.
- Planning a brand refresh / launch where the new guidelines need to land in Bynder.
- Driving Brand Guidelines adoption as part of an optimization engagement.

## Core posture

- **Brand Guidelines is the front door for non-technical users.** Designers, agencies, freelance contributors, partners, marketing operators — most of them never touch the asset bank directly. They read the guidelines, then download the cleared asset.
- **Embed, don't duplicate.** Logos, color samples, and photography examples should embed from the asset bank, not be uploaded into the guideline. One source of truth.
- **Structure for browsability, not exhaustiveness.** A 200-page brand book in Bynder is unread. A 6-section, 30-page guideline with strong navigation is used daily.
- **Govern with publish/draft.** The guideline has its own lifecycle. Draft a brand refresh in parallel; publish on launch day.
- **Sync with the design system.** Color tokens, type scale, spacing rules — the values in the guideline and the values in code should never diverge. If they can be sourced once, source them once.

## Information architecture for brand guidelines

### The standard outline

Most brand guidelines map to a recognizable structure:

```
1. Brand foundation
   1.1 Mission, vision, values
   1.2 Brand personality / voice
   1.3 Audience

2. Logo and identity
   2.1 Primary logo
   2.2 Logo variations (mono, reversed, lockups)
   2.3 Clear space and minimum size
   2.4 Misuse (do/don't)

3. Color
   3.1 Primary palette (with hex / RGB / CMYK / Pantone)
   3.2 Secondary palette
   3.3 Accessibility (WCAG combinations)
   3.4 Application examples

4. Typography
   4.1 Primary typeface (display + body)
   4.2 Type scale and hierarchy
   4.3 Web vs. print specifications
   4.4 Web font licensing and download

5. Photography and imagery
   5.1 Photography style and direction
   5.2 Approved photographers / stock sources
   5.3 Treatment (filters, crops, focal points)
   5.4 Browse approved photography (embedded asset bank filter)

6. Voice and copy
   6.1 Tone principles
   6.2 Voice examples (do/don't)
   6.3 Vocabulary preferences and forbidden words
   6.4 Channel-specific voice (web, email, social)

7. Templates and downloads
   7.1 Presentation templates
   7.2 Email signature
   7.3 Document templates
   7.4 Brand asset bundles by channel

8. Governance
   8.1 How to request brand approval
   8.2 Who owns what
   8.3 Update cadence
```

Adapt to client brand — but resist the urge to ship a 200-section monster. Brand teams want a guideline they can point partners at, not a textbook.

### Sections, pages, blocks

Bynder's structure:

- **Section** — top-level navigational unit (`Logo`, `Color`, `Typography`).
- **Page** — a sub-page of a section (`Primary Palette` under `Color`).
- **Block** — the content units inside a page. Block types include text, image (embed from asset bank), video, asset card, downloadable, color swatch, divider, columns / layout container.

Build pages as block compositions. A `Color > Primary Palette` page is typically: heading text → swatch grid → usage notes text → accessibility note → downloadable swatch file (asset embed). That composition is reproducible and editable; a screenshot of the palette is not.

### Block patterns that work

| Pattern | Block composition |
|---|---|
| Do/don't comparison | Two-column layout → image (embed) + heading "Do" / image + heading "Don't" |
| Color swatch grid | Color swatch blocks in a grid layout, with a downloadable below |
| Logo variations | Image embeds (from asset bank) with caption text + clear-space overlay note |
| Photography style | Filter-driven asset embed (shows current approved photography) + style notes |
| Template gallery | Asset cards (PowerPoint, Keynote, Google Slides templates) with download buttons |
| FAQ / governance | Accordion / collapsible text blocks |

The filter-driven asset embed is the killer feature. Set the embed to "show all assets where `property_AssetCategory = Photography` AND `property_RightsStatus = Cleared`" and the guideline auto-updates as the photography library evolves.

## Bynder mechanics

### Permission model

Brand Guidelines has its own permission model, separate from the asset bank.

- **Editor** — can edit any section / page / block.
- **Section editor** — scoped editing (one section).
- **Reader** — view-only.
- **Public** — guidelines can optionally be exposed to anonymous users (use carefully; exposes embedded assets).

Slalom default: gate reader access to authenticated users (employees, partners) unless the brand is intentionally public-facing. Limit editor access to the brand-ops team (3–5 people max). Use section-editor scopes for distributed brand teams.

### Publish / draft / scheduled

Guidelines have a publish state independent of the asset bank.

- Draft a brand refresh in parallel with the live guideline.
- Use scheduled publish for launch-day cutover.
- Keep an archive of pre-refresh guidelines for partner reference (a Bynder `Archive` section, or a dated download bundle).

### Asset embeds

Embedded assets stay live — when you swap an approved photograph in the asset bank, the guideline reflects it. Two embed modes:

- **Specific asset** — hard reference to one asset ID. Good for the primary logo (you don't want it to change accidentally).
- **Filter-driven** — query (e.g., `property_AssetType = Logo AND property_Status = Approved`). Good for photography galleries, evolving libraries.

Use specific embeds for canonical assets (logo, primary color swatch) and filter embeds for living content (current photography, latest templates).

### Downloadables

Block type that wraps an asset (or asset bundle / collection) with a download button. Track downloads in Analytics — high-traffic downloadables are leading indicators of guideline relevance.

## Sync with a design system

The high-leverage move: keep color, type scale, and spacing values single-sourced.

### Source-in-code, push-to-Bynder

Pattern:

1. Design tokens live in code (Style Dictionary, design-tokens-cli, or a custom JSON).
2. A scheduled CI job converts tokens → Brand Guidelines blocks via the Brand Guidelines API.
3. The guideline pages auto-update when tokens change.

```ts
// pseudo: nightly-sync-tokens.ts
import { tokens } from './design-tokens';

await bynderRequest('/brandguidelines/pages/{color-page-id}/blocks/', {
  method: 'PUT',
  body: JSON.stringify(
    tokens.color.primary.map(c => ({
      type: 'colorSwatch',
      label: c.name,
      hex: c.hex,
      rgb: c.rgb,
      cmyk: c.cmyk,
    }))
  ),
});
```

### Source-in-Bynder, pull-to-code

Inverse pattern (use when the brand team owns tokens and the engineering team pulls):

1. Tokens live in Brand Guidelines color-swatch and typography blocks.
2. A build-time job pulls them via API and emits Style Dictionary input.
3. Code consumes the generated tokens.

Pick one direction per token category — colors might be Bynder-sourced, type scale code-sourced. What you cannot afford is "both". Pick a master.

See `bynder-foundations.md` and `api-surface.md` for the Brand Guidelines API endpoints.

## Output formats

### Brand Guidelines structure proposal

```
# Brand Guidelines Structure: [Brand]

## Goals
[Audience, frequency of update, expected adoption surfaces.]

## Section / page outline
1. [Section name]
   1.1 [Page name] — [block composition summary]
   1.2 [Page name] — ...
2. [Section name]
   ...

## Embedded asset strategy
[Per page: specific embed vs. filter-driven; metaproperty filters used.]

## Permission plan
- Editors: [list]
- Section editors: [list with scopes]
- Readers: [authenticated org / public / partner]

## Sync-with-design-system plan
[Direction per token category: Bynder-sourced or code-sourced. Tooling.]

## Adoption levers
[Microsoft Teams / Slack search integration, email signature with guideline link, partner onboarding kit.]

## Update cadence
[Quarterly review, brand-refresh windows, ownership.]
```

## Common anti-patterns to call out

- **PDF dump**. Migrating a 200-page PDF brand book into a single Brand Guidelines page. Loses navigation, search, embeds, and update lifecycle.
- **Hard-coded swatch images.** A screenshot of the color palette doesn't update when the palette evolves. Use color-swatch blocks.
- **Public guidelines without thinking through what's embedded.** Public exposes embedded assets too — accidental brand-restricted content exposure.
- **No download tracking.** A guideline with no analytics is a guideline you can't optimize. Use Analytics API to identify under-used sections.
- **One-time launch with no maintenance plan.** Brand Guidelines that don't move stop being trusted within a quarter. Set a review cadence.
- **Tokens drifting between code and Bynder.** Worse than two sources of truth — it's two contradictory ones. Pick a master per category.

## Constraints

- Do not recommend duplicating the brand book in Bynder if Brand Guidelines is enabled — that's what the module is for.
- Do not propose public guidelines without a deliberate review of what's embedded.
- Do not skip the permission walk-through with the brand-ops team.
- Do not assume a single guideline structure works across multi-brand — `bynder-portal-architect` covers the multi-brand split.

## How this skill relates to others

- The metaproperty filters that drive filter-based embeds → `bynder-asset-model`.
- The permission profiles that govern editor access → `bynder-permissions-workflow`.
- Multi-brand or sub-portal guideline split → `bynder-portal-architect`.
- The API patterns for sync-with-design-system → `bynder-js-sdk` (with Brand Guidelines fallback to plain `fetch` since the SDK doesn't wrap Brand Guidelines yet).
- Adoption-driving connectors (Microsoft 365, Slack) that surface guidelines in daily-use tools → `bynder-marketplace-connectors`.

## Source material

- Bynder Brand Guidelines product page: https://www.bynder.com/en/products/brand-guidelines/
- Brand Guidelines API: https://api.bynder.com/docs/ (Brand Guidelines section)
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
- Brand-system canon: Wheeler, *Designing Brand Identity*; Mollerup, *Marks of Excellence*.
