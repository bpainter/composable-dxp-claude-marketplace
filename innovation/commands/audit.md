---
description: Run a full Innovation Operating System audit across all 7 layers — strategy, governance, discovery, validation, value capture, operating environment, outcomes — with prioritized fix plan

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Run the periodic Innovation OS health audit. Read-only — produces a prioritized remediation plan; doesn't fix issues directly. Use `/innovation:audit` quarterly or annually, when an innovation effort feels stuck, or before a major restructure.

Reference: [[2026-05_Innovation-Operating-System]] for the seven-layer model.

Steps:

1. **Confirm scope.**
   - Internal practice (Composable DXP at Slalom) or client situation?
   - Time window for evidence (past 6, 12, or 18 months).
   - Active bets / programs in scope.
   - Audit owner + sponsor.

2. **Run the seven-layer diagnostic.** For each layer, score healthy / warning / critical.

   **Layer 1 — Strategy** ([[innovation-strategist]]):
   - Is the mandate declared and ratified? (six types: defend the core, find next-S-curve, build new business, out-compete, reposition brand, theater)
   - Is the ambition mix (H1/H2/H3 capital) explicit and matching the mandate?
   - Is the operating model decision (line / lab / hybrid) made?
   - Is the cadence calendar real (weekly / monthly / quarterly / annual)?

   **Layer 2 — Governance** ([[innovation-portfolio-architect]]):
   - Are gates binary, observable, and written before testing?
   - Is kill rate in the healthy 30–50% range?
   - Are there zombie bets (>1.5x median time in stage)?
   - Is there pilot purgatory (Gate 3 failure)?
   - Is the dashboard refreshed weekly with learning + outcome metrics by horizon?

   **Layer 3 — Discovery** ([[innovation-discovery-coach]]):
   - Are JTBD interviews using switch-interview structure (not opinion surveys)?
   - Is observation budget ≥30% of discovery effort?
   - Are job stories validated with 10+ customers per archetype?
   - Are Gate 1 briefs producing 1-page synthesis (not 70-page report dumps)?

   **Layer 4 — Validation** ([[innovation-method-validator]]):
   - Are bets running stages in order (Insight → Problem → Solution → Business Model)?
   - Stage 2 (Problem) skipped or shallow?
   - Stage 4 (Business Model) treated as postscript?
   - Build-Measure-Learn cycle ≤14 days?

   **Layer 5 — Value Capture** ([[innovation-typologist]], [[innovation-monetization-strategist]], [[innovation-value-engineer]]):
   - Doblin Type investment — Configuration depth or Product-Performance-only?
   - WTP studies before product, or pricing-as-postscript?
   - Horizon-appropriate value math (NPV for H1, EV for H2, real options for H3)?
   - Kill economics documented per gate?

   **Layer 6 — Operating Environment** ([[innovation-lab-architect]], [[innovation-leadership-coach]]):
   - Lab charter complete (8 sections incl. sunset criteria)?
   - Year-1 KPIs are capability metrics, not revenue?
   - Sponsor backed multi-year, with named successor?
   - ≥2 named operating-BU absorbers?
   - Brain Trust mechanism running with candor norms?
   - Innovation capital deficits across the team?

   **Layer 7 — Outcomes**:
   - Are the right metrics reported per horizon?
   - Anti-metrics (revenue on H3 in year 1) absent?
   - Capability built, optionality secured, market presence quantified?

   **Cross-layer specialists:**
   - Is the situation actually disruption (run [[innovation-disruption-analyst]] test)?
   - Is the work framed as DT and would benefit from portfolio reframe ([[innovation-digital-transformation-advisor]])?

3. **Categorize findings by severity:**
   - **CRITICAL** — block continued investment. Mandate-confused portfolio, all-H1 dressed as transformation, KPI mismatch killing labs, revenue metrics on H3.
   - **HIGH** — fix this quarter. Toothless gates, pilot purgatory, Stage 2 skipping, pricing-as-postscript, capital-deficit leadership.
   - **MEDIUM** — fix this year. Type-investment imbalance, Brain Trust absent, missing absorber relationships, Storyteller deficit.
   - **LOW** — track. Minor template gaps, cadence drift, partial Ten Faces coverage.

4. **Produce prioritized fix plan** — top 12–15 items maximum, ordered by leverage. Don't surface 87 items; the audit's value is the prioritization.

5. **Layer routing.** For each finding, name the skill or command that resolves it:
   - Mandate / strategy gaps → [[innovation-strategist]] or `/innovation:strategy`
   - Governance gaps → [[innovation-portfolio-architect]] or `/innovation:portfolio`
   - Discovery thin → `/innovation:discover`
   - Validation skipping → `/innovation:validate`
   - Pricing-as-postscript → [[innovation-monetization-strategist]]
   - Value math wrong-horizon → `/innovation:value-case`
   - Lab structural issues → `/innovation:charter`
   - Disruption diagnosis required → `/innovation:disruption-test`
   - Capital / culture gaps → [[innovation-leadership-coach]]

6. **Save the audit.** Output to `30_Engagements/Active/{client}/Innovation_Audit_{date}.md` (client work) or `10_Practice/Initiatives/Innovation_Audit_{date}.md` (internal practice). Append a row to the practice's audit log MOC if one exists.

7. **Wait for user approval per fix-plan item.** Don't execute fixes during audit; route per-item afterward.

Output format:

```
# Innovation OS audit: {Org / Practice} — {Date}

## Scope
- Mandate type: {if known} | Confirmed by: {sponsor}
- Time window: {past N months}
- Bets in scope: {count, list}

## Summary
- Overall innovation health: {A/B/C/D/F}
- Critical: {N} | High: {N} | Medium: {N} | Low: {N}
- Layer-by-layer:
  - Layer 1 Strategy:              {healthy/warning/critical}
  - Layer 2 Governance:            {healthy/warning/critical}
  - Layer 3 Discovery:             {healthy/warning/critical}
  - Layer 4 Validation:            {healthy/warning/critical}
  - Layer 5 Value Capture:         {healthy/warning/critical}
  - Layer 6 Operating Environment: {healthy/warning/critical}
  - Layer 7 Outcomes:              {healthy/warning/critical}

## CRITICAL (block continued investment)
[Each: layer, what, why critical, fix path → which skill or command resolves it]

## HIGH (fix this quarter)
[...]

## MEDIUM (fix this year)
[...]

## LOW (track)
[...]

## Cross-layer findings
- Disruption test result: {pass/fail/N-A}
- DT framing accuracy: {modernization-mislabeled / transformation-mislabeled / accurate / N-A}

## Recommendations
- {Structural changes — governance, sponsorship, talent}
- {Cultural mechanisms missing}
- {Capital / sponsor work needed}

## Next audit
- Recommended date: {YYYY-MM-DD} (typically quarterly for active programs, annual for steady state)
- Recommended owner: {role}
```

Don't apply the audit's findings during the audit itself — produce the plan, then route per-item to the right skill or command.
