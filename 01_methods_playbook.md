# Methods Playbook (Detailed) — 3h Tabular ML Hackathon

This playbook is designed for **speed + grading**:
- build a correct end-to-end pipeline fast
- avoid common point-loss (leakage, wrong validation, vague justification)
- document decisions in a way that is easy to paste into your notebook

---

## 0) Standard workflow (the “approved” storyline)

1. **Problem framing**
   - What is the target? What does a “positive” mean?
   - What is the cost of FP vs FN?
   - Which metric matches that cost?

2. **Data sanity + EDA (fast, targeted)**
   - shapes, dtypes, missingness, duplicates
   - target distribution / imbalance
   - at least **2 clear patterns** (feature → target)

3. **Leakage & data availability**
   - remove features that are **not available at prediction time**
   - remove “future” info, post-outcome information, or label proxies

4. **Preprocessing plan**
   - numeric: impute (median), optional scaling
   - categorical: impute (most frequent), one-hot encode
   - keep transformations inside a **single sklearn Pipeline** (no leakage)

5. **Baseline**
   - DummyClassifier + LogisticRegression baseline

6. **Stronger model**
   - RandomForest or Gradient Boosting (XGBoost/GBDT/HistGB)

7. **Evaluation**
   - confusion matrix + ROC/AUC (and/or PR-AUC, F1, recall depending on goal)
   - compare to baseline
   - overfit check (train vs test / CV)

8. **Explainability**
   - global: coefficients / permutation importance
   - relationship: PDP (optional)
   - local: SHAP/LIME for 1–2 samples (optional)

9. **Final selection**
   - choose model with best validation metric **and** acceptable interpretability + stability
   - short “why this model” paragraph tied to the dataset you saw

---

## 1) Model menu (what you can choose from)

### A) Baselines (do these no matter what)
**DummyClassifier**
- Why: sets a floor; reveals class imbalance issues
- Use: `most_frequent` or `stratified`

**Logistic Regression (regularized)**
- Why: fast + interpretable; often strong for tabular
- Best when: linear-ish signal, many one-hot features
- Notes:
  - consider `class_weight="balanced"` if heavy imbalance
  - use `solver="liblinear"` for small/medium data or `saga` for many features

### B) Tree-based (usually best performance for tabular)

**Decision Tree (CART)**
- Why: interpretable single tree baseline
- Risk: overfits quickly; often a stepping stone

**Random Forest**
- Why: strong default; handles non-linearities/interactions with minimal tuning
- Strength: robust; decent out-of-the-box
- Risk: can be slower; probabilities may be less calibrated

**Gradient Boosting (GBDT / XGBoost / AdaBoost)**
- Why: often top performance on tabular tasks
- Strength: captures complex patterns; usually better than RF with tuning
- Risk: can overfit; tuning takes time
- Hackathon tip:
  - start with small tuning: `n_estimators`, `learning_rate`, `max_depth` (or `max_leaf_nodes`)

### C) Others (only if you have a template)
**kNN** (rarely best; scaling sensitive)
**SVM** (can be strong but tuning/time)
**Neural Networks** (usually a time sink for 3h tabular)

---

## 2) Choosing the metric (tie it to the use case)

Use one of these patterns in your notebook:

- If **false negatives are expensive** (missing a default/fraud):
  - focus on **recall**, **F1**, or **PR-AUC** (if very imbalanced)
- If ranking quality matters:
  - **ROC-AUC** is standard and quick
- If decision threshold matters:
  - confusion matrix + choose a threshold for business cost tradeoff

**Minimum to show (safe default):**
- confusion matrix
- ROC curve + AUC
- accuracy (only as a reference, not the headline)

---

## 3) Validation rules (don’t lose points)

### A) Split strategy
- Use `train_test_split(..., stratify=y, random_state=42)` for speed.
- If time: use CV (`StratifiedKFold`) for more stable comparison.

### B) Prevent leakage
- Every transformation must be inside Pipeline/ColumnTransformer
- Don’t fit encoders/imputers on the full dataset before splitting

### C) Overfitting check
- Compare train vs test scores (big gap → overfit)
- Keep hyperparameter tuning minimal and justified

---

## 4) Preprocessing decisions (what to consider & how to justify)

### A) Missing values
- Identify top missing columns
- Strategy:
  - numeric: median impute
  - categorical: most_frequent impute
- Justification text:
  - “Median is robust to outliers; most_frequent preserves frequent categories and keeps pipeline simple.”

### B) Outliers
- In hackathon time: note them, but don’t over-engineer
- Optional: winsorize/clipping for extreme numeric values (only if clearly needed)

### C) Categorical encoding
- One-hot encoding with `handle_unknown="ignore"` prevents crash on unseen categories.

### D) Scaling
- Needed for distance-based models (kNN/SVM) and sometimes logistic regression
- Not needed for tree-based models

### E) Class imbalance
Options to mention:
- `class_weight="balanced"` (fast)
- threshold tuning based on recall/precision
- resampling (only if you have time; can complicate)

---

## 5) “Why we picked this model” — fill-in template (grader-friendly)

> We selected **[MODEL]** because the dataset is **tabular** with **[numeric + categorical]** features and likely **non-linear interactions**. Given the **3-hour constraint**, we prioritized a model that is **robust out-of-the-box** and supports **fast iteration**. We evaluate using **[ROC-AUC / F1 / recall]** due to **[class imbalance / business cost of FN vs FP]**, and we complement performance with **[coefficients / permutation importance / SHAP]** to interpret key drivers.

---

## 6) Explainability “menu” (what to use, when)

### Global explanations (most important)
- **Logistic Regression coefficients**
- **Permutation Feature Importance** (model-agnostic)

### Relationship explanations (optional)
- **PDP** (average effect)
- **ICE** (individual curves)

### Local explanations (optional)
- **LIME**
- **SHAP**

**Hackathon best practice**
- Do **one global** method + optionally **one local** explanation for 1–2 rows.

---

## 7) Minimal tuning recipes (safe defaults)

### Logistic Regression
- try: `C ∈ {0.1, 1, 10}`; `class_weight` None vs balanced

### Random Forest
- try: `n_estimators=300`, `max_depth` None vs 8–12, `min_samples_leaf` 1 vs 5

### Gradient Boosting (HistGB)
- try: `max_depth` 3–6, `learning_rate` 0.05–0.1, `max_iter` 200–500

**Rule:** one quick comparison table beats “I tried 30 settings”.

---

## 8) Red flags (common point-loss)
- No stratified split on imbalanced target
- Doing preprocessing before splitting (leakage)
- Vague justification (“it’s powerful”)
- Only accuracy reported
- Notebook doesn’t run start-to-finish
