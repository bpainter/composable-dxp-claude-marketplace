---
name: design-audit
description: Read-only audit of a screen, component, or design file for design-system drift — missing shared components, local overrides, unbound tokens, repeated structures that should collapse to a single component, and global patterns built from custom frames. Outputs structured findings with confidence scores and priority rankings, plus a route to the right remediation workflow. Use this skill any time the user wants a screen reviewed for system fidelity, asks "does this follow our design system," is preparing a screen for handoff, or wants to find drift across a file, even when they don't say "audit." Trigger whenever a fidelity check is needed so drift gets surfaced as evidence-backed findings rather than vibes.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-audit]
tags: [type/skill, plugin/design, topic/design, topic/ui-ux]
status: active
---

# Design Audit

This skill reviews a designed screen, component, or file for evidence that the design is not properly integrated with the design system. It is **read-only** — it surfaces findings; it does not fix them. Remediation is routed to other skills based on scope.

The audit is most valuable: before handoff to engineering (catches drift the implementation team would otherwise inherit), during periodic system-fidelity reviews, and when a new component or pattern is being proposed.

Pair with `brand-design-process` for the rules the audit checks against, with `brand-design-screen` for the workflow that produces clean designs in the first place, with `brand-design-handoff` for the annotation pass that follows audit, and with `dev-quality-engineer` for the code-side analog (this skill audits the design file; that skill validates the implementation).

## When to use this skill

- Reviewing a screen for design-system integration before handoff.
- Spot-checking drift in a file that has been worked on by multiple designers.
- Validating that a recent screen aligns with the system after a token or component update.
- Discovering scope for a broader design-reconciliation pass.
- Sanity-checking a proposed system addition (is it really new, or does the system already have it?).

If the goal is to *fix* the drift, this is the wrong skill. Audit first, then route to remediation.

## Output format selection

- **Structured report** (default): use the human-readable Markdown report format.
- **Machine-readable**: use the structured findings format when output will feed a tool, ticketing system, or aggregator.
- **Ambiguous**: default to the structured report.

Both formats are specified in the Output Format section below.

## The audit workflow

Run these steps in order. Skipping steps produces shallow findings.

### Step 1: Identify the scope

- Confirm what is being audited: a single screen, a single component, a section, or a board / multi-screen page.
- For a single screen or component: inspect the root element, then drill into repeated or high-leverage children.
- For a board or multi-screen page: identify the most representative or highest-risk screens first. Do not attempt to audit everything. Prioritize breadth over depth on the initial pass; deeper passes follow.

### Step 2: Gather evidence

Review the design structure for:

- Component instances vs. custom-built frames.
- Token bindings vs. hardcoded values (color, spacing, type, radius, shadow).
- Variant usage vs. local overrides.
- Repeated structures that should collapse to a single shared component.
- Global patterns (navigation, headers, footers) built from scratch.

Base every finding on **structural evidence**, not visual impression. "Looks custom" is not a finding.

### Step 3: Review for systemization failures

- Look for places where the design *should* inherit from the system but is locally constructed.
- Prefer strong findings over weak ones. If evidence is ambiguous, omit the finding rather than guess.
- The point of the audit is to surface real drift, not to fill a quota.

### Step 4: Suggest replacements (when evidence is strong)

After identifying a custom-built primitive, search the design system for the closest matching component family.

Suggest a replacement only when:
- The element's role is clear.
- The candidate is from the relevant library or system context.
- The match is structurally plausible — not just visually similar.

If the match is ambiguous, omit the suggestion.

### Step 5: Present findings

- Use the report format appropriate to the request (see Output Format).
- Keep findings focused on the highest-signal issues. Aim for 0-6 findings per audit. A 30-finding audit is noise.

### Step 6: Route to remediation

- One finding with a narrow scope → targeted single-component fix workflow.
- Multiple findings requiring a coordinated pass → broader screen-reconciliation pass.
- Audit findings spanning multiple screens → file the audit as a discovery artifact and create a remediation epic.

Not every audit result should funnel through a single-fix path. Some audits are scope-discovery for a broader effort.

## What to flag

### Shared UI primitives recreated as custom frames

Common targets: buttons, icon buttons, cards, alerts, chips, avatars, stat tiles, tab bars, nav bars, list rows.

When a primitive is custom-built but the system has a near-equivalent, that is a high-leverage finding — the drift will compound across screens.

### Repeated sibling structures that should collapse

Three nearly identical stat tiles with different content, three nav rows built from scratch, four card layouts with the same shape — each is a missed component opportunity.

### Hardcoded visual values

Common targets: fills, strokes, text colors, border radius, spacing, typography, shadows.

Flag when evidence is concrete — a raw value sitting next to tokenized peers is strong evidence; an isolated raw value with no tokenized comparison is weaker and may be excluded.

### Global patterns built from custom frames

Navigation, headers, footers, and other high-leverage patterns. Drift here scales across many screens. Flag aggressively.

### Variant drift inside a nominal component

A button labeled "primary" but with non-standard size, stroke, or radius. The instance reference may exist but the override divergence is the drift.

## What not to flag

