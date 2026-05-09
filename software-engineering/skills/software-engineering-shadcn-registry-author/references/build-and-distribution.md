---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[shadcn-registry-author]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Registry Build and Distribution

How to build, host, version, and distribute a custom shadcn registry.

## The build pipeline

The shadcn CLI provides a `build` command that turns your registry source code into the JSON files consumers install from.

### Setup

In `package.json`:

```json
{
  "scripts": {
    "registry:build": "shadcn build registry"
  }
}
```

### Source structure

```
registry/
├── default/                                  # Style namespace
│   ├── card-with-icon/
│   │   └── card-with-icon.tsx
│   ├── data-table/
│   │   ├── data-table.tsx
│   │   ├── data-table-pagination.tsx
│   │   └── data-table-column-header.tsx
│   └── use-debounce/
│       └── use-debounce.ts
└── new-york/
    └── (parallel set of items in new-york style)
```

Each item lives in `registry/[STYLE]/[ITEM-NAME]/`. The build process walks this tree.

### Item registration

For each item, you need a registration entry. The standard pattern is a `registry.json` at the project root listing every item:

```json
{
  "$schema": "https://ui.shadcn.com/schema/registry.json",
  "name": "your-registry",
  "homepage": "https://your-registry.com",
  "items": [
    {
      "name": "card-with-icon",
      "type": "registry:component",
      "description": "Card with leading icon, supports light/dark mode",
      "registryDependencies": ["card"],
      "dependencies": ["lucide-react"],
      "files": [
        {
          "path": "registry/default/card-with-icon/card-with-icon.tsx",
          "type": "registry:component"
        }
      ]
    }
  ]
}
```

### Run the build

```bash
pnpm registry:build
```

Output goes to `public/r/[STYLE]/[ITEM].json` by default. These are the URLs consumers install from.

## Hosting options

### Vercel (recommended for public)

Deploy your repo to Vercel. The `public/r/` directory is served as static files.

- URLs: `https://your-registry.vercel.app/r/default/card-with-icon.json`
- Auto-deploys on push to main
- Free tier handles most public registries
- Custom domain support included

### GitHub Raw URLs (zero-deploy alternative)

If you don't want to deploy:

- URLs: `https://raw.githubusercontent.com/you/your-registry/main/public/r/default/card-with-icon.json`
- Slower (no CDN)
- Requires public repo
- Branch-specific URLs work for versioning

### Cloudflare Pages, Netlify (alternatives)

Same model as Vercel; pick based on team preference.

### Self-hosted (for private registries)

For internal/private:

- Run a Next.js or Express server serving `public/r/`
- Add authentication (basic auth, JWT, OAuth)
- Use the shadcn CLI's `--auth` flag for authenticated install

```bash
pnpm dlx shadcn@latest add https://internal.company.com/r/default/card.json --auth Bearer xxx
```

## Versioning

The registry as a whole has a version. Each install fetches the current state of that version.

### Strategy 1: Single rolling version (simplest)

`https://your-registry.com/r/default/card.json` always returns the latest.

- Easy to maintain
- Breaking changes hit consumers immediately
- Fine for small teams or pre-1.0 registries

### Strategy 2: Versioned URLs

`https://your-registry.com/v1/r/default/card.json`
`https://your-registry.com/v2/r/default/card.json`

- Consumers pin to a major version
- More work to maintain (multiple URL paths)
- Right model for public registries with stability commitments

Implementation: deploy multiple branches or build multiple version trees.

### Strategy 3: Git-tag URLs

`https://raw.githubusercontent.com/you/registry/v1.0.0/public/r/default/card.json`

- Consumers pin to a specific git tag
- No special server config needed
- Slower to install (no CDN)

### Semver discipline

Treat the registry like a library:

- **Patch** (1.0.0 → 1.0.1): bug fixes, no API change
- **Minor** (1.0.0 → 1.1.0): new components added, no breaking changes to existing
- **Major** (1.0.0 → 2.0.0): breaking changes (renamed components, changed props, removed items)

Document changes in a `CHANGELOG.md` per release.

## Documentation site

A registry without docs is unusable beyond its author. Standard pattern:

- Next.js or Astro site
- One page per registry item
- Each page has: description, install command, props, usage examples
- Live preview if possible (MDX with rendered components)

