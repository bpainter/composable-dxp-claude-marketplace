---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[pro-blocks-composer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Composition Patterns

How to assemble multiple blocks into coherent pages — visual rhythm, integration, adaptation.

## The composition mindset

Composing pages from blocks is different from composing pages from components. Components are atomic; you fully control them. Blocks are pre-baked; they ship with their own visual decisions, and your job is to thread them together.

The mental model: **blocks are like furniture**. Each piece has its own style. Your job is to arrange them in a room that feels like one space, not a furniture showroom.

## The visual rhythm checklist

After installing all the blocks for a page, before declaring done, scan top-to-bottom and check:

### Vertical spacing rhythm

Are major sections consistently spaced from each other?

- **Desktop**: 80–120px between major sections
- **Mobile**: 48–80px between major sections

If blocks ship with different default vertical padding (one with 64px, another with 96px), wrap each block in your own section container and force consistent spacing:

```jsx
<section className="page-section">
  <HeroBlock />
</section>
<section className="page-section">
  <FeaturesBlock />
</section>
```

```css
.page-section {
  @apply py-20 md:py-24;
}
```

### Container widths

Pick one canonical max-width for the page (typically `max-w-7xl` for SaaS, `max-w-6xl` for marketing). Apply it consistently:

```css
.page-container {
  @apply mx-auto px-6 max-w-7xl;
}
```

If a block ships with a different max-width hard-coded, override via a wrapper.

### Type scale

Different blocks may ship with different H1 or H2 sizes. Pick one project-level type scale and force blocks to use it.

If a hero block uses `text-6xl` for H1 and a feature block uses `text-5xl` for H1, they'll feel like they're for different products. Standardize:

```css
.page-h1 { @apply text-5xl md:text-6xl font-bold tracking-tight; }
.page-h2 { @apply text-3xl md:text-4xl font-semibold tracking-tight; }
.page-h3 { @apply text-xl md:text-2xl font-semibold; }
```

Apply via class override or by patching the block.

### Color tokens

Replace any hard-coded block colors with your CSS variable tokens. Otherwise, when you change brand color, half the page updates and half doesn't.

Search the installed block for hex codes and Tailwind color utilities like `bg-blue-500`. Replace with `bg-primary` (your token).

### Border radius scale

Blocks may use `rounded-md`, `rounded-xl`, `rounded-2xl` inconsistently. Pick a canonical radius scale for the project and apply.

Project radius scale example:
```css
:root {
  --radius-sm: 0.5rem;   /* 8px — small elements */
  --radius-md: 0.75rem;  /* 12px — cards, default */
  --radius-lg: 1rem;     /* 16px — modals, hero cards */
  --radius-xl: 1.5rem;   /* 24px — large feature cards */
}
```

### Shadow stack

Same story for shadows. Pick an elevation scale, apply consistently.

### Icons

Different blocks may use different icon libraries (Lucide, Heroicons, custom SVG). Standardize on one (typically Lucide for shadcn projects).

If a block uses Heroicons, swap them for Lucide equivalents. The imports change, the layout doesn't.

## Composition recipes

### Marketing landing page

Standard sequence:

```
1. Navbar
2. Hero (with CTA)
3. Logo cloud / social proof bar
4. Feature section #1 (primary value prop)
5. Feature section #2 (secondary value prop)
6. Feature section #3 (tertiary value prop)
7. Stats or testimonials
8. Pricing
9. FAQ
10. CTA section (final push)
11. Footer
```

Variations:

- **Conversion-focused landing**: shorter — Navbar, Hero with form, 3 features, social proof, footer.
- **Long-form launch page**: extended — adds video, customer stories, FAQs, multi-section feature deep dives.
- **Simple product page**: navbar, hero, 2-3 features, single CTA, footer.

### SaaS dashboard

Standard structure:

```
App Shell:
├── Sidebar (collapsible)
│   ├── Logo + workspace switcher (top)
│   ├── Primary nav items
│   ├── Secondary nav items
│   └── User menu (bottom)
├── Top header
│   ├── Breadcrumbs
│   ├── Search (optional)
│   ├── Actions (notifications, help)
│   └── User avatar
└── Main content
    ├── Page header (title + actions)
    ├── (Tabs if needed)
    └── Content blocks (cards, tables, charts)
```

The shadcn `Sidebar` component handles the sidebar; pro blocks ship full app shells. Combine selectively.

### Settings flow

```
Page header (Settings)
├── Tabs or sidebar nav (Profile / Account / Billing / Team / Notifications / API)
└── Active pane content
    ├── Section header
    ├── Form fields (in FieldGroup)
    └── Save / Cancel actions
```

shadcndesign settings blocks cover most common panes. Pick the structure (tabbed vs sidebar) based on number of panes — sidebar wins above 5 panes.

### Sign-in / sign-up flow

```
1. Sign-up page (form OR OAuth-first)
2. Email verification page (instructions + resend button)
3. Onboarding (optional — first-time user info collection)
4. Forgot password (request)
5. Password reset (with token from email)
6. Account confirmed (success state)
```

Each is a separate route. Pro blocks ship templates for all 6.

## Brand token application

The fastest way to make a multi-block page feel cohesive: thread your design tokens through every block.

### Step 1: Define your tokens

```css
/* In your globals.css */
:root {
  /* Color */
  --primary: 222 47% 11%;
  --primary-foreground: 210 40% 98%;
  --accent: 217 91% 60%;
  --accent-foreground: 0 0% 100%;
  --muted: 210 40% 96%;
  --muted-foreground: 215 16% 47%;

  /* Spacing */
  --section-spacing-y: 6rem;
  --container-max-w: 80rem;

  /* Type scale */
  --font-display: 'Inter', sans-serif;
  --font-body: 'Inter', sans-serif;

  /* Radius */
  --radius-sm: 0.5rem;
  --radius-md: 0.75rem;
  --radius-lg: 1rem;
}
```

### Step 2: Apply to each block

Wrap each installed block, override block defaults via tokens or props. For shadcn primitives, the tokens flow through automatically because the components reference CSS vars.

For custom blocks that hard-code, do a search-and-replace pass after install.

### Step 3: Test in light + dark mode

A common failure: tokens work in light mode, break in dark. Toggle dark mode early and often. Fix tokens, not block code, when something looks off.

## Block-to-block transitions

The space between blocks is part of the design. Two patterns:

### Hard transition (default)

Each block has its own background, full-width:

```jsx
<section className="bg-background py-20">
  <HeroBlock />
</section>
<section className="bg-muted py-20">
  <FeaturesBlock />
</section>
```

Background color shifts indicate "new section, new context."

### Soft transition (single canvas)

Blocks share the same background; only spacing separates them:

```jsx
<div className="bg-background">
  <section className="py-20"><HeroBlock /></section>
  <section className="py-20"><FeaturesBlock /></section>
</div>
```

This feels more cohesive, less "magazine layout." Use when the page has a single voice throughout.

Mixing both styles on one page rarely looks intentional. Pick one and commit.

## Dark mode considerations

Most well-built blocks support dark mode via shadcn's CSS variable tokens. Common gotchas:

- **Hard-coded `bg-white`** — should be `bg-background`
- **Hard-coded `text-gray-900`** — should be `text-foreground` or `text-primary`
- **Images that only work on light backgrounds** — may need a dark variant or a frame around them
- **Logos in logo clouds** — grayscale logos may invisibly disappear on dark. Test.

After installing all blocks, tab through dark mode systematically.

## Mobile considerations

Block libraries are responsive but assume reasonable content. Watch for:

- **Long button labels** wrapping awkwardly
- **Hero subtitles** running too many lines on small screens
- **Pricing tables** that need horizontal scroll vs stack
- **Navbar** behavior — does the block ship with a mobile menu, or do you need to add one?
- **Touch targets** — pro blocks usually meet the 44pt minimum but verify

Test at 320px, 375px (iPhone SE), 768px (tablet), 1024px (small laptop), 1440px (default desktop) before declaring complete.

## Maintenance: tracking modified blocks

Once you fork or heavily customize a block, you can't easily update it from upstream. Keep a `BLOCKS-MODIFIED.md` file that lists:

- Block name + source URL
- Date forked / modified
- Why it was modified
- What was changed

When a block library releases updates, you'll know which of your blocks are pristine (safe to update) vs modified (manual merge required).

## Anti-patterns to avoid

- **Composing blocks that have wildly different visual languages.** Pick library-of-the-day mode and you'll spend more time reconciling than designing custom would have taken. Stick to 1–2 libraries per project.
- **Using "Most Popular" block tags as recommendations for your context.** What's most popular generally is not necessarily best for your audience.
- **Ignoring the block's structural assumptions.** A pricing block assumes 3 tiers; if you have 5, that block isn't for you. Find a different block, don't hack the existing one to add tiers.
- **Building a custom "page system" on top of blocks.** This is engineering yak-shaving. Just compose pages directly. Abstract only if you find yourself building 10+ similar pages.
- **Skipping the visual rhythm pass.** A page that's 80% there visually feels 30% there. The last 20% is the rhythm reconciliation.
