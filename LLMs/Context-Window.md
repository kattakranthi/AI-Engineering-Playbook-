
## 1. What is a Context Window?

A context window is the maximum amount of text and other supported input information an LLM can process at one time to generate a response.

It is measured in **tokens**, not words.

The context window can contain:
- System instructions
- Developer instructions
- User prompts
- Conversation history
- RAG-retrieved documents
- Tool outputs
- The model's generated response (depending on the model's context accounting)

**Example:**

If a model supports a 128K-token context window, it can process up to approximately 128,000 tokens in its context.

The exact amount available for the input depends on how much space must be reserved for the output and other model-specific constraints.

---

## 2. What is a Token?

A token is a unit of text processed by an LLM.

A token can represent:
- A complete word
- Part of a word
- Punctuation
- A number
- A whitespace or text fragment

Example:

```text
Input: Artificial Intelligence is transforming banking.

The tokenizer converts the text into tokens.
```

A rough English-language estimate is:

- 1 token ≈ 0.75 words
- 100 tokens ≈ 75 words
- 1,000 tokens ≈ 750 words

This is only an approximation. Token counts vary by language, content, and tokenizer.

---

## 3. What Happens When the Context Window Is Full?

When the input exceeds the model's context limit, the request may fail with a context-length error.

The application needs to manage the context before sending the request.

Common strategies:

1. Remove irrelevant conversation history.
2. Summarize older conversation messages.
3. Retrieve only relevant documents using RAG.
4. Split large documents into chunks.
5. Limit the number of retrieved chunks.
6. Reduce unnecessary tool output.
7. Reserve sufficient tokens for the model response.

**Example:**

A user uploads a 500-page financial document.

Instead of sending all 500 pages to the LLM:

- Parse the document.
- Split it into chunks.
- Generate embeddings.
- Store embeddings in a vector database.
- Retrieve only relevant chunks for the user's question.
- Send those chunks to the LLM.

This helps keep the prompt within the context window.

---

## 4. What Is the Difference Between Context Window and Memory?

| Context Window | Memory |
|---|---|
| Information available to the model for the current request | Information retained or retrieved across interactions |
| Limited by token capacity | Depends on the application's memory implementation |
| Includes current prompt and conversation context | May contain summaries, preferences, or stored facts |
| Usually temporary | Can persist across sessions |
| Directly supplied to the model | Must be retrieved or injected when needed |

**Important:** An LLM does not automatically remember every previous conversation. Applications must implement memory using databases, summaries, retrieval, or other mechanisms.

---

## 5. What Is the Difference Between Context Window and RAG?

| Context Window | RAG |
|---|---|
| Defines how much information the model can process at once | Retrieves relevant external information |
| A model capability | An application architecture pattern |
| Measured in tokens | Uses retrieval and relevance ranking |
| Does not automatically find documents | Searches a knowledge source |
| Has a finite capacity | Can work with a much larger document collection |

RAG helps applications avoid sending an entire knowledge base into the context window.

---

## 6. How Does RAG Work With the Context Window?

Consider a financial services application where a banker asks:

"What are the termination clauses in this acquisition agreement?"

The agreement contains 500 pages.

### RAG workflow

1. Upload the agreement.
2. Extract text from the PDF.
3. Split the text into smaller chunks.
4. Generate embeddings for each chunk.
5. Store embeddings and metadata in a vector database.
6. Convert the user's question into an embedding.
7. Retrieve the most relevant chunks.
8. Optionally rerank the retrieved chunks.
9. Construct the LLM prompt with the question and retrieved context.
10. Generate a grounded response.

### Architecture

```text
              User Question
                    |
                    v
             API Gateway
                    |
                    v
             RAG Orchestrator
                    |
                    v
            Query Embedding
                    |
                    v
             Vector Database
                    |
                    v
          Retrieve Relevant Chunks
                    |
                    v
                Reranker
                    |
                    v
           Context Construction
                    |
                    v
              LLM Gateway
                    |
                    v
               LLM Model
                    |
                    v
           Grounded Response
                    |
                    v
                  User
```

