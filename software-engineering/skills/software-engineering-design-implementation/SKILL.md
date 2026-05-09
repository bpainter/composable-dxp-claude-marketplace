---
name: software-engineering-design-implementation
description: Translate a design artifact (Figma screen, component, or flow) into production-ready code with visual parity and design-system fidelity. Covers the full handoff workflow: capturing the visual reference, sourcing assets, mapping design tokens to code tokens, reusing existing components, validating against the design at the end. Use this skill any time the user is implementing a design — "build this," "implement this component," "match this design," "translate this Figma to code" — even when they don't name the workflow. Trigger whenever the deliverable is code that has to look like a design so visual parity, token fidelity, and accessibility hold up on the first pass.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-design-implementation]
tags: [type/skill, plugin/software-engineering topic/software, topic/engineering]
status: active
---

# Implement a Design

This skill is the design-to-code handoff workflow. It covers the *process* of translating a design artifact into production code — capture, source, translate, validate. The patterns and conventions for the code itself live in `software-engineering-nextjs-scaffold` (for App Router conventions) and `software-engineering-shadcn-component` (for UI primitives). This skill is the layer above those.

The default target stack is Next.js + Tailwind + shadcn/ui. When the design system the skill consumes is incomplete, escalation paths below cover the gap.

Pair with `software-engineering-nextjs-scaffold` and `software-engineering-shadcn-component` for code patterns, with `design-handoff` for the annotations this skill consumes, with `design-process` (which loads `design-taste`) for the design rules the implementation has to honor, with `software-engineering-quality-engineer` for the test pass that follows, with `software-engineering-tailwind-tokens` for the token mapping layer, and with `software-engineering-a11y-audit` for the accessibility validation pass.

## Cross-plugin references (single source of truth)

The design plugin maintains the canonical inventories and motion-craft references this skill consumes:

- **[[shadcn-component-inventory]]**, **[[shadcn-block-inventory]]**, **[[shadcn-chart-inventory]]**, **[[shadcndesign-pro-blocks-inventory]]** at `../../../design/references/` — what to reach for during translation.
- **[[motion-and-transitions]]** at `../../../design/references/` — the Animation Decision Framework, named bans, component-specific patterns (button scale 0.97, popover origin-aware, tooltip skip-delay, asymmetric enter/exit), CSS transform tactics, the Sonner Principles, performance rules. Adapted from Emil Kowalski and Hyperframes. Cite when motion is part of the design.
- **[[ai-tells-forbidden-patterns]]** at `../../../design/references/` — the named bans (LILA, Inter, Jane Doe, scale(0), ease-in, transition: all, keyframe-on-rapid-trigger). Implementation must clear these.
- **[[design-taste]]** in `../../../design/skills/design-taste/` — read MOTION_INTENSITY before implementing animation. The dial decides the budget.
- **[[pre-delivery-checklist]]** at `../../../design/references/` — Step 6 (Validate) runs this checklist.

## When to use this skill

- Implementing a new screen, page, or component from a design artifact.
- Updating an existing implementation to match a revised design.
- Building a component from a Figma file or design redline.
- Reviewing implementation work against a design reference.
- Onboarding a new engineer to the design-to-code workflow on this project.

If the task is purely architectural (system topology, integration design), use `software-engineering-composable-architect`. If the task is writing tests for the result, use `software-engineering-quality-engineer`.

## Prerequisites

- A finalized or approved design artifact (screen, component, or flow). "Finalized" means it has passed through Phase 4 (Review) of `design-process`; "approved" means the dev-lead acceptance gate has been met.
- Access to handoff annotations produced by `design-handoff` (or equivalent spec notes).
- Access to the project's design system or component library — codebase tokens, shadcn primitives, composed blocks.
- Understanding of project conventions: routing, state management, data-fetch patterns, styling approach.
- All required assets sourced (images, icons, illustrations).

If any of these are missing, do not start implementation. Surface the gap.

## The workflow

### Step 1: Understand the design

Before writing any code:

1. Review the design artifact in full — layout, hierarchy, spacing, all interactive states.
2. Identify every component, state, and variant shown.
3. Read the handoff annotations end-to-end. The annotations supply the contract; the visual is just the picture.
4. List open questions explicitly. Do not make silent assumptions.

If annotations are missing, request them via `design-handoff` before starting. Implementing without annotations produces drift.

### Step 2: Capture the visual reference

- Save or note the design as a visual reference to compare against throughout implementation.
- For multiple states (hover, active, disabled, empty, error, loading), capture all of them. Not just the default.
- This reference is the source of truth for visual validation in Step 6.

For Figma, exporting a PNG of each state is fine. For more nuanced parity, screen-recording the prototype interactions captures motion timing and transitions that PNGs cannot.

### Step 3: Source required assets

Before building:

- Identify every image, icon, illustration, and media asset needed.
- Source from the approved project asset library or design handoff.
- Do not import new icon packages or asset libraries that are not already in the project.
- Do not use placeholder assets in the final build. They get shipped.

Assets are typically delivered through the project's asset library (e.g., a CMS-managed Asset library or a project-managed `public/` directory).

### Step 4: Translate to project conventions

Translate the design into the project's framework, styles, and patterns. The goal is *visual intent in production patterns*, not pixel-by-pixel transcription.

#### Map design tokens to code tokens

Design tokens (color, spacing, type, radius, shadow, motion) must map to the project token system. For Next.js + Tailwind + shadcn/ui:

- Design `color/brand/primary` → CSS variable `--primary` → Tailwind `bg-primary`.
- Design `space/200` → Tailwind spacing scale (e.g., `p-2`, `gap-4`).
- Design `type/heading/lg` → Tailwind type scale (`text-3xl font-semibold`).
- Design `radius/md` → Tailwind `rounded-md`.

If a design token has no code equivalent, either add it to the project token system (preferred) or escalate to design-system review before implementing a one-off. Never hardcode a value in the component.

For details on the token-to-Tailwind mapping, see `software-engineering-shadcn-component` (theme via CSS variables) and `software-engineering-tailwind-tokens` when built.

#### Reuse existing components

Always use components from the project's design system when one exists.

- Need a button → use the shadcn `Button` primitive, not a custom `<button>`.
- Need a form pattern → use the shadcn `Form` primitive with react-hook-form.
- Need a composed block (Hero, FAQ, etc.) → check `components/blocks/` first.

If the design calls for a variant that doesn't exist, follow the wrap-don't-fork pattern from `software-engineering-shadcn-component`. Do not edit `components/ui/` for one-offs.

#### Follow project structure

- Place the component in the project's designated directory.
- Follow naming conventions.
- Avoid inline styles unless required for genuinely dynamic values.
- Respect existing routing, state management, and data-fetch patterns.

### Step 5: Achieve visual parity

Strive for visual accuracy with the source design.

- Prioritize fidelity within the constraints of the tech stack.
- When design specs conflict with design-system tokens, **prefer the system tokens** for consistency. Adjust spacing or sizing minimally to maintain visual intent.
- Implement accessibility requirements (contrast, focus states, screen-reader behavior, keyboard navigation, reduced-motion) even if not explicitly in the annotation. WCAG 2.2 AA is the floor.
- Document any intentional deviations from the design with a comment explaining why.

#### Common deviation reasons (acceptable)

- Token system has a slightly different value than the design spec — round to the token.
- Accessibility forces a contrast or sizing change.
- Performance requires a different approach (e.g., using next/image instead of a manually styled `<img>`).
- The design specifies a behavior that conflicts with the framework (e.g., a CSS-only effect that would require client-side state).

#### Unacceptable deviations

- Skipping a state because it was inconvenient.
- Hardcoding values to match a design when the token system covers it.
- Omitting accessibility because the design didn't show focus or hover states.
- Inventing interaction behavior the annotation didn't specify.

### Step 6: Validate against the design

Before declaring implementation complete, validate against the visual reference from Step 2.

#### Validation checklist

- [ ] Layout matches: spacing, alignment, sizing.
- [ ] Typography matches: family, size, weight, line-height.
- [ ] Colors use the correct tokens.
- [ ] All interactive states implemented: hover, active, disabled, focus, error, loading, empty, success.
- [ ] Responsive behavior matches the design's breakpoint intent.
- [ ] All assets render correctly.
- [ ] Accessibility validated (WCAG 2.2 AA): contrast, focus order, ARIA, touch targets, reduced motion.
- [ ] No hardcoded visual values (everything via tokens or system values).
- [ ] No placeholder content remains.

For systematic validation, the implementation is also subject to the `software-engineering-quality-engineer` test plan — visual regression on Storybook stories, component tests for state coverage, accessibility scan in CI.

### Implementation rules summary

- Use components from the project's design system whenever one exists.
- Map design tokens to project tokens; do not hardcode visual values.
- When a matching component exists, extend it (wrap), don't recreate it.
- Document any new components added to the system.
- Extract repeated values to constants or tokens — no magic numbers.
- Keep components composable and reusable.
- Add types for component props in TypeScript.
- Include JSDoc comments on exported components.

## Common issues and solutions

**Design doesn't match after implementation.**
- Compare side-by-side with the visual reference from Step 2.
- Check spacing, color tokens, and typography values against the design spec.
- Often the difference is one mis-mapped token. Walk the property tree.

**An asset is missing.**
- Flag immediately. Do not use a placeholder in production.
- Request the correct asset from the designer or asset library.
- For projects with a CMS, the CMS becomes the asset source of truth once it's wired up.

**Design token values differ from the system.**
- Prefer the system tokens for consistency.
- Adjust sizing minimally to maintain visual intent.
- File a discrepancy note for the designer to review. Do not silently override.

**A state is missing from the design.**
- Flag missing states (error, empty, loading, disabled) to the designer before implementing.
- Do not invent states. Confirm with the design team via `design-handoff` review.

**Annotation references a component that does not exist in code.**
- Check whether it is a system gap (component should exist but hasn't been built) or a misreference.
- For a system gap, file the addition before implementing.
- For a misreference, return to the designer to correct the annotation.

## Constraints (hard stops)

- Never use hardcoded color, spacing, or type values when a token exists.
- Never import new asset packages not already approved for the project.
- Never implement without reviewing the full design and annotations first.
- Never use placeholder assets in production output.
- Never skip accessibility validation.
- Never silently deviate from the design. Document the reason.
- Never mark complete without running the validation checklist.

## Output format

A completed implementation deliverable includes:

```
[Component / Screen Name] — v[X.X] — [Status]
Linked: [Ticket ID] | Updated: [Date] | Implementer: [Name]

Validation:
  - [ ] Layout matches design reference
  - [ ] Typography matches design reference
  - [ ] Colors use correct tokens
  - [ ] All interactive states implemented
  - [ ] Responsive behavior confirmed
  - [ ] Assets render correctly
  - [ ] Accessibility validated (WCAG 2.2 AA)
  - [ ] No hardcoded values

Deviations from design (if any):
  - [Description of deviation and reason]

New components added to design system (if any):
  - [Component name and location]

Tests added:
  - [List of unit / component / visual / E2E tests added with this implementation]
```

The tests-added section feeds directly into the `software-engineering-quality-engineer` review.

## How this skill relates to others

- Annotations this skill consumes → `design-handoff`.
- Design rules this implementation has to honor → `design-process`.
- Design-file fidelity (different layer) → `design-audit`.
- Code-side patterns and conventions → `software-engineering-nextjs-scaffold`, `software-engineering-shadcn-component`.
- Token mapping layer → `software-engineering-tailwind-tokens` (forthcoming).
- Accessibility validation → `software-engineering-a11y-audit` (forthcoming).
- Test pass that follows implementation → `software-engineering-quality-engineer`.
- System-level architecture context → `software-engineering-composable-architect`.
