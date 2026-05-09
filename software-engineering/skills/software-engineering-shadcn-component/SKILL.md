---
name: software-engineering-shadcn-component
description: Add, customize, and compose shadcn/ui components in a Next.js + Tailwind project the right way — install via the shadcn CLI, theme via CSS variables, extend via wrapping rather than forking, and avoid the common mistake of editing primitives in place. Use this skill any time the user wants to add a new shadcn component, theme an existing one, build a composed block on top of shadcn primitives, or fix Tailwind/shadcn integration issues, even when they say "add a button," "build a card," "make a dialog," or "match our brand colors." Trigger whenever shadcn/ui surface area is being added or changed so we stay updateable.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-shadcn-component]
tags: [type/skill, plugin/software-engineering topic/software, topic/engineering]
status: active
---

# shadcn/ui Component Skill

shadcn/ui is not a component library — it's a copy-and-own pattern. The CLI drops source files into your repo, you own them, and you can edit them. The win is full control; the trap is that "full control" tempts you to edit primitives in place, which makes future updates painful and inconsistent.

This skill enforces the patterns that keep shadcn maintainable on a real project.

## Foundational references

This skill operates within two shared references at `../../references/`:

- **`shadcn-ecosystem.md`** — full map of the ecosystem: core components, MCP server, custom registries, pro-blocks libraries, agent skills, and which to reach for when. Read this when picking install paths or choosing between component sources.
- **`tailwind-semantic-naming.md`** — the project-wide convention for Tailwind: utility-first for one-offs, `@layer components` semantic classes for reusable patterns. Apply this when wrapping or composing shadcn primitives into app-level components.

### Cross-plugin inventory references (single source of truth)

The canonical catalog of shadcn primitives — and the design plugin's perspective on when to reach for each — lives in:

- **[[shadcn-component-inventory]]** at `../../../design/references/` — every shadcn/ui primitive, grouped by job, with composition patterns and "when to reach for shadcn vs. build custom" guidance.
- **[[shadcn-chart-inventory]]** at `../../../design/references/` — when implementing chart-bearing components.

For the anti-bland rules every primitive customization respects:
- **[[ai-tells-forbidden-patterns]]** — the shadcn-default ban applies first to this skill. shadcn in default state announces "AI-generated SaaS." Customize radii, colors, and shadow stacks before shipping.
- **[[design-taste]]** — the dials and pre-show checks.

## Sibling skills in this plugin

- **[[pro-blocks-composer]]** — for installing and composing complete UI sections (heroes, feature blocks, pricing, navbars, app shells) from shadcndesign Pro Blocks, shadcnblocks, etc. Use when the task is page-level composition, not single-primitive work.
- **[[shadcn-registry-author]]** — for building and distributing your own custom shadcn registry, packaging team-specific components for cross-project reuse.
- **[[tailwind-tokens]]** — for wiring design tokens into Tailwind config and CSS variables.
- **[[design-system-engineer]]** — for building or maintaining the design system in code (Storybook, governance, token architecture).
- **[[design-implementation]]** — for translating design artifacts into shadcn-grounded code with visual parity.

## When to use this skill

- Adding a shadcn primitive (button, dialog, form, etc.) for the first time.
- Building a composed component on top of shadcn primitives (a "block" — Hero, FAQ, Pricing).
- Theming the project — colors, radius, typography.
- Fixing a Tailwind + shadcn integration issue (variants not applying, dark mode broken, CSS variables missing).
- Introducing a new pattern to be reused (form patterns, data-display patterns).

## The four rules

1. **Install via the shadcn CLI.** Don't hand-copy source. The CLI keeps version, dependency, and registry source-of-truth correct.
2. **Theme via CSS variables, not Tailwind config edits.** Brand colors live in `globals.css` as `--background`, `--foreground`, `--primary`, etc. Tailwind references them. This is the only place to change colors.
3. **Wrap, don't fork.** Need a "Marketing CTA Button" with extra padding and an arrow icon? Make a new component in `components/blocks/` that imports `Button` and composes it. Do not edit `components/ui/button.tsx`.
4. **`components/ui/` is sacred.** Treat it as vendored code. Update via the CLI. Custom logic goes elsewhere.

## Project setup

If shadcn/ui hasn't been initialized yet:

```bash
pnpm dlx shadcn@latest init
```

This creates `components.json`, sets up `globals.css` with theme variables, configures Tailwind, and installs Radix and other base dependencies.

