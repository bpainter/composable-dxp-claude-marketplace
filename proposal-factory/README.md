# proposal-factory

The Slalom Proposal Factory plugin — orchestration for producing pursuit proposals (SPRO / Small / Large-Slides / Large-Document variants) plus optional companion pieces (POC / video / microsite), from kickoff to submitted artifact.

## What this plugin does

Operationalizes the four-stage pipeline at [`60_Digital_Manufacturing/Proposal_Factory/`](../../60_Digital_Manufacturing/Proposal_Factory/):

```
Frame → Draft → Polish → Ship
```

Each stage has its own orchestrator skill that sequences specialist skills from other plugins (`consulting`, `design`, `marketing`, `brand`, `contentful`, `vercel`, `website-factory`) plus Proposal Factory-specific skills. The user experience is **Cowork-native** — when somebody starts a new chat in the Proposal Factory project, the main orchestrator walks them through the intake using AskUserQuestion, then routes through the stages.

**Single-operator factory.** This plugin assumes one user (Bermon) operates it — no multi-reviewer Quality Gates ceremony. Pause points between stages are self-review.

## Skills

| Skill | Type | Stage |
|---|---|---|
| `proposal-factory-create-flow` | orchestrator (main) | Frame → Draft → Polish → Ship |
| `proposal-factory-frame` | orchestrator | Stage 1 — intake + RFP auto-parse + variant + companion flags + brand |
| `proposal-factory-draft` | orchestrator | Stage 2 — variant-aware markdown drafting |
| `proposal-factory-polish` | orchestrator | Stage 3 — visuals + format + brand + companion production |
| `proposal-factory-ship` | orchestrator | Stage 4 — promote to pursuit Final/ |
| `proposal-factory-status` | advice | inspects WIP/ folders, reports on active pursuits |

## Agents

| Agent | Job |
|---|---|
| `proposal-factory-scaffolder` | Creates `WIP/[ClientName_Descriptor]/` folder, copies variant template + section snippets, creates `00_Intake_Brief.md` with collected metadata |
| `proposal-factory-rfp-parser` | Given an attached RFP/RFQ PDF, extracts evaluation criteria, mandatory sections, page limits, deadlines into `01_RFP_Parsed.md` with a draft compliance matrix |
| `proposal-factory-promoter` | Copies finished artifact + companions to `20_Proposals/Active/[Pursuit]/Final/`. Updates pursuit `_Brief.md` and `Decision_Log.md`. Archives WIP. |
| `proposal-factory-companion-router` | Orchestrates companion sub-flows at Polish — POC handoff to website-factory plugin; walkthrough video production via higgsfield + Elevenlabs; microsite deployment via Vercel MCP |

## Commands

| Command | What it does |
|---|---|
| `/proposal-create` | Start a new proposal end-to-end (default for new Cowork chats in the project) |
| `/proposal-frame` | Run only the Frame stage (intake + RFP parse) |
| `/proposal-draft` | Begin drafting from a completed intake |
| `/proposal-polish` | Convert markdown to formatted output + produce companions |
| `/proposal-ship` | Promote the finished proposal to the pursuit folder |
| `/proposal-status` | Show status of pursuits in `WIP/` |

## References

- `references/proposal-factory-foundations.md` — canonical plugin context. Names the pipeline, the four variants, the companion patterns, the single-operator posture, anti-patterns. Cited by every skill.
- `references/proposal-factory-toolchain.md` — the visual / output / deployment toolchain. Same as Solution Factory plus Elevenlabs (voiceover) and Website Factory handoff.

## How this plugin pairs with the rest of the marketplace

The Proposal Factory orchestrator skills *call* skills from other plugins. They don't reimplement them.

| When | Plugin called |
|---|---|
| Drafting Win Theme + Executive Summary | `consulting:consulting-management-consultant`, `marketing:marketing-copywriter`, `brand:brand-voice-tone`, `consulting:consulting-humanize` |
| Drafting Approach | `consulting:consulting-management-consultant`, `project-management:project-management-project-manager`, `consulting:consulting-digital-strategist` |
| Drafting Scope + Pricing | `consulting:consulting-sow-risk-advisor`, `consulting:consulting-management-consultant`, `consulting:consulting-negotiation-coach` |
| Drafting Team Bios | (pulls from team-finder; minimal LLM authoring) |
| Drafting References | `consulting:consulting-management-consultant` (pulls from `20_Proposals/Won/`) |
| Drafting Compliance Matrix | `proposal-factory-rfp-parser` agent (extraction) + `project-management:project-management-raid-log` (matrix discipline) |
| Polishing imagery | `design:design-imagery` (advice), then higgsfield MCP for generation |
| Polishing visualizations | `design:design-visualization` (advice), then Napkin or Figma |
| Polishing icons | `design:design-iconography` (advice), then Flaticon |
| Polishing layout | `design:design-presentation` (advice), then PowerPoint MCP or Word MCP |
| Polishing voice / language | `consulting:consulting-humanize` (rewrite consultant-jargon) + `brand:brand-voice-tone` (apply brand voice) |
| POC companion authoring | `website-factory` plugin (Claude Design / v0 / Figma Make) |
| Microsite companion authoring | `design:design-web-designer` + `software-engineering:software-engineering-frontend-developer` + `vercel:vercel-deploy-pipeline` |
| Walkthrough video script | `marketing:marketing-copywriter` (script) + `brand:brand-voice-tone` (voice alignment) |
| Ship promotion | `proposal-factory-promoter` agent + updates to `20_Proposals/Active/...` |
| Solution alignment (when pursuit anchors to a known solution) | `solution-factory` plugin (pull from kit) |

## Install

This plugin is part of the `composable-dxp` marketplace. Once packaged:

```bash
claude plugin install proposal-factory@composable-dxp
```

## See also

- [`60_Digital_Manufacturing/Proposal_Factory/`](../../60_Digital_Manufacturing/Proposal_Factory/) — the factory itself (pipeline, intake, variants, companions, templates, section snippets)
- [`60_Digital_Manufacturing/Proposal_Factory/instructions-claude-cowork.md`](../../60_Digital_Manufacturing/Proposal_Factory/instructions-claude-cowork.md) — Cowork project Instructions panel content
- [`80_Skills_and_Agents/solution-factory/`](../solution-factory/) — sibling factory plugin; the reference model
- [`80_Skills_and_Agents/website-factory/`](../website-factory/) — POC companion handoff target
- [`20_Proposals/`](../../20_Proposals/) — pursuit lifecycle home (promotion target at Ship)
