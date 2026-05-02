# 🌌 Predicting Reported Wellbeing Risk from Digital & Lifestyle Signals

A reproducible machine-learning notebook for modeling `high_risk_flag` from digital behavior, lifestyle/context variables, and self-reported wellbeing indicators.

This repository is designed as a **Kaggle-ready portfolio artifact**: it validates the dataset, explores risk patterns, compares feature scopes on the training split, trains leak-safe model pipelines, calibrates probabilities, selects an operating threshold on validation data, evaluates once on the final hold-out test split, and exports reusable artifacts.

> **Scope note:** This project is for educational analytics and machine-learning practice only. It is not medical advice, clinical diagnosis, mental-health screening, or a treatment recommendation.

---

## ✨ What this repo provides

- ✅ **Publish-ready Kaggle notebook** with saved outputs and a clear end-to-end workflow
- 🔎 **Data validation** for schema, missingness, duplicates, target balance, and leakage signals
- 📊 **Focused EDA** using compact Plotly visuals and interpretable summaries
- 🧪 **Leak-safe modeling** with clear train / validation / test separation
- 🧭 **Feature-scope validation on the training split only** to avoid using the final test split for diagnostics
- 🎯 **Validation-selected thresholding** with a review-rate cap before final test evaluation
- 📈 **Calibrated classification metrics**: ROC AUC, Average Precision, Brier score, and calibration error
- 🧰 **Reusable artifacts**: model, schema, metadata, threshold reports, metrics, and smoke-test report
- 🧯 **Inference smoke test** that reloads the saved model, scores sample rows, and applies the selected threshold

---

## 🧠 Notebook workflow

1. **Configuration and imports**
   - Reproducible seed, artifact folders, model settings, and decision-policy assumptions.

2. **Helper functions**
   - Robust dataset path discovery, display helpers, preprocessing builders, calibration utilities, threshold metrics, and target coercion.

3. **Data loading and validation**
   - Column sanitization, duplicate removal, schema snapshot, target checks, and target distribution review.

4. **Leakage and feature-scope review**
   - ID-like fields, constant columns, high-cardinality categoricals, target-name leakage candidates, and correlation review.

5. **Exploratory analysis**
   - Target distribution, numeric/categorical risk patterns, and compact visual summaries.

6. **Leakage-safe row-wise feature engineering**
   - Deterministic features only; no target leakage or split-level statistics.

7. **Modeling and validation strategy**
   - Stratified train / validation / test split.
   - Cross-validation leaderboard over the training split.
   - Model selection using a declared selection metric.

8. **Feature-scope validation**
   - Compares digital-behavior features against the full indicator set using the training split only.
   - Keeps the final test split reserved for final hold-out evaluation.

9. **Calibration and final hold-out evaluation**
   - Probability calibration and final reporting on the hold-out test split.

10. **Decision thresholding**
    - Threshold search on validation only.
    - Final operating policy respects the review-rate cap and is then applied once to the test split.

11. **Artifacts and smoke test**
    - Saves the model, feature schema, metadata, metrics, decision reports, and smoke-test JSON.

---

## 📊 Final policy snapshot

The executed notebook reports this validation-selected operating policy:

| Item | Value |
|---|---:|
| Operating threshold | `0.1931` |
| Test precision | `0.6406` |
| Test recall | `0.5816` |
| Test review rate | `18.29%` |
| Review-rate cap | `20%` |

These values are saved from the current notebook output and should be regenerated if the data, features, model settings, or random seed change.

---

## 📦 Repository layout

```text
.
├── predicting-wellbeing-risk.ipynb   # Main Kaggle-ready notebook
├── data/
│   └── raw/                          # Optional local Data.csv location
├── artifacts/                        # Generated model, metrics, figures, and reports
├── CASE_STUDY.md                     # Project summary and decision notes
├── requirements.txt                  # Local Python dependencies
├── LICENSE
└── .gitignore
```

---

## 🗂️ Dataset

Expected target column:

```text
high_risk_flag
```

Default Kaggle-style file name:

```text
Data.csv
```

Local run option:

```text
data/raw/Data.csv
```

Optional environment override:

```text
DWELL_DATA=/full/path/to/Data.csv
```

---

## 🚀 Run locally

Create a virtual environment, then install dependencies:

```bash
pip install -r requirements.txt
```

Then open and run:

```text
predicting-wellbeing-risk.ipynb
```

For Kaggle, attach the dataset and run the notebook normally. The notebook searches common Kaggle input paths automatically.

---

## 🧾 Generated artifacts

The notebook writes generated outputs under `artifacts/`:

```text
artifacts/
├── decision/
│   ├── threshold_report_validation.csv
│   ├── decision_summary_validation.csv
│   └── final_policy_test_metrics.csv
├── eda/
│   ├── schema_snapshot.csv
│   ├── target_distribution.csv
│   ├── leakage_review.json
│   └── engineered_feature_quality.csv
├── figures/
│   └── *.html
├── metrics/
│   ├── feature_inventory.csv
│   ├── feature_scope_comparison.csv
│   ├── model_leaderboard.csv
│   ├── calibrated_validation_metrics.csv
│   ├── calibrated_test_metrics.csv
│   └── permutation_importance.csv
└── models/
    ├── best_calibrated_model.joblib
    ├── feature_schema.json
    ├── model_metadata.json
    └── inference_smoke_test.json
```

Generated artifacts are ignored by Git by default so the repo stays lightweight.

---

## 🧭 Practical interpretation

The model should be treated as a **ranking baseline** for this dataset, not as a real-world wellbeing or mental-health screening system.

Any real deployment would require:

- informed consent and privacy safeguards
- domain review and governance
- subgroup analysis and bias checks
- external validation on fresh data
- monitoring, drift checks, and human review
- explicit threshold-governance rules

---

## 📌 Status

✅ **Ready for Kaggle publishing**  
✅ **Ready for GitHub portfolio use**  
✅ **Feature-scope comparison uses the training split only**  
✅ **Threshold selection uses validation only**  
✅ **Final test split is reserved for final evaluation**  
✅ **Smoke test included and executed in the notebook**

---

## 📄 License

This repository is released under the MIT License. See [`LICENSE`](LICENSE).
