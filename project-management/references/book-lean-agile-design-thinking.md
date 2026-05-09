---
type: reference
project: skills-library
scope: plugin
plugin: project-management
tags: [type/reference, plugin/project-management, scope/foundational, topic/methodology, topic/agile, topic/lean-startup, topic/design-thinking, source/gothelf]
status: active
---

# Lean vs. Agile vs. Design Thinking — Jeff Gothelf (2017)

**Source digest** for the project-management plugin. Page numbers refer to the Sense & Respond Press paperback (~80 pages). Cite as "(Gothelf, *LvAvDT*, p. X)" inside skill bodies.

## Thesis

Gothelf opens with a client quote that frames the whole book: "Our tech teams are learning Agile. Our product teams are learning Lean, and our design teams are learning Design Thinking. Which one is right?" (p. 3). His answer: stop asking which one wins. The three methodologies answer **different questions** and were never in competition — Agile answers *how to deliver*, Lean Startup answers *whether to build*, Design Thinking answers *what's worth building*. Treating them as rival religions produces what he calls the Goodfellas painting: "one team going one way, one team going the other way, and the manager in the middle saying, 'Hey, what do you want from me?'" (p. 6). High-performing teams don't pick one. They pick the parts of each that fit the question in front of them and run a single, balanced cadence (pp. 24, 36).

The deeper argument: each methodology has been *productized* and stripped of its original intent. Agile got reduced to velocity and Jira; Lean Startup got reduced to MVP-as-Phase-I; Design Thinking got reduced to post-its and certificates (pp. 7–9, 17–18, 22–23). The book is a corrective.

## The three methodologies in plain language

### Design Thinking — "what should we build?"

Popularized by IDEO in the 1990s though older in design practice (p. 21). Asks: "Are we solving a real problem, for a real customer, in a meaningful way?" (p. 21). Teaches teams to take an empathetic look at customers, understand the core need, then through facilitated brainstorming generate solutions that are desirable (customer), feasible (technical), and viable (business) (p. 22). Common shapes: IDEO's **Inspiration → Ideation → Implementation**; Stanford d.school's **Empathize → Define → Ideate → Prototype → Test**; the British Design Council's **Double Diamond** (discover, define, develop, deliver).

The honest critique Gothelf makes: in practice, Design Thinking in large companies degenerates into discrete workshops whose outputs are "sketches, photos of Post-it note clusters, and whiteboard scribbles, discernible, if at all, only to the person who made them" (p. 22). It's intended as a *continuous* practice woven into delivery, not a one-time front-loaded ritual.

### Lean Startup — "should we build it?"

Founded in Eric Ries's book and Steve Blank's customer-development thesis (p. 16). Posits that every project is an experiment, and the question is not "Can we build it?" but "Should we build it?" — and if so, "Can we build a sustainable business model around it?" (p. 16). Mechanism: the **Build–Measure–Learn** loop. Run short cycles (sound familiar?), collect customer-behavior evidence, then **persevere, pivot, or kill** (p. 17).

The MVP — *Minimum Viable Product* — is not "Phase I" or "the smallest thing we can ship." Gothelf gives the original two-question definition (p. 18):

1. What's the most important thing we need to learn first on our project?
2. What's the least amount of work we need to do to learn that?

The corruption: with velocity-driven incentives, teams now use "MVP" to mean "fewest features we can get away with and still ship" (pp. 18–19). They don't actually learn from the release; they move to the next backlog item.

### Agile — "how do we build it?"

Born in 2001 from seventeen engineers at Snowbird, Utah, frustrated with waterfall failures (p. 11). Core philosophy from the Manifesto: "We value responding to change over following a plan" (p. 11). Built around uncertainty: software's end state can't be predicted, complexity can't be sized in advance, customer behavior can't be guaranteed. The remedy is short cycles + reflection: work, pause, evaluate, adjust direction (pp. 12–13). Manifestations: **Scrum, XP, Kanban**; **backlog, sprint, retrospective**; **DevOps** as the modern accelerant that makes shipping a non-event (pp. 13–14); **SAFe (Scaled Agile Framework)** as the consultant-driven attempt to apply Agile across hundreds of teams (pp. 15–16).

Gothelf's critique: Agile has been "hired" to drive velocity, not responsiveness (p. 7). It treats other disciplines as support staff for engineers (p. 16). And the Manifesto authors never addressed scale — at 100+ teams, local optimization beats organizational coherence (p. 15).

## The integration model

Gothelf doesn't draw a single canonical diagram, but the integration logic is explicit across the book (esp. pp. 21, 23–24):

> *Agile helps us deliver work in regular cadences. Lean Startup helps us determine what to focus on. How, then, do we determine whether we're actually working on something of value? Enter Design Thinking.* (p. 21)

Read as a decision tree:

| Uncertainty about… | Reach for… | Tools |
|---|---|---|
| **What problem is real / what would users actually value** | Design Thinking | empathy interviews, contextual inquiry, journey maps, double diamond |
| **Whether our proposed solution will work / whether the business case holds** | Lean Startup | MVP, A/B test, landing-page test, build-measure-learn |
| **How to ship the validated solution efficiently and adapt as we learn** | Agile | sprint, backlog, retro, kanban, DevOps |

