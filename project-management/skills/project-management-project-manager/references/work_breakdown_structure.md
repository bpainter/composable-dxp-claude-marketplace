---
type: reference
project: skills-library
scope: skill
plugin: consulting
parent: "[[project-manager]]"
tags: [type/reference, plugin/consulting, scope/skill, topic/consulting]
status: active
---
# Work Breakdown Structure (WBS) & Story Definition Guide

## WBS Hierarchy: From Vision to Tasks

### Level 1: Program / Project
The overall initiative (12-24 months, $1M+)
- Example: "E-Commerce Platform Redesign"

### Level 2: Workstream / Epic (3-6 months)
Major deliverable areas spanning multiple features
- Example: "Front-End Architecture & Components"
- Example: "Content Migration from Legacy CMS"
- Owned by workstream lead

### Level 3: Feature (2-4 weeks)
Shippable capability delivering customer value
- Example: "Product Page Redesign & Launch"
- Example: "Search Functionality & Filtering"
- Owned by product lead or feature owner

### Level 4: User Story (3-5 days)
Discrete piece of work for a developer or designer
- Example: "Implement product listing component with filtering"
- Owned by engineer or designer

### Level 5: Task (4-8 hours)
Specific subtask or implementation detail
- Example: "Build Elasticsearch product index"
- Assignable to individual contributor

## Sample WBS: Digital Platform Redesign

```
DIGITAL PLATFORM REDESIGN (12 months, $2M)
│
├─ EPIC 1: DISCOVERY & STRATEGY (6 weeks)
│  ├─ Feature: Customer Research & Insights
│  │  ├─ Story: Conduct user interviews (15 users)
│  │  ├─ Story: Analyze competitive landscape
│  │  └─ Story: Create user personas and journey maps
│  ├─ Feature: Information Architecture & Wireframes
│  │  ├─ Story: Map current user journeys
│  │  ├─ Story: Create proposed IA and wireframes
│  │  └─ Story: Validate with stakeholders
│  └─ Feature: Technical Architecture & Feasibility
│     ├─ Story: Evaluate technology options (Next.js vs. Remix)
│     ├─ Story: Design API and data model
│     └─ Story: Develop proof-of-concept for critical features
│
├─ EPIC 2: DESIGN SYSTEM & COMPONENTS (8 weeks, parallel)
│  ├─ Feature: Visual Design & Brand Guidelines
│  │  ├─ Story: Create high-fidelity mockups (hero, PDP, checkout)
│  │  ├─ Story: Define color, typography, spacing system
│  │  └─ Story: Document design system in Figma
│  ├─ Feature: Component Library Development
│  │  ├─ Story: Build button, input, modal, card components
│  │  ├─ Story: Build layout and grid components
│  │  └─ Story: Create Storybook documentation
│  └─ Feature: Accessibility & Responsive Design
│     ├─ Story: Implement WCAG 2.1 AA compliance
│     ├─ Story: Design for mobile, tablet, desktop
│     └─ Story: Test with accessibility tools and users
│
├─ EPIC 3: FRONT-END BUILD (14 weeks)
│  ├─ Feature: Product Catalog Pages
│  │  ├─ Story: Build product listing with filtering
│  │  │  └─ Task: Create search/filter state management
│  │  │  └─ Task: Build filter UI components
│  │  │  └─ Task: Integrate with product API
│  │  │  └─ Task: Implement pagination
│  │  ├─ Story: Build product detail page
│  │  │  └─ Task: Fetch product data from API
│  │  │  └─ Task: Build image gallery component
│  │  │  └─ Task: Add to-cart functionality
│  │  └─ Story: Build related products / recommendations
│  ├─ Feature: Shopping Cart & Checkout
│  │  ├─ Story: Build shopping cart UI
│  │  │  └─ Task: Manage cart state (add, remove, update)
│  │  │  └─ Task: Calculate cart totals and taxes
│  │  │  └─ Task: Persist cart to browser storage
│  │  ├─ Story: Build checkout flow (1-click, express, full)
│  │  └─ Story: Build order confirmation page
│  └─ Feature: User Accounts & Orders
│     ├─ Story: Build login/signup flows
│     ├─ Story: Build user dashboard & order history
│     └─ Story: Build password reset & account settings
│
├─ EPIC 4: BACK-END & API (12 weeks, parallel)
│  ├─ Feature: Product API & Search
│  │  ├─ Story: Design and build product API (REST/GraphQL)
│  │  ├─ Story: Build Elasticsearch product index
│  │  └─ Story: Implement search filters and facets
│  ├─ Feature: Cart & Order Service
│  │  ├─ Story: Build shopping cart service
│  │  ├─ Story: Build order service and persistence
│  │  └─ Story: Integrate with payment processor
│  └─ Feature: User Service & Authentication
│     ├─ Story: Build user authentication (JWT)
│     ├─ Story: Build user profile service
│     └─ Story: Implement session management
│
├─ EPIC 5: CONTENT MIGRATION (3 weeks)
│  ├─ Feature: Migrate Product Data
│  │  ├─ Story: Audit legacy product data
│  │  ├─ Story: Build migration scripts
│  │  ├─ Story: Test migration with sample data
│  │  └─ Story: Execute full production migration
│  └─ Feature: Migrate Editorial Content
│     ├─ Story: Audit legacy content
│     ├─ Story: Import to Contentful CMS
│     └─ Story: Validate and publish content
│
├─ EPIC 6: INTEGRATION & QA (4 weeks)
│  ├─ Feature: System Integration Testing
│  │  ├─ Story: End-to-end user journey testing
│  │  ├─ Story: API integration testing
│  │  └─ Story: Performance and load testing
│  ├─ Feature: UAT & Stakeholder Testing
│  │  ├─ Story: Conduct internal UAT
│  │  └─ Story: Conduct customer beta testing (100 users)
│  └─ Feature: Security & Compliance
│     ├─ Story: Security audit and penetration testing
│     ├─ Story: Implement SSL/TLS and data encryption
│     └─ Story: Verify GDPR and privacy compliance
│
└─ EPIC 7: GO-LIVE & LAUNCH (2 weeks)
   ├─ Feature: Pre-Launch Setup
   │  ├─ Story: Set up monitoring and alerting
   │  ├─ Story: Prepare runbooks and support guide
   │  └─ Story: Brief customer service team
   ├─ Feature: Phased Launch
   │  ├─ Story: Phase 1 - 10% traffic (4 hours)
   │  ├─ Story: Phase 2 - 50% traffic (4 hours)
   │  └─ Story: Phase 3 - 100% traffic (final)
   └─ Feature: Post-Launch Support
      ├─ Story: Monitor performance and user issues
      ├─ Story: Conduct bug triage and hot fixes
      └─ Story: Gather feedback for Phase 2 roadmap
```

