---
type: reference
project: skills-library
scope: plugin
plugin: software-engineering
tags: [type/reference, plugin/software-engineering, scope/foundational, topic/software]
status: active
---
# The shadcn/ui Ecosystem

A complete map of the shadcn/ui ecosystem — core components, MCP server, custom registries, pro blocks, and agent skills. The default UI foundation for every frontend project in this engineering library.

## Why shadcn/ui

shadcn/ui isn't a component library in the traditional sense. It's a **system for getting accessible, themeable React components into your project that you fully own.** The CLI copies component source into your repo; you modify, extend, and ship them as your own code.

Key advantages:

- **You own the code.** No black-box dependency. Every component is in your `components/ui/` folder. You can read it, modify it, theme it, fork it.
- **Built on Radix primitives.** Accessibility (keyboard, screen reader, focus) handled correctly out of the box.
- **Tailwind-native.** Styles via Tailwind utilities + CSS variables. Plays well with `@layer components` semantic naming for app-level styles.
- **Composable.** Every component is a small primitive that composes with others. Build complex UIs from simple parts.
- **Active ecosystem.** The MCP server, registry standard, and pro blocks ecosystem make it the default for AI-assisted UI development.

## The four layers of the ecosystem

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 4: Agent Skills                                       │
│  AI-aware skills that read your project's components.json    │
│  and generate project-aligned shadcn code                    │
└─────────────────────────────────────────────────────────────┘
                            ↓ informs
┌─────────────────────────────────────────────────────────────┐
│  Layer 3: MCP Server                                         │
│  Browse, search, install components from any registry        │
│  via natural-language commands                               │
└─────────────────────────────────────────────────────────────┘
                            ↓ uses
┌─────────────────────────────────────────────────────────────┐
│  Layer 2: Registries (official + community + your own)       │
│  - Official shadcn/ui registry (core components, blocks)     │
│  - Pro blocks (shadcndesign, shadcnblocks, etc.)             │
│  - Your custom registry (project-specific patterns)          │
└─────────────────────────────────────────────────────────────┘
                            ↓ delivers
