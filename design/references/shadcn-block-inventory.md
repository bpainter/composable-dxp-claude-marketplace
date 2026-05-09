---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/shadcn]
status: active
source: https://ui.shadcn.com/blocks
last-synced: 2026-05-09
---

# shadcn/ui Block Inventory

The composed multi-component layouts shipped in the official shadcn/ui blocks catalog. Free, MIT, copy-and-paste via `npx shadcn add <block-name>`. Categories: **Sidebar**, **Login**, **Signup**, **Dashboard**, **Calendar**, plus a featured rotation.

This inventory is the *foundation* layer — the free, official blocks. For the larger paid catalog of landing-page, application, and e-commerce blocks, see [[shadcndesign-pro-blocks-inventory]].

For the underlying primitives, see [[shadcn-component-inventory]].

---

## Block categories

shadcn organizes free blocks into named categories, each with multiple variants. The blocks page rotates a featured set; the named category pages list every variant.

| Category | URL | Typical variants | When to reach for it |
|---|---|---|---|
| **Featured** | `/blocks` | rotating set across categories | Browsing for inspiration |
| **Sidebar** | `/blocks/sidebar` | sidebar-01 through sidebar-15+ | Any app with persistent left/right nav |
| **Login** | `/blocks/login` | login-01 through login-05+ | Auth pages |
| **Signup** | `/blocks/signup` | signup-01+ | Registration pages |
| **Dashboard** | `/blocks/dashboard` (legacy) | dashboard-01+ | Stats + chart + table layouts |
| **Calendar** | `/blocks/calendar` | calendar-01+ | Scheduling, booking, event displays |

---

## Sidebar blocks

The sidebar catalog is the deepest. Each variant is a different layout/density choice. **Pick one as the project default; do not mix variants** — sidebars define a product's spatial grammar.

Common variants and their distinct character:

- **sidebar-01** — basic vertical menu with sections.
- **sidebar-02** — collapsed-icon variant; expands on hover.
- **sidebar-03** — submenu support (multi-level navigation).
- **sidebar-04** — search-prominent (Command palette integrated).
- **sidebar-05** — workspace switcher at top.
- **sidebar-06** — user-menu + nav, no submenu.
- **sidebar-07** — collapsible-to-icons (the "Linear" sidebar pattern). Most common for prosumer SaaS.
- **sidebar-08** — inset variant (sidebar floats inside the canvas, not flush to edge).
- **sidebar-09** — header-aware sidebar.
- **sidebar-10–15+** — variants with calendar, projects, recent items, secondary nav, breadcrumbs in header, etc.

**Selection guide:**
- Dense data tool → sidebar-07 (collapsible) or sidebar-04 (search-first).
- Multi-tenant / multi-workspace → sidebar-05 (workspace switcher).
- Prosumer SaaS → sidebar-07 or sidebar-08 (inset).
- Marketing/portfolio app → sidebar-01 (basic).
- Settings-heavy → sidebar-03 (submenu).

---

## Login and signup blocks

Five-ish variants of each. Differences are mostly compositional (form alone, form + image, form + brand background, form + testimonial, etc.).

- **login-01** — minimal centered form.
- **login-02** — form with header logo lockup.
- **login-03** — form with muted background color and brand wordmark.
- **login-04** — form + image (split layout).
- **login-05** — form + testimonial (split layout).

**Selection guide:**
- Internal tool / B2B SaaS → login-01 or login-03.
- Premium B2C / brand-led → login-04 (image) or login-05 (testimonial-led).
- Multi-tenant → login-04 with custom-tenant background.

Same variants apply to signup.

---

## Dashboard blocks

The dashboard-01 block is the canonical "section-cards + chart + data-table" layout — Vercel's reference dashboard. The current shadcn-blocks page features it as the headline example. Composes:

- AppSidebar (sidebar-07-style)
- SiteHeader
- SectionCards (4-up KPI cards with deltas)
- ChartAreaInteractive (interactive area chart with date-range)
- DataTable (sortable, filterable)

**When to use dashboard-01:** any analytics/KPI surface where you need a "respectable starting point" without designing from scratch. **Customize aggressively** before shipping — the default is widely recognized as a shadcn default.

---

## Calendar blocks

Newer category. Variants cover:
- Inline calendar (date selection in-page)
- Calendar with sidebar (event list + month grid)
- Range selector (start/end date selection)
- Booking-style calendar (slot picker)

---

## Composition pattern: when to use a block vs. a component

**Reach for a block when:**
- You need a complete layout, not a single primitive.
- The brief is "respectable starting point" rather than "signature design."
- Speed matters and the layout is conventional.

**Reach for components directly when:**
- The brief calls for a layout that doesn't match any block.
- You're making a signature visual statement (hero, brand moment, novel composition).
- The block's defaults clash with the chosen stylistic register.

**Always do, regardless of starting point:**
- Customize the visual style — radius, color, shadows, type.
- Replace placeholder content with real or contextually realistic content (avoid the Jane Doe effect — see [[ai-tells-forbidden-patterns]]).
- Run the four checks from [[design-taste]] before shipping.

---

## Block customization checklist

Before any block ships:

- [ ] **Token replacement.** Replace shadcn defaults (`--primary`, `--muted`, `--border`) with brief-specific tokens that announce the project.
- [ ] **Radius scale customized.** Default `--radius: 0.5rem` is the AI tell. Pick a scale that matches the stylistic register from [[stylistic-vocabulary]].
- [ ] **Shadow stack customized.** shadcn's default elevation is conservative. Tune for the brief — flatter for Swiss-modern, layered for Neo-skeu.
- [ ] **Type system applied.** Replace default font stack with brief-specific pairings (see [[ai-tells-forbidden-patterns]] — the Inter ban).
- [ ] **Content humanized.** No "Acme Inc.," no "John Doe," no `99%`. Replace with realistic context (see [[ai-tells-forbidden-patterns]] — the Jane Doe effect).
- [ ] **Empty/loading/error states.** Blocks don't always ship complete state coverage. Add what's missing.
- [ ] **Mobile collapse verified.** Sidebar blocks especially — test the mobile breakpoint.
- [ ] **Pre-delivery checklist.** Run [[pre-delivery-checklist]] before shipping.

---

## Cross-plugin wiring

| Plugin | Skill | How |
|---|---|---|
| design | `design-screen` | Pick a starting block, then customize. |
| design | `design-product-designer` | Mirror the block's structure in Figma. |
| design | `design-web-designer` | Choose blocks for marketing pages. |
| design | `design-audit` | Detect un-customized blocks (drift from brief). |
| software-engineering | `pro-blocks-composer` | Block composition reference. |
| software-engineering | `frontend-developer` | Block install commands and customization. |
| software-engineering | `nextjs-scaffold` | Initial project scaffolding via blocks. |

---

## Re-syncing

When shadcn ships new block categories or variants, update this file:

1. Re-fetch `https://ui.shadcn.com/blocks` and the per-category pages (`/blocks/sidebar`, `/blocks/login`, etc.).
2. Note new categories and variant counts.
3. Add new categories to the table above.
4. Update `last-synced` in frontmatter.

Source: [shadcn/ui blocks](https://ui.shadcn.com/blocks)
