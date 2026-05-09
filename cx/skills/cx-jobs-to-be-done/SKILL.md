---
name: cx-jobs-to-be-done
description: Frame product, service, and content decisions through Jobs-to-be-Done — what the user is trying to get done, the situation they're in, the progress they want to make, and what is currently blocking them. Produces JTBD statements, job maps, and prioritized job lists usable for product strategy, feature scope, and messaging. Use this skill any time the team is debating "what does the user want" or "what should we build" — "should we add X," "what's the user's goal," "JTBD," "outcome statement," "what job does this do" — even when the words aren't used. Trigger whenever feature priority or value prop arguments are happening so the team has a goal to argue against, not vibes.

# Project context
type: skill
project: skills-library
plugin: cx
aliases: [cx-jobs-to-be-done]
tags: [type/skill, plugin/cx topic/customer-experience, topic/ux-research, topic/service-design]
status: active
---

# Jobs-to-be-Done

JTBD reframes products from "features for users" to "tools people hire to make progress in a specific situation." The shift matters because it forces the team to be specific about *what* the user is trying to accomplish, not just *who* they are or *what* they like.

JTBD is complementary to personas (`cx-persona`), not a replacement. Persona = who. JTBD = what they're trying to do. Both have their uses.

Pair with [[cx-customer-research]] (the evidence JTBDs come from), [[cx-persona-developer]], [[cx-journey-mapper]] (where jobs sit in the journey), `strategy-digital-strategist` (when JTBD informs roadmap or OKRs), and `cx-behavioral-design` (how users actually decide once they have the job framed).

## When to use this skill

- Framing a product or feature decision around user value rather than feature output.
- Writing positioning, value props, or messaging that lands.
- Prioritizing a backlog when "everything is important."
- Refactoring a stuck product that does many things badly into one that does one job well.
- Diagnosing why a feature that "should work" isn't getting used.
- Onboarding a team to think about the user's perspective rather than the product's.

## Core posture

- **A job is a verb, not a noun.** "Help me decide whether to incorporate" is a job. "A formation guide" is not.
- **Job ≠ task. Job ≠ feature.** A task is a step toward a job. A feature is a tool that helps with a job. The job is the underlying progress.
- **Jobs are situational.** "I want to incorporate" is incomplete. "I want to incorporate before my fundraising round closes in 6 weeks" is a job. The situation changes everything.
- **Functional + emotional + social.** Most jobs have a functional layer (the practical outcome), an emotional layer (how the user wants to feel), and a social layer (how they want others to perceive them). The functional layer is where most teams stop. Don't.
- **Hire and fire.** Users *hire* products to do jobs. They also *fire* them when something better comes along. Knowing what your product gets fired for is as useful as knowing what it gets hired for.
- **The competition is the alternative way the job gets done today.** Sometimes that's a competitor. Often it's a spreadsheet, a friend, or "doing nothing."

## JTBD statement format

The canonical Christensen / Strategyn format:

```
When [situation],
I want to [motivation],
So I can [expected outcome].
```

Worked example:

```
When I'm a first-time founder about to incorporate,
I want to make a confident decision between Delaware and another state,
So I can move on to fundraising without legal anxiety.
```

That statement gives the product team a job to test against. Compare it to:

> "Founders need formation help."

The first version constrains design. The second is a wish.

### A note on emotional and social layers

The bare statement captures the functional job. For deeper work, add:

```
Functional: [the practical thing the user is trying to accomplish]
Emotional: [how the user wants to feel during or after]
Social: [how the user wants to be perceived]
```

Worked example:

```
Functional: Decide on entity type and jurisdiction with confidence.
Emotional: Stop feeling like I'm winging it; feel like I'm doing this right.
Social: Look like I know what I'm doing in front of co-founders and investors.
```

This is how marketing copy that actually moves people gets written.

## Job map (the steps within a job)

A job has stages. A useful map identifies each stage, where the user struggles, and where the most leverage is.

For "Decide between Delaware and another state for incorporation":

```
1. Define — recognize the decision is needed; understand what's at stake.
2. Locate — find information sources to compare.
3. Prepare — gather relevant facts about my situation (raise plans, etc.).
4. Confirm — verify the recommendation with someone trusted (lawyer, peer).
5. Execute — file the paperwork.
6. Monitor — confirm filing succeeded, deal with follow-up.
7. Modify — handle changes (state of operation, conversion later).
8. Conclude — feel done; redirect attention to next thing.
```

For each stage, the team can ask: where does the user struggle today? Where is competition strong? Where would a 10x improvement most change the user's experience?

## Prioritizing jobs

Not every job is worth solving. Prioritize on:

- **Importance**: how much does the user care about getting this job done well? (Surveyed or interviewed.)
- **Satisfaction**: how well are existing solutions doing it today? (Same.)
- **Reach**: how many users have this job?
- **Strategic fit**: does the firm have the right to play here?

Importance high + satisfaction low + reach high = prime candidate. The Strategyn formulation calls these "underserved opportunities."

