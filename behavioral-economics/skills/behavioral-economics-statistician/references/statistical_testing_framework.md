---
type: reference
project: skills-library
scope: skill
plugin: behavioral-economics
parent: "[[statistician]]"
tags: [type/reference, plugin/behavioral-economics, scope/skill, topic/behavioral-econ]
status: active
---
# Statistical Testing Framework: From Question to Interpretation

## Choosing the Right Statistical Test

### Decision Tree: What Test Should I Use?

**Step 1: What is your research question?**

**Comparing groups** (Do groups differ on some outcome?):
→ Use comparison tests (t-test, ANOVA, Mann-Whitney U)

**Examining relationships** (Are two variables related?):
→ Use correlation or regression

**Predicting outcomes** (Can we predict outcome from predictors?):
→ Use regression (linear or logistic)

**Categorizing cases** (Do cases fall into natural groups?):
→ Use cluster analysis or factor analysis

### Step 2: What type of data do you have?

**Continuous data** (e.g., age, salary, satisfaction score 1-10):
- Parametric tests (t-test, ANOVA): More powerful if assumptions met
- Nonparametric tests (Mann-Whitney U, Kruskal-Wallis): If assumptions violated

**Categorical data** (e.g., gender, department, yes/no):
- Chi-square test for relationships
- Proportions tests

**Combination** (comparing categorical groups on continuous outcome):
- t-test (two groups) or ANOVA (3+ groups)

### Step 3: Check Assumptions

**For parametric tests (t-test, ANOVA, Pearson correlation)**:

**Normality**: Data is approximately normally distributed
- Check: Histogram, Q-Q plot, Shapiro-Wilk test
- Note: With large samples (n>30), slight non-normality okay (Central Limit Theorem)
- Fix if violated: Transform data (log, square root) or use nonparametric test

**Homogeneity of variance**: Variance is similar across groups
- Check: Levene's test
- Fix if violated: Use Welch's correction or nonparametric test

**Independence**: Observations are independent (not matched/paired)
- This is about study design, not testable after data collection
- Paired data uses paired t-test, not independent t-test

**Common parametric tests and when to use**:

| Test | Purpose | Groups | Assumptions |
|------|---------|--------|-------------|
| Independent t-test | Compare means of two groups | 2 groups | Normal, homogeneous variance, independent |
| Paired t-test | Compare means for same subjects at two times | 2 time points | Normal differences, independent pairs |
| One-way ANOVA | Compare means across 3+ groups | 3+ groups | Normal, homogeneous variance, independent |
| Pearson correlation | Relationship between two continuous variables | N/A | Normal, linear relationship, no outliers |
| Linear regression | Predict continuous outcome from predictors | Multiple predictors | Normal residuals, linear relationship, independence |

**Nonparametric alternatives** (use if assumptions violated):

| Parametric Test | Nonparametric Alternative |
|-----------------|---------------------------|
| Independent t-test | Mann-Whitney U test |
| Paired t-test | Wilcoxon signed-rank test |
| One-way ANOVA | Kruskal-Wallis test |
| Pearson correlation | Spearman correlation |

## Interpreting Test Results

### Understanding SPSS or Excel Output

**Key statistics for each test**:

**t-test output**:
- **t-value**: Standardized test statistic (larger = bigger difference relative to variability)
- **p-value**: Probability of this result if null hypothesis is true (p<0.05 = statistically significant)
- **df (degrees of freedom)**: Related to sample size; larger sample = larger df
- **95% CI**: Confidence interval; if it doesn't include 0, result is significant
- **Mean difference**: How much higher is one group than the other?
- **Standard error**: Uncertainty in the mean difference estimate

**ANOVA output**:
- **F-statistic**: Ratio of between-group variance to within-group variance
- **p-value**: Is overall difference significant? (If p<0.05, at least two groups differ)
- **Mean squares**: Variance estimates
- **Post-hoc tests** (if ANOVA is significant): Which specific groups differ?

**Correlation output**:
- **r (correlation coefficient)**: Strength and direction of relationship (-1 to +1)
  - 0.1-0.3: Small
  - 0.3-0.5: Medium
  - 0.5+: Large
