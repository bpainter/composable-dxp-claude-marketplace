---
type: reference
project: skills-library
scope: skill
plugin: behavioral-economics
parent: "[[pricing-strategist]]"
tags: [type/reference, plugin/behavioral-economics, scope/skill, topic/behavioral-econ]
status: active
---
# Monetization Models & Price Testing

Practical reference for pricing-strategist skill. Covers SaaS pricing architectures, subscription models, freemium strategy, consulting rate structures, price testing methodologies, and competitive positioning.

---

## 1. SaaS Tier Architecture: Good-Better-Best

**The Standard Three-Tier Model**

Most SaaS products use a "good-better-best" structure:

| Tier | Seats | Features | Price | Target Segment |
|------|-------|----------|-------|----------------|
| **Starter** | 1–3 | Core features | $29/mo | Solopreneurs, trials |
| **Professional** | 5–20 | All features + API | $99/mo | Small teams, serious users |
| **Enterprise** | Unlimited | Custom + support | $500+/mo | Large orgs, mission-critical |

**Design Principles**

1. **Feature bundling follows value escalation**: Each tier has clear additional value
2. **Pricing gap logic**: Starter to Pro is 3–4x; Pro to Enterprise is 5–10x
3. **Target segments non-overlapping**: Minimize cannibalization between tiers
4. **Upgrade path clear**: Users see natural progression (not forced to enterprise)

**Feature Differentiation Tactics**

| Feature | Starter | Pro | Enterprise |
|---------|---------|-----|-----------|
| **Users/seats** | 3 | Unlimited | Unlimited |
| **API calls/month** | 10K | 1M | 10M |
| **Custom reports** | No | Yes | Yes + scheduled |
| **Priority support** | Email | Chat | Dedicated account manager |
| **SLA** | None | 99.9% | 99.99% |
| **Customization** | No | Limited | Full |
| **Integrations** | Pre-built | Pre-built + Zapier | Pre-built + custom |

**Pricing Gap Sizing**

- **Too narrow ($29 → $49 → $99)**: Low tier captures demand; tiers feel arbitrary
- **Optimal (3–5x jumps)**: Each tier targets distinct segment; revenue maximized
- **Too wide ($29 → $299)**: Gap feels arbitrary; may be missing mid-market revenue

**Tactics**

- **Clarify upgrade triggers**: "When your team grows to 5+ people, upgrade to Pro"
- **Segment on usage, not just features**: Seats, API calls, data storage, report frequency
- **Use annual plans to shift upgrade curve**: "$29/mo" vs. "$240/year (25% discount)" at Pro tier
- **Clearly gate high-value features**: Analytics, API, integrations, support go to Pro+
- **Make enterprise pricing custom**: Avoid discounting listed tiers; use negotiation for deals

**Example: Slack Tier Structure**

| Tier | Message search | Integration | File storage | Price |
|------|-----------------|-------------|--------------|-------|
| **Free** | Limited (90 days) | Limited | 5GB | Free |
| **Pro** | Unlimited | Pre-built | 1TB | $12.50/person/month |
| **Business+** | Unlimited | Pre-built + custom | Unlimited | $18.75/person/month |
| **Enterprise** | Unlimited | Custom | Custom | Negotiated |

Slack uses **depth of integration ecosystem** and **message search** (valuable for growing teams) to drive upgrades.

---

## 2. Usage-Based Pricing (Consumption Pricing)

**Core Model**

Price correlates with usage: customers pay for what they actually use. Examples: AWS ($0.10 per 1M API calls), Stripe ($0.029 per transaction).

**Advantages**
- Aligns cost with value delivered
- Reduces buyer friction (no sticker shock; start small)
- Scales revenue with customer success
- Natural upsell (usage grows → price grows)

**Disadvantages**
- Customer unpredictability (usage spikes → bill shock)
- Harder to forecast revenue
- Requires transparent cost tracking
- May incentivize customers to find alternatives if usage grows

**Usage Metrics Selection**

What scales with customer value delivery?

| Product | Metric | Rationale |
|---------|--------|-----------|
| **API/infrastructure** | Requests, data transfer, compute | Direct cost correlation |
| **Analytics** | Events tracked, data scanned, reports | Usage scales with value |
| **Email marketing** | Emails sent | Direct value delivery |
| **Transcription** | Minutes transcribed | Direct value delivery |
| **Data warehouse** | Queries, data stored, compute | Resources consumed |
| **CMS** | API calls, seats, workflows | User activity scales |

