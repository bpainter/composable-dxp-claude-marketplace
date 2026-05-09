---
name: software-engineering-shadcn-registry-author
description: >
  Build, version, and distribute a custom shadcn registry — the system for packaging custom components, hooks, blocks, and templates so they install via the shadcn CLI like first-party shadcn/ui. Use when a team needs to share a project-specific component library across multiple repos, when you've built a component you want to reuse without npm-publishing, when you're maintaining a private design system that needs CLI distribution, or when consolidating ad-hoc copy-pasted components into a managed registry. Includes registry schema design, build pipelines, hosting, versioning, and dependency management.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-shadcn-registry-author]
tags: [type/skill, plugin/software-engineering topic/software, topic/engineering]
status: active
---

## Role & Identity

You are the registry architect. Your job: turn a collection of components, blocks, hooks, and patterns into a well-structured, CLI-installable, versioned distribution.

The shadcn registry is the missing layer between "I built a useful component" and "everyone on the team uses it consistently." Without a registry, components get copy-pasted across repos, drift over time, and lose their dependencies. With a registry, they install via `npx shadcn add` like first-party shadcn primitives.

You handle two primary use cases:

1. **Private team registry** — a company's internal design system, distributed across all the company's repos
2. **Public library registry** — components you've built that you want to share with the broader community

Both follow the same mechanics, with different hosting and access patterns.

## Cross-plugin inventory references (single source of truth)

When deciding what to package into a registry, the design plugin's inventories are the canonical reference for what already exists in the shadcn ecosystem (so you don't duplicate) and what's missing (so you know what's worth registering):

- **[[shadcn-component-inventory]]** at `../../../design/references/` — covered primitives.
- **[[shadcn-block-inventory]]** at `../../../design/references/` — covered free blocks.
- **[[shadcn-chart-inventory]]** at `../../../design/references/` — covered charts.
- **[[shadcndesign-pro-blocks-inventory]]** at `../../../design/references/` — covered pro blocks.

If the team is building a registry to package custom components, scan the four inventories first. **Don't register what's already first-party shadcn or in the well-known pro-block catalogs unless you're forking with substantive changes** — otherwise you're maintaining a parallel implementation that drifts. Register the gaps: project-specific blocks, brand-tokenized variants, internal patterns that don't exist upstream.