- Purely aesthetic preferences ("I would have used a different shade").
- Copywriting or product decisions.
- Layout choices that can reasonably remain screen-specific.
- One-off compositions where the underlying primitives are already componentized and tokenized.
- Claims that require undocumented assumptions about the system library.

If you find yourself reaching, omit.

## Evidence standard

Every finding must answer two questions:

1. **What concrete structural evidence** shows this is not systemized correctly?
2. **Why does that matter** for consistency, theming, maintenance, or propagation?

### Strong evidence

- An element is a custom frame when it should be a component instance.
- Several siblings duplicate the same structure.
- Raw color or geometry values where tokens should apply.
- A global pattern is custom-built.

### Weak evidence (do not use)

- "This looks custom."
- "I would normally make this a component."
- Any claim based only on visual appearance without structural support.

## Replacement-suggestion rule

When a finding is about a missing shared primitive, attach one likely replacement suggestion *if* the evidence supports it.

Suggest a replacement only when:
- The element's role is clear.
- The candidate is from the relevant library or system context.
- The match is structurally credible.

**Good suggestion language**:
- "This custom avatar cluster could likely be replaced with the Avatar component from the design system."
- "These repeated stat tiles appear to map to the Metric Card component."

Don't:
- Claim a suggested component is definitely correct unless evidence is explicit.
- Force a replacement candidate into every finding.
- Suggest a component from an unrelated library.

## Output formats

### Human-readable Markdown report

```
# Design System Audit: [Screen / Component]

## Verdict
[✅ Passes | ⚠️ Needs work | ❌ Significant issues] — Confidence: [%]

## Summary
[2-3 sentences on the overall state of system integration in the audited surface.]

## Findings

| # | Priority | Finding | Location |
|---|---|---|---|
| 1 | 🔴 Critical | [imperative title, ≤80 chars] | [screen / section] |
| 2 | 🟠 High | [imperative title] | [location] |
| ... | ... | ... | ... |

## Details

### Finding 1: [title]
**Priority**: 🔴 Critical
**Location**: [screen / section]
**Evidence**: [what concrete structural evidence shows this is not systemized correctly]
**Why it matters**: [maintenance, consistency, theming, propagation impact]
**Likely replacement**: [if evidence supports it; otherwise omit]

### Finding 2: ...

## Recommendations
1. [highest-priority remediation, with route to fix workflow]
2. [...]
```

### Structured findings (machine-readable)

```yaml
findings:
  - title: "[imperative, ≤80 characters]"
    body: "[explanation of why this is a problem]"
    confidence_score: 0.0-1.0
    priority: 0-3
    location: "[screen / component / section reference]"

overall_verdict: "Passes | Needs Work | Significant Issues"
overall_explanation: "[1-3 sentence summary]"
overall_confidence_score: 0.0-1.0
```

### Confidence guide

- **0.9-1.0** — Direct structural evidence (custom frame in place of an obvious component instance).
- **0.7-0.89** — Strong inference from repetition and nearby token usage.
- **0.5-0.69** — Plausible but incomplete. Prefer omitting over reporting.

Do not list findings below 0.5 confidence.

### Priority guide

- **0** — Nit or low-impact consistency issue.
- **1** — Moderate system drift.
- **2** — Important reusable primitive or tokenization issue.
- **3** — Severe issue likely to propagate widely (navigation, global patterns).

## Output rules

- Keep findings focused. 0-6 per audit; more often means the audit is too broad.
- Finding titles are imperative, ≤80 characters.
- Anchor every finding to a specific screen, component, or section so the designer can locate it.
- When a replacement is credible, include it in the finding body.
- Do not list findings below 0.5 confidence.
- Do not chase visual perfection. Surface drift, not preference.

## Trigger phrases (calibration aids)

These phrasings should reliably trigger the skill:

- "Review this screen for design-system integration."
- "Audit this design for missing component usage."
- "Check whether this design uses tokens correctly."
- "Does this follow our design system?"
- "Flag any drift from the component library."
- "Spot-check this before handoff."

## Routing remediation

After the audit:

- **One finding, narrow scope** → single-component remediation pass (often handled by `brand-design-screen` updating the screen, with the missing component proposed as a system addition).
- **Multiple findings, single screen** → full screen-reconciliation pass via `brand-design-screen`.
- **Findings span multiple screens / files** → discovery artifact for a remediation epic; do not try to fix in one sitting.
- **Findings include code-side drift** (the implementation differs from the design system, not just the design file) → `dev-quality-engineer` and `dev-design-implementation`.

## Constraints

- Do not modify the design file. This skill is read-only.
- Do not chase aesthetic preferences. Stay structural.
- Do not report findings below 0.5 confidence.
- Do not force a remediation route through every audit; sometimes the audit *is* the deliverable.
- Do not invent system components. If the system doesn't have it, that's also a finding (system gap).

## How this skill relates to others

- The rules this audit checks against → `brand-design-process`.
- Composing or fixing screens after audit findings → `brand-design-screen`.
- Annotating screens before handoff → `brand-design-handoff`.
- Code-side fidelity validation (the implementation matches the design and the system) → `dev-quality-engineer`, `dev-design-implementation`.
- Brand-identity-level fidelity (palette, type, voice) → [[brand-designer]].
