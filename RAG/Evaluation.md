# Model Evaluation Metrics – Precision, Recall, Accuracy, F1 Score and Confidence

## 1. What is Model Evaluation?

Model evaluation is the process of measuring how well a machine learning model performs on unseen data.

It helps determine whether a model makes correct predictions and whether its performance is acceptable.

Common evaluation metrics:

1. Accuracy
2. Precision
3. Recall
4. F1 Score
5. Confusion Matrix
6. Confidence Score

## 2. What is a Confusion Matrix?

A confusion matrix is a table used to evaluate a classification model.

It compares the actual labels with the model's predicted labels.

There are four important outcomes:

1. True Positive (TP)
2. True Negative (TN)
3. False Positive (FP)
4. False Negative (FN)

### Example: Email Spam Detection

Suppose a machine learning model predicts whether an email is Spam or Not Spam.

| Actual Result | Predicted Result | Outcome |
|---|---|---|
| Spam | Spam | True Positive |
| Not Spam | Not Spam | True Negative |
| Not Spam | Spam | False Positive |
| Spam | Not Spam | False Negative |

### 2.1 True Positive (TP)

The model predicts Positive, and the actual result is also Positive.

Example:

- Actual email: Spam.
- Model prediction: Spam.
- Result: True Positive.

The model correctly identified spam.

### 2.2 True Negative (TN)

The model predicts Negative, and the actual result is also Negative.

Example:

- Actual email: Not Spam.
- Model prediction: Not Spam.
- Result: True Negative.

The model correctly identified a legitimate email.

### 2.3 False Positive (FP)

The model predicts Positive, but the actual result is Negative.

Example:

- Actual email: Not Spam.
- Model prediction: Spam.
- Result: False Positive.

The model incorrectly classified a legitimate email as spam.

This is also called a Type I error.

### 2.4 False Negative (FN)

The model predicts Negative, but the actual result is Positive.

Example:

- Actual email: Spam.
- Model prediction: Not Spam.
- Result: False Negative.

The model failed to identify a spam email.

This is also called a Type II error.

## 3. Confusion Matrix Example

Suppose we have 100 emails.

The model produces these results:

- 40 spam emails correctly identified.
- 30 legitimate emails correctly identified.
- 20 legitimate emails incorrectly identified as spam.
- 10 spam emails incorrectly identified as legitimate.

Confusion matrix:

| Actual / Predicted | Spam | Not Spam |
|---|---:|---:|
| Actual Spam | TP = 40 | FN = 10 |
| Actual Not Spam | FP = 20 | TN = 30 |

Total = 100 emails.

Remember:

- TP = 40
- TN = 30
- FP = 20
- FN = 10

## 4. What is Accuracy?

Accuracy measures the percentage of total predictions that are correct.

Formula:

Accuracy = (TP + TN) / (TP + TN + FP + FN)

Using our example:

Accuracy = (40 + 30) / 100

Accuracy = 70 / 100

Accuracy = 70%

Meaning:

The model correctly classified 70% of all emails.

### When to use Accuracy?

Accuracy is useful when:
- Classes are reasonably balanced.
- False positives and false negatives have similar costs.
- Overall correctness is important.

Limitation:

Accuracy can be misleading when the dataset is highly imbalanced.

Example:

Suppose 990 transactions are legitimate and only 10 are fraudulent.

A model predicts every transaction as legitimate.

Accuracy = 990 / 1000 = 99%

However, the model detects zero fraudulent transactions.

Therefore, 99% accuracy does not necessarily mean the model is good.

## 5. What is Precision?

Precision measures how many of the positive predictions made by the model were actually positive.

Formula:

Precision = TP / (TP + FP)

Using our spam example:

TP = 40

FP = 20

Precision = 40 / (40 + 20)

Precision = 40 / 60

Precision = 66.67%

Meaning:

Out of all emails classified as spam, approximately 66.67% were actually spam.

### When is Precision important?

Precision is important when false positives are costly.

Examples:

- Spam detection: Avoid marking legitimate emails as spam.
- Fraud detection: Avoid unnecessarily blocking legitimate transactions.
- Content moderation: Avoid incorrectly flagging acceptable content.

Remember:

Precision answers:

**Out of everything the model predicted as Positive, how many were actually Positive?**

## 6. What is Recall?

Recall measures how many of the actual positive cases were correctly identified by the model.

It is also called Sensitivity or True Positive Rate.

Formula:

Recall = TP / (TP + FN)

Using our spam example:

TP = 40

FN = 10

Recall = 40 / (40 + 10)

Recall = 40 / 50

Recall = 80%

Meaning:

Out of all actual spam emails, the model successfully identified 80%.

### When is Recall important?

Recall is important when false negatives are costly.

Examples:

- Fraud detection: Identify as many fraudulent transactions as possible.
- Medical screening: Identify as many potentially positive cases as possible.
- Security threat detection: Detect as many real threats as possible.

Remember:

