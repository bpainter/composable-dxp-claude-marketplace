---
type: reference
project: skills-library
scope: plugin
plugin: product-management
tags: [type/reference, plugin/product-management, scope/foundational, topic/product-management, topic/strategy, source/perri]
status: active
---

# Escaping the Build Trap — Melissa Perri (O'Reilly, 2018)

**Source digest** for the product-management plugin. Page numbers refer to the 249-page O'Reilly first edition. Cite as "(Perri, *EBT*, p. X)".

## Thesis

The **build trap** is the organizational pattern of measuring success by *outputs* (features shipped, releases, story points) instead of *outcomes* (problems solved, value created) (pp. 16, 28). Companies fall in when they confuse "shipping software" with "creating value" — sales contracts or roadmap commitments dictate work, the team becomes a feature factory, revenue plateaus (pp. 22–23, 190).

Perri's argument: escape requires becoming a **product-led organization** by re-architecting four layers (pp. 32, 211): **roles** (a real PM career path), **strategy** (four-level deployment), **process** (the Product Kata), and **organization** (outcome reviews, VC-style budgeting, safety to fail). The book runs a recurring case study — fictional Marquetly — so each framework arrives with a concrete example.

## The four roles in a product org (Ch. 8, pp. 64–73)

Perri rejects "product owner" as a career stage: PO is a Scrum responsibility. Everyone in the discipline should be called a product manager so the ladder stays coherent (p. 67).

### Product Manager
- **Scope:** Feature set; quarterly horizon. Owns the *why*; the team owns the *what* (pp. 56, 66).
- **Mix:** Operational/tactical with strategic edges.
- **Anti-pattern:** **Mini-CEO** — arrogant, dictates solutions (pp. 45–47). **Waiter** — order-taker landing in Bland's *product death cycle* (pp. 47–49). **Former Project Manager** — answers *when* not *why* (pp. 49–50).

### Product Owner (title)
- Don't use as a separate role. PO-as-junior-PM strands people in tactical work and starves them of strategy experience (pp. 60–62, 67).

### Product Lead (Director / VP of Product)
- **Director of Product** — first level of people management; oversees PMs on one product line; year-out roadmap (pp. 70–71).
- **VP of Product** — owns strategy and ops for a product line; sets product vision; enterprise ties P&L. Failure mode: VPs who skew tactical and never grow strategic muscle (pp. 71–72).
- **Anti-pattern:** Director acting like a senior PM, writing stories instead of coaching.

### Chief Product Officer (CPO)
- **Scope:** Entire portfolio; executive seat. Hire on second product, geographic expansion, or M&A (p. 72).
- **Job:** Drive economic success through portfolio growth; translate to board-level financial language.
- **Traits (Shelley Perry):** inspires confidence, empathizes, relentless (p. 73).
- **Anti-pattern:** Tactical VP promoted in name only; CPO who can't translate to the board.

## The strategy framework (Part III, pp. 85–129)

Bungay's definition: *"a deployable decision-making framework, enabling action to achieve desired outcomes, constrained by current capabilities, coherently aligned to the existing context"* (p. 88). Strategy is not a plan — it's a framework for decisions at every level.

### Bungay's three strategic gaps (Ch. 11, pp. 89–95)

| Gap | What it is | Wrong reflex | Right move |
|---|---|---|---|
| **Knowledge** | What mgmt wants to know vs. what the company knows | Demand more detail | Communicate strategic intent; ask only enough to decide |
| **Alignment** | What people do vs. what mgmt wants | Push down detailed instructions | Each level defines *how* to deliver next level's *intent* |
| **Effects** | Expected vs. actual results | Add controls | Give teams freedom to adjust toward goals |

### The four levels of strategy deployment (Ch. 12, p. 103)

