# Cowork Project Setup — Step-by-Step

A concrete walkthrough for setting up a new Cowork project so it knows how to orchestrate multiple skills across the user's installed plugins. This is Pattern A (project-level orchestration), the default for most users.

## Prerequisites

- Claude desktop app installed
- One or more plugins installed via Settings → Customize → Personal Plugins
- A folder on the user's computer that will become the project workspace

## Step 1 — Connect the workspace folder to Cowork

In a Cowork conversation, ask Claude to access a folder, or use the "select folder" option in the UI. Cowork mounts the folder and treats files inside it as the project's filesystem.

Once connected, anything Claude writes (CLAUDE.md, memory files, slash commands) lives inside that folder and persists across sessions.

## Step 2 — Inventory installed plugins and recurring workflows

Before writing anything, name what you have and what you do with it.

**Inventory what's installed:**

```text
[ ] consulting (or whichever plugin names apply)
[ ] cx
[ ] design
[ ] software-engineering
[ ] project-management
[ ] ...
```

For each, list which skills you actually use weekly+. The orchestration documents what's *active*, not what's installed.

**List recurring workflows:**

For each weekly-or-more workflow, write one line:

> When I do X, the steps are: A → B → C, and the output goes to Z.

A few examples (to seed the user's thinking):

- **New client engagement** → frame the problem → write the proposal → review SOW risk → write the kickoff agenda → store in client folder
- **Sprint cycle** → plan the sprint → mid-sprint risk capture → run the retro → file outcomes
- **New content piece** → keyword research → SEO brief → draft → AI-extractable structure → publish
- **New design screen** → load design rules → compose from system → accessibility check → handoff to engineering

If a workflow takes 5+ steps and you do it weekly+, it's a candidate for a slash command (Pattern B).

If it's looser but still a recognizable sequence, it's a candidate for CLAUDE.md guidance (Pattern A).

## Step 3 — Write `CLAUDE.md` at the project root

The 100-line operating manual Cowork loads at the start of every session.

### Required sections

1. **What this project is** — domain, the user's role, the typical week.
2. **Recurring workflows** — table or list mapping each workflow to a skill sequence.
3. **Defaults and preferences** — tone, formats, things to avoid.
4. **When to invoke specific skills** — a routing table for the trickier judgment calls.
5. **When to escalate** — multi-domain tasks, destructive actions, external sends.

### Skill-routing table

The most useful piece. A table that helps Claude pick the right skill when the user's request is ambiguous:

| Situation | Skill | Why |
|---|---|---|
| User mentions "deal", "pursuit", "qualification" | `consulting-sales-bd-strategist` | Pursuit qualification before proposal work |
| User mentions "RFP" or "proposal" | `consulting-proposal-strategist` | Specialty over generalist |
| User mentions "scope" or "SOW" | `consulting-sow-risk-advisor` | SOW review and scope discipline |
| User mentions "workshop", "kickoff", "facilitation" | `project-management-facilitator` | Workshop design |
| User mentions "retro" | `project-management-retro-facilitator` | Retro-specific format selection |
| User mentions "feedback" or "1:1" | `leadership-people-leader` | Manager-level coaching |

Write the table in the user's voice — what they actually say, not what the skill is technically called.

### Sample CLAUDE.md skeleton

```markdown
# <Project Name>

## What this project is

I'm a <role> at <org> doing <typical work>. This Cowork project is where I run client engagements, draft proposals, manage delivery, and capture insights.

## Recurring workflows

- **New client engagement** → `consulting-management-consultant` (problem framing) → `consulting-proposal-strategist` (proposal) → `consulting-sow-risk-advisor` (SOW review) → `project-management-project-manager` (delivery plan)
- **Mid-engagement risk review** → `project-management-raid-log` (capture) → `consulting-sow-risk-advisor` (assess) → `consulting-change-management-advisor` (stakeholder readiness)
- **Sprint cycle** → `project-management-sprint-planning` (plan) → mid-sprint `project-management-raid-log` → `project-management-retro-facilitator` (retro)
- **Content piece for the company blog** → `marketing-seo-geo-strategist` → `marketing-seo-brief` → `marketing-copywriter` → `marketing-geo-content`

## Defaults and preferences

- Tone: direct, builder-oriented, no hedging, no corporate-speak
- Output formats: docx for client-facing artifacts; Markdown for internal notes
- Avoid: AI-tells (em-dashes everywhere, "I'd be happy to", "let me dive in")
- Always: cite which skill produced which artifact in the response

## When to invoke specific skills

| Situation | Skill | Why |
|---|---|---|
| "Help me think through this deal" | `consulting-sales-bd-strategist` | Pursuit qualification |
| "RFP due Friday" | `consulting-proposal-strategist` | Specialty |
| "Review the SOW" | `consulting-sow-risk-advisor` | Scope discipline |
| "Design the kickoff workshop" | `project-management-facilitator` | Workshop design |

## When to escalate

- Multi-domain tasks spanning 3+ plugins → confirm sequence before starting
- Anything writing to my OneDrive without explicit ask → confirm first
- Anything sending external messages (email, Slack) → confirm first
- Anything touching contracts, financials, or HR docs → confirm first

## Reference

- Skills documentation lives in `80_Skills_and_Agents/`
- Per-plugin READMEs at `80_Skills_and_Agents/<plugin>/README.md` describe each skill in detail
```

### Length discipline

Keep CLAUDE.md under 200 lines. If it grows beyond that, you're either:
- Over-orchestrating (move detail into per-workflow slash commands)
- Putting domain knowledge that belongs in `memory/` (move it there)
- Documenting individual skills (point to their SKILL.md files instead)

## Step 4 — Set up `memory/` (when stable knowledge is worth capturing)

Memory files live at `<project>/memory/` (or wherever your productivity-memory plugin convention says) and Cowork reads them every session. They're shared across all skills.

### When memory is the right home

- **Stable** — things that don't change often: bio, org chart, glossaries, recurring decisions
- **Cross-cutting** — needed by multiple skills, not just one
- **Compact** — keep each file focused; if it's growing past 100 lines, split

### When something does NOT belong in memory

- It's specific to one skill → put it in that skill's references
- It's project-specific but volatile → put it in CLAUDE.md or a per-workflow note
- It's external knowledge that doesn't change per session → reference it; don't copy it

### Common memory files

- **`bio.md`** — about you (background, role, expertise, communication preferences)
- **`org.md`** — your team, key stakeholders, reporting lines
- **`glossary.md`** — internal terms, acronyms, project codenames
- **`active-clients.md`** — clients/projects currently in flight (light — names, status, key contacts)
- **`decisions.md`** — durable decisions that should persist (e.g., "we standardize on X library", "we prefer Y format for handoffs")

Memory files don't get bullet-point structures imposed; write them as if you were briefing a colleague who's joining the team next Monday.

## Step 5 — Add slash commands for the predictable workflows

For each weekly+ workflow that has 5+ steps and you'd benefit from one-keystroke access, add a slash command at `<project>/commands/<name>.md`.

See `orchestration-patterns.md` → "Pattern B" for the full template.

Common Cowork slash commands worth considering:

- **`/proposal`** — qualify → draft → SOW review → kickoff agenda
- **`/retro`** — pick the format → run the retro → update RAID
- **`/morning`** — pull from Slack/Asana → triage → surface blockers
- **`/weekly-recap`** — synthesize the week's notes into a summary

Don't add empties. Add when you have a real one-keystroke need.

## Step 6 — Optional: project-local agent for complex coordination

If a workflow is too complex for CLAUDE.md prose AND not predictable enough for a slash command, codify it as a project-local agent at `<project>/agents/<name>.md`.

Most projects don't need this. The plugin-level category agents (e.g., `consulting-agent`, `marketing-agent`) usually cover cross-skill orchestration within a domain. Project-local agents make sense when you have a unique pattern that doesn't fit any single category.

## Step 7 — Test the orchestration

Open a fresh Cowork conversation and try:

1. **Trigger a recurring workflow by name:** "Help me prep for a new engagement." Claude should follow the documented sequence.
2. **Ask an ambiguous question:** "I have an RFP due Friday." Claude should reach for `consulting-proposal-strategist` per the routing table.
3. **Cross a skill boundary:** "Write the kickoff agenda after the SOW is signed." Claude should chain `project-management-facilitator` after the SOW step.
4. **Hit an approval gate:** "Update my client list with the new project." Claude should pause and confirm before writing to `memory/active-clients.md`.

If any test fails, refine CLAUDE.md to fix the gap. The most common fix is wording — Claude triggers on what's literally written. If you say "RFP" but CLAUDE.md only mentions "proposal", broaden the routing table.

## Step 8 — Iterate

Cowork projects evolve. Every couple of weeks, ask:

- **Is there a workflow I keep walking Claude through manually?** → Add it to CLAUDE.md or as a slash command.
- **Is there a skill I keep specifying when Claude should reach for it automatically?** → Add it to the routing table.
- **Is there an approval I keep granting?** → Either remove the gate (it's not earning its place) or refine it (it's surfacing the wrong things).
- **Is CLAUDE.md getting cluttered?** → Move detail into slash commands or memory files.

The orchestration layer is a living document. Treat it like a README: a small refactor every few weeks keeps it tight.

## Common Cowork project mistakes

| Mistake | Symptom | Fix |
|---|---|---|
| **CLAUDE.md too long** | Claude doesn't follow specific sections; mixed signals | Trim to under 200 lines; move detail into slash commands |
| **Routing table uses skill names, not user phrases** | Claude doesn't pick the right skill on natural prompts | Rewrite the "Situation" column in the user's actual voice |
| **Memory bloat** | Every session feels heavy; Claude pulls in irrelevant context | Cut memory files; keep each focused; archive the rest |
| **Empty `commands/` folder** | Slash menu has placeholder noise | Remove unused command stubs |
| **Approval-gate fatigue** | Claude pauses on every micro-decision | Limit gates to genuinely destructive operations |
| **No escalation rules** | Claude attempts cross-domain work without confirming | Add explicit "When to escalate" section |
| **Stale workflows** | CLAUDE.md describes 6-month-old work patterns | Quarterly refactor; archive what's no longer current |
