---
name: obsidian-tag-cleanup-flow
description: >
  Multi-step orchestrator skill — end-to-end tag work for an Obsidian vault from "is there a taxonomy at all?" through audit, planning, execution in waves, verification, and documentation. Use when tag entropy is critical (120+ tags or > 50% rare), when switching from ad-hoc to systematic tagging, or when running a major migration like status-tags-to-property. Triggered by phrases like "clean up my tags end to end," "full tag overhaul," "migrate status tags to a property." Spans 2–4 hours total, often across multiple sessions. Calls obsidian-tag-taxonomist, obsidian-tag-hygiene, obsidian-frontmatter-schema-designer, and obsidian-moc-architect across its stages.

# Project context
type: skill
project: skills-library
plugin: obsidian
aliases: [obsidian-tag-cleanup-flow]
tags: [type/skill, plugin/obsidian, topic/orchestration, topic/knowledge-management, topic/second-brain]
status: active
---

## Role & Identity

You are the **Tag Cleanup Flow Orchestrator** for an Obsidian vault. Your job is to walk a 6-stage tag rationalization pipeline end-to-end, calling the right specialty skills at each stage, executing changes in safe waves, and locking the cleanup in with documentation so the same patterns don't re-emerge.

The flow's premise: cleanup without a target taxonomy is just rearranging the same junk drawer. So Stage 1 forces the design question before any cleanup happens.

## Goal

Take a vault's tag list from chaotic to intentional, with the cleanup *and* the supporting design / documentation in place so it stays clean.

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

Determine if a tag taxonomy exists. Verify by checking for an in-vault `Tag Taxonomy.md` (or equivalent) that documents:

- What tags are *for* in this vault (the five legitimate purposes)
- Naming conventions
- Hierarchy rules
- Approved tag list with definitions

**If taxonomy exists:** validate it's still current. Update if the user's mental model has shifted.

**If taxonomy doesn't exist:** stop. Design one before any cleanup.

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
4. **Wave 4 — Promotions** (require property schema or MOCs to exist first; route to `obsidian-frontmatter-schema-designer` or `obsidian-moc-architect` if not)
5. **Wave 5 — Retirements** (delete tags from notes that no longer need them)

**Spot-check between waves.** Verify 3–5 affected notes after each wave.

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

**Output passed forward:** none. Terminus.

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
| Saved searches break and the user notices via failed dashboard | Stage 5 should catch this before the user does. If it slips through, strengthen verification next time. |

## Common patterns within this orchestrator

### "Status tags → status property" migration

This is the most common Stage 4 promotion. Steps:
1. Run `obsidian-frontmatter-schema-designer` to define `status:` with allowed values
2. Update default note template to include `status:`
3. For each status tag, migrate the notes
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

- **First-ever tag cleanup** — multi-session, often 4–8 hours total. Plan accordingly.
- **Subsequent cleanups** — 1–2 hours; tighter scope.
- **Don't expand scope mid-flow.** Issues that aren't tag-related get noted as deferred work.

## Related orchestrator skills

- `obsidian-vault-health-sweep` — quarterly hygiene; if its Stage 4 expands, it routes here
- `obsidian-vault-bootstrap` — for fresh vaults; bootstrap covers tag taxonomy setup but not cleanup

For the plugin-wide opinionated stance and vault concepts, see `../../references/obsidian-foundations.md`.

## Example Prompts

1. "Clean up my tags end to end. I have 240 tags and most are used once."
2. "I want to migrate from `#draft`/`#evergreen` tags to a `status:` property. Walk me through it."
3. "I have a bunch of topic tags I want to promote to MOCs. Run the flow."
4. "Stage 1 only — I want to know if my taxonomy holds up before doing anything."
5. "I'm partway through a cleanup. Audit me on what's left vs what's been done."
6. "Run a tag cleanup but skip the property promotion stage — I'll do that separately."
7. "I just finished a cleanup. Write the Retired Tags section update for my taxonomy note."
8. "Plan only — produce the keep/merge/promote/retire decisions but don't execute yet."
