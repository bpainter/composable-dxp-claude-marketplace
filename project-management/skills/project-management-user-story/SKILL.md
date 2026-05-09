---
name: project-management-user-story
description: Turn loose feature requests, meeting notes, or stakeholder asks into well-formed agile user stories with Gherkin acceptance criteria, INVEST-checked scope, and INVEST-aware splitting when the story is too big. Use this skill any time the user is moving from problem statement to backlog item — "write a story for X," "we need a ticket for Y," "break this epic down," "draft acceptance criteria," "this card is too big" — even when they don't say the words "user story." Trigger whenever the conversation crosses from discovery to backlog so the team gets stories engineering can actually pick up.

# Project context
type: skill
project: skills-library
plugin: project-management
aliases: [project-management-user-story]
tags: [type/skill, plugin/project-management topic/project-management, topic/agile]
status: active
---

# User Story

This skill turns vague asks into stories engineering can execute on without a follow-up meeting. The deliverable is a Markdown ticket that can paste into Jira, Linear, Asana, or a Confluence/Notion backlog with no further translation.

## When to use this skill

- A stakeholder said "we need to be able to…" and you're about to write that down.
- A PRD or workshop output needs to be sliced into backlog items.
- A story has been written but engineering pushed back as too big or ambiguous.
- Acceptance criteria are missing or fuzzy.

If the user's request is just "explain what a user story is" — answer in chat, don't run this skill.

## Story format

Use this template every time. Variations on the format aren't worth the loss of consistency.

```markdown
# [Story title — short, outcome-oriented]

## Story
As a [persona],
I want to [capability],
So that [outcome / value].

## Context
[2-4 sentences: what problem this solves, what the user is doing today, why now. Linked design / research / decisions go here.]

## Acceptance criteria

**Scenario: [behavior name]**
Given [precondition]
And [precondition]
When [action]
Then [observable outcome]
And [observable outcome]

**Scenario: [next behavior]**
...

## Out of scope
- [explicit non-goal 1]
- [explicit non-goal 2]

## Dependencies
- [other story / system / decision this needs]

## Notes
- [design link, content link, technical guidance]
- [analytics: events to fire, properties to capture]
- [accessibility: anything beyond defaults]
```

### Title rules

- Outcome-oriented, not task-oriented. **"Founder can save and resume the formation questionnaire"** beats "Add localStorage to questionnaire."
- 60 characters or fewer.
- Verb up front when natural.

### "As a / I want / So that"

The three lines exist for three reasons. If any line is missing or weak, the story is weak.

- **Persona** — who specifically. Avoid "user." Use the actual persona ("enterprise marketing buyer," "content editor," "platform admin").
- **Capability** — what the system newly enables. Verb + object. Don't describe implementation.
- **Outcome / value** — *why this matters*. If you can't fill this in convincingly, the story may not be worth doing. Push back on the requester.

Bad:
> As a user, I want a button, so that I can click it.

Better:
> As an early-stage founder, I want to save my questionnaire progress, so that I can finish it later without re-entering everything.

### Gherkin acceptance criteria

Use **Given / When / Then**. Each scenario is one observable behavior.

- **Given**: state of the world before the action.
- **When**: the action the persona takes.
- **Then**: an observable, testable outcome.

Rules:
- One scenario per behavior. If you find yourself stringing 6 `And When`s together, split into multiple scenarios.
- Observable means: visible in the UI, in the DB, in an API response, in analytics, in a sent email — something you can write a test against.
- Avoid "user-friendly" or "fast" in `Then`. Spell it out: "loads in under 2 seconds on a 3G connection," "uses the primary brand color from the design system."
- Cover the happy path **and** the obvious unhappy paths (validation errors, network failures, empty states, permission denials). Stories with only happy-path criteria are stories with hidden bugs.

Example for the questionnaire save story:

```
**Scenario: Save and resume on the same browser**
Given the founder has answered 3 of 8 questions
When the founder closes the tab and returns within 30 days
Then the questionnaire opens to question 4
And questions 1-3 retain the previously entered answers

**Scenario: Save and resume across devices via email link**
Given the founder has answered 3 of 8 questions
And the founder has provided a valid email
When the founder clicks the resume link sent to that email
Then the questionnaire opens to question 4 with previous answers retained

**Scenario: Resume after data has expired**
Given the founder's saved progress is older than 30 days
When the founder returns
Then the questionnaire opens to question 1 with no prior answers
And a message explains that saved progress has expired
```

## INVEST checklist

Before handing off a story, run this:

- **I**ndependent — can be built without waiting on another in-progress story (or dependencies are explicit).
- **N**egotiable — describes the *what*, not the *how*. Implementation choices are not in the story.
- **V**aluable — has a real outcome line. If "So that…" is hand-wavy, the story is weak.
- **E**stimable — engineering can sketch a size without further discovery.
- **S**mall — fits in a sprint. If it's bigger than ~5 days for one engineer, split it.
- **T**estable — every acceptance criterion is observable.

## Splitting big stories

When a story is too big, split along one of these axes (in preference order):

1. **By workflow step.** Save flow → Resume flow → Email-link flow.
2. **By data variation.** Single-founder questionnaire → multi-founder.
3. **By interface.** Server-side first, polish UI second (only acceptable if the slice still delivers value).
4. **By rule complexity.** Happy path now, edge cases (expired data, conflicting answers) next.
5. **By non-functional quality.** Functional behavior now, performance/accessibility hardening as a follow-up — but only when justified.

What **not** to do:
- Split horizontally by layer ("backend story" / "frontend story"). Neither slice delivers value alone. This is the most common splitting anti-pattern.
- Split by "phase 1 / phase 2" without defining the phases.
- Split a 13-pointer into "13-pointer story 1" and "13-pointer story 2." That's renaming, not splitting.

## Acceptance criteria patterns to reach for

**Validation**: explicit field-level rules. Required, format, length, allowed values.

**Empty state**: what the user sees before any data exists.

**Error state**: what happens on network failure, validation failure, permission denial.

**Permissions**: who can do this, who can't, what unauthorized access looks like.

**Analytics**: what events fire and with what properties (so the team can measure success).

**Accessibility**: keyboard nav, screen reader, contrast — only spell out if non-default behavior is needed.

**Browsers / devices**: only if scope is non-default.

## Project conventions to lock down up front

Set these before the team starts cutting stories. Without them, every PM does it differently.

- **Persona library.** A short, agreed list of personas (3–5) that appear in the "As a…" line. Avoid "user."
- **Regulated-content notes.** Any story that ships legally / medically / financially sensitive content must reference the project's review process in Notes.
- **SEO-relevant stories** should reference `strategy-seo-brief` for required fields (title, meta, slug).
- **Analytics**: include event name, properties, and trigger in acceptance criteria. The team will not retrofit instrumentation.
- **Accessibility default**: WCAG 2.2 AA. Spell out exceptions only.

## Common failure modes

- Story written from the developer's POV ("the system shall…"). Rewrite from the persona's.
- Acceptance criteria that are restatements of the title ("Then the user can save"). Push for observable detail.
- "Nice-to-have" buried in acceptance criteria. Either it's required or it's a separate story.
- Out-of-scope section missing. Without it, scope creep is inevitable.
- Story writes itself ("As a user I want fast pages so that I have a good experience"). That's not a story; it's a mood.

## How this skill relates to others

- Upstream of stories: requirements gathering and discovery synthesis (forthcoming `pm-requirements-discovery` skill).
- Story-set health and progress: weekly status (`pm-weekly-status`), RAID log (`pm-raid-log`).
- Estimation, sprint shape, retros: forthcoming `pm-sprint-planning` and `pm-retro-facilitator`.
## Source frameworks

- **Perri, *Escaping the Build Trap*** — outcome-shaped stories vs. feature-shaped stories; the Product Kata for story refinement. See [`/80_Skills_and_Agents/product-management/references/book-escaping-the-build-trap.md`](../../../product-management/references/book-escaping-the-build-trap.md).
- **Reinertsen, *The Principles of Product Development Flow*** — batch size at the story level; the U-curve of story sizing. See [`../../references/book-product-development-flow.md`](../../references/book-product-development-flow.md).

