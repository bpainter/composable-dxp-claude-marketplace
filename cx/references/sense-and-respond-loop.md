---
type: reference
project: skills-library
scope: plugin
plugin: cx
tags: [type/reference, plugin/cx, scope/plugin, topic/feedback-loops]
status: active
---
# Sense and respond — the two-way conversation

Drawn from Gothelf & Seiden, *Sense and Respond* (2017).

The CX work is not a one-time research → design → ship cycle. It's a continuous conversation: ship something, sense how the market reacts, respond, repeat. This reference is the operating model that ties the plugin's research and feedback skills to ongoing decisions.

## The shift

Old model — plan, build, ship, hope:
- Research → spec → build → release → measure once → next planning cycle.
- Cycle time: 6–18 months.
- Customer feedback enters at the start (research) and the end (CSAT survey).

Sense-and-respond model — outcomes, hypotheses, learning loops:
- Define the outcome we want for the customer.
- Hypothesize the change that produces it.
- Ship the smallest version that tests the hypothesis.
- Sense the response (behavioral data, qual signal, support pattern).
- Respond — adjust the hypothesis, scale the win, kill the loss.
- Cycle time: days to weeks per loop.

The shift is not about velocity. It's about *what counts as done.* Done is not "shipped." Done is "we have evidence the change moved the outcome."

## Outcomes vs. outputs

An **output** is a thing built (feature, page, email, channel). An **outcome** is a behavior change in the customer (used X, returned within 7 days, completed onboarding, recommended us).

Most teams plan in outputs and then can't tell whether they worked. The fix:

```
# Bad
"We will ship a new onboarding flow in Q3."

# Better
"We will increase first-week activation from 32% to 50% in Q3.
 The first hypothesis: a guided onboarding flow that completes a
 representative task in <5 minutes will move it. Test by Q3 week 4."
```

The output (onboarding flow) is the means; the outcome (activation rate) is the goal. If the flow ships and activation doesn't move, the team is not done.

## The hypothesis format

```
We believe that [a change to the experience]
For [a specific user segment]
Will result in [a measurable behavioral outcome]
We will know we are right when [observable signal] reaches [threshold]
```

Worked example:

```
We believe that adding a state-fee calculator on the entity-selection page
For first-time founders comparing Delaware vs. home state
Will result in fewer drop-offs at the checkout surprise-cost moment
We will know we are right when checkout abandonment drops from 18% to <12%
in the four weeks following launch
```

The hypothesis disciplines the team. It names what would *change our mind*. Without it, the team will rationalize any outcome as "directional."

## Two loops, not one

Sense-and-respond runs two loops in parallel:

**Customer loop (small, fast).** Ship → behavioral data + support signal + qual interviews → adjust experience. Days to weeks. Owner: product / experience team.

**Strategic loop (slower).** Aggregate the customer-loop learnings → revisit the outcome targets → adjust the bets. Quarterly. Owner: leadership.

If only the customer loop runs, the team is busy without strategic direction. If only the strategic loop runs, the strategy is divorced from what the market is saying.

## The role each CX skill plays in the loop

- **`cx-customer-research`** — runs the qual loop. Designs the studies that explain *why* a quantitative pattern shows up.
- **`cx-customer-feedback-synthesizer`** — runs the signal triage. Aggregates support, NPS, app store, behavioral data into the patterns the team responds to.
- **`cx-jobs-to-be-done`** — supplies the outcome framing. The behavior change worth chasing is usually framed as a job getting done better.
- **`cx-journey-mapper`** — locates the loop. Which stage is the experiment running on? Which moment of truth is being tested?
- **`cx-personalization-strategist`** — runs experimentation infrastructure (segments, variants, holdout groups).
- **`cx-service-designer`** — designs the change being tested.

If the loop is running but no one is closing the response side — picking up the signal and changing the experience — the loop is not actually a loop. It's a research project.

## Common failure modes

- **Outputs masquerading as outcomes.** "We shipped X" reported as success without behavior data.
- **Vanity metrics.** Pageviews up; activation flat. Track the outcome, not the proxy.
- **Loop runs only at planning time.** Quarterly reviews, no continuous learning. Reverts to plan-build-ship.
- **No mechanism to act on the signal.** Research and feedback functions produce reports; nothing changes. The bottleneck is response, not sensing.
- **Treating qual and quant as alternatives.** Quant tells you *what* moved; qual tells you *why*. Most loops need both.
- **No time budget for sensing.** Teams plan 100% of capacity to building. The loop dies under delivery pressure within two quarters.

## How to introduce this in a Slalom engagement

For Composable DXP work, sense-and-respond shows up most clearly in:
- **Personalization programs** — the loop *is* the program (test, measure, scale, kill).
- **Onboarding redesign** — outcome is activation; the loop is the cadence of changes against that outcome.
- **Search and content discoverability** — sensing comes from query logs and click data; response is content modeling and IA changes.
- **Post-launch hypercare** — name the outcomes you'll watch in the first 90 days, and the cadence on which you'll respond. (See `50_Knowledge/Frameworks/Slalom_Summit/Phase_5_Enhance_and_Operate`.)

Don't sell sense-and-respond as a methodology. Sell it as how the team will know whether the build worked.
