# LLM Fallback and Resilience Strategy

## 1. Problem Statement

What happens when an LLM provider becomes unavailable?

For example:
- OpenAI API is down.
- Azure OpenAI has a service outage.
- LLM requests start timing out.
- Rate limits (HTTP 429) are exceeded.
- The provider returns HTTP 500 or 503.
- Model response latency increases significantly.

If our application depends on a single LLM provider, the entire AI application can become unavailable.

**Solution:** Implement an LLM resilience strategy with multiple fallback mechanisms.

## 2. High-Level Architecture

```text
              User Request
                   |
                   v
             API Gateway
                   |
                   v
             AI Application
                   |
                   v
           LLM Orchestrator
                   |
                   v
          Primary LLM Provider
             (Azure OpenAI)
                   |
             Is Provider Healthy?
                /       \
              Yes        No
               |          |
               v          v
           Response   Circuit Breaker
                          |
                          v
                   Secondary LLM
                   (OpenAI / Other)
                          |
                    Is Available?
                     /       \
                   Yes        No
                    |          |
                    v          v
                 Response  Graceful
                           Degradation
```

## 3. Fallback Strategies

### Strategy 1: Primary and Secondary LLM

Configure multiple LLM providers.

Example:

| Priority | Provider | Model |
|---|---|---|
| Primary | Azure OpenAI | GPT model |
| Secondary | OpenAI | GPT model |
| Tertiary | Another LLM provider | Compatible model |

Execution:

1. Send the request to the primary provider.
2. If the primary provider fails with a retryable error, retry within a bounded limit.
3. If it continues failing, switch to the secondary provider.
4. If all providers fail, return a graceful fallback response.

Important considerations:
- Ensure secondary providers support equivalent capabilities.
- Validate model output consistency.
- Maintain security and compliance across providers.
- Do not send sensitive information to an unapproved provider.

### Strategy 2: Retry Mechanism

Transient failures can happen because of temporary network issues or rate limits.

Use exponential backoff.

Example:

```python
import time

def retry_with_backoff(call_llm, max_retries=3):
    for attempt in range(max_retries):
        try:
            return call_llm()

        except Exception:
            if attempt == max_retries - 1:
                raise

            delay = 2 ** attempt
            time.sleep(delay)
```

Retry intervals:

| Attempt | Delay |
|---|---|
| 1 | 1 second |
| 2 | 2 seconds |
| 3 | 4 seconds |

Production considerations:
- Add random jitter to avoid synchronized retries.
- Respect Retry-After for HTTP 429 responses.
- Retry only transient failures.
- Set an overall request deadline.

Do not retry authentication errors, invalid requests, or other permanent failures.

### Strategy 3: Circuit Breaker

A circuit breaker prevents continuous requests to an unhealthy LLM provider.

It has three states:

**CLOSED**
- Provider is healthy.
- Requests flow normally.

**OPEN**
- Failure threshold is reached.
- Requests are rejected or routed to a fallback provider.
- Avoids continuously calling an unhealthy provider.

**HALF-OPEN**
- After a recovery interval, allow a limited test request.
- If successful, transition to CLOSED.
- If unsuccessful, transition back to OPEN.

```text
       CLOSED
          |
     Failure Threshold
          |
          v
         OPEN
          |
     Recovery Timeout
          |
          v
       HALF-OPEN
        /      \
   Success    Failure
      |          |
      v          v
   CLOSED       OPEN
```

### Strategy 4: Timeout Management

LLM requests can take several seconds or longer.

Configure:
- Connection timeout.
- Read timeout.
- Overall request timeout.
- Token generation timeout where supported.

Example:

```python
import requests

response = requests.post(
    "https://llm-provider.example.com",
    json={"prompt": "Explain RAG"},
    timeout=(3, 20)
)
```

The example uses a 3-second connection timeout and a 20-second read timeout.

If the request exceeds its deadline, cancel it where possible and invoke the fallback strategy.

### Strategy 5: Graceful Degradation

What if every LLM provider is unavailable?

The application should still provide useful functionality where possible.

Examples:

| AI Feature | Fallback |
|---|---|
| Conversational chat | Return a temporary unavailable message |
| Document summarization | Show previously generated summary if still valid |
| RAG search | Return relevant retrieved documents without generating an answer |
| Classification | Queue the request for later processing |
| AI recommendations | Display existing approved recommendations |
| Document processing | Save the request and process asynchronously |

