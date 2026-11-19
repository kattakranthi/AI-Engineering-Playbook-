# Classification vs Regression in Machine Learning

## 1. Introduction

Classification and Regression are two major types of Supervised Machine Learning.

Both algorithms learn from historical labeled data and use the learned patterns to make predictions on new data.

The primary difference is:

- **Classification:** Predicts a category or class.
- **Regression:** Predicts a continuous numerical value.

For example:

| Business Problem | ML Type | Prediction |
|---|---|---|
| Will a customer default on a loan? | Classification | Yes / No |
| What will be the house price? | Regression | $450,000 |
| Is this transaction fraudulent? | Classification | Fraud / Not Fraud |
| What will be next month's revenue? | Regression | $2.5 Million |
| Will a customer leave the bank? | Classification | Yes / No |
| What will be the customer's lifetime value? | Regression | $15,000 |

## 2. Supervised Machine Learning

Supervised learning means training an ML model using historical data that contains input features and known output labels.

Example dataset:

| Customer | Income | Credit Score | Loan Default |
|---|---:|---:|---|
| C1 | 80000 | 750 | No |
| C2 | 45000 | 580 | Yes |
| C3 | 95000 | 790 | No |
| C4 | 55000 | 620 | No |
| C5 | 35000 | 520 | Yes |

Here:

- Input features: Income, Credit Score.
- Target variable: Loan Default.
- ML model: Classification.

The model learns patterns from historical customers and predicts whether a new customer is likely to default.

## 3. Classification

### 3.1 What is Classification?

Classification is a supervised machine learning technique used to predict discrete categories or classes.

The model learns from historical labeled data and predicts which category a new observation belongs to.

Examples:

- Fraudulent vs legitimate transaction.
- Loan default vs no default.
- Customer churn vs customer retention.
- High-risk vs low-risk customer.
- Spam vs non-spam email.

### 3.2 Types of Classification

**A. Binary Classification**

Predicts one of two possible classes.

Examples:

- Fraud / Not Fraud.
- Default / No Default.
- Churn / No Churn.
- Approved / Rejected.

**B. Multiclass Classification**

Predicts one class from three or more possible classes.

Example: Customer risk classification.

- Low Risk.
- Medium Risk.
- High Risk.

Other examples:

- Document classification.
- Product categorization.
- Customer support ticket classification.

**C. Multilabel Classification**

A single observation can belong to multiple categories simultaneously.

Example:

A financial document might be classified as:

- Annual Report.
- Financial Statement.
- Regulatory Filing.

A document can have multiple applicable labels.

### 3.3 Popular Classification Algorithms

| Algorithm | Description | Use Case |
|---|---|---|
| Logistic Regression | Predicts class probabilities | Credit default |
| Decision Tree | Uses decision rules | Loan approval |
| Random Forest | Combines multiple decision trees | Fraud detection |
| XGBoost | Gradient boosting algorithm | Credit risk |
| Support Vector Machine | Finds an optimal separating boundary | Spam detection |
| Naive Bayes | Uses Bayes' theorem | Email classification |
| Neural Networks | Learns complex patterns | Fraud detection |

## 4. How Classification Calculates Predictions

Let's understand Logistic Regression using a credit risk example.

### Business Problem

A bank wants to predict whether a customer will default on a loan.

Input features:

- Annual income.
- Credit score.
- Existing debt.
- Loan amount.
- Payment history.

The model calculates a probability of default.

### Step 1: Calculate the weighted input

Logistic Regression calculates:

```text
z = b0 + b1*x1 + b2*x2 + ... + bn*xn
```

Where:

- b0 = Intercept.
- b1, b2 = Model coefficients.
- x1, x2 = Input features.

### Step 2: Convert the result into a probability

The logistic (sigmoid) function converts the result into a value between 0 and 1.

```text
Probability = 1 / (1 + e^(-z))
```

### Step 3: Apply a classification threshold

Suppose the model predicts a 75% probability of loan default.

If the classification threshold is 50%:

```text
Probability >= 0.50  -> Default
Probability < 0.50   -> No Default
```

The model predicts Default.

The threshold is a business decision and should be calibrated based on the costs of false positives and false negatives.

### Interactive Classification Example

Try changing the classification threshold to understand how model predictions change.

<LearningViz type_id="CLASSIFICATION_THRESHOLD" initial_values={{"threshold":0.5}}/>

### 4.1 Python Example: Logistic Regression

