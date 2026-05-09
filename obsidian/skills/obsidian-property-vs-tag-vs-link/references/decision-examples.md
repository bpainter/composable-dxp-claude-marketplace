# Decision Examples

Worked examples for routing common metadata candidates to the right mechanism. Use this as a lookup when a candidate isn't obvious.

## Format

For each candidate:

- **Candidate** — the field being considered
- **Decision** — property / tag / link / folder
- **Implementation** — the concrete shape
- **Reasoning** — why this layer
- **Common mistake** — the wrong-layer version

---

## Note state and lifecycle

### Status (draft, evergreen, stale...)

- **Decision:** property
- **Implementation:** `status: text` with constrained values `[draft, refined, evergreen, stale, archive]`
- **Reasoning:** Constrained value set. Queryable. Not cross-cutting in a way that needs tag visibility. Type discipline matters.
- **Common mistake:** `#draft`, `#evergreen` as tags. Works, but drifts (`#wip` shows up); query is weaker.

### Processing state (inbox, to-process, to-link)

- **Decision:** tag (or property if you're already strict on properties)
- **Implementation:** `#inbox`, `#to-process`, `#to-link`
- **Reasoning:** Lifecycle/processing state crosses note types and the user wants Tag Pane visibility for "what's still in my inbox?"
- **Common mistake:** Inbox as a folder. Works, but means moving notes physically — adds friction to processing.

### Note type (atomic, MOC, literature, daily...)

- **Decision:** property
- **Implementation:** `type: text` with constrained values
- **Reasoning:** Structural attribute. Drives template selection, schema choice, query patterns. Property gives type discipline.
- **Common mistake:** `#moc`, `#book` as tags. Common transitional state. Promote to property when you commit.

---

## Topic / subject

### Topic with 5+ notes (`pkm`, `productivity`, `parenting`)

- **Decision:** MOC + links
- **Implementation:** `[[PKM MOC]]` note; each related note links to it.
- **Reasoning:** Topics earn MOCs once they have 5+ notes. MOC carries structure and commentary; tag carries nothing.
- **Common mistake:** `#pkm`, `#productivity` as tags. Initial OK. Becomes a sprawl. Promote to MOC.

### Topic with 1–4 notes (still emerging)

- **Decision:** lightweight tag (with a "promote at N" rule), or just inline links between the few notes
- **Implementation:** `#topic-emerging` or just `[[Note A]]`/`[[Note B]]` linking each other
- **Reasoning:** Not enough mass to justify a MOC; not zero — needs to be findable.
- **Common mistake:** Creating a MOC too early — empty MOCs are worse than no MOC.

### Top-level life areas (Health, Career, Family)

- **Decision:** depends on user's organizing style
  - PARA-style folder structure: folder
  - LYT-style: top-level MOCs
  - Hybrid: both, with the folder providing physical home and the MOC providing topical map
- **Implementation:** `Areas/Health/` folder *and* `[[Health MOC]]`
- **Reasoning:** Areas are stable categories that benefit from physical separation *and* curated indexes.
- **Common mistake:** Deep folder nesting *under* the area, replicating a topic taxonomy. Use MOCs.

---

## Identity (people, projects, places, organizations)

### Person you reference 3+ times

- **Decision:** link to a person note
- **Implementation:** `[[Jane Doe]]` person note with `type: person`
- **Reasoning:** People deserve nodes. Backlinks become their history.
- **Common mistake:** `#jane-doe` tag. Tag doesn't carry context, doesn't have backlinks.

### Project (named effort with start and end)

- **Decision:** link to a project note
- **Implementation:** `[[Website Redesign]]` with `type: project`, `project-status:`, `start-date:`, `end-date:`
- **Reasoning:** Projects have rich metadata, status, deliverables. Worth a node.
- **Common mistake:** `#project/website-redesign` tag. Strips all the structure projects need.

### Organization / company

- **Decision:** link if you'll reference it 3+ times; tag never
- **Implementation:** `[[Acme Corp]]` with `type: organization` (if vault has it) or `type: reference`
- **Reasoning:** Same as person.
- **Common mistake:** `#acme` tag. Use the link.

### Place / location

- **Decision:** link if 3+ references; otherwise inline mention
- **Implementation:** `[[Kyoto]]` if you write about Kyoto often
- **Reasoning:** Places that recur deserve nodes. Places mentioned once don't.

---

## Time / dates

### Created / modified

- **Decision:** property
- **Implementation:** `created: date`, `modified: date`
- **Reasoning:** Sortable, filterable. Dates need date type.
- **Common mistake:** `#2025` tag. Date math impossible.

### Due / deadline

- **Decision:** property
- **Implementation:** `due: date` or `target-end-date: date`
- **Reasoning:** Date type, comparison-friendly.
- **Common mistake:** Putting it in body text only. Can't query.

### Year / decade for grouping

- **Decision:** properties (`created`/`year-of`) + Dataview groupings
- **Reasoning:** Don't tag the year. Dataview can group by year from a date field.

---

## Numeric attributes

### Rating

- **Decision:** property
- **Implementation:** `rating: number` (1–5 typically)
- **Reasoning:** Comparable, sortable, summable.
- **Common mistake:** `rating: "4 stars"` as text. Ruined for query.

### Reading time / word count

- **Decision:** property
- **Implementation:** `reading-minutes: number`, `word-count: number`
- **Reasoning:** Numeric, sortable.

### Priority

- **Decision:** property with constrained list
- **Implementation:** `priority: text` with values `[low, medium, high]`
- **Reasoning:** Constrained set. Text is fine since you'll query for equality, not comparison.
- **Common mistake:** `priority: number` (1, 2, 3) when you want named tiers, or text when you want comparison.

---

## Source / provenance

### Source type (book, article, podcast)

- **Decision:** property (constrained list)
- **Implementation:** `source-kind: text` with values `[book, article, podcast, video, paper, other]`
- **Reasoning:** Type matters for queries; constrained values prevent drift.
- **Common mistake:** `#book`, `#article` tags. Common transitional state. Promote.

### Author

- **Decision:** property of type list (multi-author)
- **Implementation:** `source-author: list` of names or `[[Author Name]]` links
- **Reasoning:** Multi-value, often becomes a person link.
- **Common mistake:** `#author/jane-doe`. Use the link.

### URL / source link

- **Decision:** property
- **Implementation:** `source-url: text`
- **Reasoning:** Structured field; renders as clickable in property view.

---

## Cross-cutting facets

### "Needs citation"

- **Decision:** tag
- **Implementation:** `#needs-citation`
- **Reasoning:** Cross-cutting; transient (resolves when citation added); benefits from Tag Pane visibility.
- **Common mistake:** Property `needs-citation: true`. Works but loses Tag Pane visibility.

### "Has image"

- **Decision:** tag
- **Implementation:** `#has-image`
- **Reasoning:** Same as above. Cross-cutting facet.

### "Confidential"

- **Decision:** tag *and* property
- **Implementation:** `#confidential` for visibility + `confidential: true` checkbox
- **Reasoning:** Sensitive enough that belt-and-suspenders is fine.

---

## Relationships between notes

### "Related to"

- **Decision:** link, optionally as a property of type list
- **Implementation:** `[[Other Note]]` inline, or `related: ["[[Other Note]]"]`
- **Reasoning:** Specific relationships are graph-shaped. Inline links are usually sufficient.
- **Common mistake:** Tag the topic instead of linking the note.

### Parent / child

- **Decision:** property of type link
- **Implementation:** `parent-moc: "[[Parent MOC]]"`
- **Reasoning:** Parent-child is structural. A property carries the meaning more clearly than just a body link.

### "See also"

- **Decision:** body links, not property
- **Implementation:** Inline `[[Note A]]`, `[[Note B]]` in a "See also" section
- **Reasoning:** Loose suggestion; no need for structured field.

---

## Storage / location

### Where the note physically lives

- **Decision:** folder
- **Implementation:** `Inbox/`, `Atomics/`, `Sources/`, `Periodic/`, etc.
- **Reasoning:** Folders are filesystem-native; mutually exclusive; one source of truth for "where is this."
- **Common mistake:** Encoding location in tags or properties. Redundant; the folder already says it.

### "Should appear in this MOC"

- **Decision:** link from the note to the MOC
- **Implementation:** `[[Topic MOC]]` linked in the body or as a property
- **Reasoning:** MOC inclusion is a graph relationship.

---

## Quick lookup table

| Candidate                    | Mechanism      | Form                                         |
| ---------------------------- | -------------- | -------------------------------------------- |
| status                       | property       | `status: text` (constrained list)            |
| processing state             | tag            | `#inbox`, `#to-process`                      |
| note type                    | property       | `type: text`                                 |
| topic (5+ notes)             | MOC + links    | `[[Topic MOC]]`                              |
| topic (1–4 notes)            | links or tag   | inline `[[]]` or `#topic-emerging`           |
| life area                    | folder + MOC   | `Areas/Health/` + `[[Health MOC]]`           |
| person                       | link           | `[[Person Name]]`                            |
| project                      | link           | `[[Project Name]]`                           |
| organization                 | link           | `[[Org Name]]`                               |
| place                        | link (if recurring) | `[[Place]]`                             |
| created/modified             | property       | `created: date`                              |
| due                          | property       | `due: date`                                  |
| rating                       | property       | `rating: number`                             |
| priority                     | property       | `priority: text` (low/medium/high)           |
| source type                  | property       | `source-kind: text`                          |
| author                       | property/link  | `source-author: list` or `[[Author]]`        |
| source URL                   | property       | `source-url: text`                           |
| needs citation               | tag            | `#needs-citation`                            |
| has image                    | tag            | `#has-image`                                 |
| confidential                 | tag + property | `#confidential` + `confidential: true`       |
| related to                   | link           | inline `[[]]` or `related: list`             |
| parent (e.g., parent MOC)    | property       | `parent-moc: link`                           |
| where it lives               | folder         | filesystem path                              |
