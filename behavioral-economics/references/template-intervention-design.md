---
type: reference
project: skills-library
scope: plugin
plugin: behavioral-economics
tags: [type/reference, plugin/behavioral-economics, scope/template, topic/interventions, source/slalom]
status: active
---

# Template — Intervention Design (Behavior-Change Tactics)

**Source:** Slalom Behavioral Design Model, p. 34 (eight tactics) and pp. 8–10 (case studies). Cross-reference [BeSci tactics](https://besci.org/c/tactics) and [The Decision Lab reference guide](https://thedecisionlab.com/reference-guide) for the broader catalog.

Where the [Choice Architecture template](template-choice-architecture.md) maps the structure of a flow, the Intervention Design template names the **specific behavior-change tactic** applied at each lever. Eight core tactics (Slalom p. 34) plus the broader tactic catalog from BeSci.

## When to use it

- Stage 3 of the model, after Choice Architecture is mapped
- When the question shifts from "what's the friction?" to "what specifically will get people to do the better thing?"
- As input to Use-Case Validation (Stage 4) — each tactic produces a testable hypothesis

## The eight common behavior-change tactics (Slalom p. 34)

### 1. Goal Setting

**Definition:** Encourage individuals to define clear, achievable targets to work towards.

**Behavioral basis:** Specific goals beat "do your best" goals (Locke & Latham). Pre-commitment increases follow-through. Save More Tomorrow combines goal-setting with hyperbolic discounting (commit-now-act-later).

**Use it when:** the desired behavior is a process or habit, not a one-shot decision; the customer needs a sense of progress.

**Examples:** Duolingo streaks (PDF p. 8); Save More Tomorrow contribution-rate ramp (`book-nudge.md` Ch. 9); fitness apps' weekly mileage targets.

### 2. Cognitive Restructuring

**Definition:** Change the thought patterns that lead to undesirable behaviors.

**Behavioral basis:** Reframing alters how the same situation is processed (Kahneman, framing effects, *TFS* pp. 429–442). Borrowed from cognitive-behavioral therapy.

**Use it when:** the barrier is mental (catastrophizing, fixed-mindset thinking, all-or-nothing framing).

**Examples:** Reframing "I failed at the diet" → "I learned what doesn't work for me"; pre-mortem reframing of "we're confident" → "imagine this failed; why?".

### 3. Feedback Loops

**Definition:** Provide real-time information on behaviors to prompt self-reflection.

**Behavioral basis:** Most learning comes from feedback (Nudge "Give Feedback" principle, p. 116–117). Without feedback, errors repeat predictably.

**Use it when:** the desired behavior has a measurable signal that can be surfaced quickly; customers can't tell if they're doing well.

**Examples:** Tesla range-aware GPS; smart-meter home-energy displays; Wellth daily medication-photo + scale (PDF p. 9, ~2× adherence vs. traditional incentives).

### 4. Cue Elimination

**Definition:** Remove triggers from the environment that lead to unhealthy behaviors.

**Behavioral basis:** Behavior = Trigger + Action + Reward (the habit loop). Eliminating the trigger collapses the loop.

**Use it when:** the unwanted behavior is environment-driven and the trigger is removable.

**Examples:** Putting junk food out of sight; turning off social-media notifications; closing accounts that auto-bill.

### 5. Social Proof

**Definition:** Use the influence of peer actions to encourage healthier behaviors.

**Behavioral basis:** Asch conformity (75% conform at least once, *Nudge* pp. 49–53); descriptive norms ("85% of customers like you chose…").

**Use it when:** customers face uncertainty about the right choice; the actual majority behavior is the desired behavior.

**Caution:** boomerang effect — telling people most others *don't* do X reinforces the bad norm. Always pair "you're below average" with "and here's how to catch up." (Schultz et al. energy-bill experiments.)

**Examples:** Hotel towel-reuse signs ("75% of guests in this room reuse their towels"); LinkedIn "10 of your connections endorsed this skill"; default plan recommendations ("most popular among similar customers").

### 6. Stimulus Control

**Definition:** Reduce cues that trigger poor behaviors and increase cues for better behaviors.

**Behavioral basis:** Like cue elimination, but bidirectional — designing the environment to stack cues for the desired behavior.

**Use it when:** the desired behavior is a habit that can be triggered by environment.

**Examples:** Gym clothes laid out the night before; healthy snacks at eye level (Carolyn's cafeteria, *Nudge*); Slack do-not-disturb hours that block notifications during deep work.

### 7. Framing Effects

**Definition:** Highlight the benefits or losses associated with behavior choices.

**Behavioral basis:** Asian disease problem (TFS pp. 435–436); "90 of 100 alive" vs. "10 of 100 dead" surgery framing (*Nudge* p. 39); cash-discount vs. credit-surcharge.

**Use it when:** the same numerical reality can be presented multiple ways and one frame produces the better behavior.

**Examples:** "Avoid losing $350 in energy costs" beats "Save $350" (`book-nudge.md`); plastic-bag fees beat plastic-bag rewards.

### 8. Choice Architecture

**Definition:** Design the environment in which decisions are made.

**Behavioral basis:** The full toolkit from `template-choice-architecture.md` — defaults, partitioning, attribute translation, etc.

**Use it when:** the previous tactics aren't enough on their own and you need to redesign the surface customers interact with.

**Examples:** All Stage 3 work; the Dutch insurance broker's price+quality reordering with partitioning (PDF p. 10, +13% selection of first-ranked option).

## The Behavior = Trigger + Action + Reward loop (PDF p. 8)

A meta-tactic. Most behavior-change interventions can be decomposed into:

- **Trigger** — what cues the action (notification, time of day, location, emotion)
- **Action** — the behavior itself (open the app, take the medication, log the data)
- **Reward** — what reinforces it (achievement badge, social recognition, financial benefit, intrinsic satisfaction)

Duolingo's three playbooks map directly: emotional/push/email triggers; pre-commitment + social-nudging + streaks (action); celebrations/achievements/badges (reward).

When designing an intervention, walk through this loop explicitly. A common failure mode: building a strong trigger and reward but a hard or ambiguous action.

## Extended catalog

The eight Slalom tactics are the most-used; the broader catalog includes:

- **Reminders & prompts** — just-in-time cues (Gmail attachment reminder)
- **Commitment devices** — Stickk.com, Save More Tomorrow, Ulysses contracts
- **Pre-commitment** — Ariely's three-class deadline study (`book-predictably-irrational.md` Ch. 6)
- **Incentivization** — financial, points, recognition; tune for loss-aversion frame
- **Make it easy / channel factors** — Yale tetanus study, 3% → 28% follow-through
- **Implementation intentions** — "When X happens, I will do Y" (Gollwitzer)
- **Public commitment** — announcing the goal publicly raises follow-through
- **Mental contrasting** — explicitly imagine obstacles before goal pursuit (Oettingen)
- **Identity priming** — "be a voter" beats "go vote" (Bryan et al.)
- **Personalization** — defaults, content, messaging tuned to the individual
- **Partitioning** — explicit time/space partitions to reduce overconsumption (Cheema & Soman)
- **Salience boosts** — vivid, recent, personal framing
- **Habit stacking** — link new behavior to existing routine (BJ Fogg, James Clear)

For full catalogs, see [BeSci tactics](https://besci.org/c/tactics) and [The Decision Lab reference guide](https://thedecisionlab.com/reference-guide).

## Template (copy this for each intervention)

```markdown
# Intervention — [Name]

## Target behavior
[The specific behavior this intervention is designed to produce]

Reference: [Behavioral Profile, choice-architecture step]

## Tactic(s) selected

| Tactic | Why this tactic | Where it applies in the flow |
|---|---|---|
| [e.g., Loss-aversion framing] | [The behavioral barrier matches this tactic's strengths] | [Step N in the choice architecture] |

## Trigger / Action / Reward decomposition

- **Trigger:** [What cues the action]
- **Action:** [The behavior we want]
- **Reward:** [What reinforces it]

## Hypothesis (feeds Use-Case Validation)

By implementing [intervention],
we can [behavior change action]
for [customer segment(s)],
leading to [expected positive outcome]
that will achieve [measurable result].

## Anti-patterns to watch for
- [Reactance — overly aggressive default]
- [Boomerang — descriptive norm reinforces wrong behavior]
- [Cross-tactic interference — two levers cancel each other out]

## Ethics check (preview of Stage 5)
- Coercive? No, because: ___
- Welfare-promoting? Yes, because: ___
- Transparent? Yes, because: ___
- Non-deceptive? Yes, because: ___
- Equitable? Yes, because: ___

## How we'll validate
[Brief description; full detail in template-use-case-validation.md]
```

## Worked example — Wellth medication adherence (PDF p. 9)

**Target behavior:** Daily medication adherence in chronic-disease patients.

**Stack of tactics:**
- **Loss aversion** (primary) — upfront cash reward, daily forfeit if missed
- **Feedback loops** — patient must take a photo of medication + log weight on Bluetooth scale
- **Goal setting** — daily streak vs. baseline
- **Stimulus control** — daily routine cue + scale
- **Choice architecture** — app interaction simplified to 2 actions

**Trigger / Action / Reward:**
- Trigger: time-of-day app notification + scale presence
- Action: photo medication + step on scale
- Reward: streak preserved (no forfeit) + cumulative reward visible

**Outcome:** ~2× adherence vs. traditional incentives; 90% adherence; 40% reduction in hospital readmissions.

## Pitfalls

- **Picking a tactic without a behavioral diagnosis.** Tactics are hammers; if you don't know the nail (the bias and barrier), you'll mismatch.
- **Single-tactic thinking.** Most successful interventions stack 3–5 tactics. Wellth uses loss aversion + feedback + goal-setting + stimulus control + simplification together.
- **Reward fatigue.** Extrinsic rewards displace intrinsic motivation (`book-predictably-irrational.md` Ch. 4: social vs. market norms). Avoid pricing favors.
- **Boomerang from social proof.** "85% of patients miss their meds" reinforces the wrong norm. Always frame the target behavior, not the wrong one.
- **Ignoring measurement.** The intervention without a measurable signal is a guess.
- **Treating the tactic catalog as a menu.** The catalog is a starting point; the right intervention often requires combining or adapting tactics to the specific flow.

## Hand-off downstream

- **Use-Case Scoring** (`template-use-case-scoring.md`) — score each intervention on Customer Value × Business Value
- **Use-Case Validation** (`template-use-case-validation.md`) — turn each hypothesis into a test
- **Ethics gate** (Slalom p. 43; see `template-use-case-validation.md`)

## Skills that use this template

- `behavioral-economics-choice-architect` (primary)
- `behavioral-economics-behavioral-economist` (cross-domain)
- `behavioral-economics-organizational-behavior-specialist` (HR/team behavior)
- `behavioral-economics-behavioral-economist-social-cognition` (uses for messaging interventions)
