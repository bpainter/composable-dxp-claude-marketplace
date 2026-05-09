---
description: Run the four-stage validation cycle on a bet — Insight → Problem → Solution → Business Model — with assumption mapping, pretotyping, and 14-day build-measure-learn loops

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Run a structured validation cycle on an innovation bet. Use after `/innovation:discover` produces a Gate 1 brief, or when a bet has stalled and needs a validation reset.

Primary skill: [[innovation-method-validator]] (Layer 4 of the [[2026-05_Innovation-Operating-System|Innovation OS]]).

Source: [[book-innovators-method.md]] (Furr & Dyer's four-stage framework).

Steps:

1. **Confirm starting stage.** The most common failure is jumping from Stage 1 to Stage 3, skipping Stage 2 (Problem). Force the diagnostic:

   - **Stage 1 — Insight.** Is this a problem worth solving? Trend evidence, hot-problem signal, JTBD signal.
   - **Stage 2 — Problem.** Do customers describe the problem the way we do? Switch interviews, contextual inquiry, ≥10 validation interviews.
   - **Stage 3 — Solution.** Does our solution solve it better than alternatives, including doing nothing? Pretotyping, MVCs, concept tests.
   - **Stage 4 — Business Model.** Does value capture work? WTP studies, pilot, unit economics.

   If Stage 2 is shallow or skipped, regardless of how much Stage 3 work has happened, **return to Stage 2 first.** Build-up cost of skipping Stage 2 is higher than the time cost of redoing it.

2. **Run assumption mapping.** Plot on the 2×2 (importance × evidence). Test the top-right (high-importance, low-evidence) first.

   ```
                    Importance: Low ← → High
   Evidence: Low  │  Test second  │  Test FIRST
   Evidence: High │  Document     │  Confirm
   ```

   Identify 5–8 critical assumptions. Most teams identify 12–20 and never test the top-right ones.

3. **Design hypothesis test cards.** One per assumption. Template:

   ```
   Hypothesis: We believe [X]. We will know we are right when [observable signal].
   Method: [pretotype / MVC / concept test / WTP study]
   Sample / duration: [smallest valid; usually 5–30 customers, 5–14 days]
   Decision criteria: [written before test runs]
   Result: [confirmed / invalidated / inconclusive]
   Next: [what does this open up?]
   ```

   **Cycle time target: ≤14 days hypothesis-to-result.** Longer means over-engineered tests.

4. **Pretotype before MVP.** For Stage 3 (Solution) work, force the team to pretotype before building real product:
   - **Pinocchio** — non-functional prop
   - **Mechanical Turk** — humans simulate the algorithm
   - **Fake Door** — landing page captures intent for unbuilt feature
   - **Provincial** — launch in one geography first
   - **Pretend-to-Own** — sell competitor's offering, see if anyone wants it

   The discipline: produce a *real customer signal* (money, time, sign-ups), not a survey "I would buy this."

5. **Run Build → Measure → Learn cadence.**
   - Weekly — 1 hypothesis tested per validation team. <1/week = team is stuck
   - Bi-weekly — Build-Measure-Learn review; what was learned, what's the next riskiest assumption
   - Monthly — pivot/persevere review

6. **At Stage 4, coordinate with monetization-strategist.** Pricing is not a postscript:
   - Cross to [[innovation-monetization-strategist]] for WTP study (see [[book-monetizing-innovation.md]])
   - Don't approve Stage 4 advance without WTP signal
   - Avoid the four monetization failure modes (feature shocks, minivations, hidden gems, undeads)

7. **Decide advance / pivot / kill at each gate.**

   | Question | Advance | Kill | Pivot |
   |---|---|---|---|
   | Did we validate the assumption? | Yes, strong signal | No, no path | Partial — different than expected |
   | Are next-stage assumptions testable? | Yes, defined | No clear next step | Reframe, then re-test |
   | Has the market changed? | Stable / improving | Worsened materially | Shifted bet's shape |
   | Does the team still believe? | Yes, with evidence | Lost conviction with reason | Believe in something adjacent |

   **Lost conviction without evidence is fatigue, not data.** Don't kill on fatigue alone — surface to leadership-coach.

8. **Hand off.** Output of this command feeds:
   - `/innovation:value-case` for Gate 3 funding decision
   - `/innovation:portfolio` for portfolio steering committee review
   - [[innovation-monetization-strategist]] for Stage 4 WTP work (if not already running)
   - [[innovation-typologist]] for type-combination check (is this Product-Performance-only?)

9. **Save.** `30_Engagements/Active/{client}/Validation_{bet}_{date}/` or `10_Practice/Initiatives/Validation_{bet}_{date}/` — assumption map, hypothesis test cards, pretotype results, gate-readiness recommendation.

Output deliverables:

```
1. Assumption Map (2×2) — for the bet
2. Validation Plan — staged experiments through Gate 2 / Gate 3
3. Hypothesis Test Cards — one per experiment, with results
4. Pretotype Designs + Results — what was faked, what signal captured
5. Validation Report (gate-ready) — what was tested, what was learned, advance/kill/pivot recommendation
```

**Common failures this command prevents:**
- Stage 2 skipping (most common)
- Over-engineered MVPs before validation (4+ months building, no customer signal)
- Pricing-as-postscript at Stage 4
- 14-day cycle stretched to 8 weeks
- "I would buy this" survey responses treated as validation
- Lost-conviction killing bets that should persevere
