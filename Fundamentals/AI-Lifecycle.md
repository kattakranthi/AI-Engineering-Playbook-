1. What is the AI lifecycle?
Answer:

The AI lifecycle is the end-to-end process of building, deploying, operating, and improving an AI solution.

It typically consists of these stages:
1. Business problem definition
   
   Identify the business problem, users, expected outcomes, and success criteria.
2. Data collection and ingestion
   
   Collect data from databases, APIs, documents, data lakes, and other sources.
3. Data preparation and validation
   
   Clean, transform, validate, and prepare data for model consumption.
4. Model selection and development
   
   Select an ML model, LLM, embedding model, or combination based on the use case.
5. Model training, testing, and evaluation
   
   Train or configure the model and evaluate its accuracy, performance, and risks.
6. Deployment and integration
   
   Deploy models behind APIs and integrate them with enterprise applications.
7. Monitoring and governance
   
   Monitor quality, latency, cost, security, drift, and compliance.
8. Continuous improvement
   
   Use feedback and evaluation results to improve data, prompts, models, and workflows.

2. What are the different stages of the AI lifecycle?
Answer:

I generally divide the AI lifecycle into eight stages:
Stage	What happens?
Problem definition	Identify the business problem
Data engineering	Collect and prepare data
Model development	Select and build the model
Model evaluation	Measure model quality
Deployment	Deploy the model into production
Integration	Expose capabilities through APIs
Monitoring	Track performance and reliability
Retraining/improvement	Improve based on feedback and new data

For example, in a banking organization, if we want to predict credit risk, we collect historical loan data, prepare features, train an XGBoost model, evaluate its performance, deploy it through a Python FastAPI service, and monitor prediction quality over time.
3. What is the difference between AI, ML, and Generative AI?
Answer:
- Artificial Intelligence (AI): The broader field of building systems that perform tasks requiring human-like intelligence.
- Machine Learning (ML): A subset of AI where models learn patterns from historical data to make predictions.
- Generative AI: A subset of AI that generates new content, such as text, code, images, and summaries.
Examples in financial services:
Technology	Example
AI	Automated banking assistant
ML	Credit risk prediction
Generative AI	Summarizing financial documents
Agentic AI	Research agent that retrieves documents, calls tools, and prepares an investment research report

4. What is the difference between training and inference?
Answer:

Training is the process of teaching a model using historical data so it can learn patterns.

Inference is the process of using an already-trained model to generate predictions on new data.

Example:
- Training: Use five years of historical loan data to train an XGBoost credit risk model.
- Inference: A new loan application arrives, and the model predicts a default probability.
Training










Historical data → Learning → Trained model


Inference










New data → Trained model → Prediction



5. What is data preprocessing?
Answer:

Data preprocessing is the process of preparing raw data before passing it to an ML model.

Common activities include:
1. Removing duplicate records.
2. Handling missing values.
3. Correcting inconsistent formats.
4. Identifying outliers.
5. Encoding categorical variables.
6. Normalizing or scaling numerical features.
7. Splitting data into training, validation, and test datasets.
Example:

A bank's loan dataset might contain missing income values, inconsistent employment categories, and duplicate customer records. We clean and transform the data before training the credit risk model.
6. What is feature engineering?
Answer:

Feature engineering is the process of creating meaningful input variables from raw data to improve model performance.

For example, instead of providing only raw transaction history, we can create features such as:
- Average monthly account balance.
- Number of missed loan payments.
- Debt-to-income ratio.
- Credit utilization percentage.
- Number of transactions in the last 30 days.
These features help a credit risk model identify patterns more effectively.
7. What is model evaluation?
Answer:

Model evaluation measures how well a model performs on data that it has not previously seen.

The evaluation metrics depend on the problem.
Use case	Common evaluation metrics
Classification	Accuracy, Precision, Recall, F1
Regression	MAE, MSE, RMSE, R²
Fraud detection	Precision, Recall, PR-AUC
Credit risk	ROC-AUC, Precision, Recall, Calibration
LLM applications	Faithfulness, relevance, groundedness
RAG applications	Retrieval precision, Recall@K, answer correctness

For example, in fraud detection, accuracy alone can be misleading because fraudulent transactions may represent a very small percentage of total transactions. Precision and recall become especially important.
8. What is overfitting and underfitting?
Answer:

Overfitting: The model performs very well on training data but poorly on new, unseen data. It has learned noise or overly specific patterns.

Underfitting: The model is too simple to learn the underlying patterns, so it performs poorly on both training and test data.

Ways to reduce overfitting include:
- Cross-validation.
- Regularization.
- Feature selection.
- Pruning.
- More representative training data.
- Early stopping.
9. What is MLOps?
Answer:

MLOps stands for Machine Learning Operations. It applies DevOps and automation principles to the ML lifecycle.

It helps automate:
- Data validation.
- Model training.
- Model evaluation.
- Model versioning.
- Model deployment.
- Model monitoring.
- Retraining workflows.
For example, we can use MLflow to track experiments and model versions, Docker to package the model service, Kubernetes to deploy it, and CI/CD pipelines to automate releases.
10. What is the difference between DevOps and MLOps?
DevOps	MLOps
Focuses on application delivery	Focuses on ML model lifecycle
Manages application code	Manages code, data, and models
CI/CD	CI/CD plus model/data validation
Application monitoring	Application and model monitoring
Software releases	Model releases and retraining

11. What is model drift?
Answer:

Model drift occurs when the model's performance or the relationship between its inputs and outputs changes over time.

There are two important types:
- Data drift: The distribution of input data changes.
- Concept drift: The relationship between input variables and the target outcome changes.
Example:

A credit risk model trained using historical economic conditions may become less accurate during a recession because customer repayment behavior changes.

We monitor input distributions, prediction distributions, and actual outcomes. If performance degrades, we investigate and may retrain or recalibrate the model.
12. How do you deploy an ML model into production?
Answer:

I would typically follow these steps:
1. Train and evaluate the model.
2. Register the approved model in a model registry.
3. Package the model and dependencies using Docker.
4. Create a Python FastAPI service to expose prediction endpoints.
5. Deploy the service to Kubernetes.
6. Integrate it with enterprise applications through REST APIs.
7. Add authentication, authorization, logging, and monitoring.
8. Monitor model performance and roll back if necessary.











You're using the free version of ChatGPT. Upgrade for advanced models, better memory, and higher-quality image creation.
Try Plus free
