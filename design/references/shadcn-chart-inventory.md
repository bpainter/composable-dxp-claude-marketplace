---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/data-viz, topic/shadcn]
status: active
source: https://ui.shadcn.com/charts
last-synced: 2026-05-09
---

# shadcn/ui Chart Inventory

The complete catalog of shadcn/ui charts. Built on Recharts; styled via CSS variables and the shadcn `<Chart>` wrapper. Free, copy-and-paste, MIT.

shadcn organizes charts into seven categories. For each: **what it shows best**, **named variants in the catalog**, **when to reach for it**, **what to avoid**.

For data-visualization decisions beyond shadcn's catalog, see [[design-visualization]] (TBD — Session 2).
For the underlying Chart wrapper component, see [[shadcn-component-inventory]].

---

## Category guide

| Category | URL | Variants | Best for |
|---|---|---|---|
| **Area** | `/charts/area` | 10 | Trends over time with magnitude/volume |
| **Bar** | `/charts/bar` | ~10 | Comparison across discrete categories |
| **Line** | `/charts/line` | ~10 | Trends over time, multiple series |
| **Pie** | `/charts/pie` | ~10 | Proportion of a whole (rarely the right answer) |
| **Radar** | `/charts/radar` | ~5 | Multi-dimensional comparison (assessments, scoring) |
| **Radial** | `/charts/radial` | ~5 | Single-metric progress, gauges |
| **Tooltips** | `/charts/tooltip` | ~5 | Tooltip styling variants |

---

## Area charts

The default catalog has ten area chart variants:

- `chart-area-default` — single series, smooth curve
- `chart-area-linear` — single series, straight-line interpolation
- `chart-area-step` — single series, step interpolation (use for state changes, on/off-style data)
- `chart-area-stacked` — multiple series stacked (parts of a whole, over time)
- `chart-area-stacked-expand` — stacked normalized to 100% (proportional over time)
- `chart-area-legend` — multi-series with legend
- `chart-area-icons` — legend with icons
- `chart-area-gradient` — fill gradient (subtle premium feel)
- `chart-area-axes` — explicit axis labeling
- `chart-area-interactive` — date-range filter built-in (the dashboard-01 chart)

**When to reach for an area chart:**
- Visualizing magnitude/volume over time (revenue, signups, traffic).
- The "fill" emphasizes the magnitude beneath the line.
- Multi-series stacked area when the parts compose a whole that itself matters.

**When NOT:**
- Sparse data points (use a line chart with markers instead).
- Comparison across categories (use bar).
- More than 4 stacked series (becomes unreadable).

---

## Bar charts

Common variants:

- Default vertical bar (single series)
- Horizontal bar (long category labels, mobile)
- Grouped bar (multi-series side-by-side)
- Stacked bar (parts of a whole per category)
- Negative bar (deltas, profit/loss)
- Mixed (bar + line for budget vs. actual)

**When to reach for a bar chart:**
- Comparing a metric across discrete categories.
- The comparison is the primary message.

**When NOT:**
- Time-series with continuous data (use line or area).
- Categories number > 12 (consider Pareto sort, or shift to a different chart).

---

## Line charts

Common variants:

- Default smooth (curve)
- Linear (straight)
- Multi-series with legend
- Multi-series with markers
- Step
- With reference line
- Brush (zoomable)

**When to reach for a line chart:**
- Time-series, especially multi-series comparison.
- The trajectory is the primary message.
- 2–5 series. Above 5, consider small multiples.

**When NOT:**
- Single-series with magnitude emphasis (use area).
- Discrete categories (use bar).

---

## Pie charts

shadcn ships pie variants — but they're often the wrong choice. **Default to a horizontal bar chart for proportion data.** Pie chart works only when:

- The proportions are dramatically unequal.
- There are 2–4 slices.
- The audience absolutely expects a pie.

Variants in catalog:
- Default pie
- Donut
- Donut with label
- Pie with custom colors
- Stacked / nested

