---
name: software-engineering-tailwind-tokens
description: Wire design tokens into Tailwind CSS and CSS variables so the codebase consumes a single source of truth — color, spacing, typography, radius, elevation, motion. Covers the token mapping (HSL channel triplets, CSS variables, Tailwind theme extension), light/dark mode, multi-brand support, drift prevention against the design tool, and the token-export pipeline. Use this skill any time the user is setting up or evolving the project's token layer — adding a new color, fixing dark mode, syncing Figma tokens to code, "why isn't bg-primary working" — even when "tokens" isn't said. Trigger whenever Tailwind theming and the design system's token contract are at stake.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-tailwind-tokens]
tags: [type/skill, plugin/software-engineering topic/software, topic/engineering]
status: active
---

# Tailwind Tokens

This skill is the code-side implementation of the design system's token layer. The design tool (Figma) and the codebase share a single token vocabulary; this skill is how the codebase honors the contract.

The default stack is Tailwind CSS + shadcn/ui + Next.js. shadcn ships with a CSS-variables-based theming approach, and this skill leans into it. For component primitives that consume these tokens, see `shadcn-component`. For design-system governance and Storybook, see `design-system-engineer`.

## Foundational references

This skill operates within two shared references at `../../references/`:

- **`tailwind-semantic-naming.md`** — when to extract semantic component classes via `@layer components` and `@apply` vs when to stay inline with utilities. Tokens are the *values*; semantic class names are the *intent*. They work together: tokens centralize design decisions, semantic classes apply those decisions in named patterns.
- **`shadcn-ecosystem.md`** — how tokens propagate through the shadcn ecosystem: CSS variable conventions, theme distribution via custom registries, and the token contract between Figma and code.

## Sibling skills

- **[[shadcn-component]]** — the components that consume your tokens
- **[[shadcn-registry-author]]** — package and distribute themes (token sets) via a custom registry
- **[[design-system-engineer]]** — Storybook, governance, design-system as a whole
- **[[design-implementation]]** — translating Figma designs into token-aware code

## When to use this skill

- Setting up the project's theme and token system for the first time.
- Adding a new token (color, spacing, typography, radius, motion).
- Fixing dark mode or theme switching.
- Diagnosing why a Tailwind utility class isn't producing the expected color or spacing.
- Setting up a token-export pipeline (Style Dictionary, Figma Tokens, etc.) so design and code stay in sync.
- Supporting multiple brands or sub-brands from one codebase.
- Auditing the codebase for hardcoded values that should be tokens.

## Core posture

- **One source of truth.** CSS variables in `globals.css` are the resolved token values. Tailwind theme extension references those variables. shadcn primitives reference the Tailwind utility classes. The chain is: design token → CSS variable → Tailwind class → component.
- **Semantic, not literal.** `--primary` not `--blue-600`. Re-theming becomes safe.
- **HSL channel triplets, not full HSL strings.** `--primary: 220 90% 56%` not `--primary: hsl(220 90% 56%)`. Tailwind wraps the value with its own color functions.
- **Light/dark via class swap, not media query.** A `.dark` class on `<html>` toggles the dark token values. Lets the user override system preference.
- **Drift is a defect.** When the design file and the code show different values for the same token, fix the pipeline, not the symptom.

## The token chain (how it actually works)

```
1. Figma variable           color/brand/primary  →  hsl(220 90% 56%)
2. Token export pipeline    color/brand/primary  →  --primary: 220 90% 56%
3. CSS variable             --primary: 220 90% 56%
4. Tailwind theme           primary: hsl(var(--primary))
5. Utility class            bg-primary, text-primary, border-primary
6. Component                <Button className="bg-primary text-primary-foreground">
```

Each link is a contract. Break any one and the system drifts.

## Setup: the canonical files

Three files do most of the work.

### `app/globals.css` (or `src/styles/globals.css`)

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    /* Color — semantic, HSL channel triplets */
    --background: 0 0% 100%;
    --foreground: 222 47% 11%;
    --primary: 220 90% 56%;
    --primary-foreground: 0 0% 100%;
    --secondary: 210 40% 96%;
    --secondary-foreground: 222 47% 11%;
    --muted: 210 40% 96%;
    --muted-foreground: 215 16% 47%;
    --accent: 210 40% 96%;
    --accent-foreground: 222 47% 11%;
    --destructive: 0 84% 60%;
    --destructive-foreground: 0 0% 100%;
    --border: 214 32% 91%;
    --input: 214 32% 91%;
    --ring: 220 90% 56%;

    /* Surfaces */
    --card: 0 0% 100%;
    --card-foreground: 222 47% 11%;
    --popover: 0 0% 100%;
    --popover-foreground: 222 47% 11%;

    /* Radius */
    --radius: 0.5rem;
  }

  .dark {
    --background: 222 47% 11%;
    --foreground: 0 0% 100%;
    --primary: 220 90% 60%;
    --primary-foreground: 222 47% 11%;
    /* ...remaining dark-mode overrides */
  }

  /* Set base body styles */
  body {
    @apply bg-background text-foreground;
    font-feature-settings: "rlig" 1, "calt" 1;
  }
}
```

### `tailwind.config.ts`

```ts
import type { Config } from "tailwindcss";

