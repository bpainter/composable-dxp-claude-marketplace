---
description: Design a session end-to-end — walk the 5 Ps, pick the arc, sequence Scan/Focus/Act blocks with named games or modules, populate the session-design template.

# Project context
type: command
project: skills-library
plugin: facilitation
tags: [type/command, plugin/facilitation]
status: active
---

Design a facilitated session from scope to draft agenda. Routes to either `facilitation-meeting-architect` (for ≤90 min meetings) or `facilitation-workshop-designer` (multi-hour or multi-day workshops with a Sponsor Design Team), then pulls in specialists for game selection, decision rules, and group-dynamics anticipation.

Reference templates: [`template-session-design.md`](../templates/template-session-design.md), [`template-workshop-arc.md`](../templates/template-workshop-arc.md), [`template-collaboration-pattern-selector.md`](../templates/template-collaboration-pattern-selector.md).

Steps:

1. **Confirm scope.** If the user hasn't already run `/scope`, do that first. Don't design without an outcome.

2. **Classify the session.**
   - Length: 15–90 min → meeting; half-day to multi-day → workshop
   - Stakes: low / medium / high (drives investment in design)
   - Format: in-person, virtual, hybrid (pulls in `facilitation-virtual-hybrid-specialist` if not in-person)
   - Type: kickoff / discovery / decision / vision / retro / co-creation / planning

3. **Pick the arc.** From [`template-workshop-arc.md`](../templates/template-workshop-arc.md):
   - 90 min, half-day, 1-day, 2-day, or 3-day
   - Apply the recursion principle (Pattern 222) — Scan/Focus/Act inside each block as well as across the whole

4. **Sequence the blocks.** For each phase (Open / Scan / Focus / Act / Close), pick the right specific moves:
   - For named games → consult [`gamestorming-catalog.md`](../skills/facilitation-gamestorming-coach/references/gamestorming-catalog.md) (123 games with object-of-play, players, duration, how-to-play)
   - For wavespace-tradition modules → consult [`tools-catalog.md`](../skills/facilitation-workshop-designer/references/tools-catalog.md) (136 tools)
   - For strategy canvases → consult [`canvas-selection-guide.md`](../skills/facilitation-strategic-visualizer/references/canvas-selection-guide.md)
   - For DT-specific cycles → consult [`dt-cycle-playbook.md`](../skills/facilitation-design-thinking-coach/references/dt-cycle-playbook.md)

5. **Pre-design decision moments.** For every place where the room must produce a decision, name the rule in advance using [`decision-methods-catalog.md`](../skills/facilitation-decision-architect/references/decision-methods-catalog.md). Don't leave decision-making to chance.

6. **Pattern check.** Walk [`template-collaboration-pattern-selector.md`](../templates/template-collaboration-pattern-selector.md) and confirm the design honors the relevant Collaboration Code patterns (especially 201, 211, 222, 225, 263).

7. **Anticipate friction.** From the scoping brief, identify likely group-dynamics issues (dominance, withdrawal, conflict, authority in room) and pre-design structural mitigations using [`difficult-participants-playbook.md`](../skills/facilitation-group-dynamics-coach/references/difficult-participants-playbook.md).

8. **Populate the session design.** Use [`template-session-design.md`](../templates/template-session-design.md) as the canonical artifact. Fill in:
   - Purpose / People / Preparation / Process / Payoff
   - Block-by-block agenda with timing, named tools/games, owners
   - Risk and contingency
   - Patterns honored

9. **Generate the pre-work brief** if the session is half-day or longer. Use [`template-pre-work-brief.md`](../templates/template-pre-work-brief.md) — sent by sponsor, not facilitator, ~1 week ahead.

10. **Hand off.** If multi-day, recommend setting up the Sponsor Design Team per [`sponsor-design-team-playbook.md`](../skills/facilitation-workshop-designer/references/sponsor-design-team-playbook.md). For virtual/hybrid, recommend the `/triage-meeting` checklist for AV setup. For game-by-game how-to-run, route to `/pick-game`.

Don't try to design and run in one pass. The session-design artifact is the deliverable; running it is a separate session.
