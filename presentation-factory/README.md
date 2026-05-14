# presentation-factory

The Slalom Presentation Factory plugin — orchestration for producing branded 16:9 presentations from intake through editable PowerPoint, with three brand modes (Slalom default / Composable DXP / Client custom) and two style modes (informational / presentational).

## What this plugin does

Operationalizes a four-stage pipeline:

```
Frame → Draft → Compose → Ship
```

| Stage | Outputs |
|---|---|
| **Frame** | `00_Intake_Brief.md` · brand tokens + logo in `WIP/[deck]/brand/` |
| **Draft** | `10_Content.md` — slide-by-slide content + asset prompts + speaker notes |
| **Compose** | `slides/NN-*.html` × N + `slides/deck.html` review index |
| **Ship** | `deck.pdf` (proof of render) + `deck.pptx` (editable text, movable assets) at the canonical destination |

Each stage has its own orchestrator skill that sequences specialty skills from `consulting`, `design`, `marketing`, `brand` plugins plus the factory's own `pf-*` skills.

**Single-operator factory.** Pause points between stages are for self-review. No multi-reviewer Quality Gates ceremony.

## Brand modes

| Brand | Tokens | Logo | Italic-emphasis | When to use |
|---|---|---|---|---|
| **Slalom** | `resources/tokens/slalom.css` | Slalom Sans + Lora | **On** — the signature move | Default for almost everything |
| **Composable DXP** | `resources/tokens/composable-dxp.css` | Lora 700 + Inter | Off — non-italic display | Composable Platforms practice work |
| **Client** | derived at intake → `WIP/[deck]/brand/tokens.css` | per-client | Per-client | Client-led decks or co-branded |

For Client mode, the `pf-brand-router` skill ingests from one of: (a) attached brand styleguide PDF, (b) `.com` crawl, or (c) manual entry. Outputs a filled `client-template.css` ready to use.

## Style modes

| Style | Lean toward | Density | Image-to-text |
|---|---|---|---|
| **Informational** | Tables, lists, multi-col text, dense layouts | High text | ~30% text / ~50% structure / ~20% imagery |
| **Presentational** | Image-led, big-stat, pull-quote, gradient hero | Low text | ~10% text / ~30% structure / ~60% imagery |

The style choice at Frame locks template selection downstream.

## The 25 canonical templates

Brand-agnostic structure; brand-expressing tokens injected at compose time. See `references/presentation-factory-templates.md` for the full catalog. Live in `resources/templates/`.

| Family | Count | Coverage |
|---|---|---|
| Openers & closers | 5 | title, section-divider, agenda, thank-you, contact |
| Content layouts | 9 | one-col, two-col×3, three-col, image-led, pull-quote, big-stat, list-with-icons, eyebrow-headline-body |
| Data & diagrams | 6 | chart, table, framework-2×2, process-3-stage, comparison, timeline |
| Specialty | 5 | team-bios, case-study, pricing, roadmap, before-after |

Browse: open `resources/templates/index.html` for a scaled-down preview grid of all 25.

## The signature design moves

- **Italic Lora emphasis within sans-serif headlines** — used across 90% of slalom.com section headers. The brand's strongest visual move. Templates support it via inline `<em>` in titles.
- **Smooth brand gradient** replacing the legacy 5-color bar — used as right-edge accent strip, top/bottom band, or full-bleed hero. Per-brand color stops (Slalom: lime → lavender → coral → cyan → blue. DXP: lavender → cobalt. Client: derived).
- **Generous white space** — Slalom's web breathes; cramped slides feel off-brand.
- **One accent color per slide** — restraint, not the rainbow.
- **Photography over illustration** — Higgsfield-generated with specific scene direction. Stock-photo clichés explicitly banned.
- **Icons from Flaticon** — Slalom's own illustration-style icons are banned by user mandate. One icon family per deck.

## Skills

| Skill | Type | Stage |
|---|---|---|
| `presentation-factory-create-flow` | orchestrator (main) | Frame → Draft → Compose → Ship |
| `presentation-factory-frame` | orchestrator | Stage 1 — intake + brand ingestion |
| `presentation-factory-draft` | orchestrator | Stage 2 — content authoring |
| `presentation-factory-compose` | orchestrator | Stage 3 — assets + HTML composition |
| `presentation-factory-ship` | orchestrator | Stage 4 — PDF → PPTX → promote |
| `presentation-factory-status` | advice | inspects WIP/, reports stage and staleness |
| `pf-brand-router` | specialty | brand decision + ingestion |
| `pf-content-author` | specialty | slide-by-slide content + arc + template mapping |
| `pf-speaker-notes` | specialty | 3-5 sentence notes per slide |
| `pf-asset-orchestrator` | specialty | Higgsfield + Napkin + Flaticon routing |
| `pf-slide-composer` | specialty | HTML composition with token + slot substitution |
| `pf-html-to-pdf` | specialty | Chromium headless render |
| `pf-pdf-to-pptx` | specialty | editable PowerPoint reconstruction |

