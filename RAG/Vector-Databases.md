Vector databases are used to store and search embeddings (numerical representations of text, images, or other data) based on semantic similarity.

For AI applications such as your BlackRock Red Oak compliance agent and MyLifePath chatbot, vector databases help retrieve the most relevant documents before sending context to an LLM.

There isn't one universally best vector database. The right choice depends on data volume, latency, filtering, cloud ecosystem, security, scalability, and operational requirements.
1. Popular vector databases
Here are the major options worth knowing for AI architecture interviews.
1. Popular vector databases

1. Pinecone
Best for managed services
- Fully managed cloud vector database.
- Supports dense and sparse vector search.
- Metadata filtering and hybrid retrieval.
- Less infrastructure to manage.
Use when: You want production RAG without operating your own vector database cluster.

2. Milvus
Best for large-scale vector workloads
- Open-source and distributed architecture.
- Supports HNSW, IVF and DiskANN indexing options.
- Designed for large vector collections.
- Supports filtering and hybrid search.
Use when: You have millions or billions of vectors and need horizontal scalability.

3. Qdrant
- Written in Rust.
- Strong metadata/payload filtering.
- Dense, sparse and hybrid search.
- Cloud or self-hosted deployment.
Use when: You need fast similarity search with complex metadata filters.

4. Weaviate
- Supports vector and keyword search.
- Built-in hybrid search capabilities.
- Metadata filtering.
- Cloud and self-hosted options.
Use when: You need semantic search combined with keyword search and filtering.

5. Chroma
- Simple Python integration.
- Easy to get started.
- Useful for local development and prototypes.
Use when: You are developing a small RAG application or proof of concept.

6. pgvector
- PostgreSQL extension.
- Stores embeddings alongside relational data.
- Supports HNSW and IVFFlat indexes.
- SQL filtering and joins.
Use when: You already use PostgreSQL and don't want another database.

7. Elasticsearch / OpenSearch
- Traditional full-text search.
- Vector similarity search.
- Hybrid keyword + semantic retrieval.
- Filtering and distributed search.
Use when: Your organization already uses search infrastructure and wants to add vector search.

8. Azure AI Search
- Managed Azure search service.
- Keyword, vector and hybrid search.
- Integrated Azure ecosystem.
- Useful for enterprise RAG applications.
Use when: Your enterprise primarily runs on Azure.

Other options worth knowing: Redis, MongoDB Atlas Vector Search, Vespa, and FAISS. FAISS is a vector-search library, not a complete database. 
n8n Blog+2

2. Comparison table
Database	Main strength	Deployment
Pinecone	Managed RAG	Cloud
Milvus	Very large-scale search	Self-hosted / managed
Qdrant	Fast filtered retrieval	Both
Weaviate	Hybrid search	Both
Chroma	Simplicity	Local / cloud
pgvector	SQL + vectors	PostgreSQL
Elasticsearch	Full-text + vector	Both
Azure AI Search	Azure integration	Managed Azure


n8n Blog+2
3. Which one is better? My recommendations
1
Milvus

Large-scale enterprise AI and distributed vector workloads



2
Pinecone

Managed production RAG with minimal infrastructure operations



3
Qdrant

Metadata-heavy retrieval and performance



4
Weaviate

Hybrid keyword and semantic search



5
pgvector

Applications already using PostgreSQL



6
Azure AI Search

Azure-native enterprise AI applications



7
Chroma
