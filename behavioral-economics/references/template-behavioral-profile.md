---
type: reference
project: skills-library
scope: plugin
plugin: behavioral-economics
tags: [type/reference, plugin/behavioral-economics, scope/template, topic/behavioral-econ, source/slalom]
status: active
---

# Template — Behavioral Profile

**Source:** Slalom Behavioral Design Model, p. 28 (full template) and pp. 17–28 (component-by-component breakdown). See `2026-05_Slalom-Behavioral-Design-Model.md` in `40_Library/Solution_Briefs/`.

A Behavioral Profile is a comprehensive representation of a customer segment's behaviors, motivations, constraints, and economic context. Use it as the foundation for choice-architecture work — Stage 2 of the model. Without a tight Behavioral Profile, downstream choice-architecture and intervention design lose their target.

## When to use it

- Stage 2 of any behavioral-design engagement
- Whenever you're entering a new domain or segment and don't yet know what drives the relevant behavior
- Before a workshop where the team needs a shared picture of "the customer we're designing for"
- As input to journey mapping, service design, and persona work in adjacent disciplines

This is **not** a generic persona. A persona answers "who is this person?" A Behavioral Profile answers "what behaviors do they currently exhibit, what's blocking the better behavior, and what economic outcome will the right intervention produce?"

## The eight components

| Component | What it captures | Use it for |
|---|---|---|
| Customer Segment | Classification by shared characteristics, needs, or behaviors | Segmentation, scope of intervention |
| Socio-demographic Information | Age, occupation, education, income, marital status, geography | Targeting, channel selection |
| Goals (short- and long-term) | Health goals, lifestyle aspirations, personal development | Aligning intervention to what the customer already wants |
| Desired Economic Impact | What economic outcome the customer themselves seeks | Linking intervention to customer value |
| Key Tasks or Actions | Essential tasks framed as "When (trigger), I want to (action), so I can (outcome)" | Structuring the choice architecture; designing interventions |
| Motivations | Intrinsic + extrinsic drivers | Picking which lever to pull (autonomy vs. incentives vs. recognition) |
| Social Influences | Family, friends, community, cultural norms | Designing social proof and reference-group messaging |
| Environment | Physical and social surroundings; access to resources | Removing situational barriers; choosing intervention context |
| Barriers to Change | Cognitive, physical, emotional, resource-based obstacles | What the intervention must overcome |

## Template (copy this into your deliverable)

```markdown
# Behavioral Profile — [Segment Name]

## Customer Segment
[One-line classification — e.g., "Co-morbid patient: multiple chronic conditions requiring ongoing medical attention and care coordination"]

## Socio-Demographic Information
- Age range:
- Occupation:
- Education level:
- Income bracket:
- Marital status:
- Geographic location:

## Goals
**Short-term (next 6 months):**
- 

**Long-term (1-5 years):**
- 

## Desired Economic Impact
[What economic outcome the customer themselves wants — cost savings, value received, avoided costs, etc.]

## Key Tasks or Actions
The essential tasks the customer expects to take. Frame each as: "When (trigger), I want to (action), so I can (outcome)."

**Task 1:**
When [trigger], I want to [action], so I can [outcome].

**Task 2:**
When [trigger], I want to [action], so I can [outcome].

**Task 3:**
When [trigger], I want to [action], so I can [outcome].

**Task 4:**
When [trigger], I want to [action], so I can [outcome].

## Motivations

**Intrinsic** (autonomy, mastery, satisfaction, fear, identity):
- 

**Extrinsic** (financial incentives, social recognition, family encouragement):
- 

## Social Influences
The impact of family, friends, community, and cultural norms on this segment's choices.
- Family and friends:
- Healthcare providers / authority figures:
- Peer groups / community:

## Environment
Physical and social surroundings, including access to resources and services.
- Home setting:
- Community resources:
- Workplace / institutional context:

## Barriers to Change

**Cognitive** (information complexity, attention, comprehension):
- 

**Physical** (mobility, access, capability):
- 

**Emotional** (overwhelm, fear, depression, regret):
- 

**Resource-based** (financial, time, social capital):
- 
```

## Worked example — Co-morbid patient (Slalom p. 28)

**Customer Segment:** Co-morbid patients — multiple chronic conditions requiring ongoing medical attention and care coordination.

**Socio-Demographic:** Age 28–32; part-time employment; high school to some college; lower-to-middle income; married; urban/suburban.

**Goals — short-term:** Establish a routine for managing multiple conditions; reduce hospital visits; improve daily quality of life.

**Goals — long-term:** Stable health status; prevent progression; avoid additional comorbidities.

**Desired Economic Impact:** Reduce annual healthcare expenses 20% through proactive management — decreasing ER visits 30%, reducing readmissions 25%.

**Key Tasks:**
- When I receive a reminder, I want to record vital signs, so I can detect changes early.
- When it's time for medication, I want to follow the schedule accurately, so I can prevent complications.
- When I feel a change in health status, I want to consult my care team via telehealth, so I can adjust my care plan.
- When I receive care-plan updates, I want to integrate them into my routine, so I can adhere to recommendations.

**Motivations:** *Intrinsic* — autonomy in health management, satisfaction from outcomes, fear of progression. *Extrinsic* — financial incentives, family approval, provider reinforcement.

**Social Influences:** Family/friends provide medication management and appointment encouragement. Healthcare providers offer regular check-ins and motivation. Peer support groups share coping strategies.

**Environment:** Home setting (in-home care, pharmacy access, assistive devices); community resources (transport, support networks).

**Barriers:** Cognitive (complex medical info), Physical (mobility), Emotional (overwhelm/depression), Resource-based (financial constraints).

## Pitfalls

- **Mistaking a persona for a Behavioral Profile.** A persona names a fictional individual; a Behavioral Profile names *behaviors*. Don't ship a profile without the Tasks, Motivations, and Barriers sections.
- **Skipping the economic impact.** The "Desired Economic Impact" line is what links the customer side of the equation to the business case in stages 3 and 5. If you can't write it, the engagement isn't ready for choice-architecture work yet.
- **Over-specifying demographics.** Demographics constrain channel and tone, not the intervention itself. Don't let a tight demographic box hide a broad behavior pattern.
- **Vague tasks.** "Manage their health" is not a task. "Refill prescription" or "log a blood-pressure reading" is a task. The "When (trigger), I want to (action), so I can (outcome)" form forces specificity.
- **Single profile when there are multiple segments.** If the behavior differs by sub-segment (e.g., newly diagnosed vs. established patients), build separate profiles.

## Hand-off downstream

A complete Behavioral Profile feeds:

- **Choice Architecture template** (`template-choice-architecture.md`) — Tasks become the steps; Barriers become the friction points.
- **Intervention Design template** (`template-intervention-design.md`) — Motivations and Social Influences inform which behavior-change tactics to pick.
- **Use-Case Scoring** (`template-use-case-scoring.md`) — Desired Economic Impact informs the customer-value scoring axis.

## Skills that use this template

- `behavioral-economics-behavioral-economist` (primary)
- `behavioral-economics-choice-architect` (consumes profile output)
- `behavioral-economics-organizational-behavior-specialist` (adapts for employee segments)
- `behavioral-economics-research-methods` (validates profile claims with primary research)