## Agents

| Agent | Job |
|---|---|
| `presentation-factory-scaffolder` | Creates `WIP/[deck]/` folder structure with seeded `00_Intake_Brief.md` |
| `presentation-factory-promoter` | Promotes finished deck to canonical destination, archives WIP |

## Commands

| Command | What it does |
|---|---|
| `/presentation-create` | Start a new deck end-to-end (default entry point) |
| `/presentation-frame` | Run only Frame (intake + brand) |
| `/presentation-draft` | Run only Draft (content authoring) |
| `/presentation-compose` | Run only Compose (assets + HTML) |
| `/presentation-ship` | Run only Ship (PDF + PPTX + promote) |
| `/presentation-status` | Show status of WIP decks |

## References

- `references/presentation-factory-foundations.md` — pipeline, two styles, three brands, 25 templates, anti-patterns. Cited by every skill.
- `references/presentation-factory-templates.md` — full 25-template catalog with slot contract.
- `references/presentation-factory-toolchain.md` — Higgsfield, Napkin, Flaticon, Chromium, PowerPoint MCP usage.
- `references/slalom-brand-patterns.md` — synthesis from May 2026 slalom.com crawl.

## Resources

- `resources/tokens/slalom.css` — Slalom default brand tokens
- `resources/tokens/composable-dxp.css` — Composable DXP sub-brand tokens
- `resources/tokens/client-template.css` — Client brand template (fill at intake)
- `resources/templates/*.html` — the 25 canonical slide templates
- `resources/templates/index.html` — preview index of all 25 templates
- `resources/assets/slalom-logo*.svg` — Slalom brand marks
- `resources/assets/composable-dxp-*.svg` — Composable DXP marks

## How this plugin pairs with the rest of the marketplace

The orchestrator skills call skills from other plugins. They don't reimplement them.

| When | Plugin called |
|---|---|
| Framing the message | `consulting:consulting-management-consultant`, `consulting:consulting-digital-strategist` |
| Writing body copy | `marketing:marketing-copywriter` |
| Applying brand voice | `brand:brand-voice-tone` |
| Stripping AI tells | `consulting:consulting-humanize` (mandatory final pass) |
| Visual taste check | `design:design-taste` (four checks: swap / squint / signature / token) |
| Image generation direction | `design:design-imagery` |
| Diagram / framework direction | `design:design-visualization` |
| Icon family selection | `design:design-iconography` |

## Install

This plugin lives at `80_Skills_and_Agents/presentation-factory/` and is installable as a Claude/Cowork plugin from the `.claude-plugin/plugin.json` manifest.

```bash
# In a Cowork project or Claude Code session
claude plugin install presentation-factory
```

## Dependencies

- **Higgsfield MCP** — image / video generation
- **PowerPoint MCP (`mcp__PowerPoint__By_Anthropic__*`)** — editable PPTX creation
- **Chromium** or **Google Chrome** — for headless HTML → PDF rendering
- **pdftk** or Python **pypdf** — for merging slide PDFs
- **Slalom Sans** fonts at `/Users/bermon.painter/Downloads/Slalom Sans V1.017/Slalom Sans/`
- **Lora** fonts at `/Users/bermon.painter/Downloads/Lora/Lora/`

## See also

- [`60_Digital_Manufacturing/Presentation_Factory/`](../../60_Digital_Manufacturing/Presentation_Factory/) — runtime WIP folder, Archive/, Cowork project instructions
- [`70_Templates/Slalom_Brand_Assets/`](../../70_Templates/Slalom_Brand_Assets/) — official Slalom brand assets (Brand Guide PDF, .potx, design system)
- [`80_Skills_and_Agents/solution-factory/`](../solution-factory/) — sibling factory; reference pattern
- [`80_Skills_and_Agents/proposal-factory/`](../proposal-factory/) — sibling factory; this plugin's PowerPoint output is the visual layer for proposal-factory's Polish stage
- [`80_Skills_and_Agents/consulting/`](../consulting/) — consulting POV + humanize skills
- [`80_Skills_and_Agents/design/`](../design/) — design-taste, design-imagery, design-iconography, design-visualization
- [`80_Skills_and_Agents/brand/`](../brand/) — brand-voice-tone
