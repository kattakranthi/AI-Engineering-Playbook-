# Machine Learning & Predictive AI – Fundamentals

## 1. What is Machine Learning?

Machine Learning is a branch of AI that enables systems to learn patterns from historical data and make predictions or decisions without explicitly programming every rule.

Examples:
- Predict whether a borrower may default.
- Detect fraudulent transactions.
- Predict stock market volatility.
- Identify unusual financial activity.
- Forecast portfolio returns.

## 2. Types of Machine Learning

### A. Supervised Learning

The model learns from labeled historical data.

Examples:
- Credit default prediction.
- Fraud classification.
- Customer churn prediction.
- House price prediction.

Common algorithms:
- Logistic Regression.
- Linear Regression.
- Decision Trees.
- Random Forest.
- XGBoost.
- LightGBM.

### B. Unsupervised Learning

The model identifies patterns in data without predefined labels.

Examples:
- Customer segmentation.
- Trading behavior clustering.
- Portfolio grouping.
- Anomaly detection.

Common algorithms:
- K-Means.
- DBSCAN.
- Hierarchical Clustering.
- Isolation Forest.

### C. Reinforcement Learning

An agent learns by interacting with an environment and receiving rewards or penalties.

Examples:
- Resource optimization.
- Dynamic scheduling.
- Certain portfolio optimization research applications.

Common algorithms:
- Q-Learning.
- Deep Q-Networks.
- Policy Gradient.
- PPO.

Reinforcement learning is not the default approach for financial decision systems; it requires careful risk controls and validation.

## 3. Important ML Model Categories

| Category | Purpose | Example |
|---|---|---|
| Classification | Predict a category | Fraud or legitimate |
| Regression | Predict a numerical value | Expected loss |
| Clustering | Group similar records | Customer segments |
| Anomaly Detection | Identify unusual behavior | Suspicious transactions |
| Time Series | Analyze temporal patterns | Volatility forecasting |
| Recommendation | Recommend items or actions | Product recommendations |

## 4. Financial Services Use Cases

### Credit Risk Scoring
Predict probability of borrower default.

Models:
- Logistic Regression.
- Random Forest.
- XGBoost.

### Fraud Detection
Identify suspicious transaction patterns.

Models:
- XGBoost.
- Isolation Forest.
- Neural Networks.

### Market Risk
Estimate and forecast financial risk.

Techniques:
- Historical Simulation.
- Monte Carlo Simulation.
- GARCH.
- VaR and Expected Shortfall calculations.

Note: VaR is a risk measure, not itself a machine-learning model.

### Portfolio Analytics
Analyze portfolio characteristics and risk.

Techniques:
- Regression.
- Clustering.
- Factor models.
- Time-series forecasting.

### Customer Segmentation
Group customers based on behavior.

Models:
- K-Means.
- DBSCAN.

## 5. Credit Risk Scoring – End-to-End Example

Business problem:

A bank wants to estimate the probability that a borrower will default.

Input features:
- Annual income.
- Existing debt.
- Credit utilization.
- Payment history.
- Loan amount.
- Employment duration.

ML model:
- XGBoost Classifier.

Output:
- Probability of default.
- Model version.
- Prediction timestamp.

A separate policy engine maps model outputs to approved risk bands and decision workflows.

## 6. Model Training Lifecycle

1. Collect historical data.
2. Clean and validate data.
3. Perform feature engineering.
4. Split data into training, validation, and test sets.
5. Train the model.
6. Tune hyperparameters.
7. Evaluate model performance.
8. Validate fairness and explainability.
9. Register and version the model.
10. Deploy the model.
11. Monitor predictions and data drift.
12. Retrain when approved and justified.

## 7. Model Evaluation Metrics

Classification:
- Accuracy.
- Precision.
- Recall.
- F1 Score.
- ROC-AUC.
- PR-AUC.
- Calibration.

Regression:
- MAE.
- MSE.
- RMSE.
- R-squared.

For fraud detection, recall and precision are important because fraud is often a rare event and false positives can create substantial operational costs.

For credit risk, probability calibration and validation on representative out-of-time data are particularly important.

## 8. Model Deployment Architecture

```text
Data Sources
    |
    v
Data Engineering Pipeline
    |
    v
Feature Engineering
    |
    v
Model Training
    |
    v
Model Registry
    |
    v
Model Serving API
    |
    v
Spring Boot / AI Application
    |
    v
Prediction and Monitoring
```

## 9. Important Technologies

| Area | Technologies |
|---|---|
| Programming | Python |
| ML | Scikit-learn, XGBoost, LightGBM |
| Deep Learning | PyTorch, TensorFlow |
| Data Processing | Pandas, Spark |
| Model Serving | FastAPI |
| Model Registry | MLflow |
| Feature Store | Feast |
| Model Monitoring | Evidently, custom monitoring |
| Infrastructure | Docker, Kubernetes |
| Workflow | Airflow, Kubeflow |

## 10. ML vs Generative AI

Traditional ML predicts outcomes from structured or unstructured data.

Generative AI generates new content such as text, code, summaries, and explanations.

Example:
- XGBoost predicts a credit default probability.
- An LLM explains the risk report using approved source information.
- An embedding model enables semantic retrieval of credit policies.
- An agent orchestrates the calls to the required services.

## 11.  Summary

"Traditional machine learning is useful for predictive use cases such as credit risk scoring, fraud detection, anomaly detection, and financial forecasting. I would typically build and validate these models using Python libraries such as Scikit-learn or XGBoost, register and version them using an ML lifecycle platform, and expose them through a model-serving API.

In an enterprise AI application, the LLM can understand the user's question and orchestrate the workflow, while the ML model performs the actual prediction. The backend applies authorization and business policies before returning the result."

