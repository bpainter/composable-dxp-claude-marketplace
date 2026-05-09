# Naming by Note Type

Per-note-type filename conventions with worked examples.

## Atomic notes

### Recommended: claim-as-title (Title Case)

```
Atomic Notes Connect Better than Monoliths.md
Properties Are the Structured Layer of the Vault.md
Tags Are Cross-Cutting Facets, Not Topic Taxonomies.md
Links Between Notes Outperform Folders for Topic.md
MOCs Replace Folders as the Topical Layer.md
```

### Why claim-as-title

- Forces articulation of what the note *says*
- Reads as the note's TL;DR in backlinks and search
- Acts as the note's contract — if the body doesn't support the claim, you know to refine
- Makes the graph more meaningful (every node is a *position*, not a *bucket*)

### Patterns to follow

- Active voice ("X does Y")
- Singular focus (one claim per note)
- Title Case (every major word capitalized)
- 4–10 words is the sweet spot

### Patterns to avoid

- **Topic-only**: `Atomic Notes` — what about them? Could mean anything.
- **Question without answer**: `What Are Atomic Notes?` — fine for a draft, but the answer should become the title.
- **List-y**: `Things About Linking` — not a claim.
- **Vague**: `Notes` — useless.

### When topic-as-title is fine for atomic notes

- Very early in vault development; you'll refactor later
- The note is a definition (a "term card") — `Folgezettel`, `Backlinks`, `Aliases`. Even then, consider framing as "X Is Y" if the note is more than a one-line def.

## MOCs (Maps of Content)

### Recommended: `<Topic> MOC` (Title Case)

```
Zettelkasten MOC.md
Productivity MOC.md
Health MOC.md
PKM MOC.md
```

### Why this pattern

- "MOC" suffix makes the note's role obvious in search and links
- Distinguishes `[[Zettelkasten MOC]]` (the map) from any future atomic note titled with a Zettelkasten claim
- Stable across topic evolution

### Variations

- Drop the suffix if `type: moc` property is reliable: `Zettelkasten.md`. But this collides with topic-as-title atomic notes more often than not. The suffix is worth it.
- Use `Map of <Topic>`: `Map of Zettelkasten.md`. Reads more naturally for some users; longer titles.
- Use a leading emoji: `🗺️ Zettelkasten.md`. Some users like the visual distinction. Avoid if cross-filesystem portability matters.

### Home / Index MOC

The vault's top MOC is usually:

```
Home.md
Vault Map.md
Index.md
```

Pick one and stick with it. `Home` is the most common.

## Literature / Source notes

### Recommended: source title verbatim, Title Case

```
The Order of Time.md
How to Take Smart Notes.md
The Beginning of Infinity.md
```

### Variations

- **With author**: `The Order of Time by Carlo Rovelli.md`. More disambiguation; longer.
- **Year suffix**: `The Order of Time (2017).md`. Useful when you have multiple editions or multiple books with the same title.

### Articles, podcasts, videos

```
Why You Should Stop Reading by Cal Newport.md
The Knowledge Project Episode 174 - Ed Catmull.md
Naval on Reading - YouTube.md
```

For ephemeral content, prefer aliases over verbose titles.

### Avoid

- Encoded source type in title: `[Book] The Order of Time.md`. Use `source-kind: book` property instead.
- Date-prefixed titles unless using ZK ID system

## Daily / Periodic notes

### Recommended: ISO format

```
2026-05-07.md
2026-W19.md          (week)
2026-05.md           (month)
2026-Q2.md           (quarter)
2026.md              (year)
```

### Why ISO

- Sortable as text
- Internationally unambiguous
- Templater and Daily Notes plugin defaults

### Variations

- Some users like `2026-05-07 Thursday.md` for readability. Works, but the day-of-week computes from the date and isn't necessary in the filename.
- Avoid `May 7, 2026.md` — sort order breaks, locale-dependent.

## Project notes

### Recommended: project name in Title Case

```
Website Redesign.md
Book Launch Q2.md
Annual Review 2026.md
```

### When to add a date

Only when you have multiple iterations: `Annual Review 2026.md` vs `Annual Review 2025.md`.

### Status in the title

Don't. Use `project-status:` property. Status changes; filenames shouldn't.

## Person notes

### Recommended: full name as written

```
Jane Doe.md
Bob Smith.md
Steph Ango.md
```

### Variations

- **Aliases for first-name links**: filename `Jane Doe.md`, alias `Jane`. Then `[[Jane]]` resolves to `Jane Doe`.
- **Disambiguation for common names**: `Jane Doe (Engineering).md` and `Jane Doe (Marketing).md`.

### Avoid

- Initials only (`JD.md`)
- Username form (`@janedoe.md`)
- Role-as-name (`PM at Acme.md`)

## Reference / Resource notes

### Recommended: topic-as-title

```
Markdown Cheatsheet.md
SQL Quick Reference.md
Common Logical Fallacies.md
US Holidays.md
```

### Variations

- "Cheatsheet" / "Reference" / "Quick Reference" suffix when the note's role isn't obvious from the topic

## Template notes

### Recommended: kebab-case

```
daily-template.md
meeting-template.md
literature-template.md
project-template.md
moc-template.md
```

### Why kebab-case

- Visually distinct from content notes
- Reads as "this is plumbing"
- Templater, Periodic Notes, etc. don't care about case

### Variations

- Underscore prefix to sort to top: `_daily-template.md`. Works; not necessary if templates live in a separate folder.

## Inbox / Fleeting notes

### Recommended: whatever the user types — they get renamed on processing

```
Untitled.md
Untitled 1.md
note about that thing.md
```

These exist briefly, then become atomic notes (with claim titles) or get archived.

## Archive notes

Filenames stay the same; the location changes. Don't append `(Archived)` or rename — it breaks links.

## Quick reference table

| Type        | Pattern                              | Example                                       |
| ----------- | ------------------------------------ | --------------------------------------------- |
| Atomic      | Claim, Title Case                    | `Atomic Notes Connect Better than Monoliths`  |
| MOC         | `<Topic> MOC`                        | `Zettelkasten MOC`                            |
| Literature  | Source title, Title Case             | `The Order of Time`                           |
| Daily       | ISO date                             | `2026-05-07`                                  |
| Weekly      | ISO week                             | `2026-W19`                                    |
| Project     | Project name, Title Case             | `Website Redesign`                            |
| Person      | Full name                            | `Jane Doe`                                    |
| Reference   | Topic, Title Case                    | `Markdown Cheatsheet`                         |
| Template    | kebab-case                           | `daily-template`                              |
| Inbox       | Anything; will be renamed            | `Untitled`                                    |
