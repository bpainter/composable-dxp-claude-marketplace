---
name: project-management-retro-facilitator
description: Run retrospectives the team actually learns from — picking the right format for the moment, surfacing real signal (not just nice-to-haves), turning observations into committed actions, and tracking those actions sprint over sprint. Covers Scrum sprint retros, project-end retros, post-incident retros, and quarterly retros. Use this skill any time the user is preparing or running a retrospective — "set up retro," "what format should we use," "how do we make retros useful," "what came out of last retro" — even when "retro" isn't said. Trigger whenever a learning loop is being closed so the team's process actually improves rather than re-litigating the same complaints sprint over sprint.

# Project context
type: skill
project: skills-library
plugin: project-management
aliases: [project-management-retro-facilitator]
tags: [type/skill, plugin/project-management topic/project-management, topic/agile]
status: active
---

# Retrospective Facilitator

A retro is a learning loop. It only earns its keep if the team's process gets better between retros. The most common failure: retros become a complaint hour that produces no committed change, and over time people stop showing up emotionally even if they show up calendrically.

This skill picks the right format, runs the meeting, captures the real signal, and — most importantly — turns observations into committed actions tracked into the next sprint.

For small cross-functional teams (engineering + design + content / SMEs), retros need to surface cross-discipline friction, not just engineering issues. Default the format to one that draws out hand-off pain.

Pair with `pm-sprint-planning` (the planning loop the retro feeds), `pm-weekly-status` (the trust artifact retros help calibrate), `pm-raid-log` (where action items often land), `consulting-management-consultant` for stakeholder mechanics, and `consulting-humanize` for cleaning up retro outputs that get shared widely.

## When to use this skill

- End of every sprint (default Scrum cadence).
- Project milestone or phase boundary.
- After a launch or major release.
- After an incident (post-incident retro / blameless postmortem).
- Quarterly or end-of-engagement.
- When someone asks "why do we keep hitting the same problem?" — that's a retro signal.

## Core posture

- **Psychological safety is the binding constraint.** A retro without it produces sanitized observations and no real signal. Establish norms; protect them.
- **Specific over general.** "Communication was bad" is a vibe. "We didn't know the design was approved until Wednesday" is a fact you can act on.
- **Actions, not observations.** A retro that ends with a list of things people noticed is incomplete. Each meaningful theme becomes a committed action with an owner.
- **Track previous actions.** Every retro starts by checking last retro's actions. If they didn't get done, the retro itself isn't working.
- **Rotate facilitation.** Same person every time produces the same patterns. Rotate so the team owns the practice.
- **Match the format to the situation.** Different retros surface different signal. Don't run the same format every time forever.

## Choosing the format

Retro formats are tools. Pick the one that fits the situation.

| Format | Best for | Time | Outputs |
|---|---|---|---|
| **Start / Stop / Continue** | Sprint retros, quick check-ins | 30-45 min | Behaviors to start, stop, keep |
| **4Ls (Liked / Learned / Lacked / Longed for)** | Sprint retros where the team needs more nuance than start/stop/continue | 45-60 min | Categorized observations |
| **Mad / Sad / Glad** | Sprint retros where emotional read matters | 45 min | Emotional signal across team |
| **Sailboat (Anchors / Wind / Rocks / Island)** | Mid-project retros, looking at what's slowing vs. speeding the team | 60 min | Forces map; obstacles and accelerators |
| **5 Whys** | Post-incident, post-failure | 30-60 min | Root-cause chain |
| **Timeline / Story** | Project-end, milestone retros | 90+ min | Narrative of how it went, with reflection points |
| **Lean Coffee** | When the team has lots to say but no fixed agenda | 45-60 min | Topic-driven, time-boxed discussion |

Default cadence for cross-functional sprint retros: **Start / Stop / Continue** for routine sprints; **4Ls** when the team feels there's more to discuss; **Sailboat** at mid-phase to take a longer view.

## Standard retro agenda

Most retro formats use the same five-phase shape (Esther Derby & Diana Larsen, *Agile Retrospectives*):

```
1. Set the stage              (5 min)   — establish purpose, norms, prime listening
2. Gather data                (15-30)   — facts, events, observations
3. Generate insights          (10-20)   — patterns, themes, "why"
4. Decide what to do          (10-15)   — concrete actions with owners
5. Close                      (5)       — appreciation, next steps
```

For a 1-hour retro on a small team, the time budget is approximately:

- Set stage: 5 min
- Gather data: 20 min
- Generate insights: 15 min
- Decide: 15 min
- Close: 5 min

### Step 1: Set the stage

Open with:

- The retro's purpose (improve how we work together).
- The norms (Norm Kerth's Prime Directive is a useful anchor):

  > "Regardless of what we discover, we understand and truly believe that everyone did the best job they could, given what they knew at the time, their skills and abilities, the resources available, and the situation at hand."

- The format you'll use today and how long you'll spend in each phase.
- A check-in question (5-10 seconds per person): "How are you arriving today?" or "One word for the last sprint."

The check-in matters. People who spoke once are 3x more likely to speak again than people who haven't.

### Step 2: Gather data

This is where the format does its work. Examples below.

**Start / Stop / Continue:**

```
| Start | Stop | Continue |
|---|---|---|
| What new behaviors should we add? | What should we stop doing? | What's working that we should keep? |
```

Each person writes silently first (3-5 min), then shares one item at a time, posting to the board. Cluster related items.

**4Ls:**

```
| Liked | Learned | Lacked | Longed for |
|---|---|---|---|
| What went well? | What did we learn? | What was missing? | What did we wish we had? |
```

**Sailboat:**

```
- Anchors (what's holding us back)
- Rocks (risks ahead)
- Wind (what's pushing us forward)
- Island (where we're trying to get to)
```

For each, the team places stickies; the metaphor helps surface things that might not show up in a more neutral format.

**5 Whys** (post-incident):

Start with the failure. Ask "why?" five times. Each answer points to the next "why?" The chain often ends not at a single root cause but at a process or systemic factor.

> Failure: We shipped a Contentful schema change that broke the live site.
> Why? Because the schema migration ran in production without a check.
> Why? Because the migration script wasn't reviewed.
> Why? Because we don't have a review step for migrations.
> Why? Because we hadn't standardized the process when the team grew.
> Why? Because nobody owned defining it.

Action: define schema-migration review and ownership.

### Step 3: Generate insights

After data is on the board:

- **Cluster.** Group related stickies into themes. Use silent grouping or affinity-mapping.
- **Read out.** A team member walks through each cluster.
- **Discuss the why.** For the top 2-4 clusters, discuss what's underneath. Why is this happening? What's the pattern?
- **Vote.** Dot-vote on which themes to act on (each person gets 2-3 dots).

Avoid taking action on every theme. Two or three is plenty. A retro that produces a list of 12 actions produces zero, because nobody can hold 12.

### Step 4: Decide what to do

For each top-voted theme, the team commits to a specific action. Each action has:

- **What**: a concrete thing the team will do or change.
- **Owner**: one person accountable.
- **By when**: a deadline.
- **How we'll know it worked**: an observable.

Examples:

> **Theme**: Design hand-offs are arriving mid-sprint, blocking engineering.
>
> **Action**: Move design review gate to end-of-prior-sprint. Update the Definition of Ready.
> **Owner**: [Design lead]
> **By when**: Before next sprint planning.
> **How we'll know**: At next sprint planning, designs for committed stories are already approved.

A bad version of the same action:

> "We should probably do design earlier."

Vague, unowned, undated. Won't happen.

### Step 5: Close

- **Appreciations**: each person names one teammate they appreciated this sprint and why. Quick. Low key. Builds psychological safety for next time.
- **One-word check-out**: how each person is leaving the retro.
- **Next retro date** confirmed.

## Action tracking

Actions go somewhere durable. Options:

- **`pm-raid-log`** — actions are often dependencies or commitments.
- **A retro-actions doc** in the project wiki.
- **Sprint board** — actions become tasks in the next sprint.

The next retro starts by reviewing those actions. Not done? That's a signal: the team committed to something it couldn't do, or the action isn't a priority. Either is OK to surface; what isn't OK is silently ignoring.

## Format examples by situation

### Sprint retro (default)

Format: Start / Stop / Continue.
Time: 45-60 min.
Cadence: every sprint.

### Mid-project retro

Format: Sailboat.
Time: 60-90 min.
Cadence: at phase boundaries (e.g., end of Phase 1).

The longer time horizon surfaces things that don't show up in sprint retros — strategic drift, role clarity, team-shape issues.

### Post-incident retro (blameless postmortem)

Format: Timeline + 5 Whys.
Time: 60-90 min.
Cadence: after any production incident or significant near-miss.

Norms: explicitly blameless. The Prime Directive applies with extra force. The output is a postmortem doc with: timeline of events, what happened, contributing factors (not "root causes" in the singular — usually it's a chain), action items.

### Project-end / engagement-end retro

Format: Timeline / Story + 4Ls.
Time: 2-3 hours, possibly with a break.
Cadence: at the end of a project, phase, or engagement.

This retro produces lessons that flow forward into the next engagement. Document the output in a project-end memo. Common questions:

- What worked that we'd want to do again?
- What didn't work that we'd want to avoid?
- What surprised us?
- What were the highest-leverage decisions?
- What advice would we give the next team starting a similar engagement?

### Quarterly retro

Format: 4Ls or Sailboat.
Time: 90-120 min.
Cadence: quarterly.

Looks across multiple sprints. Good for surfacing systemic issues that don't show up in any one sprint.

## Output format

A retro generates one or two artifacts.

### Retro notes (working doc)

```markdown
# Retro: [Sprint N / Date]

**Format**: Start / Stop / Continue
**Facilitator**: [name]
**Attendees**: [names]

## Norms reaffirmed
[Prime Directive or team's working norms.]

## Last retro's actions (review)
- [Action 1]: [✅ Done / 🟡 In progress / ❌ Not done]. Notes.
- [Action 2]: ...

## Data gathered

### Start
- ...
- ...

### Stop
- ...

### Continue
- ...

## Themes (clustered)
1. [Theme name] — [count of related items / vote count]
2. [Theme name] — ...
3. ...

## Actions committed
| Action | Owner | By when | How we'll know |
|---|---|---|---|
| ... | ... | ... | ... |
| ... | ... | ... | ... |

## Appreciations
- [Name → Name]: ...

## Check-out (one word per person)
[List]
```

### Action items (durable)

Add to the action tracker (sprint board / RAID / wiki). The retro notes are working memory; the action items are the persistent layer.

### Project-end retro memo (when applicable)

A 1-2 page summary written after a project-end retro, shared with the broader org. Useful for the next team. Sections:

- What we shipped.
- What worked (with examples).
- What we'd do differently (with examples).
- Surprises.
- Highest-leverage decisions.
- Advice for the next similar engagement.

## Common failure modes

- **No actions.** Retro produces observations only. Track shows nothing changes sprint over sprint.
- **Too many actions.** Twelve commitments. Zero shipped. Cut to two or three.
- **Same actions every retro.** "Communicate better." If a theme keeps recurring, the team isn't acting on it — investigate why.
- **Last retro's actions never reviewed.** First sign the retro practice itself is broken. Open every retro with the previous one's action review.
- **Same person facilitating forever.** Patterns calcify. Rotate.
- **Retro skipped because "we're busy."** The sprints where you most need the retro are the ones you're tempted to skip.
- **Sanitized retro because of psychological-safety gaps.** Real signal doesn't come out. Address the safety problem directly; the retro format won't fix it alone.
- **Retro that's a status meeting.** "What I worked on" reports. Wrong meeting. Push back to focus on process and learning.
- **Retro that's a complaint session.** All gripe, no action. Insist on the action commitment phase.
- **Outputs that never reach stakeholders who could help.** If the team needs an external decision (e.g., a client SLA on legal review turnaround), surface it; don't keep it inside the team's wishful list.

## Adapting for small / cross-functional teams

When the team mixes engineering, design, content, and occasional client SMEs, a few patterns help:

- **Surface cross-discipline friction explicitly.** Hand-offs between design and engineering, between content and brand, between team and client SMEs — these are common pain points and worth a dedicated prompt ("how did our cross-team hand-offs go this sprint?").
- **Disciplines have different rhythms.** A design-heavy sprint feels different from an engineering-heavy one. Don't expect every retro to surface the same kinds of issues.
- **External stakeholders matter.** If client SMEs were a bottleneck this sprint, that's a real retro item — but the action involves them, so coordinate.
- **Small teams = high signal, low n.** One person's frustration is statistically meaningful in a team of 5. Don't dismiss "only one person mentioned this."

## How this skill relates to others

- The sprint loop the retro feeds → `pm-sprint-planning`.
- Stories whose patterns surface in retros → `pm-user-story`.
- Action items often land in → `pm-raid-log`.
- Reporting on retro themes when they affect the project → `pm-weekly-status`.
- Stakeholder mechanics around retro outputs → `consulting-management-consultant`.
- Cleanup of retro memos for sharing → [[consulting-humanize]].

## Source material

- Derby, E. & Larsen, D. *Agile Retrospectives: Making Good Teams Great*. (The five-phase structure.)
- Kerth, N. *Project Retrospectives: A Handbook for Team Reviews*. (Origin of the Prime Directive.)
- Allspaw, J. *Blameless Postmortems and a Just Culture*. (For post-incident retros.)
- Google SRE Book: *Postmortem Culture*. https://sre.google/sre-book/postmortem-culture/
- Various Atlassian and Spotify guides on retro formats — useful for variety, mostly cosmetic on top of the core structure.
## Source frameworks

- **Reinertsen, *The Principles of Product Development Flow*** — variability principles (when to reduce, when to preserve), cadence design (F-principles). See [`../../references/book-product-development-flow.md`](../../references/book-product-development-flow.md).
- **Josephs & Rubenstein, *Risk Up Front*** — periodic risk re-review; the lessons-learned-as-risk-register pattern. See [`../../references/book-risk-up-front.md`](../../references/book-risk-up-front.md).
- **Gothelf, *Lean vs Agile vs Design Thinking*** — pivot/persevere as a retro outcome for Lean cycles. See [`../../references/book-lean-agile-design-thinking.md`](../../references/book-lean-agile-design-thinking.md).

