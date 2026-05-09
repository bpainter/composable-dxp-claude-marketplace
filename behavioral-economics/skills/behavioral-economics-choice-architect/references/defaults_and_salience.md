---
type: reference
project: skills-library
scope: skill
plugin: behavioral-economics
parent: "[[choice-architect]]"
tags: [type/reference, plugin/behavioral-economics, scope/skill, topic/behavioral-econ]
status: active
---
# Defaults and Salience in Choice Architecture

## The Power of Defaults

Defaults are the most powerful tool in choice architecture. Research shows that:

- **Default effect**: 80-90% of people maintain whatever default is set, regardless of alternatives
- **Organ donation systems**: Countries with opt-out (default yes) have 85%+ donation rates; opt-in countries have 5-15% rates
- **Retirement savings**: Auto-enrollment increases participation from 40% to 90%+
- **Energy conservation**: Defaults for thermostat settings influence consumption more than information or incentives

Defaults work because they:
1. Reduce cognitive load and decision effort
2. Signal what is "normal" or recommended
3. Create inertia due to status quo bias
4. Require active choice to override (friction)

## Designing Defaults Ethically

**Opt-in vs. Opt-out:**
- Opt-out (default yes): Use when the behavior is beneficial and high-friction to decline
  - Examples: Automatic enrollment in benefits, privacy-protective defaults
  - Risk: Users may not realize they're enrolled; ensure visibility and ease of opting out
- Opt-in (default no): Use when the behavior requires active consent or is value-neutral
  - Examples: Marketing communications, data sharing, optional features
  - Risk: Many who would benefit don't participate; consider soft defaults (simplified opt-in)

**Soft Defaults:**
- Provide a default (preset values, selected option) but require active choice confirmation
- Balances friction reduction with informed consent
- Example: Auto-fill form fields with common values, but require explicit submission

## Salience and Attention

People notice what is:
- **Visually prominent** (size, color, position)
- **First or last** in a list (primacy and recency effects)
- **Presented with numbers or comparisons** (vs. abstract descriptions)
- **Normalized or socially reinforced** (seen as typical)

**Design strategies:**
- Place important attributes above the fold or in the top position
- Use visual hierarchy (size, weight, contrast) to signal importance
- Provide side-by-side comparisons to make trade-offs visible
- Highlight the default option explicitly ("Recommended" or "Most popular")
- Use descriptive norms ("Most similar to you choose X")

## Hyperchoice and Decision Paralysis

When people face too many options or complex trade-offs:
- Decision avoidance increases (30%+ abandon the choice)
- Satisfaction with final choice decreases
- Regret and second-guessing increase

**Solutions:**
- Reduce visible options through screening questions ("What's your priority: cost or coverage?")
- Use tiered choice architecture (choose category first, then specific option)
- Provide a simple default with an "expert mode" for detailed customization
- Offer comparison tools and decision support (e.g., "Which plan is right for you?")
- Limit options to 5-7 meaningful alternatives

## Implementation Checklist

- [ ] Define the decision: What is the user choosing? Who chooses? What's the time horizon?
- [ ] Choose the default: Opt-in or opt-out? What are the consequences of inaction?
- [ ] Design salience: What attributes should be prominent? How should options be grouped?
- [ ] Reduce friction: Where is unnecessary complexity? Can steps be streamlined?
- [ ] Enable reversal: How easily can users change their choice later?
- [ ] Test with users: Do they understand the choice? Do they perceive the default as fair?
- [ ] Monitor outcomes: Are participation rates, satisfaction, and equity aligned with goals?
