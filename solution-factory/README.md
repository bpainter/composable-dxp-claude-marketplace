# solution-factory

The Slalom Solution Factory plugin — orchestration for producing solution kits (8 deliverables) from idea to promoted artifact, end to end.

## What this plugin does

Operationalizes the four-stage pipeline at [`60_Digital_Manufacturing/Solution_Factory/`](../../60_Digital_Manufacturing/Solution_Factory/):

```
Frame → Draft → Polish → Ship
```

Each stage has its own orchestrator skill that sequences specialist skills (consulting, design, marketing, brand, contentful, vercel, anthropic-skills/pptx/docx) and Solution Factory-specific skills. The user experience is **Cowork-native** — when somebody starts a new chat in the Solution Factory project, the main orchestrator walks them through the intake using AskUserQuestion, then routes through the stages.

## Skills

| Skill | Type | Stage |
|---|---|---|
| `solution-factory-create-flow` | orchestrator (main) | Frame → Draft → Polish → Ship |
| `solution-factory-frame` | orchestrator | Stage 1 — intake via AskUserQuestion |
| `solution-factory-draft` | orchestrator | Stage 2 — markdown drafting of all 8 deliverables |
| `solution-factory-polish` | orchestrator | Stage 3 — visuals, formatting, brand application |
| `solution-factory-ship` | orchestrator | Stage 4 — Quality Gates, dual-routing promotion |
| `solution-factory-status` | advice | inspects WIP/ folders, reports on active solutions |

## Agents

| Agent | Job |
|---|---|
| `solution-factory-scaffolder` | Creates `WIP/[solution-name]/` folder, copies templates, creates `00_Intake_Brief.md` with collected metadata |
| `solution-factory-promoter` | Routes by Brand: copies WIP contents to `10_Practice/Offerings/Solutions/{Tier}/[Name]-[Date]-Slalom/` (Composable DXP) or `40_Library/Solution_Briefs/[Name]-[Date]-Slalom/` (Slalom default). Creates the library reference stub when needed. Updates the practice catalog. |
| `solution-factory-stub-generator` | Single-file pointer creator — emits the `40_Library/Solution_Briefs/[Name]-[Date]-Slalom.md` stub for Composable DXP solutions |

## Commands

| Command | What it does |
|---|---|
| `/solution-create` | Start a new solution end-to-end (default for new Cowork chats in the project) |
| `/solution-frame` | Run only the Frame stage (intake) — output is `00_Intake_Brief.md` |
| `/solution-draft` | Begin drafting from a completed intake |
| `/solution-polish` | Convert markdown to formatted output (PPTX / PDF / HTML) |
| `/solution-ship` | Run Quality Gates + promote |
| `/solution-status` | Show status of solutions in `WIP/` |

## References

- `references/solution-factory-foundations.md` — canonical plugin context. Names the pipeline, the boundary, the routing logic, anti-patterns. Cited by every skill.
- `references/solution-factory-toolchain.md` — the visual / output / deployment toolchain. Napkin / Figma / PowerPoint native / higgsfield (nano banana, chatgpt 2 fallback, Seedance) / Flaticon / Word / Vercel.

## How this plugin pairs with the rest of the marketplace

The Solution Factory orchestrator skills *call* skills from other plugins. They don't reimplement them.

| When | Plugin called |
|---|---|
| Drafting Sales Enablement Kit content | `consulting`, `marketing-copywriter`, `brand` |
| Drafting Discovery Workshop | `facilitation`, `cx-customer-research`, `consulting` |
| Drafting First Call Deck | `consulting`, `design-presentation`, `brand-voice-tone` |
| Polishing imagery | `design-imagery` (advice), then higgsfield MCP for generation |
| Polishing visualizations | `design-visualization` (advice), then Napkin or Figma |
| Polishing PPTX | `anthropic-skills:pptx` + Slalom .potx via PowerPoint MCP |
| Polishing 8.5×11 docs | `anthropic-skills:docx` + Word MCP |
| Polishing microsite (DXP only) | `vercel:vercel-deploy-pipeline` + Composable DXP design system |
| Quality humanization pass | `consulting:consulting-humanize` |

## Install

This plugin is part of the `composable-dxp` marketplace. Install via:

```bash
claude plugin install solution-factory@composable-dxp
```

After install, the Solution Factory project in Cowork can use the slash commands and skills. The main orchestrator (`solution-factory-create-flow`) auto-activates when a new chat starts in the project.

## See also

- [`60_Digital_Manufacturing/Solution_Factory/`](../../60_Digital_Manufacturing/Solution_Factory/) — the factory itself (templates, pipeline, quality gates)
- [`50_Knowledge/Frameworks/Slalom_Operating_System/`](../../50_Knowledge/Frameworks/Slalom_Operating_System/) — the canonical operating system
- [`50_Knowledge/Frameworks/Sales_Theory/`](../../50_Knowledge/Frameworks/Sales_Theory/) — pursuit-side theory
- [`50_Knowledge/Frameworks/Pursuit_to_Delivery/`](../../50_Knowledge/Frameworks/Pursuit_to_Delivery/) — the boundary