```python
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import (
    accuracy_score,
    confusion_matrix,
    classification_report
)

# Sample customer dataset
data = {
    "income": [80000, 45000, 95000, 55000, 35000,
               70000, 40000, 90000, 30000, 60000],
    "credit_score": [750, 580, 790, 620, 520,
                     710, 550, 780, 500, 680],
    "default": [0, 1, 0, 0, 1, 0, 1, 0, 1, 0]
}

df = pd.DataFrame(data)

# Input features
X = df[["income", "credit_score"]]

# Target variable
y = df["default"]

# Split training and testing data
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.3,
    random_state=42,
    stratify=y
)

# Train the model
model = LogisticRegression(max_iter=1000)

model.fit(X_train, y_train)

# Predict classes
predictions = model.predict(X_test)

# Predict probabilities
probabilities = model.predict_proba(X_test)

print("Predictions:", predictions)
print("Probabilities:", probabilities)

print("Accuracy:", accuracy_score(y_test, predictions))

print("Confusion Matrix:")
print(confusion_matrix(y_test, predictions))

print(classification_report(y_test, predictions))
```

Note: This is a small illustrative dataset. It is not suitable for training a production credit risk model.

## 5. Regression

### 5.1 What is Regression?

Regression is a supervised machine learning technique used to predict continuous numerical values.

Unlike classification, regression predicts a numerical quantity rather than a category.

Examples:

- Predicting house prices.
- Predicting monthly revenue.
- Predicting customer lifetime value.
- Forecasting stock volatility.
- Predicting loan interest rates.

### 5.2 Types of Regression

**A. Simple Linear Regression**

Uses one independent variable to predict a continuous target.

Example:

Predicting house prices based on square footage.

**B. Multiple Linear Regression**

Uses multiple independent variables to predict a continuous target.

Example:

Predicting house prices based on:

- Square footage.
- Number of bedrooms.
- Location.
- Property age.

**C. Polynomial Regression**

Models nonlinear relationships using polynomial features.

**D. Ridge Regression**

Uses L2 regularization to reduce the impact of large coefficients and help control overfitting.

**E. Lasso Regression**

Uses L1 regularization and can reduce some feature coefficients to zero.

### 5.3 Regression Algorithms

| Algorithm | Description | Use Case |
|---|---|---|
| Linear Regression | Models a linear relationship | Revenue prediction |
| Multiple Regression | Uses multiple features | House prices |
| Ridge Regression | L2 regularization | Financial forecasting |
| Lasso Regression | L1 regularization | Feature selection |
| Decision Tree Regressor | Tree-based numerical prediction | Property valuation |
| Random Forest Regressor | Combines multiple trees | Demand forecasting |
| XGBoost Regressor | Gradient boosting | Financial forecasting |

## 6. How Regression Calculates Predictions

Let's understand Linear Regression using a house price prediction example.

### Business Problem

A real estate company wants to predict house prices based on square footage.

Historical data:

| House | Square Feet | Price |
|---|---:|---:|
| A | 1000 | $200,000 |
| B | 1500 | $300,000 |
| C | 2000 | $400,000 |
| D | 2500 | $500,000 |

The model learns the relationship between square footage and price.

### Step 1: Calculate the regression equation

```text
Predicted Price = b0 + b1 * Square Feet
```

Where:

- b0 = Intercept.
- b1 = Slope or coefficient.

For this simplified example:

```text
Predicted Price = 100 * Square Feet
```

### Step 2: Predict a new value

Suppose the new house has 1,800 square feet.

```text
Predicted Price = 100 * 1800

Predicted Price = $180,000
```

This is an illustrative equation; a real model learns its coefficients from training data.

### Interactive Regression Example

Explore how changing the observed data points affects the regression line.

<LearningViz type_id="LEAST_SQUARE_REGRESSION"/>

### 6.1 Python Example: Linear Regression

```python
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error
from sklearn.metrics import mean_squared_error
from sklearn.metrics import r2_score

# Sample house dataset
data = {
    "square_feet": [1000, 1500, 2000, 2500, 3000],
    "price": [200000, 300000, 400000, 500000, 600000]
}

df = pd.DataFrame(data)

# Input features
X = df[["square_feet"]]

# Target variable
y = df["price"]

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    random_state=42
)

# Train model
model = LinearRegression()

model.fit(X_train, y_train)

# Predict
predictions = model.predict(X_test)

print("Predictions:", predictions)

print("Actual Values:", y_test.values)

print("MAE:", mean_absolute_error(y_test, predictions))

print("MSE:", mean_squared_error(y_test, predictions))

print("R2 Score:", r2_score(y_test, predictions))

# Predict price for a new house
new_house = pd.DataFrame({"square_feet": [1800]})

predicted_price = model.predict(new_house)

print("Predicted House Price:", predicted_price[0])
```

## 7. Classification vs Regression: Key Differences