- **p-value**: Is correlation statistically significant?
- **R-squared (r²)**: Proportion of variance explained (e.g., r=0.5 → r²=0.25 → 25% variance explained)

**Regression output**:
- **Coefficients**: How much does outcome change per unit increase in predictor?
- **p-value (for each predictor)**: Is this predictor statistically significant?
- **R-squared**: What % of variance is explained by all predictors?
- **Residual standard error**: Typical prediction error

### Effect Size: Practical vs. Statistical Significance

**Statistical significance** (p-value):
- Is result unlikely due to chance?
- Depends on sample size: Large samples can find tiny effects significant
- p < 0.05 is conventional threshold (5% false positive rate)

**Practical significance** (effect size):
- Is difference/relationship meaningful in real world?
- Independent of sample size

**Example**:
- p = 0.04 (statistically significant), but Cohen's d = 0.1 (tiny effect)
- Interpretation: Real difference exists, but it's trivially small
- Recommendation: May not be worth implementing or worrying about

**Calculating and interpreting effect sizes**:

**Cohen's d** (for comparing means):
- d = (Mean1 - Mean2) / Standard Deviation
- Small = 0.2, Medium = 0.5, Large = 0.8
- Example: d=0.5 means treatment group is 0.5 SD higher than control

**Correlation coefficient (r)**:
- Ranges from -1 to +1
- r² = variance explained (e.g., r=0.4 → r²=0.16 → 16% explained)

**Regression coefficient**:
- Raw coefficient: Units of outcome (e.g., salary increases $5,000 per year tenure)
- Standardized coefficient (beta): Standard deviations (e.g., engagement increases 0.3 SD per unit of psychological safety)

### Confidence Intervals: More Informative Than p-values

**What is a confidence interval?**
- Range of likely true values (usually 95%)
- If you repeated study 100 times, true value would fall in this range approximately 95 times
- Wider interval = more uncertainty; narrower interval = more precise estimate

**Interpretation**:
- 95% CI [0.2, 0.8] for mean difference
- True difference is likely between 0.2 and 0.8
- If CI includes 0: Not statistically significant (p>0.05)
- If CI doesn't include 0: Statistically significant (p<0.05)

**Advantages over p-values**:
- Shows magnitude and range of effect (not just yes/no)
- Shows precision of estimate
- Better for decision-making

## Common Statistical Errors

**Multiple comparisons problem**:
- If you do 20 statistical tests with α=0.05, expect 1 false positive by chance
- With many comparisons, use Bonferroni correction: α = 0.05/number of tests
- Example: 5 comparisons → α = 0.01 instead of 0.05

**Fishing for significance**:
- Running many analyses hoping one is significant
- If you find p=0.04 after exploring, it's likely a false positive
- Solution: Pre-register hypotheses before analyzing

**Ignoring effect size**:
- p=0.04 with tiny effect (d=0.1) is statistically significant but practically meaningless
- Always report effect size along with p-value

**Confusing correlation with causation**:
- r=0.6 between X and Y doesn't mean X causes Y
- Other variables might explain relationship
- Example: Ice cream sales and drowning deaths correlate (both up in summer), but ice cream doesn't cause drowning

**p-hacking and HARKing**:
- **p-hacking**: Trying different analyses until you find significance
- **HARKing**: Hypothesizing After Results Known (treating exploratory findings as confirmatory)
- Prevention: Pre-register hypothesis and analysis plan

## Implementation Checklist

- [ ] Define research question clearly
- [ ] Identify variables and types (continuous, categorical)
- [ ] Check sample size adequacy
- [ ] Examine data for normality, outliers, missing data
- [ ] Choose appropriate test based on question and assumptions
- [ ] Run test and obtain p-value, effect size, confidence interval
- [ ] Interpret practical significance (not just statistical significance)
- [ ] Check for violations of assumptions; use alternatives if needed
- [ ] Report findings: Means/proportions, effect sizes, CIs, p-values
- [ ] Translate to business implications
- [ ] Acknowledge limitations and alternative explanations
