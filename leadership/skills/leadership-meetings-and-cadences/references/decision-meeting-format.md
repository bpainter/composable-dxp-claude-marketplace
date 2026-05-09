---
type: reference
plugin: leadership
skill: leadership-meetings-and-cadences
status: active
tags: [type/reference, plugin/leadership, topic/meetings, topic/decisions]
---

# Decision meeting format

A decision meeting has one job: make a decision. Most "decision" meetings fail because the decision wasn't named, the decider wasn't named, the options weren't laid out before the meeting, or the team treated the meeting as a brainstorm.

## The contract

Before scheduling, the meeting owner commits to:

1. **Naming the decision.** Specific. Singular. "By end of meeting, we will decide [X]."
2. **Naming the decider.** One person. The decider may consult, but they own the call.
3. **Sending a pre-read 48 hours ahead** with options and tradeoffs. Not a problem statement. Options.
4. **Inviting the smallest set of people** who can produce the best decision: the decider, 2–3 people with deep context or accountability, and any expert needed to test a critical option.
5. **Committing to make the call in the meeting** (or escalating if the meeting reveals the decision needs higher authority / more analysis).

If any of these aren't true, it's not a decision meeting. Either rescope it or skip it.

## The pre-read

Sent 48 hours ahead. 1–3 pages. Structure:

```
Decision: [single sentence — what we're deciding]
Decider: [name]
By when (this meeting): [date / time]

Background (max 1 page)
- Why this decision matters
- Constraints
- Anything previously tried or considered

Options (2–4)
For each option:
- One-line description
- Pros (3–5 specific points)
- Cons (3–5 specific points)
- Cost / effort estimate
- Reversibility (how hard to undo if wrong)
- Recommendation (yes / no / lean / no preference)

Recommended option (if the author has one)
- Which one
- Why
- What would change the recommendation

Open questions
- Things this meeting needs to resolve before deciding
- Things deferred (not blocking this decision)
```

The pre-read does the work. If the pre-read is half-baked, the meeting will be half-baked. Insist on it.

## The meeting (30–45 min)

### 0:00–0:05 — Frame

> "We're here to decide [X]. Decider is [name]. We'll make the call by end of meeting. Pre-read assumed read — quick check: anyone need a 60-second recap?"

If 30%+ haven't read it, you have a problem. Either reschedule (they didn't take the meeting seriously) or do a 5-minute brutal recap and proceed (eats your decision time).

### 0:05–0:25 — Test the options

Not "discuss the problem." Test the options.

The decider asks the room:
- "Strongest argument *for* option A — anyone?"
- "Strongest argument *against*?"
- (Same for B, C, D)
- "What would have to be true for option B to be the right call?"
- "What's the option none of us are considering?"

The job of the room is to stress-test the options on the table, not to invent new ones in real time. If a new option emerges that's clearly stronger, name it, see whether anyone in the room can develop it in 5 minutes; if not, defer the decision and add the option to a revised pre-read.

### 0:25–0:35 — Surface dissent

> "Before I call this — what's the dissent? Who thinks I'm about to make a mistake, and why?"

Force the dissent into the open. The decider then either:
- Updates the call based on the dissent (good — that's why the meeting exists)
- Acknowledges the dissent and proceeds (also good — at least the dissenter is heard)
- Discovers the dissent reveals a missing option or analysis (defer to revised pre-read)

### 0:35–0:40 — Decision and follow-ups

> "Decision: [X]. Owner: [name]. Next steps: [list]. We'll review [outcome / risks] at [date]."

Captured live in a doc. Sent to attendees within 24 hours.

## Common failures and the fix

### "We need more data."

Sometimes true. Often a stall. The decider should ask: *"What specific data, by when, would change my decision? If we can't name it, we don't need more data — we need to decide."*

If you genuinely need more data: defer the decision to a specific date with the data-owner named. Don't let the meeting end ambiguously.

### Meeting drifts into brainstorming.

The decider's job is to redirect: *"Good idea — that's a different option. Are we replacing one of these or adding it? If adding, the discussion expands and we won't decide today. Which do you want?"*

### One person dominates and the decider doesn't push back.

> "I want to hear from [quiet person] before I call this — what's your read?"

Force the quiet voices in. They often see the option that survives stress-testing.

### The decider isn't present or hasn't read the pre-read.

This is a fatal failure. Reschedule. Don't waste 6 people's time so the decider can be brought up to speed.

### "Let's table this."

Acceptable only with: a date when it gets retabled, a specific reason, and a named owner of whatever needs to happen between now and then. Otherwise "table" = "the issue rots until it explodes."

## Disagreement after the decision

Bezos's "disagree and commit" applies. Once the decision is made, the dissenter:
- Stops re-litigating the decision
- Commits to executing
- Names the conditions under which they'd want to revisit (so the team learns)

If a dissenter cannot disagree-and-commit, the underlying issue is bigger than this decision; have that conversation separately.

## Reversibility — the most underweighted variable

Bezos's distinction: Type 1 decisions (one-way doors, hard to reverse) deserve heavy analysis; Type 2 decisions (two-way doors, easy to reverse) deserve fast experimentation.

The decision meeting should explicitly tag the type:
- "This is a two-way door — we're going to try it, learn, and adjust if needed."
- "This is a one-way door — we need to be very confident before we commit."

Many decision meetings stall because the team treats every decision like Type 1. Force the question.

## The decision log

Every significant decision should land in a Decision Log entry — see `T_Decision_Log.md` in `70_Templates/`. Records:
- The decision
- The decider
- The options considered
- The reasoning
- The dissent
- The conditions for revisiting

The log is how an org learns. Without it, you re-decide the same thing every six months, often worse the second time.

## Reading

- Hoffman, *Meeting Design*, ch. on agendas and conflict
- Bezos's annual letters on Type 1 vs. Type 2 decisions and disagree-and-commit
