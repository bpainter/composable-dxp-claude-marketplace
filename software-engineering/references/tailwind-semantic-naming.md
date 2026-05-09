---
type: reference
project: skills-library
scope: plugin
plugin: software-engineering
tags: [type/reference, plugin/software-engineering, scope/foundational, topic/software]
status: active
---
# Tailwind Semantic Naming Convention

A pragmatic approach to Tailwind that combines utility-first speed with the maintainability of semantic class names. Used as the default styling approach across this engineering library.

## The problem with utility-first at scale

Pure utility-first Tailwind looks great in tutorials and small components. In production code at scale, three problems compound:

1. **Long class strings are hard to read.** A button with 18 utility classes is a wall of text. Reviewing diffs and finding the relevant change is a chore.
2. **Repeated patterns drift.** When the same "primary button" pattern is duplicated across 40 components, half of them get out of sync over time. The visual result is a slightly inconsistent product that nobody can easily fix.
3. **CVA helps but doesn't solve readability.** [Class Variance Authority](https://cva.style/) handles variants well, but the underlying utility strings inside it are still hard to scan and harder to debug when an unwanted style appears.

Pure semantic CSS has its own problems (specificity wars, dead styles, naming exhaustion). The middle path: **utility-first for one-offs, semantic component classes via `@layer components` for anything reused.**

## The convention

Use `@layer components` with `@apply` to create semantic classes for **reusable patterns**. Use raw utilities inline for **one-off styling**.

```css
/* In your global stylesheet (e.g., app/globals.css) */

@layer components {
  /* Block: the standalone, reusable UI element */
  .btn-primary {
    @apply py-2 px-4 bg-blue-500 text-white font-semibold rounded-lg shadow-md
           hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-400;
  }

  /* Variant of the block */
  .btn-secondary {
    @apply py-2 px-4 bg-gray-100 text-gray-900 font-semibold rounded-lg
           hover:bg-gray-200 focus:outline-none focus:ring-2 focus:ring-gray-400;
  }
}
```

Then in JSX/HTML:

```jsx
<button className="btn-primary">Save</button>
<button className="btn-secondary">Cancel</button>
```

The component code stays scannable. The styles live in one place. When a designer changes the brand blue, you change it once.

## Naming convention: parent-child-grandchild

For composed components, use a hierarchical naming pattern that mirrors the component's structure. This is BEM-inspired (`block__element--modifier`) but adapted to feel natural in JSX.

### Pattern

```
<component>                            (parent / block)
<component>-<element>                  (child)
<component>-<element>-<sub-element>    (grandchild)
<component>--<modifier>                (block variant — alternate form)
<component>-<element>--<modifier>      (child variant)
.is-<state>                            (state class — applied additively)
```

We use `-` for hierarchy and `--` for variants to keep the visual distinction clear.

### Example: a card component

```css
@layer components {
  /* Parent — the card container */
  .card {
    @apply bg-white dark:bg-gray-900 rounded-lg shadow-md overflow-hidden;
  }

  /* Variants of the card */
  .card--elevated {
    @apply shadow-lg;
  }
  .card--bordered {
    @apply border border-gray-200 dark:border-gray-800 shadow-none;
  }

  /* Children — direct sub-elements */
  .card-header {
    @apply px-6 py-4 border-b border-gray-100 dark:border-gray-800;
  }
  .card-body {
    @apply px-6 py-4;
  }
  .card-footer {
    @apply px-6 py-4 border-t border-gray-100 dark:border-gray-800;
  }

  /* Grandchildren — sub-elements within children */
  .card-header-title {
    @apply text-lg font-semibold text-gray-900 dark:text-white;
  }
  .card-header-subtitle {
    @apply text-sm text-gray-500 dark:text-gray-400 mt-1;
  }

  /* States — applied additively to indicate runtime state */
  .is-loading {
    @apply opacity-60 pointer-events-none;
  }
  .is-disabled {
    @apply opacity-40 pointer-events-none cursor-not-allowed;
  }
}
```

Usage:

```jsx
<article className="card card--elevated">
  <header className="card-header">
    <h2 className="card-header-title">Settings</h2>
    <p className="card-header-subtitle">Manage your account preferences</p>
  </header>
  <div className="card-body">
    {/* content */}
  </div>
  <footer className="card-footer">
    <button className="btn-primary">Save</button>
  </footer>
</article>
```

This reads top-to-bottom. Anyone scanning the markup understands the structure without parsing 30 utility classes per element.

## When to use semantic classes vs raw utilities

| Use semantic class | Use raw utilities |
|---|---|
| Component used in 3+ places | One-off layout for a single page |
| Reusable design pattern (button, card, input, alert) | Tweaking margin/padding for a specific instance |
| Anything that maps to a design token or brand decision | Quick prototyping |
| Patterns where state matters (loading, active, disabled) | Static text styling that won't repeat |
| Things designers might rename or rebrand | Implementation details that don't carry meaning |

When in doubt: **start with utilities, extract to semantic classes when you see the third repetition.** Don't extract pre-emptively.

## Token-based variations

Combine semantic naming with CSS variable tokens to centralize design decisions:

