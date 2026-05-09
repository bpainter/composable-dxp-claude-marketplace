---
type: reference
project: skills-library
scope: plugin
plugin: behavioral-economics
tags: [type/reference, plugin/behavioral-economics, scope/template, topic/validation, topic/experiments, source/slalom]
status: active
---

# Template — Use-Case Validation

**Source:** Slalom Behavioral Design Model, p. 41 (validation template) and p. 43 (ethics gate). See `2026-05_Slalom-Behavioral-Design-Model.md` in `40_Library/Solution_Briefs/`.

The Use-Case Validation template turns a prioritized intervention into a runnable test. It's Stage 4 of the model, after scoring (Stage 3) and before scaling (Stage 6). Combined with the Stage 5 ethics gate, it produces a complete record of "what we tested, why, what we measured, and whether it's safe to ship."

## When to use it

- Stage 4 of the model — every prioritized intervention from `template-use-case-scoring.md` should generate one validation record
- Before any A/B test, pilot, or simulation involving real customer behavior
- As the documentation artifact that the team and any compliance/ethics review can sign off on
- When defending an intervention's design decision to stakeholders weeks or months later

## The seven fields (Slalom p. 41)

```markdown
# Use-Case Validation Record — [Intervention Name]

## Test Number
[Identifier for documentation, e.g., BD-2026-05-001]

## Hypothesis to Test
A clear, testable statement predicting the outcome. Specific and measurable.

By implementing [proposed intervention],
we can [action to overcome challenge]
for [customer segment(s)],
leading to [expected positive outcome]
that will achieve [measurable result].

## Description of What Will Be Tested
The methodology — including sample size, control groups, and the interventions being tested.
- What will the test consist of?
- What will we create?
- How will we test it?

## Validation Criteria
The benchmarks that determine the success of the test.
- What will we measure? (Primary metric, secondary metrics)
- What minimum success rate / lift / satisfaction levels do we expect?
- What's the threshold for "ship," "modify and retest," and "kill"?
- What time window?

## Resources Needed
- Financial budget:
- Personnel: [roles, time commitment]
- Materials/tools: [analytics, design files, prototypes]
- External (customers, panel access, vendors):

## Fidelity and Testing Tool
The level of detail and realism. Pick one:
- **Low fidelity** — paper prototype, click-through mockup, sketch
- **Medium fidelity** — interactive prototype, partial integration
- **High fidelity** — production-grade, real flow, real data

Approach:
- [ ] Pilot (small subset of users in production)
- [ ] A/B test (random assignment, statistical comparison)
- [ ] Simulation / lab experiment (not in production)
- [ ] Quasi-experiment (natural variation, observational)

## Results (filled after running)
- Primary metric:
- Secondary metrics:
- Statistical significance:
- Heterogeneous treatment effects (HTE) noticed:
- Unintended consequences:
- Decision: [Ship / Modify / Kill]
- Date completed:
```

## The Stage 5 ethics gate (Slalom p. 43)

Run this **before** scaling, ideally before the test starts. Five questions; all five must be answerable affirmatively.

### 1. Are we inhibiting freedom of choice?

- Validate that nudges do not coerce or significantly constrain available options.
- Confirm that opting out is as easy as opting in.

**Test:** Can a customer arrive at the alternative outcome in ≤2 clicks / minimal time? If not, the nudge has tipped into a mandate.

### 2. Do we promote customer welfare?

- Assess whether the nudges contribute positively to the customer's outcomes (health, financial, well-being).
- Avoid nudges that benefit the organization at the expense of the customer.

**Test:** State the customer-welfare outcome separately from the business outcome. If you can't write the customer-welfare outcome, the intervention is sludge candidacy.

### 3. Are we transparent in how we're influencing choice?

- Disclose the intent behind the intervention and how it functions.
- Ensure customers know all their options and the consequences of each.

