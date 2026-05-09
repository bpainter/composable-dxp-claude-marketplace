# MOC Lifecycle

How MOCs are created, grow, split, and retire over time.

## Stage 1 — Threshold (1–4 notes)

You have a few notes about a topic. They link to each other inline.

**Don't make a MOC yet.**

What to do instead:

- Add `[[wikilinks]]` between the related notes
- Optionally tag with a lightweight emerging-topic tag (e.g., `#topic-emerging`) with a "promote at 5" rule
- Trust backlinks to surface the cluster

When to revisit: when the cluster reaches 5 notes.

---

## Stage 2 — Promotion (5+ notes)

The cluster has crossed the threshold. Time to create a MOC.

### Steps to create

1. **Pick a name** — `<Topic> MOC` (Title Case)
2. **Pick a pattern** — see `moc-patterns.md` (linear, categorical, question-driven, reading-list, hub-spokes)
3. **Add frontmatter:**

```yaml
type: moc
status: draft
created: 2026-05-07
parent-moc: "[[Home]]"
```

4. **Draft the opening paragraph** — 1–3 sentences on what the MOC covers and from what angle
5. **Group the existing notes into sections** — at least 2, ideally 3–4
6. **Add 1–2 sentences of commentary per section**
7. **Link from the home note** to this new MOC

### Backlink in each note

For each note that the new MOC links to, add a `[[<Topic> MOC]]` link in the note (typically near the top or in a "See also" section). This creates the bidirectional connection.

### MOC `status` evolution

- **draft** — first 30 days while you find the right structure
- **refined** — structure is stable, content is current
- **evergreen** — durable, regularly maintained
- **stale** — hasn't been touched and the topic is dormant
- **archive** — retired (still exists, no longer maintained)

---

## Stage 3 — Growth (5–50 notes)

The MOC is working. New notes about the topic naturally land in it.

### Maintenance habits

- When adding a new note about the topic: add it to the MOC at the same time
- Periodically (monthly?) re-read the MOC and:
  - Verify all links still resolve
  - Refine commentary that no longer reflects current thinking
  - Move/regroup notes if the structure has drifted
- Update `status:` if it's stabilized to refined or evergreen

### Watch for sprawl

Sprawl signs:

- Sections growing past 10 links each
- New sections being added monthly
- Sub-headings appearing (H4 territory)
- The MOC starts to feel exhausting to read

When sprawl appears, the MOC is asking to split.

---

## Stage 4 — Split (50+ links or sprawl signs)

The MOC has grown beyond what one note can map. Time to subdivide.

### Steps to split

1. **Identify natural seams** — sections that have grown into their own coherent sub-topic
2. **Create child MOCs**:
   - `<Sub-topic> MOC` filename
   - `parent-moc: "[[<Original> MOC]]"` property
3. **Move the section's content** to the child MOC, expand commentary
4. **In the original MOC**, replace the section with a short paragraph + link to the child MOC

### Example

Before split — `PKM MOC` has grown to:

```
- Capture
- Atomic notes
- Linking
- MOCs (yes, recursively)
- Properties
- Tags
- Folders
- Workflow
- Tools
- Reading list
```

After split:

```
PKM MOC (parent)
├── Capture MOC
├── Note Craft MOC (atomic notes, claim titles)
├── Connection MOC (linking, MOCs, backlinks)
├── Structure MOC (properties, tags, folders)
├── Workflow MOC
├── Tools MOC
└── Reading list (kept inline; not big enough to split)
```

### Risk

- **Breaking the navigability** — if the original MOC was the entry point, users (including future you) need to be able to reach the new sub-MOCs from somewhere
- **Forgetting to update the parent** — the original MOC should now have a section that summarizes each child + a link

### Verification

- Each child MOC has reasonable note counts in its backlinks
- The parent MOC is now smaller and more readable
- Home note links still go to the right places

---

## Stage 5 — Stale → Refresh or Retire

A MOC hasn't been touched in 6+ months. The topic may be:

- **Active but quiet** — you're still working in it; the MOC just hasn't needed touching → leave it, mark as evergreen
- **Drifted** — your understanding has changed; the MOC is now misleading → refresh (re-read, refactor commentary, update structure)
- **Dormant** — you're no longer working in this topic → consider archive

### Refresh

1. Re-read the MOC end-to-end
2. Note where commentary feels wrong or outdated
3. Note where new notes should be added but aren't
4. Update structure if the topic has reorganized in your mind
5. Update `status:` and the date

### Archive

1. Add `status: archive`
2. Optionally add `archived: <date>` property
3. Move the file to the Archive folder
4. The notes that linked to it still link to it; that's fine — the connection is preserved
5. Optionally add a banner: `> This MOC is archived as of <date>. The topic is no longer active for me.`

### Don't delete a stale MOC

- Backlinks preserve information about the past structure
- Future-you may revive the topic
- Other notes may reference the MOC
- Archive ≠ delete

---

## Stage 6 — Retire

Sometimes a MOC has truly run its course. Reasons:

- The topic was a phase
- The MOC was an experiment that didn't pay off
- The MOC has been superseded by a different MOC

### Steps to retire

1. **For each note linking to the retired MOC**, decide:
   - Replace the link with a link to the new/superseding MOC
   - Replace the link with a direct link to a more appropriate MOC
   - Remove the link entirely (rare)
2. **Archive the MOC file** (don't delete, in case)
3. **Update the home note** to remove the link
4. **Verify** that no backlinks are orphaned

---

## Sub-MOCs and the parent-MOC property

The `parent-moc:` property is the durable mechanism for MOC hierarchy:

```yaml
type: moc
parent-moc: "[[PKM MOC]]"
```

Why a property and not just a link:

- Queryable: "list all MOCs whose parent is PKM MOC"
- Distinct from a body link to the parent (which is also fine, but means "I reference the parent in my prose")
- Survives commentary changes; the structural relationship is durable

The home note has no parent (or `parent-moc: ""`).

---

## Home note as meta-MOC

The home note (`Home.md`) is itself a MOC — a MOC of MOCs. Recommended structure:

```markdown
---
type: moc
status: evergreen
created: <vault start>
---

# Home

The map of my vault. Top-level MOCs sorted by how often I work in them.

## Active

- [[PKM MOC]]
- [[Productivity MOC]]
- [[Health MOC]]
- [[Career MOC]]

## Resources

- [[Reference MOC]] — cheat sheets, glossaries
- [[Reading List MOC]] — what I'm reading

## Periodic

- [[Daily Notes Index]]
- [[Weekly Review]]

## System

- [[Frontmatter Schema]]
- [[Tag Taxonomy]]
- [[Filename Conventions]]
```

The home note should fit on one screen. If it doesn't, it's becoming a real MOC and probably needs to split or delegate sections to sub-MOCs.

---

## Common MOC anti-patterns

- **MOC as exhaustive index** — every note about the topic. Use Dataview for exhaustive.
- **MOC with no commentary** — just lists. Add 1–2 sentences per section.
- **MOC that duplicates folder structure** — if the folder is `Health/`, the MOC repeats it. Either delete the MOC (folder does the job — but folders shouldn't be topic) or refactor to MOC-only and flatten the folder.
- **MOC that's a wishlist** — only "to read" / "to write." Keep wishlists as a section, not the whole MOC.
- **MOC that's never read** — symptom of a MOC made for organization-sake rather than use. Prefer no MOC over an unread MOC.
- **Nested MOCs that mirror folders** — `Vault/PKM/Atomics/` mirroring PKM MOC > Atomics MOC > .... Folders for type, MOCs for topic.
