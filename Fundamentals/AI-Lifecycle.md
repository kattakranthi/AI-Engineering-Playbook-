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
Example architecture:
#chatgpt-mermaid-_r_2il_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-_r_2il_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_2il_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_2il_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-_r_2il_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-_r_2il_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-_r_2il_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-_r_2il_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-_r_2il_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-_r_2il_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-_r_2il_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2il_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2il_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-_r_2il_ p{margin:0;}#chatgpt-mermaid-_r_2il_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-_r_2il_ .label text,#chatgpt-mermaid-_r_2il_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ .node rect,#chatgpt-mermaid-_r_2il_ .node circle,#chatgpt-mermaid-_r_2il_ .node ellipse,#chatgpt-mermaid-_r_2il_ .node polygon,#chatgpt-mermaid-_r_2il_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-_r_2il_ .rough-node .label text,#chatgpt-mermaid-_r_2il_ .node .label text,#chatgpt-mermaid-_r_2il_ .image-shape .label,#chatgpt-mermaid-_r_2il_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-_r_2il_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-_r_2il_ .rough-node .label,#chatgpt-mermaid-_r_2il_ .node .label,#chatgpt-mermaid-_r_2il_ .image-shape .label,#chatgpt-mermaid-_r_2il_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-_r_2il_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-_r_2il_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2il_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2il_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-_r_2il_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-_r_2il_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_2il_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-_r_2il_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_2il_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-_r_2il_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-_r_2il_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-_r_2il_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2il_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-_r_2il_ .icon-shape,#chatgpt-mermaid-_r_2il_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_2il_ .icon-shape p,#chatgpt-mermaid-_r_2il_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-_r_2il_ .icon-shape .label rect,#chatgpt-mermaid-_r_2il_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_2il_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-_r_2il_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-_r_2il_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-_r_2il_ [data-look="neo"].node rect,#chatgpt-mermaid-_r_2il_ [data-look="neo"].cluster rect,#chatgpt-mermaid-_r_2il_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-_r_2il_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2il_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-_r_2il_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-_r_2il_-gradient);stroke-width:1px;}#chatgpt-mermaid-_r_2il_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2il_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-_r_2il_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-_r_2il_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2il_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-_r_2il_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-_r_2il_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2il_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-_r_2il_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2il_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_2il_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_2il_ .node tspan[font-weight="normal"],#chatgpt-mermaid-_r_2il_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-_r_2il_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-_r_2il_ .node rect,#chatgpt-mermaid-_r_2il_ .node circle,#chatgpt-mermaid-_r_2il_ .node ellipse,#chatgpt-mermaid-_r_2il_ .node polygon,#chatgpt-mermaid-_r_2il_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-_r_2il_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-_r_2il_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-_r_2il_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-_r_2il_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2il_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Banking ApplicationAPI GatewayCredit Risk APIPython FastAPIML Model - XGBoostRisk PredictionPolicy / Decision EngineApplication ResponseMonitoring





Important: The model predicts risk; a separately governed policy or decision engine determines the appropriate business action.

13. What is the AI lifecycle for Generative AI applications?
Answer:

The Generative AI lifecycle is similar to the traditional AI lifecycle, but it includes additional steps such as prompt engineering, embeddings, retrieval, grounding, and guardrails.

