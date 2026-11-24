# Agentic Document Classification Using LangGraph

## 1. What Is Agentic Document Classification?

Agentic Document Classification uses multiple specialized AI components or agents to analyze, classify, validate, and route documents automatically.

Unlike traditional classification, where a single ML model predicts a document category, an agentic workflow can perform multiple reasoning and validation steps.

For example, when a financial institution receives a document:

1. An ingestion agent receives the document.
2. An extraction agent extracts text using OCR or document parsing.
3. A classification agent identifies the document category.
4. A validation agent checks whether the predicted category is appropriate.
5. A confidence evaluation step determines whether human review is required.
6. A routing agent sends the document to the appropriate business workflow.

LangGraph orchestrates these steps using state, nodes, and edges.

## 2. Business Problem

Imagine a financial institution receives 10,000 documents daily.

Documents include:
- Invoices
- Loan applications
- Bank statements
- Insurance policies
- Compliance documents
- Investment reports

Manually reviewing and categorizing these documents is expensive and time-consuming.

We want to build an agentic AI system that automatically classifies documents and routes them to the appropriate departments.

### Business Requirements

1. Automatically identify document categories.
2. Extract information from PDF and scanned documents.
3. Validate classification results.
4. Handle unknown document types.
5. Support human review for uncertain predictions.
6. Route documents to appropriate business workflows.
7. Maintain audit logs and processing history.

## 3. High-Level Agentic Architecture

```text
                Document Upload
                       |
                       v
                Ingestion Agent
                       |
                       v
                Extraction Agent
                 (OCR / Parser)
                       |
                       v
              Classification Agent
                       |
                       v
                Validation Agent
                       |
                       v
               Confidence Check
                       |
                 +-----+-----+
                 |           |
                 v           v
              High         Low
            Confidence  Confidence
                 |           |
                 v           v
             Routing      Human Review
              Agent           |
                 |            |
                 |            v
                 |       Approved /
                 |       Corrected
                 |            |
                 +-----+------+
                       |
                       v
                Business Workflow
                       |
                       v
                  Audit Log
```

## 4. Why LangGraph?

LangGraph provides an orchestration framework for building stateful and multi-step agent workflows.

It allows us to:
- Define specialized processing nodes.
- Maintain shared workflow state.
- Implement conditional routing.
- Execute nodes sequentially or in parallel.
- Handle retries and failures.
- Pause workflows for human approval.
- Persist workflow state and resume execution.

### LangGraph Core Components

| Component | Responsibility |
|---|---|
| State | Maintains document and workflow information |
| Nodes | Execute individual processing steps |
| Edges | Connect processing steps |
| Conditional Edges | Decide the next step based on results |
| Checkpointer | Persists workflow state |
| Interrupt | Pauses execution for human input |

## 5. Define the Individual Agents

### Agent 1: Document Ingestion Agent

**Responsibility:** Receive and validate uploaded documents.

Tasks:
- Accept PDF, DOCX, TIFF, and image files.
- Validate file type and size.
- Generate a unique document ID.
- Store the original document in object storage.
- Record tenant ID and metadata.

Example:

```python
def ingestion_agent(state):

    document = state["document"]

    if not document:
        return {
            "status": "failed",
            "error": "Document is missing"
        }

    return {
        "document_id": "DOC-1001",
        "status": "ingested"
    }
```

### Agent 2: Document Extraction Agent

**Responsibility:** Extract text and relevant information from documents.

Tasks:
- Identify digital versus scanned PDFs.
- Use PDF parsers for digital documents.
- Use OCR for scanned documents.
- Extract text, tables, and metadata.
- Validate extraction quality.

Example:

```python
def extraction_agent(state):

    document = state["document"]

    extracted_text = extract_document_text(document)

    return {
        "extracted_text": extracted_text,
        "extraction_status": "success"
    }
```

Possible technologies:
- Azure AI Document Intelligence
- AWS Textract
- Google Document AI
- Tesseract OCR
- PyMuPDF

### Agent 3: Document Classification Agent

**Responsibility:** Identify the document category using an LLM or ML model.

Supported categories:

- Invoice
- Loan Application
- Bank Statement
- Insurance Policy
- Compliance Document
- Investment Report
- Unknown

Example prompt:

```text
You are an enterprise document classification agent.

Classify the document into one of these categories:

1. Invoice
2. Loan Application
3. Bank Statement
4. Insurance Policy
5. Compliance Document
6. Investment Report
7. Unknown

Analyze the document content.

Return structured output:

{
  "category": "Invoice",
  "reason": "Contains invoice number and payment details",
  "confidence": 0.95
}

Document:
{extracted_text}
```

Example output:

```json
{
  "category": "Invoice",
  "reason": "Contains vendor information, invoice number and payment details",
  "confidence": 0.95
}
```

Note: The confidence score must be calibrated or independently evaluated before using it for automated decisions.

### Agent 4: Classification Validation Agent

**Responsibility:** Validate whether the predicted category is supported by the document.

Example:

The classification agent predicts:

```text
Category: Invoice
```

The validation agent checks:
- Does the document contain invoice-related information?
- Are vendor details present?
- Is there an invoice number?
- Does the document match the expected category definition?

Example:

```python
def validation_agent(state):

    category = state["predicted_category"]
    extracted_text = state["extracted_text"]

    # Simplified example only.
    if category == "Invoice" and "invoice" in extracted_text.lower():
        return {
            "validation_status": "valid"
        }

    return {
        "validation_status": "needs_review"
    }
```

For production, use category-specific validation rules and independently evaluated semantic checks rather than relying on one keyword.

### Agent 5: Confidence Evaluation Agent

**Responsibility:** Decide whether the document can be automatically processed.

Example:

| Confidence | Decision |
|---|---|
| 0.90–1.00 | Eligible for automatic routing |
| 0.70–0.89 | Additional validation |
| Below 0.70 | Human review |

These thresholds are illustrative and should be established using business risk and evaluation results.

Example:

```python
def confidence_agent(state):

    confidence = state["confidence"]

    if confidence >= 0.90:
        return {"decision": "auto_approve"}

    elif confidence >= 0.70:
        return {"decision": "additional_validation"}

    return {"decision": "human_review"}
```

### Agent 6: Human Review Agent

**Responsibility:** Handle uncertain classifications or cases requiring approval.

A human reviewer can:
- Accept the predicted category.
- Correct the category.
- Reject the classification.
- Request additional document processing.

Example:

```text
Document: DOC-1001

Predicted Category: Insurance Policy

Confidence: 0.62

Human Reviewer:

[Approve] [Correct] [Reject]
```

LangGraph supports interrupt-based human-in-the-loop workflows.

The workflow can pause, preserve state, and resume after receiving human input.

### Agent 7: Document Routing Agent

**Responsibility:** Route approved documents to the correct business process.

Example:

```python
def routing_agent(state):

    category = state["final_category"]

    routing_map = {
        "Invoice": "accounts_payable",
        "Loan Application": "loan_processing",
        "Insurance Policy": "insurance_operations",
        "Compliance Document": "compliance_team",
        "Investment Report": "investment_research"
    }

    destination = routing_map.get(category, "manual_review")

    return {
        "destination": destination,
        "status": "routed"
    }
```

## 6. LangGraph Workflow Implementation

### Step 1: Install Dependencies

```bash
pip install -U langgraph langchain langchain-openai
```

### Step 2: Define Shared State

LangGraph uses shared state to pass information between nodes.

```python
from typing import TypedDict, Optional


class DocumentState(TypedDict):

    document: str
    document_id: Optional[str]

    extracted_text: Optional[str]

    predicted_category: Optional[str]
    confidence: float

    validation_status: Optional[str]

    decision: Optional[str]

    final_category: Optional[str]

    destination: Optional[str]

    status: Optional[str]

    error: Optional[str]
```

### Step 3: Define Processing Nodes

```python
def ingestion_node(state):

    return ingestion_agent(state)


def extraction_node(state):

    return extraction_agent(state)


def classification_node(state):

    # Call the configured LLM with structured output.
    result = classify_document(
        state["extracted_text"]
    )

    return {
        "predicted_category": result["category"],
        "confidence": result["confidence"]
    }


def validation_node(state):

    return validation_agent(state)


def confidence_node(state):

    return confidence_agent(state)


def human_review_node(state):

    # Human approval is implemented separately
    # using LangGraph interrupt().
    return human_review(state)


def routing_node(state):

    return routing_agent(state)
```