Sensible project defaults:
- Style: `default` for most projects, `new-york` for cleaner type (good fit for legal/financial/professional)
- Base color: `slate`, `zinc`, or `neutral` — override via CSS variables once design system is in place
- CSS variables: yes (always — easier theming, dark mode, multi-brand)
- Path aliases: `@/components`, `@/lib/utils`

## Installing via MCP server (recommended for AI-assisted work)

The shadcn MCP server lets you install components via natural language in any AI editor. After running `pnpm dlx shadcn@latest mcp init --client claude`, you can prompt:

- "Add a button, card, dialog, and form to this project"
- "What date-picker options does shadcn have? Pick the best one and install it."
- "Install a hero section from shadcndesign that has a CTA on the right"

The MCP handles the install, dependency resolution, and integration. See `shadcn-ecosystem.md` for full setup details.

For non-AI workflows, the CLI is still the right path:

## Adding a primitive

```bash
pnpm dlx shadcn@latest add button
pnpm dlx shadcn@latest add dialog
pnpm dlx shadcn@latest add form input label
```

The CLI writes to `components/ui/`. You import from there:

```tsx
import { Button } from "@/components/ui/button";
```

## Theming

All theme colors live in `src/styles/globals.css` (or `app/globals.css` depending on project layout):

```css
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222 47% 11%;
    --primary: 220 90% 56%;        /* brand primary */
    --primary-foreground: 0 0% 100%;
    --muted: 210 40% 96%;
    --muted-foreground: 215 16% 47%;
    --border: 214 32% 91%;
    --radius: 0.5rem;
    /* ...rest of the standard set */
  }

  .dark {
    --background: 222 47% 11%;
    --foreground: 0 0% 100%;
    /* ... */
  }
}
```

Values are **HSL channel triplets without `hsl()`**, because Tailwind's color functions wrap them. Don't use hex here unless you've changed the wrapping convention.

To change the brand: change `--primary`. Every shadcn component using `bg-primary` updates.

## The wrap-don't-fork pattern

When you need a styled variant of a primitive, create a wrapper:

```tsx
// src/components/blocks/cta-button.tsx
import { Button, type ButtonProps } from "@/components/ui/button";
import { ArrowRight } from "lucide-react";
import { cn } from "@/lib/utils";

type CtaButtonProps = ButtonProps & { withArrow?: boolean };

export function CtaButton({ withArrow = true, className, children, ...props }: CtaButtonProps) {
  return (
    <Button className={cn("gap-2 px-6", className)} {...props}>
      {children}
      {withArrow && <ArrowRight className="h-4 w-4" />}
    </Button>
  );
}
```

If multiple wrappers want the same visual variant, push it into the primitive's `cva` variants — that's a CLI re-add territory, but acceptable. Document it.

## Adding a `cva` variant to a primitive