Recall answers:

**Out of everything that was actually Positive, how many did the model identify correctly?**

## 7. What is F1 Score?

F1 Score is the harmonic mean of Precision and Recall.

It combines both metrics into a single score.

Formula:

F1 = 2 × (Precision × Recall) / (Precision + Recall)

Using our example:

Precision = 0.6667

Recall = 0.80

F1 = 2 × (0.6667 × 0.80) / (0.6667 + 0.80)

F1 ≈ 0.7273

F1 Score ≈ 72.73%

Meaning:

The F1 score balances precision and recall.

### Why use F1 Score?

- Useful when both false positives and false negatives matter.
- Helpful with imbalanced datasets.
- Provides a single measure combining precision and recall.

Important:

F1 does not include True Negatives directly.

## 8. Differences Between Accuracy, Precision, Recall and F1 Score

| Metric | Formula | What it Measures |
|---|---|---|
| Accuracy | (TP + TN) / Total | Overall correctness |
| Precision | TP / (TP + FP) | Correctness of positive predictions |
| Recall | TP / (TP + FN) | Coverage of actual positive cases |
| F1 Score | 2PR / (P + R) | Balance between precision and recall |

### Easy way to remember

- Accuracy: How many predictions were correct overall?
- Precision: When the model says Positive, how often is it correct?
- Recall: How many actual Positive cases did the model find?
- F1 Score: How well are precision and recall balanced?

## 9. Precision vs Recall – Important Difference

Consider a fraud detection system.

### Scenario A: High Precision

The model identifies 10 transactions as fraudulent.

Out of those 10:
- 9 are actually fraudulent.
- 1 is legitimate.

Precision = 9 / 10 = 90%

This means most flagged transactions are genuinely fraudulent.

However, the model might miss many other fraudulent transactions.

### Scenario B: High Recall

Suppose there are 100 actual fraudulent transactions.

The model identifies 95 of them.

Recall = 95 / 100 = 95%

This means the model identifies most fraudulent transactions.

However, it might also incorrectly flag many legitimate transactions.

### Main difference

| High Precision | High Recall |
|---|---|
| Fewer false positives | Fewer false negatives |
| Positive predictions are more reliable | More actual positives are detected |
| Avoids unnecessary alerts | Avoids missing important cases |

## 10. What is a Confidence Score?

A confidence score represents how strongly a model supports a particular prediction.

For example, a classification model might return:

```text
Prediction: Spam
Confidence: 0.95
```

This means the model assigns a score of 95% to the Spam class.

Another example:

```text
Prediction: Not Spam
Confidence: 0.72
```

The model assigns a score of 72% to the Not Spam class.

Important:

A confidence score is not automatically the same as the actual probability that the prediction is correct.

A model is well-calibrated when predictions assigned a confidence of approximately 80% are correct approximately 80% of the time over a sufficiently large set of comparable predictions.

### Confidence score vs Accuracy

| Confidence Score | Accuracy |
|---|---|
| Associated with an individual prediction | Measured across a dataset |
| Indicates model-estimated certainty | Measures actual correctness |
| Can be high for an incorrect prediction | Evaluated using actual labels |
| May need calibration | Calculated from labeled results |

### What is Confidence Threshold?

A confidence threshold is a cutoff used to decide whether a prediction should be accepted automatically.

Example:

```python
confidence = 0.92
threshold = 0.80

if confidence >= threshold:
    print("Accept prediction")
else:
    print("Send for further review")
```

The threshold should be selected based on the cost of different types of errors and validated on representative data.

## 11. What is a False Positive Rate (FPR)?

False Positive Rate measures how often actual negative cases are incorrectly classified as positive.

Formula:

FPR = FP / (FP + TN)

Using our email example:

FP = 20

TN = 30

FPR = 20 / (20 + 30)

FPR = 40%

Meaning:

40% of legitimate emails were incorrectly classified as spam.

## 12. What is True Positive Rate (TPR)?

True Positive Rate measures how many actual positive cases were correctly identified.

Formula:

TPR = TP / (TP + FN)

This is the same as Recall.

Using our example:

TPR = 40 / (40 + 10)

TPR = 80%

## 13. What is Specificity?

Specificity measures how many actual negative cases were correctly identified.

Formula:

Specificity = TN / (TN + FP)

Using our example:

Specificity = 30 / (30 + 20)

Specificity = 60%

Meaning:

The model correctly identifies 60% of legitimate emails.

## 14. What is the Relationship Between Recall, TPR, FPR and Specificity?

| Metric | Formula | Meaning |
|---|---|---|
| Recall / TPR | TP / (TP + FN) | Correctly identified positives |
| FPR | FP / (FP + TN) | Incorrectly identified negatives |
| Specificity / TNR | TN / (TN + FP) | Correctly identified negatives |
| FNR | FN / (FN + TP) | Missed positive cases |

Important relationships:

- Recall = TPR.
- Specificity = TNR.
- FNR = 1 - Recall.
- FPR = 1 - Specificity.

