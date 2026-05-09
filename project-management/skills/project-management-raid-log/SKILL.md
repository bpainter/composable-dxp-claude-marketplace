---
name: project-management-raid-log
description: Create and maintain a project RAID log — Risks, Assumptions, Issues, Dependencies — as an Excel workbook with the right columns, severity scoring, owners, status tracking, and a roll-up view that feeds into weekly status. Use this skill any time the user wants a RAID log started, updated, audited, or rolled up, even when they say "track our risks," "we need an issues list," "log our assumptions," "what dependencies are blocking us," or "consolidate our risks for the steerco." Trigger whenever risk/issue/dependency tracking is being created or maintained so the team has one canonical source instead of five tabs in five tools.

# Project context
type: skill
project: skills-library
plugin: project-management
aliases: [project-management-raid-log]
tags: [type/skill, plugin/project-management topic/project-management, topic/agile]
status: active
---

# RAID Log

A RAID log is the single canonical place for the things that could derail a project. The four columns of the acronym aren't the same kind of thing — distinguishing them is the work.

- **Risk** — *might* happen. Probability × impact. Owned by someone who is mitigating.
- **Assumption** — currently believed true. If it turns out false, the plan changes. Owned by someone who is validating.
- **Issue** — *is* happening. Already real. Owned by someone who is resolving.
- **Dependency** — something the team needs from another team / vendor / process to make progress. Owned by someone who is chasing it.

The skill produces a structured xlsx workbook (because most stakeholders open xlsx, and filtering/sorting matters), and explains how to maintain it across the project lifecycle.

## When to use this skill

- New project kickoff — establish the log.
- Mid-project — add or update entries as new things surface.
- Audit / steerco prep — review aging entries, close stale ones, surface tops to status.
- Conversion — turning a Slack thread or meeting note into proper RAID entries.
- Roll-up — summarizing for `pm-weekly-status`.

For the actual xlsx generation, lean on the `xlsx` skill — this skill defines the schema and rules; the xlsx skill handles file production.

## Workbook structure

One workbook. Four sheets, one per RAID type. Plus a `Summary` sheet that aggregates open items by severity.

```
RAID-Log-{ProjectName}.xlsx
├── Summary       (read-only roll-up, formulas only)
├── Risks
├── Assumptions
├── Issues
└── Dependencies
```

### Risks sheet columns

| Column | Notes |
|---|---|
| ID | `R-001`, sequential. Never reuse. |
| Date raised | yyyy-mm-dd |
| Title | Short, scannable. "Contentful migration may slip Phase 2 launch." |
| Description | 1-3 sentences. What might happen and why. |
| Probability | 1 (rare) – 5 (almost certain) |
| Impact | 1 (negligible) – 5 (severe) |
| Severity | `Probability × Impact` (formula). Color-banded: 1-6 green, 7-14 yellow, 15-25 red. |
| Mitigation | What we're doing to reduce probability or impact. |
| Contingency | What we'll do *if* it happens. |
| Owner | One person. Not "the team." |
| Status | Open / Mitigating / Realized (became an Issue) / Closed (mitigated or expired) |
| Last updated | yyyy-mm-dd |
| Notes | Free text. Date-stamped entries: `2026-04-15: ...` |

### Assumptions sheet columns

| Column | Notes |
|---|---|
| ID | `A-001` |
| Date raised | yyyy-mm-dd |
| Assumption | The thing we're treating as true. "Client will provide content for 12 product explainers by 2026-06-01." |
| Why it matters | What plan element depends on it. |
| Validation method | How we confirm. "Confirmed in writing by client content lead." |
| Validation deadline | yyyy-mm-dd. By when we need to know. |
| Owner | One person. |
| Status | Unvalidated / Validated / Invalidated (became a Risk or Issue) / Closed |
| Last updated | yyyy-mm-dd |
| Notes | Date-stamped entries. |

### Issues sheet columns

| Column | Notes |
|---|---|
| ID | `I-001` |
| Date raised | yyyy-mm-dd |
| Title | "Contentful API rate limit hit during initial bulk import." |
| Description | What is happening and what it's blocking. |
| Impact | 1-5 |
| Urgency | 1-5 |
| Severity | `Impact × Urgency` (formula). Color-banded. |
| Resolution plan | The steps being taken. |
| Owner | One person. |
| Target resolution | yyyy-mm-dd |
| Status | Open / In progress / Blocked / Resolved |
| Last updated | yyyy-mm-dd |
| Notes | Date-stamped entries. |

### Dependencies sheet columns

| Column | Notes |
|---|---|
| ID | `D-001` |
| Date raised | yyyy-mm-dd |
| Dependency | "Client marketing approval on disclaimer wording." |
| Type | Internal Slalom / Client / Third-party vendor / Regulatory |
| Needed by | yyyy-mm-dd |
| Why | What it unblocks. |
| Owner (chasing) | One person on the project team. |
| Counterparty | Who owns it on the other side. |
| Status | Identified / Requested / In progress / Received / Blocked |
| Last updated | yyyy-mm-dd |
| Notes | Date-stamped entries. |

