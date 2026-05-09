---
name: project-management-sprint-planning
description: Plan sprints that the team can actually deliver — sprint goal, capacity calculation, story selection, estimation, dependencies, and the working agreements that hold the plan together. Covers Scrum, Kanban, and dual-track variants. Use this skill any time the user is planning or replanning a sprint, sizing capacity, picking what to commit to, refining stories before sprint start, or asking "can we get this done in two weeks" — even when "sprint planning" isn't said. Trigger whenever the team is about to commit to a chunk of work so the commitment matches reality, not optimism.

# Project context
type: skill
project: skills-library
plugin: project-management
aliases: [project-management-sprint-planning]
tags: [type/skill, plugin/project-management topic/project-management, topic/agile]
status: active
---

# Sprint Planning

This skill produces sprint plans that hold up. The most common failure mode in sprint planning is over-commitment, and the cost shows up as missed sprint goals, demoralized teams, and the slow corruption of velocity as a planning input. The cure is a structured plan: a clear goal, an honest capacity calculation, a real-talk estimation pass, and explicit dependencies.

For small cross-functional teams (engineering + design + content + occasional client SME), sprint planning has to coordinate work that isn't all "engineering tickets" — design phase gates and content reviews live in the same plan.

Pair with `pm-user-story` for the stories the sprint pulls from, with `pm-requirements-discovery` for the upstream priority that drives selection, with `pm-raid-log` for dependencies that affect commitment, with `pm-weekly-status` for reporting out, with `pm-retro-facilitator` for closing the learning loop, and with `consulting-management-consultant` for stakeholder mechanics around the plan.

## When to use this skill

- Planning the upcoming sprint.
- Re-planning mid-sprint when something material shifted (scope, capacity, blocker).
- Setting up the team's sprint cadence at project kickoff.
- Coaching a team through "we keep missing our commitments."
- Deciding whether to use Scrum, Kanban, or a hybrid.

## Core posture

- **Capacity is a fact, not an aspiration.** Hours / points available based on real availability, not last sprint's velocity in a fantasy week.
- **Sprint goal first.** A sprint with a goal is a sprint the team can defend. A sprint with only a list of tickets isn't a sprint; it's a queue.
- **Stories before commitment.** Stories must be ready (refined, sized, with acceptance criteria) before they enter the sprint. See `pm-user-story` for the readiness bar.
- **Dependencies kill commitment.** External dependencies — client SME review, content sign-off, third-party API access — get named explicitly and either resolved before sprint start or flagged as risks.
- **Honest estimation.** Estimates are forecasts, not contracts. Inflate to be safe; deflate to look fast — both lead to the same place.
- **Commit only to what the team owns end-to-end.** Stories that depend on someone else's output should be conditional, not committed.

## The Scrum / Kanban / dual-track choice

Different shapes work for different teams.

**Scrum** — fixed sprints (typically 2 weeks), planning at the start, review and retro at the end, team commits to a sprint goal.
- Best for: predictable cadence, stable team, clear roadmap.
- Default for build phases of most engagements.

**Kanban** — continuous flow, no sprints, WIP limits per column, pull-based.
- Best for: high variability, unpredictable inputs, support-heavy teams.
- Useful for the support / iteration mode after launch; rarely the right mode for active build.

**Dual-track Scrum** — discovery and delivery run in parallel tracks. Discovery feeds the delivery backlog; delivery executes against ready stories.
- Best for: teams that need to keep validating while shipping.
- Appropriate when research and design are running ahead of (and feeding) engineering.

**Hybrid (Scrumban)** — sprint cadence for ceremonies and reporting, but with WIP limits and pull-based flow inside.
- Best for: small teams that want the rhythm without the rigidity.

Default for typical consulting engagements: **Scrum (2-week sprints) for build phases, with a dual-track flavor when discovery is active**. Move to Kanban after launch if the team transitions into ongoing iteration.

## The sprint planning meeting

Standard agenda for a 2-week sprint planning session, ~90 minutes for a small team:

```
1. Sprint goal proposal           (10 min)
2. Capacity check                  (10 min)
3. Story walk-through              (40 min)
4. Estimation and commitment       (20 min)
5. Dependencies and risks          (10 min)
```

### Step 1: Sprint goal

A sprint goal is **one sentence** describing what the team will achieve. Not a list of tickets. The goal should be:

- Specific enough that mid-sprint, the team can ask "is this work serving the goal?"
- Singular — one focus, not three.
- Outcome-shaped, not output-shaped.

Examples:

> ✅ "Ship the configurator end-to-end behind a feature flag, demoable to client stakeholders by sprint end."
> ❌ "Complete the configurator stories." (Output, not outcome.)
> ❌ "Make progress on multiple workstreams." (Not focused.)

The goal lets the team make trade-offs mid-sprint. If a story slips, but the goal is intact, the sprint is succeeding.

### Step 2: Capacity check

Walk through real availability. For each team member:

```
| Person | Sprint days available | Notes |
|---|---|---|
| Eng A | 8 (out 2 days) | Out Mon-Tue week 2 |
| Eng B | 10 | Full sprint |
| Design | 6 | Half-time on this project |
| PM | 10 | Full sprint |
```

