# Chunking in Retrieval-Augmented Generation (RAG)

## Overview

Chunking is the process of splitting large documents into smaller pieces before storing them in a vector database.

In a RAG system, documents are usually too large to send directly to an LLM because of:

- Context window limitations
- Higher token costs
- Reduced retrieval accuracy

Chunking helps the system retrieve only the most relevant information needed to answer a user's question.

---

# Why Chunking is Important

A typical RAG pipeline:

```
Documents

     |

     v

Document Loader

     |

     v

Chunking

     |

     v

Embedding Generation

     |

     v

Vector Database

     |

     v

Similarity Search

     |

     v

LLM Response
```

The quality of chunking directly impacts:

- Retrieval accuracy
- Response quality
- Hallucination reduction
- Token cost
- Latency

---

# What Happens Without Chunking?

Imagine a company has a 500-page customer support manual.

User asks:

```
What is the refund policy?
```

Without chunking:

```
500 page document

        |

        v

LLM

        |

        v

High cost + poor context
```

The model receives too much unnecessary information.

---

With chunking:

```
500 page document

        |

        v

1000 smaller chunks

        |

        v

Retrieve only refund section

        |

        v

LLM generates answer
```

---

# Chunking Process

## Step 1: Document Loading

Documents can come from:

- PDF
- Word
- HTML
- CSV
- Database
- Confluence
- SharePoint
- Emails


Example:

```
customer_policy.pdf
```

---

## Step 2: Text Extraction

Convert documents into plain text.

Example:

```
PDF

 |

Text Extraction

 |

Raw Text
```

Tools:

- PyPDF
- Unstructured
- Apache Tika

---

## Step 3: Split Text into Chunks

Example:

Original:

```
Amazon return policy document
500 pages
```

After chunking:

```
Chunk 1:
Return eligibility

Chunk 2:
Refund process

Chunk 3:
Shipping rules

Chunk 4:
Warranty information
```

---

# Important Chunking Parameters

## Chunk Size

Chunk size determines how much text each chunk contains.

Example:

```
Chunk Size = 500 tokens
```

means each chunk contains approximately 500 tokens.

---

## Chunk Overlap

Chunk overlap keeps some repeated text between chunks.

Example:

Chunk 1:

```
Customers can return products within 30 days.
Products must be unused.
```

Chunk 2:

```
Products must be unused.
Refunds are processed within 5 days.
```

The repeated sentence is overlap.

---

# Why Chunk Overlap is Needed

Without overlap:

```
Chunk 1

"The customer must provide"

        |

        X

Chunk 2

"order information for refund"
```

Important context is separated.

With overlap:

```
Chunk 1

"The customer must provide order information"

Chunk 2

"provide order information for refund"
```

The meaning is preserved.

---

# Types of Chunking Strategies

## 1. Fixed Size Chunking

The simplest approach.

Example:

```
Every 500 tokens

Chunk 1
Chunk 2
Chunk 3
```

Advantages:

- Simple
- Fast
- Predictable


Disadvantages:

- May split sentences
- May lose meaning


Example:

```python
chunk_size = 500
chunk_overlap = 50
```

---

# 2. Sentence-Based Chunking

Splits documents by sentences.

Example:

```
Sentence 1
Sentence 2
Sentence 3
```

Advantages:

- Maintains meaning
- Better readability


Disadvantages:

- Uneven chunk sizes

---

# 3. Paragraph-Based Chunking

Splits documents by paragraphs.

Example:

```
Introduction paragraph

Returns paragraph

Shipping paragraph
```

Good for:

- Policies
- Documentation
- Articles

---

# 4. Recursive Character Chunking

One of the most common approaches.

The splitter tries:

```
Paragraph

        |

Sentence

        |

Word

        |

Character
```

Example:

LangChain:

```python
RecursiveCharacterTextSplitter(
    chunk_size=500,
    chunk_overlap=50
)
```

Advantages:

- Preserves structure
- Good default strategy

---

# 5. Semantic Chunking

Semantic chunking splits text based on meaning rather than length.

Example:

Document:

```
Section about payments

Section about returns

Section about shipping
```

Each topic becomes a separate chunk.

