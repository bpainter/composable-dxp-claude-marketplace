---
type: reference
project: skills-library
scope: plugin
plugin: behavioral-economics
tags: [type/reference, plugin/behavioral-economics, scope/foundational, topic/behavioral-econ, topic/choice-architecture, topic/product-design, source/wendel]
status: active
---

# Designing for Behavior Change — Stephen Wendel (O'Reilly, 2013)

**Source digest** for the behavioral-economics plugin. Cite as "(Wendel, *Designing for Behavior Change*, p. X)". Page numbers refer to the 2013 O'Reilly first edition. Foreword by BJ Fogg.

## Thesis: behavioral economics as a product-design discipline

Where Kahneman catalogues biases, Thaler & Sunstein argue for nudge as policy, and Ariely entertains with experiments, Wendel asks the missing engineering question: **how do you methodically build a product that helps a specific user take a specific action?** The book treats behavior change as a four-stage design process — Understand, Discover, Design, Refine — modeled on lean/agile development and explicitly positioned to "layer on top of" any existing methodology (p. xvii). Wendel's distinct contribution is that he replaces the "pile of tactics" model of applied behavioral science with **codified workflows, named artifacts, and a diagnostic funnel** that lets a product team isolate which precondition for action is actually failing. The book is operational where Kahneman is theoretical, individual-product-focused where Thaler & Sunstein are policy-focused, and systematic where Ariely is anecdotal.

## The CREATE Action Funnel — Wendel's signature model

Five preconditions must all hold, *at the same moment*, for a person to take a conscious action (Ch. 2, pp. 26–39). They form an acronym: **C-R-E-A-T-E** (Cue, Reaction, Evaluation, Ability, Timing → Execute). Visualize as a leaky funnel where users drop out at every stage (Figure 2-1, p. 39).

| Gate | What happens | What "fails" most often |
|---|---|---|
| **Cue** (p. 29) | Something — external (notification, sight) or internal (hunger, boredom) — makes the action cross the user's mind. Inattentional blindness (the gorilla study) means most possible actions never even register. | The cue is absent, ignored among competing cues, or not tied to the user's existing routine. |
| **Reaction** (p. 30) | System 1 produces an instant gut verdict (interesting? safe? boring?) and activates competing actions. Trust, prior experience, mood all weigh in. The conscious mind never sees what got vetoed here. | Distrust, ugly UI, bad first-time experience, or a more appealing alternative gets activated. |
| **Evaluation** (p. 32) | System 2 weighs costs and benefits *relative to alternatives*. Most product designers focus here; most users don't get here. Value is what *the user* ascribes, not what the company ascribes. | The user's perceived value is lower than yours, or the alternative (Netflix, sleep) wins. |
| **Ability** (p. 34) | Four sub-tests: (1) **action plan** — knows the steps; (2) **resources** — has what's needed; (3) **skills** — can execute; (4) **belief in success** (self-efficacy). | Any one missing forces "I'll do it later" — and "later" requires the funnel to fire again. |
| **Timing** (p. 36) | Why now and not later? External urgency (April 15 taxes), internal urgency (hunger), specificity ("Thursday at 8 p.m."), or pre-commitment. | "Beneficial" actions (exercise, saving, gardens) almost always lose to "more urgent" ones. |

**The funnel diagnoses failure.** When users abandon a flow, you don't ask "what tactic should we add?" — you ask which gate is leaking and apply the matching tactic. This is the diagnostic move that distinguishes Wendel from Anderson, Lockton, Weinschenk, and other tactic-catalog authors (p. 176).

For habitual actions, Evaluation and Timing short-circuit — the cue triggers the routine directly (p. 27, p. 173).

## The four-stage design process

The book's spine (Preface pp. xv–xix; Conclusion Ch. 16, pp. 287–294):

