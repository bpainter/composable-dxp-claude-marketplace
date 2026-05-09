---
name: obsidian-tag-cleanup-flow
description: >
  End-to-end tag work for an Obsidian vault — from "is there a taxonomy at all?" through audit, planning, execution in waves, verification, and documentation. Use when tag entropy is critical (120+ tags or > 50% rare), when switching from ad-hoc to systematic tagging, or when running a major migration like status-tags-to-property. Spans 2–4 hours total, often across multiple sessions.
stages: [taxonomy-check, audit, plan, execute-waves, verify, document]
---

# Obsidian Tag Cleanup Flow

## Goal

Take a vault's tag list from chaotic to intentional, with the cleanup *and* the supporting design / documentation in place so it stays clean.

The flow's premise: cleanup without a target taxonomy is just rearranging the same junk drawer. So Stage 1 forces the design question before any cleanup happens.

## When to invoke

- **Tag count > 120**, especially if > 50% of tags are used 1–2 times
- **No tag taxonomy** exists and the user wants to introduce one
- **Major migration** (e.g., status tags → status property, or topic tags → MOCs)
- **First-ever tag cleanup** on a vault that's been growing tags ad-hoc for a year+

## When NOT to invoke

- Light maintenance — use the quick monthly audit in `obsidian-tag-hygiene` instead
- The vault has < 60 tags and clean naming — leave it alone
- The user wants to clean *one specific* synonym pair — go directly to `obsidian-tag-hygiene`

## Stages

### Stage 1 — Taxonomy check

**Skill:** `obsidian-tag-taxonomist`

Determine if a tag taxonomy exists. Verify by checking for an in-vault `Tag Taxonomy.md` (or equivalent) note that documents:

- What tags are *for* in this vault (the five legitimate purposes)
- Naming conventions
- Hierarchy rules
- Approved tag list with definitions

**If taxonomy exists:** validate it's still current. Update if the user's mental model has shifted.

**If taxonomy doesn't exist:** stop. Design one before any cleanup. Returning to clean tags without a taxonomy is wasted effort.

**Output passed forward:** the approved tag list (with naming rules) that subsequent stages clean *toward*.

---

### Stage 2 — Audit

**Skill:** `obsidian-tag-hygiene`

Run the full tag audit:

- Total tag count
- Frequency distribution (hub / active / marginal / orphan tags)
- Hierarchy depth distribution
- Synonym/duplicate detection (casing, plural, tense, spelling, conceptual)
- Comparison against the taxonomy from Stage 1

**Output passed forward:** the tag census and the diff between current state and target state.

---

### Stage 3 — Plan

**Skill:** `obsidian-tag-hygiene`

For each tag, decide one of:

- **Keep** — fits the taxonomy, name is clean, frequency justifies
- **Rename** — same purpose, better name (`#books` → `#book`)
- **Merge** — consolidate into another tag (`#wip` → `#draft`)
- **Promote to property** — it's really a typed attribute (defer to `obsidian-frontmatter-schema-designer`)
- **Promote to MOC** — it's really a topic (defer to `obsidian-moc-architect`)
- **Promote to link** — it's really a person/project (`#jane-doe` → `[[Jane Doe]]`)
- **Retire** — doesn't earn its place, no replacement needed

Write the full plan as a working note before acting on any of it.

**Output passed forward:** the keep/rename/merge/promote/retire plan, grouped into execution waves.

---

### Stage 4 — Execute waves