**Hybrid Model: Tiered + Usage-Based**

Combine fixed base tier + overage charges:

- **Base tier**: $99/mo for up to 1M API calls
- **Overage**: $0.05 per additional 1K calls

Benefits: Predictable baseline; scales for power users; captures upside without shocking customers.

**Tactics**

- **Choose transparent metric**: Customers should understand what drives charges
- **Set fair pricing**: Usage price should be < customer's value per unit
- **Bill in arrears**: Show usage clearly; let customers control spend
- **Set usage caps**: Warn customers before they hit hard limits
- **Offer annual commitment discounts**: "Reserve 5M calls for $500/year" (3% discount)

**Example: Stripe Pricing**

2.9% + $0.30 per transaction. Usage-based perfectly aligns with Stripe's value delivery (processing payment = revenue generated).

---

## 3. Freemium Strategy and Free-to-Paid Conversion

**Freemium Economics**

| Metric | Benchmark | Notes |
|--------|-----------|-------|
| **Free user volume** | 100x paid users | 1% conversion to paid is good |
| **Conversion rate** | 0.5–3% | Depends heavily on product fit |
| **Free-to-paid time** | 30–90 days | Longer engagement → higher conversion |
| **Free user costs** | Infrastructure + support | Server, bandwidth, onboarding support |

**Free Tier Design: Maximize Conversion, Minimize Costs**

| Approach | Effect | Trade-off |
|----------|--------|----------|
| **Shallow free tier** (limited features) | Low conversion; low cost | Users leave; miss engaged segment |
| **Deep free tier** (full features, limits) | High conversion; high cost | Server load; may cannibalize paid |
| **Degraded free tier** (slower, fewer seats) | Balanced conversion; low cost | Feels cheap; may drive churn |
| **Time-limited free trial** | High conversion during trial; loses engagement after | Resets expectations; may lose users |

**Best Practice: Free Tier with Usage Limits**

- Give users **full feature access** (drives endowment effect)
- Limit **usage** (not features): API calls, data storage, seats, reports
- Usage limit triggers upgrade need naturally
- Example: Figma free tier → unlimited projects but max 3 files (hits limit naturally)

**Conversion Optimization Levers**

| Lever | Tactic | Expected Impact |
|-------|--------|-----------------|
| **Engagement** | In-app walkthroughs, onboarding sequence | +20–30% |
| **Upgrade prompts** | Show paywall before hitting limit | +10–20% |
| **Pricing clarity** | Show pricing page in-app; explain tier benefits | +5–15% |
| **Social proof** | "10,000 teams on Pro plan" | +5–10% |
| **Risk reduction** | Free trial, money-back guarantee | +5–10% |

**Freemium Cannibalization Risk**

If free tier is *too* good, high-quality users stay on free. Mitigate:

- **Feature gating**: Core analytics, reporting, advanced workflows behind paywall
- **Scalability gating**: "Your project needs 10 files; upgrade to Pro" (not arbitrary)
- **Support gating**: Free = community support; paid = priority support
- **Team gating**: Free = 1 user; paid = team collaboration

**Tactics**

- **Gate based on natural usage limits** (file limits, data limits), not artificial limits
- **Extend free trial to build endowment**: 14–30 days vs. 7 days improves conversion
- **Use smart upgrade prompts**: Trigger at moment of value (user hits limitation)
- **Communicate clearly**: "Upgrade when your team grows to 5+" vs. hard paywall
- **Offer annual plan at pro tier discount**: Drive LTV and reduce churn

**Example: Figma Freemium Model**

- **Free**: Unlimited projects, max 3 files, view-only sharing
- **Professional**: Unlimited files, full collaboration, prototyping, $12/mo
- **Organization**: Team admin, billing controls, $60/month per editor

Figma gates on **file count** (naturally hits when project grows) and **collaboration features** (high value for teams).

---

## 4. Consulting Rate Structures

**T&M (Time & Materials)**

Fixed hourly rate, bill for hours spent.

- **Rate**: $150–400/hour depending on experience, discipline, market
- **Pros**: Simple; predictable to consultant
- **Cons**: Incentivizes inefficiency (more hours = more revenue); clients resist risk
- **Best for**: Uncertain scope; augmentation (adding capacity); training

