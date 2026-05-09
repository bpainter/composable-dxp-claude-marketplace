# Common Mismatches

The most frequent "wrong layer" mistakes in Obsidian vaults, with detection signals and migration paths.

## Mismatch 1 — Status as a tag

### Signal

`#draft`, `#evergreen`, `#stale`, `#wip`, `#in-progress` exist as tags. Often together. Often with casing or synonym variants.

### Why it's wrong

- Constrained set of values that drift without enforcement (`#draft`, `#Draft`, `#wip`, `#in-progress`)
- Can't query "all evergreen notes created this year" cleanly with tags alone
- Multiple status tags can coexist on one note silently
- Tag Pane fills with status noise

### Migration

1. Add `status:` property to the schema (`obsidian-frontmatter-schema-designer`)
2. Define allowed values
3. Update default template
4. Migrate notes wave by wave (`obsidian-tag-hygiene` Playbook 4)
5. Retire status tags

### When it's actually OK

- The user genuinely won't maintain property discipline. A drift-prone tag is better than a property left blank.
- Status is also a body-text concern (e.g., the user wants `#draft` visible while reading). Then *both* — property for query, tag for visibility.

---

## Mismatch 2 — Topic as a tag

### Signal

`#pkm`, `#productivity`, `#parenting`, `#zettelkasten` — domain topics being used as tags, often with 5+ notes each.

### Why it's wrong

- Tags carry no context. The Tag Pane gives counts but no curation.
- Topics benefit from commentary, structure, and opinionated organization — none of which fits in a tag.
- Topic tags grow without bound.

### Migration

1. For each topic with 5+ notes, create a MOC (`obsidian-moc-architect`)
2. Add `[[Topic MOC]]` link in each tagged note (typically near the top or in "See also")
3. Verify the MOC's backlinks count covers the original tag count
4. Retire the tag

### When it's actually OK

- Topic with fewer than 5 notes that you don't want to invest in a MOC for yet. Lightweight tag is fine — but with a "promote at N" rule.
- Cross-cutting facets that aren't really topics (`#has-image`) — those legitimately stay as tags.

---

## Mismatch 3 — Topic as a folder

### Signal

`Topics/PKM/Zettelkasten/`, `Reference/Productivity/Methods/`, etc. — deep folder trees that mirror a topic taxonomy.

### Why it's wrong

- Folders force a single classification. A note about "Zettelkasten productivity techniques" has to pick one home.
- Renames become painful. Splits become painful.
- The folder hierarchy fights the graph instead of complementing it.

### Migration

1. Flatten the folder structure (`obsidian-folder-architect` for the design)
2. Create MOCs for the former folder topics (`obsidian-moc-architect`)
3. Move notes to the appropriate flat folder (often `Atomics/`, `Sources/`, etc.)
4. Each former folder-resident note gets a link to the MOC

### When it's actually OK

- Top-level life areas (PARA-style `Areas/Health/`) where each area is stable and mutually exclusive
- Note-type-based folders (`Atomics/`, `Sources/`, `Daily/`) — these are *type*, not *topic*

---

## Mismatch 4 — Project as a tag

### Signal

`#website-redesign`, `#book-launch`, `#q4-okrs` — named projects existing only as tags.

### Why it's wrong

- Projects have status, dates, deliverables, owners — all of which need a node, not a tag
- No place to write about the project itself
- Tag doesn't show up in graph view as a navigation point

### Migration

1. Create `[[Project Name]]` with `type: project` schema
2. Replace `#project-name` tag with `[[Project Name]]` link in each related note
3. Retire the tag

### When it's actually OK

- Very short-lived ad-hoc efforts that don't deserve a node. But these are usually meeting notes anyway.

---

## Mismatch 5 — Person as a tag

### Signal

`#jane-doe`, `#bob-smith` — people as tags.

### Why it's wrong

- Loses backlinks (the implicit "history of mentions")
- No place for context about the person
- Names duplicate as different tags via casing/spelling

### Migration

1. Create `[[Jane Doe]]` person note
2. Replace `#jane-doe` with `[[Jane Doe]]` link inline where the person is mentioned
3. Retire tag

### When it's actually OK

- One-off mentions where the person isn't worth a node. Don't create either a tag or a note — just write the name.

---

