---
type: reference
project: skills-library
scope: skill
plugin: consulting
parent: "[[sow-risk-advisor]]"
tags: [type/reference, plugin/consulting, scope/skill, topic/consulting]
status: active
---
# Scope Quantification Examples: From Vague to Defensible

## Digital Services & Content Migration

### Website Content Migration

**VAGUE (RISKY):**
"Migrate website content from current CMS to new platform."

**DEFENSIBLE:**
"Migrate up to 125 existing web pages from [current platform, version X] to [target platform, version Y], including:
- Preservation of URL structure and auto-redirects
- Metadata and SEO elements (title, meta description, keywords)
- Internal linking where source pages still exist
- Images and embedded media (up to 2,000 assets)

Excludes:
- Blog posts (manage separately)
- Archived content (pre-2020)
- Gated downloadables and form submissions
- Custom microsites
- Third-party embedded content (videos, widgets)
- 404 cleanup or backlink analysis

Timeline: 8 weeks from [start date], assuming client provides [content audit/current asset list] by [date]."

---

### User Documentation

**VAGUE:**
"Create user documentation for the system."

**DEFENSIBLE:**
"Create user documentation including:
- Administrator Guide: [15-20 pages] covering system configuration, user management, settings, backup/restore
- End-User Guide: [10-15 pages] covering core workflows, troubleshooting, FAQs
- Training Slides: [30-40 slides] aligned to training curriculum
- Quick Reference Card: [1-2 pages] laminated for field reference

Deliverables:
- Delivered in Word (.docx) with tracked changes for client review
- Covers [specific feature set]; assumes standard configuration
- Up to 2 revision rounds based on documented feedback
- Peer review with client SME; up to 4 hours SME time assumed

Does NOT include:
- Video production
- Custom illustrations or screenshots beyond UI captures
- Training delivery or trainer support
- Ongoing documentation maintenance"

---

## Design & UX

### Responsive Wireframes

**VAGUE:**
"Create wireframes for website redesign."

**DEFENSIBLE:**
"Create responsive wireframes for 12 agreed page templates, including:
- Desktop layout (1200px) with clear information hierarchy
- Tablet layout (768px) showing adaptive UI patterns
- Mobile layout (375px) with touch-friendly interactions
- Annotation of interactions, micro-interactions, and state changes
- Accessibility notes (WCAG 2.1 AA compliance review)

Deliverables:
- All wireframes in Figma with [Company] design system components applied
- Shared Figma file with view/comment access for stakeholder feedback
- Exported PDF for offline review if needed

Revision Process:
- Client review window: 5 business days per round
- Up to 3 consolidated revision rounds per template
- Revisions must address documented gaps vs. requirements; non-documented requests become change orders
- Timeline extends proportionally if client feedback windows are exceeded

Does NOT include:
- Visual/graphic design or color palettes
- Interaction prototyping or animated transitions
- Accessibility testing beyond visual review
- Implementation or hand-off to developers"

---

### Visual Design System

**VAGUE:**
"Build a design system."

**DEFENSIBLE:**
"Create a design system component library including:

Components (25-30 core components):
- Typography (3-4 heading levels, body text, captions)
- Buttons (primary, secondary, tertiary; states: default, hover, active, disabled)
- Forms (text input, select, checkbox, radio, toggle; with validation states)
- Cards (standard card, feature card with image variations)
- Navigation (top navigation, sidebar, breadcrumbs, pagination)
- Modals and Overlays (standard modal, alert, confirmation)
- Tables (sortable, with pagination, responsive behavior)
- Alerts and Notifications (success, warning, error, info)
- Loading States (skeleton screens, spinners, progress indicators)
- [Other domain-specific components]

Documentation:
- Component usage guide: 1 page per component, including: purpose, when to use, anatomy, props/variations, accessibility notes
- Figma design file with all components, variants, and design tokens applied
- Token documentation (colors, typography, spacing, shadows, etc.)
- Do's and Don'ts guidance for each component category

