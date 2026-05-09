---
type: reference
plugin: leadership
skill: leadership-people-leader
source: Lopp, Michael. *Managing Humans: Biting and Humorous Tales of a Software Engineering Manager* (3rd ed., Apress, 2016)
status: active
tags: [type/reference, plugin/leadership, topic/people-management, topic/archetypes]
---

# Managing Humans — archetypes and patterns (Lopp)

Michael Lopp ("Rands") writes about engineering management as semi-fictional tales drawn from real experience at Apple, Slack, Netscape, Palantir, Pinterest, and Borland. The value isn't a unified framework — it's a catalog of recognizable patterns, named so you can spot them faster.

This reference distills the archetypes most useful for senior leaders managing knowledge work.

## The 1:1 archetypes

Lopp's taxonomy of who shows up to your 1:1 (often the same person rotates between archetypes week to week).

### The reporter

Comes with status updates. Tells you what they did this week. Looks to you for validation or correction.

**What they need.** Engagement beyond status. Their 1:1 has become a verbal version of a written report. Push: "What's the harder problem you're not bringing me?"

### The talker

Talks the entire 30 minutes without pause. You leave knowing every detail of their week. You haven't asked one question.

**What they need.** Structure. Lopp recommends: "I want to make sure we have time for [specific thing] — let's anchor on that and come back if we have time."

### The venter

Brings frustration. The team is broken, the client is unreasonable, the tools don't work. Doesn't want solutions; wants to be heard.

**What they need.** Listening first. Then a careful pivot: "I hear that. What's the part of this you can change?" (Trichotomy of control, applied 1:1.)

### The silent one

Shows up. Doesn't bring much. "Everything's fine." 30 minutes of awkward.

**What they need.** Patience and structure. Lopp: don't fill the silence; ask better questions; sometimes the silence means real trust hasn't formed yet, and the work is to keep showing up consistently. After multiple silent 1:1s, ask directly: "Is there something you wish you could bring up but feel you can't?"

### The crisis-bringer

Every 1:1 is an emergency. Always something on fire.

**What they need.** Help distinguishing real crisis from reaction. Coach them on the trichotomy. The first 5 minutes can be "the crisis"; the rest of the 1:1 is for everything else. Otherwise, every 1:1 becomes urgent-only and the longer-arc work disappears.

### The pre-prepared

Comes with a written agenda, three open items, and a clear ask for each. Highly effective.

**What they need.** Less of you. Your job is to help, not to add structure they don't need. The risk is over-managing; defer to their agenda.

### The avoider

Cancels 1:1s. Reschedules. Shows up late. The 1:1 keeps slipping.

**What they need.** A direct conversation. The avoidance is data. Are they overwhelmed? Avoiding feedback they fear? Don't trust the cadence? Name it: "I notice we keep moving our 1:1. Help me understand what's going on."

## The Update / Question / Decision taxonomy

Every item that surfaces in a manager's day is one of three:
- **An update** ("here's what's happening")
- **A question** ("I need help thinking through X")
- **A decision** ("I need a call by [date]")

Updates can usually go async. Questions and decisions need synchronous time.

When a direct comes to you with something, ask: which is this? They often don't know. Forcing them to classify sharpens the request and conserves your time.

## The "asshole detector"

Lopp's framing: meetings reveal team health. The asshole pattern (one person dominating, dismissing others, pattern-matching everyone to their previous bad takes) is visible in 30 seconds if you watch.

The hard call for a manager: the asshole is sometimes a high performer. Lopp's read: keep them anyway and the team's culture decays; lose them and the team's potential rises (and usually their performance was less load-bearing than it appeared).

This is a Lopp specialty: he writes about firing high performers who are toxic. The math usually favors the team.

## The new boss

When you're the new boss to an inherited team:
- The team is watching everything you do
- The first 30 days set the tone for the next 18 months
- Don't reorganize in week 2 (the most common new-boss mistake)
- Have 1:1s before doing anything material
- Listen more than you talk
- Honor the predecessor publicly

Connection to `taking-over-a-team.md` in `leadership-onboarding-and-transitions` — Lopp's version is the same playbook in different language.

