# Tag Naming Patterns

Conventions and worked examples for naming tags consistently in an Obsidian vault.

## The non-negotiables

These rules apply to *every* tag, regardless of purpose or hierarchy.

### Lowercase always

Obsidian treats `#Draft` and `#draft` as different tags. Pick lowercase and stay there.

```
✅ #draft
❌ #Draft
❌ #DRAFT
```

### Hyphens for word separation

```
✅ #needs-citation
✅ #to-process
❌ #needsCitation        (camelCase fails search and looks foreign)
❌ #needs_citation       (underscores read like code, not language)
❌ #needs citation       (spaces break tags entirely)
```

### No leading or trailing punctuation

```
✅ #moc
❌ #-moc
❌ #moc-
❌ #_moc
```

### ASCII safe characters only

Tags support letters, numbers, hyphens, slashes, and underscores. Stick to letters, hyphens, and slashes.

```
✅ #book
❌ #book✨            (emoji works in some contexts, breaks others)
❌ #livre/français     (accented characters survive but fragment search)
```

## Singular vs plural

Pick **singular** unless the tag fundamentally describes a set.

```
✅ #book
❌ #books
```

Reasoning: a tag applied to a note about *one* book reads more naturally as `#book`. The plural creates the awkward "this note is about books" framing when it's really about *a* book.

The same logic applies to people, places, projects (when those should be tags at all):

```
✅ #person       (when used as a note-type marker)
✅ #project      (same)
✅ #meeting
```

Genuine exceptions where plural fits:

```
✅ #patterns     (a note that is about patterns as a category)
✅ #principles   (same)
```

But these are usually MOCs in disguise. If a tag is consistently being used to mean "notes about a category," it's a MOC.

## Hyphenation patterns

For multi-word tags, hyphenate naturally:

```
✅ #needs-citation
✅ #to-process
✅ #has-image
✅ #well-defined
```

For compound concepts where the hyphen would be confusing, prefer a single concept:

```
✅ #stub                 (rather than #needs-development)
✅ #scratch              (rather than #not-yet-organized)
```

## Hierarchy patterns

When using one level of hierarchy (`parent/child`), the parent must:

- Represent a clear facet (status, type, processing-state)
- Have at least 3 distinct children that all live under it
- Be a noun, not an adjective

### Good hierarchies

```
#status/draft
#status/evergreen
#status/archive
#status/stale
```

```
#processing/inbox
#processing/to-link
#processing/to-archive
```

```
#type/moc
#type/person
#type/book
```

### Bad hierarchies (smell, fix)

```
❌ #status/draft/needs-review        (two levels deep without earning it)
❌ #project/website/redesign         (project name belongs in [[Website Redesign]] note)
❌ #topic/pkm/zk                     (topic taxonomy — use MOCs)
❌ #personal/health/sleep            (life-area taxonomy — use MOCs or PARA folders)
```

## Prefixes — when they earn their keep

Use a prefix only if the prefix carries genuine meaning that distinguishes the tag from other tags. The above status/processing/type prefixes pass this test. Most other prefixes don't.

### Worth using

- `status/` — clearly distinguishes lifecycle state from other facets
- `type/` — distinguishes note type from facets (transitional; prefer `type:` property)
- `processing/` — distinguishes workflow state from durable state
- `area/` — when used as a flat layer of life-areas (transitional toward PARA folders)

### Don't bother

- `tag/` — meaningless ("tag/draft"?)
- `note/` — meaningless
- `obsidian/` — what would this distinguish from?
- `general/` — same problem

## Aliasing and synonyms

Obsidian tags don't natively support aliases. To handle synonyms:

1. Pick the canonical tag (the one in your taxonomy note)
2. When you encounter a synonym in the wild, replace it (manual or with bulk-rename plugin)
3. Document the retired synonym in the taxonomy note

You cannot "redirect" `#books` to `#book` automatically. The fix is the rename, not a synonym layer.

## Tag length

Keep tags short. Aim for 1–3 words.

```
✅ #to-process
✅ #needs-citation
❌ #notes-that-need-additional-citation-research
```

If a tag wants to be a sentence, it's probably a checklist item or a property.

## Naming worked examples

### Status taxonomy

```
#status/inbox
#status/draft
#status/refined
#status/evergreen
#status/stale
#status/archive
```

Six values, one facet, clean parallel structure.

### Processing taxonomy (GTD-flavored)

```
#processing/to-process
#processing/to-link
#processing/waiting
#processing/someday
#processing/dropped
```

### Note type (transitional, prefer property)

```
#type/atomic
#type/moc
#type/literature
#type/daily
#type/person
#type/project
```

### Cross-cutting facets

```
#needs-citation
#has-image
#has-quote
#confidential
#published
```

Notice these don't get a prefix. They're cross-cutting, so a prefix wouldn't add information.

## Renaming a tag safely

When you rename a tag (`#books` → `#book`):

1. Search for `#books` (use the Search pane with `tag:#books`)
2. Verify the count — if 200+, plan a session, not a hot fix
3. Use a bulk-rename mechanism: Search-and-replace plugin, or a script if comfortable
4. Verify with a fresh search that no `#books` remain
5. Update the taxonomy note: move `#books` to the "Retired tags" section with the date and reason

## A tag-naming quick checklist

Before adding a new tag, verify:

- [ ] Lowercase
- [ ] Hyphenated if multi-word
- [ ] Singular form (unless deliberately plural for a category)
- [ ] No leading/trailing punctuation
- [ ] No deeper than one slash level
- [ ] Doesn't duplicate or near-duplicate an existing tag
- [ ] Documented in the taxonomy note before use
