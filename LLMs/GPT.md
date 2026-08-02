
# GPT (Generative Pre-trained Transformer)

## What is GPT?

GPT (Generative Pre-trained Transformer) is a family of large language models (LLMs) developed to understand and generate human-like text. It is based on the Transformer architecture and is trained on massive amounts of text data using self-supervised learning.

GPT can perform many NLP tasks without task-specific training, including:

- Question Answering
- Text Summarization
- Translation
- Code Generation
- Content Creation
- Information Extraction
- Chatbots
- AI Agents

---

# How GPT Works

GPT training happens in two major stages.

## 1. Pre-training

The model learns language by predicting the next token.

Example:

Input:

```
The capital of France is
```

Target:

```
Paris
```

During pre-training, the model learns:

- Grammar
- Facts
- Reasoning patterns
- Programming languages
- Mathematics
- World knowledge

This stage requires enormous datasets and GPU clusters.

---

## 2. Fine-tuning / Alignment

After pre-training, the model is improved using techniques such as:

- Supervised Fine-Tuning (SFT)
- Reinforcement Learning from Human Feedback (RLHF)
- Direct Preference Optimization (DPO)

This makes responses:

- More helpful
- More truthful
- Safer
- Better aligned with user intent

---

# Transformer Architecture (High Level)

GPT is built on the Transformer decoder architecture.

Main components:

```
Input Text
      │
      ▼
Tokenizer
      │
      ▼
Embeddings
      │
      ▼
Positional Encoding
      │
      ▼
Multiple Transformer Blocks
      │
      ▼
Self Attention
      │
      ▼
Feed Forward Network
      │
      ▼
Next Token Prediction
```

The Transformer uses **self-attention**, allowing every token to attend to previous tokens and understand context efficiently.

Advantages:

- Parallel processing
- Long-range dependency handling
- Better scalability
- High-quality text generation

---

# Tokens

LLMs do not process words directly.

They process **tokens**.

Example:

Sentence:

```
ChatGPT is amazing.
```

Possible tokens:

```
Chat
GPT
is
amazing
.
```

Another example:

```
unbelievable
```

might become

```
un
believ
able
```

Different models use different tokenizers.

Examples:

- Byte Pair Encoding (BPE)
- SentencePiece
- WordPiece

---

# Context Window

The context window is the maximum number of tokens the model can consider during one request.

Example:

```
Prompt

+

Conversation History

+

Retrieved Documents

<= Context Window
```

If the prompt exceeds the limit:

- older tokens may be removed
- the request may fail
- important information may be lost

Larger context windows enable:

- better conversations
- document analysis
- long code generation
- enterprise RAG systems

---

# Temperature

Temperature controls randomness.

| Temperature | Behavior |
|------------|----------|
| 0 | Deterministic |
| 0.2 | Stable |
| 0.7 | Balanced |
| 1.0 | Creative |
| >1 | Very Random |

Examples:

Use low temperature for:

- SQL
- Code
- Financial reports

Use higher temperature for:

- Story writing
- Brainstorming
- Marketing

---

# Top-p (Nucleus Sampling)

Top-p limits token selection to the smallest set of tokens whose cumulative probability reaches **p**.

Example:

```
Top-p = 0.9
```

The model considers only the most likely tokens until their combined probability reaches 90%.

Benefits:

- More natural outputs
- Reduces unlikely token selection
- Improves diversity

---

# Function Calling

Modern LLMs can call external tools.

Example:

User:

```
What's the weather in New York?
```

Instead of guessing:

```
LLM

↓

Weather API

↓

JSON

↓

LLM Response
```

Typical use cases:

- Database queries
- Calendar
- Email
- Search
- GitHub
- Payment systems

---

# Structured Outputs

Instead of plain text, an LLM can generate structured data.

Example:

```json
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```

Benefits:

- Reliable APIs
- Easier parsing
- Better integrations
- Fewer hallucinations

Often implemented using:

- JSON Schema
- Pydantic
- Function Calling

---

# Fine-Tuning

Fine-tuning trains an existing model on domain-specific data.

Examples:

- Medical
- Finance
- Insurance
- Legal
- Cybersecurity

Advantages:

- Better domain knowledge
- Improved terminology
- Consistent responses

Disadvantages:

- Expensive
- Requires training infrastructure
- Difficult to maintain

---

# RAG vs Fine-Tuning

| RAG | Fine-Tuning |
|------|-------------|
| Retrieves external knowledge | Updates model weights |
| Easy to update | Requires retraining |
| Lower cost | Higher cost |
| Great for enterprise documents | Great for specialized behavior |
| Uses vector databases | Uses training datasets |

**Rule of thumb:**

Use **RAG** when knowledge changes frequently.

Use **Fine-Tuning** when behavior or style needs to change.

---

# Common Interview Questions

### What is GPT?

A decoder-only Transformer-based Large Language Model that predicts the next token.

---

### What is a token?

The smallest unit of text processed by an LLM.

---

### Why are embeddings required?

To convert text into numerical vectors that models can understand.

---

### What is self-attention?

A mechanism that allows every token to understand relationships with previous tokens.

---

### What is the difference between temperature and top-p?

Temperature changes randomness.

Top-p limits candidate token selection based on cumulative probability.

---

### When should you use RAG instead of Fine-Tuning?

Use RAG when knowledge changes frequently or comes from enterprise documents.

---

### Why is GPT called "Generative Pre-trained Transformer"?

- **Generative** → Generates new content.
- **Pre-trained** → Trained on large datasets before deployment.
- **Transformer** → Uses the Transformer neural network architecture.

---

# Best Practices

- Use low temperature for deterministic tasks.
- Use structured outputs for APIs.
- Use RAG for enterprise knowledge.
- Use function calling for external tools.
- Monitor token usage and costs.
- Cache repeated prompts.
- Validate model outputs.

---

# References

- Attention Is All You Need (Transformer Paper)
- GPT-3 Paper
- GPT-4 Technical Report
- OpenAI API Documentation
- LangChain Documentation
- LlamaIndex Documentation
