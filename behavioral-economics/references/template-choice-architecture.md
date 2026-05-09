---
type: reference
project: skills-library
scope: plugin
plugin: behavioral-economics
tags: [type/reference, plugin/behavioral-economics, scope/template, topic/choice-architecture, source/slalom, source/thaler]
status: active
---

# Template — Choice Architecture

**Source:** Slalom Behavioral Design Model, p. 30 (core elements) and pp. 31–36 (worked example: co-morbid patient flow). Synthesizes Thaler & Sunstein's NUDGES mnemonic from `book-nudge.md`. See `2026-05_Slalom-Behavioral-Design-Model.md` in `40_Library/Solution_Briefs/`.

A Choice Architecture template documents the current decision flow your customer faces, identifies friction and failure modes, and redesigns the flow using a small set of architectural levers. Use it in Stage 3 of the model, after the Behavioral Profile is complete.

## When to use it

- Stage 3 of any behavioral-design engagement
- Whenever a customer flow has known drop-off, abandonment, or wrong-choice problems
- When converting a "long, scary form" or "shelf full of options" into something a Human (not an Econ) can actually navigate
- Before A/B testing — the architecture work surfaces the candidates worth testing

## The five core elements (Slalom p. 30)

Each is a lever. Reach for the one that matches the symptom.

| Element | Definition | Reach for it when... |
|---|---|---|
| **Sensible Defaults** | Pre-set options that take effect if no active choice is made | Inertia is strong, "non-choosing" is common, the right answer is the same for most people |
| **Choice over Time** | Structuring choices to highlight long-term outcomes and mitigate short-sighted decisions | Present bias is dominating; future-self consequences are hard to feel today (e.g., retirement saving) |
| **Partitioning Options and Attributes** | Grouping options to influence which is chosen | The list is too long; subsetting/grouping changes which option becomes salient |
| **Avoiding Attribute Overload** | Balancing information against cognitive load by simplifying decisions and limiting attributes | Customers stall, abandon, or pick a default they don't want because the page is too dense |
| **Translating Attributes** | Making attributes more understandable and comparable | Numerical or jargon-heavy info doesn't map to consequence; e.g., MPG vs gallons-per-mile, "3 apples per glass of cider" |

## Extended toolkit (from `book-nudge.md` and `book-predictably-irrational.md`)

Beyond Slalom's five core elements, the full choice-architect toolkit includes:

- **Anchoring** — set a reference number (taxi tip 15/20/25 vs. 20/25/30, *Nudge* pp. 28–30; SSN bidding, *PI* pp. 26–29)
- **Decoy / asymmetric dominance** — add a dominated option to make the target look better (*Economist* subscription, *PI* pp. 1–6)
- **Framing** — "90% fat-free" vs. "10% fat"; "saved" vs. "die" (Asian disease, Kahneman pp. 435–436)
- **Social proof / descriptive norms** — Asch line experiments; "85% of similar customers chose…"
- **Salience** — make the relevant attribute vivid (Lake Shore Drive curve stripes, *Nudge* p. 33)
- **Reminders & prompts** — just-in-time cues (Gmail "did you forget your attachment?", *Nudge* p. 115)
- **Commitment devices** — Save More Tomorrow; Stickk.com; pre-commitment deadlines
- **Make it easy / channel factors** — Yale tetanus study (3% → 28% follow-through with map + planning)
- **Active / required / prompted choice** — DMV organ-donor prompt
- **Curation & collaborative filtering** — Netflix-style recommendations, paint wheel by hue
- **Smart Disclosure / RECAP** — make complex info machine-readable so third-party choice engines help
- **Expect error** — forgiving design, forcing functions (Don Norman)
- **Give feedback** — pink-to-white ceiling paint; Tesla range-aware GPS

For deep coverage and examples, see `book-nudge.md` and `book-predictably-irrational.md`.

## Template (copy this into your deliverable)

