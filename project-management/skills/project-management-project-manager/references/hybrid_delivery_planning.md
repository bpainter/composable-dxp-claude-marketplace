---
type: reference
project: skills-library
scope: skill
plugin: consulting
parent: "[[project-manager]]"
tags: [type/reference, plugin/consulting, scope/skill, topic/consulting]
status: active
---
# Hybrid Delivery Planning: Blending Waterfall & Agile

## When to Use Which Methodology

### Waterfall is Better For:
- **Well-understood scope**: Requirements are clear and stable
- **High regulatory/compliance requirements**: Need detailed documentation and approval gates
- **Fixed-price contracts**: Scope and timeline locked upfront
- **Hardware/infrastructure**: Physical constraints require upfront planning
- **Large team coordination**: Need detailed planning to coordinate across many people

**Example**: CMS migration, infrastructure overhaul, regulatory compliance project

### Agile is Better For:
- **Uncertain requirements**: Customer will learn and change their mind
- **Rapid feedback loops**: Build, learn, adjust cycle is valuable
- **Technology exploration**: Using new tools/frameworks where approach unclear
- **Feature refinement**: Need customer feedback to shape product
- **Cross-functional collaboration**: Design, dev, product need daily interaction

**Example**: Product features, UI/UX redesign, new capability exploration

### Hybrid Approach:
- **Waterfall for planning phases**: Clear discovery, design, architecture decisions
- **Agile for execution**: Sprints for design and development
- **Waterfall for integration & launch**: Structured final testing and go-live

## Hybrid Delivery Model: Real Example

### Project: E-Commerce Platform Redesign (12 months, $2M budget)

**PHASE 1: DISCOVERY & STRATEGY (Weeks 1-6) — Waterfall**
- Structured research, interviews, competitor analysis
- Deliverable: Requirements doc, wireframes, technical architecture
- Timeline: Fixed, 6 weeks
- Gate review: Stakeholder sign-off before proceeding to build

**PHASE 2: DESIGN & BUILD (Weeks 7-20) — Agile**
- 2-week sprints: Design high-fidelity mockups + front-end build
- Daily standups, sprint reviews with stakeholders
- Feedback loops: Refine design based on developer/QA feedback
- Parallel: Back-end API development (separate Waterfall track with detailed spec)

**PHASE 3: INTEGRATION & QA (Weeks 21-26) — Waterfall**
- Integration testing: Front-end + back-end
- UAT with stakeholders (formal acceptance)
- Performance and security testing
- Gate review: Production readiness assessment

**PHASE 4: GO-LIVE (Weeks 27-28) — Phased**
- Week 27: Phased launch (10% → 50% → 100% traffic)
- Week 28: Post-launch monitoring and optimization sprints

## Detailed Hybrid Timeline