**Risks to Consultant**: Client pushes for discount if project runs short; client unhappy if scope balloons

**Tactics**
- Set clear hourly rate (no per-project discounts)
- Bill weekly, in arrears (visible to client)
- Set scope boundaries upfront (5–10 hours budgeted for phase)

**Fixed-Fee (Project-Based)**

Single price for defined deliverable.

- **Rate**: Estimate hours × 1.2–1.5x (buffer for unknowns) × hourly rate
- **Pros**: Consultant absorbs risk; client knows total cost; forces clear scoping
- **Cons**: Consultant bears overrun risk; incentivizes cutting corners
- **Best for**: Defined scope; repeatable projects; client wants certainty

**Example**: Audit project = 40 hours estimated × $300/hour × 1.3 (risk buffer) = $15,600

**Risks to Consultant**: Scope creep; unknowns inflate hours; rushed delivery

**Tactics**
- Scope clearly in writing (what's included, what's not)
- Use phased delivery (phase 1: assessment for $5K; phase 2: implementation TBD after assessment)
- Include change order process (scope change = price change)
- Build in buffer (1.3–1.5x is standard)

**Value-Based Pricing**

Price based on value delivered, not hours spent.

- **Calculation**: Value to client × 10–30% (consultant's cut)
- **Examples**:
  - "Help reduce costs by $100K/year" → charge $15–30K
  - "Implement CRM saving 500 hours/year" → charge $10–20K
- **Pros**: Scales with success; aligns incentives; higher margins
- **Cons**: Requires trust; hard to estimate value; client may dispute value

**Risks to Consultant**: Client disputes value realization; harder to collect if value doesn't materialize

**Tactics**
- Validate value with client upfront (get agreement on ROI metric)
- Tie payment to outcomes: "Pay $5K now, $10K when system is live, $5K if cost savings exceed $50K"
- Use case studies to support value claims
- Start with one value-based project to build proof

**Retainer (Monthly Recurring)**

Agreed hours per month for ongoing support.

- **Price**: Average monthly hours × hourly rate; negotiate 10–20% discount vs. T&M
- **Best for**: Ongoing advisory, maintenance, fractional service
- **Example**: $5K/month for 15–20 hours of consulting + support

**Hybrid Models**

Combine structures to reduce risk:

- **Fixed-fee + T&M for unknowns**: "$15K for assessment; $300/hr for implementation"
- **Retainer + project work**: "$2K/month base + project fees for special work"
- **Value-based with hourly floor**: "Value-based pricing, minimum $20K" (protects consultant if client overestimates value)

**Rate Card by Discipline and Experience**

| Discipline | Junior (1–5 yr) | Mid (5–10 yr) | Senior (10+ yr) | Principal |
|-----------|-----------------|---------------|-----------------|-----------|
| **Strategy** | $150–200/hr | $250–350/hr | $400–500/hr | $500+/hr |
| **Implementation** | $100–150/hr | $200–300/hr | $350–450/hr | $450+/hr |
| **Fractional exec** | N/A | $400+/hr | $500+/hr | $750+/hr |

---

## 5. Price Testing Methodologies

**Van Westendorp Price Sensitivity Meter**

Survey-based, finds optimal price range.

**Method**:
1. Ask respondents four questions:
   - "At what price would you consider this product a bargain?" → Lower threshold
   - "At what price would you consider this product reasonably priced?" → Sweet spot (lower)
   - "At what price would you consider this product expensive?" → Sweet spot (higher)
   - "At what price would you consider this product so expensive you wouldn't buy it?" → Upper threshold

2. Plot cumulative distributions; find intersection points:
   - Cheapness-threshold: Where "bargain" and "expensive" cross
   - Expensiveness-threshold: Where "reasonably priced" and "too expensive" cross
   - Optimal price range: Between the two crossovers

**Pros**: Quick, low-cost, large sample size
**Cons**: Hypothetical (not actual behavior); doesn't account for competitive reference
**Best for**: Rapid ideation, narrowing price range

**Gabor-Granger Price Testing**

Iterative bidding approach; shows actual price acceptance.

**Method**:
1. Show product to respondent; ask "Would you buy at $X?"
2. If yes → raise price to $X+$10; ask again
3. If no → lower price to $X–$10; ask again
4. Repeat until find price ceiling (no = 1, yes = 0 transition)
5. Aggregate across respondents to find WTP curve

**Pros**: Mimics actual negotiation; captures price ceiling; accounts for quality signals
**Cons**: Labor-intensive; order effects (starting price anchors)
**Best for**: Single-product pricing; finding price ceiling

**A/B Testing (Controlled Experiment)**

Show different price to different cohorts; measure conversion.

**Setup**:
- **Cohort A**: See pricing at $49/month → measure conversion rate
- **Cohort B**: See pricing at $99/month → measure conversion rate
- **Cohort C**: See pricing at $199/month → measure conversion rate
- **Duration**: Run for 2–4 weeks to reach statistical significance

**Analysis**:
- Calculate conversion rate per price point
- Calculate revenue per user (price × conversion rate)
- Optimize for revenue, not conversion (lower price might have higher conversion but lower revenue)

**Sample Size Calculation** (for 80% power, 5% significance):

For 2% baseline conversion, 25% lift target:
- Baseline: 2% conversion
- Target: 2.5% conversion
- Sample needed per cohort: ~3,000 users
- Total users needed: 9,000+ (all three cohorts)

**Pros**: Measures actual behavior; accounts for all variables (positioning, landing page, etc.)
**Cons**: Requires statistical rigor; long test duration; expensive for low-volume products
**Best for**: High-volume SaaS, consumer products, validated idea testing

**Conjoint Analysis**

Tests how features and price trade off; finds optimal feature-price combinations.

**Method**:
1. Define features/attributes: seats (1, 5, 10), storage (1GB, 10GB, 100GB), price ($10, $50, $100)
2. Create feature combinations (profiles): 5 seats + 10GB + $50, etc.
3. Ask respondents to rate preference for each profile (rank or score)
4. Statistical analysis reveals:
   - Which features drive choice most
   - Acceptable price for each feature level
   - Optimal feature-price combination

**Example Conjoint Study** (for project management tool):

| Profile | Seats | Integrations | Support | Price | Preference |
|---------|-------|--------------|---------|-------|-----------|
| A | 5 | 50 | Email | $50 | 6/10 |
| B | 10 | 100 | Chat | $99 | 8/10 |
| C | 20 | 200 | Phone | $199 | 7/10 |

Conjoint reveals: "Phone support is worth $50; 100 integrations worth $30; 20 seats worth $40"

**Pros**: Comprehensive; shows trade-offs; optimal combinations
**Cons**: Complex; requires statistical expertise; expensive to conduct
**Best for**: Mature products with pricing optimization; complex feature trade-offs

**Iterative Pricing Optimization**

Ongoing testing; adjust price based on real data.

**Approach**:
1. Launch at initial price estimate
2. Monitor: conversion rate, revenue per user, churn rate, NPS
3. Run A/B test on new price quarterly
4. Implement winning variation; repeat

**Example**: Start at $99/month. Test $79 (higher conversion) vs. $119 (higher revenue). Winner is $119 (revenue per user higher despite lower conversion). Implement $119; test $129 vs. $119 next quarter.

**Pros**: Continuous learning; data-driven; adapt to market
**Cons**: Requires automation; needs high volume; risky (price changes can churn customers)
**Best for**: Mature products; high-volume SaaS

---

## 6. Competitive Pricing Analysis

**Competitive Positioning Matrix**

Map yourself vs. competitors on two axes: price and value perception.

| Position | Strategy | Example |
|----------|----------|---------|
| **Premium (high price, high value)** | Feature-rich, full support, mission-critical | Salesforce CRM ($300+/month) |
| **Value (high value, low price)** | Stripped down, DIY support, cost-efficient | Zapier competitor at lower price |
| **Budget (low price, low value)** | Basic features, minimal support | Free tier alternatives |
| **Overpriced (low value, high price)** | Avoid this position | Dead end |

**Competitive Feature Parity Analysis**

| Feature | Us | Competitor A | Competitor B |
|---------|----|--------------|----|
| API access | ✓ | ✓ | ✗ |
| Webhooks | ✓ | ✗ | ✓ |
| Custom fields | ✓ | ✓ | ✓ |
| SSO | ✓ (Pro tier) | ✓ (All) | ✓ (Enterprise) |
| Support SLA | None (Standard) | 99.9% (all) | 99.99% (Enterprise) |
| Price | $99 | $149 | $199 |

Insight: You're 33% cheaper but lack SLA commitment. Opportunity: Add SLA to Pro tier; repositions as premium alternative.

**Tactics**

- **Don't compete on price alone**: Lower price in crowded market = race to bottom
- **Find differentiation axis**: Feature (unique integration), quality (uptime SLA), service (support), experience (UX)
- **Position relative to reference**: "50% cheaper than Competitor A, same features"
- **Segment competitors**: Direct (exact alternative), substitutes (different approach), aspirational (future state)

---

## 7. Price Increase Communication

**Announcing Without Churn**

| Approach | Message | Expected Churn |
|----------|---------|-----------------|
| **Abrupt increase** | "Price increases to $149 effective immediately" | High (20–50%) |
| **Advanced notice + grandfather** | "New customers pay $149; current users stay $99 forever" | Low (2–5%) |
| **Packaging change** | "Professional plan now includes X, Y, Z. Price: $149" | Very low (0–2%) |
| **Selective increase** | "We adjusted plans. You qualify for Pro at $99; others pay $149" | Low (5–10%) |

**Best Practice Template**

1. **Announce 60 days in advance** — gives customers time to budget
2. **Grandfather existing customers** — fairness; retain LTV-positive users
3. **Frame as value increase** — "Added features: X, Y, Z. Price: $149"
4. **Offer downgrade path** — "Stay at $99 with Basic plan; upgrade to Pro for $149"
5. **Highlight new value** — Emphasize benefits, not cost increase
6. **Segment communication**:
   - In-app message for self-serve users
   - Personal email for high-value accounts
   - Sales team calls for enterprise

**Example Announcement Email**

*Subject: New Professional Plan — More Features, Better Value*

"We've been listening. Our most requested features are now in the Professional plan: custom reporting, API access, and priority support. As of [date], Professional plan pricing is $149/month. Your current plan stays at $99/month forever—no action needed. If you want the new features, upgrade to Professional anytime."

---

## 8. Enterprise Deal Pricing and Negotiation

**Anchor Strategy**

1. **Set list price high**: List price = anchor
2. **Offer discount range**: 10–30% discount is typical for annual commitment
3. **Anchor negotiation**: Start negotiation from list price, not their first offer

| List Price | Standard Discount | Enterprise Discount |
|-----------|-------------------|-------------------|
| $500/month ($6K/year) | 20% → $4,800/year | 30% → $4,200/year + $5K implementation |

**Multi-Year Deal Structure**

Lock in customer with favorable pricing:

- **Year 1**: $500/month × 12 = $6,000
- **Year 2**: Increase 5% = $525/month × 12 = $6,300
- **Year 3**: Increase 5% = $551/month × 12 = $6,612
- **Total 3-year value**: $18,912 (vs. $21,600 at flat pricing)
- **Customer savings**: 13% for commitment

**BATNA (Best Alternative to Negotiated Agreement)**

Before negotiating, know your walk-away point:

- **Minimum deal size**: $5K/year (not worth enterprise support)
- **Minimum term**: 1 year (reduce churn risk)
- **Discount floor**: 25% off list (preserve margin)
- **Support SLA expectation**: Reasonable (24-hour response)

**Tactics**

- **Anchor first**: You propose price first; anchors negotiation
- **Justify price**: "This price reflects your usage of [metric]. Fair market value is [competitor price]"
- **Bundle support**: Include implementation, onboarding, training (de-risks for customer)
- **Use multi-year for discount**: "Year 1: $500/month; Year 2–3: 15% discount for commitment"
- **Escalation path**: Standard → Professional ($X) → Enterprise ($Y+) → Custom (negotiated)

---

## Key Integration Points

**Tier Architecture** + **Feature Differentiation** = Decoy effect + charm pricing + prestige pricing

**Freemium** + **Endowment Effect** = Deeper free trial + full feature access + usage-limit paywall

**Price Testing** → **A/B Testing** → **Pricing Optimization** → **Enterprise Negotiation**

**Competitive Analysis** + **Value-Based Pricing** = Position for differentiation, not race to bottom

---

*For behavioral framing and perception tactics, see behavioral_pricing_framework.md. For foundational behavioral economics theory, see ../../references/behavioral-foundations.md.*
