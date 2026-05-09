---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[shadcn-registry-author]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Registry Item Schema

The complete schema for a registry item, with every field explained and example combinations.

## Top-level fields

```typescript
interface RegistryItem {
  // Required
  name: string;                              // Unique identifier within the registry
  type: RegistryItemType;                    // What kind of item this is

  // Optional metadata
  description?: string;                      // Short description for docs and CLI
  title?: string;                            // Display title (defaults to name)
  author?: string;                           // Author name or email

  // Dependencies
  dependencies?: string[];                   // npm packages this needs
  devDependencies?: string[];                // npm dev packages
  registryDependencies?: string[];           // Other registry items (names or URLs)

  // Files (the actual code distribution)
  files: RegistryFile[];

  // Theme integration
  tailwind?: TailwindConfig;                 // Tailwind config to merge
  cssVars?: CssVars;                         // CSS variables to add

  // CLI hooks
  meta?: Record<string, any>;                // Custom metadata
  docs?: string;                             // URL or path to docs
}
```

## `type` enum

```typescript
type RegistryItemType =
  | "registry:component"   // UI component
  | "registry:block"       // Composed UI section
  | "registry:hook"        // React hook
  | "registry:lib"         // Utility library
  | "registry:page"        // Full page template
  | "registry:file"        // Generic file (config, schema, etc.)
  | "registry:theme"       // Design tokens / theme
  | "registry:style"       // Style preset
  | "registry:ui"          // Lower-level UI primitive
  | "registry:example";    // Demo / example code
```

The CLI uses this to determine where to place files in the consumer project. For example:

- `registry:component` → `components/`
- `registry:hook` → `hooks/`
- `registry:lib` → `lib/`

These can be customized via the consumer's `components.json` aliases.

## `files` array

Each file entry:

```typescript
interface RegistryFile {
  path: string;                              // Relative path in consumer project
  type: RegistryItemType;                    // Same enum as parent
  content?: string;                          // Inline file content
  target?: string;                           // Override path for this file specifically
}
```

Files can either inline `content` (used during build) or reference source files in your registry directory (the build process inlines them).

Example:

```json
{
  "files": [
    {
      "path": "components/card-with-icon.tsx",
      "type": "registry:component"
    },
    {
      "path": "components/card-with-icon-icon.tsx",
      "type": "registry:component"
    }
  ]
}
```

## `dependencies` vs `registryDependencies`

This is where most registries break. Get both right.

### `dependencies`

npm packages the consumer needs. The CLI installs these via the consumer's package manager.

```json
{
  "dependencies": [
    "lucide-react",
    "date-fns",
    "react-day-picker"
  ]
}
```

Only list **direct, non-shadcn dependencies**. Don't list React or Tailwind — they're assumed.

### `registryDependencies`

Other registry items this depends on. The CLI installs them recursively.

For items from the **official shadcn registry**, use the bare name:

```json
{
  "registryDependencies": ["card", "button", "tooltip"]
}
```

For items from **another registry** (including your own), use the full URL:

```json
{
  "registryDependencies": [
    "card",
    "https://your-registry.vercel.app/r/default/icon-set.json"
  ]
}
```

The CLI walks the dependency tree and installs everything in topological order.

## `tailwind` config

If your component uses custom Tailwind config (animations, colors, plugins), declare them here. The CLI merges these into the consumer's `tailwind.config.ts`.

```json
{
  "tailwind": {
    "config": {
      "theme": {
        "extend": {
          "colors": {
            "brand": "hsl(var(--brand))"
          },
          "animation": {
            "fade-in": "fadeIn 0.2s ease-out"
          },
          "keyframes": {
            "fadeIn": {
              "from": { "opacity": "0" },
              "to": { "opacity": "1" }
            }
          }
        }
      }
    }
  }
}
```

Don't ship plugin config (`plugins: [...]`) here unless absolutely necessary — that's invasive on the consumer side. Prefer in-component CSS-in-JS or vanilla CSS for one-off animations.

## `cssVars` config

Component-specific or theme-wide CSS variables. The CLI merges these into the consumer's globals.css.

```json
{
  "cssVars": {
    "light": {
      "brand": "210 100% 50%",
      "brand-foreground": "0 0% 100%"
    },
    "dark": {
      "brand": "210 100% 60%",
      "brand-foreground": "0 0% 100%"
    }
  }
}
```