```
HYBRID DELIVERY ROADMAP: E-Commerce Platform

PHASE 1: DISCOVERY & STRATEGY (6 weeks)
├─ Week 1:
│  ├─ Kickoff: Define project goals, constraints, success metrics
│  ├─ Stakeholder interviews (C-suite, marketing, ops, IT)
│  └─ Competitive analysis: Audit competitor platforms (3-5 companies)
├─ Week 2:
│  ├─ Customer research: Interviews with 15+ high-value customers
│  ├─ Analytics audit: Understand current user behavior/bottlenecks
│  └─ Technology assessment: Evaluate tech stack options
├─ Week 3:
│  ├─ Design workshops: IA, wireframes, user flows
│  ├─ Requirements documentation: Detailed feature list
│  └─ Technical architecture: Platform, APIs, data model design
├─ Week 4-5:
│  ├─ High-fidelity mockups: 3-5 key pages (hero, PDP, checkout)
│  ├─ Prototype: Clickable prototype for stakeholder feedback
│  └─ Design system foundation: Color, typography, spacing
├─ Week 6:
│  ├─ Executive presentation: Findings and recommendations
│  ├─ Stakeholder review: Final approval on direction
│  └─ Gate Decision: GO to Phase 2 (design & build)
│
DELIVERABLES:
├─ Requirements document (20-30 pages)
├─ Wireframes (30-40 pages)
├─ High-fidelity mockups (Figma file with all key pages)
├─ Technical architecture diagram and specification
├─ Prototype (interactive Figma or Webflow)
├─ Project plan and resource allocation
└─ Risk register and mitigation strategies

PHASE 2: DESIGN & BUILD (14 weeks) — AGILE SPRINTS
├─ Pre-Sprint Setup:
│  ├─ Backlog refinement: Epic → Feature → Story breakdown
│  ├─ Capacity planning: Team allocation (5 designers, 7 dev, 2 QA)
│  ├─ Velocity baseline: Historical estimates or assumption (30 points/sprint)
│  └─ Infrastructure: Dev environments, CI/CD pipelines, monitoring
│
├─ SPRINT 1-2 (Weeks 7-10): Design System & Components
│  ├─ Design track: Finalize visual design, build component library
│  ├─ Dev track: Setup infrastructure, Storybook, testing frameworks
│  ├─ Daily standups: 15 min sync
│  ├─ Weekly demos: Show what's built to stakeholders
│  └─ Sprint review & retro: What did we learn? What's next?
│
├─ SPRINT 3-6 (Weeks 11-18): Front-End & Back-End Build
│  ├─ Front-End:
│  │  ├─ Product catalog pages (listing, filtering, detail)
│  │  ├─ Shopping cart and checkout flow
│  │  └─ User accounts and order history
│  ├─ Back-End (parallel):
│  │  ├─ Product API with search (Elasticsearch)
│  │  ├─ Cart and order services
│  │  └─ User authentication and profiles
│  ├─ QA: Continuous testing, daily build validation
│  └─ Stakeholder feedback: Weekly demos, sprint refinement
│
├─ SPRINT 7 (Weeks 19-20): Content Migration & Polish
│  ├─ Migrate product data from legacy system
│  ├─ Migrate editorial content to CMS
│  ├─ Bug fixes and UI polish
│  └─ Performance optimization (bundle size, API response times)
│
CADENCE:
├─ Daily: 15-min standup (status, blockers, daily goals)
├─ Twice weekly: Design / dev sync (dependencies, coordination)
├─ Weekly: Demo and stakeholder review (30 min)
├─ Weekly: Retro + backlog planning (90 min)
└─ Bi-weekly: Executive update (status, forecast, decisions)

PHASE 3: INTEGRATION & QA (6 weeks) — WATERFALL
├─ Week 21:
│  ├─ System integration: Connect front-end to back-end APIs
│  ├─ Smoke tests: Basic workflows functional
│  └─ Functional testing: Detailed test against requirements
├─ Week 22:
│  ├─ UAT setup: Prepare test data and scenarios
│  ├─ Internal UAT: Marketing, ops, customer service test
│  └─ Customer beta: 100 selected customers test platform
├─ Week 23:
│  ├─ Performance testing: Load test (1000 concurrent users)
│  ├─ Security audit: Penetration testing, data protection
│  └─ Accessibility testing: WCAG 2.1 AA compliance audit
├─ Week 24:
│  ├─ Bug triage: High priority fixes immediately
│  ├─ UAT sign-off: Customer sign-off on functionality
│  └─ Go-live readiness review: Runbooks, support prep
├─ Week 25-26:
│  ├─ Final testing: Edge cases, integration scenarios
│  ├─ Monitoring setup: Alerting, dashboards, runbooks
│  └─ Team training: Ops, support, customer service briefing
│
DELIVERABLES:
├─ Test plan and test cases (comprehensive)
├─ Bug log and resolution tracking
├─ UAT sign-off documentation
├─ Performance test results
├─ Security audit report
├─ Operations runbooks (incident response, rollback)
├─ Support documentation (FAQs, troubleshooting)
└─ Go-live checklist

PHASE 4: GO-LIVE & LAUNCH (2 weeks + ongoing)
├─ Week 27: Phased Launch
│  ├─ 00:00 - Phase 1 (10% of traffic → new platform)
│  ├─ 04:00 - Monitoring check: Error rates, performance normal?
│  ├─ 12:00 - Phase 2 (50% of traffic)
│  ├─ 20:00 - Monitoring check: Major issues? Customer feedback?
│  └─ [If critical issue: ROLLBACK to 100% old platform]
│
├─ Week 27-28: Final Ramp & Stabilization
│  ├─ 24 hours: Phase 3 (100% to new platform)
│  ├─ 48 hours: Post-launch monitoring (critical bug fixes)
│  ├─ 1 week: Performance optimization, first round of feedback
│  └─ 2 weeks: Stabilization, prepare for Phase 2 features
│
├─ Week 28+: Post-Launch Support
│  ├─ Daily incident review (critical bugs)
│  ├─ Weekly: Customer feedback triage
│  ├─ Bi-weekly: Quick wins and fixes (1-week sprints)
│  └─ Monthly: Lessons learned, Phase 2 planning

DELIVERABLES:
├─ Launch plan (minute-by-minute runbook)
├─ Monitoring dashboards (performance, errors, customer experience)
├─ Incident response playbook
├─ Post-launch communication plan
└─ Phase 2 roadmap (features for next 6 months)
```

