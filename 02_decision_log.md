# Decision Log (Detailed) — What we did and why

Write short entries with: **What**, **Why**, **Evidence**.
Time-stamp sections so it looks like a real hackathon workflow.

---

## 0) Context
- Dataset name/version:
- Target variable and definition:
- Positive class meaning:
- Metric(s) we optimize and why:
- Train/test split strategy:

---

## 1) Step-by-step entries

### [00:00–00:10] Read task + define target
**What we did**
- Target: `[target]`
- Positive class: `[...]`

**Why**
- Metric + evaluation depend on the positive class.

**Evidence**
- `y.value_counts()`:
- Notes from data description:

---

### [00:10–00:20] Load data + sanity checks
**What we did**
- Checked shape/dtypes, duplicates, missingness.

**Why**
- Prevent dtype bugs; missingness impacts preprocessing.

**Evidence**
- `df.shape`:
- top missing columns:

---

### [00:20–00:35] EDA: imbalance + two patterns
**What we did**
- Target distribution
- Pattern 1 (numeric): [...]
- Pattern 2 (categorical): [...]

**Why**
- Required by rubric; informs transformations/model choice.

**Evidence**
- Cell references + 1–2 line interpretation:

---

### [00:35–00:45] Leakage + availability check
**What we did**
- Dropped/flagged: [...]
- Kept: [...]

**Why**
- Prevent inflated scores; only use info available at prediction time.

**Evidence**
- Data dictionary snippets/notes:

---

### [00:45–01:05] Preprocessing pipeline
**What we did**
- Numeric: median impute (+ optional scaling)
- Categorical: most_frequent + one-hot (handle_unknown='ignore')
- Built Pipeline + ColumnTransformer

**Why**
- Leak-safe + robust; prevents crashes on unseen categories.

**Evidence**
- Pipeline cell reference:

---

### [01:05–01:20] Baseline models
**What we did**
- DummyClassifier
- LogisticRegression baseline

**Why**
- Floor + fast interpretable baseline.

**Evidence**
- Metrics + confusion matrix:

---

### [01:20–02:05] Strong model + minimal tuning
**What we did**
- Trained `[RF / GradientBoosting / XGBoost]`
- Small param sweep (3–6 settings)
- Selected best by `[metric]`

**Why**
- Tree ensembles capture non-linear patterns; tuning kept minimal for time.

**Evidence**
- Comparison table; train vs test gap:

---

### [02:05–02:30] Evaluation + error analysis
**What we did**
- Confusion matrix + ROC-AUC (optional PR-AUC/F1/recall)
- Discussed which errors matter (FP vs FN)

**Why**
- Accuracy can mislead under imbalance; matrix shows error types.

**Evidence**
- AUC:
- Confusion matrix:

---

### [02:30–02:50] Explainability
**What we did**
- Global: `[coefficients / permutation importance]`
- Optional: PDP/ICE
- Optional: SHAP/LIME for 1–2 samples

**Why**
- Interprets drivers; sanity-checks model behavior.

**Evidence**
- Top features list:

---

### [02:50–03:00] Final selection + polish
**What we did**
- Final model: [...]
- Added markdown “what+why”
- Restart kernel → Run all

**Why**
- Reproducibility and clean submission.

**Evidence**
- Run-all success + final metric summary:
