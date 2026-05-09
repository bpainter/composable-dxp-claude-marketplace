---
type: reference
project: skills-library
scope: plugin
plugin: innovation
tags: [type/reference, plugin/innovation, scope/template, topic/value-engineering, topic/finance]
status: active
---

# Value Engineering Canvas — Template

One-page value case for an innovation bet. Horizon-aware: NPV math for H1, expected-value for H2, real-options for H3. Always includes kill economics.

## When to use

- Gate 1 / Gate 2 / Gate 3 funding decisions
- Portfolio-level capital reallocation
- CFO conversations about innovation ROI
- Re-pricing case for under-priced launches
- Strategic option valuation

## The canvas

```
═════════════════════════════════════════════════════════════════
  VALUE CASE — [Initiative]
  Date: [date]   Owner: [name]   Horizon: H[1/2/3]   Stage: [stage]
═════════════════════════════════════════════════════════════════

1. VALUE HYPOTHESIS (one sentence)
─────────────────────────────────
"If this bet is right, [customer / segment] will [behavior / outcome]
worth [magnitude] over [time horizon] because [mechanism]."

[your hypothesis]


2. MECHANISM (how value is created and captured)
─────────────────────────────────────────────────
Value created:
  • [for the customer — functional / emotional / social progress]
  • [magnitude / direction]

Value captured:
  • [pricing model]  ← coordinates with monetization strategist
  • [revenue mechanism — subscription, transaction, outcome-share, etc.]
  • [unit economics target — CAC, LTV, gross margin, payback]

Doblin types touched: [list — see ten-types-diagnostic]


3. SCENARIOS (probabilities sum to 100%)
────────────────────────────────────────
Bear case   ([X]%):  $[Y]   Why: [what would have to be true]
Base case   ([X]%):  $[Y]   Why: [what's expected]
Bull case   ([X]%):  $[Y]   Why: [what would have to break right]

E[V] = [computed expected value]

PROBABILITIES MUST BE DEFENSIBLE — analog data, validated learning,
or market sizing. Made-up probabilities are worse than no math.


4. INVESTMENT TRAJECTORY (staged, with kill points)
───────────────────────────────────────────────────
Gate 0 → 1  Discovery     | $[X]K  | Cum: $[X]K  | Decision criteria: [...]
Gate 1 → 2  Alpha / MVC   | $[X]K  | Cum: $[X]K  | Decision criteria: [...]
Gate 2 → 3  Beta / Pilot  | $[X]M  | Cum: $[X]M  | Decision criteria: [...]
Gate 3 → 4  Launch        | $[X]M  | Cum: $[X]M  | Decision criteria: [...]
Year 2+     Scale         | $[X]M  | Cum: $[X]M  | Decision criteria: [...]


5. KILL ECONOMICS (what gets retained if killed)
────────────────────────────────────────────────
At Gate 1 kill: spent $[X]K, retain $[Y]K (interview transcripts,
                JTBD framework, audience access, brand work)
At Gate 2 kill: spent $[X]M, retain $[Y]K (validated personas,
                MVP IP, brand assets, tech learning)
At Gate 3 kill: spent $[X]M, retain $[Y]M (codebase, partnerships,
                patents, sales process, talent)
At Launch kill: spent $[X]M, retain $[Y]M (operating capability,
                customer relationships, brand)


═════════════════════════════════════════════════════════════════
HORIZON-SPECIFIC ANALYSIS — fill the section for this bet's horizon
═════════════════════════════════════════════════════════════════

▼ FOR H1 BETS (use NPV / DCF) ▼
─────────────────────────────────
NPV (5-yr, [X]% discount): $[Y]M
IRR: [X]%
Payback: [X] years
Sensitivity (top 3 drivers):
  • [driver] — ±[X]% changes NPV by ±$[Y]M
  • [driver] — ±[X]% changes NPV by ±$[Y]M
  • [driver] — ±[X]% changes NPV by ±$[Y]M

Gotchas to verify:
  ☐ Cannibalization modeled (net uplift, not gross)
  ☐ Sustaining costs included (years 2–5, not just build cost)
  ☐ Discount rate ≥ WACC + risk premium
  ☐ Time horizon ≤ 5 years


▼ FOR H2 BETS (use expected value with scenarios) ▼
─────────────────────────────────────────────────────
Risk-adjusted NPV at WACC + [X]%: $[Y]M
E[V] across scenarios: $[Y]M (from section 3)
Probability weighting source: [analog data / validated learning / sizing]

Strategic option value (additional):
  • Capability built: $[X]M (talent, IP, partnerships)
  • Market presence: $[X]M (defensive value, brand authority)
  • Insurance value: $[X]M (cost to enter category later if delayed)

Total expected value: $[Y]M


▼ FOR H3 BETS (use real options) ▼
─────────────────────────────────────
This is an option, not a business. Don't compute traditional NPV.

Discovery investment (option premium): $[X]
If exercised, scale investment (strike): $[X]M
Upside if option exercises and works: $[Y]M
Downside if option doesn't exercise: lose discovery investment only

Decision question:
  ☐ Is the upside ≥ 10x the discovery cost?
  ☐ Is the discovery cost small enough to lose?
  ☐ Is there real volatility in the underlying outcome?
  ☐ Do we own the right to exercise? (Capability, customer access, IP)

Strategic option value (capability / insurance / defensive):
  • [...]

If yes to top decision questions → fund discovery. The math doesn't
have to be precise; the structure has to be right.


═════════════════════════════════════════════════════════════════

6. LEARNING ROI (use for early-stage bets)
─────────────────────────────────────────
$ spent ÷ # critical assumptions tested:        $[X] / [N] = $[Y]
$ spent ÷ # invalidated hypotheses:             $[X] / [N] = $[Y]
Time to invalidation (median):                  [N] days

Note: invalidations are valuable. They save future bad spend.


7. RECOMMENDATION
─────────────────
☐ Fund this gate at $[X] for [duration]
☐ Pivot — recommended pivot: [...]
☐ Kill — residual value: $[X]; reallocate capital to: [...]
☐ Defer — pending [specific dependency / data]

Confidence: [low / medium / high]
Conditions for re-evaluation: [what would change the recommendation]
═════════════════════════════════════════════════════════════════
```

