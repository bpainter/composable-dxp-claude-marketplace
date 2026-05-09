---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[design-system-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Token Architecture & Storybook Configuration

## Tailwind CSS v4 Token Architecture

### Three-Layer Token System

Design systems that scale use a three-layer token approach:

**Layer 1: Primitive Tokens**

Raw color, size, and typographic values with no semantic meaning. These are rarely used directly in components.

```css
:root {
  /* Colors */
  --color-blue-50: #eff6ff;
  --color-blue-100: #dbeafe;
  --color-blue-200: #bfdbfe;
  --color-blue-300: #93c5fd;
  --color-blue-400: #60a5fa;
  --color-blue-500: #3b82f6;
  --color-blue-600: #2563eb;
  --color-blue-700: #1d4ed8;
  --color-blue-800: #1e40af;
  --color-blue-900: #1e3a8a;

  --color-gray-50: #f9fafb;
  --color-gray-100: #f3f4f6;
  --color-gray-200: #e5e7eb;
  --color-gray-500: #6b7280;
  --color-gray-900: #111827;

  --color-red-600: #dc2626;

  /* Sizing */
  --size-4: 4px;
  --size-8: 8px;
  --size-12: 12px;
  --size-16: 16px;
  --size-20: 20px;
  --size-24: 24px;
  --size-32: 32px;

  /* Typography */
  --font-size-12: 12px;
  --font-size-14: 14px;
  --font-size-16: 16px;
  --font-size-18: 18px;
  --font-size-20: 20px;
  --font-size-28: 28px;
  --font-size-36: 36px;

  --line-height-tight: 1.2;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.6;
}
```

**Layer 2: Semantic Tokens**

Contextual aliases that map primitive values to design intent. These are what your components actually use.

```css
:root {
  /* Colors */
  --color-primary: var(--color-blue-600);
  --color-primary-hover: var(--color-blue-700);
  --color-primary-active: var(--color-blue-800);
  --color-primary-light: var(--color-blue-50);
  --color-primary-disabled: var(--color-gray-200);

  --color-surface: var(--color-gray-50);
  --color-surface-interactive: var(--color-gray-900);
  --color-surface-muted: var(--color-gray-100);
  --color-surface-border: var(--color-gray-200);

  --color-text: var(--color-gray-900);
  --color-text-secondary: var(--color-gray-500);
  --color-text-on-primary: #ffffff;

  --color-destructive: var(--color-red-600);
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-info: var(--color-blue-600);

  /* Spacing */
  --space-xs: var(--size-4);
  --space-sm: var(--size-8);
  --space-md: var(--size-12);
  --space-lg: var(--size-16);
  --space-xl: var(--size-24);
  --space-2xl: var(--size-32);

  /* Component-specific spacing */
  --space-gutter: var(--space-lg); /* Horizontal padding for pages */
  --space-section: var(--space-2xl); /* Vertical space between sections */
  --space-card-gap: var(--space-md); /* Space inside cards */

  /* Typography */
  --font-size-body-sm: var(--font-size-14);
  --line-height-body-sm: var(--line-height-normal);

  --font-size-body-md: var(--font-size-16);
  --line-height-body-md: var(--line-height-relaxed);

  --font-size-heading-lg: var(--font-size-28);
  --line-height-heading-lg: var(--line-height-tight);

  --font-size-heading-2xl: var(--font-size-36);
  --line-height-heading-2xl: var(--line-height-tight);

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.15);
  --shadow-xl: 0 20px 25px rgba(0, 0, 0, 0.2);

  /* Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
}

/* Dark mode: override semantic tokens */
@media (prefers-color-scheme: dark) {
  :root {
    --color-primary: var(--color-blue-500);
    --color-primary-hover: var(--color-blue-400);
    --color-surface: #1f2937;
    --color-surface-interactive: #f9fafb;
    --color-surface-muted: #374151;
    --color-surface-border: #4b5563;
    --color-text: #f9fafb;
    --color-text-secondary: #d1d5db;
    --color-text-on-primary: #ffffff;
  }
}
```

**Layer 3: Component Tokens (Optional)**

Rarely needed—most components derive from semantic tokens. Use when a specific component needs hardcoded defaults.

```css
:root {
  /* Button component defaults */
  --button-height-sm: var(--size-8);
  --button-height-md: var(--size-10); /* 40px */
  --button-height-lg: var(--size-12);
  --button-padding-x: var(--space-md);

  /* Input component defaults */
  --input-height: var(--size-10);
  --input-padding-x: var(--space-md);
  --input-border-width: 1px;

  /* Card component defaults */
  --card-padding: var(--space-lg);
  --card-border-radius: var(--radius-md);
}
```

### Tailwind Config Integration

Map semantic tokens into Tailwind config to get IDE autocomplete and class generation.

```typescript
// tailwind.config.ts
import type { Config } from 'tailwindcss';

export default {
  content: [
    './src/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50: 'var(--color-blue-50)',
          100: 'var(--color-blue-100)',
          200: 'var(--color-blue-200)',
          300: 'var(--color-blue-300)',
          400: 'var(--color-blue-400)',
          500: 'var(--color-blue-500)',
          600: 'var(--color-primary)',
          700: 'var(--color-blue-700)',
          800: 'var(--color-blue-800)',
          900: 'var(--color-blue-900)',
          DEFAULT: 'var(--color-primary)',
        },
        surface: {
          DEFAULT: 'var(--color-surface)',
          interactive: 'var(--color-surface-interactive)',
          muted: 'var(--color-surface-muted)',
          border: 'var(--color-surface-border)',
        },
        destructive: 'var(--color-destructive)',
        success: 'var(--color-success)',
        warning: 'var(--color-warning)',
        info: 'var(--color-info)',
        text: {
          DEFAULT: 'var(--color-text)',
          secondary: 'var(--color-text-secondary)',
          'on-primary': 'var(--color-text-on-primary)',
        },
      },
      spacing: {
        xs: 'var(--space-xs)',
        sm: 'var(--space-sm)',
        md: 'var(--space-md)',
        lg: 'var(--space-lg)',
        xl: 'var(--space-xl)',
        '2xl': 'var(--space-2xl)',
        gutter: 'var(--space-gutter)',
        section: 'var(--space-section)',
        'card-gap': 'var(--space-card-gap)',
      },
      fontSize: {
        'body-sm': [
          'var(--font-size-body-sm)',
          { lineHeight: 'var(--line-height-body-sm)' },
        ],
        'body-md': [
          'var(--font-size-body-md)',
          { lineHeight: 'var(--line-height-body-md)' },
        ],
        'heading-lg': [
          'var(--font-size-heading-lg)',
          { lineHeight: 'var(--line-height-heading-lg)', fontWeight: '600' },
        ],
        'heading-2xl': [
          'var(--font-size-heading-2xl)',
          { lineHeight: 'var(--line-height-heading-2xl)', fontWeight: '700' },
        ],
      },
      boxShadow: {
        sm: 'var(--shadow-sm)',
        md: 'var(--shadow-md)',
        lg: 'var(--shadow-lg)',
        xl: 'var(--shadow-xl)',
      },
      borderRadius: {
        sm: 'var(--radius-sm)',
        md: 'var(--radius-md)',
        lg: 'var(--radius-lg)',
      },
    },
  },
} satisfies Config;
```

### Dark Mode Implementation

Use CSS variables + media queries for automatic dark mode support.

**Approach 1: Prefer Color Scheme (No JS)**

Simplest: let OS settings control dark mode.

```css
/* globals.css */
:root {
  color-scheme: light;
}

@media (prefers-color-scheme: dark) {
  :root {
    color-scheme: dark;
    --color-primary: var(--color-blue-500);
    --color-surface: #1f2937;
    --color-text: #f9fafb;
    /* ... other dark mode overrides */
  }
}
```

**Approach 2: Data Attribute Toggle (User Control)**

If you need an explicit theme selector:

```typescript
// lib/theme.ts
export type Theme = 'light' | 'dark' | 'system';

export function setTheme(theme: Theme) {
  const html = document.documentElement;

  if (theme === 'system') {
    html.removeAttribute('data-theme');
  } else {
    html.setAttribute('data-theme', theme);
  }

  // Persist preference
  localStorage.setItem('theme', theme);
}

export function getTheme(): Theme {
  if (typeof window === 'undefined') return 'system';
  return (localStorage.getItem('theme') as Theme) || 'system';
}

export function initTheme() {
  const theme = getTheme();
  setTheme(theme);
}
```

```css
/* globals.css */
:root {
  color-scheme: light;
}

/* Light mode (explicit) */
[data-theme='light'] {
  color-scheme: light;
}

/* Dark mode (explicit) */
[data-theme='dark'] {
  color-scheme: dark;
  --color-primary: var(--color-blue-500);
  --color-surface: #1f2937;
  --color-text: #f9fafb;
}

/* Dark mode (system preference) */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme='light']) {
    color-scheme: dark;
    --color-primary: var(--color-blue-500);
    --color-surface: #1f2937;
    --color-text: #f9fafb;
  }
}
```

```tsx
// app/providers.tsx
'use client';

import { useEffect } from 'react';
import { initTheme } from '@/lib/theme';

export function Providers({ children }: { children: React.ReactNode }) {
  useEffect(() => {
    initTheme();
  }, []);

  return children;
}
```

### Token Naming Conventions

Clear naming prevents confusion and makes refactoring easier.

**Pattern: `--category-property-variant`**

```
--color-primary           /* Primary action color */
--color-primary-hover     /* Hover state variant */
--color-surface           /* Background surface */
--color-surface-border    /* Border on surface */
--space-md                /* Medium spacing unit */
--font-size-body-md       /* Body medium typography */
--shadow-lg               /* Large shadow depth */
--radius-md               /* Medium border radius */
```

**Do's:**
- Use semantic names: `primary`, `destructive`, `surface`, `text`
- Group related tokens: `--color-*`, `--space-*`, `--font-*`
- Be consistent: all color tokens start with `--color-`
- Name for intent, not color: `--color-destructive` not `--color-red`

**Don'ts:**
- Don't use abstract names: `--color-1`, `--size-a`, `--neutral-shade-2`
- Don't repeat the category in the property: `--color-color-primary` is redundant
- Don't mix layers: don't use primitive names in component code

---

## Storybook 8 Setup for Next.js

### Installation & Initial Config

```bash
npx storybook@latest init --type next
```

This scaffolds:
- `.storybook/main.ts`
- `.storybook/preview.ts`
- `src/stories/` with example stories

### Main Configuration (`.storybook/main.ts`)

```typescript
import type { StorybookConfig } from '@storybook/nextjs';

const config: StorybookConfig = {
  stories: [
    '../src/**/*.stories.@(js|jsx|mjs|ts|tsx)',
  ],
  addons: [
    '@storybook/addon-onboarding',
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-interactions',
    '@storybook/addon-a11y',
    '@storybook/addon-themes',
    '@storybook/addon-viewport',
  ],
  framework: {
    name: '@storybook/nextjs',
    options: {
      nextConfigPath: './next.config.ts',
    },
  },
  docs: {
    autodocs: 'tag',
  },
  staticDirs: ['../public'],
};
export default config;
```

### Preview Configuration (`.storybook/preview.ts`)

```typescript
import type { Preview } from '@storybook/react';
import { withThemeByDataAttribute } from '@storybook/addon-themes';

// Import your global styles
import '../src/globals.css';

const preview: Preview = {
  parameters: {
    actions: { argTypesRegex: '^on[A-Z].*' },
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/i,
      },
    },
    // Layout
    layout: 'centered', // or 'fullscreen', 'padded'
    // Viewport sizes
    viewport: {
      viewports: {
        mobile: {
          name: 'Mobile',
          styles: {
            width: '375px',
            height: '667px',
          },
          type: 'mobile',
        },
        tablet: {
          name: 'Tablet',
          styles: {
            width: '768px',
            height: '1024px',
          },
          type: 'tablet',
        },
        desktop: {
          name: 'Desktop',
          styles: {
            width: '1280px',
            height: '800px',
          },
          type: 'desktop',
        },
      },
    },
    // Accessibility addon config
    a11y: {
      config: {
        rules: [
          {
            id: 'color-contrast',
            enabled: true,
          },
          {
            id: 'valid-aria-role',
            enabled: true,
          },
          {
            id: 'button-name',
            enabled: true,
          },
        ],
      },
      options: {
        checks: { 'valid-aria-role': { enabled: true } },
        runOnly: { type: 'tag', values: ['wcag2aa', 'wcag21aa'] },
      },
    },
  },
  decorators: [
    // Theme switcher decorator
    withThemeByDataAttribute({
      themes: {
        light: 'light',
        dark: 'dark',
      },
      defaultTheme: 'light',
      attributeName: 'data-theme',
    }),
  ],
};

export default preview;
```

### Story Organization

Organize stories by atomic design layers:

```
src/
├── stories/
│   ├── atoms/
│   │   ├── Button.stories.tsx
│   │   ├── Input.stories.tsx
│   │   ├── Badge.stories.tsx
│   │   ├── Avatar.stories.tsx
│   │   └── Icon.stories.tsx
│   ├── molecules/
│   │   ├── FormField.stories.tsx
│   │   ├── Card.stories.tsx
│   │   ├── Dialog.stories.tsx
│   │   ├── Tooltip.stories.tsx
│   │   └── DropdownMenu.stories.tsx
│   ├── organisms/
│   │   ├── Form.stories.tsx
│   │   ├── DataTable.stories.tsx
│   │   ├── Sidebar.stories.tsx
│   │   └── Header.stories.tsx
│   └── templates/
│       ├── DashboardLayout.stories.tsx
│       └── SettingsPage.stories.tsx
```

This matches your folder structure and makes navigation in Storybook intuitive.

### Story Template (Best Practices)

```typescript
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from '../components/ui/button';

const meta: Meta<typeof Button> = {
  title: 'Atoms/Button',
  component: Button,
  tags: ['autodocs'], // Enable automatic documentation
  parameters: {
    layout: 'centered',
  },
  argTypes: {
    appearance: {
      control: 'select',
      options: ['primary', 'secondary', 'ghost', 'destructive'],
      description: 'Visual style variant',
    },
    size: {
      control: 'select',
      options: ['sm', 'md', 'lg'],
      description: 'Button size',
    },
    disabled: {
      control: 'boolean',
      description: 'Whether button is disabled',
    },
    children: {
      control: 'text',
      description: 'Button label text',
    },
  },
};

export default meta;
type Story = StoryObj<typeof meta>;

/**
 * Primary action button. Use for the main call-to-action.
 */
export const Primary: Story = {
  args: {
    appearance: 'primary',
    size: 'md',
    children: 'Click me',
  },
};

/**
 * Secondary action button. Use for alternative actions.
 */
export const Secondary: Story = {
  args: {
    appearance: 'secondary',
    size: 'md',
    children: 'Secondary',
  },
};

/**
 * Ghost style button. Use for less prominent actions.
 */
export const Ghost: Story = {
  args: {
    appearance: 'ghost',
    size: 'md',
    children: 'Ghost',
  },
};

/**
 * Destructive button. Use for deletion or irreversible actions.
 */
export const Destructive: Story = {
  args: {
    appearance: 'destructive',
    size: 'md',
    children: 'Delete',
  },
};

/**
 * All available sizes.
 */
export const AllSizes: Story = {
  render: () => (
    <div className="flex gap-4 items-center">
      <Button size="sm">Small</Button>
      <Button size="md">Medium</Button>
      <Button size="lg">Large</Button>
    </div>
  ),
};

/**
 * Disabled state. Button cannot be clicked.
 */
export const Disabled: Story = {
  args: {
    disabled: true,
    children: 'Disabled',
  },
};

/**
 * Loading state. Typically used with spinner or loading indicator.
 */
export const Loading: Story = {
  args: {
    disabled: true,
    children: 'Loading...',
  },
};

/**
 * With icon content.
 */
export const WithIcon: Story = {
  args: {
    children: '→ Next Step',
  },
};
```

### Essential Addons Configuration

**Accessibility Addon (@storybook/addon-a11y)**

Automatically scans components for WCAG violations:
- Color contrast issues
- Missing ARIA labels
- Keyboard navigation problems
- Semantic HTML errors

Enable in stories with:
```typescript
export const PrimaryButton: Story = {
  args: { ... },
  parameters: {
    a11y: {
      disable: false, // Enable accessibility checks
    },
  },
};
```

**Themes Addon (@storybook/addon-themes)**

Switch between light and dark themes directly in Storybook.

```typescript
// preview.ts
import { withThemeByDataAttribute } from '@storybook/addon-themes';

const preview: Preview = {
  decorators: [
    withThemeByDataAttribute({
      themes: {
        light: 'light',
        dark: 'dark',
      },
      defaultTheme: 'light',
      attributeName: 'data-theme',
    }),
  ],
};
```

**Viewport Addon (@storybook/addon-viewport)**

Test responsive behavior by switching viewports:
- Mobile (375px)
- Tablet (768px)
- Desktop (1280px)

**Interactions Addon (@storybook/addon-interactions)**

Test user interactions (clicks, input, form submission) visually.

```typescript
import { fn } from '@storybook/test';

export const ClickableButton: Story = {
  args: {
    onClick: fn(),
  },
  play: async ({ canvasElement }) => {
    // Interaction testing
    const button = canvasElement.querySelector('button');
    await userEvent.click(button);
  },
};
```

### Autodocs Configuration

Automatically generate component documentation from stories and types.

Enable with `tags: ['autodocs']` in Meta:

```typescript
const meta: Meta<typeof Button> = {
  tags: ['autodocs'],
  // ...
};
```

This generates:
- Component description (from comment above export)
- Props table (from argTypes + TypeScript types)
- Accessible variants (from stories)
- Usage examples (rendered stories)

No manual documentation needed—it's automatic.

### Storybook in CI/CD

**Run checks in CI:**

```bash
# Build Storybook
npm run build-storybook

# Run a11y checks
npm run test-storybook:a11y

# Run visual regression (Chromatic)
npx chromatic --project-token=<TOKEN>
```

**GitHub Actions workflow:**

```yaml
name: Storybook Tests
on: [pull_request, push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci && npm run build-storybook
      - run: npm run test-storybook:a11y
      - uses: chromaui/action@v9
        with:
          projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}
```

---

## Component Documentation Template

Use this template for every component's stories:

```markdown
# Component Name

## Usage

The component is used for [brief description of purpose].

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `appearance` | 'primary' \| 'secondary' | 'primary' | Visual style variant |
| `size` | 'sm' \| 'md' \| 'lg' | 'md' | Component size |
| `disabled` | boolean | false | Whether component is disabled |

## Examples

### Primary Action
Used for the main call-to-action on a page.

### Secondary Action
Used for alternative or less prominent actions.

### Disabled State
Component cannot be interacted with.

## Accessibility

- Keyboard navigation: supported
- Screen reader: announces label and state
- Color contrast: WCAG AA compliant
- Focus indicator: visible on focus
```

This ensures consistency and makes documentation easy to maintain.
