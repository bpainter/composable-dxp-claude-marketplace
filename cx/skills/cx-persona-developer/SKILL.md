---
name: cx-persona-developer
description: Build research-grounded personas — goals, frustrations, behaviors, decision contexts, and the verbatim quotes that anchor each one — usable as inputs to product, design, content, and marketing decisions. Not generic marketing personas. Use this skill any time the user is creating, refining, evaluating, or applying personas — "make a persona," "who is our user," "audience profile," "ICP," "we need a persona for X" — even when the word "persona" isn't used. Trigger whenever the team needs a shared mental model of the audience so design, copy, and feature decisions stop being argued from anecdote.

# Project context
type: skill
project: skills-library
plugin: cx
aliases: [cx-persona-developer]
tags: [type/skill, plugin/cx topic/customer-experience, topic/ux-research, topic/service-design]
status: active
---

# Persona

A persona is a research artifact, not a marketing flourish. It exists to settle arguments before they happen: when the team disagrees about who the user is, the persona is the document everyone returns to. A persona made up at a whiteboard does not do that work.

Pair with [[cx-customer-research]] (the evidence personas come from), [[cx-jobs-to-be-done]] (a complementary lens; personas are who, JTBD is why), [[cx-journey-mapper]] (where personas live in the experience), and `cx-behavioral-design` (how personas behave in specific decision contexts).

## When to use this skill

- Building personas from research (interview synthesis, survey segmentation, behavioral data).
- Refining stale or generic personas into something the team can actually use.
- Evaluating someone else's personas for evidence-fit, accuracy, or usefulness.
- Applying personas to a specific product, design, or content decision.
- Onboarding new team members to the audience the project serves.

If the user wants a "marketing persona" with stock photos and a fictional last name and a fake car, push back. That format is the most common reason personas get ignored.

## Core posture

- **Evidence first.** Every persona claim ties to evidence: an interview quote, a survey result, a behavioral signal. If you cannot defend a claim, drop it.
- **Specificity over averages.** A persona is a *typified* user, not a statistical mean. The mean is rarely useful; the type is.
- **Behavior, not demographics.** Age, income, and job title rarely predict the decisions you're designing for. What people actually do, what they care about, and what stops them does.
- **Quote the user.** Verbatims anchor a persona more than any synthesis sentence. Use them.
- **Limit count.** Three to five personas is usually right. Ten is "we did not segment, we listed."
- **Ruthlessly cut.** The persona document should be short enough that the team actually reads it. Twenty-page personas die in a folder.

## When personas are right vs. when JTBD is right

Personas describe *who* the user is. Jobs-to-be-Done describes *what they're trying to get done*. Both are useful; they answer different questions.

Use a persona when:
- The team disagrees about audience.
- You are designing for behavior that varies by user type (founders vs. investors).
- Marketing or sales positioning is the deliverable.

Use JTBD when:
- The team disagrees about feature priority.
- Different users do the same job in the same situation.
- Product decisions need a goal, not a personality.

For most product work on this project, you'll want both. See `cx-jobs-to-be-done`.

## Persona structure

A persona is one page (markdown) or one slide. If it doesn't fit, it's not a persona; it's a research summary.

```markdown
# [Persona name] — [One-line role/identity]

## Snapshot
- **Who they are**: [1-2 sentences. Specific. Not "young professionals."]
- **Where we found them**: [research source — interviews from X, survey of Y, etc.]

## Goals
[2-4 goals, framed as outcomes the user is trying to achieve. Not "use our product."]

## Frustrations / barriers
[2-4 specific frustrations. Tie each to an interview signal or quote.]

## Behaviors
[How they actually act in the relevant context. Where they look for information. What they ignore. How they decide.]

## Decision context
[What does the buying / using / acting decision look like? Who else is involved? What information do they need? What is the time pressure?]

## Quotes
> "[Verbatim quote that anchors the persona]"
> — [Interview ID or research source]

> "[Second verbatim quote]"
> — [Source]

## Watch out for
[1-2 things the team will get wrong about this persona if not careful. Misconceptions to head off.]

## Implications
[2-4 specific implications for product / design / content / messaging. Each one names what to do and what not to do.]
```

