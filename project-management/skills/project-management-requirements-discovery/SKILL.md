---
name: project-management-requirements-discovery
description: Turn unstructured stakeholder input — meeting notes, Slack threads, exec emails, workshop output, ad-hoc asks — into structured, prioritized, traceable requirements with assumptions, constraints, and open questions explicit. The discovery layer that sits upstream of stories. Use this skill any time the user is consolidating asks into requirements, scoping a new initiative, untangling fuzzy stakeholder input, or saying "we need a requirements doc," "what are we actually building," "scope this" — even when the word "requirements" isn't used. Trigger whenever the conversation crosses from raw stakeholder input to "what is this project actually doing" so the team isn't building from a vibe.

# Project context
type: skill
project: skills-library
plugin: project-management
aliases: [project-management-requirements-discovery]
tags: [type/skill, plugin/project-management topic/project-management, topic/agile]
status: active
---

# Requirements Discovery

This skill is the upstream-of-stories layer. Stories (`pm-user-story`) capture *what to build*. Requirements capture *why we are building it, who needs it, what constraints we're under, and what good looks like*. Skipping this step produces stories that feel right and ship the wrong thing.

On most engagements, requirements arrive as a mix of client / partner asks, user research insights, design intent, and engineering reality. The job is to converge those sources into a shared definition before the team sprints on it.

Pair with `consulting-management-consultant` for the consulting posture this skill operates inside, with [[cx-customer-research]] (and the cx-* cluster) for the user evidence requirements rest on, with `strategy-digital-strategist` when requirements roll up into strategic intent, with `pm-user-story` for what comes after, and with `pm-raid-log` for assumptions and dependencies that fall out.

## When to use this skill

- A new initiative or epic is being scoped.
- Stakeholder input is scattered across messages, meetings, and slides.
- The team disagrees about what "this project" includes.
- A workshop produced a wall of stickies and someone needs to make it useful.
- A request like "we need to add X" needs to be turned into a real scope.
- An existing project's scope is being re-validated mid-flight.

## Core posture

- **Distill before you decide.** The first job is to capture inputs faithfully — what people said, what they meant, where they disagreed. Decisions come after.
- **Surface assumptions explicitly.** Every requirement rests on assumptions. If they aren't named, they will surprise the team later.
- **Trace every requirement to a need.** A requirement without a stated user or business need behind it is an opinion. If you can't trace it, push back.
- **Constrain before you commit.** Time, budget, technical, regulatory, brand — name the constraints before agreeing to scope.
- **Open questions are first-class.** A requirements doc that pretends nothing is unknown is a doc that will be re-litigated. Name the unknowns; name who can resolve them.
- **Prioritize, don't just list.** A flat list is a wishlist. Priority makes a list a plan.

## The requirements discovery process

Run these phases. They overlap in time, but each has a distinct output.

### Phase 1: Gather

Inputs come from many places:

- Stakeholder interviews (often via `cx-research` for users, ad-hoc for internal stakeholders).
- Workshop output, meeting notes, Slack threads.
- Existing artifacts: prior strategy docs, briefs, exec memos.
- Behavioral data and analytics.
- Existing system constraints (technology, vendor agreements, regulatory).
- Brand and content guardrails (project-specific brand reference).

**Output**: a catalog of raw inputs, each with source, date, and the person it came from. Don't synthesize yet. Capture is the job.

### Phase 2: Cluster

Group raw inputs into themes. Common cluster shapes:

- **By user need**: "Founders want clarity on incorporation" pulls together six interview quotes, a Slack thread, and a partner ask.
- **By system area**: "Formation flow" gathers all asks that touch the questionnaire.
- **By concern**: scope, schedule, budget, risk, opportunity.

**Output**: a clustered set of inputs, with each cluster named and the supporting raw inputs listed. This is the working set the rest of the process operates on.

### Phase 3: Synthesize

Convert clusters into structured requirements. For each cluster:

- **What is the underlying need?** Phrase as a user or business outcome.
- **What does success look like?** Observable, not "good UX."
- **What are the implicit assumptions?** Surface them.
- **What constraints apply?** Time, money, technology, brand, legal.
- **What are the open questions?** Name them; assign owners; set a resolve-by date.

**Output**: a draft requirements document.

### Phase 4: Pressure-test

Walk the document through:

- **The user**: would the people the requirement claims to serve recognize themselves in it? (See `cx-research`.)
- **The sponsor**: does the executive owner sign off on the framing?
- **The team**: can engineering, design, and content actually deliver this?
- **The constraints**: do the listed constraints actually hold? Have any been missed?
- **The dependencies**: who or what does this rely on outside the team?

Update the document with what pressure-testing revealed.

### Phase 5: Prioritize

Apply a prioritization frame. Common ones:

- **MoSCoW**: Must / Should / Could / Won't (this phase).
- **Importance × Urgency** matrix.
- **Cost of Delay / WSJF** (Weighted Shortest Job First) for portfolios.
- **RICE** (Reach × Impact × Confidence / Effort).

Pick one and stick with it for the project. Don't switch frames mid-flight.

