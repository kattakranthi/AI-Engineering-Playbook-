1. Vector search fundamentals
Q1. What is vector search?
Vector search is a technique used to find information based on semantic meaning rather than exact keyword matching.

It converts text, images, or other data into numerical vectors called embeddings and compares those vectors to find similar content.

Example:

User asks: "How do employees protect customer information?"

A document says: "Staff must safeguard confidential client data."

A keyword search may not find an exact match. Vector search can identify that both sentences have similar meanings.
Q2. What is an embedding?
An embedding is a numerical representation of data in a multidimensional vector space.

Example:
text = "Financial risk management"embedding = [0.12, 0.45, 0.78, 0.23, 0.91]



In production, embeddings typically contain hundreds or thousands of dimensions, depending on the model.

Similar meanings tend to produce vectors that are close together in the embedding space.
Q3. What is the difference between keyword search and vector search?
Keyword search	Vector search
Matches exact words or terms	Matches semantic meaning
Often uses inverted indexes	Uses vector indexes
Good for exact identifiers	Good for natural language
BM25 is a common approach	Cosine similarity is common
May miss synonyms	Can identify related wording

Example:
- Query: "car"
- Document: "automobile"
Keyword search may not match.
Vector search may find the semantic relationship.
Q4. What is cosine similarity?
Cosine similarity measures the angle between two vectors.

It is commonly used to determine how similar two embeddings are.
\[
\text{Cosine Similarity} =
\frac{A \cdot B}{\|A\|\|B\|}
\]

Typical interpretation:
- 1: Same direction.
- 0: Orthogonal vectors.
- -1: Opposite direction.
For many text embedding models, similarity is often between 0 and 1 in practical retrieval examples, but the mathematical range is -1 to 1.
Q5. What is Euclidean distance?
Euclidean distance measures the straight-line distance between two vectors.
\[
d(A,B)=\sqrt{\sum_{i=1}^{n}(A_i-B_i)^2}
\]

Smaller distance means closer vectors.
Q6. What is dot product similarity?
Dot product multiplies corresponding vector dimensions and sums the results.
\[
A \cdot B = \sum_{i=1}^{n} A_iB_i
\]

It is commonly used in vector search. Its value can depend on vector magnitude, unlike cosine similarity.
Q7. What is the difference between cosine similarity, Euclidean distance, and dot product?
Metric	Measures	Better score
Cosine similarity	Direction / angle	Higher
Euclidean distance	Straight-line distance	Lower
Dot product	Alignment and magnitude	Higher

The appropriate metric depends on the embedding model and whether its vectors are normalized.
2. Vector database and indexing
Q8. What is a vector database?
A vector database stores embeddings and supports efficient similarity searches.

Popular options include:
- Milvus.
- Pinecone.
- Weaviate.
- Chroma.
- Qdrant.
- Azure AI Search.
Q9. Why can't we use a normal SQL database for vector search?
Traditional relational databases are optimized for structured data and relational queries.

Vector databases provide specialized vector indexing and similarity-search capabilities.

However, modern databases such as PostgreSQL with pgvector can also support vector search. The decision depends on scale, latency, filtering, operations, and workload.
Q10. What is an index in a vector database?
An index is a data structure that makes vector similarity searches faster.

Without an index, the database may need to compare the query vector against every stored vector.

This is called brute-force or exact search.
Q11. What is ANN?
ANN stands for Approximate Nearest Neighbor.

It finds vectors that are very close to the query vector without necessarily calculating exact distances against every stored vector.

Advantages:
- Faster retrieval.
- Efficient at large scale.
- Lower search cost for many workloads.
Trade-off: It may not always return the mathematically exact nearest neighbors.
Q12. What are the different vector indexing algorithms?










1. HNSW – Hierarchical Navigable Small World

Graph-based index that navigates between nearby vectors. Often provides excellent retrieval speed and recall.













2. IVF – Inverted File Index

Divides vectors into clusters and searches selected clusters rather than the entire dataset.













3. PQ – Product Quantization

Compresses vectors to reduce memory requirements. Often used with IVF.













4. FLAT – Exact Search

Compares the query against every vector. Accurate, but can be expensive for large datasets.



Q13. What is HNSW and how does it work?
HNSW builds a multi-layer graph of vectors.
- Upper layers provide long-distance navigation.
- Lower layers provide more detailed local navigation.
- The search moves through the graph toward vectors closest to the query.
Important configuration parameters:
Parameter	Purpose
M	Number of graph connections
efConstruction	Search width during index construction
efSearch	Search width during query execution

Increasing search effort can improve recall but may increase latency.
Q14. What is the difference between HNSW and IVF?
HNSW	IVF
Graph-based index	Cluster-based index
Navigates neighboring vectors	Searches selected clusters
Often strong recall and low latency	Can be memory-efficient and scalable
Can require substantial memory	Performance depends on cluster selection
Good for many interactive workloads	Useful for large-scale workloads

