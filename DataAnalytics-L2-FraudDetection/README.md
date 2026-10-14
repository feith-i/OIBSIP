# Fraud Detection — Task 3

**Name:** Faith Aipoh
**Track:** Data Analytics
**Level:** 2
**Task:** 3

## Objective
Build a machine learning pipeline to detect fraudulent financial transactions from a heavily imbalanced dataset, addressing class imbalance as a core challenge.

## Dataset
- **Source:** Credit Card Fraud Detection dataset (anonymized, PCA-transformed)
- **Size:** 284,807 transactions
- **Features:** `Time`, `V1`–`V28` (PCA components), `Amount`, `Class` (target: 0 = legitimate, 1 = fraud)
- **Class distribution:** 284,315 legitimate vs. 492 fraudulent transactions (**~0.17% fraud**) — a heavily imbalanced dataset

## Tech Stack
- Python
- pandas
- scikit-learn
- imbalanced-learn (SMOTE)
- matplotlib
- seaborn
- Jupyter Notebook

## Workflow

1. **Class imbalance analysis** — confirmed fraud makes up only 0.173% of transactions.
2. **Exploratory Data Analysis (EDA)**
   - Compared transaction `Amount` distributions for fraud vs. non-fraud.
   - Analyzed fraud frequency by hour of day, revealing sharp fraud spikes at unusual hours.
3. **Why accuracy is misleading** — documented in a markdown cell: a model predicting "not fraud" for everything would score ~99.8% accuracy while catching zero fraud.
4. **Class imbalance handling** — applied **SMOTE** (Synthetic Minority Oversampling Technique) to the training data only, after the train/test split, to avoid data leakage.
5. **Train/test split** — stratified split (80/20) to ensure fraud cases appear proportionally in both sets.
6. **Feature scaling** — applied `StandardScaler` to all features before training Logistic Regression (resolved a solver convergence warning).
7. **Model training** — trained two models on the SMOTE-balanced training data:
   - Logistic Regression
   - Random Forest Classifier
8. **Evaluation** — assessed both models on the original (untouched, imbalanced) test set using Precision, Recall, F1-Score, and AUC-ROC.
9. **Feature importance / coefficient analysis** — compared Random Forest's feature importances with Logistic Regression's coefficients.
10. **Scalability discussion** — outlined what would be needed to handle ~1 million transactions/hour in production.

## Results

| Model | Precision (Fraud) | Recall (Fraud) | F1-Score (Fraud) | AUC-ROC |
|---|---|---|---|---|
| Logistic Regression | 0.0018 | 1.0000 | 0.0036 | 0.9222 |
| Random Forest | 0.8438 | 0.8265 | 0.8351 | 0.9694 |

**Key finding:** Logistic Regression's default decision threshold, calibrated on SMOTE-balanced 50/50 data, was miscalibrated for the real imbalanced test distribution — it flagged the vast majority of transactions as fraud, making it impractical despite a strong AUC-ROC. Random Forest produced a far more usable balance of precision and recall, and is the recommended model from this comparison.

Both models independently ranked the same handful of anonymized features (notably **V14, V17, V10, V12**) as most important, reinforcing that these signals are genuine rather than model-specific artifacts.

## Key Takeaways
- Accuracy is not a meaningful metric for highly imbalanced classification problems.
- Resampling techniques like SMOTE must be applied only to training data, never to the test set.
- A model's decision threshold matters as much as its underlying discriminative power (AUC-ROC) — a high AUC does not guarantee usable predictions at the default threshold.
- Recall is generally prioritized over precision in fraud detection (missing fraud is costlier than a false alarm), but precision still needs to stay high enough for the system to be operationally usable.

## Images

**Transaction Hour Distribution — Fraud vs. Non-Fraud**
![Transaction Hour Distribution](transaction_hour_dist.png)

**Random Forest — Top Feature Importances**
![Random Forest Top Features](random_top_features.png)

**ROC Curve — Logistic Regression vs. Random Forest**
![Logistic Regression vs Random Forest ROC Curve](logistic_vs_random.png)

**Logistic Regression — Top Coefficients**
![Logistic Regression Top Coefficients](logistic_top_coeff.png)

## Files
- `Fraud_Detention.ipynb` — full notebook with code, visualizations, and markdown discussion
