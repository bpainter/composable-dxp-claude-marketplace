# Health Metrics

Definitions, rationales, and thresholds for the eight vault health metrics.

## 1 — Frontmatter coverage

### Definition

Percentage of notes that have the universal required properties (`type:` and `created:`).

### Why it matters

`type:` drives schema, template selection, and queries. Without it, the vault can't be queried by type, can't apply per-type schemas, and can't generate per-type dashboards. `created:` enables time-based queries and stale-note detection.

### Calculation

```
coverage = count(notes with `type:` and `created:`) / count(all notes)
```

### Thresholds

- **Healthy: 95%+** — Daily notes and inbox notes may legitimately not have `type:` (because they're typed by folder); 95% is realistic.
- **Warning: 70–94%** — Backfill is plausible in a few sessions.
- **Critical: < 70%** — Schema is aspirational, not actual. Either schema is too new or it's not being enforced. Templates aren't doing their job.

### Improvement actions

- Update templates so all new notes get the universals
- Wave-based backfill of legacy notes
- Watch for trends: new notes after the schema was introduced should be at 100%

---

## 2 — Type distribution

### Definition

Number of notes per distinct `type:` value, plus list of types in use.

### Why it matters

The shape of the type distribution tells you whether the schema fits the vault:

- All notes one type → over-flat schema; probably missing types
- Many types with 1–2 notes → over-granular schema; types not earning their keep
- Types in the wild not in the schema doc → drift

### Calculation

Group by `type:`, count, sort descending.

### Thresholds

- **Healthy** — Every type in use has 5+ notes; no undocumented types
- **Warning** — Some types with 1–4 notes; or 1–2 undocumented types
- **Critical** — Many types with 0–1 notes; or several undocumented types

### Improvement actions

- For under-used types, decide: retire from schema or grow
- For undocumented types: document or rename to a known type

---

## 3 — Schema drift

### Definition

Inconsistencies in property usage:

- Different keys for the same concept (`author` vs `source-author`)
- Same key with different value types
- Same key with different semantics on different note types

### Why it matters

Drift breaks queries. Once `created` is sometimes a date and sometimes a string, no Dataview query that depends on `created` is reliable.

### Calculation

Manual scan via Properties view + sample queries that test type consistency.

### Thresholds

- **Healthy: 0 inconsistencies**
- **Warning: 1–3 inconsistencies**
- **Critical: 4+ inconsistencies**

### Improvement actions

- Reconcile keys (rename one or merge)
- Migrate values to consistent type
- Update schema doc to be the source of truth

---

## 4 — Tag entropy

### Definition

Combination of:

- Total tag count
- % of tags used 1–2 times (low-frequency)
- Maximum hierarchy depth

### Why it matters

Tag entropy is a leading indicator of organizational discipline. High entropy means tags are being created ad-hoc, which usually correlates with other organizational issues.

### Thresholds

| Metric                | Healthy | Warning | Critical |
| --------------------- | ------- | ------- | -------- |
| Total tag count       | < 60    | 60–120  | 120+     |
| % used 1–2 times      | < 25%   | 25–50%  | 50%+     |
| Max hierarchy depth   | ≤ 1     | 2       | 3+       |

A vault can be in different states on different sub-metrics. Aggregate state = worst sub-metric state.

### Improvement actions

- High count: tag hygiene + retirement
- High % rare: stop adding new tags ad-hoc; require taxonomy review
- High depth: flatten via tag-hygiene playbooks

---

## 5 — Link density / orphan rate

### Definition

Orphan rate = % of notes with zero outgoing AND zero incoming links.

### Why it matters

Orphans can't be discovered through the graph or backlinks. They're only findable through search or folder browsing — both of which assume you remember the note exists.

### Thresholds

| State    | Orphan rate |
| -------- | ----------- |
| Healthy  | < 5%        |
| Warning  | 5–15%       |
| Critical | > 15%       |

### Caveats

- Daily notes often have no incoming links (other than from MOCs); subtract them or measure them separately
- Inbox and archive notes are exempt
- A small population of legitimate orphans (one-off references) is fine

### Improvement actions

- Sort orphans by date; recent orphans likely meant to link to existing notes
- Triage in batches of 20–30
- For genuine orphans that are still useful: link to a relevant MOC, even minimally

---

## 6 — Broken links

### Definition

Count of `[[wikilinks]]` that don't resolve to an existing note.

### Why it matters

Broken links are usually one of:

- Typos (a real issue)
- Renamed-but-not-updated (a process issue)
- Planned-but-not-created notes (a deferred issue)
- References to deleted notes (cleanup not finished)

### Thresholds

| State    | Count   |
| -------- | ------- |
| Healthy  | 0–5     |
| Warning  | 6–25    |
| Critical | 25+     |

### Improvement actions

- Verify "Update Links on Rename" is enabled
- Fix typos
- Create or remove planned-link targets
- Remove references to deleted notes (or restore the targets)

---

## 7 — Stale notes

### Definition

% of non-archived notes whose `mtime` (last modified) is more than 6 months ago.

### Why it matters

Some stale is fine — evergreen notes are *supposed* to be stable. But high stale rates indicate notes that no longer reflect current thinking and may mislead.

### Thresholds

| State    | % stale       |
| -------- | ------------- |
| Healthy  | < 20%         |
| Warning  | 20–40%        |
| Critical | > 40%         |

Subtract `status: evergreen` and `status: archive` notes from the stale count — they're stable on purpose.

### Improvement actions

- Triage stalest 50 first
- For each: refresh, archive, or mark evergreen (if it's correct and stable)
- Don't aim for 0% — that's churning for its own sake

---

## 8 — Folder discipline

### Definition

Combination of:

- Max folder depth
- Number of empty (or under-5-note) folders
- Number of topic-shaped folders (folders that look like topics rather than note types or life areas)

### Why it matters

Deep folders, empty folders, and topic folders all indicate that folders are being asked to do work that links and MOCs should do.

### Thresholds

| Metric              | Healthy | Warning | Critical |
| ------------------- | ------- | ------- | -------- |
| Max depth           | ≤ 2     | 3       | 4+       |
| Empty/under-5       | 0       | 1–3     | 4+       |
| Topic folders       | 0       | 1–3     | 4+       |

### Improvement actions

- Flatten depth via folder-architect migrations
- Merge or delete under-populated folders
- Convert topic folders to MOCs

---

## Aggregate health rating

For a single overall rating, take the worst category:

- All Healthy → **Healthy**
- Any Warning, no Critical → **Warning**
- Any Critical → **Critical**

This is intentionally pessimistic. A vault with one critical issue *is* in critical state for that aspect even if everything else is fine.

## Recording metrics over time

Keep a `Vault Audit Log.md` MOC. After each audit, append a date-stamped row to a table:

```markdown
| Date       | Notes | FM cov | Types | Drift | Tags | Orphan | Broken | Stale | Folders | Overall |
| ---------- | ----- | ------ | ----- | ----- | ---- | ------ | ------ | ----- | ------- | ------- |
| 2026-02-01 | 1,637 | 85%    | OK    | 4     | 78   | 12%    | 18     | 19%   | OK      | Warning |
| 2026-05-07 | 1,847 | 91%    | OK    | 2     | 47   | 8%     | 12     | 23%   | OK      | Warning |
```

Trends matter more than single readings. A vault going from Warning-trending-better is in better shape than Healthy-trending-worse.

## What metrics do NOT tell you

These metrics are necessary but not sufficient. They don't measure:

- Whether the *content* of the vault is good
- Whether the user actually uses the vault for thinking
- Whether the MOCs are insightful or just lists
- Whether the user's tags reflect a clear mental model

For those, you need user judgment, not metrics. The metrics are a hygiene floor.
