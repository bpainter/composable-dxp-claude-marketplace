---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-jobs-to-be-done]]"
tags: [type/reference, plugin/cx, scope/skill, topic/jtbd, source/kalbach]
status: active
---
# JTBD plays — Kalbach

Drawn from Jim Kalbach, *The Jobs To Be Done Playbook* (2020). The pluralist counterpart to Ulwick's ODI — practical plays that work across qual and quant, lightweight when needed, deeper when warranted.

## How to choose a play

Each play has an effort tier. Match effort to the decision being made.

```
Light play   → next-sprint feature decision, messaging test, support copy
Medium play  → quarterly roadmap, value-prop sharpening, segmentation
Heavy play   → multi-year strategy, market entry, ODI-equivalent rigor
```

Don't run a heavy play for a light decision. Don't run a light play and pretend it's a multi-year evidence base.

## Discovery plays

### Play: Scope the JTBD domain

**Use when:** Starting any JTBD effort. Frames everything downstream.

**Steps:**
1. Define the main job (verb + object + clarifier; solution-free).
2. Define the job performer (the role doing the job, not the demographic).
3. Form a hypothesis about job process and circumstances.

**Effort:** Low to medium. Get a small group together; don't do this alone.

**Watch out:** Wrong level of abstraction is the most common error. When in doubt, scope **broad** rather than narrow — narrow scopes hide solution assumptions.

### Play: Open jobs interview

**Use when:** Building a JTBD evidence base from scratch.

**Steps:**
1. Recruit job performers with recent task experience (within 90 days).
2. Open with: "Walk me through the last time you tried to [main job]."
3. Probe for: situation that triggered the job, alternatives considered, pushes and pulls, anxieties, what made the choice.
4. Code transcripts for job statements, not features.

**Effort:** Medium to high. 8–15 interviews per job performer type.

**Watch out:** Don't ask about your product. Ask about the job. The product is one solution among many.

### Play: Switch interview

**Use when:** Understanding why customers chose your product, why they left, or why they switched from a competitor.

**Steps:**
1. Recruit people who recently switched (your way or away).
2. Anchor on the **moment of purchase** — when did they decide? Where were they? What triggered it?
3. Map the **timeline**: first thought → passive looking → active looking → deciding → consuming.
4. At each stage, capture pushes (frustrations with old solution), pulls (attraction to new), anxieties (what almost stopped them), habits (what made it easy to stay).

**Effort:** Medium to high. Interviews can be done in days; coding takes longer.

**Watch out:** Switch interviews drift toward the product. Keep refocusing on the **job and the moment of switching**, not on features.

### Play: Four Forces analysis

**Use when:** Designing for behavior change — onboarding, conversion, churn prevention.

**The four forces:**

```
            Pushes (away from old solution)        Pulls (toward new solution)
                       ↓                                       ↓
                    THE SWITCH
                       ↑                                       ↑
       Habits (anchoring to old)              Anxieties (about the new)
```

**Steps:**
1. From Switch interviews, code statements into the four quadrants.
2. For each quadrant, rank statements by frequency and intensity.
3. Design moves: amplify pushes/pulls, reduce habits/anxieties.

**Effort:** Medium. Pairs naturally with Switch interviews.

**Watch out:** Most teams over-invest in pulls (more features) and under-invest in anxiety reduction (the silent killer). The four forces force the team to look at all four.

## Defining-value plays

### Play: Build the job map

**Use when:** Decomposing a job into its sequence so the team can find leverage points.

**Steps:**
1. Take the main job. Walk through each stage of the universal job map: define → locate → prepare → confirm → execute → monitor → modify → conclude.
2. For each stage, list what the job performer is trying to accomplish (still solution-free).
3. Note where the user struggles today (from research).
4. Identify highest-leverage stages — usually 2–3 of the 8.

**Effort:** Low to medium. Can be done in a workshop with prior research.

**Watch out:** Don't conflate the job map with the customer journey map. The job map is the steps of the job (universal, solution-free). The journey map is the steps of *the experience with your solution*. Both useful, different.

### Play: Goal-driven personas

**Use when:** Personas need to be tied to jobs, not to demographics.

**Steps:**
1. Start with the job performer (the role doing the job).
2. From research, identify the **circumstances** that vary across the population (e.g., first-time vs. repeat, time-pressured vs. exploratory, solo vs. team).
3. Cluster respondents by circumstance, not by demographic.
4. For each cluster, capture: the dominant job, the desired outcomes most important to them, the pushes/pulls/anxieties they share.

**Effort:** Medium to high.

**Watch out:** Personas without a job ground them in are still demographics. The job is the persona's load-bearing element.

