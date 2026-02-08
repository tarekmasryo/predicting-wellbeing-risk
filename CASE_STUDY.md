# 🧠 Case Study — Digital Habits & Mental Health (Wellbeing Risk)

## Problem
Build a **binary wellbeing risk** model from digital behavior signals and export a **calibrated risk score** in `[0, 1]` for downstream decisioning.

- **Goal:** predict `high_risk_flag` reliably (not leaderboard luck).
- **Output:** calibrated probability + decision thresholds (cost-aware, capacity-aware).

---

## Data
- **File:** `Data.csv`
- **Grain:** one row per person/day (digital behavior snapshot)
- **Target:** `high_risk_flag` ∈ {0,1} (or mapped to 0/1)

Quality and guardrails:
- column sanitization + duplicate removal
- leakage scan by correlation and near-ID checks

---

## Approach
- **EDA (focused):** class balance, distributions, correlations, quick leakage checks
- **Feature engineering:** ratios, interactions, winsorized z-scores, log1p transforms
- **Models:** Logistic Regression, Random Forest, XGBoost (CPU/GPU aware)
- **Evaluation:** stratified CV + holdout, ROC–AUC / AP / Brier score
- **Calibration:** isotonic calibration on the best model
- **Decisioning:** threshold search for min-cost + optional review-capacity constraint
- **Interpretability:** AUC-based permutation importance (Kaggle-safe)

---

## Results
This notebook reports:
- model comparison table (CV + holdout)
- calibration and reliability curves
- decision cost curve and threshold summary
- permutation importance ranking

---

## Artifacts
Saved under `artifacts/`:
- schema snapshots and EDA tables
- metrics tables and decision thresholds
- best calibrated model + schema + metadata
- inference smoke-test report

---

## Next Steps
- add a small `src/` inference helper (pure Python) if you want API-ready usage
- add a simple drift check on a future batch (feature ranges + PSI)
