---
type: reference
project: skills-library
scope: plugin
plugin: product-management
tags: [type/reference, plugin/product-management, scope/foundational, topic/product-management, topic/roadmaps, source/lombardo, source/mccarthy, source/ryan, source/connors]
status: active
---

# Product Roadmaps Relaunched — Lombardo, McCarthy, Ryan, Connors (O'Reilly, 2017)

**Source digest** for the product-management plugin (also referenced from the cx plugin). Cite as "(Lombardo et al., *PRR*, ch. X)".

## Thesis

The roadmap most product orgs maintain — a feature-by-feature spreadsheet stretched across dated columns — is a **lie pretending to be a plan**. It overcommits to features that won't survive contact with reality, sets external expectations that can't be met, and trains stakeholders to evaluate the team on shipping dates rather than customer outcomes.

The book's argument: **a roadmap is a strategic communications artifact**, not a delivery schedule. Its job is to communicate *what we believe will be true about our customers and the market over time, and what we plan to do about it* — calibrated to the audience's real need for certainty. The reality is that most "roadmap" requests are really requests for one of three things: a delivery commitment, a strategic narrative, or a permission slip. The roadmap should serve the second; the first belongs in a project plan; the third is a political problem.

The book provides the canonical *theme-driven roadmap* format: outcomes-first, broad timeframes, themes-not-features, with explicit confidence levels. This is the format Perri references in *Escaping the Build Trap* and Banfield et al. references in *Product Leadership*. PRR is the deep treatise on the format itself.

## Part I — Roadmap fundamentals (Chs. 1–3)

### Why old roadmaps fail (Ch. 1)

Symptoms of broken roadmaps:
- Feature lists with hard dates ("Q3 2026: Search v2").
- Tediously updated quarterly to match what wasn't shipped.
- Cause death marches when the dates become contracts.
- Build features that are out of date before they ship.
- Train sales to sell what's on the roadmap; produce backlash when the roadmap moves.

Root cause: the artifact is being used for the wrong job. Engineers need delivery plans; sales needs commitments to sell against; executives need strategic narratives. Forcing one artifact to serve all three produces something that serves none.

The book's fix: **separate the roadmap (strategic outcomes) from the release plan (delivery commitments)**. Both can exist; they're different artifacts for different audiences.

### Components of a roadmap (Ch. 2)

The book defines a canonical component set. Primary components are required; secondary are added based on stakeholder need.

**Primary components (every roadmap):**

| Component | What it does |
|---|---|
| **Product vision** | The "north star" — how a specific customer benefits when product is fully realized. Timeless, technology-independent. |
| **Business objectives** | Measurable goals tied to vision. Often expressed as OKRs. |
| **Themes** | Customer needs the team commits to address. *Not features.* |
| **Timeframes** | Broad — "Now / Next / Later" or quarter-precision. Not week-precision. |
| **Disclaimer** | Explicit: "This is a direction, not a commitment." |

**Secondary components (added when stakeholders need them):**

| Component | When to add |
|---|---|
| **Features and solutions** | Only for near-term themes; only when the team has high confidence. |
| **Confidence** | Tag each item: high / medium / low confidence. Powerful expectation-setter. |
| **Stage of development** | Discovery / Validation / Build / Launch — Bruce McCarthy's GA-style stages. |
| **Target customers** | Which segment each theme serves. |
| **Product areas** | Which area of the platform each theme touches. |

### Gathering inputs (Ch. 3)

The roadmap is the *output* of a synthesis process. Inputs come from:

- **Customer research** — interviews, surveys, behavioral analytics. Generates themes.
- **Stakeholder requests** — sales, support, exec. Filtered through "what problem is this trying to solve?"
- **Competitive intelligence** — for context, not for direct feature copying.
- **Technical and operational debt** — internal investments that earn slots.
- **Strategic bets** — leadership-driven, often non-customer-validated initially.

The book's discipline for stakeholder requests (the "feature request" trap):

```
1. What problem is this request trying to solve?
2. Does solving that need align with our objectives?
3. Is it more important than what's on the roadmap now?
```