## How to use this canvas at each gate

**Gate 1 (Discovery → Alpha)** — sections 1, 2, 4, 6 fully completed; section 3 draft scenarios; section 5 first-pass; horizon-specific section as best-guess. Heavy on Learning ROI (section 6).

**Gate 2 (Alpha → Beta)** — sections 1–6 fully completed with validated assumptions reflected. Probabilities should now have evidence. Horizon-specific section uses real WTP signal.

**Gate 3 (Beta → Launch)** — full canvas with pilot data; all probabilities should have empirical anchors. Horizon-specific section is final-form for the funding decision.

**Post-launch / quarterly** — update sections 3, 4, 5 with actuals; rebalance the canvas if reality diverges from base case.

## Common errors this canvas prevents

- **NPV-on-H3 errors** — the horizon-specific sections force the right math
- **Made-up probabilities** — section 3 demands a probability source
- **Sunk-cost paralysis** — section 5 (kill economics) shows what's salvaged
- **Pricing-as-postscript** — section 2 mechanism forces the pricing model upfront
- **Single-number budget asks** — section 4 forces staged commitments
- **Theater metrics** — section 6 (learning ROI) replaces revenue metrics for early-stage work

## Hand-off

Skill owners:

- `innovation-value-engineer` — primary owner; runs the math
- `innovation-monetization-strategist` — feeds section 2 (value capture, pricing)
- `innovation-portfolio-architect` — uses canvas at gate reviews
- `innovation-method-validator` — feeds learning data into section 6
