---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[customer-feedback-synthesizer]]"
tags: [type/reference, plugin/cx, scope/skill, topic/customer-experience]
status: active
---
# Customer Feedback Synthesis Playbook

## Step 1: Data Collection & Normalization

### Feedback Sources to Monitor

**Quantitative:**
- NPS surveys (with follow-up questions)
- CSAT/CES surveys
- Product analytics (feature usage, funnel drops, session recordings)
- Revenue data (expansion, churn, downgrades)

**Qualitative:**
- Support tickets and chat transcripts
- User interviews and research reports
- App store and product review sites
- Social listening (Twitter, Reddit, industry forums, Facebook groups)
- Sales notes and customer conversations
- In-app feedback prompts and surveys

**Mixed:**
- Call/meeting recordings and transcripts
- Community forum posts and comments
- Email replies to company outreach
- Customer advisory board feedback

---

### Normalizing Data for Analysis

Create a simple intake format to standardize inputs:

| Field | Description | Example |
|---|---|---|
| **Source** | Where feedback came from | NPS survey, support ticket, interview |
| **Date** | When feedback was provided | 2026-03-15 |
| **Customer Segment** | Type of customer | Enterprise / SMB / Freemium |
| **Lifecycle Stage** | Where they are in journey | Onboarding / Active / Churned |
| **Verbatim Feedback** | Original quote or full comment | "Setup takes too long. Wish there was a wizard" |
| **Topic** | High-level category | Onboarding / Features / Price |
| **Sentiment** | Overall tone | Positive / Negative / Neutral |
| **Severity** | How critical is this | Low / Medium / High |
| **Actionable** | Can we act on this? | Yes / No |

**Tool options**: Google Sheets, Airtable, Notion, Dovetail, Condens, Taguette

---

## Step 2: Qualitative Synthesis (Affinity Mapping)

### The Process

1. **Create a list** of all feedback items (quotes, observations, complaints)
2. **Read through** all items to get a sense of the full data set
3. **Create sticky notes** (digital or physical) with each piece of feedback
4. **Group by theme**: What themes emerge naturally? What appears multiple times?
5. **Label themes**: Name each cluster (e.g., "Onboarding pain points", "Pricing concerns")
6. **Prioritize themes**: Which appear most frequently? Which are most severe?

### Example Affinity Map: Customer Complaints

**Raw feedback (10+ similar comments):**
- "Setup wizard didn't explain what to do next"
- "After initial setup, I didn't know which feature to use first"
- "Took me an hour to find where to import data"
- "No clear first workflow; I felt lost"
- "First 30 minutes were confusing; didn't know where to start"
- "Onboarding walks through features, but doesn't show me MY workflow"

**Identified Theme:** "Unclear First Success Path"

**Insights:**
- Problem: Setup process doesn't connect to first meaningful use
- Impact: New users feel lost after setup; may abandon before experiencing value
- Drivers: Tutorial-focused (features) vs. workflow-focused (tasks); no goal-setting step

**Recommendations:**
- Add goal-setting step after signup: "What do you want to accomplish first?"
- Personalize onboarding path based on goal
- Create guided workflow for first task, not just feature overview
- Add success milestone: "You just completed your first [workflow]"

---

## Step 3: Quantitative Analysis

### Aggregation & Counting

**Simple counts:**
- How many customers mentioned this theme? (frequency)
- Is it growing or shrinking? (trend)
- Which customer segments mention this most? (segmentation)

**Example:**
- 47 customers mentioned "slow performance" in last 90 days
- 23 of them mentioned it multiple times (severity indicator)
- 60% of enterprise customers mentioned it vs. 20% of SMB (segment difference)
- Mentions peaked 2 weeks after feature launch (correlation)

---

### Correlation Analysis

Look for patterns:
- Do churned customers mention different issues than retained customers?
- Do low-NPS respondents cite specific pain points?
- Does product usage correlate with satisfaction?
- Did feedback change after a product update?

**Example:**
- Customers who didn't complete onboarding have 5x higher churn rate
- Customers using [Feature X] have 30% higher retention
- Churned customers cited "complicated pricing" 3x more than retained customers

---

### Segmentation

