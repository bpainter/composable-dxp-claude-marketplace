# Three Orchestration Patterns — Deep Reference

Three patterns for coordinating multiple skills, agents, or plugins in a Claude project. Each pattern's mechanics, when it fits, and a concrete template.

> **Sources:**
> - Anthropic's official `feature-dev` plugin: https://github.com/anthropics/claude-plugins-official (Pattern B template)
> - Morphllm's Claude Orchestrator: https://www.morphllm.com/claude-orchestrator (Pattern C primitives and proven scaling characteristics)
> - This marketplace's category-agent convention: see any plugin's `agents/<plugin>-agent.md` (Pattern A's plugin-internal variant)

## Pattern A: Cowork project-level orchestration

The default. A workspace folder gets a `CLAUDE.md` (and often a `memory/` folder) that teaches Claude when to reach for which skill across the user's installed plugins. Sometimes paired with one or two slash commands for predictable workflows.

### Mechanics

- The user opens a Cowork project (workspace folder).
- Cowork loads the project's `CLAUDE.md` at the start of every session.
- The `CLAUDE.md` names the recurring workflows and points to the right skills.
- Skills are matched naturally by their `description:` fields when the user asks something matching their triggers.
- The orchestration is *guidance*, not enforcement: Claude can deviate when it makes sense, but the defaults bias toward the documented sequences.

### When it fits

- Multiple plugins installed; recurring multi-skill workflows.
- Workflows are common but not predictable enough to slash-key.
- User wants Claude to default to known sequences without re-explaining each time.
- Cowork (desktop) is the surface — not Claude Code.

### Cost

Low. A 100-line `CLAUDE.md`, 3–7 memory files, optionally 1–2 slash commands.

### Template: project-level `CLAUDE.md`

```markdown
# <Project Name>

## What this project is
One paragraph: domain, the user's role, the typical week.

## Recurring workflows
- **New client engagement** → `consulting-management-consultant` (problem framing) → `consulting-proposal-strategist` (proposal) → `consulting-sow-risk-advisor` (SOW review) → `project-management-project-manager` (delivery plan)
- **Mid-engagement risk review** → `project-management-raid-log` (capture) → `consulting-sow-risk-advisor` (assess) → `consulting-change-management-advisor` (stakeholder readiness)
- **Sprint cycle** → `project-management-sprint-planning` (planning) → mid-sprint `project-management-raid-log` → `project-management-retro-facilitator` (retro)

## Defaults and preferences
- Tone: direct, builder-oriented, no hedging
- Output formats: Word (docx) for client-facing artifacts, Markdown for internal
- Avoid: corporate-speak, em-dash overuse
- Always: cite which skill produced which artifact

## When to invoke specific skills
| Situation | Skill | Why |
|---|---|---|
| User mentions a deal or pursuit | `consulting-sales-bd-strategist` | Pursuit qualification before proposal work |
| User mentions a workshop or kickoff | `project-management-facilitator` | Workshop design before facilitation |
| User mentions an "RFP" | `consulting-proposal-strategist` | Specialty over generalist |

## When to escalate
- Any task that crosses 3+ plugins (e.g., brand + design + engineering for a product launch) → confirm sequence before starting.
- Anything writing to my Slalom OneDrive without explicit ask → confirm first.
- Anything sending external messages → confirm first.

## Reference
- Skills documentation lives in `80_Skills_and_Agents/`
- See per-plugin READMEs for skill-by-skill detail
```

### Variant: plugin-internal category agent

A close cousin to project-level orchestration but scoped to a single plugin. A category agent (e.g., `consulting-agent`) lives at `<plugin>/agents/<plugin>-agent.md` and codifies the same workflow logic but only for the skills in that plugin.

When to use the variant: the orchestration is fully contained within one plugin (e.g., consulting workflows that don't cross into other domains). Use Pattern A's project-level `CLAUDE.md` when the orchestration genuinely crosses plugins.

This marketplace already implements category agents for every plugin — see `behavioral-economics/agents/behavioral-economics-agent.md` for an exemplar.

## Pattern B: Multi-phase command orchestrator (the feature-dev pattern)

A slash command walks through numbered phases with explicit user-approval gates. Each phase has a clear goal, concrete actions, and may invoke one or more skills/agents (sometimes in parallel).

### Mechanics

- The user types `/<command>` (e.g., `/feature-dev`, `/audit`, `/proposal`).
- The command file is structured as numbered phases.
- Each phase: goal + actions + (optionally) example agent prompts + an approval gate before continuing.
- Worker agents may be launched in parallel during a phase (e.g., 3 `code-explorer` agents during exploration).
- The orchestrator (the command body) merges agent outputs and presents synthesis to the user.
- Approval gates between phases let the user redirect or stop.

### When it fits

- Workflow is predictable enough for one-keystroke access (`/<command>`).
- Workflow has 5+ steps or branching decisions.
- User-approval at intermediate gates is valuable.
- Parallel exploration before synthesis pays off.

### Cost

Medium. A `commands/<command>.md` file with 50–200 lines of structured prose. May reference custom worker agents in the same plugin's `agents/`.

### Template: multi-phase command (modeled on `feature-dev`)

```markdown
---
description: Build a feature end-to-end across discovery, design, implementation, and review.
argument-hint: <feature-description>
---

# Feature Development

The user wants to build: $ARGUMENTS

Walk through the following phases. **Wait for user confirmation at every gate marked APPROVAL REQUIRED.**

## Phase 1: Discovery

**Goal:** Confirm the feature shape and constraints.

**Actions:**
1. Restate the feature request in your own words.
2. Identify acceptance criteria (3–5 bullets).
3. List open questions for the user.
4. **APPROVAL REQUIRED:** Confirm the discovery summary before continuing.

## Phase 2: Codebase Exploration

**Goal:** Build deep context on the relevant code.

**Actions:**
1. Launch 2–3 `code-explorer` agents in parallel. Each agent targets a different aspect of the codebase (similar features, architecture, data layer).
   - Example agent prompts:
     - "Find features similar to [feature] and trace through their implementation."
     - "Map the architecture and abstractions for [feature area]."
     - "Identify the data model and any migrations needed."
2. Once agents return, read the union of identified files.
3. Present a comprehensive summary of findings.
4. **APPROVAL REQUIRED:** Confirm the architectural picture before designing.

## Phase 3: Architecture

**Goal:** Design the solution.

**Actions:**
1. Propose an implementation approach with explicit trade-offs.
2. Identify files to create/modify and any new dependencies.
3. Surface any migrations or breaking changes.
4. **APPROVAL REQUIRED:** Confirm approach before implementation.

## Phase 4: Implementation

**Goal:** Make the changes.

**Actions:**
1. Implement file by file. Run tests after each meaningful change.
2. **APPROVAL REQUIRED before any destructive action** (delete, large refactor).

## Phase 5: Review

**Goal:** Catch issues before commit.

**Actions:**
1. Read every changed file once more.
2. Run linters and tests.
3. Surface any issues for user attention.
4. **APPROVAL REQUIRED:** User accepts or directs further changes.

## Phase 6: Summary

**Goal:** Hand off cleanly.

**Actions:**
1. Summarize what was built and why.
2. List any follow-ups (TODOs left, deferred refactors).
3. Update relevant docs.
```

### Pitfalls specific to Pattern B

- **Too many phases.** Most workflows fit in 3–5 phases. Seven is the upper bound for genuinely complex flows.
- **Approval-gate fatigue.** Don't gate every micro-step. Gate before destructive actions and at major decision points.
- **Lead doing the work.** The command body should sequence and merge; the actual work is in skills and agents it invokes.
- **No fallback path.** What happens when an agent comes back empty-handed? Define the fallback explicitly.

## Pattern C: Parallel agent team with task queue (the Morphllm pattern)

A lead Claude session decomposes work into JSON tasks, spawns specialized worker agents (each with a worktree for safety), routes typed messages between them, and merges outputs. Requires Claude Code, not Cowork.

### Mechanics

- **Lead session** runs `claude` in a project root with `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` enabled in `settings.json`.
- **Task queue** lives at `~/.claude/tasks/<team-name>/` as JSON files. Each task: `subject`, `description`, `status`, `owner`, `blocks`, `blocked_by`.
- **Worker agents** are defined in `<project>/.claude/agents/<name>.md` with frontmatter (name, description, tools, model, isolation: worktree, memory). Body describes the agent's focus, file ownership, and reporting protocol.
- **Messaging** lives at `~/.claude/teams/<team-name>/inboxes/<agent>.json`. Typed messages: direct, broadcast, shutdown, plan-approval.
- **File ownership rules** in `CLAUDE.md` prevent concurrent edits.
- **Worktree isolation** means each agent operates in a separate git worktree; the lead merges.
- **Hooks** can validate completed tasks (e.g., run tests, exit 0 to accept or 2 to reject).

### When it fits

- Work decomposes cleanly by file ownership (e.g., backend, frontend, tests).
- Parallel execution provides meaningful wall-clock savings (Morphllm reports ~40% with 3 agents, ~15% with 16).
- You're in Claude Code, not Cowork.
- Token cost (~3x of single-agent) is acceptable.

### Cost

High. Multiple agent definitions, hooks, careful CLAUDE.md, ~3x tokens. Most users will never need this.

### Template: agent definition

```markdown
---
name: backend-billing
description: Implements the billing API and database schema. Owns src/api/billing/ and src/db/migrations/.
tools: Read, Grep, Glob, Bash, Edit, Write
model: sonnet
isolation: worktree
memory: user
---

You are the **backend-billing** agent on this team.

## File ownership
- `src/api/billing/`
- `src/db/migrations/`
- `tests/api/billing/`

Do NOT edit files outside your ownership. If you need a change in another agent's territory, send a message via SendMessage.

## Focus
- JWT validation for billing endpoints
- Schema design for invoices, line items, payments
- Test coverage for the API surface

## Reporting protocol
- Update task status when starting/completing work
- Report blockers via SendMessage to the lead immediately
- Use broadcast only for cross-cutting discoveries

## Defaults
- Use Postgres conventions (snake_case columns, FK constraints, timestamps with timezone)
- All endpoints require auth except `/health`
- Tests use the project's existing pytest fixtures
```

### Template: project `CLAUDE.md` for an agent team

```markdown
# Project Context for All Agents

## Tech Stack
- Backend: Python + FastAPI + SQLAlchemy + Postgres
- Frontend: Next.js (App Router) + Tailwind + shadcn
- Tests: pytest (backend), Playwright (e2e)

## File Ownership Rules
- `backend-billing` agent: src/api/billing/, src/db/migrations/, tests/api/billing/
- `frontend-billing` agent: src/components/billing/, src/app/dashboard/, tests/e2e/billing/
- `test-orchestrator` agent: tests/integration/, tests/contract/

## Communication Protocol
- Report blocking issues via SendMessage immediately
- Update task status when starting/completing work
- Use broadcast only for critical cross-cutting discoveries
- Never edit files outside your ownership; request changes via message

## Hooks
- TaskCompleted runs ./scripts/validate-task.sh — exit 0 to accept, 2 to reject with feedback
```

### Pitfalls specific to Pattern C

- **Lead implementing code** → coordination quality drops. Enforce delegation strictly.
- **Concurrent edits to the same file** → ownership rules prevent this; worktrees catch what slips through.
- **Single giant task** → agents serialize. Decomposition discipline is critical.
- **Over-parallelization** → file conflict risk scales with agent count. 3–5 is the sweet spot for most projects; 16 requires extremely strict ownership.
- **Missing dependency edges** → if task A produces a prerequisite for task B but `blocked_by` isn't set, B starts before A finishes.
- **Broadcast spam** → reserve for genuine cross-cutting discoveries.
- **Wrong surface** → Cowork doesn't have the agent-team primitives; this is Claude Code only.

## How to choose between the three patterns

Quick decision tree:

1. **Are you in Cowork (desktop), or Claude Code (terminal)?**
   - Cowork → Pattern A or Pattern B (no Pattern C support).
   - Claude Code → All three patterns are options.

2. **Is the workflow predictable enough to slash-key?**
   - Yes → Pattern B (multi-phase command).
   - No → Pattern A (CLAUDE.md guidance).

3. **Does the work decompose cleanly by file ownership AND would parallel execution help?**
   - Yes (and you're in Claude Code) → Pattern C.
   - No → Stay with A or B.

4. **Is the orchestration scoped to one plugin's skills?**
   - Yes → Pattern A's *plugin-internal category agent* variant (this marketplace's existing `<plugin>-agent.md`).
   - No → Pattern A's *project-level CLAUDE.md* variant.

Default: Pattern A (project-level). Move up only when the constraint is real.
