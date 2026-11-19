
# AI Security Checklist

A practical security checklist for designing, developing, deploying, and operating enterprise AI applications, including LLMs, RAG pipelines, and agentic AI systems.

## 1. Identity and Access Management

- [ ] Implement authentication using OAuth 2.0 / OIDC.
- [ ] Enforce role-based access control (RBAC).
- [ ] Apply least-privilege access to users, services, and agents.
- [ ] Validate JWT tokens, claims, issuer, audience, and expiration.
- [ ] Enforce tenant-level access isolation.
- [ ] Secure service-to-service communication.
- [ ] Rotate API keys, certificates, and credentials regularly.
- [ ] Store secrets in a secure secrets manager.
- [ ] Avoid hardcoding credentials in source code or configuration files.

## 2. Data Security and Privacy

- [ ] Encrypt data at rest using approved encryption standards.
- [ ] Encrypt data in transit using TLS.
- [ ] Classify sensitive data (PII, financial, confidential, and regulated data).
- [ ] Mask or redact sensitive information before sending it to an LLM.
- [ ] Define data retention and deletion policies.
- [ ] Prevent unauthorized access to customer documents and conversations.
- [ ] Enforce tenant-level isolation for documents, embeddings, and conversation history.
- [ ] Validate data residency and regulatory requirements.
- [ ] Ensure customer data is not used for model training without explicit authorization.
- [ ] Apply data-loss prevention (DLP) controls where appropriate.

## 3. LLM Security and Prompt Injection

- [ ] Treat all user input as untrusted.
- [ ] Treat retrieved documents and external content as untrusted.
- [ ] Detect direct prompt injection attempts.
- [ ] Detect indirect prompt injection embedded in documents and web content.
- [ ] Separate system instructions from user-provided content.
- [ ] Never allow retrieved content to override system instructions.
- [ ] Restrict model access to sensitive information.
- [ ] Validate model output before executing downstream actions.
- [ ] Prevent the model from exposing system prompts, secrets, or internal instructions.
- [ ] Test jailbreak and instruction-override scenarios.

## 4. RAG Pipeline Security

### Document Ingestion

- [ ] Authenticate and authorize document uploads.
- [ ] Validate file types, file size, and document structure.
- [ ] Scan uploaded files for malware.
- [ ] Validate document ownership and access permissions.
- [ ] Extract metadata such as tenant ID, document ID, owner, and classification.
- [ ] Reject malformed or unsupported documents.
- [ ] Maintain document versioning and audit history.

### Chunking and Embeddings

- [ ] Apply consistent document chunking strategies.
- [ ] Avoid mixing data across tenants.
- [ ] Prevent sensitive information from entering unauthorized embedding stores.
- [ ] Secure embedding models and embedding APIs.
- [ ] Maintain metadata and access-control information with each chunk.
- [ ] Support document deletion and corresponding embedding removal.

### Retrieval and Context Construction

- [ ] Apply authorization filters before retrieving documents.
- [ ] Enforce tenant and user-level filtering.
- [ ] Retrieve only documents the requesting user is authorized to access.
- [ ] Validate retrieved content before passing it to the LLM.
- [ ] Limit retrieved context to relevant information.
- [ ] Protect against cross-tenant retrieval.
- [ ] Include source references or citations in generated answers.
- [ ] Handle missing, stale, or conflicting source documents.

## 5. Agentic AI and Tool Security

- [ ] Define an explicit list of tools an agent can access.
- [ ] Apply least-privilege permissions to every tool.
- [ ] Validate tool arguments before execution.
- [ ] Authenticate and authorize every tool invocation.
- [ ] Validate MCP server identity and permissions.
- [ ] Prevent agents from accessing unauthorized APIs or databases.
- [ ] Require human approval for high-impact or irreversible actions.
- [ ] Apply execution timeouts and resource limits.
- [ ] Prevent uncontrolled recursive agent execution.
- [ ] Limit the number of tool calls per workflow.
- [ ] Maintain an audit trail of agent plans, tool calls, and results.
- [ ] Validate tool responses before adding them to the agent context.
- [ ] Implement circuit breakers and failure handling for external tools.