## Mismatch 6 — Note type as a tag

### Signal

`#moc`, `#person`, `#book`, `#daily` — note types as tags, often with `type:` property absent.

### Why it's wrong

- Tag is unstructured; can't query "all evergreen MOCs" cleanly
- Multiple type-tags can coexist (smell)
- Templates can't enforce a tag-based type as cleanly as a property

### Migration

1. Add `type:` property to the schema with constrained values
2. Update templates per note type
3. Migrate `#moc` → `type: moc`, etc.
4. Retire the type tags

### When it's actually OK

- Very early in vault development before properties are formalized. Transitional state, not a destination.

---

## Mismatch 7 — Date as a tag

### Signal

`#2025`, `#january-2026`, `#q4-2025` — date or period tags.

### Why it's wrong

- Dates need date type for sort, range, and math
- Tag explosion: every month becomes a new tag

### Migration

1. Add `created: date` property if not present
2. Use Dataview groupings for time-based queries (`GROUP BY dateformat(created, "yyyy-MM")`)
3. Retire date tags

### When it's actually OK

- Era markers that aren't really dates — `#early-vault` to mark notes from the chaotic period before discipline. These are facets, not dates.

---

## Mismatch 8 — Sentiment / quality as a tag

### Signal

`#favorite`, `#important`, `#meh` — qualitative judgments as tags.

### Why it's wrong

- Subjective; gets reapplied inconsistently
- "Important" rarely answers a real query — what would you do with it?

### Migration

- Replace with a `rating: number` property if it's truly a rank
- Or with a tag like `#starred` if it's a binary "I want to find this again" — but verify it answers a real query
- Most often, just retire — the desire to mark "favorites" rarely produces useful retrieval

### When it's actually OK

- A binary "starred" or "favorite" facet that the user *will* query. Then `#starred` is fine.

---

## Mismatch 9 — Many checkboxes as separate tags

### Signal

`#reviewed`, `#published`, `#shared`, `#imported` — a stack of tags, each binary.

### Why it's wrong

- Each is a checkbox property in disguise
- Tags can't be queried in combination as easily as properties

### Migration

1. Add checkbox properties: `reviewed: true`, `published: true`, etc.
2. Migrate per-tag, retire the tag

### When it's actually OK

- One or two binary facets that you genuinely want in the Tag Pane for visibility. Then keep as tag.

---

## Mismatch 10 — Body data that should be a property

### Signal

A note's body has a header line like:

```
**Author:** Jane Doe
**Year:** 2024
**Source:** [link]
```

### Why it's wrong

- Not queryable by Dataview / Bases
- Inconsistent across notes (different keys, different formatting)
- Editing requires opening the note

### Migration

1. Add the fields to the relevant note-type schema
2. Move values from body to frontmatter
3. Optionally remove the body header (or keep it for visual reading — duplication is OK if conscious)

---

## Mismatch 11 — Frontmatter that should be body

### Signal

Long prose in `summary:`, `notes:`, `description:` properties.

### Why it's wrong

- Frontmatter is for structured metadata, not narrative
- Long property values render awkwardly
- Search and link weighting prefer body content

### Migration

- Move prose to the body
- Keep a `tagline:` or `summary:` property only if very short (1 sentence)

---

## Mismatch 12 — Folder that should be a tag or property

### Signal

Notes get moved between folders to express status (`Active/`, `Archived/`, `Cold/`).

### Why it's wrong

- Folders are physical homes; status is an attribute
- Moving files just to change status is friction
- Status changes should be a property edit, not a filesystem operation

### Migration

1. Add `status:` property
2. Move all the notes from the status folders back to a single typed folder (`Atomics/`, `Sources/`, etc.)
3. Use Dataview for "active notes" views

### When it's actually OK

- Strict GTD-style "next-action" folder for visual review of an inbox. Even then, a tag or property usually serves better.

---

## Diagnosis flow when a vault has many mismatches

1. **Inventory the symptoms.** Which mismatches show up?
2. **Sequence the fixes.** Status mismatches first (high signal, low friction). Then topic-as-tag (highest information loss). Then folder structure last (most disruptive).
3. **Don't fix all at once.** One mismatch at a time. Verify before moving on.
4. **Update documentation.** Schema note, taxonomy note, and home/MOC structure all evolve with the migration.
