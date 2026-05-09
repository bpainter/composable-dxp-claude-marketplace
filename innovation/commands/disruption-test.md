---
description: Apply Christensen's disruption test rigorously — diagnose position (incumbent vs. disruptor, low-end vs. new-market), surface response patterns, refuse misuse of "disruption" as marketing

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Apply the disruption test rigorously to a competitive situation. Use when "are we being disrupted?" surfaces, when a startup claims "disruptive innovation," when an incumbent client is afraid of a new entrant, or when leadership is using "disruption" as marketing language.

Primary skill: [[innovation-disruption-analyst]] (cross-layer in the [[2026-05_Innovation-Operating-System|Innovation OS]]).

Source: [[book-innovators-dilemma.md]] + Christensen, Raynor, McDonald, *What Is Disruptive Innovation?* (HBR Dec 2015).

Steps:

1. **Apply the four-test rigorously.** A disruptive innovation has all four characteristics:

   ☐ **Started at the low end or in a non-consumption market.** Not better at what mainstream customers want, but acceptable for under-served or non-consuming customers.

   ☐ **Rides a steep performance trajectory.** Typically a different technology / business model curve that improves faster than incumbent's curve.

   ☐ **Eventually reaches mainstream "good enough."** At which point incumbents lose their core market.

   ☐ **Wins on a different value dimension.** Convenience, price, accessibility, simplicity — not the dimension incumbents compete on.

   **If a new entrant is *better* at what incumbents already do well, it's sustaining innovation, not disruption.** Incumbents usually win sustaining battles. Most "disruption" claims fail this test.

2. **Diagnose the position.** Map to one of four:

   - **Position A: Incumbent facing low-end disruption** — cheaper entrant taking your low-margin tier; you're happy to lose those customers; entrant is climbing toward your core
   - **Position B: Incumbent facing new-market disruption** — entrant serving customers who weren't your customers; non-consumption space
   - **Position C: Low-end disruptor** — serving over-shot customers at lower price; performance trajectory steep
   - **Position D: New-market disruptor** — enabling new consumption; lower performance in incumbent metrics, higher on new dimensions

3. **Run the over-shoot diagnostic.** Often missed but creates the conditions for disruption:

   ☐ Are customers paying for features they don't use?
   ☐ Is the price-performance curve too steep — each performance increment increasingly costly with diminishing customer value?
   ☐ Is "good enough" emerging as a credible category?
   ☐ Are switching costs the only thing holding customers in the premium tier?

   If yes to any 2 of 4 → over-shoot risk; disruption is structurally possible.

4. **Surface the textbook failure patterns.** Christensen's contribution wasn't just describing disruption — it was diagnosing why incumbents respond systematically badly. Flag these explicitly when present:

   - **"Listening to your best customers"** — when best customers want sustaining improvements, listening blinds you to disruptors
   - **"Margin-driven product roadmap"** — innovation budget flows to highest-margin products, which are the over-shot tier
   - **"Same metrics for new units"** — applying core-business margin/growth/utilization metrics to a low-end response unit kills it
   - **"Consolidating the response into core"** — moving the disruption response back into the core; destroys the cost structure
   - **"Acquiring and integrating"** — buying the disruptor and folding into the mother ship; destroys what made it work

5. **Recommend the response pattern.** The reliable response for incumbents under disruption: **a separate organizational unit with separate P&L, separate metrics, separate decision rights, geographically and culturally distinct.** Charter elements (coordinate with `/innovation:charter`):

   - Separate budget; not subject to core business cost cuts
   - Different metrics — adoption velocity, learning, market presence — not core margin
   - Separate talent path
   - Decision rights to ship at lower quality / lower margin than core would tolerate
   - Multi-year runway (≥3 years)

   For disruptors (Positions C and D): protect the cost-advantaged business model; ascend deliberately; don't get drawn upmarket too fast and lose the cost structure that made you possible.

6. **Refuse misuse.** Push back on common misuses of "disruption" as marketing:
   - A new feature is *not* disruption
   - A faster competitor is *not* disruption
   - A price war is *not* disruption
   - Industry consolidation is *not* disruption
   - A platform shift (mobile, cloud, AI) creates *conditions* for disruption but isn't itself disruption

   If the situation is sustaining innovation pressure, recommend sustaining-innovation response (catch-up R&D, defend with switching cost, accept margin compression). Disruption response (separate unit, multi-year runway) is expensive overkill for sustaining-innovation pressure.

7. **Apply context-specific frames where applicable.**

   - **Composable / headless replacing monoliths:** partial disruption + sustaining-innovation hybrid. Composable serves a job (modularity, pace) that monoliths overshot for some customers; for others, monolith is still right. Watch the trajectory.
   - **AI-native experiences:** new-market disruption where AI enables jobs that weren't being done; sustaining innovation where AI augments existing experiences.
   - **DTC brands replacing traditional retail:** classic low-end + new-market hybrid. Textbook study.
   - **AI replacing professional services:** partial disruption underway. Junior-consultant / analysis-heavy work most exposed; senior advisory and complex execution sustaining-innovation territory.

8. **Hand off.** Diagnostic feeds:
   - `/innovation:charter` for separate-unit response (if disruption confirmed)
   - `/innovation:strategy` for mandate redesign if response is needed
   - `/innovation:value-case` for build-vs-buy economics if acquiring a disruptor is on the table
   - [[innovation-monetization-strategist]] for low-end pricing strategy
   - [[innovation-leadership-coach]] for sponsor protection of the separate unit
   - [[consulting-management-consultant]] if the response involves M&A or org restructure

9. **Save.** `30_Engagements/Active/{client}/Disruption_Diagnostic_{date}.md` or `10_Practice/Strategy/Disruption_Diagnostic_{topic}_{date}.md`.

Output format:

```
# Disruption diagnostic — {Situation} — {Date}

## Test results
☐ Started at low end or non-consumption?  {pass/fail/partial}
☐ Steep performance trajectory?            {pass/fail/partial}
☐ Will reach mainstream "good enough"?     {pass/fail/partial}
☐ Different value dimension?               {pass/fail/partial}

## Diagnosis
- Position: {A / B / C / D / N-A (sustaining innovation)}
- Over-shoot risk: {yes / no / partial}
- Time horizon to crossover: {1–2 yr / 3–5 yr / 5–7 yr}

## If sustaining innovation (test fails)
- Re-frame the situation
- Recommend sustaining-innovation response (catch-up R&D, defend, accept margin compression)
- Refuse the disruption framing in strategy docs

## If disruption (test passes)
- Recommended response: separate-unit pattern
- Charter elements (hand to /innovation:charter)
- Failure patterns to avoid (5 textbook ones)
- Multi-year runway commitment required
- Sponsor protection plan

## Anti-pattern brief
- Which textbook failure pattern is the org most likely to fall into?
- Specific intervention to prevent it

## Routing
- /innovation:charter (if separate unit)
- /innovation:strategy (if mandate change)
- /innovation:value-case (if M&A on the table)
- [other handoffs]
```

**Naming things accurately is half the strategic work.** Most "disruption" engagements should be reframed as sustaining-innovation engagements; that produces better strategy and saves years.
