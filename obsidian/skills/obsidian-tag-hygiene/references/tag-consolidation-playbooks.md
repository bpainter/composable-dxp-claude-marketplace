# Tag Consolidation Playbooks

Step-by-step playbooks for the most common tag cleanups. Each playbook assumes the user has a backup or version control on the vault.

## Playbook 1 — Casing duplicate (`#Tag` and `#tag`)

**Symptom:** Two tags differing only in case both appear in the Tag Pane.

**Decision:** Lowercase wins. Always.

**Steps:**

1. Search `tag:#Tag` (the uppercase variant). Note the count.
2. Use a bulk rename tool (Bulk Rename plugin, or careful Search-and-Replace) to replace `#Tag` with `#tag` across all notes.
3. Verify the uppercase version is gone from the Tag Pane.
4. Update taxonomy note: add a row to the Retired section.

**Risk:** Very low. Casing-only renames don't change meaning.

---

## Playbook 2 — Plural/singular duplicate (`#book` and `#books`)

**Decision:** Singular wins (default). Document the rule.

**Steps:**

1. Count uses of each variant. Verify both genuinely refer to the same thing — sometimes `#books` is being used for "notes about books in general" (a MOC pattern) while `#book` is "notes about *a* book."
2. If they're the same concept: rename plural → singular.
3. If they're different concepts: pick clearer names (e.g., `#book` for individual book notes, `[[Books MOC]]` for the category note).
4. Verify Tag Pane.
5. Update taxonomy note.

**Risk:** Low–medium. Watch for the genuine "category vs. instance" case where the plural is doing different work.

---

## Playbook 3 — Synonym group (`#todo`, `#task`, `#action`, `#to-do`)

**Decision:** Pick the canonical winner based on:

- Which is most-used (least migration work)
- Which fits naming conventions best (lowercase, hyphenated, singular)
- Which is the most natural sentence-fit ("this note is a #todo")

**Steps:**

1. List all synonyms with counts.
2. Pick the winner. Document reasoning in the taxonomy note.
3. Migrate one synonym at a time, not all at once — easier to spot mistakes.
4. After each migration, spot-check 5 notes to confirm the tag still makes sense (sometimes synonyms had subtly different meanings).
5. Update the taxonomy note as you go.

**Risk:** Medium. Synonyms often have small semantic differences that surface only on close reading.

---

## Playbook 4 — Status tags → `status:` property

**Symptom:** `#draft`, `#evergreen`, `#stale`, `#archive` exist as tags. The user is ready to commit to a `status:` property.

**Steps:**

1. Define the `status:` property schema first (defer to `obsidian-frontmatter-schema-designer`). Decide on the canonical values: e.g., `draft`, `refined`, `evergreen`, `stale`, `archive`.
2. Update the relevant note templates so new notes get `status:` by default.
3. Migrate existing notes one tag at a time:
   - For all notes with `#draft`: add `status: draft` to frontmatter, remove `#draft` tag
   - Repeat for each value
4. Use Dataview to verify: `LIST WHERE status` should match the previous count of status-tagged notes.
5. Retire all status tags. Update taxonomy note.

**Risk:** Medium. Migration is mechanical but high-volume. Do in a session, not piecemeal.

**Common pitfall:** Notes that had multiple status tags simultaneously (`#draft` + `#stale`). Decide a precedence rule before migrating: "stale wins over draft," or whatever fits.

---

## Playbook 5 — Topic tags → MOCs

**Symptom:** `#zettelkasten`, `#para`, `#productivity` exist as tags with 5+ notes each.

**Steps:**

1. For each topic tag, decide: does it deserve a MOC? (Threshold: 5+ notes, expected to grow.)
2. Defer to `obsidian-moc-architect` for MOC design. Each MOC starts as a note titled e.g., `[[Zettelkasten MOC]]` with `type: moc` and a curated list of links to the relevant notes.
3. For each note that had `#zettelkasten`: add a `[[Zettelkasten MOC]]` link in the note body (typically near the top or in a "See also" section).
4. Once all relevant notes link to the MOC: retire the `#zettelkasten` tag.
5. The MOC's backlinks pane will show the relationships that the tag formerly indicated.

