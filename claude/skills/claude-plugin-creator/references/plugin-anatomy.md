# Plugin Anatomy

What's inside a Claude Code / Claude Desktop plugin folder, what each piece is for, and when to include it.

## The minimum viable plugin

A plugin can be as small as two files:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json    # required: { "name": "my-plugin" }
└── skills/
    └── my-plugin-hello/
        └── SKILL.md   # the skill itself
```

That's it. Claude Code/Desktop will discover the skill, the `name` in the manifest is enough for installation, and you can ship a `.plugin` ZIP of the whole thing.

Everything else (README, references, agents, commands, top-level references) is optional and earns its place based on the plugin's scope.

## The full structure

```
<plugin-name>/
├── README.md                          # plugin overview for humans (recommended)
├── .claude-plugin/
│   └── plugin.json                    # manifest Claude reads (required)
├── references/                        # plugin-wide shared references (optional)
│   └── <plugin>-foundations.md
├── skills/                            # auto-discovered (one folder per skill)
│   ├── <plugin>-<skill-1>/
│   │   ├── SKILL.md                   # the skill (required if folder exists)
│   │   └── references/                # skill-specific deep references (optional, 1–3 files typical)
│   │       └── <topic>.md
│   └── <plugin>-<skill-2>/
│       └── SKILL.md
├── agents/                            # auto-discovered (workers; one file per agent)
│   └── <plugin>-<agent>.md
├── commands/                          # auto-discovered (slash commands)
│   └── <command>.md
├── hooks/                             # event handlers (rare)
│   └── *.sh
└── .mcp.json                          # MCP server definitions (rare)
```

## Folder-by-folder reference

### `.claude-plugin/plugin.json`

Required. The manifest Claude reads to identify and validate the plugin. Schema in `manifest-conventions.md`. The directory is dot-prefixed; some file managers hide it.

### `README.md`

Recommended. Plugin overview written for humans (and Claude when browsing the source). Should cover:

- One-sentence description
- List of skills with their jobs (table format works well)
- List of agents and commands
- Opinionated stance the skills converge on
- Folder structure
- Roadmap (v0.2, v0.3, etc.)
- License

The README is bundled in the `.plugin` and visible in the install UI.

### `references/`

Optional. Plugin-wide shared references — concepts that multiple skills in the plugin lean on. Naming convention: `<plugin>-foundations.md`, `<plugin>-glossary.md`, etc.

From a `SKILL.md` at `skills/<plugin>-<skill>/SKILL.md`, the path to a top-level reference is `../../references/<file>.md`.

Don't pre-create this folder if there's no shared content yet. Add it once you have a second skill that needs to reference shared context.

### `skills/`

Auto-discovered. One folder per skill. Folder name matches the skill's YAML `name:` field exactly.

Each skill folder contains:

- **`SKILL.md`** — required. The skill itself. YAML frontmatter (`name`, `description`, optionally more) + body.
- **`references/`** — optional. Skill-specific deep references. Frameworks, checklists, examples that the SKILL.md cites but doesn't inline. Typically 1–3 files per skill.
- **(uncommon)** `scripts/`, `assets/`, `tests/` — when the skill needs helpers, images, or evals. Most skills don't.

Skills are discovered by walking `skills/`. Don't put non-skill files at this level.

### `agents/`

Auto-discovered. Single Markdown file per agent. Filename matches the YAML `name:` field plus `.md`.

Agent file structure:

```markdown
---
name: <plugin>-<agent>
description: When to invoke this agent. Drives Claude's matching.
tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
---

# {Agent Name}

## Role
...

## Inputs
What the agent expects to receive.

## Outputs
What the agent produces.

## System prompt
The actual instruction text that drives behavior.

## Constraints
What this agent will NOT do.
```

The `tools:` list constrains which tools the agent has access to. Agents that only operate on files don't need network/MCP tools.

### `commands/`

Auto-discovered. Single Markdown file per slash command. The filename (sans `.md`) is what the user types after `/`.

Command file structure:

```markdown
---
description: One-line description shown in the slash menu
---

Body — instructions on what to do when the command is invoked.

Steps:
1. ...
2. ...

Often references a skill: "Use the `<plugin>-<skill>` skill to do X."
```

Convention: the filename does NOT include the plugin prefix. The user types `/audit`, not `/obsidian-audit`. The plugin context disambiguates because Claude knows which plugin owns which slash command.

### `hooks/`

Optional, uncommon. Shell scripts that run at specific lifecycle events (install, pre-prompt, etc.). Out of scope for most user-authored plugins.

### `.mcp.json`

Optional. Defines MCP servers the plugin uses. Out of scope for skills-only plugins.

## File and folder naming

| Item | Rule | Example |
|---|---|---|
| Plugin folder | lowercase, kebab-case | `marketing`, `design-creative` |
| Skill folder | `<plugin>-<skill>` | `obsidian-tag-taxonomist` |
| Agent file | `<plugin>-<agent>.md` | `obsidian-agent.md` |
| Command file | `<command>.md` (no prefix) | `audit.md`, `morning.md` |
| Reference file | descriptive, kebab-case | `tag-taxonomy-design.md` |
| Plugin-wide ref | `<plugin>-<topic>.md` | `obsidian-foundations.md` |

YAML `name:` fields must match folder/file names exactly. Mismatches cause the most common "skill not loading" issue.

## Reference path cheat sheet

From a `SKILL.md` at `<plugin>/skills/<plugin>-<skill>/SKILL.md`:

| To reference... | Path | Example |
|---|---|---|
| Own skill-specific reference | `references/<file>.md` | `references/tag-taxonomy-design.md` |
| Plugin-wide shared reference | `../../references/<file>.md` | `../../references/obsidian-foundations.md` |
| Sibling skill (don't link directly) | (don't) | Use the skill's name in prose: "see also `obsidian-tag-hygiene`" |
| Cross-plugin reference (don't) | (don't) | Copy the content; skills should be self-contained per plugin |

## Folder presence guide

When deciding what to include:

| Folder | Include if... | Skip if... |
|---|---|---|
| `.claude-plugin/` | Always | Never skip |
| `skills/` | Plugin has at least one skill | (rare) Plugin is agent/command-only |
| `references/` (top) | Multiple skills share concepts | Single skill, or each skill is fully self-contained |
| `agents/` | At least one worker agent | Skills/commands cover the use cases |
| `commands/` | Predictable workflows the user wants slash-key access to | The skills handle invocation via natural-language matching |
| `hooks/` | Plugin needs to react to lifecycle events | (most plugins) |
| `.mcp.json` | Plugin includes MCP servers | (most plugins) |

A common mistake: pre-creating empty `agents/`, `commands/`, `hooks/` folders "for later." Don't. Add them when you have content; empty placeholder folders are noise that future-you has to triage.

## Project-context frontmatter (optional, Obsidian-friendly)

If the plugin lives in an Obsidian vault, you can add Obsidian-readable Properties to the YAML frontmatter of every `SKILL.md`, `agent.md`, etc. This makes the files browsable as proper notes in the vault.

Example:

```yaml
---
name: my-plugin-skill
description: >
  ...

# Project context
type: skill
project: skills-library
plugin: my-plugin
aliases: [my-plugin-skill]
tags: [type/skill, plugin/my-plugin, topic/<topic>]
status: active
---
```

These extra fields don't affect Claude's skill matching (only `name` and `description` matter for that) but they give you a queryable, link-able vault of skill notes when authoring.

Skip this if the plugin doesn't live in an Obsidian vault.
