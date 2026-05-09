---
type: reference
project: skills-library
scope: plugin
plugin: innovation
tags: [type/reference, plugin/innovation, scope/template, topic/three-horizons]
status: active
---

# Three Horizons Canvas — Template

A working canvas for classifying current portfolio bets across the three horizons and surfacing gaps. Use during portfolio audits, strategy refreshes, and lab charters.

## When to use

- Quarterly portfolio steering committee
- New innovation strategy stand-up
- Lab charter design
- "Our portfolio feels unbalanced" diagnostics

## The canvas

```
═══════════════════════════════════════════════════════════════
  THREE HORIZONS CANVAS — [Org / Practice]   [Date]
═══════════════════════════════════════════════════════════════

┌─ HORIZON 1 ──────────── Defend & extend the core ─────────────┐
│ Time frame: 0–12 months                                       │
│ Default capital allocation: ~70%                              │
│ Governance: Stage-gate                                        │
│ Metrics: NPV, payback, NRR, retention, margin                 │
│                                                               │
│ Active bets:                                                  │
│  • [Initiative]   Owner: [name]   Stage: [stage]   Health: [] │
│  • [Initiative]   Owner: [name]   Stage: [stage]   Health: [] │
│                                                               │
│ Capital deployed: $[X]M     Headcount: [N]                    │
└───────────────────────────────────────────────────────────────┘

┌─ HORIZON 2 ──────────── Build emerging businesses ────────────┐
│ Time frame: 1–3 years                                         │
│ Default capital allocation: ~20%                              │
│ Governance: Stage-gate + discovery-driven planning            │
│ Metrics: pilot adoption, unit economics, market validation    │
│                                                               │
│ Active bets:                                                  │
│  • [Initiative]   Owner: [name]   Stage: [stage]   Health: [] │
│                                                               │
│ Capital deployed: $[X]M     Headcount: [N]                    │
└───────────────────────────────────────────────────────────────┘

┌─ HORIZON 3 ──────────── Create viable options ────────────────┐
│ Time frame: 3+ years                                          │
│ Default capital allocation: ~10%                              │
│ Governance: Discovery-driven, real-options                    │
│ Metrics: option value, capability built, learning velocity    │
│                                                               │
│ Active bets:                                                  │
│  • [Initiative]   Owner: [name]   Stage: [stage]   Health: [] │
│                                                               │
│ Capital deployed: $[X]M     Headcount: [N]                    │
└───────────────────────────────────────────────────────────────┘
```

## Diagnostic prompts

After completing the canvas:

1. **Allocation match.** Is the actual capital split close to the strategic target? If H1 holds 95% of capital, the strategy is "defend the core" regardless of what the deck says.
2. **Headcount drift.** Often headcount skews more H1-heavy than capital because H1 work is bigger. That's fine. The failure is when capital *also* skews H1-heavy beyond mandate.
3. **H3 emptiness.** Most enterprise portfolios have zero or one H3 bet. If the mandate is anything other than "defend the core," that's a portfolio gap.
4. **Governance mismatch.** Are H3 bets being run with stage-gate governance? Move to discovery-driven planning before you kill them on criteria they can't satisfy.
5. **Metric mismatch.** Are H3 bets being held to NPV / revenue metrics? Switch to learning ROI and option value.
6. **Aging.** How long has each bet been at its current stage? >1.5x median in stage = stalled. Force a decision.

## Common failure patterns to surface

- **All-H1 portfolio dressed as transformation** — every bet is a sustaining improvement; transformation framing is theater. Recommend: introduce explicit H2/H3 bets with horizon-appropriate governance, or rename the program "modernization."
- **H3 bets with H1 metrics** — the gates are killing them. Recommend: adopt real-options and learning ROI for H3 specifically.
- **H2 stuck-in-pilot purgatory** — successful pilots that never advance to launch. Recommend: pre-commit scale-up funding at Gate 2; assign a launch owner inside an operating BU.
- **Activity rich, capital thin** — many H3 ideas exist as side projects with no funding. Recommend: pick 2–3, fund them properly, kill the rest.

## Capital reallocation worksheet

Once gaps are surfaced:

| Bet | Current horizon | Recommended | Action | Capital change |
|---|---|---|---|---|
| | | | Continue / Kill / Pivot / Promote | $[+/-]M |

## Hand-off

Pair this canvas with:

- `template-ambition-matrix.md` — risk-based view (different axis from time-based)
- `template-value-engineering-canvas.md` — value math by horizon
- `template-innovation-charter.md` — codify the mandate that should drive allocation

Skill owners:

- `innovation-strategist` — uses to set/reset ambition mix
- `innovation-portfolio-architect` — uses to design governance per horizon
- `innovation-value-engineer` — uses to pick the right value math per bet
