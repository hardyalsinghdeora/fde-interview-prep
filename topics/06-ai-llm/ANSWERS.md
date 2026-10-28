# 06 — AI / LLM: Model Answers

Questions: [README.md](README.md)

This file covers sections A–E (fundamentals, prompting, RAG, agents, evaluation). Sections F–K are in [ANSWERS-part2.md](ANSWERS-part2.md).

**How to use these answers:** these are reference answers, not scripts. Adapt them to your own projects and numbers, and practise saying them out loud in 1–2 minutes. Model names, prices and limits change fast — check current docs before an interview and say "as of the last time I checked" when quoting specifics.

---

## A. LLM fundamentals

### Explain how a transformer works (attention, self-attention, multi-head, positional encoding).

- **Core idea:** every token builds its representation by looking at every other (allowed) token and taking a weighted mix of their information. No recurrence, so it parallelises well on GPUs.
- **Attention:** each token is projected into a query `Q`, key `K` and value `V`. Scores are `QKᵀ / √d_k` (scaling keeps softmax from saturating), softmaxed into weights, then used to average the `V`s: `Attention(Q,K,V) = softmax(QKᵀ/√d_k)·V`.
- **Self-attention:** Q, K and V all come from the same sequence. In decoders a **causal mask** stops a token attending to future tokens. **Cross-attention** (encoder-decoder) takes Q from the decoder and K/V from the encoder.
- **Multi-head:** run h attention heads in parallel, each with its own smaller projections, then concatenate and project. Different heads learn different relations (syntax, coreference, position). Variants like **GQA/MQA** share K/V across heads to shrink the KV cache.
- **Positional encoding:** attention is order-blind, so position is injected. Original paper: fixed sinusoids added to embeddings. Modern LLMs mostly use **RoPE** (rotary embeddings that rotate Q/K by position, encoding relative distance) or ALiBi; these extrapolate better to longer contexts.
- Cost: attention is O(n²) in sequence length for compute and memory (mitigated by FlashAttention, sliding windows, etc.).

### Explain the whole transformer architecture end to end (reported at Databricks).

Walk the data path for a decoder-only LLM:

1. **Tokenize** text into integer IDs (BPE etc.).
2. **Embedding lookup:** each ID → a d_model vector (e.g. 4096). Position information added (or applied as RoPE inside attention).
3. **N stacked blocks** (e.g. 32–100+). Each block:
   - LayerNorm/RMSNorm (pre-norm in modern models)
   - Multi-head causal self-attention
   - Residual connection (add input back)
   - Norm → **feed-forward network** (MLP, ~4x width with GELU; ~8/3x with SwiGLU's three matrices) — or an MoE layer
   - Residual connection
4. **Final norm → unembedding (LM head):** project to vocabulary size → logits.
5. **Softmax + sampling** (temperature, top-p) → next token, appended to input; repeat autoregressively, reusing the **KV cache**.

Mention the original **encoder-decoder** (Vaswani 2017): encoder stack with bidirectional attention, decoder stack with masked self-attention plus cross-attention to the encoder. Training objective for LLMs: next-token prediction with cross-entropy loss, teacher forcing, all positions in parallel. Residuals + normalisation are what make very deep stacks trainable; attention mixes information *across* tokens, the MLP processes *each* token (and is where much "knowledge" is stored).

### What is tokenization? BPE vs. WordPiece vs. SentencePiece? Why does it matter for cost and for non-English text?

Tokenization splits text into subword units from a fixed vocabulary (typically 32k–200k+ tokens), so any string can be represented without an unbounded vocabulary.

- **BPE (Byte-Pair Encoding):** start from characters/bytes, repeatedly merge the most frequent adjacent pair. Byte-level BPE (GPT family, tiktoken) never has unknown tokens.
- **WordPiece (BERT):** similar merging, but picks merges that maximise training-data likelihood; uses `##` continuation prefixes.
- **SentencePiece:** a library/framework that treats input as a raw stream (spaces encoded as `▁`), language-agnostic, no pre-tokenization; supports BPE or **Unigram LM** (start large, prune tokens). Used by Llama 2, T5, many multilingual models.

**Why it matters:**
- **Cost and limits** are billed and enforced per token, not per character. English averages ~4 chars/token; code, JSON, numbers and whitespace can be much less efficient.
- **Non-English text** (Hindi, Thai, Arabic, CJK) can take 2–5x more tokens for the same meaning on English-heavy vocabularies → higher cost, higher latency, less effective context. Benchmark token counts on the customer's actual language mix before quoting costs.
- Tokenization causes quirks: poor character counting/spelling, arithmetic on multi-digit numbers, trailing-whitespace sensitivity. Different model families have different tokenizers, so token counts aren't portable — use the provider's token-counting endpoint.

### What are embeddings? How are they produced and used?

An embedding is a dense vector (e.g. 384–3072 dimensions) where semantic similarity corresponds to geometric closeness (usually cosine similarity).

- **Produced by:** an encoder model (often a transformer) whose output is pooled (mean pooling or [CLS]/last-token) into one vector. Trained with **contrastive learning**: pull together (query, relevant passage) pairs, push apart negatives (hard negatives matter). Examples: OpenAI `text-embedding-3-*`, Cohere Embed, Voyage, Google, open models like BGE, E5, GTE, Nomic, Jina.
- Token embeddings inside an LLM are a different (but related) thing: the input lookup table.
- **Uses:** semantic search and RAG retrieval, clustering/topic discovery, deduplication, classification (embeddings + logistic regression is a strong cheap baseline), recommendation, anomaly detection, routing queries to intents.
- **Practical points:** embed queries and documents with the same model (some need `query:`/`passage:` prefixes or an input-type flag); normalise vectors; re-embed everything if you change models; Matryoshka-trained models let you truncate dimensions to save storage.

### What's the context window? What happens when you exceed it?

The context window is the maximum number of tokens the model processes in one call — **input plus output** (system prompt, tools, history, retrieved docs, and the generated response). Current frontier models range roughly 128k to 1M+ tokens; output is capped separately (`max_tokens`).

When you exceed it:
- **API:** typically a 400 error ("prompt is too long"), or generation stops with a `max_tokens`/length stop reason if output runs out.
- **Chat apps / frameworks:** silently truncate or drop old messages — so the model "forgets" early instructions.
- Even below the limit, quality can drop with very long contexts (lost in the middle, distraction), and latency and cost grow with input size.

Mitigations: count tokens before sending; trim or summarise history; retrieve only relevant chunks (RAG); put stable content first to use prompt caching; use context-editing/compaction features where the API offers them; split work into map-reduce over chunks.

### What are temperature, top-p, top-k? When would you change each?

All control sampling from the next-token distribution:

- **Temperature:** divides logits before softmax. Low (0–0.3) → sharper, more deterministic; high (0.8–1.2) → flatter, more diverse. 0 ≈ greedy (still not perfectly deterministic due to batching/floating-point effects).
- **Top-k:** sample only from the k most likely tokens.
- **Top-p (nucleus):** sample from the smallest set of tokens whose cumulative probability ≥ p (e.g. 0.9). Adapts to how confident the model is.

When to change:
- Extraction, classification, code, tool calls → low temperature.
- Brainstorming, creative writing, generating diverse synthetic data → higher temperature.
- Generally **change one, not both** temperature and top-p (some providers recommend or enforce this). Top-k is mostly used to cut off long-tail garbage.
- Reasoning models often fix or restrict these parameters — check the docs.
- Sampling won't fix a bad prompt; inconsistency is usually a prompt-clarity problem first.

### What's the difference between pre-training, fine-tuning, instruction tuning, and RLHF/RLAIF?

- **Pre-training:** self-supervised next-token prediction on trillions of tokens of web, code, books. Produces a **base model** with broad knowledge and capability but no notion of "follow instructions". Most of the compute.
- **Fine-tuning (general):** continue training on a smaller, targeted dataset to adapt behaviour or domain. Can be full-parameter or PEFT (LoRA).
- **Instruction tuning / SFT:** fine-tune on (instruction, ideal response) pairs and multi-turn dialogues so the model follows requests and uses a chat format.
- **RLHF:** humans compare model outputs → train a **reward model** on preferences → optimise the policy against it (PPO), with a KL penalty to stay near the SFT model. Improves helpfulness, harmlessness, tone. **DPO** is a popular simpler alternative that optimises directly on preference pairs without a separate reward model.
- **RLAIF:** same idea, but preference labels come from an AI model guided by principles (e.g. Constitutional AI). Cheaper and more scalable than human labelling.
- Newer: **RL with verifiable rewards** (unit tests, math answers) is the main driver of reasoning models.

### What's Constitutional AI? (Anthropic)

A method Anthropic introduced (2022) to train a harmless, helpful assistant using a written set of principles (a "constitution") instead of relying only on human labels for harmful content.

Two phases:
1. **Supervised (critique and revise):** the model answers red-team prompts, then critiques its own response against a sampled principle and revises it. Fine-tune on the revised responses.
2. **RL from AI Feedback (RLAIF):** the model compares pairs of responses according to the principles; those AI preferences train a preference model, which is used for RL.

Why it matters:
- **Transparency:** the values are written down and inspectable, rather than implicit in thousands of labels.
- **Scale:** less human exposure to harmful content, cheaper feedback.
- **Better trade-off:** aims for models that are harmless *without being evasive* — explaining objections rather than refusing flatly.

Anthropic has since published an updated constitution for Claude (2026; verify current wording) describing priorities (broadly safe, broadly ethical, following guidelines, genuinely helpful) and reasoning, not just rules. In an interview, connect it to deployment: it is why Claude tends to explain refusals and why system prompts can shape but not override core safety behaviour.

### Encoder-only vs. decoder-only vs. encoder-decoder models — examples and use cases.

| Type | Attention | Examples | Good for |
|---|---|---|---|
| Encoder-only | Bidirectional | BERT, RoBERTa, DeBERTa, most embedding and reranker models | Classification, NER, embeddings, cross-encoder reranking — understanding, not generation |
| Decoder-only | Causal (left-to-right) | GPT, Claude, Llama, Mistral, Gemini (mostly) | General generation, chat, code, reasoning, agents; scale best and dominate today |
| Encoder-decoder | Bidirectional encoder + causal decoder with cross-attention | T5, BART, original Transformer, Whisper | Seq2seq: translation, summarisation, speech-to-text |

FDE angle: you usually call a decoder-only API for generation, but a small encoder model is often the right tool for cheap, fast, high-volume classification, embeddings or reranking — sometimes fine-tuned on a few thousand labels.

### Why do LLMs hallucinate? How do you reduce it?

**Why:**
- The objective is to produce *plausible* next tokens, not verified truth; the model has no built-in "I don't know" signal.
- Knowledge gaps (long-tail facts, post-cutoff events, private data) get filled with plausible patterns.
- Training and evals have rewarded confident guessing over abstaining.
- Prompt pressure: leading questions, requests for specifics (citations, numbers) the model doesn't have, long noisy context.
- Sampling randomness and compounding errors in long generations.

**Reduce it:**
- **Ground it:** RAG with good retrieval; tell it to answer *only* from provided context and to say "I don't know" when it isn't there.
- **Require citations/quotes** and verify them programmatically (does the quote appear in the source?).
- Give it tools (search, calculator, database) instead of relying on memory.
- Lower temperature for factual tasks; use stronger/reasoning models for hard tasks.
- Structured outputs with an explicit `"unknown"`/null option.
- Post-hoc checks: a verifier/judge pass for faithfulness, self-consistency across samples.
- **Measure it:** a faithfulness eval set, tracked over time. You can reduce hallucination, not eliminate it — design UX (citations, human review for high-stakes outputs) accordingly.

### What is the KV cache? Why does it matter for latency?

During autoregressive generation, each new token attends to all previous tokens. The keys and values for previous tokens don't change, so they are **cached** instead of recomputed every step.

- Without it, each new token would recompute attention over the full prefix → quadratic waste. With it, each decode step only computes Q/K/V for the new token.
- Two phases: **prefill** (process the whole prompt in parallel, compute-bound, drives **time-to-first-token**) and **decode** (one token at a time, memory-bandwidth-bound, drives **tokens/sec**).
- **Memory cost:** `2 (K,V) × layers × kv_heads × head_dim × seq_len × bytes × batch`. For long contexts and big batches the KV cache can exceed the model weights in memory — which limits concurrency and throughput.
- Optimisations: GQA/MQA (fewer KV heads), KV quantisation, **PagedAttention** (vLLM) to avoid fragmentation, prefix sharing.
- Prompt caching is essentially persisting the KV cache for a shared prefix across requests.

### What is prompt caching and when does it save money?

Prompt caching stores the processed (KV) state of a prompt **prefix** so later requests that start with the exact same prefix skip recomputing it.

- **Anthropic:** you mark breakpoints with `cache_control`; cache writes cost more than base input (about 1.25x for the 5-minute TTL, 2x for 1-hour), cache reads are about 0.1x base input price (lower on some newer models); caches are isolated per workspace/org, never shared across customers. **OpenAI:** automatic for prompts over ~1024 tokens, discounted cached input. **Gemini:** implicit and explicit context caching. (Multipliers and minimums change — verify.)
- Also cuts **time-to-first-token** substantially for long prefixes.

Saves money when:
- A large, **stable prefix** is reused often within the TTL: long system prompts, tool definitions, few-shot examples, a big document users ask many questions about, agent loops (the growing conversation is re-sent every turn), multi-turn chat.
- Order matters: put static content first (tools → system → documents → history), dynamic content last. Any change in the prefix (even a timestamp in the system prompt) invalidates everything after it.

Doesn't help: short prompts below the minimum cacheable length (model-dependent, from a few hundred to a few thousand tokens), unique prompts, low traffic where the cache expires between calls (then you pay the write premium for nothing). Check `cache_read_input_tokens` in usage to confirm hits.

```python
client.messages.create(
    model=MODEL, max_tokens=1024,
    system=[{"type": "text", "text": LONG_POLICY_DOC,
             "cache_control": {"type": "ephemeral"}}],
    messages=[{"role": "user", "content": question}],
)
```

### What's the difference between a base model and a chat/instruct model?

- **Base model:** output of pre-training only. It continues text. Ask it a question and it may produce more questions, or a forum thread. Useful for: completion-style tasks, few-shot pattern continuation, as a starting point for your own fine-tuning, research, sometimes more diverse/creative text and better-calibrated probabilities.
- **Chat/instruct model:** base + SFT + preference tuning (RLHF/DPO/RLAIF). Follows instructions, uses a role-based chat template (system/user/assistant), refuses harmful requests, has a consistent persona, supports tool calling.
- Practical: virtually all API products are instruct models. If you self-host open weights, make sure you use the instruct variant and its **exact chat template** — a wrong template silently degrades quality.

### What are reasoning / "thinking" models? When are they worth the extra latency and cost?

Models trained (largely via RL on verifiable tasks) to generate an extended internal chain of thought before the final answer — e.g. OpenAI o-series / GPT-5 reasoning modes, Claude extended/adaptive thinking, Gemini thinking, DeepSeek-R1. Many let you set an effort level or thinking token budget.

They spend **test-time compute**: more tokens → better answers on hard problems, at the cost of latency (seconds to minutes) and output tokens (thinking is billed).

**Worth it for:** multi-step math/logic, complex coding and debugging, planning in agents, analysing long contracts with subtle conditions, tasks where errors are expensive and you've seen non-reasoning models fail on your evals.

**Not worth it for:** simple classification/extraction, chit-chat, latency-sensitive UX (voice, autocomplete), high-volume cheap tasks. Common pattern: route — a fast model for most traffic, escalate hard cases to reasoning, or use reasoning offline (planning, generating eval data). Decide with evals: measure quality gain vs. p95 latency and cost per task. Prompting note: give high-level goals rather than step-by-step "think like this" instructions; don't rely on reading the thinking for compliance.

### Explain mixture-of-experts at a high level.

In an MoE layer, the single feed-forward block is replaced by **many expert FFNs** plus a small **router** (gating network). For each token, the router picks the top-k experts (e.g. 2 of 8, or 8 of 256) and combines their outputs weighted by router scores.

- **Benefit:** huge total parameter count (more capacity/knowledge) but only a fraction is active per token, so compute per token is like a much smaller dense model. E.g. Mixtral 8x7B: ~47B total, ~13B active. DeepSeek-V3: ~671B total, ~37B active. Many frontier models are believed to be MoE.
- **Costs:** all experts must still fit in memory (high VRAM), harder to train (load balancing losses to avoid collapse onto few experts), communication overhead across GPUs (expert parallelism), and less efficient at small batch sizes.
- For a customer: MoE open-weight models give strong quality per FLOP but need serious multi-GPU hosting.

### What's quantization? Trade-offs?

Representing weights (and sometimes activations or the KV cache) with fewer bits: FP16/BF16 → INT8, FP8, INT4 (or lower).

- **Benefits:** 2–4x less memory (a 70B model: ~140 GB in FP16, ~70 GB in 8-bit, ~35–40 GB in 4-bit), fits on fewer/cheaper GPUs or on-device, faster decode (decode is memory-bandwidth bound), higher batch sizes.
- **Methods:** post-training quantization (GPTQ, AWQ, SmoothQuant, GGUF/llama.cpp formats), FP8 on Hopper/Blackwell GPUs, quantization-aware training. QLoRA fine-tunes on top of a 4-bit base.
- **Trade-offs:** quality loss, usually small at 8-bit, noticeable at 4-bit and below, and worse on reasoning, math, long context and non-English than headline benchmarks suggest. Kernel/hardware support varies; some formats are slower if not natively supported.
- Advice: run *your* evals on the quantized model before committing.

### What's distillation?

Training a smaller **student** model to imitate a larger **teacher**.

- **Classic (Hinton):** train on the teacher's soft probability distributions (logits with temperature), which carry more information than hard labels.
- **LLM practice:** generate high-quality outputs (often including reasoning traces) from a frontier model on your task distribution, then SFT a smaller model on them. Several providers offer this as a product (store teacher completions → fine-tune smaller model).
- **Why:** much lower latency and cost at scale with near-teacher quality on a *narrow* task.
- **Caveats:** student generalises poorly outside the distilled distribution; check provider terms (many prohibit using outputs to train competing models); quality ceiling is the teacher; you need an eval to prove parity.
- FDE use case: prototype with the big model, collect production traffic + evals, then distil once volume justifies it.

### What's the "lost in the middle" problem with long contexts?

Research (Liu et al., 2023) showed LLMs use information at the **beginning and end** of a long context much better than information in the **middle** — a U-shaped accuracy curve on multi-document QA. Recent models are much better on simple needle-in-a-haystack tests, but degradation still appears on harder tasks: multiple facts to combine, distractors, reasoning over long inputs ("context rot").

Mitigations:
- Retrieve and include fewer, more relevant chunks; rerank so the best are first.
- Put long documents at the **top** and the question/instructions at the **end**.
- Ask the model to first extract relevant quotes, then answer from them.
- Split into map-reduce: process sections independently, then combine.
- Test with your own long-context evals, placing answers at various depths.

### What are scaling laws?

Empirical power-law relationships: loss falls predictably as you increase **parameters (N)**, **training data (D)** and **compute (C ≈ 6·N·D FLOPs)**.

- **Kaplan et al. (OpenAI, 2020):** loss scales smoothly with each; suggested prioritising model size.
- **Chinchilla (DeepMind, 2022):** for a fixed compute budget, scale parameters and tokens roughly equally — about **20 tokens per parameter** is compute-optimal. Many earlier models were undertrained.
- In practice, models are now trained far beyond Chinchilla-optimal (e.g. trillions of tokens for 8B models) because **inference** cost matters more than training cost — a smaller model trained longer is cheaper to serve.
- **Test-time compute scaling:** reasoning models show accuracy improves with more inference tokens — a second scaling axis.
- Why it matters: labs can predict big-model performance from small runs; capabilities keep improving, so design customer systems to swap in better models easily.

### How do multimodal models handle images/audio?

- **Images:** split into patches (e.g. 14x14 or 16x16 pixels), encode with a vision encoder (ViT, often CLIP/SigLIP-style), then a projector/adapter maps patch features into the LLM's token embedding space. The LLM sees them as a sequence of "image tokens" interleaved with text. Some models are natively multimodal (trained jointly from the start). Images cost tokens — roughly proportional to resolution (often around a thousand-plus tokens for a large image).
- **Audio:** convert to a spectrogram, encode with an audio encoder (Whisper-like) into tokens; or use discrete audio tokens (codec models) so the model can also *generate* speech. Speech-to-speech/realtime models skip the separate STT → LLM → TTS pipeline, lowering latency and preserving tone.
- **Video:** sample frames + audio track.
- **Output:** most LLMs output text; image generation uses diffusion or autoregressive image tokens.
- FDE notes: vision is great for documents, charts, screenshots, and forms, but check accuracy on small text, dense tables, and counting; resize images to control cost; for voice, latency budget drives architecture.

### Compare major model families (GPT, Claude, Gemini, Llama, Mistral). How do you pick one for a customer?

High-level (verify current versions before the interview):
- **OpenAI GPT / o-series:** broad ecosystem, strong reasoning modes, multimodal (realtime voice, image), Responses API and Agents SDK, Azure OpenAI for enterprise.
- **Anthropic Claude (Opus/Sonnet/Haiku):** strong at coding, agentic tool use, long documents, instruction following and writing; extended thinking; available via API, AWS Bedrock, Google Vertex.
- **Google Gemini:** very long context, strong multimodal (video/audio), tight GCP/Workspace integration, competitive price tiers (Flash).
- **Meta Llama:** open weights, huge community, self-host or via clouds; good for fine-tuning and data control.
- **Mistral:** European provider, open-weight and commercial models, efficient smaller models, on-prem/sovereignty friendly. (Also: DeepSeek, Qwen as strong open-weight options; Cohere for enterprise RAG/embeddings/rerank.)

**How to pick** (don't pick on brand or leaderboards):
1. Hard constraints: data residency, compliance (HIPAA, FedRAMP), cloud the customer already uses, on-prem requirement, contract terms.
2. Task needs: modality, context length, tool use, latency, languages.
3. **Run the customer's eval set** on 2–4 candidates; compare quality, p50/p95 latency, cost per task.
4. Operational: rate limits, SLAs, deprecation policy, support.
5. Design for portability (abstraction layer, provider-agnostic evals) and use routing (small model for easy, large for hard).

### Open-weight vs. closed API models: when would a customer choose each?

**Closed API (GPT, Claude, Gemini):**
- Best frontier quality, especially for reasoning, coding and agents.
- No infra: pay per token, fast to start, elastic scaling, continuous improvements.
- Enterprise options: zero data retention, regional endpoints, available through AWS/GCP/Azure under existing contracts.
- Downsides: data leaves your boundary (contractually protected), vendor dependency, model deprecations, rate limits, less control over versions.

**Open-weight (Llama, Mistral, Qwen, DeepSeek, Gemma):**
- Full control: on-prem/air-gapped, strict sovereignty, no third-party data processing.
- Deep customisation: full fine-tuning, distillation, inspect logits, custom decoding.
- Cost at **high, steady volume** can be lower; predictable latency on dedicated hardware.
- Downsides: you own GPUs, serving (vLLM/TGI), scaling, security patches, evals; usually below frontier quality on hard tasks; total cost of ownership often underestimated (engineers + idle GPUs).

Rule of thumb: start with a closed API to prove value fast; move specific high-volume narrow workloads to open-weight (often distilled/fine-tuned) once the eval proves parity and volume justifies it — or go open-weight from day one if regulation demands it.

---

## B. Prompt engineering

### What makes a good prompt? Walk through how you'd write a system prompt for X.

A good prompt is what you'd give a smart new contractor with zero context: clear goal, context, constraints, examples, output format, and what to do when unsure.

Example: system prompt for a **customer-support assistant for an internet provider**.

1. **Role and goal:** "You are a support assistant for Acme Fiber. Your goal is to resolve billing and connectivity issues in as few turns as possible."
2. **Context:** who the users are, what tools/data are available, today's date, the product catalog or policy docs (in tags).
3. **Rules with reasons:** "Never promise refunds over $50 — those need a human agent, because finance must approve them." Reasons help the model generalise.
4. **Process:** "First identify the issue type; for connectivity, run `check_line_status` before suggesting a reboot."
5. **Edge cases:** angry customer, out-of-scope requests, unknown answers → "Say you don't know and offer a handoff."
6. **Output format and tone:** concise, plain text, no markdown (it renders in SMS), match the customer's language.
7. **Examples:** 2–3 short examples of ideal exchanges, varied.

```text
<role>You are Acme Fiber's support assistant...</role>
<policies>{{policy_doc}}</policies>
<rules>
- Only answer using <policies> and tool results. If unsure, say so and offer a human.
- Refunds over $50: escalate with `create_ticket`; finance must approve them.
</rules>
<format>2–4 short sentences, plain text, same language as the customer.</format>
```

Then: **build a small eval set first** (20–50 realistic cases, including adversarial ones), iterate on the prompt against it, and version it.

### Zero-shot vs. few-shot vs. chain-of-thought prompting.

- **Zero-shot:** instructions only, no examples. Works well with modern instruct models for common tasks; cheapest and easiest to maintain. Start here.
- **Few-shot:** include 2–5 input → output examples. Best for showing an exact format, tone, or tricky decision boundary that's hard to describe. Make examples **diverse** (the model over-copies surface features), representative of hard cases, and wrapped in tags (`<example>`). Costs tokens (mitigate with caching). Can bias the label distribution.
- **Chain-of-thought (CoT):** ask the model to reason step by step before answering ("Think through this in <thinking> tags, then give the answer in <answer> tags"). Improves multi-step reasoning, math, and complex classification. Costs latency and tokens; parse only the answer. With reasoning models, built-in thinking replaces manual CoT — give goals, not step scripts.

Combining is common: few-shot examples that include the reasoning. Decide using evals, not intuition.

### How do you get reliable structured output (JSON) from an LLM?

In order of reliability:

1. **Native structured outputs / constrained decoding:** provide a JSON Schema and the API guarantees schema-valid output (OpenAI `response_format` with `json_schema` + `strict: true`; Anthropic structured outputs / strict tool use; Gemini `response_schema`; vLLM/Outlines guided decoding for self-hosted).
2. **Tool calling as a schema:** define a tool whose `input_schema` is your output (with `strict: true` where offered) and force it with `tool_choice` — note some newer models reject forced `tool_choice`, so check the docs (verify).
3. **Prompting + validation:** describe the schema, give an example, then **validate with Pydantic** and retry with the error message on failure. (Prefilling the assistant turn with `{` used to be a common trick; newer Claude models reject assistant prefill, so prefer native structured outputs — verify per model.)

```python
from pydantic import BaseModel, Field
from typing import Literal

class Invoice(BaseModel):
    vendor: str
    total: float = Field(description="Grand total incl. tax")
    currency: Literal["USD", "EUR", "GBP"]
    due_date: str | None = Field(description="ISO date, null if absent")

# OpenAI-style: client.responses.parse(model=..., input=..., text_format=Invoice)
# Anthropic: pass Invoice.model_json_schema() as a tool's input_schema or structured output schema

def extract(text, call_llm, retries=2):
    err = None
    for _ in range(retries + 1):
        raw = call_llm(text, error=err)
        try:
            return Invoice.model_validate_json(raw)
        except Exception as e:
            err = str(e)          # feed the validation error back
    raise ValueError(err)
```

Also: schema-valid isn't *correct* — still validate semantics (totals add up, dates plausible), allow nullable fields so the model doesn't invent values, keep schemas flat-ish, use enums, and put descriptions on fields. If the task needs reasoning, add a `reasoning` field *before* the answer fields.

### How do you make prompts robust to edge-case inputs?

- **Collect edge cases deliberately:** empty input, very long input, wrong language, typos, multiple intents, out-of-scope requests, adversarial/prompt-injection text, missing fields, ambiguous references, PII.
- **Tell the model what to do for each class:** "If the email contains no order number, return `order_id: null` — do not guess." Give an explicit escape hatch (`"unknown"`, `needs_human`).
- **Separate instructions from data** with XML tags so user content can't be confused with instructions.
- **Pre-process** in code: truncate, normalise encoding, detect language, reject obviously invalid input before calling the model.
- **Post-validate** outputs (schema, allowed values, business rules) and fall back gracefully.
- Add each failure you find to the **eval set** so it stays fixed. Generate more variants synthetically (ask an LLM to produce tricky inputs).
- Don't overfit with dozens of special-case rules; prefer explaining the underlying goal so the model generalises.

### How do you version and test prompts?

- **Prompts are code:** store in git (or a prompt registry like LangSmith/Braintrust/PromptLayer) as templates separate from application logic, with a version ID. Log the prompt version, model ID and parameters with every request so you can trace any output.
- **Pin model versions** (dated snapshots), since a model change is a prompt change.
- **Test suite:**
  - Unit-style checks: schema validity, required keywords, forbidden content, length.
  - **Eval set** (golden examples + edge cases) scored with exact match, programmatic checks, or LLM-as-judge.
  - Run on every PR in CI (promptfoo, Braintrust, custom pytest) and compare against the baseline; block merges on regressions beyond a threshold.
- **Rollout:** shadow traffic or canary/A-B the new version, monitor online metrics, keep instant rollback (feature flag on prompt version).
- Review diffs in prompts like code reviews, with the eval results attached.

### Here's a prompt that gives inconsistent output — fix it.

Example bad prompt: *"Summarize this ticket and tell me how urgent it is."*

Diagnosis process:
1. Run it 10–20 times on the same and different inputs; categorise the variation (format? judgement? length?).
2. Usually the cause is **underspecification**: urgency undefined, no format, no examples, no handling for missing info.

Fixed version:

```text
You triage support tickets for a SaaS company.

<ticket>{{ticket}}</ticket>

Classify urgency using these definitions:
- P1: service down or data loss for multiple users
- P2: a core feature broken for one user, no workaround
- P3: everything else (questions, minor bugs, requests)

Return only JSON:
{"summary": "<one sentence, max 25 words>",
 "urgency": "P1" | "P2" | "P3",
 "reason": "<which definition applied>"}
If the ticket is unclear, choose the lower urgency and say why in "reason".
```

Plus: lower temperature, use structured outputs, add 2–3 examples for borderline cases, put reasoning before the label if judgement is hard, and measure consistency (agreement rate across runs) before and after. If still inconsistent, the task definition may be genuinely ambiguous — go back to the customer for clearer criteria.

### How do you handle a prompt that works on one model but not another?

- **Expect it:** prompts are tuned to a model's training. Treat a model switch like a migration with evals, not a config change.
- **Run the eval set on both** and look at *which* cases fail and how (format, refusals, verbosity, instruction following, tool use).
- Common fixes:
  - Read the target provider's prompting guide (e.g. Claude responds well to XML tags and explicit instructions; newer models follow instructions more literally, so vague "be helpful" prompts may under-deliver).
  - Be more explicit; remove model-specific hacks ("IMPORTANT!!!" emphasis can cause over-reaction in newer models).
  - Adjust format control: structured outputs instead of prompt-only JSON.
  - Check system prompt vs. user message placement, and chat template differences (open models).
  - Re-tune few-shot examples and parameters (temperature).
- Architecturally: keep **per-model prompt variants** behind an abstraction, with the same eval suite across them.

### Prompt chaining vs. a single big prompt: trade-offs?

**Chaining** (split into steps: extract → classify → draft → check):
- Each step simpler → higher accuracy, easier to debug and eval individually.
- Can use different models per step (cheap for easy steps), insert code validation, parallelise independent steps.
- Costs: more calls, more latency, more orchestration code, error propagation between steps, lost context between steps.

**Single prompt:**
- Lower latency, simpler, model sees everything at once (good for tasks needing holistic judgement).
- Harder to debug, more likely to skip instructions when doing many things, harder to evaluate parts.

Rule: start single; split when evals show a specific sub-task failing, when you need validation/branching between steps, or when steps need different models/tools. Strong modern models handle more in one prompt than older ones, so re-evaluate chains periodically.

### How do you use XML tags / delimiters / role prompting?

- **XML tags / delimiters** separate kinds of content: `<instructions>`, `<document>`, `<examples>`, `<user_input>`. Benefits: the model clearly distinguishes instructions from data (helps against injection and confusion), you can refer to sections by name ("using the contract in `<contract>` tags"), and you can ask for tagged output (`<answer>`, `<quotes>`) that's easy to parse. Be consistent with tag names; nest for hierarchy. Claude is specifically trained to pay attention to XML structure; other models work fine with markdown headers or `"""` delimiters.
- **Multiple documents:** wrap each in `<document index="1"><source>…</source><content>…</content></document>`.
- **Role prompting:** set a role in the system prompt ("You are a senior tax accountant reviewing filings for errors"). It shifts vocabulary, depth and focus. Be specific (domain, audience, goal) — the role helps less than clear task instructions, but it's cheap and useful for tone and expertise.

### How would you reduce verbosity, or make the model refuse less / more appropriately?

**Verbosity:**
- State the target explicitly: "Answer in at most 3 sentences", "No preamble, no summary at the end."
- Say what *to do* rather than only what not to do ("Write in flowing prose paragraphs" vs. "don't use bullets").
- Show concise examples; match prompt style (a markdown-heavy prompt invites markdown output).
- Use structured output fields with length limits; set `max_tokens` as a backstop (not a style control — it truncates).
- Lower reasoning effort where available.

**Over-refusal:**
- Provide legitimate context: who the users are and why ("Users are licensed pharmacists; dosage questions are expected").
- Explain the boundaries precisely so the model doesn't over-generalise caution.
- Collect refused-but-legitimate examples into an eval set and track the false-refusal rate.

**Refuse more (appropriately):**
- Define scope ("Only answer questions about Acme products; politely decline others and suggest support@").
- Give a refusal template, add classifiers/guardrails on input and output for policy enforcement, not just the prompt.
- Measure both directions: false refusals and harmful completions.

### How do you write prompts for long documents (put docs first, quote extraction, etc.)?

- **Documents at the top, question and instructions at the end.** Anthropic's guidance reports noticeable quality gains from putting the query after long context.
- **Structure each document** with tags and metadata: `<document><source>contract_v3.pdf</source><content>…</content></document>`.
- **Quote extraction first:** "Find quotes from the documents relevant to the question and put them in `<quotes>` tags. Then answer using only those quotes in `<answer>` tags." This focuses attention, reduces hallucination, and gives verifiable citations.
- Ask for citations by document index; verify quotes programmatically.
- **Cache** the document prefix if multiple questions will be asked.
- If the document exceeds the window or quality drops: chunk + map-reduce (summarise/extract per section, then combine), or RAG.
- Pre-clean: strip boilerplate, headers/footers, repeated tables of contents — they waste tokens and distract.

### When does prompting stop being enough and you need fine-tuning or RAG?

- **Need RAG when the problem is knowledge:** the model lacks private, large, or frequently-changing information (internal docs, product catalog, tickets), or answers need citations. Signal: failures are "wrong/missing facts", not "wrong behaviour".
- **Need fine-tuning when the problem is behaviour at scale:**
  - A consistent format/style/domain-specific skill the model can't reliably reach with prompts + examples.
  - You need a smaller/cheaper/faster model to match a big model on a narrow, high-volume task (distillation).
  - The prompt has grown huge with examples and you pay for them on every call.
  - Specialised classification with thousands of labelled examples.
- **Prompting still enough** when: evals pass, volume is moderate, requirements change often (prompts iterate in minutes; fine-tunes take days and need data).

Order of escalation: better prompt → few-shot → better model / reasoning → RAG/tools → fine-tuning. Always justify each step with eval results, and remember fine-tuning doesn't reliably add facts — use RAG for knowledge.

---

## C. RAG (Retrieval-Augmented Generation)

### Explain RAG end to end.

RAG = retrieve relevant information at query time and put it in the prompt so the model answers from it.

**Offline (indexing) pipeline:**
1. **Ingest** from sources (SharePoint, Confluence, S3, DBs) with connectors; capture metadata (source, date, author, permissions/ACLs).
2. **Parse and clean** (PDF/HTML → text, tables, OCR).
3. **Chunk** into retrievable units, with metadata and optionally added context.
4. **Embed** chunks; also build a **keyword (BM25) index**.
5. **Store** in a vector DB / search engine with metadata filters.

**Online (query) pipeline:**
1. (Optional) **Query understanding:** rewrite, expand, classify, extract filters (dates, product).
2. **Retrieve** top-k (e.g. 50) with hybrid search + permission filters.
3. **Rerank** with a cross-encoder to top 5–10.
4. **Assemble the prompt:** instructions + retrieved chunks with IDs + question.
5. **Generate** a grounded answer with citations; refuse when context is insufficient.
6. **Post-process:** verify citations, guardrails, log everything.

**Around it:** evals (retrieval recall, faithfulness, answer correctness), freshness/sync jobs, monitoring, feedback collection. In an interview, emphasise that most RAG quality problems are **retrieval and data-quality problems**, not generation problems.

### Why RAG instead of fine-tuning? When is fine-tuning better?

**RAG is better for knowledge:**
- Data changes → just re-index; no retraining.
- Citations and traceability (users and auditors can verify).
- Permissions enforced at retrieval time per user — impossible once facts are baked into weights.
- Lower hallucination on facts; can delete data (GDPR) by removing it from the index.
- Cheaper and faster to iterate; works with any frontier model.

**Fine-tuning is better for behaviour:**
- Consistent output format, tone, or domain-specific style.
- Specialised tasks (classification, extraction in a niche domain) where examples beat instructions.
- Reducing latency/cost by moving to a smaller model (distillation).
- Teaching the model to use domain jargon or a custom tool/DSL.

They combine well: fine-tune for how to answer (and how to use retrieved context), RAG for what to answer with. Fine-tuning is unreliable for injecting facts — the model may still hallucinate details and can't cite them.

### How do you chunk documents? Why this chunking strategy? What chunk size and overlap?

Goal: each chunk is **self-contained enough to answer a question** and small enough to be specific in embedding space.

Strategies:
- **Fixed-size token windows with overlap** (e.g. 512 tokens, 10–20% overlap): simple baseline, but splits mid-thought.
- **Recursive / structure-aware:** split on headings → paragraphs → sentences; keep sections together. Best default for docs with structure (Markdown, HTML, manuals).
- **Document-type specific:** code by function/class (AST); tables kept whole with headers; emails/tickets as one chunk per message; FAQs one Q&A pair per chunk; legal by clause.
- **Semantic chunking:** split where embedding similarity between sentences drops.
- **Parent-child / small-to-big:** embed small chunks (precise matching), but return the larger parent section to the LLM (enough context).

Size guidance: **~200–800 tokens** is typical; 300–500 with ~50–100 token overlap is a reasonable starting point. Smaller = more precise retrieval but less context; larger = more context but diluted embeddings and more tokens. Prepend the document title/section path to each chunk.

How to justify in an interview: "I started with structure-aware 400-token chunks because the docs are sectioned manuals, then compared 256/512/1024 on retrieval recall@k on our eval set and picked the best." Chunking is a hyperparameter you **tune with evals**.

### How do you handle tables, PDFs, images, and code in RAG?

- **PDFs:** use a layout-aware parser (Unstructured, Azure Document Intelligence, AWS Textract, Google Document AI, LlamaParse, Docling, PyMuPDF for simple digital PDFs). Remove headers/footers, handle multi-column layouts, OCR scanned pages. Alternatively, send page images to a vision LLM to transcribe to Markdown. Check extraction quality manually on a sample — garbage in, garbage out.
- **Tables:** keep each table (or row-group) intact with column headers repeated; convert to Markdown/HTML; optionally generate a natural-language summary of the table to embed while storing the raw table for the LLM. For large numeric tables, put them in a database and let the model query with SQL (text-to-SQL tool) rather than embedding.
- **Images/charts/diagrams:** caption them with a vision model and embed the caption (linking back to the image), or use multimodal embeddings (e.g. CLIP-style, ColPali-style page-image retrieval) and pass the image itself to a multimodal LLM at answer time.
- **Code:** chunk by function/class using an AST parser (tree-sitter), include file path, signatures, docstrings and imports as context; combine with keyword search (identifiers are exact-match friendly) and code-specific embeddings. For repo-level tasks, agentic search (grep/read files) often beats pure vector search.

### Which embedding model would you choose and why?

Choose by evaluation on the customer's data, considering:
- **Quality on your domain and languages:** MTEB leaderboard is a starting point, not an answer; benchmark 2–4 candidates on recall@k with real queries.
- **Language coverage:** multilingual models (Cohere embed multilingual, BGE-M3, multilingual-e5, OpenAI text-embedding-3) if non-English.
- **Deployment constraints:** API (OpenAI text-embedding-3-small/large, Cohere Embed, Voyage — recommended by Anthropic, Google) vs. self-hosted open models (BGE, E5, GTE, Nomic, Jina) for data residency or cost at scale.
- **Dimensions and storage:** 1536–3072 dims × millions of chunks adds up; Matryoshka models allow truncation; quantised/binary embeddings reduce cost.
- **Max input length** (512 vs. 8k tokens) relative to chunk size.
- **Latency and price** for query-time embedding.
- **Domain-specific** options (code, legal, finance) or fine-tuning an open embedding model on in-domain query–passage pairs if off-the-shelf recall is poor.

Example answer: "For an English internal-docs assistant on AWS, I'd baseline with a strong API embedding model plus BM25, measure recall@20, and only consider fine-tuning an open model if recall on domain jargon is weak." Remember: switching models later requires re-embedding the whole corpus.

### Vector DB options (pgvector, Pinecone, Weaviate, Qdrant, Milvus, FAISS, Elasticsearch) — how do you choose?

- **pgvector (Postgres):** best default if the customer already runs Postgres and has < tens of millions of vectors. Transactions, joins with relational data, existing backups/security/RBAC. HNSW and IVFFlat indexes.
- **Pinecone:** fully managed, serverless, low ops, scales well; data leaves to a SaaS (check residency), cost at large scale.
- **Weaviate:** open-source or managed; built-in hybrid search, modules for vectorisation, multi-tenancy support.
- **Qdrant:** open-source (Rust), fast, strong payload filtering, quantisation, easy to self-host or managed.
- **Milvus / Zilliz:** built for billion-scale, distributed, GPU indexes; more operational complexity.
- **FAISS:** a library, not a database — great for in-memory prototypes, offline batch, or embedding inside a service; no persistence, filtering, or CRUD out of the box.
- **Elasticsearch / OpenSearch:** if the customer already uses it — mature BM25 + kNN vectors = hybrid in one system, plus security and ops tooling they know.
- Others worth naming: Azure AI Search, Vertex AI Vector Search, MongoDB Atlas Vector Search, LanceDB, Chroma (prototyping).

Decision criteria: existing stack (biggest factor for an FDE), scale (vectors, QPS), filtering and hybrid needs, multi-tenancy, deployment (VPC/on-prem), latency, cost, team ops skills. Usually: "use what they already run unless it can't meet requirements."

### ANN algorithms: HNSW vs. IVF vs. flat — trade-offs.

- **Flat (brute force):** compare the query with every vector. Exact (100% recall), O(N·d) per query. Fine up to ~100k–1M vectors or with GPUs; also the ground truth for measuring ANN recall.
- **HNSW (Hierarchical Navigable Small World):** multi-layer proximity graph; search greedily descends from sparse top layers. Very high recall and low latency, supports incremental inserts. Downsides: high memory (graph links + full vectors in RAM), slow builds, deletes are awkward. Tune `M`, `ef_construction`, `ef_search` (recall vs. latency). Default choice in most vector DBs.
- **IVF (inverted file):** k-means clusters; search only the `nprobe` nearest clusters. Less memory, faster to build, good for very large datasets; recall depends on `nprobe`; needs training and re-clustering if data drifts. Often combined with **PQ (product quantization)** — IVF-PQ compresses vectors heavily for billion scale at some recall cost.
- Others: DiskANN/ScaNN for disk-based or very large indexes.

Summary: small → flat; general purpose → HNSW; huge and memory-constrained → IVF-PQ or DiskANN. Always measure recall@k against flat on a sample.

### Dense vs. sparse (BM25) vs. hybrid retrieval. When does each win?

- **Dense (embeddings):** matches meaning, synonyms, paraphrases, cross-lingual. Wins on natural-language questions ("how do I get my money back" → refund policy). Weak on exact tokens: product codes, error IDs, names, rare jargon, numbers.
- **Sparse (BM25 / keyword, or learned sparse like SPLADE):** exact term matching weighted by rarity. Wins on identifiers, part numbers, legal citations, acronyms, code. No training, interpretable, cheap. Fails on vocabulary mismatch.
- **Hybrid:** run both and fuse — **Reciprocal Rank Fusion** (`score = Σ 1/(k + rank)`, k≈60) or weighted score normalisation. Almost always matches or beats either alone in enterprise search, which mixes both query types. My default, followed by reranking.

```python
def rrf(result_lists, k=60):
    scores = {}
    for results in result_lists:          # each: list of doc_ids in rank order
        for rank, doc in enumerate(results, 1):
            scores[doc] = scores.get(doc, 0) + 1 / (k + rank)
    return sorted(scores, key=scores.get, reverse=True)
```

### What is re-ranking (cross-encoders)? When is it worth it?

- Bi-encoders (embeddings) encode query and document **separately** — fast, precomputable, but coarse.
- A **cross-encoder** reranker reads the query and a candidate **together** and outputs a relevance score — much more accurate because it models interactions between query and document tokens, but too slow to run over the whole corpus.
- Pattern: retrieve top 50–200 cheaply (hybrid) → rerank → keep top 5–10 for the LLM.
- Options: Cohere Rerank, Voyage rerank, Jina, open models like BGE-reranker / mxbai, or an LLM as a listwise reranker (more expensive).

**Worth it when:** retrieval recall@50 is good but precision@5 is poor (the right chunk is retrieved but ranked low); you want to send fewer chunks to the LLM (cost, lost-in-the-middle); queries are complex. Adds ~50–300 ms latency and per-query cost. Measure nDCG/MRR with and without — it's usually one of the highest-ROI improvements in a RAG pipeline.

### Query rewriting, HyDE, multi-query retrieval — what and why?

User queries are often short, vague, conversational, or phrased differently from the documents.

- **Query rewriting:** an LLM rewrites the query into a better search query — e.g. turning a follow-up ("what about for enterprise plans?") into a standalone query using chat history; fixing typos; expanding acronyms; extracting metadata filters (date ranges, product).
- **HyDE (Hypothetical Document Embeddings):** ask the LLM to write a hypothetical answer, then embed *that* and search. The fake answer looks more like real documents than the question does, improving dense retrieval. Risk: if the model's hypothesis is wrong it can steer retrieval wrong; adds a generation call.
- **Multi-query:** generate 3–5 diverse reformulations (or decompose a complex question into sub-questions), retrieve for each, and fuse (RRF). Improves recall for ambiguous or multi-part questions.
- Also: **step-back prompting** (ask a more general question too), **routing** (choose which index/source to search).

Trade-off: each adds latency and cost; use a small fast model, run retrievals in parallel, and confirm with retrieval evals that recall actually improves.

### How do you handle permissions / access control in RAG?

Principle: **the user must never retrieve a chunk they couldn't open in the source system.** Enforce in retrieval, not in the prompt.

- **Ingest ACLs** with each document: allowed users/groups (from SharePoint, Google Drive, Confluence permissions), tenant ID, classification level. Store as metadata on every chunk.
- **Filter at query time:** resolve the authenticated user's identity and groups (from the IdP — Okta/Entra ID via SSO token, not from the prompt) and apply a metadata filter in the vector search (pre-filtering, so you still get k results).
- **Sync permissions** continuously — permission changes must propagate quickly (a revoked user shouldn't see the doc tomorrow). Some teams do a final live permission check against the source system for retrieved docs (late binding).
- **Don't** rely on the LLM to "not mention" restricted content — anything in the context window can leak.
- Watch other leak paths: caches keyed without user identity, conversation memory, logs, evals datasets, and summaries built across documents with different ACLs.
- Audit-log who retrieved what.

### How do you keep the index fresh when documents change?

- **Change detection:** use source webhooks/change feeds (Google Drive changes API, SharePoint delta queries, Confluence webhooks), CDC from databases (Debezium), or scheduled incremental crawls using `modified_at` timestamps.
- **Idempotent upserts keyed by document ID:** on change, delete all old chunks for that doc and insert new ones (chunk IDs like `doc_id#chunk_n`); use content hashes to skip unchanged docs/chunks and avoid paying to re-embed.
- **Deletes:** tombstones/hard deletes when source docs are deleted or permissions change — critical for compliance.
- **Queue-based pipeline** (SQS/Kafka → workers) for retries, backpressure and observability; track lag (time from source change to searchable).
- **Freshness metadata:** store `last_updated` and show it in answers; optionally boost recency in ranking.
- **Full rebuilds** periodically or when changing embedding model/chunking — build a new index in parallel and switch an alias (blue/green).
- Define an SLA with the customer: e.g. "changes searchable within 15 minutes."

### How do you cite sources and prevent the model from answering outside the context?

**Citations:**
- Give each chunk an ID and source metadata in the prompt: `<doc id="3" title="Refund Policy" url="...">`.
- Instruct: "Cite the doc id after every claim like [3]." Or use native citation features (e.g. Anthropic's citations API returns exact cited spans).
- **Verify** in code: cited IDs exist; for quote-based citations, check the quote appears in the source (fuzzy match). Render links in the UI.

**Staying in context:**
- System prompt: "Answer only using the documents provided. If they don't contain the answer, say 'I couldn't find this in the knowledge base' and suggest next steps. Do not use outside knowledge."
- Quote-first pattern: extract supporting quotes, then answer from them.
- Give an explicit "insufficient context" output option in structured outputs.
- Retrieval threshold: if top rerank scores are low, skip generation and return the fallback.
- **Groundedness check:** a judge model (or NLI model) verifies each claim is supported by the cited chunk; block or flag unsupported answers.
- Measure: faithfulness score and "correct abstention" rate on unanswerable questions in the eval set.

### Retrieval returns the wrong documents. How do you debug it?

Work systematically from data to ranking:

1. **Collect failing queries** and label the documents that *should* have been retrieved.
2. **Is the answer in the index at all?** Search the raw store for the text. Common culprits: parsing failed (scanned PDF, tables lost), doc never ingested, filtered out by wrong metadata/ACL, stale index.
3. **Inspect the chunks:** is the answer split across chunks, or buried in a huge chunk? Missing context (chunk says "it costs $5" with no product name)?
4. **Check the query side:** vague/conversational query? Needs rewriting? Acronyms/IDs dense search doesn't handle?
5. **Check ranking:** where does the right chunk rank — 15th (rerank/k problem) or not in the top 200 (recall problem)? Compare dense vs. BM25 results.
6. **Check embedding setup:** same model for query and docs, correct query/passage prefixes, normalisation, distance metric matches index.
7. **Check filters** (pre- vs. post-filtering dropping results) and ANN recall vs. flat search.

Fixes map to causes: better parsing, chunking or contextual retrieval; hybrid search; reranker; query rewriting; metadata filters; fine-tuned embeddings. Build a retrieval eval set (query → relevant doc IDs) so every fix is measured with recall@k/MRR rather than spot checks.

### What changes if the dataset becomes 100x larger?

Example: 1M chunks → 100M chunks.

- **Ingestion:** batch embedding with parallel workers and batch APIs; incremental updates only; embedding cost becomes a real budget item (consider cheaper/smaller-dimension embeddings, self-hosted embedding models).
- **Storage and index:** memory for HNSW explodes (100M × 1536 floats ≈ 600 GB raw) → vector quantisation (scalar/PQ/binary), reduced dimensions (Matryoshka), disk-based indexes (DiskANN), IVF-PQ; sharding and replicas; a distributed DB (Milvus, managed services, Elasticsearch/OpenSearch clusters) instead of a single pgvector node.
- **Retrieval quality:** more near-duplicates and distractors → precision drops. Add deduplication, stronger metadata filtering (partition by product/date/team), reranking, hierarchical retrieval (first pick documents/collections via summaries, then chunks), routing to sub-indexes.
- **Latency:** sharding fan-out, caching frequent queries, tune `ef_search`/`nprobe`.
- **Ops:** reindexing takes days → blue/green indexes, backfill pipelines, monitoring index lag, cost tracking.
- **Evals:** re-run the eval set at scale — recall often degrades in ways small tests don't show.

### How do you evaluate retrieval separately from generation (recall@k, MRR, nDCG)?

Build a labelled set of **(query → relevant chunk/doc IDs)** — from real user queries labelled by SMEs, or synthetic questions generated from chunks (then filtered by humans). Then evaluate the retriever alone:

- **Recall@k:** fraction of relevant docs found in the top k. Most important for RAG — if it's not retrieved, the LLM can't use it. Typical target: recall@10–20 > 0.9.
- **Precision@k:** fraction of top k that are relevant (affects noise in context).
- **MRR (Mean Reciprocal Rank):** average of 1/rank of the first relevant result — good when one right answer exists.
- **nDCG@k:** accounts for graded relevance and position (discounted by log of rank) — best for ranked lists with multiple relevance levels.
- **Hit rate@k:** did at least one relevant doc appear?

```python
import math
def recall_at_k(ret, rel, k): return len(set(ret[:k]) & rel) / len(rel)
def mrr(ret, rel): return next((1/i for i, d in enumerate(ret, 1) if d in rel), 0.0)
def ndcg_at_k(ret, rel, k):
    dcg = sum(1/math.log2(i+1) for i, d in enumerate(ret[:k], 1) if d in rel)
    idcg = sum(1/math.log2(i+1) for i in range(1, min(len(rel), k)+1))
    return dcg / idcg if idcg else 0.0
```

Then evaluate generation **given fixed retrieved context**: faithfulness/groundedness, answer relevance, correctness vs. reference. Separating them tells you which component to fix. Ragas-style metrics (context precision/recall, faithfulness) can automate parts with LLM judges.

### GraphRAG / knowledge graphs — when are they useful?

**GraphRAG** builds a graph of entities and relationships from documents (often LLM-extracted), and retrieves via the graph — following relationships or using community summaries (Microsoft's GraphRAG clusters the graph and pre-summarises communities).

Useful when:
- **Multi-hop questions:** "Which suppliers of our top-selling product had compliance issues last year?" — requires connecting facts across documents.
- **Global/sense-making questions:** "What are the main themes across all customer complaints?" — vector search over chunks can't answer "across everything"; community summaries can.
- Domains that are naturally relational: org charts, supply chains, fraud, biomedical, legal citations, IT asset dependencies.
- The customer already has a knowledge graph or structured master data.

Downsides: expensive and slow to build (LLM extraction over the whole corpus), entity resolution is hard, harder to keep fresh, and often overkill. Start with hybrid RAG + metadata; add graph or SQL tools only when evals show multi-hop or aggregate failures. Often a text-to-SQL tool over existing structured data beats building a graph.

### Long-context models vs. RAG — when would you just stuff the context?

**Stuff the context when:**
- The corpus fits comfortably (e.g. a few hundred pages, well under the window) — one contract, a codebase subset, a handbook.
- Questions need **whole-document reasoning** (summarise, compare sections, find contradictions) where chunk retrieval would miss pieces.
- Low query volume or a single session with many questions over the same docs (use **prompt caching** to make repeated queries cheap).
- Prototyping: fastest way to prove value before building a retrieval pipeline.

**Use RAG when:**
- Corpus is far larger than the window (most enterprise knowledge bases).
- Cost/latency matter at volume — paying for 500k input tokens per question is expensive and slow even with caching.
- You need **permissions**, freshness, and precise citations per user.
- Quality: long contexts still suffer distraction and lost-in-the-middle; focused context often beats more context.

Hybrid is common: retrieve whole relevant documents (not small chunks) and let a long-context model read them.

### Contextual retrieval (adding chunk context before embedding) — what problem does it solve?

**Problem:** chunks lose context when split out. A chunk saying "The company's revenue grew 3% over the previous quarter" doesn't say which company or quarter, so it won't match "ACME Q2 2023 revenue growth" well in either embeddings or BM25.

**Contextual retrieval (Anthropic, 2024):** for each chunk, use an LLM with the full document to generate a short (50–100 token) context explaining where the chunk fits ("This chunk is from ACME's Q2 2023 10-Q, discussing revenue..."), prepend it to the chunk, then build both the **embedding index and BM25 index** on the contextualised text.

- Anthropic reported retrieval failure rates dropping by ~35% with contextual embeddings, ~49% adding contextual BM25, and ~67% when also adding reranking.
- Cost is controlled with **prompt caching**: the full document is cached once and reused for each of its chunks.
- Cheaper alternatives for some corpora: prepend document title + section headings to each chunk.
- Trade-off: one-time LLM cost per chunk at ingestion and on every re-index; more tokens stored.

### Multi-tenant RAG: how do you isolate customers' data?

Options, from strongest to most efficient isolation:
1. **Separate deployments/databases per tenant** (silo): strongest isolation, per-tenant encryption keys and residency, easy deletion; highest cost and ops overhead. For regulated or large enterprise tenants.
2. **Separate index/collection/namespace per tenant** (Pinecone namespaces, Weaviate multi-tenancy, Qdrant collections/shard keys, separate Postgres schemas): good isolation and performance, simple deletion, moderate overhead.
3. **Shared index with a mandatory `tenant_id` filter** (pool): cheapest, scales to many small tenants; isolation depends entirely on code always applying the filter. Mitigate with Postgres **row-level security**, a single data-access layer that injects the tenant filter from the authenticated session (never from user input or the LLM), and tests that try cross-tenant access.

Also isolate everything else: caches (include tenant in cache keys), prompt-cache prefixes, conversation memory, logs and traces, fine-tuned models and eval data, background jobs. Use per-tenant encryption keys (KMS) where required, audit logs, and automated cross-tenant leak tests in CI. Many products use a tiered approach: pooled for small tenants, siloed for enterprise.

---

## D. Agents and tool use

### What is an AI agent? How is it different from a chain/workflow?

- **Workflow/chain:** the *developer* defines the control flow in code — fixed steps (classify → retrieve → draft → check), maybe with branches. The LLM fills in steps but doesn't decide what happens next. Predictable, testable, cheaper.
- **Agent:** an LLM runs in a **loop**, deciding which tools to call and when to stop, based on observations from the environment: `while not done: think → act (tool call) → observe result`. The model controls the flow, so it can handle open-ended tasks where steps can't be predicted (debug this failing build, research this account, resolve this support case end to end).

This distinction comes from Anthropic's "Building effective agents" framing. Agents trade predictability, cost and latency for flexibility. Key ingredients: a capable model, well-designed tools, a clear goal and stopping criteria, context/memory management, and guardrails. In practice most production systems are mostly workflows with agentic sub-steps.

### Explain function calling / tool use. How does the model decide which tool to call?

**Mechanics:**
1. You send the model a list of tools: name, description, JSON Schema for inputs.
2. The model, based on the conversation, can respond with a structured **tool call** (`tool_use` block / `tool_calls`) with a name and JSON arguments instead of (or alongside) text. The stop reason indicates a tool call.
3. **Your code** executes the function (the model never executes anything itself), then sends the result back as a `tool_result` message referencing the call ID.
4. The model continues — calling more tools or producing the final answer.

```python
while True:
    resp = client.messages.create(model=MODEL, max_tokens=2048,
                                  tools=TOOLS, messages=messages)
    messages.append({"role": "assistant", "content": resp.content})
    if resp.stop_reason != "tool_use":
        break
    results = [{"type": "tool_result", "tool_use_id": b.id,
                "content": run_tool(b.name, b.input)}
               for b in resp.content if b.type == "tool_use"]
    messages.append({"role": "user", "content": results})
```

**How it decides:** tool definitions are injected into the context (as a special system section); the model was fine-tuned on tool-use data to emit calls when a tool's description matches the need. So the decision relies on **names, descriptions, and the system prompt** — that's where you steer it. You can also control it with `tool_choice` (auto / any / a specific tool / none — some newer models accept only auto/none; verify), and models can make parallel tool calls.

### How do you design good tool definitions (names, descriptions, schemas)?

Treat tools as an API designed for a new teammate (the model) — "agent-computer interface" design.

- **Names:** clear, specific, namespaced verbs: `crm_search_accounts`, `crm_update_opportunity` — not `tool1` or `do_action`.
- **Descriptions (most important):** what it does, *when* to use it and when not to, what it returns, side effects, limits. Several sentences is fine.
- **Schemas:** typed parameters with descriptions, enums for fixed choices, required vs. optional, formats (ISO dates), examples. Avoid ambiguous params (`user` → `user_email`).
- **Right granularity:** design around tasks, not raw endpoints. One `schedule_meeting` tool beats `list_users` + `list_events` + `create_event` that the model must orchestrate. Don't expose 100 tools — group or load dynamically.
- **Token-efficient, meaningful outputs:** return relevant fields with human-readable identifiers, paginate/truncate large results, offer a `detail` level.
- **Helpful errors:** "No account found for 'Acme Corp'. Try `crm_search_accounts` with a partial name." — the model can self-correct.
- **Safety:** make destructive actions separate tools requiring confirmation; validate inputs server-side.
- **Test with evals:** check tool-selection accuracy and argument correctness; read transcripts to see where the model misuses tools, then improve descriptions.

```json
{
  "name": "crm_search_accounts",
  "description": "Search CRM accounts by name or domain. Use before any account update to get the account_id. Returns up to 10 matches with id, name, domain, owner.",
  "input_schema": {
    "type": "object",
    "properties": {
      "query": {"type": "string", "description": "Company name or web domain, partial match allowed"},
      "region": {"type": "string", "enum": ["NA", "EMEA", "APAC"], "description": "Optional region filter"}
    },
    "required": ["query"]
  }
}
```

### ReAct, plan-and-execute, reflection — explain each pattern.

- **ReAct (Reason + Act):** interleave reasoning and actions: *Thought → Action (tool call) → Observation → Thought → ...* until done. Adapts at each step to what it observes. Simple and robust; the default loop in most agents (native tool calling is essentially ReAct). Downsides: can be myopic, many sequential LLM calls, may wander.
- **Plan-and-execute:** a planner LLM first writes a multi-step plan; an executor (possibly a cheaper model or deterministic code) carries out each step; re-plan if a step fails or new information arrives. Better for long tasks, gives a reviewable plan (good for human approval), allows parallel steps, cheaper executors. Downsides: plans can be wrong upfront; needs re-planning logic.
- **Reflection (self-critique / Reflexion):** after producing an output or failing an attempt, the model (or a separate critic) evaluates it against criteria or test results and revises. Works best with **external feedback** — unit tests, validators, compiler errors — rather than pure self-judgement. Improves quality at the cost of extra calls; cap the iterations.

Related patterns: evaluator-optimizer, orchestrator-workers, routing, parallelisation (voting).

### When should you *not* use an agent?

- **The steps are known:** a fixed workflow (extract → validate → write to DB) is cheaper, faster, more predictable and easier to test. Use a chain.
- **Single-shot tasks:** classification, extraction, summarisation — one call does it.
- **Low error tolerance with irreversible actions** and no way to add human approval (payments, deleting production data).
- **Tight latency budgets** (voice, autocomplete) — agent loops add multiple round trips.
- **Cost-sensitive high volume** where each agent run costs 10–100x a single call.
- **No good tools or feedback signal:** agents need reliable tools and a way to check progress.
- **Can't evaluate it:** if you can't measure success, you can't ship it safely.

Rule: start with the simplest thing (single call → workflow), add agency only where flexibility measurably improves outcomes.

### How do you stop an agent from looping forever or taking dangerous actions?

**Looping / runaway:**
- Hard limits: max iterations/tool calls, max tokens/cost budget, wall-clock timeout per run and per tool.
- Detect repetition: same tool + same args N times → stop or inject a hint.
- Clear stopping criteria in the prompt and a `finish`/`submit` tool.
- Progress checks: periodic summarisation; escalate to a human if stuck.

**Dangerous actions:**
- **Least privilege:** scoped credentials per user/task, read-only by default, no raw shell/SQL in prod unless sandboxed.
- **Classify tools by risk:** read → auto; reversible writes → auto with logging; irreversible/external (send email, pay, delete, deploy) → **human approval**.
- Validation layer outside the model: allowlists, argument checks, rate limits, spend caps, dry-run modes.
- **Sandboxing:** containers/VMs with no network or restricted egress for code execution and computer use.
- Defend against prompt injection from tool outputs (treat retrieved content as data; don't let it trigger privileged actions without confirmation).
- Full audit logs/traces and a kill switch.

### Human-in-the-loop: where would you put approval steps?

Place approvals where **risk × irreversibility** is high, not everywhere (approval fatigue makes humans rubber-stamp).

- **Before irreversible or external side effects:** sending emails/messages to customers, payments/refunds, deleting data, production deploys, contract changes, anything legally binding.
- **At the plan stage** for long tasks: approve the plan once rather than each step.
- **Low-confidence outputs:** route to review when the model or a validator flags uncertainty, policy-sensitive topics, or high-value accounts.
- **Escalation paths:** the agent hands off to a human when stuck or the user asks.
- **Early rollout:** review 100% of actions at launch, then reduce to sampling as measured accuracy earns trust ("autonomy dial").

Design details: show the human a clear diff/preview of the proposed action and the evidence; make approve/edit/reject fast; log decisions and use rejections/edits as eval data. Implement as an interrupt that persists agent state (e.g. LangGraph interrupts, a pending-approval queue) so the run resumes after approval.

### How do you handle tool errors and retries inside an agent loop?

- **Classify errors:**
  - *Transient* (timeouts, 429, 5xx): retry in the tool wrapper with exponential backoff + jitter, idempotency keys for writes; don't burn LLM turns on them.
  - *Model-fixable* (invalid arguments, not found, validation failure): return a clear error message as the tool result (`is_error: true`) so the model can correct itself.
  - *Fatal* (auth failure, permission denied, service down): stop or escalate; don't let the model keep trying.
- **Validate arguments** against the schema before executing; return the validation error.
- **Limit retries** per tool and per run; detect repeated identical failing calls.
- **Idempotency and compensation:** for multi-step writes, design idempotent operations and rollback/compensating actions (saga pattern).
- **Timeouts** on every tool; return partial results where possible.
- **Observability:** log every call, argument, error and latency in a trace; alert on error-rate spikes per tool.
- Graceful degradation: if a non-critical tool is down, tell the model so it can proceed without it, and tell the user what couldn't be done.

### How do you manage memory (short-term, long-term, summarisation) for agents?

- **Short-term (working) memory:** the conversation/context window — messages, tool calls, results. Manage it actively:
  - Trim or clear old tool results (large outputs are the main bloat); keep a reference/ID instead.
  - **Summarise/compact** older turns when nearing a token threshold (many SDKs have built-in compaction/context editing).
  - Keep the system prompt and key facts pinned at the start (and cached).
- **Long-term memory:** information persisted across sessions:
  - Facts and preferences (user profile, "customer prefers email") in a database or key-value store.
  - Episodic memory (past interactions) in a vector store, retrieved by relevance.
  - Agent-managed memory files/notes: a memory tool the agent reads and writes (e.g. a `NOTES.md` or memory directory), useful for long tasks.
- **Scratchpad/state:** structured task state (plan, todo list, progress) stored outside the context and re-injected — critical for long-running agents to survive context resets.
- **Design concerns:** what to write (avoid storing noise or wrong facts), staleness and conflicts, privacy/PII and per-user isolation, user visibility and deletion rights, and eval of memory retrieval.

### Multi-agent systems: orchestrator–worker, handoffs. Trade-offs?

- **Orchestrator–worker:** a lead agent decomposes the task, spawns sub-agents (often in parallel) with focused instructions and their own context windows, then synthesises results. Great for breadth-first work like research across many sources. Anthropic reported its multi-agent research system outperformed a single agent substantially on research evals — but used roughly 15x the tokens of a chat interaction.
- **Handoffs (routing/triage):** a front agent identifies the intent and transfers the conversation to a specialist agent (billing, tech support) with its own prompt and tools (OpenAI Agents SDK handoffs). Keeps each agent's prompt and toolset small.
- **Other:** pipeline (sequential specialists), debate/critic, hierarchical teams.

**Pros:** separation of concerns, smaller focused contexts, parallelism, specialised tools/models per role, scales beyond one context window.
**Cons:** much higher token cost and latency, coordination failures (duplicated work, lost context between agents, contradictory results), harder debugging and evaluation, more nondeterminism.

Use multi-agent only when the task is parallelisable or exceeds one agent's context; otherwise a single agent with good tools is usually better. Invest in clear task specs for sub-agents and tracing across agents.

### How do you evaluate an agent (task success, trajectory eval, tool-call accuracy)?

Evaluate at several levels:

- **Final outcome / task success:** did it achieve the goal? Best checked against **environment state** (the ticket is closed with correct fields, the test suite passes, DB row correct) rather than the agent's claims. Report success rate over many runs (pass@1, and pass^k for consistency since agents are nondeterministic).
- **Trajectory evaluation:** were the steps reasonable? Correct tool order, no unnecessary or forbidden actions, policy followed (e.g. verified identity before refund). Compare to reference trajectories loosely (multiple valid paths) or use an LLM judge with a rubric.
- **Tool-call accuracy (unit level):** given a state, did it choose the right tool with correct arguments? Measure tool selection accuracy and argument exact-match/validity. Cheap and fast to iterate on.
- **Efficiency:** steps, tokens, cost, latency per task; loops and errors.
- **Safety:** rate of attempted dangerous actions, prompt-injection robustness.

Infrastructure: a **sandboxed, resettable environment** with mocked or staging APIs, simulated users (an LLM playing the customer, as in τ-bench), deterministic fixtures, and saved traces for human review. Read transcripts regularly — most insights come from failure analysis.

### How do you make agents deterministic enough for enterprise use?

You can't make LLMs fully deterministic, so constrain where variability matters:

- **Reduce agency:** encode known business processes as workflows/state machines (e.g. LangGraph, explicit code), and use the LLM only for fuzzy steps (understanding, drafting, choosing among a few options).
- **Constrain outputs:** structured outputs, enums, strict tool schemas, `tool_choice` to force specific steps where the model supports it (otherwise make that step a plain workflow call).
- **Deterministic guards:** validation and business rules in code; policy checks before actions; approvals for high-risk steps.
- **Low temperature, pinned model snapshots**, versioned prompts and tools.
- **Idempotent, transactional tools** so retries are safe.
- **Consistency evals:** run each test case multiple times; track pass^k and variance; fix prompts where outputs flip.
- **Observability and audit:** full traces, reproducible inputs, explanations for each action.
- **Fallbacks:** route to a human when confidence is low or validations fail.

Message to the customer: aim for *predictable outcomes and bounded behaviour*, not identical text.

### Computer use / browser agents — risks and use cases.

The model sees screenshots (or DOM/accessibility tree) and outputs mouse/keyboard actions — Anthropic computer use, OpenAI's computer-using agent / Operator-style products, browser automation agents (Playwright-based).

**Use cases:** automating legacy apps with no API (old ERPs, government portals, mainframe UIs), form filling and data entry across systems, QA/UI testing, web research requiring navigation, RPA replacement that tolerates UI changes.

**Risks:**
- **Prompt injection from web pages/documents** — the biggest one; a page can instruct the agent to exfiltrate data or take actions.
- Unintended actions (purchases, deletions, sending messages), especially with logged-in sessions.
- Credential and data exposure; screenshots containing PII in logs.
- Slow, expensive, and less reliable than APIs; brittle on complex UIs, CAPTCHAs, pop-ups.
- Terms of service of target sites.

**Mitigations:** run in an isolated VM/container with a dedicated browser profile and least-privilege accounts; domain allowlists; no access to sensitive credentials; human confirmation for consequential actions; injection classifiers; session recording for audit; prefer APIs/MCP tools whenever available and use computer use as the fallback.

### What's MCP (Model Context Protocol)? What problem does it solve? Tools vs. resources vs. prompts.

MCP is an open protocol (introduced by Anthropic in late 2024, since donated to the Linux Foundation's Agentic AI Foundation — verify; widely adopted across OpenAI, Google, Microsoft and many tools) that standardises how AI applications connect to external tools and data.

**Problem:** the M×N integration problem — every AI app (Claude, ChatGPT, IDEs, custom agents) needed custom integrations with every system (GitHub, Slack, Postgres, internal APIs). MCP makes it M+N: build one MCP server for a system, and any MCP client can use it. "USB-C for AI apps."

**Architecture:** host app → MCP client → MCP server. JSON-RPC 2.0 messages over **stdio** (local servers) or **Streamable HTTP** (remote servers). Capability negotiation at initialisation.

**Server primitives:**
- **Tools** — *model-controlled* functions the LLM can call (with JSON Schema inputs), e.g. `create_issue`, `run_query`. Can have side effects.
- **Resources** — *application-controlled* read-only data identified by URIs (files, DB schemas, documents) that the host can attach to context.
- **Prompts** — *user-controlled* reusable templates/workflows (often surfaced as slash commands), e.g. `/summarize-incident`.

Client-side features include **sampling** (server asks the client's LLM for a completion), **roots**, and **elicitation** (server requests input from the user). Authorization for remote servers is based on OAuth 2.1.

### How would you build an MCP server for a customer's internal system? How do you secure it?

**Build:**
1. **Scope with the customer:** which user workflows matter (e.g. "look up an order, check shipment, issue a return"). Design tools around those tasks, not a 1:1 mirror of 200 REST endpoints.
2. Use an official SDK (Python `mcp` with FastMCP, or TypeScript). Wrap the internal API with typed tools, good descriptions, concise outputs, pagination, helpful errors.
3. Expose reference data as **resources** (schemas, policy docs) and common workflows as **prompts**.
4. Transport: **stdio** for local developer use; **Streamable HTTP** for a shared remote server deployed in the customer's VPC.
5. Test with the MCP Inspector and an eval set of tasks; version the server.

```python
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("orders")

@mcp.tool()
def get_order(order_id: str) -> dict:
    """Get an order's status, items and shipment tracking by order ID (e.g. 'ORD-1234')."""
    return orders_api.get(order_id, fields=["status", "items", "tracking"])

if __name__ == "__main__":
    mcp.run()  # stdio by default; use streamable HTTP transport for remote deployment
```

**Secure:**
- **AuthN/AuthZ:** OAuth 2.1 with the customer's IdP (Okta/Entra); act **on behalf of the end user** with their permissions (token exchange), not a god-mode service account. Validate token audience; never pass tokens through to downstream APIs unvalidated.
- **Least privilege:** read-only tools by default; separate write tools; scopes per tool; human confirmation for destructive actions.
- **Input validation** on every argument; parameterised queries; no arbitrary SQL/shell.
- **Output hygiene:** strip secrets/PII not needed; treat data returned to the model as potentially carrying prompt injection.
- **Network:** deploy inside the VPC, TLS, private endpoints, rate limiting, origin checks (DNS-rebinding protection for local HTTP servers).
- **Audit logging** of who called what with which arguments; monitoring and alerts.
- **Supply chain:** pin dependencies; only allow vetted MCP servers in clients (allowlist), since third-party servers can be malicious or "rug-pull" tool descriptions.

### OpenAI Assistants/Responses API, Agents SDK, Claude Agent SDK — what do they give you?

- **OpenAI Assistants API:** earlier stateful API (threads, runs, hosted file search and code interpreter). **Deprecated in favour of the Responses API**, with shutdown scheduled for August 2026 (verify whether it is already gone) — mention it as legacy.
- **OpenAI Responses API:** the current primitive. Single endpoint combining chat with **built-in tools** (web search, file search over vector stores, code interpreter, computer use, image generation, remote MCP), server-side conversation state (`previous_response_id` / conversations), reasoning-item handling, structured outputs, streaming events.
- **OpenAI Agents SDK** (Python/TS): lightweight framework on top — `Agent` (instructions + tools + model), runner loop, **handoffs** between agents, **guardrails** (input/output validation running in parallel), sessions for memory, built-in **tracing**, MCP support.
- **Claude Agent SDK** (formerly Claude Code SDK; Python/TS): the agent harness that powers Claude Code, as a library. Gives the agent loop, built-in tools (file read/write/edit, bash, grep/glob, web search/fetch), automatic **context compaction**, **subagents**, **hooks** (run code before/after tool use for validation, logging, blocking), fine-grained **permissions**, MCP integration, skills, and session management. Good for building agents that work on files, code, or computers.
- **Anthropic Messages API** itself provides tool use, server tools (web search, code execution), structured outputs, prompt caching, citations, and MCP connector.

What they give you overall: you skip writing loop/state/tracing plumbing and get batteries-included tools. Trade-offs: vendor lock-in, less control over internals, fast-changing APIs. As an FDE: use them to move fast, but keep business logic and evals provider-agnostic.

### Frameworks (LangChain, LangGraph, LlamaIndex, CrewAI, DSPy) — when would you use or avoid them?

- **LangChain:** large library of integrations (loaders, vector stores, model wrappers). Use for quick prototypes and connectors. Avoid deep abstraction stacks in production — they can hide prompts, complicate debugging, and churn between versions.
- **LangGraph:** low-level graph/state-machine framework for agents: explicit nodes/edges, persisted state (checkpointing), human-in-the-loop interrupts, streaming, durable execution. Good fit for enterprise agents needing controllable flows. Learning curve; can be overkill for simple loops.
- **LlamaIndex:** data/RAG-focused — document parsing (LlamaParse), indexing, retrievers, query engines, plus agent workflows. Good for building RAG quickly over messy data.
- **CrewAI:** role-based multi-agent orchestration ("crews" of agents with roles and tasks). Quick for multi-agent demos; less control and harder to make reliable in production.
- **DSPy:** "programming, not prompting" — declare modules with signatures, then **optimise** prompts/few-shot examples automatically against a metric and dataset. Great when you have a good eval set and a pipeline to tune; requires that investment and is less transparent.

**When to avoid:** simple apps where ~100 lines of direct SDK calls are clearer; when abstractions block access to new provider features (caching, structured outputs, thinking); when the customer's team can't maintain the dependency. Many strong teams (and Anthropic's guidance) start with direct API calls and add a framework only for a specific need (state persistence, tracing, integrations). Whatever you choose, make sure you can see the exact prompts sent.

---

## E. Evaluation (most important)

### How do you know your AI system is actually working?

This is the differentiator question. Answer with a system, not a feeling:

1. **Define success with the customer in business terms first:** e.g. "resolve 40% of tier-1 tickets without escalation, with < 2% incorrect policy statements", or "extraction field accuracy ≥ 98% vs. manual entry."
2. **Translate into measurable criteria:** correctness, faithfulness, format validity, safety, latency, cost — each with a target.
3. **Offline eval set:** representative real inputs + edge cases + adversarial cases, with reference answers or rubrics. Scored by code checks where possible, calibrated LLM judges where necessary, and SME review for a sample.
4. **Component evals:** retrieval recall separately from generation; tool-call accuracy separately from task success — so you know *what* is broken.
5. **Regression gating in CI:** every prompt/model/retrieval change runs the suite before shipping.
6. **Online monitoring:** user feedback (thumbs, edits, escalations), task outcome metrics (resolution rate, handle time, acceptance of suggestions), automated judges on sampled traffic, latency/cost/error dashboards, drift in input distribution.
7. **Human review loop:** regularly read transcripts; failures become new eval cases.
8. **Compare to baseline:** the current human process or old system — "better than what" matters.

One-liner: "I know it works when it beats an agreed baseline on an eval set that reflects production, and online metrics confirm it — and I keep checking because inputs and models drift."

### How do you build an eval set when the customer has no labelled data?

- **Mine real inputs:** historical tickets, emails, search logs, documents, chat transcripts. Even unlabelled, they give you the true distribution. Sample across segments (categories, languages, lengths).
- **SME labelling sessions:** get 1–2 domain experts for a few hours to label 50–200 examples or write reference answers. Run a short calibration first; measure inter-annotator agreement to check the task is well defined. Often reveals the customer's real success criteria.
- **Model-assisted labelling:** have a strong model pre-label; SMEs correct (much faster than labelling from scratch). Beware anchoring bias.
- **Synthetic data:** generate questions from documents (good for RAG: question → source chunk gives a free relevance label), generate variations and edge cases, adversarial inputs. Filter and spot-check by humans; keep synthetic and real sets separate and tag them.
- **Use existing outcomes as labels:** the category a ticket was eventually routed to, the final approved contract clause, the resolution code.
- **Rubrics instead of references** for open-ended tasks: criteria an LLM judge or human can score ("mentions refund window, doesn't promise exceptions").
- Start small (30–50 high-quality cases) on day one, grow continuously from production failures.

### Offline evals vs. online evals vs. A/B tests.

- **Offline evals:** run the system on a fixed dataset before deployment. Fast, cheap, repeatable, safe; ideal for iteration and regression gating. Limitation: only as good as the dataset's coverage of real traffic; can be overfit.
- **Online evals (monitoring):** score live production traffic — automated judges on samples, guardrail trigger rates, user feedback, implicit signals (copy/accept rate, rephrasing, escalation, abandonment), latency, cost. Catches drift and real-world failures; signals are noisier and delayed; privacy constraints on reviewing data.
- **A/B tests:** randomly split users between variants and compare **business metrics** (resolution rate, conversion, CSAT, retention) with statistical significance. The only way to prove causal impact on outcomes; needs enough traffic and time, and guardrail metrics to stop a harmful variant. Variants: shadow mode (run new version silently, compare outputs), canary releases, interleaving for ranking.

Flow: offline evals to choose candidates → shadow/canary → A/B test for the decision → continuous online monitoring feeding new cases back into offline sets.

### LLM-as-a-judge: how do you make it reliable? How do you validate the judge?

**Make it reliable:**
- Use a **specific rubric** with concrete criteria and defined scale anchors; prefer **binary or pass/fail per criterion** over 1–10 scores.
- One criterion per judge call (faithfulness separately from tone).
- Provide the inputs it needs: source context, reference answer, the rubric.
- Ask for **reasoning before the verdict**, and structured output (`{"reasoning": ..., "verdict": "pass"}`).
- Use a strong model, temperature 0, pinned version; include few-shot examples of borderline cases.
- Mitigate known biases: **position bias** in pairwise comparisons (swap order and require agreement), **verbosity bias** (instruct to ignore length), **self-preference** (use a different model family to judge than the one generating where feasible).

**Validate the judge:**
1. Have humans (SMEs) label a set of 100–200 outputs with the same rubric.
2. Run the judge on the same set; measure agreement — accuracy, precision/recall on "fail", Cohen's kappa — and compare to human–human agreement.
3. Inspect disagreements; refine the rubric/prompt; repeat until agreement is acceptable (e.g. kappa > 0.6–0.8 depending on stakes).
4. Keep a held-out set to avoid overfitting the judge prompt, and **re-validate** when you change the judge model or prompt. Periodically spot-check judge verdicts in production.

```text
You are grading whether an answer is fully supported by the provided context.
<context>{{context}}</context>
<answer>{{answer}}</answer>
List each factual claim in the answer and whether the context supports it.
Then output JSON: {"unsupported_claims": [...], "verdict": "pass" | "fail"}
"pass" only if every claim is supported. Ignore style and length.
```

### What metrics would you use for: classification, extraction, summarisation, Q&A, agents?

- **Classification:** accuracy, per-class precision/recall/F1, macro-F1 for imbalanced classes, confusion matrix; cost-weighted errors (a missed fraud case ≠ a false alarm); calibration if using confidence scores.
- **Extraction:** field-level precision/recall/F1 and exact match (after normalising dates, numbers, casing); document-level "all fields correct" rate; schema validity rate; null handling (correctly returns null when absent vs. hallucinated value).
- **Summarisation:** faithfulness/hallucination rate (claims supported by source), coverage of key points (checklist of required facts from SMEs), conciseness/length compliance, judged by rubric-based LLM judge + human review. ROUGE/BERTScore are weak proxies — mention them but don't rely on them.
- **Q&A / RAG:** answer correctness vs. reference (judge or exact/F1 for short answers), faithfulness/groundedness, citation accuracy, answer relevance, correct abstention on unanswerable questions; retrieval recall@k, MRR, nDCG.
- **Agents:** task success rate (verified by end state), pass^k consistency, trajectory/policy compliance, tool-call accuracy, steps/cost/latency per task, safety violations, escalation rate.
- **Always add:** latency (p50/p95), cost per request/task, safety/guardrail violation rate, and the business KPI the system is meant to move.

### How do you measure hallucination / faithfulness / groundedness?

Definitions: **faithfulness/groundedness** = every claim in the output is supported by the provided source context; **factual correctness** = true in the world (harder, needs references).

Methods:
- **Claim decomposition + verification:** split the answer into atomic claims, check each against the context with an LLM judge or NLI model; score = supported claims / total claims (this is how Ragas faithfulness and similar metrics work). Track the rate of answers with *any* unsupported claim.
- **Citation checks:** verify that cited passages exist and actually support the sentence; check quotes appear verbatim in sources.
- **Reference-based:** compare against SME-written reference answers for correctness.
- **Unanswerable test cases:** questions whose answers aren't in the corpus — measure how often the model correctly abstains vs. invents.
- **Self-consistency signals:** sample multiple answers; disagreement suggests hallucination (SelfCheckGPT-style). Useful online without references.
- **Specialised detectors:** grounding check APIs/models from cloud providers, HHEM-style classifiers.
- **Human review** of a sample to validate the automated metric.

Report it as a rate on a fixed eval set (and on sampled production traffic) so you can track it over time and by category.

### How do you catch regressions when you change the prompt or model?

- **Versioned eval suite** run automatically in CI on every change to prompts, models, retrieval config, tools or code (promptfoo, Braintrust, LangSmith, or pytest).
- **Compare against baseline**, not just absolute thresholds: per-metric deltas and **per-example diffs** (which cases flipped pass→fail). Aggregate scores can hide new failures offset by new fixes.
- **Slice metrics** by category/language/customer segment — regressions often hide in one slice.
- **Multiple runs per case** to separate real regressions from sampling noise; use confidence intervals or significance tests for small differences.
- **Include must-pass cases** (critical safety and business rules) that block deploy if any fail.
- **Model upgrades:** pin snapshots; when a provider releases a new version, run the full suite, compare cost/latency too, then canary/shadow before switching.
- **Production safety net:** canary release, online metrics and judge scores on sampled traffic, alerts on shifts, one-click rollback via prompt/model version flags.

```python
def run_suite(cases, system, baseline):
    report = []
    for c in cases:
        out = system(c["input"])
        passed = all(check(out, c) for check in c["checks"])
        report.append({"id": c["id"], "passed": passed,
                       "was": baseline.get(c["id"]), "out": out})
    regressions = [r for r in report if r["was"] and not r["passed"]]
    rate = sum(r["passed"] for r in report) / len(report)
    return rate, regressions   # fail CI if regressions hit must-pass cases or rate drops
```

### How do you balance quality vs. latency vs. cost in evals?

- **Measure all three in the same eval run:** quality metrics plus p50/p95 latency (time to first token and total), and cost per request/task (tokens × price, including retries and tool calls).
- **Set constraints with the customer:** e.g. "p95 < 3 s for chat; cost < $0.02 per ticket; quality ≥ baseline." Then optimise quality within the constraints, or cost subject to a quality floor.
- **Plot the Pareto frontier:** run model × prompt × config variants (model size, reasoning effort, number of retrieved chunks, reranker on/off) and pick from the frontier.
- **Levers:** smaller models or routing (easy → small, hard → large), prompt caching, fewer/shorter retrieved chunks, shorter outputs, batch APIs for non-real-time work, parallel calls, streaming for perceived latency, distillation for high-volume narrow tasks.
- **Value errors in business terms:** a 1% quality drop may cost more than the savings (e.g. wrong medical info) or be irrelevant (internal tagging). Make the trade-off explicit and documented.
- Re-evaluate periodically — model prices fall and new models shift the frontier.

### Golden datasets: how big, how to build, how to maintain?

**How big:**
- Start with **30–100** carefully chosen cases to iterate quickly; this catches big problems.
- Grow to **200–1,000+** for decisions requiring statistical confidence: detecting a 5-point change in pass rate needs a few hundred examples (standard error ≈ √(p(1-p)/n) — at n=400, p=0.8, SE ≈ 2%, so the difference between two independent runs has SE ≈ 2.8% and 5 points is only borderline significant; paired comparisons on the same cases help).
- Coverage matters more than raw size: every important category, language, edge case, and known failure mode should be represented.

**How to build:**
- Sample real production or historical inputs stratified by segment; add edge cases and adversarial inputs; include negative/unanswerable cases.
- SME-written references or rubrics; record labelling guidelines; double-label a subset to measure agreement.
- Store with metadata (category, difficulty, source, date, rationale) in version control or an eval platform.

**How to maintain:**
- Version it like code; changes reviewed.
- Add production failures and user-reported issues continuously (with PII scrubbed).
- Retire or fix stale cases when policies/products change; periodically re-audit labels.
- Keep a **held-out set** not used during prompt iteration to detect overfitting.
- Watch for saturation: if everything passes, add harder cases.

### How do you collect user feedback and turn it into evals?

**Collect:**
- **Explicit:** thumbs up/down with optional reason categories ("wrong", "incomplete", "outdated", "tone"), free-text comments, ratings. Keep it one click.
- **Implicit (often more valuable):** user edits to drafts (diff = correction), copy/accept rate, regenerate clicks, rephrased follow-up questions, escalation to human, task abandonment, downstream outcomes (ticket reopened, refund reversed).
- **Expert review:** SMEs review a sampled queue weekly.
- Log everything with trace IDs linking feedback to the exact prompt version, model, retrieved context and output.

**Turn into evals:**
1. Triage negative feedback; cluster into failure categories (retrieval miss, hallucination, format, policy, missing knowledge).
2. For each real issue, create a test case: input + context + expected behaviour (reference answer, rubric, or assertion), reviewed by an SME. Scrub PII.
3. Add to the golden set tagged with category; track category counts to prioritise fixes.
4. Fix, confirm the case passes, and keep it as a regression test.
5. Use accepted edits as preference/fine-tuning data later.

Watch for biases: feedback comes mostly from unhappy users, and thumbs-up is often given to confident-but-wrong answers — validate with expert review.

### Tools: promptfoo, Ragas, DeepEval, LangSmith, Braintrust, OpenAI Evals — have you used any?

Answer honestly with what you've used; know what each is for:

- **promptfoo:** open-source CLI, YAML-configured test cases and assertions across prompts/providers, side-by-side comparison, CI-friendly; strong red-teaming features. Great for prompt regression testing.
- **Ragas:** open-source library of RAG metrics — faithfulness, answer relevancy, context precision/recall — plus synthetic test set generation.
- **DeepEval:** open-source, pytest-style LLM testing with many built-in metrics (G-Eval, hallucination, RAG metrics, agent metrics); integrates with CI.
- **LangSmith:** LangChain's platform for tracing, datasets, evaluators, annotation queues, prompt management; works without LangChain too.
- **Braintrust:** eval and observability platform — datasets, scorers, experiment comparison with per-example diffs, prompt playground, production logging.
- **OpenAI Evals:** OpenAI's open-source framework and the hosted Evals in the platform (datasets, graders including model graders, runs).
- Also worth knowing: Arize Phoenix, Langfuse (open-source tracing + evals), Weights & Biases Weave, Inspect AI (UK AISI), Anthropic's Console evaluation tool.

Good interview framing: "The tool matters less than the discipline. For a quick start I'd use promptfoo or a pytest harness in CI; for a team needing traces, datasets and human review I'd use Braintrust/LangSmith/Langfuse. I always make sure scorers are validated against human labels."

Example (adapt to your own experience): "On a support-bot project I used promptfoo with ~150 cases in CI, a custom faithfulness judge validated against 100 SME labels at about 90% agreement, and Langfuse for production traces."

### The customer says "it feels worse this week." How do you investigate?

1. **Get specifics:** examples (conversation IDs, screenshots), which users/teams, when it started, what "worse" means (wrong, slow, verbose, refusing, missing info).
2. **Check what changed** in the time window:
   - Our side: prompt, code, retrieval config, model version/alias (did an unpinned alias move?), tool definitions, deploys, feature flags.
   - Provider side: model update, incident, latency degradation, rate limiting causing fallbacks to a weaker model.
   - Data side: index ingestion failures, stale or newly added documents (duplicates/conflicting policies), permission changes, upstream API changes.
   - Usage side: new user group, new use case, product launch, seasonal question mix.
3. **Look at data, not anecdotes:** dashboards for online judge scores, feedback rate, escalation rate, latency, error and fallback rates, token counts, retrieval scores — compare this week vs. previous weeks, sliced by segment.
4. **Reproduce:** replay the reported examples and a sample of this week's traffic through last week's and this week's configuration; run the golden eval set on current production config.
5. **Root cause and fix:** roll back if a change caused it; fix ingestion; adjust the prompt; add the failing cases to the eval set.
6. **Communicate:** acknowledge quickly, share findings with evidence, the fix, and the prevention (new regression tests, alerts, pinned versions). Sometimes the answer is "quality is unchanged, but the question mix shifted" — show data and address the new use case.