┌─────────────────────────────────────────────────────────────┐
│  Layer 1: Components in your codebase                        │
│  Vendored React + Tailwind source, owned by your project     │
└─────────────────────────────────────────────────────────────┘
```

## Layer 1: Core components

Install via:

```bash
pnpm dlx shadcn@latest init                    # one-time setup
pnpm dlx shadcn@latest add button card dialog  # add components
```

This generates `components.json` (project config) and copies the named components into `components/ui/`.

### Categories of core components

**Form primitives**: Button, Input, Textarea, Select, Checkbox, Radio Group, Switch, Toggle, Toggle Group, Slider, Form, Label, Field, FieldGroup

**Layout**: Card, Separator, Aspect Ratio, Resizable, Scroll Area, Tabs, Accordion, Collapsible

**Overlay**: Dialog, Alert Dialog, Sheet, Drawer, Popover, Tooltip, Hover Card, Context Menu, Dropdown Menu, Menubar, Command

**Display**: Avatar, Badge, Progress, Skeleton, Calendar, Date Picker, Carousel, Chart, Table, Data Table

**Feedback**: Alert, Sonner (toast)

**Navigation**: Breadcrumb, Navigation Menu, Pagination

**Other**: Combobox, Input OTP

For the canonical list, run `pnpm dlx shadcn@latest add` with no args, or check [ui.shadcn.com/docs/components](https://ui.shadcn.com/docs/components).

### Composition rules (from official docs)

- Use **FieldGroup** for forms — wraps multiple fields with consistent spacing and labels
- Use **ToggleGroup** for option sets where exactly one (or one of N) must be selected
- Use **semantic color variables** (`--primary`, `--secondary`, `--destructive`) — never hard-code Tailwind colors in component logic
- Use **base-specific APIs correctly** — every shadcn component has the right variant prop names; don't invent your own (use the documented `variant`, `size`, etc.)

## Layer 2: Registries

A registry is a JSON-based system for distributing component code. The official shadcn registry hosts core components and blocks. Anyone can publish their own.

### Registry-item schema

Every registry item is a JSON file conforming to the `registry-item` schema:

```json
{
  "name": "my-component",
  "type": "registry:component",
  "dependencies": ["lucide-react"],
  "registryDependencies": ["button", "card"],
  "files": [
    {
      "path": "components/my-component.tsx",
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

The CLI uses this metadata to:

- Install npm dependencies
- Recursively install other registry items (transitive dependencies)
- Place files in the correct location
- Merge Tailwind config and CSS variables

### Official + community registries

| Registry | Source | Specialization |
|---|---|---|
| **Official shadcn/ui** | ui.shadcn.com | Core components, official blocks |
| **shadcndesign Pro Blocks** | shadcndesign.com/pro-blocks | Page-level sections (hero, features, footers, etc.) |
| **shadcnblocks** | shadcnblocks.com | 1,500+ blocks, 1,000+ components, 15 templates |
| **shadcn.io** | shadcn.io | 6,000+ production-ready sections across 46 categories |
| **Shadcn Studio** | shadcnstudio.com | 1,000+ components, blocks, kits |

Pick one or two as defaults; install components from others as needed.

### Your custom registry

For project-specific components that you want to share across multiple repos (or with your team), build your own registry. See `shadcn-registry-author` skill for the full workflow.

The 30-second version:

1. Create `registry/[STYLE]/[NAME]/` directories with component code
2. Add a `registry:build` script to `package.json`
3. Run the build to generate registry JSON files
4. Deploy the JSON to a public URL (GitHub raw, Vercel, etc.)
5. Other projects install via:

   ```bash
   pnpm dlx shadcn@latest add https://your-registry.com/components/my-button.json
   ```

This is how you turn "we keep copying this CardWithIcon component across 6 repos" into "everyone installs `@bermon/card-with-icon`."

## Layer 3: MCP Server

The shadcn MCP server lets Claude (and other AI assistants) browse, search, and install components from any configured registry through natural language.

### Setup

```bash
# In your project directory
pnpm dlx shadcn@latest mcp init --client claude
```

This adds the MCP server to your Claude Code / Cursor / VS Code MCP configuration. Restart your editor.

### Authentication (recommended)

Without a GitHub token, the MCP server is rate-limited to 60 requests/hour. Add a Personal Access Token:

```bash
npx @jpisnice/shadcn-ui-mcp-server --github-api-key ghp_your_token_here
```

This bumps you to 5,000 requests/hour, which matters for any sustained dev session.

### What it can do

Once connected, the MCP server exposes tools the assistant can use:

- **Browse** — list all available components, blocks, templates from configured registries
- **Search** — find components by name or functionality
- **Install** — add components via natural language: "add a date picker and a calendar to this project"
- **Read documentation** — pull component docs into the conversation context

### Example prompts that work well

- "Show me all available navigation components"
- "Install a date picker and integrate it into the booking form"
- "What's the difference between Sheet and Drawer in shadcn?"
- "Add a pricing table block from shadcndesign pro-blocks"
- "Find me a multi-select combobox component"

The assistant uses the MCP to find the right component, install it, and write the integration code in one flow.

## Layer 4: Agent Skills

Agent skills are curated instruction sets that teach AI assistants to generate **project-aligned** shadcn code — not generic shadcn code.

### How they work

The skill activates when it detects `components.json` in your project. It then runs:

```bash
shadcn info --json
```

This returns your project's shadcn configuration (style, base color, CSS variable mode, alias paths, etc.). The skill injects this into the assistant's context.

The result: when the assistant generates UI code, it uses **your actual setup**:

- The right import paths (matching your `@/components/ui` alias)
- The right semantic color variables (`--primary` if you use CSS vars, hardcoded if not)
- The right base API (correct prop names per shadcn version)
- The right composition patterns (FieldGroup for forms, etc.)

### Available agent skills

- **Official shadcn/ui Skills** — `ui.shadcn.com/docs/skills` — official skill packs for the major AI editors
- **shadcndesign Agent Skills** — `shadcndesign.com/docs/agent-skills` — adds pro-blocks awareness, Figma-to-code generation, design token sync
- **Community skills** — Shadcnblocks-Skill (2,500+ blocks), and others

### Using them with this project

The shadcn ecosystem skills complement the skills in this plugin (`shadcn-component`, `pro-blocks-composer`, `shadcn-registry-author`). When working on UI:

- Bermon's skills provide *strategy and conventions* (when to extract semantic classes, naming patterns, registry decisions)
- shadcn agent skills provide *project-specific accuracy* (correct imports, correct theme variables, correct API)

Pair them, don't replace one with the other.

## Putting it together: a typical workflow

### New marketing page

1. Use `pro-blocks-composer` skill — pick a hero block, feature block, pricing block, footer
2. Run shadcn MCP: "install hero-section-with-cta from shadcndesign and a pricing-table-3-tier"
3. Use `shadcn-component` skill to add any missing primitives the blocks need
4. Use `tailwind-semantic-naming.md` conventions for app-level wrapper styles
5. Hand off to `frontend-developer` skill for integration logic and routing

### New dashboard

1. Use `nextjs-scaffold` skill for App Router structure
2. Use `pro-blocks-composer` skill to pull in app-shell + sidebar + page-header blocks
3. Use shadcn primitives (Card, Tabs, Charts, Data Table) for the dashboard tiles
4. Use `design-system-engineer` skill if you need to extend the design system itself
5. Use `tailwind-tokens` skill to wire any custom tokens

### New custom component for reuse

1. Build the component using shadcn primitives + your conventions
2. Use `shadcn-registry-author` skill to package it as a registry item
3. Publish to your custom registry
4. Now installable across your projects via `npx shadcn add <your-url>`

## Decision matrix: which skill / tool when

| Task | Use |
|---|---|
| Add a button, card, dialog, etc. | `shadcn-component` skill (or MCP "install button") |
| Add a hero/feature/pricing section | `pro-blocks-composer` skill |
| Build a custom component for cross-project reuse | `shadcn-registry-author` skill |
| Wire design tokens into Tailwind | `tailwind-tokens` skill |
| Implement a complete design system | `design-system-engineer` skill |
| Style application-level patterns (not single components) | `tailwind-semantic-naming.md` reference |
| Generate UI from a Figma design | shadcndesign agent skills + `design-implementation` skill |
| Integrate UI with backend logic | `frontend-developer` skill |
| Scaffold the Next.js routes around the UI | `nextjs-scaffold` skill |

## When NOT to use shadcn/ui

shadcn/ui is the right default for new React/Next.js projects in 2026. There are exceptions:

- **Native mobile (iOS/Android)** — use `mobile-app-developer` skill with platform-native components
- **Email templates** — shadcn doesn't apply; use react-email or MJML
- **Marketing-only static sites where bundle size is critical** — sometimes raw HTML + Tailwind is lighter
- **Existing codebases on Material-UI, Chakra, Mantine, etc.** — migration is significant; only worth it for major redesigns
- **Vue, Svelte, Solid projects** — port versions exist (shadcn-vue, shadcn-svelte) but the ecosystem is React-centric

## Anti-patterns

- **Editing components in `components/ui/` directly without a plan** — once you customize a primitive, you can't easily update it from upstream. Either: (a) keep primitives unmodified and wrap them, or (b) consciously fork and document the modification.
- **Mixing pro-block styles with custom inline styles** — pro blocks have their own visual rhythm. If you mix them with hand-rolled sections, the page feels stitched together. Either commit to using blocks (and adapt others to match) or build everything custom.
- **Installing too many similar components** — Sheet, Drawer, Dialog, Alert Dialog all overlap. Pick the canonical one for your project's interaction pattern and stick to it. Don't accumulate.
- **Skipping `components.json` review** — the config file controls aliases, base color, CSS vars. Misconfigured aliases cause every subsequent install to land in the wrong directory.
- **Treating shadcn as a black box** — read the components you install. They're React code in your repo. Understanding them lets you debug confidently and extend safely.

## Versioning and updates

shadcn doesn't ship as an npm package, so there's no `npm update`. To update a component to the latest upstream version:

```bash
pnpm dlx shadcn@latest add button --overwrite
```

This re-pulls the latest version of `button.tsx` and overwrites the local copy. **Lose any local modifications.**

If you've modified the component, you'll need to manually merge upstream changes. This is by design — shadcn trades automatic updates for full ownership.

For long-term maintenance: keep a list of which components have been modified and which are pristine. Re-pull pristine ones; manually merge modified ones during updates.

## Source

- Official: [ui.shadcn.com/docs/components](https://ui.shadcn.com/docs/components), [ui.shadcn.com/docs/registry](https://ui.shadcn.com/docs/registry), [ui.shadcn.com/docs/mcp](https://ui.shadcn.com/docs/mcp), [ui.shadcn.com/docs/skills](https://ui.shadcn.com/docs/skills)
- Pro blocks: [shadcndesign.com/pro-blocks](https://www.shadcndesign.com/pro-blocks), [shadcndesign.com/docs/agent-skills](https://www.shadcndesign.com/docs/agent-skills), [shadcnblocks.com](https://www.shadcnblocks.com/), [shadcn.io](https://www.shadcn.io/)
- MCP server: [github.com/Jpisnice/shadcn-ui-mcp-server](https://github.com/Jpisnice/shadcn-ui-mcp-server)
- Registry template: [github.com/shadcn-ui/registry-template](https://github.com/shadcn-ui/registry-template)
