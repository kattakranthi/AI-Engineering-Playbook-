# Customer Churn Prediction

## 1. What Is Customer Churn?

Customer churn occurs when a customer stops using a company's product or service.

**Customer churn prediction** is a machine learning problem that predicts whether a customer is likely to leave or discontinue a service.

Examples:
- A customer cancels a subscription.
- A bank customer closes an account.
- A telecom customer switches to another provider.
- A streaming customer cancels a membership.

The goal is to identify customers who may leave so the company can take preventive action.

## 2. Types of Churn

### Voluntary Churn
The customer intentionally stops using the service.

Examples:
- Cancelling a subscription.
- Switching to a competitor.
- Closing an account.

### Involuntary Churn
The customer stops using the service because of an external or operational reason.

Examples:
- Failed payments.
- Expired credit cards.
- Account suspension.

## 3. Customer Churn as a Machine Learning Problem

Churn prediction is generally a **supervised binary classification problem**.

The model predicts one of two classes:

| Class | Meaning |
|---|---|
| 0 | Customer stays |
| 1 | Customer churns |

The model learns patterns from historical customer data where the actual churn outcome is already known.

## 4. Important Features

Features are input variables used by the machine learning model.

| Feature | Description |
|---|---|
| Customer tenure | How long the customer has been using the service |
| Monthly charges | Amount paid each month |
| Total charges | Total amount paid |
| Contract type | Monthly, annual, or multi-year |
| Payment method | Credit card, bank transfer, etc. |
| Usage frequency | How frequently the customer uses the service |
| Support tickets | Number of customer support requests |
| Complaints | Number of complaints |
| Late payments | Number of delayed payments |
| Service changes | Upgrades, downgrades, or cancellations |

These features help the model identify patterns associated with customer churn.

## 5. Machine Learning Algorithms

### Logistic Regression

A classification algorithm that estimates the probability of a customer churning.

Output example:

- Customer A: 0.85 probability of churn.
- Customer B: 0.20 probability of churn.

If the classification threshold is 0.50:

- Probability >= 0.50: Predict churn.
- Probability < 0.50: Predict no churn.

### Decision Tree

Uses a sequence of rules to make predictions.

Example:

- Is customer tenure less than 6 months?
- Is the monthly charge greater than $100?
- Does the customer have multiple support complaints?

Based on these conditions, the tree predicts whether the customer may churn.

### Random Forest

Combines predictions from multiple decision trees.

Advantages:
- Handles nonlinear relationships.
- Can capture interactions between features.
- Often performs better than a single decision tree.

### Gradient Boosting

Builds models sequentially, with each new model attempting to correct errors made by earlier models.

Popular implementations:
- XGBoost
- LightGBM
- CatBoost

## 6. Typical Customer Churn Prediction Workflow

1. Collect historical customer data.
2. Clean missing, duplicate, and inconsistent records.
3. Perform exploratory data analysis.
4. Identify useful features.
5. Encode categorical variables.
6. Split data into training and testing datasets.
7. Train classification models.
8. Evaluate model performance.
9. Select a suitable classification threshold.
10. Deploy the model.
11. Monitor model performance and data drift.

## 7. Data Preprocessing

### Handling Missing Values

Missing values can be handled using:
- Mean or median imputation for numerical features.
- Mode imputation for categorical features.
- Removing records when appropriate.

### Encoding Categorical Variables

Machine learning models generally require numerical inputs.

Example:

| Contract Type | Encoded Value |
|---|---:|
| Monthly | 0 |
| Annual | 1 |
| Two-year | 2 |

For nominal categories, one-hot encoding may be more appropriate than assigning arbitrary numeric values.

### Feature Scaling

Algorithms such as logistic regression may benefit from feature scaling.

Common techniques:
- StandardScaler
- MinMaxScaler

Tree-based algorithms generally do not require feature scaling.

## 8. Train-Test Split

Divide the historical dataset into separate training and testing datasets.

Example:

- 80% training data.
- 20% testing data.

The training dataset teaches the model patterns.

The testing dataset evaluates how well the model performs on unseen records.

For classification, stratified splitting helps preserve the churn-to-non-churn ratio in both datasets.

For time-dependent customer data, use a time-based split to avoid training on information that would not have been available at prediction time.

## 9. Confusion Matrix

A confusion matrix compares actual customer behavior with model predictions.

| | Predicted Churn | Predicted No Churn |
|---|---:|---:|
| Actual Churn | True Positive (TP) | False Negative (FN) |
| Actual No Churn | False Positive (FP) | True Negative (TN) |

### True Positive (TP)

The model correctly predicts that a customer will churn.

### True Negative (TN)

The model correctly predicts that a customer will stay.

### False Positive (FP)

The model predicts churn, but the customer actually stays.

### False Negative (FN)

The model predicts that the customer will stay, but the customer actually churns.

False negatives can be costly because the company misses customers who are actually at risk of leaving.

## 10. Evaluation Metrics

### Accuracy

Percentage of all predictions that are correct.

Accuracy = (TP + TN) / (TP + TN + FP + FN)

### Precision

Out of all customers predicted to churn, how many actually churned?

Precision = TP / (TP + FP)

High precision means fewer customers are incorrectly flagged as churn risks.

### Recall

Out of all customers who actually churned, how many did the model identify?

Recall = TP / (TP + FN)

High recall means the model identifies a larger proportion of customers who are likely to leave.

### F1 Score

Harmonic mean of precision and recall.

