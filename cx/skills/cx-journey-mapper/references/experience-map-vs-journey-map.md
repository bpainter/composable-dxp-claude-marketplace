---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-journey-mapper]]"
tags: [type/reference, plugin/cx, scope/skill, topic/journey-mapping, source/risdon]
status: active
---
# Experience map vs. journey map vs. service blueprint vs. job map

Drawn from Risdon & Quattlebaum, *Orchestrating Experiences* (chs. 4–5), and Kalbach, *Mapping Experiences*.

These four artifacts get conflated constantly. Each answers a different question. Picking the wrong one produces a beautiful artifact that doesn't move decisions.

## Quick decision

| If the question is … | Use … |
|---|---|
| What does *this customer* go through with *us*? | **Journey map** |
| What does *this customer* go through, *across all providers* (us + competitors + alternatives)? | **Experience map** |
| What's happening *backstage* to deliver this experience? | **Service blueprint** (`cx-service-designer`) |
| What are the *steps of the job* (regardless of solution)? | **Job map** (`cx-jobs-to-be-done`) |

Three more diagnostics:

- **Scope: us-only or broader?** — journey for us-only; experience for the broader market; job map for the job itself.
- **Layer: front-of-stage or back-of-stage?** — journey/experience are front-of-stage; blueprint adds back-of-stage.
- **Lens: solution-aware or solution-free?** — journey/experience/blueprint are solution-aware; job map is solution-free.

## Journey map

**Question:** What is *our* customer's experience with *our* product or service, stage by stage?

**Scope:** One persona. One end-to-end path with our offering.

**Rows:**
- Doing
- Thinking
- Feeling (often as an emotion curve)
- Touchpoints (channels they hit)
- Pain points
- Opportunities

**Use for:**
- Diagnosing where users drop off in our funnel.
- Briefing design before screens get composed.
- Aligning a product team on what to fix.

**Don't use for:**
- Strategic positioning ("how do we compete with alternatives").
- Service-level operational design (use blueprint).
- Roadmap prioritization across audiences.

## Experience map

**Question:** What does this customer's broader experience of *getting this job done* look like — across all providers, channels, and alternatives, including non-product alternatives?

**Scope:** One persona. The entire experience of the job, of which our product is one part (sometimes a small part).

**Rows:** Same as journey map, but the touchpoints column includes competitors, partners, friends, search engines, "doing nothing." The opportunities column often includes "where could we exist that we don't today?"

**Use for:**
- Strategic positioning. Where in the experience does our offer matter most? Where are we absent?
- Identifying expansion opportunities. The map surfaces moments where the customer needs something and nobody is providing it.
- Ecosystem decisions — what to build, partner, or ignore.

**Don't use for:**
- Replacing journey maps. Experience maps are for strategy; journey maps are for execution.
- Operational design. Too high-level.

Risdon's distinction (paraphrased): a journey map asks "how is our experience?" An experience map asks "how is the customer's life — and where do we show up in it?"

## Service blueprint

**Question:** What's happening backstage — staff, systems, processes — that produces the front-of-stage experience?

**Scope:** One service. Front-of-stage and back-of-stage.

**Layers:**
- Physical evidence
- Customer actions
- Frontstage actions
- Backstage actions
- Support processes

See [[service-blueprint]] in the service-designer skill for the full layout and method.

**Use for:**
- Diagnosing why a customer-facing problem is actually a backstage problem.
- Operationalizing a service design — the spec that ops, support, training, and IT can execute.
- Identifying handoff failures between teams.

**Don't use for:**
- Strategy. Blueprints are operational.
- Customer-facing communication. Customers don't care about the support-process layer.

## Job map

**Question:** What are the *steps of the job* the customer is trying to get done — regardless of how they're getting it done today?

**Scope:** One job. Solution-free.

**Stages (universal, from Ulwick):**
1. Define
2. Locate
3. Prepare
4. Confirm
5. Execute
6. Monitor
7. Modify
8. Conclude

See [[odi-methodology]] for the full universal job map.

**Use for:**
- Finding underserved stages of a job (where today's solutions don't help).
- Generating new product or feature ideas anchored in stages.
- Discovering competitive whitespace.

**Don't use for:**
- Designing UI flows. Job maps are solution-free; UIs are solutions.
- Communication with customers. Job-map language ("locate," "confirm") is internal.

## How they fit together

For a major engagement, you may need three or four of these. Sequence:

```
1. Job map (solution-free)
   → identifies what the customer is trying to do.

2. Experience map (broad scope)
   → shows the full landscape; positions our offer.

3. Journey map (us-only)
   → details what we deliver today and where it breaks.

4. Service blueprint (us-only, with backstage)
   → designs how we operationalize the future-state journey.
```

Most engagements pick two of the four. Common pairings:

- Job map + journey map — for product design where the job is central.
- Experience map + journey map — for strategic redesign.
- Journey map + service blueprint — for operational redesign.

## Common confusions

- **"Journey map" used for everything.** The catch-all label loses information. Be specific in deliverables — "journey map (current state)," "experience map (cross-provider)," "service blueprint (backstage)."
- **Journey maps that include backstage rows.** That's a blueprint.
- **Journey maps with multiple personas.** Each persona deserves its own map. Merging produces uninterpretable artifacts.
- **Job maps with product names in them.** Strip them — job map is solution-free.
- **Experience maps that only show our touchpoints.** Then it's a journey map. The point of an experience map is the *broader* landscape.

## Sources

- Risdon, C. & Quattlebaum, P. *Orchestrating Experiences*. Rosenfeld, 2018.
- Kalbach, J. *Mapping Experiences*. O'Reilly, 2nd ed., 2020.
- Stickdorn, M. et al. *This Is Service Design Doing*. O'Reilly, 2018.
- Ulwick, A. *Jobs to be Done: Theory to Practice*. 2016.
