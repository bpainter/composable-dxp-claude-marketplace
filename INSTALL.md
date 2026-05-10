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
| `algolia` | 0.1.0 | `algolia/` | `Plugins/algolia.plugin` |
| `behavioral-economics` | 0.3.0 | `behavioral-economics/` | `Plugins/behavioral-economics.plugin` |
| `brand` | 0.3.0 | `brand/` | `Plugins/brand.plugin` |
| `bynder` | 0.1.0 | `bynder/` | `Plugins/bynder.plugin` |
| `claude` | 0.3.0 | `claude/` | `Plugins/claude.plugin` |
| `consulting` | 0.4.0 | `consulting/` | `Plugins/consulting.plugin` |
| `contentful` | 0.1.0 | `contentful/` | `Plugins/contentful.plugin` |
| `cx` | 0.2.0 | `cx/` | `Plugins/cx.plugin` |
| `design` | 0.4.0 | `design/` | `Plugins/design.plugin` |
| `facilitation` | 0.4.0 | `facilitation/` | `Plugins/facilitation.plugin` |
| `innovation` | 0.2.0 | `innovation/` | `Plugins/innovation.plugin` |
| `leadership` | 0.3.0 | `leadership/` | `Plugins/leadership.plugin` |
| `marketing` | 0.2.0 | `marketing/` | `Plugins/marketing.plugin` |
| `obsidian` | 0.4.0 | `obsidian/` | `Plugins/obsidian.plugin` |
| `product-management` | 0.1.0 | `product-management/` | `Plugins/product-management.plugin` |
| `project-management` | 0.4.0 | `project-management/` | `Plugins/project-management.plugin` |
| `software-engineering` | 0.3.1 | `software-engineering/` | `Plugins/software-engineering.plugin` |
| `vercel` | 0.1.0 | `vercel/` | `Plugins/vercel.plugin` |

## Recommended path: Cowork via Git marketplace

Cowork's marketplace registration requires a **Git URL**. This marketplace lives in a private GitHub repo under the Slalom org.

```bash
# In Claude Desktop's Cowork mode (or via the desktop app's Settings UI):
claude plugin marketplace add https://github.com/Slalom/composable-dxp-claude-marketplace.git
claude plugin marketplace update second-brain

# Install plugins (Cowork)
claude plugin install obsidian@second-brain
claude plugin install behavioral-economics@second-brain
# ... etc.
```

> **Marketplace name vs repo name:** the GitHub repo is `composable-dxp-claude-marketplace`; the *marketplace* name (what you reference in `<plugin>@<marketplace>`) is `second-brain`, set in `.claude-plugin/marketplace.json`. They don't have to match.

For the private Slalom repo, ensure your Mac has a working GitHub auth path (SSH key, `gh` CLI, or HTTPS personal access token) with read access to the `Slalom` org. SSH config alias `github-slalom` (with `IdentityFile ~/.ssh/id_ed25519_slalom`) is the working setup for this repo.

## Claude Code CLI

Same marketplace, but Claude Code accepts local paths *or* Git URLs:

```bash
# Local path (works only for Claude Code, not Cowork)
claude plugin marketplace add "/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"

# Or Git URL (works for both Claude Code and Cowork)
claude plugin marketplace add https://github.com/Slalom/composable-dxp-claude-marketplace.git

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

## Publishing the marketplace to GitHub

The marketplace lives at `git@github.com:Slalom/composable-dxp-claude-marketplace.git` (private, Slalom org). Pushing requires the Slalom-specific SSH key, configured via this `~/.ssh/config` host alias:

```
Host github-slalom
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_slalom
  AddKeysToAgent yes
  UseKeychain yes
```

Use the alias in remote URLs (no `git@` prefix — the alias's `User git` directive handles that). For a fresh push or to replace the remote contents:

```bash
cd "/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"

# Stage and commit current state
git add -A
git commit -m "Replace marketplace with current Composable DXP plugin set"

# Add the remote using the SSH alias (note: no git@ prefix)
git remote add origin github-slalom:Slalom/composable-dxp-claude-marketplace.git

# Confirm it resolved correctly
git remote -v

# Quick auth sanity check — should report your Slalom GitHub identity
ssh -T github-slalom

# Force-push main, replacing whatever is there
git push --set-upstream origin main --force
```

If the existing remote uses `master` as the default branch, push as `main:master` and rename it on GitHub afterward.

> **OneDrive caveat:** Git inside a OneDrive-synced folder can occasionally produce sync conflicts on `.git/index` and similar files during heavy commits. For a low-traffic personal marketplace this is usually fine, but if you hit `.git` corruption, consider moving the repo outside OneDrive (e.g., `~/Code/composable-dxp-claude-marketplace/`) and treat OneDrive as a separate backup.

> **"Repository not found" on push:** GitHub returns this error both for missing repos and for keys without access. If the repo exists at the path above, the issue is almost always SSH using the wrong key. Check `ssh-add -l` lists `id_ed25519_slalom`, run `ssh -T github-slalom` to confirm it authenticates as your Slalom identity, and verify the URL in `git remote -v` uses the `github-slalom:` prefix (not `git@github.com:`).

## Updating the marketplace after a change

```bash
cd "/Users/bermon.painter/Library/CloudStorage/OneDrive-Slalom/Slalom Second Brain/80_Skills_and_Agents"

# Make changes, rebuild relevant .plugin files
# Then commit and push (using the github-slalom alias)
git add .
git commit -m "Update behavioral-economics to 0.4.0; rebuild .plugin"
git push origin main

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
