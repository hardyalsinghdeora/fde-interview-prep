# LLM Evaluation

"How do you know it works?" is the question that separates demo builders from people who ship LLM systems. Senior interviewers expect a concrete evaluation strategy that covers offline tests, CI gates and production signals. Agentic-AI Architect, Forward Deployed Engineer and Staff/Principal AI platform roles ask most; Engineering Manager roles ask how you make teams adopt it.

> Related: [AI / LLM](../topics/06-ai-llm/README.md) · [System design](../topics/03-system-design/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Why are LLM systems hard to test with ordinary unit tests?
- [ ] Offline evaluation vs. online evaluation — what does each tell you?
- [ ] What is a golden (reference) dataset, and how big does it need to be to be useful?
- [ ] Reference-based metrics (exact match, F1, BLEU/ROUGE) vs. reference-free metrics — when is each appropriate?
- [ ] What is LLM-as-judge, and what are its known biases (position, verbosity, self-preference)?
- [ ] For RAG, what do faithfulness (groundedness), answer relevance, context precision and context recall measure?
- [ ] Why evaluate retrieval and generation separately?
- [ ] What is a regression in an LLM system, and what usually causes one (prompt change, model change, data change)?

## B. Working depth
- [ ] How do you build a golden set from production logs, support tickets and subject-matter experts?
- [ ] How do you write a judge rubric that is reliable — pass/fail vs. 1–5 scales, examples, chain of reasoning?
- [ ] How do you check that the judge agrees with humans, and what agreement level is good enough?
- [ ] How do you evaluate structured output (schema validity, field-level accuracy)?
- [ ] How do you evaluate tool calling: right tool, right arguments, right order, no unnecessary calls?
- [ ] What is a trajectory evaluation for an agent, and how is it different from scoring only the final answer?
- [ ] How do you deal with non-determinism: temperature, multiple samples, pass@k, confidence intervals?
- [ ] How do you compare two prompts or models fairly (pairwise, same set, statistical significance)?
- [ ] Eval frameworks (Ragas, DeepEval, promptfoo, OpenAI Evals, LangSmith) — what do they give you over a script? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you put an eval gate in CI without making the pipeline slow, flaky or expensive?
- [ ] Your golden set passes at 95% but users complain. What are the likely gaps?
- [ ] How do you stop the golden set from going stale or leaking into prompts (overfitting to the eval)?
- [ ] Which online metrics would you track (thumbs, edits, escalations, task completion, latency, cost per task)?
- [ ] How do you run an A/B test or shadow test for a model upgrade, and what is your rollback trigger?
- [ ] How do you evaluate safety: red-teaming, prompt injection suites, jailbreak sets, PII leakage checks?
- [ ] How do you evaluate a multi-turn conversation or a long-running agent where success is delayed?
- [ ] What does a judge model cost at scale, and how do you sample production traffic for evaluation?
- [ ] How do you get domain experts to label data efficiently, and how do you handle disagreement between them?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design an evaluation harness for a customer-support RAG assistant from zero to CI gate in two weeks.
- [ ] The provider releases a new model version. Describe exactly how you decide whether to switch.
- [ ] A customer's legal team asks you to prove the assistant does not invent policy. What evidence do you produce?
- [ ] Design production monitoring that detects quality drift before customers report it.
- [ ] Build a red-team plan for an agent that can send emails and update CRM records.

## E. Hands-on checks (things to be able to DO)
- [ ] Write a 50-case golden set with expected facts and a script that scores a RAG pipeline on faithfulness and context recall.
- [ ] Write an LLM-as-judge prompt with a rubric, and measure its agreement with 30 hand labels.
- [ ] Add a CI job that fails the build if the pass rate drops more than a set threshold from the main branch baseline.
- [ ] Score agent trajectories from traces: tool selection accuracy, steps per task, and cost per successful task.

---

**Good answers show:**
- An eval strategy in layers: deterministic checks first, then judges, then humans, then production signals.
- Judges that are themselves validated against human labels, with known failure modes named.
- Retrieval and generation measured separately so failures can be located.
- Statistical honesty: sample sizes, variance across runs, and confidence before claiming improvement.
- Eval wired into the delivery process (CI gates, model-upgrade playbook), not run once for a slide.
- Business-level metrics (task success, cost per resolved case) tied back to model-level metrics.

**Prove it:** Take an existing RAG or agent project, build a 50–100 case golden set, an LLM-as-judge validated against your own labels, and a CI gate; then change the prompt or model and write up how the gate caught (or missed) a real regression.
