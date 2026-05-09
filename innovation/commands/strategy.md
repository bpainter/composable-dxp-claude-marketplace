---
description: Design a new innovation strategy on a page — mandate, ambition mix across Three Horizons, operating model, cadence, top-line metrics

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Design the top-of-funnel innovation strategy. Use when standing up a new innovation effort, refreshing one that's drifted, or shaping a client engagement at the strategy layer.

Primary skill: [[innovation-strategist]] (Layer 1 of the [[2026-05_Innovation-Operating-System|Innovation OS]]).

Steps:

1. **Diagnose the mandate.** Refuse to design strategy without a named mandate. Six types:
   - Defend the core
   - Find next-S-curve
   - Build new business
   - Out-compete a faster rival
   - Reposition the brand
   - Innovation theater (most common, rarely declared — diagnose and refuse to legitimize)

   If the mandate isn't declared in the brief, surface this as the first thing to ratify. Don't proceed without it.

2. **Set the ambition mix.** Default 70/20/10 across H1/H2/H3 capital, adjusted by mandate:
   - Defend the core: 80/15/5
   - Find next-S-curve: 60/30/10
   - Build new business: 40/30/30
   - Out-compete: 70/25/5
   - Reposition brand: 65/25/10

   These are *capital* allocations, not headcount. Headcount usually skews more H1-heavy because H1 work is bigger; that's fine — the failure is when capital *also* skews H1-heavy beyond mandate.

3. **Choose the operating model.** Two decisions in order:
   - **Where does innovation work happen?** Line, lab, or hybrid (default to hybrid)
   - **Which governance per horizon?** Stage-gate for H1, discovery-driven for H2/H3, portfolio steering across all

4. **Define the cadence calendar.** Real innovation strategy has a calendar:
   - Weekly — discovery / validation team standups, experiment results
   - Monthly — portfolio status review (45 min)
   - Quarterly — portfolio steering committee (2–3 hrs)
   - Annually — strategy refresh, mandate review, ambition mix re-set

5. **Name the top-line metrics.** Two layers, both required:
   - Activity / learning metrics for early stage (experiments per quarter, validation cycle time, % bets advancing through gates, % killed at gates 30–50%)
   - Outcome metrics per horizon (H1: NRR, margin; H2: pilot adoption, unit economics; H3: option value, capability built)

6. **Hand off implementation.** This command produces the strategy on a page. After ratification:
   - Governance design → `/innovation:portfolio`
   - Lab / center design (if hybrid or lab) → `/innovation:charter`
   - Value math by horizon → `/innovation:value-case` template applied per bet

7. **Save the output.** `30_Engagements/Active/{client}/Innovation_Strategy_{date}.md` for client work; `10_Practice/Strategy/Innovation_Strategy_{date}.md` for internal practice.

Output format — Innovation Strategy on a Page:

```
# Innovation Strategy — {Org / Practice}
## On a Page · {Date} · Sponsor: {name}

## Mandate (one sentence)
"{Lab / function name} exists to {protect / enable / build} {what}, for {whom}, because {why now}."
Mandate type: ☐ Defend the core ☐ Find next-S-curve ☐ Build new business
              ☐ Out-compete    ☐ Reposition brand  ☐ Theater (refuse)

## Ambition mix (capital allocation)
H1 (defend / extend core, 0–12 mo):  {N}%   $${X}M
H2 (build emerging, 1–3 yr):         {N}%   $${X}M
H3 (create options, 3+ yr):          {N}%   $${X}M

## Operating model
- Where: ☐ Line  ☐ Lab  ☐ Hybrid
- Governance: H1 stage-gate · H2 stage-gate + discovery-driven · H3 discovery-driven w/ real-options
- Portfolio steering cadence: {quarterly}

## Cadence
- Weekly: {what, who}
- Monthly: {what, who}
- Quarterly: {portfolio steering, who, duration}
- Annual: {strategy refresh, who}

## Metrics (top 5)
1. {Activity / learning metric}
2. {H1 outcome metric}
3. {H2 outcome metric}
4. {H3 outcome metric}
5. {Capability metric}

## What's NOT innovation work (the discipline test)
- {What's out of scope so the mandate stays sharp}

## 18-month roadmap (bullet)
- Months 0–3: {mandate ratification, charter, governance design}
- Months 3–9: {first wave of bets through gates, lab stand-up if applicable}
- Months 9–18: {validated bets advance, first H2 launches, H3 options matured}

## Next step / decision asks
- {Specific decisions needed, who owns, by when}
```

Pair with: [[template-innovation-charter.md]] (if a lab is recommended), [[template-three-horizons-canvas.md]], [[template-ambition-matrix.md]].