## 15. What is a Classification Threshold?

A classification threshold determines when a model's predicted probability is converted into a positive or negative classification.

Example:

```text
Predicted probability = 0.85

Threshold = 0.50

Prediction = Positive
```

If the threshold is increased to 0.90:

```text
Predicted probability = 0.85

Threshold = 0.90

Prediction = Negative
```

Generally:

- Lower threshold: More positive predictions, usually higher recall and potentially lower precision.
- Higher threshold: Fewer positive predictions, usually higher precision and potentially lower recall.

The actual changes depend on the model and dataset.

## 16. What is an ROC Curve?

ROC stands for Receiver Operating Characteristic.

An ROC curve evaluates classification performance across different thresholds.

It plots:

- X-axis: False Positive Rate.
- Y-axis: True Positive Rate (Recall).

It helps evaluate how well a classification model distinguishes between positive and negative classes.

## 17. What is AUC?

AUC stands for Area Under the Curve.

ROC-AUC measures the model's ability to rank positive examples above negative examples.

Typical interpretation:

| AUC | General Interpretation |
|---|---|
| 1.0 | Perfect ranking |
| 0.9–1.0 | Excellent discrimination |
| 0.8–0.9 | Good discrimination |
| 0.7–0.8 | Fair discrimination |
| 0.5 | Random-ranking baseline |

These are approximate conventions, not universal acceptance standards.

## 18. What is a Classification Report?

A classification report summarizes important evaluation metrics.

Example:

```text
              precision    recall    f1-score

Spam             0.67       0.80       0.73
Not Spam         0.75       0.60       0.67

Accuracy                    0.70
```

It helps compare the performance of different classes.

## 19. Python Example – Calculate Evaluation Metrics

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
y_true = [1, 1, 1, 1, 1, 0, 0, 0, 0, 0]

# Predicted labels
y_pred = [1, 1, 1, 1, 0, 1, 1, 0, 0, 0]

print("Confusion Matrix:")
print(confusion_matrix(y_true, y_pred))

print("Accuracy:", accuracy_score(y_true, y_pred))

print("Precision:", precision_score(y_true, y_pred))

print("Recall:", recall_score(y_true, y_pred))

print("F1 Score:", f1_score(y_true, y_pred))

print("Classification Report:")
print(classification_report(y_true, y_pred))
```

Expected confusion matrix:

```text
[[3 2]
 [2 3]]
```

Scikit-learn uses the convention:

```text
[[TN FP]
 [FN TP]]
```

## 20. When Should We Use Each Metric?

| Use Case | Important Metric | Reason |
|---|---|---|
| Spam detection | Precision and Recall | Avoid false spam classifications and missed spam |
| Fraud detection | Recall and Precision | Detect fraud while controlling false alerts |
| Medical screening | Recall | Minimize missed positive cases |
| General balanced classification | Accuracy | Measure overall correctness |
| Imbalanced classification | Precision, Recall, F1 | Better understand minority-class performance |
| Ranking model | ROC-AUC | Evaluate ranking discrimination |

The appropriate metric depends on the business objective and the relative cost of false positives and false negatives.

## 21. Complete Example – All Metrics Together

Given:

```text
True Positive  = 40
True Negative  = 30
False Positive = 20
False Negative = 10

Total = 100
```

Calculations:

```text
Accuracy = (40 + 30) / 100
         = 70%

Precision = 40 / (40 + 20)
          = 66.67%

Recall = 40 / (40 + 10)
       = 80%

F1 Score = 2 * (0.6667 * 0.80)
           / (0.6667 + 0.80)
         = 72.73%

Specificity = 30 / (30 + 20)
            = 60%

False Positive Rate = 20 / (20 + 30)
                    = 40%
```

## 22. Final Quick Revision

| Term | Remember |
|---|---|
| TP | Predicted Positive, Actually Positive |
| TN | Predicted Negative, Actually Negative |
| FP | Predicted Positive, Actually Negative |
| FN | Predicted Negative, Actually Positive |
| Accuracy | Overall correctness |
| Precision | Reliability of positive predictions |
| Recall | Coverage of actual positive cases |
| F1 Score | Harmonic mean of precision and recall |
| Confidence | Model's estimated certainty |
| Threshold | Cutoff for classifying predictions |
| TPR | Same as Recall |
| FPR | False positive proportion among actual negatives |
| Specificity | Correctly identified negatives |
| ROC | TPR vs FPR across thresholds |
| AUC | Area under ROC curve |

## 23. Important Formulas

Accuracy:

(TP + TN) / (TP + TN + FP + FN)

Precision:

TP / (TP + FP)

Recall:

TP / (TP + FN)

F1 Score:

2 × (Precision × Recall) / (Precision + Recall)

Specificity:

TN / (TN + FP)

False Positive Rate:

FP / (FP + TN)

False Negative Rate:

FN / (FN + TP)
