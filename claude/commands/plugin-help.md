---
description: Get guided help building, validating, or debugging a Claude plugin

# Project context
type: command
project: skills-library
plugin: claude
tags: [type/command, plugin/claude]
status: active
---

Use the `claude-plugin-creator` skill to help the user with whatever plugin task they're working on.

Decide based on what they ask:

- **Starting a new plugin** — walk them through scoping (name, domain, scope, components, marketplace), then scaffold the folder structure, write `plugin.json`, draft a starter `SKILL.md`
- **Debugging an install error** — diagnose the manifest (empty optional fields, missing required `name`, malformed JSON), the build (corrupted ZIP, missing files, dehydrated OneDrive references), or the install path (wrong CLI syntax, marketplace not registered, plugin name collision)
- **Building a `.plugin` file** — walk through the staging-in-/tmp build process, including OneDrive workarounds (force-materialize references, exclude `.fuse_hidden*`)
- **Adding components to an existing plugin** — new skill, new agent, new slash command, with proper naming conventions and YAML frontmatter
- **Registering in a marketplace** — update `marketplace.json` `plugins[]`, refresh the registration with `claude plugin marketplace update`

Reference files for the skill:
- `claude-plugin-creator/references/plugin-anatomy.md` — folder structure, file naming, what each piece is for
- `claude-plugin-creator/references/manifest-conventions.md` — plugin.json schema, marketplace.json schema, build commands, common errors with fixes

Don't write the actual skill content (Role, Methodology, etc.) — that's the user's domain expertise. Set up the structure that holds those things and make sure the build and install path works.
