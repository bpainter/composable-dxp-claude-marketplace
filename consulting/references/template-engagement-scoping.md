---
type: reference
project: skills-library
scope: plugin
plugin: consulting
tags: [type/reference, plugin/consulting, scope/template, topic/engagement, topic/contracting, source/block, source/kubr]
status: active
---

# Template — Engagement Scoping & Stakeholder Map

**Sources:** Block, *Flawless Consulting* (Pfeiffer, 2011) — see [`book-flawless-consulting.md`](book-flawless-consulting.md). Kubr, *Management Consulting* (4th ed., ILO, 2002) — see [`book-management-consulting-kubr.md`](book-management-consulting-kubr.md). Burtonshaw-Gunn, *Essential Tools for Management Consulting* (Wiley, 2010) for stakeholder analysis.

A scoping worksheet that combines Block's contracting model (the "what we agree to with the client" piece) with classical stakeholder analysis. Use it at the start of any engagement to prevent the most expensive failure mode in consulting: a fuzzy contract that produces a mismatch between what you delivered and what they expected.

## When to use it

- Before the SOW is signed (the formal contract document)
- Before the kickoff meeting (the relationship contract)
- When an existing engagement is drifting — re-contract
- When stakeholders shift mid-engagement (new sponsor, new exec, M&A)

## Section 1: Block's Contracting Meeting (the "social contract")

The SOW is the legal document; the **contracting meeting** is the relationship document. Block argues most consulting failures trace to a contracting meeting that was rushed, incomplete, or unilateral.

### The 8-step contracting structure (Block, p. 71, Figure 2)

1. **Personal acknowledgement** — start with rapport, not agenda
2. **Restate what the client said they want** — verbatim, then check it
3. **State your wants** — what you need from them to do good work
4. **Convert wants to commitments** — what each side will actually do
5. **Address concerns about control and vulnerability** — surface what's not yet said
6. **Reach agreement** — explicit, named
7. **Ask for feedback on the contracting process itself** — last 5 minutes
8. **Plan next steps** — concrete dates, owners, deliverables

### Worksheet

```markdown
# Engagement Contract — [Engagement Name]

## What the client said they want
[Their words, verbatim — restated and confirmed]

## What we say is the underlying problem
[Our diagnostic read — may differ from above]

## Success criteria — measurable
- [ ] By [date], we will see [observable outcome]
- [ ] By [date], we will see [observable outcome]
- [ ] By [date], we will see [observable outcome]

## Mutual commitments

**We commit to:**
- [Deliverables, time, expertise, approach]

**They commit to:**
- [Access to people, data, decisions; sponsor presence at key milestones; timeliness of feedback; ground rules]

## Ground rules (Block's defaults, p. 47)
- 50/50 responsibility: this engagement only works if both sides bring it
- No-fault: when something doesn't work, we don't blame; we recontract
- Equal footing: we're peers in this conversation; we both have authority to say no
- Personal feedback: we'll surface concerns in real time, not save them for the postmortem

## Engagement boundaries (what we WILL NOT do)
- [Out of scope: e.g., "We will not redesign your org chart"]
- [Out of scope: "We will not produce a final implementation plan"]
- [Out of scope: "We will not act as a manager for your team"]

## Decision rights
- Decisions we make alone: [internal methodology]
- Decisions they make alone: [scope changes, resource allocation, business strategy]
- Decisions we make together: [intermediate deliverable acceptance, pivot points]
```

## Section 2: Stakeholder Map (Kubr / Burtonshaw / Mendelow)

Even with a solid contract, the engagement runs through people whose interests, power, and engagement vary. The stakeholder map prevents being blindsided.

### The 4-quadrant influence/interest grid (Mendelow's matrix, in Burtonshaw)

```
                                  HIGH INTEREST
                                       │
                  Keep informed        │     Manage closely
                                       │       (key players)
                                       │
              ──────────────────────── ┼ ──────────────────────── 
                  HIGH                 │             LOW
                  POWER                │             POWER
                                       │
                  Monitor              │     Keep satisfied
                  (minimal effort)     │
                                       │
                                  LOW INTEREST
```

For each stakeholder, place them on the grid based on their **power** to affect the engagement and **interest** in its outcome.

### Worksheet

```markdown
# Stakeholder Map — [Engagement Name]

| Stakeholder | Role | Power (1-5) | Interest (1-5) | Quadrant | Position (Champion/Neutral/Skeptic/Blocker) | Key concern | Engagement plan |
|---|---|---|---|---|---|---|---|
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |

## Key players (Power 4-5, Interest 4-5)
For each: their goals, their fears, their measure of success for this engagement, the access they'll need.
- 

## Hidden players (Power 3-5, Interest 0-2)
The "didn't see them coming" risk. Often surface late and kill engagements. Identify and engage early.
- 

## Champions
The 1-2 people who actively want the engagement to succeed and will spend political capital on it.
- 

## Skeptics
Not blockers — but unconvinced. Convert through evidence and small wins. Often become the strongest champions.
- 

## Blockers
People with both power and an interest in the engagement failing (e.g., the predecessor whose strategy you're replacing). Strategy: isolate, work around, or convert via a credible peer.
- 
```

