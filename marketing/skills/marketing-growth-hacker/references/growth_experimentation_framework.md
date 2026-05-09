---
type: reference
project: skills-library
scope: skill
plugin: marketing
parent: "[[growth-hacker]]"
tags: [type/reference, plugin/marketing, scope/skill, topic/marketing]
status: active
---
# Growth Experimentation Framework

## The Growth Testing Methodology

### 1. Hypothesis Development

**Structure**:
```
If we [make this change],
then [this outcome] will [improve/increase/decrease]
by [X%] for [this user segment]
because [underlying assumption].
```

**Good hypothesis** (testable, measurable):
"If we add a referral incentive ($10 credit), then signup rate will increase by 10% because current users will invite more friends."

**Bad hypothesis** (vague, unmeasurable):
"If we make the product better, more people will use it."

---

### 2. Impact vs. Effort Matrix

Prioritize experiments by ROI (impact per effort):

```
High Impact,  │  HIGH PRIORITY      │  MEDIUM PRIORITY
Low Effort    │  Test first!        │  Do next
              │  (10x payoff)       │
              │
Medium        │  MEDIUM PRIORITY    │  LOW PRIORITY
Impact/Effort │  Strategic bets     │  Avoid unless quick
              │                     │
Low Impact,   │  SKIP               │  SKIP
High Effort   │  Waste of time      │  Not worth effort
```

**Examples**:
- High impact, low effort: Change CTA button color (if CTR matters)
- High impact, high effort: Rebuild signup flow
- Low impact, low effort: Fix typo on about page (nice to do, low priority)
- Low impact, high effort: Redesign entire website (avoid)

---

### 3. RICE Prioritization

Quantify impact/effort trade-off:

```
Priority = (Reach × Impact × Confidence) / Effort

Reach = # users affected (1, 10, 100, 1000, 10000)
Impact = magnitude of change (0.25 = minimal, 0.5 = small, 1.0 = major)
Confidence = belief in hypothesis (0.25 = low, 0.75 = medium, 1.0 = certain)
Effort = dev weeks needed (0.5, 1, 2, 4 weeks)
```

**Example**:
```
Experiment: Add referral bonus ($10)
Reach: 5,000 users (not all use referral)
Impact: 1.0 (10% signup lift)
Confidence: 0.75 (have similar data from other SaaS)
Effort: 0.5 weeks (simple feature)

Priority = (5000 × 1.0 × 0.75) / 0.5 = 7,500
```

Compare across experiments; higher score = higher priority.

---

### 4. Experiment Design

