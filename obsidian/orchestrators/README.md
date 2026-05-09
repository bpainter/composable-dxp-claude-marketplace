# Orchestrators (deprecated folder — see skills/)

This folder is **deprecated as of v0.3.0**. Orchestrator workflows have been consolidated into `skills/` so they're auto-discovered by Claude.

## Where the orchestrator-shaped skills live now

| Old location (deprecated) | New location (canonical) |
|---|---|
| `orchestrators/obsidian-vault-health-sweep.md` | `skills/obsidian-vault-health-sweep/SKILL.md` |
| `orchestrators/obsidian-tag-cleanup-flow.md` | `skills/obsidian-tag-cleanup-flow/SKILL.md` |
| `orchestrators/obsidian-vault-bootstrap.md` | `skills/obsidian-vault-bootstrap/SKILL.md` |

The content is the same. Only the path and frontmatter changed (frontmatter no longer has the non-standard `stages:` field).

## Why the move

- **Auto-discovery** — Claude auto-discovers `skills/` but not `orchestrators/`. Putting orchestrator-shaped workflows in `skills/` makes them invoke-able by phrase ("run a vault health sweep") without manual loading.
- **One primitive** — orchestrators were a custom concept; treating them as a *flavor* of skill (with Goal / Stages / Handoffs / Failure modes body structure) keeps the plugin model simple.
- **Naming convention preserves the distinction** — orchestrator-shaped skills still have suffixes `-sweep`, `-flow`, `-bootstrap`, so they're recognizable in the catalog.

## What you should do

If you can manually delete the legacy `.md` files in this folder, please do — they're superseded. The folder itself can stay as documentation, or you can remove it entirely; nothing references it.

## Adding new orchestrator-shaped workflows

Don't add them here. Add them to `skills/` as a regular skill folder with a `SKILL.md` whose body uses the orchestrator structure (Goal, Stages, Handoffs, Failure modes). Use the `-sweep` / `-flow` / `-bootstrap` filename suffix to mark it as orchestrator-shaped.

See the three existing orchestrator-shaped skills in `skills/` as templates.
