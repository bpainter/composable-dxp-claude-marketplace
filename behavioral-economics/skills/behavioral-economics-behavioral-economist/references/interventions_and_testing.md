---
type: reference
project: skills-library
scope: skill
plugin: behavioral-economics
parent: "[[behavioral-economist]]"
tags: [type/reference, plugin/behavioral-economics, scope/skill, topic/behavioral-econ]
status: active
---
# Behavioral Interventions: Design, Implementation, and Testing

## Categories of Behavioral Interventions

### Low-Friction Nudges

**Defaults and opt-out:**
- Make beneficial behavior the default option
- Requires active choice to opt out (creates friction to inaction)
- Examples: Auto-enrollment (retirement), organ donation (opt-out), privacy settings
- Strength: Powerful effect size, low cost
- Risk: Must ensure easy opt-out; avoid perception of manipulation

**Reminders and prompts:**
- Email, notification, or message reminding person to act
- Most effective when timed strategically (before decision point)
- Examples: Vaccine appointment reminders, medication adherence reminders, tax filing deadline
- Strength: Improves follow-through and reduces inertia
- Risk: Can be perceived as annoying; frequency matters

**Salience and presentation:**
- Make important information prominent and visible
- Reorder or highlight key attributes
- Examples: Calorie counts on menu items, carbon footprint labels, APR disclosure on credit offers
- Strength: Influences decision without limiting choice
- Risk: Information overload can backfire; ensure clarity

**Social proof and norms:**
- Show what similar others do ("Most employees use flexible scheduling")
- Highlight descriptive norms (what is typical)
- Examples: Occupancy rates on Airbnb, donation matching, peer behavior messaging
- Strength: Leverages conformity and social proof
- Risk: Can backfire if norm is opposite of desired behavior; use descriptive + injunctive norms

**Simplification and choice architecture:**
- Reduce number of options or complexity of decision
- Use tiered choice (simple first, expert mode available)
- Examples: Pre-selected investment portfolios, simplified menu items, decision trees
- Strength: Reduces decision paralysis and effort
- Risk: May limit informed choice; ensure transparency

### Structural Changes

**Incentive redesign:**
- Change rewards or penalties to align behavior with goals
- Examples: Rebates for energy efficiency, penalties for late fees, bonuses for participation
- Strength: Can drive sustained behavior change
- Risk: Crowding out effect (extrinsic incentive may undermine intrinsic motivation)

**Choice architecture redesign:**
- Reorganize how choices are presented (sequence, grouping, comparison)
- Examples: Presenting options side-by-side for comparison, filtering before detailed options, progress bars
- Strength: Can dramatically improve participation and satisfaction
- Risk: Requires user research to ensure improvements match needs

**Commitment devices:**
- Ask people to commit in advance to desired behavior
- Examples: Advance pledges ("I will save 5% next year"), deposit contracts, public commitments
- Strength: Helps overcome present bias and willpower failures
- Risk: Some find commitments off-putting; provide easy exit

**Friction reduction:**
- Reduce effort required for desired behavior
- Examples: One-click enrollment, pre-filled forms, mobile-friendly interfaces
- Strength: Increases participation without limiting choice
- Risk: Can reduce careful deliberation; balance with transparency

### Information and Communication Interventions

**Decision support tools:**
- Recommender systems, choice calculators, comparison tools
- Help people understand trade-offs and find best option for them
- Examples: Investment risk assessment tools, insurance plan comparison tools, loan calculators
- Strength: Improves decision quality and confidence
- Risk: Complex tools may increase cognitive load; test for usability

**Education and clarification:**
- Plain-language explanations, visual presentations, examples
- Make trade-offs explicit and understandable
- Examples: Treatment option explanations (shared decision-making), fee disclosures, risk characterization
- Strength: Improves comprehension and informed consent
- Risk: Longer/more complex information may discourage engagement; test optimal amount

**Narrative and framing:**
- Use stories, examples, and reframing to improve persuasiveness
- Examples: Success stories of others, before/after examples, loss vs. gain framing
- Strength: Activates both System 1 (emotion) and System 2 (logic)
- Risk: Can backfire if perceived as manipulation; maintain transparency

## Testing and Validation Framework

### Experimental Design

**Randomized controlled trial (RCT):**
- Randomly assign some users to intervention, others to control (no intervention)
- Compare outcomes between groups
- Strongest evidence of causation
- Needed for: Major decisions, high-cost interventions, policy implementation

**A/B test:**
- Two versions of intervention (A and B); randomly assign users
- Compare outcomes; measure which version wins
- Useful for: Comparing design options, optimization
- Faster and cheaper than full RCT

**Quasi-experimental design:**
- No random assignment, but leverage natural variation
- Examples: Different rollout timing, different geographic regions, natural policy variation
- Useful for: Situations where randomization isn't feasible
- More prone to confounding variables

### Designing the Test

**Key elements:**

1. **Hypothesis**: What is the intervention? What outcome do you expect?
   - Clear, specific hypothesis improves interpretation
   - Example: "Auto-enrollment will increase participation from 25% to 60%"

2. **Outcome metrics**: What will you measure?
   - Primary outcome (main metric of interest)
   - Secondary outcomes (side effects, satisfaction, retention)
   - Example: Enrollment rate (primary), satisfaction (secondary), dropout rate (secondary)

3. **Sample size**: How many users do you need?
   - Depends on: Expected effect size, statistical power (usually 80%), significance level
   - Power analysis: Calculate required sample before test
   - Too small: May miss real effect; too large: Wasting resources

4. **Control group**: Who doesn't get intervention?
   - Needed to isolate intervention effect from other changes
   - Size: Usually 50% control, 50% treatment (but depends on context)

5. **Test duration**: How long to run test?
   - Should capture full behavior cycle
   - Example: Retirement savings test should run at least 1 quarter to capture decisions

6. **Statistical analysis**:
   - Calculate difference between groups
   - Test significance (p-value < 0.05 = statistically significant)
   - Calculate effect size (small/medium/large)
   - Confidence interval around estimate

### Monitoring and Risk Management

**Key metrics to track:**

- **Participation**: % who adopt behavior
- **Intensity**: How much of behavior (frequency, amount, depth)?
- **Satisfaction**: Do participants like it?
- **Retention**: Do they stick with it?
- **Unintended effects**: Adaptation, gaming, equity issues?

**Red flags to watch for:**

- **Adaptation**: If intervention creates perverse incentives (e.g., setting caps too low)
- **Fatigue**: If repeated nudges lose effectiveness
- **Backlash**: If intervention triggers reactance or negative publicity
- **Equity**: Does intervention affect groups differently?

**Escalation plan:**
- What metrics trigger intervention changes?
- How quickly can you respond to problems?
- Who monitors and makes decisions?

## Implementation Checklist

- [ ] Start with pilot test on small group before full rollout
- [ ] Define clear hypothesis and primary outcome metric
- [ ] Calculate sample size needed to detect effect
- [ ] Use randomization or matched control group
- [ ] Run test long enough to capture full behavior cycle
- [ ] Monitor for unintended consequences during test
- [ ] Analyze results: Significance + effect size + practical importance
- [ ] Check effects by demographic group (are effects equal?)
- [ ] Plan follow-up tests if needed (retention, long-term effects)
- [ ] Document learnings for future interventions
- [ ] Scale intervention with continued monitoring for changes in effect
