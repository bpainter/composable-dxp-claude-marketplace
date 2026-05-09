---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-customer-feedback-synthesizer]]"
tags: [type/reference, plugin/cx, scope/skill, topic/feedback-loops, source/gothelf]
status: active
---
# Feedback loop design

Drawn from Gothelf & Seiden, *Sense and Respond* (2017), and the operational practices behind continuous discovery teams (Torres, Perri).

The synthesizer skill covers *how* to analyze feedback once it's in front of you. This reference covers *how to design the loop that produces and consumes feedback continuously* — without it, the synthesizer is a one-time engagement, not an ongoing function.

## The two failure modes that loops fix

**Failure 1: Sensing without responding.** Research function produces reports; nothing changes. Symptom: shelves of decks, "we already studied that," frustrated researchers.

**Failure 2: Responding without sensing.** Team ships continuously without aggregating signal. Symptom: feature factory, vanity metrics, surprise churn.

A working loop solves both. Sensing produces signal at a cadence the team can consume. Responding closes the loop by changing the experience based on signal.

## The minimum loop

```
1. Source — where the signal is captured.
2. Aggregator — the synthesis function (this skill).
3. Channel — how the signal reaches the team that responds.
4. Cadence — how often the cycle runs.
5. Response — the team that changes the experience.
6. Verification — was the signal addressed?
```

Without all six, it's not a loop. It's an artifact.

## Sources of signal

Most CX programs have all of these and aggregate few of them. Inventory yours:

- **Behavioral analytics.** Click streams, funnels, feature usage, search queries (especially failed searches).
- **Survey infrastructure.** NPS, CSAT, CES — both scores and open-text.
- **Support tickets.** Volume by category, time-to-resolution, repeat issues.
- **App store / public reviews.** Direct customer voice, lightly filtered.
- **Sales-call recordings.** Why prospects almost-bought-or-didn't (Gong, Chorus, Zoom).
- **Customer-success notes.** What CS teams hear repeatedly.
- **Social listening.** Mentions, sentiment, competitive comparisons.
- **Community forums / Discord / Slack.** Where power users converge.
- **Win/loss interviews.** Direct, structured, post-decision.

The synthesizer's job isn't to consume all of them. It's to triage — which sources to monitor weekly, which monthly, which only when investigating a hypothesis.

## Cadence design

Different sources warrant different cadences. Mismatch causes the loop to fail.

| Source | Default cadence | Why |
|---|---|---|
| Support volume by category | Weekly | Operational; detects emerging issues fast |
| NPS scores + open-text | Monthly | Trend signal; need volume |
| App store reviews | Weekly | Public; fast-moving |
| Sales-call themes | Monthly | Pattern-based; need volume |
| Win/loss interviews | Quarterly | Strategic; expensive to run |
| Community / forums | Weekly skim, monthly deep | Light surface, deep dive periodically |
| Behavioral funnel anomalies | Real-time alerts; weekly review | Fast; needs judgment |

Quarterly is too slow for operational signal; weekly is too often for strategic signal.

## The triage discipline

The hardest part of the synthesizer's job is filtering. Most signal is noise. Triage criteria:

```
1. Frequency — how often does this appear?
2. Severity — how strong is the pain or intensity?
3. Reach — how many users / segments are affected?
4. Decision-readiness — is there a decision pending that this informs?
5. Strategic fit — does responding align to current priorities?
```

A signal that scores high on 3+ of these is worth surfacing. One that scores high on only frequency is often a false positive (the loud minority).

## Closing the loop

Most loops fail at the response step. The signal arrives; nothing changes. Three structural fixes:

### 1. Named recipient

Every signal type has a named recipient who is *expected* to respond. Not "product team" — a specific name. Without it, signal disappears into a Slack channel.

### 2. Response cadence

The recipient has a standing slot to consume signal: weekly product review, monthly experience review, quarterly strategy session. The signal is the agenda.

### 3. Verification

After the response is shipped (a feature, a content change, a process fix), the synthesizer verifies whether the signal moved. If not, the response was wrong or insufficient. The loop continues.

```
Signal → triage → named recipient → response → verification → close or repeat
```

If the loop has no verification step, it's a recommendation engine, not a learning system.

## Pattern: customer-feedback briefing

A reusable monthly artifact that converts the loop into a forcing function.

```markdown
# Customer feedback briefing — [Month / Year]

## Top of mind (the 1–3 things to act on)
1. [Pattern] — [Evidence: source + count + intensity] — [Recommended response] — [Owner]
2. [...]
3. [...]

## Watching (signal building, not yet actionable)
- [Pattern + source + early count]

## Closed last cycle
- [Signal] → [Response shipped] → [Verification result]

## Backlog
[Issues raised previously, not yet addressed.]
```

The structure forces five things to be visible:
- What's most important now.
- What might be next.
- What was acted on (and whether it worked).
- What's been raised but ignored.
- Whose name is attached.

If three months go by and the "Closed last cycle" section is empty, the loop is broken.

## Pattern: weekly support digest

Lighter cadence, operational focus.

```
# Support digest — week of [Date]

## Up
- [Category] — [N tickets] vs [last week] vs [4-week avg] — [Likely cause]

## Down
- [Category] — [N tickets] vs [last week]

## New patterns
- [Category not seen before, with sample tickets and hypothesis]

## Action
- [What support / engineering / content is doing this week as a result]
```

The digest is for the operational team that can act this week. The monthly briefing is for the leadership team that can act this quarter.

## Common failure modes

- **One-shot synthesis.** Quarterly reports. Slow loop. Misses signal that needed action this month.
- **No triage.** Everything surfaces equally. Recipients tune out.
- **No owner.** "Product should look at this" — product won't.
- **No verification.** Recommendation made; nobody checks if it landed.
- **Aggregating across sources without weighting.** Twitter complaint = NPS detractor in count, but very different signal quality. Weight by source reliability.
- **Surveying for things that are already in the support tickets.** Survey is expensive; tickets are free. Use surveys for what you can't get any other way.
- **Open-text from NPS treated as a source.** Open-text is noisy and self-selecting. It's a hint generator, not an evidence base. Pair with deeper qual when a hint is interesting.

## Tools commonly used

- **Aggregation:** Dovetail, Condens, EnjoyHQ, Notably (research repositories).
- **Behavioral:** Mixpanel, Amplitude, FullStory, Looker.
- **Support:** Zendesk, Intercom, Freshdesk.
- **Survey:** Delighted, Qualtrics, SurveyMonkey, Typeform.
- **Voice:** Gong, Chorus, Otter.
- **Reviews:** Appbot, AppFollow, Trustpilot's API, manual scrape for low-volume.

The right tool stack matters less than the loop design. A well-designed loop in a spreadsheet outperforms a tool stack with no cadence and no owner.

## How this connects to other skills

- **`cx-customer-research`** — runs the deeper qual when feedback signal hints at a question worth investigating.
- **`cx-jobs-to-be-done`** — frames the signal. "Users complain about X" is noise; "users can't get job Y done because of X" is signal.
- **`cx-journey-mapper`** — locates the signal. Which stage? Which moment of truth?
- **`cx-personalization-strategist`** — the experimentation function that responds to signal.
- **`cx-service-designer`** — the design function that responds to signal that needs structural change.

## Source

- Gothelf, J. & Seiden, J. *Sense and Respond*. Harvard Business Review Press, 2017.
- Torres, T. *Continuous Discovery Habits*. Product Talk, 2021.
- Perri, M. *Escaping the Build Trap*. O'Reilly, 2018.
- See also: [[sense-and-respond-loop]] for the broader operating model.