That's it. No stock photo. No fictional last name. No fake spouse and pets. The discipline is what makes the persona useful.

## How to build a persona from research

```
1. Sample selection. Pick the research segment you're typifying.
   Differentiate by behavior and circumstance, not by demographic
   alone (e.g., "first-time enterprise buyer with active vendor
   evaluation" vs. "renewing buyer in expansion phase").

2. Read transcripts. All of them. Once for shape, once for coding.

3. Code by behavior, goal, and frustration. Not by demographic.
   Watch for patterns that repeat across multiple interviews.

4. Identify the typified user. Not the mean — the most representative
   pattern. The type that, if you designed for them, would also serve
   the variations.

5. Draft the persona using the structure above. Anchor every claim
   to evidence.

6. Pull verbatim quotes that capture the persona's voice. Two or
   three is enough.

7. Pressure-test with the research team. Anyone who reads the
   transcripts should agree this is a fair typification.

8. Review with the broader team. Test usefulness: can the team make
   a design / content / feature decision faster with this persona
   than without it?

9. Iterate. Personas drift. Re-validate every 6-12 months or after
   major audience shifts.
```

## What makes a persona useful

A useful persona helps the team make decisions. Concrete tests:

- **Decision test**: Pick a real decision the team is facing. Does the persona narrow the choice? If not, the persona is too generic.
- **Disagreement test**: Two team members read the persona and a brief. Do they agree on what to build? If not, the persona has gaps or is inconsistent.
- **Quote test**: Read the persona aloud. Does it sound like a real human you could imagine meeting? If it sounds like a marketing brochure, rewrite.
- **Cut test**: What is the smallest version of the persona that still moves decisions? Cut to that.

## Common failure modes

- **Demographic-first personas.** "Sarah is a 34-year-old marketing manager from Brooklyn." The age, location, and title rarely matter. The behavior does.
- **Vibes-only personas.** Built from team intuition, no research. Read as confident; collapse under contact with real users.
- **Too many personas.** Twelve "user types" is a tag system, not a persona set. Limit to three to five primary, two to three secondary at most.
- **Personas that all want the same thing.** If three personas have the same goals and frustrations, they're one persona with surface variation.
- **Outdated personas.** Audiences shift. A persona built two years ago may no longer reflect the user.
- **Personas that contradict the journey.** If the journey map shows users skipping a step, but the persona says they value it deeply, one of the artifacts is wrong.
- **Marketing-flavored personas mistaken for product personas.** Marketing personas focus on positioning and channels. Product personas focus on goals, decisions, and behavior. Don't conflate.

## Output format

Markdown one-pager (above) or PowerPoint single slide. Both formats use the same structure. For a slide:
- Top half: snapshot, goals, frustrations.
- Bottom half: behaviors, decision context, quotes.
- Right column or footer: implications.

Run the persona text through [[consulting-humanize]] before publication.

## How this skill relates to others

- The research that personas come from → [[cx-customer-research]].
- Complementary lens (what users are trying to do, regardless of identity) → [[cx-jobs-to-be-done]].
- Where personas live in the experience → [[cx-journey-mapper]].
- How personas behave in decision contexts → `cx-behavioral-design`.
- Personas applied to design → `brand-design-process`, `brand-design-screen`.
- Personas applied to content → `strategy-seo-brief`, `strategy-geo-optimization`.
- Persona text cleanup → [[consulting-humanize]].

## References

- [[jtbd-aligned-personas]] — JTBD-aligned persona format that anchors on job + circumstance instead of demographics. The format that makes personas usable for product decisions.

## Source material

- Cooper, A. *The Inmates Are Running the Asylum*. (Original goal-directed personas.)
- Pruitt, J., & Adlin, T. *The Persona Lifecycle*.
- Young, I. *Mental Models*. (Behavior-led audience modeling, more rigorous than typical personas.)
- Goodwin, K. *Designing for the Digital Age*. (Persona craft from research.)
- Kalbach, J. *The Jobs To Be Done Playbook*. (See [[jtbd-aligned-personas]].)
