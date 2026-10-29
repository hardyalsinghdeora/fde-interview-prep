# 06 — AI / LLM Questions

The core of FDE loops at OpenAI, Anthropic, Scale AI, Cohere, Mistral, Sierra, Decagon, Harvey, Glean and most AI startups. Many loops include a **take-home** (build a RAG system, an agent, or an eval harness on the company's API) followed by a live session where interviewers challenge your design choices.

> The differentiator question: **"How do you know your AI system is actually working?"**

---

## A. LLM fundamentals
- [ ] Explain how a transformer works (attention, self-attention, multi-head, positional encoding).
- [ ] Explain the whole transformer architecture end to end (reported at Databricks).
- [ ] What is tokenization? BPE vs. WordPiece vs. SentencePiece? Why does it matter for cost and for non-English text?
- [ ] What are embeddings? How are they produced and used?
- [ ] What's the context window? What happens when you exceed it?
- [ ] What are temperature, top-p, top-k? When would you change each?
- [ ] What's the difference between pre-training, fine-tuning, instruction tuning, and RLHF/RLAIF?
- [ ] What's Constitutional AI? (Anthropic)
- [ ] Encoder-only vs. decoder-only vs. encoder-decoder models — examples and use cases.
- [ ] Why do LLMs hallucinate? How do you reduce it?
- [ ] What is the KV cache? Why does it matter for latency?
- [ ] What is prompt caching and when does it save money?
- [ ] What's the difference between a base model and a chat/instruct model?
- [ ] What are reasoning / "thinking" models? When are they worth the extra latency and cost?
- [ ] Explain mixture-of-experts at a high level.
- [ ] What's quantization? Trade-offs?
- [ ] What's distillation?
- [ ] What's the "lost in the middle" problem with long contexts?
- [ ] What are scaling laws?
- [ ] How do multimodal models handle images/audio?
- [ ] Compare major model families (GPT, Claude, Gemini, Llama, Mistral). How do you pick one for a customer?
- [ ] Open-weight vs. closed API models: when would a customer choose each?

## B. Prompt engineering
- [ ] What makes a good prompt? Walk through how you'd write a system prompt for X.
- [ ] Zero-shot vs. few-shot vs. chain-of-thought prompting.
- [ ] How do you get reliable structured output (JSON) from an LLM?
- [ ] How do you make prompts robust to edge-case inputs?
- [ ] How do you version and test prompts?
- [ ] Here's a prompt that gives inconsistent output — fix it.
- [ ] How do you handle a prompt that works on one model but not another?
- [ ] Prompt chaining vs. a single big prompt: trade-offs?
- [ ] How do you use XML tags / delimiters / role prompting?
- [ ] How would you reduce verbosity, or make the model refuse less / more appropriately?
- [ ] How do you write prompts for long documents (put docs first, quote extraction, etc.)?
- [ ] When does prompting stop being enough and you need fine-tuning or RAG?

## C. RAG (Retrieval-Augmented Generation)
- [ ] Explain RAG end to end.
- [ ] Why RAG instead of fine-tuning? When is fine-tuning better?
- [ ] How do you chunk documents? Why this chunking strategy? What chunk size and overlap?
- [ ] How do you handle tables, PDFs, images, and code in RAG?
- [ ] Which embedding model would you choose and why?
- [ ] Vector DB options (pgvector, Pinecone, Weaviate, Qdrant, Milvus, FAISS, Elasticsearch) — how do you choose?
- [ ] ANN algorithms: HNSW vs. IVF vs. flat — trade-offs.
- [ ] Dense vs. sparse (BM25) vs. hybrid retrieval. When does each win?
- [ ] What is re-ranking (cross-encoders)? When is it worth it?
- [ ] Query rewriting, HyDE, multi-query retrieval — what and why?
- [ ] How do you handle permissions / access control in RAG?
- [ ] How do you keep the index fresh when documents change?
- [ ] How do you cite sources and prevent the model from answering outside the context?
- [ ] Retrieval returns the wrong documents. How do you debug it?
- [ ] What changes if the dataset becomes 100x larger?
- [ ] How do you evaluate retrieval separately from generation (recall@k, MRR, nDCG)?
- [ ] GraphRAG / knowledge graphs — when are they useful?
- [ ] Long-context models vs. RAG — when would you just stuff the context?
- [ ] Contextual retrieval (adding chunk context before embedding) — what problem does it solve?
- [ ] Multi-tenant RAG: how do you isolate customers' data?

## D. Agents and tool use
- [ ] What is an AI agent? How is it different from a chain/workflow?
- [ ] Explain function calling / tool use. How does the model decide which tool to call?
- [ ] How do you design good tool definitions (names, descriptions, schemas)?
- [ ] ReAct, plan-and-execute, reflection — explain each pattern.
- [ ] When should you *not* use an agent?
- [ ] How do you stop an agent from looping forever or taking dangerous actions?
- [ ] Human-in-the-loop: where would you put approval steps?
- [ ] How do you handle tool errors and retries inside an agent loop?
- [ ] How do you manage memory (short-term, long-term, summarisation) for agents?
- [ ] Multi-agent systems: orchestrator–worker, handoffs. Trade-offs?
- [ ] How do you evaluate an agent (task success, trajectory eval, tool-call accuracy)?
- [ ] How do you make agents deterministic enough for enterprise use?
- [ ] Computer use / browser agents — risks and use cases.
- [ ] What's MCP (Model Context Protocol)? What problem does it solve? Tools vs. resources vs. prompts.
- [ ] How would you build an MCP server for a customer's internal system? How do you secure it?
- [ ] OpenAI Assistants/Responses API, Agents SDK, Claude Agent SDK — what do they give you?
- [ ] Frameworks (LangChain, LangGraph, LlamaIndex, CrewAI, DSPy) — when would you use or avoid them?

## E. Evaluation (most important)
- [ ] How do you know your AI system is actually working?
- [ ] How do you build an eval set when the customer has no labelled data?
- [ ] Offline evals vs. online evals vs. A/B tests.
- [ ] LLM-as-a-judge: how do you make it reliable? How do you validate the judge?
- [ ] What metrics would you use for: classification, extraction, summarisation, Q&A, agents?
- [ ] How do you measure hallucination / faithfulness / groundedness?
- [ ] How do you catch regressions when you change the prompt or model?
- [ ] How do you balance quality vs. latency vs. cost in evals?
- [ ] Golden datasets: how big, how to build, how to maintain?
- [ ] How do you collect user feedback and turn it into evals?
- [ ] Tools: promptfoo, Ragas, DeepEval, LangSmith, Braintrust, OpenAI Evals — have you used any?
- [ ] The customer says "it feels worse this week." How do you investigate?

## F. Fine-tuning and model customisation
- [ ] When should you fine-tune vs. prompt vs. RAG?
- [ ] SFT vs. RLHF vs. DPO — differences.
- [ ] LoRA / QLoRA / PEFT — how do they work?
- [ ] How much data do you need for fine-tuning? How do you prepare it?
- [ ] How do you evaluate a fine-tuned model? Catastrophic forgetting?
- [ ] Fine-tuning for style vs. for knowledge — which works?
- [ ] Synthetic data generation: how and risks?
- [ ] Explain training strategies for a domain-specific model (reported at Databricks).

## G. Production LLM engineering
- [ ] How do you handle rate limits (429s) from an LLM API? Retries, backoff, queueing.
- [ ] How do you reduce latency (streaming, smaller models, caching, parallel calls, speculative decoding)?
- [ ] How do you reduce cost (model routing, caching, batching, prompt compression, batch APIs)?
- [ ] How do you handle model deprecation / version upgrades for a customer?
- [ ] How do you monitor an LLM app in production (traces, tokens, latency, quality, drift)?
- [ ] How do you handle timeouts and partial failures in streaming responses?
- [ ] How do you design fallbacks between model providers?
- [ ] How do you handle PII in prompts and logs?
- [ ] Data residency / zero-data-retention requirements — how do you meet them?
- [ ] Batch vs. real-time inference: when to use each.
- [ ] Self-hosting a model (vLLM, TGI, Triton) — when and what does it take?
- [ ] GPU sizing basics: how many tokens/sec, how much memory for a 70B model?

## H. Safety, security and responsible AI
- [ ] What is prompt injection? Direct vs. indirect. How do you defend against it?
- [ ] What is jailbreaking? How do you mitigate it?
- [ ] How do you prevent data leakage between users/tenants in an LLM app?
- [ ] What guardrails would you add for a healthcare/finance chatbot?
- [ ] How do you handle bias and fairness concerns?
- [ ] How do you handle a customer who wants to use the model for a borderline use case?
- [ ] OWASP Top 10 for LLM applications — name a few.
- [ ] How do you red-team an LLM application?
- [ ] What's your view on AI safety and the role of deployment in it? (Anthropic/OpenAI)
- [ ] How would you explain model limitations honestly to a customer who wants guarantees?

## I. Classic ML (sometimes asked)
- [ ] Bias–variance trade-off.
- [ ] Precision, recall, F1, ROC-AUC — when to use which?
- [ ] Overfitting — how to detect and prevent.
- [ ] Train/validation/test split; cross-validation; data leakage.
- [ ] Class imbalance — how to handle.
- [ ] Explain gradient descent; regularisation (L1/L2).
- [ ] Decision trees vs. random forests vs. gradient boosting.
- [ ] Feature engineering for tabular data.
- [ ] How do you deploy and monitor a classical ML model (drift, retraining)?
- [ ] MLOps: feature stores, model registries, CI/CD for models.

## J. Take-home / build exercises (reported)
- [ ] Build a RAG system over a document set using our API; include an eval.
- [ ] Build an agent that uses 2–3 tools to complete a task.
- [ ] Build an evaluation harness for an LLM feature.
- [ ] Build a customer-support bot prototype and present it as if to the customer.
- [ ] Build a structured-data extraction pipeline from unstructured documents.
- [ ] Live follow-up: Why this chunking strategy? Why not another retrieval method? What would you change at 100x data? What failed? What would you do with another week?

## K. Recent topics (2025–2026)
- [ ] What is context engineering, and how is it different from prompt engineering?
- [ ] What is the A2A (Agent2Agent) protocol, and how does it relate to MCP?
- [ ] Estimate the monthly cost of an LLM feature for a customer (back-of-envelope).
- [ ] When would you use a small or on-device model instead of a frontier model?
- [ ] What are time-series foundation models, and should a customer use one for forecasting?

## L. MCP in depth
- [ ] Walk through an MCP connection: discovery, capabilities and versions, then requests. What changed in the 2026-07-28 revision?
- [ ] stdio vs. Streamable HTTP: when do you use each transport, and what replaced HTTP+SSE?
- [ ] MCP vs. plain function calling vs. a REST/OpenAPI integration: when is MCP worth it?
- [ ] Explain sampling, elicitation and roots with an example of each.
- [ ] Write a small MCP server with one tool, one resource and one prompt, and test it.
- [ ] How do you design good MCP tools (granularity, names, descriptions, output size, errors)?
- [ ] How does authorization work for a remote MCP server?
- [ ] What are the main MCP security risks (tool poisoning, rug pulls, confused deputy, token passthrough, injection through tool results) and how do you mitigate them?
- [ ] How do you handle long-running operations, progress and cancellation in MCP?
- [ ] How do you deploy and scale a remote MCP server in production?
- [ ] A client has hundreds of MCP tools available. How do you keep tool selection accurate and context cost low?
- [ ] How do you test and evaluate an MCP server before giving it to a customer?
- [ ] How would you roll out MCP across an enterprise (registry, gateway, allowlists, observability)?
- [ ] MCP vs. A2A vs. Agent Skills: when do you use which?

## M. More LLM topics
- [ ] What are logprobs, and what can you use them for?
- [ ] Decoding strategies: greedy, beam search, sampling, and constrained decoding. How are structured outputs enforced?
- [ ] What is agentic RAG, and when is it better than a fixed retrieval pipeline?
- [ ] What are late-interaction (ColBERT-style) and multi-vector retrieval, and when are they worth it?
- [ ] What is reinforcement fine-tuning (RL with verifiable rewards, GRPO), and when would a customer use it?
- [ ] How would you roll out a coding agent (Claude Code, Codex, Cursor) safely in a customer's engineering organisation?
- [ ] How do you handle a model's knowledge cutoff and questions about recent events?