There is no universally best index. Benchmark using your actual dataset and queries.
3. RAG and vector retrieval
Q15. How does vector search work in a RAG application?
Example: An enterprise financial services chatbot.
1. Admin uploads PDF documents.
2. Document parser extracts text.
3. Text is split into chunks.
4. Embedding model converts chunks into vectors.
5. Vectors and metadata are stored in Milvus.
6. Employee asks a question.
7. The question is converted into an embedding.
8. Vector search retrieves similar chunks.
9. Retrieved content is passed to the LLM.
10. LLM generates an answer with citations.
#chatgpt-mermaid-_r_241_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-_r_241_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_241_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-_r_241_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-_r_241_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-_r_241_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-_r_241_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-_r_241_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-_r_241_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-_r_241_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-_r_241_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_241_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_241_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-_r_241_ p{margin:0;}#chatgpt-mermaid-_r_241_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-_r_241_ .label text,#chatgpt-mermaid-_r_241_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ .node rect,#chatgpt-mermaid-_r_241_ .node circle,#chatgpt-mermaid-_r_241_ .node ellipse,#chatgpt-mermaid-_r_241_ .node polygon,#chatgpt-mermaid-_r_241_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-_r_241_ .rough-node .label text,#chatgpt-mermaid-_r_241_ .node .label text,#chatgpt-mermaid-_r_241_ .image-shape .label,#chatgpt-mermaid-_r_241_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-_r_241_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-_r_241_ .rough-node .label,#chatgpt-mermaid-_r_241_ .node .label,#chatgpt-mermaid-_r_241_ .image-shape .label,#chatgpt-mermaid-_r_241_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-_r_241_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-_r_241_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_241_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-_r_241_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-_r_241_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-_r_241_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_241_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-_r_241_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_241_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-_r_241_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-_r_241_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-_r_241_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-_r_241_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-_r_241_ .icon-shape,#chatgpt-mermaid-_r_241_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-_r_241_ .icon-shape p,#chatgpt-mermaid-_r_241_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-_r_241_ .icon-shape .label rect,#chatgpt-mermaid-_r_241_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-_r_241_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-_r_241_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-_r_241_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-_r_241_ [data-look="neo"].node rect,#chatgpt-mermaid-_r_241_ [data-look="neo"].cluster rect,#chatgpt-mermaid-_r_241_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-_r_241_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_241_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-_r_241_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-_r_241_-gradient);stroke-width:1px;}#chatgpt-mermaid-_r_241_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_241_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-_r_241_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-_r_241_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_241_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-_r_241_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-_r_241_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_241_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-_r_241_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-_r_241_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_241_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-_r_241_ .node tspan[font-weight="normal"],#chatgpt-mermaid-_r_241_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-_r_241_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-_r_241_ .node rect,#chatgpt-mermaid-_r_241_ .node circle,#chatgpt-mermaid-_r_241_ .node ellipse,#chatgpt-mermaid-_r_241_ .node polygon,#chatgpt-mermaid-_r_241_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-_r_241_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-_r_241_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-_r_241_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-_r_241_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-_r_241_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Enterprise DocumentsDocument ParserChunkingEmbedding ModelVector DatabaseUser QuestionQuery EmbeddingVector Similarity SearchTop-K Retrieved ChunksLLM + ContextGrounded Answer + Citations





Q16. What is Top-K retrieval?
Top-K retrieval returns the K most similar chunks for a query.

Example:

If K = 5, the system returns the five most relevant chunks.

Choosing K involves a trade-off:
- Too small: important context may be missed.
- Too large: irrelevant context, token consumption, and latency can increase.
Q17. What is a similarity threshold?
A similarity threshold is a cutoff used to decide whether a retrieved result is sufficiently relevant.

Example:
if similarity_score >= 0.75:    use_document()else:    abstain()



The value 0.75 is illustrative, not a universal threshold. It must be calibrated using evaluation data and the selected similarity metric.
Q18. What is metadata filtering?
Metadata filtering restricts vector retrieval using additional fields.

Example:
{    "tenant_id": "tenant-001",    "department": "compliance",    "document_version": 2,    "access_group": "legal"}



If a compliance employee asks a question, retrieval can be restricted to documents the employee is authorized to access.

Important: Authorization must be enforced by trusted backend logic, not by relying on the LLM.
Q19. What is hybrid search?
Hybrid search combines:
1. Keyword search.
2. Vector search.
Example:

An employee asks:

"What is policy AML-2026-001?"

Keyword search finds the exact policy identifier. Vector search retrieves semantically related policy content.

The results can be combined using Reciprocal Rank Fusion (RRF) or another ranking method.
Q20. What is reranking?
Reranking improves retrieval quality by reordering the initial search results.

Typical architecture:
User Question
     |
     v
Vector + Keyword Search
     |
     v
Top 20 Candidates
     |
     v
Reranking Model
     |
     v
Top 5 Relevant Chunks
     |
     v
LLM