The three are not phases. They are **three concurrent lenses** on the same balanced team's work, with weight shifting depending on which question is hottest in the current sprint (pp. 24, 32). The "what / should we / how" framing is the cleanest one-line summary of the book.

## High-performing-team characteristics

Gothelf's ten practices (pp. 24–34), regardless of which methodology dominates:

1. **Work in short cycles** — including process changes, run as "process experiments" over two sprints (p. 25).
2. **Hold regular retrospectives** — the engine of continuous improvement; bring in an outside facilitator if venting dominates (pp. 25–26).
3. **Put the customer at the center** — and define customer as *the person who uses the product*, not the product owner or "the business" (p. 27).
4. **Go and see (gemba)** — managers walk the floor, ask what's working, amplify patterns that yield outcomes (p. 28).
5. **Balance discovery with delivery by only testing high-risk hypotheses** — not every backlog item warrants research; rank by risk × value, then pick the cheapest discovery technique (spike, A/B test, contextual visit) for that risk type (pp. 29–30).
6. **Do less research, more often** — three participants weekly beats twelve quarterly; on-site, broadcast results immediately (pp. 30–31).
7. **Work and train as one balanced team** — designers, engineers, PMs as the atomic unit; no discipline-specific training tracks; this is how you avoid "AgileFall" (waterfall with Agile vocabulary) (pp. 31–32).
8. **Radical transparency** — open retros, public success metrics, default to open (p. 32).
9. **Review your incentive and performance-management structure** — teams optimize what they're measured on; if you reward velocity you'll get features, if you reward learning you'll get discovery (p. 33).
10. **Make product discovery a first-class citizen of the backlog** — visualized in Jira alongside delivery tasks, prioritized, tracked, owned. Otherwise Agile "wins" because Agile is the only work that gets visualized (pp. 33–34).

## Common confusions Gothelf clears up

- **Agile ≠ standups + Jira + velocity.** Agile is a *philosophy of responsiveness under uncertainty*. Standups, sprints, and Jira are implementations, not the thing itself (pp. 7, 14–15).
- **Lean Startup ≠ MVP, and MVP ≠ Phase I.** MVP is "the least amount of work to learn the most important thing first." Most enterprises use it to mean "stripped-down launch" — that's not it (pp. 18–19).
- **Design Thinking ≠ post-its on a wall, ≠ a one-time workshop.** It's a *continuous* customer-empathy practice. The post-it artifacts are residue, not the work (p. 22).
- **Scaled Agile (SAFe) ≠ Agile at scale.** SAFe is one consulting product addressing scale, and Gothelf is skeptical: it tends to reproduce the engineer-centric problem at larger scope (p. 16).
- **Innovation labs ≠ Lean Startup adoption.** Bean-bag-chair labs separated from production capability "rarely yield anything of value" because the production handoff has no enthusiasm or capacity (pp. 19–20).

## Named frameworks and practices worth pulling out

1. **Build–Measure–Learn loop (Lean Startup)** — Ries's experimentation cadence; outputs are *persevere, pivot, or kill* decisions (p. 17).
2. **Two-question MVP definition** — (1) most important thing to learn first; (2) least work to learn it (p. 18). Pull this verbatim whenever someone says "MVP" and means "Phase I."
3. **Double Diamond / Empathize–Define–Ideate–Prototype–Test (Design Thinking)** — the d.school five-stage shape and IDEO's Inspiration–Ideation–Implementation are interchangeable for most working purposes (p. 22).
4. **Agile Manifesto core value** — "We value responding to change over following a plan" (p. 11). Cite when teams confuse predictability with success.
5. **Risk × value triage for discovery work** — rank backlog items by risk (technical, design, market) × value; only the high–high quadrant gets discovery effort, matched to the *type* of risk (pp. 29–30).
6. **Process experiments** — run new methodology changes as time-boxed two-sprint experiments rather than top-down mandates (p. 25).
7. **Gemba walks** — managers regularly observe teams working, surface patterns, scale what works regardless of methodology label (p. 28).

## When to use which — decision rules

- **High uncertainty about *the problem*** (do users actually struggle with X? does this segment exist?) → **Design Thinking** lead. Tools: contextual inquiry, journey maps, jobs-to-be-done interviews (p. 22).
- **High uncertainty about *value or business model*** (will users pay? will this convert? does the unit economics work?) → **Lean Startup** lead. Tools: A/B test, landing-page test, smoke test, concierge MVP (pp. 17–18, 30).
- **High uncertainty about *delivery / technical feasibility / how to coordinate work*** → **Agile** lead. Tools: spike, sprint, kanban, retro (pp. 12–13, 29).
- **All three uncertainties present** (greenfield product, new segment, new tech stack) → balanced team running all three concurrently with discovery as a first-class backlog item (pp. 31–34).
- **Low uncertainty across the board** (regulated, well-understood domain, fixed-spec build) → none of these three is your primary tool. Don't pretend.

## Cross-references

