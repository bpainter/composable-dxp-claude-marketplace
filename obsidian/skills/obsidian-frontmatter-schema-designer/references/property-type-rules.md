# Property Type Rules

Reference for choosing property types correctly in Obsidian frontmatter.

## The six types Obsidian supports

| Type      | Frontmatter syntax            | When to use                                                 |
| --------- | ----------------------------- | ----------------------------------------------------------- |
| text      | `key: value`                  | Short freeform strings; unique IDs; URLs                    |
| list      | `key: [a, b]` or block list   | Multi-value: tags, aliases, related notes, authors          |
| date      | `key: 2026-05-07`             | Calendar dates                                              |
| datetime  | `key: 2026-05-07T14:30:00`    | Time-of-day matters                                         |
| number    | `key: 4.5`                    | Ratings, counts, weights, anything you'll compare or sum    |
| checkbox  | `key: true` / `key: false`    | Binary state                                                |

## Type-choice decision rules

### Rule 1 — If you'll query for equality with a small set of values, use a constrained text or list

`status` should be `text` with a documented allowed list, *not* free-form text. Same for `priority`, `type`, `category`.

Document the allowed values in the schema note. Without that, drift is inevitable.

### Rule 2 — If you'll query with comparison operators, use number or date

Use `rating: 4` (number), not `rating: "4 stars"` (text). Use `created: 2026-05-07` (date), not `created: "May 7, 2026"` (text).

Comparison-friendly types let Dataview and Bases do real filtering.

### Rule 3 — If the value is or might become multi-value, use list

Even if you only have one author today, `authors: [Steph Ango]` (list) is more durable than `author: Steph Ango` (text). Lists handle one element fine; texts can't grow gracefully.

Common list properties: `tags`, `aliases`, `authors`, `related`, `topics`, `participants`, `attachments`.

### Rule 4 — If the value is a relationship to another note, use list of links

```yaml
related:
  - "[[Linking strategy]]"
  - "[[MOC architecture]]"
```

Note the quotes: `[[...]]` inside YAML strings should be quoted to avoid YAML parsing surprises.

These count toward the graph as outgoing links, exactly like inline `[[wikilinks]]` would.

### Rule 5 — Never overload a property's meaning

If a property name has different meanings on different note types, split it. `target:` on a project means due date; `target:` on a meeting means audience. These should be `target-date:` and `target-audience:`, or live on different schemas.

### Rule 6 — Reserved keys do specific jobs — don't redefine

- `tags:` (list) — note tags
- `aliases:` (list) — alternative titles for linking and search
- `cssclasses:` (list) — CSS classes applied to the note view

Don't repurpose any of these.

## Naming conventions

- **Lowercase** — `created`, not `Created`. Keys are case-sensitive in YAML and Dataview.
- **Hyphenated for multi-word** — `source-author`, not `sourceAuthor` or `source_author`. Reads as natural language; matches the rest of Obsidian's idioms.
- **Singular when the value is single, plural when the value is a list** — `author` (text) vs. `authors` (list). For genuinely list-valued properties, the plural reads correctly.
- **No verbs** — `is-published` is a smell; use `published: true` (checkbox) or `status: published`.
- **Consistent prefixes for related fields** — `source-author`, `source-title`, `source-year`, `source-url`. Helps Dataview projection and visual scan.

## Bad type choices and their fixes

| Bad                                  | Why                                                | Fix                                            |
| ------------------------------------ | -------------------------------------------------- | ---------------------------------------------- |
| `status: text` (free-form values)    | Drift: "Draft", "draft", "In Progress", "wip"      | Constrained list, documented in schema note   |
| `tags: text` (single string)         | Breaks Obsidian's tag handling                     | `tags: list`, no `#` in values                 |
| `created: text` (e.g., "May 7")      | Not sortable, not date-filterable                  | `created: date`, ISO format                    |
| `rating: text` (e.g., "4 stars")     | Can't compare or sum                               | `rating: number`                               |
| `done: text` (e.g., "yes"/"no")      | Inconsistent values                                | `done: checkbox`                               |
| `topic: text` (e.g., "PKM")          | Should be a link or a tag                          | `topics: list` of links, or `[[PKM MOC]]` link |

## Required vs. optional

Make the universal properties required (every note has them via template), and per-type properties required only if the note is *defined* by that property.

### Universal required (recommendation)

```yaml
type: <one of allowed values>
created: <date>
```

Two properties. Every note. No exceptions.

### Common optional universal

```yaml
status: <constrained value> | <unset>
modified: <date> | <unset>
tags: [list] | []
```

### Per-type required (examples)

- **literature** — `source-title`, `source-author` (yes, even if guessed)
- **project** — `project-status` (different from note status), `start-date`
- **person** — none required beyond universal; even names are in the title
- **moc** — `topic` (text or link to a parent MOC)
- **daily** — `date` (= note date; redundant with title but useful for query)

## Constrained value lists — how to document

In the schema note, list values explicitly:

```yaml
status: [draft, refined, evergreen, stale, archive]
type: [atomic, moc, literature, daily, project, person, reference]
priority: [low, medium, high]
project-status: [planning, active, blocked, complete, abandoned]
rating: [1, 2, 3, 4, 5]            # numbers, but values are constrained
```

When a new value seems to be needed, that's a schema decision, not a per-note decision. Add it to the schema note first.

## Multi-line and block scalars

For longer text properties, YAML supports block scalars:

```yaml
summary: |
  Multi-line summary that
  preserves line breaks.
```

Avoid these in Obsidian unless the property genuinely needs multi-line content. The note body is the right place for prose.

## Quoting

Quote values when:

- The value contains a `:` or other YAML control character: `title: "Note: with colon"`
- The value contains `[[...]]`: `related: ["[[Some Note]]"]`
- The value would be misparsed (`yes`, `no`, `true`, `false` if you mean strings)

Don't over-quote. Plain values are easier to read.

## Properties that are almost always wrong as text

- Anything date-shaped → use `date`
- Anything number-shaped → use `number`
- Anything yes/no → use `checkbox`
- Status, type, category from a known set → still text, but with a constrained list documented

## Sample frontmatter schema documentation

Excerpt from a `Frontmatter Schema.md` note:

```markdown
## Universal properties (every note)

| Key      | Type    | Required | Allowed values             | Example         |
|----------|---------|----------|-----------------------------|-----------------|
| type     | text    | yes      | atomic, moc, literature,...| `type: atomic`  |
| created  | date    | yes      | YYYY-MM-DD                  | `2026-05-07`    |
| status   | text    | no       | draft, refined, evergreen,..| `status: draft` |
| tags     | list    | no       | (per tag taxonomy)          | `[#pkm, #draft]`|

## Type-specific: literature

| Key            | Type   | Required | Notes                          |
|----------------|--------|----------|--------------------------------|
| source-title   | text   | yes      | Title of the source            |
| source-author  | list   | yes      | One or more authors            |
| source-year    | number | no       | Publication year               |
| source-url     | text   | no       | URL if online                  |
| rating         | number | no       | 1–5                            |
| read-date      | date   | no       |                                |
```

This note is the source of truth. Drift starts when the note isn't updated; updating it should always precede schema changes.
