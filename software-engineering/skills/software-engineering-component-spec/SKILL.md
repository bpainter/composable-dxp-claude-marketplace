---
name: software-engineering-component-spec
description: Document a UI component (primitive or composed block) for the project's design system — anatomy, props, variants, states, do/don't, accessibility, examples, and changelog. The standard pattern every system component follows. Use this skill any time the user is documenting a component, writing a component README, adding a Storybook MDX page, contributing a new system component, or auditing existing component docs — "document this component," "write a spec," "add MDX docs," "what does this component do" — even when "spec" isn't said. Trigger whenever a component's contract needs to be captured so designers, engineers, and authors all consume the same definition.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-component-spec]
tags: [type/skill, plugin/software-engineering topic/software, topic/engineering]
status: active
---

# Component Spec

A component spec is the definition contract between the people who design a component, the people who implement it, the people who consume it across screens, and the people who maintain it over time. A component without a spec is a guess; future contributors will guess differently than you did.

This skill produces the canonical doc for any component in the project's design system — primitives in `components/ui/` and composed blocks in `components/blocks/`. Pair with `brand-identity-system` for the system-level governance these specs sit inside, with `software-engineering-shadcn-component` for the primitive layer specifically, with `software-engineering-tailwind-tokens` for the tokens specs reference, with `software-engineering-a11y-audit` for the accessibility validation each spec calls for, and with `design-handoff` (which produces ad-hoc handoff annotations — different artifact, complementary purpose).

## When to use this skill

- Documenting a new component being added to the system.
- Backfilling docs for an existing component that has none.
- Reviewing or updating an existing spec.
- Onboarding a new contributor; the spec is what they read first.
- Auditing the system for components missing required documentation.

## Core posture

- **One spec per component.** Lives next to the component code, ideally as a Storybook MDX file or a README in the component folder.
- **The spec is the source of truth for the contract.** What the component does, what props it takes, what variants exist, what states it has. If a code change diverges from the spec, one of the two is wrong — fix.
- **Examples are not optional.** Three to five real uses across the product. Without them, the spec is theory.
- **Accessibility is a section, not a footnote.** Each component captures its keyboard, ARIA, contrast, and motion behavior.
- **Changelog matters.** A component's API drift over time is the single most common source of "I thought this took X" bugs.

## The spec template

This is the canonical structure every spec follows. Adapt for primitives vs. blocks where noted.

