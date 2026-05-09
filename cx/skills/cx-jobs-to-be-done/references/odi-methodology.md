---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-jobs-to-be-done]]"
tags: [type/reference, plugin/cx, scope/skill, topic/jtbd, source/ulwick]
status: active
---
# Outcome-Driven Innovation (ODI) — Ulwick

Drawn from Anthony W. Ulwick, *Jobs to be Done: Theory to Practice* (2016), and Strategyn's published methodology.

ODI is the most rigorous JTBD methodology. It's heavier than Kalbach's plays, quantitative-first, and built for organizations willing to invest in market research at meaningful scale. Use Ulwick when the decision warrants the investment — major platform bets, market-entry choices, multi-year roadmaps.

## The needs framework — six categories

Ulwick's contribution is taxonomy. He separates "need" into six discrete things, each requiring different research methods:

| Need type | What it is | Captured as |
|---|---|---|
| **Core functional job-to-be-done** | The fundamental task the customer is trying to accomplish (job-level, not product-level). | Job statement |
| **Desired outcomes on the core job** | The metrics by which the customer judges how well the job is being done. 50–150 per job. | Outcome statements |
| **Related jobs** | Other jobs the customer would like to get done at the same time. | Job list |
| **Emotional / social jobs** | How the customer wants to feel and how they want to be perceived. | Statement format |
| **Consumption-chain jobs** | The supporting jobs around acquisition, install, use, fix, dispose. | Job list |
| **Financial desired outcomes** | Outcomes tied to cost, total cost of ownership, ROI. | Outcome statements |

Most teams collapse all six into "user need" and lose all of the strategic resolution. ODI insists on keeping them distinct.

## The job statement format

Job-level, not product-level. Solution-free. Verb + object + clarifier.

```
[Verb] [object of the verb] [contextual clarifier]
```

Worked examples:

```
Listen to music while exercising
Decide which jurisdiction to incorporate in
Get a wound to stop bleeding before help arrives
Plan a meal for a family with mixed dietary needs
```

Validation tests:
- **Stable over time.** Would this job statement be true 50 years ago and 50 years from now? If not, you've baked in a solution.
- **Solution-free.** Can multiple solutions get the job done? "Order a ride home" is a job; "Hail a taxi" is not.
- **Performed by a defined job performer.** Who is doing this job? If "everyone," go narrower.

## The desired-outcome statement format

The format that distinguishes ODI from looser JTBD work. Every desired outcome follows the same syntax so it can be measured at scale:

```
Minimize / Increase
the time / likelihood / frequency / number of
[unit of measure]
[contextual clarifier]
```

Worked examples:

```
Minimize the time it takes to determine the right entity type
Minimize the likelihood that the wrong jurisdiction is chosen
Increase the likelihood that the filing is accepted on first submission
Minimize the time it takes to update the registration when the company moves
Minimize the number of decisions that require attorney consultation
```

Why the rigid format: outcomes written this way can be reliably surveyed at scale. The respondent can rate each on **importance** (how important is it to the user that this outcome is achieved?) and **satisfaction** (how well are existing solutions delivering it today?).

A typical core job has 50–150 desired outcomes. An ODI engagement captures all of them.

## The opportunity algorithm

The defining ODI scoring formula:

```
Opportunity Score = Importance + max(Importance − Satisfaction, 0)
```

Where Importance and Satisfaction are both rated 1–10 by the surveyed customer.

Interpretation:
- **Score > 15** — extreme opportunity. Severely underserved.
- **Score 12–15** — significant opportunity. Worth pursuing.
- **Score 10–12** — opportunity. Pursue if strategic fit is high.
- **Score < 10** — overserved or appropriately served. Avoid.

The math captures the asymmetry: high-importance + high-satisfaction outcomes are not opportunities (the market already serves them well). High-importance + low-satisfaction outcomes are the targets.

## The five growth strategies

Once outcomes are scored, ODI identifies which growth strategy applies:

| Strategy | Condition | Move |
|---|---|---|
| **Differentiated** | Underserved outcomes exist; competitors don't address them | Build a solution that addresses them better |
| **Dominant** | Underserved + customers are willing to pay more | Get the job done better *and* cheaper |
| **Disruptive** | Overserved outcomes; segments would accept "good enough" for less | Strip and price down |
| **Discrete** | Underserved customers in a niche | Specialized solution for the niche |
| **Sustaining** | All outcomes adequately served | Incremental improvement only |

Naming the strategy explicitly prevents the "we'll be different *and* cheaper *and* premium" trap.

## The universal job map (8 stages)

Every job has stages. Ulwick's universal map:

```
1. Define   — what they need to get done; what's at stake
2. Locate   — find inputs / information needed
3. Prepare  — set up to do the job
4. Confirm  — verify they have what they need to proceed
5. Execute  — perform the job
6. Monitor  — verify the job is being done well
7. Modify   — make adjustments
8. Conclude — wrap up; transition to next thing
```

Use this as a checklist when mapping a job. Most teams skip stages (especially Confirm and Monitor), which is where most underserved outcomes hide.

## When ODI vs. lighter JTBD

Use ODI (heavier) when:
- The decision is multi-year and high-investment (platform, M&A, market entry).
- The organization has access to a survey panel or is willing to recruit one (200–400 respondents minimum for meaningful opportunity scoring).
- The team will commit to outcome-statement rigor — no shortcuts on the format.

Use Kalbach's plays (lighter) when:
- The decision is shorter-cycle (next quarter's roadmap, a feature debate).
- The team needs JTBD framing fast and is choosing between qualitative-only or no JTBD at all.
- The organization is new to JTBD and needs a starting point that doesn't require buying a survey panel.

The two methodologies are not in conflict. Many teams use Kalbach's qual plays to scope the work and Ulwick's quantitative ODI when the decision warrants the cost.

## What Ulwick gets right that lighter JTBD often gets wrong

- **Solutions don't appear in job statements.** ODI's discipline keeps the statement abstract enough to admit multiple solutions.
- **Outcomes are measurable.** "Reduce friction" is not an outcome; it's a hope. ODI's syntax forces measurability.
- **Importance and satisfaction must be measured separately.** Without satisfaction data, "important" is everything.
- **The market is the segment.** ODI segments by job-level needs (people who share underserved outcomes), not by demographics. The segments emerge from the data.

## What to push back on

- **The 84-step process.** Real engagements rarely run all 84 steps. The skeleton (define customer → define job → uncover outcomes → score → identify strategy) is what matters.
- **Quantitative-only insistence.** Ulwick will do 50 qualitative interviews and 200+ surveys. For many teams, that's overkill and the money would be better spent on faster qualitative cycles.
- **"86% success rate" claim.** Treat as marketing, not as evidence.

## Sources

- Ulwick, A. *Jobs to be Done: Theory to Practice*. Idea Bite Press, 2016.
- Ulwick, A. *What Customers Want*. McGraw-Hill, 2005.
- Ulwick, A. & Bettencourt, L. "The Customer-Centered Innovation Map." *Harvard Business Review*, May 2008.
- Strategyn. ODI methodology — https://strategyn.com/outcome-driven-innovation/
