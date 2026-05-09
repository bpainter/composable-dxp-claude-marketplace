# Audit Checklist

Step-by-step checklist for a full vault audit, with specific Obsidian-native commands for each check.

## Prep

- [ ] Backup or commit the vault
- [ ] Note vault size: total notes (run `path: .md` search; count)
- [ ] Note vault age (date of oldest note)
- [ ] Open or create `Vault Audit Log.md` (or your equivalent)

## Check 1 — Frontmatter coverage

### What to measure

Percentage of notes with the universal required properties (`type:` and `created:`).

### How

**Search:**
- `["type"]` → count of notes with any `type:` property
- `["created"]` → count of notes with any `created:` property

**Dataview:**
```dataview
TABLE WITHOUT ID
length(rows) as "count"
FROM ""
WHERE type
```

Compare counts to total notes.

### Threshold

| State    | Coverage |
| -------- | -------- |
| Healthy  | 95%+     |
| Warning  | 70–94%   |
| Critical | < 70%    |

### Action if not healthy

Route to `obsidian-frontmatter-schema-designer` to define/refine schema, then plan a wave-based backfill.

---

## Check 2 — Type distribution

### What to measure

Count of notes per `type:` value. Identifies under-used types (1–4 notes — probably noise) and missing types (no `type:` set).

### How

**Dataview:**
```dataview
TABLE length(rows) as "count"
FROM ""
GROUP BY type
SORT count DESC
```

### Threshold

| State    | Distribution                                |
| -------- | ------------------------------------------- |
| Healthy  | Every type has 5+ notes                     |
| Warning  | Some types with 1–4 notes                   |
| Critical | Many types with 0–1 notes (means schema drift) |

### Action

- Types with 0–4 notes: are they real? Either grow them or retire from schema.
- Types not in the schema doc but appearing: schema drift; document or retire.

---

## Check 3 — Schema drift

### What to measure

Properties that are used inconsistently across notes. Common patterns:

- Different keys for the same concept (`created`, `Created`, `date-created`)
- Same key with different value types (`created: 2026-05-07` vs `created: "May 7"`)
- Properties used on note types that shouldn't have them

### How

**Open Properties view** (Settings → Core plugins → Properties view).
- Sort by name; spot duplicates with different casing
- Click each property; see usage count and value samples

**Dataview:**
```dataview
TABLE length(rows) as "count"
FROM ""
WHERE created
GROUP BY typeof(created)
```
(Reveals if `created` is used as both date and text.)

### Threshold

| State    | Inconsistencies |
| -------- | --------------- |
| Healthy  | 0               |
| Warning  | 1–3             |
| Critical | 4+              |

### Action

Route to `obsidian-frontmatter-schema-designer` for schema reconciliation.

---

## Check 4 — Tag entropy

### What to measure

- Total tag count
- % of tags used 1–2 times
- Hierarchy depth distribution

### How

**Tag pane**:
- Total count visible at top
- Sort by count ascending; count tags with 1–2 uses
- Scan for slashes; identify hierarchy depth

### Threshold

| State    | Total tags     | % rare (1–2 uses) |
| -------- | -------------- | ----------------- |
| Healthy  | < 60           | < 25%             |
| Warning  | 60–120         | 25–50%            |
| Critical | 120+           | 50%+              |

### Action

Route to `obsidian-tag-hygiene` for cleanup; if no taxonomy exists, route to `obsidian-tag-taxonomist` first.

---

## Check 5 — Link density and orphan rate

### What to measure

- Orphan rate: % of notes with zero outgoing *and* zero incoming links
- Average outgoing links per note (rough)

### How

**Settings → Plugins → Outgoing Links** (core)
**Settings → Core Plugins → Backlinks**
Use both pane to identify orphans.

**Search**:
- `[file:]` then sort to find no-link notes (limited)

**Dataview:**
```dataview
LIST length(file.outlinks)
FROM ""
WHERE length(file.outlinks) = 0 AND length(file.inlinks) = 0
```

This lists fully-orphaned notes. Count them. Divide by total.

### Threshold

| State    | Orphan rate |
| -------- | ----------- |
| Healthy  | < 5%        |
| Warning  | 5–15%       |
| Critical | > 15%       |