Most "feature requests" stop at question 1. Forcing the conversation to all three drops the request rate by ~70% in the book's case studies and improves the quality of what survives.

## Part II — The strategic roadmap (Chs. 4–5)

### Establishing the why with vision and strategy (Ch. 4)

Vision-to-roadmap cascade:

```
Product Vision
    ↓
Business Objectives (often as OKRs)
    ↓
Themes (customer-need clusters)
    ↓
(Optional: Subthemes, then Features)
```

The cascade is the same one Banfield et al. describe in *Product Leadership*; PRR adds the formal artifact structure.

**Vision check:** "How will a specific customer benefit from your product when it is fully realized and ubiquitous?" If the answer references current technology or current competitors, the vision is too small.

**OKRs:**
- *Objectives* are qualitative directional goals.
- *Key results* are quantitative, measurable.
- The most common OKR mistake: KRs that are deliverables ("ship v2 by Q3") instead of outcomes ("increase activation from 32% to 50%"). PRR explicitly calls this out as the dominant failure mode of OKR adoption.

### Themes — the central concept (Ch. 5)

Spool: **"Themes are a promise to solve problems, not build features."**

A theme is a *customer-need cluster* the team commits to address. The format that works:

```
Theme: [Customer-need statement]
   Subtheme: [More specific need within the theme]
      Subtheme: [More specific need within the theme]
```

Worked example from the book (Wombat lawn product):

```
Theme: No-leak hose connections
   Subtheme: One-handed snap-fit
   Subtheme: Visual fit confirmation
   Subtheme: Compatibility with major brands
```

The subthemes might eventually become features. The theme is what the roadmap commits to.

