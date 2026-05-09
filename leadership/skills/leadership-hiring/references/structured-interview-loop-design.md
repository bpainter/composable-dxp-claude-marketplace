---
type: reference
plugin: leadership
skill: leadership-hiring
sources:
  - Adler, *Hire With Your Head* (3rd ed., Wiley, 2007)
  - Grosse & Loftesness, *Scaling Teams* (O'Reilly, 2017)
status: active
tags: [type/reference, plugin/leadership, topic/hiring, topic/interviewing]
---

# Structured interview loop design

Most loops fail in the same way: 4–6 interviewers each ask the same generic questions, no one owns specific evidence, the debrief is a personality vote, and the hiring manager defaults to whoever was loudest.

A structured loop assigns each interviewer a slice of the Performance Profile, gives them the question type they'll run, prepares them for the close they'll attempt, and produces evidence ready for the debrief.

## The principles

**One profile, distributed.** The loop covers the full Performance Profile across interviewers. No bullet is unprobed; no bullet is double-probed. Each interviewer knows which bullet(s) they own.

**Two-question interviews, not behavioral grab-bags.** Each interviewer runs an MSA + PSQ tailored to their bullet (see `performance-profiles-and-the-two-question-interview.md`).

**Distinct angles, not redundant questions.** Different interviewers probe different evidence. Never have two interviewers run the "tell me about a hard conflict" question.

**The hiring manager owns one of the deepest interviews.** Minimum 60 minutes, not stuffed in alongside something else. The hiring manager has the most context and the most accountability.

**One interviewer owns the close.** Late in the loop. They explicitly recruit — answer questions, address concerns, paint the path forward. Without this, your loop is interrogation; with it, the candidate leaves wanting the role.

**The loop is consistent across candidates for the same role.** Same bullets, same general structure. This is what lets you compare candidates in debrief and reduces bias.

## Loop archetypes

### Senior IC (Principal Engineer / Solution Architect) — 4-stage loop

| Stage | Who | Length | Focus | What they bring to debrief |
|---|---|---|---|---|
| 1. Recruiter screen | Recruiter / TA | 30 min | Motivation, comp, timeline, basic role fit | Pass/no-pass, motivation map, comp expectations |
| 2. Hiring manager | Hiring manager | 75 min | MSA on Profile bullets 1+2 (largest technical accomplishments) + PSQ on a real architectural problem | Evidence on bullets 1+2, hire/no-hire lean |
| 3. Technical peer | Senior peer | 60 min | MSA on bullets 3+4 (delivery, collaboration) + PSQ on a real delivery problem | Evidence on bullets 3+4, peer-fit read |
| 4. Cross-functional + close | Director or PD | 60 min | Bullets 5+6 (scope, growth, external presence), then explicit close | Evidence on bullets 5+6, candidate's questions/concerns |

### Engineering Manager / Engineering Lead — 5-stage loop

| Stage | Who | Length | Focus |
|---|---|---|---|
| 1. Recruiter screen | TA | 30 min | Motivation, scope expectations, comp |
| 2. Hiring manager | Hiring manager | 75 min | MSA on people-leadership accomplishments + PSQ on a real team problem |
| 3. Skip-level peer | Engineer who'd report up | 45 min | "What would you do in your first 30 days managing my team?" + their MSA on a similar transition |
| 4. Cross-team partner | PD or PM | 45 min | MSA on cross-functional collaboration + PSQ on a real partnership problem |
| 5. Senior leader + close | Senior Director | 60 min | Bigger-picture: vision, growth, the close |

### Director or Principal Solution Director — 5-stage loop

| Stage | Who | Length | Focus |
|---|---|---|---|
| 1. Recruiter screen | TA | 30 min | Motivation, scope, comp |
| 2. Hiring manager | Senior Director / Practice Lead | 90 min | MSA on practice-building or engagement-leading accomplishment + PSQ on a real practice problem |
| 3. Cross-capability partner | Peer Director from adjacent capability | 60 min | Influence, partnership, deal-shaping evidence |
| 4. Practitioner panel | 2–3 senior practitioners (panel) | 60 min | Technical credibility, leadership presence, "how would you spend the first 90 days" |
| 5. Executive + close | GM or VP | 60 min | Vision, fit with firm direction, the close |

## Allocating Performance Profile bullets across interviewers

Walk through the Profile with the loop laid out and write the interviewer's name next to each bullet. Every bullet should have a name. No name should have more than 2–3 bullets.

If a bullet has no obvious owner, that means either (a) you don't have the right interviewer in the loop, or (b) the bullet is too vague to probe. Fix one or the other before you start.

## Pre-loop interviewer briefing

Before the candidate arrives, the hiring manager spends 15 minutes with each interviewer (ideally async via a 1-page brief):

1. **The Performance Profile** (full document, not just their bullets — they need to know how their bullets fit)
2. **Which bullets they own**
3. **The MSA stem** ("Tell me about your most significant accomplishment that's similar to [bullet 2]…")
4. **The PSQ** (a specific real problem from the role they should pose)
5. **What the hiring manager *needs* this interview to surface** (the open question that will tip the decision)
6. **The debrief format and timing**

If you can't brief the interviewer this much, postpone their slot or replace them. Untrained interviewers introduce noise.

## Question allocation matrix — example

For the Senior Engineering Lead Performance Profile in `performance-profiles-and-the-two-question-interview.md`:

| Bullet | Owner | MSA | PSQ |
|---|---|---|---|
| 1. Architecture review + reference architecture | Hiring manager | "MSA where you owned an architecture decision under delivery pressure" | "30-day-in scenario at the F500 retailer — first conversation with their CTO" |
| 2. Hire and onboard 2 senior engineers | Hiring manager | "MSA where you scaled a team mid-engagement" | (briefly probed, since it overlaps bullet 1) |
| 3. Lead technical discovery for a $1M+ pursuit | Cross-functional peer | "MSA where you shaped a deal technically" | "Walk through how you'd structure the technical discovery for a [real pursuit] with these stakeholders" |
| 4. Quarterly reusable-asset cadence | Cross-functional peer | "MSA where you stood up a recurring practice ritual" | (lighter probe) |
| 5. Direct manage 4–6 engineers | Tech peer | "MSA managing a senior engineer through a hard quarter" | "Direct report says they want to be a Director in 18 months — they're not ready. Walk me through the conversation." |
| 6. External presence (talks, whitepapers) | Director / close | "MSA where you represented a practice externally" | (briefly probed) |

Now no one is duplicating coverage. The candidate gets a coherent loop. The debrief has bullet-by-bullet evidence.

## Sample MSA / PSQ stems by role family

### For an engineer/architect:
- MSA: "Tell me about the most technically demanding system you've shipped end-to-end."
- PSQ: "We have a real performance issue at [client X] — [describe it]. How would you triage it in the first 48 hours?"

### For an engineering manager:
- MSA: "Tell me about a team you took over that wasn't performing — what state was it in, what did you do, what changed?"
- PSQ: "One of your senior engineers and a senior PM are in repeated conflict. Walk me through what you do."

### For a senior consultant / solution director:
- MSA: "Tell me about the largest deal you've shaped from discovery through close — your role, the strategy, the result."
- PSQ: "[Real pursuit context]. Walk me through how you'd run the discovery."

### For a delivery director:
- MSA: "Tell me about a multi-stream engagement you ran where the timeline slipped. What did you do?"
- PSQ: "You inherit an engagement at Risk Gate 3 with two unhappy client stakeholders. Walk me through your first week."

## The interview-to-debrief handoff

Within 24 hours of their interview, every interviewer files written evidence per Performance Profile bullet. Not a vote — evidence. See `evidence-based-debrief.md` and `T_Interview_Scorecard.md` in `70_Templates/`.

## Common loop failures and the fix

| Failure | Symptom | Fix |
|---|---|---|
| No Performance Profile | Each interviewer has their own bar; debrief is chaos | Build the Profile *first*; don't open the role without it |
| Interviewer redundancy | Three people ask the "tell me about a hard project" question | Allocate bullets in the loop matrix |
| Hiring manager interview is too short | Manager runs a 30-min screen alongside other meetings | Block 75 min, no exceptions |
| No close | Strong candidate with competing offer goes elsewhere | Designate a closer; add 15 min to their slot for the close |
| Interviewers untrained | Half the loop runs behavioral, half runs gut | 15-min briefings or written brief; if you can't, replace them |
| Debrief is a vote | Whoever's loudest wins | Force evidence-per-bullet; see debrief facilitation |

## Reading

- Adler, *Hire With Your Head*, ch. 4–6
- Grosse & Loftesness, *Scaling Teams*, ch. on Interviewing (interviewer training, calibration, structured loops)
