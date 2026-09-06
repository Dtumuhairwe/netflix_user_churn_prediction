# Netflix User Churn Prediction

Predicting subscription cancellations from customer behavior, and investigating why the model performed as well as it did.

---

## The problem

Subscription businesses lose revenue two ways: customers who leave, and money spent retaining customers who were never going to leave. Both are expensive. The question this project addresses is which customers are at risk of cancelling, and which behaviors are associated with that risk.

**Dataset:** 5,000 customer records covering subscription details, engagement, and account history.

---

## Approach

1. Exploratory analysis of engagement, subscription tier, and account activity
2. Feature engineering and encoding
3. Baseline (logistic regression) and comparative (random forest) modeling
4. Investigation of the model's performance through feature importance, ablation, and cross-validation

---

## What the data showed

Churned customers watched 5.9 hours on average against 17.4 for active customers, and last logged in 38 days ago against 22. Churn rose across login-recency bands from 14.8% within the first week to 75.1% at 31-60 days of inactivity.

Customers with both low watch time and a stale login churned at 95.8%, against roughly 50% with either signal alone and 2.1% with neither.

Basic subscribers had a 61.8% churn rate, compared with 45.4% for Standard and 43.7% for Premium, despite similar average watch hours across tiers. The tier difference is not explained by average engagement alone.

Age, region, and device showed little separation.

---

## Model results

| Model | Accuracy | ROC-AUC |
|---|---:|---:|
| Logistic Regression | 89% | 0.966 |
| Random Forest | 99% | 0.999 |

Both models were evaluated on a stratified 20% holdout with precision, recall, and F1 reported for each class.

---

## Investigating the result

A ROC-AUC of 0.999 warranted investigation rather than acceptance.

**Feature importance.** Three engagement variables account for 0.78 of total importance: `avg_watch_time_per_day` (0.393), `watch_hours` (0.197), `last_login_days` (0.186). Subscription tier falls below 0.01 despite the 18-point marginal churn gap seen in EDA.

**Ablation.** Removing any single engagement feature leaves performance high, because the remaining engagement variables retain substantial predictive information. Removing all three drops ROC-AUC from 0.999 to 0.589, close to random ranking.

**Duplicates.** No exact duplicate feature rows were found, so identical repeated observations do not explain the unusually high performance.

**Cross-validation.** Mean ROC-AUC 0.9984 across five folds, standard deviation 0.00037.

---

## Conclusion

The random forest achieves unusually high out-of-sample performance, and ablation shows it is driven almost entirely by three engagement variables. Removing them reduces ROC-AUC to 0.589, much closer to random ranking, meaning demographic, plan, payment, and content-preference features carry little predictive information on their own.

The near-perfect separation produced by the engagement variables is unusual enough that the result should be treated cautiously. It may reflect how this dataset was constructed rather than performance achievable on real customer data. Without documentation of how the churn label and engagement variables were recorded, target leakage cannot be confirmed or ruled out from model performance alone.

**Next step, not built here.** Predicting who is at risk answers half the question. Whether an intervention reduces churn requires a controlled experiment comparing retention between treated and untreated at-risk customers.

---

## Stack

Python · pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Jupyter

---

## Notebooks

- `01_data_exploration.ipynb` — data checks and exploratory analysis
- `02_eda_visualizations.ipynb` — charts and interpretation
- `03_feature_engineering.ipynb` — encoding and train/test split
- `04_model_building.ipynb` — modeling and investigation

---

## What I took from this

A result that flatters you deserves more scrutiny than one that disappoints you. My first explanation for the high performance was target leakage. Ablation, duplicate checks, and cross-validation helped narrow down what was driving the result, but they could not establish whether leakage was present. Without documentation of how the target and engagement variables were generated, that question remains unresolved.