**Test:** Could you explain the design publicly without losing customer trust? (Thaler & Sunstein's "Publicity Principle," `book-nudge.md` p. 273.) Research shows transparency does not reduce nudge effectiveness and often increases it.

### 4. Are we avoiding deceptive practices?

- Respect the customer's ability to make informed choices without deception.
- Provide clear and accurate information.

**Test:** Are any claims false, misleading, or omitting material information? Are price components shrouded? (`book-nudge.md` Ch. 8 on sludge.)

### 5. Is our choice architecture equitable?

- Ensure nudges don't favor one customer group over another without justification.
- Avoid exploiting vulnerabilities of specific demographics.

**Test:** Run the intervention through demographic sub-segments. Are the outcomes proportional? Are vulnerable populations (low income, low literacy, elderly, cognitively impaired) being differentially harmed?

## Hypothesis-writing rules

The Slalom hypothesis template is structured for a reason. Common failure modes:

- **Vague intervention.** "We make the page better" is not a testable intervention. Name the specific change.
- **Missing segment.** "Customers will…" — which customers? Specify the segment from the Behavioral Profile.
- **Unmeasurable outcome.** "Improved engagement" is not measurable. "Daily active retention rises ≥5% within 30 days of enrollment" is.
- **No threshold.** Without a "ship/modify/kill" threshold, the test outcome will be argued forever.
- **Single metric.** Optimize a primary metric; track 2–3 secondaries to catch unintended consequences.

## Fidelity selection

Match fidelity to the **risk** and **decision-making cost** of being wrong:

| Risk if wrong | Fidelity |
|---|---|
| Reversible, low cost | Low — paper, click-through, paint-the-future |
| Reversible, moderate cost | Medium — prototype, in-app simulation |
| High cost or hard to reverse | High — production A/B with guardrails |
| Affects compliance, finances, health | High + extra ethics review + small initial sample |

For behavior-change interventions in customer-facing flows, A/B testing in production with a small initial cohort (1–10% of traffic) and clear stop conditions is the standard approach.

## Worked example — Wellth medication adherence (PDF p. 9)

**Test Number:** WL-2024-001

**Hypothesis:**
> By implementing an upfront cash reward + daily forfeit + photo-and-scale verification,
> we can increase daily medication adherence
> for chronic-disease patients with poor adherence baselines,
> leading to fewer hospital readmissions
> that will achieve ≥40% reduction in 90-day readmissions and ≥80% medication adherence.

**Description:** Random assignment of N patients to control (standard care) vs. treatment (Wellth program). 90-day window. Primary metric: medication adherence (verified via photo + scale). Secondary: 90-day readmission rate, app engagement, self-reported satisfaction.

**Validation Criteria:**
- Primary: ≥80% adherence in treatment vs. baseline ~50%
- Secondary: ≥30% reduction in readmissions
- Stop conditions: <55% adherence at 30-day check → kill; concerning safety signal → kill immediately

**Resources:** Cash reward budget; Bluetooth scales; nursing staff for onboarding; engineering for app; analytics for tracking; ethics-board approval.

**Fidelity:** High — production pilot in clinical setting.

**Approach:** Quasi-experiment with matched controls (full RCT not feasible in clinical context).

**Results (per PDF p. 9):**
- ~2× adherence vs. traditional incentives
- 90% adherence in treatment
- 40% reduction in readmissions
- Decision: scale (Stage 6)

## Pitfalls

- **Designing the test around what you want to find.** Bias the design toward falsification, not confirmation.
- **Too small a sample for behavioral effects.** Behavioral interventions often show 2–10% lifts; detecting them needs substantial sample size. Use a power calculation; don't eyeball it.
- **Skipping HTE analysis.** Population-average effects can hide important sub-group divergence. Choudhry's free-statins study showed highest-risk patients responded *most* to price changes (`book-nudge.md` p. 196).
- **Anchored on the average.** Bimodal responses (Haggag taxi-tip study, `book-nudge.md` p. 30) — higher anchors raised average tip *and* zero-tip share. Always look at the full distribution, not just the mean.
- **Running the ethics gate after the test.** The gate must run before you can ethically test on customers, not after the data is in.
- **No stop conditions.** Tests run forever waiting for "more data." Pre-commit to thresholds.
- **Forgetting to record results.** The validation record without results is just a brief.
- **Over-fidelity.** A high-fidelity production test of a low-confidence idea wastes resources. Start cheap.

## Skills that use this template

- `behavioral-economics-research-methods` (primary — owns the validation methodology)
- `behavioral-economics-statistician` (sample size, analysis plan, results)
- `behavioral-economics-choice-architect` (consumer of validation results)
- `behavioral-economics-behavioral-economist` (cross-domain validation oversight)
- `consulting-management-consultant` (engagement-level governance of validation programs)
