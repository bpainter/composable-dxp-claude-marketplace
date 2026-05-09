---
description: Run the four checks (swap, squint, signature, token) plus named-bans audit on a design — produce a tabular Before/After/Why critique

# Project context
type: command
project: skills-library
plugin: design
tags: [type/command, plugin/design]
status: active
---

Run a craft critique on an existing design. Output is a markdown table — Before / After / Why — that names specific issues by their cataloged ban. Adapted from Emil Kowalski's review-as-table format.

Steps:

1. **Confirm the artifact** — single screen, single deck, single document, single asset, or component family.

2. **Run [[design-taste]]'s four checks:**
   - **Swap test:** if I swapped the typeface for the AI default (Inter), would anyone notice? If I swapped the layout for a centered-hero template, would it feel different? Note where swapping wouldn't matter — that's where you defaulted.
   - **Squint test:** blur your eyes. Is hierarchy still perceptible? Is anything jumping out harshly?
   - **Signature test:** can you point to five specific elements where the chosen direction's signature appears? Not "the overall feel" — actual components.
   - **Token test:** read the tokens out loud. Do they sound like they belong to this brief's world, or could they belong to any project?

3. **Audit against [[ai-tells-forbidden-patterns]]** — cite each violation by its named ban (the LILA ban, the Inter ban, the centered-hero ban, the 3-column-card-row ban, the Jane Doe effect, etc.).

4. **For motion-bearing designs, audit against [[motion-and-transitions]]** named bans (the scale(0) ban, the ease-in ban, the transition: all ban, the keyframe-on-rapid-trigger ban, the transform-origin-center-on-popovers ban).

5. **Run [[pre-delivery-checklist]] CRITICAL and HIGH gates.**

6. **Output as a Before / After / Why table.** No prose-style "Issue 1: ... Issue 2: ..." — tabulate.

Output format:

```
# Critique: {Artifact} — {Date}

## The four checks
- Swap test: {pass / fail with specifics}
- Squint test: {pass / fail with specifics}
- Signature test: {pass / fail — list 5 elements, or note where signature is invisible}
- Token test: {pass / fail with specifics}

## Findings

| Before | After | Why |
| --- | --- | --- |
| {what's there} | {what to change to} | {named ban or rule + brief rationale} |
| {next finding} | {fix} | {named ban} |
| ...

## CRITICAL items (block ship)
- {Item 1}
- {Item 2}

## Recommended next iteration
- Top 3 fixes by leverage:
  1. {fix}
  2. {fix}
  3. {fix}
- After this iteration, re-run critique before showing the user.
```

Tone: direct, specific, named. Cite every finding by its rule or ban. Avoid taste-based feedback ("I prefer the second one"). Tie all critique to the brief and the cataloged rules.
