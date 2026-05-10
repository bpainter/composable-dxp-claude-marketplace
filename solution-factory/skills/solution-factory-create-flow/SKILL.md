---
name: solution-factory-create-flow
description: >
  The main orchestrator for the Slalom Solution Factory. Auto-activates when a user
  starts a new chat in the Solution Factory Cowork project. Diagnoses intent (new
  solution / refresh / single-stage), routes to the right stage orchestrator, and
  walks the user through the four-stage pipeline (Frame → Draft → Polish → Ship)
  end-to-end. Uses AskUserQuestion for discrete decisions; conversational input for
  free-text fields. Sequences other Solution Factory orchestrators (frame, draft,
  polish, ship) and pulls specialty skills from consulting, design, marketing,
  brand, contentful, and vercel plugins where needed. Also known as: solution
  factory orchestrator, solution-create entry point, full pipeline runner.

type: skill
project: skills-library
plugin: solution-factory
aliases: [solution-factory-create-flow]
tags: [type/skill, plugin/solution-factory, scope/orchestrator, topic/pipeline]
status: active
---

## Goal

Take a user from "I want to create a Slalom solution" to a promoted, ready-to-sell kit. End-to-end. Cowork-native — uses AskUserQuestion at every discrete decision so the user feels prompted, not interrogated.

## When to invoke

- User starts a new chat in the Solution Factory Cowork project (default)
- User runs `/solution-create`
- User asks any version of: "create a new solution," "build a sales kit," "I need to package up X for sales," "let's build the kit for [solution name]"

For partial workflows (just intake, just polish, etc.), route to the stage orchestrator directly via the matching slash command — `/solution-frame`, `/solution-draft`, `/solution-polish`, `/solution-ship`.

## Required context

For pipeline mechanics, routing, and anti-patterns, see `../../references/solution-factory-foundations.md`. For visual / output toolchain, see `../../references/solution-factory-toolchain.md`. **Do not re-explain the pipeline** — cite the foundations file.

## Stages

### Stage 0 — Diagnose intent (30 sec)

Before invoking any stage orchestrator, confirm what the user wants. Use **AskUserQuestion** with this question:

```
Question: "What would you like to do?"
Options:
1. Create a new solution from scratch (full pipeline) — Recommended
2. Update or refresh an existing solution
3. Run only one stage (intake / draft / polish / ship)
4. Check the status of solutions currently in WIP
```

**If "Create a new solution from scratch":** proceed to Stage 1.

**If "Update or refresh":** ask for the existing solution name. Pull the prior `00_Intake_Brief.md` if found. Skip to the stage where revisions begin (usually Draft).

**If "Run only one stage":** route to the matching orchestrator skill (`solution-factory-frame`, `-draft`, `-polish`, or `-ship`) without running the full flow.

**If "Check status":** invoke `solution-factory-status` and stop.

### Stage 1 — Frame

Hand off to **`solution-factory-frame`**. That skill walks the user through the 5 required calls + the rest of the intake using AskUserQuestion. Returns when `00_Intake_Brief.md` exists in `WIP/[solution-name]/` and the readiness check passes.

**Pause point:** confirm the intake before drafting begins. Use AskUserQuestion:

```
Question: "Intake complete. Ready to start drafting?"
Options:
1. Yes, start drafting all 8 deliverables (Recommended)
2. Yes, but draft only the One-Pager first so I can review the framing
3. Pause — I want to refine the intake first
```

**Recommendation logic:** if the readiness check failed any of the five required calls, default to option 3.

### Stage 2 — Draft

Hand off to **`solution-factory-draft`**. That skill drafts all 8 deliverables in markdown by:

1. Copying the templates from `60_Digital_Manufacturing/Solution_Factory/Templates/` into the WIP folder, prefixed `01_` through `08_`.
2. Drafting in this order: One-Pager first (anchors the kit), then Sales Enablement Kit (depth), then First Call Deck (most important client-facing artifact), then Discovery Workshop, then Case Study, Email Templates, Handoff Packet, Marketing Handoff.
3. Calling specialty skills as needed:
   - `consulting:consulting-humanize` — prose pass on AI-feeling output
   - `marketing:marketing-copywriter` — for email copy + LinkedIn drafts
   - `consulting:consulting-management-consultant` — for engagement-shape and pricing
   - `cx:cx-jobs-to-be-done` — when surfacing the Pain Chain needs JTBD framing
   - `brand:brand-voice-tone` — for voice-and-tone adherence per brand selected at intake