## Section 3: Discovery Interview Plan

Block's contracting meeting establishes the relationship. The discovery interviews surface the actual problem. Combine Block's five-layer interview with Kubr's diagnostic structure (Kubr §8.1–8.3).

### Block's 5 layers of discovery (Ch. 8)

1. **The presenting problem** — what they say is wrong
2. **Technical/business dimension** — facts, data, processes, structures
3. **The personal layer** — who's affected, what's at stake personally for the interviewees
4. **Client-as-player** — what role *the client* is playing in keeping the problem alive (politically risky, but essential)
5. **Resistance to change** — what's been tried before, why it didn't stick

Most consultants only get to layer 2. Layers 3-5 are where the engagement gets traction.

### Worksheet — interview planning

```markdown
# Discovery Interviews — [Engagement Name]

## Interview list (10-15 interviews typical for a mid-sized engagement)
| Interviewee | Role | Why this person | Date scheduled |
|---|---|---|---|
| | | | |

## Interview guide (45-60 min per interview)

**Opening (5 min)** — purpose, confidentiality, time check, "anything off-limits today?"

**Section 1: Their take on the problem (10 min)**
- "Tell me about [problem area] from your seat."
- "When did you first notice this?"
- "What's been tried?"

**Section 2: Technical/business dimension (15 min)**
- Specific facts, data sources, processes affected
- Bottlenecks, rework, dependencies

**Section 3: The personal layer (10 min)**
- "What does this mean for you personally?"
- "Who else is affected — and how?"
- "What are you most worried about as we work on this?"

**Section 4: Client as player (10 min — handle with care)**
- "What's the conversation about this that doesn't happen at the leadership level?"
- "If we solve this, what could go wrong politically?"
- "Where does the resistance live?"

**Section 5: Their wishes (5 min)**
- "If you had a magic wand, what would the future state look like?"
- "What can we do that would make the biggest difference for you?"

**Closing (5 min)** — "What didn't I ask that I should have?", confirm next steps, thank them.

## Synthesis themes
After all interviews, synthesize across the 5 layers. The synthesis is the diagnosis — feeds the analysis-and-decision phase.
```

## Pitfalls Block, Kubr, and Burtonshaw flag

- **Pretending to know what you don't.** Both Block and Kubr emphasize: clients respect "I don't know yet" more than confident hand-waving. Especially in discovery. (Block, Ch. 12; Kubr §8.5.)
- **Contracting too quickly.** The pressure to sign and start can short-circuit the contracting meeting. Slow down. (Block, Ch. 4.)
- **Ignoring the personal layer.** "Just the facts" interviews surface 30% of the problem. The personal and political layers are where the rest lives. (Block, p. 169.)
- **Stakeholder map drifts.** Power and interest shift mid-engagement (M&A, leadership change, project results). Refresh the map quarterly. (Burtonshaw, Ch. 3.)
- **Treating skeptics as blockers.** Skeptics push back, but skeptics with evidence often become the strongest champions. Spend time with them. (Block, Ch. 12 on resistance; Kubr §8.5.)
- **Over-relying on the sponsor.** If only one person wants the engagement to succeed, you're a sponsor change away from cancellation. Build a coalition.
- **Missing the implementation question.** Kubr notes only 30–50% of consulting assignments include implementation in scope (§1.4, p. 24). If you stop at recommendations, the work doesn't stick. Decide upfront whether implementation is in or out.
- **Saying yes to everything in contracting.** Block's most-quoted point: "**Saying no with grace** is harder than saying yes, and it's often the consultant's most valuable contribution." (Block, p. 51.)

## Hand-off downstream

- **Proposal/SOW** — the formal documents reflect the contracting meeting outcomes. See `consulting-proposal-strategist`.
- **Discovery findings** — feed the Analysis & Decision phase (Block phase 3, Kubr §8.6 synthesis).
- **Stakeholder map** — refreshed throughout; feeds change-management readiness if change is in scope (see `template-change-readiness-discovery.md`).
- **Risk register** — risks surfaced in discovery feed `consulting-sow-risk-advisor`'s RAID-log work.

## Skills that use this template

- `consulting-management-consultant` (primary — owns engagement scoping)
- `consulting-change-management-advisor` (consumes stakeholder map)
- `consulting-proposal-strategist` (uses scoping output to write the formal proposal)
- `consulting-sow-risk-advisor` (uses scoping output to identify SOW risks)
- `project-management-project-manager` (uses scoping output for delivery planning)