**Risk:** Medium. Forgetting to add the MOC link to a note before retiring the tag means the connection is lost.

**Verification:** Backlinks count on the MOC should equal (or exceed, due to mentions in body text) the original tag count.

---

## Playbook 6 — Person tags → person notes

**Symptom:** `#jane-doe`, `#bob-smith` exist as tags.

**Steps:**

1. For each person tag with 3+ uses: create a `[[Person Name]]` note with `type: person` and any context you have (role, relationship, notes about them).
2. For each note that had the person tag: replace the tag with a `[[Person Name]]` link, ideally inline where they're mentioned.
3. Retire the person tag.
4. The person note's backlinks become the index.

**Risk:** Low. The migration is transparent because backlinks pick up automatically as soon as the link exists.

---

## Playbook 7 — Project tags → project notes

**Symptom:** `#website-redesign`, `#book-launch` exist as tags.

**Steps:**

1. For each project tag: create a `[[Project Name]]` note with `type: project`, status, dates, links to deliverables.
2. For each note that had the project tag: link to `[[Project Name]]`.
3. Retire the project tag.

**Risk:** Low–medium. Take the chance to also add `status:` and `start:`/`end:` properties to the new project notes — projects benefit from structured properties.

---

## Playbook 8 — Hierarchy collision (`#draft` and `#status/draft`)

**Symptom:** Both forms exist; some notes have one, some the other, some both.

**Decision:** If using a hierarchy, the `#status/draft` form wins. If not using hierarchies, the bare `#draft` wins.

**Steps:**

1. Pick the winner based on the established convention.
2. Migrate the loser to the winner.
3. Decide if the parent (`#status/`) is doing real work or if you should drop the hierarchy entirely.

**Risk:** Low. Mechanical.

---

## Playbook 9 — Mass retirement of one-use tags

**Symptom:** Tag Pane shows 80+ tags used exactly once.

**Steps:**

1. Sort Tag Pane by count ascending.
2. For each one-use tag, ask: does the note that has this tag *need* it? If yes, you should be able to articulate the query the tag answers.
3. For tags that don't earn their place: open the note and remove the tag. Don't migrate it; just delete.
4. For tags that *might* earn their place but only have one use: park them in a "Watch list" section of the taxonomy note. Re-evaluate next audit. If still unused after 60 days, retire.

**Risk:** Low. Removing a tag from a note never breaks anything that wasn't already broken — you couldn't query for it usefully anyway.

---

## Playbook 10 — Migrating from chaotic tags to a fresh taxonomy

**Symptom:** The user has 200+ tags, no taxonomy, and wants a reset.

**Steps:**

1. Route to `obsidian-tag-taxonomist` first. Don't audit until there's a target taxonomy.
2. With the v1 taxonomy in hand, classify *current* tags into:
   - **Already in target** — keep, normalize naming if needed
   - **Maps to target with rename** — rename
   - **Should be a property** — defer to property migration playbook (4)
   - **Should be a MOC** — defer to MOC migration playbook (5)
   - **Should be a link to a note** — defer to person/project playbooks (6, 7)
   - **Doesn't earn its place** — retire
3. Execute in waves (see audit checklist Phase 5).
4. Plan for the audit to take a session of 1–3 hours — bigger jobs should be broken across multiple sessions.

**Risk:** High effort, but the risk is to *time*, not to data, as long as backups exist.

---

## Common pitfalls across all playbooks

- **Forgetting saved searches.** Saved searches and Dataview queries that referenced renamed/retired tags need updating. Check at the end of every cleanup.
- **Forgetting templates.** Templates that auto-add tags will keep adding the retired ones. Update templates *before* retiring.
- **No taxonomy update.** Every retirement and rename is a row in the Retired section of the taxonomy note. Skip this and the next audit re-discovers all the same patterns.
- **Big-bang cleanups.** Tempting but risky. Wave-based execution with verification between waves catches mistakes early.
- **Cleaning the wrong layer.** If a tag is being misused, sometimes the fix is to introduce a property, not to rename the tag. Defer to `obsidian-property-vs-tag-vs-link` if uncertain.
