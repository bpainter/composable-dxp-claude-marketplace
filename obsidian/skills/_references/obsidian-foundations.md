# Obsidian Foundations

Shared context for every skill in the `obsidian/` category. Skills should reference this file for vault concepts, Obsidian-native idioms, and the opinionated stance the category takes on best practices.

## What an Obsidian vault is

A vault is a folder of plain Markdown files plus a `.obsidian/` config directory. Every "note" is a `.md` file. Everything else — links, tags, properties, embeds, search, plugins — operates on top of those files.

Concrete implications:

- **Files outlive Obsidian.** The vault is grep-able, git-able, and editable in any text editor. Skills should never recommend patterns that only work inside the app.
- **Folder = filesystem folder.** No magic. Moves and renames are real filesystem operations.
- **The graph is emergent.** It's computed from `[[wikilinks]]`, frontmatter properties of type `link`, and embeds. If two notes don't link, they don't connect — even if they're in the same folder.

## The four organizing mechanisms

Every Obsidian vault uses four mechanisms to express structure. Skills must teach the user when to reach for each.

| Mechanism      | What it expresses                                | Strengths                                       | Weaknesses                                          |
| -------------- | ------------------------------------------------ | ----------------------------------------------- | --------------------------------------------------- |
| **Folders**    | Mutually exclusive *location* of a note          | Familiar; one-place-per-note; works outside app | Forces single classification; nesting gets brittle  |
| **Properties** | Structured *attributes* of a note (frontmatter)  | Queryable; typed; per-note-type schemas         | Invisible until rendered; schema drift is real      |
| **Tags**       | Cross-cutting *facets* (status, type, topic)     | Many per note; great for filters and dashboards | Easy to over-tag; synonyms multiply silently        |
| **Links**      | *Relationships* between notes ([[wikilinks]])    | Compose into the graph; bidirectional via backlinks | Require maintenance; broken on rename without care |

The dominant failure mode in vaults: people use folders to express what links and properties should express. Result is brittle, deep folder trees that fight the tool.

## The opinionated stance for this category

These are the defaults skills should advocate. Users can override, but skills lead with these positions.

1. **Links over folders for connection.** Folders express *where a note lives*; links express *what it relates to*. If you find yourself wanting a note in two folders, that's a link, not a folder problem.
2. **Properties over inline metadata.** Frontmatter (YAML properties) is the structured layer. Use `status:`, `type:`, `created:`, `source:` as properties, not `#status/active` tags. Tags should be reserved for facets that don't deserve a typed slot.
3. **Tags for facets, not taxonomies.** Tags are best at status (`#draft`, `#evergreen`), note type when no property exists, and lightweight cross-cutting markers. Topic taxonomies belong in **links and MOCs**.
4. **Shallow folders, deep linking.** Most vaults thrive with a top-level structure of 5–9 folders and almost no nesting beyond two levels. Depth is a smell.
5. **MOCs, not folders, are the topic layer.** A Map of Content (MOC) is a note that links to other notes about a topic, with structure and commentary. MOCs scale with the vault; folders don't.
6. **Atomic notes connect better.** Notes built around a single idea or claim link cleanly into multiple contexts. Monolithic notes become orphan continents.
7. **Search and Dataview beat rigid structure.** With good properties and links, retrieval is a query, not a folder walk. Skills should bias toward making notes *findable*, not *located*.
8. **The vault is plain text first.** Avoid patterns that depend on a specific theme, plugin, or rendering. Properties, links, tags, and basic Markdown are the durable substrate.

## Note types

A vault typically contains a small set of note types. The exact list varies, but the *concept* of typing notes is foundational — most organization decisions flow from "what type of note is this?"

Common types:

- **Atomic** — one declarative idea or claim. The Zettelkasten "permanent note." Rare in beginner vaults; abundant in mature ones.
- **Literature / Source** — notes *about* an external source (book, article, podcast). Quotes, summaries, your reactions.
- **MOC (Map of Content)** — a curated index note for a topic, linking to other notes with structure.
- **Daily / Periodic** — daily, weekly, monthly notes. Time-anchored capture and review.
- **Project** — a bounded effort with an end state.
- **Person** — a person you interact with frequently enough to want a node for.
- **Reference / Resource** — durable lookup material (cheat sheets, glossaries, checklists).
- **Inbox / Fleeting** — unprocessed capture awaiting triage.

The note type is usually expressed as a property (`type: moc`) and sometimes mirrored in folder placement.

## Frontmatter (Properties) basics

Frontmatter is a YAML block at the top of a note, between `---` markers, that Obsidian renders as Properties:

```markdown
---
title: Designing tag taxonomies
type: atomic
status: evergreen
created: 2025-03-04
tags:
  - knowledge-management
  - taxonomy
related:
  - "[[Linking strategy]]"
  - "[[MOC architecture]]"
---
```