**vs. Perri (*Escaping the Build Trap*):** Perri sharpens Gothelf's Lean Startup chapter into a full operating model — *outcomes over outputs*, the product-kata, the four-fits, the dual-track. Where Gothelf says "make discovery a first-class citizen" (p. 34), Perri provides the org structure to do it. Use Gothelf to *win the argument* that the three methodologies are complementary; use Perri to *redesign the org*.

**vs. Reinertsen (*Principles of Product Development Flow*):** Reinertsen is the deeper engineering substrate beneath Agile — queuing theory, batch size, WIP limits, cost of delay. Gothelf treats Agile as a philosophy; Reinertsen treats it as economics. When a team's Agile practice is failing for *flow* reasons (handoffs, batch size, WIP), reach for Reinertsen, not more Scrum coaching.

**vs. Block (*Flawless Consulting*):** Block is the change-management parallel for moving an org *to* a Lean/Agile/DT operating model. Gothelf's chapter on incentives and performance-management criteria (p. 33) is exactly the kind of resistance Block's contracting and discovery phases are built to surface.

**vs. *The Principles of Product Development Flow* (Reinertsen):** Read together — Reinertsen is *why short cycles and small batches work* (economics of variability and queues); Gothelf is *which short-cycle ritual to run when*.

## How `project-management-methodology-advisor` (NEW skill) should use it

This is the home reference for that skill. Concrete pull list:

1. **The decision tree** (pp. 21, 23–24) — What/Should-we/How → DT/Lean/Agile. This is the skill's single most-used framework and should appear in the system prompt.
2. **Two-question MVP definition** (p. 18) — paste verbatim when a stakeholder uses "MVP" loosely.
3. **Risk × value discovery triage** (pp. 29–30) — the rule for *when to do discovery at all*; matched-tool selection (spike vs. A/B vs. contextual visit).
4. **Process experiments** (p. 25) — recommend this any time a client says "we want to roll out Agile org-wide."
5. **Discovery-as-first-class-backlog-item** (pp. 33–34) — the practical lever; include the visualization rationale ("the work that gets visualized is the work that gets done").
6. **Common confusions / methodology-zealotry checklist** (pp. 7, 18, 22) — diagnostic questions for the methodology-advisor's intake.
7. **Balanced-team staffing rule** (pp. 31–32) — the atomic unit of planning is the cross-functional team, not the discipline.

## How `project-management-project-manager` should reference it

For engagement design, two or three frameworks carry most of the weight:

1. **What/Should-we/How decision tree** (pp. 21, 23–24) — frames discovery vs. delivery scope at engagement start.
2. **Risk × value triage** (pp. 29–30) — concrete planning move for each sprint: which backlog items warrant discovery this cycle, which don't.
3. **Discovery-as-first-class-backlog-item** (pp. 33–34) — defends learning work in status meetings against velocity pressure.

Optionally: **process experiments** (p. 25) for transformation-flavored engagements where the PM is rolling out a new ceremony.

## Pitfalls

- **Methodology zealotry** — the certificate-on-the-wall problem. Each discipline trains in its tribe's framework, then defends it in cross-functional meetings (pp. 3–6). Symptom: arguments about *which method* instead of *which question we're answering*.
- **Confusing the question** — using Agile to answer "what should we build?" produces feature-factory output with no value validation. Using Design Thinking to answer "how do we ship?" produces beautiful concepts that never deliver. Match the methodology to the uncertainty (pp. 23–24).
- **Frankenstein Scrum / AgileFall** — waterfall handoffs with Agile vocabulary; sprints that are really mini-waterfalls; standups that are status reporting (p. 31). Gothelf: "we've been actively moving away from it for years."
- **MVP-as-Phase-I** — shipping less, but not learning from what shipped, then moving to the next "MVP" (pp. 18–19). The release isn't an experiment, it's just a smaller waterfall.
- **Front-loaded one-shot Design Thinking** — teams fail to "arrive at an epiphany during their first brainstorming session" and abandon the practice for the rest of the project (p. 22). DT is continuous or it isn't DT.
- **Innovation-lab isolation** — bean-bag chairs without a transition path to production (pp. 19–20). Discovery without delivery handoff equals shelfware.
- **Velocity as the only metric** — incentivizes feature-factory behavior and starves discovery (p. 33). The fix is in performance-management criteria, not in more coaching.
- **Scaling Agile by mandate (e.g., SAFe rollout)** without addressing incentives, balanced teams, or discovery visualization (pp. 15–16, 33–34) — produces large-scale local optimization, not org-wide responsiveness.

## Pull-quotes worth keeping verbatim

- "The question every project seeks to answer is not, 'Can we build it?' It's, 'Should we build it?'" (p. 16)
- "Are we solving a real problem, for a real customer, in a meaningful way?" (p. 21)
- "Agile helps us deliver work in regular cadences. Lean Startup helps us determine what to focus on. … Design Thinking [tells us whether] we're actually working on something of value." (p. 21)
- "Your job is to pick and choose the specific elements from each practice that work well for your teams and the brand values you're trying to convey." (p. 24)
- "The work that gets visualized is the work that gets done." (p. 33)
- "At the end of the day, your customers don't care whether you practice Agile, Lean, or Design Thinking." (p. 35)
