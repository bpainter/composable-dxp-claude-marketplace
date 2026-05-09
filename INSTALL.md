# Installing Plugins from this Marketplace

This marketplace publishes 18 plugins to **Claude Desktop (Cowork mode)**, **Claude Code (CLI)**, and via **drag-and-drop `.plugin` files** as a fallback.

> **Heads-up:** Claude Desktop has three contexts and they handle plugins differently:
>
> 1. **Claude Chat** — no plugin support; skills must be uploaded individually per conversation.
> 2. **Claude Cowork** — Personal Plugins panel; **marketplace requires a Git URL** (local paths not supported); drag-and-drop `.plugin` files supported.
> 3. **Claude Code (CLI + Desktop integration)** — Personal plugins live in `~/.claude.json`, separate from Cowork; **marketplace accepts local paths or Git URLs**.
>
> Cowork and Code maintain separate plugin registries even when both run on the same Mac. The CLI's `claude plugin remove <name>` only manages Code's registry — it cannot touch Cowork's "Local uploads" store.

## Marketplace contents

| Plugin | Version | Source | Artifact |
|---|---|---|---|
| `obsidian` | 0.4.0 | `obsidian/` | `Plugins/obsidian.plugin` |
| `claude` | 0.2.0 | `claude/` | `Plugins/claude.plugin` |
| `behavioral-economics` | 0.3.0 | `behavioral-economics/` | `Plugins/behavioral-economics.plugin` |
| `brand` | 0.3.0 | `brand/` | `Plugins/brand.plugin` |
| `consulting` | 0.4.0 | `consulting/` | `Plugins/consulting.plugin` |
| `innovation` | 0.2.0 | `innovation/` | `Plugins/innovation.plugin` |
| `facilitation` | 0.4.0 | `facilitation/` | `Plugins/facilitation.plugin` |
| `cx` | 0.2.0 | `cx/` | `Plugins/cx.plugin` |
| `design` | 0.4.0 | `design/` | `Plugins/design.plugin` |
| `leadership` | 0.3.0 | `leadership/` | `Plugins/leadership.plugin` |
| `marketing` | 0.2.0 | `marketing/` | `Plugins/marketing.plugin` |
| `project-management` | 0.4.0 | `project-management/` | `Plugins/project-management.plugin` |
| `product-management` | 0.1.0 | `product-management/` | `Plugins/product-management.plugin` |
| `software-engineering` | 0.3.1 | `software-engineering/` | `Plugins/software-engineering.plugin` |
| `contentful` | 0.1.0 | `contentful/` | `Plugins/contentful.plugin` |
| `algolia` | 0.1.0 | `algolia/` | `Plugins/algolia.plugin` |
| `vercel` | 0.1.0 | `vercel/` | `Plugins/vercel.plugin` |
| `bynder` | 0.1.0 | `bynder/` | `Plugins/bynder.plugin` |

## Recommended path: Cowork via Git marketplace

Cowork's marketplace registration requires a **Git URL**, so this marketplace publishes to a private GitHub repo. Replace the URL below with your actual repo URL once published.

```bash
# In Claude Desktop's Cowork mode (or via the desktop app's Settings UI):
claude plugin marketplace add https://github.com/bermonpainter/claude-plugins-skills-agents.git
claude plugin marketplace update claude-plugins-skills-agents

# Install plugins (Cowork)
claude plugin install obsidian@second-brain
claude plugin install behavioral-economics@second-brain
# ... etc.
```

> **Note on the marketplace name:** the GitHub repo is named `claude-plugins-skills-agents`, but the *marketplace* name (what you reference in `<plugin>@<marketplace>`) is `second-brain` — that's the `name` field inside `.claude-plugin/marketplace.json`. The repo name and marketplace name don't have to match.

For private GitHub repos, ensure your Mac has a working GitHub auth path (SSH key, `gh` CLI, or HTTPS personal access token).

## Claude Code CLI

Same marketplace, but Claude Code accepts local paths *or* Git URLs:

```bash
# Local path (works only for Claude Code, not Cowork)
claude plugin marketplace add "/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"

# Or Git URL (works for both Claude Code and Cowork)
claude plugin marketplace add https://github.com/bermonpainter/claude-plugins-skills-agents.git

claude plugin install obsidian@second-brain
claude plugin install <name>@second-brain
```

CLI-installed plugins land in `~/.claude.json` and `~/.claude/plugins/` and are visible to Claude Code anywhere on the machine.

## Drag-and-drop `.plugin` fallback (Cowork or Code)

For one-off installs without registering a marketplace:

1. Locate the `.plugin` file in `Plugins/<name>.plugin`
2. Open **Settings → Customize → Personal Plugins** in Claude Desktop
3. Drag the file into the install area, or **Add Plugin → From file...**
4. Approve permissions; **enable** the plugin (installation alone doesn't activate it)
5. Start a fresh conversation; verify with a trigger phrase or `/` slash command

## Verification

Once installed, test each plugin with a trigger phrase. A few quick checks:

- "What's in the obsidian plugin?" → `obsidian-overview` should activate
- "/audit" → the `/audit` slash command should appear in the slash menu
- "Help me design a nudge" → `behavioral-economics-choice-architect` should activate
- "Run a RUF kickoff" → `project-management-raid-log` should activate

If a skill doesn't trigger on its expected phrase, the description's trigger language is too narrow — expand the description, rebuild the `.plugin` (or push a new commit if installing via marketplace), reinstall.

## Updating

### Marketplace install (Cowork or Code)

```bash
# Refresh the marketplace listing (after a git push)
claude plugin marketplace update second-brain

# Update a specific plugin
claude plugin update obsidian@second-brain
```

### Drag-and-drop install

Rebuild the `.plugin` and re-drag — it overwrites the prior install.

## Building `.plugin` files from source

```bash
PLUGIN_NAME=obsidian
ROOT="/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"

# Stage in /tmp to avoid OneDrive sync interference
STAGE="/tmp/${PLUGIN_NAME}-stage-$$"
rm -rf "$STAGE"
mkdir -p "$STAGE"
rsync -a --exclude='.DS_Store' --exclude='.fuse_hidden*' --exclude='*.bak' \
  "$ROOT/$PLUGIN_NAME/" "$STAGE/"

# Build the .plugin (ZIP renamed)
cd "$STAGE"
zip -rq "/tmp/${PLUGIN_NAME}.plugin" . -x "*.DS_Store" -x "*.bak"

# Move into Plugins/
cp -f "/tmp/${PLUGIN_NAME}.plugin" "$ROOT/Plugins/${PLUGIN_NAME}.plugin"

# Cleanup
rm -rf "$STAGE" "/tmp/${PLUGIN_NAME}.plugin"
```

For all 18 plugins at once, see the build loop in this repo's earlier conversation history; or run the same command in a `for` loop.

## Publishing the marketplace to GitHub (one-time setup)

If this repo isn't on GitHub yet:

```bash
cd "/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"

# Initialize git
git init
git add .
git commit -m "Initial marketplace: 18 plugins"

# Create the repo on GitHub (private). Requires `gh` CLI authenticated.
gh repo create claude-plugins-skills-agents --private --source=. --remote=origin

# Or if using web UI: create the repo at github.com/new (private),
# then connect:
git remote add origin https://github.com/bermonpainter/claude-plugins-skills-agents.git
git branch -M main
git push -u origin main
```

> **OneDrive caveat:** Git inside an OneDrive-synced folder can occasionally produce sync conflicts on `.git/index` and similar files during heavy commits. For a low-traffic personal marketplace this is usually fine, but if you hit `.git` corruption, consider moving the repo outside OneDrive (e.g., `~/Code/claude-plugins-skills-agents/`) and treat OneDrive as a separate backup.

## Updating the marketplace after a change

```bash
cd "/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"

# Make changes, rebuild relevant .plugin files
# Then commit and push
git add .
git commit -m "Update behavioral-economics to 0.4.0; rebuild .plugin"
git push

# Refresh the marketplace listing in Claude Code / Cowork
claude plugin marketplace update second-brain
```

## Cleaning up stale Cowork "Local uploads"

If Cowork's Personal Plugins panel shows old drag-and-drop installs that don't match the current marketplace (e.g., `Business`, `Delivery`, `Diy`, `Health`):

1. **Try the trash icon** in the Personal Plugins panel. There's a known bug ([#28068](https://github.com/anthropics/claude-code/issues/28068)) where uninstall sometimes fails silently — restart Claude Desktop and retry.
2. The CLI's `claude plugin remove` does **not** affect Cowork's Local uploads store (it only manages `~/.claude.json`, which is the Claude Code registry).
3. Cowork's local-uploads store is in the Claude Desktop app's sandboxed application support directory — not user-editable in any documented way.
4. If the UI's trash icon won't clear them, the practical workaround is to **install via the Git marketplace** (a separate registry). The stale Local uploads will still show but won't conflict with marketplace plugins.

## Troubleshooting

**"Marketplace not found"** — Check the URL or path. Git URLs need to end in `.git` and the repo must be accessible to your auth.

**"Plugin already installed"** — Remove via UI trash icon, then reinstall. `claude plugin remove <name>` works for CLI installs only.

**`.plugin` file rejected on import** — File must be a valid ZIP. Verify with `unzip -l Plugins/<name>.plugin`. If 0 bytes or corrupted, rebuild from source.

**Manifest validation error (e.g., "homepage: Invalid URL")** — Empty optional fields fail validation. Open `<plugin>/.claude-plugin/plugin.json`, remove any empty-string fields. Rebuild.

**Skill not loading on the right phrase** — The skill's `description:` field drives matching. Open the SKILL.md, expand the description to mention the trigger phrase explicitly, save, rebuild, reinstall.

**OneDrive sync conflicts** — If two devices edit the same SKILL.md simultaneously, OneDrive may create conflict files. Resolve manually; only one SKILL.md per skill folder is valid.

**`.plugin` build has 0-byte references** — OneDrive's "Files On-Demand" can leave files as online-only placeholders. Force-materialize before building:
```bash
find <plugin>/ -name '*.md' -exec cat {} > /dev/null \;
```

## Per-machine vs vault-shared installs

The marketplace *source* lives in OneDrive, so it's the same on every device that syncs the vault. Once published to GitHub, every machine pulls from the same canonical source. But Claude's *installed plugin state* is per-machine — each Mac has its own `~/.claude.json` (Code) and Cowork local-uploads store. You must `claude plugin install` on each machine where you want a plugin active.

## Reading order for getting started

1. This file — install paths
2. `README.md` — marketplace overview and conventions
3. `obsidian/README.md` — the canonical example plugin (multi-skill, with agent and commands)
4. `claude/skills/claude-plugin-creator/SKILL.md` — the skill that helps you build new plugins
5. `claude/skills/claude-orchestrator/SKILL.md` — the skill that helps you orchestrate across plugins for a Cowork project