```markdown
# Choice Architecture — [Flow Name]

## Customer segment in scope
Reference: [link to Behavioral Profile]

## Current flow (state)
Map every decision point the customer faces. Use a simple flowchart or numbered list:

1. [Trigger: what brings the customer here]
2. [First decision point — what options are presented]
3. [Action — what they do]
4. [Outcome — what they get]

For each step, capture:
- **Options presented:** how many, in what order, with what attributes
- **Default:** what happens if they take no action
- **Information surfaced:** what's visible, what's hidden
- **Friction:** time, cognitive load, required inputs
- **Failure mode:** how customers drop off, choose wrong, or get stuck

## Friction points and failure modes
- [Step N]: [what's broken and why — abandonment, wrong choice, frustration]
- [Step N]: ...

## Levers to apply
For each problematic step, choose one or more levers from the toolkit:

| Step | Symptom | Lever | Specific design |
|---|---|---|---|
| Step N | High abandonment at form fill | Avoiding Attribute Overload | Reduce 12 fields → 5 essential, defer rest |
| Step N | Customers picking wrong tier | Partitioning + Decoy | Group by use case; add a dominated tier above the target |
| Step N | "Default = none" produces 50% non-enrollment | Sensible Defaults | Switch to opt-out with personalized default |
| Step N | Numbers don't map to consequence | Translating Attributes | "$X per month" → "$Y per year" → "≈ Z weeks of groceries" |

## Future flow (state)
[Redrawn flow with the levers applied. Annotate each changed step with the lever name.]

## Hypotheses to validate (feed Stage 4)
1. By [intervention], we can [behavioral change] for [segment], leading to [outcome].
2. ...

## Ethics check (Stage 5 preview)
- Freedom to choose preserved? Yes/No, with note
- Customer welfare improved? Yes/No, with note
- Transparency about the design? Yes/No, with note
- No deceptive practices? Yes/No, with note
- Equitable across segments? Yes/No, with note
```

## Worked example — Co-morbid patient enrollment (Slalom pp. 31–36)

**Current state (p. 31).** A patient visits the care management portal and faces:
- 6 care management options
- 4 benefit program options
- A long enrollment form requiring "additional info"
- A separate "seek assistant" path with frustrating wait times and misinformation
- High dropout: customers either complete enrollment frustrated, or abandon and disengage entirely

**Friction inventory:**
- Too many options (6 × 4 = 24 combinations)
- No personalization
- Long forms requiring redundant input
- Wait times in the support path
- Misinformation when the support path delivers an answer
- "Provides additional info" step is redundant given what the system already knows

**Levers applied (p. 36):**
- **Sensible defaults** at the entry point — pre-select the personalized care management option
- **Personalization** at the benefit program selection — AI-enabled "Personalized Benefit Program Option"
- **Sequential choice** rather than presenting all options at once
- **Social proof** — "Customers like you chose X"
- **Option bundling** — group care management + benefit program into pre-validated combos
- **Attribute highlighting** — surface the 2–3 attributes that matter most for this segment
- **Simplification** — eliminate the "provides additional info" step
- **Loss aversion** + **incentivization** at completion — surface the savings/benefits being realized
- **Self-service** path replaces the "seeks assistant → contacts support" loop

**Future state outcomes (anticipated):**
- Reduced cognitive load (3 personalized options instead of 24 combinations)
- Higher completion (default nudges toward an active engagement state)
- Lower support load (self-service AI replaces phone tree)
- Customer feels recognized rather than processed

## Pitfalls

- **Adding levers without naming the symptom.** Defaults are not a universal solvent. If the symptom is "complex info doesn't map to consequence," the answer is *Translating Attributes*, not *Defaults*.
- **Stacking too many levers on one step.** Each step should have at most 2–3 active levers. More creates a frankenstein design with unpredictable interactions.
- **Defaulting to "the right answer for most" when the right answer varies a lot.** When right answers are heterogeneous, **personalized defaults** (Nudge p. 108) or **active/required choice** (Nudge p. 110) beat a one-size default.
- **Forgetting reactance.** Overly aggressive defaults can backfire — Haggag taxi-tip study showed higher anchors increased average tip *and* the share of zero-tip riders (`book-nudge.md`).
- **Skipping the ethics check.** A choice architecture that improves business metrics by exploiting cognitive limits is sludge. Run the ethics gate (`template-use-case-validation.md`'s sibling at Stage 5) before shipping.
- **Architecture that breaks at scale.** A flow that works for 100 prototypes may collapse at 100,000 customers. Stage 4 validation includes load and fairness testing.

## Hand-off downstream

- **Intervention Design template** (`template-intervention-design.md`) — for each lever applied, identify the specific behavior-change tactic
- **Use-Case Scoring** (`template-use-case-scoring.md`) — score each redesigned flow on Customer Value × Business Value
- **Use-Case Validation** (`template-use-case-validation.md`) — turn each hypothesis into a test
- **Ethics gate** (in `template-use-case-validation.md` and book-nudge.md p. 273) — five questions before scale

## Skills that use this template

- `behavioral-economics-choice-architect` (primary user — this is their core deliverable)
- `behavioral-economics-pricing-strategist` (uses for pricing-page redesigns specifically)
- `behavioral-economics-behavioral-economist` (uses for cross-domain interventions)
- `behavioral-economics-organizational-behavior-specialist` (adapts for HR/benefits flows)