The application sends only the relevant retrieved content to the LLM rather than the entire document.

---

## 7. What Is a Context Budget?

A context budget is the number of tokens allocated to different parts of an LLM request.

For example, assume a model supports a 32K-token context window.

An application might allocate:

| Component | Token Budget |
|---|---:|
| System instructions | 2,000 |
| Developer instructions | 1,000 |
| Conversation history | 4,000 |
| Retrieved RAG documents | 15,000 |
| Current user question | 1,000 |
| Reserved output | 5,000 |
| Additional buffer | 4,000 |
| **Total** | **32,000** |

These are illustrative allocations, not fixed model requirements.

The application should monitor token usage and ensure the total request fits the model's limits.

---

## 8. What Is Context Engineering?

Context engineering is the process of selecting, organizing, and managing the information provided to an LLM so it can produce accurate and relevant responses.

It includes:

- System instructions
- Developer instructions
- User prompts
- Conversation history
- RAG retrieval
- Tool outputs
- Memory
- Context compression
- Token-budget management

**Example:**

For a compliance AI agent, we should provide:

1. The compliance policy.
2. The submitted marketing document.
3. Relevant policy sections retrieved through RAG.
4. Instructions to identify policy violations.
5. Output format requirements.
6. Instructions to cite supporting evidence.

This creates a focused context for the model.

---

## 9. What Is Context Compression?

Context compression reduces the amount of information sent to the LLM while preserving the most relevant information.

Common approaches:

- Summarize older conversation history.
- Remove duplicate content.
- Remove irrelevant retrieved chunks.
- Extract important facts from long documents.
- Use structured representations instead of verbose text.
- Limit unnecessary tool outputs.

**Example:**

Instead of including 50 previous conversation messages, the application summarizes them:

```text
Previous conversation summary:
- User is reviewing an acquisition agreement.
- Main concern is termination clauses.
- Focus on payment obligations and termination fees.
- Previously identified section 8.2 as relevant.
```

The summarized context is shorter and easier to manage.

---

## 10. What Happens if a Conversation Exceeds the Context Window?

A conversation can grow beyond the model's context limit.

The application can use a sliding-window strategy:

1. Keep recent messages.
2. Summarize older messages.
3. Preserve important decisions and facts.
4. Remove irrelevant details.
5. Include the summary in the next request.

Example:

```text
Conversation:
Message 1
Message 2
Message 3
...
Message 100

Context management:

Summary of Messages 1-70
+
Recent Messages 71-100
+
Current User Question
```

This allows the application to maintain useful conversational continuity without sending the entire conversation every time.

---

## 11. What Is the Difference Between a Large Context Window and RAG?

A larger context window allows a model to process more information in one request.

RAG retrieves selected information from an external knowledge base.

| Large Context Window | RAG |
|---|---|
| Processes more information at once | Retrieves relevant information |
| Useful for long documents | Useful for large document collections |
| Can increase input-token cost | Can reduce unnecessary input tokens |
| Still has a finite limit | Knowledge base can grow independently |
| May include irrelevant information | Retrieval can focus on relevant information |

**Interview point:**

A large context window does not replace RAG. RAG is still valuable for enterprise applications requiring document retrieval, metadata filtering, access control, and current knowledge.

---

## 12. What Is the Lost-in-the-Middle Problem?

The lost-in-the-middle problem describes a limitation where an LLM may not use information placed in the middle of a very long context as effectively as information near the beginning or end.

For example:

```text
Beginning of context:
Important policy information

Middle of context:
Several hundred pages of documents

End of context:
User question
```

The model may not reliably identify every relevant detail buried in the middle.

Ways to mitigate this:

- Retrieve only relevant chunks.
- Rerank retrieved results.
- Remove irrelevant information.
- Put critical instructions in appropriate prompt positions.
- Ask focused questions.
- Test retrieval and answer quality.

