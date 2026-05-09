---
type: reference
project: skills-library
scope: skill
plugin: consulting
parent: "[[sow-risk-advisor]]"
tags: [type/reference, plugin/consulting, scope/skill, topic/consulting]
status: active
---
# SOW Risk Review Framework

## Risk Categories & Detection

### 1. Vague or Unbounded Language (MARGIN EROSION RISK)

**What to Look For:**
- Words without defined limits: "comprehensive," "as needed," "ongoing," "unlimited," "all," "full"
- Undefined quantities: "design iterations," "revisions," "workshops," "support," "integrations"
- Subjective standards: "quality," "timely," "reasonable effort," "best practices," "industry standard"

**Impact:**
- Client expects more than scoped; expects unlimited revisions, workshops, support hours
- Delivery team depletes budget halfway through; project becomes unprofitable
- Change order disputes; customer argues work was "in scope"

**Red Flag Language:**
| High Risk | Medium Risk | Defensible |
|---|---|---|
| "Unlimited iterations" | "Design iterations" | "Up to three (3) consolidated revision cycles" |
| "Ongoing support" | "Post-launch support" | "30-day hypercare, limited to 40 hours" |
| "As needed" | "As required" | "Per agreed schedule; up to X hours/month" |
| "Comprehensive" | "Full solution" | "Solution per Section 2, limited to: [items]" |
| "All integrations" | "Integration support" | "Integration with [Platform A] and [Platform B]" |

**Mitigation:**
- Replace every vague term with a quantity: hours, days, deliverables, revision rounds, workshop counts
- Define "support" explicitly: hours/month, response times, categories covered
- Cap revision cycles: "up to 3 consolidated revision rounds"
- Define integration scope by system name: "integrations with Salesforce and Slack only"

---

### 2. Missing Assumptions & Dependencies (TIMELINE & DELIVERY RISK)

**What to Look For:**
- No "Assumptions" section, or assumptions are generic
- Scope depends on client actions but doesn't say so
- No mention of client responsibilities
- Timeline assumes instant decisions, approvals, access
- No mention of third-party dependencies or constraints

**Impact:**
- Client delays providing required inputs; project timeline slips
- Blame falls on consulting firm for "missing deadline"
- Client blames firm for not being "proactive" about delays
- Team burns budget waiting for approvals

**Critical Assumptions to Document:**
- **Client Availability**: "[X] hours/week stakeholder availability; [X] business days for approvals"
- **Access & Credentials**: "Client provides system access within [3] business days of engagement start"
- **Content/Assets**: "Client provides existing content in [format]; out of scope if not in specified format"
- **Decision-Making**: "Client has identified decision-maker; approval authority is clear"
- **Third-Party Dependencies**: "If [integration] requires vendor cooperation, client responsible for coordination"
- **Regulatory/Compliance**: "Client responsible for compliance review; consulting firm provides guidance only"

**Mitigation:**
- Build an explicit "Assumptions" section with client sign-off
- Make assumptions operationally testable: not "timely approvals" but "approvals within 5 business days"
- Tie assumptions to timeline: "If client exceeds approval windows, timeline extends proportionally"
- Document dependencies on third-party systems or integrations
- Clarify what's client responsibility vs. consulting firm responsibility

---

### 3. Unbounded Deliverables (SCOPE CREEP RISK)

**What to Look For:**
- Deliverables listed without quantities or definitions
- "Deliverables" section doesn't reference "Activities"
- No acceptance criteria defined
- Deliverable format or medium not specified

**Impact:**
- Client expects 50 pages of documentation; firm delivers 20 and calls it done
- Design deliverables lack clear acceptance criteria; revision disputes
- Format/platform confusion: client expects Figma; firm delivers PowerPoint

**Mitigation Example:**

