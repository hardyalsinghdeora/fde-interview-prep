# AWS Bedrock & Google Vertex AI

Enterprises often refuse to send data to a model provider directly and insist on their own cloud account, so managed LLM platforms come up in almost every customer-facing AI role. Forward Deployed Engineer, Agentic-AI Architect and Software Architect interviews ask most, usually about residency, quotas, cost and when to bypass the platform.

> Related: [AI / LLM](../topics/06-ai-llm/README.md) · [Cloud, DevOps & integration](../topics/08-cloud-devops-integration/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is AWS Bedrock, and why would an enterprise use it instead of calling a model provider's API directly?
- [ ] What is Vertex AI, and how do Gemini on Vertex and Model Garden relate to each other? *(verify)*
- [ ] What does "your data is not used for training" mean contractually on these platforms, and how would you check it? *(verify)*
- [ ] On-demand vs. provisioned throughput — what are you paying for in each? *(verify)*
- [ ] How does authentication work: IAM roles on AWS vs. service accounts on Google Cloud?
- [ ] What is a quota / rate limit on these platforms (requests and tokens per minute), and how is it scoped (account, region, model)? *(verify)*
- [ ] What is a cross-region inference profile on Bedrock, and what does it mean for data residency? *(verify)*

## B. Working depth
- [ ] Bedrock Converse API vs. model-native request formats — why does a unified API matter? *(verify)*
- [ ] Bedrock Knowledge Bases: what does it do for ingestion, chunking, embeddings and the vector store behind it? *(verify)*
- [ ] Bedrock Agents: action groups, Lambda tools, session state — and what you give up vs. your own loop *(verify)*
- [ ] Bedrock Guardrails: content filters, denied topics, PII redaction, contextual grounding checks *(verify)*
- [ ] Vertex AI RAG Engine and Vertex AI Search — when would you use each? *(verify)*
- [ ] Vertex AI evaluation service: what metrics does it offer and how would you plug it into CI? *(verify)*
- [ ] How do you call these platforms privately (VPC endpoints / PrivateLink on AWS, Private Service Connect on Google Cloud)?
- [ ] How do you log prompts and responses for audit (model invocation logging, Cloud Logging) without leaking PII?
- [ ] Batch inference on Bedrock/Vertex — when is it cheaper, and what are the latency trade-offs? *(verify)*
- [ ] Prompt caching on managed platforms — is it available for your model, and how is it billed? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Managed platform vs. direct provider API: features lag, model availability, price, support, compliance — how do you decide?
- [ ] A new model version is out on the provider's API but not yet in the customer's region. What do you do?
- [ ] You hit throttling at peak. Walk through quota increases, provisioned capacity, cross-region routing and backpressure.
- [ ] How do you build a multi-provider gateway (Bedrock + Vertex + direct) with fallbacks, without the lowest common denominator problem?
- [ ] Estimate monthly cost for 2M requests/day at 3k input / 500 output tokens, and name three levers to cut it.
- [ ] How do you meet data residency for India-only or EU-only customers on each platform? *(verify)*
- [ ] How do managed guardrails compare with your own guardrail layer — latency, false positives, portability?
- [ ] What is the lock-in risk of Bedrock Agents / Knowledge Bases or Vertex Agent Builder, and how do you contain it? *(verify)*
- [ ] Fine-tuning or custom models on these platforms — when is it worth it vs. RAG and prompting? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A bank wants a document Q&A assistant, all data in their AWS account, in one region. Design it on Bedrock.
- [ ] The same customer later acquires a company on Google Cloud. How do you serve both without two codebases?
- [ ] Security asks "where exactly does the prompt go, and who can see it?". Draw the data flow and answer.
- [ ] Your Bedrock bill tripled in a month. How do you find the cause and stop it?
- [ ] Design a model-upgrade process on a managed platform: evaluation, shadow traffic, rollback.

## E. Hands-on checks (things to be able to DO)
- [ ] Call a model through the Bedrock Converse API with a tool definition, using an IAM role, from Node.js or Python.
- [ ] Call Gemini on Vertex AI with structured (JSON schema) output from a service account.
- [ ] Write a least-privilege IAM policy that allows invoking only one model in one region.
- [ ] Set up a Bedrock Knowledge Base or Vertex RAG corpus on a small document set and compare answers with your own RAG pipeline.

---

**Good answers show:**
- The real reason customers choose these platforms: their account, their IAM, their audit, their region.
- Awareness that features, model versions and prices change often, and a habit of checking current docs.
- Quota and capacity planning before launch, with a degradation plan for throttling.
- Cost estimates worked out from token volumes, not guessed.
- A thin abstraction that keeps the app portable across providers while still using platform strengths.
- Security and residency explained as a data-flow diagram, not reassurances.

**Prove it:** Build one small RAG service that can run against Bedrock and Vertex AI behind the same interface, with private networking, invocation logging and a cost-per-request log line; write up latency, cost and setup friction for each.