## JTBD statement library

For most projects, capture 5-15 high-importance jobs. Each gets a statement, an importance score (where measured), a current-satisfaction score, and a status (target / not addressing / mature).

Format:

```
| ID | Job | Importance | Satisfaction | Reach | Status |
|---|---|---|---|---|---|
| J-001 | Decide between Delaware and another state | 9/10 | 4/10 | High | Target Phase 1 |
| J-002 | Split equity with co-founders fairly | 8/10 | 3/10 | High | Target Phase 2 |
| ... | ... | ... | ... | ... | ... |
```

The actual research synthesis (see [[cx-customer-research]]) determines which jobs are real, which are high-importance, and which are underserved. Don't ship a JTBD library straight from a workshop — every statement should trace to evidence.

## How to derive JTBDs from research

```
1. Use research already done. Interviews, surveys, behavioral data.
   See [[cx-customer-research]].

2. Code transcripts for "I was trying to..." and "I needed to..." moments.
   These are job indicators.

3. For each candidate job, write the When/I want to/So I can statement.
   Make the situation specific.

4. Strip product references. "When I'm using your tool, I want a button..."
   is not a job. "When I'm preparing to incorporate, I want clarity on
   entity type..." is.

5. Test with the research team: would the user we interviewed recognize
   this as their job? Does it match the words they used?

6. Score importance and satisfaction (interview signal or follow-up survey).

7. Pressure-test with stakeholders for strategic fit. A high-importance,
   high-reach job that the firm has no right to play in is not a target.

8. Publish as a JTBD library. Refresh quarterly.
```

## Common failure modes

- **Solution-flavored jobs.** "I want a calculator that..." That's a feature, not a job. Strip it.
- **Persona-flavored jobs.** "Sarah wants to..." Personas describe types; jobs describe what types are trying to do. Keep them distinct.
- **One job to rule them all.** "I want to start a successful company." Too abstract. Useful jobs are more specific and bounded.
- **No situation in the statement.** "I want to incorporate" misses the *when*. The situation is most of the design constraint.
- **Functional-only jobs.** Most jobs have an emotional or social layer. Missing it produces messaging that's accurate and unmoving.
- **JTBDs invented from a workshop without research.** Same trap as personas-from-vibes. Without evidence, the statements look right and don't predict behavior.
- **Confusing "users hire X" with "we want users to hire X."** The former is research. The latter is a sales pitch.
- **Static job libraries.** Jobs evolve. A library that's two years old is probably wrong.

## Outputs

### A single JTBD statement

```
**Job ID**: J-001
**When**: I'm a first-time founder about to incorporate.
**I want to**: Make a confident decision between Delaware and another state.
**So I can**: Move on to fundraising without legal anxiety.

**Functional**: Decide on entity type and jurisdiction with confidence.
**Emotional**: Stop feeling like I'm winging it.
**Social**: Look credible to co-founders and investors.

**Importance**: 9/10
**Satisfaction with existing solutions**: 4/10
**Strategic fit**: high — the firm has deep expertise in this domain.
**Status**: Target Phase 1.
```

### A job map

The 8-stage map (or the project-specific version), with notes on where the user struggles and where there is most leverage.

### A prioritized library

The table format above, with all in-scope jobs.

## Tone

When writing JTBD-derived content (homepage copy, feature framing, messaging), lead with the answer, anchor in concrete consequences (money / time / risk), plain English. Run the output through [[consulting-humanize]] before publication.

## How this skill relates to others

- The research that JTBDs come from → [[cx-customer-research]].
- The complementary "who" lens → [[cx-persona-developer]].
- Where each job lives in the user's broader experience → [[cx-journey-mapper]].
- How users decide once they have the job framed → `cx-behavioral-design`.
- JTBDs informing strategy and OKRs → `strategy-digital-strategist`.
- JTBDs informing content → `strategy-seo-brief`, `strategy-geo-optimization`.
- JTBDs informing product backlog → `pm-user-story` (stories often emerge from jobs).

## References

- [[odi-methodology]] — Ulwick's Outcome-Driven Innovation. Job statement format, desired-outcome statement format, the opportunity algorithm, the universal job map, the five growth strategies. Heavier methodology; reach for it on multi-year decisions.
- [[jtbd-plays]] — Kalbach's plays. Switch interviews, Four Forces, job map, goal-driven personas, JTBD-driven roadmap, churn diagnosis. Lighter and more pluralist; reach for it on shorter-cycle decisions.

## Source material

- Christensen, C. M., et al. *Competing Against Luck*.
- Ulwick, A. *Jobs to Be Done: Theory to Practice*. (See [[odi-methodology]].)
- Kalbach, J. *The Jobs To Be Done Playbook*. (See [[jtbd-plays]].)
- Klement, A. *When Coffee and Kale Compete*.
- Klement, A. *Jobs To Be Done: A Roadmap for Customer-Centered Innovation*.
- Strategyn / Outcome-Driven Innovation methodology.