Property type discipline matters:

- **text** — short string
- **list** — array (use for `tags`, `related`, `aliases`)
- **date** — `YYYY-MM-DD`
- **datetime** — ISO 8601
- **number** — for ratings, counts, weights
- **checkbox** — booleans (`reviewed: true`)

The reserved property `tags:` is a list, and tag values do **not** include `#`. The reserved `aliases:` is a list of alternate titles a note responds to in search and link autocomplete.

## Links

Three flavors:

1. **`[[Note]]`** — vault-internal wikilink. Resolves to a note by title (or alias).
2. **`[[Note#Heading]]`** or **`[[Note#^block-id]]`** — link to a specific heading or block.
3. **`[[Note|alias]]`** — display alternate text.

Embeds use `![[Note]]` (or `![[Note#Heading]]`). Use embeds sparingly; they can mask the difference between owning content and referencing it.

Backlinks are automatic. The "Linked mentions" pane shows notes that reference the current note. Mature vaults treat backlinks as a primary navigation tool.

## Tags

- Inline as `#tag` anywhere in the note body, or as a `tags:` list in frontmatter (no `#`).
- Hierarchical via slashes: `#status/draft`, `#topic/pkm/zettelkasten`. Hierarchies are best used sparingly — one or two top-level facets, not deep trees.
- Tags are cheap to add and expensive to clean. Bias toward fewer, more meaningful tags.

## Filenames

The filename **is** the note title (without `.md`) and is what `[[wikilinks]]` resolve against. This makes filenames first-class content, not implementation detail.

Patterns the category recommends:

- **Title Case for content notes** ("Designing Tag Taxonomies") — readable in links and search.
- **Kebab-case for system / template / utility files** (`daily-template`, `moc-template`).
- **No timestamps in filenames** unless using a Zettelkasten ID system intentionally. Use `created:` property instead.
- **Aliases for variants** rather than duplicate notes.
- **Avoid characters that break links across systems**: `/`, `\`, `:`, `|`, `?`, `*`, `<`, `>`, `"`. Obsidian sanitizes some, but cross-system portability suffers.

## MOCs (Maps of Content)

A MOC is a curated note that organizes other notes around a topic. Key traits:

- Lives in the vault as a regular `.md` file, typically with `type: moc` in frontmatter.
- Contains links to other notes, organized with headings, lists, or short paragraphs of commentary.
- Is *opinionated* — it reflects how *you* think about the topic, not an exhaustive index.
- Grows and splits as the underlying body of notes grows.

MOCs replace deep folder hierarchies as the topical organization layer. A vault with strong MOCs can have a flat folder structure and still feel highly organized.

## Common Obsidian features skills should be aware of

- **Properties view** — table-like view of frontmatter across notes; surfaces schema drift.
- **Search operators** — `path:`, `tag:`, `file:`, `["property":"value"]`, `line:`. Useful for hygiene and audits.
- **Dataview / Bases** — query notes by frontmatter and links. Bases is Obsidian's native equivalent (live in the app); Dataview is a community plugin.
- **Templates / Templater** — for new notes of a given type. Skills should treat templates as the enforcement mechanism for property schemas.
- **Canvas** — visual node-and-edge surface. Useful for thinking; not the primary organizing layer.
- **Graph view** — useful for spotting clusters, hubs, and orphans, but not for daily use.

## Anti-patterns the category names and discourages

- **Deep nested folders** mirroring an org chart or a topic taxonomy. (Use MOCs.)
- **Tag explosion** — hundreds of tags, many used once. (Tag hygiene + properties.)
- **Status as a tag instead of a property.** (Use `status:` property.)
- **Topic as a tag instead of a link.** (Use `[[Topic MOC]]`.)
- **Catch-all "Misc" or "Uncategorized" folders** that grow without bound.
- **Renaming a note without updating links** when not using "Update links on rename" (it's on by default; verify).
- **Frontmatter freelancing** — every note inventing its own keys. (Schemas + templates.)
- **Daily notes as a junk drawer** with no transitions to atomic notes.
- **MOCs that are exhaustive indexes** instead of opinionated maps.

## Influences this category leans on

- **kepano (Steph Ango)** — file over app, properties-first, simple structure.
- **Nick Milo (LYT)** — Maps of Content, IMFs (Ideal Mental Framework), home notes.
- **Andy Matuschak** — evergreen notes, atomicity, declarative titles.
- **Tiago Forte** — PARA as one valid folder approach (one option among several).
- **Niklas Luhmann (Zettelkasten)** — atomic notes, Folgezettel, ID systems (an option, not the default).

Skills should cite these influences when explaining a recommendation, but should not require the user to adopt any single thinker's full system.
