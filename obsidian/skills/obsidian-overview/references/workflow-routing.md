# Workflow Routing

Common user intents mapped to the right skill or orchestrator. Use this when the user's phrasing doesn't obviously match a single skill name.

## Routing by phrase

### "I want to..."

| User says | Route to |
|---|---|
| "...clean up my vault" | `obsidian-vault-health-sweep` (orchestrator-shaped) |
| "...clean up my tags" | `obsidian-tag-cleanup-flow` (orchestrator-shaped) |
| "...set up my vault" | `obsidian-vault-bootstrap` (orchestrator-shaped) |
| "...understand my vault's state" | `obsidian-vault-audit-framework` |
| "...design my tag system" | `obsidian-tag-taxonomist` |
| "...add Properties to my notes" | `obsidian-frontmatter-schema-designer` |
| "...reorganize my folders" | `obsidian-folder-architect` |
| "...rename my notes consistently" | `obsidian-filename-conventions` |
| "...build a MOC for X" | `obsidian-moc-architect` |
| "...migrate topic tags to MOCs" | `obsidian-moc-architect` + `obsidian-tag-hygiene` |
| "...migrate status tags to a property" | `obsidian-frontmatter-schema-designer` + `obsidian-tag-hygiene` |

### "Should I..." / "Help me decide..."

