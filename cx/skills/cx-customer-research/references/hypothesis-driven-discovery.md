---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-customer-research]]"
tags: [type/reference, plugin/cx, scope/skill, topic/research, topic/lean, source/gothelf]
status: active
---
# Hypothesis-driven discovery

Drawn from Gothelf & Seiden, *Sense and Respond* (2017) and *Lean UX*. Pairs research with the operating model in [[sense-and-respond-loop]].

The classic research engagement: scope a study, run it, deliver a report, hand off. Findings become a deck. The deck dies in a folder.

The hypothesis-driven alternative: research is structured around specific decisions. Each round of research tests a named hypothesis. The output is a *decision*, not a report. The cycle is short.

This reference covers when to use which mode and how to structure hypothesis-driven research.

## When to use which mode

| Mode | Use when |
|---|---|
| **Discovery research** (open) | Entering a new domain. Don't know enough to form hypotheses. |
| **Hypothesis-driven research** (focused) | Have hypotheses worth testing. Decision pending. |
| **Validation research** (confirm) | Hypothesis is strong; need to verify before scaling. |

Most teams over-invest in discovery (familiar) and under-invest in hypothesis-driven (uncomfortable, fast). Most decisions are better served by hypothesis-driven cycles.

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
Will result in fewer drop-offs at the surprise-cost moment
We will know we are right when checkout abandonment drops from 18% to <12%
in the four weeks following launch
```

The hypothesis disciplines the team. It names what would *change our mind*. Without it, the team will rationalize any outcome as "directional."

## The discovery → hypothesis → test cycle

```
1. Discovery (1–4 weeks)
   - Understand the domain. Interviews, observations, current-state map.
   - Output: a list of hypotheses worth testing.

2. Hypothesis (1 day)
   - Pick the highest-leverage hypothesis.
   - Write it in the format above.
   - Define the smallest test that would falsify it.

3. Test (1–4 weeks)
   - Run the smallest possible experiment.
   - Hold yourself to the falsifiable signal.

4. Decide (1 day)
   - Confirmed → scale.
   - Rejected → reformulate or kill.
   - Inconclusive → the test wasn't well-designed; redesign or move on.

5. Repeat with the next hypothesis.
```

Total cycle: 3–7 weeks. Some teams compress to 1–2 weeks. The research methods stay the same; the framing changes.

## Method selection by hypothesis type

| Hypothesis is about … | Use … |
|---|---|
| Will users understand X? | Cognitive walkthrough, concept test |
| Will users do X? | Behavioral test, prototype |
| Why do users do X? | Open interview, contextual observation |
| How many users do X? | Survey, behavioral analytics |
| Did the change move X? | Quasi-experiment, before/after analysis, A/B test |

Pick the cheapest method that can falsify the hypothesis. If users will say "yes I understand" to a confusing concept (they often do), behavior beats self-report.

## What "smallest possible" looks like

A hypothesis test should be designed for *falsification*, not for *confirmation*. The cheapest valid test is usually:

- **Concept testing** — show the idea, ask what it does, ask what they'd do next.
- **Prototype testing** — paper, clickable, or coded prototype with the change.
- **Wizard-of-Oz** — manually simulate the system response while the user thinks it's real.
- **Smoke test** — a landing page with the offer; measure intent without building the offer.
- **Pilot** — limited rollout to a constrained segment; behavioral data after.

Common over-investments to avoid:
- Building the full feature before testing.
- Running an A/B test on a change that hasn't been concept-tested.
- Surveying for a "yes I'd want this" before observing behavior.

## When the hypothesis is wrong

Most hypotheses turn out to be partially or fully wrong. That's the point — that's what makes the cycle valuable.

When rejected:
- Don't rationalize. The signal said no.
- Reformulate: was the change wrong, or was the segment wrong, or was the outcome wrong?
- Kill or repair. Often there's a smaller, sharper hypothesis hiding inside the rejected one.

Teams that never have rejected hypotheses are running the cycle wrong. Either the hypotheses are too vague to falsify, or the team is shaping the test to confirm.

## Pairing with traditional research

Hypothesis-driven discovery doesn't replace traditional research — it integrates with it.

- **Use traditional discovery** when entering a domain. The output of discovery is the hypothesis backlog.
- **Use hypothesis-driven** when you have a backlog and a decision to make.
- **Use validation research** to verify a hypothesis at scale before the strategy is bet on it.

The mistake is using one mode where another fits. Discovery applied to a clear question wastes time; hypothesis-driven applied to an unfamiliar domain produces premature commitments.

## Common failure modes

- **Hypotheses that can't be falsified.** "Users will value X" — what would prove they don't? If nothing, it's not a hypothesis.
- **No threshold.** "Activation will improve" — by how much? Without a number, any outcome confirms.
- **Tests that take longer than the decision they inform.** A 12-week test for a 2-week decision is research theater.
- **No mechanism to act.** Hypothesis tested, signal received, nothing changes. The bottleneck is response, not test design.
- **Confusing hypothesis with prediction.** "I think users will…" is a guess. "We will believe X if signal Y reaches Z" is a hypothesis with falsifiability.
- **Survey-only hypothesis tests.** People say things they don't do. Behavioral tests are stronger; pair survey with behavior whenever possible.

## How this fits the broader skill

This reference is the methodological extension of `cx-customer-research`. The core skill covers traditional research (discovery, qual, quant, mixed). This reference adds the operating mode that pairs with `cx-personalization-strategist`'s experimentation work and `cx-customer-feedback-synthesizer`'s ongoing signal triage.

When advising clients on research, default to:
- Hypothesis-driven for product / experience decisions in active development.
- Traditional for strategic / market / segmentation work.
- Both running in parallel for mature CX programs.

## Sources

- Gothelf, J. & Seiden, J. *Sense and Respond*. Harvard Business Review Press, 2017.
- Gothelf, J. & Seiden, J. *Lean UX*. O'Reilly, 3rd ed., 2021.
- Maurya, A. *Running Lean*. O'Reilly, 3rd ed., 2022 (the leaner adjacent practice).
- See also: [[sense-and-respond-loop]] for the operating model around it.
