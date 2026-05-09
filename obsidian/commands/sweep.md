---
description: Run a full vault health sweep — audit, then sequentially fix broken links, frontmatter, tags, orphans, stale notes, then re-audit

# Project context
type: command
project: skills-library
plugin: obsidian
tags: [type/command, plugin/obsidian]
status: active
---

Use the `obsidian-vault-health-sweep` orchestrator-shaped skill to run a periodic full hygiene pass on the vault.

This is the quarterly cadence command. It runs all 7 stages end-to-end:

1. **Audit (baseline)** — `obsidian-vault-audit-framework` produces the fix plan
2. **Broken links** — repair flagged broken links
3. **Frontmatter normalization** — `obsidian-frontmatter-schema-designer` reconciles schema drift
4. **Tag hygiene** — `obsidian-tag-hygiene` audits and cleans (defer to `obsidian-tag-cleanup-flow` if extensive)
5. **Orphan triage** — review orphans flagged in Stage 1; link to MOCs or accept as standalone
6. **Stale note review** — refresh, archive, or mark evergreen
7. **Re-audit** — compare to baseline; append to Vault Audit Log

Each stage has skip-conditions (defined in the skill). Stages 2–6 can skip if Stage 1 reported the relevant metric as healthy.

Expected duration: 60–90 minutes for a healthy vault; multiple sessions for a critical one.

Don't expand scope mid-sweep. Issues that surface that aren't covered by the standard stages get noted in the audit log as "deferred to next sweep."

If Stage 4 reveals tag work too big for one stage (e.g., > 50% of tags need attention), pause the sweep and switch to the `obsidian-tag-cleanup-flow` orchestrator. Resume Stage 5 of the sweep after.

Confirm before any destructive operations (tag retirements, frontmatter migrations affecting > 50 notes, etc.).
