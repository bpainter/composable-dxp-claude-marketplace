# Tag Audit Checklist

A repeatable checklist for auditing the tag list in an Obsidian vault. Use full version when starting a cleanup; use the quick version monthly.

## Quick monthly audit (15 minutes)

- [ ] Open Tag Pane. Note the total tag count and compare to last month.
- [ ] Sort by count ascending. Scan tags with 1–2 uses.
- [ ] For each one-use tag: keep (it answers a query), promote (to property/link/MOC), or retire.
- [ ] Scan for any new tags created since last audit. Confirm each is in the taxonomy note.
- [ ] Check for casing/plural duplicates: are `#Book` and `#book` both present? `#book` and `#books`?
- [ ] Update taxonomy note with any retirements.

## Full audit (60–90 minutes)

### Phase 1 — Inventory

- [ ] Total tag count (record for trend)
- [ ] Distribution by frequency band:
  - 50+ uses (hubs)
  - 10–49 uses (active)
  - 3–9 uses (marginal)
  - 1–2 uses (orphans)
- [ ] Hierarchy depth distribution:
  - Flat (no slashes)
  - One level (`parent/child`)
  - Two+ levels (smell)
- [ ] List tags not present in the taxonomy note (these are unauthorized — review)

### Phase 2 — Synonym and duplicate detection

For each pair below, document the match and the winner:

- [ ] **Casing pairs** — `#Tag` and `#tag`, `#TAG` and `#tag`
- [ ] **Plural/singular pairs** — `#book`/`#books`, `#person`/`#people`
- [ ] **Tense pairs** — `#read`/`#reading`, `#draft`/`#drafting`
- [ ] **Spelling pairs** — `#color`/`#colour`, `#favorite`/`#favourite`
- [ ] **Synonym groups** — `#todo`/`#task`/`#to-do`/`#action`
- [ ] **Hierarchy collisions** — `#draft` and `#status/draft` both present

### Phase 3 — Triage by purpose

For each tag, classify into one of the five legitimate purposes (status, type, lifecycle, facet, lightweight topic). Tags that don't fit are candidates for promotion or retirement.

- [ ] List tags that are genuinely **status** — should they be a property?
- [ ] List tags that are genuinely **note type** — should they be a property?
- [ ] List tags that are **topics** — should they be MOCs?
- [ ] List tags that are **people or projects** — should they be notes?
- [ ] List tags that are **facets** — these usually keep their tag form

### Phase 4 — Decisions

For each tag, record one of:

- **Keep** — fits the taxonomy, name is clean, frequency justifies it
- **Rename** — same purpose, better name (e.g., `#books` → `#book`)
- **Merge** — consolidate into another tag (`#wip` → `#draft`)
- **Promote to property** — it's really a typed attribute
- **Promote to MOC** — it's really a topic
- **Promote to link** — it's really a person/project
- **Retire** — doesn't earn its place, no replacement needed

Document each decision in a temporary working note. Don't act on any of them until the full plan is reviewed.

### Phase 5 — Sequence

Group decisions into execution waves:

1. **Wave 1: Casing and exact-duplicate merges** (low risk)
2. **Wave 2: Plural/tense/spelling consolidations**
3. **Wave 3: Synonym consolidations** (review each note for sense)
4. **Wave 4: Promotions** (require property schema or MOCs to exist first)
5. **Wave 5: Retirements** (delete tags from notes that no longer need them)

### Phase 6 — Execute

- [ ] Wave 1 done; spot-check 3–5 affected notes
- [ ] Wave 2 done; spot-check
- [ ] Wave 3 done; spot-check
- [ ] Wave 4 done; verify properties/MOCs exist and notes link correctly
- [ ] Wave 5 done; final spot-check

### Phase 7 — Verify

- [ ] Saved searches that referenced retired/renamed tags — updated
- [ ] Dataview queries that referenced retired/renamed tags — updated
- [ ] Templates that auto-applied retired/renamed tags — updated
- [ ] Dashboards / home note links — checked
- [ ] Taxonomy note reflects all changes (Approved + Retired sections)

### Phase 8 — Document

- [ ] Date of audit
- [ ] Total tags before/after
- [ ] Notable consolidations (e.g., "Merged 12 status synonyms into a `status:` property")
- [ ] Open questions / deferred decisions

Filing this in a `Vault Maintenance Log` MOC keeps a useful track record.

## Health metrics to track over time

| Metric                                  | Healthy range                    | Notes                                                |
| --------------------------------------- | -------------------------------- | ---------------------------------------------------- |
| Total tags                              | 15–60 for most personal vaults    | Above 100, audit aggressively                        |
| % of tags used 1–2 times                | < 25%                            | Higher means tag-on-impulse without taxonomy review |
| Tags not in taxonomy note               | 0                                | Anything else is unauthorized                        |
| Hierarchy depth max                     | 1                                | Two+ levels = smell                                 |
| Synonym pairs detected this audit       | 0–2                              | More than 2 means naming convention isn't holding  |

## When *not* to audit

- During an active writing sprint — tag noise is fine while ideas are flowing
- Within a week of a major vault restructure — let the dust settle
- If the user hasn't yet established what tags are *for* (route to `obsidian-tag-taxonomist` first)

## Audit cadence recommendation

| Vault age            | Recommended cadence            |
| -------------------- | ------------------------------ |
| First 90 days        | Full audit at day 30, day 90    |
| First year           | Quick monthly + full quarterly |
| 1+ year              | Quick monthly + full annually  |
| Vaults of 2,000+ notes | Quick monthly + full annually   |
