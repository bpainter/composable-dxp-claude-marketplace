# 80_Skills_and_Agents

The Composable DXP **plugin marketplace** for Claude. 18 plugins, 182 skills, packaged for Claude Code (CLI), Claude Desktop (Cowork mode), and drag-and-drop install.

**Marketplace name:** `second-brain` (the value in `.claude-plugin/marketplace.json`)
**Repo:** `https://github.com/Slalom/composable-dxp-claude-marketplace` *(private, Slalom org)*

## Quick install

```bash
# Cowork (Git URL — Cowork requires Git, not local paths)
claude plugin marketplace add https://github.com/Slalom/composable-dxp-claude-marketplace.git
claude plugin install <plugin>@second-brain

# Claude Code (also accepts the local path)
claude plugin marketplace add "/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"
claude plugin install <plugin>@second-brain
```

**For full install paths (Cowork, Claude Code, drag-and-drop), see `INSTALL.md`.**

## Top-level layout

```
80_Skills_and_Agents/
├── README.md                    # this file (conventions and structure)
├── INSTALL.md                   # how to install the marketplace and plugins
├── .claude-plugin/
│   └── marketplace.json         # marketplace manifest — lists every plugin
├── Memory/                      # cross-cutting working memory (CLAUDE.md, people, glossary, etc.)
│                                # NOT a plugin — read by Claude every session
├── Plugins/                     # built .plugin files (build artifacts)
│   └── <plugin>.plugin
└── <plugin-name>/               # one folder per plugin SOURCE (e.g., obsidian/, claude/)
    └── ... (see "Plugin layout" below)
```

The marketplace itself is a **single Claude plugin marketplace**. Tools that consume marketplaces (Claude Code CLI, Claude Desktop via Cowork) read `.claude-plugin/marketplace.json` to discover what's installable.

`Memory/` is *not* a plugin. It's the user's working memory and shared knowledge base, read every session by Claude, and shared across all plugins.

`Plugins/` holds **built artifacts** — the `.plugin` files (renamed ZIPs) that get dragged into Claude Desktop's Personal Plugins panel for install. The *source* of each plugin lives in its own folder at the marketplace root (e.g., `obsidian/`, `claude/`); the `.plugin` files in `Plugins/` are derived from those sources via the build process described in `INSTALL.md`.

## Plugin layout (the standard)

Every plugin under this marketplace follows this exact shape. Deviations break tooling.

```
<plugin-name>/                              # plugin SOURCE (lives at marketplace root)
├── README.md                              # plugin overview for humans
├── .claude-plugin/
│   └── plugin.json                        # plugin manifest for Claude (only `name` is required)
├── references/                            # plugin-wide shared references (optional)
│   └── <plugin>-foundations.md
├── skills/                                # auto-discovered by Claude
│   ├── <plugin>-<skill-name>/             # one folder per skill
│   │   ├── SKILL.md                       # required — frontmatter (name, description) + body
│   │   └── references/                    # skill-specific deep references (typical, 1–3 files)
│   │       └── <topic>.md
│   └── ... (more skills)
├── agents/                                # auto-discovered by Claude (workers)
│   └── <plugin>-<agent-name>.md           # single file per agent, Claude Code agent format
└── commands/                              # slash commands (optional)
    └── <command-name>.md
```

### Why each piece exists

| Piece | Required? | Purpose |
|---|---|---|
| `README.md` | recommended | Plugin overview, opinion, roadmap, written for humans browsing the repo |
| `.claude-plugin/plugin.json` | required | Manifest Claude reads. Only `name` is strictly required; `version`, `description`, `author`, `keywords`, `license` are optional. **Don't include empty optional fields** (e.g., `"homepage": ""`) — they fail manifest validation |
| `references/` | optional | Plugin-wide shared references at the plugin root. From a `SKILL.md`, cite as `../../references/<file>.md` |
| `skills/` | optional | Skills (one folder per skill, each with a `SKILL.md`). Auto-discovered by Claude |
| `skills/<skill>/SKILL.md` | required if folder exists | The skill itself — YAML frontmatter (`name`, `description`) + body |
| `skills/<skill>/references/` | optional | Skill-specific deep references (frameworks, checklists, examples). From the `SKILL.md`, cite as `references/<file>.md` |
| `agents/` | optional | Workers that act on user data — Claude Code agent file format (one file per agent) |
| `commands/` | optional | Slash commands — single Markdown files with `description:` frontmatter and a body that names the skill or workflow to invoke |

