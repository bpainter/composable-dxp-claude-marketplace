---
type: reference
project: skills-library
scope: plugin
plugin: solution-factory
tags: [type/reference, plugin/solution-factory, scope/foundational, topic/solution-factory]
status: active
---

# Solution Factory — Foundations

Plugin-wide shared reference. Cited by every skill in the `solution-factory` plugin.

## What the Solution Factory is

The Slalom Solution Factory produces ready-to-sell solution kits from idea to promoted artifact. It lives at [`60_Digital_Manufacturing/Solution_Factory/`](../../../60_Digital_Manufacturing/Solution_Factory/).

Each kit ships **8 deliverables**:

1. **One-Pager** — single slide, 60-second relevance scan
2. **Sales Enablement Kit** — 30–40 slide internal reference
3. **Discovery Workshop** — 4–6 slide engagement template
4. **First Call Deck** — 20–40 slide client-facing introduction
5. **Case Studies** — 1–2 slides each, reference-story format
6. **Email Templates** — outbound, nurture, follow-up
7. **Handoff Packet** — 10-item kit bridging sales close → engagement kickoff
8. **Marketing Handoff** — LinkedIn / social / Marketing_Factory handoff

## The four-stage pipeline

```
Frame  →  Draft  →  Polish  →  Ship
~30min   ~hours    ~hours    ~30min
```

- **Frame** — intake. Capture the 5 required calls (Reframe / Deb Oler / Buyer State / Posture / Tier) plus minimum viable problem statement. Output: `00_Intake_Brief.md` in a `WIP/[solution-name]/` folder.
- **Draft** — markdown-first drafting of all 8 deliverables. No formatting yet. Output: 8 markdown files in WIP.
- **Polish** — visual generation (Napkin / Figma / higgsfield / Flaticon), brand application (Slalom default vs. Composable DXP secondary), output formatting (16:9 PPTX, 8.5×11 PDF, optional microsite). Output: PPT/PDF/HTML files at the top level of WIP.
- **Ship** — Quality Gates checklist, promotion routing by Brand. Output: kit at canonical location.

## The five required calls (made at intake)

These determine every downstream decision. They MUST be captured before drafting begins.

1. **Reframe headline** — the one-line contrarian claim. Reads aloud, produces *"huh, I never thought of it that way"* — not *"yes, exactly."*
2. **The Deb Oler answer** — *"Why should our customers buy from us over anyone else (specifically for this solution)?"* If unanswerable, the kit isn't ready.
3. **Dominant Buyer State** — Latent / Admitted / Vision / Evaluation. Drives First Call Deck variant + qualification posture.
4. **Posture** — TO / FOR / WITH / BY them (Mabee). Slalom default is **WITH them**. Set in pursuit; inherited by delivery.
5. **Tier** — Advisory / Simple / Complex. Maps to engagement level (Low Touch / Standard / Complex). **Drives the promotion target** at Stage 4.

## Brand selection (also at intake)

Two brands. Pick one.

- **Slalom default** — for solutions outside the Composable DXP line (AI Transformation, Future of Marketing, Contact Center Transformation, Zero Legacy, etc.). Brand framework: `50_Knowledge/Frameworks/Slalom_Brand/`. Brand assets: `70_Templates/Slalom_Brand_Assets/`.
- **Composable DXP secondary** (TFIC — *The Future Is Composable*) — for DXP-line solutions (Composable DXP migrations, headless commerce/content, MACH architecture, anchored on Contentful/Vercel/Algolia/Bynder). Brand framework: `50_Knowledge/Frameworks/Composable_DXP_Brand/`. Brand assets: `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/`.

**Brand selection drives promotion routing.** See "Promotion routing" below.

## Promotion routing (Stage 4 Ship)

Single source of truth. A solution exists as a full kit in **one** canonical location, with optional reference stubs for discoverability.

```
At Stage 4 Ship, route by Brand selected at intake:

Brand = Composable DXP secondary
└── Canonical:  10_Practice/Offerings/Solutions/{Tier}/[Solution-Name]-[YYYY-MM]-Slalom/
    │           where {Tier} ∈ {Advisory | Simple | Complex}
    └── Library reference stub:  40_Library/Solution_Briefs/[Solution-Name]-[YYYY-MM]-Slalom.md
        (single-file pointer back — NOT a folder copy)

Brand = Slalom default
└── Canonical:  40_Library/Solution_Briefs/[Solution-Name]-[YYYY-MM]-Slalom/
    └── (no reference stub — already in the discovery surface)
```

The reference stub for Composable DXP solutions is a small markdown file with frontmatter, a link to the canonical practice path, and a brief summary. Not a kit copy.

## Foundation knowledge the factory consumes

Skills in this plugin reference (don't duplicate) these knowledge surfaces:

| Surface | What it provides |
|---|---|
| [`50_Knowledge/Frameworks/Slalom_Operating_System/`](../../../50_Knowledge/Frameworks/Slalom_Operating_System/00_Master_Operating_System.md) | The canonical end-to-end OS — vocabulary glossary, four postures, per-stage playbook, anti-patterns. **Every template references this.** |
| [`50_Knowledge/Frameworks/Sales_Theory/`](../../../50_Knowledge/Frameworks/Sales_Theory/) | Pursuit-side depth — Strategic Selling, Solution Selling, Challenger, Collaborative Sale, Collins, EOS |
| [`50_Knowledge/Frameworks/Pursuit_to_Delivery/`](../../../50_Knowledge/Frameworks/Pursuit_to_Delivery/) | The boundary — Sponsor Letter ⇋ Block contracting; Collaboration Plan ⇋ mutual commitments; Win-Results ⇋ wants/offers |
| [`50_Knowledge/Frameworks/Slalom_Capabilities/`](../../../50_Knowledge/Frameworks/Slalom_Capabilities/README.md) | The 7 Slalom capability areas — used in Intake (Cloud_Build / CX / Data_and_AI / Delivery / Strategy / Transformation / Enterprise) |
| [`10_Practice/Offerings/Solutions/`](../../../10_Practice/Offerings/Solutions/) | The Composable DXP practice catalog — promoted solutions live here for DXP brand |
| [`70_Templates/Slalom_Brand_Assets/`](../../../70_Templates/Slalom_Brand_Assets/) | Slalom .potx, brand guide PDF, web design system |
| [`70_Templates/Composable_DXP_Brand_Assets/`](../../../70_Templates/Composable_DXP_Brand_Assets/) | TFIC design system bundle |

## Anti-patterns this plugin prevents

The 15 unified anti-patterns from the Slalom OS, plus Solution Factory-specific gates. Lifted from `60_Digital_Manufacturing/Solution_Factory/Quality_Gates.md`. **Any kit that fails 2 or more is rejected for revision.**

The five most-violated ones (worth memorizing):

1. **Lead with Slalom** — logo / services bar in the first 30% of any first-touch artifact. Anti-Challenger.
2. **Generic "we transform" framing** — no Reframe, no quantified pain, no Win-Results.
3. **Win without Win** — closing on a personal stakeholder Win that wasn't named. Strategic Selling: Win-Lose degrades to Lose-Lose.
4. **Boundary failure** — sales-side and delivery-side use different vocabularies. Pain Chain dissolves; stakeholder map rebuilt fresh.
5. **Free consulting** — insight that doesn't terminate at a Slalom-specific capability.

## How the orchestrators use this reference

Every orchestrator skill in this plugin cites this foundations file in its body:

> "For the canonical pipeline, routing logic, and anti-patterns, see `../../references/solution-factory-foundations.md`."

This keeps each skill focused on its stage's responsibility without re-explaining the system.
