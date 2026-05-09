---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/shadcn]
status: active
source: https://ui.shadcn.com/docs/components
last-synced: 2026-05-09
---

# shadcn/ui Component Inventory

The complete catalog of shadcn/ui base components. This is the **single source of truth** cited by both `design` plugin skills (when designing UI from system primitives) and `software-engineering` plugin skills (when implementing). Update `last-synced` in frontmatter when shadcn ships new components.

shadcn/ui is not a component library you install. It's a copy-and-paste registry — `npx shadcn add accordion` drops the Accordion source into your repo. You own it; you customize it. The default styling is the *floor*, never the ceiling. See [[ai-tells-forbidden-patterns]] — "the shadcn-default ban."

For blocks (composed multi-component layouts), see [[shadcn-block-inventory]].
For charts, see [[shadcn-chart-inventory]].
For pro blocks (paid catalog from shadcndesign.com), see [[shadcndesign-pro-blocks-inventory]].

---

## Components by category

shadcn/ui's docs list components alphabetically. Below they're grouped by *what job they do* — easier to choose from when designing.

### Disclosure / progressive reveal

- **Accordion** — vertical list of headers that expand to show content. Use for FAQs, settings sections, structured details. Don't stack more than 7 items without sub-grouping.
- **Collapsible** — single panel that opens/closes. Lighter than Accordion. Use for "show more" affordances.
- **Hover Card** — popover that opens on hover (desktop) or focus. Use for username previews, link previews. **Always pair with focus trigger** — hover-only fails accessibility.
- **Tooltip** — short label on hover/focus. Use for icon-only button explanations. Keep to 1 short phrase. Long content goes in Hover Card.

### Dialogs and surfaces

- **Dialog** — centered modal. Use for focused tasks (forms, confirmations) that need user attention.
- **Alert Dialog** — modal that requires explicit user choice. Use for destructive confirmations only. Cannot be dismissed by overlay click.
- **Drawer** — slide-up sheet (mobile-first by default). Use for forms, filters, secondary navigation on mobile. Backed by Vaul (Emil Kowalski) — physics-based.
- **Sheet** — side-anchored drawer (left/right/top/bottom). Use for filters, settings panels, secondary nav.
- **Popover** — anchored content panel. Use for date pickers, color pickers, contextual settings.
- **Sidebar** — persistent app sidebar. Use for primary app navigation. Supports collapsible-to-icon and inset variants. The sidebar block catalog has dozens of pre-composed variants.

### Inputs

- **Input** — text input. Standard for short-form text fields.
- **Input Group** — input with leading/trailing addons (icons, units, prefixes/suffixes).
- **Input OTP** — one-time-passcode segmented input. Use for 2FA, verification codes.
- **Textarea** — multi-line text input. Use for body text, comments, descriptions.
- **Field** — semantic form field wrapper (label + input + error + helper). The recommended primary form primitive in current shadcn.
- **Label** — accessible label paired with form controls.
- **Checkbox** — single boolean choice. Use for opt-ins, multi-select lists.
- **Radio Group** — single choice from set. Use for 2–6 mutually exclusive options. Above 6, use Select.
- **Select** — dropdown picker. Use for 6+ options or when space is constrained.
- **Native Select** — system-rendered `<select>`. Use only when native rendering matters (mobile keyboard integration, search-by-typing).
- **Combobox** — searchable Select with autocomplete. Use for large option sets.
- **Switch** — boolean toggle. Use for instant-effect settings (autosave, dark mode). Don't use for form submission state.
- **Slider** — range input. Use for continuous values where exactness matters less than relative position (volume, opacity).
- **Calendar** — date picker. Pairs with Popover via Date Picker.
- **Date Picker** — input + Calendar combo. The complete date-input UX.

### Data display

- **Table** — semantic HTML table with shadcn styling. Use for static or simple data.
- **Data Table** — composed Table with sorting, filtering, pagination via TanStack Table. Use for any non-trivial data display.
- **Card** — container for grouped content. **Customize aggressively** — default Card is the most over-used shape in AI-generated UI.
- **Avatar** — user image or initials placeholder. Use realistic photos or themed initials, never the default Lucide silhouette.
- **Badge** — short status indicator. Variants: default, secondary, destructive, outline. Use for labels, counts, statuses.
- **Skeleton** — placeholder shimmer for loading content. Use for content blocks loading 100ms–10s.
- **Spinner** — inline loading indicator. Use on buttons during async, not as primary loading UI.
- **Aspect Ratio** — enforces image/video proportions. Use to prevent CLS.

### Navigation

- **Breadcrumb** — hierarchical path indicator. Use on web for 3+ level deep nav.
- **Navigation Menu** — desktop top-level nav with hover dropdowns.
- **Menubar** — desktop application-style menu bar (File, Edit, View). Use for desktop apps, rarely for web.
- **Pagination** — page navigation for lists. Use when total pages is known.
- **Tabs** — switch between related views in the same context. Use for 2–6 tabs at most.
- **Toggle Group** — segmented control / multi-toggle. Use for tool palettes, filter groups.