### Summary sheet (read-only)

Pulls counts and tops from the four sheets:

- Open Risks by severity band (count of red / yellow / green).
- Top 5 risks by severity (live formula).
- Open Issues by severity band.
- Top 5 issues by severity.
- Unvalidated Assumptions whose validation deadline is within 14 days.
- Dependencies overdue (Needed by < today, Status ≠ Received).

This sheet is what gets screen-shared in steercos. Keep it free of formatting clutter.

## Severity scoring conventions

Use 1-5 scales to keep math simple. A 5-point scale gives meaningful granularity without false precision.

**Probability (Risks)**:
1. Rare — surprising if it happened.
2. Unlikely — could happen, no current signal.
3. Possible — coin flip / depends on factors not in our control.
4. Likely — current signals suggest it will.
5. Almost certain — would be surprising if it didn't.

**Impact (Risks & Issues)**:
1. Negligible — no schedule, scope, or budget effect.
2. Minor — hours of rework, no stakeholder visibility.
3. Moderate — days of rework or one stakeholder ask.
4. Major — sprint-level slip or material scope cut.
5. Severe — phase or release at risk; executive escalation.

**Urgency (Issues)**:
1. Whenever — no time pressure.
2. This sprint — should be addressed in current sprint.
3. This week — within 5 working days.
4. Today — within 24 hours.
5. Now — work blocked until resolved.

The math: severity = factor1 × factor2. Range 1-25. Color bands:
- 1-6: green
- 7-14: yellow
- 15-25: red

Stakeholders learn this scale week over week — don't change it mid-project.

## Maintenance rhythm

**Daily**: anyone can add an entry. Use Slack channel or PR template that says "raised in RAID as R-NNN."

**Weekly (Thursday before status)**:
- Owner reviews their entries.
- Update Last updated and Notes for any movement.
- Close anything resolved or expired.
- Surface top severity items into `pm-weekly-status`.

**Monthly / mid-phase**:
- Audit aged Open items. Anything Open >30 days without a Notes update is a smell — chase the owner or close.
- Re-score items where the situation has changed.
- Move realized Risks to Issues; move invalidated Assumptions to Risks or Issues.

## Lifecycle transitions

The four types are not silos. Things move:

- **Risk → Issue** when it materializes. Add a note on the Risk closure (`Realized — see I-NNN`). Open a fresh Issue with a backlink.
- **Assumption → Risk** when it's looking shaky. Same backlink pattern.
- **Assumption → Issue** when it's already been falsified.
- **Dependency → Issue** when "Needed by" passes and it's still not delivered.

Backlinks are how the log stays auditable.

## Common failure modes

- One spreadsheet with mixed types. Risks and issues are different — don't merge them.
- "The team" as owner. Always name a person.
- Severity stuck at 3/3 forever because nobody re-scores. Re-score when status changes.
- Issues that should be Risks (or vice versa). If it hasn't happened yet, it's a Risk.
- Closed items deleted from the sheet. Don't — keep the audit trail. Filter by Status instead.
- 100 entries, all "Open." Most of these are stale. Close aggressively; reopen if needed.
- "Mitigation: monitoring." That's not mitigation, that's denial. Push for a real action.

## Producing the xlsx

When asked to generate the workbook, use the `xlsx` skill to create the file with:
- Headers frozen on row 1
- Data validation on Status, Type, Probability, Impact, Urgency columns (dropdowns)
- Conditional formatting on Severity columns (color bands)
- Summary sheet formulas as described
- Save as `RAID-Log-{ProjectName}.xlsx` (or project-appropriate name) in the workspace folder

For ongoing maintenance via this skill, accept input as either:
- A markdown table (paste from Slack, doc, meeting notes) → produce updated xlsx.
- An updated xlsx → re-summarize and surface changes for `pm-weekly-status`.

## How this skill relates to others

- Feeds directly into `pm-weekly-status` (Risks & Issues table, Decisions Needed).
- Items often spawn user stories — see `pm-user-story`.
- File production handled by the `xlsx` skill.
- For requirements gathering that surfaces assumptions, see forthcoming `pm-requirements-discovery` skill.
## Source frameworks

- **Josephs & Rubenstein, *Risk Up Front*** — primary source for the R column. RUF meeting structure, familiarity scoring, cause→effect→impact format, RAP-sheet structure, the chicken test, the <80% escalation rule. See [`../../references/book-risk-up-front.md`](../../references/book-risk-up-front.md).
- **Burtonshaw-Gunn, *Essential Tools for Management Consulting*** — 4Ts Risk Response (Treat, Transfer, Terminate, Tolerate); standard risk-management process. See [`/80_Skills_and_Agents/consulting/references/book-essential-tools-burtonshaw.md`](../../../consulting/references/book-essential-tools-burtonshaw.md).

## Templates this skill uses

- [`../../references/template-risk-up-front.md`](../../references/template-risk-up-front.md) — primary deliverable for kickoff RUF

