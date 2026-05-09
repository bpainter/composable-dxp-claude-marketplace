---
type: reference
project: skills-library
scope: plugin
plugin: project-management
tags: [type/reference, plugin/project-management, scope/foundational, topic/lean-product-development, topic/flow, source/reinertsen]
status: active
---

# The Principles of Product Development Flow — Donald G. Reinertsen (Celeritas, 2009)

**Source digest** for the project-management plugin. Reinertsen organizes content into **175 numbered principles** prefixed by chapter — E (Economics), Q (Queues), V (Variability), B (Batch Size), W (WIP), F (Flow), FF (Fast Feedback), D (Decentralization). Cite as "(Reinertsen, *Flow*, Principle E3)". The principle number is the canonical citation.

## Thesis

The dominant paradigm of PD management is **wrong at its core** — it treats development as deterministic manufacturing (chase efficiency, drive out variability, manage to milestones) and produces the universal pathology: 98.5% utilization, invisible mountains of DIP, cycle times an order of magnitude too long (Ch. 1).

Reinertsen's **second generation lean** rests on three claims:

1. **PD is a stochastic queueing system**, not a manufacturing line. Non-repetitive, non-homogeneous, high-variability arrivals and service times. FIFO and utilization-maximization are economically wrong.
2. **Economic decisions, not proxy variables, drive outcomes.** Cycle time, utilization, value-added %, waste — all proxies. Translate every choice into life-cycle profit impact (E1, E2).
3. **Variability is not waste** — it's the raw material of option value when payoffs are asymmetric (V1, V2). Eliminating variability eliminates innovation.

The signature move: convert fuzzy lean/agile intuitions ("small batches," "fast feedback," "limit WIP") into quantitative principles backed by queueing theory, control theory, and information theory. Eric Ries: "the most advanced product development book you can buy."

## Cost of Delay (CoD) and CD3

**Definition (E3).** Dollar value of life-cycle profit lost per unit of time the product is late. Shipping a week late forfeits $50K → CoD = $50K/week.

**Calculation.** Sensitivity analysis: vary cycle time alone, hold the other four levers (product cost, value, dev expense, risk) constant, observe life-cycle profit change (E1). 85% of companies have never done this.

**CD3 = CoD ÷ Duration (F17).** WSJF — Weighted Shortest Job First. When delay costs and durations are heterogeneous, sequence by CoD ÷ Duration to minimize total delay cost. SAFe's WSJF is taken directly from this.

**Why it dominates.** All other proxies (queue size, batch size, utilization, variability reduction, speed) collapse to comparable units only via CoD. Without CoD you cannot evaluate queue cost, capacity value, batch-size benefit, or variability reduction. "The golden key that unlocks many doors" (E3).

**Examples.** Boeing 777 published $300/lb for weight reduction so 1,000 engineers could decide consistently without escalation (E13, FF13). Reinertsen asks 10 people on the same project to estimate CoD: answers span 50:1.

## Queue theory for product development

**Why queues are the hidden cost.** PD inventory is *invisible* — bits on a disk, not pallets on a floor. CFOs report zero because R&D is expensed. Yet design-in-process (DIP) creates six wastes: longer cycle time, higher risk, more variability, more overhead, lower quality (delayed feedback), demotivation (Q1, Q2). 100% of developers measure cycle time; 2% measure queues; 15% know CoD. The blindness is the central problem.

**Little's Law (Q12).** Wait Time = Queue Size ÷ Processing Rate. Robust across all disciplines. Predict cycle time directly from WIP and throughput — no need to measure utilization. 50 projects in flight, 10/year completed → 5-year cycle time.

**Exponential utilization curve (Q3).** For an M/M/1 queue, queue size grows exponentially as utilization → 100%. Each halving of excess capacity *doubles* queue size: 60→80% doubles; 80→90% doubles; 90→95% doubles; 95→98% doubles. Average product developer runs at **98.5% utilization** — meaning 98.5% of cycle time is waiting.

**Variability amplification (Q5, Q6).** Queue size is proportional to the *square* of the coefficient of variation. Operating high on the curve amplifies whatever variability you have — a self-inflicted wound.

## Batch size principles

Small batches reduce cycle time, variability, risk, and overhead while accelerating feedback (Ch. 5).