| User says | Route to |
|---|---|
| "...use a tag or a property?" | `obsidian-property-vs-tag-vs-link` |
| "...use a folder or a MOC?" | `obsidian-property-vs-tag-vs-link` (then `obsidian-moc-architect` if MOC) |
| "...use PARA or type-based folders?" | `obsidian-folder-architect` |
| "...use Title Case or kebab-case?" | `obsidian-filename-conventions` |
| "...promote this tag to a MOC?" | `obsidian-moc-architect` |
| "...promote this tag to a property?" | `obsidian-property-vs-tag-vs-link` (then `obsidian-frontmatter-schema-designer`) |
| "...keep this stale note?" | `obsidian-vault-audit-framework` (or specialty if it's about specific notes) |

### "My vault has..."

| User says | Route to |
|---|---|
| "...too many tags" | `obsidian-tag-hygiene` |
| "...inconsistent frontmatter" | `obsidian-frontmatter-schema-designer` |
| "...folders 4 levels deep" | `obsidian-folder-architect` |
| "...lots of orphans" | `obsidian-vault-audit-framework` (then specialty fixes) |
| "...broken links" | `obsidian-vault-audit-framework` |
| "...notes I never look at" | `obsidian-vault-audit-framework` (stale-note check) |
| "...no system" | `obsidian-vault-bootstrap` (orchestrator-shaped) |
| "...a sprawling MOC" | `obsidian-moc-architect` (split workflow) |

### "What should I..."

| User says | Route to |
|---|---|
| "...name this note?" | `obsidian-filename-conventions` |
| "...tag this note with?" | `obsidian-tag-taxonomist` (after taxonomy is established) |
| "...put in frontmatter?" | `obsidian-frontmatter-schema-designer` |
| "...do with stale notes?" | `obsidian-vault-audit-framework` |

## Routing by symptom

### Tag-related symptoms

| Symptom | Likely cause | Route |
|---|---|---|
| Tag pane has 200+ tags | Ad-hoc tagging without taxonomy | `obsidian-tag-taxonomist` first, then `obsidian-tag-hygiene` |
| Many tags used 1–2 times | Same as above | Same |
| `#Tag` and `#tag` both exist | Casing inconsistency | `obsidian-tag-hygiene` |
| `#book` and `#books` both exist | Plural inconsistency | `obsidian-tag-hygiene` |
| `#topic/pkm/zettelkasten` (3+ levels) | Topic taxonomy in tags | `obsidian-moc-architect` |
| `#draft` and `status: draft` both exist | Mismatch | `obsidian-property-vs-tag-vs-link` then `obsidian-tag-hygiene` |

### Frontmatter-related symptoms

| Symptom | Likely cause | Route |
|---|---|---|
| Different keys for same concept (`created`, `Created`, `date-created`) | Schema drift | `obsidian-frontmatter-schema-designer` |
| Same key with different value types | Schema drift | Same |
| Frontmatter inconsistent across notes of same type | No schema enforcement | Same |
| Long prose in `summary:` property | Wrong layer | `obsidian-property-vs-tag-vs-link` |

### Folder-related symptoms

| Symptom | Likely cause | Route |
|---|---|---|
| Folders 3+ levels deep | Topic taxonomy in folders | `obsidian-folder-architect` then `obsidian-moc-architect` |
| Many folders with < 5 notes | Folders don't earn their existence | `obsidian-folder-architect` |
| Notes moving between folders to express status | Status as folder | `obsidian-property-vs-tag-vs-link` |
| `Misc/` or `Uncategorized/` growing without bound | Catch-all problem | `obsidian-folder-architect` |

### Filename-related symptoms

| Symptom | Likely cause | Route |
|---|---|---|
| Atomic notes with topic-as-title | Underdeveloped claim discipline | `obsidian-filename-conventions` |
| Multiple notes with similar names | Collisions | `obsidian-filename-conventions` (aliases or disambiguation) |
| Date-prefixed filenames not in daily folder | Time-stamping in name | `obsidian-filename-conventions` |

### Connection-related symptoms

| Symptom | Likely cause | Route |
|---|---|---|
| Many orphan notes | Insufficient linking discipline | `obsidian-vault-audit-framework` to identify, then specialty for each |
| Single MOC has 80+ links | Sprawl | `obsidian-moc-architect` (split workflow) |
| No central home/index | Missing home note | `obsidian-moc-architect` (home note design) |
| MOCs that are just lists | Missing commentary | `obsidian-moc-architect` |

## Routing for multi-skill goals

When the user's goal genuinely spans multiple skills and isn't covered by an existing orchestrator:

### "I want to migrate from PARA to type-based folders"

Sequence:
1. `obsidian-folder-architect` for the design + migration plan
2. `obsidian-frontmatter-schema-designer` if `type:` property doesn't exist yet
3. `obsidian-moc-architect` for replacement MOCs of former topic folders
4. `obsidian-vault-audit-framework` for verification baseline before/after

### "I want to introduce a `status:` property and stop using `#draft`/`#evergreen` tags"

Sequence:
1. `obsidian-frontmatter-schema-designer` to define the property
2. `obsidian-tag-hygiene` Playbook 4 for the migration
3. Update templates so new notes inherit the property

### "I want to start using MOCs across my whole vault"

Sequence:
1. `obsidian-vault-audit-framework` to find current topic clusters (orphans, tag-defined, folder-defined)
2. `obsidian-moc-architect` for each topic that crosses the 5-note threshold
3. `obsidian-folder-architect` if topic folders need to flatten as MOCs replace them
4. `obsidian-tag-hygiene` if topic tags need retiring as MOCs replace them

These multi-skill goals are candidates for promoting to new orchestrator-shaped skills in future versions.

## When the user's request doesn't match anything

Sometimes the request is out of scope for this plugin (e.g., "help me write a blog post from my notes" — that's an output workflow, not in v0.1.0).

In those cases:
- Be direct: "this plugin is focused on organization, tagging, and structure. For [output / capture / writing], I'd reach for [different skill or external workflow]."
- Don't force a fit. The skills lose value when they're applied to problems they weren't designed for.

## Pattern recognition tips

The user's exact words usually under-specify. A few patterns that help:

- **"My vault is messy"** → almost always wants `obsidian-vault-health-sweep` orchestrator. Confirm scope before invoking.
- **"I want to be more organized"** → ambiguous. Ask: design (start) or cleanup (mid-stream)?
- **"How do other people organize Obsidian?"** → educational; load `obsidian-overview` and explain the four-mechanism model from `obsidian-foundations.md`.
- **"What's a MOC?"** → educational; explain from `obsidian-foundations.md` and `obsidian-moc-architect`. Don't immediately try to build one.
- **"I'm coming from Notion / Roam / Logseq"** → cross-system; explain the Obsidian opinions before designing. Don't import their mental model.