| Level | Definition | Horizon | Cadence | Owner |
|---|---|---|---|---|
| **Vision** | Why the company exists, where it's going | 5–10+ yrs | Rarely changed | CEO |
| **Strategic Intent** | Few business outcomes that move toward vision now ("expand into enterprise"); 1 small co, ≤3 large (p. 113) | 1–several yrs | QBR | C-Suite + CPO |
| **Product Initiative** | Customer problem solved to deliver an intent (Netflix: "watch anywhere") (p. 117) | 6–12 mo | Initiative Review (quarterly, off-cycle) | CPO / VP |
| **Options** | Bets — solutions teams explore (Spotify "DIBBs"; pp. 99, 119) | Wks–mo | Release Review (monthly) | PM + team |

Variants: **OKRs** (Google), **Hoshin Kanri** (Toyota), **Mission Command** (military). Same pattern: set direction at each level so the next can act (p. 102).

## The Product Kata (Ch. 15, pp. 130–133)

Perri's signature framework, adapted from Mike Rother's *Toyota Kata*. The discipline's engine — repeated until habit.

**The six questions** (after the team has a goal):
1. **What is the goal?** (option-level metric)
2. **Where are we now?** (current condition; baseline data)
3. **What is the biggest obstacle?**
4. **What is the next experiment?** (one experiment, one hypothesis)
5. **What do we expect to happen?** (explicit prediction)
6. **What actually happened, and what did we learn?**

Steps 1–4 plan the move; 5–6 close the loop and feed the next iteration. Phases: understand direction → problem exploration → solution exploration → solution optimization (p. 133).

**How to use it:**
- Same questions at every level: option, initiative, intent.
- "What do we need to learn next?" is the canonical reframe (p. 162).
- **Don't over-experiment.** When the problem is solved by best practices (e-commerce checkout), implement and measure — reserve experimentation for value-prop work (p. 133).
- Pair with success metrics. **Real Definition of Done** = outcome reached. Gothelf: *"Version 2 is the biggest lie in software development"* (p. 184).

## Outcomes vs. outputs

**Outputs** are quantified things produced — features, releases, velocity, story points (p. 28). **Outcomes** are the resulting changes in customer or business behavior.

**Outcome-statement template** (p. 142):
> *We believe that, by [solution direction], we can [outcome A] and [outcome B], resulting in [business impact].*

Marquetly example: *"by increasing content in key areas of interest, we can double acquisition and lift retention to 70%, resulting in $8M/year"* (p. 142).

**Metric pitfalls:**
- **Vanity metrics** (Lean Startup): totals that always grow. Convert to actionable by adding time and comparison (p. 145).
- **Output metrics disguised as success:** features shipped, story points. Productivity, not product (p. 146).
- **OKR theater:** Marquetly had OKRs but KRs were deliverables — *"Ship v1 by June 2018"* (p. 99). KRs must be outcomes.
- **Mutually destructive pairs:** every metric needs a counter-metric (acquisition + retention) so single-metric gaming surfaces (p. 153).
- **Leading vs. lagging:** retention is lagging; activation, happiness, engagement lead it. Option metrics lead initiative metrics.

Default scaffolds: **Pirate Metrics (AARRR)** — Acquisition, Activation, Retention, Referral, Revenue (McClure, p. 149). **HEART** — Happiness, Engagement, Adoption, Retention, Task success (Rodden, p. 151).

## The product discovery process (Part IV, pp. 130–189)

Four phases, run inside the Kata:

1. **Understand direction & set success metrics** (Ch. 16). Translate intent + initiative into a measurable team goal; pull baseline data; write the option statement.
2. **Problem exploration** (Ch. 17, pp. 144–155). Generative research — interviews, observation, behavior instrumentation. Identify the highest-leverage obstacle. *Fall in love with the problem, not the solution* (p. 162).
3. **Solution exploration** (Ch. 18, pp. 156–174). Build to learn, not earn. Three experiment types:
   - **Concierge** — manually deliver outcome to a few customers; doesn't scale by design (pp. 162–163).
   - **Wizard of Oz** — looks finished; manual on the backend (early Zappos) (pp. 163–165).
   - **Concept testing** — landing page / prototype / demo video (Dropbox investor video) with an "ask" (email, money, time) to convert generative to evaluative (pp. 165–168).
4. **Build & optimize** (Ch. 19). North Star + story mapping (Patton); prioritize by **Cost of Delay** (Reinertsen, p. 180; Arnold/Yuce qualitative grid, p. 181); Real Definition of Done = outcome reached (p. 184).

