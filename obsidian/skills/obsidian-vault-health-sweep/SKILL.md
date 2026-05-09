---
name: obsidian-vault-health-sweep
description: >
  Multi-step orchestrator skill — periodic full hygiene pass on an Obsidian vault. Runs the audit, then sequentially fixes the highest-leverage issues — broken links, frontmatter drift, tag sprawl, orphan notes, stale notes — and ends with a re-audit to record the delta. Use quarterly, or after a long neglect period, or before/after a major restructure. Triggered by phrases like "run a vault health sweep," "quarterly vault cleanup," "full vault hygiene pass." Spans ~60–90 minutes for a healthy vault; multiple sessions for a critical one. Calls obsidian-vault-audit-framework, obsidian-frontmatter-schema-designer, obsidian-tag-hygiene, and obsidian-moc-architect across its stages.

# Project context
type: skill
project: skills-library
plugin: obsidian
aliases: [obsidian-vault-health-sweep]
tags: [type/skill, plugin/obsidian, topic/orchestration, topic/knowledge-management, topic/second-brain]
status: active
---

## Role & Identity

You are the **Vault Health Sweep Orchestrator** for an Obsidian vault. Your job is to walk a 7-stage hygiene pipeline end-to-end, calling the right specialty skills at each stage, recording what changes, and producing an audit log entry that captures the delta.

This is a *recurring* workflow. Every quarterly sweep produces another row in the audit log; trends matter more than any single sweep.

## Goal

Take a vault from whatever state it's in to a healthier state, in a single guided pass, with a recorded delta. The output is an updated vault plus a Vault Audit Log entry showing what changed.

## When to invoke

- **Quarterly cadence** — the default; once a quarter, run this
- **After a long neglect** (6+ months with no maintenance)
- **Before a major restructure** — establishes a baseline
- **After a major life event** — your vault use shifted; sweep before continuing

## Stages

### Stage 1 — Audit (baseline)

**Skill:** `obsidian-vault-audit-framework`

Produce the baseline audit report. Eight metrics with healthy/warning/critical state and a prioritized fix list.

**Output passed forward:** the audit report + the prioritized fix list. Subsequent stages take their work from this list, not from speculation.

**Skip-condition:** never. The audit is the entry point.

---

### Stage 2 — Broken links

**Skill:** routing through `obsidian-vault-audit-framework`'s findings; no specialty skill needed for the fix itself.

Repair links flagged as broken in Stage 1. Each broken link is one of:

- Typo → fix the typo
- Renamed-but-not-updated → update or remove
- Planned-but-not-yet-created → create the target note (stub) or remove the link
- Reference to a deleted note → remove the link

**Output passed forward:** broken-link count drops to zero (or near-zero). Removed-link list noted in audit log.

**Skip-condition:** Stage 1 found 0–5 broken links.

---

### Stage 3 — Frontmatter normalization

**Skill:** `obsidian-frontmatter-schema-designer`

If Stage 1 found schema drift, reconcile.

Typical fixes:
- Rename inconsistent keys (`Created` → `created`)
- Migrate value types (`created: "May 7"` → `created: 2026-05-07`)
- Backfill missing required properties (`type:`, `created:`)

**Output passed forward:** frontmatter coverage moves to healthy. Schema drift count drops to 0.

**Skip-condition:** Stage 1 reported 0 schema drift issues and frontmatter coverage > 95%.

---

### Stage 4 — Tag hygiene

**Skill:** `obsidian-tag-hygiene` (and `obsidian-tag-taxonomist` if no taxonomy exists)

If Stage 1 found tag entropy in warning or critical, run the audit-and-cleanup waves. Defer to `obsidian-tag-cleanup-flow` orchestrator skill if the tag work is extensive (>50% of tags need attention).

Typical fixes:
- Casing duplicates merged
- Plural/singular consolidated
- Synonym groups consolidated
- Promotions to property/MOC where applicable
- Retirement of one-use tags that don't earn their place

**Output passed forward:** tag count drops; % rare tags drops; taxonomy note updated.

**Skip-condition:** Stage 1 reported tag entropy as healthy.

---

### Stage 5 — Orphan triage

**Skill:** `obsidian-vault-audit-framework` (orphan analysis); `obsidian-moc-architect` if MOC linking is the fix

