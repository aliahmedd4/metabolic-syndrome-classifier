# Metabolic Syndrome Classifier

A machine learning project comparing **Naive Bayes** and **k-Nearest Neighbours (kNN)** for predicting metabolic syndrome risk from clinical and demographic features.

---

## Problem Statement

Metabolic syndrome is a cluster of conditions (elevated blood pressure, high blood sugar, excess abdominal fat, abnormal cholesterol levels) that together raise the risk of heart disease, stroke, and type 2 diabetes. Early identification is critical.

This project trains binary classifiers to predict whether a patient is **At Risk (1)** or **Not At Risk (0)** of metabolic syndrome using the NHANES-derived dataset from the Kaggle competition.

---

## Dataset

Source: [Kaggle — Metabolic Syndrome Prediction](https://www.kaggle.com/competitions/metabolic-syndrome-prediction)

- **Train:** 1,920 samples × 14 features  
- **Test:** 481 samples  
- **Target:** `MetabolicSyndrome` (0 = No Risk, 1 = At Risk)  
- **Features:** Age, Sex, Marital status, Income, Race, WaistCirc, BMI, Albuminuria, UrAlbCr, UricAcid, BloodGlucose, HDL, Triglycerides  

> Datasets are not included in this repository (excluded via `.gitignore`). Download them directly from the Kaggle competition page linked above.

---

## Approach

### Why Naive Bayes and kNN?

These two algorithms represent fundamentally different learning philosophies and serve as a natural comparison:

| | Naive Bayes | kNN |
|---|---|---|
| **Type** | Probabilistic, generative | Instance-based, lazy |
| **Assumption** | Feature independence given class | Local similarity (distance) |
| **Scaling needed** | No | Yes (Euclidean/Manhattan distance) |
| **Interpretability** | High | Moderate |

**Naive Bayes** is fast and provides well-calibrated probability estimates, but the conditional independence assumption is violated in this dataset — medical features like BMI, WaistCirc, BloodGlucose, and Triglycerides are clinically correlated.

**kNN** makes no distributional assumptions and relies purely on local similarity in feature space, which better captures these inter-feature relationships.

### Preprocessing

- **Numerical features:** Median imputation
- **Categorical features:** Mode imputation → OneHotEncoding
- **kNN only:** StandardScaler (distance-based algorithms require feature scaling)

### Hyperparameter Tuning (Validation Set)

- **Naive Bayes:** `var_smoothing` ∈ {1e-9, 1e-7, 1e-5, 1e-3, 1e-1} → best: `1e-7`
- **kNN:** k ∈ {3, 5, 7, 11, 15} × metric ∈ {euclidean, manhattan} → best: k=5, euclidean

---

## Results

### Validation Set Performance

| Model | Accuracy | Precision | Recall | Specificity | F1 Score | ROC-AUC |
|---|---|---|---|---|---|---|
| Naive Bayes | 0.7891 | 0.7604 | 0.5573 | 0.9091 | 0.6432 | **0.8850** |
| **kNN** | **0.8021** | 0.7523 | **0.6260** | 0.8933 | **0.6833** | 0.8581 |

**Selected model:** kNN (k=5, Euclidean) — higher F1 and Recall, which matters in a medical context where missing an at-risk patient is more costly than a false alarm.

### Kaggle Leaderboard

| Submission | Score (AUC) |
|---|---|
| kNN (k=5, Euclidean) | **~0.766** |

---

## Running Locally

```bash
# 1. Clone the repository
git clone https://github.com/aliahmedd4/metabolic-syndrome-classifier.git
cd metabolic-syndrome-classifier

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download the dataset
#    Go to https://www.kaggle.com/competitions/metabolic-syndrome-prediction
#    Download train.csv and test.csv and place them in the project root

# 4. Open and run the notebook
jupyter notebook project2.ipynb
```

---

## Contributors

| Name | GitHub |
|---|---|
| Ali Sherif | — |
| Ahmed Rashad | [@aliahmedd4](https://github.com/aliahmedd4) |
| Asser Ehab | — |

---

*Course: Introduction to Data Science, Spring 2026*