```css
@layer base {
  :root {
    --color-button-primary: theme('colors.blue.500');
    --color-button-primary-hover: theme('colors.blue.700');
    --radius-button: theme('borderRadius.lg');
  }

  .dark {
    --color-button-primary: theme('colors.blue.400');
    --color-button-primary-hover: theme('colors.blue.600');
  }
}

@layer components {
  .btn-primary {
    background-color: var(--color-button-primary);
    border-radius: var(--radius-button);
    @apply py-2 px-4 text-white font-semibold shadow-md;
  }

  .btn-primary:hover {
    background-color: var(--color-button-primary-hover);
  }
}
```

Component classes describe *intent* (`btn-primary`). Tokens describe *value* (`--color-button-primary`). Light/dark/multi-brand all change tokens; component classes don't change.

## Pairing with shadcn/ui

shadcn/ui uses CVA and inline utility strings inside component code. That's fine — those components are vendored into your repo and you own them. The semantic naming convention applies to **everything you build on top of shadcn**:

- Application-level layouts and patterns (page wrappers, section containers, dashboards) → semantic classes
- Component variants you compose from shadcn primitives → semantic classes
- One-off marketing pages with bespoke layouts → utilities are fine
- The shadcn primitives themselves → leave as-is, modify when needed

Pattern in practice:

```jsx
// Building a dashboard tile from shadcn Card primitive
<Card className="dashboard-tile">
  <CardHeader className="dashboard-tile-header">
    <CardTitle className="dashboard-tile-header-title">Revenue</CardTitle>
    <CardDescription className="dashboard-tile-header-subtitle">This month</CardDescription>
  </CardHeader>
  <CardContent className="dashboard-tile-body">
    {/* ... */}
  </CardContent>
</Card>
```

The shadcn classes (`Card`, `CardHeader`) are React components. The semantic classes (`dashboard-tile`, `dashboard-tile-header`) are app-level styles. They coexist cleanly.

## File organization

```
app/
├── globals.css              # @tailwind directives + global resets
├── styles/
│   ├── base.css             # @layer base — typography, root variables
│   ├── components/
│   │   ├── buttons.css      # @layer components — .btn-* classes
│   │   ├── cards.css        # @layer components — .card-* classes
│   │   ├── forms.css        # @layer components — .form-* classes
│   │   └── tables.css       # @layer components — .table-* classes
│   └── utilities.css        # @layer utilities — custom utility classes
```

Imported in order from `globals.css`:

```css
@tailwind base;
@import './styles/base.css';

@tailwind components;
@import './styles/components/buttons.css';
@import './styles/components/cards.css';
@import './styles/components/forms.css';
@import './styles/components/tables.css';

@tailwind utilities;
@import './styles/utilities.css';
```

Each component file is small enough to scan in 30 seconds. New patterns get a new file when they reach critical mass.

## Naming gotchas

- **Don't conflict with Tailwind utilities.** Naming a class `.flex` would override Tailwind's `flex`. Stick to multi-word names: `.flex-row`, `.card-flex`, etc.
- **Don't conflict with shadcn class names.** shadcn uses `data-*` attributes for state (`data-state="open"`). Your semantic classes shouldn't collide with those patterns.
- **Avoid abbreviations.** `.btn-primary` is fine because `btn` is universally understood. `.usr-prof-img` is too cryptic.
- **Use kebab-case.** Match the rest of the CSS world. JavaScript camelCase reserved for component props, not CSS.

## When NOT to extract to a semantic class

The biggest failure mode: extracting *too early* or *too aggressively*.

Don't extract if:
- The pattern is used in 1–2 places. Just inline.
- The "pattern" is actually a few utilities glued together (`flex items-center gap-2`). Inline is fine.
- The class would only ever be used inside one specific component. The component itself is the abstraction; you don't need a CSS abstraction *plus* a React abstraction.
- You're trying to make code look "cleaner" without a real reuse case.

The Tailwind team's [official guidance](https://tailwindcss.com/docs/reusing-styles): "If you start using `@apply` for everything, you are basically just writing CSS again and throwing away all of the workflow and maintainability advantages Tailwind gives you."

Use `@apply` when the **pattern itself** is the abstraction worth naming. Not as a default reflex.

## Refactoring checklist

When you see a long inline utility string, ask:

1. Is this pattern used in ≥3 places? → Extract to a semantic class.
2. Does it represent a brand-level decision (color, spacing, radius)? → Extract.
3. Does it have multiple states (hover, focus, active, disabled, loading)? → Strongly consider extracting.
4. Is it a one-off page layout? → Keep inline.
5. Is it a deeply nested utility chain that's hard to read? → Extract for readability even at 2 uses.

## The decision tree

```
Is this styling reused anywhere?
├── No → inline utilities
└── Yes →
    Is it a brand/system decision (color, spacing, type)?
    ├── Yes → extract semantic class with @apply + tokens
    └── No →
        Is it more than ~6 utilities?
        ├── Yes → extract for readability
        └── No → keep inline, revisit if it spreads
```

## Why this matters

Production codebases live for years. The team rotates. The brand changes. A new designer joins. The button color updates.

When styles live in one named place per pattern, those changes take 5 minutes. When they're scattered across 200 inline utility strings, they take a day and you'll miss three.

Utility-first is fast on day 1. Semantic component classes pay rent on day 365.

## Source

- Tailwind official docs: [Reusing Styles](https://tailwindcss.com/docs/reusing-styles), [Functions and Directives](https://tailwindcss.com/docs/functions-and-directives)
- BEM methodology adapted for hierarchical naming
- Real-world experience scaling Tailwind across multi-team codebases