Advantages:

- Better retrieval quality
- Better context preservation


Disadvantages:

- More expensive
- Requires embeddings

---

# 6. Parent-Child Chunking

Uses two levels:

```
Parent Chunk

        |

        |

Child Chunks
```

Example:

Large document section:

```
Return Policy
```

Child chunks:

```
Eligibility

Refund timeline

Exceptions
```

Benefits:

- Better context
- Better retrieval accuracy

---

# 7. Document Structure-Based Chunking

Uses document structure.

Examples:

Markdown:

```
# Title

## Section

### Subsection
```

HTML:

```
<h1>
<h2>
<p>
```

Useful for:

- Technical documentation
- Websites
- Knowledge bases

---

# Chunk Size Selection

There is no perfect chunk size.

Common values:

| Use Case | Chunk Size |
|---|---|
| FAQ | 200-500 tokens |
| Documentation | 500-1000 tokens |
| Research Papers | 1000+ tokens |
| Code | Structure based |
| Legal Documents | Larger chunks |

---

# Chunking Trade-offs

## Small Chunks

Advantages:

- More precise retrieval
- Lower token cost

Disadvantages:

- Less context


---

## Large Chunks

Advantages:

- More context
- Better understanding

Disadvantages:

- Higher cost
- Less precise retrieval

---

# Chunk Metadata

Always store metadata with chunks.

Example:

```json
{
 "document": "refund_policy.pdf",
 "page": 5,
 "section": "Returns",
 "created_date": "2026-08-01"
}
```

Metadata helps with:

- Filtering
- Citations
- Debugging
- Security

---

# Chunking in Enterprise RAG

Enterprise documents include:

- Policies
- Contracts
- Manuals
- Product documentation
- Knowledge bases


A production pipeline:

```
Documents

↓

Document Parser

↓

Structure Detection

↓

Semantic Chunking

↓

Metadata Creation

↓

Embedding Generation

↓

Vector Database

```

---

# Chunking Challenges

## 1. Losing Context

Problem:

Important information split across chunks.


Solution:

- Use overlap
- Use semantic chunking

---

## 2. Too Many Chunks

Problem:

Large vector database.

Solution:

- Optimize chunk size
- Remove duplicate content

---

## 3. Poor Retrieval

Problem:

Wrong chunks returned.

Solution:

- Hybrid search
- Re-ranking
- Better embeddings

---

## 4. Tables and Images

Problem:

Important information is not plain text.

Solution:

Use:

- OCR
- Vision models
- Table extraction

---

# Chunking Evaluation

Measure:

## Retrieval Accuracy

Did we retrieve the correct chunk?


## Answer Quality

Did the LLM generate the correct answer?


## Context Relevance

Was the retrieved information useful?


Tools:

- RAGAS
- LangSmith
- Human evaluation

---

# Interview Questions

## What is chunking in RAG?

Chunking is the process of splitting large documents into smaller pieces before creating embeddings and storing them in a vector database.

---

## Why is chunking important?

Because LLMs have context limits and retrieving smaller relevant sections improves accuracy, cost, and response quality.

---

## What chunk size should we use?

There is no universal value. It depends on document type, model context window, and retrieval requirements.

---

## What is chunk overlap?

Chunk overlap repeats some text between chunks to preserve context across boundaries.

---

## Fixed chunking vs semantic chunking?

Fixed chunking splits based on size. Semantic chunking splits based on meaning.

Semantic chunking usually provides better retrieval but has higher processing cost.

---

## How do you improve poor RAG retrieval?

Options:

- Improve chunking
- Use better embeddings
- Add metadata filtering
- Use hybrid search
- Add re-ranking
- Improve prompts

---

# Best Practices

- Start with recursive chunking.
- Store metadata with every chunk.
- Use overlap to preserve context.
- Test different chunk sizes.
- Evaluate retrieval quality.
- Use semantic chunking for complex documents.
- Monitor failed searches.
- Keep chunking strategy configurable.

---

# References

- LangChain Text Splitters
- LlamaIndex Document Processing
- RAG Research Papers
- RAGAS Evaluation Framework
- Vector Database Documentation
