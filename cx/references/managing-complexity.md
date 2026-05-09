---
type: reference
project: skills-library
scope: plugin
plugin: cx
tags: [type/reference, plugin/cx, scope/plugin, topic/complexity]
status: active
---
# Managing complexity in services

Drawn from Norman, *Living with Complexity* (2010).

Norman's central distinction: **complexity is a property of the world**; **complicated is a property of how we experience it**. A modern service is necessarily complex — many actors, channels, dependencies, edge cases. The design job is not to remove complexity. It's to make complexity navigable.

This reference is short by design. Reach for it when a team is reflexively reaching for "let's simplify" as the answer.

## Why complexity is necessary

Real-world tasks are complex. A bank statement *should* show transaction type, date, amount, balance, category, and source — collapsing that into "you spent $X" hides the things people actually need. The right move is not to hide complexity but to *structure it so the user can navigate it*.

When stakeholders ask to "simplify the experience," translate the request: do they want fewer features, fewer steps, less information, or just better signposting? They are not the same problem.

## Six tools for managing complexity

### 1. Provide a conceptual model

Users build a mental model of how the system works. Good design supplies that model explicitly — through naming, layout, status indicators, and feedback. Bad design leaves the user to guess, and they often guess wrong.

Conceptual model checks:
- Can the user predict what will happen before clicking?
- After an action, does the system confirm what just happened in language matching the user's mental model?
- Are similar things grouped, dissimilar things separated?

### 2. Communicate (signifiers, signs, structure)

A signifier is anything that tells the user what's possible or what's expected — a button shape, a placeholder, a help text, an arrow, a queue rope. Service experiences are full of invisible signifiers (queue lines, agent uniforms, hold music). Audit them.

### 3. Make the wait seem appropriate

Norman's research on waits applies broadly to service design. Users tolerate waits when:
- They know why they're waiting.
- They know how long they'll wait.
- They feel the wait is fair (no one cutting the line).
- They have something to occupy them during the wait.
- The wait ends with a strong, clear close.

Loading states, status emails, queue position indicators, and "we'll get back to you within 24h" are all wait management.

### 4. Be fair

Users tolerate complexity and inconvenience when they perceive the system as fair. They do not when they perceive it as arbitrary, gameable, or rigged.

Common unfairness signals: hidden fees revealed late; "premium" customers getting visible advantages; opaque approval processes; agents who can't or won't explain why a decision went against you.

### 5. End strong, start strong

The peak-end rule. Users remember the most intense moment and the final moment of an experience disproportionately. Investment at start (first impression, first success) and end (resolution, summary, send-off) pays back more than investment in the middle.

This applies to onboarding (first-success moment), incident recovery (final apology and what's-different message), and service close (the bill, the goodbye, the thank-you).

### 6. Treat complexity as a partnership

Complex systems require something from the user: learning, attention, patience. The design job is not to do all the work for the user, but to make the user's part doable. Norman frames this as a partnership: the system meets the user halfway by being legible, predictable, and honest about what it requires.

This is the antidote to two common failure modes:
- **Over-simplification.** Hiding the complexity. The user can't act because they can't see what's happening.
- **Under-investment in onboarding.** Throwing the user into the system and expecting them to figure it out.

## How to use this in CX work

- **Diagnosing a stuck experience.** Map which of the six tools the experience is missing. Often one is doing all the work and the others are absent.
- **Service blueprinting.** Each backstage step has a frontstage signifier. Audit for invisible work.
- **Designing waits.** Waits are not failures of design; they're a design surface. Most service experiences mishandle them.
- **Pushing back on "let's simplify."** Translate to: which complexity is necessary, which is incidental, which is the user's job and which is ours.
