# Clinical Risk Flagging — Reducing False Negatives with Logistic Regression

## 1. Business Problem

In clinical laboratories, the worst possible failure is a false negative — a high-risk test that is not flagged for manual review.

Missing such a case can lead to patient harm, legal exposure, and regulatory consequences.

Objective: Build a model that flags test results for manual review, prioritizing high recall to minimize false negatives, even at the cost of increased manual workload.

## 2. Dataset

Source:
UCI Machine Learning Repository — Breast Cancer Wisconsin (Diagnostic) dataset.

Description:
Each row represents a diagnostic test derived from digitized images of cell nuclei.
The dataset contains 30 numeric features describing shape, texture, and structural properties.

Target variable:
flag_for_review  
1 = requires manual review  
0 = does not require review

## 3. Approach

Problem framing:
Binary classification.

Why accuracy was rejected:
Accuracy can appear high while hiding dangerous false negatives. In this context, accuracy is misleading.

Primary metric:
Recall on the positive class (flag_for_review = 1).

## 4. Modeling

Baseline model:
Logistic Regression.

Preprocessing:
- Feature scaling (StandardScaler) due to different feature scales
- Class imbalance handled using class_weight='balanced'

Threshold tuning:
The default probability threshold (0.5) was reduced to 0.3 to increase sensitivity to high-risk cases.

## 5. Results

Baseline (threshold = 0.5):

Confusion matrix:
[[41  1]
 [ 4 68]]

False negatives: 4

After threshold tuning (threshold = 0.3):

Confusion matrix:
[[41  1]
 [ 0 72]]

False negatives: 0

## 6. Model Stability — Cross Validation

5-fold cross-validation recall scores:
[0.9577, 0.9859, 0.9861, 0.9722, 1.0000]

Mean recall ≈ 0.98  
Low variance indicates stable performance.

## 7. Business Interpretation

The model reliably flags high-risk cases for review.
Manual workload increases slightly, but safety is prioritized.
Clinicians retain final decision authority.

## 8. Recommendation

Deploy as a decision-support tool, not an automated decision-maker.
Use threshold = 0.3 and monitor recall continuously.

## 9. Tech Stack

Python  
pandas, numpy  
scikit-learn  
Jupyter Notebook
