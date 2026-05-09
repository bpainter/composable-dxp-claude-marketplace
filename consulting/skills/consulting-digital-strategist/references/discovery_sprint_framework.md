---
type: reference
project: skills-library
scope: skill
plugin: consulting
parent: "[[digital-strategist]]"
tags: [type/reference, plugin/consulting, scope/skill, topic/consulting]
status: active
---
# Discovery Sprint: Validating High-Risk Assumptions

## Purpose

A Discovery Sprint is a 1-2 week intensive focused research effort designed to test riskiest assumptions about market, customer, or business model *before* investing in full product development.

Goal: Move from assumption/speculation → validated learning → go/no-go decision

## Pre-Sprint Preparation (1 week before)

### 1. Define the Core Question
What's the biggest uncertainty driving your strategy?

**Strong questions:**
- "Do target customers actually experience this problem?" (Problem validation)
- "Would they pay for this solution?" (Pricing validation)
- "Can we build this capability?" (Technical validation)
- "Does this new market segment exist and buy?" (Market validation)

**Weak questions:**
- "Do customers like our product?" (Too vague)
- "How should we design the UI?" (Implementation, not validation)
- "Is this a good idea?" (Too broad)

### 2. Map Your Assumptions
List 5-8 critical assumptions for this initiative:

**Assumption Canvas Template:**

| Category | Assumption | Confidence | Criticality | Risk if Wrong |
|----------|-----------|-----------|------------|---|
| **Customer** | Target customer has time mgmt pain | 70% | High | No customer, no market |
| **Problem** | Current tools are too complex | 60% | High | Customer doesn't care about simplicity |
| **Solution** | Mobile-first experience solves problem | 50% | High | Our solution doesn't differentiate |
| **Business** | Customers will pay $50/mo for this | 40% | Critical | Business model doesn't work |
| **Market** | TAM is $500M+ | 30% | Medium | Market too small to scale |

### 3. Prioritize by Impact & Uncertainty
Test high-uncertainty, high-criticality assumptions first.

**Prioritization Matrix:**
- High uncertainty + High criticality = Test first (most important learning)
- High uncertainty + Low criticality = Test last or skip
- Low uncertainty + High criticality = Monitoring assumptions (validate through ongoing metrics)
- Low uncertainty + Low criticality = No need to test

### 4. Research Plan
Design your week around testing riskiest assumptions:

```
DISCOVERY SPRINT PLAN

Riskiest Assumption: Target customer experiences significant time-management pain
Confidence: 50% | Criticality: High | Expected Learning: Do customers confirm problem?

Research Approach:
- 20 user interviews (target: early-adopter type customer)
- Ethnographic observation: shadow 2-3 customers doing time mgmt work
- Competitive feature audit: analyze 5 competitor solutions
- Prototype/concept testing: show rough wireframe to 5 customers

Success Criteria: 15+ customers confirm problem is significant; 8+ indicate they'd consider alternative solution
Failure Gate: <10 customers confirm problem OR they don't see urgency

Timeline:
- Monday: Planning, interview recruitment, design interview guide
- Tues-Thurs: Customer interviews and observation
- Friday: Synthesis, data analysis, recommendation
```

## Sprint Week Execution

### Monday: Problem Framing & Research Design