Break feedback by meaningful groups:
- **Customer segments**: Enterprise vs. SMB vs. Freemium
- **Lifecycle**: New vs. Active vs. Churned
- **Cohort**: Customers by sign-up month or product version
- **Geography**: Regional differences
- **Use case**: Different use patterns or industries

**Example:**
- SMB customers: Focus on ease of use, quick setup (85% of feedback)
- Enterprise customers: Focus on features, customization, integrations (72% of feedback)
- Churned customers: Price concerns, feature gaps, switching to competitors (majority)

---

## Step 4: Root Cause Analysis

### The Five Whys (Simplified)

For each major complaint, dig deeper to find the root cause.

**Surface complaint**: "Reporting is slow"

**Root cause investigation:**
1. "Why is reporting slow?" → "Queries against the live database take too long"
2. "Why do queries take too long?" → "We don't have data warehouse; aggregating real-time data"
3. "Why haven't we built a data warehouse?" → "It's complex and lower priority than features"
4. **Root cause**: Infrastructure gap; not a simple fix

**Implication**: "Slow reporting" is a symptom of a larger technical debt issue, not a surface bug.

---

**Surface complaint**: "Pricing is too high"

**Root cause investigation:**
1. "Why is pricing too high?" → "Can't justify cost for what we get"
2. "Why can't you justify the cost?" → "Don't see ROI; features don't match our needs"
3. "Why don't the features match your needs?" → "We switched from [competitor]; different workflow"
4. **Root cause**: Feature-market fit gap; customers came from different tool and workflows don't align

**Implication**: Price isn't the real problem—the product isn't a good fit. Discounting won't help.

---

### Pattern Recognition

**Look for clusters of related issues:**
- Are complaints about [Feature A] always tied to [Feature B]?
- Do customers who struggle with [Workflow X] always churn within 90 days?
- Do [Customer Segment A] always mention [Pain Point Y]?

**Example:**
- All complaints about "slow performance" come from customers using [Feature X] with [Large Dataset Size]
- Customers who churn mention "can't customize workflows" → usually enterprise customers
- Customers in [Industry] complain about compliance, but other industries don't

---

## Step 5: Prioritization Framework

### Impact × Effort Matrix

Prioritize improvements by potential value vs. effort required.

| Quadrant | Effort | Impact | Action |
|---|---|---|---|
| **Quick Wins** | Low | High | Do immediately |
| **Strategic Bets** | High | High | Prioritize for roadmap |
| **Fill-Ins** | Low | Low | Do if capacity exists |
| **Avoid** | High | Low | Don't do |

**Example:**

**Quick Wins:**
- Add in-app help for confused users (doc fixes, tooltips, better labels)
- Improve onboarding copy to clarify first steps
- Add missing links in help documentation

**Strategic Bets:**
- Redesign onboarding to be goal-based instead of feature-based
- Build data warehouse for performance
- Develop missing feature customers request most

**Fill-Ins:**
- Minor UI improvements
- Additional template library
- Better error messages

**Avoid:**
- Overhaul of features only a few customers use
- Complex customizations for single-customer requests
- Nice-to-have enhancements with low impact

---

### Validation Questions Before Prioritizing

1. **Is this a real problem?** (Frequency: how many customers? Severity: how painful?)
2. **Is it fixable?** (Can we actually address it, or is it an edge case?)
3. **Will it impact retention/expansion?** (Does fixing it affect key metrics?)
4. **Is it aligned to strategy?** (Does it fit our roadmap direction?)
5. **Do we have capacity?** (Can we realistically address it in the next quarter?)

---

## Step 6: Insight Translation & Recommendations

### Translating Feedback for Different Audiences

**FOR PRODUCT LEADERSHIP:**
"43% of churned customers cited missing [Feature X]. Top 3 competitors all offer this. Recommendation: prioritize [Feature X] in next roadmap cycle. Estimated impact: 10-15% improvement in retention."

**FOR PRODUCT DESIGNERS:**
"Users struggle with [Workflow] because [reason]. Specific complaint: '[Quote]'. Recommendation: redesign this flow to [specific idea]. Quick win: improve labels and add contextual help first."

**FOR MARKETING:**
"Customers are confused about [positioning]. They think [Product] is for [wrong use case]. Recommendation: refresh messaging to clarify we're designed for [actual use case]. Also: update case studies to show [target segment] examples."

