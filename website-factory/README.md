# website-factory

The Slalom Website Factory plugin — orchestration for delivering production-grade websites end to end (Discovery → Definition → **Review** → Implementation → Quality & UAT → Launch).

## What this plugin does

Operationalizes the six-phase pipeline at [`60_Digital_Manufacturing/Website_Factory/`](../../60_Digital_Manufacturing/Website_Factory/).

```
Discovery → Definition → Review → Implementation → Quality & UAT → Launch
```

Each phase has its own orchestrator skill (planned). Phase 3 Review is the first skill shipped because it's the highest-leverage piece — the local stakeholder microsite that lets the client sign off on Discovery + Definition deliverables before any code is written.

This plugin is **thin**. It owns the pipeline orchestration and the Phase 3 microsite generator. Every other phase delegates to existing plugins (`cx`, `brand`, `design`, `software-engineering`, `contentful`, `algolia`, `vercel`, `marketing`, `project-management`).

## Skills

| Skill | Type | Phase | Status |
|---|---|---|---|
| `website-factory-review-microsite` | generator | Phase 3 — Review | ✅ v0.1 |
| `website-factory-create-flow` | orchestrator (main) | all | 📋 planned |
| `website-factory-intake` | orchestrator | Phase 1 — Intake | 📋 planned |
| `website-factory-discover` | orchestrator | Phase 1 — Sub-tracks | 📋 planned |
| `website-factory-define` | orchestrator | Phase 2 | 📋 planned |
| `website-factory-implement` | orchestrator | Phase 4 | 📋 planned |
| `website-factory-launch` | orchestrator | Phase 6 | 📋 planned |
| `website-factory-status` | advice | cross-cutting | 📋 planned |
| `website-factory-gate` | gate runner | cross-cutting | 📋 planned |

## Agents (planned)

| Agent | Job | Status |
|---|---|---|
| `website-factory-scaffolder` | Creates `WIP/[project-code]/` from `_template/`, fills `PROJECT.md` from intake | 📋 planned |
| `website-factory-gate-runner` | Walks any phase's gate checklist with the operator, captures outcomes | 📋 planned |
| `website-factory-implementation-coordinator` | Handles Phase 4 sprint-by-sprint orchestration | 📋 planned |

## Commands (planned)

| Command | What it does |
|---|---|
| `/website-create` | Start a new website project end-to-end |
| `/website-intake` | Run only Phase 1 intake |
| `/website-review` | Generate the Phase 3 stakeholder microsite |
| `/website-status` | Multi-project status across `WIP/` |
| `/website-gate` | Run the gate check for the current phase |

## How this plugin pairs with the rest of the marketplace

The Website Factory orchestrators *call* skills from other plugins. They don't reimplement them.

| When | Plugin called |
|---|---|
| Phase 1 intake | `consulting:consulting-digital-strategist`, `project-management:project-management-requirements-discovery` |
| Phase 1 trend + market | `cx:cx-product-trend-researcher` |
| Phase 1 brand exploration | `brand:brand-strategist`, `brand:brand-naming` |
| Phase 1 design exploration | `design:design-web-designer`, `design:design-taste` |
| Phase 1 initial CX | `cx:cx-customer-research`, `cx:cx-persona-developer`, `cx:cx-jobs-to-be-done` |
| Phase 2 CX | `cx:cx-*` |
| Phase 2 brand | `brand:brand-identity-system`, `brand:brand-voice-tone`, `brand:brand-guidelines-composer`, `brand:brand-tokenize` |
| Phase 2 content | `marketing:marketing-copywriter`, `marketing:marketing-seo-brief`, `cx:cx-information-architect` |
| Phase 2 design | `design:design-*`, `software-engineering:software-engineering-design-system-engineer`, `software-engineering:software-engineering-pro-blocks-composer` |
| Phase 2 modeling | `contentful:contentful-content-model`, `algolia:algolia-index-design` |
| Phase 3 microsite generation | `design:design-web-designer`, `design:design-presentation`, `software-engineering:software-engineering-frontend-developer`, `design:design-visualization`, `design:design-imagery`, `design:design-iconography`, `software-engineering:software-engineering-a11y-audit`, `consulting:consulting-humanize` |
| Phase 4 Sprint 0 | `software-engineering:software-engineering-nextjs-scaffold`, `software-engineering:software-engineering-tailwind-tokens`, `vercel:vercel-deploy-pipeline` |
| Phase 4 build | `software-engineering:software-engineering-*`, `contentful:contentful-*`, `algolia:algolia-*` |
| Phase 5 quality | `software-engineering:software-engineering-quality-engineer`, `-a11y-audit`, `-security-advisor`, `-nextjs-performance` |
| Phase 6 launch | `software-engineering:software-engineering-devops-engineer`, `vercel:vercel-deploy-pipeline`, `marketing:marketing-seo-geo-strategist` |

## Install

This plugin is part of the `composable-dxp` marketplace. Once packaged:

```bash
claude plugin install website-factory@composable-dxp
```

## See also

- [`60_Digital_Manufacturing/Website_Factory/`](../../60_Digital_Manufacturing/Website_Factory/) — the factory itself (pipeline, gates, templates, references)
- [`60_Digital_Manufacturing/Website_Factory/References/Agents_and_Skills.md`](../../60_Digital_Manufacturing/Website_Factory/References/Agents_and_Skills.md) — full phase-to-skill mapping + gap analysis
- [`60_Digital_Manufacturing/Website_Factory/References/Stakeholder_Microsite.md`](../../60_Digital_Manufacturing/Website_Factory/References/Stakeholder_Microsite.md) — Phase 3 microsite structure + CSS pattern
- [`80_Skills_and_Agents/solution-factory/`](../solution-factory/) — sibling factory plugin; reference for plugin shape
