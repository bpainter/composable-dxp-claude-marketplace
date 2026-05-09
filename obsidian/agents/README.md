# Obsidian Plugin / Agents

Agents that act on an Obsidian vault. **Empty in v0.1.0** — placeholder for v0.3.

## Distinction from skills

- **Skills** (in `../skills/`) are advisor personas. They teach Claude *how to think* about a vault. They don't take action on their own.
- **Agents** (here) are workers. They read, write, refactor, link, and clean. An agent typically pulls in one or more skills as reference, then acts on files.

## Planned for v0.3

- `obsidian-vault-scanner` — walks the vault and builds an inventory (notes, tags, links, orphans, frontmatter shape)
- `obsidian-tag-cleaner` — executes hygiene plans produced by `obsidian-tag-hygiene`
- `obsidian-frontmatter-normalizer` — applies a schema across notes; backfills required properties
- `obsidian-moc-generator` — drafts a starter MOC from a topic + a list of related notes
- `obsidian-link-suggester` — scans a note and proposes `[[wikilinks]]` to existing notes
- `obsidian-orphan-rescuer` — finds isolated notes and proposes connections
- `obsidian-broken-link-fixer` — repairs links broken by renames

## Format (when added)

Each agent is a single Markdown file: `obsidian-<agent-name>.md`.

```markdown
---
name: obsidian-<agent-name>
description: One paragraph on what this agent does and when to invoke it.
tools: [Read, Write, Edit, Glob, Grep, Bash]
---

# {Agent Name}

## Role
...

## Inputs
...

## Outputs
...

## System prompt
...

## Constraints
...
```

The `tools:` list constrains which tools the agent has access to. Agents that only modify files in the vault don't need network tools.
