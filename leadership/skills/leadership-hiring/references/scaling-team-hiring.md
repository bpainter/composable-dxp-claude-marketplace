---
type: reference
plugin: leadership
skill: leadership-hiring
source: Grosse & Loftesness, *Scaling Teams: Strategies for Building Successful Teams and Organizations* (O'Reilly, 2017)
status: active
tags: [type/reference, plugin/leadership, topic/hiring, topic/team-scaling]
---

# Scaling team hiring (Grosse & Loftesness)

For when you're hiring a lot of people in a short window — practice growth, an engagement ramp, a transformation. Hyper-growth (>50% headcount in 6–12 months) breaks every system that worked at smaller scale: sourcing, interviewing, onboarding, and management.

Grosse & Loftesness organize the work into four phases — planning, recruiting, interviewing, onboarding — with characteristic failure modes at each.

## Phase 1 — Planning the scale

Most hiring crises begin here, not in the loop.

### The hiring plan

Before opening a single req, build the hiring plan:
- **What roles, by when?** Specific. Not "we need engineers" — "we need 2 senior engineers, 1 staff, 1 EM by end of Q2."
- **Who's the hiring manager for each?** Named. If you're the hiring manager for everything, you can't hire well.
- **What's the source mix?** Internal referral target, direct outreach target, recruiter target. Be honest about which channels actually work for senior roles in your space.
- **What's the interview capacity?** Each open req consumes ~5–10 hours of your team's interviewing time per week. Multiply across reqs. If your existing team is already at capacity, you cannot hire faster than a certain rate without breaking delivery.
- **What's the onboarding capacity?** Same calculation. Onboarding eats into senior engineer time. If you're hiring 5 people per quarter, who's onboarding them?

### Anti-patterns in planning

| Anti-pattern | Symptom | Fix |
|---|---|---|
| Hiring without a plan | Roles open ad-hoc as pain emerges | Quarterly hiring plan tied to growth and engagement pipeline |
| Hiring beyond interview capacity | Loops slow, debriefs slip, candidates lose interest | Cap concurrent open reqs at the team's interview throughput |
| Hiring beyond onboarding capacity | New hires bored or floundering in first 30 days | Enforce a max number of new hires onboarding at once |
| Hiring "everyone like the founder" | Team becomes a monoculture; gaps emerge | Profile-based hiring against actual needed skills, not pattern-match |

### The hiring rubric

Grosse & Loftesness recommend a written rubric per role family that interviewers calibrate against. Rubric entries are *behavioral* and *observable*:

- "Strong" on technical depth = "Walked through architectural trade-offs of two designs they actually shipped, named the failure mode of each, predicted the gotcha I was about to ask."
- "Weak" = "Could name technologies but couldn't reason about why a choice was made, or claimed credit for decisions made by others."

This rubric is what allows interviewers across the team to make consistent calls. Without it, "senior" means whatever the loudest interviewer thinks it means this week.

## Phase 2 — Recruiting (sourcing pipeline)

### Source diversification

In hyper-growth, no single channel scales. You need:
- **Internal referrals** (highest yield, but doesn't scale infinitely — your team's network exhausts)
- **Direct outreach** (recruiter-driven targeted search)
- **Brand-driven inbound** (your blog posts, talks, open-source, customer stories drive applicants over time)
- **Specialized channels** for diversity, specific skill sets, geos
- **Past candidates** (silver-medal pool — people who interviewed well but didn't land)

If you're at 80% from one channel, you have a brittle pipeline. One disruption (a key referrer leaves; the recruiter quits) and you stop hiring.

### The hiring funnel as an instrument

Track the full funnel weekly:
- Sourced → screened → interviewed → offer → accepted
- Conversion rate at each step
- Time spent at each step

When growth slows, the funnel tells you where:
- Sourced is low → outreach problem; pipe needs more capacity
- Screened-to-interviewed is low → JD or screen criteria too narrow, or recruiter calibration is off
- Interviewed-to-offer is low → loop quality or rubric is off
- Offer-to-accepted is low → close, comp, or competition issues

## Phase 3 — Interviewing at scale

The single biggest failure mode: as the team grows, interviewers proliferate, no one trains them, calibration drifts, and the bar drops by half a level per quarter.

### Interviewer training as a real investment

Grosse & Loftesness argue: every interviewer goes through a structured certification:
1. Read the rubric and the structured interview format
2. Shadow 2–3 interviews led by a calibrated interviewer
3. Reverse-shadow 2 interviews (lead with observer feedback)
4. Get debriefed against rubric calibration on at least one strong yes and one strong no

Cost: ~6 hours per interviewer over 3–4 weeks. Benefit: hiring decisions stay calibrated as the team grows.

### Calibration sessions

Quarterly: gather the interviewers, replay 2–3 anonymized recent decisions (one strong yes, one strong no, one disagreement). Discuss whether evidence cited actually justified the call. Recalibrate the rubric where decisions diverged from intent.

### Interviewer load

Cap each interviewer at a maximum of 4–5 interview slots per week. Beyond that:
- Quality drops (interviewer is tired, less prepared, less attentive)
- Delivery work suffers
- Burnout signals appear

Rotate the load. Don't burn your best interviewers (you know who they are).

## Phase 4 — Onboarding

Most teams stop thinking about hiring at offer-accept. The new hire shows up Monday and someone gives them a laptop. By month 3, you've lost 20% of your hires to "wasn't a good fit" and you blame the hiring loop.

### The 30/60/90 plan tied to the Performance Profile

Before the new hire's start date, the hiring manager writes the 30/60/90 plan:
- **First 30 days:** learning, relationship-building, low-risk wins. What specific things will they have *done* by day 30?
- **First 60 days:** small ownership, contributing to active work. What will they own by day 60?
- **First 90 days:** owning a meaningful piece of work tied to a Performance Profile bullet. What's the visible outcome by day 90?

See `T_30_60_90.md` in `70_Templates/`.

This is given to the new hire on day 1 (or before), discussed in their first 1:1, and revisited at the 30/60/90 marks. If the plan stalls, you have an early-warning signal.

### Onboarding buddy

Every new senior hire gets a peer buddy who's not their manager. The buddy is responsible for:
- Answering "stupid" questions the new hire is embarrassed to ask the manager
- Introducing them to the social fabric of the team
- Flagging early-warning signals to the manager

The buddy investment is ~3 hours per week for the first month, tapering. It dramatically reduces month-3 attrition.

### Manager 1:1 cadence in the first 90 days

- Week 1: daily 15-min check-ins
- Weeks 2–4: 30-min 3x/week
- Weeks 5–8: weekly 45-min standard 1:1
- Weeks 9–12: weekly, with explicit 30/60/90 review at the end of week 12

The frequency front-loads the relationship and surfaces problems while they're cheap to fix.

### Early-warning signals in the first 90 days

Watch for:
- New hire is quiet in meetings beyond week 4 (still feeling out the team or struggling?)
- Their first owned task slips beyond plan, twice
- Peers report "I'm not sure what they do all day"
- The new hire stops asking questions (often a worse sign than asking too many)
- They're not on calendar with people outside their immediate team by week 6

These don't always mean a bad hire — sometimes it's a slow ramp, a personality difference, or a team-fit gap. But they do mean a conversation. The hiring manager should be checking in deliberately at week 4 and week 8 with the new hire and with at least 2 peers.

### Course-correcting vs. exiting

If at week 8 you have a real concern:
1. Have the conversation directly with the new hire. Cite specifics.
2. Reset expectations and the 30/60/90 if needed.
3. Give it 4 more weeks.
4. At week 12, decide.

The early decision is harder than the late one but cheaper for everyone. A bad hire who lingers 6+ months damages the team and the person. A clean exit at month 3 is recoverable.

## Hyper-growth specifics

When you're growing >50% in 6 months:

- **Designate a recruiting tsar.** Someone whose primary job is the hiring system, not someone doing it on the side.
- **Weekly hiring sync.** All open reqs reviewed in 30 minutes. Funnel metrics, blockers, decisions.
- **Onboarding cohorts.** If you're hiring 4 people in a month, onboard them as a cohort with shared learning sessions. Reduces individual onboarding load.
- **Defer non-essential interviewer work.** If your senior engineers are interviewing 6 hours/week, accept lower delivery throughput — don't pretend you can have both.
- **Watch for senior-to-junior ratio drift.** Hyper-growth often means hiring more juniors because they're easier to source. Without seniors to mentor and review, junior throughput collapses. Maintain ratio explicitly.

## Reading

- Grosse & Loftesness, *Scaling Teams* — particularly the chapters on recruiting, interviewing, and onboarding
- Adler, *Hire With Your Head*, ch. 8 (implementation across an org)