| Risky | Defensible |
|---|---|
| "Design mockups" | "Responsive mockups for 12 agreed page templates, delivered in Figma, covering desktop/tablet/mobile breakpoints, with accessibility review (WCAG 2.1 AA), up to 3 revision cycles based on documented client feedback" |
| "Project documentation" | "Project documentation including: stakeholder interview summary (5 pages), process flow diagrams (3 diagrams), requirements matrix, risk log, and closeout report, delivered as Word document with tracked changes" |
| "Training materials" | "Training curriculum for [topic], including: instructor guide (15-20 pages), participant workbook (10-15 pages), slide deck (40-50 slides), recorded video (1-2 minutes per section), delivered in PowerPoint and video format" |

---

### 4. Misalignment: Scope vs. Effort vs. Fees (MARGIN RISK)

**What to Look For:**
- Fixed-fee price with undefined scope
- Timeline seems compressed relative to scope
- Team availability seems optimistic
- Scope creep is baked into price but not acknowledged

**Impact:**
- Project exceeds estimated effort; becomes unprofitable
- Team burns out trying to meet timeline
- Delivery quality suffers
- Customer disputes arise over "promised but undelivered"

**Sanity Checks:**
- Effort estimation: "Does the team have capacity for X hours of work on this timeline?"
- Scope: "Can we realistically deliver all deliverables in the proposed timeline?"
- Fee alignment: "Is the fixed fee sufficient to cover estimated effort + margin?"
- Risk buffer: "Have we built in contingency for unknowns?"

**Red Flags:**
- Timeline seems aggressive relative to scope
- Scope boundary is vague; feels unbounded
- Fixed fee with T&M-like scope
- No mention of how out-of-scope work is handled

---

### 5. Acceptance & Approval Language (DISPUTE RISK)

**What to Look For:**
- No definition of "acceptance"
- Acceptance is subjective (client satisfaction, "quality", "meeting expectations")
- No timeline for acceptance review
- No deemed-accepted clause if client doesn't respond
- Indefinite revision cycles after "acceptance"

**Impact:**
- Client withholds acceptance indefinitely, blocking payment
- Post-delivery revisions requested but not treated as out-of-scope
- Disputes about whether deliverables meet "quality" standards

**Better Approach:**
- Define acceptance criteria explicitly: functional requirements checklist, format, completeness
- Set review window: "Client has 5 business days to review and provide feedback"
- Deemed acceptance: "If no feedback received within 5 days, deliverable deemed accepted"
- Distinguish between acceptance revisions and post-acceptance support
- Make post-acceptance support a separate line item or follow-up SOW

**Example:**

```
ACCEPTANCE CRITERIA & PROCESS

Deliverable: UX Wireframes

Acceptance Criteria:
- Covers agreed page inventory [list specific pages]
- Delivered in Figma format with agreed design system components
- Includes accessibility annotations per WCAG 2.1 AA
- Includes notes on interaction flows and state changes
- Includes responsive design coverage (desktop, tablet, mobile)

Review Process:
- Client has 5 business days from delivery to provide consolidated feedback
- Up to 3 revision cycles are included per wireframe template
- Revisions must be requested within the review window; post-acceptance changes require change order
- Delivery timeline extends proportionally if client exceeds review windows

Deemed Acceptance:
- If no feedback received within 5 business days, deliverable deemed accepted
- Acceptance unblocks payment milestone
```

---

### 6. Change Management Language (SCOPE CONTROL RISK)

**What to Look For:**
- No mention of change order process
- No out-of-scope definition
- No mechanism for pricing out-of-scope work
- Scope changes treated informally (not documented, not priced)

**Impact:**
- Scope creep becomes normal; no way to track or charge for extras
- Team depletes budget serving scope that wasn't priced
- Disputes about what was "included" vs. "out of scope"
- Customer resents being charged for changes they thought were included

**Better Approach:**
- Explicit "Out of Scope" section listing what's NOT included
- Formal change order process triggered when scope changes
- Clear change order content: description, effort estimate, fee, timeline impact
- Named approval authority for change orders (who signs off on extra scope?)
- Billing rate for change orders: T&M or per-deliverable

**Example:**

