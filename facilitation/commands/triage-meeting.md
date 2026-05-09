---
description: Diagnose a recurring meeting that's stopped producing value — apply the "do we even need this meeting?" filter, then recommend kill, halve, restructure, or convert to async.

# Project context
type: command
project: skills-library
plugin: facilitation
tags: [type/command, plugin/facilitation]
status: active
---

Use the `facilitation-meeting-architect` skill to triage a recurring or one-off meeting that isn't earning its slot. This is a Hoffman-grounded diagnosis: meetings that "took place" without producing outcome are bad design, not bad personalities.

Reference: [`book-meeting-design.md`](../references/book-meeting-design.md), [`meeting-design-toolkit.md`](../skills/facilitation-meeting-architect/references/meeting-design-toolkit.md).

Steps:

1. **Get the inputs.** Ask if not provided:
   - **Meeting name and cadence** (weekly status, monthly steering, daily standup, etc.)
   - **Length** (current)
   - **Number of attendees**
   - **Stated purpose** vs. **actual outcomes** in the last 4 sessions
   - **Symptoms** the user is observing (no decisions, low energy, side conversations, the same items recurring, attendance dropping, phones-out behavior)

2. **Run the five-test filter.** From the toolkit:
   - **Outcome test** — can the user name what's true after the meeting that wasn't before? If no → kill or restructure
   - **Participation test** — does the outcome require multiple voices? If no → send a doc, kill the meeting
   - **Synchronous test** — does the conversation need to be real-time? If no → convert to async
   - **People test** — are decision-makers actually attending? If no → reschedule or kill
   - **Information test** — does everyone arrive with prerequisite context? If no → fix pre-read first

3. **Diagnose the root cause.** Almost every failed recurring meeting falls into one of these patterns:
   - **Status theater** — performative updates that produce nothing. Fix: async status doc + 15-min monthly blockers-only sync.
   - **Wrong agenda math** — too many ideas × too many people × too little time (Hoffman's Ideas/People/Time). Fix: cut ideas or people.
   - **Wrong meeting type** — middle-meeting structure used for what should be a beginning or end meeting. Fix: classify and redesign per Chapter 7/8/9.
   - **Decision rule missing** — the meeting "discusses" because no one named the decision rule. Fix: pre-name with `/decision-rule`.
   - **Missing outcome** — the meeting exists because it's on the calendar. Fix: kill it.

4. **Recommend one of four moves**, in this order of preference:
   - **Kill it.** Replace with async + an on-demand sync if blockers emerge. Default for status meetings.
   - **Halve it.** Cut to half the duration and half the participants. Force the discipline.
   - **Restructure.** Convert to a clean meeting type (decision point, retro, course correction) per Hoffman's Beginning/Middle/End taxonomy.
   - **Leave it.** Only if the five-test filter passes cleanly.

5. **Output the redesign.** If recommending restructure, draft the new agenda using [`meeting-design-toolkit.md`](../skills/facilitation-meeting-architect/references/meeting-design-toolkit.md) — pick the right "Agendas by length" template (15 / 30 / 45 / 60 / 90 min) and populate it.

6. **Working agreements.** Recommend any of the cadence-level agreements that compound:
   - No agenda, no meeting (24h before)
   - Standing meetings reviewed quarterly
   - Decisions captured live, not in follow-up doc
   - Action items have owners and dates
   - Late attendees catch up after, not in the room

7. **Communication script.** If the recommendation is kill or halve, draft a 2–3 sentence message the user can send to the existing attendees explaining the change. Soften by acknowledging the meeting's history; harden by naming the outcome the new format will produce.

Don't recommend "make the meeting more engaging" — that treats a structural problem as a personality problem.
