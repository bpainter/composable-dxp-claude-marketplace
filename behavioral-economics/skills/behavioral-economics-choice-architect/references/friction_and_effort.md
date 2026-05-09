---
type: reference
project: skills-library
scope: skill
plugin: behavioral-economics
parent: "[[choice-architect]]"
tags: [type/reference, plugin/behavioral-economics, scope/skill, topic/behavioral-econ]
status: active
---
# Behavioral Friction and Decision Effort

## What is Friction?

Friction is any factor that increases cognitive or physical effort required to make and execute a choice. High friction leads to:
- Choice avoidance or abandonment
- Longer decision times
- Lower satisfaction with outcomes
- Regret and switching behavior

## Sources of Friction

### Cognitive Friction
- **Information overload**: Too many attributes, options, or details to process
- **Complexity**: Trade-offs that are hard to compare (e.g., "cost vs. comprehensiveness")
- **Jargon or unclear language**: Technical terms, unfamiliar metrics, or vague descriptions
- **Working memory limits**: Information presented without scaffolding or structure

### Interaction Friction
- **Multiple steps or clicks**: Enrollment forms with 10+ steps vs. single-page forms
- **Re-entering information**: Forms that don't remember previous inputs
- **Error messages**: Unclear feedback about what went wrong
- **Time pressure**: Artificial deadlines that prevent careful consideration

### Social or Emotional Friction
- **Uncertainty about norms**: "What do most people choose? Am I making a weird choice?"
- **Regret or loss aversion**: Fear of making the "wrong" choice
- **Lack of decision support**: No guidance or social proof to build confidence

## Friction Mapping

To diagnose friction:

1. **Map the user journey**: Trace the steps from problem awareness to choice completion to behavior execution
2. **Identify drop-off points**: Where do users abandon? Where do they get stuck?
3. **Classify friction sources**: Is the drop-off due to cognitive load, effort, unclear information, or emotional uncertainty?
4. **Estimate impact**: Which friction points affect the most users?
5. **Assess legitimacy**: Some friction is necessary (e.g., confirmation steps for irreversible choices)

## Reducing Friction Ethically

### For Complex Choices
- **Progressive disclosure**: Show essential information first; hide details behind "learn more" or advanced tabs
- **Decision trees**: Guide users through questions to narrow options before presenting detailed choices
- **Comparison tools**: Provide structured side-by-side comparisons with user-selected filters
- **Simplified defaults**: Offer a recommended option that captures 80% of preferences
- **Explanation**: Use plain language, visuals, and examples to clarify trade-offs

### For Multi-Step Processes
- **Single-page optimization**: Where possible, consolidate into fewer steps
- **Progress indicators**: Show how far users are through the process
- **Auto-fill and memory**: Remember previous inputs and pre-fill forms
- **Mobile-first design**: Ensure touch interfaces are easy to navigate
- **Checkpoint reviews**: Let users confirm their selections before submitting

### For Error Prevention
- **Clear validation**: Show errors in context with suggestions for correction
- **Confirmations for irreversible choices**: "Are you sure?" prompts before permanent deletions
- **Undo or change windows**: Allow edits after submission
- **Constraint-based design**: Make invalid choices impossible rather than penalizing them

## Friction and Equity

Not all friction affects users equally:
- **Literacy variation**: Complex language and dense information barriers exclude low-literacy users
- **Language barriers**: Non-native speakers face higher cognitive load
- **Digital literacy**: Some users are unfamiliar with digital interfaces
- **Accessibility needs**: Users with visual, motor, or cognitive disabilities may face hidden friction

**Design for inclusion**: Test with diverse users; use plain language; provide multiple input methods; support assistive technologies.

## Examples in Practice

**High friction → Low adoption:**
- Healthcare enrollment forms with 20+ pages and medical jargon → 40% completion rate
- Investment platform with 500+ options and no guidance → 70% choose nothing (default remains)
- Donation checkout with 12 steps and repeated address entry → 60% abandonment

**Reduced friction → Higher adoption:**
- Auto-enrollment in benefits → 85%+ participation (vs. 25% with opt-in)
- Tiered choice architecture for investment options → 90% completion and satisfaction
- One-click donation with pre-filled address → 85% completion

## Implementation Checklist

- [ ] Map user journeys with real users; identify abandonment points
- [ ] Count cognitive steps and decision points
- [ ] Test clarity with target audience; measure comprehension
- [ ] Measure drop-off rates at each step
- [ ] Benchmark against industry standards or competitors
- [ ] A/B test friction-reduction changes (e.g., fewer steps, clearer language)
- [ ] Monitor unintended consequences (e.g., reduced friction may lead to less thoughtful choices)