## The skip-level

Lopp on skip-levels:
- The IC's signal beats the manager's filter
- Quarterly is usually enough cadence
- Don't ask "how is your manager doing?" — ask about the work and the team
- The skip-level should know it's not an end-run on their manager
- Patterns across skip-levels matter more than individual reports

See `skip-level-meetings.md` in `leadership-meetings-and-cadences` for the broader framework.

## The Bored Person

Lopp identifies a specific failure mode: a high-performing person who is no longer challenged. Symptoms:
- Output quality high but engagement visibly dropping
- Meetings: less contribution than usual
- Code reviews / artifact reviews: less depth, more "looks fine"
- Side conversations / extracurriculars increasing
- Vague unhappiness in 1:1s — "I don't know, I just feel… off"

The diagnosis is often: they've outgrown the role and need a stretch. The intervention is to find the next thing — a stretch project, a new scope, a role transition.

If you don't intervene, they leave within 6 months. Lopp's experience: high performers who are bored leave for the next thing whether or not you make space for them. The choice is whether you create the space or watch them find it elsewhere.

## The Difficult Conversation

Lopp's signature topic. Patterns:

- The conversation should happen weeks before it does (most managers wait too long)
- The hard conversation is usually a series of conversations, not a single event
- Don't pad with positive feedback (the "shit sandwich"); be direct
- Specifics over generalities
- Listen to the response before responding
- Don't write the script in your head and then deliver it regardless of what they say
- Sometimes the conversation reveals you were wrong about the situation

For firing specifically (see also `firing-with-dignity.md`):
- By the time you're firing, the conversation should be expected
- Surprise = your fault for not having earlier conversations
- Be clear, brief, dignified
- Have logistics ready (last day, transition plan, severance, communications)
- Don't moralize or justify excessively

## The Senior Hire

Lopp on hiring senior people specifically (relevant to Bermon's hires of Senior Architects, Solution Directors):
- They know more than you about their domain — that's the point
- They will probe whether you'll let them actually own scope
- The first 90 days they're testing whether the role matches the pitch
- Onboarding senior people is harder than juniors (less guidance to give; more autonomy to design)
- The early-warning signal: they go quiet in their first month — could be ramping, could be deciding the role isn't what they thought

## The Open Floor Plan

Lopp is direct about open offices: they suppress deep work, increase interruption, and produce surface-level collaboration that isn't worth the cost. (Seconds DeMarco & Lister in *Peopleware*.)

For consulting context: client-site, hot-desking, and constant availability are open-office equivalents. The senior practitioner who can never get a quiet hour is being silently degraded.

## The Bad Manager Patterns (per chapter intro)

Each Lopp chapter has a "Good Manager / Bad Manager" framing. Common bad-manager patterns:

- Talks more than listens
- Confuses busy-ness with productivity
- Skips 1:1s when busy
- Avoids hard conversations
- Reorganizes in response to discomfort
- Hires in their own image
- Confuses charisma with leadership
- Performs urgency they don't feel
- Confuses consensus-building with leadership
- Confuses being liked with being effective

Use as a self-audit. Most senior leaders have 2–3 from this list as drift patterns.

## What Lopp gets right that other management books miss

**Texture.** The semi-fictional story format gives you the *feel* of these situations, not just the framework. You recognize the meeting where the asshole derailed everything; you recognize the bored high performer; you recognize the silent 1:1.

**The role-specific context.** Engineering management has its own vocabulary, dynamics, and failure modes. Lopp writes from inside it. This translates to senior consulting management with adjustments — the patterns are remarkably consistent.

**Permission to be human.** Lopp writes about losing his cool, about uncertainty, about the ambiguity of every senior role. The vulnerability is a counter to the "great leader" mythology that makes other books less useful.

## Reading

- Lopp, *Managing Humans* (3rd ed.) — the canonical collection; readable in arbitrary order
- Lopp's blog *Rands in Repose* (randsinrepose.com) — ongoing essays in the same voice
- Lopp, *The Art of Leadership: Small Things, Done Well* — more recent, more synthesized
- Lopp's podcast *Whatevr* (when active) — conversational version of the same patterns