**Ten benefits (B1–B10):** cycle time drops linearly (B1); variability falls — bus-of-80-tourists effect (B2); feedback accelerates (B3); risk decreases via V7 plus shorter cycle time (B4); overhead falls — 300 open bugs need more cross-checking than 30 (B5); efficiency improves — debug complexity grows as 2^n (B6); motivation rises (B7); slippage scales with the *fourth power* of duration (B8); death-spiral large projects attract scope (B9); least-common-denominator effects vanish (B10).

**"Bake-In-The-Cake" effect** (B5, B7): large batches bake problems into all in-flight work before feedback arrives. 200 drawings reviewed at once = 200 drawings sharing the same wrong assumption.

**U-curve trade-off (B11, B12).** Optimum batch size balances holding cost (queue) vs. transaction cost (overhead per batch). Toyota insight: aggressively reduce *transaction cost*, then drop batch size. Err small — U-curve is shallow at the bottom and transaction cost falls as you get good.

## WIP constraints (limiting work in progress)

**Kanban roots (W4).** Reinertsen grounds WIP control in Toyota's kanban — local pools, downstream-pulls-from-upstream. Theory of Constraints (W3) is the alternative: a single global pool tied to the bottleneck. Reinertsen prefers kanban for PD because PD bottlenecks are stochastic; kanban responds locally, 400× faster than weekly MRP.

**Why WIP limits force prioritization.** When WIP hits cap, new work is blocked. The team must reject demand, shed work, or shed scope (W6–W8) — forcing the value conversation. Without WIP limits, "yes" is free.

**Applied to PD.** A 2× average WIP cap saves ~28% cycle time at ~1% utilization penalty — roughly 10:1 payoff (Figure 6-3). Mechanisms: visual boards (W23), demand blocking (W6), purging (W7), resource pulling (W9), T-shaped people (W12), preplanned escalation (W16). Small reductions compound (W22) — start small, ratchet down.

## Variability principles

Variability isn't waste — it's the raw material of value when payoffs are asymmetric (Ch. 4).

- **V1.** Path A (50% × $100K, EV $35K) beats Path C (100% × $16K, EV $1K). Eliminating variability eliminates the option.
- **V2.** Black-Scholes: volatility *creates* option value when downside is bounded. PD has bounded downside, large upside — exploit it.
- **V3.** Neither minimize nor maximize. PD has unbounded downside (spend forever fixing bad specs), bounded upside. Interior optimum.
- **V4.** 50% failure maximizes information per test. Most teams test for ≤5% and waste their information budget. Distinguish *exploratory* (50%) from *validation* (100%).
- **V5–V14** — pool variability (V5), forecast short (V6), break bets small (V7), substitute cheap variability for expensive (V14: trade scope for schedule).

Reducing variability is valuable only with symmetric or inverted-U payoffs (manufacturing). In PD, *exploit* asymmetry.

## Cadence and synchronization

Cadence — regular, predictable rhythm — lowers transaction cost and prevents variance from accumulating (F5–F9).

- **F5 — Periodic Resynchronization.** Variance accumulates in unbuffered chains (bus-bunching). Fixed cadence resets to the control-range center, isolating variance to one interval.
- **F7 — Cadence Reliability.** Predictable rhythm makes wait times bounded. Miss a 2-week launch? Next is 2 weeks away — no panic-feature-stuffing.
- **F8 — Batch Size Enabling.** A fixed weekly review forces a one-week batch. Cadence and small batches reinforce each other.
- **F9 — Cadenced Meetings.** Weekly Wednesday 1pm has zero scheduling overhead; "as-needed" takes 3 days to schedule. **Daily standups respond 3× faster than weekly, same total meeting time.**

**Synchronization (F10–F14).** Aligning streams to one rhythm exploits scale economies (F11), batches cross-functional handoffs (F12), shrinks queues without changing batch size or capacity (F13). Use *harmonic* nested cadences — daily inside weekly inside quarterly (F14). "Routine harvest meetings" — cadenced economic reviews where the team reaps accumulated decisions.

## Decentralized control

**Core rule (D1, D2):** *Decentralize for problems that age poorly; centralize for problems that are infrequent or have scale economies.*