Perri retires **MVP** as corrupted to "first release" — her replacement: *minimum amount of effort to learn* (p. 159).

## Project orgs vs. product orgs (Ch. 3, pp. 30–31; Part V)

A **project** is discrete scope with a deadline; a **product** is a value vehicle that needs nurturing (p. 30). PRINCE2/PMBOK optimize delivering defined scope; product management optimizes ongoing value creation against a moving market.

| Dimension | Project org | Product org |
|---|---|---|
| Funding | Annual per-project budget; business case up front | VC-style stage-gated; fund by certainty (pp. 207–208) |
| Success metric | On time, on scope, on budget | Outcome reached |
| Roadmap | Gantt chart of dated features | Now/Next/Later w/ hypothesis, metric, stage (p. 197) |
| Team org | Around components/features | Around **value streams** (p. 76) |
| Definition of Done | Feature shipped | Outcome achieved (p. 184) |
| Reward | Bonuses for delivery | Bonuses for outcomes/learning (pp. 200–202) |
| Cadence | Annual planning | QBR + Initiative Review + monthly Release Review (pp. 192–194) |

**Transition failures:**
- **Peanut-buttering** strategy across too many initiatives (a 5,000-person co. had 80 intents; shipped 1 feature/quarter; p. 113).
- **Sales-led drift** — contracts dictate roadmap (pp. 32–33).
- **OKR theater** — adopted form, output-shaped KRs.
- **Innovation silo** — Kodak 2008: right insight, no funding pipeline (pp. 213–214).
- **Penalized for unspent budget** — yearly cycles punish teams that find a cheaper path (p. 207).
- **PO-as-junior-PM** — strands talent (p. 67).
- **Leadership delegating the change** — Marquetly's Chris succeeded because he changed himself first (p. 211).

## Eight named frameworks worth pulling out

