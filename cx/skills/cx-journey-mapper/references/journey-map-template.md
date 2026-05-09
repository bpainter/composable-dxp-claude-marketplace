---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-journey-mapper]]"
tags: [type/reference, plugin/cx, scope/skill, topic/journey-mapping, topic/template]
status: active
---
# Journey map template

Drawn from Risdon & Quattlebaum (*Orchestrating Experiences*) and Stickdorn et al. (*This Is Service Design Doing*).

A reusable scaffold. Copy and adapt. The art is filling each cell with evidence; the template only holds the shape.

## Header

Always present. Without it, the map is unanchored.

```
Persona:        [Name + one-line identity. Single persona only.]
Job:            [The JTBD this journey supports. Reference: J-001.]
Scope:          [Start state → end state. Be specific.]
Map type:       [Current state | Future state]
Evidence base:  [Research IDs / interview pool / data sources.]
Date | Version: [2026-Q2 | v1.2]
Owner:          [Single name.]
```

## Stages (columns)

Five to seven stages. Use the user's experience, not the company's funnel.

```
| Stage 1   | Stage 2   | Stage 3   | Stage 4   | Stage 5   |
| Trigger   | Research  | Decide    | Act       | Confirm   |
```

Naming heuristics:
- **Verb form preferred** ("Decide," "Compare") — names what the user is doing.
- **User language preferred** — if research participants called it "shopping around," don't relabel to "evaluation phase."
- **Length-similar** — uneven stages produce visual misreading.

## Rows

Standard rows, in this order. Every row is required for a complete journey map; remove only with explicit reason.

### DOING — observable actions

```
Stage 1                   Stage 2
Searches "form a Delaware  Compares Stripe Atlas, Clerky,
C-corp"                    DIY filing service
```

What's the user actually doing? Verbs and objects. Avoid "engages with" and "thinks about" (those are other rows).

### THINKING — what the user is asking themselves

```
Stage 1                   Stage 2
"Do I really need to       "How do I know which one
form in Delaware?"          to trust?"
```

User-voice. Quote-style. Surfaces the actual concern.

### FEELING — emotional state

```
Stage 1                   Stage 2
😐 Curious, mildly         😟 Overwhelmed by choice,
overwhelmed                 worried about wrong choice
```

Emotion + intensity. Plot as a curve overlay if doing slide work. Emotional dips often predict drop-off better than functional friction.

### TOUCHPOINTS — channels and surfaces

```
Stage 1                   Stage 2
Google search, Reddit       Stripe Atlas page,
threads, friend Slack DM   Clerky page, comparison
                           blog post, friend's email
```

Be exhaustive. The third-party touchpoints (search, friend, review site) are usually the most-missed.

### PAIN POINTS — specific friction with severity

```
Stage 1                       Stage 2
- Conflicting advice on       - No comparison view across
  whether DE is needed (high)   providers (high)
- "Delaware" as a term not    - Pricing buried (medium)
  defined for first-timers
  (medium)
```

Tag severity (high / medium / low). Tag the evidence ID where possible.

### OPPORTUNITIES — what could be different

```
Stage 1                       Stage 2
Plain-English DE primer       Side-by-side comparison
that ranks at top of          tool with real pricing,
search                        sortable by founder type
```

Specific. Not "improve clarity" — what specifically.

### MOMENTS OF TRUTH — flagged stages

Mark 2–4 stages as moments of truth. Disproportionate impact on overall experience. Investment here repays the most.

## Optional rows

Add only when relevant; skip when not.

### KPI — success metric per stage

```
Stage 1                       Stage 2
% reaching site after         Comparison-page bounce
search (target: >40%)         rate (target: <30%)
```

Useful for current-state maps that will be tracked over time, or future-state maps where target metrics are part of the design brief.

### CHANNEL — explicit channel the user is on

Useful in omnichannel journeys where the same stage happens on different channels and you need to compare.

### BACKSTAGE ACTIONS / SUPPORT PROCESSES

If included, the artifact has become a service blueprint. Promote to that artifact instead. See [[service-blueprint]].

## Visual format options

| Format | Best for | Watch out |
|---|---|---|
| **Slide grid** (PowerPoint, Keynote) | Executive readout | Forces compression that hides nuance |
| **FigJam / Miro** | Collaborative current-state work | Becomes sprawling without facilitation |
| **Smaply / Custellence / UXPressia** | Production deliverables | Tool lock-in; harder to share with non-licensed reviewers |
| **Spreadsheet** | Underlying database + filtering | Visual quality suffers; works if you generate views from it |
| **Journey-curve graphic** | Communicating emotional arc | Stripped of operational detail; pair with full map |

## Worked example header

```
Persona:        Enterprise marketing buyer evaluating composable DXP (P-001)
Job:            Decide whether composable architecture fits our team and stack (J-001)
Scope:          Trigger event (monolith pain / leadership mandate) → vendor shortlist
Map type:       Current state
Evidence base:  Interviews IDs 23–45 (Q1 2026 buyer pool); analyst-call notes;
                website analytics; sales-call transcripts (Gong)
Date | Version: 2026-04-12 | v1.0
Owner:          Bermon Painter
```

## Validation checklist

Before sharing the map:

- [ ] Header is filled completely, including evidence base.
- [ ] Stages match user experience, not company funnel.
- [ ] Every cell ties to evidence (interview ID, ticket, analytic).
- [ ] Pain points have severity tags.
- [ ] At least one opportunity per pain.
- [ ] Moments of truth marked (2–4).
- [ ] Map fits on one screen / page (compress if needed; don't sprawl).
- [ ] Single persona. (If multiple, build separate maps.)

## How to use the artifact

The map is a tool, not a deliverable. Test of value: after publishing, did at least one decision change in the next 30 days because of this map? If not, the map didn't earn its keep.

Use it in:
- **Sprint planning.** Pull from the opportunities row.
- **Cross-functional alignment.** Walk the map stage by stage; let each function name what they own.
- **Pre-design briefs.** Hand to designers as the "what's broken" upstream of any screen work.
- **Roadmap reviews.** Use the moments of truth as the strategic priority filter.

## Sources

- Risdon, C. & Quattlebaum, P. *Orchestrating Experiences*. Rosenfeld, 2018, ch. 5.
- Stickdorn, M. et al. *This Is Service Design Doing*. O'Reilly, 2018.
- Kalbach, J. *Mapping Experiences*. O'Reilly, 2nd ed., 2020.