**FOR SUPPORT:**
"Common complaint: [Issue]. Root cause: [explanation]. Workaround: [solution]. Recommendation: add to FAQ and create support article. Also consider: [Product improvement that would eliminate the need for workaround]."

**FOR SALES:**
"Customers ask about [Feature] in conversations. It's not currently a blocker, but it's becoming table stakes. Recommendation: add [Feature] to roadmap within next 2 quarters. In the meantime, positioning is: 'On our roadmap for [quarter]; here's our vision for how it will work...'"

---

### Sample Insight Formats

**One-Pager Insight Brief:**
- **Problem**: [What customers are struggling with]
- **Severity**: [How many affected? How painful?]
- **Root Cause**: [Why is this happening?]
- **Customer Impact**: [What does this cost them?]
- **Our Impact**: [How does this affect retention, expansion, support load?]
- **Recommended Action**: [What should we do?]
- **Success Metric**: [How will we know if we fixed it?]

**Feedback Roundup (Weekly/Sprint):**
- **Top Theme**: [Most common issue this week]
- **Quick Wins**: [3 small things we could fix immediately]
- **Questions for Product**: [What product decisions would reduce this feedback?]
- **Segment Alert**: [Did one customer segment suddenly start complaining about something?]
- **Trending Up**: [Is this issue getting worse?]

**Executive Summary (Monthly):**
- **Overall Sentiment**: [Happy, frustrated, indifferent?]
- **Key Trends**: [What's changing month-over-month?]
- **Top Issues**: [3-5 most common themes]
- **Actions from Last Month**: [What did we do? Did it help?]
- **Roadmap Alignment**: [Is feedback aligning with our strategy?]

---

## Step 7: Closing the Loop

### Communicating Back to Customers

**If you take action:**
- "We heard your feedback about [issue]. We just shipped [solution]. Appreciate you for the feedback!"

**If you decide not to act:**
- "We heard your feedback about [issue]. Here's why it's not a priority right now: [reason]. Here's an alternative: [workaround]."

**If you're still investigating:**
- "We're looking into your request about [issue]. We'll have an update in [timeframe]."

---

### Documenting Decisions

Create a simple feedback decision log:

| Feedback Theme | Volume | Decision | Rationale | Status |
|---|---|---|---|---|
| "Setup is slow" | 47 customers | Act: Optimize flow | High impact; affects 30% of new users | In progress |
| "Dark mode" | 12 customers | Defer | Nice to have; low business impact | Backlog |
| "Export to PDF" | 24 customers | Act: Build feature | 3x enterprise customer request | Planned for Q3 |
| "[Obscure edge case]" | 1 customer | Decline | Affects only power users; workaround exists | Closed |

---

## Tools & Platforms

### Text Analysis & Synthesis
- **Dovetail**: AI-powered qualitative analysis platform
- **Condens**: Transcription + synthesis for qualitative research
- **Taguette**: Open-source text annotation and tagging
- **NVivo**: Enterprise qualitative analysis
- **Airtable**: DIY database for feedback tracking and analysis

### Feedback Collection
- **NPS Tools**: Delighted, Promoter.io, SurveySparrow
- **CSAT Tools**: Qualtrics, SurveySense, Typeform
- **Support Platforms**: Zendesk, Freshdesk, Intercom (all have built-in analytics)
- **Session Replay**: Hotjar, FullStory, LogRocket
- **Survey Tools**: Google Forms, SurveyMonkey, Typeform

### Dashboarding & Reporting
- **Looker Studio**: Free Google dashboards over survey/support data
- **Tableau / Power BI**: Enterprise analytics
- **Notion**: Database-backed dashboards with filtering
- **Google Sheets**: Simple pivot tables and charts

---

## Common Mistakes to Avoid

1. **Treating feedback as gospel**: One angry customer isn't a trend; look for patterns.
2. **Over-weighting recent feedback**: Recency bias can distort priorities.
3. **Confusing causation with correlation**: "Customers who use Feature X stay longer" doesn't mean Feature X causes retention.
4. **Ignoring segment differences**: What matters to enterprise is different from SMB.
5. **Not closing the loop**: Customers feel heard only if you tell them what you're doing with feedback.
6. **Treating feedback analysis as one-time**: Set up systems to continuously monitor and synthesize.
7. **Not acting on clear patterns**: If multiple customers report the same problem, it's a signal—don't ignore it.
