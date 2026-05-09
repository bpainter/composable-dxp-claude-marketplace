---
description: Scope a session with the sponsor — Job 1 of the Six Jobs. Output is a sponsor-aligned scoping brief naming outcomes, decisions already made, what's out of scope, who must be present.

# Project context
type: command
project: skills-library
plugin: facilitation
tags: [type/command, plugin/facilitation]
status: active
---

Use the `facilitation-facilitator` persona to walk the user through Klein & Newman's first job: scoping. Most workshop pain originates here. Scoping is the most leveraged time you spend on a session — get it right and the design becomes obvious; get it wrong and no agenda will save you.

Reference: [`book-facilitating-collaboration.md`](../references/book-facilitating-collaboration.md), Chapter 3 (Scoping) and Chapter 4 (Working with Sponsors).

Steps:

1. **Establish the baseline.** Ask the user:
   - Who's the sponsor? (Title, decision authority, what they own)
   - What's the trigger event — why is this session being requested *now*?
   - What's been said about it already, to whom, and what got committed?

2. **Surface the outcome.** Push past topic to outcome:
   - Vague: "discuss strategy" → Specific: "prioritize Q2 initiatives and align on resource trade-offs"
   - Vague: "team alignment" → Specific: "leave the room with named owners on the top 3 stuck initiatives"
   - If the user can't name a measurable outcome, that's the finding — surface it back. The session may not be ready to design.

3. **Name what's out of scope.** Equally important. What is the sponsor *not* expecting from this session? What decisions are already made? What topics will inflame and derail?

4. **Identify who must be present.** Apply Pattern 211 (*Get the whole system in the room*). Push back on convenience-based attendance — the right people, not the available people.

5. **Surface political dynamics.** Per Klein & Newman: "Event sponsors will only trust an outside facilitator to shape critical work with a large team if he or she invests the time and care to understand the business issue at hand and the personal and political challenges faced by sponsors." Ask: who has interests in this outcome? Who's pre-aligned? Who's friction? What's the sponsor's relationship with the dissenters?

6. **Ask the budget question.** Time, money, sponsor energy. If this is a multi-day workshop, is the sponsor committed to a Sponsor Design Team? (See [`sponsor-design-team-playbook.md`](../skills/facilitation-workshop-designer/references/sponsor-design-team-playbook.md).)

7. **Produce the scoping brief.** A short markdown document with:
   - Outcome (one sentence, measurable)
   - Out of scope (3–5 named items)
   - Participants (with roles and rationale for each)
   - Constraints (time, budget, decisions already made)
   - Political map (who's pre-aligned, who's friction)
   - Open questions for sponsor
   - Recommended session length / format

8. **Hand off.** Recommend the next step:
   - For a single meeting (≤90 min) → call `/design-session` with the scoping brief
   - For multi-day → bring in the workshop-designer to set up the Sponsor Design Team
   - If the scoping conversation revealed the session shouldn't happen → say so plainly

Don't draft the agenda yet. Scoping is its own discipline. Get the outcome right before designing the process.
