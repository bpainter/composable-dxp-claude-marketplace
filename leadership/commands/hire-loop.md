---
description: Hiring workflow end-to-end — Performance Profile, sourcing plan, interview loop design, evidence-based debrief, close. Routes through the right phase based on where you are.
type: command
project: skills-library
plugin: leadership
tags: [type/command, plugin/leadership, plugin/hiring]
status: active
---

Use the `leadership-hiring` skill to run any phase of a hiring loop.

## When the user invokes this

Ask one question if not provided: **"Which role and which phase — opening (Performance Profile), interviewing (loop design / scorecards), debriefing (decision), or closing?"**

## Phase routing

### Opening — define success first

Use when the role is being scoped or the JD reads as a skills shopping list.

1. Pull or create the role brief at `10_Practice/People/Hiring/{Role}/_Brief.md` from `70_Templates/T_Hiring_Role_Brief.md`
2. Walk the user through writing the **Performance Profile** — 5–8 outcome-based bullets at the 6–12 month horizon. Use the test from `leadership-hiring/references/performance-profiles-and-the-two-question-interview.md`: would I be thrilled if they did exactly this and nothing else?
3. Draft the **sourcing plan** — channels, target outreach count, named referrals
4. Design the **interview loop** — assign each Performance Profile bullet to an interviewer; build the matrix; specify the close owner
5. Save the brief

### Interviewing — prep an interviewer

Use when an interview slot is approaching.

1. Pull the role brief; identify which bullets the interviewer owns
2. Draft the **MSA stem** ("Tell me about your most significant accomplishment that's similar to [bullet]")
3. Draft the **PSQ** — a real problem from the role
4. Provide the fact-finding follow-up sequence (when, where, role, what they did, who, result, what they'd change, what they learned)
5. Render the in-conversation scorecard scaffold from `leadership-hiring/references/scorecard-mini.md`

### Debriefing — facilitate the decision

Use after all loop interviews are complete.

1. Pull all filed scorecards
2. Frame the debrief: round of leans first (no rationale), then bullet-by-bullet evidence walk
3. Identify the bullets where leans diverged — focus the discussion there
4. Probe the strongest no-hire and strongest hire on disputed bullets
5. Force the hiring manager's call at the end (hire / hire pending follow-up / no-hire)
6. Capture the decision in `T_Interview_Debrief.md` — decision + reasoning + dissent

### Closing — convert the candidate

Use when an offer is imminent or a candidate is wavering.

1. Pull the motivation map from earlier interviews
2. Identify the **opportunity gap** — what does this role offer that the candidate said they wanted that their current role doesn't?
3. Draft the close conversation — open questions to surface remaining concerns; specific responses
4. If counteroffer: don't engage in bidding war; reframe to opportunity gap
5. Set offer timeline and commitment process

## Don't

- Skip the Performance Profile because "we know what we want." If it isn't written, the loop will drift.
- Conduct debrief as a vote — every interviewer cites evidence per bullet, not "I liked them"
- Soft-close — the close is explicit recruiting, not waiting for the candidate to decide

## Pairs with

- `/transition` — once the hire accepts, immediately set up the 30/60/90 plan
- `/feedback` — when interviewer calibration is off and someone needs coaching on hiring craft

## See also

- `leadership-hiring/SKILL.md` for the full method
- `70_Templates/T_Hiring_Role_Brief.md`, `T_Interview_Scorecard.md`, `T_Interview_Debrief.md`