F1 = 2 × (Precision × Recall) / (Precision + Recall)

Useful when balancing precision and recall.

### ROC-AUC

Measures how well a model distinguishes between churn and non-churn customers across different classification thresholds.

A higher ROC-AUC generally indicates better ranking discrimination.

### Precision-Recall AUC

Useful when churn is relatively rare and the dataset is imbalanced.

## 11. Why Accuracy Can Be Misleading

Suppose a dataset contains 1,000 customers:

- 900 customers stay.
- 100 customers churn.

A model predicts that every customer will stay.

The model achieves:

Accuracy = 900 / 1,000 = 90%

However, it identifies zero churn customers.

This demonstrates why accuracy alone is not sufficient for evaluating an imbalanced classification problem.

Precision, recall, F1 score, and PR-AUC should also be considered.

## 12. Python Example Using Scikit-Learn

```python
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score,
    confusion_matrix,
    classification_report
)

# Load dataset
df = pd.read_csv("customer_churn.csv")

# Separate features and target
X = df.drop(columns=["Churn"])
y = df["Churn"]

# Split dataset
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)

# Scale numerical features
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Train model
model = LogisticRegression(
    max_iter=1000,
    class_weight="balanced"
)

model.fit(X_train_scaled, y_train)

# Predict classes
y_pred = model.predict(X_test_scaled)

# Predict churn probabilities
y_probability = model.predict_proba(X_test_scaled)[:, 1]

# Evaluate model
print("Accuracy:", accuracy_score(y_test, y_pred))
print("Precision:", precision_score(y_test, y_pred))
print("Recall:", recall_score(y_test, y_pred))
print("F1 Score:", f1_score(y_test, y_pred))

print("Confusion Matrix:")
print(confusion_matrix(y_test, y_pred))

print("Classification Report:")
print(classification_report(y_test, y_pred))
```

Note: This example assumes all features are already numeric and have been cleaned. In a real dataset, categorical and numerical columns may need separate preprocessing pipelines.

## 13. Handling Imbalanced Data

Customer churn datasets often contain significantly more non-churn customers than churn customers.

Common techniques include:

### Class Weights

Give greater importance to the minority churn class during model training.

```python
model = LogisticRegression(class_weight="balanced")
```

### Oversampling

Increase the number of minority-class training examples.

Example: SMOTE (Synthetic Minority Over-sampling Technique).

### Undersampling

Reduce the number of majority-class training examples.

Important: Apply resampling only to the training data, never to the test data.

## 14. Choosing a Classification Threshold

A classification threshold determines when a customer is classified as likely to churn.

Default threshold: 0.50.

Example:

| Customer | Churn Probability | Prediction at 0.50 |
|---|---:|---|
| A | 0.90 | Churn |
| B | 0.70 | Churn |
| C | 0.40 | No Churn |
| D | 0.10 | No Churn |

Lowering the threshold generally increases recall but can also increase false positives.

Raising the threshold generally increases precision but can miss more actual churn customers.

The threshold should be selected based on business costs, such as retention campaign costs and the financial impact of losing a customer.

## 15. Confidence Score vs Churn Probability

A model may return a probability such as 0.85.

This means the model estimates an 85% probability of churn under its learned model and assumptions.

It does not automatically mean the model is 85% accurate.

A probability is better interpreted as a confidence estimate when the model is properly calibrated.

Calibration can be evaluated using:
- Calibration curves.
- Brier score.
- Log loss.

## 16. Model Deployment

A trained model can be deployed as a REST API.

Example architecture:

1. Customer data is collected from operational systems.
2. Data preprocessing transforms customer information into model features.
3. The deployed ML model predicts churn probability.
4. The prediction is stored with a timestamp and model version.
5. A business application consumes the prediction.
6. A retention workflow may contact selected customers.

Example API response:

```json
{
  "customer_id": "C1001",
  "churn_probability": 0.87,
  "predicted_churn": true,
  "model_version": "1.0"
}
```

Avoid exposing sensitive customer information unnecessarily.

## 17. Model Monitoring

After deployment, continuously monitor:

- Accuracy, precision, recall, and F1 score.
- ROC-AUC and PR-AUC.
- Prediction probability distribution.
- Data quality and missing values.
- Feature drift.
- Concept drift.
- Model latency and availability.
- Business outcomes, such as retention rate.

Actual churn labels may arrive later, so model performance monitoring should account for label delays.

Retrain the model when performance degrades or customer behavior changes significantly.

## 18. Common Challenges

| Challenge | Possible Solution |
|---|---|
| Imbalanced dataset | Class weights, SMOTE, undersampling |
| Missing customer data | Imputation and data-quality checks |
| Too many false positives | Adjust threshold and improve precision |
| Missing actual churn customers | Improve recall |
| Overfitting | Cross-validation and regularization |
| Data leakage | Use only information available at prediction time |
| Changing customer behavior | Monitor drift and retrain |
| Poor probability calibration | Calibration techniques |

## 19. Key Takeaways

- Customer churn prediction is generally a supervised binary classification problem.
- The target variable indicates whether a customer churned.
- Logistic regression, decision trees, random forests, and gradient boosting are common algorithms.
- Precision measures how many predicted churn customers actually churned.
- Recall measures how many actual churn customers were identified.
- F1 score balances precision and recall.
- Accuracy alone can be misleading with imbalanced data.
- Classification thresholds should reflect business costs.
- Probability calibration matters when probabilities are used for decisions.
- Monitor deployed models for performance degradation and data drift.
