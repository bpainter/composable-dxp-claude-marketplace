---
description: Build a horizon-aware value case for an innovation bet — NPV/DCF for H1, expected value for H2, real options for H3 — with kill economics and learning ROI

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Build the value case the steering committee needs to fund, advance, or kill an innovation bet. Use at every gate decision, for portfolio reallocation, and for CFO conversations about innovation ROI.

Primary skill: [[innovation-value-engineer]] (Layer 5 of the [[2026-05_Innovation-Operating-System|Innovation OS]]).

Reference template: [[template-value-engineering-canvas.md]].

Steps:

1. **Classify the horizon first.** This determines the math.
   - **H1** (0–12 months, defending / extending core) → NPV / DCF, IRR, payback
   - **H2** (1–3 years, building emerging) → expected value with scenarios; risk-adjusted NPV
   - **H3** (3+ years, creating options) → real options + strategic option value; **don't compute traditional NPV**

   The most-violated rule in corporate innovation finance: applying H1 math to H3 bets. NPV of an H3 bet is structurally negative because cash flows are unknowable in year one. That doesn't mean kill; it means use the right math.

2. **Build the 5-part value case structure** (regardless of horizon):

   - **Value hypothesis** (one sentence): "If this bet is right, [customer / segment] will [behavior / outcome] worth [magnitude] over [horizon] because [mechanism]."
   - **Mechanism** — how value is created (customer outcome) and captured (revenue model, Doblin types touched)
   - **Scenarios** — bear / base / bull with probabilities summing to 100%; probabilities must come from defensible source (analog data, validated learning, market sizing)
   - **Investment trajectory** — staged commitment with kill points per gate
   - **Kill economics** — at each gate, cumulative spend and residual value if killed

3. **Apply horizon-specific math:**

   **For H1 bets (NPV done right):**
   - Discount rate ≥ WACC + risk premium
   - Time horizon ≤5 years (3 often more honest)
   - Cannibalization modeled (net uplift, not gross)
   - Sustaining costs included (years 2–5, not just build cost)
   - Sensitivity table on top 3 drivers

   **For H2 bets (expected value with scenarios):**
   - Probability weights from defensible source
   - Risk-adjusted NPV at WACC + 5–10%
   - Strategic option value added (capability built, market presence, defensive value)

   **For H3 bets (real options):**
   - **Don't compute traditional NPV.** Reframe:
     - Discovery investment = option premium ($50K–$500K typical)
     - Scale investment = strike price (if exercised)
     - Upside if it works
     - Downside = lose discovery investment only
   - **Decision question** (not the math): Is upside ≥10x discovery cost? Is discovery cost small enough to lose? Is there real volatility in the underlying outcome?
   - Strategic option value (capability, market presence, defensive, insurance)

4. **Document kill economics per gate.** The discipline most portfolios skip.

   ```
   Gate 1 spend: $50K   |  Cum: $50K   | Residual if killed: $20K (interview transcripts, JTBD framework, audience access)
   Gate 2 spend: $250K  |  Cum: $300K  | Residual if killed: $80K (MVP IP, validated personas, brand work)
   Gate 3 spend: $1.5M  |  Cum: $1.8M  | Residual if killed: $400K (codebase, patents, partner agreements)
   ```

   Without kill economics, sunk-cost paralysis dominates funding decisions.

5. **For early-stage bets, use Learning ROI** instead of revenue ROI:
   - $ spent ÷ # critical assumptions tested
   - $ spent ÷ # invalidated hypotheses (invalidations save future bad spend; quantify avoided cost)
   - Time to invalidation
   - Pivots informed by validation

6. **For CFO conversations, prepare the language reframe.**
   - "Traditional NPV systematically underweights uncertain bets. Here's the option case the board can defend."
   - "Discovery investment is buying an option, not a business."
   - "Kill economics show what we actually risk at each gate."
   - Pair the brief with a vivid demo (impression amplifier per [[book-innovation-capital.md]]).

7. **Roll up to portfolio level when appropriate.**
   - Expected portfolio value = sum of E[V] for H1/H2 + sum of (option value × probability of exercise) for H3 bets
   - Capital efficiency = expected portfolio value ÷ total capital deployed
   - Kill capital = capital deployed on killed bets; healthy 15–30% of total
   - Identify the dominators — usually 2–3 bets account for 60%+ of portfolio expected value

8. **Hand off.** Value case feeds:
   - `/innovation:portfolio` steering committee for funding decision
   - [[innovation-monetization-strategist]] if pricing has not been validated
   - `innovation-strategist` if portfolio rebalance is implied
   - [[innovation-leadership-coach]] for sponsor / CFO conversation prep (innovation capital)

9. **Save.** `30_Engagements/Active/{client}/Value_Case_{bet}_{date}.md` or `10_Practice/Initiatives/Value_Case_{bet}_{date}.md`.

Output format — use the [[template-value-engineering-canvas.md]] one-pager. Add full financial model as supporting attachment if H1 with detailed cash flows; for H3, the canvas is sufficient.

**Recommendations follow this framework:**

```
☐ Fund this gate at $[X] for [duration]
☐ Pivot — recommended pivot: [...]
☐ Kill — residual value: $[X]; reallocate capital to: [...]
☐ Defer — pending [specific dependency / data]

Confidence: [low / medium / high]
Conditions for re-evaluation: [what would change the recommendation]
```

**Common errors this command prevents:**
- NPV-on-H3 errors (the most common)
- Made-up scenario probabilities
- Sunk-cost paralysis (kill economics counters this)
- Pricing-as-postscript (mechanism section forces pricing model upfront)
- Single-number budget asks (forces staged commitments)
- Theater metrics (learning ROI replaces revenue metrics for early-stage)