**Pie failure modes:**
- 5+ slices = unreadable.
- Similar-sized slices = harder to compare than bars.
- "Other" slice that exceeds 30% = the chart structure is wrong.

---

## Radar charts

Use case: multi-dimensional capability/competency comparison. "Skills assessment," "feature comparison," "category-by-category ranking."

**When to reach for a radar chart:**
- 3–8 dimensions, 1–3 series.
- The comparison across dimensions matters more than each individual value.

**When NOT:**
- 2 dimensions (use a scatter or simple bar).
- The dimensions are not commensurate (radar implies they share a scale).

---

## Radial charts

Use case: single-metric progress gauges, fitness-tracker-style displays.

Variants:
- Single-arc gauge
- Multiple concentric arcs (multi-metric)
- With label center
- With axis

**When to reach for a radial chart:**
- A single bounded metric (0–100%, 0–goal).
- Visual prominence matters more than precision.

**When NOT:**
- Comparing absolute values (precision matters; use bar).
- Time-series.

---

## Tooltips

shadcn's `<ChartTooltip>` and `<ChartTooltipContent>` provide consistent tooltip styling across chart types. Variants:
- Default (label + value)
- Indicator (color dot prefix)
- Multiple values
- Custom content

**Tooltip discipline (cite [[pre-delivery-checklist]]):**
- Numbers formatted per locale (`Intl.NumberFormat`).
- Dates formatted per locale and granularity (day/week/month visible).
- Tabular numerals for alignment.
- Keyboard-reachable (no hover-only).

---

## Consulting-deliverable chart guide

Specific to Slalom Composable DXP work — when proposing or presenting data, default to:

| Story | Chart |
|---|---|
| Revenue trajectory | Area (with gradient fill) |
| Pipeline by stage | Horizontal bar |
| Conversion funnel | Funnel (custom; not shadcn) or sequential bar |
| Year-over-year comparison | Multi-series bar |
| Velocity over time | Line (with markers) |
| Capability assessment | Radar |
| Single-KPI vs. goal | Radial gauge |
| Cost composition | Stacked bar (not pie) |
| Headcount by team | Horizontal bar |
| Adoption curve | S-curve line with annotation |

For executive audiences, default to **horizontal bar charts** unless time-series demands a different shape. Vertical bars look cramped at deck dimensions.

---

## Chart customization checklist

Before any chart ships:

- [ ] **Color palette aligned with brief.** Default shadcn chart palette (`var(--chart-1)` through `var(--chart-5)`) is too AI-default. Customize.
- [ ] **Color is not the only encoding.** Add patterns, labels, or icons (see [[pre-delivery-checklist]] — Priority 10).
- [ ] **Axis labels with units.** "$M ARR," "users (k)," not raw numbers.
- [ ] **Tabular numerals on axes.**
- [ ] **Tooltip wired with locale formatting.**
- [ ] **Empty state designed.** Don't ship a chart that renders an empty axis frame on no-data.
- [ ] **Loading state with skeleton.**
- [ ] **Reduced-motion respected** for entrance animations.
- [ ] **Mobile reflow tested.** Charts that work at desktop often fail at 375px.
- [ ] **Direct labeling for ≤8 data points.** Reduces eye travel to legend.

---

## Cross-plugin wiring

| Plugin | Skill | How |
|---|---|---|
| design | `design-visualization` (TBD) | Routes to shadcn for chart needs in product UI. |
| design | `design-document` (TBD) | Embeds shadcn charts in long-form documents. |
| design | `design-presentation` (TBD) | Embeds shadcn charts in 16:9 decks. |
| software-engineering | `frontend-developer` | Chart implementation. |
| software-engineering | `data-engineer` | Data shape for charts. |

---

## Re-syncing

When shadcn ships new chart variants:

1. Re-fetch the seven category pages.
2. Note new variants.
3. Update the variant counts in the category guide.
4. Update `last-synced` in frontmatter.

Source: [shadcn/ui charts](https://ui.shadcn.com/charts/area)