4. Pausing for review at the end of each deliverable (or after One-Pager + Sales Enablement Kit + First Call Deck — the three most consequential — and batching the rest).

**Pause point:** read all 8 markdown drafts. Confirm framing is right before Polish. AskUserQuestion:

```
Question: "Draft complete. Ready to Polish (PPTX, visuals, brand)?"
Options:
1. Yes, polish everything (Recommended)
2. Yes, but only the One-Pager + First Call Deck for now (most-used artifacts)
3. Pause — I want to revise the drafts first
```

### Stage 3 — Polish

Hand off to **`solution-factory-polish`**. That skill:

1. Generates visuals using the toolchain (`../../references/solution-factory-toolchain.md`):
   - Diagrams via Napkin
   - Flowcharts via Figma + Mermaid
   - Charts via PowerPoint native
   - Images via higgsfield (nano banana default, chatgpt 2 fallback)
   - Animations via higgsfield (Seedance)
   - Icons via Flaticon (lined or colored, never hand-drawn)
2. Formats to output:
   - 16:9 PPTX via PowerPoint MCP (using `Slalom_PowerPoint_16x9_Aug2025.potx` as base)
   - 8.5×11 PDF via Word MCP for facilitator guides + long-form case studies
3. Applies brand:
   - Slalom default → standard logo / color / typography
   - Composable DXP secondary → TFIC design system overrides
4. (Optional) For Composable DXP Complex tier — proposes a microsite. AskUserQuestion confirms whether to deploy.

**Pause point:** review the polished output before Ship. AskUserQuestion:

```
Question: "Polish complete. Ready to run Quality Gates and ship?"
Options:
1. Yes, run Quality Gates and promote (Recommended)
2. Show me the visual review checklist first
3. Pause — I want to revise polish output first
```

### Stage 4 — Ship

Hand off to **`solution-factory-ship`**. That skill:

1. Walks the Quality Gates checklist (15 anti-patterns + ~10 SF-specific gates).
2. Cross-document consistency check (One-Pager is the source of truth).
3. Routes by Brand:
   - Composable DXP secondary → `10_Practice/Offerings/Solutions/{Tier}/[Name]-[Date]-Slalom/` + reference stub at `40_Library/Solution_Briefs/`
   - Slalom default → `40_Library/Solution_Briefs/[Name]-[Date]-Slalom/`
4. Updates the Composable DXP practice catalog (if applicable).
5. Tags Salesforce Solution per `50_Knowledge/Frameworks/Go_to_Market/`.
6. Archives the WIP folder.

**Final confirmation:** AskUserQuestion before promotion:

```
Question: "Ready to promote [Solution Name] to its canonical home?"
Options:
1. Yes, promote to {target} (Recommended)
2. Show me the promotion target details first
3. Pause — I want to revise once more
```

## Handoffs

| Hands off to | When | What it returns |
|---|---|---|
| `solution-factory-frame` | Stage 1 — intake | Filled `00_Intake_Brief.md` + WIP folder created |
| `solution-factory-draft` | Stage 2 — drafting | 8 markdown deliverables in WIP |
| `solution-factory-polish` | Stage 3 — visuals + format | PPTX/PDF/HTML files at WIP top level |
| `solution-factory-ship` | Stage 4 — Quality Gates + promote | Kit at canonical location + (optional) reference stub + WIP archived |
| `solution-factory-status` | "Check status" intent at Stage 0 | Status report on active solutions |

## Failure modes (and what to do)

- **User picks "create" but already has a solution with that name in WIP.** Surface the existing WIP folder. AskUserQuestion: "Resume that one, start fresh, or pick a different name?"
- **Intake stalls because the user can't answer the Deb Oler question.** Don't push past it. The kit isn't ready. Suggest: park this and revisit when the unique benefit is clearer. Optionally call `consulting:consulting-management-consultant` to help frame.
- **Brand selection is ambiguous.** Default per heuristic: if solution name or anchor capability includes any of {Composable, Headless, DXP, MACH, Contentful, Vercel, Algolia, Bynder, modern commerce}, recommend Composable DXP secondary; otherwise Slalom default. Surface the recommendation in the AskUserQuestion options.
- **Higgsfield credits low.** Polish stage checks via `mcp__higgsfield__balance` before image-heavy generation. If low, surface to user: "Higgsfield credits at X — proceed, top up first, or skip image generation?"
- **Quality Gates fail at Ship.** Don't proceed with promotion. Surface failures via the `QUALITY_GATES_FAILED.md` template in Quality_Gates.md. Hand back to Stage 2 or 3.
- **Promotion target conflict.** If a folder of the same name already exists at the target, AskUserQuestion: "Versioned overwrite, replace, or pick a new name?" Default to "versioned overwrite" with a `_v2` suffix.

