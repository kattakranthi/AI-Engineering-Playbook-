# Reranking in RAG (Retrieval-Augmented Generation)

## 1. What is Reranking?

Reranking is a process used in information retrieval to reorder retrieved documents based on how relevant they are to a user's query.

In a RAG system, the retriever initially retrieves multiple documents from a vector database.

However, the retrieved documents may not be in the best relevance order.

A reranker evaluates the relationship between the user's query and each retrieved document and assigns a relevance score.

The documents are then sorted based on these scores.

**Main objective:** Improve the relevance and quality of the context provided to an LLM.

## 2. Why Do We Need Reranking?

Vector search retrieves documents based on semantic similarity.

However, the highest semantic similarity does not always mean the document is the most relevant to the exact question.

For example:

User query:

"What is the refund policy for damaged products?"

Initial retrieval:

| Document | Description | Similarity Score |
|---|---|---:|
| Doc A | General product return policy | 0.91 |
| Doc B | Refund process for damaged products | 0.85 |
| Doc C | Product shipping information | 0.82 |
| Doc D | Damaged product replacement policy | 0.78 |

The initial retrieval ranks Doc A first.

However, Doc B is more relevant to the exact question.

A reranker evaluates the query-document relationship and reorders the documents.

Reranked results:

| Document | Description | Reranking Score |
|---|---|---:|
| Doc B | Refund process for damaged products | 0.96 |
| Doc D | Damaged product replacement policy | 0.89 |
| Doc A | General product return policy | 0.72 |
| Doc C | Product shipping information | 0.35 |

The top documents are now more relevant to the query.

*Note: These are illustrative scores, not calibrated probabilities.*

## 3. Where Does Reranking Fit in RAG?

Typical RAG architecture:

```text
User Query
    |
    v
Query Embedding
    |
    v
Vector Database
    |
    v
Initial Retrieval
    |
    v
Top-K Documents
    |
    v
Reranking Model
    |
    v
Sort by Relevance
    |
    v
Select Top-N Documents
    |
    v
LLM
    |
    v
Generated Answer
```

Explanation:

1. User submits a query.
2. The query is converted into an embedding.
3. Vector database retrieves relevant document chunks.
4. Initial retrieval returns Top-K documents.
5. Reranker evaluates each query-document pair.
6. Documents are reordered based on relevance.
7. The best Top-N documents are selected.
8. Selected documents are passed to the LLM as context.
9. LLM generates a grounded response.

## 4. What is a Retrieval Model?

A retrieval model finds potentially relevant documents from a large collection.

Common retrieval techniques:

- Dense vector retrieval.
- Sparse keyword retrieval.
- Hybrid retrieval.

### Dense Retrieval

Uses embeddings to identify semantically similar documents.

Example:

Query: "How can I reset my password?"

A dense retriever may find:

"Steps to recover account credentials."

Even though the wording differs, the meaning is similar.

### Sparse Retrieval

Uses keyword matching techniques such as BM25.

Example:

Query: "Password reset"

The retriever searches for documents containing relevant terms.

### Hybrid Retrieval

Combines dense retrieval and sparse retrieval.

It can capture both semantic meaning and exact keyword matches.

## 5. What is a Reranking Model?

A reranking model evaluates each retrieved document against the original user query.

It determines which documents are most relevant to the query.

Unlike the initial retriever, which efficiently searches a large collection, a reranker performs a more detailed relevance evaluation on a smaller candidate set.

Example:

```text
Query:
"What is the delivery time for international orders?"

Candidate Documents:
1. Domestic delivery time.
2. International shipping policy.
3. Product cancellation policy.
4. International delivery tracking.

Reranker:
Evaluates all four documents.

Final Ranking:
1. International shipping policy.
2. International delivery tracking.
3. Domestic delivery time.
4. Product cancellation policy.
```

## 6. What is a Cross-Encoder?

A cross-encoder is a model that takes the query and document together as input.

It jointly processes both pieces of text to determine their relevance.

Example:

```text
Query:
"What is the refund policy?"

Document:
"Customers can request a refund within 30 days."

          |
          v
    Cross-Encoder
          |
          v
   Relevance Score
```

Advantages:

- Evaluates query-document relationships deeply.
- Often provides strong reranking quality.
- Considers the interaction between query and document tokens.

Disadvantages:

- Computationally expensive.
- Slower than vector similarity search.
- Not suitable for comparing every document in a massive database.

Cross-encoders are commonly used after initial retrieval, when the candidate document set is smaller.

## 7. What is a Bi-Encoder?

A bi-encoder independently converts the query and document into embeddings.

The similarity between the two embeddings is calculated.

Example:

```text
Query
  |
  v
Query Encoder
  |
  v
Query Embedding
                 \
                  Similarity
                 /
Document
  |
  v
Document Encoder
  |
  v
Document Embedding
```

Advantages:

- Efficient retrieval.
- Documents can be embedded and indexed in advance.
- Scales to large document collections.

Disadvantages:

- Query and document are encoded independently.
- Does not jointly evaluate detailed token-level interactions.

## 8. Cross-Encoder vs Bi-Encoder

| Feature | Bi-Encoder | Cross-Encoder |
|---|---|---|
| Input | Query and document separately | Query and document together |
| Processing | Independent encoding | Joint processing |
| Speed | Faster | Slower |
| Scalability | High | Lower |
| Typical usage | Initial retrieval | Reranking |
| Relevance evaluation | Embedding similarity | Joint relevance score |

## 9. What is Top-K Retrieval?

Top-K retrieval means retrieving the K most relevant documents from a database.

Example:

If K = 10, the retrieval system returns the top 10 candidate document chunks.

A reranker then evaluates these candidates.

Example:

```text
Vector Search:
10,000 documents
       |
       v
Retrieve Top 50
       |
       v
Reranking Model
       |
       v
Select Top 5
       |
       v
LLM Context
```

The values of K and N should be selected based on relevance, latency, cost, and context-window constraints.

## 10. What is the Difference Between Top-K and Top-N?

Top-K represents the number of documents retrieved initially.

Top-N represents the number of documents retained after reranking.

Example:

```text
Initial Retrieval:
Top-K = 50

After Reranking:
Top-N = 5
```

The system retrieves 50 candidates, reranks them, and selects the best 5.

Usually, N is smaller than K.

## 11. What is Hybrid Search and Reranking?

Hybrid search combines keyword-based retrieval and vector-based retrieval.

The combined candidates are passed to a reranking model.

Example:

```text
                User Query
                    |
             +------+------+
             |             |
             v             v
        Vector Search   BM25 Search
             |             |
             v             v
        Semantic Docs   Keyword Docs
             |             |
             +------+------+
                    |
                    v
              Merge Results
                    |
                    v
                 Reranker
                    |
                    v
             Final Documents
                    |
                    v
                   LLM
```

Hybrid retrieval helps identify documents through both semantic relevance and exact keyword matching.

A fusion method such as Reciprocal Rank Fusion (RRF) can combine ranked lists before reranking.

## 12. What is Reciprocal Rank Fusion (RRF)?

RRF is a technique used to combine results from multiple ranked retrieval systems.

For example:

- Vector search returns one ranked list.
- BM25 returns another ranked list.
- RRF combines their rankings.

Formula:

RRF(d) = Sum [1 / (k + rank(d))]

Where:

- d = Document.
- k = Constant used to reduce the influence of high ranks.
- rank(d) = Position of the document in a retrieval list.

A document appearing near the top of multiple lists receives a higher combined score.

## 13. What is a Reranking Score?

A reranking score represents how relevant a document is to a particular query according to the reranking model.

Example:

| Document | Score |
|---|---:|
| Document A | 0.97 |
| Document B | 0.89 |
| Document C | 0.45 |

The system ranks Document A first.

Important:

A reranking score is not necessarily a probability or a confidence score. Its meaning and scale depend on the model.

## 14. What are the Benefits of Reranking?

1. Improves document relevance.
2. Reduces irrelevant context.
3. Helps improve answer grounding.
4. Improves context selection.
5. Can reduce irrelevant information passed to an LLM.
6. Helps improve retrieval quality.

Reranking does not guarantee factual answers or eliminate hallucinations.

The quality of the source documents and generation process still matters.

## 15. What are the Disadvantages of Reranking?

