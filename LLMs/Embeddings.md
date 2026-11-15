What are embeddings in AI? Complete explanation for AI system design interviews
Embeddings are one of the most important concepts to understand for RAG, vector databases, semantic search, and agentic AI.

Let's break this down from basics to implementation, including:
1. What embeddings are.
2. How text is converted into embeddings.
3. Different types of embeddings.
4. Float32, Float16, and other vector data types.
5. Dense vs. sparse embeddings.
6. Dimensions and similarity calculations.
7. How embeddings work in RAG with Milvus.
8. Interview questions and answers.
1. What are embeddings?
An embedding is a numerical representation of data (text, images, audio, etc.) in a multidimensional vector space.

An embedding model converts text into a list of numbers called a vector. Text with similar meanings should have vectors that are close to each other in that vector space.

For example:
- "How do I reset my password?"
- "I forgot my password. How can I recover it?"
Although the words are different, their meanings are similar. An embedding model should generate vectors that are relatively close together.
Conceptual example of embeddings
Sentence A

"How do I reset my password?"

[0.21, 0.85, 0.42, 0.17, ...]

Sentence B

"I forgot my password. How can I recover it?"

[0.23, 0.82, 0.44, 0.19, ...]

Sentence C

"What is the weather today?"

[-0.65, 0.12, -0.31, 0.78, ...]

Illustrative numbers only, not actual model outputs. Similarity depends on all vector dimensions, not individual values.

The first two sentences should have higher semantic similarity than the third.
2. How does text become an embedding?
Consider this sentence:

"How do I withdraw money from my retirement account?"

The embedding pipeline looks like this:
Input text

"How do I withdraw money?"

Tokenization

Break text into tokens

Embedding model

Text embedding model processes tokens

Numerical vector

[0.21, 0.85, 0.42, 0.17, ...]

Vector database

Store vector + text + metadata


Important: The embedding model and the LLM are not necessarily the same model.
- Embedding model: converts data into vectors.
- LLM: generates responses using the retrieved context.
3. Different types of embeddings
There are two ways to classify embeddings:
- By what kind of data you are converting into vectors.
- By how the vectors are represented and stored.
These are different concepts. For example, a text embedding can be stored as Float32 or Float16.
A. Based on the input data
Embedding type	What it converts	Example
Text embeddings	Text into vectors	text-embedding-3-small
Word embeddings	Individual words	Word2Vec, GloVe
Sentence embeddings	Complete sentences	Sentence Transformers
Document embeddings	Paragraphs or documents	Enterprise RAG
Code embeddings	Source code	Code search
Image embeddings	Images into vectors	CLIP
Audio embeddings	Audio or speech	Audio retrieval
Video embeddings	Video frames or clips	Video search
Multimodal embeddings	Multiple modalities into a shared representation	Text + image

Let's understand the important ones.

1. Text embeddings

Convert text into numerical vectors.

Example:
- "Retirement account withdrawal"
- "How can I withdraw funds from my 401(k)?"
Used in semantic search and RAG.

2. Word embeddings

Convert individual words into vectors.

Examples:
- Word2Vec
- GloVe
- FastText
Older NLP approaches often represented individual words this way. Modern transformer embedding models generally generate contextual representations, where a word's representation depends on its surrounding text.

3. Sentence embeddings

Convert an entire sentence into one vector.

Example:

"How do I change my retirement contribution?"

The model generates one vector representing the overall meaning.

Useful for:
- Semantic search
- Question matching
- Duplicate detection
- Classification
4. Document embeddings

Represent a larger piece of content, such as a paragraph or document, using a vector.

In enterprise RAG, we generally split large documents into smaller chunks and create embeddings for each chunk rather than embedding an entire large document as one vector.

5. Image embeddings

Convert images into numerical vectors.

Example:
- Upload an image of a car.
- Generate its embedding.
- Search for visually similar cars.
6. Audio embeddings

Represent audio or speech in vector form.

Useful for:
- Voice search
- Audio similarity
- Speech retrieval
- Speaker-related tasks
7. Multimodal embeddings

Represent different modalities, such as text and images, in a shared embedding space.

For example, you can search for an image using a text query such as "a red sports car."

4. Dense embeddings vs. sparse embeddings
This is particularly important for RAG interviews.

There are two major vector representation approaches.
Dense embeddings

Most vector dimensions contain nonzero values.
Example

[0.21, 0.85, 0.42, 0.17, 0.63, 0.91]

- Captures semantic meaning.
- Commonly generated by neural embedding models.
- Used in vector databases such as Milvus.
  
  Sparse embeddings
Most dimensions are zero, with only a small number of nonzero values.
Example

[0, 0, 0.8, 0, 0, 0.5, 0, 0, 0.9]

- Often captures keyword importance.
- Can represent terms with their weights.
- Useful for exact keyword and terminology matching.

Examples of sparse retrieval:
- BM25
- SPLADE
What is hybrid search?

Hybrid search combines dense and sparse retrieval.

For example, a user searches:

- Dense search understands the meaning of the query.
- Sparse search identifies exact terms such as LifePath 2040.
- Hybrid search combines the results to improve retrieval.
For enterprise financial applications, hybrid search can be especially useful because exact financial terminology matters.

5. Float32 vs. Float16 — the two floating-point types you asked about
This is an important distinction.

An embedding vector is a list of numerical values. Those values can be stored using different numerical precision formats.

The two common floating-point formats are:
Float32 (32-bit floating point)
Higher precision
- Uses 32 bits per number.
- 4 bytes per dimension.
- Offers more numerical precision.
- Requires more memory and storage.
Example:

[0.12345678, 0.87654321, 0.34567891]

Float16 (16-bit floating point)
Lower storage
- Uses 16 bits per number.
- 2 bytes per dimension.
- Less numerical precision than Float32.
- Requires approximately half the storage.
Example:

[0.1235, 0.8765, 0.3457]

Illustrative rounded values.

Storage comparison
Let's assume you have an embedding with 1,536 dimensions.
Format	Bytes per dimension	Total vector size
Float32	4 bytes	6,144 bytes
Float16	2 bytes	3,072 bytes
Int8	1 byte	1,536 bytes
Binary	1 bit	192 bytes

Approximate raw vector storage only, excluding database and index overhead.

For 1 million vectors of 1,536 dimensions:
Format	Approximate storage
Float32	6.14 GB
Float16	3.07 GB
Int8	1.54 GB
Binary	192 MB

This is why vector quantization is useful for large-scale vector databases.
What about Int8 and Binary?
There are also compressed representations.

Int8 (8-bit integer)
- Stores values as integers from -128 to 127.
- Uses less memory than Float16 and Float32.
- Requires quantization, which maps floating-point values to integer values.
- May reduce retrieval accuracy slightly.
Binary (1-bit)
- Each dimension is represented by 0 or 1.
- Extremely compact.
- Uses Hamming distance for similarity comparisons.
- Usually involves a greater accuracy trade-off.
Important: Int8 and binary are not floating-point formats. They are quantized representations.
6. What are embedding dimensions?
Dimensions represent how many numerical values are present in an embedding vector.

For example:
embedding = [0.21, 0.85, 0.42, 0.17]



This embedding has 4 dimensions.

Real embedding models generate much larger vectors.

For example, OpenAI models:
Model	Default dimensions
text-embedding-3-small	1,536
text-embedding-3-large	3,072

You can also request reduced dimensions with supported models.

For example, you could request a 1,024-dimensional vector from text-embedding-3-large.

Why does dimension matter?
- Higher dimensions can represent richer information.
- More dimensions require more storage.
- More dimensions increase vector-processing costs.
- Reducing dimensions can improve efficiency, sometimes with a retrieval-quality trade-off.
Source: OpenAI Embeddings documentation.
7. How do we compare two embeddings?
Once we generate embeddings, we need to determine how similar they are.

Three common similarity/distance methods are:
1. Cosine similarity
Measures the angle between two vectors.
- Higher similarity means more related.
- Often used for semantic search.
- Commonly used in RAG.
2. Euclidean distance (L2)
Measures the straight-line distance between two vectors.

- Smaller distance means more similar.
3. Dot product (inner product)
Multiplies corresponding dimensions and sums the results.

- Higher scores generally indicate stronger similarity for appropriately trained and normalized vectors.

For normalized embeddings, cosine similarity and dot product give equivalent rankings.
8. How embeddings work in your RAG architecture
Let's use your Red Oak AI Compliance Agent as an example.

Suppose a compliance document contains:

"Marketing materials must include appropriate investment risk disclosures."

The workflow is:
Document ingestion
Compliance PDF

Investment compliance policies
Document parsing and chunking

Split the document into smaller passages.

Text embedding model

Convert each chunk into a Float32 vector (for example).

Milvus vector database

Store vectors, document IDs, and metadata.


User query / retrieval
"Does this presentation have the required risk disclosure?"
Query embedding

Convert the question into a vector using the same compatible embedding model.

Vector similarity search

Retrieve the most relevant compliance-policy chunks.

LLM + retrieved context

Evaluate the submitted marketing document against the retrieved policies.

Grounded response

Identify potential issues and cite supporting policy references for human review.


One important rule: the document and query embeddings must use compatible embedding spaces. Normally, use the same embedding model and configuration for both.
9. How are embeddings stored in a vector database?
For example, in Milvus, a record might look like this:
Field	Example
id	1001
document_id	POLICY_101
text	Investment risk disclosure policy
embedding	[0.21, 0.85, ...]
region	US
last_updated	2026-11-15

Milvus supports vector fields and different vector search approaches. PostgreSQL with pgvector also provides types such as vector, halfvec, bit, and sparsevec. 
GitHub+1

10. Python example: Generate a text embedding
Here is a practical example using OpenAI's embedding API.
from openai import OpenAIclient = OpenAI()text = "How do I withdraw money from my retirement account?"response = client.embeddings.create(    model="text-embedding-3-small",    input=text)embedding = response.data[0].embeddingprint("Dimensions:", len(embedding))print("Embedding:", embedding[:5])



The output is a list of floating-point numbers.

You can then store that vector in Milvus or another vector database.

1. What is an embedding?
An embedding is a numerical vector representation of data that captures semantic or learned relationships between items.

2. What is the difference between Float32 and Float16?
3. What is the difference between dense and sparse embeddings?
4. What is hybrid search?
5. Why do we use embeddings in RAG?
6. Can we use different embedding models for documents and queries?
7. What is quantization?
8. What is the difference between embedding dimensions and data types?

12. The most important concepts to remember
Concept	Remember this
Text embedding	Converts text into vectors
Dense	Mostly nonzero numbers
Sparse	Mostly zero values
Float32	4 bytes per dimension
Float16	2 bytes per dimension
Int8	1 byte per dimension
Binary	1 bit per dimension
Dimensions	Number of values in a vector
Cosine similarity	Measures directional similarity
Quantization	Reduces vector precision/storage
Hybrid search	Combines dense and sparse retrieval
