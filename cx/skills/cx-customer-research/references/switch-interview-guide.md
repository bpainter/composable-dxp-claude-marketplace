---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-customer-research]]"
tags: [type/reference, plugin/cx, scope/skill, topic/research, topic/interviews, source/kalbach]
status: active
---
# Switch interview guide

Drawn from Kalbach, *The Jobs To Be Done Playbook* (ch. 3), and the Re-Wired Group's "Switch Method." A structured interview format for understanding *why people switched* from one solution to another. Useful for product, marketing, and onboarding work.

## When to use the Switch interview

- Understanding why customers chose your product (acquisition).
- Understanding why customers left your product (churn).
- Understanding what customers used before yours (competitive context).
- Understanding why prospects almost-but-didn't switch (anxiety reduction).

Don't use Switch for general discovery research — it's a moment-anchored format. Use it when there's a recent switching event you can investigate.

## The premise

Switching from one solution to another isn't a feature comparison. It's a balance between four forces:

```
            Push of the situation        Pull of the new solution
              (frustrations               (attraction)
               with current)
                    ↓                              ↓
                    THE SWITCHING EVENT
                    ↑                              ↑
             Habit of the present         Anxiety about the new
             (inertia, routine)           (fear, uncertainty)
```

A switch happens when the **pushes + pulls > habits + anxieties**. The interview surfaces all four forces.

## The structure

Anchor on a specific switching event in the past. Walk backwards from the moment of choice. The full timeline:

```
First thought  →  Passive looking  →  Active looking  →  Deciding  →  Consuming
```

Each phase has signal worth capturing.

### Phase 0: Recruit and screen

The participant must:
- Have made the switch recently (within 90 days, ideally <30).
- Remember the experience clearly.
- Be willing to walk through it slowly.

Screen out:
- People who "always wanted to try it" — no real switching event.
- Industry insiders or journalists — they switch for different reasons.
- Heavy researchers — switching processes are atypical.

### Phase 1: Set the scene (5 min)

```
"Today I want to understand the story of how you came to use [new solution].
Specifically, I want to walk through it almost in slow motion, so I can
understand what was happening, what you were thinking, and how the decision
came together. There are no right answers — I'm just trying to understand
your experience."
```

### Phase 2: Anchor on the moment (10 min)

Find the *moment of purchase* / signup / commitment. Ground the interview in the specific calendar day.

```
"Tell me about the day you actually decided to switch to [new solution].
Where were you? What were you doing? What time of day was it?"
```

If they can't remember, dial back to "the week" or "the month." Specificity is the goal — abstract memories produce abstract answers.

### Phase 3: First thought (10 min)

```
"Now let's go back further. When did the idea of switching first cross
your mind? What triggered it?"
```

You're hunting for the **first push** — the first moment something about the existing solution started to grate. Often it's a specific incident.

Probes:
- "What were you trying to do at that moment?"
- "What about it didn't work?"
- "Did you tell anyone about it? What did you say?"

### Phase 4: Passive looking (10 min)

```
"After that first thought, what changed? Did you start noticing things
you hadn't noticed before?"
```

Passive looking = they weren't searching, but their attention shifted. Notices ads they ignored before. Mentions to friends. Idle browsing.

Probes:
- "Did you talk about it with anyone else?"
- "What did you start paying attention to that you hadn't before?"

### Phase 5: Active looking (10 min)

```
"At some point you went from noticing to actually looking. When did that
shift happen? What kicked it off?"
```

Active looking = they're now researching. Searches. Comparison sites. Trial signups.

Probes:
- "Walk me through what you looked at."
- "What stood out — for or against?"
- "What got ruled out, and why?"
- "What were you nervous about?"

This is where **anxieties** surface. People don't volunteer fears; ask explicitly: "What concerned you about [the new solution]?" "What almost made you stay?"

### Phase 6: Deciding (10 min)

```
"Walk me through the decision itself. When you actually committed,
what tipped it? Was there a specific moment?"
```

You're hunting for the **strongest pull** — the thing that closed the deal. Often something small: a free trial that overcame anxiety, a friend's endorsement, a feature they didn't know existed.

