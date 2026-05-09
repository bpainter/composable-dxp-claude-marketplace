---
description: Design or audit innovation portfolio governance — gates, allocation logic, kill discipline, learning vs. outcome metrics, dashboard, calendar

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Design new portfolio governance from scratch, or audit and remediate existing governance. Use when standing up a new innovation portfolio, when an existing portfolio's gates are toothless, or when the dashboard is producing activity reports instead of decision support.

Primary skill: [[innovation-portfolio-architect]] (Layer 2 of the [[2026-05_Innovation-Operating-System|Innovation OS]]).

Steps:

1. **Establish context.** Clarify:
   - Is this **design** (new governance from scratch) or **audit + remediate** (fix existing)?
   - Mandate type from `/innovation:strategy` (or pull from existing strategy doc)
   - Ambition mix and capital available
   - Number of active bets in flight

2. **Map the current portfolio** (audit path) or design the target shape (new path):

   ```
   | Initiative | Horizon | Ambition | Doblin Types | Stage | Months in stage | Health | Decision |
   ```

   Diagnostic columns: months in stage (>1.5x median = stalled) and decision needed (advance / pivot / kill / merge).

3. **Pick governance pattern by stage and horizon.**

   | Stage | Default pattern | Why |
   |---|---|---|
   | Idea / discovery | Light intake — strategic fit + customer-problem clarity | Don't kill ideas with paperwork |
   | Problem validation | Discovery-driven — assumption tests | Data isn't there for stage-gate yet |
   | Solution validation | Discovery-driven w/ stage-gate review | Concept tests, MVP signal |
   | Pilot / limited launch | Stage-gate — adoption, unit economics, NPS | Real metrics now exist |
   | Scale / launch | Standard product governance | Hand off to operating BU |

4. **Define gate criteria.** Binary, observable, written before testing.

   - **Gate 0** Idea intake — strategic fit, customer problem clarity, rough feasibility
   - **Gate 1** Discovery → Alpha — ≥10 customer interviews, job story, riskiest assumption identified
   - **Gate 2** Alpha → Beta — MVC tested with ≥30 users, engagement signal, initial WTP signal
   - **Gate 3** Beta → Launch — pilot adoption ≥ target, unit economics viable, scale operating model defined
   - **Gate 4** Launch → Scale — full launch metrics

   Reference: [[portfolio_governance_template.md]] for the canonical gate criteria.

5. **Choose allocation logic.**
   - **Fixed budget** — pre-allocated pools per horizon. Predictable.
   - **Performance-based advancement** — staged investment per gate. Risk-aligned.
   - **Hybrid** — fixed pool per horizon, performance advancement within. Most common.

   Recommend hybrid unless org maturity argues otherwise.

6. **Build kill discipline.**
   - Stage SLAs — bets exceeding 1.5x median time in stage default to kill review
   - Pre-mortems at funding — name kill conditions in writing
   - Kill ceremonies — celebrate the team and the learning
   - Healthy kill rate target — 30–50%
   - Quarterly zombie audit — bets "almost ready" 9+ months → kill

7. **Define metrics.** Two layers:
   - Learning metrics for early stage — experiments / week (target 5–10), cycle time (target <14 days), assumptions tested, % invalidated
   - Outcome metrics for late stage by horizon — H1 (NRR, margin), H2 (pilot adoption, unit economics), H3 (option value secured)

8. **Build the cadence.**
   - Weekly — team standups
   - Monthly — portfolio status (45 min)
   - Quarterly — portfolio steering committee (2–3 hrs)
   - Annually — strategy refresh

9. **Build the dashboard.** Single page, refreshed weekly.

   ```
   PORTFOLIO HEALTH SCORECARD — Q{N} {Year}
   Bets H1: {N} ${X}M | Bets H2: {N} ${X}M | Bets H3: {N} ${X}M
   Velocity: experiments/wk {N} · cycle time {N}d · advanced {N} · killed {N}
   ```

10. **For audit path:** produce the kill / advance / pivot / merge recommendations per bet. Include kill economics (residual value at each gate) — coordinate with `/innovation:value-case`.

11. **Hand off.** Save deliverables to `10_Practice/Initiatives/Portfolio_Governance_{date}.md` (internal) or `30_Engagements/Active/{client}/...` (client). Pair with `/innovation:value-case` for funding decisions and `/innovation:charter` if a lab is in scope.

Output format depends on path:

**Design path** produces:
- Governance design doc (gates, criteria, allocation, kill discipline)
- Cadence calendar
- Portfolio dashboard template
- First-cycle decision pack

**Audit path** produces:
- Portfolio map (current state)
- Recommended kill list
- Recommended advance list
- Recommended pivot list
- Recommended merge list
- Governance redesign recommendations (if gates are toothless)

Don't try to do both in one pass. If the engagement is "we have governance but it's not working," do audit first, then redesign.
