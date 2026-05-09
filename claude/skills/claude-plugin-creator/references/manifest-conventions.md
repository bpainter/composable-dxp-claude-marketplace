# Manifest Conventions

Schemas for `plugin.json` and `marketplace.json`, common gotchas, and the build process for `.plugin` files.

## `plugin.json` schema

Path: `<plugin>/.claude-plugin/plugin.json`

### Required field

| Field | Type | Notes |
|---|---|---|
| `name` | string | Lowercase, single word or kebab-case. Matches the plugin's folder name. |

### Optional fields

| Field | Type | Notes |
|---|---|---|
| `version` | string | SemVer (e.g., `0.1.0`). Bump for releases. |
| `description` | string | One paragraph; shown in install UI and marketplace listings. |
| `author` | object | `{ "name": "...", "email": "..." }` (also `url`) |
| `license` | string | SPDX identifier (e.g., `"MIT"`, `"Apache-2.0"`). Don't use empty string. |
| `homepage` | string | URL. Don't use empty string — fails URL validation. |
| `repository` | string | URL. Same caveat. |
| `keywords` | array of strings | For discoverability in marketplaces. |

### The empty-field trap

The single most common manifest validation error:

```json
{
  "name": "obsidian",
  "homepage": "",      // ✗ Empty string fails URL validation
  "license": ""        // ✗ Empty string fails SPDX validation
}
```

**Fix: omit the field entirely**, don't leave it empty:

```json
{
  "name": "obsidian"
}
```

Or fill it with a real value:

```json
{
  "name": "obsidian",
  "homepage": "https://github.com/me/obsidian-plugin",
  "license": "MIT"
}
```

Apply this rule to every optional field. If you don't have a real value, omit.

### Example: minimal manifest

```json
{
  "name": "marketing"
}
```

Valid. Will install. Nothing extra.

### Example: complete manifest

```json
{
  "name": "obsidian",
  "version": "0.4.0",
  "description": "Opinionated toolkit for Obsidian vault organization. 12 skills, 1 agent, 4 slash commands.",
  "author": {
    "name": "Bermon Painter",
    "email": "claude@bermonpainter.com"
  },
  "license": "MIT",
  "keywords": [
    "obsidian",
    "pkm",
    "knowledge-management"
  ]
}
```

## `marketplace.json` schema

Path: `<marketplace>/.claude-plugin/marketplace.json`

### Required fields

| Field | Type | Notes |
|---|---|---|
| `name` | string | Marketplace name (e.g., `"second-brain"`) |
| `owner` | object | `{ "name": "...", "email": "..." }` |
| `plugins` | array | List of plugin entries |

### Plugin entry schema

Each entry in `plugins[]`:

