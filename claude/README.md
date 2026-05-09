---
type: readme
project: skills-library
scope: plugin
plugin: claude
tags: [type/readme, plugin/claude]
status: active
---

# Claude Plugin

**Skills for building, customizing, and operating Claude Desktop (Cowork) and Claude Code.**

This plugin holds Claude-tooling-specific skills — things like scaffolding new plugins, designing the orchestration layer for a new Cowork project, configuring hooks, customizing the status line, working with MCP servers. Distinct from domain plugins (`obsidian`, `marketing`, `consulting`, etc.) which advise on *content* domains; the `claude` plugin advises on *the platform itself*.

## Skills (2)

| Skill | What it does |
|---|---|
| [[claude-plugin-creator]] | Scaffolds and builds new Claude plugins for Cowork (Claude Desktop) and Claude Code — folder structure, plugin.json, skills, agents, commands, and the `.plugin` build process. Defaults to Cowork install paths (Settings → Customize → Personal Plugins) and references Anthropic's official plugin examples. |
| [[claude-orchestrator]] | Designs the orchestration layer for a new Cowork project so it knows how to coordinate multiple skills across plugins. Covers three patterns: project-level CLAUDE.md (default), multi-phase slash command (the feature-dev pattern), and parallel agent team (the Morphllm pattern). Use this when starting a new Cowork project, designing a category agent, or authoring a multi-phase command. |

## Roadmap

Likely additions as Claude tooling needs surface:

- `claude-slash-command-author` — design slash commands with good descriptions and bodies
- `claude-agent-author` — design Claude Code subagents (the `agents/` files)
- `claude-hook-author` — write Claude Code hooks (event handlers in plugins)
- `claude-mcp-server-author` — design MCP servers for tooling integration
- `claude-statusline-customizer` — customize the Claude Code status line
- `claude-skill-evaluator` — write evals/tests for skills (manual or programmatic triggers)
- `claude-marketplace-curator` — design and maintain a plugin marketplace

The pattern: each skill is a focused expert on one Claude-tooling concern. They compose when a task spans multiple concerns (e.g., "build a plugin with hooks and a slash command and a project-level orchestration layer" → plugin-creator + hook-author + orchestrator).

## Authoritative documentation

The skills cite these as the source of truth — keep them handy:

- **Claude Code plugins docs** — https://code.claude.com/docs/en/plugins (manifest schema, commands, agents, marketplaces, hooks)
- **Cowork plugin tutorial** — https://claude.com/resources/tutorials/how-to-customize-plugins-in-cowork (Customize panel install path)
- **Anthropic's official plugin examples** — https://github.com/anthropics/claude-plugins-official (40+ reference plugins; `feature-dev` is the canonical multi-phase orchestrator)

## Folder structure

```
claude/
├── README.md                                 # this file
├── .claude-plugin/
│   └── plugin.json                           # plugin manifest
├── commands/
│   └── plugin-help.md                        # /plugin-help slash command
└── skills/
    ├── claude-plugin-creator/
    │   ├── SKILL.md
    │   └── references/
    │       ├── plugin-anatomy.md             # folder structure, file naming
    │       ├── manifest-conventions.md       # plugin.json + marketplace.json schemas, build process
    │       ├── cowork-install-path.md        # Settings → Customize → Personal Plugins flow
    │       └── official-plugins-reference.md # Anthropic's reference plugins as exemplars
    └── claude-orchestrator/
        ├── SKILL.md
        └── references/
            ├── orchestration-patterns.md     # the three patterns with full templates
            └── cowork-project-setup.md       # step-by-step Cowork project setup walkthrough
```

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and the standard sections (Role, Methodology, Engage, Deliverables, Boundaries, Examples).
- Each skill has a `references/` folder with 2–4 skill-specific deep references.
- Skills are prefixed with `claude-` to match the plugin name.

## Installing

Same pattern as every plugin in this marketplace. See `../INSTALL.md`.

```bash
# Cowork (Claude Desktop) — recommended
# Drag Plugins/claude.plugin into Settings → Customize → Personal Plugins

# Or marketplace install (Claude Code CLI)
claude plugin install claude@second-brain
```

## License

MIT.
