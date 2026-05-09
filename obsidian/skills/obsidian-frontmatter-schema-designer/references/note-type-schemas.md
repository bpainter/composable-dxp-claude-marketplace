# Note Type Schemas

Starter schemas for the most common note types in an Obsidian vault. Use these as v1 — extend cautiously.

Every schema below assumes the **universal properties** apply on top:

```yaml
type: <one of allowed values>     # required
created: <date>                    # required
status: <one of: draft, refined, evergreen, stale, archive>   # optional
tags: [list]                       # optional
aliases: [list]                    # optional
```

The per-type properties extend this baseline.

## Atomic note

A single declarative idea or claim.

```yaml
type: atomic
created: 2026-05-07
status: refined
related:
  - "[[Linking strategy]]"
  - "[[Atomic notes principle]]"
```

**Notes:**
- Atomic notes typically have *fewer* properties, not more. The graph carries the structure.
- `related:` is optional — if links are inline in the body, they count toward the graph anyway.
- A `source:` link to the literature note that triggered the idea is common.

## MOC (Map of Content)

A curated index for a topic.

```yaml
type: moc
created: 2026-05-07
status: evergreen
parent-moc: "[[Home]]"
```

**Notes:**
- `parent-moc:` (single link) tracks the MOC hierarchy when you have one.
- MOCs are typically `evergreen` once they stabilize.
- The note body holds the actual content — links to other notes, organized with headings and commentary.

## Literature / Source note

Notes about an external source: book, article, podcast, video.

```yaml
type: literature
created: 2026-05-07
status: refined
source-kind: book              # book | article | podcast | video | paper | other
source-title: "The Order of Time"
source-author:
  - Carlo Rovelli
source-year: 2017
source-url: ""
rating: 4
read-date: 2026-04-22
```

**Notes:**
- `source-kind:` is a constrained list. Pick a small set up front.
- `source-author:` as a list handles multi-author sources.
- `rating:` as number (1–5) supports queries like "highly-rated books on time."
- For podcasts/videos: add `source-host:` and `source-episode:` if needed.

## Daily / Periodic note

Time-anchored capture and review.

```yaml
type: daily
created: 2026-05-07
date: 2026-05-07
```

**Notes:**
- `date:` is redundant with `created:` for daily notes, but useful for Dataview queries that filter explicitly.
- Don't over-schema daily notes — they're inboxes, not structured records.
- Weekly/monthly variants: change `type:` to `weekly`/`monthly`, replace `date:` with `week-of:` / `month-of:`.

## Project note

A bounded effort with an end state.

```yaml
type: project
created: 2026-05-07
status: refined
project-status: active        # planning | active | blocked | complete | abandoned
start-date: 2026-05-07
end-date: ""
target-end-date: 2026-08-01
client: ""
related:
  - "[[Some Project MOC]]"
```

**Notes:**
- `project-status` is *separate* from note `status:`. The note can be `evergreen` (well-maintained) while the project is `active`. Don't conflate.
- `client:` empty if not applicable; can be a link to a `[[Client Name]]` note.
- For projects with subprojects: use a project MOC, not nested project frontmatter.

## Person note

A person you reference frequently enough to deserve a node.

```yaml
type: person
created: 2026-05-07
status: refined
relationship: colleague       # colleague | friend | family | mentor | other
org: "[[Company Name]]"
role: "Engineering Manager"
last-contact: 2026-05-01
```

**Notes:**
- `org:` as link if the company has its own note; as text otherwise.
- `last-contact:` is optional but useful for re-engagement queries.
- Avoid sensitive personal data — frontmatter ends up in backups, exports, sharing scenarios.

## Reference / Resource note

Durable lookup material: cheat sheets, glossaries, checklists.

```yaml
type: reference
created: 2026-05-07
status: evergreen
domain: "Obsidian"
```

**Notes:**
- Reference notes are usually `evergreen` quickly — they're meant to last.
- `domain:` (or `category:`) helps when you have many. Constrained value list.

## Meeting note

Captured during or after a meeting.

```yaml
type: meeting
created: 2026-05-07
status: draft
meeting-date: 2026-05-07
participants:
  - "[[Jane Doe]]"
  - "[[Bob Smith]]"
project: "[[Some Project]]"
decisions:
  - "Decision text"
action-items:
  - "[ ] Follow up with Jane on X"
```

**Notes:**
- `participants:` as list of links picks up backlinks on each person's note — instant meeting history.
- `decisions:` and `action-items:` as list-of-strings work, but inline in the body is often cleaner.
- For recurring meetings, consider a `meeting-series:` link to a parent.

## Inbox / Fleeting note

Unprocessed capture awaiting triage.

```yaml
type: fleeting
created: 2026-05-07
status: draft
```

**Notes:**
- Minimal schema by design. The whole point is friction-free capture.
- Once processed, the note's `type:` changes (atomic, literature, etc.).

## Task / Decision / Question (optional)

Some vaults track these as their own types. Schemas:

### Decision

```yaml
type: decision
created: 2026-05-07
status: refined
decision-status: pending      # pending | made | reversed
decided-on: ""
revisit-on: 2026-08-01
```

### Question (open question / research lead)

```yaml
type: question
created: 2026-05-07
status: draft
question-status: open         # open | answered | dropped
related:
  - "[[Topic MOC]]"
```

## How to choose your starter set of types

Don't define all 10+ types up front. Start with what you're actually creating:

**Minimal personal vault (3 types):**
- atomic, daily, reference

**Writer / researcher (5 types):**
- atomic, literature, moc, daily, reference

**Generalist work + life (7 types):**
- atomic, literature, moc, daily, project, person, reference

**Knowledge worker / consultant (8–10 types):**
- atomic, literature, moc, daily, project, person, reference, meeting, decision

Add types only when you have at least 3 notes that genuinely fit and the existing types don't.

## Schema growth — how to add a property

When you realize you want a new property:

1. **Pause.** Verify it's not better as a tag or link.
2. **Pick the type** carefully (use the type rules reference).
3. **Add it to the schema note** with type, required/optional, allowed values, example.
4. **Update the relevant template(s)** so new notes get it.
5. **Decide whether to backfill** existing notes. For optional properties, often skip. For required properties, do a wave-based migration.
6. **Verify Dataview queries** that might break or could use the new property.

## Schema growth — how to retire a property

1. Mark it deprecated in the schema note (don't delete the row yet — it's documentation for the next person, including future you).
2. Update templates to remove it.
3. Strip the property from existing notes in waves.
4. Once stripped, move the row to a "Retired properties" section.