Important: Never present stale or cached information as a newly generated answer.

## 4. LLM Fallback Decision Flow

```text
Receive User Request
        |
        v
Check Primary Provider Health
        |
        v
Call Primary LLM
        |
        v
    Request Failed?
      /       \
    No         Yes
    |           |
    v           v
 Return     Is Error Retryable?
 Answer       /       \
            Yes        No
             |          |
             v          v
           Retry    Check Fallback
             |          |
        Still Failed?   |
             |          |
             v          v
        Secondary LLM Provider
                   |
                   v
              Available?
               /      \
             Yes       No
              |         |
              v         v
           Response  Graceful
                     Degradation
```

## 5. Important HTTP Status Codes

| HTTP Status | Meaning | Recommended Action |
|---|---|---|
| 400 | Bad Request | Do not retry |
| 401 | Unauthorized | Check credentials |
| 403 | Forbidden | Check permissions |
| 408 | Request Timeout | Retry cautiously |
| 429 | Rate Limited | Respect Retry-After |
| 500 | Internal Server Error | Retry with backoff |
| 502 | Bad Gateway | Retry with backoff |
| 503 | Service Unavailable | Trigger fallback |
| 504 | Gateway Timeout | Trigger fallback |

## 6. Additional Production Considerations

### Health Checks
- Monitor provider availability.
- Track API latency.
- Monitor timeout and error rates.
- Track rate-limit responses.

### Observability
Use Prometheus and Grafana to monitor:

- LLM request count.
- Success and failure rates.
- Average response latency.
- Token consumption.
- Provider-specific error rates.
- Fallback invocation count.
- Circuit breaker state.

### Security and Governance
- Ensure every provider is approved.
- Maintain PII protection.
- Apply input and output guardrails.
- Enforce tenant isolation.
- Maintain audit logs.
- Validate model compatibility and response quality.

### Cost Management
- Monitor cost per provider.
- Avoid unnecessary retries.
- Set token limits.
- Route requests based on model capability and cost.
- Prevent retry storms.

## 7. Python Implementation Concept

A simplified provider fallback implementation:

```python
class LLMService:

    def __init__(self, primary, secondary):
        self.primary = primary
        self.secondary = secondary

    def generate(self, prompt):

        try:
            return self.primary.generate(prompt)

        except TimeoutError:
            print("Primary provider timed out")

        except ConnectionError:
            print("Primary provider unavailable")

        try:
            return self.secondary.generate(prompt)

        except Exception:
            return {
                "status": "temporarily_unavailable",
                "message": "AI service is temporarily unavailable. Please try again."
            }
```

This demonstrates the fallback concept. A production implementation should add bounded retries, circuit breakers, typed error handling, request deadlines, telemetry, and provider-specific security controls.

## 8. Interview Explanation

**Question: What happens if your LLM provider goes down?**

My answer:

"I would design the LLM integration with high availability and fault tolerance. I would configure a primary and an approved secondary LLM provider.

First, I would configure appropriate connection and response timeouts. For transient failures, such as network errors or HTTP 429 and 503, I would implement bounded retries with exponential backoff and jitter.

If the primary provider continues failing, I would use a circuit breaker to stop sending requests to the unhealthy provider and route requests to the secondary provider.

If both providers are unavailable, I would implement graceful degradation. Depending on the use case, I could return cached results, provide retrieved documents without LLM generation, or queue non-interactive requests for asynchronous processing.

I would also monitor provider latency, error rates, fallback invocation rates, and circuit breaker states through Prometheus and Grafana.

Finally, I would ensure that all fallback providers meet our security, data privacy, governance, and model-quality requirements."

## 9. Key Takeaways

Remember these six concepts:

1. **Retry** - Handle temporary failures.
2. **Timeout** - Avoid waiting indefinitely.
3. **Circuit Breaker** - Stop calling unhealthy providers.
4. **Fallback** - Switch to another approved provider.
5. **Graceful Degradation** - Keep essential functionality available.
6. **Observability** - Monitor and detect failures.

Mnemonic: **RTCFG O**

- Retry
- Timeout
- Circuit Breaker
- Fallback
- Graceful Degradation
- Observability