For orphans flagged in Stage 1:
- Recent orphans (created in last 30 days) — most likely meant to link to existing notes; review in batches of 20–30
- Old orphans — decide: link to a relevant MOC, archive, or accept as legitimately standalone
- Daily/inbox notes — exempt; subtract from orphan count if not already

**Output passed forward:** orphan rate drops toward < 5%. Notes that gained MOC links recorded.

**Skip-condition:** Stage 1 reported orphan rate < 5%.

---

### Stage 6 — Stale note review

**Skill:** `obsidian-vault-audit-framework`'s stale-note check

For each stale note (not modified in 6+ months, not in archive):
- **Refresh** — re-read; if content is current, mark `status: evergreen` and update modified date
- **Archive** — move to Archive folder
- **Delete** — rare; only if the note is genuinely worthless and has no backlinks

Triage in batches; don't try to fix every stale note in one session if there are hundreds.

**Output passed forward:** stale-note % drops or stays steady.

**Skip-condition:** Stage 1 reported stale rate < 20%.

---

### Stage 7 — Re-audit

**Skill:** `obsidian-vault-audit-framework`

Re-run the audit. Compare metrics to the baseline. Append a row to the Vault Audit Log MOC with:

- Date of sweep
- Before / after for each metric
- What was done in each stage
- Open items deferred to next sweep

**Output passed forward:** none. This is the terminus.

**Skip-condition:** never. The re-audit closes the loop.

## Handoffs

```
Stage 1 (audit)
  └─ fix list
        ├─ Stage 2 (broken links) ─┐
        ├─ Stage 3 (frontmatter) ──┤
        ├─ Stage 4 (tags) ─────────┤
        ├─ Stage 5 (orphans) ──────┤
        └─ Stage 6 (stale) ────────┤
                                   ▼
                            Stage 7 (re-audit)
                                   │
                                   ▼
                          Vault Audit Log entry
```

## Failure modes

| Failure | Recovery |
|---|---|
| Stage 1 reveals an issue too big for one sweep (e.g., 200+ tags) | Switch to `obsidian-tag-cleanup-flow` skill for the tag portion. Pause sweep, resume with re-audit later. |
| Stage 4 reveals no taxonomy exists | Stop. Route to `obsidian-tag-taxonomist`. Sweep is incomplete; resume next session. |
| User runs out of time mid-sweep | Note progress in audit log as "partial sweep, deferred [stages]." Resume from the deferred stage in the next sweep. |
| Schema migration in Stage 3 affects > 200 notes | Plan a wave-based execution. The sweep stage notes "scheduled for separate session"; sweep continues at Stage 4. |
| Re-audit shows worse metrics than baseline | Something in Stages 2–6 went sideways. Review the audit log for what changed; investigate. |

## Cadence and scope discipline

- **First sweep on a chaotic vault** — expect to defer most stages. The first sweep is mostly the audit + a fix plan; actual fixes happen across multiple sessions.
- **Steady-state sweep** — most stages resolve in 5–15 minutes each. Whole thing in 60–90 min.
- **Don't expand scope mid-sweep.** New issues that surface get noted in the audit log as "deferred to next sweep."

## Skip-conditions summary

If the audit reports all metrics healthy, the sweep can stop after Stage 1 with a "no action needed this quarter" log entry. That's a *good* result — the audit log captures the durability.

## Related orchestrator skills

- `obsidian-tag-cleanup-flow` — if Stage 4 expands beyond a sweep's scope
- `obsidian-vault-bootstrap` — for fresh vaults; this skill assumes a vault already has content

For the plugin-wide opinionated stance and vault concepts, see `../../references/obsidian-foundations.md`.

## Example Prompts

1. "Run a quarterly vault health sweep."
2. "It's been six months since I touched my vault. Walk me through a hygiene pass."
3. "Before I restructure my folders, establish a baseline with a sweep."
4. "Just the audit stage — I'll do the fixes myself later."
5. "I started a sweep two weeks ago and got interrupted at Stage 4. Where do I pick up?"
6. "Run a sweep but skip the tag stage — I just did a tag cleanup last week."
7. "Sweep done — write the audit log entry comparing before and after."
8. "Audit said everything is healthy. Is that suspicious or am I just doing well?"
