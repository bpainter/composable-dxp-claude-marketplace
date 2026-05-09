---
name: design-handoff
description: Generate complete, dev-ready handoff annotations for any screen or component — covering component identification, all interactive states, design-token references, interaction behavior (trigger, action, transition, destination), copy and content rules, responsive breakpoints, accessibility (WCAG 2.2 AA), and edge cases. Use this skill any time the user is preparing a design for engineering handoff, redlining a component, writing spec notes, or filling gaps engineering keeps asking about, even when they say "annotate this," "write specs," "redline," or "this is going to dev." Trigger whenever a design is moving from review to engineering so engineers don't have to guess at states, tokens, transitions, or edge behavior.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-handoff]
tags: [type/skill, plugin/design, topic/design, topic/ui-ux]
status: active
---

# Design Handoff Annotations

This skill produces the annotation layer that engineering needs to build a design without back-and-forth. The deliverable is a structured annotation set — not flowery design rationale — covering states, tokens, interactions, responsive behavior, accessibility, and edge cases.

This skill operates inside Phase 4 (Review) and Phase 5 (Handoff) of `brand-design-process`. The screen itself was composed using `brand-design-screen`. Pair with `dev-design-implementation` for the engineering side of the handoff, and with `dev-quality-engineer` so the same edge cases the annotation covers also drive test design.

## When to use this skill

- Annotating a screen or component for engineering handoff.
- Writing redlines after design review.
- Filling spec gaps that engineering keeps asking about.
- Reviewing existing annotations for completeness.
- Onboarding a new engineering pair to a design file.

## The nine annotation rules

These layer over the always-on rules from `brand-design-process`. They fire on every annotation output.

### Rule 1: Annotation intent

Every annotation must explain design intent, not just restate visual properties. The visual already says "this is blue" — the annotation says "primary action; only one per view."

**Violation**: "Background: #1A73E8."
**Correct**: "Background: `color/brand/primary`. Primary action; only one primary CTA per view."

### Rule 2: State coverage

For any component with interactive behavior, annotate every applicable state.

| State | Required when |
|---|---|
| Default | Always |
| Hover | Always (pointer devices) |
| Focus | Always |
| Active / pressed | Always |
| Disabled | Always |
| Loading | Async behavior |
| Error | Validated input |
| Empty | Lists or inputs |
| Success | Forms or transactions |

State changes must include both visual spec and trigger. If a state is intentionally not implemented, annotate the omission and why.

### Rule 3: Component identification

Name the exact library component including variant, size, and state. Engineers should never guess which variant.

**Violation**: "Use the button component here."
**Correct**: "Button / Primary / Large / Default — from project library v2."

If the component isn't in the library, file the system addition before handoff. Do not reference unpublished components.

### Rule 4: Interaction behavior

Document trigger, action, transition, and destination for every interactive element.

**Violation**: "Tapping opens the modal."
**Correct**: "Tap → open ConfirmationModal. Transition: slide-up 300ms ease-out. Dismiss: tap backdrop or press Escape."

Cover keyboard equivalents alongside touch and pointer triggers.

### Rule 5: Token documentation

Every visual property references a design token. No raw values in the annotation output.

| Token category | Examples |
|---|---|
| Color | `color/brand/primary`, `color/feedback/error` |
| Spacing | `space/100` through `space/800` |
| Radius | `radius/sm`, `radius/md`, `radius/full` |
| Typography | `type/heading/xl`, `type/body/md` |
| Shadow | `shadow/sm`, `shadow/md` |
| Motion | `motion/duration/fast`, `motion/easing/standard` |

If a value cannot be tokenized, that's a system gap, not an annotation exception. Flag it.

### Rule 6: Copy and content rules

Document character limits, truncation behavior, placeholder text, error copy, and empty-state copy for every text element.

**Violation**: "Title text goes here."
**Correct**: "Title: max 60 chars. Truncate with ellipsis after 2 lines. Placeholder: 'Your account name.'"

Run UX copy through the project's voice rules and [[consulting-humanize]] before locking it.

### Rule 7: Responsive and breakpoint behavior

Document layout behavior at each breakpoint.

**Violation**: "Looks good on mobile."
**Correct**: "≥1280px: 3-column grid. 768-1279px: 2-column. <768px: single column, cards stack vertically."

Cover at minimum the project's three primary breakpoints, plus any cases where elements appear, disappear, or reorder.

### Rule 8: Accessibility requirements

Annotate WCAG 2.2 AA requirements per component. Engineering should not have to derive accessibility behavior from the visual alone.

| Requirement | Detail |
|---|---|
| Contrast ratio | Text ≥ 4.5:1; large text ≥ 3:1; UI components ≥ 3:1 |
| Focus order | Logical DOM order; visible focus indicator |
| ARIA roles | role, aria-label, aria-describedby, aria-expanded, aria-controls |
| Touch target | Minimum 44 × 44 px |
| Motion | Respect `prefers-reduced-motion` |
| Screen reader | Meaningful alt text; decorative elements hidden |

For any customer-facing tool, accessibility is part of brand quality, not a compliance afterthought. Build it into the spec, not bolted on at QA.

### Rule 9: Edge cases and constraints

Document every known edge case before handoff.