The `classify_document`, `extract_document_text`, and `human_review` functions represent integrations that must be implemented using your selected services.

### Step 4: Define Conditional Routing

LangGraph supports conditional edges to decide which node executes next.

```python
def route_after_confidence(state):

    if state["decision"] == "auto_approve":
        return "routing"

    if state["decision"] == "additional_validation":
        return "validation"

    return "human_review"


def route_after_validation(state):

    if state["validation_status"] == "valid":
        return "routing"

    return "human_review"
```

### Step 5: Build the LangGraph

```python
from langgraph.graph import StateGraph, START, END

workflow = StateGraph(DocumentState)

# Register nodes
workflow.add_node("ingestion", ingestion_node)
workflow.add_node("extraction", extraction_node)
workflow.add_node("classification", classification_node)
workflow.add_node("validation", validation_node)
workflow.add_node("confidence", confidence_node)
workflow.add_node("human_review", human_review_node)
workflow.add_node("routing", routing_node)

# Define workflow
workflow.add_edge(START, "ingestion")

workflow.add_edge("ingestion", "extraction")

workflow.add_edge("extraction", "classification")

workflow.add_edge("classification", "confidence")

workflow.add_conditional_edges(
    "confidence",
    route_after_confidence,
    {
        "routing": "routing",
        "validation": "validation",
        "human_review": "human_review"
    }
)

workflow.add_conditional_edges(
    "validation",
    route_after_validation,
    {
        "routing": "routing",
        "human_review": "human_review"
    }
)

workflow.add_edge("human_review", "routing")

workflow.add_edge("routing", END)

# Compile graph
app = workflow.compile()
```

This example shows the orchestration structure. For production, implement the human-review node with an interrupt and a checkpointer, and route rejected documents to a separate rejection or reprocessing path.

### Step 6: Execute the Workflow

```python
initial_state = {
    "document": "invoice_1001.pdf",
    "document_id": None,
    "extracted_text": None,
    "predicted_category": None,
    "confidence": 0.0,
    "validation_status": None,
    "decision": None,
    "final_category": None,
    "destination": None,
    "status": None,
    "error": None
}

result = app.invoke(initial_state)

print(result)
```

## 7. Human-in-the-Loop Using LangGraph

For production workflows, use `interrupt()` and a checkpointer.

Example:

```python
from langgraph.types import interrupt, Command
from langgraph.checkpoint.memory import InMemorySaver

def human_review_node(state):

    reviewer_decision = interrupt({
        "document_id": state["document_id"],
        "predicted_category": state["predicted_category"],
        "confidence": state["confidence"],
        "message": "Please approve or correct the category"
    })

    if reviewer_decision["action"] == "approve":

        return {
            "final_category": state["predicted_category"],
            "status": "approved"
        }

    if reviewer_decision["action"] == "correct":

        return {
            "final_category": reviewer_decision["category"],
            "status": "corrected"
        }

    return {
        "status": "rejected"
    }


checkpointer = InMemorySaver()

app = workflow.compile(
    checkpointer=checkpointer
)
```

To start the workflow:

```python
config = {
    "configurable": {
        "thread_id": "DOC-1001"
    }
}

result = app.invoke(initial_state, config=config)
```

The workflow pauses when it reaches the interrupt.

After a reviewer responds, resume it:

```python
result = app.invoke(
    Command(
        resume={
            "action": "approve"
        }
    ),
    config=config
)
```

For production, replace the in-memory checkpointer with a durable database-backed checkpointer.

Also add a conditional edge after human review so rejected documents do not proceed to normal business routing.

## 8. Complete Workflow Diagram

```text
START
  |
  v
Ingestion Agent
  |
  v
Extraction Agent
  |
  v
Classification Agent
  |
  v
Confidence Evaluation
  |
  +---------------------------+
  |              |            |
  v              v            v
High           Medium         Low
  |              |            |
  v              v            v
Routing       Validation   Human Review
                 |            |
             +---+---+        |
             |       |        |
             v       v        |
           Valid  Invalid     |
             |       |        |
             |       v        |
             |   Human Review |
             |       |        |
             +-------+--------+
                     |
                     v
               Final Decision
                     |
                     v
               Business Routing
                     |
                     v
                    END
```

## 9. Production Considerations