**Skill:** `obsidian-tag-hygiene` (Playbooks 1–10 depending on what's needed)

Execute the plan in safe-sequence waves:

1. **Wave 1 — Casing & exact-duplicate merges** (lowest risk)
2. **Wave 2 — Plural / tense / spelling consolidations**
3. **Wave 3 — Synonym consolidations** (review each note for sense)
4. **Wave 4 — Promotions** (require property schema or MOCs to exist first; if they don't, route to `obsidian-frontmatter-schema-designer` or `obsidian-moc-architect`)
5. **Wave 5 — Retirements** (delete tags from notes that no longer need them)

**Spot-check between waves.** Verify 3–5 affected notes after each wave to catch mistakes early.

**Output passed forward:** the cleaned tag list. Promoted-to-property and promoted-to-MOC items recorded for verification.

---

### Stage 5 — Verify

**Skill:** ad-hoc; touches `obsidian-vault-audit-framework` to re-run tag entropy check

Verify nothing downstream broke:

- **Saved searches** — any that reference renamed/retired tags need updating
- **Dataview queries** — same
- **Templates** — templates that auto-applied retired tags need updating
- **Dashboards / home note links** — checked
- **Tag Pane** — final state matches the plan

If anything's broken, fix before Stage 6.

**Output passed forward:** confirmation that no downstream artifacts reference the old tags.

---

### Stage 6 — Document

**Skill:** `obsidian-tag-taxonomist`

Update the in-vault Tag Taxonomy note:

- **Approved tags section** — reflects current state
- **Retired tags section** — every retired tag gets a row with date and reason
- **Promoted tags** — note where they went (property name, MOC name, person/project link)
- **Conventions** — confirm naming rules section is still accurate

This step makes the cleanup compound. Without it, the next audit re-discovers the same patterns.

**Output passed forward:** none. This is the terminus.

## Handoffs

```
Stage 1 (taxonomy check) ─→ approved tag list / target state
        │
        ▼
Stage 2 (audit) ─→ census + diff against target
        │
        ▼
Stage 3 (plan) ─→ keep/rename/merge/promote/retire decisions
        │
        ▼
Stage 4 (execute waves) ─→ cleaned tag list
        │  ↑
        │  └─ may route OUT to:
        │     - obsidian-frontmatter-schema-designer (for tag→property promotions)
        │     - obsidian-moc-architect (for tag→MOC promotions)
        ▼
Stage 5 (verify) ─→ downstream artifacts updated
        │
        ▼
Stage 6 (document) ─→ taxonomy note current
```

## Failure modes

| Failure | Recovery |
|---|---|
| Stage 1 reveals no taxonomy exists | Stop. Design taxonomy with `obsidian-tag-taxonomist`. Don't proceed without one. |
| Stage 3 plan reveals 50%+ of tags are "promote to MOC" | Pause this flow. The cleanup is actually a `obsidian-moc-architect`-led MOC build. Tags can wait. |
| Wave 4 promotions need property schema that doesn't exist | Pause Stage 4. Run `obsidian-frontmatter-schema-designer` first. Resume waves once schema is in place. |
| Wave execution touches 500+ notes | Plan multi-session. Don't try to do it in one sitting. Document where you stopped in the working plan. |
| Saved searches break and the user notices via failed dashboard | Stage 5 should catch this before the user does. If it slips through, it's a Stage 5 quality issue — strengthen verification next time. |

## Common patterns within this orchestrator

### "Status tags → status property" migration

This is the most common Stage 4 promotion pattern. Steps:
1. Run `obsidian-frontmatter-schema-designer` to define `status:` with allowed values
2. Update default note template to include `status:`
3. For each status tag (`#draft`, `#evergreen`, etc.), migrate the notes (add property, remove tag)
4. Verify with Dataview that property values match previous tag counts
5. Retire status tags
6. Update taxonomy note

### "Topic tags → MOCs" migration

Second-most common. Steps:
1. For each topic tag with 5+ notes, run `obsidian-moc-architect` to design the MOC
2. Create the MOC note with structure
3. For each note that had the tag, add `[[Topic MOC]]` link in body
4. Verify MOC backlinks count covers original tag count
5. Retire the tag
6. Update taxonomy note

## Cadence and scope discipline

- **First-ever tag cleanup on a chaotic vault** — multi-session, often 4–8 hours total. Plan accordingly.
- **Subsequent cleanups** — 1–2 hours; tighter scope.
- **Don't expand scope mid-flow.** Issues that surface that aren't tag-related (folder problems, schema drift) get noted as deferred work, not folded into this flow.

## Related orchestrators

- `obsidian-vault-health-sweep` — quarterly hygiene; if the sweep's Stage 4 reveals tag work too big for one stage, it routes here
- `obsidian-vault-bootstrap` — for fresh vaults; bootstrap covers tag taxonomy setup but not cleanup
