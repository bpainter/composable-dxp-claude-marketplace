---
name: design-screen
description: Compose a new screen or update an existing screen in a design tool by reusing the project's design system — published components, semantic tokens, and defined text and effect styles — rather than building from primitives. Tool-agnostic (Figma, Sketch, Penpot, etc.). Use this skill any time the user wants to design a screen, update a screen, build a page, or translate a brief or reference into a designed view, even when they don't say "design system" or "compose." Trigger whenever the deliverable is a designed screen so the result is system-faithful, traceable, and ready for handoff on the first pass.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-screen]
tags: [type/skill, plugin/design, topic/design, topic/ui-ux]
status: active
---

# Design a Screen

This skill produces designed screens by composing existing design-system components, tokens, and styles — not by drawing primitives with hardcoded values. The default move is to find before you build.

The composition hierarchy, in order of preference:

1. **Existing project design-system components** — if the team has its own DS, use it first
2. **Pro Blocks** (shadcndesign Pro Blocks, shadcnblocks, shadcn.io) — when the team's DS doesn't cover a section, pre-built blocks are the next best source. They ship Figma + React + Tailwind versions, so design and code stay aligned.
3. **shadcn/ui core primitives** — when no block fits but a primitive does (button, card, dialog, etc.)
4. **Custom components** — last resort, only when the unique brand value of the screen requires it

Where a needed component or token does not yet exist in the team's DS, propose it as a system addition rather than implementing ad hoc. Pair with `design-process` for the always-on rules, with `design-handoff` for the annotation pass that follows screen design, and with `design-audit` to spot-check DS fidelity before handoff.

For the engineering side of this composition (installing pro-blocks, integrating with project tokens, semantic CSS), the cross-plugin skill is `software-engineering:pro-blocks-composer`. Designers using this skill should be aware that engineering composes from the same source so design and implementation stay aligned.

## When to use this skill

- Designing a new screen or page from a brief, reference, or content source.
- Updating an existing screen with new content, new states, or new variants.
- Translating a wireframe or rough sketch into a hi-fi designed screen.
- Composing a marketing page from copy and a brand brief.

If the task is creating brand identity (naming, logo, palette), use `brand-designer` instead. If the task is generating production code, use `dev-design-implementation` instead. If the task is reviewing a screen for system drift, use `brand-design-audit` instead.

## Prerequisites

- An established design system with components, tokens, and styles, *or* an explicit acknowledgment that the system is partial and proposed additions will be flagged.
- A brief or reference that describes the screen's purpose, structure, and content.
- Source content (copy, data, imagery references) to populate the screen.

If any of these are missing, push back before composing. A screen built without a brief produces rework.

## The required workflow

Run these steps in order. This skill operates inside Phase 3 (Design) of `brand-design-process`. The Define and Explore phases happen *before* you reach this skill.

### Step 1: Understand the screen

- Read the brief or reference end-to-end.
- Identify the major sections (hero, content panels, lists, footer, etc.).
- For each section, list the UI components likely needed (buttons, inputs, cards, navigation, accordions).
- Note any media — images, illustrations, icons — that need to be sourced before composition begins.

If the brief is unclear, surface the ambiguity now. Designing through unclear requirements is the most common source of rework on this stack.

### Step 2: Discover the design system

You need three things from the system before placing anything:

#### 2a — Components

- If the project already has existing screens using the design system, treat those as authoritative — inspect what components and variants are already in use. Match conventions.
- If no precedent exists, search the system library broadly. Try multiple terms and synonyms (button, input, nav, card, accordion, header, tag, avatar, toggle, icon).
- For each component, note which text, icon, and state properties it exposes so content overrides land cleanly.

#### 2b — Tokens

- Identify semantic color tokens for backgrounds, text, borders, interactive states.
- Identify spacing tokens for padding, gap, and the layout grid.
- Identify border-radius tokens.
- Never use raw hex values or arbitrary pixel values when a token exists.

#### 2c — Styles

- Identify text styles that map to the screen's typographic needs (heading, body, label, caption).
- Identify effect styles (shadows, elevation levels).

