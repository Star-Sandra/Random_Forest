Maternal Health Risk Prediction Dataset Lab

A notebook that cleans a maternal health dataset, explores it visually, and prepares it for training a Random Forest classifier to predict patient risk level.

Dataset

The dataset (`RandomForest.csv`) contains routine health measurements per patient:

| Column        | Description                          |
|---------------|---------------------------------------|
| `Age`         | Patient age                           |
| `SystolicBP`  | Systolic blood pressure               |
| `DiastolicBP` | Diastolic blood pressure              |
| `BS`          | Blood sugar level                     |
| `BodyTemp`    | Body temperature                      |
| `HeartRate`   | Heart rate                            |
| `RiskLevel`   | Target label: `0` = low, `1` = mid, `2` = high risk |

What the notebook does

1. Reads the raw CSV, checks shape, dtypes, missing values, and duplicates.
2. Drops duplicates and null rows, saves the cleaned dataset as `RandomForestSet.csv`.
3. Creates visuals of the data via charta
   - Pie chart — proportion of patients per risk level
   - Scatterplot — Blood Sugar vs Systolic BP, colored by risk level, sized by age
   - Boxplot — Diastolic BP spread across risk levels
   - Histograms — distribution of each numeric feature (age, BP, blood sugar, body temp, heart rate)


Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
```

Usage

```bash
jupyter notebook
```

Run all cells top to bottom. The cleaned dataset is written to `RandomForestSet.csv` in the working directory.
