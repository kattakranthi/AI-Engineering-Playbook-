# Training vs. Inference in AI / Machine Learning

## 1. Introduction

Training and inference are two major phases of the Machine Learning lifecycle.

- **Training:** The process of teaching a machine learning model using historical data so that it can learn patterns and relationships.
- **Inference:** The process of using a trained model to make predictions or generate responses for new, unseen inputs.

In simple terms:

- Training = Learning from data.
- Inference = Using what the model has learned.

## 2. Training vs. Inference

| Feature | Training | Inference |
|---|---|---|
| Purpose | Learn patterns from data | Generate predictions |
| Input | Training dataset | New input data |
| Output | Trained model and weights | Prediction or response |
| Frequency | Periodic | Every prediction/request |
| Compute | Usually GPU-intensive | CPU/GPU depending on model |
| Cost | Often high | Depends on request volume and model size |
| Data | Historical/labeled or self-supervised data | Real-time or batch inputs |
| Optimization | Model accuracy and generalization | Latency, throughput, cost |
| Example | Train fraud detection model | Predict whether a transaction is suspicious |

## 3. Machine Learning Training Lifecycle

The training process generally involves the following steps:

1. Data Collection
   - Collect historical transaction data.
   - Gather customer profiles and business records.
   - Obtain labels where supervised learning is used.

2. Data Preprocessing
   - Handle missing values.
   - Remove duplicates.
   - Normalize numerical features.
   - Encode categorical variables.

3. Feature Engineering
   - Identify useful input variables.
   - Create features such as transaction frequency, transaction amount, and geographic risk.

4. Model Selection
   - Select an appropriate algorithm.
   - Examples: Logistic Regression, Random Forest, XGBoost, Neural Networks.

5. Model Training
   - Feed training data into the algorithm.
   - The model learns patterns by adjusting parameters.

6. Model Evaluation
   - Evaluate using a validation/test dataset.
   - Measure accuracy, precision, recall, F1 score, and other relevant metrics.

7. Model Deployment
   - Package the trained model.
   - Deploy it as an API or batch prediction service.

## 4. Training Architecture

```text
       Historical Data Sources
                 |
                 v
           Data Collection
                 |
                 v
          Data Preprocessing
                 |
                 v
          Feature Engineering
                 |
                 v
            Training Data
                 |
                 v
           ML Training Job
                 |
                 v
           Model Evaluation
                 |
                 v
          Model Registry
                 |
                 v
          Model Deployment
                 |
                 v
          Prediction Service
```

## 5. Inference Lifecycle

Inference happens when an application sends new data to an already-trained model.

Steps:

1. Receive Input
   - Application sends a transaction, customer profile, or document information.

2. Input Validation
   - Validate required fields.
   - Check data types and schema.

3. Feature Transformation
   - Apply the same preprocessing and feature transformations used during training.

4. Model Execution
   - Load the trained model.
   - Run prediction using the input features.

5. Postprocessing
   - Convert model output into a business-readable result.
   - Apply thresholds or business rules.

6. Return Prediction
   - Return the prediction to the calling application.

## 6. Inference Architecture

```text
       Client Application
               |
               v
           API Gateway
               |
               v
       Prediction Service
               |
               v
        Input Validation
               |
               v
       Feature Transformation
               |
               v
          Trained Model
               |
               v
        Prediction Output
               |
               v
        Business Rules
               |
               v
        Client Response
```

## 7. Practical Financial Services Example: Fraud Detection

### Training

A bank collects historical transaction data.

Example:

| Transaction | Amount | Country | Label |
|---|---:|---|---|
| T1001 | $100 | USA | Legitimate |
| T1002 | $9,000 | USA | Fraud |
| T1003 | $50 | USA | Legitimate |
| T1004 | $7,500 | UK | Fraud |

The model learns patterns from these historical transactions.

Possible models:
- Logistic Regression
- Random Forest
- XGBoost
- Neural Networks

After training and evaluation, the model is deployed.

### Inference

A new transaction arrives:

```json
{
  "transaction_id": "T2001",
  "amount": 8500,
  "country": "UK",
  "customer_id": "C100"
}
```

The inference service sends the transaction features to the trained model.

Example output:

