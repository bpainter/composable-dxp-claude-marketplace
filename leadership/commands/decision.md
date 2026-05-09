---
description: Walk a material decision through the full structure — options, trade-offs, pre-mortem, dissent surfacing, decision log entry. Type-1 (one-way door) gets heavier analysis; Type-2 (two-way door) gets faster experimentation.
type: command
project: skills-library
plugin: leadership
tags: [type/command, plugin/leadership]
status: active
---

Use the `leadership-meetings-and-cadences` skill to structure a material decision and produce a durable record.

## When the user invokes this

Ask one question if not provided: **"What's the decision, and is it Type-1 (hard to reverse) or Type-2 (easy to reverse)?"**

## Steps

1. **Name the decision in one sentence.** If it can't be named in one sentence, the decision isn't sharp enough. Iterate until it can.
2. **Name the decider.** One person, even if input comes from many. If the user isn't the decider, surface that and ask whether they're escalating, recommending, or scoping.
3. **Tag the type:**
   - **Type 1** (one-way door, hard to reverse): heavier analysis, more options, more pre-mortem, broader consultation
   - **Type 2** (two-way door, easy to reverse): faster experimentation, fewer options, lighter analysis, smaller consultation
4. **Lay out 2–4 options.** For each:
   - One-line description
   - Pros (3–5 specific)
   - Cons (3–5 specific)
   - Cost / effort
   - Reversibility
   - Recommendation (yes / no / lean)
5. **Pre-mortem.** "If this decision turns out wrong, the most likely reason is…" Force the honest answer. What leading indicators would tell us we made the wrong call?
6. **Surface dissent.** Who would disagree, and why? What's the strongest case against the recommendation? If no dissent surfaces, name that explicitly — but probe whether you're missing perspectives.
7. **Make or stage the call.** If the user is the decider and ready: produce the call. If not: produce the recommendation and the question for the decider.
8. **Log the decision.** Create a Decision Log entry from `70_Templates/T_Decision_Log.md` in the relevant location:
   - Practice-level: `10_Practice/Strategy/Decisions/`
   - Engagement-level: `30_Engagements/{Engagement}/Decisions/`
   - Pursuit-level: `20_Proposals/Active/{Pursuit}/Decisions/`
   - Personal: in your daily note's "decisions" section, optionally promoted later
9. **Specify revisit conditions.** Under what circumstances would we reopen this? By what date should we sanity-check?
10. **List stakeholders to inform** and the follow-ups with owners.

## Don't

- Skip the pre-mortem — it's the most-skipped section and the most-valuable in retrospect
- Treat every decision like Type-1 — most aren't, and the over-analysis is a stall
- Let the decision evaporate without a log entry — the log is how the org learns

## When to escalate

If the decision affects multiple practices, requires firm-level air cover, or has client-commercial dimensions outside Bermon's authority, route to escalation in step 7 with explicit recommendation rather than calling.

## Pairs with

- `/perspective` — if the user is reactive and the decision is being driven by anxiety rather than analysis
- `/okr-set` — if the decision is about quarterly priorities and trade-offs
- `/cadence-audit` — if the decision is about adding or killing a recurring meeting

## See also

- `leadership-meetings-and-cadences/references/decision-meeting-format.md`
- `70_Templates/T_Decision_Log.md`