```markdown
# [Component Name]

> [One-sentence purpose. What problem does this solve?]

**Status**: [Stable | Beta | Deprecated]
**Version**: [x.y.z]
**Owner**: [name or team]
**Source**: `components/ui/component.tsx` (or `components/blocks/...`)

## Anatomy

[Labeled diagram or annotated screenshot showing the component's parts.
For primitives: include all variants. For blocks: show the canonical
composition with parts labeled.]

```
┌────────────────────────────────────┐
│ Container                          │
│ ┌──────────────┐  ┌──────────────┐ │
│ │ LeadingIcon  │  │ Label        │ │
│ └──────────────┘  └──────────────┘ │
└────────────────────────────────────┘
```

## When to use

[2-4 sentences. The decision context: "Use [Component] when [situation].
Don't use it when [adjacent situation] — use [other component] instead."]

## Variants

| Variant | Purpose | Example |
|---|---|---|
| Default | Standard use | [example] |
| Secondary | Lower visual weight | [example] |
| Destructive | Confirms destructive action | [example] |

[For each variant, name the prop and the use case.]

## Sizes

[If applicable. sm, md (default), lg, etc. Each tied to a token / type-scale step.]

## States

| State | Visible spec | Trigger | Notes |
|---|---|---|---|
| Default | bg-primary, text-primary-foreground | n/a | Standard render |
| Hover | bg-primary/90 | pointer-over | |
| Focus | ring-2 ring-ring | tab / click | Visible at all times when focused |
| Active | bg-primary/80 | press | |
| Disabled | opacity-50, cursor-not-allowed | `disabled` prop | aria-disabled when applicable |
| Loading | spinner, label hidden | `loading` prop (where applicable) | Use Loading from system, not custom |

## Props

\```tsx
type ComponentProps = {
  variant?: "default" | "secondary" | "destructive";
  size?: "sm" | "md" | "lg";
  // ...
};
\```

| Prop | Type | Default | Description |
|---|---|---|---|
| `variant` | `"default" \| "secondary" \| "destructive"` | `"default"` | Visual variant |
| `size` | `"sm" \| "md" \| "lg"` | `"md"` | Component size |
| `disabled` | `boolean` | `false` | Disables the component; sets aria-disabled |
| `children` | `ReactNode` | required | Component content |
| `className` | `string` | `""` | Extends component classes (forwarded via `cn`) |
| `...rest` | `HTMLAttributes` | — | Forwarded to the root element |

[For blocks consumed from Contentful, also note which props map to
which Contentful fields.]

## Tokens used

| Token | Where applied |
|---|---|
| `color/brand/primary` | Background |
| `color/brand/primary-foreground` | Text |
| `space/200` | Internal padding |
| `radius/md` | Border radius |
| `motion/duration/fast` | Hover transition |

## Accessibility

- **Role / element**: [e.g., `<button>` for Button, `role="dialog"` for Dialog]
- **Keyboard**: [Tab / Shift+Tab / Enter / Space / Escape / Arrow — what each does]
- **ARIA**: [aria-label, aria-describedby, aria-expanded, aria-controls — what to set and when]
- **Focus management**: [where focus goes on open/close, trap behavior, return focus]
- **Contrast**: [confirmed AA at default and dark themes]
- **Motion**: [respects prefers-reduced-motion; specifics]
- **Touch target**: [min 44 × 44 px confirmed]

## Do / Don't

**Do**
- [Pattern 1 — concrete, with example]
- [Pattern 2]
- [Pattern 3]

**Don't**
- [Anti-pattern 1 — concrete, with example. Common misuse.]
- [Anti-pattern 2]

## Examples

### Example 1: [name]

\```tsx
<Component variant="default" size="md">
  Continue
</Component>
\```

[Brief context on where this is used in the product.]

### Example 2: [name]

\```tsx
// Form-bound, with server action
<form action={submitAction}>
  <Component type="submit">Submit</Component>
</form>
\```

[2-4 examples covering the main usage patterns.]

## Composition rules

[How this component composes with others. What it can contain.
What containers it lives inside. Any forbidden combinations.]

## Related components

- [Sibling component]: when to use one vs. the other
- [Parent / child relationships]
- [Replacement for older patterns]

## Performance notes

[If applicable. Rendering cost, bundle size implications, lazy-loading
strategy.]

## Changelog

| Version | Date | Change |
|---|---|---|
| 1.2.0 | 2026-04-15 | Added `loading` prop |
| 1.1.0 | 2026-03-20 | Added `destructive` variant |
| 1.0.0 | 2026-02-10 | Initial release |

## Open questions

[Any known gaps, deferred decisions, or areas under review. Better to
surface than hide.]
```

## Adapting for primitives vs. blocks

**For primitives** (shadcn-vended components in `components/ui/`):

- Source line points to the file but notes that the primitive is vendored from shadcn (re-add via `pnpm dlx shadcn@latest add ...`).
- Examples emphasize the wrap-don't-fork pattern from `software-engineering-shadcn-component`.
- Variants section captures any local additions (e.g., a `ghostDestructive` variant added beyond the registry).
- Changelog flags `LOCAL` divergences so re-adds via the CLI don't blow them away.

**For composed blocks** (`components/blocks/`):

- Anatomy emphasizes which primitives the block uses.
- Props section maps to Contentful fields when the block is content-driven (Phase 2; see `dev-contentful-model` and `dev-contentful-wrapper`).
- Composition rules section is more important — blocks often have specific containment expectations.
- Examples cover both the in-code use and the Contentful-authored use where applicable.

## Where the spec lives

Two acceptable patterns:

**Storybook MDX (preferred when Storybook is set up)**:

```
components/
  ui/
    button.tsx
    button.stories.tsx
    button.mdx
```

The MDX file is what designers and engineers both reference. Storybook stories cover each variant and state visually; MDX provides the prose context.

