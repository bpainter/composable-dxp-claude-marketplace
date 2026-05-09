---
name: design-process
description: Always-on design discipline for any experience-design task — nine behavioral rules (anti-verbosity, system-first, scope discipline, research-first, token fidelity, frame metadata) plus the five-phase workflow (Define → Explore → Design → Review → Handoff) with explicit human review gates. Use this skill any time the user is doing design work — wireframing, screen design, flow design, annotation, design review, or design handoff — even when they don't say "process" or "workflow." Trigger whenever a design artifact is being created, reviewed, or handed off so quality, consistency, and reviewability hold up across the team.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-process]
tags: [type/skill, plugin/design, topic/design, topic/ui-ux]
status: active
---

# Design Process

This skill is the always-on discipline layer for any design work. Nine rules fire silently on every design task, and the five-phase workflow shapes how the work moves from brief to handoff. You do not invoke these rules; they shape the output.

The rules and the workflow together replace ad-hoc design review with a predictable rhythm. They also make it harder for the design to drift from the system, the brief, or the team's prior research.

**Always pair with [[design-taste]].** This skill governs the *workflow* (phases, rules, gates). `design-taste` governs the *output* — the three dials (variance, motion, density), the intent-first protocol, the four pre-show checks, and the named AI tells to reject. Load both at the start of any design task. The workflow protects you from process drift; taste protects you from the AI default center.

When the project's design system is still under construction, the rules below work without it: when a component or token doesn't yet exist as a system primitive, flag it as a proposed addition rather than building it ad hoc.

Companion skills in this plugin: `design-screen` for design-tool composition, `design-handoff` for annotation, `design-audit` for fidelity review. For design-to-code translation after handoff, see `software-engineering-design-implementation`.

## When to use this skill

