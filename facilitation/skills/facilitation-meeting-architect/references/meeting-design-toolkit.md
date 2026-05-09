---
type: reference
project: skills-library
scope: skill
plugin: facilitation
parent: "[[facilitation-meeting-architect]]"
tags: [type/reference, plugin/facilitation, scope/skill]
status: active
---
# Meeting design toolkit

Patterns and agendas keyed to meeting length and project moment. Use as the menu when designing.

## The "do we even need this meeting?" filter

Before scheduling any meeting, run these five tests:

1. **Outcome test** — can you name what will be true after the meeting that wasn't true before? If no, kill it.
2. **Participation test** — does the outcome require multiple voices? If one person has the answer, send a doc.
3. **Synchronous test** — does the conversation need to happen in real time? If async would work, use async.
4. **People test** — are the people who can decide actually attending? If not, reschedule.
5. **Information test** — does everyone have the prerequisite context? If not, send pre-read first.

If any test fails, fix the meeting before sending the invite.

## Agendas by length

### 15 minutes — standup or quick decision

```
00:00  Frame (1 min): "We're deciding X."
00:01  Round-robin (10 min): blocking issues, no discussion yet
00:11  Decisions (3 min): one issue, one decision, one owner
00:14  Close (1 min): what's the next checkpoint?
```

Use for: daily standups (with stand-up rule), single-decision sync, status check before a bigger meeting.

### 30 minutes — single decision

```
00:00  Frame (3 min): purpose, decision rule, options on the table
00:03  Information (5 min): the missing context, no debate yet
00:08  Discussion (15 min): trade-offs, dissent surfaced
00:23  Decision (5 min): choice made, owner named, recap
00:28  Close (2 min): next checkpoint
```

Use for: most decision meetings. Cap participants at 5 if possible.

### 45 minutes — design or proposal review

```
00:00  Frame (5 min): purpose, what kind of feedback wanted
00:05  Present (10 min): the work, uninterrupted
00:15  Clarifying questions (10 min): no critique yet, just questions
00:25  Structured critique (15 min): one round-robin pass; what works, what doesn't, what to change
00:40  Decisions and next steps (5 min): what changes, who owns the revision
```

Use for: design reviews, proposal reviews, technical RFC reviews.

### 60 minutes — kickoff or course correction

```
00:00  Frame (5 min): purpose, agenda, working agreements
00:05  Context (10 min): what brought us here, what's already decided
00:15  Discovery (15 min): surface concerns, perspectives, missing info
00:30  Direction (15 min): trade-offs, candidate paths
00:45  Decisions (10 min): land the decisions, name owners
00:55  Close (5 min): recap, communication plan, next checkpoint
```

Use for: project kickoffs, mid-project course corrections, scope renegotiations.

### 90 minutes — strategy session or complex decision

A full Scan/Focus/Act compressed. See [`template-workshop-arc.md`](../../../templates/template-workshop-arc.md) for the 90-minute arc.

## Standing-meeting redesigns

### The status meeting that's stopped working

Typical symptom: "everyone's in the room and nothing's changing."

Three redesign options, in order of preference:

1. **Kill it.** Replace with an async status doc updated weekly. Keep a 15-minute monthly sync for blockers only.
2. **Halve it.** Cut to 15 minutes, blockers only, hard stop. Owners go to a parking lot for follow-up.
3. **Restructure it.** Convert to a decision meeting (one decision per meeting). Status moves to async.

Avoid: "let's make the status meeting more engaging." That treats a structural problem as a personality problem.

### The recurring 1:1

Default 30 minutes, weekly or bi-weekly with direct reports. Two-thirds report-driven (their agenda), one-third manager-driven (your agenda + feedback).

Logged in `10_Practice/People/Reports/{Name}/` per the working agreement (one running 1:1 log per direct report, dated entries at the top).

### The recurring leadership meeting

Cadence to consider:

- **Weekly (30 min)** — operational; what changed, what's blocked
- **Monthly (60 min)** — tactical; review key metrics, surface emerging issues
- **Quarterly (full day)** — strategic; OKRs, resourcing, priorities — this is workshop-scale, not meeting-scale

## Beginning meeting patterns (Hoffman's Part 2, Ch 7)

The chapter opens many beginning-meeting designs. The four most useful for Bermon's work:

### Project kickoff
- **Outcome**: aligned scope, named roles, working agreements, first risks
- **Length**: 60–90 minutes
- **Agenda spine**: purpose → introductions → scope → roles → risks → working agreements → next checkpoint

### Stakeholder alignment
- **Outcome**: shared understanding of positions, named trade-offs, agreed success criteria
- **Length**: 60 minutes
- **Agenda spine**: frame → each stakeholder's position (round-robin) → trade-off mapping → success criteria → decisions deferred to whom

### Sprint planning
- **Outcome**: sprint goal, committed backlog, capacity-confirmed, dependencies surfaced
- **Length**: depends on sprint length (60–120 min for a 2-week sprint)
- See [`project-management-sprint-planning`](../../../../project-management/skills/project-management-sprint-planning/SKILL.md)

### Discovery kickoff (Slalom Summit Mobilize → Discover)
- **Outcome**: aligned on discovery scope, methods, participants, timeline
- **Length**: 60 minutes
- **Agenda spine**: discovery hypothesis → method selection → participants and roles → timeline → outputs → checkpoints

## Middle meeting patterns (Hoffman's Part 2, Ch 8)

The most common patterns:

### Decision point
- **Outcome**: decision made, owner named
- **Length**: 30–45 min
- **Agenda spine**: see "30 minutes — single decision" above

### Design review
- **Outcome**: agreed changes to the work
- **Length**: 45–60 min
- **Agenda spine**: see "45 minutes — design or proposal review" above

### Risk review
- **Outcome**: re-prioritized RAID log
- **Length**: 30 min
- **Agenda spine**: top 5 risks → mitigation status → new risks → decisions → owners
- See [`project-management-raid-log`](../../../../project-management/skills/project-management-raid-log/SKILL.md)

### Course correction
- **Outcome**: agreed redirect
- **Length**: 60 min
- **Agenda spine**: see "60 minutes — kickoff or course correction" above

## End meeting patterns (Hoffman's Part 2, Ch 9)

### Retro
- **Outcome**: 2–3 changes for next iteration
- **Length**: 60–90 min
- See [`project-management-retro-facilitator`](../../../../project-management/skills/project-management-retro-facilitator/SKILL.md)

### Project close
- **Outcome**: outcomes vs. intent assessed, lessons captured, handoff complete
- **Length**: 60 min
- **Agenda spine**: outcomes vs. intent → key decisions and what we learned → handoff items → next checkpoints (if any) → recognition

### Handoff meeting
- **Outcome**: receiving party has the context to operate
- **Length**: 30–60 min
- **Agenda spine**: state of play → open items and owners → decisions made and rationale → next milestones → questions

## Working agreements that compound

Worth establishing once and keeping:

- **No agenda, no meeting** — agenda visible 24h before
- **Standing meetings reviewed quarterly** — kill or restructure anything not earning its slot
- **Decisions captured live** — not in a follow-up doc
- **Action items have owners and dates** — no "TBD"
- **Late attendees catch up after, not in the room** — the meeting doesn't restart for them
