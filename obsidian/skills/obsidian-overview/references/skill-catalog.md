# Skill Catalog

Detailed reference for every skill and orchestrator in the obsidian plugin. Use this when the SKILL.md's one-line summary isn't enough.

## Skills

### `obsidian-tag-taxonomist`

**Purpose:** Design the tag system from first principles — what tags are *for*, naming conventions, hierarchy rules, entry/exit criteria.

**When to invoke:**
- Starting a new vault
- Reining in an ad-hoc tag mess by deciding what the system should look like
- Adding a new tag and wanting to know if it fits

**Output:** Tag purpose statement, approved tag list, naming rules, hierarchy decision, entry/exit criteria, draft of the in-vault Tag Taxonomy note.

**Pairs with:**
- `obsidian-tag-hygiene` — taxonomist outputs become hygiene's input
- `obsidian-property-vs-tag-vs-link` — for decisions about whether a candidate is really a tag

**Don't invoke when:** the work is execution (cleaning up an existing list); use `obsidian-tag-hygiene`.

---

### `obsidian-tag-hygiene`

**Purpose:** Audit and clean up an existing sprawling tag list. Triage by frequency, detect synonyms, plan promotions and retirements.

**When to invoke:**
- Tag count > 60 or > 25% used 1–2 times
- Switching from ad-hoc to systematic tagging
- Periodic monthly tag audit

**Output:** Tag census, keep/merge/promote/retire decisions, sequenced execution plan, taxonomy note updates.

**Pairs with:**
- `obsidian-tag-taxonomist` — hygiene routes to taxonomist if no taxonomy exists
- `obsidian-frontmatter-schema-designer` — for tag→property promotions
- `obsidian-moc-architect` — for tag→MOC promotions

**Don't invoke when:** there's no existing taxonomy yet; design first with the taxonomist.

---

### `obsidian-frontmatter-schema-designer`

**Purpose:** Design Properties (frontmatter) schemas per note type with type discipline (date, list, number, checkbox, constrained text).

**When to invoke:**
- Introducing properties for the first time
- Frontmatter has gone freelance — every note has different keys
- Designing templates that enforce schemas
- Preparing a migration from tag→property (e.g., status tags → status property)

**Output:** Note-type list, per-type schemas (keys, types, required/optional, allowed values), in-vault Frontmatter Schema note, template recommendations.

**Pairs with:**
- `obsidian-property-vs-tag-vs-link` — for individual field-routing decisions
- `obsidian-tag-hygiene` — schema must exist before tag→property migrations

**Don't invoke when:** the user just wants to add one field; route through `obsidian-property-vs-tag-vs-link` first.

---

### `obsidian-property-vs-tag-vs-link`

**Purpose:** Decision framework — should this metadata be a property, tag, link, or folder?

**When to invoke:**
- Any "should this be X or Y?" question about metadata mechanisms
- Auditing existing structure for "wrong layer" mismatches
- Adding a new piece of metadata and wanting it routed correctly

**Output:** Single answer for each candidate (property/tag/link/folder), with reasoning and migration path if currently in the wrong layer.

**Pairs with everything else:** this is the most-invoked skill. Other skills defer to it for individual routing decisions.

**Don't invoke when:** the user wants the *full* schema or taxonomy designed; route to `obsidian-frontmatter-schema-designer` or `obsidian-tag-taxonomist`.

---

### `obsidian-folder-architect`

**Purpose:** Design top-level folder structure — by note type, PARA, Johnny Decimal, or flat.

**When to invoke:**
- Starting a new vault
- Folder tree past depth 3 and brittle
- Deciding whether to add a new folder vs. a MOC

**Output:** Top-level shape recommendation, depth rules, folder definitions, migration plan if changing.

**Pairs with:**
- `obsidian-moc-architect` — folders for *where notes live*, MOCs for *what notes are about*
- `obsidian-frontmatter-schema-designer` — note types live in folders

**Don't invoke when:** the user wants topic folders. Push back, route to `obsidian-moc-architect`.

---

### `obsidian-filename-conventions`

