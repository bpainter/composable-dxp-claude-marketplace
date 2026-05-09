---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[pro-blocks-composer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Pro Blocks Catalog

A working catalog of common block categories across the major shadcn-compatible block libraries. Use this to quickly identify what's available before designing from scratch.

## Library map

| Library | URL | Specialization |
|---|---|---|
| shadcndesign Pro Blocks | shadcndesign.com/pro-blocks | Curated, Figma-aligned, agent-skill ready |
| shadcnblocks | shadcnblocks.com | Largest selection (1,500+ blocks), wide variety |
| shadcn.io | shadcn.io | 6,000+ sections, 46 categories, AI-native |
| Shadcn Studio | shadcnstudio.com | Components + blocks + UI kits + boilerplate |
| Official shadcn blocks | ui.shadcn.com/blocks | Reference implementations from the core team |

For most projects, shadcndesign Pro Blocks + the official shadcn library are sufficient. Pull from others when you need volume or variety.

## Block categories

### Marketing — Hero Sections

The first thing visitors see. Sets brand tone. Must communicate value in <5 seconds.

Common patterns:
- **Centered with CTA** — value prop, subtitle, primary + secondary CTA, single column
- **Hero with image right** — value prop on left, product screenshot on right
- **Hero with video** — autoplaying loop, demo or product visualization
- **Hero with form** — for direct conversion (signup, demo request, calculator)
- **Hero with social proof** — logos or stats integrated into the hero
- **Hero with terminal/code** — dev-tool brands often show code inline

Decision factors:
- Audience-aware: B2B SaaS often uses image-right or terminal; consumer often uses centered or video.
- Conversion-focused: hero with form converts higher when the form is short (1–3 fields).

### Marketing — Feature Sections

Where you make the case. Usually 3–6 features per page.

Common patterns:
- **Three-column grid** — three benefits side by side, each with icon + headline + paragraph
- **Alternating left/right** — feature image alternates sides as you scroll, creates rhythm
- **Bento grid** — varied tile sizes for visual interest, harder to maintain
- **Numbered steps** — for process-oriented features ("how it works")
- **Comparison table** — when the feature *is* the differentiator vs competitors

### Marketing — Pricing Sections

The conversion-critical block. Most products have 3 tiers; some have 2 or 4.

Common patterns:
- **3-tier card** — Basic / Pro / Enterprise, middle tier highlighted as "Most Popular"
- **2-tier with toggle** — Personal / Team, monthly/annual toggle for the discount
- **Calculator-based** — usage-based pricing with a slider (per seat, per request, etc.)
- **Single-plan with feature list** — when there's only one product

Watch for dark patterns: anchoring with a useless decoy tier, hiding monthly behind annual default, "most popular" on a tier that's not actually most popular.

### Marketing — Social Proof

Logo Clouds, Testimonials, Stats, Customer Story Cards.

Common patterns:
- **Logo cloud** — grayscale or full-color logos of customers/partners, single row or 2-row grid
- **Testimonial carousel** — quote, name, title, avatar, often with company logo
- **Testimonial grid** — 3–6 testimonials in a card layout, no carousel needed
- **Stats bar** — "10,000+ teams, 99.99% uptime, 4.9 stars" — numbers as social proof
- **Customer story cards** — link to longer case studies

### Marketing — CTA Sections

The "now act" block, usually before the footer.

Common patterns:
- **Big text + single CTA** — "Ready to ship faster? [Start free trial]"
- **Side-by-side** — heading on left, CTAs on right
- **CTA with image** — for visual products that benefit from one more product moment
- **Newsletter capture** — when conversion isn't ready, get an email

### Marketing — FAQ Sections

Reduce sales friction by answering objections.

Common patterns:
- **Single column accordion** — question + click-to-expand answer
- **Two-column accordion** — denser, fits more
- **Tabbed FAQ** — categories (general, pricing, technical) as tabs

### Site Chrome — Navbars

Top navigation. Usually persistent. Critical for brand consistency.

Common patterns:
- **Logo + links + CTA** — classic SaaS navbar, links centered or left, CTA right
- **Logo + mega-menu** — for larger products with many subpages, nav items reveal panels on hover
- **Logo + search + actions** — apps where search is primary
- **Sticky with shrink** — full-height on first viewport, collapses on scroll
- **Mobile hamburger** — secondary nav only; primary nav stays visible

### Site Chrome — Footers

Bottom navigation. Often sitemap-like.

Common patterns:
- **4-column** — Company / Product / Resources / Legal columns of links
- **Newsletter + columns** — newsletter signup top, columns below
- **Logo + copyright + minimal links** — for landing pages or one-pagers
- **Footer with social icons** — social media presence emphasized

### Site Chrome — Page Headers

The top of an internal page, after the navbar.

Common patterns:
- **Title + subtitle** — page name, brief description
- **Title + tabs** — tabs immediately below the title for sub-navigation
- **Title + breadcrumbs + actions** — internal app pages with deep hierarchies
- **Hero-style page header** — for marketing pages where the page header IS the hero

### Application — App Shells

The persistent UI chrome around your app's content.

Common patterns:
- **Sidebar + top header + content** — most desktop SaaS apps
- **Top nav + content** — simpler apps, content-first
- **Sidebar (collapsible) + main** — for content-heavy apps where users want max canvas
- **Tabbed top with sidebar** — for apps with both horizontal contexts (workspaces) and vertical sections (within a workspace)

shadcndesign and others ship app-shell blocks that are nearly drop-in.

### Application — Sidebars

Often the most-customized block — your nav structure is project-specific.

Common patterns:
- **Single-level list** — flat list of nav items
- **Two-level nav** — collapsible sections with sub-items
- **Pinned + scrollable** — favorites on top, full nav below
- **Workspace switcher + nav** — when users belong to multiple workspaces or organizations

The shadcn `Sidebar` component (in core) is composable enough to handle all of these.

### Application — Sign-in / Sign-up

Authentication flows. Often legally and brand-critical.

Common patterns:
- **Centered card** — single card with form, branding above
- **Split with image** — form on one side, brand image/illustration on other
- **Multi-step** — for complex onboarding (collect email → verify → name → company → done)
- **OAuth-first** — "Continue with Google/Apple/GitHub" prominent, email as fallback

Required surrounding patterns: forgot password, email verification, password reset, account confirmation.

### Application — Settings

Where users manage their account, profile, billing, team, notifications.

Common patterns:
- **Sidebar settings** — left rail with section names, content on right
- **Tabbed settings** — horizontal tabs at top
- **Stacked sections** — single scroll with section headers (mobile-first)
- **Accordion settings** — collapsible sections, dense

shadcndesign ships several settings-pane templates that handle the common categories: Profile, Account, Billing, Team, Notifications, API Keys, Integrations.

### Application — Empty Sections

What users see before they have data. The make-or-break onboarding moment.

Common patterns:
- **First-time empty** — illustration + welcome text + "Create your first X" CTA
- **Search-result empty** — "No results for 'xyz'. Try a broader query."
- **Cleared empty** — "All caught up!" — celebratory after completing tasks
- **Feature-locked empty** — "Upgrade to enable this" — gating behind a paywall

Each empty state has different appropriate voice and action. Don't reuse the same illustration for all four.

### Data — Cards

Display blocks for content that's part of a feed or grid.

Common patterns:
- **Article card** — image, title, excerpt, byline, date
- **Product card** — image, name, price, rating, action
- **Stat card** — large number, label, change indicator (up/down arrow + delta)
- **User card** — avatar, name, role, action (message, follow, etc.)
- **Project/work card** — preview image, title, status, assignees

### Data — Description Lists

Key/value pairs of structured data.

Common patterns:
- **Two-column** — key on left, value on right, repeated rows
- **Single column** — label above value, denser
- **Card-wrapped** — same as two-column but inside a card
- **Editable** — values become inputs on click

### Data — Tables

The bread and butter of every B2B SaaS app.

Common patterns:
- **Standard data table** — sortable headers, pagination, row actions
- **Selectable rows** — checkboxes for bulk actions
- **Expandable rows** — click row to reveal detail
- **Card-style on mobile** — tables collapse to card lists on small screens
- **Inline editable** — cells become inputs on click

shadcn's `Table` and `DataTable` components are the right primitives. Pro blocks ship complete table layouts with filters, search, and pagination wired.

### Data — Product List Filters

Sidebar or top-bar filter UI for catalog views.

Common patterns:
- **Sidebar filters** — left rail with collapsible filter sections
- **Top-bar filters** — pills or dropdowns for quick filtering
- **Modal filters (mobile)** — full-screen filter sheet on mobile
- **Saved filter views** — users can save and reuse filter combinations

## Selection heuristics

When picking between similar blocks:

1. **Closest visual language to your existing pages** wins — every block you don't have to reconcile saves time.
2. **Smallest block that fits** wins — over-featured blocks have more to remove than missing blocks have to add.
3. **Block with the most prop-driven customization** wins — props are easier to maintain than forks.
4. **Block from a library you've already configured** wins — fewer setup steps, fewer dependencies.

## Adaptation patterns (preferred order)

When a block is close but not perfect:

1. **Pass different props** — most variation should be prop-driven (size, variant, color)
2. **Wrap with project-level styles** — apply your tokens via parent class, override block defaults
3. **Compose with project-level components** — replace one part of the block with your own component
4. **Fork the block** — last resort, accept the maintenance cost

Forking is sometimes the right call. Just do it consciously, document why, and add to your "modified-block tracking" list.

## Common pitfalls

- **Lorem Ipsum left in production** — happens way too often. Always replace placeholder text/images before merge.
- **Block dependencies missed** — when you copy-paste from a block preview, you miss the npm packages and shadcn primitives the block depends on. Use the CLI/MCP install instead.
- **Tailwind config not updated** — some blocks expect custom Tailwind config (specific shadows, colors, animations). The CLI handles this; manual paste doesn't.
- **CSS vars not added** — same story for CSS variables.
- **Routes pointing to `#`** — block links default to `#`. Hooking up actual routes is your job.
- **Mobile breakage** — blocks are usually responsive but assume content fits. Long button labels, long product names, etc. break layouts. Test with real-length content.
