# Official Plugin Examples — How Anthropic Builds Plugins

When in doubt about file conventions, agent metadata, or how to shape a multi-phase workflow, look at Anthropic's reference plugins. They're the canonical examples.

> **Repo:** https://github.com/anthropics/claude-plugins-official
>
> 40+ first-party plugins covering common developer workflows. Read the README for the marketplace, then drill into individual plugins.

## What you learn from the official examples

The official plugins land on a few conventions worth absorbing:

1. **Commands + agents** is the dominant pattern, not skills-as-primary. A typical plugin has zero or few `SKILL.md` files but several `commands/*.md` and `agents/*.md`. Each command orchestrates one or more agents.
2. **Agents have rich frontmatter:** `name`, `description`, `tools`, `model` (e.g., `sonnet`), and `color`. The color shows up in Claude Code's terminal UI.
3. **Commands have minimal frontmatter:** usually just `description` and sometimes `argument-hint`. The body is structured prose with numbered phases.
4. **Multi-phase workflow commands** (the orchestrator pattern) drive the most complex plugins. They define explicit phases with goals, actions, examples, and user-approval gates.

This marketplace's pattern (skills-first with category agents) is a different point on the design space — neither is "right." The official pattern leans into slash-key UX; this marketplace leans into natural-language matching. Mix and match per plugin based on whether the workflow is predictable enough to slash-key.

## Five plugins worth reading in full

| Plugin | What it teaches |
|---|---|
| **`feature-dev`** | The canonical multi-phase orchestrator. 7 phases (discovery → exploration → clarifying questions → architecture → implementation → review → summary), each with goal/actions/examples and user-approval gates. Launches 2–3 worker agents in parallel during exploration phase. **Read this when authoring an orchestrator.** |
| **`code-review`** | Parallel-agent review with confidence scoring. Demonstrates how to fan out specialized review agents (security, perf, style) and merge findings. |
| **`skill-creator`** | Anthropic's own opinion on how skills should be authored, evaluated, and benchmarked. Useful counterpoint to this marketplace's conventions. |
| **`mcp-server-dev`** | Step-by-step guidance for authoring MCP servers. Reference if a plugin needs to ship its own MCP server. |
| **`agent-sdk-dev`** | Building agents with the Claude Agent SDK. Good for understanding the agent file format end-to-end. |

When in doubt about a structural call, open the corresponding official plugin and copy its shape.

## Frontmatter conventions from the official plugins

### Agent file frontmatter

```yaml
---
name: code-explorer
description: Explore the codebase, find files relevant to a task, return a list of files and their relevance.
tools: Glob, Grep, Read, Bash
model: sonnet
color: yellow
---
```

Notes:
- `tools:` is a comma-separated string in the official examples (this marketplace uses YAML lists; both parse fine, but stay consistent within a plugin).
- `model:` controls which Claude model the agent uses. Defaults to the session's model if omitted. Use `sonnet`, `opus`, or `haiku` (full model strings work too).
- `color:` is purely cosmetic — controls the agent's badge color in Claude Code's terminal UI. Cowork ignores it.
- `description:` drives invocation matching, same as for skills.

### Command file frontmatter

```yaml
---
description: Build a feature end-to-end across discovery, design, implementation, and review.
argument-hint: <feature-description>
---
```

Notes:
- `argument-hint:` is shown in the slash-command menu next to the command name, telling the user what to type after `/feature-dev`. Optional but helpful.
- Body is the prompt — instructions for what to do when invoked. Often references `$ARGUMENTS` to inline what the user typed.

### Skill frontmatter (when present)

The official plugins' skills (e.g., in `skill-creator`) follow the same shape this marketplace uses: `name:`, `description:`, body with role/methodology/etc.

## Multi-phase orchestrator command shape (from `feature-dev`)

Use this as a template when authoring an orchestrator-style slash command:

```markdown
---
description: Build a feature end-to-end.
argument-hint: <feature-description>
---

# Feature Development

The user wants to build: $ARGUMENTS

Walk through the following phases. Wait for user confirmation at each gate marked **APPROVAL REQUIRED**.

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
2. Once agents return, read the union of identified files.
3. Present a comprehensive summary of findings.
4. **APPROVAL REQUIRED:** Confirm the architectural picture before designing.

## Phase 3: Architecture

**Goal:** Design the solution.

**Actions:**
1. Propose an implementation approach with trade-offs.
2. Identify files to create/modify.
3. Surface any new dependencies or migrations.
4. **APPROVAL REQUIRED:** Confirm approach before implementation.

[... more phases ...]

## Phase 7: Summary

**Goal:** Hand off cleanly.

**Actions:**
1. Summarize what was built and why.
2. List any follow-ups (TODO comments, deferred refactors).
3. Update relevant docs.
```

The pattern's strength: each phase is a clear contract with a goal, concrete actions, and an approval gate. The user can interrupt at any boundary, redirect, or fork the work. The orchestrator's "intelligence" is in the **sequencing and gates**, not in re-implementing what the worker agents already do.

For the deep dive on orchestrator patterns and when to reach for them, see the sibling skill `claude-orchestrator`.

## What to copy and what to adapt

When borrowing from official plugins:

- **Copy:** frontmatter shape, phase-and-gate command structure, parallel-agent invocation patterns, the model/color metadata format.
- **Adapt:** the marketplace conventions (this marketplace prefixes skill names with `<plugin>-`; the official plugins don't always do this, since they assume install in a fresh marketplace), the file naming for skills (this marketplace uses kebab-case folders; official examples are consistent with that).
- **Don't copy verbatim:** specific agent prompts and command bodies — those are tied to the official plugins' specific use cases.

## How official plugins handle marketplaces

The repo IS a marketplace. Its `.claude-plugin/marketplace.json` lists every plugin under `plugins/<name>/`. This is the same shape as your `second-brain` marketplace:

```json
{
  "name": "claude-plugins-official",
  "owner": { "name": "Anthropic" },
  "plugins": [
    { "name": "feature-dev", "source": "./plugins/feature-dev", "version": "...", "description": "..." },
    ...
  ]
}
```

Two structural differences from this marketplace:

1. **Plugins live under a `plugins/` subfolder** in the official repo, not at the marketplace root. The `source` paths reflect that. Either layout works — pick one and stay consistent.
2. **No `Plugins/` build-output folder** in the official repo (CLI install is the assumed path; no drag-and-drop `.plugin` files are pre-built). For Cowork-targeted marketplaces, keeping `Plugins/` is worthwhile.

## Reading list when starting a new plugin

In order:

1. The official `README.md` at the repo root — the marketplace description and install instructions
2. `feature-dev/README.md` — the canonical orchestrator
3. One simple plugin in full (e.g., `code-review`) — to see a smaller example end-to-end
4. The Cowork tutorial (https://claude.com/resources/tutorials/how-to-customize-plugins-in-cowork) for the install path
5. The Claude Code plugin docs (https://code.claude.com/docs/en/plugins) for the manifest schema and any newer features

Then start authoring.
