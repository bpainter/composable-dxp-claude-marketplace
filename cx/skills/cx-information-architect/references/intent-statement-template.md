---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-information-architect]]"
tags: [type/reference, plugin/cx, scope/skill, topic/information-architecture, topic/template]
status: active
---
# Intent statement template

Drawn from Covert's *How to Make Sense of Any Mess* (step 2). Use at the start of any IA engagement, before content modeling begins.

The intent statement is the single most useful artifact for keeping IA work scoped. It's also the most-skipped step.

## Format

```
For [stakeholder]
[verb] [object] [outcome / quality]
[so that] [downstream business or user impact].
```

The five parts:

| Part | What it does | Common failure |
|---|---|---|
| **For [stakeholder]** | Names whose experience this is for | "For everyone" — useless |
| **[Verb]** | The active change | Passive verbs ("understand," "be aware of") that can't be measured |
| **[Object]** | The thing being changed | Vague nouns ("experience," "engagement") that hide the real object |
| **[Outcome / quality]** | The standard the change is held to | "Good," "better" — unmeasurable |
| **[So that]** | The downstream impact that justifies the work | Missing — strands the IA work without business case |

## Worked examples

### Composable DXP marketing site

```
For prospects evaluating composable DXP platforms,
make pricing, capability, and support information findable in two clicks
so that we stop losing late-stage deals to competitors with clearer sites.
```

### Internal knowledge base

```
For new consultants in their first 90 days,
reduce the number of distinct places they need to look for project-execution guidance from 7 to 1
so that ramp time on engagements drops below 30 days and senior time spent on basic questions drops by half.
```

### E-commerce product catalog

```
For shoppers comparing across product variants,
surface comparable specs (price, availability, shipping) on the same screen
so that comparison time drops from current 4 minutes per product to under 60 seconds, and add-to-cart rate rises measurably.
```

### Founder-formation workflow

```
For first-time founders deciding on entity type and jurisdiction,
present the trade-offs and total costs in plain English at the moment of decision
so that the rate of completed-without-a-lawyer-call decisions doubles, and post-decision regret (measured by entity changes within 12 months) drops below 5%.
```

## Validation tests

Before treating an intent statement as approved:

- **Specificity test.** Could three different teams read this and design wildly different things? If yes, sharpen.
- **Measurability test.** Can you point at the post-launch state and say definitively whether intent was achieved? If not, tighten the outcome.
- **Cut test.** What does this statement *exclude* from scope? If it excludes nothing, it's not yet doing its job.
- **Owner test.** Who signs off on whether intent was achieved? If "the team," it's nobody.

## Stakeholder definition

Be specific. "Customers" or "users" are not stakeholders for this purpose; they're populations.

A stakeholder for an intent statement is:

- A specific role (a "first-year consultant," a "B2B buyer in evaluation phase," a "founder pre-incorporation").
- In a specific context (during onboarding, when comparing vendors, in the first session).
- With a specific need (find guidance, compare specs, decide).

If you can't picture them — what's on their screen, what's on their mind — they're too generic.

## Multi-stakeholder intent

When the IA serves more than one audience, write one intent per audience. Don't merge.

```
For prospects evaluating composable DXP platforms,
make pricing and capability findable in two clicks,
so that we stop losing late-stage deals.

For existing customers researching specific implementation patterns,
make solution briefs and customer stories surface within search and contextual links,
so that customer success time spent answering "how do I" questions drops by 40%.
```

Two intents, two designs, one site that has to honor both. That's the actual scope.

## When to revise the intent

- After facing reality (Covert's step 3) — sometimes the intent gets sharper or more bounded once the inventory is real.
- When stakeholders change ownership of the work.
- When the underlying business strategy shifts (new GTM, new product, new audience).

If the intent never changes, ask whether it was ever real.

## What this isn't

- **Not a vision statement.** Vision is "we believe X." Intent is "we will make Y true."
- **Not a brief.** A brief includes constraints, timeline, audience, deliverables. Intent is one paragraph that goes inside a brief.
- **Not an OKR.** OKRs are measurement structures; intent is the design constraint that the OKRs measure.

## Source

- Covert, A. *How to Make Sense of Any Mess*. 2014. Step 2.
- See also: [[sensemaking-framework]] for the seven-step sensemaking discipline this fits inside.
