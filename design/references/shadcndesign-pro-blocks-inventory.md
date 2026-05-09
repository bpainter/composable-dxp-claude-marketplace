---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/shadcn]
status: active
source: https://www.shadcndesign.com/pro-blocks
last-synced: 2026-05-09
---

# shadcn Design Pro Blocks Inventory

The catalog of paid pro blocks from [shadcndesign.com](https://www.shadcndesign.com/pro-blocks) (Matt Wierzbicki) — 343 production-grade composed blocks built on shadcn/ui primitives, organized by surface and section type. Independent project, not officially affiliated with Figma or shadcn/ui.

This complements the free official blocks at [[shadcn-block-inventory]]. Where official shadcn blocks cover dashboards, sidebars, login/signup, and calendar, this catalog covers the **landing page**, **application UI**, and **e-commerce** sections that the official blocks don't.

**License note:** Pro blocks are paid (one-time purchase). Use only when the project has a license. The catalog *structure* (categories, conventional section types) is reference-only and free to draw on conceptually even without the implementations.

---

## Catalog structure

| Surface | Categories | Total blocks |
|---|---|---|
| **Landing Page** | 23 categories | 204 blocks |
| **Application UI** | 15 categories | 84 blocks |
| **E-commerce** | 11 categories | 55 blocks |
| **Total** | 49 categories | 343 blocks |

---

## Landing page categories (204 blocks)

The full catalog of marketing-page section types. Each category has 3–22 variants. **Use this list as a checklist when scoping a marketing site** — it names every section type a typical landing page might compose.

| Category | Count | What it covers |
|---|---|---|
| 404 Sections | 3 | Error page layouts with navigation back |
| Banners | 6 | Top-of-page promotional banners, dismissible alerts |
| Bento Grids | 6 | Asymmetric tile-based feature grids (Apple Control Center pattern) |
| Blog Sections | 5 | Blog index layouts, post listings |
| CTA Sections | 7 | Call-to-action blocks (forms, buttons, urgency) |
| Comparison Sections | 6 | Us-vs-them tables and matrices |
| Contact Sections | 6 | Contact forms with map, address, support options |
| Empty LP Sections | 4 | Generic blank section starters |
| FAQ Sections | 5 | Accordion-driven FAQs |
| Feature Sections | 20 | Product feature presentations (icon grids, screenshots) |
| Footers | 9 | Site footers with link columns, newsletter, legal |
| Gallery Sections | 12 | Image grids, masonry, lightbox |
| Header Sections | 20 | Marketing top headers, hero containers |
| Hero Sections | 22 | Above-the-fold hero variants |
| LP Navbars | 7 | Marketing-page navigation bars |
| Landing Page Examples | 1 | Complete page assemblies |
| Logo Sections | 7 | Customer-logo strips, partner walls |
| Patterns | 30 | Background patterns, decorative elements |
| Pricing Sections | 5 | Tiered pricing tables |
| Rich Text Sections | 5 | Long-form content layouts |
| Stats Sections | 7 | Number/metric presentations |
| Team Sections | 4 | About-the-team grids |
| Testimonials Sections | 7 | Customer quote presentations |

### Category guide for consulting deliverables

For a Slalom Composable DXP marketing surface (rare but possible — solution microsite, campaign page), the relevant categories cluster:

- **Foundation:** LP Navbars + Hero Sections + Footers — the page bones.
- **Story:** Feature Sections + Bento Grids + Gallery Sections — what the offering does.
- **Proof:** Logo Sections + Testimonials Sections + Stats Sections — credibility.
- **Conversion:** CTA Sections + Pricing Sections + Contact Sections — the action.
- **Support:** FAQ Sections + Comparison Sections — answer doubts.

---

## Application UI categories (84 blocks)

Internal-app and SaaS dashboard section types. Complements the official shadcn dashboard/sidebar blocks.

| Category | What it covers |
|---|---|
| App Shells | Full app frame (sidebar + topbar + content) |
| Application Examples | Complete app assemblies |
| Buttons | Composed button groups, action sets |
| Cards | Application-grade card variants |
| Description Lists | Key-value display lists (product specs, profiles) |
| Empty Sections | App-level empty states |
| Navbars | App-internal navigation bars |
| Page Headers | Page-level title bars with actions |
| Section Footers | Form footers, action bars |
| Section Headers | Sub-section heading bars |
| Sections | Generic content sections |
| Settings | Settings page layouts |
| Sign in | Authenticated sign-in flows |
| Sign up | Registration flows |
| Table Headers | Composed table header patterns |

### Use for product UI prototypes

When building a product UI mockup or prototype for a client engagement:

1. Start with **App Shells** for the frame.
2. Compose **Page Headers** + **Sections** + **Section Headers** for content surfaces.
3. Use **Description Lists** for any settings, profiles, or detail views.
4. Wire navigation with **Navbars** (in addition to a sidebar from the official catalog).
5. Cover state with **Empty Sections**.

---

## E-commerce categories (55 blocks)

For client engagements with retail or commerce dimensions:

| Category | What it covers |
|---|---|
| Checkouts | Multi-step checkout flows |
| Examples | Complete e-comm assemblies |
| Incentives | Trust signals (free shipping, returns) |
| Order Summary Sections | Cart/order review |
| Product Category Sections | Category landing pages |
| Product Headers | PDP (product detail page) headers |
| Product List Filters | Faceted search filtering |
| Product List Sections | Product grids and lists |
| Sale Sections | Promo/sale section blocks |
| Shopping Carts | Cart pages and drawers |
| Store Navbars | E-commerce-specific navigation |

---

## Cited sections by name

When designing or critiquing, cite specific section types from this catalog by name. It anchors conversations:

> "We need a Logo Section above the Stats Section so the credibility cascade lands before the metrics."

> "The current Pricing Section is a 3-column card row — see [[ai-tells-forbidden-patterns]] — propose a Comparison Section variant instead."

> "Add a Testimonials Section between Feature Section and CTA Section to break the feature-list rhythm."

This is more useful than "we need more social proof" because it points at a specific layout the team can pick from.

---

## Pro blocks vs. official blocks vs. custom

| Source | Cost | When to reach for it |
|---|---|---|
| **Custom built** | Designer time | Signature moments, brand statements, unique interactions |
| **Free shadcn blocks** | Free, MIT | Sidebars, dashboards, login, calendar — covered well |
| **shadcn Design pro blocks** | Paid | Marketing-page sections, e-commerce flows, settings pages — official catalog gaps |
| **Tailwind UI** | Paid | Older catalog with broader coverage; less shadcn-native |

For Slalom work specifically:
- **Internal vault tools / dashboards** → free shadcn blocks.
- **Client demo prototypes** → mix of custom and free blocks; consider pro blocks if license available.
- **Marketing/promotional surfaces** (rare) → pro blocks if license; otherwise custom.

---

## Block customization checklist (same as free blocks)

Same gates as [[shadcn-block-inventory]]:

- [ ] Token replacement (radius, color, type, shadows).
- [ ] Content humanized (no Jane Doe effect).
- [ ] States complete (empty, loading, error).
- [ ] Mobile collapse verified.
- [ ] Pre-delivery checklist run.

Adding for pro blocks specifically:
- [ ] **License confirmed** before shipping pro-block code.
- [ ] **Attribution honored** if required by license terms.

---

## The "agent skills" note

shadcndesign.com also publishes [agent skills](https://www.shadcndesign.com/agent-skills) — Claude Code skills for working with their pro blocks. These are an interesting parallel implementation worth examining when we build [[pro-blocks-composer]] enhancements; not yet evaluated.

---

## Cross-plugin wiring

| Plugin | Skill | How |
|---|---|---|
| design | `design-screen` | Pick blocks for marketing pages. |
| design | `design-web-designer` | Default reference for web layouts. |
| software-engineering | `pro-blocks-composer` | Implementation reference. |
| software-engineering | `frontend-developer` | Block install and customization. |

---

## Re-syncing

When the catalog grows:

1. Re-fetch `https://www.shadcndesign.com/pro-blocks` (any category page lists category counts in the sidebar).
2. Update the counts per category.
3. Note new categories.
4. Update `last-synced` in frontmatter.

Source: [shadcndesign.com Pro Blocks](https://www.shadcndesign.com/pro-blocks)