**Job stories and user stories** as theme support:
- *User story:* "As a [persona] I want [feature] so that [outcome]." Comes from Agile.
- *Job story:* "When [situation], I want to [motivation], so I can [expected outcome]." Comes from JTBD (see CX plugin's [[cx-jobs-to-be-done]] skill).

The book explicitly endorses job stories over user stories for theme work because user stories smuggle solutions ("I want this feature"), while job stories surface the underlying motivation.

**Themes vs. features — the test:** Could two completely different solutions both serve this item? If yes, it's a theme. If only one solution makes sense, it's a feature in disguise.

## Part III — Prioritization and presentation (Chs. 6–9)

### Prioritizing — with science (Ch. 7)

The book's most-cited chapter. Five frameworks; they overlap and you should mix-and-match.

#### 1. Critical path

Some needs *must* be served for the product to function. Map the critical path first; everything else is candidate work.

#### 2. Kano model

Three categories of customer expectation:

| Category | Effect if present | Effect if absent |
|---|---|---|
| **Expected (dissatisfier)** | Neutral | Negative — won't buy |
| **Normal (satisfier)** | Positive — proportional to investment | Mild negative |
| **Exciting (delighter)** | Disproportionately positive | Neutral — they didn't expect it |

Use Kano when prioritizing among satisfier-and-above ideas. Expectations rise over time — yesterday's delighter is today's expected (intermittent wipers in 1970 vs. now).

#### 3. Desirability, feasibility, viability (IDEO origin)

Score each candidate on three axes:

- **Desirability** — do customers want it? (qualitative + behavioral signal)
- **Feasibility** — can we build it? (technical + operational)
- **Viability** — does it pay back? (economic, strategic)

Items must score acceptably on *all three*. Strong on one or two is not enough.

#### 4. ROI scorecard (McCarthy's preferred)

A weighted scorecard. Each candidate gets scored on multiple criteria — value to customer, value to business, effort, risk, strategic fit — with criteria weighted to taste. Output is a ranked list.

The book is honest about the limits: numerical scoring can produce false confidence. Use as a *conversation starter*, not a decision-maker.

**Simple version** for small teams:

```
Score = (Value × Confidence) / Effort
```

**More complex version** breaks Value into customer value + business value, breaks Effort into time + cost + risk.

#### 5. Cost of Delay (Reinertsen, also in Perri)

For time-sensitive items: how much does the value drop if we wait a quarter? A feature that competitors are about to ship has high CoD; a long-shelf-life feature has low CoD.

Use CoD when sequencing items already deemed worth building.

### MoSCoW — the communication tool (Ch. 7)

Not a prioritization method; a *communication* method for release criteria.

| Bucket | Meaning |
|---|---|
| **Must have** | Required for launch. Without it, no launch. |
| **Should have** | Important but not launch-blocking. |
| **Could have** | Nice-to-have if time permits. |
| **Won't have (this round)** | Explicitly out of scope. |

The "Won't have" bucket is the most useful and most under-used. Naming what's out of scope sets expectations more clearly than naming what's in.

### Achieving alignment and buy-in (Ch. 8)

Three audiences need different framings of the same roadmap:

| Audience | What they need | Format |
|---|---|---|
| **Engineering** | Confidence, problem clarity | Themes + acceptance criteria |
| **Sales** | What can I sell? | Stage-of-development columns; clear "what's GA, what's beta" |
| **Executives** | Strategic narrative | Vision → objectives → themes; defer detail |
| **Customers** | What's coming for me? | Themes + broad timeframes + explicit disclaimer |

The roadmap that works for engineering will not work for sales. The fix is *multiple views of the same underlying data*, not one artifact stretched to serve all audiences.

### Presenting the roadmap (Ch. 9)

Presentation discipline:

```
1. Identify your audience.
2. Clarify your goals for the presentation.
3. Determine what your audience cares about.
4. Assemble your components from the master.
5. Train your stakeholders to contribute (the long-term move).
```

The big idea: stakeholders who understand how the roadmap is built make better requests against it. The book's case studies repeatedly show that *teaching the roadmap process* to stakeholders changes the quality of inputs more than any framework choice.

### Keeping it fresh (Ch. 10)

Roadmaps drift. Cadence:

- **Monthly** — light refresh; update progress, swap items, adjust confidence.
- **Quarterly** — full review with leadership; major theme changes.
- **Annually** — vision and strategic objectives revisited.

Roadmaps that are updated less than monthly are decoration, not direction.

## Eight named frameworks worth pulling out

1. **Theme-driven roadmap structure** — Vision → Objectives → Themes → (Subthemes / Features) (Ch. 4–5).
2. **Spool's themes-as-promises** — "Themes are a promise to solve problems, not build features" (Ch. 5).
3. **Three-question stakeholder filter** — What problem? Aligned? More important than what's on now? (Ch. 3).
4. **Kano model** — expected / normal / exciting (Ch. 7).
5. **Desirability, feasibility, viability** triple (Ch. 7).
6. **ROI scorecard** (McCarthy) — quantitative prioritization conversation-starter (Ch. 7).
7. **MoSCoW for release criteria** — Must / Should / Could / Won't (Ch. 7).
8. **Audience-specific roadmap views** — engineering vs. sales vs. exec vs. customer (Ch. 8).

Bonus: **Disclaimer-as-component** — explicit "this is a direction, not a commitment" line on every roadmap; **stage-of-development column** for items already in flight.

## Cross-references

**vs. Perri, *Escaping the Build Trap*:** Perri introduces theme-driven roadmaps in passing (her "Now / Next / Later with hypothesis, metric, stage"); PRR is the deep treatment. Where Perri's roadmap discussion is one chapter, PRR is an entire book. Pair them: Perri for the operating model, PRR for the artifact craft.

**vs. Banfield et al., *Product Leadership*:** Banfield et al. names themes as the right roadmap shape but doesn't go deep on how to build them. PRR is the implementation guide for the leadership argument Banfield makes. See [[book-product-leadership]].

**vs. Reinertsen, *Principles of Product Development Flow*:** PRR cites Reinertsen on Cost of Delay (Ch. 7). Reinertsen provides the queueing economics; PRR provides the artifact that surfaces sequencing decisions to stakeholders.

**vs. CX plugin (this digest is shared):**
- **Themes are job-clusters.** PRR's themes map directly onto JTBD outputs. A well-defined theme answers a customer job. See the CX plugin's [[cx-jobs-to-be-done]] skill.
- **Stakeholder filter is the saying-no muscle.** The three-question filter is what CX synthesis equips the PM to use. Without research-grounded answers to "what problem is this trying to solve?", the question can't be answered.
- **Journey-map opportunities feed the theme list.** See the CX plugin's [[cx-journey-mapper]] skill — opportunities row should map to themes, not directly to features.

## How `product-management-product-manager` skill should use it

1. **Theme-driven roadmap structure** (Chs. 4–5) — the canonical roadmap shape. Use whenever a feature-list roadmap is encountered.
2. **Three-question stakeholder filter** (Ch. 3) — "What problem? Aligned? More important than what's on now?" Use to manage the inbound feature-request stream.
3. **Confidence / stage / target-customer columns** (Ch. 2) — secondary components added when stakeholders need them.
4. **Kano + desirability/feasibility/viability** (Ch. 7) — the prioritization frameworks worth memorizing.
5. **Audience-specific roadmap views** (Ch. 8) — when stakeholders argue about roadmap content, the issue is usually that they need different views.
6. **Disclaimer-as-component** (Ch. 2) — every roadmap artifact should explicitly say "this is direction, not commitment." Stops the contract-creep dynamic.

## How CX skills should reference it (cross-plugin)

### `cx-jobs-to-be-done`
- **Themes ARE jobs at the roadmap level.** The PRR theme structure is the bridge between JTBD analysis and the product roadmap. JTBD outputs become candidate themes; themes become the commitments on the roadmap.
- **Spool's themes-as-promises** (Ch. 5) — endorses JTBD framing of roadmap items.

### `cx-journey-mapper`
- **Journey opportunities → themes** (Ch. 5) — the opportunities column on a journey map maps to candidate themes on the roadmap, not directly to features. This is the closure point between journey work and product work.

### `cx-customer-research`
- **Research-as-input to gathering** (Ch. 3) — research is one of five input streams to the roadmap. Research that doesn't feed roadmap is research without an action attached.

### `cx-customer-feedback-synthesizer`
- **Stakeholder filter requires synthesizer outputs** (Ch. 3) — answering "what problem is this trying to solve?" requires the synthesizer's pattern work. Without it, the filter can't operate.

### `cx-personalization-strategist`
- **Confidence column** (Ch. 2) — personalization experiments produce confidence-update signal that should feed back into roadmap items.

## Pitfalls

### Roadmap-as-commitment failures
- Sales sells against the roadmap; team can't deliver on date; trust collapses.
- Engineering builds against the roadmap; date slips; team blamed for missed dates.
- Executives evaluate the team on roadmap delivery instead of outcome achievement.
- Customer success commits dates to renewing customers based on the roadmap.

Fix: explicit disclaimer; separate release plan for delivery commitments.

### Feature-list-disguised-as-themes failures
- "Theme: Implement search v2." That's a feature. Test: could two completely different solutions serve this? If no, it's a feature.
- Themes that are the team's solution language ("Theme: Migrate to microservices"). That's a project, not a customer-need theme.
- Themes named after technology rather than customer need ("Theme: AI-powered X").

### OKR theater
- KRs that are deliverables, not outcomes ("ship v2 by Q3").
- KRs that are leading indicators dressed as outcomes ("publish 5 blog posts").
- KRs that are vanity metrics ("total signups," "page views") rather than actionable.

### Stakeholder-management failures
- Accepting feature requests without applying the three-question filter.
- One roadmap stretched across all audiences; pleases none.
- Roadmap presentation that leads with feature lists instead of vision-to-themes narrative.
- No mechanism for stakeholders to learn how to contribute well.

### Cadence failures
- Roadmap updated only quarterly; misses signal that needed faster action.
- Roadmap presented as "set" once; never re-opened until the quarter ends.
- Roadmap reviewed by the team but never with stakeholders; alignment evaporates.

### Prioritization failures
- One framework treated as authoritative ("the ROI scorecard says do X"); decision-by-spreadsheet.
- Cost of Delay used without ever measuring actual delay cost.
- Kano applied without research; team's guess at what's a delighter is usually wrong.
- "MoSCoW everything is a Must-Have" — the must-have category becomes meaningless.
