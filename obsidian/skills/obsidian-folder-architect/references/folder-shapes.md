# Folder Shapes

Top-level structures for Obsidian vaults, with worked examples and trade-offs.

## Shape 1 — By note type (recommended default)

```
Vault/
  00-Inbox/              # unprocessed capture
  Atomics/               # atomic notes (your evergreen ideas)
  Sources/               # literature notes about external sources
  MOCs/                  # Maps of Content
  Periodic/              # daily/weekly/monthly notes
    Daily/
    Weekly/
    Monthly/
  Reference/             # cheat sheets, glossaries, durable lookup
  People/                # person notes
  Projects/              # project notes (small bounded efforts)
  Templates/             # note templates
  Attachments/           # images, PDFs, audio
  Archive/               # retired notes kept for record
```

### Why it works

- Stable across content. The shape doesn't change as topics shift.
- Pairs cleanly with MOCs: folders for *kind*, MOCs for *topic*.
- Each folder has a clear note-type rule, which means templates and automations target cleanly.

### Variations

- Drop `People/` and `Projects/` if not needed; merge into `Notes/` or `Atomics/`
- Combine `Periodic/Daily/`, `Periodic/Weekly/`, `Periodic/Monthly/` if you only do daily
- Some users prefer `Sources/Books/`, `Sources/Articles/` — fine if each subfolder reaches 20+ notes

### When this is the right shape

- Personal knowledge vault
- Research vault
- Writing-and-thinking vault
- Most vaults, frankly

---

## Shape 2 — PARA (Tiago Forte)

```
Vault/
  1-Projects/            # active efforts with deadlines
  2-Areas/               # ongoing responsibilities
    Health/
    Career/
    Family/
    Finance/
  3-Resources/           # reference material organized by topic
  4-Archive/             # completed projects, dormant areas, deprecated resources
  Inbox/                 # capture
  Templates/
```

### Why it works

- Action-oriented. The Projects folder is always front-and-center.
- Mature concept with a wide community.
- Good for users who think in terms of "what am I working on?"

### Trade-offs

- Notes that are *just thinking* (atomic notes about an idea) don't have a great home — they end up in `Resources/` and the shape starts to feel like a knowledge filing cabinet.
- The Areas substructure tempts deep nesting. Resist; one level deep, max.
- Resources organized by topic *is* topic-as-folder. Use MOCs in `Resources/` to compensate.

### Variations

- **PARA + Atomics** — keep PARA for execution, add a top-level `Atomics/` for atomic knowledge notes
- **PARA + Sources** — same idea: keep PARA, add `Sources/` for literature notes

These hybrids are very common and often the best fit for users who do both deep thinking and project execution.

### When this is the right shape

- Execution-heavy work vaults
- Action-oriented users who want their projects always visible
- Vaults that intentionally don't go deep on knowledge management

---

## Shape 3 — Johnny Decimal

```
Vault/
  10-19 Capture/
    11 Inbox/
    12 Daily/
  20-29 Notes/
    21 Atomics/
    22 Sources/
    23 People/
  30-39 Projects/
    31 Active/
    32 Archive/
  40-49 Reference/
    41 Cheatsheets/
    42 Glossaries/
  90-99 System/
    91 Templates/
    92 Attachments/
```

### Why it works

- Numbered prefixes give predictable sort order
- The "categories of categories" (the 10-19 grouping) creates intentional spacing
- Forces discipline about what's worth a top-level group

### Trade-offs

- Numbers are noise for small vaults
- Adding a new category requires planning (what number?)
- The system has a learning curve users don't always want

### Variations

- Just numeric prefixes without full JD discipline (`00-Inbox/`, `10-Atomics/`, `20-Sources/`) — gives sort order without the formal system
- This is sometimes called "numbered prefix lite" and is very common

### When this is the right shape

- Vaults with 2,000+ notes
- Users who like systems-with-rules
- Cases where the file system is shared with other apps that benefit from prefixed sort

---

## Shape 4 — Flat / minimal

```
Vault/
  Inbox/                 # all capture
  Notes/                 # everything that's not the below
  Daily/                 # daily notes
  Templates/
  Attachments/
  Archive/
```

### Why it works

- Minimum filesystem complexity
- Forces organization into MOCs and properties
- Folder-related decisions are rare

### Trade-offs

- New users hate the empty-folder feeling and over-rely on search
- Templates can't auto-target by note type as easily (everything ends up in `Notes/`)
- Hard to share or export a subset by folder

### When this is the right shape

- Power users who fully commit to MOCs for organization
- Vaults where the user trusts search and Dataview
- Very small vaults (< 200 notes) where structure isn't paying off

---

## Shape 5 — Zettelkasten classic (Folgezettel)

```
Vault/
  Zettel/                # all atomic notes, named by Folgezettel ID
  Inbox/
  Sources/
  Templates/
  Attachments/
```

### Why it works

- Reflects Luhmann's original system: notes get IDs, IDs encode lineage
- Filenames carry order (`1a3b.md`, `1a3c.md`)

### Trade-offs

- Filename IDs are friction in 2026; most users prefer titles + properties
- Folgezettel order is one organization; doesn't replace MOCs

### When this is the right shape

- Users specifically committed to classical Zettelkasten
- Researchers who want lineage encoded in IDs

---

## Comparison table

| Shape           | Top-level folders | Best for                               | Worst for                          |
| --------------- | ----------------- | -------------------------------------- | ---------------------------------- |
| By note type    | 7–10              | Most vaults; balanced thinking + exec   | Heavy execution-only workflows     |
| PARA            | 4–5               | Action-heavy, project-driven users     | Pure knowledge vaults              |
| Johnny Decimal  | 6–9               | Very large vaults; system-loving users | Small vaults; minimalists          |
| Flat / minimal  | 3–6               | MOC-power users; trust-search users    | New users; type-aware automation   |
| Zettelkasten    | 3–5               | Classical ZK practitioners             | Most modern PKM users              |

---

## Choosing — a quick decision

| If you...                                              | Try this shape           |
| ------------------------------------------------------ | ------------------------ |
| Are starting a new general-purpose vault                | **By note type**         |
| Are heavy on projects with deadlines                    | PARA (or PARA + Atomics) |
| Have 2,000+ notes already and want sort discipline      | Johnny Decimal lite      |
| Trust MOCs and want minimal structure                   | Flat                     |
| Are a classical Zettelkasten practitioner               | Zettelkasten             |

If unsure, default to **by note type**. It's the most forgiving and pairs best with the rest of the skills in this category.

---

## Anti-patterns to avoid in any shape

- **Topic folders** (`Topics/PKM/`) — use MOCs
- **Per-project subfolders** for small projects — use a project note instead
- **Mirror of a CRM/task system** — Obsidian isn't a project tracker
- **Year folders** for daily notes (`2026/05/07/`) — Obsidian's daily note plugin handles this; if you want it, the plugin has a setting
- **Catch-all `Misc/` or `Uncategorized/`** — these grow unbounded
- **More than two levels of nesting** — almost always means topic-as-folder
- **`Active/` and `Archive/` siblings** for status routing — use a `status:` property