For an enterprise banking RAG application:
1. Identify the business use case.
2. Collect banking policies, research reports, and documents.
3. Extract and clean document content.
4. Split documents into chunks.
5. Generate embeddings.
6. Store embeddings in a vector database such as Milvus.
7. Retrieve relevant chunks using vector or hybrid search.
8. Pass retrieved context to the LLM.
9. Apply input and output guardrails.
10. Evaluate answer quality and groundedness.
11. Deploy and monitor the application.
#chatgpt-mermaid-_r_2is_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-_r_2is_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_2is_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_2is_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-_r_2is_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-_r_2is_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-_r_2is_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-_r_2is_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-_r_2is_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-_r_2is_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-_r_2is_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2is_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2is_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-_r_2is_ p{margin:0;}#chatgpt-mermaid-_r_2is_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-_r_2is_ .label text,#chatgpt-mermaid-_r_2is_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ .node rect,#chatgpt-mermaid-_r_2is_ .node circle,#chatgpt-mermaid-_r_2is_ .node ellipse,#chatgpt-mermaid-_r_2is_ .node polygon,#chatgpt-mermaid-_r_2is_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-_r_2is_ .rough-node .label text,#chatgpt-mermaid-_r_2is_ .node .label text,#chatgpt-mermaid-_r_2is_ .image-shape .label,#chatgpt-mermaid-_r_2is_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-_r_2is_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-_r_2is_ .rough-node .label,#chatgpt-mermaid-_r_2is_ .node .label,#chatgpt-mermaid-_r_2is_ .image-shape .label,#chatgpt-mermaid-_r_2is_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-_r_2is_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-_r_2is_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2is_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2is_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-_r_2is_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-_r_2is_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_2is_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-_r_2is_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_2is_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-_r_2is_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-_r_2is_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-_r_2is_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_2is_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-_r_2is_ .icon-shape,#chatgpt-mermaid-_r_2is_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_2is_ .icon-shape p,#chatgpt-mermaid-_r_2is_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-_r_2is_ .icon-shape .label rect,#chatgpt-mermaid-_r_2is_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_2is_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-_r_2is_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-_r_2is_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-_r_2is_ [data-look="neo"].node rect,#chatgpt-mermaid-_r_2is_ [data-look="neo"].cluster rect,#chatgpt-mermaid-_r_2is_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-_r_2is_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2is_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-_r_2is_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-_r_2is_-gradient);stroke-width:1px;}#chatgpt-mermaid-_r_2is_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2is_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-_r_2is_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-_r_2is_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2is_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-_r_2is_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-_r_2is_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2is_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-_r_2is_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_2is_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_2is_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_2is_ .node tspan[font-weight="normal"],#chatgpt-mermaid-_r_2is_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-_r_2is_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-_r_2is_ .node rect,#chatgpt-mermaid-_r_2is_ .node circle,#chatgpt-mermaid-_r_2is_ .node ellipse,#chatgpt-mermaid-_r_2is_ .node polygon,#chatgpt-mermaid-_r_2is_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-_r_2is_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-_r_2is_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-_r_2is_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-_r_2is_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_2is_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Business Use CaseDocument IngestionParsing and ChunkingEmbedding ModelVector DatabaseUser QueryQuery EmbeddingHybrid RetrievalContext and PromptLLMOutput GuardrailsResponseEvaluation and Monitoring





14. What is the difference between the ML lifecycle and the GenAI lifecycle?
Traditional ML	Generative AI
Feature engineering	Prompt engineering
Model training	Model selection or fine-tuning
Predictive output	Generated output
Accuracy, precision, recall	Groundedness, relevance, faithfulness
Feature drift	Data, retrieval, prompt, and model drift
Model registry	Model and prompt versioning
Prediction monitoring	Token, latency, cost, and quality monitoring

15. What is the difference between fine-tuning and RAG?
Answer:

RAG retrieves relevant information from external knowledge sources at runtime and provides it to the LLM as context.

Fine-tuning trains an existing model further using a specialized dataset to change or improve its behavior.

For a bank:
- RAG is useful when policies, procedures, and research documents change frequently.
- Fine-tuning may be useful when we need consistent specialized behavior, terminology, or output formats.
I would generally consider RAG first for frequently changing enterprise knowledge because it avoids retraining the model whenever source documents change.
16. How do you monitor an AI application in production?
Answer:

I monitor four major areas:
1. Infrastructure: CPU, memory, availability, and container health.
2. Application: API latency, throughput, error rates, and timeouts.
3. Model: Prediction quality, drift, confidence, and evaluation results.
4. Business: Cost per request, user adoption, task completion, and business impact.
For a GenAI application, I additionally monitor:
- Token consumption.
- LLM response latency.
- Retrieval quality.
- Hallucination and groundedness indicators.
- Prompt injection attempts.
- Guardrail violations.
- Cost per conversation.
Tools can include Prometheus, Grafana, OpenTelemetry, MLflow, and LLM evaluation frameworks.

17. How would you explain the complete AI lifecycle in an interview?
Important interview question
Suggested answer:

"I look at the AI lifecycle as an end-to-end process, starting with identifying a business problem and defining measurable success criteria.

First, I work with business stakeholders to understand the use case and requirements. Then I identify the required data sources and build the data ingestion and preprocessing pipeline.

Next, based on the problem, I select the appropriate approach. It could be a traditional ML model for prediction, an LLM for document understanding, or a RAG-based architecture for enterprise knowledge retrieval.

After development, I evaluate the solution using appropriate metrics, including accuracy, precision, recall, groundedness, and latency, depending on the use case.

Once validated, I deploy it using containerized services and CI/CD pipelines, integrate it with enterprise applications through APIs, and implement security and governance controls.

Finally, I continuously monitor application health, model quality, data drift, cost, and business outcomes. Based on the feedback, I improve the prompts, retrieval strategy, data pipeline, or model.

So, I consider AI a continuous lifecycle rather than a one-time model development exercise."

18. Quick interview practice
Let's check your understanding. Answer one question at a time, and I'll score your answer out of 10 and improve it.
Question 1 of 5
AI lifecycle



What are the major stages of the AI lifecycle?
Show answerNext question 
Start again
My recommendation: Learn the traditional ML lifecycle first, then the GenAI/RAG lifecycle, and finally MLOps and production monitoring. These are especially important for explaining how you take an AI solution from proof of concept to an enterprise production platform.












You're using the free version of ChatGPT. Upgrade for advanced models, better memory, and higher-quality image creation.
Try Plus free