## User Story Definition Template

```
STORY: Build Product Listing with Filtering
Story ID: PM-042
Story Type: Feature
Sprint: Sprint 8
Assignee: Frontend Engineer (Senior)
Story Points: 8 points (relative estimation)

=== STORY NARRATIVE ===
As a customer browsing the product catalog,
I want to filter products by category, price, rating, and brand,
so that I can quickly find products that meet my needs.

=== ACCEPTANCE CRITERIA ===
1. Filtering Functionality
   ✓ Users can filter by Category (dropdown, multi-select)
   ✓ Users can filter by Price (range slider $0-$500)
   ✓ Users can filter by Star Rating (4-5 stars, 3+ stars, all)
   ✓ Users can filter by Brand (checkbox, multi-select)

2. Filter State & UX
   ✓ Selected filters persist as URL query parameters
   ✓ Clearing filters removes all selections and resets to default
   ✓ Filter badge shows number of active filters
   ✓ Product count updates in real-time as filters change

3. Performance
   ✓ Filter results return in <500ms (including API call)
   ✓ Pagination works correctly with active filters
   ✓ No layout shift when results update

4. Accessibility
   ✓ All filter controls are keyboard accessible
   ✓ ARIA labels for screen readers on all inputs
   ✓ Focus states visible on interactive elements

5. Mobile Responsiveness
   ✓ Filters display in collapsible drawer on mobile
   ✓ "Show Filters" button visible and functional
   ✓ Filter UI doesn't overlap product results

=== DEFINITION OF DONE ===
✓ Code written and peer reviewed (approved by 1 senior engineer)
✓ Unit tests written (jest, >80% coverage for filter logic)
✓ Integration tests (Cypress for user flows)
✓ Manual testing completed by QA (checklist: desktop, mobile, tablet)
✓ Performance testing (response time, bundle size impact)
✓ Accessibility audit passed (axe, keyboard nav, screen reader)
✓ Storybook stories added for component variations
✓ Documentation updated (component API, filter params)
✓ Ready for deployment (no console errors/warnings)

=== SUBTASKS ===
1. [Dev] Build filter component UI (inputs, dropdowns, range slider)
2. [Dev] Implement filter state management (React Context / Redux)
3. [Dev] Build API integration (fetch filtered products from backend)
4. [Dev] Add URL query parameter handling (persist/restore filters)
5. [Dev] Implement real-time result count updates
6. [QA] Test filter combinations (category + price + brand)
7. [QA] Test mobile/tablet responsiveness
8. [QA] Accessibility testing (screen reader, keyboard nav)

=== DEPENDENCIES ===
DEPENDS ON:
  - PM-040: Product API filtering endpoint complete
  - PM-041: Product listing component built

BLOCKS:
  - PM-043: Product sorting (must have filtering before sorting)

=== NOTES ===
High Risk: Performance may degrade if product list is 10k+ items.
Mitigation: Use pagination (20 items/page); test with full product catalog.

Technical Approach: Use React Context for filter state, Elasticsearch API for backend filtering.
Consider: Should we implement faceted search (show counts per filter option)?

Story is tied to OKR: Improve product discovery conversion by 15%.
Success Metric: Customers using filters increase from 10% to 30%.

=== ACCEPTANCE TEST CHECKLIST ===
□ Open product listing
□ Click Category filter → select "Electronics"
  → Verify: Results filtered, URL shows ?category=electronics
□ Add Price filter → $0-$100
  → Verify: Results further filtered, count updates
□ Add Brand filter → select "Apple", "Sony"
  → Verify: Results show only matching products
□ Click "Clear Filters"
  → Verify: All filters reset, full product list shows
□ Test on mobile
  → Verify: Filters in drawer, "Show Filters" button works
□ Test with screen reader (NVDA)
  → Verify: All filter controls readable, labels clear
```