The shadcn-ui [registry-template](https://github.com/shadcn-ui/registry-template) is a working scaffold for this exact pattern.

### Minimum docs per item

```markdown
# CardWithIcon

A card component with a leading icon, supporting light/dark mode and 4 size variants.

## Install

```bash
pnpm dlx shadcn@latest add https://your-registry.vercel.app/r/default/card-with-icon.json
```

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `icon` | `LucideIcon` | required | Icon component from lucide-react |
| `title` | `string` | required | Card title |
| `description` | `string` | optional | Card description |
| `size` | `"sm" \| "md" \| "lg" \| "xl"` | `"md"` | Card size |

## Usage

```tsx
import { CardWithIcon } from "@/components/card-with-icon"
import { Sparkles } from "lucide-react"

export function Example() {
  return (
    <CardWithIcon icon={Sparkles} title="AI-Powered" description="Smarter than your average card" size="lg" />
  )
}
```

## Customization

Override the icon color via the `--brand` CSS variable in your theme.
```

Each item's docs takes 30 minutes to write. Skipping this saves 30 minutes and costs hours of developer support questions later.

## Testing the registry end-to-end

Before any release:

### 1. Spin up a fresh consumer project

```bash
mkdir test-registry-install
cd test-registry-install
pnpm create next-app@latest .
```

### 2. Initialize shadcn

```bash
pnpm dlx shadcn@latest init
```

### 3. Install a basic item from your registry

```bash
pnpm dlx shadcn@latest add https://your-registry.vercel.app/r/default/card-with-icon.json
```

### 4. Verify

- Files landed in the right directories
- npm dependencies installed
- Tailwind config merged correctly
- CSS vars added to globals.css
- Component imports correctly
- Component renders without errors

### 5. Test transitive dependencies

Pick an item that depends on other registry items:

```bash
pnpm dlx shadcn@latest add https://your-registry.vercel.app/r/default/data-table.json
```

This should install `data-table.json` PLUS `table.json`, `input.json`, etc.

### 6. Test theme installation

If your registry includes themes:

```bash
pnpm dlx shadcn@latest add https://your-registry.vercel.app/r/default/ocean-theme.json
```

Verify CSS vars added correctly, dark mode works.

### 7. Test in dark mode

After install, toggle dark mode in the consumer app and verify components render correctly.

## CI/CD

### Auto-deploy on push

Add a GitHub Action:

```yaml
name: Deploy Registry

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v3
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'
      - run: pnpm install
      - run: pnpm registry:build
      # If using Vercel, the auto-deploy handles publishing
```

### Validation in PR

Validate the registry on every PR:

```yaml
- name: Validate registry
  run: pnpm registry:build && pnpm registry:validate
```

A `validate` script can use `jq` or a custom script to check all items have required fields.

## Common build issues

### "File not found" during build

The `files` array references a path that doesn't exist on disk. Check `registry.json` paths match actual file locations.

### Transitive deps not installing

`registryDependencies` field missing or has wrong names. Use exact names of items in the official shadcn registry, or full URLs for items in other registries.

### CSS vars not appearing in consumer

CSS vars in the wrong format — must be HSL values without `hsl()` wrapper. Check the `cssVars` field.

### Tailwind config not merging

The `tailwind.config` field path or shape is wrong. Check structure matches Tailwind's expected `theme.extend.*` format.

### Stale content after source change

If you've inlined `content` in registry items, regenerate after every source code change. Better: don't inline, let the build read from source paths.

## Security and access control

For private registries:

- Use HTTPS always
- Add authentication (Bearer token, mTLS, OAuth)
- The CLI supports `--auth` flag for authenticated installs
- Rotate credentials periodically
- Audit who has access to the registry source repo

## Migration: from copy-pasted components to a registry

If you have an existing collection of copy-pasted components across repos:

1. **Pick the canonical version** — usually the most recently updated, or the one with the most usage
2. **Move it into `registry/default/[NAME]/`** — establish the source of truth
3. **Add to `registry.json`** — declare dependencies, type, files
4. **Build and deploy** — get the registry live
5. **In each consumer repo, install from the registry** — `npx shadcn add <url>`
6. **Delete the local copy** — replace with the registry-installed version
7. **Diff and reconcile** — if local versions had drift, reconcile in the registry source, then re-install

This consolidation pass is annoying but pays back immediately the next time the component changes.
