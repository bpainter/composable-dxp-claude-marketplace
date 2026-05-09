---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-service-designer]]"
tags: [type/reference, plugin/cx, scope/skill, topic/service-design, topic/methods]
status: active
---
# Service design methods toolkit

Drawn from Stickdorn et al., *This Is Service Design Doing* (research, ideation, prototyping, implementation chapters) and *This Is Service Design Thinking*. The full method library is at thisisservicedesigndoing.com; this reference is the curated subset that actually shows up in Composable DXP work.

Methods are not a sequence. The skill is choosing the right method for the question being asked.

## Five service design principles (recap)

Stickdorn's foundational principles. Every method below should honor all five.

1. **User-centred** — designed through the customer's experience.
2. **Co-creative** — all stakeholders, including frontline staff and customers, participate.
3. **Sequencing** — the service is a sequence, not a moment.
4. **Evidencing** — intangible service is made tangible through artifacts.
5. **Holistic** — the entire environment, including backstage, is in scope.

## Research methods

### Service safari
The team experiences the service themselves. End-to-end. As a real customer.

**When:** First contact with a domain. Calibrates the team to what the customer actually goes through.

**How:** Each team member runs the journey. No skipping steps because "that's just an FAQ." Capture friction, confusion, surprises. Debrief and synthesize.

**Watch out:** Team members substitute their judgment for the customer's. Pair with real-customer interviews; safari finds the gross friction, customers find what's specifically confusing for *their* segment.

### Contextual interview / observation
Interview the customer in the environment where they perform the job. Watch what they actually do.

**When:** The job is embedded in a context (workplace, home, mobile). What people say and what they do diverge.

**How:** 60–90 minutes per session. Combine watching, asking, and probing artifacts ("show me the spreadsheet you built for this").

**Watch out:** Observation alone produces description, not insight. Pair with "why did you do that?" probes throughout.

### Stakeholder interview
Interview people who participate in the service from the inside — staff, partners, support, ops.

**When:** Mapping the ecosystem and the service blueprint. Frontline staff know failure modes the team has never heard about.

**How:** 45–60 minutes. Focus on: handoffs they own, escalations they receive, workarounds they perform, what they wish was different.

**Watch out:** Staff have grievances; not all are insights. Cross-validate against customer signal.

### Cultural probes / diary studies
Customers self-document the experience over days or weeks.

**When:** Longitudinal jobs (formation, learning, healthcare, financial planning). The full arc doesn't fit in an interview.

**How:** Provide a kit (notebook, prompts, photo task, app). Light-touch reminders. Aggregate at the end.

**Watch out:** Compliance falls off in week 2. Plan for it; design the prompts so partial completion still yields signal.

### Mobile ethnography
Customers capture moments in real time via app or messaging.

**When:** Geographically distributed customers, or moments that can't be observed in person.

**How:** Smaply's ExperienceFellow, dscout, Indeemo, or simple WhatsApp prompts.

**Watch out:** Self-report bias. Ask for raw observation ("what just happened, in your words"), not interpretation.

## Synthesis methods

### Affinity mapping
Cluster raw research notes by emerging theme.

**When:** After a wave of qualitative research, before insight reporting.

**How:** Each note → sticky. Cluster by similarity. Name clusters. Re-cluster. Three to five rounds.

**Watch out:** Premature naming forces clusters; let them shape themselves first.

### Insight statements
Translate themes into stated insights with implications.

**When:** Closing a research wave. Insights are the deliverable; data is the appendix.

**Format:**

```
**Insight:** [What the data tells us — past tense, factual.]
**Why it matters:** [Implication for the design / strategy decision.]
**Evidence:** [Quotes, numbers, observations.]
**Confidence:** [How strongly the data supports it. High / medium / low.]
```

**Watch out:** Insights without confidence ratings get cited at full strength regardless of evidence.

### Personas (service design flavor)
Goal-driven personas grounded in the service context. See `cx-persona-developer`.

### Stakeholder map
Visualization of every actor that touches the service. See [[ecosystem-mapping]] in the shared plugin references.

## Ideation methods

### How might we (HMW)
Reframe an insight as an open design question.

**Format:** "How might we [verb] [stakeholder] [outcome] given [constraint]?"

**Example:** "How might we help first-time founders gain confidence in their entity decision without forcing a lawyer call?"

**Watch out:** HMWs that smuggle solutions ("how might we add a chatbot...") are not HMWs; they're feature requests.

### Co-creation workshop
Customers, staff, and design team in the same room generating ideas together.

**When:** Problem is well-understood, solution space is open.

**How:** 2–4 hours. Mix groups. Use HMWs as prompts. Generate widely, then converge. Always end with a tangible artifact (storyboard, sketch, role-play).

**Watch out:** Customers who attend co-creation sessions are not representative. Use co-creation for divergence; validate with broader research.

### Bodystorming / role-play
Act out the service end-to-end. Frontline staff and customers play themselves; designers play the system.

**When:** Service has high human-interaction content (support, in-person, healthcare). Role-play surfaces awkward moments, missing words, unclear roles.

**Watch out:** Role-play takes practice. Plan a warmup; don't drop senior leaders into cold improvisation.

## Prototyping methods

### Service prototype
Stage the service end-to-end in a low-fidelity simulation. Use a room, props, scripts. Customers (or proxies) walk through.

**When:** Validating an end-to-end service before operational investment.

**Watch out:** Treat the prototype as a test, not a demo. The point is to find what's broken.

### Desktop walkthrough
3-D model on a table. Lego figures, sketches, props. Walk through the service step by step.

**When:** Aligning a cross-functional team on what the service actually is. Cheap, fast, surfaces a lot.

**Watch out:** Useful for internal alignment; don't show to customers as an actual prototype.

### Storyboard
Comic-strip rendering of the service flow.

**When:** Communicating service concepts up the org or to vendors. Anyone can read a storyboard.

**Watch out:** Storyboards become decorative if not paired with a service prototype that pressure-tests them.

### Investigative rehearsal / dark horse prototype
Rehearse a deliberately extreme version of the service ("what if the service was completely automated / completely human / completely free / required deep commitment up-front"). Surfaces what the team really values.

**When:** Strategy is fuzzy; team is converging too early on a familiar pattern.

**Watch out:** Easily becomes performative. Pick three dark-horse versions; spend equal time on each.

## Implementation methods

### Service blueprint
The operational spec. See [[service-blueprint]].

### Pilot
Run the new service with a constrained group of customers / staff / regions before scaling.

**When:** Always, when the change is operational. Always, when frontline staff need to learn new behavior.

**How:** Define success criteria *before* the pilot. Plan the response to each plausible outcome.

**Watch out:** Pilots that have no defined exit criteria run forever. Pilots that are evaluated only on customer satisfaction miss operational scalability.

### Service standards / experience principles
Codified rules that govern future decisions. See [[experience-principles]] in the shared plugin references.

### Train-the-trainer / staff handover
Frontline staff need to learn the new service before customers see it. Train enough staff that they can train others.

**Watch out:** Training is the most under-budgeted part of service launches. The blueprint is not real until staff can run it cold.

## Method selection

| If you're … | Reach for … |
|---|---|
| New to the service domain | Service safari, contextual interviews |
| Mapping the system | Stakeholder interviews + ecosystem map |
| Diagnosing why a stage breaks | Contextual observation + Switch interviews (`cx-jobs-to-be-done`) |
| Generating new directions | Co-creation, HMWs, bodystorming |
| Validating a concept end-to-end | Service prototype, desktop walkthrough |
| Operationalizing | Service blueprint, pilot, train-the-trainer |
| Locking in alignment | Experience principles, blueprint, story-of-the-future |

## Sources

- Stickdorn, M., Hormess, M., Lawrence, A., Schneider, J. *This Is Service Design Doing*. O'Reilly, 2018.
- Stickdorn, M. & Schneider, J. *This Is Service Design Thinking*. BIS Publishers, 2011.
- Method library: thisisservicedesigndoing.com (free downloadable templates).
- Risdon, C. & Quattlebaum, P. *Orchestrating Experiences*. Rosenfeld, 2018.