Calculate team capacity in hours, points, or days — pick one unit and stay with it.

**Adjust for overhead.** Real team capacity is rarely 100% of nominal. Account for:
- Standups, ceremonies, planning, retros (~10-15%).
- Code review and PR work (~10-15%).
- Bug fixes and unplanned support (~10-20%, varies).
- Slack, email, ad-hoc (~5-10%).

Effective capacity for delivery work is often **50-65% of nominal**. A team with 40 nominal hours/week per person ships maybe 25 hours of planned story work. Plan against the effective number.

### Step 3: Story walk-through

For each story being considered:

- Read the story aloud (As a / I want / So that).
- Read the acceptance criteria.
- Confirm the team understands what "done" means.
- Note any open questions; if unanswered, the story isn't ready.

A story that's not ready doesn't enter the sprint. Refine it before next sprint, or pull from the ready set.

### Step 4: Estimation and commitment

Estimation method options:

- **Story points** (Fibonacci: 1, 2, 3, 5, 8, 13). Relative sizing. Team-specific velocity.
- **T-shirt sizes** (XS, S, M, L, XL). Lower precision, faster.
- **Ideal days / hours**. Absolute. Useful for fixed-bid projects but easy to mis-translate to calendar time.
- **No estimate (#NoEstimates)** — slice everything to small (1-2 day) work units, count throughput. Works only when slicing discipline is real.

For most consulting engagements, **story points (Fibonacci)** is the default. The team builds intuition over a few sprints; velocity becomes a useful planning input.

Commitment rule:

```
Selected work ≤ effective capacity × confidence factor
```

Confidence factor: 0.8 for a stable team after several sprints; 0.6-0.7 for a new team; 0.5 for a sprint with major dependencies. **Never commit at 100%.** Sprints have surprises.

If the team wants to add stretch work — fine. Stretch is not committed. The team picks it up if the committed work finishes early.

### Step 5: Dependencies and risks

For each committed story, name external dependencies:

- Content writeups awaiting client SME or legal review.
- Design awaiting design-lead approval.
- Engineering blocked on third-party API or env access.
- Stakeholder decisions needed mid-sprint.

Each dependency gets:

- **Owner** (often outside the team).
- **Needed-by date** (mid-sprint or end-of-sprint).
- **Status** (Confirmed / Pending / At risk).
- **Backup plan** if the dependency slips.

Dependencies migrate into `pm-raid-log` if they're not resolved by sprint start.

## Working agreements

Set these once at project kickoff; refer back when they break:

- **Sprint length**: 2 weeks (default for most consulting engagements).
- **Sprint cadence**: Planning Monday morning, mid-sprint check Wednesday week 2, demo Friday week 2, retro after demo.
- **Definition of Ready** (story can enter sprint): see `pm-user-story` INVEST checklist.
- **Definition of Done** (story can be marked complete): see below.
- **WIP limits**: at most one story in progress per engineer; at most two in review at any time.
- **Scope changes**: anything new added mid-sprint requires removing equivalent work; sponsor sign-off if the change exceeds 20% of capacity.
- **Demos**: every sprint ends with a demo to stakeholders; if a story can't be demoed, it isn't done.

### Definition of Done

A story is done when:

- [ ] Acceptance criteria all pass.
- [ ] Code is merged to main and deployed to preview.
- [ ] Tests are added or updated (per `dev-quality-engineer`).
- [ ] Accessibility check passed for any UI work (per `dev-a11y-audit`).
- [ ] Documentation updated where needed.
- [ ] Story is demoed to the product owner / sponsor.
- [ ] Any follow-up tickets are filed (not just verbalized).

This is a default; the team can extend it with project-specific items. Whatever's in DoD is binding.

## Sprint plan output

```markdown
# Sprint [N] Plan: [Sprint name / dates]

## Sprint goal
[One sentence. Outcome-shaped. Singular focus.]

## Dates
- Start: [Mon date]
- End: [Fri date 2 weeks later]
- Demo: [Fri date]
- Retro: [date]

## Capacity
- Team capacity: [X effective points / hours / days]
- Confidence factor: [0.X]
- Committed capacity: [Y]

## Committed work
| ID | Story | Owner | Estimate | Dependencies | Notes |
|---|---|---|---|---|---|
| PRJ-201 | Configurator — Step 1 | [Eng A] | 5 | None | |
| PRJ-202 | Configurator — Step 2 | [Eng B] | 8 | Client SME review needed by Wed week 2 | |
| PRJ-205 | Hero block: Contentful wiring | [Eng A + Design] | 5 | Hero design final | |

**Total committed**: [Z points]

## Stretch (if committed work finishes)
| ID | Story | Owner | Estimate | Notes |
|---|---|---|---|---|
| PRJ-208 | Glossary term Contentful migration | TBD | 3 | |

## Dependencies
| Item | Owner | Needed by | Status |
|---|---|---|---|
| SME review of configurator copy | [Client SME / counsel] | Wed wk 2 | Pending |
| Hero design final | [Design lead] | EOD Mon | Confirmed |

## Risks
[Linked to `pm-raid-log` items relevant to this sprint.]

## Out of scope this sprint
- [Item 1]
- [Item 2]

## Working notes
[Anything else the team needs to remember mid-sprint.]
```

## Mid-sprint check

A short standup at the midpoint checks:

- Are we on track for the sprint goal?
- Are any dependencies slipping?
- Has anything new come in that wants to be added (and what comes out if so)?
- Are any stories blocked?

A mid-sprint that's already off track usually means: replan now, don't wait for retro.

## Velocity and using it well

Velocity = points completed per sprint, averaged over the last 3-5 sprints.

Useful uses:
- Forecasting how much the team will likely complete next sprint.
- Roadmapping ("at this velocity, when does the milestone hit?").

Misuses (avoid):
- Comparing velocity across teams. Different teams, different scales.
- Using velocity as a productivity metric. Encourages estimate inflation; corrupts the data.
- Treating velocity as a target. Goodhart's Law.
- Promising velocity-derived dates externally with no buffer.

In the first quarter of any new engagement, velocity is unstable until ~3 sprints in. Plan with broad ranges and acknowledge the uncertainty in `pm-weekly-status`.

## Common failure modes

- **Sprint goal that's a list of stories.** Defeats the purpose. Pick one outcome.
- **Capacity calculated as 100%.** Sprint always misses. Use effective capacity.
- **Stories not ready.** Stories enter the sprint with open questions; mid-sprint, the team is debating instead of building. Refine before sprint start.
- **External dependencies treated as committed work.** "We'll get the legal sign-off Tuesday" — no, that's a risk. Plan for the dependency slipping.
- **No retro, or retros that don't change anything.** Sprint plans get worse over time without retros that improve them. See `pm-retro-facilitator`.
- **Velocity-driven scope gymnastics.** Padding estimates to "make velocity," or trimming to "show improvement." Either corrupts the planning input.
- **Mid-sprint scope creep without a swap.** Adding work without removing equivalent work. Sprint goal slips; team morale slips faster.
- **Unclear definition of done.** Story marked complete but missing tests / docs / demo. Bites in the next sprint or two.
- **Planning without the people doing the work.** Estimates from leadership without the team aren't estimates; they're wishes.

## Adapting for small / cross-functional teams

When the team includes design, content, and engineering on the same sprint board, a few adjustments help:

- **Mixed-discipline stories**: a "ship the configurator flow" story may have design, copy, and engineering tasks. Keep the story whole; track the discipline contributions inside.
- **Disciplines have different cadences**: design may want phase-gated reviews mid-sprint. Build them into the plan; don't pretend everything is engineering.
- **Capacity per discipline**: a sprint can be engineering-rich and design-lean (or vice versa); plan accordingly.
- **Demos cover the whole stack**: the demo isn't just engineering. Design walks the rationale; content shows the published output; engineering shows the tech.

## How this skill relates to others

- Stories that fill the sprint → `pm-user-story`.
- Priority order driving story selection → `pm-requirements-discovery`.
- Dependencies and risks tracked at project level → `pm-raid-log`.
- Reporting out at sprint and week boundaries → `pm-weekly-status`.
- Closing the learning loop after the sprint → `pm-retro-facilitator`.
- Stakeholder framing and scope conversations → `consulting-management-consultant`.
- Quality gates the sprint commitment depends on → `dev-quality-engineer`.

## Source material

- Sutherland, J. *Scrum: The Art of Doing Twice the Work in Half the Time*.
- Cohn, M. *Agile Estimating and Planning*.
- Anderson, D. *Kanban: Successful Evolutionary Change for Your Technology Business*.
- Larman, C. & Vodde, B. *Large-Scale Scrum*.
- Patton, J. *User Story Mapping* (the bridge from backlog to sprint planning).
- Forsgren, N., Humble, J., Kim, G. *Accelerate*. (For when planning practices need defending against "but we ship faster without all this.")
## Source frameworks

- **Reinertsen, *The Principles of Product Development Flow*** — Little's Law (cycle time = WIP ÷ throughput), batch-size principles, WIP constraints, the exponential utilization curve, CD3 for sprint priority. See [`../../references/book-product-development-flow.md`](../../references/book-product-development-flow.md).
- **Gothelf, *Lean vs Agile vs Design Thinking*** — when sprints are appropriate (delivery work) vs. when they hide discovery work in delivery cadence. See [`../../references/book-lean-agile-design-thinking.md`](../../references/book-lean-agile-design-thinking.md).
- **Perri, *Escaping the Build Trap*** — outcome-shaped sprint goals beat feature-shaped sprint goals. See [`/80_Skills_and_Agents/product-management/references/book-escaping-the-build-trap.md`](../../../product-management/references/book-escaping-the-build-trap.md).

## Templates this skill uses

- [`../../references/template-flow-metrics.md`](../../references/template-flow-metrics.md) — Cycle time, WIP, CD3 for sprint capacity