## Managing Risk in Hybrid Delivery

### Critical Path Analysis

In the example above:
1. **Critical Path**: Back-end API design (Week 3) → Back-end build (Weeks 11-18) → Integration (Week 21) → Go-live (Week 27)
2. **Slack**: Front-end design can slip 1-2 weeks without impacting timeline
3. **Mitigation**: Any delay in back-end must be immediately escalated

### Risk Management During Execution

| Phase | Key Risks | Mitigation |
|-------|-----------|-----------|
| **Discovery** | Requirements incomplete; stakeholders misaligned | Weekly steering reviews; written requirements approval |
| **Design & Build** | Scope creep; skill gaps on new technology | Strict change control; pair programming; external expert review |
| **Integration** | API compatibility issues; performance problems | Early integration testing (Sprint 6); load testing early |
| **Go-live** | Major bugs in production; traffic spike handling | Phased rollout (10% → 50% → 100%); extensive monitoring |
| **Post-launch** | Customer dissatisfaction; unexpected issues | 24/7 on-call team; rapid incident response team |

### Gate Decisions

**Gate 1: End of Discovery (Week 6)**
- ✓ Requirements clear and stakeholder-approved?
- ✓ Architecture approach validated?
- ✓ Team and budget committed?
- **Decision**: Proceed to design & build OR revisit discovery

**Gate 2: End of Sprint 2 (Week 10)**
- ✓ Velocity established (can we maintain pace)?
- ✓ Infrastructure and tools working?
- ✓ Design system and components ready for handoff?
- **Decision**: Continue sprints OR adjust approach

**Gate 3: End of Phase 2 (Week 20)**
- ✓ All features built and coded?
- ✓ Unit tests passing, >80% coverage?
- ✓ No critical bugs in code review?
- **Decision**: Proceed to integration OR extend build phase

**Gate 4: End of Phase 3 (Week 26)**
- ✓ UAT passed and stakeholders approved?
- ✓ Performance and security audit passed?
- ✓ Team trained and runbooks reviewed?
- **Decision**: GO for launch OR delay until ready

## Communication Cadence in Hybrid Model

| Meeting | Frequency | Audience | Purpose |
|---------|-----------|----------|---------|
| **Daily Standup** | 15 min | Dev/Design team | Status, blockers, goals |
| **Design/Dev Sync** | 2x/week | Design, dev leads | Dependencies, technical decisions |
| **Sprint Demo** | Weekly | Stakeholders, product | Show working software |
| **Sprint Retro** | Weekly | Dev/Design team | What went well? What to improve? |
| **Backlog Refinement** | Weekly | Product, Dev leads | Upcoming stories, estimates |
| **Executive Update** | Bi-weekly | Leadership, sponsor | Status, budget, timeline, risks |
| **Phase Gate Review** | End of phase | Steering committee | Gate decision: proceed/pivot/kill |

## Tools for Hybrid Delivery

**Waterfall & Planning:**
- Jira (Gantt, roadmaps, dependencies)
- Microsoft Project
- Smartsheet

**Agile Sprints:**
- Jira (Scrum board, backlog)
- Azure DevOps
- Linear
- Trello

**Communication & Collaboration:**
- Slack (daily standups, decisions)
- Figma (design handoff)
- GitHub/GitLab (code review, CI/CD)
- Confluence (documentation, runbooks)

**Monitoring & Testing:**
- Datadog (monitoring, alerts)
- Cypress (automated testing)
- LoadImpact (performance testing)
- Sentry (error tracking)
