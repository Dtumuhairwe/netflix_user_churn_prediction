# Netflix User Churn Prediction

Predicting subscription cancellations from customer behavior.

---

## The problem

Subscription businesses lose revenue two ways: customers who leave, and money spent retaining customers who were never going to leave. Both are expensive. The question this project answers is which customers are at risk of cancelling, and which behaviors signal it early enough to act on.

**Dataset:** 5,000 customer records covering subscription details, engagement, and account history.

---

## Approach

1. Exploratory analysis of engagement, tenure, subscription tier, and account activity
2. Feature engineering and encoding
3. Baseline and comparative modeling
4. Validation, leakage investigation, and correction
5. Evaluation on the corrected model

---

## What the data showed

Declining engagement and longer gaps since last login were the strongest behavioral signals of churn. Pricing tier was a weaker predictor than expected. Customers were not primarily leaving because of cost, they were leaving after they stopped using the service, which is a different problem with a different intervention.

---

## The leakage problem

The initial Random Forest returned **97.7% accuracy**.

That result was implausibly high for churn prediction, so I investigated rather than reported it. Examining feature importances showed a small number of variables dominating the model. Tracing those fields back to how they were recorded showed at least one was only populated after a customer had already cancelled.

The model was not predicting churn. It was reading it.

I removed the leakage-prone features and rebuilt the evaluation from the start rather than patching the existing one.

---

## Results after correction

| Model | Accuracy | Precision (churn) | Recall (churn) | F1 (churn) | ROC-AUC |
|---|---|---|---|---|---|
| Logistic Regression | — | — | — | — | — |
| Random Forest (with leakage) | 97.7% | — | — | — | — |
| **Random Forest (corrected)** | **74.6%** | — | — | — | — |

*Fill these from your notebook output.*

After removing leakage-prone features, the Random Forest achieved **74.6% test accuracy**, providing a more realistic estimate of performance on unseen customers.

**Confusion matrix (corrected model):**

|  | Predicted: Stay | Predicted: Churn |
|---|---|---|
| **Actual: Stay** | — | — |
| **Actual: Churn** | — | — |

For churn, recall on the positive class matters more than overall accuracy. A model that misses churners is not useful for retention, regardless of how accurate it looks overall.

---

## What this would support

Customers showing declining engagement and longer periods since their last login are more likely to churn. These signals can identify at-risk subscribers early enough to prioritize retention effort, and the model's ranked probabilities allow that effort to be directed rather than applied broadly.

**Next step, not yet built:** predicting who is at risk answers only half the question. Whether an intervention actually reduces churn requires a controlled experiment comparing retention rates between treated and untreated at-risk customers. That is a separate piece of work and I have not done it here.

---

## Stack

Python · pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Jupyter

---

## Running it

```bash
pip install -r requirements.txt
jupyter notebook netflix_churn.ipynb
```

---

## What I took from this

The moment to investigate a result is when it flatters you. A weak result gets scrutinized automatically; a strong one gets accepted. Tracing a variable back to how it came to exist, before trusting what it appears to show, is now part of how I validate any model.