Daily notes are often legitimately orphan; subtract them or run the query excluding `type: daily`.

### Action

- High orphan rate often means atomic notes that were meant to link aren't. Triage in batches.
- Some orphans are by design (inbox, archive). Don't chase 0%.

---

## Check 6 — Broken links

### What to measure

Number of unresolved `[[wikilinks]]`. Each is either a typo, a planned-but-not-yet-created note, or a reference to a deleted note.

### How

**Built-in:**
- Settings → Editor → Linter (community plugin) or rely on yellow underline on broken links
- **Search:** Some plugins offer "list broken links" — Various Complements, Janitor

**Dataview:**
```dataview
LIST file.outlinks
FROM ""
WHERE length(filter(file.outlinks, (l) => !l.path)) > 0
```
(Approximation; behavior varies by Dataview version.)

### Threshold

| State    | Count   |
| -------- | ------- |
| Healthy  | 0–5     |
| Warning  | 6–25    |
| Critical | 25+     |

Some broken links are intentional (planned future notes). Subtract those if you can identify them.

### Action

- Fix typos
- Create planned notes if they should exist
- Remove links to deleted notes
- For frequent typos, add aliases on the target

---

## Check 7 — Stale notes

### What to measure

% of non-archived notes untouched in 6+ months.

### How

**Dataview:**
```dataview
LIST file.mtime
FROM "" AND !"Archive"
WHERE file.mtime < date(today) - dur(6 months)
SORT file.mtime ASC
```

### Threshold

| State    | % stale       |
| -------- | ------------- |
| Healthy  | < 20%         |
| Warning  | 20–40%        |
| Critical | > 40%         |

Reference notes and evergreen MOCs are *supposed* to be stable; don't count them as stale. Filter by type or status.

### Action

- For each stale note: refresh, archive, or leave (if it's evergreen and still correct)
- Don't aim for 0% stale — some notes are durable and shouldn't be touched

---

## Check 8 — Folder discipline

### What to measure

- Max folder depth
- Folders with < 5 notes
- Folders that look like topics (rather than note types or life areas)

### How

**File explorer:**
- Visual scan; depth is countable
- Note count per folder (some plugins show this; otherwise click each)

### Threshold

| State    | Depth | Empty folders | Topic folders |
| -------- | ----- | ------------- | ------------- |
| Healthy  | ≤ 2   | 0             | 0             |
| Warning  | 3     | 1–3           | 1–3           |
| Critical | 4+    | 4+            | 4+            |

### Action

Route to `obsidian-folder-architect` for restructure; route to `obsidian-moc-architect` for replacing topic folders.

---

## After-audit synthesis

For each check, record:

- Current value
- Health state (healthy / warning / critical)
- Trend vs. last audit (better / same / worse)
- Action item if any

### Sample audit log entry

```markdown
# Vault Audit — 2026-05-07

| Check                | Value       | State    | Trend   | Action |
|----------------------|-------------|----------|---------|--------|
| Total notes          | 1,847       | —        | +210    | —      |
| Frontmatter coverage | 91%         | Warning  | +6 pp   | Backfill 100 notes missing `type:` |
| Type distribution    | 6 types, all 5+ | Healthy | same    | —      |
| Schema drift         | 2 issues    | Warning  | -1      | Reconcile `source-author` (str vs list) |
| Tag entropy          | 47 tags, 18% rare | Healthy | -3 tags | —    |
| Link density         | 8% orphan   | Warning  | -2 pp   | Triage 30 atomic-note orphans |
| Broken links         | 12          | Warning  | +4      | Fix typos and plan-or-prune |
| Stale notes          | 23%         | Warning  | +1 pp   | Decide on 50 stalest notes  |
| Folder discipline    | depth 2, 0 empty, 0 topic | Healthy | same | —    |

## Top 3 actions for next 90 minutes

1. Fix 12 broken links
2. Reconcile `source-author` schema drift (route to frontmatter-schema-designer)
3. Triage atomic-note orphans (start with most-recent 30)

## Top action for next quarter

Backfill `type:` on 100 legacy notes. Wave-based. Plan in obsidian-frontmatter-schema-designer.
```

This entry takes 5 minutes to fill in once you have the metrics; it's the single most valuable record of vault evolution.
