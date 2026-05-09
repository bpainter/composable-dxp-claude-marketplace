---
description: Bootstrap a new Obsidian vault — folders, filename conventions, frontmatter schema, tag taxonomy, templates, starter MOCs, home note, documentation notes

# Project context
type: command
project: skills-library
plugin: obsidian
tags: [type/command, plugin/obsidian]
status: active
---

Use the `obsidian-vault-bootstrap` orchestrator-shaped skill to set up a brand-new Obsidian vault with conventions in place from day one.

The flow runs 8 stages:

1. **Folders** — `obsidian-folder-architect` designs top-level structure (default: by note type)
2. **Filename conventions** — `obsidian-filename-conventions` establishes per-type rules
3. **Frontmatter schema** — `obsidian-frontmatter-schema-designer` defines properties per note type
4. **Tag taxonomy** — `obsidian-tag-taxonomist` decides what tags are *for* and v1 list
5. **Templates** — create one template per note type, pre-filled with required schema
6. **Starter MOCs** — `obsidian-moc-architect` drafts top-level MOCs for the user's active topics
7. **Home note** — design `Home.md` as the meta-MOC; pin the tab
8. **Documentation notes** — write Tag Taxonomy.md, Frontmatter Schema.md, Filename Conventions.md as in-vault source of truth

Pick the user's vault flavor first (personal/writing, research, generalist work+life) — this calibrates the choices in each stage.

Expected duration: ~2 hours for a focused setup; can be done across two sessions.

Don't skip stages. Each builds on the previous. If the user wants to skip Stage 4 ("I'll figure tags out later"), push back — tags accumulate fast without v1 taxonomy.

After bootstrap completes, the vault is *ready to grow*. The first `obsidian-vault-health-sweep` should run roughly 90 days later as the validation pass.

Don't run bootstrap on a vault that already has 200+ notes with active conventions — that's a restructure, not a bootstrap. Use `/sweep` instead.
