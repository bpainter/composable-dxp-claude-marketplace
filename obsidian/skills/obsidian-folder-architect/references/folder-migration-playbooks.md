# Folder Migration Playbooks

Playbooks for changing the folder structure of an existing Obsidian vault without breaking links or losing notes.

## Before any migration

- **Backup or commit.** If using git, commit. Otherwise, copy the vault folder.
- **Confirm "Update links on rename" is enabled.** Settings → Files & Links. This is on by default. Without it, links break on file move.
- **Plan in waves.** Big-bang migrations are risky and exhausting.
- **Verify with a small batch first.** Move 5 notes, confirm links updated, then proceed.

---

## Playbook 1 — Deep tree (3+ levels) → Flat (1–2 levels)

### Symptom

`Topics/PKM/Zettelkasten/Permanent Notes/note.md` — folders mirroring topic taxonomy.

### Steps

1. **Decide the new top-level shape.** Use `folder-shapes.md`.
2. **List all current folders past depth 2.** These are the migration target.
3. **For each deep folder, decide the destination:**
   - The notes inside become flat-located by *type* (e.g., they're all atomic notes → `Atomics/`)
   - The folder concept becomes a MOC (e.g., `Permanent Notes/` → `[[Permanent Notes MOC]]`)
4. **Build the MOCs first.** (Defer to `obsidian-moc-architect`.)
5. **Move notes wave by wave** (e.g., one deep folder per session). Verify links update.
6. **Add `[[MOC]]` link to each migrated note** (typically near the top).
7. **Delete empty folders.**

### Risk

- Medium. The risk is forgetting to add MOC links before moving — the topical connection is lost.

### Verification

- Each MOC's backlink count covers the original folder's note count.
- No notes left in the deleted folders.
- Internal links still resolve (search for `[[`-prefixed broken links).

---

## Playbook 2 — Topic-based folders → Type-based folders

### Symptom

`Vault/Health/`, `Vault/Career/`, `Vault/PKM/` — top-level by topic.

### Steps

1. **Inventory all current top-level folders** by topic.
2. **Create new top-level folders by note type** (`Atomics/`, `Sources/`, `MOCs/`, etc.).
3. **For each topic folder:**
   - Identify the note types inside (atomic, literature, MOC)
   - Move each note to the appropriate type folder
   - Create or update a topic MOC (e.g., `[[Health MOC]]`)
   - Add the MOC link to each migrated note
4. **Verify links** between notes still resolve.
5. **Delete the empty topic folders.**

### Risk

- Medium–high. Many notes move at once.
- The single biggest risk is conflating *type* and *topic* during the move. Take the migration as an opportunity to also fix `type:` properties.

### Verification

- Each topic MOC has reasonable note counts in backlinks
- Type folders contain *only* notes of that type
- Spot-check 10 random notes for both correct location and a MOC link

---

## Playbook 3 — Flat → Type-based

### Symptom

Vault is entirely flat (`Notes/`) and growing past 500 notes. Becoming hard to scan.

### Steps

1. **Define the new top-level shape** (`folder-shapes.md` Shape 1).
2. **Verify or define the `type:` property** on existing notes (`obsidian-frontmatter-schema-designer`).
3. **Bulk-move notes by type:**
   - All `type: atomic` → `Atomics/`
   - All `type: literature` → `Sources/`
   - All `type: moc` → `MOCs/`
   - etc.
4. **Update default new-note location** in Obsidian settings to `Inbox/` or wherever you want raw capture.
5. **Update template targets** so each template files its output in the right folder.

### Risk

- Low–medium. Mostly mechanical once `type:` is reliable.

### Verification

- Check that automation (templates, daily note creation) still files correctly.

---

## Playbook 4 — PARA → Type-based + Atomics

### Symptom

User has been on PARA, now wants to centralize knowledge work and de-emphasize project-folders.

### Steps

1. **Keep PARA's `Projects/` and `Areas/`** as-is — they're working.
2. **Demote `Resources/` to MOCs.** Most resource notes are actually atomic notes or literature notes mis-categorized.
3. **Create new top-level folders:** `Atomics/`, `Sources/`, `MOCs/`, `Reference/`.
4. **Migrate from `Resources/`:**
   - Atomic ideas → `Atomics/`
   - Literature notes → `Sources/`
   - Cheat sheets / glossaries → `Reference/`
   - Topical groupings → MOCs
5. **Keep `Archive/`** at the top level.

### Risk

- Medium. PARA's `Resources/` often contains a wide mix that requires per-note classification.

### Verification

- `Resources/` is empty or contains only legacy items not yet migrated.
- Each MOC pulls together the related notes via backlinks.

---

## Playbook 5 — Adding numeric prefixes for sort order

### Symptom

User wants `Inbox/` to always sort first, etc., without committing to full Johnny Decimal.

### Steps

1. **Decide a numbering scheme:**
   - `00-Inbox/`, `10-Atomics/`, `20-Sources/`, etc.
2. **Rename folders one at a time** (Update links on rename should handle internal links, but folders don't have backlinks like notes — verify).
3. **Update any saved searches, Dataview queries, or paths in templates** that referenced the old folder names.

### Risk

- Low. Renaming folders is supported; the gotcha is paths in YAML or queries.

### Verification

- Templates still work
- Dataview / Bases queries still resolve
- New-note default location updated if it was set to a renamed folder

---

## Playbook 6 — Adding `Templates/` to a vault that doesn't have one

### Symptom

User has been creating notes ad-hoc with no templates. Wants to start using Templater or core Templates.

### Steps

1. **Create `Templates/` folder.**
2. **Set Obsidian's Templates plugin** to use this folder (Settings → Templates).
3. **If using Templater**: configure its template folder location.
4. **Create one template per note type**, pre-filled with the type-specific schema.
5. **Update the default new-note hotkey** to use a template (Templater: `tp.file.create_new`).

### Risk

- Very low. New folder, no migration of existing notes.

### Verification

- Creating a new note via template produces the expected frontmatter

---

## Playbook 7 — Splitting an `Archive/` that's grown too large

### Symptom

`Archive/` has 2,000+ notes and is mixed-content.

### Steps

1. **Decide an archive sub-structure.** Two common options:
   - **By year:** `Archive/2024/`, `Archive/2025/`
   - **By note type mirroring the live structure:** `Archive/Atomics/`, `Archive/Sources/`
2. **Move notes accordingly.** Use `created:` property if going by year.
3. **Optional:** add an `archived: date` property to track when each note was archived.

### Risk

- Low. Archive content is by definition not in active use.

### Verification

- `Archive/` is no longer flat.
- Spot-check that archived notes still appear in MOCs (they should — links don't care about location).

---

## Playbook 8 — Removing a folder layer (un-nesting)

### Symptom

`Vault/Sources/Books/`, `Vault/Sources/Articles/` — sub-typing books vs. articles by folder. The user wants to flatten and use a `source-kind:` property.

### Steps

1. **Verify or add `source-kind:`** to each literature note (`obsidian-frontmatter-schema-designer`).
2. **Move all books and articles up one level into `Sources/`.**
3. **Verify Dataview queries** that previously filtered by folder are now filtering by `source-kind:`.
4. **Delete empty `Books/` and `Articles/` subfolders.**

### Risk

- Low. Mechanical.

---

## Common pitfalls

- **Forgetting to update saved searches.** Folder-path-dependent searches break on rename.
- **Forgetting templates.** Templates that file output to a specific folder need updating.
- **Forgetting attachments.** Attachment paths can break if the attachment folder is per-note.
- **Trying to do everything in one session.** Migrations spanning thousands of notes should be staged across multiple sessions with verification between.
- **Not updating the home note.** If your home/dashboard MOC links to specific folders, those links can break.