The first-stage retriever is optimized for speed and recall. The reranker spends more computation evaluating relevance.
4. Embeddings and advanced concepts
Q21. What is the difference between query embedding and document embedding?
Both convert text into vectors, but they represent different inputs.
- Document embedding: Converts document chunks into vectors during ingestion.
- Query embedding: Converts a user question into a vector during retrieval.
They must be compatible with the same embedding space. Some retrieval models use separate query and document encoders designed to work together.
Q22. What happens if we change the embedding model?
The new embedding model may produce vectors in a different semantic space or dimension.

You generally cannot safely compare new query embeddings with old document embeddings.

You should:
1. Select the new embedding model.
2. Reprocess document chunks.
3. Generate new embeddings.
4. Build or update the vector index.
5. Validate retrieval quality.
6. Switch traffic after testing.
Q23. What is vector dimensionality?
Dimensionality refers to the number of numerical values in an embedding vector.

For example:
embedding = [0.12, 0.45, 0.78, 0.23]



This example has four dimensions.

Real embedding models may generate hundreds or thousands of dimensions.

Higher dimensionality does not automatically mean better retrieval quality.
Q24. What is vector quantization?
Vector quantization compresses vectors to reduce memory and storage requirements.

For example, instead of storing every vector as 32-bit floating-point values, a compressed representation can use fewer bits.

Trade-off:
- Less memory.
- Potentially faster search.
- Possible loss of precision and recall.
Q25. What is Graph RAG, and how is it different from vector RAG?
Vector RAG	Graph RAG
Finds semantically similar chunks	Finds entities and relationships
Uses embeddings	Uses graph structures
Good for document Q&A	Good for relationship-based questions
Retrieves nearest chunks	Traverses connected entities

Example:

Vector RAG: "Explain the investment policy."

Graph RAG: "Which funds are managed by a particular manager and exposed to a particular risk?"

Graph RAG can complement vector retrieval rather than replace it.
5. Production-level architecture questions
Q26. How do you improve vector search performance?
My approach would be:
1. Select an appropriate index such as HNSW or IVF.
2. Tune index parameters.
3. Use metadata filtering.
4. Optimize chunk size and overlap.
5. Use hybrid search.
6. Add reranking where justified.
7. Cache frequent queries when safe.
8. Measure recall, latency, and throughput.
9. Scale the vector database based on workload.
Q27. How do you evaluate vector search quality?
Common metrics include:
Metric	Meaning
Precision@K	How many retrieved results are relevant
Recall@K	How many relevant results were retrieved
MRR	How highly the first relevant result is ranked
NDCG@K	Quality of the ranking, considering relevance
Latency	Time required for retrieval

For example, if 10 documents are retrieved and 8 are relevant:
\[
Precision@10 = \frac{8}{10}=0.8
\]

Q28. How do you handle millions of embeddings?
For millions of vectors:
- Choose a suitable ANN index.
- Partition or shard based on workload and tenant requirements.
- Use metadata indexes.
- Tune memory and disk usage.
- Consider quantization.
- Benchmark ingestion throughput and search latency.
- Monitor recall and index health.
Q29. How do you handle document updates and deletions?
Document update:
1. Create a new document version.
2. Parse and chunk the new content.
3. Generate new embeddings.
4. Update vector records.
5. Update SQL metadata.
6. Make the new version searchable only when indexing is complete.
7. Retire the previous version.
Document deletion:
1. Mark the document as unavailable.
2. Prevent retrieval immediately.
3. Delete associated vector chunks.
4. Remove or expire the original file according to retention rules.
5. Record the deletion in audit logs.
Q30. Design vector search for a financial institution.

"I would design a hybrid vector retrieval architecture for the financial institution.

During ingestion, documents are uploaded to object storage, parsed, and divided into meaningful chunks. An embedding model converts those chunks into vectors, which are stored in Milvus along with metadata such as document ID, tenant ID, version, page number, and access groups.

When an employee asks a question, the backend authenticates the employee and resolves their permissions. The retrieval service generates a query embedding and performs hybrid search using vector similarity and keyword matching.

I would apply metadata filters to ensure users only retrieve authorized documents. Then I would use a reranking model to improve relevance and send the top results to the LLM through the enterprise LLM gateway.

Finally, I would return an answer with citations and measure retrieval quality using precision, recall, and ranking metrics. I would also monitor latency, throughput, and cost."

6. Quick revision sheet
Concept	Remember
Embedding	Text converted into numerical vector
Vector search	Semantic similarity search
Cosine similarity	Measures angle between vectors
Euclidean distance	Measures straight-line distance
Dot product	Vector alignment and magnitude
ANN	Approximate nearest neighbors
HNSW	Graph-based index
IVF	Cluster-based index
PQ	Vector compression
Top-K	Number of retrieved results
Hybrid search	Keyword + vector
Reranking	Reorder retrieved results
Metadata filtering	Filter by tenant, version, access
Graph RAG	Entity and relationship retrieval
Precision@K	Relevance of retrieved results
Recall@K	Coverage of relevant results