export default {
  darkMode: ["class"],
  content: ["./src/**/*.{ts,tsx}"],
  theme: {
    container: {
      center: true,
      padding: "2rem",
      screens: { "2xl": "1400px" },
    },
    extend: {
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
          foreground: "hsl(var(--destructive-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
        popover: {
          DEFAULT: "hsl(var(--popover))",
          foreground: "hsl(var(--popover-foreground))",
        },
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))",
        },
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
      fontFamily: {
        sans: ["var(--font-sans)", "system-ui", "sans-serif"],
        serif: ["var(--font-serif)", "Georgia", "serif"],
        mono: ["var(--font-mono)", "ui-monospace", "monospace"],
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
} satisfies Config;
```

### Font loading (`app/layout.tsx`)

Use Next.js's font loaders so font CSS variables flow through cleanly:

```tsx
import { Inter, Source_Serif_4, JetBrains_Mono } from "next/font/google";

const fontSans = Inter({ subsets: ["latin"], variable: "--font-sans" });
const fontSerif = Source_Serif_4({ subsets: ["latin"], variable: "--font-serif" });
const fontMono = JetBrains_Mono({ subsets: ["latin"], variable: "--font-mono" });

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" suppressHydrationWarning className={`${fontSans.variable} ${fontSerif.variable} ${fontMono.variable}`}>
      <body className="font-sans">{children}</body>
    </html>
  );
}
```

## Token categories — defaults and rules

### Color

- All colors in HSL channel triplets, no leading `hsl()`.
- Tokens named semantically (`primary`, `muted`, `destructive`, `border`), not by hue.
- Foreground tokens travel with their background tokens (`primary` + `primary-foreground`).
- WCAG-checked at definition time. Each background/foreground pair must hit AA contrast (or be flagged).
- Dark mode is a re-mapping, not a separate naming scheme. Same token names, different values.

### Spacing

- Tailwind ships a 4-pt-based scale. Use it; do not override unless you have a system reason.
- For an 8-pt grid, use even steps (`p-2`, `p-4`, `p-8`, `p-12`, `p-16`) and avoid the odd ones.
- Custom spacing tokens are rare. When needed, name semantically (`--spacing-section-y: 8rem`).

### Typography

- Three families: sans (default), serif (where called for), mono (code/numerals). Loaded via `next/font` with CSS-variable fallbacks.
- Type scale uses Tailwind's defaults (`text-sm`, `text-base`, `text-lg`, `text-xl`, etc.) unless the design system specifies a different scale.
- Line heights set per scale step.
- Weight tokens via Tailwind's `font-medium`, `font-semibold`, `font-bold`, etc.
- Reading-comfort utilities (`leading-relaxed`, `tracking-tight`) used judiciously, not by default.

### Radius

- One source: `--radius` in CSS, derived steps in Tailwind (`rounded-sm`, `rounded-md`, `rounded-lg`).
- Updating the radius means updating one variable.

### Elevation (shadow)

- Tailwind defaults are usually fine: `shadow-sm`, `shadow-md`, `shadow-lg`, `shadow-xl`.
- For a project-specific elevation system, define `--shadow-sm`, `--shadow-md`, etc., and extend Tailwind's `boxShadow`.

### Motion

- Use `tailwindcss-animate` for keyframes (already in shadcn defaults).
- Project-specific timing tokens via CSS variables: `--motion-duration-fast: 150ms`, `--motion-easing-standard: cubic-bezier(0.2, 0.0, 0, 1)`.
- Honor `prefers-reduced-motion`. Set up a global rule:
  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation-duration: 0.01ms !important;
      animation-iteration-count: 1 !important;
      transition-duration: 0.01ms !important;
      scroll-behavior: auto !important;
    }
  }
  ```

### Breakpoints

- Tailwind defaults (`sm: 640px`, `md: 768px`, `lg: 1024px`, `xl: 1280px`, `2xl: 1400px`) match shadcn assumptions.
- Custom breakpoints are rare. When needed, document the reason.

## Light / dark mode

```tsx
// providers.tsx
"use client";
import { ThemeProvider as NextThemesProvider } from "next-themes";

export function ThemeProvider({ children, ...props }: React.ComponentProps<typeof NextThemesProvider>) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>;
}
```

```tsx
// layout.tsx
import { ThemeProvider } from "@/components/providers";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body>
        <ThemeProvider attribute="class" defaultTheme="system" enableSystem>
          {children}
        </ThemeProvider>
      </body>
    </html>
  );
}
```

`next-themes` toggles the `.dark` class on `<html>`; the CSS variables in `.dark { ... }` take over.

`suppressHydrationWarning` on `<html>` prevents the React warning when the theme class is applied client-side; required when using `next-themes`.

## Multi-brand support (Phase 2+)

If the project later needs multiple brand themes (e.g., a primary brand + a sub-brand), use class-based theming:

```css
.theme-primary {
  --primary: 220 90% 56%;
  --secondary: 210 40% 96%;
  /* ... */
}

.theme-subbrand {
  --primary: 12 76% 61%;
  --secondary: 30 30% 92%;
  /* ... */
}
```

Wrap routes or layouts with the relevant theme class. `dark` mode still works on top.

## Token-export pipeline (design ↔ code sync)

For Phase 2, when the design system matures, automate the token export from Figma to code. Options:

- **Style Dictionary** — most flexible; pipeline transforms tokens to many formats.
- **Figma Tokens / Tokens Studio** — Figma-native; exports JSON.
- **Custom Figma Variables export** — Figma's native variables can be exported via API.

The output is a JSON file that the codebase consumes (either committed to the repo or pulled at build time). The CSS-variable layer in `globals.css` is generated from this source — designers don't edit `globals.css`, the pipeline does.

For Phase 1 (small token set, single contributor doing both design and code), a manual sync is fine. Document the manual sync in the repo so it doesn't become a broken-by-default invisible thing.

## Common failure modes

- **Hardcoded hex in components.** Use semantic tokens via Tailwind classes. Catch in audit (`design-audit`) and code review.
- **`bg-blue-500` instead of `bg-primary`.** Loses theming. Always use the semantic token.
- **HSL with `hsl()` wrapper in CSS variables.** `--primary: hsl(220 90% 56%)` doesn't compose with Tailwind's `hsl(var(--primary) / 0.5)` opacity syntax. Use channel triplets.
- **Dark mode bolted on at the end.** Bake it in from the start. Even if the project doesn't ship with dark mode initially, structure the variables for it.
- **Token names by hue.** `--blue-primary` makes rebranding painful. Stay semantic.
- **Two sources of truth.** Designers using Figma tokens, engineers maintaining `globals.css` separately. Drift within a sprint. Pipeline or single source.
- **`!important` to fix Tailwind specificity.** Almost always a sign that the token system is wrong. Fix at the token / variant layer.
- **Custom CSS for things Tailwind utilities cover.** Reach for utilities first; fall to custom CSS only for genuinely novel needs.
- **Adding tokens "just in case."** Each token is a maintenance liability. Add when there's a real consumer.

## Auditing for hardcoded values

Quick grep audit:

```bash
# Find raw hex colors in component source
grep -rE '#[0-9a-fA-F]{3,8}' src/components/

# Find raw rgba/rgb
grep -rE 'rgba?\(' src/components/

# Find raw px values that should be spacing tokens (judgment call)
grep -rE '\b\d+px\b' src/components/
```

Each hit is a candidate; not every hit is a defect. Margin / size values inside CSS-in-JS dynamic styles may legitimately be raw. Border radius and color rarely should be.

## Outputs

### Token reference (markdown)

```markdown
# Project Tokens

## Color
| Token | CSS variable | Light value | Dark value | Used by |
|---|---|---|---|---|
| color/brand/primary | --primary | 220 90% 56% | 220 90% 60% | Primary button, links, focus ring |
| color/text/default | --foreground | 222 47% 11% | 0 0% 100% | Body text |
| ... | ... | ... | ... | ... |

## Spacing
[Tailwind default scale, with notes on grid alignment.]

## Typography
[Type scale with px and rem values per step.]

## Radius
| Step | Value |
|---|---|
| sm | calc(var(--radius) - 4px) |
| md | calc(var(--radius) - 2px) |
| lg | var(--radius) |

## Motion
[Duration and easing tokens.]
```

### Token review checklist

- [ ] Every token semantic-named (no hue / step-number naming).
- [ ] Light + dark values defined for every color token.
- [ ] Foreground / background pairs WCAG AA-checked.
- [ ] HSL channel triplets in CSS variables.
- [ ] Tailwind config references CSS variables (no duplicated values).
- [ ] `prefers-reduced-motion` rule in place.
- [ ] No hardcoded hex / rgba in components.
- [ ] Token names match design tool variable names.
- [ ] Sync mechanism documented (manual or automated).

## How this skill relates to others

- The system-level governance and inclusion criteria for tokens → `brand-identity-system`.
- Identity decisions (palette, type) that drive the token values → [[brand-designer]].
- Component primitives that consume tokens → `software-engineering-shadcn-component`.
- Application-level conventions → `software-engineering-nextjs-scaffold`.
- Component-spec documentation pattern → `software-engineering-component-spec`.
- Accessibility validation of contrast and motion → `software-engineering-a11y-audit`.
- Visual-parity validation when implementing a design → `software-engineering-design-implementation`.

## Source material

- Tailwind CSS docs: https://tailwindcss.com/docs/theme
- shadcn/ui theming: https://ui.shadcn.com/docs/theming
- next-themes: https://github.com/pacocoursey/next-themes
- Style Dictionary: https://amzn.github.io/style-dictionary/
- Tokens Studio for Figma: https://tokens.studio/
- WCAG 2.2 contrast guidance: https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum
