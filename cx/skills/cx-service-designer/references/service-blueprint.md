---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-service-designer]]"
tags: [type/reference, plugin/cx, scope/skill, topic/service-design, topic/blueprint]
status: active
---
# Service blueprint

Drawn from Stickdorn et al., *This Is Service Design Doing* (chs. 5–7), and *This Is Service Design Thinking* (the original Shostack-derived blueprint format).

A service blueprint is the operational view of a service. Where the journey map shows what the customer experiences, the blueprint adds the **layers underneath** — the staff, systems, processes, and supporting infrastructure that produce that experience. Without the blueprint, the team designs the customer-facing layer in isolation and is then surprised when ops can't deliver it.

## When to build a blueprint

- After current-state journey mapping, before designing the future state — so the future-state design accounts for backstage feasibility.
- When a customer-facing problem turns out to be a backstage problem (handoff failures, unclear ownership, gaps between systems).
- When operationalizing a new service — the blueprint becomes the spec for ops, support, training, and tooling.
- When standing up cross-functional alignment — the blueprint is the shared artifact that names every team's role.

If the work is purely customer-facing and there's no operational change, a journey map alone is enough. Don't blueprint when nothing backstage moves.

## The five layers

```
┌──────────────────────────────────────────────────────────────────┐
│ PHYSICAL EVIDENCE — tangible things the customer sees / receives │
├──────────────────────────────────────────────────────────────────┤
│ CUSTOMER ACTIONS — what the customer is doing                    │
├──────────────────────────────────────────────────────────────────┤
│ ─── LINE OF INTERACTION ───                                      │
├──────────────────────────────────────────────────────────────────┤
│ FRONTSTAGE ACTIONS — what staff/system does that customer sees   │
├──────────────────────────────────────────────────────────────────┤
│ ─── LINE OF VISIBILITY ───                                       │
├──────────────────────────────────────────────────────────────────┤
│ BACKSTAGE ACTIONS — what staff/system does that customer doesn't │
├──────────────────────────────────────────────────────────────────┤
│ ─── LINE OF INTERNAL INTERACTION ───                             │
├──────────────────────────────────────────────────────────────────┤
│ SUPPORT PROCESSES — systems, vendors, infrastructure underneath  │
└──────────────────────────────────────────────────────────────────┘
```

Read top-to-bottom at any moment in time to see: what the customer experiences, what's happening that they see, what's happening that they don't, and what's underneath enabling it.

### Physical evidence

The artifacts the customer encounters: receipts, summaries, status emails, packaging, signage, the look of the website, the room they're in. Norman calls these *signifiers* — they tell the customer what's happening.

Most service designs underuse evidence. Customers experience uncertainty when intangible work is invisible. A "we received your application; here's what happens next" message is evidence; its absence is also evidence (of an unprofessional service).

### Customer actions

What the customer is doing. Pulled from the journey map. Be specific: "Submits formation questionnaire" not "engages with site."

### Frontstage actions

What staff or the system does that the customer can see. The agent who picks up the phone, the form validation message, the chat reply, the email confirmation.

### Backstage actions

What happens that the customer never sees. The ops team that routes the form to the right reviewer, the system that pings legal, the manager who approves the exception. This is where most service failures hide.

### Support processes

The infrastructure beneath both stages: CRM, ticketing system, vendor APIs, IT, finance, legal, knowledge bases, training programs. Things that aren't actions per se but enable actions.

## The three lines

The lines are diagnostic. Crossings reveal handoff risk.

- **Line of interaction** — every crossing is a customer-facing moment.
- **Line of visibility** — every crossing is an opportunity to make hidden work visible (or to keep it appropriately invisible).
- **Line of internal interaction** — every crossing is a handoff between teams or systems. Most service failures live here.

Audit your blueprint by asking: at every line crossing, who owns the handoff, what happens if it fails, and what evidence reaches the customer either way?

## How to build one

```
1. Pick the journey scope. The blueprint follows one journey for one
   persona. Multiple personas → multiple blueprints. Don't merge.

2. Lay down the customer actions row from the journey map. This is the
   spine.

3. For each customer action, fill in:
   - Physical evidence the customer encounters.
   - Frontstage action that produces it.
   - Backstage actions required to make the frontstage possible.
   - Support processes/systems involved.

4. Mark every line-of-internal-interaction crossing. List the team or
   system on each side and the artifact passed (a record, a notification,
   a file).

5. Identify failure modes. At each crossing, ask: what happens if this
   handoff is delayed, dropped, or wrong? Mark high-risk crossings.

6. Identify pain points and opportunities. Same as a journey map, but
   sourced from the operational layer too. Many opportunities live in
   backstage simplification, not in the customer-facing redesign.

7. Validate with frontline staff. They will spot omissions a designer
   never could. Their corrections are gold.
```

## Worked-example skeleton (formation flow)

```
                      | Land on site | Take questionnaire | Receive results | Schedule call
PHYSICAL EVIDENCE     | Hero, reviews | Save indicator    | Results PDF     | Calendar invite, prep email
CUSTOMER ACTIONS      | Browses       | Answers Q1–Q12    | Reviews + saves | Books, prepares
─── LINE OF INTERACTION ────────────────────────────────────────────
FRONTSTAGE ACTIONS    | Site loads    | Saves at each Q   | Generates PDF   | Sends invite
─── LINE OF VISIBILITY ────────────────────────────────────────────
BACKSTAGE ACTIONS     | None          | Routes to lawyer  | Lawyer reviews  | Lawyer prep notes
─── LINE OF INTERNAL INTERACTION ─────────────────────────────────
SUPPORT PROCESSES     | CMS, analytics | CMS, queue       | Lawyer panel    | Calendly, CRM
```

For each cell, full blueprints add: failure mode, owner, KPI. Production blueprints in tools like Miro / Smaply / Custellence include those.

## Common failure modes

- **Customer-facing only.** Stops at the line of interaction. Ops can't deliver what was designed; design rework follows.
- **Backstage romanticism.** Backstage is "where the magic happens." No, it's where invisible failures pile up. Audit it cold.
- **No support-process row.** The systems and infrastructure are missing. The blueprint reads as if work happens in air.
- **Frozen blueprints.** Blueprint built once, never updated. Reality drifts; blueprint goes stale; team stops trusting it.
- **No failure mode column.** A blueprint without failure modes is a process map. The diagnostic value comes from where things break.
- **One blueprint for everyone.** Different journeys, different blueprints. Merging produces unreadable artifacts.

## Tools

- Miro / FigJam — best for collaborative current-state work.
- Smaply, Custellence, UXPressia — purpose-built for journeys and blueprints; better for production deliverables.
- Excel/Sheets — fine for the table; lacks the visual diagnostic value of the line crossings.
- Slide format — readable; usually too constrained for full operational depth. Use for executive summary; keep the full blueprint elsewhere.

## Sources

- Stickdorn, M., et al. *This Is Service Design Doing*. O'Reilly, 2018. (Methods chapters and the implementation chapter.)
- Stickdorn, M. & Schneider, J. *This Is Service Design Thinking*. BIS Publishers, 2011.
- Shostack, G. L. "How to Design a Service." *European Journal of Marketing*, 1982.
- Polaine, A., Løvlie, L., Reason, B. *Service Design: From Insight to Implementation*. Rosenfeld, 2013.
