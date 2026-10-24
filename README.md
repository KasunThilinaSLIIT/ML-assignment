```markdown
# Predicting Credit Card Default: Logistic Regression vs Random Forest

Group assignment for **Fundamentals of Machine Learning**, MSc in Artificial Intelligence,
Sri Lanka Institute of Information Technology (SLIIT).

| Member | Student ID | Contribution |
|---|---|---|
| H.K.T Bamunuge | MS26930022 | <e.g. data cleaning, EDA, logistic regression> |
| C.P Madubashana | MS26929200 | <e.g. feature engineering, random forest, evaluation> |

**Video demo:** https://youtu.be/WDd_jv7EDCc
**GitHub repository:** https://github.com/KasunThilinaSLIIT/ML-assignment.git
**Report:** `ML-assignment-report.pdf`

## Problem
Predict whether a credit card customer will fail to pay next month's bill (binary classification),
and compare a linear model with a tree ensemble on the same data.

## Dataset
**Default of Credit Card Clients**, UCI Machine Learning Repository (Yeh and Lien, 2009)
https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients

* 30,000 customers of a Taiwanese bank, April to September 2005
* 23 predictors (credit limit, demographics, 6 months of repayment status, bill and payment amounts)
* Target: default next month (22.1% positive, imbalanced)
* `data/default of credit card clients.xls` is the original UCI file; `data/credit_default.csv` is the same data converted to CSV.

## Repository structure
```
credit_default_analysis.ipynb   full analysis: EDA, preprocessing, both models, evaluation
data/                           raw dataset (xls) and CSV copy
figures/                        all figures used in the report
ML-assignment-report.pdf        final report (source code in Appendix A)
```

## How to run
```bash
jupyter notebook credit_default_analysis.ipynb
```
Run all cells in order (about 7 minutes, mostly grid search). All randomness is seeded
(`random_state=42`), so results are reproducible.

## Method
* **Preprocessing:** merged undocumented EDUCATION/MARRIAGE codes into "other", dropped ID,
  engineered AVG_BILL, AVG_UTILISATION and PAY_RATIO, one hot encoded nominal features,
  standardised numeric features, all inside a scikit-learn Pipeline to avoid data leakage.
  PCA was explored (17 of 29 components for 95% variance) but not used.
* **Split:** stratified 80/20 train/test.
* **Models:** Logistic Regression (L1/L2) and Random Forest, both with balanced class weights,
  tuned by grid search with 5 fold stratified cross validation on ROC AUC.
* **Evaluation:** accuracy, precision, recall, F1, ROC AUC, confusion matrices, ROC curves,
  and F1 optimal decision thresholds chosen on out of fold training predictions.

## Results (6,000 test customers, 0.5 threshold)
| Model | Accuracy | Precision | Recall | F1 | ROC AUC |
|---|---|---|---|---|---|
| Logistic Regression | 0.682 | 0.370 | 0.624 | 0.464 | 0.711 |
| Random Forest | 0.777 | 0.497 | 0.586 | 0.538 | 0.778 |

The Random Forest performs better overall. The most recent repayment status (PAY_1) is the
strongest predictor in both models. Full discussion, limitations and future work are in the report.

## Tools
Python, pandas, NumPy, scikit-learn, matplotlib, seaborn, Jupyter Notebook
```