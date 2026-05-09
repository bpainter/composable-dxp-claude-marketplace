# Collisions and Aliases

Strategies for handling name collisions and using aliases to consolidate naming variants.

## Aliases — when and why

The `aliases:` property in frontmatter is a list of alternate titles a note responds to in search and `[[wikilink]]` autocomplete.

```yaml
aliases:
  - MOC
  - Maps of Content
  - Curated Index
```

When you type `[[MOC` in another note, all three names appear and any of them resolves to the same target.

### When to use aliases

- Multiple natural ways to refer to the same concept
- Acronyms vs. spelled-out forms (`PKM` ↔ `Personal Knowledge Management`)
- First-name-only links to person notes (`Jane` ↔ `Jane Doe`)
- Plural ↔ singular when both feel right (`Atomic Note` ↔ `Atomic Notes`)
- Old name kept findable after a rename (back-compat)

### When NOT to use aliases

- The variant is genuinely a different concept — make a different note
- The variant is a status/type rather than a name (`Active Project X` should not alias `Project X`)
- The variant is misspelled — fix the misspelling at the source instead

### Populating aliases

Add aliases as you write. When you find yourself wanting to type `[[MOC]]` but the file is `Maps of Content.md`, add `MOC` to its aliases right then.

Periodically review: are there aliases that should be promoted to canonical filenames? Are there aliases that are unused and bloating the property?

## Collisions — when two notes want the same filename

### Diagnosis

Obsidian doesn't allow two `.md` files with the same name in the same folder. Across folders, it allows them but `[[link]]` becomes ambiguous (Obsidian picks one and warns).

### Strategy 1 — Disambiguate with parenthetical

```
Inbox (GTD).md
Inbox (Folder).md
```

```
Slack (App).md
Slack (Practice).md
```

The parenthetical part is part of the filename, not metadata. Choose what's in parens to be the *minimal* distinguishing context.

### Strategy 2 — Disambiguate with qualifier

```
Jane Doe (Engineering).md
Jane Doe (Marketing).md
```

```
Reflection (Annual).md
Reflection (Daily).md
```

Same idea, but the qualifier is more semantic.

### Strategy 3 — Promote one to a MOC

If one of the two notes is a topic and the other is a specific instance:

- `Productivity` (the topic) → `Productivity MOC.md`
- `Productivity` (a specific atomic note about productivity) → keeps the bare name

Now `[[Productivity]]` goes to the atomic note; `[[Productivity MOC]]` goes to the map. No collision.

### Strategy 4 — Rename one entirely

If neither is a MOC, sometimes one note just deserves a different name. Atomic notes especially: a topic-titled atomic note is a smell anyway. Refactor to a claim-title and the collision disappears.

## Aliases for migration / back-compat

### Scenario: rename a note but keep old links working

You rename `Linking Strategy.md` to `Links Between Notes Outperform Folders for Topic.md` (claim-as-title refactor). But other notes link via `[[Linking Strategy]]`.

Without aliases:

- "Update Links on Rename" handles existing links — they get updated.
- New typing of `[[Linking Strategy]]` won't resolve to the new note.

With an alias `Linking Strategy` on the renamed note, both work.

### Scenario: deprecating a note

You're consolidating two atomic notes. The "loser" gets merged into the "winner."

- Move the loser's content into the winner.
- Add the loser's title as an alias on the winner.
- Delete the loser file.
- Update Links on Rename doesn't fire on delete; you'll need to verify links to the deleted note resolved correctly (they'll go to the alias target on the winner).

## Aliases for first-name links

For person notes:

```yaml
# Jane Doe.md
aliases:
  - Jane
```

This works only if there's exactly one Jane in the vault. If there are multiple Janes, drop the alias and link with full names.

## The "alias as future-proofing" pattern

When creating a note with a likely-to-be-referenced topic, add 1–2 aliases at creation time:

```yaml
# Maps of Content.md
aliases:
  - MOC
  - Map of Content
```

You'll be glad later when you want to type the short form.

## Avoiding collision via folder paths — don't

Some users put `Inbox` notes in different folders to "disambiguate":

```
GTD/Inbox.md
Vault/Inbox.md
```

Obsidian *does* allow this and search filters work, but `[[Inbox]]` becomes ambiguous. Avoid this pattern; use parenthetical disambiguation in the filename instead.

## Aliases vs. tags vs. links

A common confusion:

- **Alias** = "this file has multiple names that all link to it"
- **Tag** = "this file shares a facet with other files"
- **Link** = "this file references another file"

These don't overlap. If you want `[[Method]]` and `[[Approach]]` to both resolve to the same note, alias them. If you want notes about `methods` to share a tag, that's `#methods` (or a MOC). If `Method` is a different concept from `Approach`, link them.

## Renaming etiquette

- Verify "Update Links on Rename" is on (default) before any rename.
- For high-traffic notes (linked from 50+ places), do the rename in a moment when you're not mid-writing — link updates can take a second.
- After renaming, verify a sample link works in a different note.
- Add the old name as an alias if there's any chance of typed back-references.

## Aliases for variant spellings and language

```yaml
aliases:
  - Color  # primary
  - Colour
```

Useful for users who want either spelling to resolve. Don't go overboard — every alias is a search hit, and noise builds up.

## Length and noise considerations

Aliases appear in `[[link]]` autocomplete. Too many aliases on too many notes = noisy autocomplete.

- 0–2 aliases per note: ideal
- 3–5 aliases per note: acceptable for hub notes (MOCs, person notes)
- 6+ aliases: probably overkill; ask if some are really separate concepts

## Common mistakes

- **Aliases as tags**: `aliases: [draft, atomic, idea]`. No — those are tags or properties.
- **Aliases as descriptions**: `aliases: ["a note about how MOCs work"]`. No — that's body content.
- **Aliases without canonical name**: aliases are alternates *to* the canonical filename. The filename is still primary.
- **Forgetting to add an alias when renaming**: links from outside the vault (Readwise exports, mobile shares) won't go through Update-Links-on-Rename. Add the old name as alias for back-compat.
