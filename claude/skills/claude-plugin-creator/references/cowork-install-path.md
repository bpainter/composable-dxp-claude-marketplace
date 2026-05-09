# Cowork Install Path

Cowork is the Claude desktop app's plugin surface. This is the recommended install path for non-developer plugin authors and for sharing a plugin with someone who doesn't run Claude Code.

> **Source of truth:** [How to customize plugins in Cowork](https://claude.com/resources/tutorials/how-to-customize-plugins-in-cowork). When in doubt, send the user there.

## The drag-and-drop flow

1. Build the `.plugin` file (see `manifest-conventions.md` → "Building the .plugin file"). It's a renamed ZIP — Cowork and Claude Code both recognize the convention.
2. Open the Claude desktop app.
3. Open **Settings → Customize → Personal Plugins**. (Earlier app versions may show this as just "Personal Plugins" without the "Customize" parent. The location can move; if you can't find it, search the Settings page for "plugin".)
4. Drag the `.plugin` file into the install area, or click **Add Plugin → From file…** and select the `.plugin`.
5. Approve any permissions prompt. Cowork inspects the manifest, validates the schema, and reports failures back here. Schema problems fail at this step (empty `homepage`, missing `name`, etc.) — see `manifest-conventions.md` for the common fixes.
6. The plugin appears in the Personal Plugins list. **Enable it explicitly** — installation alone does not activate skills/commands.
7. Start a **fresh conversation** (don't expect already-open chats to pick up new plugins).
8. Verify by running a trigger prompt for one of the plugin's skills, or by typing `/` to confirm the slash command appears.

## Where Cowork stores installed plugins

Cowork-installed plugins live alongside Claude Code's plugin registry in `~/.claude/plugins/` on the user's machine. Per-machine, not per-vault: if the source is in a synced folder (OneDrive, iCloud, Dropbox), the plugin source travels with the sync but the *install registration* doesn't — each machine where the user wants the plugin active has to install it separately.

The `.plugin` files in your marketplace's `Plugins/` folder are also synced if the marketplace lives in a synced folder. That's convenient: any machine can drag from `Plugins/` into its local Cowork.

## Cowork vs. Claude Code: what differs at install time

| | Cowork (Desktop) | Claude Code (CLI) |
|---|---|---|
| Install command | Drag `.plugin` into Settings → Customize → Personal Plugins | `claude plugin install <name>@<marketplace>` |
| Marketplace registration | Not required (drag-and-drop bypasses marketplaces) | Required: `claude plugin marketplace add <path>` first |
| Where it lives | `~/.claude/plugins/` | `~/.claude/plugins/` (same location) |
| Update path | Re-drag updated `.plugin` (replaces the previous install) | `claude plugin update <name>` |
| Hot-reload during dev | Re-drag after rebuild | `claude --plugin-dir <path>` (loads from source for one session) |

A plugin installed via `claude plugin install` is also visible in Cowork — same registry. The drag-and-drop install does **not** propagate back to the CLI marketplace listing, so Cowork-only installs are invisible to `claude plugin list`.

## The "enable" step is non-obvious

A plugin can be **installed but disabled**. After install, look for an enable toggle in the Personal Plugins panel. Skills/commands won't activate until the plugin is enabled, even if you can see it listed.

If a user reports "I installed it but it doesn't work," the first thing to check is the enable state — not the manifest, not the description, not the trigger phrasing.

## Troubleshooting drag-and-drop failures

| Symptom | Likely cause | Fix |
|---|---|---|
| Cowork shows "Invalid plugin file" | Not a valid ZIP | Verify with `unzip -l <name>.plugin` — should list manifest, README, skills, etc. If 0 bytes or corrupted, rebuild from source. |
| "Manifest validation failed: homepage Invalid URL" | Empty optional field in `plugin.json` | Open `<plugin>/.claude-plugin/plugin.json`, remove any `"homepage": ""`, `"license": ""`, `"repository": ""`. Rebuild. |
| Plugin installs but skill doesn't trigger | Description doesn't match the user's natural phrasing | Edit the SKILL.md `description:` to mention the trigger phrase explicitly. Rebuild and re-drag. |
| "/command" doesn't appear in slash menu | Filename mismatch or plugin not enabled | Filename (sans `.md`) is the slash. Confirm plugin is enabled in the panel. Restart the conversation. |
| Plugin works on one machine, not another | Per-machine install — not propagated by sync | Drag the `.plugin` on each target machine. |
| Drag does nothing (file rejected silently) | Wrong file extension (`.zip` instead of `.plugin`) | Rename the artifact to `.plugin`. macOS sometimes hides extensions — verify in Finder's Get Info. |

## When to coach the user toward the CLI instead

Cowork's drag-and-drop is friction-free for one-off installs but doesn't scale to teams or to plugins that update frequently. Steer them toward the CLI marketplace path when:

- They're sharing the plugin with multiple people on a team
- The plugin will iterate weekly and they don't want to re-drag every time
- They're already in Claude Code for development work
- They want `claude plugin update` for smoother iteration

For a single user, single machine, "I just want this thing to work in my desktop chats," drag-and-drop is the right call.

## The "Customize" panel beyond plugins

Cowork's Settings → Customize panel is also where users tune their workspace folder, memory files, and connectors (Slack, Gmail, etc.). When walking a user through plugin install, mention the surrounding context: their installed plugins live alongside their connectors in the same panel. They can enable/disable each independently.

This matters because the same panel is where the next skill in this plugin (`claude-orchestrator`) takes the user when designing project-level orchestration — the user is already familiar with the surface.