1. **The Build Trap** (pp. 16, 28) — outputs measured instead of outcomes.
2. **Value Exchange System** (pp. 24–27) — value flows back only after the customer's problem is solved.
3. **Bungay's Strategic Gaps** (pp. 89–95) — Knowledge / Alignment / Effects.
4. **Strategy Deployment** (p. 103) — Vision → Strategic Intent → Product Initiative → Options.
5. **The Product Kata** (pp. 130–133) — six-question habit pattern.
6. **PM Career Path** (pp. 64–73) — Associate / PM / Sr. PM / Director / VP / CPO.
7. **Bad PM Archetypes** (pp. 45–50) — Mini-CEO, Waiter (Bland's death cycle), Former PM.
8. **Real Definition of Done + Cost of Delay** (pp. 180–184) — Arnold/Yuce urgency × value.

Bonus: **Roadmap stages Experiment / Alpha / Beta / GA** (p. 197); **North Star + Story Mapping** (Patton, pp. 175–176); **Press Release product vision** (Amazon, p. 119); **Product Operations team** (pp. 198–200).

## Cross-references

**vs. Reinertsen, *Principles of Product Development Flow*:** Perri cites Reinertsen on **Cost of Delay** (p. 180). Reinertsen contributes queueing/flow economics justifying small batches; Perri contributes the org and discovery layer that produces the right batches. Reinertsen = *how to structure the flow*, Perri = *what to put in it*. The Product Kata is the discovery engine; flow economics determine release sequencing.

**vs. Gothelf, *Lean UX* / *Sense & Respond*:** Gothelf is in the praise quotes (p. 1) and cited on Definition of Done (p. 184). Gothelf works at the team-bench level (hypothesis → experiment → MVP within one cycle); Perri works one level up (org architecture, strategy deployment, career path, budgeting). Lean UX = how a team works inside one Kata cycle; Perri = how the org produces valid Katas at scale.

**vs. Block, *Flawless Consulting* / Kubr, *Management Consulting*:** Internal vs. external lens, overlapping diagnostic muscles. Block's contracting and discovery map onto Perri's strategic-intent + initiative work (problem definition before solution scope). Kubr's diagnosis and intervention chapters parallel Perri's problem exploration and concierge experimentation. When the engagement is *redesigning a client's product org*, Perri is the deliverable playbook; Block/Kubr are the engagement playbooks. The Marquetly arc is itself a consulting engagement told from the consultant's chair.

## How `product-management-product-manager` skill should use it

1. **Bad PM archetypes** (pp. 45–50) — vocabulary for diagnosing dysfunction.
2. **Outcome-statement template** (p. 142) — *"We believe that, by X, we can A and B, resulting in Z."* Canonical hypothesis format.
3. **Product Kata's six questions** (pp. 131–132) — running checklist every cycle.
4. **Bungay's three strategic gaps** (pp. 89–95) — diagnostic for "why is leadership demanding more detail?"
5. **Vision → Strategic Intent → Product Initiative → Options** (p. 103) — ladder a request up to strategy, or strategy down to a team goal.
6. **Pirate Metrics + HEART as mutually destructive pairs** (pp. 149–153) — default metric scaffolds.
7. **Concierge / Wizard of Oz / Concept Testing** (pp. 162–168) — the experimentation menu.
8. **Real Definition of Done** (p. 184) — done = success metric reached, not code merged.

## How existing project-management skills should reference it

### `project-management-project-manager` (3 frameworks)

1. **Projects vs. products** (pp. 30–31) — boundary statement: project = defined scope + deadline; product = ongoing value vehicle. Use when a "project" is product-shaped (no fixed scope, ongoing iteration).
2. **Real Definition of Done** (p. 184) — for software deliverables, define "done" as outcome-met, not feature-shipped. Negotiate acceptance up front.
3. **Cost of Delay prioritization** (pp. 180–182) — Arnold/Yuce's qualitative urgency × value grid. Replacement for "everything is P1."

### `project-management-requirements-discovery` (3 frameworks)

1. **Product Kata's question 3 — "What's the biggest obstacle?"** (p. 132) — reframes requirements gathering as obstacle identification, not feature collection. Stops accepting a stakeholder's solution as a requirement.
2. **Problem exploration before solution exploration** (Ch. 17, pp. 144–155) — qualify the problem with data before scoping a solution. Counters the *product death cycle* (Bland, p. 48).
3. **Generative vs. evaluative research distinction** (p. 167) — concept testing is generative until an "ask" is added; landing pages and prototypes without an ask widen understanding but don't validate. Sets realistic expectations for discovery artifacts.

## Pitfalls

### Build-trap symptoms (Part I, pp. 16–34)
- Success measured by features shipped, releases per quarter, story points.
- Sales-driven roadmap; product team plays catch-up to contracts.
- Scrum teams own components ("login API forever") instead of outcomes.
- Many parallel projects, one junior PM each, peanut-buttered strategy (p. 113).
- Bonuses tied to delivery dates, not problems solved.
- "We're going Agile" as the answer; PMs still operate Waterfall.

### Output-think disguised as outcome-think
- *"Ship platform v1 by June"* is an output dressed as an objective (p. 99); real KRs read *"Increase completion from 40% to 70%."*
- "Now / Next / Later" roadmap populated only with feature names is still output-shaped. Each item needs hypothesis + metric + stage (p. 197).
- "We're outcome-focused" while reviews still cover what shipped, not what changed.
- **MVP** treated as first release rather than *minimum effort to learn* (p. 159).
- "Version 2" as dumping ground for what wasn't measured on V1 (p. 184).

### OKR theater and reward misalignment (pp. 200–202)
- December scramble — "ship whatever satisfies the scorecard so bonuses pay out."
- Sales commissions on bookings only, not retention — wrong-fit customers, overpromised roadmaps.
- Reorganizing into a product org without changing bonuses — reverts within 1–2 quarters.

### Transition pitfalls (p. 211)
- Treating transition as everyone else's job — leaders must change first.
- Hiring a CPO without authority over budgeting cadence or org structure.
- Innovation lab siloed with no funding pipeline to the core (Kodak 2008, pp. 213–214).
- Annual budget cycle preserved while everything else moves to outcome-mode — the budget re-imposes project-think every January.