If a new variant is genuinely a new primitive variant (e.g., a "ghost-destructive" button you'll use in 5 places), edit the primitive — but do it through the variant system, not by adding a one-off className branch:

```ts
// in components/ui/button.tsx
const buttonVariants = cva(
  "inline-flex items-center justify-center ...",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive: "...",
        outline: "...",
        secondary: "...",
        ghost: "...",
        link: "...",
        // NEW
        ghostDestructive: "text-destructive hover:bg-destructive/10",
      },
      // ...
    },
  },
);
```

Note this divergence from the registry in a comment so the next person re-adding the component doesn't blow it away:

```tsx
// LOCAL: ghostDestructive variant added 2026-04-15. Re-merge if you re-add via CLI.
```

## Forms

shadcn/ui's `Form` is `react-hook-form` + `zod` + Radix labels. The pattern:

```tsx
"use client";

import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import { z } from "zod";
import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";

const FormSchema = z.object({
  email: z.string().email("Enter a valid email."),
});

export function NewsletterForm() {
  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: { email: "" },
  });

  function onSubmit(values: z.infer<typeof FormSchema>) {
    // call server action
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Email</FormLabel>
              <FormControl>
                <Input type="email" placeholder="you@startup.com" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit">Subscribe</Button>
      </form>
    </Form>
  );
}
```

For Server Actions that don't need rich client validation, just use a plain `<form action={action}>` — see `software-engineering-nextjs-scaffold` server-action template. Don't use `react-hook-form` if you don't need its features.

## Composed blocks

Pages should compose blocks, not primitives directly, when the same visual unit appears in 2+ places. Common examples:

- `<Hero />` — a marketing hero with eyebrow, headline, subhead, CTAs.
- `<FaqSection />` — title + question/answer accordion.
- `<PricingTable />` — tier cards with feature list and CTAs.
- `<FeatureGrid />` — three or four columns of icon + headline + paragraph.

Put these in `src/components/blocks/`, kebab-case. Each block:

- Takes typed props.
- Uses primitives from `components/ui/`.
- Has no internal data fetching — data is passed as props from the page.
- Is documented with a JSDoc comment at the top describing its role.

**Before building blocks from scratch, check the pro-blocks libraries.** shadcndesign, shadcnblocks, shadcn.io, and Shadcn Studio ship hundreds of pre-built blocks for hero, features, pricing, navbar, app shells, sign-in, settings, and more. The `pro-blocks-composer` skill handles selection, installation, and visual rhythm reconciliation. Build custom only when no block fits the brand or the page expresses unique value that pre-built blocks can't capture.

## Semantic class naming for app-level styles

When wrapping shadcn primitives into app-level components, use the semantic naming convention from `tailwind-semantic-naming.md`. shadcn primitives use Tailwind utilities and CVA internally — that's fine, you own that code. But the application-level styles you compose around them should use semantic class names with `@layer components`:

```tsx
// Wrap a shadcn Card primitive into an app-level "dashboard tile"
<Card className="dashboard-tile">
  <CardHeader className="dashboard-tile-header">
    <CardTitle className="dashboard-tile-header-title">Revenue</CardTitle>
  </CardHeader>
  <CardContent className="dashboard-tile-body">
    {/* ... */}
  </CardContent>
</Card>
```

```css
@layer components {
  .dashboard-tile { @apply bg-card rounded-xl shadow-sm hover:shadow-md transition-shadow; }
  .dashboard-tile-header { @apply px-6 pt-5 pb-3 border-b; }
  .dashboard-tile-header-title { @apply text-lg font-semibold text-foreground; }
  .dashboard-tile-body { @apply px-6 py-5; }
}
```

The shadcn classes (`Card`, `CardHeader`) are React components. The semantic classes (`dashboard-tile`, `dashboard-tile-header`) are app-level styling. They coexist cleanly.

## Accessibility

shadcn primitives are built on Radix, which gives you correct ARIA out of the box for most cases. Don't break it:

- Don't replace `<DialogClose>` with a plain `<button>`. You'll lose focus management.
- Don't strip `data-state` attributes — they drive variant styling.
- Always pass labels to form controls. `FormLabel` wires `htmlFor`/`id` automatically; if you bypass `FormField`, you must wire it yourself.
- Use the `Button` primitive, not bare `<button>`, for keyboard focus-ring consistency.

A11y checks belong in their own pass — see the forthcoming `software-engineering-a11y-audit` skill.

## Updating shadcn components

Periodically the registry updates a primitive (bug fix, dependency bump). To pull updates:

```bash
pnpm dlx shadcn@latest add button --overwrite
```

If you've added local divergences (custom variants, see above), this will overwrite them. The fix is to:

1. Diff before overwriting.
2. Re-apply local changes after overwrite.
3. Note re-applies in the file with a `LOCAL:` comment.

## Common failure modes

- Editing `components/ui/*` for one-off styles. Wrap instead.
- Hardcoding hex colors in components. Use `bg-primary` etc.; change the variable.
- Setting `className="bg-blue-500"` instead of `bg-primary`. Loses theming.
- Importing `Button` from `@radix-ui/...` instead of the local primitive. Bypasses your variants.
- Skipping the CLI and pasting from the docs. Misses dependency wiring.
- Using `react-hook-form` + Zod when a Server Action with `useActionState` would do (see `software-engineering-nextjs-scaffold`).

## How this skill relates to others

- **File and folder conventions, page templates, server actions** → [[nextjs-scaffold]]
- **Translating a design artifact into code with visual parity** → [[design-implementation]]
- **Page-level composition from pre-built sections** → [[pro-blocks-composer]]
- **Authoring a custom registry to share components across projects** → [[shadcn-registry-author]]
- **Tokens and theming as a system** → [[tailwind-tokens]]
- **Component documentation pattern** → [[component-spec]]
- **Accessibility audit and remediation** → [[a11y-audit]]
- **Annotation that drives the implementation** → [[design-handoff]] (in design plugin)
- **Visual hierarchy, color, spacing principles** → `visual-design-foundations.md` reference (in design plugin)
- **State design, motion, dark patterns** → `interaction-patterns.md` reference (in design plugin)
