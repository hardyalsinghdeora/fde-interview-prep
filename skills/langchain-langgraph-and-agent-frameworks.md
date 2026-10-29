# LangChain, LangGraph & Agent Frameworks

Teams building agents either adopt a framework or hand-roll the tool loop, and interviewers want to know you can choose deliberately and live with the consequences. Expect it in Agentic-AI Architect, Forward Deployed Engineer and Staff/Principal roles on AI platform teams, often framed as "we use LangGraph — how would you…?".

> Related: [AI / LLM](../topics/06-ai-llm/README.md) · [System design](../topics/03-system-design/README.md) · [Decomposition](../topics/04-decomposition/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What does an agent framework give you over a plain loop of "call model, run tools, append results"?
- [ ] LangChain vs. LangGraph vs. LlamaIndex — what problem is each mainly built for? *(verify)*
- [ ] What is a runnable / LCEL chain in LangChain, and why did it replace the older chain classes? *(verify)*
- [ ] In LangGraph, what are state, nodes, edges and conditional edges?
- [ ] What is a checkpointer in LangGraph, and what does it let you do (resume, time travel, human review)?
- [ ] How does a framework turn a Python/TypeScript function into a tool schema the model can call?
- [ ] CrewAI and AutoGen — what is the role/conversation model, and how is it different from a state graph? *(verify)*

## B. Working depth
- [ ] How do you model a ReAct-style agent as a LangGraph graph, and where does the loop end?
- [ ] How do reducers on state keys work (e.g. appending messages) and what bug appears without one?
- [ ] Implement human-in-the-loop: pause before a risky tool call, show the user, resume with approval or edits.
- [ ] Short-term memory (thread state) vs. long-term memory (cross-thread store) — how does each persist?
- [ ] How do you stream tokens and intermediate steps to a UI from a graph run?
- [ ] How do you add retries, timeouts and fallback models to a node without hiding errors?
- [ ] Sub-graphs and the supervisor pattern — how does a supervisor route to worker agents?
- [ ] How do you get structured output from a node, and what happens when validation fails?
- [ ] How does LlamaIndex structure ingestion (loaders, node parsers, indexes, query engines)?
- [ ] How do you trace and debug a framework run (LangSmith or OpenTelemetry-based tracing)? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] When does a framework hurt: abstraction leaks, hidden prompts, version churn, debugging through layers?
- [ ] How do you keep business logic portable so you can leave the framework later?
- [ ] How do you bound an agent (max steps, token budget, wall-clock, tool allow-list) inside a framework?
- [ ] What state belongs in the checkpointer vs. your own database, and what are the retention/PII implications?
- [ ] How do you make tool side effects idempotent when a checkpointed run is resumed or replayed?
- [ ] How do you upgrade a framework major version in production with long-running paused threads? *(verify)*
- [ ] Multi-agent conversation frameworks can loop or "chat" forever — how do you detect and stop that?
- [ ] How do you control cost when a framework adds hidden calls (query rewriting, summarisation memory)?
- [ ] Self-hosted graph runtime vs. a managed agent platform — what are you buying? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design a refund-processing agent with LangGraph: states, tools, approval step, failure paths.
- [ ] A team's LangChain app is slow and expensive. How do you find out what calls it is actually making?
- [ ] You inherit a CrewAI prototype that works in demos but fails 30% of the time in production. What do you do first?
- [ ] Argue for and against replacing a hand-rolled TypeScript tool loop with LangGraph for an existing product.
- [ ] Design a research → draft → review workflow where a human can edit the draft mid-run and the run continues.

## E. Hands-on checks (things to be able to DO)
- [ ] Build a LangGraph graph with two tools, a conditional edge, a step limit and a Postgres/SQLite checkpointer.
- [ ] Add an interrupt before a "send email" tool and resume the thread with an approval from a CLI.
- [ ] Rewrite the same agent as a ~100-line hand-rolled loop and compare lines, latency and debuggability.
- [ ] Wire tracing so each node, model call and tool call shows as a span with tokens and cost.

---

**Good answers show:**
- A clear reason to use (or not use) a framework, tied to state, durability and human review needs.
- Knowledge that the core loop is simple, and that the hard parts are bounds, idempotency and observability.
- Explicit control over prompts and hidden model calls, so cost and behaviour are predictable.
- A plan for persistence, resumption and replay that respects side effects.
- Healthy caution about version churn, with a way to isolate the framework behind your own interfaces.

**Prove it:** Take an agent you already built with a hand-rolled loop and re-implement it in LangGraph with a checkpointer and a human-approval interrupt; write up the trade-offs (code size, latency, resume behaviour, tracing) with numbers from the same 20-task test set.