### Security
- Authenticate users.
- Enforce tenant-level isolation.
- Validate document access permissions.
- Protect sensitive information and PII.
- Treat uploaded documents as untrusted input.
- Protect against prompt injection.

### Reliability
- Implement retries with exponential backoff.
- Use timeouts for LLM and OCR calls.
- Add circuit breakers.
- Persist workflow state.
- Support idempotent document processing.
- Use dead-letter queues for failed jobs.

### Scalability
- Use asynchronous document ingestion.
- Use message queues for high-volume processing.
- Scale extraction and classification workers independently.
- Batch embedding or classification requests where appropriate.

### Observability

Monitor:

| Metric | Purpose |
|---|---|
| Documents processed | Processing throughput |
| Classification accuracy | Model quality |
| Precision and recall | Classification performance |
| Human review rate | Automation effectiveness |
| OCR failure rate | Extraction quality |
| LLM latency | Performance |
| Workflow failure rate | Reliability |
| Cost per document | Cost management |

## 10. Evaluation Strategy

Evaluate the overall system, not just the LLM.

### Classification Evaluation
- Accuracy
- Precision
- Recall
- F1 Score
- Confusion Matrix

### Extraction Evaluation
- Field-level accuracy
- OCR accuracy
- Missing-field rate

### Agent Workflow Evaluation
- Correct routing percentage
- Validation failure rate
- Human intervention rate
- End-to-end processing time
- Workflow completion rate

### Business Evaluation
- Reduction in manual processing time
- Cost savings
- Documents processed per day
- Reduction in incorrect routing

## 11. Traditional ML vs Agentic Document Classification

| Traditional ML | Agentic AI with LangGraph |
|---|---|
| One model predicts category | Multiple specialized steps |
| Fixed prediction workflow | Conditional workflow |
| Limited workflow reasoning | LLM-driven classification and validation |
| Usually requires labeled training data | Can use predefined categories and prompts |
| Limited human review integration | Supports interrupt-based human review |
| Primarily prediction-focused | Classification plus business execution |

Important: Agentic AI is not automatically more accurate than traditional ML. Select the architecture based on complexity, volume, latency, cost, and business risk.

## 12. Interview Explanation

**Question: How would you build an agentic document classification system using LangGraph?**

My answer:

"I would design a stateful document classification workflow using LangGraph.

First, an ingestion component would receive the document, validate the file, and store it securely.

Next, an extraction agent would use OCR or document parsing to extract the content.

Then, a classification agent would use an LLM or a trained ML model to identify the document category and provide structured output.

After classification, I would have a validation step to verify whether the predicted category is supported by the document content.

I would evaluate the confidence and validation results using conditional edges in LangGraph.

For high-confidence documents, the workflow would automatically route them to the appropriate business process. For ambiguous or low-confidence documents, I would use LangGraph's human-in-the-loop capability to pause the workflow and request reviewer approval.

I would persist workflow state using a checkpointer so that processing can resume after interruptions.

Finally, I would implement tenant-level isolation, audit logging, retries, observability, and model evaluation to ensure the system is reliable and secure in production."

## 13. Key Takeaways

Remember the workflow:

1. Ingestion - Receive documents.
2. Extraction - Extract document text.
3. Classification - Identify document category.
4. Validation - Verify classification.
5. Confidence - Decide automation eligibility.
6. Human Review - Handle uncertain predictions.
7. Routing - Execute the business workflow.
8. Observability - Monitor performance and reliability.

**Golden Rule:** LangGraph orchestrates the workflow. LLMs provide language understanding and reasoning. Backend services enforce security, authorization, and business rules.

## 14. References

- [LangGraph Overview](https://docs.langchain.com/oss/python/langgraph/overview)
- [LangGraph Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api)
- [LangGraph Human-in-the-Loop](https://docs.langchain.com/oss/python/langgraph/interrupts)
- [LangGraph Quickstart](https://docs.langchain.com/oss/python/langgraph/quickstart)
- [Hugging Face Transformers](https://huggingface.co/docs/transformers)

---

**Next recommended topics:**

- Agentic Document Extraction Using LangGraph
- Agentic Document Understanding Using LangGraph
- Agentic Invoice Processing Using LangGraph
- Multi-Agent Document Processing Using LangGraph