1. **Understand** how the mind decides — the funnel and the three strategies.
2. **Discover** the right outcome, actor, and action (Part II, Ch. 4–5).
3. **Design** the conceptual plan, then the interface (Parts III–IV, Ch. 6–11).
4. **Refine** through measurement and iteration (Part V, Ch. 12–14).

**Contrast with Slalom's six-stage Behavioral Design Model.** Slalom expands stage 2 ("Discover") into separate Behavioral-Profile, Choice-Architecture, and Intervention-Design stages, and stage 4 ("Refine") into Use-Case-Scoring and Use-Case-Validation. Wendel collapses these because his audience is a *single product team*; Slalom's audience is consultants serving *multiple stakeholders*. The mappings:

| Slalom stage | Wendel stage | Wendel chapter |
|---|---|---|
| 1. Foundational research | Understand | Ch. 1–3 |
| 2. Behavioral Profile | Discover (actor + persona) | Ch. 5 (pp. 100–103) |
| 3. Choice Architecture | Design — Conceptual / Action | Ch. 6 (Behavioral Plan) |
| 4. Intervention Design | Design — Environment + User | Ch. 7–8, Ch. 10 (Table 10-1) |
| 5. Use-Case Scoring | Discover (action selection) | Ch. 5 (pp. 103–105) |
| 6. Use-Case Validation | Refine | Ch. 12–14 |

## Three strategies for behavior change

The choice of strategy (Ch. 3) is logically prior to any tactic. Pick one:

