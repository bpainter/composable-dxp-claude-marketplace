---
type: reference
project: skills-library
scope: skill
plugin: behavioral-economics
parent: "[[statistician]]"
tags: [type/reference, plugin/behavioral-economics, scope/skill, topic/behavioral-econ]
status: active
---
# I/O Psychology Applications: Statistical Analysis in Organizations

## Analyzing Organizational Outcomes

### Employee Engagement and Satisfaction

**Common measures**:
- **Engagement scales**: UWES (Utrecht Work Engagement Scale), Gallup Q12
- **Job satisfaction**: Overall job satisfaction, facet satisfaction (pay, supervision, autonomy)
- **Commitment**: Organizational commitment (affective, continuance, normative)
- **Psychological safety**: Team psychological safety scale
- **Culture perception**: Values alignment, fairness, inclusion

**Typical analyses**:

**Descriptive statistics**:
- Mean and SD of engagement by department or team
- % scoring high (e.g., "actively engaged" vs. "disengaged")
- Trends over time (pre-post change, year-over-year)

**Inferential statistics**:
- Compare engagement across groups: ANOVA (department, tenure group, manager)
- Test relationship between engagement and outcomes: Correlation or regression
  - Does engagement predict retention? (r = 0.3-0.5 typical)
  - Does engagement predict performance? (r = 0.2-0.4 typical)

**Example analysis**:
- Survey 200 employees on engagement (1-5 scale)
- Compare engagement by department: ANOVA (F-test)
- "Engineering has significantly higher engagement (M=4.1) than Sales (M=3.2), F(3,196)=4.2, p=0.007"
- Test if engagement predicts turnover: Logistic regression
- "Each unit increase in engagement score reduces odds of leaving by 40%"

### Performance Appraisal and Rating Data

**Statistical concerns**:
- **Rater leniency**: Tendency to rate everyone as "meets expectations" or higher
- **Range restriction**: If everyone rated 3-4 on 1-5 scale, variance is artificial
- **Halo effect**: Raters allow one strong trait to influence all ratings

**Analyses**:
- Describe distribution of ratings: Are they normally distributed or skewed high?
- Compare ratings by rater: Does one manager rate significantly differently?
- Validate performance ratings against outcomes: Do higher-rated employees actually perform better?
- Fairness analysis: Do ratings vary by demographic group? (May indicate bias)

**Example**:
- Performance ratings show mean 3.9 on 1-5 scale; skewed toward high ratings
- Correlation between manager rating and sales performance: r = 0.15 (very weak)
- Interpretation: Ratings may not be valid measures of actual performance

### Retention and Turnover Analysis

**Common metrics**:
- **Turnover rate**: % of employees leaving per year
- **Voluntary vs. involuntary**: Did they quit or were they fired?
- **Tenure**: How long do employees typically stay?
- **Early departures**: Do people leave in first 12 months?

**Statistical analyses**:
- Compare turnover across groups: Is turnover higher in some departments?
- Predict who will leave: Logistic regression using engagement, pay, manager quality
- Compare retention before/after intervention: Did policy change reduce turnover?

**Example**:
- Engineering has 8% annual turnover; Sales has 22%
- Statistical test: Chi-square for independence
- "Turnover differs significantly by department, χ²(3)=8.4, p=0.04"
- Interview Sales department to understand why

### Leadership and Manager Effectiveness

**Measures**:
- 360-degree feedback (subordinates, peers, self, supervisor rate manager)
- Direct report engagement (team engagement often correlates with manager quality)
- Span of control metrics (how many reports? productivity per report?)
- Turnover and retention on their team

**Analyses**:
- Compare 360 ratings across managers: Who are the most/least effective?
- Correlate manager effectiveness with team outcomes: Does better-rated manager have higher team engagement?
- Predict manager success: What 360 dimensions predict retention and performance?

**Example**:
- Conduct 360-degree feedback for 30 managers
- Correlate manager ratings with team engagement scores
- "Manager consideration (listening, support) correlates r=0.52 with team engagement; initiating structure correlates r=0.31"
- Recommendation: Focus leadership training on empathy and support

## Statistical Validation of Assessment Tools

### Reliability