Probes:
- "Was there anyone else involved in the decision?"
- "What if [the new solution] hadn't had [the tipping factor] — would you still have switched?"
- "What's the one thing that, if it hadn't been true, would have stopped you?"

### Phase 7: Consuming (10 min)

```
"Now that you've been using [new solution] for a while, how is it going?"
```

You're checking whether the switch lived up to expectations and identifying *anxieties they have now* — these are predictors of churn.

Probes:
- "What's been better than expected?"
- "What's been worse?"
- "Have you considered switching again?"

### Phase 8: Close (5 min)

```
"Thinking back over this whole process — what should I have asked you that
I didn't? Anything you wish I'd understood better?"
```

Useful both for content and for refining the next interview's guide.

## Coding the four forces

After the interview, code the transcript into the four-force grid. Each statement gets tagged.

```
PUSHES (away from old)              PULLS (toward new)
- "I was so frustrated I…"          - "What got me was…"
- "It kept happening that…"         - "I'd been hearing about…"
- "The last straw was…"             - "[Friend] said it was…"
- "I just couldn't…"                - "I tried it and…"

HABITS (anchor to old)              ANXIETIES (about new)
- "I'd been using [old] for…"       - "I was worried that…"
- "I knew where everything was…"    - "What if it didn't…"
- "It was just easier to…"          - "I couldn't tell whether…"
- "I didn't want to retrain…"       - "I almost didn't because…"
```

## Synthesis: from interviews to design

After 8–12 Switch interviews:

1. **Aggregate the four-force grid.** Tally frequency and intensity of each statement type.
2. **Identify the dominant pushes** — what frustrations consistently triggered the search.
3. **Identify the dominant pulls** — what consistently closed the deal.
4. **Identify the dominant anxieties** — what nearly stopped the switch.
5. **Identify the dominant habits** — what kept people on the old solution.

For each force, design moves:

| Force | Design move |
|---|---|
| Pushes | Marketing — name the frustration explicitly in messaging |
| Pulls | Product — strengthen the pulls; make them visible in early experience |
| Anxieties | Conversion — preempt and reduce. Free trials, money-back, comparison tools |
| Habits | Onboarding — ease the transition cost. Importers, training, parallel use period |

## Common failure modes

- **Drifting to the product.** The interview becomes "what do you like about us?" — meaningless. Keep the focus on the *switching event*, not on product features.
- **Skipping anxieties.** They don't volunteer fears; you have to ask. If your transcript has no anxiety statements, the interview wasn't deep enough.
- **Too few interviews.** Switch interviews need 8–12 per pattern to surface the four forces with confidence. Three interviews are anecdote.
- **Over-recruiting fans.** People who happily switched will overweight pulls. Recruit also: people who almost-didn't, people who switched and switched back, churners.
- **Coding by feature instead of by force.** Statements like "the search was good" should be coded as pulls, not just as features. The point is the *direction* of the force.

## Worked example outputs

**Pushes (composable DXP shopping):**
- "I was tired of waiting on engineering for every CMS change." [n=8]
- "Our marketing team couldn't ship campaigns without dev tickets." [n=6]

**Pulls:**
- "Composable means I can pick best-of-breed for each layer." [n=7]
- "We saw [competitor] migrate and it took them 4 months — that gave us confidence." [n=5]

**Anxieties:**
- "What if we end up with five vendors instead of one to manage?" [n=9]
- "Our team doesn't have the API skills." [n=6]

**Habits:**
- "[Old monolith] is what everyone knows. Retraining is real cost." [n=8]
- "We have ten years of content in [old CMS]." [n=7]

→ Design moves:
- Marketing — lead with the engineering-bottleneck push.
- Product — emphasize the integrator-led approach (one accountable partner) to reduce vendor-sprawl anxiety.
- Sales — develop a migration-time benchmark and case studies to ease both anxiety and habit.

## Sources

- Kalbach, J. *The Jobs To Be Done Playbook*, ch. 3. Two Waves / Rosenfeld, 2020.
- Re-Wired Group, "The Switch Method" — re-wiredgroup.com.
- Klement, A. *When Coffee and Kale Compete*. NYC Publishing, 2016.
- Christensen, C. *Competing Against Luck*. HarperBusiness, 2016 (the milkshake / mattress examples).
