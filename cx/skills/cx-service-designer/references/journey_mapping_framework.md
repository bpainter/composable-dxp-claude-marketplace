---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[service-designer]]"
tags: [type/reference, plugin/cx, scope/skill, topic/customer-experience]
status: active
---
# Customer Journey Mapping Framework

## Core Elements of a Journey Map

### 1. Journey Stages
Define the major phases of the customer lifecycle. Typical stages:
- **Awareness**: Discovery, initial interest, consideration
- **Consideration**: Evaluation, comparison, decision-making
- **Onboarding**: Setup, first use, initial adoption
- **Usage**: Regular usage, feature exploration, deepening engagement
- **Support**: Help-seeking, troubleshooting, escalation
- **Renewal/Expansion**: Renewal decision, upsell, cross-sell
- **Advocacy/Churn**: Retention, advocacy, or churn decision

Customize stages based on your business model and customer lifecycle.

---

### 2. Touchpoints
List every interaction moment across channels:
- **Digital**: Website, app, email, chatbot, search ads
- **Physical**: Store, events, offices, product packaging
- **Human**: Sales, support, account management, success team
- **Third-Party**: Marketplace, review sites, partner integrations, community

Map both company-initiated touchpoints (outbound) and customer-initiated (inbound).

---

### 3. Customer Actions & Behaviors
What is the customer doing at each stage?
- Specific actions (searching, comparing, using features, contacting support)
- Decision points (choose vs. don't choose, stay vs. leave)
- Workarounds (things customers do that shouldn't be necessary)

---

### 4. Emotional Journey
Where do customers feel delight or frustration?
- **Highs**: Moments of success, ease, delight, trust
- **Lows**: Moments of confusion, friction, anxiety, disappointment
- **Turning points**: Critical moments that shift sentiment

Use emoticon charts or emotional curves to visualize.

---

### 5. Thoughts & Feelings
What's running through the customer's mind?
- Expectations: "I expect this to be easy"
- Concerns: "Will this work for my use case?"
- Frustrations: "This is more complicated than it should be"
- Motivations: "I want to accomplish X"

Include both rational (task-focused) and emotional (feeling-focused) dimensions.

---

### 6. Pain Points & Gaps
Where does the experience break down?
- **Usability issues**: Confusing UI, unclear instructions, hard to find information
- **Emotional gaps**: Lack of trust, unclear value, anxiety about commitment
- **System failures**: Slow performance, errors, missing features
- **Misalignment**: What the company promised vs. what customers experience
- **Handoff failures**: Breakdowns when work transfers between teams or systems

---

### 7. Internal Processes & Systems (Backstage)
Map the invisible work that creates the experience:
- **Teams involved**: Sales, support, delivery, product, operations
- **Systems and tools**: CRM, helpdesk, payment processor, fulfillment
- **Policies and processes**: Approval workflows, SLAs, escalation paths
- **Support processes**: How issues are resolved, decisions made

Show where internal silos create customer friction.

---

### 8. Opportunities
Where can you improve the experience?
- **Quick wins**: Small changes with high impact
- **Structural improvements**: Process or system changes that solve root causes
- **Experience innovations**: New ways to delight customers

Prioritize by impact and effort.

---

## Journey Mapping Process

### Phase 1: Research & Discovery

**Gather qualitative insights:**
- Conduct 8-12 customer interviews (mix of loyal and at-risk customers)
- Observe customers in action (contextual inquiry)
- Run diary studies (customers document journey over time)
- Review support tickets for common pain points

**Analyze quantitative data:**
- Funnel analysis: Where do customers drop off?
- NPS/CSAT trends: Where is satisfaction declining?
- Feature analytics: What features are used? What's ignored?
- Cohort analysis: How do different customer segments experience the journey?

**Synthesize insights:**
- Affinity mapping: Group themes from research
- Pattern identification: What appears repeatedly vs. edge cases
- Hypothesis formation: Why do customers feel X at stage Y?

---

### Phase 2: Build the Map

**Structure:**
1. Identify all stages (4-8 major phases)
2. List touchpoints per stage
3. Document customer actions, thoughts, feelings
4. Plot emotional highs and lows
5. Identify pain points and opportunities

**Visualize:**
- Use timeline or stage-based layout
- Color-code by emotional tone (green = positive, yellow = neutral, red = negative)
- Use icons or illustrations to make it scannable
- Include actual customer quotes
- Show team/system responsibilities below touchpoints

---

### Phase 3: Service Blueprinting (Optional)

Extend the journey map by adding the backstage:

- **Frontstage (Customer-Visible)**: Touchpoints and customer actions
- **Line of Interaction**: The boundary between visible and invisible
- **Backstage (Internal Processes)**: Teams, tools, policies that enable the experience
- **Support Systems**: HR, IT, finance that enable frontline teams

---

### Phase 4: Insight Synthesis & Prioritization

**Translate findings into action:**

1. **Root Cause Analysis**: Why are customers frustrated at stage X?
   - Is it a usability problem? A process problem? An expectation gap?

2. **Opportunity Prioritization**:
   - **Impact**: How many customers does this affect? How severe is the pain?
   - **Effort**: How much work is required to improve?
   - **Strategic Fit**: Does this align to our business goals?

3. **Recommendations**:
   - **Quick wins**: Small improvements with immediate impact
   - **Structural improvements**: Larger changes with bigger payoff
   - **Strategic bets**: Innovation opportunities aligned to future roadmap

---

## Common Journey Mapping Patterns

### SaaS Product Onboarding Journey

**Stages**: Signup → Initial Setup → First Success → Regular Use → Expansion

**Touchpoints by Stage**:
- Signup: Landing page, signup form, welcome email, activation link
- Setup: Welcome sequence, onboarding flow, feature discovery, documentation
- First Success: Tutorial/walkthrough, sample data, support chat, "aha moment" trigger
- Regular Use: Feature navigation, notifications, in-app help, support tickets
- Expansion: Upsell emails, feature announcements, success check-ins, upgrade flows

**Common Pain Points**:
- Unclear value proposition; users don't understand why they're signing up
- Confusing setup process; users struggle to complete onboarding
- Feature discovery gap; users don't know what's possible
- Unclear when to upgrade; users stuck on free tier not knowing value of paid
- Support friction; hard to get help when stuck

**Opportunities**:
- Clarify value prop on landing page with segment-specific messaging
- Streamline setup with guided experience and autocomplete
- Add contextual help and in-app tutorials
- Create activation milestone (first meaningful success) and track rate
- Build expansion flows that showcase premium features

---

### E-Commerce Customer Journey

**Stages**: Discovery → Consideration → Purchase → Delivery → Post-Purchase Support → Loyalty

**Touchpoints**:
- Discovery: Search, ads, social media, influencers, word of mouth
- Consideration: Product page, reviews, competitor comparison, FAQ
- Purchase: Add to cart, checkout, payment, order confirmation
- Delivery: Shipping updates, delivery experience, unboxing
- Support: Returns, product questions, warranty service
- Loyalty: Loyalty program, repeat purchase, referral

**Common Pain Points**:
- High cart abandonment (unclear pricing, complicated checkout, shipping costs)
- Delivery delays or no tracking information
- Product doesn't match expectations (photos vs. reality)
- Returns/refunds are difficult
- No personalization or loyalty incentive

**Opportunities**:
- Simplify checkout; one-page flow, guest checkout option
- Transparent shipping costs and delivery timeline upfront
- Clear product photos, video, detailed descriptions
- Easy returns; prepaid shipping, no-question refund window
- Personalization based on browsing/purchase history

---

### B2B SaaS Sales Journey

**Stages**: Awareness → Evaluation → Trial → Negotiation → Implementation → Success

**Touchpoints**:
- Awareness: Content marketing, industry events, referrals, sales outreach
- Evaluation: Product demo, case studies, comparison documents, trial signup
- Trial: Trial access, onboarding, success metrics, support
- Negotiation: Pricing discussions, contract review, legal alignment
- Implementation: Kickoff, training, data migration, go-live
- Success: Regular check-ins, feature rollout, expansion conversations

**Common Pain Points**:
- Long sales cycle with unclear decision criteria
- Overly complex trial that requires IT setup
- Disconnect between sales promises and implementation reality
- Poor onboarding; customers don't see quick wins
- Account management gaps; no one driving expansion

**Opportunities**:
- Streamline trial signup; reduce technical barriers
- Create clear trial success path with win criteria
- Better sales-to-delivery handoff; ensure continuity
- Dedicated onboarding with clear milestones
- Assigned account managers for retention and expansion

---

## Tools & Templates

### Tools for Creating Journey Maps
- **Figma/FigJam**: Collaborative design environment
- **Miro/Mural**: Real-time whiteboarding and mapping
- **Notion**: Database-backed journey maps with filtering
- **Excel/Google Sheets**: Simple stage-based format
- **Specialized tools**: Smaply, UXPressia, Custellence

### Common Map Formats
1. **Horizontal Timeline**: Stages left to right, touchpoints below
2. **Vertical Stages**: Each stage is a column with rows for elements
3. **Journey Arc**: Emotional curve overlaid on timeline
4. **Ecosystem Map**: Shows all actors (company, customer, partners) and interactions

---

## Facilitating Alignment Workshops

### Cross-Functional Workshop (2-3 hours)

**Objective**: Align teams around customer journey and opportunities

**Participants**: Product, marketing, sales, support, operations, leadership

**Agenda**:
1. **Context** (15 min): Current state research findings
2. **Journey Review** (30 min): Walk through the map stage by stage
3. **Pain Point Identification** (20 min): Where do customers struggle? Why?
4. **Opportunity Mapping** (20 min): Quick wins vs. structural improvements
5. **Prioritization** (15 min): What should we focus on first?
6. **Action Planning** (10 min): Who owns what? What's next?

**Outputs**:
- Aligned understanding of customer needs and experience gaps
- Prioritized list of improvements
- Assigned owners and next steps

---

## Measuring Impact

### Experience Metrics
- **NPS by journey stage**: Where is satisfaction lowest?
- **Time to first success/activation**: How fast do new users get value?
- **Feature adoption rate**: What features drive retention?
- **Churn rate by stage**: Where do customers leave?
- **Support ticket volume by stage**: When do customers struggle?

### Business Metrics
- **Retention rate**: Do improvements reduce churn?
- **Expansion revenue**: Do improvements drive upsells?
- **Customer satisfaction**: Do improvements increase NPS/CSAT?
- **Support efficiency**: Do improvements reduce support load?
- **Operational efficiency**: Do improvements reduce internal friction?

### Connecting Improvements to Outcomes
1. Identify a pain point (e.g., "30% cart abandonment in checkout")
2. Implement an improvement (e.g., "simplify checkout flow")
3. Measure the impact (e.g., "cart abandonment drops to 20%")
4. Calculate value (e.g., "10% improvement = $X additional revenue")

---

## Common Mistakes to Avoid

1. **Designing only the happy path**: Include edge cases, errors, and workarounds
2. **Ignoring internal systems**: The customer experience depends on backstage operations
3. **Making assumptions without research**: Talk to real customers, don't assume
4. **Over-complicating the map**: Keep it scannable; focus on key insights
5. **Treating journey as one-time artifact**: Update and evolve as the experience changes
6. **Ignoring segment variations**: Different customer types may have different journeys
7. **Not translating to action**: Insights are valuable only if they drive decisions