### Action triggers

- **Button** — primary action element. Variants: default, secondary, ghost, outline, destructive, link. **Always customize** the radius, color, and shadow stack from defaults.
- **Button Group** — connected button row. Use for tool palettes, segmented actions.
- **Toggle** — single boolean button. Use for inline state toggles (bold, italic).
- **Dropdown Menu** — button-triggered menu. Use for action menus, user menus.
- **Context Menu** — right-click triggered menu. Use sparingly on web; users don't expect it.

### Feedback

- **Alert** — inline message banner. Variants: default, destructive. Use for page-level info that's not blocking.
- **Sonner** — toast notification system (Emil Kowalski's library, official shadcn pick). Use for transient feedback. **Replaces the deprecated Toast component** — always use Sonner.
- **Toast** — *deprecated.* Use Sonner.
- **Progress** — known-duration progress indicator.
- **Empty** — empty-state container with icon, headline, description, action. Use whenever a list, table, or container has no data.

### Layout primitives

- **Separator** — horizontal/vertical divider line.
- **Resizable** — split-pane resizable panels. Use for IDE-style layouts, multi-column dashboards.
- **Scroll Area** — custom-styled scroll container. Use when default scrollbars don't fit the design.
- **Carousel** — horizontal scrolling content panel. Use sparingly — autoplay is almost always wrong.
- **Item** — semantic list-item primitive (icon + content + meta). Newer addition; replaces ad-hoc list rows.

### Specialized

- **Command** — command palette / Cmd+K interface. Backed by cmdk (pacocoursey). Use for power-user nav, search, action launchers.
- **Chart** — wrapper around Recharts with consistent styling. See [[shadcn-chart-inventory]] for variants.
- **Kbd** — keyboard shortcut display (e.g., `⌘K`). Use to indicate keyboard accelerators.
- **Direction** — RTL/LTR direction provider. Use for internationalized apps.
- **Typography** — text-style primitives (h1, h2, p, blockquote, code, list). Reference for prose-content styling.

---

## Composition patterns (cite these by name)

These aren't components themselves — they're recurring compositions worth naming so design and engineering reach for the same thing:

- **Field-with-helper-and-error** — Field + Label + Input + helper text + error. The single primitive every form should use.
- **Searchable-Select** — Combobox configured for fuzzy-search filtering. Use over Select once options exceed 8.
- **Date-range-picker** — Date Picker with start/end. Recharts often pairs.
- **Filter-bar** — Toggle Group + Combobox + Date Picker assembled into a horizontal row above a Data Table.
- **Empty-with-action** — Empty composed with a primary CTA Button. The shape every empty list should take.
- **Confirm-destructive** — Alert Dialog with destructive Button variant. The only acceptable pattern for delete/cancel/discard.
- **Command-K nav** — Command palette wired to global Cmd+K. Use for any app with >5 routes.

---

## When to reach for shadcn vs. building custom

**Reach for shadcn when:**
- The component has accessibility complexity (Dialog, Combobox, Date Picker, Drawer) — shadcn's Radix-backed implementations handle focus, ARIA, keyboard nav correctly.
- You need consistent design-system tokens across all primitives.
- Speed matters and the component is a known commodity.

**Build custom when:**
- The interaction is genuinely unique to your product (no shadcn equivalent).
- The brief calls for a stylistic register where shadcn defaults work against you (Brutalist, Anti-design, Maximalist — see [[stylistic-vocabulary]]).
- You're making a brand statement that a generic primitive would dilute (hero buttons, signature interactions).

**Customize aggressively when:**
- Using shadcn at all. Defaults are bland; the bands and shadow stacks announce "AI-generated SaaS." Shape the radius scale, color tokens, and elevation system to match the brief before shipping.

---

## Cross-plugin wiring

Skills that should cite this inventory:

| Plugin | Skill | How |
|---|---|---|
| design | `design-screen` | Pick primitives during composition |
| design | `design-product-designer` | Map Figma components 1:1 to shadcn |
| design | `design-web-designer` | Web layout primitives |
| design | `design-handoff` | Reference component names in handoff annotations |
| design | `design-audit` | Cite drift from system primitives |
| software-engineering | `frontend-developer` | Implementation reference |
| software-engineering | `shadcn-component` | Component spec authoring |
| software-engineering | `shadcn-registry-author` | Custom registry composition |
| software-engineering | `pro-blocks-composer` | Block composition |
| software-engineering | `design-implementation` | Design-to-code translation |

---

## Re-syncing

When shadcn ships new components, update this file:

1. Re-fetch `https://ui.shadcn.com/docs/components`.
2. Diff the alphabetical list against the categories above.
3. Add new components to the most appropriate category section.
4. Update `last-synced` in frontmatter.
5. If the new component changes a composition pattern, update the "Composition patterns" section.

Source: [shadcn/ui components](https://ui.shadcn.com/docs/components)