Use HSL values without the `hsl()` wrapper — the consumer's CSS already wraps them.

For theme-only items (no component code, just tokens), use:

```json
{
  "name": "brand-theme",
  "type": "registry:theme",
  "files": [],
  "cssVars": {
    "light": { "...": "..." },
    "dark": { "...": "..." }
  }
}
```

## Common patterns

### Component depending on other components

```json
{
  "name": "data-table",
  "type": "registry:component",
  "registryDependencies": ["table", "input", "button", "dropdown-menu"],
  "dependencies": ["@tanstack/react-table"],
  "files": [
    { "path": "components/data-table.tsx", "type": "registry:component" },
    { "path": "components/data-table-pagination.tsx", "type": "registry:component" },
    { "path": "components/data-table-column-header.tsx", "type": "registry:component" }
  ]
}
```

### Block (composed section) with component dependencies

```json
{
  "name": "pricing-block",
  "type": "registry:block",
  "registryDependencies": ["card", "button", "badge"],
  "files": [
    { "path": "components/blocks/pricing-block.tsx", "type": "registry:block" }
  ]
}
```

### Hook with no UI

```json
{
  "name": "use-debounce",
  "type": "registry:hook",
  "files": [
    { "path": "hooks/use-debounce.ts", "type": "registry:hook" }
  ]
}
```

### Theme-only item

```json
{
  "name": "ocean-theme",
  "type": "registry:theme",
  "description": "Ocean-blue brand palette with light and dark mode",
  "files": [],
  "cssVars": {
    "light": {
      "primary": "200 100% 30%",
      "accent": "200 100% 50%",
      "background": "0 0% 100%",
      "foreground": "200 50% 10%"
    },
    "dark": {
      "primary": "200 100% 70%",
      "accent": "200 100% 60%",
      "background": "200 50% 5%",
      "foreground": "200 20% 95%"
    }
  }
}
```

### Page template with multiple files

```json
{
  "name": "dashboard-template",
  "type": "registry:page",
  "registryDependencies": ["card", "button", "data-table", "sidebar"],
  "dependencies": ["@tanstack/react-table"],
  "files": [
    { "path": "app/dashboard/page.tsx", "type": "registry:page" },
    { "path": "app/dashboard/layout.tsx", "type": "registry:page" },
    { "path": "components/dashboard/sidebar.tsx", "type": "registry:component" },
    { "path": "components/dashboard/header.tsx", "type": "registry:component" },
    { "path": "components/dashboard/main.tsx", "type": "registry:component" }
  ]
}
```

## Validation checklist

Before publishing a registry item:

- [ ] `name` is unique within the registry
- [ ] `type` is correct for the item kind
- [ ] All `files` exist on disk in the registry source
- [ ] All `dependencies` are real npm packages
- [ ] All `registryDependencies` are real registry items (resolvable by name or URL)
- [ ] `cssVars` use HSL values without `hsl()` wrapper
- [ ] `tailwind.config` doesn't include unnecessary plugins
- [ ] `description` is filled in (used by CLI listing)
- [ ] Component imports use consumer-relative paths (`@/components/...`), not registry-internal paths
- [ ] No hardcoded brand colors that should be CSS variables
- [ ] No personal credentials, API keys, or env vars baked into source

## Common schema mistakes

- **Missing `registryDependencies`** — most common. Component installs but errors at runtime because Card or Button isn't installed. Always declare these.
- **Wrong `type`** — putting a hook as `registry:component` causes wrong directory placement.
- **Using `npm install` paths in component imports** — your registry source might have `from '../../packages/card'` paths. These break in consumer projects. Always use the consumer's alias (`@/components/ui/card`).
- **Hardcoded HSL with hsl()** — the wrapper is added by the consumer's CSS. Don't double-wrap.
- **Missing files** — the JSON references a file that doesn't exist. The CLI errors mid-install, leaving a partial install.
- **Stale `content` after source change** — if you inline content, regenerate after every source change. Better: don't inline, let the build process handle it.

## Schema source

Official: [ui.shadcn.com/docs/registry/registry-item-schema](https://ui.shadcn.com/docs/registry/registry-item-schema)