| Edge case | Examples |
|---|---|
| Empty state | No items in list; no search results |
| Long content | 200+ char names; multi-line labels |
| Error state | Network failure; validation failure |
| Permissions | Read-only mode; locked record |
| Loading | Skeleton screens; spinner placement |
| Boundary data | 0 items, 1 item, max items |

Edge cases marked "TBD" in the handoff become bugs in the build. Resolve or file a ticket; do not ship the ambiguity.

## How rules layer

```
1. Annotation intent wraps every annotation: always explain the why
2. Component identification before state coverage
3. Token documentation applies to every visual property
4. State coverage and interaction behavior apply to interactive components
5. Copy and content rules apply to all text elements
6. Responsive and breakpoint behavior applies to layout-level annotations
7. Accessibility requirements and edge cases apply to all outputs
```

## The five-phase annotation workflow

| Phase | Activity | Gate |
|---|---|---|
| 1. Inventory | List all components and interactions on the screen | Component list approved |
| 2. Token audit | Verify all visual properties have token references | All raw values resolved |
| 3. State mapping | Document all states per component | State table complete |
| 4. Interaction spec | Document triggers, transitions, destinations | Interaction table reviewed |
| 5. Publish | Add metadata, link ticket, share with engineering | Dev lead sign-off |

Skip any phase and you ship gaps.

## Annotation checklist

Run this before declaring annotation complete.

### Components

- [ ] Library name, component name, variant, size, state documented for every instance.
- [ ] Custom or non-library components flagged with a system request filed.
- [ ] Component hierarchy documented (parent, child, nested instances).
- [ ] Variant switching rules documented (e.g., when to use destructive vs. default).

### States

- [ ] Default annotated.
- [ ] Hover annotated.
- [ ] Focus annotated with visible-indicator spec.
- [ ] Active / pressed annotated.
- [ ] Disabled annotated with reason.
- [ ] Loading annotated where async exists.
- [ ] Error annotated for validated inputs.
- [ ] Empty annotated for lists and inputs.
- [ ] Success annotated for forms and transactions.

### Interactions

- [ ] Trigger documented (tap, click, swipe, key).
- [ ] Action documented (navigate, open, close, submit).
- [ ] Transition documented (animation type, duration, easing).
- [ ] Destination documented (screen, modal, drawer, state change).
- [ ] Dismiss documented for overlays.
- [ ] Keyboard path documented (Tab, Escape, Enter, Space, Arrow).

### Tokens

- [ ] All fill colors token-referenced.
- [ ] All stroke colors token-referenced.
- [ ] All spacing values token-referenced.
- [ ] All radius values token-referenced.
- [ ] All typography references a text style.
- [ ] All shadows reference an effect style.
- [ ] All motion values reference a motion token.

### Copy and content

- [ ] Character limits for all text fields.
- [ ] Truncation behavior documented.
- [ ] Placeholder text documented.
- [ ] Error message copy documented.
- [ ] Empty-state copy documented.

### Responsive

- [ ] Behavior at all primary breakpoints (mobile, tablet, desktop).
- [ ] Column changes documented.
- [ ] Stacking order documented.
- [ ] Hidden / shown elements per breakpoint documented.

### Accessibility

- [ ] Contrast ratios verified for all text and UI.
- [ ] Focus order documented.
- [ ] ARIA roles and labels documented.
- [ ] Touch target sizes verified (min 44 × 44 px).
- [ ] Motion preferences documented.
- [ ] Alt text documented for all images.

### Edge cases

- [ ] Empty state documented.
- [ ] Long-content behavior documented.
- [ ] Error state documented.
- [ ] Permission-based states documented.
- [ ] Boundary data cases documented (0, 1, max).

## Constraints

- Do not annotate without first completing the token audit. No raw values reach the output.
- Do not publish without the dev-lead sign-off gate.
- Do not skip state coverage for interactive components.
- Do not reference unpublished system components.
- Do not omit accessibility requirements.
- Do not ship "TBD" edge cases — resolve or file a ticket.
- Do not exceed ~300 lines per deliverable. If it's longer, split by section.

## Output format

```
Artifact: Handoff Annotation — [Screen Name]
Author: [name]
Status: [Draft | In Review | Approved]
Version: [x.y]
Linked ticket: [ID]
Reviewed by: [name or TBD]

--- Components ---
[Component name] | Library: [name] | Variant: [name] | Size: [x] | State: [default]

--- States ---
[Component] | State | Visual spec | Token | Notes

--- Interactions ---
[Element] | Trigger | Action | Transition | Destination | Dismiss

--- Tokens ---
[Property] | Token name | Resolved value

--- Copy and content ---
[Element] | Char limit | Truncation | Placeholder | Error copy | Empty copy

--- Responsive ---
[Breakpoint] | Layout change | Columns | Stacking | Hidden elements

--- Accessibility ---
[Component] | Contrast | Focus order | ARIA | Touch target | Motion | Alt text

--- Edge cases ---
[Case type] | Trigger | Expected behavior | Fallback
```

## How this skill relates to others

- Always-on rules and workflow phases this skill operates inside → `brand-design-process`.
- Composing the screen this skill annotates → `brand-design-screen`.
- Reviewing the screen for system drift before handoff → `brand-design-audit`.
- Engineering side of the handoff (translating annotations into code) → `dev-design-implementation`.
- Edge cases drive test design → `dev-quality-engineer`.
- UX copy cleanup → [[consulting-humanize]].
