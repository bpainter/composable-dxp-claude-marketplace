---
type: reference
project: skills-library
scope: plugin
plugin: innovation
tags: [type/reference, plugin/innovation, scope/foundational, topic/validation, source/furr-dyer]
status: active
---

# The Innovator's Method — Furr & Dyer (2014)

**Source digest** for the innovation plugin. Cite as "(Furr & Dyer, *Innovator's Method*, p. X)" inside skill bodies.

## Thesis

The Lean Startup methodology pioneered for startups is incomplete for established organizations. *The Innovator's Method* extends it into a four-stage validation sequence — **Insight → Problem → Solution → Business Model** — designed for corporate environments where capital, brand, and stakeholder politics matter. The book's central discipline: validate stages *in order*; teams that skip Problem (the most common failure) build solutions to imagined needs.

Built on study of Amazon, Apple, Google, Intuit, P&G, Salesforce, plus 1,000+ entrepreneurs.

## The four-stage validation sequence

| Stage | Question | Primary methods | Exit criteria |
|---|---|---|---|
| **1. Insight** | Is this a problem worth solving? | Trend analysis, JTBD signal, hot-problem evidence | Problem is real, frequent, painful |
| **2. Problem** | Do customers describe the problem the way we do? | Switch interviews, contextual inquiry | Problem statement validated by 10+ customers; circumstance mapped |
| **3. Solution** | Does our solution solve it better than alternatives, including doing nothing? | Pretotyping, MVCs, concept tests | Customers prefer our solution; engagement signal positive |
| **4. Business Model** | Does the value capture work? | WTP studies, pilot, unit economics | Pricing test passed; LTV:CAC > 3 (or category-appropriate) |

Most enterprise innovation programs jump from Stage 1 to Stage 3, skipping Problem. They fall in love with their solution and cherry-pick interviews to confirm it.

## Stage 1 — Insight

The hot-problem test: does this problem appear *frequent*, *painful*, and *currently unsolved*? Sources of insight:

- Customer frustration (anomalies, workarounds)
- Trend convergence (technology, demographic, regulatory shift)
- Adjacent-industry analog (a problem solved elsewhere; not yet here)
- Internal pain (where your own org is struggling, often a market in waiting)

Tools: trend analysis, JTBD signal scans, "innovation outlook" reports.

## Stage 2 — Problem (the most-skipped stage)

Validate that customers describe the problem the way you do. Methods:

- **Switch interviews** (Klement / Moesta) — last-time-they-switched stories
- **Contextual inquiry** — observation in real settings
- **Pain-paid scoring** — how much have customers spent or worked-around to address this?

The discipline: don't show the solution. Don't propose features. Stay in the customer's circumstance and language.

Exit criteria: 10+ customers describe the problem in similar terms, with similar circumstance triggers, in similar emotional weight.

## Stage 3 — Solution

Validate that your solution solves the problem *better than alternatives, including doing nothing*. Methods:

- **Pretotyping** — Pinocchio, Mechanical Turk, Fake Door, Provincial, Pretend-to-Own
- **Minimum Viable Concept (MVC)** — smallest representation of the full value proposition
- **Concept tests** — monadic, A/B, conjoint
- **Behavioral signal** — money, time, sign-ups; not survey "I would buy this"

The Innovator's Method's contribution to Lean: emphasize *the comparison* against doing nothing. Many MVPs validate that customers like the solution but never ask if they like it more than what they currently do.

## Stage 4 — Business Model

Validate that value capture works. Methods:

- **Willingness-to-pay studies** — van Westendorp, Gabor-Granger, monadic, conjoint
- **Pilot pricing** — real money in market
- **Unit-economics math** — CAC, LTV, payback, gross margin
- **Channel + cost validation** — does the path to customer / cost to deliver work at scale?

Most enterprise innovations skip this stage and price-after-launch — leading to Ramanujam & Tacke's four monetization failure modes (feature shocks, minivations, hidden gems, undeads).

## The corporate adaptations (where the book diverges from Lean Startup)

Furr & Dyer's contribution beyond Lean is corporate-specific:

- **Stakeholder management** — innovation work in corporates needs internal advocates and political cover; the book includes a chapter on "Get the Right Boss"
- **Corporate-specific kill discipline** — escape velocity vs. failure within a portfolio
- **Pivot vs. persevere in corporate context** — pivots have political cost; design accordingly
- **Talent flow** — bring people in/out of innovation work without burning bridges

## How this maps to the innovation plugin

| Skill | Use of this book |
|---|---|
| `innovation-method-validator` | Primary source. Runs the four-stage sequence |
| `innovation-discovery-coach` | Stage 2 (Problem) is JTBD discovery territory |
| `innovation-monetization-strategist` | Stage 4 (Business Model) WTP work |
| `innovation-portfolio-architect` | Gates align to stages 1→2, 2→3, 3→4 |
| `innovation-leadership-coach` | "Get the Right Boss" chapter informs sponsor work |

## Three patterns worth replicating

- **Run a "Stage 2 reset"** when a team is in love with a solution that hasn't been Problem-validated. Ban solution discussion for 2 weeks; do only switch interviews.
- **Pre-mortem at funding** — before funding a stage, name the conditions under which we'd kill the bet. Ratify in writing.
- **MVC before MVP** — pretotype the value proposition before building the feature-minimal product. Often invalidates the bet faster and cheaper.