A plugin can have any combination of skills, agents, and commands. Empty folders are fine if you intend to fill them later.

### Two flavors of skills: advice vs orchestrator-shaped

All skills live in `skills/`, but they come in two flavors:

- **Advice skills** (the default) — one focused topic, one persona. Examples: `obsidian-tag-taxonomist`, `obsidian-folder-architect`. Body follows the standard skill template (Role & Identity, Core Methodology, How to Engage, etc.).
- **Orchestrator-shaped skills** — sequence multiple advice skills into a multi-stage workflow. Naming convention: filenames end with `-sweep`, `-flow`, or `-bootstrap`. Body has Goal / Stages / Handoffs / Failure modes sections instead of (or in addition to) the standard template. Example: `obsidian-vault-health-sweep`.

There is no separate `orchestrators/` folder — orchestrator-shaped skills live in `skills/` like everything else, so they're auto-discovered by Claude. The distinction is conceptual, encoded in the naming convention and the skill's own description.

## Naming conventions

### Plugin names

- Lowercase, single word when possible: `obsidian`, `claude`, `consulting`, `marketing`
- Two words: hyphenated (`design-creative`, `behavioral-economics`)
- The plugin name is also the folder name AND the prefix for everything inside it

### Skill / agent / command names

- **Always prefixed with the plugin name**: `obsidian-tag-taxonomist`, `claude-plugin-creator` — not `tag-taxonomist`, not `plugin-creator`
- Lowercase, kebab-case: `obsidian-vault-audit-framework`, not `Obsidian_Vault_Audit_Framework`
- The folder name (for skills) and filename (for agents/commands) match the YAML `name:` field exactly
- Prefix prevents name collisions across plugins and makes ownership obvious

### Reference file names

- Lowercase, kebab-case: `tag-taxonomy-design.md`, not `Tag_Taxonomy_Design.md`
- Descriptive of content, not generic (`tag-taxonomy-design.md` ✓, `reference-1.md` ✗)
- Plugin-wide shared references can use the plugin prefix: `obsidian-foundations.md`, `claude-foundations.md`

## SKILL.md format

Required structure for every `SKILL.md`:

```markdown
---
name: <plugin>-<skill-name>
description: >
  One paragraph describing the skill's persona, when to use it, and the trigger
  conditions. This text drives Claude's skill-matching, so be specific. Mention
  related skills it pairs with. Also known as: <synonyms>.

# Project context (optional — Obsidian-readable Properties)
type: skill
project: skills-library
plugin: <plugin>
aliases: [<plugin>-<skill-name>]
tags: [type/skill, plugin/<plugin>, topic/<topic>, topic/knowledge-management]
status: active
---

## Role & Identity

You are a **<Title>** for <domain>. Your job is to ...

## Core Methodology
...

## How to Engage
...

## Key Deliverables
...

## Domain Expertise
...

For shared <plugin>-wide concepts, see `../../references/<plugin>-foundations.md`.
For <topic 1> framework, see `references/<file>.md`.

## Boundaries & Escalation
...

## Example Prompts
1. "..."
2. "..."
```

Aim for 8 example prompts. They sharpen the description and help with manual testing.

The "Project context" frontmatter block is optional — it makes SKILL.md files Obsidian-readable as Properties when browsed in the vault. Useful for cross-referencing skills via Obsidian's link/Properties UI; not required by Claude Code.

## Reference conventions

Skills can cite two kinds of reference files. Paths are relative to the SKILL.md file.