## 6. Output Validation and Guardrails

- [ ] Validate LLM responses against expected output schemas.
- [ ] Detect personally identifiable information (PII) in responses.
- [ ] Detect confidential or restricted information.
- [ ] Apply content safety and policy filters.
- [ ] Check generated responses against trusted source material.
- [ ] Detect unsupported claims and potential hallucinations.
- [ ] Require citations for factual or policy-based answers.
- [ ] Reject invalid or malformed structured responses.
- [ ] Prevent generated SQL, shell commands, or code from executing without validation.
- [ ] Provide safe fallback responses when confidence is insufficient.

## 7. API and Application Security

- [ ] Implement API authentication and authorization.
- [ ] Apply rate limiting and request throttling.
- [ ] Validate and sanitize request parameters.
- [ ] Protect against SQL injection and command injection.
- [ ] Configure CORS policies appropriately.
- [ ] Enforce request size and timeout limits.
- [ ] Use API gateways for centralized security enforcement.
- [ ] Protect internal APIs from public exposure.
- [ ] Implement idempotency for sensitive operations.
- [ ] Avoid exposing stack traces or internal implementation details.

## 8. LLM Provider and Model Security

- [ ] Use approved enterprise LLM providers and models.
- [ ] Review provider data retention and privacy policies.
- [ ] Validate whether prompts and customer data are retained.
- [ ] Review model hosting and data residency requirements.
- [ ] Secure API keys and model credentials.
- [ ] Maintain an approved model inventory.
- [ ] Evaluate model changes before production rollout.
- [ ] Monitor model versions and configuration changes.
- [ ] Implement fallback models where appropriate.
- [ ] Assess third-party model and dependency risks.

## 9. Logging, Monitoring, and Auditability

- [ ] Log user identity, tenant ID, request ID, and workflow ID.
- [ ] Track model name, version, and configuration.
- [ ] Track prompt and response metadata securely.
- [ ] Record retrieval sources and document references.
- [ ] Log agent tool invocations and execution outcomes.
- [ ] Monitor prompt injection and guardrail violations.
- [ ] Monitor latency, token consumption, and API errors.
- [ ] Track hallucination and retrieval-quality indicators.
- [ ] Integrate monitoring with centralized observability platforms.
- [ ] Redact sensitive information from logs.
- [ ] Maintain tamper-resistant audit records where required.

## 10. AI Evaluation and Testing

- [ ] Create a representative evaluation dataset.
- [ ] Test retrieval precision and recall.
- [ ] Measure answer relevance and factual accuracy.
- [ ] Evaluate groundedness against trusted documents.
- [ ] Test prompt injection and jailbreak attacks.
- [ ] Test cross-tenant data access attempts.
- [ ] Test PII leakage and data-exfiltration scenarios.
- [ ] Test unauthorized tool invocation.
- [ ] Test model behavior with ambiguous or incomplete requests.
- [ ] Perform regression testing after prompt or model changes.
- [ ] Conduct red-team security testing.
- [ ] Define release criteria before production deployment.

## 11. Infrastructure and Deployment Security

- [ ] Deploy AI services in approved secure environments.
- [ ] Apply network segmentation and private connectivity.
- [ ] Restrict inbound and outbound network traffic.
- [ ] Secure containers and Kubernetes workloads.
- [ ] Scan container images and dependencies for vulnerabilities.
- [ ] Use non-root containers where possible.
- [ ] Apply Kubernetes RBAC and network policies.
- [ ] Secure CI/CD pipelines and deployment credentials.
- [ ] Perform dependency and software composition analysis.
- [ ] Separate development, testing, staging, and production environments.
- [ ] Maintain backup, disaster recovery, and incident response procedures.

## 12. Human-in-the-Loop Governance