### Play: Job-driven competitive analysis

**Use when:** Understanding the real competitive set — usually broader than "competitors in our category."

**Steps:**
1. Take the main job.
2. List every alternative way the job currently gets done — including non-product alternatives (a spreadsheet, a friend, doing nothing).
3. For each alternative, score on importance × satisfaction for the key outcomes.
4. The competitive set is whoever scores well on the same outcomes you target.

**Effort:** Low to high depending on data.

**Watch out:** Most "competitive analyses" miss the largest competitor — "doing nothing" or "the way we do it now."

## Designing-value plays

### Play: JTBD-driven roadmap

**Use when:** Backlog prioritization keeps cycling because everything looks important.

**Steps:**
1. Group backlog items by the job and the desired outcome each addresses.
2. For each group, score the underlying outcome's opportunity (importance + max(importance − satisfaction, 0)).
3. Rank groups by opportunity score, then sequence within group.
4. Frame roadmap themes as outcomes ("increase first-week activation"), not features.

**Effort:** Medium to high.

**Watch out:** Roadmaps confused with project plans. The roadmap names the bets; the plan names the work. Keep them separate.

### Play: Job stories

**Use when:** Translating jobs into specs for product teams.

**Format:**

```
When [situation],
I want to [motivation],
So I can [expected outcome].
```

This is the JTBD statement reused as a spec format. It replaces "As a [persona] I want [feature]" because it forces the situation and outcome — both of which are missing from typical user stories.

**Effort:** Low. Most useful when paired with prior research.

**Watch out:** Job stories without research grounding are just better-formatted user stories.

## Delivery plays

### Play: Job-aligned onboarding

**Use when:** First-week experience isn't producing activation.

**Steps:**
1. Identify the job the user is hiring the product for in week one.
2. Map the universal job stages (define → locate → prepare → confirm → execute → monitor → modify → conclude) for that first job.
3. Audit the current onboarding against each stage. Most onboardings skip define and confirm; users get rushed to execute without context or verification.
4. Redesign to honor the missing stages.

**Effort:** Medium to high.

### Play: Churn diagnosis (Switch in reverse)

**Use when:** Diagnosing why users leave.

**Steps:**
1. Recruit recent churners (not just people who downgraded — people who fully left).
2. Run Switch interviews focused on the moment they decided to leave.
3. Code for: pushes (frustrations that built up), pulls (alternative they switched to), anxieties about leaving (what almost made them stay), habits (why they stayed as long as they did).

**Effort:** Medium.

**Watch out:** Recruiting churners is hard. Build incentive into the contact strategy or work through customer-success outreach.

### Play: Support-as-research

**Use when:** Continuous source of job signal at near-zero marginal cost.

**Steps:**
1. Train support agents to probe for the job: "Before you reached out, what were you trying to get done?"
2. Tag tickets by job and stage of the universal job map.
3. Aggregate weekly into the feedback synthesizer (`cx-customer-feedback-synthesizer`).

**Effort:** Low. Pays back continuously.

## Strategic plays

### Play: Job-driven strategy

**Use when:** Strategy is being built around technology or competitor positioning rather than around customer outcomes.

**Steps:**
1. Pick the strategic horizon (3 years).
2. Identify the underserved outcomes for the chosen job performer (from research).
3. Plot existing strategy against those outcomes — which are addressed, which are ignored, which are owned by competitors.
4. Reframe strategy as bets to address the highest-opportunity unaddressed outcomes.

**Effort:** High. Requires prior research and senior buy-in.

### Play: Reorganize around jobs

**Use when:** Org structure produces handoff failures because teams own products / channels / features instead of jobs.

**Steps:**
1. Map the major jobs the company serves.
2. For each job, identify which teams currently touch the experience.
3. Propose teams that own a job end-to-end (research, design, build, market, support) for at least the highest-priority jobs.

**Effort:** High and politically charged.

**Watch out:** Total reorganization is rarely possible. Start with one job and one team as a pilot.

## When to choose Kalbach over Ulwick

- The decision is shorter-cycle than 12 months.
- You don't have access to a survey panel.
- The team is new to JTBD and the cost of full ODI would kill adoption.
- The work is qualitative-led; you'll add quantitative validation later.

## Sources

- Kalbach, J. *The Jobs To Be Done Playbook*. Two Waves Books / Rosenfeld, 2020.
- Klement, A. *When Coffee and Kale Compete*. NYC Publishing, 2016.
- Klement, A. *Jobs To Be Done: A Roadmap for Customer-Centered Innovation*. NYC Publishing, 2018.