## How this skill collaborates with other plugins

The Solution Factory orchestrators *call* skills from other plugins. They don't reimplement them. Specifically:

| When | Plugin / Skill |
|---|---|
| Drafting Sales Enablement Kit | `consulting:consulting-management-consultant`, `marketing:marketing-copywriter`, `brand:brand-voice-tone` |
| Drafting Discovery Workshop | `facilitation:facilitation-workshop-designer`, `cx:cx-jobs-to-be-done`, `consulting:consulting-management-consultant` |
| Drafting First Call Deck | `consulting:consulting-sales-bd-strategist`, `design:design-presentation`, `brand:brand-voice-tone` |
| Drafting Marketing Handoff | `marketing:marketing-social-media-marketer`, `marketing:marketing-copywriter`, `marketing:marketing-geo-content` |
| Polishing imagery | `design:design-imagery` (advice), then higgsfield MCP |
| Polishing visualizations | `design:design-visualization` (advice), then Napkin or Figma |
| Polishing PPTX | `anthropic-skills:pptx` + Slalom .potx via PowerPoint MCP |
| Polishing 8.5×11 docs | `anthropic-skills:docx` + Word MCP |
| Polishing microsite | `vercel:vercel-deploy-pipeline` + Composable DXP design system + `software-engineering:software-engineering-shadcn-component` |
| Quality humanization | `consulting:consulting-humanize` |
| Quality Gates audit | `solution-factory-ship` (calls foundations + design:design-audit if visual review fails) |

## Token discipline

- **Don't re-explain the pipeline.** Cite `../../references/solution-factory-foundations.md`.
- **Don't re-explain the toolchain.** Cite `../../references/solution-factory-toolchain.md`.
- **Don't re-author templates.** The templates are the canonical content guide — read them, don't paraphrase them.
- **Use AskUserQuestion for discrete choices**, not to ask things that have answers in the intake.
- **Hand off promptly.** This skill is a router, not a worker. Worker logic lives in stage orchestrators, advice skills, and agents.

## Example prompts

1. *"I want to create a new solution for Composable DXP migrations from AEM."* → Stage 0 routes to `solution-factory-frame`.
2. *"Refresh the Future of Marketing kit — the stats are old."* → Stage 0 detects "refresh", asks for the existing solution name, routes to Stage 2.
3. *"Just polish what I drafted yesterday — solution is `composable-dxp-migration` in WIP."* → Stage 0 detects single-stage intent, routes to `solution-factory-polish`.
4. *"What's in WIP right now?"* → Stage 0 routes to `solution-factory-status`.
5. *"Build me a kit."* → Stage 0 asks the diagnostic question, then routes per the answer.
6. *"I need a one-pager for [solution]. That's it."* → Stage 0 routes to `solution-factory-frame` for the intake-to-One-Pager flow only.
7. *"Walk me through the intake — I have a vague idea."* → Stage 0 routes to `solution-factory-frame`. The frame skill is patient with the Deb Oler question.
8. *"My CMS migration kit needs a microsite. We're proposing to a Complex tier client."* → Stage 0 routes to `solution-factory-polish` with the microsite flag enabled.

## See also

- `../../references/solution-factory-foundations.md` — pipeline, routing, anti-patterns
- `../../references/solution-factory-toolchain.md` — visual / output / deployment toolchain
- `../solution-factory-frame/SKILL.md` — Stage 1 orchestrator
- `../solution-factory-draft/SKILL.md` — Stage 2 orchestrator
- `../solution-factory-polish/SKILL.md` — Stage 3 orchestrator
- `../solution-factory-ship/SKILL.md` — Stage 4 orchestrator
- `../solution-factory-status/SKILL.md` — WIP status reporter
- `60_Digital_Manufacturing/Solution_Factory/Pipeline.md` — the canonical pipeline doc
