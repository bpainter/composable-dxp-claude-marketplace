---
description: Generate a dev-ready handoff package — annotations, component identification, interactive states, design-token references, motion specs, content guidance, edge cases

# Project context
type: command
project: skills-library
plugin: design
tags: [type/command, plugin/design]
status: active
---

Generate a complete, dev-ready handoff package for an implemented design.

Steps:

1. **Confirm the design is in Phase 4 or beyond** ([[design-process]]). If still in exploration or design phases, run `audit-system` first.

2. **Run [[design-handoff]]** to author the annotation set:
   - **Component identification** — every shadcn / system primitive used, with names matching [[shadcn-component-inventory]] or the project's design system.
   - **Interactive states** — default, hover, active, focus, disabled, loading, error, success. Per [[interaction-patterns]] §1.
   - **Design-token references** — colors as semantic tokens (`--color-primary`, not `#3B82F6`), spacing on the grid, type styles by name.
   - **Interaction behavior** — what triggers what; dismissal patterns; keyboard support per [[interaction-patterns]].
   - **Motion specs** — duration, easing, sequence, reduced-motion alternative. Cite [[motion-and-transitions]] named patterns where applicable.
   - **Content guidance** — copy, voice, error wording.
   - **Edge cases** — empty states, error states, loading states, long content, RTL if applicable.

3. **Cross-cite the canonical references** so engineering implements from the same source:
   - shadcn primitives → [[shadcn-component-inventory]].
   - Blocks → [[shadcn-block-inventory]].
   - Charts → [[shadcn-chart-inventory]].
   - Motion → [[motion-and-transitions]] (defaults to Sonner for toasts, Vaul for drawers).
   - Anti-bland gates → [[ai-tells-forbidden-patterns]].

4. **Run [[pre-delivery-checklist]]** with handoff focus: every interactive element has all states; every animation has reduced-motion alternative; every form field has labels and error placement; every figure has alt text.

5. **Update artifact metadata** ([[design-process]] Rule 9): status = "Handed Off"; version bumped; linked story / epic.

6. **Hand off** to `software-engineering-design-implementation` (the skill) or `software-engineering-frontend-developer` (the implementer).

Output format:

```
# Handoff: {Screen / Component} — v{X.X}
Linked: {Ticket ID} | Date: {YYYY-MM-DD} | Designer: {Name}

## Component inventory
- {primitive 1} (shadcn / system component name)
- {primitive 2}
- ...

## States
| Element | Default | Hover | Active | Focus | Disabled | Loading | Error |
| ... | ... | ... | ... | ... | ... | ... | ... |

## Design tokens
- Color: --color-x, --color-y
- Spacing: 8pt grid
- Type: heading/lg, body/md, caption/sm

## Interactions
- {trigger} → {behavior}
- {dismissal pattern: mouse / keyboard / focus-out / outside-tap}

## Motion
- Component: {name}
- Duration / easing: {ms} / {curve} (cite motion-and-transitions named pattern)
- Reduced-motion alternative: {opacity-only or instant}

## Content
- Copy: {final copy or placeholder with note}
- Error messages: {specific wording}

## Edge cases
- Empty state: {treatment}
- Error state: {treatment}
- Loading state: {skeleton / spinner / progress}
- Long content: {truncation, wrap, scroll}
```