If a needed component, token, or style is missing from the system, document it and either propose the addition (preferred) or use a clearly flagged stand-in (acceptable when the system team's review will follow).

### Step 3: Create the page wrapper first

- Create the top-level page frame with the correct dimensions and layout direction *before* placing any sections inside it.
- Position it away from existing content on the canvas.
- Do not build sections as separate top-level frames and re-parent them later — build each section directly inside the wrapper. Re-parenting causes layout drift and broken auto-layout.

### Step 4: Build each section inside the wrapper

Build one section at a time. For each section:

1. Create the section frame with correct layout, spacing tokens, and background token. No hardcoded values.
2. Place component instances from the design system for all UI elements.
3. Override component content (text, icons, images) using the component's exposed properties — do not detach the component or modify it locally.
4. Check the brief for default prop values; do not assume the first visible variant is the right one.
5. Validate the section visually before moving to the next.

What to build manually vs. what to import:

| Build manually | Import from system |
|---|---|
| Page wrapper frame | Components: buttons, cards, inputs, navigation |
| Section container frames | Tokens: colors, spacing, radii |
| Layout grids (rows, columns, alignment) | Text styles |
| | Effect styles |

### Step 5: Validate the full screen

After composing all sections:

1. Review the full screen against the brief or reference.
2. Review each section individually, not just at zoomed-out full-page view. Reduced-zoom views hide truncation, wrong variants, and unoverwritten placeholder content.
3. Look for:
    - Cropped or clipped text.
    - Overlapping elements.
    - Placeholder text still showing ("Label," "Title," "Button").
    - Wrong component variants.
    - Missing or blank image areas.
4. Fix issues with targeted edits. Do not rebuild the screen.

Run a quick fidelity self-check (`brand-design-audit` is the formal version):

- All colors token-bound.
- All spacing on the grid.
- All type uses defined styles.
- All components are instances, not detached.
- All content overrides applied (no placeholders).

### Step 6: Updating an existing screen

When the task is an update rather than a new build:

1. Review the existing screen structure to understand what is there.
2. Identify which sections need updating and which can stay.
3. For each section needing changes:
    - Locate the existing elements by name or position.
    - Swap component instances if the system component changed.
    - Update content overrides, variant properties, or layout as needed.
    - Remove deprecated sections.
    - Add new sections.
4. Validate after each change.

Avoid bulk find-and-replace at the file level; updates landed section-by-section are easier to review and revert.

## Best practices

- **Search before building.** Almost always the system has the component, token, or style. Manual construction should be the exception.
- **Search broadly.** A "navigation pill" might be under "pill," "nav," "tab," or "chip." Try synonyms.
- **Tokens over hardcoded values.** Every color, spacing value, and radius should reference a token. This keeps the screen linked to the system; theme changes flow automatically.
- **Component instances over custom frames.** Instances stay linked to the source; system updates propagate.
- **Section by section, not whole-screen-at-once.** Catches issues early.
- **Validate after each section.** Prevents end-of-build rework.
- **Match existing conventions.** If the file already has screens, match their naming, sizing, and layout.

## Hard stops

- Never hardcode a color, spacing value, or border radius when a token exists.
- Never detach a component instance to make a local edit. Propose a system variant instead.
- Never build a section as a top-level frame and re-parent it.
- Never skip Step 1 — design without understanding the brief produces rework.
- Never leave placeholder content in the final deliverable.

## Output format

A completed screen deliverable includes:

```
[Screen Name] — v[X.X] — [Status]
Linked: [Ticket ID] | Updated: [Date] | Designer: [Name]

Sections:
  - [Section 1 name] — components used, token bindings confirmed
  - [Section 2 name] — components used, token bindings confirmed
  - ...

Component inventory:
  - List of every design-system component instance used
  - Any proposed new components flagged for system review

Content overrides applied:
  - All placeholder text replaced with real content
  - All image areas populated or flagged as pending

Design-system fidelity:
  - Token checklist passed
  - Layout grid confirmed
  - Text styles confirmed
  - No hardcoded values
```

This output meets the metadata requirements of `brand-design-process` Rule 9 and is ready to advance to the Review phase, then to `brand-design-handoff`.

## How this skill relates to others

- Always-on rules and the workflow this skill sits inside → `brand-design-process`.
- Brand identity (naming, palette, type system, voice) → [[brand-designer]].
- Annotation pass that follows screen design → `brand-design-handoff`.
- Fidelity review of a screen → `brand-design-audit`.
- Translating designed screens into production code → `dev-design-implementation`.
- UX copy cleanup → [[consulting-humanize]].
- Stories that drive screen design → `pm-user-story`.
