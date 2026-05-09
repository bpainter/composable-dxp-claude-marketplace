---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-persona-developer]]"
tags: [type/reference, plugin/cx, scope/skill, topic/personas, source/kalbach]
status: active
---
# JTBD-aligned personas

Drawn from Kalbach, *The Jobs To Be Done Playbook* (the "goal-driven personas" play) and Cooper's goal-directed persona work.

The traditional persona answers *who* — Sarah, 34, marketing manager, Brooklyn. The JTBD-aligned persona answers *who-doing-what-in-what-circumstance*. The shift makes personas useful for product decisions, not just for marketing positioning.

## What's wrong with demographic-first personas

The first decade of personas in industry produced a lot of stock photos and not much utility. The pattern:

- A persona names age, location, job title, hobbies, fictional spouse and pets.
- The team reads it once, files it, never refers to it again.
- Product debates continue from anecdote because the persona doesn't constrain anything.

What went wrong: demographics rarely predict the design decisions that matter. Two 34-year-old marketing managers in Brooklyn can have completely different jobs to be done, completely different decision contexts, and completely different reasons to choose your product.

## What JTBD-aligned personas do differently

A JTBD-aligned persona starts from the *job* and the *circumstances around it*, not from the person. Demographics enter only when they meaningfully shape behavior in those circumstances.

Three layers, in priority order:

1. **The job they're hiring solutions to do.** What are they trying to accomplish?
2. **The circumstances around that job.** Time pressure, who else is involved, what they've tried, what's at stake.
3. **The person doing it.** Role, context, behavior patterns. Demographics if and only if predictive.

## Format

```markdown
# [Persona name] — [Role + circumstance]

## Snapshot
- **Who they are**: [Role + life situation that frames the job. 1–2 sentences.]
- **Where we found them**: [Research source. Interview pool, segment, count.]

## Job they're hiring for
- **Main job**: [JTBD statement. When/I want to/So I can.]
- **Related jobs**: [2–3 adjacent jobs they'd like to bundle.]
- **Why now**: [Trigger event that put this job on their plate.]

## Circumstances
- **Time pressure**: [Tight, flexible, episodic.]
- **Who else is involved**: [Co-founders, family, team, manager, no one.]
- **What's at stake**: [Money, time, reputation, opportunity cost.]
- **What they've tried**: [Workarounds, alternatives, "doing nothing."]

## Behaviors
- **How they research**: [Where they look. Who they trust.]
- **How they decide**: [Speed, criteria, who gets vetoed.]
- **What stops them**: [The anxieties from the four forces.]

## Quotes
> "[Verbatim that anchors the persona]"
> — [Interview ID]

## Watch out for
[1–2 misconceptions the team will have.]

## Implications
- **Product**: [What this persona changes about feature priorities.]
- **Content**: [What language and framing they respond to.]
- **Channel**: [Where to reach them.]
- **Anti-pattern**: [What this persona tells us not to build.]
```

## Worked example