**Internal consistency** (Do items measuring same construct correlate?):
- Cronbach's alpha: 0.70+ acceptable; 0.80+ strong
- If alpha too low: Items not measuring same thing; remove weak items
- If alpha too high (>0.95): Items are redundant; remove duplicates

**Test-retest reliability** (Do scores stay stable over time?):
- Administer test twice with time interval (e.g., 2 weeks)
- Correlate scores: r=0.70+ acceptable
- Low correlation: Test is unstable; may not be reliable measure

### Validity

**Construct validity** (Does test measure what it claims?):
- Convergent validity: Measure correlates with other measures of same construct
- Discriminant validity: Measure does not correlate with unrelated constructs
- Example: Psychological safety scale should correlate with team trust but not with cognitive ability

**Criterion validity** (Do scores predict relevant outcomes?):
- Predictive validity: Does score predict future performance?
- Concurrent validity: Does score correlate with current performance?
- Example: Interview score should predict job performance; correlation r>0.3 expected

**Differential validity** (Does test work equally well for all groups?):
- Are correlations similar for different demographic groups?
- If different: Test may be biased; consider alternative measures

## Pre-Post Change and Intervention Evaluation

### Before-After Design

**Simple comparison**:
- Measure outcome before intervention (pretest)
- Implement intervention
- Measure outcome after intervention (posttest)
- Compare: Did outcome change significantly?
- Statistical test: Paired t-test (if normal) or Wilcoxon signed-rank (if non-normal)

**Example**:
- Measure team engagement before leadership coaching: M = 3.2
- Coaching intervention over 3 months
- Posttest engagement: M = 3.7
- Paired t-test: t(24) = 2.3, p = 0.03, Cohen's d = 0.46 (medium effect)
- Interpretation: Engagement improved significantly; improvement is meaningful

### Control Group Comparison

**Stronger design**:
- Random assignment to intervention or control group
- Pretest both groups
- Implement intervention in treatment group only
- Posttest both groups
- Compare: Did treatment group improve more than control?
- Statistical test: ANOVA with time (pre-post) and group (treatment-control) factors

**Advantages**:
- Controls for natural improvement over time
- Controls for testing effects (practice from pretest)
- Stronger evidence of intervention effect

**Example**:
- Randomly assign 60 employees to coaching (n=30) or control (n=30)
- Pretest both groups on engagement
- Coaching for treatment group; control group does nothing
- Posttest both groups
- ANOVA: Interaction of time × group: F(1,58)=4.1, p=0.05
- Interpretation: Treatment group improved more than control; coaching appears effective

## Common I/O Analysis Scenarios

**Scenario 1: Declining Engagement Survey**
- Compare engagement this year to last year: Paired t-test or repeated ANOVA
- Identify which departments dropped most: ANOVA comparison across departments
- Correlate engagement with turnover in same period: Do declining engagement and rising turnover correlate?
- Investigate barriers: Analyze open-ended comments; conduct focus groups

**Scenario 2: Performance Rating Across Managers**
- Describe distribution of ratings by manager: Means, SDs, ranges
- Test if ratings differ significantly: ANOVA
- Assess fairness: Are certain groups rated significantly lower? (t-test or ANOVA by demographic)
- Validate ratings: Correlate ratings with objective performance (sales, quality metrics)

**Scenario 3: Leadership Development Effectiveness**
- Pre-post engagement scores for leaders who attended program: Paired t-test
- Compare to leaders who didn't attend: Randomized control trial with ANOVA
- 360-degree feedback change: Paired t-test on each dimension
- Correlate coaching with team retention: Regression of team turnover on manager coaching participation

## Implementation Checklist

- [ ] Define outcome variables and measures clearly
- [ ] Check reliability of assessment tools (Cronbach's alpha, test-retest)
- [ ] Validate assessment tools (convergent validity, criterion validity)
- [ ] Describe distributions: Means, SDs, skewness, outliers
- [ ] Check for group differences: ANOVA or comparison tests
- [ ] Test relationships: Correlation or regression
- [ ] Report both p-values and effect sizes
- [ ] Interpret practical significance, not just statistical
- [ ] Control for confounding variables in regression
- [ ] Check assumptions; use alternatives if violated
- [ ] Report limitations and alternative interpretations
