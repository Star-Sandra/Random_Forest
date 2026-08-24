# Maternal Health Risk — Random Forest Classifier

Dataset cleaning and processing for the SmartMama project. Predicts a patient's maternal health risk level
(low / mid / high) from vitals, using a Random Forest classifier trained in
`RandomForest.ipynb`.

## Data

- **Source file:** `Maternal_Health_Risk.csv`, produced by `crawler.py`.
 - **Rows:** 1014 raw records, 7 columns (`Age`, `SystolicBP`, `DiastolicBP`,
  `BS`, `BodyTemp`, `HeartRate`, `RiskLevel`).
## What the notebook actually does

Run top to bottom, in this order:

1. **Load** `Maternal_Health_Risk.csv`.
2. **Drop the `HeartRate` column.** It's excluded from the model. This step
   runs *before* de-duplication, since removing a column can turn rows that
   used to differ only by `HeartRate` into exact duplicates.
3. **Clean the data:**
   - Drop exact duplicate rows (574 of 1014 rows are duplicates once
     `HeartRate` is dropped → 440 unique rows remain).
   - Encode `RiskLevel` to numeric: `low risk`→0, `mid risk`→1, `high risk`→2.
4. **Save the cleaned data** to `RandomForestSet_clean.csv` (the raw file is
   never overwritten).
5. **Explore the data:** class balance (pie chart), BS vs SystolicBP scatter,
   diastolic BP by risk (boxplot), per-feature histograms, a correlation
   heatmap, and per-feature boxplots split by risk level.
6. **Split** the cleaned data into train/test sets (80/20, `stratify`d by
   `RiskLevel`, `random_state=42` for reproducibility). Because
   de-duplication already happened, no patient record appears in both splits.
7. **Train** a `RandomForestClassifier` (`n_estimators=200`) on the training
   set only.
8. **Evaluate** on the held-out test set: overall accuracy, a full
   per-class `classification_report` (precision/recall/F1), a confusion
   matrix, and the model's `feature_importances_`.

## Current results

- **Overall test accuracy: 58%**
- **Recall by class:** low risk 76%, mid risk 24%, high risk 55%
- `mid risk` is the weak point — it overlaps with both other classes on most
  vitals (visible in the per-feature boxplots), so the model confuses it most
  often.
- **Feature importances:** `BS` dominates, followed by `Age` and `SystolicBP`;
  `BodyTemp` contributes least (it's near-constant across the dataset).
- Dropping `HeartRate` cost some accuracy (it was 65% with `HeartRate`
  included) — a deliberate trade-off, not a regression.

## Files in this project

| File | Purpose |
|---|---|
| `crawler.py` | Fetches/produces `Maternal_Health_Risk.csv` |
| `Maternal_Health_Risk.csv` | Raw source data |
| `RandomForest.ipynb` | Cleaning, EDA, training, and evaluation |
| `RandomForestSet_clean.csv` | Cleaned, de-duplicated, encoded output |

## Known limitations / next steps

- No baseline model (e.g. Logistic Regression) has been trained yet for
  comparison 
- `mid risk` recall (24%) is the main weakness.
- Hyperparameters (`n_estimators`, `max_depth`) are fixed, not tuned via
  cross-validation.