- **D1 — Second Perishability.** Fires, market windows, emergent needs, integration bugs — push decision and resources to the edge.
- **D2 — Scale.** Heavy artillery, regulatory experts, compliance reviews — centralize. Pool variable demand (V5).
- **D3 — Layered Control.** When severity is unclear at intake, use triage in front and *automatic escalation* in the rear.
- **D6 — Inefficiency.** Decentralized resources will be locally inefficient; pay for response time. CPR is taught to many who'll rarely use it.
- **D7–D11 — Mission Command** (Marine Corps): specify *end state + intent* (D8), set *boundaries* (D9), name a *main effort* (D10), reset alignment dynamically (D11). Plans are alignment tools, not conformance tools (D4).
- **D17 — Decentralized Information.** Decentralized control needs decentralized economic data — push CoD, decision rules, and current state to the edge.

Rule of thumb: high-frequency / low-impact → edge. Low-frequency / high-impact → center. In between → escalation ladders.

## Fast feedback

Fast feedback works on *both sides* of the asymmetric payoff function: truncates losing paths early (left tail), exploits emergent opportunities (right tail) — structurally different from manufacturing feedback, which only reduces variation around a fixed target (FF1).

**Why a 1-week feedback loop trumps a 6-week one, even at higher cost:**

- Bad assumptions baked into N decisions while feedback is delayed; rework grows geometrically in delay (FF11).
- Work decays in working memory. 24-hour feedback finds the engineer remembering what she was doing; 90-day feedback meets a stranger (B6).
- Faster loops shrink the planning horizon, which by V6 makes the residual forecast exponentially more accurate.

**Mechanisms:** small batches as feedback amplifiers (FF11), local before global (FF14), nested fast/slow loops (FF16), feed-forward of arrival rate (FF18), colocation (FF19). **Decision rules over decisions (FF13):** control the *economic logic* ("save weight up to $300/lb"), not the decision itself.

## Eight principles worth pulling out as quick-reference

| # | Principle | Concept |
|---|---|---|
| 1 | **E3 — Cost of Delay** | If you only quantify one thing, quantify CoD. Makes every trade-off comparable. |
| 2 | **Q3 — Utilization** | Queues grow exponentially with utilization. Each halving of slack doubles the queue. Plan to 60–80%. |
| 3 | **Q12 — Little's Formula** | Cycle Time = WIP ÷ Throughput. Predict cycle time without measuring utilization. |
| 4 | **B1 — Batch Size Queueing** | Cycle time scales with batch size at unchanged capacity. |
| 5 | **W1 — WIP Constraints** | Constrain WIP, not utilization or cycle time. WIP leads; cycle time lags. |
| 6 | **V2 — Asymmetric Payoffs** | When downside is bounded and upside large, variability creates option value. |
| 7 | **F17 — WSJF / CD3** | Sequence by CoD ÷ Duration. Economically optimal queue discipline. |
| 8 | **D1 + D2 — Perishability vs. Scale** | Decentralize what ages poorly; centralize what is infrequent or has scale. |

Bonus: **F9 — Cadenced Meetings** and **FF13 — Decision Rules**.

## Cross-references

**vs. Lean / TPS (Toyota).** Reinertsen *extends* Toyota — borrows EOQ, kanban, takt directly — but argues lean-manufacturing prescriptions break in PD because manufacturing assumes repetitive, homogeneous, deterministic flows. PD is stochastic, one-of-a-kind. FIFO, variability suppression, and eliminate-all-waste are wrong here. "Second generation lean" is the subtitle.

**vs. Agile.** Reinertsen *predates* the scaled-agile literature (SAFe arrives 2011) and provides the rigorous economic foundation most agile texts gesture at. WSJF (F17), batch size (Ch. 5), WIP limits (Ch. 6), cadence (Ch. 7), fast feedback (Ch. 8) are all agile orthodoxy now; SAFe cites Reinertsen as backbone. Use Reinertsen for *why* and *how much*; agile for ceremony.

**vs. Perri (*Escaping the Build Trap*).** Perri is outcomes-driven product management (*what* to build). Reinertsen is flow economics (*how* to run the pipeline once the what is decided). CoD is the bridge — Perri's outcome metrics translate into CoD per feature.

