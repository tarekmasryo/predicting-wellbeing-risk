# 🌌 Digital Habits & Mental Health — Wellbeing Risk

Production-minded notebook for **binary wellbeing risk** (`high_risk_flag ∈ {0,1}`) derived from digital behavior signals.

---

## 🎯 Goal
Predict a **calibrated wellbeing risk score** in `[0,1]` from daily digital patterns — screen time, unlocks, sleep, notifications, activity.

Focus:
- Stable CV, not leaderboard luck  
- Minimal, meaningful visuals  
- Zero leakage · Deployable artifacts  

---

## 🧠 End-to-End Pipeline
1. **Data Loading** → clean read, column sanitization, dedup, schema snapshot  
2. **EDA (Focused)** → leakage guard · top Spearman correlations · compact Plotly visuals  
3. **Feature Engineering** → ratios & interactions · winsorized z-scores · log1p transforms  
4. **Models** → Logistic Regression · Random Forest · XGBoost (auto GPU/CPU)  
5. **Calibration** → isotonic reliability fit on the best model  
6. **Decisioning** → min-cost threshold + capacity constraint (optional review rate)  
7. **Interpretability** → AUC-based permutation importance (Kaggle-safe)  
8. **Export** → calibrated model + schema + metadata + inference smoke test  

---

## 🔬 Features & Cross-Validation
**Key features (examples):**
- `screen_per_sleep_norm` = device_hours_per_day / sleep_hours  
- `unlocks_per_hour` = phone_unlocks / (device_hours_per_day + 0.1)  
- `social_ratio`, `work_ratio` from time splits  
- Winsorized z-scores for device_hours, sleep_hours, stress, anxiety, depression, happiness  
- log1p transforms for unlocks, social_media_minutes, work_minutes, notifications_per_day  


---

## 📈 Metrics & Visualization
- ROC–AUC · Average Precision · Brier Score  
- Comparative ROC/PR curves (all models)  
- Reliability curve (calibrated best) · Lift curve  
- AUC-based permutation importance for interpretability  

---

## 🧮 Cost-Aware Decisioning
Threshold grid `t ∈ [0,1]` using:
```
total_cost = FP*C_fp + FN*C_fn - TP*benefit
```
- Report **min-cost** and **best-F1** thresholds  
- Optional **capacity constraint** → choose lowest-cost threshold under review limit  

---

## 📂 Outputs
All reproducible artifacts stored under `artifacts/`:

```
eda/        → schema_overview.csv, engineered_corr.csv
metrics/    → leaderboard_metrics.csv, permutation_importance_auc.csv
decision/   → decision_cost_curve.csv, thresholds_summary.csv
models/     → best_calibrated.joblib, feature_schema.json, model_info.json
inference/  → smoke_test_report.json
```

---

## ⚡ Quick Start
```bash
pip install -r requirements.txt

```
Plotly renderer:
```python
import plotly.io as pio
pio.renderers.default = "notebook_connected"  # auto PNG fallback (kaleido)
```

---

## 🗂️ Dataset
- **Target:** `high_risk_flag` (0/1)  
  If categorical → `TARGET_MAP = {"Low":0, "High":1}`  
- **Default path:** `/kaggle/input/digital-health-and-mental-wellness/Data.csv`

---

## 📁 Repo layout

```text
.
├── predicting-wellbeing-risk.ipynb
├── data/
│   └── raw/               # put Data.csv here for local runs
├── artifacts/             # exported tables / metrics / model files
├── repo_utils/
│   └── pathing.py         # local + Kaggle path helpers
├── CASE_STUDY.md
├── requirements.txt
└── .gitignore
```

---

## 🧭 Data loading (local + Kaggle)

**Local (recommended):**
- Put `Data.csv` under `data/raw/`

**Kaggle:**
- Falls back to `/kaggle/input/digital-health-and-mental-wellness/Data.csv`

**Optional override:**
- Set `DATA_PATH` to a full file path (local runs).

---

## 🧾 Case Study
See **CASE_STUDY.md** for the project story, decisions, and takeaways.
