---
description: Run a vault audit — eight metrics across frontmatter, tags, links, orphans, stale notes, folder discipline, with a prioritized fix plan

# Project context
type: command
project: skills-library
plugin: obsidian
tags: [type/command, plugin/obsidian]
status: active
---

Use the `obsidian-vault-audit-framework` skill to run a periodic vault health audit.

Steps:

1. Take a baseline snapshot: total notes, total tags, total backlinks, top-level folder counts.
2. Run the eight health checks from `obsidian-vault-audit-framework/references/audit-checklist.md`:
   - Frontmatter coverage (% with `type:` and `created:`)
   - Type distribution (notes per `type:` value; identify under-used types)
   - Schema drift (different keys for same concept; type mismatches)
   - Tag entropy (total count, % rare, hierarchy depth)
   - Link density / orphan rate (% of notes with no in/out links)
   - Broken links count
   - Stale notes (% untouched in 6+ months, not archived)
   - Folder discipline (depth, empty folders, topic-shaped folders)
3. For each metric, score healthy / warning / critical against the thresholds in `obsidian-vault-audit-framework/references/health-metrics.md`.
4. Compare against the previous audit log entry if one exists. Surface trends.
5. Produce a prioritized fix plan, ordered by leverage (highest-leverage first):
   - Foundational: type/schema/taxonomy if missing
   - High signal-to-noise: broken links, tag synonyms
   - Structural: folders, MOCs
   - Defer: stale-note review unless growing fast
6. Append a row to the Vault Audit Log MOC (or create one if missing) with date, metrics, fix plan, and any notes.
7. Wait for user approval per fix-plan item before handing off to specialty skills.

Don't execute fixes during the audit itself — produce the plan, then route per-item.
