---
name: software-engineering-a11y-audit
description: Audit a screen, component, or flow for WCAG 2.2 AA conformance and remediate the findings — keyboard navigation, focus management, screen-reader semantics, contrast, motion, touch targets, error handling, and forms. Covers automated tooling (axe, Lighthouse, Storybook a11y addon) plus the manual passes that automation can't replace. Use this skill any time the user is auditing for accessibility, fixing a specific a11y finding, preparing for an external audit, or asking "is this accessible," even when "WCAG" or "a11y" isn't said. Trigger whenever accessibility is the question so the audit goes beyond the automated tools' false sense of completeness.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-a11y-audit]
tags: [type/skill, plugin/software-engineering topic/software, topic/engineering]
status: active
---

# Accessibility Audit

Accessibility is part of quality, not a compliance overlay. WCAG 2.2 AA is the floor for any project this skill governs. This skill produces both the audit (what's wrong, with severity) and the remediation guidance (how to fix it, in this stack).

A few principles up front:

- **Automated tools catch about 30-40% of issues.** axe, Lighthouse, and Pa11y find the obvious — missing labels, contrast failures, missing alt text. They cannot evaluate semantic correctness, screen-reader narration quality, keyboard-only flow, or whether a focus ring is meaningful in context. The manual passes do the rest.
- **Disability is a design context, not a user persona.** Users encounter accessibility barriers temporarily (broken arm), situationally (bright sun, noisy environment), and permanently. Building for the permanent case usually fixes the temporary and situational ones too.
- **Founders are sophisticated. They notice.** A founder-facing site that fails on keyboard or screen reader signals that the firm doesn't take craft seriously.

Pair with `software-engineering-shadcn-component` (Radix-based primitives give us most of the keyboard and ARIA defaults), `software-engineering-nextjs-scaffold` (where some semantic decisions live), `software-engineering-tailwind-tokens` (contrast pairs and motion tokens), `design-handoff` (which sets a11y requirements at design-time), `software-engineering-quality-engineer` (CI integration).

## When to use this skill

- Auditing a specific screen or component.
- Pre-launch accessibility readiness review.
- Investigating a single a11y bug or screen-reader complaint.
- Preparing for an external accessibility audit.
- Reviewing a PR for accessibility regressions.
- Onboarding someone to the project's a11y expectations.
- Standing up CI a11y gates.

## What we test against

- **WCAG 2.2 AA** — the standard floor.
- **WAI-ARIA Authoring Practices** — patterns for components (combobox, dialog, etc.) where ARIA is required.
- **Section 508** — for any US government-adjacent context (usually out of scope for our work, but worth knowing the overlap with WCAG).
- **EAA (European Accessibility Act)** — if the site serves EU users at scale, applicable from June 2025.

## The four-layer audit

Run these layers in order. Each catches things the next can't see.

### Layer 1: Automated scan

Tools: axe (via `@axe-core/playwright` in CI, or the browser extension), Lighthouse, the Storybook a11y addon, ESLint plugin `eslint-plugin-jsx-a11y`.

What it catches reliably:

- Missing form labels.
- Missing alt text on images.
- Contrast failures.
- Improper heading nesting.
- Missing language attribute.
- Tabindex misuse (`tabindex > 0`).
- Buttons without accessible names.
- Empty links.

How to run:

```bash
# In CI: axe via Playwright
import { test } from "@playwright/test";
import AxeBuilder from "@axe-core/playwright";

test("a11y on home", async ({ page }) => {
  await page.goto("/");
  const results = await new AxeBuilder({ page }).analyze();
  expect(results.violations).toEqual([]);
});

# Local: axe browser extension or Lighthouse in DevTools.
```

For Storybook, install `@storybook/addon-a11y` and per-story violations show up in the addon panel. Catches issues at the component-isolated level.

Don't trust this layer to be complete; it's the smoke detector, not the inspection.

### Layer 2: Keyboard-only pass

Disconnect the mouse. Walk every flow with Tab, Shift+Tab, Enter, Space, Escape, and Arrow keys.

What it catches:

- Focus indicators that disappear or are too low-contrast.
- Tab order that doesn't match visual order.
- Tab traps (focus locked inside a region).
- Skipped interactive elements.
- Modals that don't trap focus, or that don't return focus on close.
- Custom widgets (sliders, comboboxes) that don't behave per WAI-ARIA Authoring Practices.
- Elements that are interactive but not keyboard-reachable.

What to check on each interactive element:

- Is the focus ring visible?
- Is the focus ring meaningful (not just inherited from `:focus`, which is often suppressed)?
- Does Enter or Space activate it correctly?
- Does Escape dismiss overlays / cancel actions?
- Do arrow keys behave as expected for menus, sliders, listboxes?

For overlays (Dialog, Sheet, Popover, DropdownMenu): focus must move *into* the overlay on open and *back* to the trigger on close. Radix-based primitives in shadcn handle this; custom widgets often do not.

### Layer 3: Screen-reader pass

Tools: VoiceOver (macOS / iOS), NVDA (Windows, free), JAWS (Windows, paid), TalkBack (Android).

For consulting-grade web projects:

- Primary screen reader to test: VoiceOver on macOS Safari (most common combo for the founder audience on Apple hardware).
- Secondary: NVDA on Windows Firefox.
- Mobile: VoiceOver on iOS Safari.

What to check:

- **Page identity**: Does the page announce its title clearly? Is the language attribute set?
- **Landmark structure**: Banner, navigation, main, contentinfo. The screen reader should be able to jump between them.
- **Heading structure**: H1 → H2 → H3 hierarchy without skipping. The screen reader's headings list should be navigable.
- **Form fields**: Each field's purpose is announced. Required fields are flagged. Errors are announced when they appear, ideally with `aria-live="polite"` or `aria-describedby` linking to the error.
- **Buttons and links**: Each has a meaningful name (no "click here," no icon-only buttons without aria-label).
- **Images**: Decorative ones are hidden (`aria-hidden="true"` or empty alt), informative ones have alt text that conveys the information.
- **Dynamic content**: Loading states, success messages, validation errors are announced. Use `aria-live` regions.
- **Custom widgets**: Comboboxes, sliders, tabs, accordions narrate per ARIA pattern.

### Layer 4: Manual semantic and content review

What it catches:

- Headings used for visual size rather than structure.
- Buttons that should be links and vice versa (`<button>` for "go somewhere," `<a>` for "do something" — both wrong).
- ARIA used where semantic HTML would do (and is preferred).
- Color used as the only signal (e.g., red for error without an icon or label).
- Motion that triggers on every page load (animations should respect `prefers-reduced-motion`).
- Touch targets smaller than 44 × 44 px.
- Time-based content (auto-rotating carousels, timeouts) without user control.
- Forms with no autocomplete attributes where they would help.
- Plain-language failures — content that's accessible technically but unreadable.

## WCAG 2.2 AA the practical checklist

Grouped by the area of the project they affect. Not every criterion applies to every screen; mark N/A and move on.

### Perceivable

- [ ] **1.1.1 Non-text content** — Alt text on images. `alt=""` for decorative.
- [ ] **1.2.x Time-based media** — Captions and transcripts for video/audio (likely N/A on a static site).
- [ ] **1.3.1 Info and relationships** — Semantic HTML. Headings nest correctly. Lists are `<ul>`/`<ol>`. Tables use `<th>` with scope.
- [ ] **1.3.5 Identify input purpose** — `autocomplete` attributes on form fields where applicable (`autocomplete="email"`, etc.).
- [ ] **1.4.3 Contrast (minimum)** — Text ≥ 4.5:1, large text (18.66px+ regular or 14px+ bold) ≥ 3:1.
- [ ] **1.4.4 Resize text** — Up to 200% without loss of content or function.
- [ ] **1.4.10 Reflow** — No horizontal scrolling at 320px viewport (except specific element types like maps).
- [ ] **1.4.11 Non-text contrast** — UI components and graphical objects ≥ 3:1.
- [ ] **1.4.12 Text spacing** — Tolerates user-overrides for line-height, letter-spacing, word-spacing, paragraph spacing.
- [ ] **1.4.13 Content on hover or focus** — Tooltips and popovers can be dismissed (Escape), don't disappear too soon, persist while hovered.

### Operable

- [ ] **2.1.1 Keyboard** — Everything operable via keyboard. No mouse-only interactions.
- [ ] **2.1.2 No keyboard trap** — Focus can leave any region.
- [ ] **2.1.4 Character key shortcuts** — Single-character shortcuts can be disabled, remapped, or only fire when focused.
- [ ] **2.2.1 Timing adjustable** — User can extend or disable session timeouts.
- [ ] **2.2.2 Pause, stop, hide** — Auto-moving content can be paused.
- [ ] **2.3.1 Three flashes or below threshold** — No content flashes more than 3 times/sec.
- [ ] **2.4.1 Bypass blocks** — Skip-to-main-content link.
- [ ] **2.4.2 Page titled** — Each page has a unique, descriptive `<title>`.
- [ ] **2.4.3 Focus order** — Tab order matches visual order.
- [ ] **2.4.4 Link purpose (in context)** — Link text describes the destination.
- [ ] **2.4.5 Multiple ways** — More than one way to reach a page (search, nav, sitemap).
- [ ] **2.4.6 Headings and labels** — Descriptive, not generic.
- [ ] **2.4.7 Focus visible** — Visible focus indicator on all interactive elements.
- [ ] **2.4.11 Focus not obscured (minimum)** — Focused element not entirely hidden by a sticky header / modal. *(New in 2.2.)*
- [ ] **2.5.1 Pointer gestures** — Multipoint or path-based gestures have a single-pointer alternative.
- [ ] **2.5.2 Pointer cancellation** — Down-event triggers don't fire actions; cancellable on up-event.
- [ ] **2.5.3 Label in name** — Accessible name contains the visible label.
- [ ] **2.5.4 Motion actuation** — No required device motion (shake to undo) without alternative.
- [ ] **2.5.7 Dragging movements** — Single-pointer alternative to drag operations. *(New in 2.2.)*
- [ ] **2.5.8 Target size (minimum)** — 24 × 24 px CSS pixels minimum (44 × 44 strongly preferred). *(New in 2.2.)*

### Understandable

- [ ] **3.1.1 Language of page** — `<html lang="en">` set.
- [ ] **3.2.1 On focus** — No unexpected context change on focus.
- [ ] **3.2.2 On input** — No unexpected context change on input.
- [ ] **3.2.6 Consistent help** — Help mechanisms (contact, FAQ link) appear in the same place across pages. *(New in 2.2.)*
- [ ] **3.3.1 Error identification** — Errors are identified in text.
- [ ] **3.3.2 Labels or instructions** — Form fields have labels and any required instructions.
- [ ] **3.3.3 Error suggestion** — When possible, suggest a fix for errors.
- [ ] **3.3.4 Error prevention (legal, financial, data)** — Reversible / checked / confirmable for important submissions (questionnaire, formation).
- [ ] **3.3.7 Redundant entry** — Information already entered isn't asked for again unless necessary. *(New in 2.2.)*
- [ ] **3.3.8 Accessible authentication (minimum)** — No cognitive function tests for authentication unless there's an alternative. *(New in 2.2.)*

### Robust

- [ ] **4.1.2 Name, role, value** — All UI components have programmatically determinable name, role, and value.
- [ ] **4.1.3 Status messages** — Status messages communicated to assistive tech without focus shift.

## Stack-specific guidance

### Next.js / App Router

- Set `<html lang="en">` in the root layout.
- Use `<title>` via metadata API on every page (template + per-page; see `software-engineering-nextjs-scaffold`).
- For `error.tsx` pages, ensure they communicate error state to assistive tech (use semantic markup; an alert role on the heading where appropriate).
- For `loading.tsx`, use `aria-live="polite"` and a meaningful loading message, not just a spinner.
- Use `next/image` with proper `alt`. Decorative images: `alt=""`.

### shadcn/ui (Radix-based)

The shadcn primitives inherit Radix's a11y defaults — keyboard support, ARIA, focus management. Do not break them:

- Don't strip `data-state` attributes; they drive styling and signal state to assistive tech.
- Don't replace built-in dismiss / close behavior with custom close buttons that bypass focus return.
- Use `<DialogClose>`, `<SheetClose>`, etc., not bare buttons.
- For `<DropdownMenu>`, items should use the menuitem role (Radix handles this); custom menu items must follow.

### Tailwind / focus rings

The `--ring` token (see `software-engineering-tailwind-tokens`) drives focus rings. Don't suppress with `focus:outline-none` without applying a `focus-visible:ring-...` replacement. Many a11y bugs are "we suppressed the default focus outline and forgot to replace it."

### Forms

- Use shadcn's `Form` primitive (react-hook-form + zod resolver). It wires labels and error messages correctly via `FormField`.
- Server Actions (see `software-engineering-nextjs-scaffold`) need server-side validation; surface errors via the action's return shape and announce them with `aria-live`.
- Required fields: use `aria-required="true"` (browser-native `required` attribute is fine, but pair with the label so screen readers don't only learn it from validation).
- Inline validation: announce changes (`aria-live="polite"`).

## Output formats

### Audit report

```markdown
# A11y Audit: [Page / Component / Flow]

**Audited**: [date]
**Auditor**: [name]
**Standard**: WCAG 2.2 AA
**Tools used**: axe, manual keyboard, VoiceOver / NVDA

## Verdict
[✅ Passes | ⚠️ Needs work | ❌ Significant issues]

## Summary
[2-3 sentences on the overall state.]

## Findings

| # | Severity | WCAG | Finding | Location | Fix |
|---|---|---|---|---|---|
| 1 | 🔴 Critical | 2.1.1 | "Continue" button not keyboard-reachable | Step 3 of questionnaire | Replace `<div onClick>` with `<Button>` |
| 2 | 🟠 High | 1.4.3 | Muted-foreground text fails 4.5:1 on white | Body copy | Update `--muted-foreground` to a darker hex |
| ... | ... | ... | ... | ... | ... |

## Details

### Finding 1: [...]
**Severity**: 🔴 Critical
**WCAG**: 2.1.1 (Keyboard)
**Location**: src/app/(tools)/formation-questionnaire/_components/step-3.tsx:42
**Issue**: The "Continue" affordance is rendered as a styled `<div>` with `onClick`. Tab does not reach it; Enter does not activate it.
**User impact**: Keyboard-only users cannot advance past step 3.
**Fix**: Use the `<Button>` primitive. Move the click handler to its `onClick` prop. Ensure the button receives correct semantic role.
**Verification**: Tab-key walkthrough confirms reachability; VoiceOver announces "Continue, button."

### ...

## Recommendations
1. [Highest-priority fix.]
2. [Next.]
3. [...]
```

### Severity guide

- **🔴 Critical** — Blocks a user with a disability from completing a key task. WCAG A failures and severe AA failures.
- **🟠 High** — Significantly degrades the experience but doesn't block.
- **🟡 Medium** — Falls short of best practice; AA conformance edge cases.
- **⚪ Low** — Polish, AAA-level improvements, future-friendly hardening.

### Per-component a11y addendum

For component specs (see `software-engineering-component-spec`), each component's a11y section captures: role, keyboard, ARIA, focus management, contrast, motion, touch target. The component-level spec is the canonical contract; the audit checks that the contract holds.

## Common findings on this stack

- **Suppressed focus outline.** `focus:outline-none` without a `focus-visible:ring-...` replacement. Standard fix: ensure ring tokens are applied.
- **Custom click-handlers on `<div>`.** Replace with `<Button>` or `<a>`.
- **Icon-only buttons without aria-label.** Add `aria-label` or visible text. shadcn's `Button` doesn't enforce this; we have to.
- **Missing form labels.** Use shadcn's `<FormLabel>` paired with `<FormField>`; never put labels in `placeholder` only.
- **Muted text below contrast.** Tighten the `--muted-foreground` value or reserve `text-muted-foreground` for genuinely tertiary content.
- **Modal that doesn't trap focus.** Use `<Dialog>`, not a hand-rolled overlay.
- **Animations on every page load.** Wrap in `motion-safe:animate-...` or rely on the global `prefers-reduced-motion` rule from `software-engineering-tailwind-tokens`.
- **Skipped heading levels.** A `<h2>` followed by `<h4>` confuses screen-reader heading nav. Use `<h3>` or restructure.
- **Auto-rotating carousels.** Provide pause and prev/next buttons, or just don't use them.

## CI integration

Per `software-engineering-quality-engineer`:

- axe runs in Playwright E2E suite; violations fail CI.
- `eslint-plugin-jsx-a11y` runs as part of the lint step.
- Storybook a11y addon catches per-component issues at story-write time.
- Lighthouse CI runs on a representative page set; a11y score below threshold fails the build.

These catch the automation-discoverable subset. Manual passes (Layers 2-4 above) happen pre-launch and on substantive flow changes.

## Constraints

- Don't substitute automated scans for manual passes. The 30-40% statistic is real.
- Don't ship "we'll fix a11y after launch." It compounds; retrofitting is more expensive than building in.
- Don't use ARIA where semantic HTML works. `aria-button` on a `<div>` is worse than a `<button>`.
- Don't suppress focus indicators without replacement. Most expensive single defect.
- Don't rely on color as the only signal. Pair color with icon or text.

## How this skill relates to others

- Tokens and contrast pairings → `software-engineering-tailwind-tokens`.
- shadcn/Radix-based primitives that give us a11y defaults → `software-engineering-shadcn-component`.
- Application-level concerns (titles, language, error pages) → `software-engineering-nextjs-scaffold`.
- Per-component a11y contract → `software-engineering-component-spec`.
- Design-time a11y requirements → `design-handoff`.
- CI a11y gates and the broader test pyramid → `software-engineering-quality-engineer`.

## Source material

- WCAG 2.2 (W3C): https://www.w3.org/TR/WCAG22/
- Understanding WCAG 2.2: https://www.w3.org/WAI/WCAG22/Understanding/
- WAI-ARIA Authoring Practices: https://www.w3.org/WAI/ARIA/apg/
- axe rules reference: https://dequeuniversity.com/rules/axe/
- WebAIM articles: https://webaim.org/articles/
- Inclusive Components (Heydon Pickering): https://inclusive-components.design/
- A11y Project checklist: https://www.a11yproject.com/checklist/