| Factor | Classification | Regression |
|---|---|---|
| Objective | Predict a category | Predict a numerical value |
| Output | Discrete class | Continuous value |
| Example | Fraud / Not Fraud | House price |
| Target Variable | Categorical | Numerical |
| Algorithms | Logistic Regression, Random Forest Classifier | Linear Regression, Random Forest Regressor |
| Evaluation | Accuracy, Precision, Recall, F1 | MAE, MSE, RMSE, R2 |
| Business Application | Fraud detection | Revenue forecasting |

## 8. Model Evaluation Metrics

### 8.1 Classification Metrics

**Accuracy**

Measures the percentage of correct predictions.

```text
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

**Precision**

Measures how many predicted positive cases were actually positive.

```text
Precision = TP / (TP + FP)
```

**Recall**

Measures how many actual positive cases the model correctly identified.

```text
Recall = TP / (TP + FN)
```

**F1 Score**

Harmonic mean of precision and recall.

```text
F1 = 2 * (Precision * Recall) /
     (Precision + Recall)
```

**ROC-AUC**

Measures how well a classifier distinguishes between positive and negative classes across different thresholds.

### 8.2 Confusion Matrix

A confusion matrix evaluates classification predictions.

| | Predicted Fraud | Predicted Not Fraud |
|---|---|---|
| Actual Fraud | True Positive (TP) | False Negative (FN) |
| Actual Not Fraud | False Positive (FP) | True Negative (TN) |

Example:

- TP: Fraud correctly detected.
- FP: Legitimate transaction incorrectly flagged.
- FN: Fraud transaction missed.
- TN: Legitimate transaction correctly identified.

For fraud detection, missing actual fraud cases can be expensive, so recall is often important. However, precision also matters because excessive false alerts increase investigation costs.

### 8.3 Regression Metrics

**Mean Absolute Error (MAE)**

Average absolute difference between actual and predicted values.

```text
MAE = Sum(|Actual - Predicted|) / N
```

**Mean Squared Error (MSE)**

Average squared difference between actual and predicted values.

```text
MSE = Sum((Actual - Predicted)^2) / N
```

**Root Mean Squared Error (RMSE)**

Square root of MSE.

```text
RMSE = sqrt(MSE)
```

**R2 Score**

Measures how much variation in the target variable is explained by the model relative to a mean baseline.

A higher R2 generally indicates a better fit, but it does not guarantee good predictions on unseen data.

## 9. Financial Services Use Cases

### 9.1 Credit Risk Prediction

Business problem:

A bank wants to predict whether a customer will default on a loan.

ML type: Classification.

Input features:

- Annual income.
- Credit score.
- Existing loans.
- Debt-to-income ratio.
- Payment history.
- Loan amount.

Algorithm:

- Logistic Regression.
- Random Forest.
- XGBoost.

Output:

```text
Customer A:
Probability of Default = 5%

Customer B:
Probability of Default = 35%
```

These probabilities are illustrative, not actual borrower assessments.

### 9.2 Fraud Detection

Business problem:

A bank wants to identify fraudulent credit card transactions.

ML type: Classification.

Input features:

- Transaction amount.
- Transaction location.
- Transaction time.
- Merchant category.
- Customer transaction history.
- Device information.

Algorithms:

- Logistic Regression.
- Random Forest.
- XGBoost.
- Isolation Forest.

Output:

```text
Transaction A:
Fraud Probability = 95%

Transaction B:
Fraud Probability = 3%
```

A production system may combine ML predictions with business rules and additional verification.

### 9.3 Customer Churn Prediction

Business problem:

A financial institution wants to predict which customers are likely to close their accounts.

ML type: Classification.

Input features:

- Account age.
- Transaction frequency.
- Customer complaints.
- Account balance trends.
- Product usage.
- Customer engagement.

Output:

```text
Customer A:
Churn Probability = 80%

Customer B:
Churn Probability = 10%
```

### 9.4 Revenue Forecasting

Business problem:

An investment bank wants to forecast future monthly revenue.

ML type: Regression / Time-Series Forecasting.

Input features:

- Historical revenue.
- Market conditions.
- Transaction volume.
- Seasonal trends.
- Deal pipeline.

Algorithms:

- Linear Regression.
- Random Forest Regressor.
- XGBoost Regressor.
- ARIMA.
- Prophet.

Output:

```text
Predicted Revenue for Next Month = $5 Million
```

### 9.5 Customer Lifetime Value Prediction

Business problem:

A bank wants to estimate how much revenue a customer may generate over their relationship with the bank.

ML type: Regression.

Input features:

- Customer income.
- Account balances.
- Product usage.
- Transaction frequency.
- Customer tenure.
- Historical revenue.

Output:

```text
Predicted Customer Lifetime Value = $25,000
```

## 10. Classification vs Regression in an End-to-End Architecture

```text
                Business Application
                         |
                         v
                    API Gateway
                         |
                         v
                   ML Prediction API
                         |
                         v
                   Feature Processing
                         |
                         v
                    Feature Store
                         |
                         v
                    Model Service
                         |
              +----------+----------+
              |                     |
              v                     v
        Classification          Regression
              |                     |
              v                     v
        Fraud Prediction       Revenue Prediction
              |                     |
              v                     v
        Risk Probability       Predicted Revenue
              |                     |
              +----------+----------+
                         |
                         v
                  Result Validation
                         |
                         v
                  Business Decision
                         |
                         v
                  Monitoring / Audit