**Output**: a prioritized requirements list with priority calls explained.

### Phase 6: Approve and baseline

The requirements doc gets approved by the project sponsor and the relevant lead(s) — design, engineering, content. Baseline it: this version is the agreed scope. Future changes are change requests, tracked separately.

**Output**: an approved, baselined requirements document. The downstream artifacts (epics, stories, RAID, sprint plan) trace back to this.

## The requirements document structure

```markdown
# Requirements: [Initiative / Project / Epic]

## Snapshot
- **Project**: [name]
- **Sponsor**: [exec / partner who owns this]
- **Lead**: [PM / lead delivering]
- **Time horizon**: [phase / quarter / specific dates]
- **Status**: Draft / In Review / Approved / Baselined / Superseded
- **Version**: [x.y]
- **Last updated**: [date]

## Context
[2-4 paragraphs. Why this initiative now. What the team is trying to achieve.
What problem is being solved. Who benefits if it succeeds.]

## Goals and success criteria
[3-5 goals. For each:
- The goal stated as an outcome.
- How we'll know it succeeded — observable measure.
- Time horizon for the measure.]

## Users and audiences
[Primary and secondary audiences. Reference personas (`cx-persona`) where they exist.]

## Requirements

[Each requirement gets its own block:]

### R-001: [Short imperative title]
- **Need**: [The user / business need this serves.]
- **Description**: [What is required. Specific.]
- **Success looks like**: [Observable outcome.]
- **Priority**: [Must / Should / Could / Won't]
- **Source**: [Where this requirement came from. Interview ID, partner ask, workshop output.]
- **Assumptions**: [What this requirement rests on.]
- **Open questions**: [Unknowns that must be resolved before delivery.]

### R-002: ...

## Constraints
- **Schedule**: [Hard dates, milestone constraints.]
- **Budget**: [Investment ceiling.]
- **Technology**: [Stack constraints, vendor lock-ins.]
- **Brand / content**: [Voice rules, legal-content guardrails.]
- **Regulatory / legal**: [Disclosures, jurisdictional requirements.]
- **Operational**: [What the team can and cannot do given current capacity.]

## Out of scope
[Explicit non-goals. The "we are not doing this in this initiative" list.
This list saves arguments later.]

## Dependencies
[Internal and external dependencies. Owners and target dates.
Cross-references to `pm-raid-log`.]

## Open questions
[Cross-cutting unknowns. Each with owner and resolve-by date.]

## Risks and assumptions
[High-level. Detail lives in `pm-raid-log`.]

## Approvals
- Sponsor: [name, date]
- Design lead: [name, date]
- Engineering lead: [name, date]
- Content / brand lead: [name, date]
```

## Requirements vs. user stories

Requirements describe *what is required*; user stories describe *what the team will build* (a smaller, story-shaped slice). The relationship:

```
Initiative (strategic intent)
  └── Requirements (what + why + constraints)
        └── Epics (large slices of work)
              └── User stories (sprint-sized work, see `pm-user-story`)
                    └── Tasks (engineering breakdowns)
```

A story should always trace back to a requirement, which traces back to a goal, which traces back to a user or business need.

## Disambiguating fuzzy input

Stakeholders rarely state requirements clearly. Patterns to watch for and unpack:

**Solution stated as a requirement.**
> "We need a chatbot."

Push back: what is the underlying user need? Maybe it's "founders need answers fast." A chatbot is one way; an FAQ, a search, or better content might serve the need at lower cost.

**Adjective stated as a requirement.**
> "It needs to be modern and seamless."

Push back: modern relative to what? Seamless across what touchpoints? Convert to observable outcomes.

**"Just like" comparison.**
> "We want it to feel like Stripe Atlas."

Useful starting point; not a requirement. Identify the specific qualities being referenced (clarity, plain language, founder-friendly tone) and capture those as requirements.

**Bundled need + solution + emotion.**
> "Founders are getting confused; we need a step-by-step wizard with a progress bar."

Three things in one sentence: the need (confusion), the proposed solution (wizard), the proposed UI (progress bar). Capture the need; treat solution and UI as proposals to evaluate, not requirements.

**Implicit "everyone."**
> "We need this for our users."

Which users? See `cx-persona`. Different segments have different needs; a requirement that serves everyone usually serves no one.

## How to handle conflicting input

Multiple stakeholders disagreeing is normal. The requirements doc surfaces the conflict explicitly:

```markdown
### R-007: Email capture on questionnaire

**Need**: [from research] Users return to finish a multi-step form later.
**Description**: Capture email at start of form to enable resume-by-link.

**Open question (CONFLICT)**: Whether to require email or make it optional.
- **Marketing position**: Required. Email is the primary lead.
- **Product team position**: Optional. Required field is friction; users bounce.
- **Design lead position**: Optional with a clear value-prop ("save your progress") to encourage opt-in.

**Resolution path**: Sponsor decision by [date]. Recommended: optional with prominent value prop (matches design lead position; testable in research).
```

The conflict gets named, the positions get named, and the resolution path is clear. Pretending stakeholders agree when they don't is the most common requirements failure.

## Common failure modes