**vs. Gothelf / Lean Startup (Ries).** Lean Startup operationalizes V2/V4 (asymmetric payoffs, 50% failure) and FF1/FF11 (MVPs, validated learning) at the venture-discovery level. Reinertsen names the physics; Lean Startup gives the workflow.

## How `project-management-flow-engineer` (NEW skill) should use it

Direct application of Reinertsen. Pull list:

1. **E1 + E3 — Quantified economics + CoD.** Every recommendation terminates in life-cycle profit impact via CoD. If the team can't articulate CoD, that's the first deliverable.
2. **Q12 — Little's Law diagnostic.** Count WIP, divide by completion rate, get cycle time. Catches "we're working on everything" instantly.
3. **Q3 — Utilization curve.** Plot the team on the M/M/1 curve. Prescribe 60–80% slack.
4. **B1 + B11 — Batch size and U-curve.** Audit everywhere (PR size, deploy frequency, requirement chunks, review cadence). Reduce transaction cost first.
5. **W1 + W4 — WIP limits, kanban-style.** Local pools per stage; visual board (W23); demand blocking (W6) when caps hit.
6. **F5 + F8 + F9 — Cadence.** Install ceremonies at frequencies that enforce small batches. Daily beats weekly at same time cost.
7. **F17 — WSJF / CD3.** Replace P1/P2/P3 with CD3 when CoD and durations are heterogeneous.
8. **FF14 + FF16 — Nested feedback.** Fast inner loops (CI, standups) inside slower outer loops (sprint, quarter).
9. **FF13 — Decision rules.** Surface the economic logic ("trade $X engineering hours for 1 week cycle time"). Push to ICs.
10. **D1 + D2 — Perishability vs. scale.** Decide edge vs. center; document the escalation ladder (D3).

## How `project-management-sprint-planning` should reference it

1. **Q3 — Plan to 60–80% utilization.** Reserve 20–40% for variability and emergent work. The 98.5% datapoint is the cautionary tale.
2. **Q12 — Little's Law.** WIP ÷ throughput gives cycle time — more reliable than story-point forecasts when WIP is changing.
3. **B1 — Story slicing.** Lower cycle time, faster feedback, lower risk, less overhead, higher motivation.
4. **W1 — Per-column WIP limits.** Constrain per status, not just total commitment. Forces prioritization.
5. **F17 / WSJF.** Default to CD3 when stakeholders disagree. Stops loudest-voice-wins.

## Pitfalls — common Reinertsen-misreadings

1. **Treating everything as a queue.** Queue math applies to repeatable handoffs and gates, not a research scientist's morning of thinking. Don't kanban what doesn't flow.
2. **Ignoring variability's value.** Common misread: "reduce variability." He says **exploit asymmetry**. V3: do not minimize, do not maximize. Six Sigma in PD destroys innovation.
3. **Optimizing utilization.** "Everyone is busy" feels like good management; it's the failure mode. Slack must be defended with CoD numbers. Build CoD before attacking utilization.
4. **Confusing CoD with priority.** CoD is a *rate*; priority is a *sequence*. Conversion is CD3, not raw CoD. Raw CoD prioritizes long expensive jobs over short cheap ones — wrong (F15–F17).
5. **WSJF / CD3 without quantifying CoD.** "High/medium/low" CoD defeats the model. Quantify, or admit you're using a heuristic.
6. **Cadence as ritual.** Value comes from *predictability* and *resynchronization*. Cadenced theater has the cost without the benefit.
7. **Decentralization without decision rules.** D1 pushes decisions to the edge; FF13 pushes the *economic logic* with them. Without the rules: 50:1 variance in the same decision.
8. **Treating principles as independent.** They interlock — small batches require low transaction cost (B12), which requires cadence (F8), which requires capacity margin (F6). Single-lever moves usually fail.

## Pull-quotes worth keeping verbatim

- "If you only quantify one thing, quantify the cost of delay." (E3)
- "Queues are the root cause of the majority of economic waste in product development." (Q2)
- "Variability is only desirable when it increases economic value." (V3)
- "Don't control capacity utilization, control queue size." (Q13)
- "A little rudder early is better than a lot of rudder late." (Ch. 8 epigraph)
