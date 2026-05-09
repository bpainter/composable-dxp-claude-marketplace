---
type: reference
project: skills-library
scope: plugin
plugin: project-management
tags: [type/reference, plugin/project-management, scope/template, topic/methodology, source/gothelf]
status: active
---

# Template — Methodology Selector

**Source:** Gothelf, *Lean vs Agile vs Design Thinking* (~2017) — see [`book-lean-agile-design-thinking.md`](book-lean-agile-design-thinking.md).

A decision worksheet for picking the right methodology — Lean Startup, Agile, Design Thinking, or a sequenced combination — based on the *question* the team is currently trying to answer. Use it when a team is debating "should we go Agile?", when discovery is being managed as if it were delivery, or when methodology zealotry is masking an unresolved question.

## When to use it

- Before adopting or changing a team's methodology
- When discovery work is being forced into sprint cadence
- When the team is "doing Agile" but moving no metrics
- When leadership confuses Agile with standups + Jira
- During a project-to-product transition
- When scaling teams (one team's methodology rarely scales unchanged)

## The 3-question diagnostic

Each methodology answers a different question. Start by naming the question.

| Question | Answered by | Output |
|---|---|---|
| **What problem should we solve?** | Design Thinking | Reframed problem statement, validated user need |
| **Should we build it?** | Lean Startup | Validated learning, go/pivot/persevere decision |
| **How do we build it?** | Agile | Shipped increment, learning from production |

```markdown
# Methodology Diagnostic — [Team / Initiative]

## What's the question we're trying to answer right now?
- [ ] What problem should we solve? (Design Thinking)
- [ ] Should we build it? (Lean Startup)
- [ ] How do we build it? (Agile)
- [ ] More than one (sequence them — see integration model below)

## What methodology is the team currently using?
[Scrum, kanban, waterfall, design sprints, ad hoc, mix]

## Is there a mismatch?
[Yes/No — if yes, what's the gap?]
```

## The integration model

When the question is "all three at once" — common at engagement start — sequence them:

```
                    Design Thinking          Lean Startup           Agile
                    "What should we build?"  "Should we build it?"  "How do we build it?"
                            │                       │                    │
                            ▼                       ▼                    ▼
                    Problem framing         Hypothesis test         Delivery iteration
                            │                       │                    │
                            └────► Output ◄─────────┘                    │
                            problem statement,      validated learning   shipped, iterating
                            initial concept         go/pivot/persevere   product
                                                            │                    │
                                                            └─── feeds ──────────┘
```

A typical sequence for a new product Initiative:
1. **Design Thinking sprint** (1–2 weeks) — frame the problem, prototype solutions
2. **Lean experiment** (1–4 weeks) — test the riskiest assumption with the cheapest test
3. **Agile delivery** (ongoing) — ship validated solution, iterate
4. Loop — production data feeds new DT or Lean cycles

## Risk × discovery triage (Gothelf pp. 29–30)

Once you've decided that "should we build it?" needs answering, match the discovery technique to the **type of risk**:

| Risk type | Best discovery technique |
|---|---|
| Problem existence | Contextual visit, ethnographic interview, JTBD interview |
| Value | A/B test, smoke test (landing page), Wizard of Oz, Concierge MVP |
| Usability | Prototype testing, moderated usability test |
| Technical feasibility | Engineering spike, technical prototype |
| Business viability | Cost of delay analysis, unit economics modeling |

```markdown
# Risk × Discovery Plan

| Risk | Type | Discovery technique | Owner | Duration |
|---|---|---|---|---|
| | | | | |
```

## When to use which (decision rules)

| Situation | Methodology |
|---|---|
| "We don't know what users actually need" | Design Thinking |
| "We have a solution idea but high uncertainty about value" | Lean Startup |
| "We know what to build; we need to ship it" | Agile |
| "We're shipping but metrics don't move" | Lean Startup (validate) and/or DT (reframe problem) |
| "We don't know what to test" | Design Thinking (frame the right hypothesis first) |
| "Engineering is fast, product impact is flat" | Build trap — DT + Lean to find the right work |
| "We have a complex regulatory delivery" | Agile (with explicit governance) |
| "We're entering a new market" | DT first, then Lean, then Agile |
| "The team's been doing Scrum for 2 years and is bored / stuck" | Diagnose: is it actually Agile, or Scrum theater? |
| "We need to scale from 5 → 50 teams" | Reinertsen flow first, then methodology |

## Common confusions Gothelf clears up

- **Agile ≠ standups + Jira.** Standups + Jira are tools. Agile is principles: working software, customer collaboration, responding to change, individuals over process.
- **Lean Startup ≠ MVP.** MVP is one tool. Lean is the loop: hypothesis → minimum experiment → measured result → pivot/persevere.
- **Design Thinking ≠ post-its on a wall.** Sticky notes are a tool. DT is the discipline of empathic problem framing before solution generation.
- **MVP ≠ first release.** MVP is a learning experiment; "first release" is a delivery milestone. Conflating them produces undertested products.
- **Pivot ≠ failure.** Pivot is a structured change in approach based on evidence.

## High-performing-team characteristics

Independent of methodology, Gothelf names ten characteristics of high-performing product teams. If a team is missing 3+, methodology won't rescue them.

```markdown
# Team Characteristic Diagnostic

For each, score the team 1–5 (1 = absent, 5 = strong):

- [ ] Outcome-oriented (not output-oriented): __
- [ ] Cross-functional (PM, design, engineering, plus specialists): __
- [ ] Decision-making authority: __
- [ ] Continuously discovers AND delivers: __
- [ ] Measures what matters (not vanity metrics): __
- [ ] Takes calculated risks: __
- [ ] Reflects and adjusts regularly: __
- [ ] Deeply understands customers: __
- [ ] Uses data to validate (not to perform): __
- [ ] Operates in short cycles with fast feedback: __

Total: __ / 50

If total < 30 → no methodology will rescue this team. Address the characteristics first.
```

## Worksheet — pulling it together

```markdown
# Methodology Selector — [Team / Initiative]

## Diagnostic
- Question hot right now: __
- Current methodology: __
- Mismatch: yes / no

## Recommended methodology
- Primary: [DT / Lean / Agile / sequenced]
- Sequence (if multiple): __ → __ → __

## Discovery plan (if Lean or DT in scope)
| Risk | Type | Technique | Owner | Duration |
| | | | | |

## Delivery cadence (if Agile in scope)
- Sprint length: __
- Standup: yes/no, frequency
- Retro: yes/no, frequency
- Working agreement (1 page): __

## Anti-patterns to watch for
- [ ] Frankenstein Scrum (rituals without principles)
- [ ] DT as decoration (workshops that produce nothing usable)
- [ ] Lean as cost-cutting
- [ ] MVP as "first release"
- [ ] Methodology zealotry

## High-performing-team gap
Top 2 characteristics to invest in:
1. __
2. __
```

## Pitfalls

- **Methodology zealotry.** "We're an Agile shop" or "we don't do Lean here" — both prevent the right answer when the question changes.
- **Frankenstein Scrum.** Standups, sprint planning, retros — but no actual working software in the sprint. Diagnose and reform.
- **DT as theater.** Post-it workshops that produce no validated insight. The DT discipline is rigor in problem framing, not crafty supplies.
- **Lean as "smaller team / less budget."** Lean Startup is about learning fast. It's not a code word for cuts.
- **MVP as "first release."** Conflating learning experiments with launch milestones. Define what each MVP is testing.
- **Methodology answer to a strategy problem.** When the team is in the build trap, no methodology rescues them. Address strategy first (`product-management-product-manager`).
- **Methodology answer to a flow problem.** When sprints feel theoretical, the issue is usually flow, not methodology. Check `template-flow-metrics.md` first.
- **Methodology answer to a leadership problem.** When the team can't make decisions, no methodology compensates. Address leadership/authority first.

## Hand-off downstream

- **For DT-led work:** hand off to `project-management-facilitator` and `cx-customer-research`
- **For Lean-led work:** hand off to `product-management-product-manager` (discovery design) and `project-management-flow-engineer` (CD3 prioritization of experiments)
- **For Agile delivery:** hand off to `project-management-sprint-planning` and `project-management-flow-engineer`
- **For team-characteristic gaps:** hand off to `leadership-people-leader` (manager-level coaching) and `consulting-change-management-advisor` (org-level transition)

## Skills that use this template

- `project-management-methodology-advisor` (primary)
- `project-management-project-manager` (uses for engagement design)
- `consulting-management-consultant` (uses for client engagement methodology choice)
- `consulting-digital-strategist` (uses for digital-product engagement design)
- `product-management-product-manager` (consumes for discovery vs. delivery split)
