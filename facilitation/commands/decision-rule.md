---
description: Recommend a decision-making rule for a specific decision — leader-decides, majority vote, multi-vote, fist of five, consent, consensus, RAPID, integrative, or trade-off matrix. Includes the moment-of-decision script.

# Project context
type: command
project: skills-library
plugin: facilitation
tags: [type/command, plugin/facilitation]
status: active
---

Use the `facilitation-decision-architect` skill to pick the right decision method for a specific decision the user is facing. The most common workshop failure is "we discussed but didn't decide" — that's a decision-rule failure, named in advance prevents it.

Reference: [`decision-methods-catalog.md`](../skills/facilitation-decision-architect/references/decision-methods-catalog.md) for the full method-by-method working notes.

Steps:

1. **Get the inputs.** Ask if not provided:
   - **What's the decision?** (Specific — "should we ship Friday or push to Monday?" not "release decision")
   - **Stakes** (low/medium/high — irreversible or hard to reverse?)
   - **Urgency** (today, this week, this month, this quarter?)
   - **Buy-in needed** (will the people who execute have to believe in it?)
   - **Information distribution** (does any one person have the full picture?)
   - **Group size** (1–4, 5–8, 9+)
   - **Existing trust** (high, mixed, low)

2. **Apply the decision tree** from the catalog:

```
Is the decision irreversible or high-stakes?
├─ Yes → invest in deliberation
│        Is execution buy-in critical?
│        ├─ Yes → Consent or RAPID
│        └─ No → Leader decides after consultation
└─ No → use a faster method
         Is one person holding the picture?
         ├─ Yes → Leader decides
         └─ No → Multi-vote (narrow) → Fist of five (confirm)
```

3. **Recommend one method**, with rationale in 2–3 sentences. If it's close between two, name both and the discriminating factor.

4. **Anti-recommendations.** Name what *not* to use and why:
   - "Don't use consensus here — group is too large and time-pressured. Use consent instead."
   - "Don't use RAPID — this is a personal call, not cross-functional. Leader decides after consultation."

5. **Show the moment-of-decision script.** For the recommended method, give the user the actual sentences to say:
   - **Frame the rule**: "We're using [method]. Here's what that means for how this conversation lands..."
   - **Frame the proposal**: For consent/leader-after-consultation, what to say
   - **Test for objection / agreement**: The exact prompt
   - **Land the decision**: How to close
   - **Capture live**: Use [`template-decision-record.md`](../templates/template-decision-record.md)
   - **Name the owner**: "[Name], you're owning the next step?"
   - **Set the checkpoint**: "We'll review on [date]"

6. **Anticipate dissent.** If the decision is contested:
   - Recommend pre-mortem or "Why It Won't Work" before the decision moment
   - Coach on devil's-advocate assignment
   - Plan how dissent will be honored in the [decision record](../templates/template-decision-record.md) ("dissent voiced" section)

7. **For recurring decisions of similar type** (steering committee items, architecture review, hiring), recommend the cadence-level rule rather than per-decision: "For all decisions like this, this body uses [method]." Document in the team's working agreements.

Don't pick consensus when consent will do — it's the most common over-investment in deliberation cost.
