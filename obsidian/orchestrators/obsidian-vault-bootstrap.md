---
name: obsidian-vault-bootstrap
description: >
  Set up a brand-new Obsidian vault with conventions in place from day one — folder structure, filename conventions, frontmatter schema, tag taxonomy, templates, starter MOCs, home note, and in-vault documentation notes that lock the conventions in. Use on day 1 of a fresh vault, when resetting after a chaotic first attempt, or when starting a new vault for a specific purpose. ~2 hours for a focused setup; can be done across two sessions.
stages: [folders, filenames, schema, tags, templates, mocs, home, document]
---

# Obsidian Vault Bootstrap

## Goal

Take an empty (or near-empty) vault to v1 conventions in a single guided pass. The output is a vault that's *ready to grow* — folder structure, schemas, taxonomy, templates, and starter MOCs all in place, with documentation notes that future-you can refer back to.

The flow's premise: every vault eventually develops conventions. Better to set them deliberately at day 1 than retrofit them after a year of drift.

## When to invoke

- **Day 1 of a new vault**
- **Reset after a chaotic first attempt** — wipe the structure (keep the notes), bootstrap fresh
- **New vault for a specific purpose** — research vault, work vault, project vault — convention choices may differ
- **Migrating from another tool** (Notion, Roam, Logseq) — bootstrap the Obsidian conventions before bringing notes over

## When NOT to invoke

- Vault already has 200+ notes with active conventions — restructure with `obsidian-vault-health-sweep` instead, don't bootstrap
- User just wants to "play" with Obsidian — bootstrap is for serious vaults
- The user wants to skip steps — better to commit to all eight stages or postpone

## Stages

### Stage 1 — Folders

**Skill:** `obsidian-folder-architect`

Decide the top-level folder shape. Default is **by note type** (`Inbox/`, `Atomics/`, `Sources/`, `Periodic/`, `MOCs/`, `Reference/`, `Templates/`, `Attachments/`, `Archive/`).

Variations:
- PARA if the user is execution-heavy
- Johnny Decimal lite if the user wants numeric prefixes
- Flat if the user is a MOC-power-user

Set the default new-note location in Obsidian Settings → Files & Links to match (`Inbox/` or `Atomics/`).

**Output passed forward:** folder structure created. Default new-note location set.

---

### Stage 2 — Filename conventions

**Skill:** `obsidian-filename-conventions`

Establish per-note-type filename rules:

- **Atomic notes**: claim-as-title, Title Case
- **MOCs**: `<Topic> MOC`
- **Literature**: source title, Title Case
- **Daily**: ISO date (`2026-05-07`)
- **Project**: project name, Title Case
- **Person**: full name
- **Reference**: topic, Title Case
- **Templates**: kebab-case (`daily-template`, `meeting-template`)

**Output passed forward:** filename rules documented (will be written to a `Filename Conventions.md` note in Stage 8).

---

### Stage 3 — Frontmatter schema

**Skill:** `obsidian-frontmatter-schema-designer`

Define:

- **Universal required properties** — `type:` and `created:` on every note
- **Universal optional** — `status:` (constrained values), `tags:`, `aliases:`
- **Per-type required** — `source-title`, `source-author` for literature; `meeting-date`, `participants` for meetings; etc.
- **Per-type optional** — rating, read-date, etc.

Pick a starter set of types based on use case:
- **Personal/writing vault**: atomic, daily, reference (3 types)
- **Research vault**: atomic, literature, moc, daily, reference (5 types)
- **Generalist work + life**: atomic, literature, moc, daily, project, person, reference (7 types)

**Output passed forward:** schema documented (will be written to a `Frontmatter Schema.md` note in Stage 8).

---

### Stage 4 — Tag taxonomy

**Skill:** `obsidian-tag-taxonomist`

Decide what tags are *for* (the five legitimate purposes — status, type, lifecycle, facet, lightweight topic). Lock in:

- Naming conventions (lowercase, hyphenated, singular default)
- Hierarchy depth (default flat; one level if status/processing facet)
- Entry/exit criteria

Draft a v1 tag list — typically 7–8 tags for a personal vault. Resist the urge to pre-create tags for every imaginable need; let new tags earn their place.

**Output passed forward:** v1 tag list (will be written to a `Tag Taxonomy.md` note in Stage 8).

---

### Stage 5 — Templates

**Skill:** none specifically; integrates outputs from Stages 2–4

Create one template per note type, pre-filled with the schema's required properties:

```
Templates/
├── atomic-template.md
├── moc-template.md
├── literature-template.md
├── daily-template.md
├── project-template.md
├── person-template.md
└── reference-template.md
```

