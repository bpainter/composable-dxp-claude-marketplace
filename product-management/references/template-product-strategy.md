---
type: reference
project: skills-library
scope: plugin
plugin: product-management
tags: [type/reference, plugin/product-management, scope/template, topic/strategy, source/perri]
status: active
---

# Template — Product Strategy Cascade

**Source:** Perri, *Escaping the Build Trap* (2018), Ch. 8–11. See [`book-escaping-the-build-trap.md`](book-escaping-the-build-trap.md).

A four-level strategy cascade that ties day-to-day product work to the company's long-term direction. Use it when starting a new product engagement, when a roadmap is feature-shaped instead of outcome-shaped, or when the team can't articulate why they're working on what they're working on.

## When to use it

- Defining or refreshing product strategy
- Quarterly planning
- Onboarding a new product team
- Diagnosing whether the team is in the build trap
- Translating leadership-speak ("be the leader in X") into product-team-actionable bets

## The four levels

| Level | Time horizon | Cadence | Owner | Output |
|---|---|---|---|---|
| **Vision** | 5–10 years | Annual review | CEO/Founder | The world the company is trying to create |
| **Strategic Intents** | 1–3 years | Quarterly review | CEO + leadership | 2–4 strategic gaps to close |
| **Product Initiatives** | 6–12 months | Monthly review | Product Lead / CPO | Outcome-shaped bets owned by product teams |
| **Options** | 1–6 weeks | Weekly review | Product Manager | Specific experiments, MVPs, features |

## Worksheet

```markdown
# Product Strategy Cascade — [Product / Company]

## Vision
[2–4 sentences describing the world this company creates if it succeeds. Aspirational but recognizable. Not a tagline.]

Examples:
- (Stripe-like) "Increase the GDP of the internet."
- (Tesla-like) "Accelerate the world's transition to sustainable energy."
- "Make [domain] something everyone can do well, not just experts."

## Strategic Intents (2–4 max)
For each Intent, name:
- The strategic gap (Bungay): the difference between current state and where the Vision requires us to be
- The outcome (measurable, time-bound): what shifts for the customer or business
- The priority (which is #1, #2, etc.)

### Intent 1: [name]
- Strategic gap: [the gap]
- Outcome: [measurable, time-bound]
- Priority: P1
- Owner: [exec name]

### Intent 2: [name]
...

## Product Initiatives (3–7 in flight)
Each Initiative is owned by a product team and has a 6–12 month outcome.

| # | Initiative | Tied to which Intent | Outcome (measurable, time-bound) | Owner | Status |
|---|---|---|---|---|---|
| 1 | [name] | Intent 1 | "[Customer/segment] [does measurable thing] by [date/threshold]" | [PM/Lead] | Active |
| 2 | | | | | |

### Outcome statement examples (good)
- "Existing customers complete first-time invoice creation within 5 minutes of signup, by end of Q3."
- "Free-tier accounts convert to paid at ≥4% within 30 days of signup, by end of fiscal year."
- "Self-service customers resolve their tickets without contacting support ≥70% of the time, by end of Q4."

### Outcome statement examples (bad — output in disguise)
- "Ship the new onboarding flow." (an output, not an outcome)
- "Increase engagement." (vanity metric — what behavior?)
- "Hit 99.9% uptime." (this is an SLA, not a product outcome)
- "Customers love the product." (unmeasurable)

## Options (per Initiative, refreshed weekly)

For Initiative #1: [name]

| Option | Type (Discovery / Delivery / Ops) | Hypothesis or Description | Status |
|---|---|---|---|
| Option A | Discovery | "We hypothesize [problem] is real for [segment]. Test via [method]." | In flight |
| Option B | Delivery | Ship [feature]. Expected lift: [outcome change]. | Backlog |
| Option C | Discovery | "We hypothesize [solution] creates value. Test via [MVP type]." | Done — pivoted |

Options are bets, not commitments. Each Option has a decision rule for when to continue, pivot, or kill.

## Pirate Metrics or HEART (one per Initiative)

For each Initiative, name the primary metric and how it'll be measured.

### Pirate Metrics (AARRR — Dave McClure)
- Acquisition
- Activation
- Retention
- Referral
- Revenue

### HEART (Google)
- Happiness
- Engagement
- Adoption
- Retention
- Task success

Pick the framework that fits; don't try to track all 5 of either at once. The North Star metric concept (one metric that matters most) usually maps to one of the above.

## Roadmap (Now / Next / Later)

Roadmap is outcome-shaped. Items are Initiatives or major Options, not feature dates.

### Now (in flight)
- [Initiative 1] — outcome status: [tracking / off-track / on-track]
- [Initiative 2]
- [Initiative 3]

### Next (next 1–2 quarters)
- [Initiative]
- [Initiative]

### Later (≥6 months)
- [Theme or Initiative under consideration]
- [Theme]

NOT in this roadmap: feature lists with dates, "we will ship X in Q4." If you can't write the roadmap as outcomes, the strategy cascade isn't done yet.
```

## Pitfalls (Perri Ch. 11, 14, 15)

- **Vision as tagline.** A Vision that fits on a coffee mug isn't actionable. Write the world you're trying to create.
- **Strategic Intents as buckets.** "Customer Experience" is not a strategic intent — it's a category. The intent is the specific outcome you're trying to drive *in* that category over the next 1–3 years.
- **Outcome statements without thresholds.** "Increase activation" is not an outcome — it's a direction. "≥45% of new signups complete first invoice in 5 min by Q4" is an outcome.
- **OKR theater.** Outputs reformatted as Os and KRs. The KRs are usually the tell — "ship N features" is an output, not a key result.
- **Mutually destructive outcomes.** Two Initiatives whose outcomes can't both be true. Surface the conflict; don't paper over it.
- **Vanity metrics.** Page views, downloads, "time on page." Tied to no business outcome. Replace with behavior-tied metrics.
- **Roadmap as feature backlog.** If your roadmap is feature-shaped, you don't have a strategy — you have a backlog with dates.
- **Annual lock-in.** Strategy cascades that can't be reviewed quarterly become legacy after 6 months. Build in cadence.

## Hand-off downstream

- **Discovery plan** — for each Option marked "Discovery," design the experiment (matching technique to risk type)
- **Delivery plan** — for each Option marked "Delivery," hand to `project-management-sprint-planning`
- **Measurement plan** — for each Initiative, instrument the outcome metric (`marketing-analytics-strategist` for GA4/event setup)
- **Ops plan** — for each Option marked "Ops," scope the change-management implications

## Skills that use this template

- `product-management-product-manager` (primary)
- `consulting-digital-strategist` (uses for client product strategy work)
- `project-management-project-manager` (consumes for delivery planning)
- `behavioral-economics-pricing-strategist` (for pricing-driven outcomes)
