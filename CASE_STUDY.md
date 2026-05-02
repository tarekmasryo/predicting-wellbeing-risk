# 🌌 Case Study — Predicting Reported Wellbeing Risk

## 1. Problem

This project builds a reproducible binary-classification workflow for `high_risk_flag` using a digital wellbeing dataset.

The goal is not to build a clinical screening system. The goal is to show a clean machine-learning workflow that can:

- validate the dataset structure
- inspect potential leakage risks
- compare feature scopes without touching the final test split
- train leak-safe model pipelines
- calibrate predicted probabilities
- select a decision threshold on validation data
- report final performance on a hold-out test split
- export reusable model and reporting artifacts

---

## 2. Dataset scope

The dataset can include a mix of:

- digital behavior signals
- lifestyle/context variables
- self-reported wellbeing indicators
- a binary reported-risk label, `high_risk_flag`

Because the domain touches wellbeing and mental-health-adjacent indicators, the notebook includes a clear scope note: results are for educational analytics and ML practice only, not medical advice or diagnosis.

---

## 3. Technical approach

### Validation and leakage review

The notebook checks:

- schema and column types
- missingness
- duplicates
- target balance
- ID-like columns
- high-cardinality categorical columns
- target-name leakage candidates
- strong target correlations for review

### Feature-scope validation

The notebook compares signal groups on the training split only. This keeps the final test split reserved for final hold-out evaluation and avoids mixing exploratory diagnostics with final test reporting.

### Modeling

The workflow uses sklearn-compatible pipelines with preprocessing handled inside the pipeline to reduce leakage risk.

Candidate models are evaluated with cross-validation on the training split, then the selected model is calibrated and evaluated on validation/test splits according to the notebook workflow.

### Metrics

The notebook reports:

- ROC AUC
- Average Precision
- Brier score
- Expected Calibration Error
- precision / recall / F1 at the selected operating threshold
- review rate at the selected policy

---

## 4. Decision policy

The operating threshold is selected on validation data only.

The final policy uses a review-rate cap so the selected threshold is not just mathematically favorable, but also operationally plausible.

Final reported test policy in the current notebook output:

| Metric | Value |
|---|---:|
| Threshold | `0.1931` |
| Precision | `0.6406` |
| Recall | `0.5816` |
| Review rate | `18.29%` |

This keeps the final policy below the configured `20%` review-rate cap.

---

## 5. Artifacts

The notebook exports:

- calibrated model as `joblib`
- feature schema as `JSON`
- model metadata as `JSON`
- validation threshold report as `CSV`
- final test policy metrics as `CSV`
- inference smoke-test report as `JSON`
- interactive Plotly figures as `HTML`

The smoke test reloads the saved model, scores sample rows, applies the selected threshold, and asserts that outputs are finite and schema-compatible.

---

## 6. Result interpretation

The final model is best understood as a **ranking baseline**: it separates higher-risk and lower-risk profiles in this dataset under the available feature set.

It should not be interpreted as a real-world diagnostic or screening tool without external validation, consent, privacy safeguards, subgroup analysis, monitoring, and domain governance.

---

## 7. Next steps

Reasonable next improvements would be:

- keep target-label generation documented in the dataset card
- validate on fresh data from a different time period or source
- add subgroup performance review for relevant demographic/context segments
- add drift monitoring for future batches
- package inference behind a small API only after governance requirements are clear