- Starting any design task with more than a single trivial frame.
- Reviewing a design artifact (your own or someone else's).
- Preparing a design for handoff to engineering.
- Onboarding a new designer or design-adjacent contributor.
- Auditing process drift: are we still doing the explore phase, or jumping straight to hi-fi?

If the task is a single quick edit to an already-shipped frame, the workflow phases are overkill — but the nine rules still apply.

## The nine rules

### 1. Anti-verbosity

Annotations and design rationale explain *why*, not *what*. Screens speak for themselves; annotations should add information the visual cannot.

- Prevents over-annotated wireframes that restate what is already visible.
- Prevents rationale docs that summarize the deck instead of capturing the decision.

**Violation**: "This button is a primary CTA that the user can click to submit the form."
**Correct**: "Primary CTA — disabled until all required fields validate."

### 2. System-first

Use existing design-system components before creating a new pattern. No bespoke components when a system component exists or could be reasonably extended.

- Prevents inconsistent spacing, elevation, color across screens.
- Prevents one-off patterns that engineering then has to interpret and maintain.

**Violation**: A custom input field with unique border radius.
**Correct**: Use the system Text Field component. If a needed variant doesn't exist, propose it as a system addition; don't implement ad hoc.

### 3. Writing style

UX copy — labels, error messages, microcopy, annotations — is clear, direct, and human. Inherit the rules from `consulting-humanize` (no em-dashes, no comparative "not X but Y," no summary closers, no AI verb tics) plus the named bans in [[ai-tells-forbidden-patterns]] (filler words, generic placeholder names, fake numbers). UX-specific rules:

- Buttons are verbs in the user's frame: "Continue," "Review order," not "Click here," not "Submit information."
- Error messages name the problem and the next step: "Email is missing" beats "Please fill in all required fields."
- Empty states tell the user what they're looking at and what to do: "No tasks yet — add your first task."
- Avoid hedge stacks: "may potentially allow users to..." → "lets users..."

**Violation**: CTA label "Proceed to the Next Step."
**Correct**: "Continue" or "Review order."

### 4. Research-first

Before designing a new flow, pattern, or interaction, check what already exists: prior journey maps, interview notes, workshop outputs, precedent screens, design-system patterns. Do not design in a vacuum.

Protocol:
1. Check prior research artifacts (interviews, journey maps, JTBD, personas).
2. Check existing screens for established patterns in the same product area.
3. Check workshop / session notes for relevant decisions.
4. If no prior context exists, flag the assumption explicitly in the design file.

Most "we should design X" debates are answered by research someone already did.

### 5. Scope discipline

Do not add unrequested screens, states, or features beyond the brief. If you identify scope gaps that would break the flow, flag them as open questions; do not silently expand scope.

**Violation**: Asked for a confirmation screen, delivered a six-screen post-submission flow.
**Correct**: Design the confirmation screen. Note in the file: "Identified two adjacent states (timeout, partial success) not in scope — flagged for backlog."

### 6. Pattern reuse

Search before creating. Three places to look in order: design-system component library, existing screens in the current product area, screens from adjacent product areas. Only when nothing fits, propose a new pattern and flag it for system review.

- Prevents redundant components that duplicate existing ones.
- Prevents inconsistent interaction patterns across the product.
- Saves engineering time at handoff.

### 7. Design-system fidelity

Every component placed and every annotation written must use design-system tokens — color, spacing, typography, radius, elevation, motion. No hardcoded values.

Pre-handoff checklist:

- [ ] All colors reference semantic tokens (e.g., `color/brand/primary`, not `#1A73E8`).
- [ ] All spacing uses the project grid (8pt or whatever the system specifies).
- [ ] All type uses defined text styles (e.g., `type/heading/lg`, not raw font sizes).
- [ ] All elevation follows defined shadow levels.
- [ ] All radius uses defined values.
- [ ] All components are from the approved library or flagged as proposed additions.

If a token doesn't exist for what you need, flag it for the design-system team. Don't invent a one-off.

### 8. Workflow phases

Every design task of meaningful scope follows the five-phase workflow below. No phase is silently skipped. No deliverable advances without the phase gate met.

### 9. Artifact metadata

Every frame, file, or exported deliverable carries the same metadata so it can be traced, versioned, and handed off without ambiguity.

Required metadata per deliverable:

| Field | Value |
|---|---|
| Screen / flow name | Plain-language name matching the story or epic |
| Status | Exploration / In Review / Approved / Handed Off |
| Version | v1.0, v1.1, etc. |
| Linked story / epic | Ticket ID (Jira, Linear, GitHub Issue) |
| Last updated | Date |
| Designer | Name |
| Notes | Open questions, known gaps, deferred scope |

**Violation**: A Figma frame labeled "New Screen 3" with no status, no linked story.
**Correct**: "Formation Questionnaire — Step 2 of 4 | v1.1 | In Review | GNX-204"

## The five-phase workflow

Each phase has an output and a gate. Gates are explicit human approval points; the work does not advance silently.

### Phase 0: Calibrate (always-on, before Define)

Before any phase of the workflow runs, calibrate the work to the brief using [[design-taste]]:

1. **Set the dials.** Read DESIGN_VARIANCE, MOTION_INTENSITY, VISUAL_DENSITY from the brief or from the Slalom defaults (6/4/5). Override per user instruction.
2. **Run the intent-first protocol.** Answer who is this human, what must they accomplish, what should this feel like — in writing, with specifics.
3. **Produce the four required outputs.** Domain (5+ concepts), Color world (5+ colors that exist in this brief's actual world), Signature (one element unique to this audience), Defaults to reject (3 obvious choices you won't make).
4. **Pick a stylistic register** from [[stylistic-vocabulary]].

This is not a phase with a deliverable. It's the preamble that prevents the workflow from carrying defaults into Phase 1.

### Phase 1: Define

- Restate the brief in your own words.
- List constraints (platform, device, design system, timeline).
- List open questions (flag for stakeholders, not for silent assumption).
- Confirm scope boundaries.

**Output**: brief, scope, constraints, open questions list.
**Gate**: human approves scope before exploration begins.

### Phase 2: Explore

- Produce 2-3 lo-fi directional concepts.
- Each concept must represent a meaningfully different approach. Three variants of the same idea is one concept, not three.
- Annotate the trade-off of each direction in 1-2 bullets.
- Do not polish — the point is divergence.

**Output**: 2-3 lo-fi directional concepts with trade-off notes.
**Gate**: human selects a direction before hi-fi begins.

For single-screen tasks, two concepts is enough. For multi-screen flows, three. For trivial single-edit tasks, you may skip explore — but say so explicitly.

### Phase 3: Design

- Build hi-fi screens for the selected direction.
- Cover all required states: default, empty, error, loading, success.
- Use design-system components exclusively (Rules 2, 6).
- Apply correct tokens throughout (Rule 7).
- Write all UX copy per Rule 3.

**Output**: hi-fi screens with states.
**Gate**: human reviews hi-fi before annotation begins.

### Phase 4: Review

- Add annotations explaining decisions, not describing visuals (Rule 1).
- Flag open edge cases or deferred states.
- Run the design-system fidelity checklist (Rule 7).
- Run the four pre-show checks from [[design-taste]] (swap, squint, signature, token).
- Run the [[pre-delivery-checklist]] (CRITICAL and HIGH items must clear).
- Update status to "In Review" (Rule 9).

**Output**: annotated screens with redlines and spec notes.
**Gate**: human sign-off before handoff.

For the actual annotation work, switch to `design-handoff`. This phase is the trigger to invoke that skill.

### Phase 5: Handoff

- Export dev-ready file.
- Add component inventory (every component used).
- Document edge cases, even deferred ones.
- Update metadata: status = "Handed Off"; version bumped.
- Link to story / epic.

**Output**: dev-ready file with inventory and edge cases.
**Gate**: dev lead acceptance.

For the design-to-code translation that happens after handoff, see `software-engineering-design-implementation`.

## How rules layer during a task

A single task — "design the architecture overview for the [Client] composable DXP proposal" — activates all nine rules plus the design-taste preamble:

```
phase-0-calibrate      → Dials set, intent-first answered, register chosen
  + research-first         → Check prior research and workshop notes before starting
  + scope-discipline       → Confirm scope: which views? which states?
  + workflow-phases        → Phase 1 (Define) before any pixels
    + pattern-reuse        → Search existing components first
    + system-first         → Use system components; no bespoke
    + system-fidelity      → Tokens only; defined type and spacing
    + writing-style        → Labels, CTAs, errors reviewed
    + anti-verbosity       → Annotations explain why, not what
    + artifact-metadata    → Status and version on every frame
```

The rules are non-negotiable defaults. The workflow phases are a sequence. Together they keep the work auditable.

## Hard stops

- Never skip the Explore phase for any task with more than one screen, except trivial edits.
- Never use hardcoded color, spacing, or type values.
- Never deliver a frame without status and version metadata.
- Never add screens or flows not in the brief without flagging first.
- Never copy-paste UX copy without reviewing it against Rule 3.
- Never create a new component without checking the design system first.

## Output format for a compliant deliverable

```
[Screen / Flow Name] — v[X.X] — [Status]
Linked: [Ticket ID] | Updated: [Date] | Designer: [Name]

Screens included:
  - Default state
  - Empty state
  - Error state(s)
  - Loading state
  - Success / confirmation state

Annotations:
  - Decision notes (why, not what)
  - Open questions flagged
  - Deferred scope noted

Component inventory:
  - List of every design-system component used
  - Any proposed new components flagged for system review

UX copy review:
  - All labels, CTAs, errors checked against Rule 3

Design-system fidelity:
  - Token checklist passed
  - Grid confirmed
  - Type styles confirmed
```

## How this skill relates to others

- **Always-on output discipline (taste, dials, AI tells, four checks)** → [[design-taste]] — load alongside this skill.
- **Stylistic register selection** → [[stylistic-vocabulary]].
- **Pre-show quality gate** → [[pre-delivery-checklist]].
- **Specific bans to cite in critique** → [[ai-tells-forbidden-patterns]].
- **Static fundamentals (hierarchy, color, space, depth)** → [[visual-design-foundations]].
- **Dynamic fundamentals (states, motion, dismissal)** → [[interaction-patterns]].
- **Brand identity, naming, color/type systems** → [[brand-strategist]] and the brand-designer split (`brand-naming`, `brand-identity-system`, `brand-voice-tone`, `brand-guidelines-composer`).
- **Surface-specific composition** → `design-screen` (product UI), `design-presentation` (16:9 decks), `design-document` (8.5×11 long-form), `design-social-asset` (LinkedIn / Instagram).
- **Producing handoff annotations** → `design-handoff`.
- **Reviewing a design for system drift** → `design-audit`.
- **Translating designs into production code** → `software-engineering-design-implementation` and `software-engineering-frontend-developer`.
- **UX copy cleanup beyond the writing-style rule** → [[consulting-humanize]].
- **Stories that drive design tasks** → `project-management-user-story`.