| Strategy | What it is | What user consciously chooses | Use when |
|---|---|---|---|
| **Cheat** (p. 49) | Default it, make it incidental, or automate the act of repetition. The action happens with informed consent rather than active work. | Whether to give consent. | The action is doable on the user's behalf and personalization isn't critical. |
| **Make/change habits** (p. 58) | Cue → routine → reward, repeated until the conscious mind drops out (per Duhigg's *Power of Habit*, building on operant conditioning). | Whether to set up the habit-formation conditions. | The action is frequent, contextually consistent, and rewardable. |
| **Support conscious action** (p. 67) | Walk the user through all five funnel gates every time. The hardest path. | Whether to take the action this time. | Cheating and habits aren't feasible; action is novel, complex, or one-off. |

Wendel's heterodox argument: **cheat first** (p. 56). Auto-enrollment, iodized salt, prize-linked savings, intelligent defaults on cameras. "There are no martyrs in beneficial behavior change" (p. 56). This is more aggressive than Thaler & Sunstein, who treat defaults as one nudge among many; Wendel treats them as the first move.

## Wendel's relationship to Fogg's B = MAT

Wendel explicitly **builds on and extends BJ Fogg's Behavior Model** (B = MAT: Motivation × Ability × Trigger; pp. 28, 42–43). Three key differences:

1. **Wendel adds Reaction and Timing** as separate gates. Fogg folds reaction into motivation; Wendel argues the intuitive verdict can veto an action before motivation is even consulted (p. 28). Fogg has no separate construct for urgency; Wendel argues "April 15 taxes" can't be explained by motivation, ability, or trigger alone (p. 28).
2. **Wendel makes the diminishing-returns insight explicit.** Fogg's curve (Figure 2-2, p. 42) shows extra motivation or ability buys you less and less. Practical lesson: don't over-invest in motivation when the action is already easy and motivated.
3. **Tiny Habits.** Fogg's foreword (p. ix–xi) describes Tiny Habits — pick a tiny version of the behavior, anchor it to an existing routine — as the "feather" of habit design. Wendel's "Minimum Viable Action" (p. 89) is the product-team analog: cut the target action to its smallest testable form and build from there.

## Concrete artifacts Wendel codifies

These are template-ready. Each is a named, repeatable workflow output.

### 1. Behavioral Plan (Ch. 6, pp. 113–124) — the central artifact

A start-to-finish narrative of how a user moves from inaction to taking the target action, both inside and outside the product. Construction sequence:

1. **Write out the rough sequence of real-world steps** (not just product steps). The radio-call example (pp. 115) lists nine: find a quiet place, identify the program, listen for the right moment, get the number, work up the gumption, call, get past the screener, say something intelligible, report back (p. 115).
2. **Label each step**: in-product / product-response / real-world (p. 117).
3. **Look for missing steps for new users**, **and one-time-only steps for experienced users** (p. 117).
4. **Tailor it** — walk each persona through and note thoughts, motivations, obstacles (p. 118).
5. **Simplify it** — pare to the Minimum Viable Action; cut repeated actions to first action (p. 119).
6. **Make it easy** — automate, default, build on existing skills (p. 121).

Expressible as a customer-experience map (Figure 6-2), a flowchart with sidebar notes, a written narrative, or a hierarchical outline (p. 116). The Behavioral Plan **is** the conceptual design and feeds directly into agile user stories or waterfall functional specs (p. xviii, p. 156).

### 2. Behavioral Persona (Ch. 5, pp. 100–103) — persona for behavior change

Distinct from a generic UX persona. A Behavioral Persona is built around five behavior-relevant questions (Table 5-1, p. 101):

1. **Experience with similar actions** (have they done this before?)
2. **Experience with similar products** (which features failed last time?)
3. **Relationship to the company** (do they trust you?)
4. **Existing motivations** around the target action
5. **Hard barriers to action**

Plus a one-paragraph bio. Wendel's HelloWallet "Frugals vs. Spendthrifts" pair (p. 101) is the canonical example. **Generated per target action**, because the same person can be a Frugal for emergency savings and a Spendthrift for retirement (p. 102). Personas should be **exhaustive and mutually exclusive** — every user fits one and only one (p. 102).

### 3. Action-Funnel diagnosis (Ch. 13, pp. 242–246)

When a flow leaks, walk the failing screen back through CREATE and ask, for each gate, "what would fail here?" Generates a hypothesis tree and testable changes. Used as the structured-debugging tool when interviews and analytics don't produce an obvious cause (p. 243).

### 4. Intervention catalog by funnel gate (Table 10-1, pp. 176–177)

Two dozen tactics organized by which CREATE gate they affect. Excerpt:

| Gate | Goal | Tactic |
|---|---|---|
| **Cue** | Cue action | Tell the user what the action is |
| | Increase power of cue | Make it clear where to act; clear distractions |
| **Reaction** | Increase trust | Make the site professional and beautiful |
| | | Deploy social proof; display authority; be authentic |
| **Evaluation** | Increase motivation | Prime user-relevant associations; leverage loss aversion; peer comparisons; competition |
| | Decrease cost | Avoid cognitive overhead; avoid choice overload |
| | | Avoid direct payments (extrinsic-crowds-out-intrinsic) |
| **Ability** | Increase logistical ability | Elicit implementation intentions |
| | Decrease constraints | Default everything; cheat |
| | Self-efficacy | Positive peer comparisons |
| **Timing** | Increase urgency | Frame to avoid temporal myopia; remind of prior commitment; commitments to friends; scarcity |

This is the most directly transferable artifact in the book. Slalom's eight tactics are a strict subset; Wendel adds eighteen more, each tagged to its funnel gate.

### 5. Causal Map (Ch. 13, pp. 239–241) and Data Bridge (Ch. 12, pp. 225–230)

The **Causal Map** diagrams every path — ideal, alternative-in-product, alternative-out-of-product — by which a user can reach the target outcome. Distinguishes ideal-blue, alternative-green, external-red. Used to (a) target the application better, (b) control for confounders in pre-post analysis, (c) forecast impact of feature changes (p. 241).

The **Data Bridge** is a statistical relationship that links a measurable in-product behavior to an unmeasurable real-world outcome (e.g., "60% of users who indicate they will plant a vegetable garden actually plant one" — p. 299). Required when the outcome happens outside the product. Built from existing research or a small pilot.

### 6. Measurement plan for behavioral A/B tests (Ch. 12, pp. 211–225)

A/B test cookbook: random-assignment via roulette wheel (p. 218); pre-pick the stopping rule (p. 219, citing List et al. 2010 on optional-stopping bias); staggered rollout when withholding the product is unethical (p. 219); matching/quasi-experiments as second-best (p. 220); pre-post analysis with informal then formal confound control (p. 222). Wendel is unusually firm that **multivariate testing is just an experimental design**, so use Optimizely or Maxymiser rather than rolling your own (p. 217).

## 7 patterns and insights worth quoting

1. **"There are no martyrs in beneficial behavior change."** (p. 56) The do-gooder reflex that users should *earn* the outcome by working hard is wrong; if you can engineer the work away, do.
2. **"Watch their behavior, and don't listen to their mouths."** (p. 31) Self-report taps the conscious mind; the intuitive mind drives the action and never appears in surveys.
3. **"Inverts the funnel."** (p. 57) Cheating succeeds when the user *fails* to pass through the funnel — inaction becomes the desired outcome.
4. **"The more mental leaps that are required between what we see and the action, the less likely it is that the action will cross our minds before we're distracted by something else."** (p. 178) The Curtis blog experiment: "I'm on Twitter" → 4.7% clicks; "Follow me on Twitter" → 7.31%; "You should follow me on Twitter here" → 12.81%.
5. **Diminishing marginal returns on motivation and ability.** (Fogg's curve, p. 42) A common product-team error is doubling motivation when ability is already high (or vice versa); the Behavior Model says it won't help much.
6. **"Pull future motivations into the present."** (p. 131) The future self is alien to the present self; commitment contracts (Ulysses contracts), age-progressed photos (Hershfield et al. 2011), and reward substitution (Ariely's medication-plus-movie story) bridge the gap.
7. **"A behaviorally effective product must first be a good product."** (p. 295) If the product is ugly, slow, or unusable, no amount of behavioral design rescues it. The intuitive Reaction gate vetoes it before behavioral magic gets a chance.
8. **"We should assume we're (partially) wrong."** (p. 296) Humility in the face of human complexity is what mandates the Refine stage and a culture of A/B testing.

## Cross-references

**vs. Kahneman, *Thinking, Fast and Slow*.** Wendel adopts System 1 / System 2 wholesale (Ch. 1), but where Kahneman explains *why* the mind works this way, Wendel asks *what to do about it in a product*. The CREATE funnel's Reaction gate is Kahneman's System 1 made operational — a place in the design process where you ask, "what will users intuit before they think?" Use Kahneman for the cognitive substrate; use Wendel to design around it.

**vs. Thaler & Sunstein, *Nudge*.** Both are prescriptive, but at different scales. Thaler & Sunstein design **policy environments** (cafeterias, organ-donor laws, Medicare Part D); Wendel designs **individual products** (HelloWallet, FuelBand, asthma trackers). NUDGES is a six-principle checklist for choice architects; CREATE is a five-gate diagnostic funnel for product teams. Where they overlap (defaults, feedback, expecting error), Wendel is more aggressive — he calls defaults "cheating" and ranks them first among strategies (p. 49), while Thaler & Sunstein treat them as one nudge among six. Sludge has no Wendel equivalent because Wendel's products are by-design, not adversarial.

**vs. Ariely, *Predictably Irrational*.** Ariely is the experiment storyteller; Wendel is the workflow architect. Ariely shows you that anchoring exists (the social-security-number wine-bidding study); Wendel tells you that "leverage loss aversion" goes in the Evaluation gate of your funnel diagnosis (Table 10-1). Ariely's "reward substitution" idea (the medication-plus-movie story) is cited approvingly by Wendel (p. 131) as a way to pull future motivation into the present. Ariely is the inspiration; Wendel is the engineering manual.

## Skill-by-skill pull list

### choice-architect (PRIMARY USER)

1. The full **CREATE Action Funnel** (Ch. 2) as the working diagnostic — every flow audit walks each screen through the five gates.
2. The **three strategies — Cheat / Habits / Support Conscious Action** (Ch. 3) as the strategy-selection move *before* picking tactics.
3. The **Behavioral Plan** workflow (Ch. 6) as the central artifact for any choice-architecture engagement, mapping to Slalom's Stage 3.
4. The **Intervention Catalog Table 10-1** (pp. 176–177) — two dozen tactics tagged by funnel gate. This is a strict superset of Slalom's eight tactics.
5. The **Action-Funnel debugging method** (Ch. 13, pp. 242–246) for diagnosing leaky flows.
6. The **Minimum Viable Action** concept (p. 89) as the analog of MVP for behavioral pilots.
7. **"Cheat first"** as the heuristic over Thaler & Sunstein's six co-equal nudges.

### behavioral-economist (generalist)

1. The **four-stage Understand-Discover-Design-Refine process** (Preface, Conclusion) as the framing arc for any behavioral engagement.
2. The **Wendel/Fogg/Slalom translation table** (above) for moving between vocabularies.
3. **Outcome → Actor → Action** clarification (Ch. 4) as the first-meeting move.
4. The **Causal Map** (Ch. 13, pp. 239–241) for forecasting and confound control.
5. **"Behaviorally effective product must first be a good product"** (p. 295) as the humility check.

### behavioral-economist-social-cognition

1. The **Reaction gate** (p. 30) — trust, professionalism, social proof, authority — as the first 200ms of persuasion.
2. **Tactics for Reaction** (Table 10-1, pp. 176, 179): "make the site professional and beautiful," "deploy social proof," "display authority," "be authentic and personal."
3. **Curtis's Twitter-CTA experiment** (p. 178) as the canonical micro-copy / framing case.
4. **Loss aversion via the Asian Disease frame** (p. 175, citing Tversky & Kahneman 1981) for messaging design.
5. **Prime user-relevant associations** (p. 176) and **avoid temporal myopia** framing for far-future benefits (p. 131).

### organizational-behavior-specialist

1. The **three strategies** applied to org change: cheat (intelligent defaults in HRIS, auto-enrollment), build habits (standups, retros), or support conscious action (training programs).
2. **Ability gate's four dimensions** (action plan, resources, skills, self-efficacy; p. 34) as a manager's diagnostic for team blockers.
3. **Intrinsic vs. extrinsic motivation crowding** (p. 128, citing Deci et al. 1999) — the warning against over-incentivizing.
4. **Commitment contracts and Ulysses contracts** (p. 130) for goal-setting and accountability programs.
5. **Behavioral Personas applied per behavior** (p. 102) — the same employee can be motivated for one behavior and not another.

### pricing-strategist

1. **Anchoring in the Evaluation gate** (Table 10-1) and **avoid choice overload** (p. 177) for menu design.
2. **Loss aversion** (p. 175 Asian Disease example, p. 176 tactic) — frame surcharges as losses, discounts as gains-foregone.
3. **Pull future motivations into the present** (p. 131) for subscription-vs-perpetual pricing.
4. **Make a reward scarce** (p. 177) — scarcity in the Timing gate.
5. **Avoid direct payments** (p. 176) — paying for behavior often crowds out intrinsic motivation; relevant for loyalty programs and behavior-based discounts.

### research-methods

1. The **Discover phase** (Ch. 4–5) as a research-design template: outcome → constraints → action list → user research → personas → action selection.
2. **Five behavior-relevant persona questions** (p. 101) as an interview-guide skeleton distinct from generic UX research.
3. **Direct observation > self-report** (p. 31) — "watch their behavior, don't listen to their mouths."
4. **Causal map construction** (Ch. 13) as a research-synthesis tool — what paths to the outcome did we miss?
5. **Trade-off elicitation** for measuring relative motivation (p. 128) instead of Likert-scale "how important is X?".

### statistician

1. **Random-assignment "roulette wheel"** procedure (Ch. 12, p. 218) for in-product A/B tests.
2. **Pre-pick your stopping rule** (p. 219, citing List et al. 2010) — optional-stopping bias is the most common error in startup A/B testing.
3. **Multivariate testing is just an experimental design** (p. 217) — use off-the-shelf tools (Optimizely, Maxymiser) rather than reinventing.
4. **Staggered rollout** (p. 219) for ethically-mandated full coverage.
5. **Pre-post analysis with informal then formal confound control** (p. 222) — informal first to gauge whether formal econometrics is worth it.
6. **Data Bridge** (p. 225) — the statistical model linking in-product action to out-of-product outcome.

## Templates worth adding to our plugin

Two new templates and one extension are worth building based on this book:

### New: `template-behavioral-plan.md` (PRIMARY recommendation)

Wendel's Behavioral Plan (Ch. 6) is the most distinctive workflow output in the book and **has no current Slalom-template equivalent**. Slalom's Choice Architecture template captures the *structure* of a flow; the Behavioral Plan captures the *narrative* of a user moving from inaction to action *across* the product/non-product boundary. Recommended structure:

- **Real-world step list** (numbered, including out-of-product actions)
- **Per-step labels**: in-product / product-response / real-world
- **Persona walk-through** (one column per persona, sticky-note-style annotations on motivation, friction, obstacles)
- **Cuts taken to reach Minimum Viable Action**
- **Strategy choice per step**: Cheat / Habit / Support Conscious Action
- **Funnel-gate review per step**: which CREATE gate is most likely to fail here?

This becomes the bridge between Slalom's Behavioral Profile (Stage 2) and Choice Architecture (Stage 3), filling the gap where the team currently jumps from "who is this user" to "what's the friction in their flow" without an intermediate narrative.

### New: `template-action-funnel-diagnosis.md`

A diagnostic worksheet for an existing flow that's leaking. For each screen in the flow, ask:

- **Cue**: is the next action obvious? Is it the *only* obvious action?
- **Reaction**: would a first-time user trust this screen in 200ms? Why or why not?
- **Evaluation**: what does the user perceive as the cost? The benefit? Relative to what alternative?
- **Ability**: does the user know the action plan? Have the resources? The skills? Believe they can succeed?
- **Timing**: why now? What makes this screen urgent vs. closeable?

Output: ranked list of leak hypotheses, each tagged to a CREATE gate, each with a candidate tactic from the (extended) intervention catalog. Maps to Slalom's Use-Case-Validation stage as the "first thing to do" before A/B testing.

### Extension: `template-intervention-design.md` extended to 26 tactics

Slalom's eight tactics (Goal Setting, Cognitive Restructuring, Feedback Loops, Problem Solving, Self-Monitoring, Stimulus Control, Decision Making, Reinforcement) are a useful curated list but are not tagged to a *gate of failure*. Recommend extending the existing template with Wendel's Table 10-1 (pp. 176–177), keeping the Slalom eight as the "primary" tactics and adding the remaining ~18 as "secondary" tactics, with each tactic tagged to one or more CREATE gates. This lets the consultant move from "this gate is leaking" → "here are the tactics targeted at that gate" rather than "here are eight tactics, pick one."

## Pull-quotes worth keeping

- "There are no martyrs in beneficial behavior change." (p. 56)
- "Watch their behavior, and don't listen to their mouths." (p. 31)
- "Most of the time, we're not actually 'choosing' what to do next." (Ch. 1 section header, p. 6)
- "A behaviorally effective product must first be a good product." (p. 295)
- "We should assume we're (partially) wrong." (p. 296)
- "When the product cheats, 'success' occurs when the user agrees to the action occurring, but doesn't pass through the funnel to stop it from occurring." (p. 57)