**README in the component folder**:

```
components/
  blocks/
    download-card/
      download-card.tsx
      download-card.stories.tsx
      README.md
```

For Phase 1 (lighter docs), README.md is fine. Migrate to MDX as Storybook matures (Phase 2).

## Integration with Storybook stories

Each story in the `.stories.tsx` file corresponds to a state or variant. The MDX or README references the stories so the spec and the visual stay in sync.

```tsx
// button.stories.tsx
import type { Meta, StoryObj } from "@storybook/react";
import { Button } from "./button";

const meta: Meta<typeof Button> = {
  title: "UI/Button",
  component: Button,
};
export default meta;

type Story = StoryObj<typeof Button>;

export const Default: Story = { args: { children: "Continue" } };
export const Destructive: Story = { args: { variant: "destructive", children: "Delete" } };
export const Disabled: Story = { args: { disabled: true, children: "Continue" } };
export const Loading: Story = { args: { loading: true, children: "Saving..." } };
// ...one story per variant + state combination that matters
```

These stories also drive visual regression in CI (Chromatic), per `software-engineering-quality-engineer`.

## Common failure modes

- **Specs that describe what the code does line-by-line.** The code is the line-by-line; the spec is the contract. Stay at the contract level.
- **Skipping accessibility section.** Most expensive omission. Backfilling later means re-doing the implementation.
- **No examples or stale examples.** A spec without real product examples is theory. Update the examples when the product changes.
- **No changelog.** Three contributors later, nobody knows which version added the `loading` prop, and bugs cluster around that ambiguity.
- **One mega-spec for many components.** Each component gets its own spec. Even if they're related (e.g., Button and IconButton), they get separate docs.
- **Specs that diverge from the implementation.** When the code changes, the spec gets updated in the same PR. Otherwise drift.
- **Forgotten do/don't section.** This is where the most useful guidance lives — the failure modes the team has already encountered.
- **Treating Storybook stories as the spec.** Stories show visuals; specs explain the contract. Both are needed.
- **No status / version / owner header.** Reviewers can't tell if the spec is current. Always include the metadata block.

## Spec authoring checklist

Before merging a spec:

- [ ] Anatomy diagram or annotated screenshot present.
- [ ] When-to-use clear; alternatives named.
- [ ] All variants documented with prop name and use case.
- [ ] All sizes documented (if applicable).
- [ ] All required states (default, hover, focus, active, disabled, plus applicable: loading, error, empty, success) covered.
- [ ] Props table complete and matches the TypeScript types.
- [ ] Tokens used section lists every design token consumed.
- [ ] Accessibility section covers role, keyboard, ARIA, focus management, contrast, motion, touch target.
- [ ] Do / don't has at least 3 of each.
- [ ] 3-5 real product examples.
- [ ] Composition rules and related components.
- [ ] Changelog updated for this version.
- [ ] Storybook stories cover at minimum the variants and states above.

## Outputs

The spec itself, as MDX or README, lives next to the component code and is referenced from the system's component reference index.

For the system-level catalog of components, see `brand-identity-system`. The component reference index is the table of contents of all specs.

## How this skill relates to others

- The system-level catalog and governance these specs feed → `brand-identity-system`.
- Primitive-specific implementation patterns → `software-engineering-shadcn-component`.
- Tokens that specs reference → `software-engineering-tailwind-tokens`.
- Accessibility validation each spec calls for → `software-engineering-a11y-audit`.
- Test coverage that pairs with each spec → `software-engineering-quality-engineer` (component tests, visual regression).
- Handoff annotations (different artifact: ad-hoc per-screen rather than per-component canonical) → `design-handoff`.
- Content-driven blocks integrate with Contentful → `dev-contentful-model`, `dev-contentful-wrapper`.

## Source material

- Storybook MDX docs: https://storybook.js.org/docs/writing-docs/mdx
- Storybook component story format: https://storybook.js.org/docs/api/csf
- WAI-ARIA Authoring Practices: https://www.w3.org/WAI/ARIA/apg/
- Material Design and GOV.UK Design System component documentation as references for thoroughness, not visual style.
