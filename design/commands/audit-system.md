---
description: Audit a screen, component, deck, document, or asset for design-system drift and named AI-tell violations — produce a prioritized fix list

# Project context
type: command
project: skills-library
plugin: design
tags: [type/command, plugin/design]
status: active
---

Audit an existing design artifact for system drift, anti-bland violations, and accessibility gaps.

Steps:

1. **Confirm scope.** Which artifact or set of artifacts? Which surfaces? What's the source-of-truth design system being audited against?

2. **Run [[design-audit]]** for design-system drift:
   - Missing shared components (using primitives where a composed component exists).
   - Local overrides (color, spacing, type values that don't reference tokens).
   - Unbound tokens (`#1A73E8` instead of `--color-primary`).
   - Repeated structures that should collapse to a single component.

3. **Run anti-bland audit** against [[ai-tells-forbidden-patterns]]:
   - Color tells (LILA ban, pure-black ban, neon-glow ban).
   - Typography tells (Inter ban, oversized H1 ban, modular-scale absence).
   - Layout tells (centered-hero, 3-column card row, anti-card-overuse).
   - Content tells (Jane Doe effect, fake-numbers, filler-words, generic avatars).
   - Surface-specific tells if applicable (deck five-pillar slide, document executive-summary-that-summarizes, social teal-quote-card).

4. **Run [[pre-delivery-checklist]]** for accessibility, performance, layout consistency.

5. **For motion-bearing artifacts**, check [[motion-and-transitions]] named bans (the scale(0) ban, ease-in ban, transition: all ban, keyframe-on-rapid-trigger ban).

6. **Categorize findings** by severity:
   - **CRITICAL** — accessibility violations, brand-defining errors. Block ship.
   - **HIGH** — style-system drift, named-ban violations, missing states. Fix before ship unless explicitly waived.
   - **MEDIUM** — typography drift, spacing rhythm issues. Fix before final.
   - **LOW** — minor inconsistencies, polish items.

7. **Output prioritized fix list** — top 20 items, not 87.

Output format:

```
# Design audit: {Artifact} — {Date}

## Scope
- Artifact(s): {list}
- Surfaces: {web / deck / document / etc.}
- Source-of-truth design system: {ref}

## Summary
- Overall: {A/B/C/D/F}
- Critical: {N} | High: {N} | Medium: {N} | Low: {N}

## CRITICAL (block ship)
[Each: what, where (link/screenshot), why critical, fix path]

## HIGH (fix before ship)
[...]

## MEDIUM (fix before final)
[...]

## LOW
[...]

## Recommendations
- {Structural changes to prevent recurrence}
```