**Key elements**:
- **Hypothesis** (what are we testing?)
- **Variant A (Control)** (current state)
- **Variant B (Test)** (what we're trying)
- **Success metric** (how we measure)
- **Sample size** (how many users, how long)
- **Significance threshold** (95% confidence = ~2,000 samples for most tests)

**Example: CTA Button Color Test**

| Element | Description |
|---------|-------------|
| Hypothesis | If we change CTA button from gray to green, signup rate increases 5% |
| Control | Gray button: "Get Started" |
| Test | Green button: "Start Free Trial" |
| Metric | Signup rate (clicks / visitors) |
| Sample | 2,000 visitors per variant (4,000 total) |
| Duration | 1 week (until 4,000 visitors) |
| Significance | 95% confidence; winner must be 5%+ better |

---

### 5. Statistical Significance

**Why it matters**: Without enough data, differences could be random chance.

**Rule of thumb**: Need ~1,000-2,000 samples per variant to detect 10% difference at 95% confidence.

**Power Analysis Calculator**: Use Optimizely, VWO, or online calculators to determine sample size.

**Example**:
- Testing CTA copy change on landing page
- Current conversion rate: 5%
- Want to detect: 10% improvement (5% → 5.5%)
- Confidence: 95%
- Sample needed: ~3,200 per variant (6,400 total)
- If 100 visitors/day: ~64 days to run test

**Avoid**:
- Running tests until you "see" a winner (statistical bias)
- Stopping early because variant is "obviously winning" (needs full sample)
- Running multiple tests on same metric (increases false positives)

---

### 6. Experimentation Velocity

**Growth Sprint Structure** (weekly):

| Day | Activity |
|-----|----------|
| Monday | Team brainstorm: Generate 5-10 growth hypotheses |
| Tuesday | Prioritize using RICE; select top 3-5 experiments |
| Wednesday | Set up experiments; deploy variants |
| Thursday | Monitor: Do experiments have data? Adjust if needed |
| Friday | Analyze results; decide what to scale, kill, or iterate |

**Minimum viable test**: Can we run this experiment with minimal engineering effort?

Example: Changing email subject line doesn't require engineering (run test in email platform).
Example: Landing page copy change requires one developer day.

---

## Conversion Funnel Testing

### Identifying Friction Points

**Test each stage systematically**:

```
Landing Page
  ↓ (Conversion Rate: 30% of visitors)
Signup Form
  ↓ (Conversion Rate: 60% of signups)
Onboarding
  ↓ (Conversion Rate: 50% complete onboarding)
Feature Trial
  ↓ (Conversion Rate: 20% trial-to-paid)
Paid Customer
```

**Where to focus**: Highest-impact stage with lowest conversion.

Example: If signup form has 60% conversion (good) but feature trial has 20% (bad), focus on trial experience first.

### Funnel Experiments

**Landing Page Experiments**:
- Headline A vs. Headline B (which resonates more?)
- Value prop clarity (long form vs. short)
- Visitor-to-signup CTA position (top, middle, multiple)

**Signup Flow Experiments**:
- Fields required (fewer fields = higher signup, lower quality)
- Form field order (email first or name first?)
- Social login vs. email (OAuth conversion rate)

**Onboarding Experiments**:
- Tutorial required vs. optional (skipped more?)
- Feature introduction order (most important first or most fun?)
- Incentive (offer gift card if complete onboarding)

**Trial-to-Paid Experiments**:
- Trial length (7 days vs. 14 days vs. unlimited until feature limit)
- Feature gating (what features are locked in trial?)
- Upgrade prompt timing (when do we ask? Day 3? Day 5?)
- Pricing presentation (3 tiers vs. 2 tiers, monthly vs. annual)

---

## Channel Testing

### Paid Channel Experiments

**Channel A/B Testing**:
```
Budget: $1,000/month
Allocate: $500 Google Ads, $500 Meta Ads
Track: CAC, conversion rate, LTV by channel
After 30 days: Double budget for winning channel
After 90 days: Optimize winning channel heavily
```

**Creative Testing**:
- Ad copy (benefit vs. curiosity vs. urgency)
- Visual (product screenshot vs. lifestyle image vs. video)
- CTA button (Free Trial vs. Learn More vs. Get Started)
- Landing page (custom vs. general)

**Audience Testing**:
- Interests/keywords (broad vs. narrow)
- Job titles / industries
- Company size
- Geographic regions

### Organic Channel Experiments

**Content Testing**:
- Format (blog vs. video vs. infographic)
- Topic (which topics get most engagement?)
- Title (data-driven vs. curiosity-driven)
- Length (short-form vs. long-form)

**Social Media Testing**:
- Posting time (which times get most engagement?)
- Posting format (carousel vs. video vs. text)
- Caption length (long vs. short)
- Hashtag strategy (how many, which ones?)

**Email Testing**:
- Subject line (personalized vs. benefit-driven vs. curiosity)
- Send time (morning vs. afternoon vs. evening)
- Content length (long vs. short)
- CTA wording (free trial vs. schedule demo)

---

## Retention & Churn Testing

### Retention Experiments

**Goal**: Increase day 7 / day 30 retention

**Tactics**:
- **In-app guidance**: Show new users where to go
- **Feature education**: Tutorial, tip, demo video
- **Social proof**: Show what others are doing
- **Gamification**: Badges, streaks, progress bars
- **Incentives**: Points, rewards for completing actions
- **Engagement reminders**: Email, in-app notifications

**Example**:
```
Test: Adding progress bar to first-time user dashboard
Control: No progress bar
Metric: Day 7 retention (% still active)
Expected: +5-10% improvement
Sample: 500 new users per variant
Duration: 2 weeks
```

### Churn Prevention Experiments

**Goal**: Identify and prevent churn

**Tactics**:
- **Win-back emails**: Re-engage inactive users
- **Feature highlights**: Show unused features they might like
- **Upgrade incentive**: Offer discount if they upgrade
- **Cancellation survey**: Ask why they're leaving (learn pattern)
- **Premium tier test**: Sometimes users churn because plan is wrong fit

**Example**:
```
Segment: Users inactive for 14 days
Test: Send "come back" email with discount offer
Control: No email
Metric: Return and re-activation rate
Expected: 10-15% re-engagement
```

---

## Data-Driven Decision Making

### Analyzing Test Results

**Questions to ask**:
1. **Is result statistically significant?** (>95% confidence)
2. **What's the actual lift?** (5%? 10%? 50%?)
3. **Is it meaningful?** (5% lift × 10 users = not worth scaling)
4. **What segments won the most?** (Maybe only certain users benefited)
5. **Any surprises?** (Why did variant B work for segment X but not Y?)

**Avoiding false positives**:
- Don't stop test early ("winning so far")
- Don't run multiple tests on same metric (increases false positive rate)
- Measure intent (if signup up but quality down, is that good?)

### Learning from Failed Tests

**Example failed test**:
```
Hypothesis: Adding referral incentive will increase signups 10%
Result: No statistically significant change
Learning: Referral incentive doesn't work for our audience
Next action: Instead of referral bonus, test content marketing
```

Failed tests are just as valuable as winners—they teach you what your audience cares about.

---

## Growth Experiment Calendar

**Sample 8-week plan**:

| Week | Experiments | Focus | Expected Impact |
|------|-------------|-------|-----------------|
| 1 | CTA button copy (3 variants) | Signup rate | +5-10% signups |
| 2 | Email subject line (2 variants) | Engagement | +3-5% opens |
| 3 | Onboarding flow simplification | Day 7 retention | +10% retention |
| 4 | Referral incentive test | Viral coefficient | +20% referrals (if works) |
| 5 | Paid channel allocation (Google vs Meta) | CAC optimization | -20% CAC |
| 6 | Trial-to-paid pricing test (2 tiers vs 3) | Conversion rate | +5-10% trial-to-paid |
| 7 | Churn prevention: Win-back email | Reactivation | +10-15% reactivate |
| 8 | Double down on week 1-7 winners | Scale | 2-3x impact |

**Expected compounding effect**: By week 8, combining small wins compounds to 30-50% overall funnel improvement.

---

## Tools for Growth Experiments

| Tool | Purpose | Cost |
|------|---------|------|
| **Optimizely** / **VWO** | A/B testing platform | $500-5000/mo |
| **Unbounce** | Landing page builder + tests | $99-499/mo |
| **Google Optimize** | Free A/B testing (paired with GA) | Free |
| **Mixpanel** / **Amplitude** | Analytics + funnels | $999-3000+/mo |
| **Intercom** / **Drift** | In-app messaging tests | $500-2000+/mo |
| **ConvertKit** / **ActiveCampaign** | Email marketing + tests | $100-500/mo |
| **Google Ads** / **Meta Ads** | Paid channel testing | Budget dependent |

---

## Red Flags in Growth Testing

1. **Testing without hypothesis**: "Let's just try something"
2. **Stopping too early**: "It's winning, let's ship it" (before statistical significance)
3. **Moving goalposts**: Changing success metric mid-test
4. **Not documenting results**: Can't learn from test later
5. **Testing too many things**: Can't attribute results
6. **Ignoring segments**: Overall positive but negative for some users
7. **No follow-up**: Winner ships, but nobody tracks if it stuck
8. **Vanity metrics**: Testing things that don't matter (e.g., colors when UX is broken)

---

## Growth Experiment Checklist

Before launching any test:
- [ ] Is there a clear, testable hypothesis?
- [ ] Have we estimated sample size needed for significance?
- [ ] Is success metric well-defined and trackable?
- [ ] Have we accounted for confounding variables (seasonality, etc.)?
- [ ] Do we have enough traffic to reach sample size in reasonable time?
- [ ] Is test setup correct (no bugs, random assignment)?
- [ ] Are we tracking against a control (not just assuming baseline)?
- [ ] Have we communicated plan to team (aligned expectations)?
- [ ] Do we know when we'll analyze results (avoid peeking)?
- [ ] What's the rollout plan if test wins?