```markdown
# First-time founder, pre-formation, time-pressured

## Snapshot
- **Who they are**: Solo or co-founder pair who has just decided to incorporate.
  Has a customer lined up, a fundraising window, or both. Engineer or product
  background; not a finance or legal person.
- **Where we found them**: 12 interviews from Q1 2026 founder pool (accelerator
  contacts + Twitter recruit).

## Job they're hiring for
- **Main job**: When I'm a first-time founder about to incorporate, I want to
  make a confident decision between Delaware and another state, so I can move
  on to fundraising without legal anxiety.
- **Related jobs**: Split equity. File 83(b). Set up a bank account.
- **Why now**: Co-founder has agreed to commit; fundraising round opens in 6
  weeks; first paying customer is asking for an LLC name on the contract.

## Circumstances
- **Time pressure**: 2–4 weeks to make and execute the decision.
- **Who else is involved**: Co-founder (technical, similar profile), occasionally
  a more-experienced friend, sometimes an investor.
- **What's at stake**: Wrong choice = potential rework cost ($2–10K) and
  fundraising delay; right choice = invisible / no credit.
- **What they've tried**: Googled "Delaware C-corp"; read 1–2 Stripe Atlas /
  Clerky comparison posts; asked a friend.

## Behaviors
- **How they research**: Google and Reddit first. Will trust a Y Combinator
  resource or a known founder's blog post over a law firm's marketing.
- **How they decide**: Wants to be told the right answer with the reasoning,
  not given a list of options. Will cross-check with one trusted human.
- **What stops them**: Fear of hidden ongoing costs. Fear of a Delaware tax
  surprise. Fear that DIY filing will produce a problem they can't see.

## Quotes
> "I just want someone to tell me 'do this' and explain why for 30 seconds.
> Every site wants to walk me through 14 questions."
> — Interview 27

> "I have a customer who needs an EIN by next Friday. I don't have time to
> become an expert in entity types."
> — Interview 31

## Watch out for
- This persona looks like "we should sell to lawyers." They're not. They're
  the founder. The lawyer is downstream.
- They're not price-sensitive in absolute terms; they are anxiety-sensitive
  about hidden costs.

## Implications
- **Product**: A clear default recommendation with reasoning, not a 14-question
  flow. The flow can exist for the rare case where the default doesn't apply.
- **Content**: Plain-English; lead with money/time/risk; cite specific examples
  ("most YC companies form in DE because…"). No legal jargon front-of-stage.
- **Channel**: Twitter, Reddit, accelerator partner pages. Not law-firm marketing.
- **Anti-pattern**: Don't add a "talk to a lawyer" CTA at the top. They want
  to *avoid* the lawyer for this decision. Move the lawyer offer downstream
  (after they have a recommendation).
```

## How JTBD-aligned personas relate to traditional personas

| Element | Traditional | JTBD-aligned |
|---|---|---|
| **Anchor** | Demographics | Job + circumstance |
| **Use** | Marketing positioning | Product decisions, feature scoping, content tone |
| **Demographics** | Required, prominent | Optional, only if predictive |
| **Quotes** | Sometimes | Required |
| **Implications** | "Targets messaging at…" | "Build / don't build…" |
| **Stock photo** | Often | Never |

JTBD-aligned personas are the right format for product work. Traditional personas can still earn their keep for marketing-channel-and-message decisions, where demographics actually influence reach and resonance.

## When you need both

For an end-to-end engagement, you may need both:

- **Marketing persona** — segments by demographic and channel; informs media planning, ad targeting, lifecycle messaging.
- **Product persona (JTBD-aligned)** — segments by job and circumstance; informs feature priority, UX language, design constraints.

Don't merge. The two have different jobs.

## How many personas

Three to five primary, two to three secondary. More than that and the team won't remember them. If you have nine "user types," you have a tag system, not a persona set.

When in doubt: which personas would change the design decision? Those are the primaries. The rest are reference.

## Common failure modes

- **Demographics-first.** Falls back into the old pattern. Test: does the persona's age, location, or title appear before its job?
- **One persona per segment with the same job.** If two personas have the same job and circumstance, they're one persona with surface variation. Merge.
- **No anti-pattern in implications.** What this persona tells us *not* to build is often more useful than what it tells us to build.
- **No verbatim quotes.** A persona without quotes is a synthesis. Synthesis without source feels confident and isn't.
- **Persona in the wrong format for the audience.** Product team needs the JTBD format; brand team needs the marketing format. Producing one and using it for both fails both.

## Sources

- Kalbach, J. *The Jobs To Be Done Playbook*, ch. 4 (Defining Value). Two Waves / Rosenfeld, 2020.
- Cooper, A. *The Inmates Are Running the Asylum*. (Original goal-directed personas.)
- Goodwin, K. *Designing for the Digital Age*. (Persona craft from research.)
- Young, I. *Mental Models*. (Behavior-led audience modeling.)
