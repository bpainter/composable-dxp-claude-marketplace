# Tag Taxonomy Design

Reference framework for designing a tag system in an Obsidian vault. Use alongside SKILL.md.

## The five legitimate purposes for tags

A tag earns its place by serving one of these purposes. If a candidate tag doesn't fit, it probably belongs as a property, link, or MOC.

### 1. Status

Lifecycle state of the note itself.

Examples: `#draft`, `#evergreen`, `#archive`, `#stale`, `#review`

Caveat: status is *almost always* better as a `status:` property. Use a tag only if the user refuses property discipline or if the same note legitimately holds multiple statuses simultaneously.

### 2. Note type (when no property exists)

What kind of note this is, when the vault doesn't yet enforce a `type:` property.

Examples: `#moc`, `#person`, `#book`, `#fleeting`

Migration target: convert these to a `type:` property as soon as the user is ready. Tags-as-types is a common transitional state, not a destination.

### 3. Lifecycle / processing state

Where a note is in your workflow, distinct from status.

Examples: `#inbox`, `#to-process`, `#to-link`, `#to-archive`, `#dropped`

These are excellent tag use cases — they're transient, they cross note types, and they answer real queries ("what's still in my inbox?").

### 4. Cross-cutting facets

Attributes that span many note types and don't deserve a typed property slot.

Examples: `#has-image`, `#needs-citation`, `#confidential`, `#published`, `#source-paywalled`

Test: would this go on notes of *very different types*? If yes, it's a facet. If only on one note type, it's probably a property of that type.

### 5. Lightweight topic markers (carefully)

Topic markers used as a temporary holding pattern before a MOC exists.

Examples: `#quick-take`, `#anecdote`, `#parenting` (early in a vault)

Caveat: this is the most dangerous purpose. Topic tags grow without bound and almost always graduate to MOCs. Allow these only with an explicit "promote to MOC at N notes" rule (typical N: 5–10).

## What tags are bad at

- **Deep topic taxonomies.** `#topic/pkm/zettelkasten/permanent-notes` is a folder tree in disguise. Use links and MOCs.
- **Person and project identity.** Use `[[Person Name]]` and `[[Project Name]]` notes — they carry context, backlinks, and properties.
- **Time.** Use `created:`/`modified:` properties or daily notes. `#2025` is rarely useful.
- **Anything you'd want to query with a comparison.** Tags don't have operators. If you want "rating > 3," use a property.
- **Fine-grained categorization within a single note type.** Use properties on that note type instead.

## Hierarchy depth heuristic

| Levels | When it's appropriate                                                                                      | Examples                                |
| ------ | ---------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 0 (flat) | Default. Always start here.                                                                              | `#draft`, `#moc`, `#inbox`              |
| 1      | When a single facet has 3+ values that benefit from grouping (e.g., status, processing state).            | `#status/draft`, `#status/evergreen`    |
| 2      | Almost never. Reach for properties or MOCs first.                                                          | `#status/draft/needs-review` (smell)    |
| 3+     | Wrong tool. Stop.                                                                                          | `#topic/pkm/zk/permanent`               |

## Decision tree: should this be a tag?

```
Is it a typed attribute (date, status, rating, source)?
  YES → property
  NO  → continue

Is it a relationship to a specific other note or concept?
  YES → [[wikilink]] or property of type "list"
  NO  → continue

Is it a topic that already has 3+ notes about it?
  YES → MOC + links
  NO  → continue

Will it cross multiple note types as a facet?
  YES → tag (facet)
  NO  → continue

Is it a lifecycle/processing state?
  YES → tag
  NO  → it's probably not earning its place; reconsider
```

## v1 starter tag lists by vault style

### Personal knowledge / writing vault

```
#inbox
#to-process
#evergreen
#draft
#stale
#needs-citation
#has-quote
#anecdote
```

8 tags. Status as property is recommended; if not, add `#status/draft`, `#status/evergreen`, `#status/stale` and drop the bare `#draft`/`#evergreen`/`#stale`.

### Research vault

```
#inbox
#to-read
#read
#to-cite
#methodology
#dataset
#confidential
```

7 tags. Most "topic" classification belongs in MOCs and links. Bibliographic data goes in properties.

### Generalist work + life vault

```
#inbox
#to-process
#waiting
#someday
#reference
#evergreen
#archive
```

7 tags. GTD-style processing states map well to tags here.

## Entry criteria checklist

A tag earns creation only if **all** are true:

- [ ] It serves one of the five legitimate purposes above
- [ ] It will appear on at least 3 existing notes within 7 days
- [ ] It answers a query the user will actually run (search, dashboard, MOC inclusion criterion)
- [ ] It doesn't overlap with an existing tag, property, or link convention
- [ ] It follows the established naming conventions

If you can't check all five boxes, don't create the tag.

## Exit criteria — when to retire a tag

- **Used on fewer than 3 notes after 60 days** → merge into the closest neighbor or delete
- **Merged purpose with another tag** → consolidate, update notes
- **Promoted to a property** → strip from notes and migrate values
- **Promoted to a MOC** → unlink the tag, link notes to the MOC

## Documenting the taxonomy

A vault should have one note that *is* the tag taxonomy. Recommended structure:

```markdown
---
type: reference
status: evergreen
---

# Tag Taxonomy

Last reviewed: 2026-05-01

## Approved tags

### Status (use property when possible)
- `#draft` — note is in progress, not yet ready for reuse
- `#evergreen` — note is durable, refined, citable

### Processing state
- `#inbox` — unprocessed capture
- `#to-process` — triaged but not refactored
- `#to-link` — needs links to existing notes

### Facets
- `#needs-citation` — claim needs a source
- `#has-quote` — note contains a direct quote

## Conventions

- Lowercase always
- Hyphens for word separation
- Singular form preferred
- One level of hierarchy max

## Retired tags

- `#books` → merged into `#book` (2026-01-15)
- `#topic/pkm` → promoted to [[PKM MOC]] (2026-02-03)
```

This note is the source of truth. If a tag isn't here, it shouldn't be in the vault.

## Common anti-patterns and their fixes

| Anti-pattern                                              | Fix                                                                            |
| --------------------------------------------------------- | ------------------------------------------------------------------------------ |
| 200+ tags, most used once                                 | Audit; retire low-frequency tags; promote topics to MOCs                       |
| `#book` and `#books` both exist                          | Pick singular, merge, document the rule                                        |
| `#topic/pkm/zk/permanent`                                 | Replace with `[[Permanent Notes]]` linked from `[[PKM MOC]]`                   |
| `#status/active` on a project                             | Move to `status:` property on the project note                                 |
| `#person/jane-doe`                                       | Replace with `[[Jane Doe]]` person note                                        |
| `#2025` for time-stamping                                 | Use `created:` property                                                        |
| `#draft` and `status: draft` both exist                  | Pick one (property preferred); strip the other                                |
| Tags created during note-writing without taxonomy review | Capture a "tag candidates" list; review weekly, only formalize ones that earn it |