```

### Production Components

- API Gateway: Authentication, authorization, and rate limiting.
- Feature Processing: Cleans and transforms input data.
- Feature Store: Stores reusable ML features.
- Model Service: Loads and executes trained models.
- Model Registry: Tracks model versions.
- Monitoring: Tracks accuracy, drift, latency, and failures.
- Audit: Maintains prediction and decision records.

## 11. Important Interview Questions and Answers

### Q1. What is the difference between classification and regression?

Classification predicts discrete categories, whereas regression predicts continuous numerical values.

For example, predicting whether a borrower will default is classification, while predicting the amount of financial loss is regression.

### Q2. Is Logistic Regression a classification or regression algorithm?

Despite its name, Logistic Regression is primarily used for classification.

It calculates the probability of an observation belonging to a particular class using the sigmoid function.

### Q3. Can we use Random Forest for both classification and regression?

Yes.

RandomForestClassifier predicts categories, while RandomForestRegressor predicts numerical values.

### Q4. What is the difference between probability and prediction?

Probability represents the model's estimated likelihood of an outcome.

Prediction represents the final class or numerical output.

For example:

```text
Default Probability = 80%

Threshold = 50%

Final Classification = Default
```

### Q5. Why is accuracy not sufficient for fraud detection?

Fraud datasets are often highly imbalanced.

For example:

- 99,000 legitimate transactions.
- 1,000 fraudulent transactions.

A model predicting every transaction as legitimate achieves 99% accuracy but detects no fraud.

Therefore, precision, recall, F1 score, and PR-AUC are also important.

### Q6. What is overfitting?

Overfitting occurs when a model learns the training data too closely, including noise and irrelevant patterns.

It performs well on training data but poorly on unseen data.

Solutions include:

- Cross-validation.
- Regularization.
- Feature selection.
- Pruning.
- More representative training data.

### Q7. What is the difference between supervised and unsupervised learning?

Supervised learning uses labeled data to learn a relationship between input features and target variables.

Unsupervised learning identifies patterns or groups in data without predefined labels.

Examples:

- Supervised: Fraud classification.
- Unsupervised: Customer segmentation using K-means clustering.

### Q8. When would you choose classification instead of regression?

I choose classification when the business needs to predict a category, such as fraud versus legitimate or default versus no default.

I choose regression when the business needs a continuous numerical prediction, such as expected revenue, financial loss, or customer lifetime value.

### Q9. How do you select an ML algorithm?

I start with the business problem and understand the target variable.

Then I evaluate:

- Dataset size.
- Feature types.
- Interpretability requirements.
- Prediction accuracy.
- Latency.
- Scalability.
- Model training and inference costs.

I establish a baseline model and compare candidate algorithms using appropriate evaluation metrics.

### Q10. Can we combine classification and regression in one business application?

Yes.

For example, in credit risk:

1. A classification model predicts the probability of loan default.
2. A regression model estimates the expected financial loss.

The outputs can be combined to support credit risk decisions.

## 12. Key Takeaways

- Classification predicts categories.
- Regression predicts numerical values.
- Both are supervised machine learning techniques.
- Logistic Regression is commonly used for classification.
- Linear Regression is used for numerical prediction.
- Random Forest and XGBoost support both classification and regression.
- Classification uses metrics such as Precision, Recall, F1, and ROC-AUC.
- Regression uses MAE, MSE, RMSE, and R2.
- Financial services applications frequently combine multiple ML models.
- Model selection should be driven by business requirements, data characteristics, and risk considerations.

## 13. Next Topics to Learn

1. Logistic Regression – Mathematical calculations and Python.
2. Decision Trees – How splitting and Gini impurity work.
3. Random Forest – How multiple trees make predictions.
4. XGBoost – Gradient boosting and model training.
5. Confusion Matrix – TP, FP, TN, FN.
6. Credit Risk Scoring – Probability of Default (PD).
7. Fraud Risk Scoring – Transaction-level risk prediction.
8. Model Evaluation – Precision, Recall, F1, ROC-AUC, PR-AUC.
9. Feature Engineering – Selecting and transforming input variables.
10. Model Drift – Monitoring production ML models.
