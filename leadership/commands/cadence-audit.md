---
description: Audit the calendar against the cadence-by-level standard — what to keep, redesign, async, kill. Quarterly use; sometimes triggered by a senior leader's "I can't get any work done."
type: command
project: skills-library
plugin: leadership
tags: [type/command, plugin/leadership]
status: active
---

Use the `leadership-meetings-and-cadences` skill to audit calendar / cadence and produce a prioritized prune list.

## When the user invokes this

Ask one question if not provided: **"Paste a representative week of your calendar (or describe your standing meetings), and I'll audit."**

## Steps

1. **Categorize each calendar block:**
   - 1:1s (with directs, with peers, with manager)
   - Standing team meetings (staff, retros, planning)
   - Project / engagement / pursuit syncs
   - External (clients, partners, recruiting)
   - Working blocks (focused alone time, deliberate)
   - Open / discretionary
2. **Tag each by Q1/Q2/Q3/Q4** (Covey):
   - Q1 (urgent + important) — real fires, deadlines
   - Q2 (important + not urgent) — strategy, prevention, relationships
   - Q3 (urgent + not important) — interruptions, performative urgency
   - Q4 (not urgent + not important) — busywork, drift
3. **Compare against the cadence-by-level standard** in `leadership-meetings-and-cadences/references/cadence-by-level.md`:
   - For Bermon (Senior Director): defended cadence is ~12–18 hours/week of structured leadership time, ~22+ hours of discretionary / making time
   - Are 1:1s with all directs intact?
   - Are skip-levels happening?
   - Is there a no-meeting day or block?
   - Is there protected thinking time?
4. **For each recurring meeting, run the four questions:**
   - What decision does this meeting drive? (If none, it's status — could be async)
   - Who must be in the room? (Cut anyone not directly contributing)
   - What would happen if it didn't exist for a month? (If "nothing," kill)
   - Has it drifted from its original purpose? (If yes, redesign or split)
5. **Produce the categorized list:**
   - Keep as-is
   - Trim attendees / shorten / sharpen agenda
   - Convert to async
   - Move biweekly → monthly, weekly → biweekly
   - Redesign
   - Kill
6. **Identify what's missing** — Q2 work that should be on calendar and isn't (strategy thinking time, peer relationships, learning, personal renewal)
7. **Propose a defended cadence for next quarter:**
   - Re-block the kept slots first
   - Add the missing Q2 blocks
   - Then let the rest of the week absorb other meetings
8. **Pre-mortem the prune.** What's the political cost? Whose toes get stepped on? What's the comms plan for any high-attendance kill?

## Don't

- Suggest cuts the user can't actually defend politically — be realistic about constraints
- Treat the audit as the work — produce the plan, then the user executes over a quarter
- Add new ceremonies on top of an over-loaded calendar

## Pairs with

- `/okr-set` — the Q2 priorities that the calendar needs to defend
- `/perspective` — if the calendar bloat reflects identity-fusion and the user can't bear to cut anything
- `/decision` — for any kill that requires a written decision (high-attendance recurring; political consequence)

## See also

- `leadership-meetings-and-cadences/references/cadence-by-level.md`
- `leadership-meetings-and-cadences/references/calendar-pruning.md`
- `leadership-goals-and-okrs/references/q2-prioritization-big-rocks.md`