**Morning: Core Team Planning (2 hours)**
- Define the core question and success criteria
- Review your 5-8 assumptions; prioritize by impact
- Agree on what "go" looks like (we'll advance to Alpha if we learn X)
- Agree on what "no-go" looks like (we'll pivot/kill if we learn Y)

**Afternoon: Research Tool Setup**
- Draft interview guide (10-12 open-ended questions)
- Design prototype/concept (wireframe, mockup, or role-play script)
- Plan competitive analysis (tool list, feature audit template)
- Schedule customer interviews (target 15-20 for problem validation)

**Customer Interview Guide Template:**
- Warm-up: Tell me about your role and biggest current challenge
- Problem: Walk me through the last time you faced [problem]. What happened?
- Current solution: How do you currently address this? What works? What frustrates you?
- Trigger: When is this most painful? What would need to change for you to try something new?
- Willingness: If there was a better solution, would you consider it? Why/why not?
- Close: Who else should I talk to?

### Tuesday-Thursday: Customer Research Execution

**Concurrent tracks:**
1. **Problem Validation Interviews** (15-20 customers)
   - Target: Early adopters, power users, passionate complainers
   - Goal: Do they confirm problem? How acute is it? How are they solving it today?

2. **Competitive Analysis**
   - Map existing solutions (direct competitors, adjacent categories, workarounds)
   - Feature audit: How do they solve the problem? What gaps exist?
   - Pricing research: How are they monetizing this?

3. **Concept Testing** (5-8 customers)
   - Show rough prototype or describe solution concept
   - Gauge reaction: Is this compelling? Better than current solution?
   - Pricing feedback: How much would you pay?

**Daily Synthesis (30 min end-of-day)**
- Highlight quotes and patterns from interviews
- Update assumptions tracker: which are being confirmed? Which invalidated?
- Identify follow-up questions for next day's interviews
- Note emerging insights

### Friday: Synthesis & Decision

**Morning: Data Synthesis (3-4 hours)**

Organize findings by assumption:

```
PROBLEM VALIDATION FINDINGS

Assumption: "Target customers experience acute time-management pain"

Evidence For:
- 14/18 interviewees confirmed problem exists
- Phrases: "I lose hours every week searching", "constant context-switching"
- Trigger: New tools/team size expansion makes problem acute

Evidence Against:
- 4/18 described workaround that works "fine for now"
- Price sensitivity: 60% said "wouldn't pay more than $15/mo"
- Adoption friction: "Too busy to learn new tool"

Confidence Lift: 50% → 75% (problem is real for segment)
Gate Status: PASS - Sufficient evidence of problem
Next: Test solution concept
```

**Afternoon: Leadership Debrief (90 min)**

Present findings and recommendation:

**GO Criteria** (advance to Alpha):
- 15+ customers confirmed problem validity
- 8+ would consider alternative solution
- TAM estimate appears viable
- Solution concept received 4+/5 interest rating

**PIVOT Criteria** (change direction):
- Problem is different from hypothesis (but still valid)
- Solution needs major rethink
- Customer segment shifted based on learning

**KILL Criteria** (exit initiative):
- <10 customers confirmed problem
- Customers don't feel urgency to solve
- Better solutions already exist

**Decision:** Go / Pivot / Kill
**Next Phase:** If GO, move to Alpha (build MVP); commit resources and timeline
**Learning Captured:** Update business model canvas, roadmap, and strategy

## Common Discovery Sprint Scenarios

### Scenario 1: Problem Validation
"Does this customer problem actually exist and is it significant?"
- Primary: Customer interviews (20x)
- Secondary: Usage data from competitors, market research reports
- Success gate: 15+ customers confirm problem; 8+ express willingness to try alternative

### Scenario 2: Solution Validation
"Do customers think our proposed solution addresses their problem?"
- Primary: Prototype/concept testing (8-10 customers)
- Secondary: Competitive feature comparison
- Success gate: 6+/10 customers rate concept 4+/5 for appeal; 5+ indicate purchase intent

### Scenario 3: Market & TAM Validation
"Is the addressable market large enough to build a viable business?"
- Primary: Customer interviews + market research data
- Secondary: Pricing research, segment willingness-to-pay
- Success gate: TAM estimate >$100M (or your threshold); 40%+ adoption potential in segment

### Scenario 4: Business Model Validation
"Will customers pay and in what packaging/pricing?"
- Primary: Pricing research interviews (15x), willingness-to-pay testing
- Secondary: Pricing comparison, competitive analysis
- Success gate: 60%+ would consider paying; Average WTP within target margin

### Scenario 5: Go-to-Market Validation
"Can we acquire customers cost-effectively in target segment?"
- Primary: Channel assessment (where do target customers gather?), sales/marketing research
- Secondary: Competitor GTM audit, distribution channel analysis
- Success gate: Clear, cost-effective acquisition path identified; CAC model appears viable

## Post-Sprint: Roadmap Implications

**If GO:**
- Commit to Alpha phase (6-12 weeks, MVP build)
- Update roadmap and OKRs based on learning
- Reframe riskiest assumptions for next phase

**If PIVOT:**
- Refine problem/solution based on learning
- Design new discovery sprint for refined approach
- Update business model canvas and positioning

**If KILL:**
- Document learnings and insights for future reference
- Reallocate resources to higher-confidence opportunities
- Celebrate learning vs. blame failure

## Discovery Sprint Tools & Templates

**Tools:**
- Interview guide template (Google Doc)
- Assumption canvas (spreadsheet)
- Competitive analysis template
- Concept testing scorecard
- Data synthesis worksheet
- Decision memo template

**Outputs:**
- Interview notes and quotes database
- Assumption validation tracker
- Competitive analysis summary
- Decision memo and recommendation
- Updated business model canvas
- Revised roadmap and next-phase plan