---

# Advanced Interview Questions

## Q1. How would you manage context windows in an enterprise AI application?

**Answer:**

I would implement context management as part of the AI orchestration layer.

First, I would define a token budget for system instructions, user input, conversation history, retrieved documents, tool outputs, and the expected response.

For large document collections, I would use RAG rather than sending entire documents to the LLM.

I would also implement conversation summarization, context compression, and retrieval filtering to remove irrelevant information.

Finally, I would monitor token consumption, context-length errors, latency, and response quality.

---

## Q2. How would you handle a 1,000-page document when the model has a 128K context window?

**Answer:**

I would not send the entire document directly to the LLM.

I would implement a document ingestion pipeline:

1. Extract text using a PDF parser or OCR.
2. Split the document into meaningful chunks.
3. Generate embeddings.
4. Store chunks and metadata in a vector database.
5. Retrieve relevant chunks based on the user's question.
6. Apply reranking to improve relevance.
7. Construct a prompt with the selected chunks.
8. Send the prompt to the LLM.
9. Return an answer with document citations.

For questions requiring information across many sections, I would use multi-step retrieval or map-reduce summarization.

This approach improves scalability and keeps the prompt within the context limit.

---

## Q3. How do you calculate whether a prompt fits within the context window?

**Answer:**

I would use the model's tokenizer to calculate the token count before making the inference request.

The basic calculation is:

```text
Total Tokens =
System Instructions
+ Developer Instructions
+ User Prompt
+ Conversation History
+ Retrieved Documents
+ Tool Outputs
+ Reserved Output Tokens
```

I would ensure the total stays within the model's supported context limit.

I would also reserve a safety buffer rather than using the entire context window.

---

## Q4. Can increasing the context window eliminate hallucinations?

**Answer:**

No.

A larger context window allows the model to process more information, but it does not guarantee that the model will understand or correctly use that information.

Hallucinations can still happen because of:

- Ambiguous prompts.
- Incorrect or conflicting source documents.
- Poor retrieval quality.
- Model limitations.
- Unsupported assumptions.

I would combine RAG, source citations, input/output guardrails, validation, and human review for high-risk financial workflows.

---

## Q5. How would you manage context in an agentic AI system?

**Answer:**

In an agentic AI system, the context can grow quickly because agents interact with tools, APIs, databases, and other agents.

I would manage context through an orchestration layer.

My approach would include:

1. Maintain workflow state separately from the LLM prompt.
2. Store important execution results in a database.
3. Pass only relevant state to each agent.
4. Summarize long tool outputs.
5. Maintain a token budget for every LLM call.
6. Remove redundant tool responses.
7. Use RAG to retrieve relevant enterprise knowledge.

For example, in a banking case-management workflow, the investigation agent does not need the entire history of every tool execution. It needs the current case details, relevant transaction records, and important findings from previous steps.

---

## Q6. What is the difference between context storage and workflow state storage?

**Answer:**

Context is the information provided to the LLM for a particular model call.

Workflow state is the persistent execution information maintained by the application.

For example:

```text
Workflow State:
- Case ID
- Current status
- Assigned analyst
- Investigation results
- Approval status
- Tool execution history

LLM Context:
- Current case summary
- Relevant transaction details
- Retrieved policy documents
- Current task instructions
```

I would store workflow state in a database and construct the LLM context dynamically for each step.

---

## Q7. How would you prevent irrelevant RAG documents from consuming the context window?

**Answer:**

I would use a multi-stage retrieval strategy.

1. Apply metadata filtering based on user permissions, region, document type, and version.
2. Perform semantic or hybrid search.
3. Retrieve an initial set of candidate chunks.
4. Rerank the candidates.
5. Select the most relevant chunks.
6. Apply a token budget before constructing the prompt.

This ensures that the LLM receives relevant and authorized information.

---

## Q8. What is the relationship between context window and attention mechanism?