1. Adds computational overhead.
2. Increases retrieval latency.
3. May increase infrastructure costs.
4. Requires additional model execution.
5. Can still rank incorrect or incomplete documents highly.

A good system balances retrieval quality, latency, and cost.

## 16. What is the Difference Between Retrieval and Reranking?

| Feature | Retrieval | Reranking |
|---|---|---|
| Purpose | Find candidate documents | Reorder candidate documents |
| Input | User query | User query + retrieved documents |
| Search scope | Large document collection | Smaller candidate collection |
| Common technique | Vector search, BM25 | Cross-encoder |
| Output | Initial Top-K results | Final ranked results |
| Main focus | Candidate discovery | Relevance refinement |

## 17. What is a Reranker Model Example?

Some commonly used reranking models and services include:

- BGE Reranker.
- Cohere Rerank.
- Jina Reranker.
- Cross-encoder models from Sentence Transformers.

These models can be integrated into RAG pipelines after the retrieval stage.

## 18. How Do We Evaluate Reranking Quality?

Common evaluation metrics:

### Mean Reciprocal Rank (MRR)

Measures how highly the first relevant result appears in a ranked list.

MRR = Average of (1 / rank of first relevant result)

Example:

If the first relevant document appears at position 2:

Reciprocal Rank = 1 / 2 = 0.5

### Normalized Discounted Cumulative Gain (NDCG)

Measures ranking quality while considering the relevance of documents and their positions.

Highly relevant documents appearing near the top receive greater importance.

### Precision@K

Measures the proportion of relevant documents among the top K results.

Precision@K = Relevant documents in Top-K / K

### Recall@K

Measures the proportion of all relevant documents retrieved in the top K results.

Recall@K = Relevant documents in Top-K / Total relevant documents

## 19. Example Python Reranking Concept

The following is a simplified conceptual example using a cross-encoder model.

```python
from sentence_transformers import CrossEncoder

model = CrossEncoder(
    "cross-encoder/ms-marco-MiniLM-L-6-v2"
)

query = "What is the refund policy?"

documents = [
    "Customers can return products within 30 days.",
    "International shipping takes five days.",
    "Refunds are available for eligible purchases.",
    "Products are available in different sizes."
]

pairs = [(query, doc) for doc in documents]

scores = model.predict(pairs)

ranked_documents = sorted(
    zip(documents, scores),
    key=lambda x: x[1],
    reverse=True
)

for document, score in ranked_documents:
    print(f"Score: {score:.4f} | {document}")
```

The model calculates relevance scores for query-document pairs and sorts the documents from highest to lowest score.

The scores are model-specific relevance scores, not necessarily probabilities.

## 20. Complete RAG Pipeline with Reranking

```text
              User Query
                  |
                  v
             Query Embedding
                  |
                  v
            Vector Database
                  |
                  v
            Initial Retrieval
                  |
                  v
              Top 50 Docs
                  |
                  v
             Cross-Encoder
                  |
                  v
               Reranking
                  |
                  v
             Select Top 5
                  |
                  v
            Context Assembly
                  |
                  v
           Prompt Construction
                  |
                  v
                  LLM
                  |
                  v
           Generated Response
                  |
                  v
          Output Validation
                  |
                  v
              Final Answer
```

## 21. Quick Revision

| Concept | Meaning |
|---|---|
| Retrieval | Finds relevant candidate documents |
| Reranking | Reorders retrieved documents |
| Bi-Encoder | Independently encodes query and document |
| Cross-Encoder | Jointly evaluates query and document |
| Top-K | Initial candidate document count |
| Top-N | Final selected document count |
| Hybrid Search | Combines vector and keyword search |
| BM25 | Keyword-based retrieval |
| RRF | Combines multiple ranked lists |
| MRR | Measures position of first relevant result |
| NDCG | Evaluates ranking quality |
| Precision@K | Relevant results among Top-K |
| Recall@K | Retrieved relevant results divided by all relevant results |

## 22. Key Takeaway

Retrieval finds potentially relevant documents.

Reranking identifies and prioritizes the most relevant documents from the retrieved candidates.

The purpose of reranking is to provide better-quality context to the LLM, improving the relevance and grounding of generated responses.