| Where | Path from SKILL.md | Use case |
|---|---|---|
| Skill-specific | `references/<file>.md` | Frameworks, checklists, playbooks specific to this skill |
| Plugin-wide shared | `../../references/<file>.md` | Concepts shared across multiple skills in this plugin |
| Cross-plugin | (don't) | Skills should be self-contained per plugin; if you need shared content across plugins, copy it |

Plugin-wide shared references live at `<plugin>/references/` (top of the plugin folder, sibling to `skills/`).

## Adding a new plugin

1. Create folder: `80_Skills_and_Agents/<plugin-name>/`
2. Add `<plugin-name>/.claude-plugin/plugin.json` — at minimum `{"name": "<plugin-name>"}`. Don't include empty optional fields. See `obsidian/.claude-plugin/plugin.json` for a complete example
3. Add `<plugin-name>/README.md` (plugin overview)
4. Add the relevant subfolders: `references/`, `skills/`, `agents/`, `commands/` (only the ones you need)
5. Add at least one skill at `skills/<plugin>-<skill-name>/SKILL.md` to give the plugin functionality
6. Update `.claude-plugin/marketplace.json` at the marketplace root to register the new plugin (`name`, `source`, `description`)
7. Build the `.plugin` file and place it in `Plugins/<plugin-name>.plugin` (see `INSTALL.md` for the build command)
8. Install in Claude Desktop via Personal Plugins panel, or via Claude Code CLI

The `claude-plugin-creator` skill in the `claude` plugin walks through this process step-by-step.

## Adding a new skill to an existing plugin

1. Create folder: `<plugin-name>/skills/<plugin-name>-<skill-name>/`
2. Write `SKILL.md` following the format above
3. Add `references/` with 1–3 deep-reference files
4. If the content references plugin-wide concepts, link to `../../references/<plugin>-foundations.md`
5. Rebuild the `.plugin` file (if you've already shipped it) and reinstall

No marketplace.json update needed — Claude discovers skills automatically by walking `skills/` folders.

## Adding an agent

A Claude Code agent file at `<plugin>/agents/<plugin>-<agent>.md` with frontmatter (`name`, `description`, `tools`) and a system-prompt body. Agents are workers that *act* on user data (read/write files, scan, transform). They're invoked by skills or directly by Claude when the task matches their description.

## Adding a slash command

A single file at `<plugin>/commands/<command>.md` with `description:` frontmatter and a body that names the skill or workflow to invoke. Slash commands are for predictable workflows the user wants one-keystroke access to (e.g., `/audit`, `/sweep`).

## Adding an orchestrator-shaped skill

Orchestrators are a *flavor of skill*, not a separate primitive. To add one:

1. Create `<plugin>/skills/<plugin>-<workflow-name>/SKILL.md` (use a `-sweep`, `-flow`, or `-bootstrap` suffix per convention)
2. Frontmatter follows the same `name:` + `description:` shape as advice skills
3. Body uses Goal / When to invoke / Stages / Handoffs / Failure modes structure
4. Reference the advice skills called by each stage

The orchestrator-shaped skill *coordinates*; it doesn't reimplement the advice skills it calls. Each stage names the advice skill responsible.

## Installing

See `INSTALL.md` for full instructions. Short version:

```bash
# Personal Plugin install (Claude Desktop)
# Drag Plugins/<plugin>.plugin into Settings → Personal Plugins

# Or marketplace install (Claude Code CLI)
claude plugin marketplace add "/path/to/80_Skills_and_Agents"
claude plugin install obsidian@second-brain
```

Plugins installed via the CLI become available in both Claude Code sessions and Claude Desktop's Cowork mode.

## Current plugins

| Plugin | Version | Skills | Description |
|---|---|---|---|
| `algolia` | 0.1.0 | 12 | Algolia search platform — index design, relevance tuning, InstantSearch React, Autocomplete, search-client, indexing pipelines (Contentful → Algolia), API key strategy, Recommend, NeuralSearch/Personalization, Insights events + A/B, MCP/CLI |
| `behavioral-economics` | 0.3.0 | 7 | Decision science grounded in Kahneman/Thaler-Sunstein/Ariely/Wendel + Slalom Behavioral Design Model. 6 templates: Behavioral Profile, Choice Architecture, Nudges, Intervention Design, Scoring, Validation |
| `brand` | 0.3.0 | 7 | Strategy + identity — strategist, designer, naming, voice & tone, identity system, guidelines composer, audit |
| `bynder` | 0.1.0 | 12 | Bynder DAM — asset model, brand guidelines, derivatives, marketplace connectors, Contentful pairing, JS SDK, migration, optimization audit, permissions/workflow, portal architect, webhooks, Compact View |
| `claude` | 0.3.0 | 2 | Claude tooling — `claude-plugin-creator` (build new plugins) and `claude-orchestrator` (Cowork project orchestration) |
| `consulting` | 0.4.0 | 9 | Management consulting grounded in Block/Kubr/Burtonshaw/Mabee/Robbins-Judge/Voss/Challenger Sale. 4 templates (Engagement Scoping, Negotiation Prep, Challenger Sale, Change Readiness). Companion Solution Brief in /40_Library |
| `contentful` | 0.1.0 | 12 | Contentful platform — content modeling, React/Next.js wiring, GraphQL, space architecture, CMA migrations, webhooks, delivery optimization, App Framework, Personalization (former Ninetailed), rich text, localization, MCP/CLI |
| `cx` | 0.2.0 | 9 | Research, JTBD, personas, journeys, service design, IA, product trends, customer-feedback synthesis, personalization |
| `design` | 0.4.0 | 15 | Product/web/motion/accessibility specialists, plus process/audit/screen/handoff/iconography/imagery/document/social/presentation/visualization/taste |
| `facilitation` | 0.4.0 | 10 | Senior facilitator + 9 specialists — meeting architect, workshop designer, collaboration patterns, design thinking, strategic visualizer, decision architect, group dynamics, virtual/hybrid, gamestorming. 9 templates, 6 slash commands |
| `innovation` | 0.2.0 | 11 | Innovation operating system grounded in Christensen/Doblin/Dyer/Furr + Slalom DT books. Strategy, portfolio, value engineering, JTBD, lean validation, ten-types, monetization, lab design, disruption, creative leadership, digital transformation. 6 templates + 8 slash commands |
| `leadership` | 0.3.0 | 7 | Executive coaching (neurodivergent), people-leader, hiring, meetings/cadences, onboarding/transitions, goals/OKRs, stoic perspective |
| `marketing` | 0.2.0 | 8 | Copy, SEO/GEO/AEO, growth, analytics, social, events, briefs, AI-extractable content |
| `obsidian` | 0.4.0 | 12 | Obsidian vault organization (9 advice + 3 orchestrator-shaped), 1 agent, 4 slash commands |
| `product-management` | 0.1.0 | 1 | Product management discipline grounded in Perri's *Escaping the Build Trap*. Strategy cascade, Product Kata, outcomes-over-outputs |
| `project-management` | 0.4.0 | 9 | PM persona, task workflows + flow-engineer (Reinertsen) and methodology-advisor (Gothelf). Templates: Flow Metrics, Methodology Selector, Risk Up Front |
| `software-engineering` | 0.3.1 | 28 | Architecture, build, AI/data/security/writing personas, Next.js deep dives, shadcn + Tailwind workflows. Platform implementations (Contentful, Vercel, Algolia, Bynder) live in their own plugins |
| `vercel` | 0.1.0 | 11 | Vercel platform — deploy pipeline, Fluid Compute, CDN/edge, storage, observability, security, AI SDK, AI Gateway, v0, Sandbox/Agent/Workflows, REST API. Adjacent to software-engineering's Next.js skills — that plugin owns the framework, this owns the platform |

## Reading order for a new contributor (or future-you)

1. This README — the marketplace conventions
2. `obsidian/README.md` — the canonical example plugin (multi-skill, with agent and commands)
3. `obsidian/references/obsidian-foundations.md` — example of a plugin-wide shared reference
4. `obsidian/skills/obsidian-tag-taxonomist/SKILL.md` — example of an advice skill
5. `obsidian/skills/obsidian-vault-health-sweep/SKILL.md` — example of an orchestrator-shaped skill
6. `claude/skills/claude-plugin-creator/SKILL.md` — the skill that helps you build new plugins

The `obsidian/` plugin is intentionally the canonical example. New plugins should mirror its shape.