**Answer:**

The context window defines the maximum input sequence length supported by the model.

The attention mechanism allows the model to use relationships between tokens in that sequence when generating its response.

In standard Transformer self-attention, computational and memory requirements can grow significantly with sequence length.

Modern architectures and inference optimizations can reduce some of these costs, but a larger context window can still increase latency and resource consumption.

---

## Q9. Does every LLM call start with a new context window?

**Answer:**

Typically, each API request provides its own context.

For a multi-turn conversation, the application may send previous messages again as part of the new request, or use a provider-supported conversation/state mechanism.

The model does not necessarily retain the full conversation automatically between independent requests.

Some inference providers also support prompt caching or stateful sessions to reduce repeated processing.

---

## Q10. How does context window impact cost and latency?

**Answer:**

A larger input generally means more tokens to process.

This can increase:

- Input-token cost.
- Time to process the prompt.
- Memory requirements.
- Inference resource consumption.

I would optimize by retrieving only relevant documents, summarizing older messages, limiting tool output, and using an appropriate model for the task.

---

# Practical Financial Services Example

## AI-Powered Compliance Review Agent

**Scenario:**

A marketing employee uploads a 100-page client presentation. The AI agent must check it against internal compliance policies.

### Context management approach

```text
             Marketing Document
                     |
                     v
              PDF Extraction
                     |
                     v
               Chunking
                     |
                     v
                Embedding
                     |
                     v
              Vector Database
                     |
                     v
          Compliance Policy Retrieval
                     |
                     v
             Relevant Policy Chunks
                     |
                     v
            Context Construction
                     |
                     v
                LLM Gateway
                     |
                     v
           Compliance Review Agent
                     |
                     v
          Findings and Policy Citations
                     |
                     v
           Human Compliance Approval
```

### What goes into the LLM context?

- System instructions defining the compliance review role.
- User instructions and submitted document details.
- Relevant sections from the marketing document.
- Retrieved compliance policy chunks.
- Output format and citation requirements.

### What should not go into the context?

- Every document in the enterprise knowledge base.
- Unrelated historical conversations.
- Irrelevant tool execution logs.
- Duplicate policy documents.
- Documents the user is not authorized to access.

### Interview response

"In our enterprise compliance use case, I would not send the entire document repository to the LLM. I would use RAG to retrieve relevant policy and document sections, apply metadata filters for region and access control, and rerank the results.

I would then construct a token-budgeted prompt containing the system instructions, submitted document content, relevant policy sections, and the user's question.

I would also manage conversation history and tool outputs so that the context stays within the model's supported limit. This improves relevance, reduces unnecessary token consumption, and supports grounded responses."

---

# Quick Revision

| Term | Meaning |
|---|---|
| Context Window | Maximum information an LLM can process in a request |
| Token | Unit of text processed by the model |
| Tokenizer | Converts text into tokens |
| Context Budget | Allocation of tokens across prompt components |
| Context Engineering | Selecting and organizing information for the LLM |
| Context Compression | Reducing context while preserving important information |
| RAG | Retrieving relevant external knowledge |
| Sliding Window | Keeping recent conversation context |
| Summarization | Condensing older information |
| Lost in the Middle | Reduced effective use of information buried in long contexts |
| Prompt Caching | Reusing previously processed prompt content where supported |

## Key Interview Takeaways

1. Context windows are measured in tokens, not words.
2. Larger context windows do not eliminate hallucinations.
3. RAG helps retrieve relevant information instead of sending entire knowledge bases.
4. Always reserve output tokens.
5. Use tokenizers to estimate prompt size.
6. Summarize older conversation history.
7. Keep workflow state in persistent storage rather than relying entirely on LLM context.
8. Use metadata filtering and reranking to improve RAG relevance.
9. Monitor context usage, latency, token cost, and response quality.
10. Context engineering is especially important in agentic AI systems because tool outputs and execution history can increase context size quickly.