Scope:
- Assumes 1 standard device breakpoint system (mobile, tablet, desktop)
- Assumes [X] colors; unlimited colors become change order
- Assumes [X] font families; additional fonts become change order
- Assumes web platform only (iOS/Android design systems are separate)

Revision Process:
- Up to 2 revision rounds for full system
- Client review window: 7 business days per round
- Substantial additions or scope changes trigger change order

Does NOT include:
- Implementation code (component code is separate)
- Animation/interaction design
- Web accessibility testing (visual review only)
- Icon set creation (assumes existing icon library)"

---

## Workshops & Facilitation

### Discovery Workshop

**VAGUE:**
"Conduct discovery workshops to understand requirements."

**DEFENSIBLE:**
"Facilitate discovery workshops, including:

Sessions:
- Business Stakeholder session: [2 hours] with [up to 8 participants] covering business goals, success metrics, constraints, competitive landscape
- User Workflow session: [2 hours] with [5-6 power users] covering day-to-day workflows, pain points, workarounds
- Technical Architecture session: [2 hours] with [technical team] covering existing systems, integrations, technical constraints
- Executive Alignment session: [1 hour] for decision-maker sign-off on findings and next steps

Total: [7 hours] of facilitation time

Deliverables:
- Workshop facilitation notes (documented in real time)
- Consolidated findings summary: [8-10 page] report with key themes, gaps, and recommendations
- Presentation to stakeholders on key findings
- Recorded sessions (video only; transcription is out of scope)

Assumptions:
- Client provides space and A/V setup (or we facilitate virtually)
- Key participants identified in advance; confirmed availability
- Client consolidates internal feedback into single response
- Materials provided 5 business days before sessions (if client needs advance prep)

Revision Process:
- Findings document: up to 2 revision rounds based on stakeholder feedback
- Revisions based on documented inaccuracies; interpretation disagreements are addressed in consensus session

Does NOT include:
- Internal client workshop (stakeholder alignment); client responsibility
- Custom travel beyond [X miles] from [base location]
- Video transcription or professional transcription services
- Implementation of recommendations from workshops"

---

## Development & Technical

### API Documentation

**VAGUE:**
"Document the API."

**DEFENSIBLE:**
"Create comprehensive API documentation including:

Content:
- API Overview: Purpose, auth methods (OAuth 2.0, API key), rate limits, versioning strategy
- Authentication Guide: Setup, credential management, scope permissions
- Endpoint Reference: [20-25 core endpoints] documented with:
  - Description and use case
  - HTTP method, URL, and parameters
  - Request/response examples (JSON, XML)
  - Error codes and handling
  - Rate limits and performance considerations
  - Deprecation warnings (if applicable)
- Code Examples: Sample API calls in [Python, JavaScript, cURL]
- SDKs and Libraries: Links and basic usage examples for [officially supported SDKs]
- Troubleshooting Guide: [Common issues and solutions]
- Changelog: Version history and breaking changes

Deliverables:
- API documentation in [format: Swagger/OpenAPI, Postman, custom HTML]
- Interactive API explorer (if using Swagger/Postman)
- Versioned documentation (current version + 1 prior version supported)
- Searchable/indexed documentation

Scope:
- Covers API endpoints; assumes separate documentation for UI
- Assumes API is [mostly] stable; major changes during documentation scope become change orders
- Assumes access to API sandbox/test environment for validation

Revision Process:
- Client review window: 5 business days
- Up to 2 revision rounds for content accuracy
- Developer feedback loop: up to 2 hours of dev team time

Does NOT include:
- Endpoint implementation or feature development
- SDK development
- Integration testing beyond documentation verification
- Ongoing documentation maintenance (separate support contract)"

---

## Training & Enablement

### Instructor-Led Training

**VAGUE:**
"Provide training to end users."

**DEFENSIBLE:**
"Deliver instructor-led training program:

Sessions:
- [4 training sessions] of [2 hours] each, scheduled on [dates]
- Maximum [15 participants] per session
- Covers: [Core workflows], [Advanced features], [Reporting], [Administration] (select 4 topics)

