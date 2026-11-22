# Confusion Matrix in Machine Learning

## 1. What Is a Confusion Matrix?

A confusion matrix is a table used to evaluate the performance of a classification model.

It compares:
- Actual values: What really happened.
- Predicted values: What the machine learning model predicted.

It helps us understand how many predictions were correct and how many were incorrect.

A binary classification confusion matrix contains four outcomes:

1. True Positive (TP)
2. True Negative (TN)
3. False Positive (FP)
4. False Negative (FN)

## 2. Confusion Matrix Structure

Consider a binary classification problem where a model predicts whether an email is spam.

| | Predicted Positive | Predicted Negative |
|---|---:|---:|
| Actual Positive | True Positive (TP) | False Negative (FN) |
| Actual Negative | False Positive (FP) | True Negative (TN) |

The rows represent actual values, and the columns represent predicted values.

## 3. Understanding TP, TN, FP, FN

### True Positive (TP)

The model predicts positive, and the actual result is also positive.

Example:
- An email is actually spam.
- The model predicts it is spam.

The prediction is correct.

### True Negative (TN)

The model predicts negative, and the actual result is also negative.

Example:
- An email is not spam.
- The model predicts it is not spam.

The prediction is correct.

### False Positive (FP)

The model predicts positive, but the actual result is negative.

Example:
- An email is not spam.
- The model predicts it is spam.

This is an incorrect positive prediction.

It is also called a Type I error.

### False Negative (FN)

The model predicts negative, but the actual result is positive.

Example:
- An email is actually spam.
- The model predicts it is not spam.

This is an incorrect negative prediction.

It is also called a Type II error.

## 4. Numerical Example

Suppose a model classifies 100 emails.

- 40 emails are actually spam.
- 60 emails are not spam.
- The model correctly identifies 30 spam emails.
- The model correctly identifies 50 non-spam emails.
- It incorrectly classifies 10 non-spam emails as spam.
- It misses 10 spam emails.

The confusion matrix:

| | Predicted Spam | Predicted Not Spam |
|---|---:|---:|
| Actual Spam | 30 (TP) | 10 (FN) |
| Actual Not Spam | 10 (FP) | 50 (TN) |

Total records:

TP + TN + FP + FN = 30 + 50 + 10 + 10 = 100.

## 5. Evaluation Metrics from the Confusion Matrix

### Accuracy

Accuracy measures the percentage of total predictions that are correct.

Formula:

Accuracy = (TP + TN) / (TP + TN + FP + FN)

Using the example:

Accuracy = (30 + 50) / 100 = 80%

### Precision

Precision measures how many predicted positive results were actually positive.

Formula:

Precision = TP / (TP + FP)

Using the example:

Precision = 30 / (30 + 10) = 75%

Interpretation: Out of 40 emails predicted as spam, 30 were actually spam.

### Recall (Sensitivity)

Recall measures how many actual positive results the model correctly identified.

Formula:

Recall = TP / (TP + FN)

Using the example:

Recall = 30 / (30 + 10) = 75%

Interpretation: Out of 40 actual spam emails, the model identified 30.

### Specificity

Specificity measures how many actual negative results the model correctly identified.

Formula:

Specificity = TN / (TN + FP)

Using the example:

Specificity = 50 / (50 + 10) = 83.33%

### F1 Score

F1 score is the harmonic mean of precision and recall.

Formula:

F1 = 2 × (Precision × Recall) / (Precision + Recall)

Using the example:

F1 = 2 × (0.75 × 0.75) / (0.75 + 0.75)

F1 = 0.75 or 75%

## 6. Summary of Metrics

| Metric | Formula | What It Measures |
|---|---|---|
| Accuracy | (TP + TN) / Total | Overall correctness |
| Precision | TP / (TP + FP) | Correctness of positive predictions |
| Recall | TP / (TP + FN) | Coverage of actual positives |
| Specificity | TN / (TN + FP) | Coverage of actual negatives |
| F1 Score | 2PR / (P + R) | Balance between precision and recall |
| False Positive Rate | FP / (FP + TN) | Percentage of actual negatives incorrectly flagged |

## 7. Precision vs Recall

Precision and recall measure different aspects of model performance.

### High Precision

The model makes fewer false positive predictions.

Useful when false alarms are expensive.