```json
{
  "transaction_id": "T2001",
  "fraud_probability": 0.94,
  "risk_category": "HIGH"
}
```

The application can then apply business rules and route the transaction for additional verification or review.

Important: A high fraud probability is a model prediction, not proof of fraud.

## 8. LLM Training vs. LLM Inference

LLMs also have training and inference phases.

### LLM Training

An LLM is trained on large datasets to learn language patterns and relationships.

Typical stages:

1. Pretraining
   - Model learns general language patterns from large text datasets.
   - Uses self-supervised learning, commonly next-token prediction.

2. Supervised Fine-Tuning (SFT)
   - Model learns from curated instruction-response examples.
   - Improves its ability to follow instructions.

3. Preference Optimization / Alignment
   - Model behavior is refined using preference data and alignment techniques.
   - Examples include RLHF and direct preference optimization (DPO).

### LLM Inference

When a user submits a prompt:

1. Application receives the prompt.
2. System and developer instructions establish behavior and constraints.
3. User prompt and conversation context are assembled.
4. Input is tokenized.
5. Model performs a forward pass.
6. Model generates output tokens.
7. Output is decoded and returned to the application.

Example:

```text
User:
Explain the difference between a hedge fund and a mutual fund.

        |
        v

Prompt Construction
        |
        v

Tokenization
        |
        v

LLM Inference
        |
        v

Next Token Prediction
        |
        v

Generated Response
        |
        v

Output Guardrails
        |
        v

Final Response
```

Note: Prompting does not normally retrain or update the underlying LLM weights.

## 9. LLM Inference Parameters

| Parameter | Purpose |
|---|---|
| Temperature | Controls randomness in token selection |
| Top-p | Controls sampling from cumulative token probability |
| Max tokens | Limits generated output length |
| Context window | Limits the tokens the model can process in one context |
| Stop sequences | Define conditions for stopping generation |

### Temperature example

- Temperature 0: More deterministic output, although not necessarily identical across every system.
- Temperature 0.7: More varied output.
- Higher temperature: Generally more randomness.

For financial services applications, a lower temperature is often preferred for structured, consistent responses, but it does not guarantee factual correctness.

## 10. RAG Inference vs. LLM Training

An important distinction in enterprise AI architecture is that RAG does not normally train the LLM.

### RAG Workflow

1. Ingest enterprise documents.
2. Parse and chunk documents.
3. Generate embeddings.
4. Store embeddings in a vector database.
5. Receive a user query.
6. Retrieve relevant document chunks.
7. Add retrieved context to the LLM prompt.
8. Generate a grounded response.

Example:

```text
                 User Question
                       |
                       v
                Query Embedding
                       |
                       v
                 Vector Search
                       |
                       v
               Retrieve Documents
                       |
                       v
                Prompt Assembly
                       |
                       v
                 LLM Inference
                       |
                       v
               Grounded Response
```

The model's weights remain unchanged during ordinary RAG inference.

### Training vs. RAG vs. Fine-Tuning

| Approach | What changes? | Purpose |
|---|---|---|
| Training | Model weights | Learn patterns |
| Fine-tuning | Model weights | Adapt model behavior |
| RAG | Retrieved context, not model weights | Provide external knowledge |
| Prompt engineering | Prompt instructions | Guide model behavior |

## 11. Batch Inference vs. Real-Time Inference

### Batch Inference

Predictions are generated for large datasets at scheduled intervals.

Examples:
- Daily fraud scoring.
- Portfolio risk calculations.
- Customer segmentation.
- Daily compliance screening.

Architecture:

```text
Data Warehouse
      |
      v
Batch Processing
      |
      v
Trained Model
      |
      v
Bulk Predictions
      |
      v
Database / Data Lake
```

### Real-Time Inference

Predictions are generated when an application sends a request.

Examples:
- Real-time fraud detection.
- AI chatbot responses.
- Customer risk scoring.
- Transaction anomaly detection.

Architecture:

```text
Client Request
      |
      v
API Gateway
      |
      v
Inference Service
      |
      v
Trained Model
      |
      v
Prediction
      |
      v
Client Response
```

## 12. Model Deployment and Production Considerations

After training, a model must be deployed and monitored.

Important considerations:

- Model versioning.
- Model registry.
- API availability.
- Horizontal scaling.
- GPU and CPU resource allocation.
- Inference latency.
- Throughput.
- Request batching.
- Caching.
- Authentication and authorization.
- Input validation.
- Monitoring and logging.
- Rollback to previous model versions.

### Common Deployment Tools

| Tool | Purpose |
|---|---|
| MLflow | Experiment tracking and model management |
| Kubeflow | Kubernetes-based ML workflows |
| Docker | Containerization |
| Kubernetes | Deployment and scaling |
| FastAPI | Model inference APIs |
| TensorFlow Serving | Serving TensorFlow models |
| NVIDIA Triton | High-performance model inference |
| Prometheus | Metrics collection |
| Grafana | Monitoring dashboards |

## 13. Model Monitoring

Models must be monitored after deployment because production data can change over time.

Important concepts:

**Data Drift**
- Production input data changes compared with training data.

**Concept Drift**
- The relationship between input features and target outcomes changes.

**Model Performance Degradation**
- Predictions become less accurate.

**Inference Latency**
- Time taken to generate a prediction.

**Throughput**
- Number of requests processed per second.

Monitoring metrics:

- Accuracy
- Precision
- Recall
- F1 score
- Prediction latency
- Error rate
- GPU utilization
- Request volume
- Data drift

When performance degrades, teams may investigate, retrain, validate, and deploy a new model version.

## 14. Interview Questions and Answers

### Q1. What is the difference between training and inference?

Training is the process of learning patterns from historical data by adjusting model parameters. Inference is the process of using the trained model to generate predictions for new input data.

### Q2. Does inference update model weights?

No. Normal inference uses the existing trained model weights. Model weights change during training or fine-tuning, not ordinary inference.

### Q3. Why is inference important in production?

Inference is where the trained model delivers business value. It enables applications to generate real-time predictions, recommendations, classifications, and AI responses.

### Q4. What is the difference between batch and real-time inference?

Batch inference processes large volumes of data at scheduled intervals. Real-time inference processes individual requests as they arrive and returns predictions with low latency.

### Q5. What is model drift?

Model drift occurs when the production environment changes and the model's performance or assumptions no longer hold. Data drift concerns changes in input distributions; concept drift concerns changes in the relationship between inputs and outcomes.

### Q6. How do you reduce inference latency?

- Use optimized model architectures.
- Use GPU acceleration when appropriate.
- Apply batching where suitable.
- Cache repeated results where safe.
- Use quantization.
- Scale inference services horizontally.
- Optimize network communication.
- Use smaller models when quality requirements permit.

### Q7. What is the difference between fine-tuning and RAG?

Fine-tuning updates model weights using additional training data. RAG retrieves relevant external information and provides it as context during inference without normally changing model weights.

### Q8. How do you deploy a trained ML model?

I would package the model, register and version it, expose it through a prediction API, deploy it using containers and Kubernetes, and monitor latency, availability, and model performance.

### Q9. What is the difference between a training dataset and an inference dataset?

The training dataset is used to teach the model. The inference dataset contains new input records that the trained model processes to produce predictions.

### Q10. How do you ensure training and inference use consistent features?

I would use a shared, versioned preprocessing and feature transformation pipeline, validate schemas, and test feature consistency between training and production.

### Q11. What is GPU inference?

GPU inference uses a graphics processing unit to execute model computations. It is particularly useful for deep learning and LLM workloads that involve large matrix operations.

### Q12. Can an LLM generate a response without RAG?

Yes. A pretrained LLM can generate responses using knowledge encoded in its learned parameters and the context supplied in the prompt. RAG is useful when answers need current or enterprise-specific information.

## 15. Interview Summary

Training:
- Learn from historical data.
- Update model weights.
- Evaluate model quality.
- Register and deploy the model.

Inference:
- Receive new input.
- Apply preprocessing.
- Execute the trained model.
- Generate predictions.
- Return results to the application.

**Remember:**

Training = Learning

Inference = Prediction / Generation

RAG = Retrieve external knowledge and use it during inference

Fine-tuning = Further train a model to adapt its behavior

These are foundational concepts for Machine Learning, Generative AI, and Enterprise AI Architecture.