Training Materials:
- Instructor guide: [15-20 pages] with lesson plans, timing, talking points, Q&A notes
- Participant workbook: [10-15 pages] with exercises, checklists, reference guides
- Slide deck: [40-50 slides] covering training topics
- Video recording: [2-3 minutes per section] for asynchronous learners
- Quick reference card: [1-page laminated] for on-the-job reference

Delivery:
- Live instruction (Zoom or in-person)
- Interactive exercises and hands-on practice
- Q&A and troubleshooting (up to 30 min per session)
- Attendance tracking and sign-in
- Recording available to participants for 6 months post-training

Scope:
- Assumes [system is stable] during training period
- Assumes [client provides access credentials] to training environment in advance
- Assumes [max 2 trainers]; additional instructors are out-of-scope
- Covers [core use cases]; advanced customizations are handled separately

Does NOT include:
- Ongoing coaching or support post-training
- Custom scenario development (uses standard examples)
- Train-the-trainer certification
- Ongoing refresher training or new-hire onboarding
- Video translation or closed captions"

---

## Testing & QA

### User Acceptance Testing (UAT)

**VAGUE:**
"Conduct comprehensive testing."

**DEFENSIBLE:**
"Conduct User Acceptance Testing (UAT) including:

Scope:
- Test cases for [core workflows]: [accounting for 30-40 total test cases]
- Covers: [happy paths and common edge cases]
- Assumes [standard configuration]; custom configurations are out-of-scope
- Tests [integration with Salesforce, Slack]; other integrations not included

Deliverables:
- Test plan: [5-8 page] document with test strategy, scope, timeline, success criteria
- Test cases: [30-40 documented cases] with step-by-step instructions, expected results, pass/fail criteria
- Test execution report: [2-3 page] summary with results, defects found, severity levels
- Defect log: Detailed log of [20-30 anticipated] defects with severity, priority, resolution status

Test Environment:
- Assumes [client provides UAT environment] with [production-like data volume]
- Assumes [client provides 3-4 power users] for UAT participation ([X] hours each)
- Testing conducted over [2-week] period

Revision Process:
- Defect fixes during UAT: Up to [X] hours of consulting support for test verification
- Major configuration changes during UAT extend timeline proportionally

Does NOT include:
- Performance testing or load testing
- Security testing
- Automated test script development
- Post-UAT support (handled in hypercare period)"

---

## Support & Hypercare

### Post-Launch Hypercare

**VAGUE:**
"Provide ongoing support for 30 days post-launch."

**DEFENSIBLE:**
"Provide 30-day post-launch hypercare support:

Coverage:
- Duration: [30 calendar days] from go-live date
- Hours: [8am-6pm ET, Mon-Fri], no weekend or holiday coverage
- Response time: [1 hour] for critical issues; [4 business hours] for non-critical
- Support types covered: Issue triage, workarounds, escalation to engineering
- Support channels: Email, Slack, or scheduled phone support

Scope:
- Addresses issues caused by [deployment process] or [identified defects]
- Covers [core workflows]; advanced features are out-of-scope
- Provides up to [40 hours] of consulting time (additional hours are T&M)

Escalation:
- Critical issues: Escalated to engineering team within 1 hour
- Non-critical issues: Escalated to engineering team within 1 business day
- Issue definition: Any blocker to normal operations (not workaround-able)

Deliverables:
- Daily status reports during first week
- Weekly summary reports weeks 2-4
- Hypercare close-out report with lessons learned and recommendations

Out of Scope:
- Changes to the system during hypercare
- Custom configuration or data cleanup
- Performance tuning or optimization
- Post-hypercare support (separate contract)"

---

## Key Quantification Principles

1. **Every deliverable gets a number**: pages, hours, items, rounds, templates, participants
2. **Every timeline gets a window**: "5 business days" not "promptly"
3. **Every revision limit gets a count**: "up to 3 rounds" not "design iterations"
4. **Every assumption gets a trigger**: "if client exceeds approval windows, timeline extends"
5. **Exclusions are explicit**: "Does NOT include: [specific list]"
6. **Client responsibilities are defined**: "[X] hours/week availability, approvals within Y days"
7. **Dependencies are documented**: "assumes access provided within 3 business days"