Example:
- A system flags a transaction as fraudulent.
- A false alarm may unnecessarily block a legitimate transaction.

### High Recall

The model identifies a larger percentage of actual positive cases.

Useful when missing a positive case is expensive.

Example:
- A medical screening system identifies potential disease cases.
- Missing an actual positive case can be serious.

### Trade-off

Increasing recall often increases false positives and may reduce precision.

The appropriate balance depends on the application and the cost of each type of error.

## 8. Why Accuracy Can Be Misleading

Consider a dataset with 1,000 transactions:

- 990 transactions are legitimate.
- 10 transactions are fraudulent.

Suppose a model predicts every transaction as legitimate.

It correctly classifies 990 transactions.

Accuracy = 990 / 1,000 = 99%.

However, the model detects zero fraudulent transactions.

This is why accuracy alone is not sufficient for imbalanced datasets.

Other useful metrics include:
- Precision
- Recall
- F1 score
- Precision-Recall AUC
- ROC-AUC

## 9. Python Example Using Scikit-Learn

```python
from sklearn.metrics import (
    confusion_matrix,
    accuracy_score,
    precision_score,
    recall_score,
    f1_score,
    classification_report
)

# Actual labels
y_actual = [1, 1, 1, 1, 0, 0, 0, 0]

# Model predictions
y_predicted = [1, 1, 0, 1, 1, 0, 0, 0]

# Generate confusion matrix
cm = confusion_matrix(y_actual, y_predicted)

print("Confusion Matrix:")
print(cm)

# Evaluation metrics
print("Accuracy:", accuracy_score(y_actual, y_predicted))
print("Precision:", precision_score(y_actual, y_predicted))
print("Recall:", recall_score(y_actual, y_predicted))
print("F1 Score:", f1_score(y_actual, y_predicted))

print(classification_report(y_actual, y_predicted))
```

Expected confusion matrix:

```text
[[3 1]
 [1 3]]
```

For scikit-learn's binary classification matrix, the layout is:

```text
[[TN  FP]
 [FN  TP]]
```

Therefore:
- TN = 3
- FP = 1
- FN = 1
- TP = 3

Note: The positive class is assumed to be label `1`.

## 10. Visualizing the Confusion Matrix

You can use Seaborn and Matplotlib to visualize the results.

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.heatmap(
    cm,
    annot=True,
    fmt="d",
    cmap="Blues",
    xticklabels=["Negative", "Positive"],
    yticklabels=["Negative", "Positive"]
)

plt.xlabel("Predicted")
plt.ylabel("Actual")
plt.title("Confusion Matrix")
plt.show()
```

## 11. Multi-Class Confusion Matrix

A confusion matrix can also evaluate models with more than two classes.

Example: An image classification model predicts whether an image contains a cat, dog, or bird.

| Actual / Predicted | Cat | Dog | Bird |
|---|---:|---:|---:|
| Cat | 40 | 5 | 2 |
| Dog | 3 | 35 | 4 |
| Bird | 1 | 2 | 38 |

The diagonal values represent correct predictions.

Values outside the diagonal represent classification errors.

For multi-class classification, precision, recall, and F1 can be calculated for each class and then averaged using:
- Macro average: Equal weight for every class.
- Micro average: Aggregates total TP, FP, and FN across classes.
- Weighted average: Weights each class according to its number of actual samples.

## 12. Important Concepts

| Concept | Meaning |
|---|---|
| TP | Predicted positive and actually positive |
| TN | Predicted negative and actually negative |
| FP | Predicted positive but actually negative |
| FN | Predicted negative but actually positive |
| Type I Error | False Positive |
| Type II Error | False Negative |
| Sensitivity | Recall |
| Specificity | Correctly identifying negative cases |
| Imbalanced Dataset | One class has significantly more records than another |

## 13. Quick Revision

- A confusion matrix evaluates classification model predictions.
- TP and TN are correct predictions.
- FP and FN are incorrect predictions.
- Accuracy measures overall correctness.
- Precision measures the correctness of positive predictions.
- Recall measures how many actual positive cases were identified.
- Specificity measures how many actual negative cases were identified.
- F1 score balances precision and recall.
- Accuracy can be misleading when the dataset is imbalanced.
- The diagonal of a multi-class confusion matrix represents correct predictions.
