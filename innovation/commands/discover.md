---
description: Kick off a JTBD discovery sprint — switch interviews, observation, the five Innovator's DNA discovery skills, opportunity scorecard, Gate 1 brief

# Project context
type: command
project: skills-library
plugin: innovation
tags: [type/command, plugin/innovation]
status: active
---

Run a structured 4–6 week JTBD discovery sprint that produces validated job stories, a circumstance map, and a Gate 1 brief ready for validation.

Primary skill: [[innovation-discovery-coach]] (Layer 3 of the [[2026-05_Innovation-Operating-System|Innovation OS]]).

Reference templates: [[template-jtbd-interview-guide.md]], [[jobs_to_be_done_discovery.md]].

Steps:

1. **Confirm scope and frame.**
   - What strategic question is discovery answering? Pull from `/innovation:strategy` if relevant.
   - Which segment(s) — 2–3 archetypes max for a 6-week sprint.
   - Why now — what triggered this discovery?
   - Who owns the bet downstream (handoff to validation / portfolio)?

   **Force the JTBD-vs-CX distinction:** if the team is researching current-product satisfaction, that's CX research, not JTBD. JTBD is pre-product circumstance — the job customers were doing that brought them to want any solution at all. Reset the frame if confused.

2. **Design the hypothesis.**
   - Suspected jobs (functional, emotional, social) per archetype
   - Hypothesized circumstances and triggers
   - Hypothesized competing solutions (incl. doing nothing)
   - Riskiest assumption to challenge

3. **Plan recruiting.**
   - Target 12–16 customers per archetype to net 8–10 useful interviews
   - Mix of switched-recently (last 6–12 months) and considered-but-didn't-switch (status-quo'd)
   - 2–3 archetypes total
   - Recruiting via: existing customer panels, third-party panels, network referrals, design-partner outreach

4. **Run Week 1 — hypothesis design + recruit + interview guide.**
   - Use [[template-jtbd-interview-guide.md]] structure (90-minute switch interview, 6 phases)
   - Train the interview team in advance — JTBD is harder than CX research

5. **Run Weeks 2–3 — interview round 1 (8–12 customers).**
   - Switch-interview structure — past tense, real recent experiences
   - Capture the four forces per interview: push of situation, pull of new solution, anxiety of switching, habit of present
   - Reserve 30–40% of effort for observation / contextual inquiry
   - Workaround spotting — workarounds reveal jobs no product is doing

6. **Run synthesis (Week 2–3, ongoing).**
   - Affinity mapping (group raw observations into themes; no ideas yet)
   - First-pass job stories: "When [circumstance], I want to [motivation], so I can [outcome]"
   - Force map per archetype
   - Trigger event taxonomy

7. **Run Week 3–4 — round 2 interviews (6–10) testing job stories.**
   - Validate the hypothesized stories with new customers
   - Refine, contest, eliminate as appropriate

8. **Run Weeks 4–5 — circumstance mapping + competing solutions inventory.**
   - Map customer's actual journey when the job arises
   - Inventory all competing solutions (products, workarounds, partial solutions, doing nothing)
   - Score each on customer satisfaction across the job dimensions

9. **Run Weeks 5–6 — opportunity prioritization + Gate 1 brief.**
   - Rank unmet needs: customer value × competitive gap × feasibility × strategic fit
   - Identify the riskiest assumption for the next stage
   - Write the Gate 1 brief (1 page, not 70)

10. **Apply the five Innovator's DNA discovery skills throughout.**
    - **Associating** — connect insights from unrelated industries
    - **Questioning** — challenge assumed constraints; "what if the opposite were true?"
    - **Observing** — watch in context, not just interview
    - **Networking** — talk to adjacent-industry experts before convergence
    - **Experimenting** — pretotype the most-promising direction within the sprint if time allows

11. **Hand off to validation.** Output of this command feeds:
    - `/innovation:validate` for the four-stage validation cycle on the prioritized opportunity
    - `/innovation:value-case` for value math once validation begins
    - `innovation-typologist` for type-combination ideation if multiple solution shapes emerge

12. **Save.** `30_Engagements/Active/{client}/Discovery_Sprint_{topic}_{date}/` (client work) or `10_Practice/Initiatives/Discovery_Sprint_{topic}_{date}/` (internal) — discovery brief, interview guide, transcripts (private), synthesis docs, Gate 1 brief.

Output deliverables:

```
1. Discovery Sprint Brief — design, hypothesis, recruiting plan
2. Interview Guide — JTBD switch-interview structured
3. Interview Notes — 12–20 transcripts (private; sensitive)
4. Job Stories Set — 3–5 validated job statements with quote evidence
5. Force Map — push / pull / anxiety / habit per archetype
6. Trigger Event Taxonomy — what causes customers to switch
7. Competing Solutions Inventory — what they considered, ranked
8. Unmet Needs Scorecard — ranked opportunities
9. Gate 1 Brief — 1 page, with riskiest assumption identified for validation
```

**Common failures to flag during the sprint:**
- CX research dressed as JTBD — reset the frame
- Solution-suggesting in interviews — coach the interviewer; pause and reset
- Aggregating jobs across segments too early — preserve circumstance distinctions
- 70-page report instead of 1-page brief — force synthesis
- Confirmation bias (everyone "loved" the idea) — adversarial recruitment for round 2