```
OUT OF SCOPE

The following are explicitly out of scope unless modified by written change order:

- A/B testing, optimization, or performance tuning
- Third-party system integration (except Salesforce and Slack as described in Scope section)
- Regulatory compliance review or legal analysis
- Data migration or historical data cleansing
- Ongoing support beyond the 30-day hypercare period
- Custom training delivery (recorded training only)
- Photography, videography, or custom illustration

CHANGE ORDER PROCESS

If client requests work outside the defined scope:

1. Request is submitted in writing to [Project Manager]
2. Consulting firm provides written estimate: scope, effort hours, fee, timeline impact
3. Client reviews and approves in writing before work begins
4. Change order is executed; work begins only after written approval
5. Change order fees are billed separately from base SOW
6. Timeline extension is documented if change order impacts deliverable dates
```

---

## SOW Review Checklist

### Structure & Completeness
- [ ] All standard SOW sections present: background, scope, activities, deliverables, assumptions, timeline, acceptance, change management, fees, client responsibilities, out-of-scope
- [ ] Sections are organized logically and internally consistent
- [ ] No contradictions between sections (e.g., scope says X, but activities assume Y)
- [ ] Clear cross-references where sections depend on each other

### Scope & Deliverables
- [ ] Every activity has a corresponding deliverable
- [ ] Every deliverable has explicit acceptance criteria
- [ ] All deliverables are quantified (number of pages, hours, items, rounds)
- [ ] Deliverable format/platform is specified
- [ ] No undefined or vague deliverables ("design," "documentation," "consulting")
- [ ] Scope boundaries are clear; out-of-scope items are listed

### Assumptions & Dependencies
- [ ] Explicit assumptions section with client sign-off
- [ ] Client responsibilities clearly stated (availability, approvals, resource provision)
- [ ] Timeline dependencies documented (e.g., "timeline assumes client provides access within 3 business days")
- [ ] Third-party dependencies and constraints noted
- [ ] Assumptions are operationally testable (not "timely approvals" but "within 5 business days")

### Timeline & Milestones
- [ ] Milestones tied to deliverable completion (not just calendar dates)
- [ ] Timeline accounts for client approval windows
- [ ] Timeline is realistic for defined scope
- [ ] Dependencies between phases are documented
- [ ] Risk timeline extensions (if client exceeds approval windows) are noted

### Acceptance & Approvals
- [ ] Acceptance criteria are explicit (not subjective)
- [ ] Review window is defined (e.g., 5 business days)
- [ ] Deemed-accepted clause included (if no feedback, considered accepted)
- [ ] Approval authority is named (who signs off?)
- [ ] Revision limits are clear (up to X rounds, then change order)

### Change Management
- [ ] "Out of Scope" section explicitly lists what's excluded
- [ ] Change order process is documented (how requests are made, reviewed, priced)
- [ ] Billing rate for change orders is specified
- [ ] Timeline impact process is clear (how extensions are calculated)
- [ ] Named authority for approving change orders

### Language & Risk Control
- [ ] No vague language ("comprehensive," "as needed," "unlimited," "ongoing")
- [ ] All quantities are specified (hours, days, deliverables, revision rounds)
- [ ] No absolute promises ("guarantee," "ensure," "promise")
- [ ] Defensible language throughout ("will use reasonable efforts," "subject to dependencies")
- [ ] Tone is professional and clear

### Commercial Alignment
- [ ] Effort estimate supports fixed fee (if fixed-fee)
- [ ] Timeline is achievable with proposed team
- [ ] Scope and fee are aligned (no obvious underpricing)
- [ ] Payment milestones make business sense
- [ ] Risk allocation is clear (who bears risk of delays, changes, etc.)

### Legal Review
- [ ] SOW aligns with governing Master Services Agreement
- [ ] Language is consistent with company's standard legal reviews
- [ ] Liability language is appropriate
- [ ] IP ownership and confidentiality are clear (or deferred to MSA)
- [ ] Force majeure and dispute resolution reference MSA

### Sign-Off
- [ ] All parties (client, project manager, delivery lead, legal) have reviewed
- [ ] Final SOW matches version signed by client
- [ ] Delivery team has aligned on commitments
- [ ] Risk mitigations are understood by team