| Field | Type | Notes |
|---|---|---|
| `name` | string | Plugin name (matches the plugin's `plugin.json` `name`) |
| `source` | string | Path or URL to the plugin source |
| `description` | string | Short description shown in marketplace listings |
| `version` | string | SemVer matching the plugin's manifest version |

### Source paths

`source` can be:

- **Relative path** — relative to the marketplace root (one level above `.claude-plugin/`). Example: `"./obsidian"` resolves to the `obsidian/` folder at the marketplace root.
- **URL** — for remote distribution. The marketplace clones/downloads from there.

Mixing relative paths and URLs in the same marketplace is fine.

### Example marketplace.json

```json
{
  "name": "second-brain",
  "owner": {
    "name": "Bermon Painter",
    "email": "claude@bermonpainter.com"
  },
  "metadata": {
    "description": "Personal marketplace of Claude plugins.",
    "version": "0.1.0"
  },
  "plugins": [
    {
      "name": "obsidian",
      "source": "./obsidian",
      "version": "0.4.0",
      "description": "Obsidian vault organization toolkit"
    },
    {
      "name": "claude",
      "source": "./claude",
      "version": "0.1.0",
      "description": "Claude tooling skills — plugin authoring, customization"
    }
  ]
}
```

## SKILL.md YAML frontmatter

Required:

```yaml
---
name: <plugin>-<skill-name>
description: >
  One paragraph describing the skill, when to invoke it, what triggers it,
  and what related skills it pairs with.
---
```

Optional Obsidian-readable Properties block (if the plugin lives in an Obsidian vault):

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

The Obsidian-readable fields don't affect Claude's matching; they make the files queryable as Obsidian notes.

## Agent file frontmatter

```yaml
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
```

The `tools:` list is the constraint surface. Agents only get the listed tools — important for principle-of-least-privilege.

## Command file frontmatter

```yaml
---
description: One-line description shown in the slash menu when the user types /
---
```

The filename (sans `.md`) is the slash command. So `commands/audit.md` becomes `/audit`.

## Building the .plugin file

`.plugin` is a renamed ZIP. The build process:

```bash
PLUGIN_NAME=my-plugin
ROOT="/path/to/marketplace"

# 1. Stage in /tmp (avoids OneDrive sync interference)
STAGE="/tmp/${PLUGIN_NAME}-stage"
rm -rf "$STAGE"
mkdir -p "$STAGE"
rsync -a \
  --exclude='.DS_Store' \
  --exclude='.fuse_hidden*' \
  "$ROOT/$PLUGIN_NAME/" "$STAGE/"

# 2. Zip the staging dir
cd "$STAGE"
zip -rq "/tmp/${PLUGIN_NAME}.plugin" . -x "*.DS_Store"

# 3. Move into the marketplace's Plugins/ folder
cp -f "/tmp/${PLUGIN_NAME}.plugin" "$ROOT/Plugins/${PLUGIN_NAME}.plugin"
```

### Why stage in /tmp

OneDrive's "Files On-Demand" feature can leave files as online-only placeholders. When `zip` reads them in bulk, it sometimes hits "Resource deadlock avoided" errors and writes 0-byte entries.

Workaround: `rsync` the source into `/tmp` first (forces real reads), then zip from `/tmp`. The deadlocks happen during reads, but writing the zip file from a fully-materialized source works.

### When you still get 0-byte references

If staging doesn't materialize files, force it manually before staging:

```bash
find "$ROOT/$PLUGIN_NAME" -name '*.md' -exec cat {} > /dev/null \;
```

This reads each file, triggering OneDrive to download it locally.

### Build exclusions worth applying

Common exclusions when staging or zipping:

- `.DS_Store` — macOS metadata noise
- `.fuse_hidden*` — OneDrive sync temp files
- `node_modules/`, `dist/`, etc. — if the plugin includes any code build artifacts
- Deprecated folder locations during structural migrations (e.g., old `orchestrators/`, `_references/`)
- Placeholder READMEs that are superseded by real content

## Plugin lifecycle

| Phase | What you do |
|---|---|
| Scaffold | Create folder, write `plugin.json`, write `README.md`, create first `SKILL.md` |
| Develop | Iterate on skill content, test via `claude --plugin-dir <path>` |
| Ship v0.1 | Build `.plugin`, install via Personal Plugin or marketplace, verify trigger phrases |
| Iterate | Edit source, rebuild `.plugin`, reinstall (or `claude plugin update`) |
| Add components | New skills, agents, or commands — bump minor version |
| Breaking change | Rename or remove components — bump major version |

## Versioning conventions

SemVer (`major.minor.patch`):

- **Patch** (`0.1.0` → `0.1.1`) — content tweaks, typo fixes, description refinements
- **Minor** (`0.1.0` → `0.2.0`) — new skills, new agents, new commands; structural additions that don't break existing components
- **Major** (`0.x` → `1.0.0`) — breaking changes: renamed skills, removed components, fundamentally new structure

Pre-1.0 versions allow more flexibility — minor versions can include breaking changes during early iteration. Once you hit `1.0.0`, treat the API (skill names, command names) as stable.

## Common manifest errors and their fixes

| Error | Cause | Fix |
|---|---|---|
| `homepage: Invalid URL` | Empty string in optional field | Omit the field |
| `license: Invalid SPDX identifier` | Empty string or non-standard value | Omit, or use `"MIT"`, `"Apache-2.0"`, etc. |
| `name: Required` | Missing required field | Add `"name": "..."` |
| Plugin not discoverable | `name` in plugin.json doesn't match folder | Make them match exactly |
| Skill not triggering | Description doesn't include the user's natural phrasing | Expand `description:` field, reinstall |
| Skill name collision | Two plugins use the same skill name | Apply plugin prefix consistently (`<plugin>-<skill-name>`) |
| Marketplace plugin not found | `source` path wrong in `marketplace.json` | Verify path is relative to marketplace root |
| Build error: missing files | OneDrive dehydration | Force-materialize via `cat` then rebuild |

## Distribution options

| Option | Audience | Mechanism |
|---|---|---|
| Personal Plugin | Yourself | Drag `.plugin` into Claude Desktop's Settings |
| Local marketplace | Yourself + your devices | `claude plugin marketplace add <local path>` |
| Shared file/repo | Small team | Share the `.plugin` file or git repo of the source folder |
| Public marketplace | Internet | Host the marketplace at a URL; users add via `claude plugin marketplace add <URL>` |
| Anthropic's marketplace | Public, official | Submit through Anthropic's plugin submission process (out of scope for this skill) |
