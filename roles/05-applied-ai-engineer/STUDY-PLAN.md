# Study Plan — Applied AI Engineer

For engineers preparing for a customer-facing Applied AI Engineer loop at a model provider, cloud AI team or AI-native company; assumes about 10–12 hours a week for six weeks.

Read the role overview first: [README.md — The role](./README.md#the-role) and the [typical interview loop](./README.md#typical-interview-loop). This loop goes deeper on the model layer than any other role here: a prompt-optimisation exercise, live coding against an LLM API, a take-home with an eval, and cost and model-choice conversations with customers. This plan is hands-on from day one: every week produces working code you can show.

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim areas you rate 4 or 5.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| LLM API fluency | | From a blank file you can call the target API with structured output, tool use, streaming and retries in under 20 minutes. |
| Prompt engineering | | Given a failing prompt and examples, you diagnose the failure categories before editing, and improve it measurably in 30 minutes. |
| Evals | | You can build a labelled set, an automatic scorer and an LLM-as-judge rubric, and explain when the judge can be trusted. |
| Model selection and migration | | You can recommend a model for a use case from quality, latency and cost, and plan a migration with regression checks. |
| Cost and latency | | You can estimate monthly token cost out loud and name the levers: caching, batching, smaller models, shorter prompts, streaming. |
| Python practical coding | | You write clean async or threaded code with rate limiting, error handling and simple tests. |
| RAG and agents | | You have built a RAG with citations and a tool-use loop, and know their common failure modes. |
| Safety and enterprise blockers | | You can answer data retention, training-on-data, hallucination and prompt-injection concerns for a security or legal team. |
| Customer communication | | You can run a workshop for engineers new to LLMs and present results to executives. |

## 6-week plan

Assumes ~10–12 hours a week. Use the target company's API (or the one you will most likely be tested on) for every build, and read its official docs as you go.

### Week 1 — LLM fundamentals and API fluency

**Goals:** fluent, error-tolerant calls to the API; fundamentals solid enough to explain to customers.

- [ ] Study [06 AI/LLM — A. LLM fundamentals](../../topics/06-ai-llm/ANSWERS.md#a-llm-fundamentals) and [B. Prompt engineering](../../topics/06-ai-llm/ANSWERS.md#b-prompt-engineering).
- [ ] Read the README's [classification live exercise](./README.md#live-classify-500-support-tickets-into-categories-with-structured-output-handle-api-errors-and-report-accuracy-against-labels).
- [ ] Practical coding: two problems from [02 Coding — A. Reported FDE coding problems](../../topics/02-coding/ANSWERS.md#a-reported-fde-coding-problems), favouring rate limiter and concurrency problems.
- [ ] Python fundamentals: [13 — Language-specific (Python)](../../topics/13-cs-fundamentals/ANSWERS.md#language-specific-python).
- [ ] Motivation: [01 — AI-company specific motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md#ai-company-specific-motivation).

**Deliverable:** a ticket classifier over a public dataset (100–500 items) using structured output, with retries and backoff, a labelled subset, and an accuracy and confusion report.

**Mock:** a 30-minute recruiter screen ("why customer-facing AI, why this company") followed by a 30-minute practical coding problem with a peer.

**End of week check:** you can rebuild the classifier's core from memory in 20–30 minutes.

### Week 2 — Prompt optimisation on someone else's prompt

**Goals:** pass the prompt-optimisation exercise: diagnose first, then change, then prove it.

- [ ] Read [Prompt optimisation — model answers](./README.md#prompt-optimisation-on-a-customers-prompt-1): [the 30-minute exercise](./README.md#live-exercise-here-is-a-customers-production-prompt-and-20-examples-where-it-fails-improve-it-in-30-minutes), [diagnosing first](./README.md#how-do-you-diagnose-why-a-prompt-fails-before-you-change-anything), [refactoring a 6,000-token prompt](./README.md#the-customers-prompt-is-6000-tokens-of-rules-added-over-a-year-how-do-you-refactor-it) and [porting from another provider](./README.md#the-customers-prompt-was-tuned-for-another-providers-model-how-do-you-port-it).
- [ ] Re-read [06 — B. Prompt engineering](../../topics/06-ai-llm/ANSWERS.md#b-prompt-engineering) and the target provider's official prompt engineering guide.
- [ ] Read [when prompting will not fix the problem](./README.md#when-do-you-tell-a-customer-that-prompting-will-not-fix-their-problem).

**Deliverable:** two prompt-optimisation write-ups on different tasks (for example extraction and classification): the weak prompt, 20 test cases, failure categories, the improved prompt, and before/after numbers.

**Mock:** a 30-minute prompt-optimisation exercise with a partner who gives you a deliberately weak prompt and 20 failing examples you have not seen.

**End of week check:** you spend the first five minutes categorising failures, not editing, and you finish with a number.

### Week 3 — Evals with and for customers

**Goals:** evals are the core of the job; be able to build one fast and defend it.

- [ ] Study [06 — E. Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important).
- [ ] Read [Building evals for a customer — model answers](./README.md#building-evals-for-a-customer-1), especially [week one with no evals](./README.md#the-customer-wants-to-go-live-in-three-weeks-and-has-no-evals-what-do-you-build-in-week-one), [the contract-clause eval](./README.md#design-an-eval-for-a-contract-clause-extraction-use-case) and [LLM-as-judge rubrics](./README.md#how-do-you-write-an-llm-as-judge-rubric-that-the-customers-domain-experts-trust).
- [ ] Read the [two-prompt comparison harness exercise](./README.md#live-build-a-minimal-harness-that-compares-two-prompts-on-a-dataset-running-calls-concurrently).
- [ ] Read [good eval scores but unhappy users](./README.md#eval-scores-look-good-but-the-customers-end-users-are-unhappy-what-is-going-on).

**Deliverables:**
1. A reusable eval harness: runs two prompts or models over a dataset concurrently with a rate limit, scores with exact-match and an LLM judge, and prints a comparison table with cost and latency.
2. A one-page LLM-as-judge rubric with three calibration examples and a note on how you checked judge agreement with human labels.

**Mock:** a 45-minute "design an eval" interview: the partner gives a use case (for example contract extraction or support replies) and asks you to define metrics, data, labelling and sign-off.

**End of week check:** you can turn "the answers should be good" into two or three numbers a customer would sign off on.

### Week 4 — Model choice, migration, cost, latency and production

**Goals:** be the person a customer trusts on "which model, how much, how fast".

- [ ] Read [Model selection and migration — model answers](./README.md#model-selection-and-migration-1) and [Cost and latency trade-offs — model answers](./README.md#cost-and-latency-trade-offs-1).
- [ ] Study [06 part 2 — G. Production LLM engineering](../../topics/06-ai-llm/ANSWERS-part2.md#g-production-llm-engineering) and [F. Fine-tuning and model customisation](../../topics/06-ai-llm/ANSWERS-part2.md#f-fine-tuning-and-model-customisation).
- [ ] Troubleshooting: [12 — C. LLM-specific troubleshooting](../../topics/12-debugging-troubleshooting/ANSWERS.md#c-llm-specific-troubleshooting).
- [ ] Read the target provider's current pricing page, rate-limit docs, prompt caching and batch docs, and the release notes for its last two model versions.

**Deliverables:**
1. A one-page cost model template (requests per day, input and output tokens, cache hit rate, batch share, model mix) filled in for an example workload, with three cost-reduction levers and their trade-offs.
2. A migration plan from one model version to the next, using your Week 3 harness to show regressions.

**Mock:** a 30-minute customer role-play: a partner plays a customer CTO who says ["the API is too expensive"](./README.md#the-customer-says-the-api-is-too-expensive-walk-through-the-levers) and then ["p95 latency is 12 seconds"](./README.md#a-chat-feature-has-a-p95-latency-of-12-seconds-how-do-you-bring-it-down).

**End of week check:** you can estimate a monthly bill out loud with stated assumptions in under five minutes.

### Week 5 — Live LLM coding and the take-home

**Goals:** survive live coding with API access and produce a strong take-home.

- [ ] Read [Live LLM coding with API access — model answers](./README.md#live-llm-coding-with-api-access-1): the [tool-use loop](./README.md#live-implement-a-tool-use-loop-where-the-model-answers-questions-by-calling-a-lookup-function), [streaming with mid-stream failure](./README.md#live-stream-a-response-to-a-user-and-handle-a-failure-mid-stream) and [using AI coding assistants well](./README.md#if-ai-coding-assistants-are-allowed-in-the-interview-how-do-you-use-them-well).
- [ ] Study [06 — C. RAG](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation), [D. Agents and tool use](../../topics/06-ai-llm/ANSWERS.md#d-agents-and-tool-use) and [part 2 — J. Take-home exercises](../../topics/06-ai-llm/ANSWERS-part2.md#j-take-home--build-exercises-reported).
- [ ] Do two prompts from [03 System design — B. AI / LLM prompts](../../topics/03-system-design/ANSWERS.md#b-ai--llm-system-design-prompts).

**Deliverable:** a take-home-style project: RAG with citations over a public document set, with an eval set, a failure analysis, cost and latency numbers, and a README written as a cookbook ([how to structure a cookbook](./README.md#you-are-asked-to-write-a-cookbook-for-a-common-pattern-for-example-rag-with-citations-how-do-you-structure-it)).

**Mock:** a 60-minute live LLM coding session with a peer: build a tool-use loop from scratch, then add streaming and error handling as new requirements.

**End of week check:** you can write a working tool-use loop from memory and explain each stop condition.

### Week 6 — Enablement, adoption blockers, presentation and company

**Goals:** the customer side of the role, and a polished take-home presentation.

- [ ] Read [Enterprise adoption blockers — model answers](./README.md#enterprise-adoption-blockers-1) and [06 part 2 — H. Safety, security and responsible AI](../../topics/06-ai-llm/ANSWERS-part2.md#h-safety-security-and-responsible-ai).
- [ ] Read [Cookbooks, reference implementations and enablement — model answers](./README.md#cookbooks-reference-implementations-and-enablement-1), including the [LLM workshop agenda](./README.md#run-a-technical-workshop-for-a-customer-engineering-team-that-is-new-to-llms-what-is-the-agenda).
- [ ] Read [Presentation and behavioral — model answers](./README.md#presentation-and-behavioral-role-specific-1).
- [ ] Company research: [11 — OpenAI](../../topics/11-company-specific/ANSWERS.md#openai--forward-deployed-engineer) or [Anthropic](../../topics/11-company-specific/ANSWERS.md#anthropic--forward-deployed-engineer--applied-ai), and the [research checklist](../../topics/11-company-specific/ANSWERS.md#company-research-checklist-do-for-every-company).
- [ ] Customer scenarios: [09 — B. Difficult situations](../../topics/09-customer-scenarios/ANSWERS.md#b-difficult-situations) and [E. Business acumen](../../topics/09-customer-scenarios/ANSWERS.md#e-business-acumen).
- [ ] Tick off the role's [Prep checklist](./README.md#prep-checklist).

**Deliverable:** a 20-minute presentation of your Week 5 take-home for a mixed audience: the problem, a live demo, eval results, one failure analysis, cost, and what you would do next.

**Mock:** a full mock loop: practical coding (45), prompt optimisation (30), take-home presentation with deep-dive questions (45), and a behavioural / values round including your view on AI safety (45).

**End of week check:** you can answer a security team's data-retention and training-on-data questions accurately for the target provider, citing its current docs.

## 2-week crash plan

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and fundamentals | Read the role; fundamentals refresh | [README — role](./README.md#the-role), [06 — A](../../topics/06-ai-llm/ANSWERS.md#a-llm-fundamentals) |
| 2 | API fluency | Structured-output classifier with retries | [Classification exercise](./README.md#live-classify-500-support-tickets-into-categories-with-structured-output-handle-api-errors-and-report-accuracy-against-labels) |
| 3 | Prompting | One prompt-optimisation exercise, timed | [Prompt optimisation answers](./README.md#prompt-optimisation-on-a-customers-prompt-1), [06 — B](../../topics/06-ai-llm/ANSWERS.md#b-prompt-engineering) |
| 4 | Evals | Eval harness comparing two prompts | [Harness exercise](./README.md#live-build-a-minimal-harness-that-compares-two-prompts-on-a-dataset-running-calls-concurrently), [06 — E](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important) |
| 5 | Evals for customers | LLM-as-judge rubric; success criteria | [Evals answers](./README.md#building-evals-for-a-customer-1) |
| 6 | Tool use | Tool-use loop from scratch | [Tool-use exercise](./README.md#live-implement-a-tool-use-loop-where-the-model-answers-questions-by-calling-a-lookup-function), [06 — D](../../topics/06-ai-llm/ANSWERS.md#d-agents-and-tool-use) |
| 7 | Mock day | Prompt-optimisation mock + live coding mock | [Mock schedule](#mock-interview-schedule) |
| 8 | Model choice | Model selection and migration answers | [Model selection answers](./README.md#model-selection-and-migration-1) |
| 9 | Cost and latency | Cost model template; latency levers | [Cost and latency answers](./README.md#cost-and-latency-trade-offs-1), [06 part 2 — G](../../topics/06-ai-llm/ANSWERS-part2.md#g-production-llm-engineering) |
| 10 | Take-home | Small RAG with citations and evals | [06 — C](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation), [06 part 2 — J](../../topics/06-ai-llm/ANSWERS-part2.md#j-take-home--build-exercises-reported) |
| 11 | Blockers and safety | Security, legal, low adoption | [Adoption blockers](./README.md#enterprise-adoption-blockers-1), [06 part 2 — H](../../topics/06-ai-llm/ANSWERS-part2.md#h-safety-security-and-responsible-ai) |
| 12 | Presentation | 20-minute take-home presentation | [Presentation answer](./README.md#presentation-round-present-your-take-home-to-a-mixed-technical-and-executive-audience) |
| 13 | Company and stories | Company docs and release notes; stories; full mock | [11 — Anthropic](../../topics/11-company-specific/ANSWERS.md#anthropic--forward-deployed-engineer--applied-ai), [my story bank](../../my-prep/STORY-BANK.md) |
| 14 | Rest and review | Re-read cost model and eval rubric | [README checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–45 | Build: one more piece of the week's project, writing the API code by hand (no copy-paste from earlier projects) to build recall. |
| 45–65 | One README model answer, then answer a sibling question out loud as if to a customer. |
| 65–80 | Read the target provider's docs or a cookbook section; note one thing that changed recently. |
| 80–90 | Log eval numbers and mistakes; pick tomorrow's task. |

Weekends: the week's deliverable and the mock. Keep API spend low by using small datasets and the cheapest suitable model for iteration.

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Recruiter screen + practical coding | A friend; a peer engineer or Pramp / interviewing.io for coding |
| 2 | Prompt-optimisation exercise | A peer who prepares a weak prompt and failing examples |
| 3 | Design an eval | An engineer who has shipped an LLM feature, if you can find one |
| 4 | Cost and latency customer role-play | A friend playing a skeptical CTO |
| 5 | 60-minute live LLM coding | A peer engineer; record your screen |
| 6 | Full loop with take-home presentation | Two or three partners, one non-technical; Exponent and similar platforms for the behavioural part |

Do one timed prompt-optimisation or live-coding rep every week from Week 2 onwards.

## Fast track for the repo owner's profile

- **Skim:** API design, rate limiting and integration patterns; building an API gateway with JWT auth and rate limiting means rate limiters, retries and backoff in live coding should come quickly.
- **Skim:** data handling and SQL in the take-home; deep MySQL and PostgreSQL experience covers the data side.
- **Biggest gap 1 — production LLM, RAG, agent and eval experience.** This role is built on it. Do every Week 1–5 build, and give Weeks 2 and 3 (prompting and evals) extra time.
- **Biggest gap 2 — Python practical coding fluency.** Only PHP and SQL are evidenced; confirm the language first, then do daily API coding in Python so live coding is not slowed by syntax.
- **Biggest gap 3 — public code portfolio.** Publish the Week 3 harness and the Week 5 take-home; they become the portfolio and the presentation material.
- **Cost and model-choice rounds:** the speech-to-text provider evaluation (a contribution, not sole authorship) with a buy-vs-build cost model and data-protection constraints is the closest real story; describe your part precisely.
- **Enterprise adoption blockers:** the India DPDP Act constraints in that evaluation, and row-level access control work, support answers on data retention and access.
- **Applied system design:** the API gateway and aggregation/BFF layer across about 13 services gives a credible base for "where the LLM call sits in the customer's stack".
- **Behavioural ("improved quality through measurement", "told a customer it was not the right fit"):** the migration with reconciliation across six product lines shows measurement-driven work; delivery leadership and mentoring cover the workshop and enablement questions.
- Keep all stories in [../../my-prep/STORY-BANK.md](../../my-prep/STORY-BANK.md).

## Resources

- Anthropic documentation (https://docs.anthropic.com), including the prompt engineering guide, tool use, prompt caching and batch processing pages.
- OpenAI platform documentation (https://platform.openai.com/docs) and the OpenAI Cookbook.
- Anthropic's engineering article "Building effective agents".
- *AI Engineering* by Chip Huyen — evaluation, prompting, RAG and production concerns.
- Hamel Husain's and Eugene Yan's published writing on LLM evals and LLM application patterns.
- DeepLearning.AI short courses on prompting, RAG and evaluation.
- Mock platforms: Pramp, interviewing.io and Exponent.