- [ ] Define which AI decisions require human approval.
- [ ] Require human review for high-impact business decisions.
- [ ] Allow users to review and correct AI-generated results.
- [ ] Preserve human accountability for final decisions.
- [ ] Track approval, rejection, and override actions.
- [ ] Define escalation paths for uncertain or unsafe outputs.
- [ ] Document AI limitations and intended use.
- [ ] Maintain model and workflow governance records.

## 13. Production Readiness Checklist

Before releasing an AI application to production:

- [ ] Security architecture reviewed and approved.
- [ ] Threat modeling completed.
- [ ] Authentication and authorization tested.
- [ ] Tenant isolation validated.
- [ ] Prompt injection tests passed.
- [ ] Sensitive data leakage tests passed.
- [ ] RAG retrieval access controls validated.
- [ ] Agent tool permissions reviewed.
- [ ] Output guardrails validated.
- [ ] AI evaluation metrics meet acceptance criteria.
- [ ] Logging and monitoring enabled.
- [ ] Incident response plan documented.
- [ ] Human approval workflows tested.
- [ ] Rollback and disaster recovery procedures tested.
- [ ] Business, security, legal, and compliance approvals obtained.

---

## 14. Common AI Security Threats

| Threat | Description | Mitigation |
|---|---|---|
| Prompt Injection | Malicious instructions manipulate model behavior | Input validation, instruction separation, guardrails |
| Indirect Prompt Injection | Malicious instructions embedded in retrieved documents | Treat retrieved content as untrusted |
| Data Leakage | Sensitive information exposed through prompts or responses | RBAC, masking, DLP, tenant isolation |
| Cross-Tenant Access | One customer accesses another customer's data | Tenant-aware retrieval and authorization |
| Hallucination | Model generates unsupported information | RAG grounding, citations, evaluation |
| Excessive Agency | Agent performs unauthorized actions | Tool allowlists, least privilege, human approval |
| Insecure Tool Execution | Malicious or invalid tool arguments are executed | Schema validation, authorization, sandboxing |
| Model Supply Chain Risk | Vulnerable or compromised model dependencies | Approved models, scanning, version controls |
| Denial of Service | Excessive requests or token usage overload services | Rate limiting, quotas, timeouts |
| Sensitive Logging | Private information exposed through logs | Redaction, access control, retention policies |

## 15. Recommended Security Architecture

A secure enterprise AI application should follow this general flow:

```text
User
  |
  v
API Gateway
  |
  +--> Authentication / Authorization
  +--> Rate Limiting
  +--> Request Validation
  |
  v
AI Application / Orchestrator
  |
  +--> Input Guardrails
  +--> Prompt Injection Detection
  +--> PII Detection
  |
  v
RAG Retrieval Layer
  |
  +--> Tenant-Level Authorization
  +--> Vector Database
  +--> Document Metadata
  +--> Trusted Knowledge Sources
  |
  v
LLM Gateway
  |
  +--> Approved Models
  +--> Model Access Controls
  +--> Output Guardrails
  |
  v
Agent / Tool Execution
  |
  +--> MCP / REST Tools
  +--> Tool Authorization
  +--> Human Approval
  |
  v
Response to User

Cross-Cutting Controls:
  - Audit Logging
  - Monitoring and Alerting
  - AI Evaluation
  - Data Privacy
  - Governance
```

## 16. Key Principles to Remember

1. Never trust user input or retrieved documents.
2. Never allow an LLM to bypass application authorization.
3. Enforce access control before retrieving enterprise data.
4. Give agents only the permissions they need.
5. Validate every tool call and model-generated action.
6. Ground enterprise answers in approved, authoritative sources.
7. Keep humans involved in high-impact decisions.
8. Monitor AI behavior continuously in production.
9. Protect customer data throughout the AI lifecycle.
10. Treat AI security as an end-to-end application security responsibility.

---

**Objective:** Build enterprise AI systems that are secure, reliable, explainable, auditable, privacy-preserving, and safe for production use.