Each template includes:
- The frontmatter schema's required and recommended properties
- Default values where appropriate (`type: atomic`, `status: draft`, `created: <%tp.date.now()%>`)
- A skeletal structure for the body (e.g., literature notes with sections for "Source," "Key Claims," "Reactions," "Quotes")

Configure Obsidian's Templates plugin (or Templater) to use the `Templates/` folder.

For daily notes specifically: configure the Daily Notes plugin to use `daily-template.md` and file output to `Periodic/Daily/`.

**Output passed forward:** templates in place; Obsidian configured to use them.

---

### Stage 6 — Starter MOCs

**Skill:** `obsidian-moc-architect`

Identify the user's top 3–7 active topics. For each:
- Decide if it earns a MOC at day 1 (typically yes for top-level life areas, no for emerging topics)
- Draft a starter MOC with type, parent-moc, and skeletal structure
- Don't fill MOCs with placeholder links; leave them lean and grow as content accumulates

For an empty vault, MOCs may be just structure (headings, brief commentary) without links yet. That's fine — they're designed to grow.

**Output passed forward:** 3–7 top-level MOCs in `MOCs/`, each with `parent-moc: "[[Home]]"`.

---

### Stage 7 — Home note

**Skill:** `obsidian-moc-architect` (home note design specifically)

Design the home note (`Home.md` typically) as the meta-MOC:

```
Home
├── Active (links to top-level MOCs from Stage 6)
├── Resources (links to reference MOCs)
├── Periodic (links to daily-notes index, weekly review)
└── System (links to Tag Taxonomy, Frontmatter Schema, Filename Conventions notes from Stage 8)
```

The home note should fit on one screen. If it doesn't, the vault has more structure than a fresh vault should — pull back.

Pin the home note (Obsidian: right-click → Pin tab) so it's always reachable.

**Output passed forward:** home note pinned and in place.

---

### Stage 8 — Documentation notes

**Skill:** `obsidian-tag-taxonomist`, `obsidian-frontmatter-schema-designer`, `obsidian-filename-conventions`

Write three durable system notes that lock the conventions in:

1. **Tag Taxonomy.md** — purpose, conventions, approved tag list, retired tags section (empty initially)
2. **Frontmatter Schema.md** — universal properties, per-type schemas, allowed values
3. **Filename Conventions.md** — per-type rules, casing, claim-as-title for atomics, alias strategy

These notes:
- Live in `Reference/` (or wherever feels natural)
- Are linked from the home note's "System" section
- Are the source of truth that future-you and Claude both read when answering "what's the convention here?"

**Output passed forward:** none. This is the terminus.

## Handoffs

```
Stage 1 (folders)              ─→ folder structure
Stage 2 (filenames)            ─→ naming rules
Stage 3 (frontmatter)          ─→ schema definitions
Stage 4 (tags)                 ─→ tag taxonomy
        │
        └─ all four converge into ─→ Stage 5 (templates)
                                          │
                                          ▼
                                  Stage 6 (starter MOCs)
                                          │
                                          ▼
                                  Stage 7 (home note)
                                          │
                                          ▼
                                  Stage 8 (documentation notes)
```

Stages 1–4 are largely independent (could parallelize). Stage 5 depends on all four. Stages 6–8 are sequential.

## Failure modes

| Failure | Recovery |
|---|---|
| User skips Stage 4 ("I'll figure tags out later") | Push back. Tags accumulate fast; without v1 taxonomy, the next audit will be the cleanup flow before this vault is even six months old. |
| User wants 12 note types at day 1 | Cut to 5–7. New types can be added later; types defined in advance and never used clutter the schema. |
| User wants 15 starter MOCs | Cut to 3–7. Empty MOCs are worse than no MOCs. |
| User wants to skip the home note | Don't. The home note is the entry point; without it, the user opens the vault into search and never sees the structure. |
| User has thousands of existing notes from another tool | Bootstrap conventions first. Migrate notes after, in waves, applying the conventions as they come over. Don't try to bootstrap and migrate simultaneously. |
| User changes their mind on folder shape mid-bootstrap | Restart. Don't try to mix shapes; the v1 needs to be coherent. |

## Cadence and scope discipline

- **One bootstrap pass = one vault.** Don't run this twice on the same vault unless it's a deliberate reset.
- **All eight stages or none.** Each builds on the previous; skipping creates gaps that compound.
- **2 hours focused, or 2 sessions of 1 hour each.** Trying to spread across a week loses momentum and the user makes inconsistent decisions.
- **First sweep should be Quarter 1.** Bootstrap doesn't replace `obsidian-vault-health-sweep`; the first sweep validates the bootstrap and starts the audit log.

## Related orchestrators

- `obsidian-vault-health-sweep` — runs *after* bootstrap as the periodic checkup
- `obsidian-tag-cleanup-flow` — for cleaning up an existing tag mess; not used during bootstrap (clean slate)
