---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[ai-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# ML Project Lifecycle (2025)

## Phase 1: Problem Definition

Before coding, establish:

- **Business Objective**: What does success look like? Revenue lift, efficiency gain, risk reduction?
- **Success Metrics**: How do you measure success? (accuracy, precision, recall, AUC, NDCG, user engagement?)
- **Constraints**: Latency (ms), cost ($/inference), fairness requirements?
- **Baseline**: What's the current approach? (rule-based, simpler model, manual process?)
- **Failure Modes**: What could go wrong? How do you mitigate?

## Phase 2: Data Foundation

Quality data is non-negotiable.

- **Exploratory Data Analysis (EDA)**
  - Distributions, correlations, missing values
  - Class imbalance, outliers
  - Feature interactions

- **Data Quality**
  - Validation checks (schema, range, cardinality)
  - Handling missing/corrupted values
  - Temporal consistency (for time-series)

- **Feature Engineering**
  - Domain knowledge application
  - Statistical techniques (binning, interactions, polynomial)
  - Dimensionality reduction if needed

## Phase 3: Modeling & Experimentation

Start simple, iterate with discipline.

- **Baseline Model**: Simple, interpretable, fast to train
- **Incremental Complexity**: Add features/capacity based on learnings
- **Experiment Tracking**: Log all configurations, metrics, artifacts
- **Validation Strategy**:
  - Cross-validation for stability
  - Stratified splits for imbalanced data
  - Temporal splits for time-series
  - Hold-out test set (never tune on test!)

## Phase 4: Evaluation & Diagnostics

Comprehensive evaluation beyond a single metric.

- **Multiple Metrics**: Precision, recall, F1, AUC, calibration
- **Confusion Matrix**: Understand error types
- **Feature Importance**: SHAP, permutation importance
- **Error Analysis**: Where does the model fail? Systematic patterns?
- **Fairness Audit**: Performance across demographic groups
- **Benchmark Comparison**: How does it compare to baselines and published results?

## Phase 5: Production Deployment

- **Model Serialization**: Save trained model with versioning (DVC, MLflow)
- **API Development**: FastAPI, gRPC, or cloud-native serving
- **Testing**: Unit tests for preprocessing, integration tests for pipelines
- **Monitoring**: Prediction distribution, inference latency, error rates
- **Drift Detection**: Input distribution shift, output drift, concept drift

## Phase 6: Monitoring & Iteration

Production models degrade. Establish continuous monitoring.

- **Model Performance**: Compare predictions to actuals (with label delay)
- **Data Drift**: Monitor input distributions
- **Business Metrics**: Track impact on business KPIs
- **Retraining Trigger**: Automatic or manual based on thresholds
- **A/B Testing**: Validate improvements before full rollout

## Industry Benchmarks (2025)

- **Training time**: 80% of ML work is data prep and experimentation
- **Model lifespan**: Average production model degrades in 6-12 months
- **Data quality cost**: 20-40% of project time
- **Monitoring overhead**: 15-25% of production ML infrastructure cost