- **Wishlist masquerading as requirements.** Long, flat, unprioritized list. No constraints. No assumptions. Reads like everything is essential.
- **Solution-first writing.** "The system shall have a button that..." That's a story (or a UI spec). The requirement is the underlying need.
- **Implicit assumptions.** "Founders will know what a SAFE is." Maybe. State it; if it's wrong, the requirement changes.
- **Missing source attribution.** Where did this requirement come from? If you can't tell six months later, the requirement is unverifiable.
- **No "out of scope" section.** Without explicit non-goals, scope creeps every meeting.
- **Adjective-driven success criteria.** "Modern, intuitive, fast." Define each. Or cut.
- **Requirements without owners or sign-off.** Who is accountable for the outcome? Who can change the requirement? If it's nobody, it's nothing.
- **Stale requirements doc never updated.** The doc was written at kickoff; the project changed; the doc didn't. Refresh on phase changes; baseline new versions.
- **Requirements docs longer than the build.** A 50-page doc nobody reads is worse than a 5-page doc the team can recite.

## For multi-stakeholder consulting engagements

A few patterns that recur across client work:

- **Two stakeholder camps.** Client side (sponsor, BD, marketing, content, SMEs, legal) and delivery side (Slalom team). Each has legitimate input. Capture both and resolve disagreements explicitly — don't paper over them.
- **Regulated-content requirements** flow through legal review. Any requirement touching legal, healthcare, financial, or other regulated content must respect the engagement's review process and turnaround time.
- **Early phases run lightweight.** Requirements at discovery stage are appropriately compressed — a one-pager per epic is often fine. Don't over-engineer the doc before the team can act on it.
- **Content-modeling decisions flow through requirements** when the build includes a CMS. Don't model content types without naming the requirement they serve. Pair with `dev-contentful-model`.
- **User needs are evidence-grounded.** Tie requirements to research from [[cx-customer-research]] whenever possible. "Users want X" without an interview ID behind it is a guess.

## Output formats

### Full requirements document

The structure above, in markdown. For Phase 1 work, often a single markdown file per epic; for larger initiatives, a directory with one file per area.

### Requirements summary (executive-facing)

A one-page summary used in steerco / partner reviews:

```markdown
# [Initiative] — Requirements Summary

## In one paragraph
[What we're doing, why, and by when.]

## Goals (3)
1. ...
2. ...
3. ...

## Top requirements (Must)
- R-001: ...
- R-002: ...
- R-003: ...

## Out of scope
- ...

## Constraints
- Schedule: ...
- Budget: ...
- Other: ...

## Open decisions needed
- [Decision 1, owner, by-date]
- [Decision 2, owner, by-date]

[Link to full requirements doc.]
```

### Requirements changelog

When the baselined doc changes (new requirement, scope cut, priority change), log it:

```markdown
## v1.3 — [date]
- Added R-014 (workflow automation for editorial team) — sponsor request, MoSCoW: Should.
- Cut R-009 (multi-language support) — out of scope for this phase; deferred.
- Re-prioritized R-006 from Should to Must — research signal.
```

The changelog is a trust artifact; without it, the team loses confidence the doc reflects reality.

## How this skill relates to others

- The consulting posture this skill operates inside → `consulting-management-consultant`.
- User evidence requirements rest on → [[cx-customer-research]], [[cx-persona-developer]], [[cx-jobs-to-be-done]], [[cx-journey-mapper]].
- Requirements rolling up into strategic intent → `strategy-digital-strategist`.
- Stories and acceptance criteria that come downstream → `pm-user-story`.
- Risks, assumptions, dependencies that fall out → `pm-raid-log`.
- Sprint planning that consumes the prioritized list → `pm-sprint-planning`.
- Status that reports against requirements progress → `pm-weekly-status`.
- Cleanup of requirements prose → [[consulting-humanize]].

## Source material

- IIBA *BABOK Guide* — the canonical business-analysis reference; pick what's useful, ignore the certification overhead.
- Wiegers, K. & Beatty, J. *Software Requirements*. Microsoft Press.
- Robertson, S. & Robertson, J. *Mastering the Requirements Process* (Volere).
- Patton, J. *User Story Mapping* (the bridge from requirements to stories).
- Klement, A. *When Coffee and Kale Compete* (JTBD framing for needs).
## Source frameworks

- **Perri, *Escaping the Build Trap*** — outcome statements; matching discovery to risk type; the Product Kata for surfacing requirements. See [`/80_Skills_and_Agents/product-management/references/book-escaping-the-build-trap.md`](../../../product-management/references/book-escaping-the-build-trap.md).
- **Gothelf, *Lean vs Agile vs Design Thinking*** — when requirements work is actually discovery (DT/Lean) and should be treated as such. See [`../../references/book-lean-agile-design-thinking.md`](../../references/book-lean-agile-design-thinking.md).
- **Block, *Flawless Consulting*** — 5-layer discovery interview (presenting → technical → personal → client-as-player → resistance). See [`/80_Skills_and_Agents/consulting/references/book-flawless-consulting.md`](../../../consulting/references/book-flawless-consulting.md).