**Purpose:** Filename and note-title rules per note type — claim-as-title for atomic notes, ISO dates for daily, Title Case for content, kebab-case for system.

**When to invoke:**
- Establishing conventions for a new vault
- Filenames are inconsistent across the vault
- Designing templates that auto-name new notes
- Disambiguating two notes that want the same name (aliases, parentheticals)

**Output:** Per-type filename rules, casing rules, alias strategy, collision handling, in-vault Filename Conventions note.

**Pairs with:**
- `obsidian-frontmatter-schema-designer` — both feed templates
- `obsidian-folder-architect` — filename conventions complete the structural picture

---

### `obsidian-moc-architect`

**Purpose:** Design Maps of Content — when to spawn one, structure (linear / categorical / question-driven / reading list / hub-spokes), when to split or retire.

**When to invoke:**
- A topic has reached 5+ notes and earns a MOC
- Existing MOC is sprawling (> 50 links) and needs splitting
- Migrating from topic folders or topic tags to MOCs
- Designing the home note as a meta-MOC

**Output:** MOC structure, draft content, parent-MOC hierarchy decision, split/retirement plans.

**Pairs with:**
- `obsidian-folder-architect` — MOCs replace topic folders
- `obsidian-tag-hygiene` — MOCs replace topic tags

**Don't invoke when:** topic has fewer than 5 notes — defer; use links between notes for now.

---

### `obsidian-vault-audit-framework`

**Purpose:** Periodic vault health audit across eight metrics with prioritized fix plan.

**When to invoke:**
- Quarterly or annual vault checkup
- Vault feels off, want a baseline
- Before a major restructure, want a baseline to compare against
- Tracking vault health metrics over time

**Output:** Audit report (8 metrics, healthy/warning/critical), prioritized fix plan, baseline for trend tracking.

**Pairs with everything else:** the audit's fix plan routes to specialty skills.

**Don't invoke when:** the user just wants to fix one specific issue; go straight to the specialty skill.

---

## Orchestrator-shaped skills

These are skills whose body walks a multi-stage workflow. Their filenames follow `-sweep` / `-flow` / `-bootstrap` suffixes by convention. They live in `skills/` like all other skills.

### `obsidian-vault-health-sweep`

**Purpose:** Periodic full hygiene pass. Audit → broken links → frontmatter normalization → tag hygiene → orphan triage → stale review → re-audit.

**When to invoke:**
- Quarterly vault cleanup
- After a long neglect period (6+ months)
- Before/after a major life event that changed how you use the vault

**Stages:** 7 stages, ~60–90 minutes for a healthy vault, multiple sessions for a critical one.

---

### `obsidian-tag-cleanup-flow`

**Purpose:** End-to-end tag work from design to execution. Verify taxonomy → audit → plan → execute waves → verify → document.

**When to invoke:**
- Tag list has reached critical (120+ tags or > 50% rare)
- Switching from ad-hoc to systematic tagging
- Major migration (e.g., status tags → status property)

**Stages:** 6 stages, ~2–4 hours total, often spanning multiple sessions.

---

### `obsidian-vault-bootstrap`

**Purpose:** Setting up a brand-new vault with conventions in place. Folder structure → filename conventions → frontmatter schema → tag taxonomy → templates → starter MOCs → home note → documentation.

**When to invoke:**
- Day 1 of a new vault
- Resetting after a chaotic first attempt
- Setting up a new vault for a specific purpose (research, work, project)

**Stages:** 8 stages, ~2 hours for a focused setup.

---

## Quick decision matrix

If the user's request is...

| Request shape | Reach for |
|---|---|
| Single decision ("should this be X or Y?") | `obsidian-property-vs-tag-vs-link` |
| Single design task ("design my X") | the matching specialty skill |
| Single cleanup ("clean up my X") | the matching specialty skill |
| "How healthy is my vault?" | `obsidian-vault-audit-framework` |
| "Run a full cleanup" | `obsidian-vault-health-sweep` |
| "Clean up my tags end to end" | `obsidian-tag-cleanup-flow` |
| "Set up my new vault" | `obsidian-vault-bootstrap` |
| "What's in this plugin?" | This overview skill (the one you're in) |
