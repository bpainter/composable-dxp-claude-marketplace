---
type: reference
project: skills-library
scope: skill
plugin: behavioral-economics
parent: "[[research-methods]]"
tags: [type/reference, plugin/behavioral-economics, scope/skill, topic/behavioral-econ]
status: active
---
# Mixed Methods Research: Combining Qualitative and Quantitative Approaches

## Why Mixed Methods?

Mixed methods combines the strengths of qualitative (understanding "why") and quantitative (measuring "what") research.

**Benefits**:
- **Triangulation**: Different methods converge on same findings, increasing confidence
- **Completeness**: Both breadth (quant) and depth (qual)
- **Explanation**: Qual explains quant patterns; quant tests qual hypotheses
- **Flexibility**: Can adapt methods based on initial findings

## Two Primary Approaches

### 1. Explanatory Sequential Design: Quantitative First, Then Qualitative

**Sequence**:
1. Conduct quantitative study (survey, experiment)
2. Analyze findings to identify patterns, unexpected results
3. Conduct qualitative study (interviews) to explain "why"

**Example**:
- **Quant**: Survey 500 customers on satisfaction; find that 30% will churn
- **Qual**: Interview 20 customers likely to churn; understand specific reasons
- **Integration**: Quantitative shows "what" (30% churn risk); qualitative shows "why" (feature gaps, poor support, competitor offerings)

**When to use**:
- Already have quantitative data but need to understand findings
- Want to test prevalence before deep exploration
- Findings have surprises that need explanation
- Budget is tight (quantitative can be more cost-effective first)

**Challenges**:
- Qualitative participants may not represent survey respondents
- Time-intensive (sequential adds time)
- Qual sample may not capture all reasons found in quant

### 2. Exploratory Sequential Design: Qualitative First, Then Quantitative

**Sequence**:
1. Conduct qualitative study (interviews, focus groups)
2. Identify themes and constructs
3. Develop survey based on themes
4. Conduct quantitative study to test prevalence and relationships

**Example**:
- **Qual**: Interview 15 new customers; identify barriers to onboarding (unclear UI, lack of tutorials, poor documentation)
- **Quant**: Survey 300 customers; measure how many experience each barrier; test correlation between barriers and retention
- **Integration**: Qualitative generates hypotheses; quantitative tests prevalence and relationships

**When to use**:
- Topic is new or poorly understood
- Need to develop constructs before measurement
- Want to ensure survey captures actual customer concerns
- Limited existing research on topic

**Challenges**:
- Qualitative sample may not reflect broader population
- Survey design depends on qualitative themes; if limited qual sample, may miss themes
- Takes more time (sequential)

## Integration Approaches

### Data Triangulation: Do Methods Agree?

**Convergence**:
- Qual and quant findings point to same conclusions
- Increases confidence in findings
- Example: Interviews show churn drivers; survey shows same factors correlate with retention

**Divergence**:
- Qual and quant findings differ or conflict
- Signals need for deeper investigation
- May reveal different dynamics in different groups or contexts
- Example: Interviews suggest price is barrier; survey shows price is not top-cited reason

**Strategies for divergence**:
1. Check for sampling differences: Were qual and quant samples comparable?
2. Check methodology: Could method differences explain results?
3. Investigate deeper: Maybe qual captured drivers, quant captured rationalizations
4. Segment analysis: Maybe true for some groups but not others
5. Acknowledge complexity: Sometimes reality is more nuanced than either method suggests

### Sequential Integration: Qual Findings Guide Quant; Quant Tests Qual

**Theme development**:
- Qualitative research identifies themes, concepts, hypotheses
- Themes become constructs to measure in survey
- Example: Interviews identify "sense of community" as adoption driver
- Survey asks: "I feel connected to other users" (1-5 Likert)

**Hypothesis testing**:
- Qual generates hypotheses: "X barrier prevents adoption"
- Quant tests hypothesis: What % experience barrier? Is barrier related to adoption?
- Example: Interviews suggest simplicity is key; survey tests whether perceived simplicity predicts satisfaction

**Scope refinement**:
- Qual findings show what's most important to measure
- Quant can focus on drivers identified in qual rather than guessing
- More efficient survey design

## Mixed Methods Analysis and Reporting

### Integration in Analysis

**Separate analysis first**:
- Analyze qualitative data independently (thematic coding, patterns)
- Analyze quantitative data independently (descriptive stats, hypothesis tests)
- Document findings from each method

**Integration step**:
- **Narrative integration**: Tell story using findings from both methods
  - "Interviews showed X barriers. Survey found Y% experience each barrier. Strongest predictor of churn is Z."
- **Joint display**: Visual or table showing qual and quant findings side-by-side
  - Barriers (from interviews) | % Experiencing (from survey) | Correlation with churn
- **Triangulation assessment**: Where do methods agree? Diverge?

### Weighing Evidence

**When methods converge**:
- High confidence in findings
- Can make stronger recommendations

**When methods diverge**:
- Acknowledge and explore divergence
- May reveal nuance or context-dependence
- Don't dismiss one method; investigate why

**Sample and context matter**:
- Small qual sample may miss important themes
- Large quant sample may not capture lived experience
- Each method has strengths and limitations

## Implementation Checklist

- [ ] Choose approach: Explanatory sequential or exploratory sequential?
- [ ] Design qualitative phase: Interview guide, sample size, recruitment
- [ ] Analyze qualitative data: Thematic coding, pattern identification
- [ ] Design quantitative phase: Survey based on qual themes or quant first for exploration
- [ ] Recruit and conduct survey: Sample representative of population you want to understand
- [ ] Analyze quantitative data: Prevalence of qual themes; relationships and predictors
- [ ] Integration: Compare findings across methods; create joint displays if helpful
- [ ] Interpretation: Tell complete story using both methods; acknowledge trade-offs
- [ ] Reporting: Make clear which findings come from which method; note convergence/divergence

## Example: Customer Adoption Research

**Exploratory Sequential Approach**:

**Phase 1 - Qualitative** (10 interviews with users):
- Identify adoption drivers: Ease of use, trusted recommendation, low switching cost, integration with existing tools
- Identify barriers: Technical requirements, onboarding friction, lack of community support

**Phase 2 - Quantitative** (Survey 200 customers):
- Measure prevalence: What % value ease of use? What % experienced onboarding friction?
- Test relationships: Does ease of use perception predict adoption? Does onboarding friction predict dropout?
- Segment: Do adoption drivers differ by customer type (SMB vs. enterprise)?

**Integration**:
- Interviews identified drivers; survey shows which drivers matter most (in order of correlation)
- Interviews identified barriers; survey shows which barriers prevent most adoption
- Creates priority list for product improvements based on impact on adoption/retention

**Report**: "Interviews identified 6 adoption drivers and 4 barriers. Survey of 200 customers shows ease of use is strongest driver (r=0.68), followed by trusted recommendation (r=0.52). Onboarding friction is strongest barrier (r=-0.64). Improving onboarding and perceived simplicity would likely have highest impact on adoption."