## Story Sizing & Estimation

### Story Point Scale (Fibonacci: 1, 2, 3, 5, 8, 13, 21)

| Points | Meaning | Timeline | Example |
|--------|---------|----------|---------|
| **1** | Trivial, well-known | <4 hours | "Fix typo in UI", "Update CSS color" |
| **2** | Small, straightforward | 4-8 hours | "Add new button component", "Update form label" |
| **3** | Small feature, some unknowns | 8-16 hours | "Build login form", "Add email validation" |
| **5** | Medium feature, moderate complexity | 2-3 days | "Build product detail page", "Implement search" |
| **8** | Complex feature, significant unknowns | 3-5 days | "Build shopping cart with multiple states", "API integration" |
| **13** | Very complex, major unknowns | 5-8 days | "Build checkout flow with payment integration" |
| **21** | Epic (should be broken down) | >8 days | "Rebuild entire front-end" (break this down!) |

### Estimation Guidelines

**Do:**
- ✓ Estimate relative to known stories (Story X is 5 points, so Story Y is similar = 5 points)
- ✓ Use planning poker (team estimation, reveals disagreement)
- ✓ Account for unknowns (if you're uncertain, size up; 8 not 5)
- ✓ Break down 13+ point stories (they're too big, too much uncertainty)
- ✓ Adjust for team context (experienced team = smaller estimates; new tech = larger)

**Don't:**
- ✗ Convert story points to time ("8 points = 2 days") — varies by person and context
- ✗ Estimate false precision (don't use 7 points; use 5 or 8)
- ✗ Let story size grow (if scope changes, update the estimate)
- ✗ Use estimation as performance metric (estimating poorly != bad engineer)
- ✗ Compare velocity across teams (their 5 ≠ your 5)

## Testing & QA Strategy per Story

### Testing Layers

**Unit Tests** (Developer)
- Test individual functions/components in isolation
- Example: Filter function returns correct results for given input
- Typical: 80%+ code coverage target

**Integration Tests** (Developer)
- Test components working together
- Example: Filter component talks to API correctly
- Use tools: Cypress, Playwright for UI integration

**User Acceptance Tests** (QA)
- Test user workflows end-to-end
- Example: Customer searches → filters → adds to cart → checks out
- Manual testing against acceptance criteria checklist

### Story-Level Testing Plan

```
STORY: Build Shopping Cart

UNIT TESTS:
✓ addToCart() increases quantity if product exists
✓ removeFromCart() removes product from cart
✓ calculateTotal() sums prices correctly with taxes
✓ applyDiscount() reduces total by correct amount

INTEGRATION TESTS:
✓ Cart persists to localStorage on browser reload
✓ Cart displays correct totals with discounted price
✓ Remove and update buttons work correctly in UI

USER ACCEPTANCE TEST (Manual):
✓ User adds product to cart
✓ Cart count increments (header badge)
✓ User navigates to cart page
✓ Cart shows correct product, quantity, price
✓ User updates quantity → total updates
✓ User applies coupon code → discount applies
✓ User removes item → cart updates, empty cart message shows
✓ User proceeds to checkout
```