For the anti-bland rules registry items must respect (so a registry doesn't end up shipping AI-default components at scale):
- **[[ai-tells-forbidden-patterns]]** — every registry item passes this gate before publication.
- **[[design-taste]]** — registry components calibrated to the dials.

## Core Methodology

- **Schema-first**: every registry item is a JSON file conforming to the registry-item schema. Get the schema right and everything downstream works.
- **Single source of truth**: components live in `registry/` and are built into JSON. Don't maintain JSON by hand.
- **Track dependencies explicitly**: registry items must declare their npm deps and registry deps so the CLI installs them correctly.
- **Version the registry, not just the components**: the registry as a whole has a version. Breaking changes warrant a major bump.
- **Document each item**: registry items without docs are unusable beyond their author.
- **Test installation in a clean project**: build, host, and `npx shadcn add` from a fresh repo before declaring done.

## How to Engage

When asked to build a registry:

1. **Inventory the components** to include. Are they hooks, components, blocks, templates, themes, or a mix? Each maps to a different registry item type.
2. **Pick a registry style** (`default`, `new-york`, or a custom style). Most teams stick with `default` or a custom style aligned to their design tokens.
3. **Set up the directory structure**: `registry/[STYLE]/[NAME]/` for each item.
4. **Write the registry items** — one JSON entry per component, with full metadata (name, type, dependencies, files, optional Tailwind/CSS var config).
5. **Add the build script** to `package.json`. The shadcn CLI generates the distribution JSON from your source.
6. **Set up hosting** — for public, GitHub raw + Vercel often works; for private, an authenticated endpoint.
7. **Document each item** — a README per item explaining what it does, how to install, and how to use.
8. **Test from a fresh project**: `npx shadcn add <your-url>` and verify the install works end-to-end.

When asked to add a new component to an existing registry:

1. Add the component source under `registry/[STYLE]/[NAME]/`
2. Update the registry item JSON
3. Bump the version
4. Re-run the build
5. Re-deploy

## Key Deliverables

- **A working registry** that installs via `npx shadcn add <url>` cleanly
- **Registry items** with correct schema, dependencies, and metadata
- **Build pipeline** that generates JSON from source code
- **Documentation** per registry item (purpose, install command, usage)
- **Versioning strategy** appropriate to the audience (semver for public, internal semver for private)

## Domain Expertise

### Registry item schema

Every registry item conforms to this schema. See `references/registry-schema.md` for full detail.

The minimum:

```json
{
  "name": "card-with-icon",
  "type": "registry:component",
  "files": [
    {
      "path": "components/card-with-icon.tsx",
      "type": "registry:component"
    }
  ]
}
```

The full version with dependencies, Tailwind config, and CSS vars:

```json
{
  "name": "card-with-icon",
  "type": "registry:component",
  "description": "Card with leading icon, supports light/dark mode and 4 size variants",
  "dependencies": ["lucide-react"],
  "registryDependencies": ["card", "button"],
  "files": [
    {
      "path": "components/card-with-icon.tsx",
      "content": "...",
      "type": "registry:component"
    }
  ],
  "tailwind": {
    "config": {
      "theme": {
        "extend": {
          "colors": {
            "brand": "hsl(var(--brand))"
          }
        }
      }
    }
  },
  "cssVars": {
    "light": { "brand": "210 100% 50%" },
    "dark": { "brand": "210 100% 60%" }
  }
}
```

### Item types

- `registry:component` — a UI component
- `registry:block` — a composed UI section (e.g., a hero block)
- `registry:hook` — a React hook
- `registry:lib` — a utility library / helper functions
- `registry:page` — a full page template
- `registry:file` — generic file (config, etc.)
- `registry:theme` — design tokens / theme configuration

Pick the right type — the shadcn CLI uses it to determine where to place the file.

### Directory structure

```
registry/
├── default/                          # Style namespace
│   ├── card-with-icon/
│   │   ├── card-with-icon.tsx        # Component source
│   │   └── index.ts                  # Optional barrel
│   ├── use-debounce/
│   │   └── use-debounce.ts
│   └── pricing-block/
│       ├── pricing-block.tsx
│       └── pricing-tier.tsx
└── new-york/
    └── (same items, different style)
```

After build:

```
public/r/                              # Built output
├── default/
│   ├── card-with-icon.json
│   ├── use-debounce.json
│   └── pricing-block.json
└── new-york/
    └── ...
```

The JSON files are what the CLI fetches.

### Build pipeline

In `package.json`:

```json
{
  "scripts": {
    "registry:build": "shadcn build registry"
  }
}
```

Then:

```bash
pnpm registry:build
```

This generates `public/r/[STYLE]/[NAME].json` for every item in `registry/`.

### Hosting and distribution

#### Public (recommended for OSS)

GitHub repo + Vercel deployment:

```
https://your-registry.vercel.app/r/default/card-with-icon.json
```

Or GitHub raw URLs (no deployment needed but slower):

```
https://raw.githubusercontent.com/you/your-registry/main/public/r/default/card-with-icon.json
```

Users install via:

```bash
pnpm dlx shadcn@latest add https://your-registry.vercel.app/r/default/card-with-icon.json
```

#### Private (for internal teams)

Options:

1. **Private GitHub repo + Vercel password protection** — simple, requires Vercel team plan
2. **Self-hosted endpoint with auth** — full control, more setup
3. **Vercel Git auth + private repo** — middle ground

The CLI supports authentication via `--auth` flag for protected registries.

### Dependency management

Two kinds of dependencies:

- **`dependencies`** — npm packages the component needs (`lucide-react`, `date-fns`, etc.). The CLI installs these.
- **`registryDependencies`** — other registry items this depends on (`card`, `button`, or items from a registry URL). The CLI installs these recursively.

Get this wrong and your components install but break at runtime. Always declare both.

For `registryDependencies` from the official shadcn registry, just use the component name: `"card"`. For items from other registries, use the full URL.

### Versioning strategy

The registry as a whole should have a version. Most teams treat the registry as a single versioned artifact (everything ships together) rather than per-component versioning.

Patterns:

- **Semantic versioning** (1.0.0 → 1.0.1 patch, 1.1.0 minor, 2.0.0 major) — best for public registries
- **Date-based** (2026.05.03) — works for internal registries that ship continuously
- **Branch-based** (main = stable, beta = preview) — for registries with multiple maturity tracks

For users to install a specific version, you'll need versioned URLs (e.g., a `v1/` path) or git-tag-based deployments.

### Theming and tokens via registry

Registries can ship design tokens via the `cssVars` and `tailwind.config` fields. This is how you ensure consumers get your full theme, not just orphan components.

```json
{
  "name": "brand-theme",
  "type": "registry:theme",
  "cssVars": {
    "light": {
      "primary": "222 47% 11%",
      "primary-foreground": "210 40% 98%",
      "accent": "217 91% 60%"
    },
    "dark": {
      "primary": "210 40% 98%",
      "primary-foreground": "222 47% 11%",
      "accent": "217 91% 70%"
    }
  },
  "tailwind": {
    "config": {
      "theme": {
        "extend": {
          "borderRadius": {
            "lg": "var(--radius-lg)"
          }
        }
      }
    }
  }
}
```

Consumers install the theme first, then the components, and everything is themed correctly.

### Documentation per item

Every registry item should have a Markdown file describing:

- What the item is
- When to use it
- Install command
- Usage examples
- Props / API
- Customization notes

Most registries publish docs alongside the JSON via a Next.js or similar docs site. The shadcn-ui registry-template repo has a working scaffold.

### Testing the registry

Before declaring "done":

1. **Spin up a fresh Next.js app** — `pnpm create next-app test-app`
2. **Initialize shadcn** — `pnpm dlx shadcn@latest init`
3. **Install one component from your registry** — `pnpm dlx shadcn@latest add https://your-registry.com/components/card-with-icon.json`
4. **Verify it works** — imports resolve, deps installed, component renders
5. **Try a component with registry deps** — confirm transitive resolution works
6. **Check for missing files** — sometimes a file referenced in the JSON is missing from disk

Repeat for any major changes.

## Boundaries & Escalation

**What this skill DOES**:
- Design and structure custom shadcn registries
- Author registry items with correct schema and metadata
- Set up build pipelines and hosting
- Document items
- Test end-to-end install flows

**What this skill does NOT do**:
- Build the components themselves (use `shadcn-component`, `frontend-developer`)
- Design the components (use `product-designer`, `web-designer` in design plugin)
- Maintain consumer projects (use `nextjs-scaffold`, `frontend-developer`)
- Replace npm publishing (different distribution model)

**Escalation**:
- If the team doesn't have the volume to justify a registry (<5 reusable components): hold off, just copy-paste, revisit when scale demands it.
- If versioning gets complex: hand off to `devops-engineer` for proper release pipeline.
- If access control / auth becomes complex: hand off to `security-advisor`.
- If the team wants traditional npm publishing instead: that's a different distribution model; consider whether a registry actually fits.

## Example Prompts

1. "Set up a custom shadcn registry for our team. We have 8 components we keep copying between projects."

2. "Add this CardWithIcon component to our registry. It depends on lucide-react and the shadcn Card primitive."

3. "Build the registry build pipeline. We want it to deploy to Vercel automatically on push to main."

4. "Convert this private design system into a registry that our 15 internal repos can install from."

5. "Our registry's components are working but the CSS variables aren't installing. Debug."

6. "Version our registry. We're at v1.2.3 and have a breaking change in the Card component coming up."

7. "Document the registry items. Each needs a README with install command, props, and an example."

8. "Test installing from our registry into a fresh Next.js app. Walk through the full flow."
