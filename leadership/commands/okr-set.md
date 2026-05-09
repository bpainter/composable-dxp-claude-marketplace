---
description: Set, sharpen, or review quarterly OKRs — Wodtke format with stretch calibration. Routes through setting (start of quarter), mid-quarter check, or end-of-quarter retro.
type: command
project: skills-library
plugin: leadership
tags: [type/command, plugin/leadership, plugin/okrs]
status: active
---

Use the `leadership-goals-and-okrs` skill to set, sharpen, or review OKRs.

## When the user invokes this

Ask one question if not provided: **"Setting new OKRs, mid-quarter check, or end-of-quarter retro?"**

## Phase routing

### Setting — start of quarter

1. Pull or create `10_Practice/Strategy/OKRs/{Period}/_OKRs.md` from `70_Templates/T_OKR_Worksheet.md`
2. Check the **hedgehog** alignment — does this Objective sit at the intersection of (a) what we can be world-class at, (b) what we're passionate about, (c) what drives the economic engine? Reject Objectives that don't.
3. Draft **one Objective** — qualitative, motivating, time-bounded, memorable. Should make the team lean in.
4. Draft **3 Key Results** — each quantitative, measurable, outcome-focused. Calibrate to ~50% confidence at start (not 90% sandbagged; not 10% fantasy).
5. Surface the **stop-doing list**. For each new OKR, what does the team explicitly deprioritize to make room? If "nothing," the OKR isn't real.
6. Identify **risks and dependencies** — the things outside the team's control that could derail
7. Schedule the **operating cadence** — Monday commit / Friday celebrate, mid-quarter review (week 6 or 7), end-quarter retro (week 13)
8. Save the worksheet

### Mid-quarter check — week 6 or 7

1. Pull the worksheet; read the latest KR confidence numbers
2. For each KR with confidence < 30%, sort:
   - **Push** (we can still hit it; what specifically gets us there)
   - **Descope** (redefine the KR with explicit reasoning — better than silent drift)
   - **Escalate** (we need help we can't provide ourselves; what specifically)
   - **Accept** (we'll miss this one; what do we learn)
3. Update the worksheet with the calls and reasoning
4. If a descope is material, log it as a Decision via `/decision`

### End-of-quarter retro — week 13

1. Score each KR honestly: actual / target = %
2. Average across KRs (target zone: 60–70%; 90%+ means OKRs were too easy; <30% means fantasy or wrong)
3. For each KR, ask: what conditions enabled the win or prevented it? Be specific.
4. Capture forward-looking changes for next quarter — in OKRs, in process, in resources
5. Update the worksheet's retro section
6. Carry learnings into the next quarter's `/okr-set`

## Don't

- Set more than 1 Objective with 3 KRs (sometimes 2 Os max). Multiple Objectives means no priorities.
- Sandbag KRs to ensure 100% completion — the discipline is stretch
- Skip the stop-doing list — adding without subtracting is the OKR-killer
- Cascade individual OKRs from team OKRs literally — individuals hold *parts* of team KRs, not separate personal mini-OKRs
- Tie OKR completion directly to bonus comp (creates sandbagging incentives)

## Pairs with

- `/cadence-audit` — to install the Monday-commit / Friday-celebrate ritual
- `/decision` — for material descope or escalation calls
- `/feedback` — when motivation is flagging and the OKR rhythm isn't fixing it (route to motivation diagnosis with AMP)

## See also

- `leadership-goals-and-okrs/references/okr-design-and-rhythm.md`
- `leadership-goals-and-okrs/references/team-priority-rituals.md`
- `leadership-goals-and-okrs/references/saying-no-and-killing-priorities.md`
- `70_Templates/T_OKR_Worksheet.md`
