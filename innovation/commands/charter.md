---
description: Design or recharter an innovation lab / center / venture studio — archetype, charter, operating model, KPIs, Ten Faces role design, survival mechanics

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Design a new innovation lab from scratch, or recharter a failing / drifting one. Use when the strategy decision requires lab-vs-line, when a board mandates "build us a lab," or when an existing lab is at year-2 collapse risk.

Primary skill: [[innovation-lab-architect]] (Layer 6 of the [[2026-05_Innovation-Operating-System|Innovation OS]]).

Reference template: [[template-innovation-charter.md]] (the 8-section charter).

Steps:

1. **Run the "what protects what" diagnostic.** Refuse to design a lab without it.

   | If the lab protects... | ...from what? | Lab type |
   |---|---|---|
   | Learning velocity | Quarterly delivery cadence | Discovery lab |
   | H3 bets | H1 capital starvation | Ventures studio |
   | Talent | Operating-BU politics | Studio / CoE |
   | Brand novelty | Brand orthodoxy | Innovation showcase |
   | Customer co-creation | Internal product silos | CX / experience center |
   | Nothing in particular | (rationalized later) | THEATRE — refuse |

   If the answer is "nothing in particular" or "the board wants one," refuse the engagement or reframe the request. Mandate-confused labs fail in 18–24 months.

2. **Pick the archetype** (don't blend more than two):
   - Discovery Lab — research / JTBD / validation; doesn't ship products
   - Venture Studio — builds new businesses, separate P&L
   - Center of Excellence — owns a capability across the org
   - Customer Experience Center — physical / virtual demos and co-creation
   - Corporate Accelerator — cohort programs with external startups
   - Hybrid (max 2) — Discovery + CXC, Venture Studio + CoE most common

3. **Draft the 8-section charter** (use [[template-innovation-charter.md]]):

   1. **Mandate** (one sentence)
   2. **Scope** — in / out, especially what's *out*
   3. **Funding model** — annual budget, source, multi-year (≥3 yr)
   4. **Decision rights** — what lab decides alone, what escalates
   5. **Talent model** — entry, exit, rotation, comp
   6. **KPIs by year** — capability (Y1), advancement (Y2), outcomes (Y3+)
   7. **Sponsorship** — named sponsor + reporting line + named successor
   8. **Sunset criteria** — when to retire / restructure

   A charter without sunset criteria becomes a permanent department.

4. **Design the operating model** (six structural decisions):
   - Reporting — CEO / COO / CSO direct (lab type-dependent)
   - Funding — corporate budget / chargeback / outcome-share / hybrid
   - Talent — permanent / rotational / hybrid (default 60/40)
   - Geography — co-located / distributed / hub-and-spoke
   - External partners — none / preferred / open ecosystem
   - Customer access — none / curated / embedded

5. **Set year-1 KPIs that don't kill the lab.**

   - **Year 1 — capability and learning:** capability built ($, talent, IP, partnerships, customer relationships), experiments per quarter, insights produced, org reach, customer experiences delivered
   - **Year 2 — bets advancing:** bets advanced through gates, pilot launches, validated learning, ecosystem partnerships
   - **Year 3+ — outcomes:** option value secured, revenue from H2/H3 bets, capability sustained

   **Revenue KPIs in year 1 = lab is being killed before it starts.** Negotiate this in the charter.

6. **Run the Ten Faces of Innovation coverage check** (Kelley):

   - Anthropologist (observation) ☐
   - Experimenter (prototyping) ☐
   - Cross-Pollinator (cross-industry input) ☐
   - Hurdler (org friction) ☐
   - Collaborator (silo bridging) ☐
   - Director (talent / craft) ☐
   - Experience Architect (end-to-end) ☐
   - Set Designer (environment / studio) ☐
   - Caregiver (customer-facing) ☐
   - Storyteller (outward narrative) ☐

   Most failed labs over-staff Anthropologist + Experimenter and under-staff Hurdler + Storyteller. Recommend rebalancing.

7. **Build survival mechanics into Day 1:**
   - Multi-year funding committed in writing (not verbal)
   - Successor sponsor named at charter signing
   - ≥2 operating BU absorbers committed in charter
   - Customer Love evidence pipeline (measurable customer impact by month 12)
   - Quarterly visibility drumbeat (whitepaper, prototype, customer story)
   - Talent rotation designed (not pure-permanent priesthood)
   - Independent budget line (not subject to BU cost cuts)

8. **For recharter path** (existing failing lab):
   - Run diagnostic against year-2 collapse pattern
   - Likely findings: KPI mismatch, sponsor exit, absorber gap, visibility gap
   - Recovery plan: re-ratify mandate, reset KPIs to year-1 capability metrics, name successor sponsor, commit to drumbeat, identify absorbers, build 60–90 day visible win
   - Or recommend sunset — see step 9

9. **Recommend sunset when applicable:**
   - 18+ months with no advanced bets (Discovery lab)
   - 30+ months with no validated business (Venture studio)
   - Capability not adopted by 50%+ of org (CoE)
   - Customer engagement metric below baseline (CX center)
   - Sponsor changed twice without succession

   A lab that should sunset and doesn't poisons the next attempt. Be honest.

10. **Hand off.** Charter feeds:
    - `/innovation:portfolio` for governance design within the lab
    - `/innovation:value-case` for funding model and year-1 KPI math
    - [[innovation-leadership-coach]] for sponsor management, successor cultivation, candor culture
    - [[innovation-strategist]] for mandate ratification with executive sponsor
    - [[consulting-management-consultant]] for legal structure if venture studio (LLC, sub-board)

11. **Save.** `30_Engagements/Active/{client}/Lab_Charter_{date}.md` or `10_Practice/Initiatives/Lab_Charter_{date}.md`.

Output deliverables:

```
1. Lab Diagnostic — what protects what, archetype recommendation
2. Lab Charter (8 sections, full)
3. Operating Model — six decisions documented
4. Year-1 KPI Pack — capability + learning metrics
5. Talent / Role Plan — Ten Faces coverage map
6. Sponsor Brief — what the executive sponsor commits and gets
7. Year-2 Survival Plan — explicit mechanics built in
8. (If recharter) Recovery Plan or Sunset Recommendation
```

**Common failures this command prevents:**
- Designing a lab without a mandate
- Blending more than 2 archetypes (Discovery + Venture Studio + CoE = none of them work)
- Year-1 revenue KPIs (kills the lab in year 1)
- No named successor sponsor (lab dies when sponsor moves)
- No absorber relationships (lab becomes cul-de-sac)
- Pure-permanent talent (becomes priesthood)
- Copy-Pixar / Skunk-Works / Google-X aesthetics (mechanisms, not aesthetics, are what makes them work)
