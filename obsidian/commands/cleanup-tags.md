---
description: Clean up tags end-to-end — verify taxonomy, audit, plan, execute migrations in waves, verify, document

# Project context
type: command
project: skills-library
plugin: obsidian
tags: [type/command, plugin/obsidian]
status: active
---

Use the `obsidian-tag-cleanup-flow` orchestrator-shaped skill to run end-to-end tag work.

This command is for major tag operations — invoke when tag count > 120, > 50% of tags are used 1–2 times, or you're running a major migration like status-tags-to-property.

The flow runs all 6 stages:

1. **Taxonomy check** — `obsidian-tag-taxonomist` verifies a target taxonomy exists; designs one if not
2. **Audit** — `obsidian-tag-hygiene` runs the full census and diffs against target
3. **Plan** — keep / rename / merge / promote / retire decisions, grouped into execution waves
4. **Execute waves**:
   - Wave 1: casing & exact-duplicate merges
   - Wave 2: plural / tense / spelling consolidations
   - Wave 3: synonym consolidations
   - Wave 4: promotions (to property, MOC, or link — may route to `obsidian-frontmatter-schema-designer` or `obsidian-moc-architect` first)
   - Wave 5: retirements
5. **Verify** — saved searches, Dataview queries, templates, dashboards all updated
6. **Document** — update the in-vault Tag Taxonomy note with retired-tag rows

Spot-check between waves. Verify 3–5 affected notes after each wave to catch mistakes early.

Expected duration: 2–4 hours, often across multiple sessions.

Confirm before each wave's execution. Tag retirement is non-reversible; ensure the user has opted in.

If Stage 3's plan reveals 50%+ of tags are "promote to MOC" candidates, pause this flow — the work is actually a MOC build, not a tag cleanup. Switch to `obsidian-moc-architect`.
