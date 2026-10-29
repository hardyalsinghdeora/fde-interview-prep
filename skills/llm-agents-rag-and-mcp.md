# LLM Agents, RAG & MCP

This is where interviewers go deepest for anyone claiming production LLM experience: they want to hear how tool loops are bounded, how retrieval is made reliable, and how things fail. Agentic-AI Architect and Forward Deployed Engineer roles ask most, with Staff/Principal and Software Architect roles probing the system design and cost side.

> Related: [AI / LLM](../topics/06-ai-llm/README.md) · [System design](../topics/03-system-design/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [Roles](../roles/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Walk through a tool/function-calling round trip: tool schema, model response, execution, result message, next turn.
- [ ] What makes a good tool definition (name, description, parameter schema, error messages)?
- [ ] Workflow vs. agent: when should the code decide the steps, and when should the model?
- [ ] What is MCP, and what are its main primitives (tools, resources, prompts) on the server side? *(verify)*
- [ ] MCP transports: stdio vs. streamable HTTP — when do you use each? *(verify)*
- [ ] RAG in one minute: ingestion, chunking, embedding, retrieval, re-ranking, generation with citations.
- [ ] Why hybrid retrieval (keyword + vector) beats either alone for enterprise documents.
- [ ] Structured output: JSON mode vs. schema-constrained output vs. tool-call arguments — guarantees of each *(verify)*.

## B. Working depth
- [ ] How do you bound an agent loop (max iterations, token budget, wall-clock, repeated-call detection)?
- [ ] How do you handle tool errors so the model can recover instead of looping or making things up?
- [ ] Parallel tool calls — when are they safe, and how do you merge results?
- [ ] How do you design an MCP server: tool granularity, auth, pagination, and returning large results?
- [ ] How does authorisation work for a remote MCP server (OAuth, per-user tokens, scopes)? *(verify)*
- [ ] Chunking strategies (fixed, structural, semantic, parent–child) and how to choose with evals.
- [ ] Query rewriting, multi-query and HyDE — when do they help and what do they cost?
- [ ] How do you make the model cite sources and refuse when retrieval finds nothing relevant?
- [ ] What goes in context vs. memory: conversation summarisation, scratchpads, long-term stores.
- [ ] Prompt caching — how do you structure prompts and tools to get cache hits? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Prompt injection through retrieved documents or tool results — how do you limit the blast radius?
- [ ] Guardrails in layers: input checks, tool allow-lists, argument validation, output checks, human approval for side effects.
- [ ] Which agent actions need human approval, and how do you design the approval UX so people do not rubber-stamp?
- [ ] Multi-agent patterns (orchestrator–workers, planner–executor, critic/reviewer) — when do they beat one agent with more tools?
- [ ] Common agent failure modes: tool-choice errors, loops, premature stopping, context overflow, hallucinated arguments — how do you detect each?
- [ ] Cost control: model routing (small vs. large), caching, context trimming, batch APIs, per-tenant budgets.
- [ ] Latency budget for an interactive agent: where the time goes and how streaming changes perceived latency.
- [ ] Exposing internal APIs over MCP to many clients: versioning, rate limits, audit logs, tenant isolation.
- [ ] How do you make agent side effects idempotent and safe to retry (idempotency keys, dry-run, compensation)?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design an agent that handles billing disputes end to end: tools, data access, approvals, audit trail.
- [ ] Design an MCP server that exposes a company's order and invoice systems to an AI assistant safely.
- [ ] An agent in production occasionally issues duplicate refunds. Find the cause and fix it.
- [ ] Your RAG assistant answers confidently from an outdated policy document. Diagnose and redesign.
- [ ] Design a multi-agent research workflow with a hard cost ceiling per request and a human checkpoint.

## E. Hands-on checks (things to be able to DO)
- [ ] Write a bounded tool loop from scratch in TypeScript or Python against a provider SDK, with step and token limits.
- [ ] Build a small MCP server with two tools and one resource, and connect it to an MCP client over stdio and HTTP.
- [ ] Implement hybrid retrieval with RRF and a re-ranker in Postgres, with citations in the answer.
- [ ] Add a trace view showing each model call, tool call, tokens and cost for one agent run.

---

**Good answers show:**
- Concrete bounds and stop conditions, stated as numbers, for every loop.
- Side effects treated like payments: idempotent, auditable, and gated by approval where risky.
- Security thinking about injection and data access, not just content filtering.
- Retrieval quality backed by evals, with a clear story of what changed and what improved.
- Cost and latency estimated per request and per tenant, with levers named.
- Real failure stories from production and what was changed because of them.

**Prove it:** Build a public demo MCP server over a fake order/invoice dataset with a bounded agent client, hybrid RAG over policy documents, an approval step for any write tool and a small eval set; publish a short write-up with traces, cost per task and two failure cases you fixed.
