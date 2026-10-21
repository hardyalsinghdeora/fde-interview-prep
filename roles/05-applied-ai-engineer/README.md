# Applied AI Engineer

Customer-facing engineer at an AI lab or AI-native company who makes a customer's LLM use case work in production: better prompts, real evals, the right model, acceptable cost and latency.

---

## The role

**Day to day.** You sit between a model provider (or an AI platform) and its customers' engineering teams. A typical week mixes: reviewing a customer's prompt and failing examples, building or extending an eval set with their domain experts, advising on which model or model version to use and planning migrations when new versions ship, explaining cost and latency trade-offs, unblocking security/legal reviews, writing reference code and cookbooks that many customers reuse, and feeding recurring problems back to product and research teams.

**Common titles / aliases.** Applied AI Engineer, AI Engineer (customer-facing), AI Solutions Engineer, Solutions Architect (GenAI), Customer Engineer (AI), AI Deployment Engineer, Applied AI Architect. Some companies fold this work into a Forward Deployed Engineer title.

**Who hires for it (examples; titles vary and change).**
- Model providers: Anthropic (Applied AI team), OpenAI (solutions / deployment / forward deployed teams), Google (Cloud and DeepMind customer-facing roles), Cohere, Mistral.
- Cloud platforms with GenAI customer teams: AWS, Google Cloud, Microsoft Azure.
- AI-native companies and data/AI platforms: Databricks, Scale AI, Glean, Harvey, and many YC-stage AI startups.

**How it differs from FDE.** An FDE often embeds with one or a few customers for months and owns a full deployment (data pipelines, integrations, infra). An Applied AI Engineer usually covers more customers at a time, goes deeper on the model layer (prompting, evals, model behaviour, migrations) and shallower on the customer's surrounding systems. The output is more often advice, prototypes, evals and reusable assets than production code inside the customer's stack. The boundary is blurry and company-dependent.

**Who it suits.** People who enjoy experimenting with models, can be precise about what "better" means, explain trade-offs clearly to both engineers and executives, and like breadth (many customers, many use cases) over owning one codebase for years.

**Skills mix.**

| Skill | Weight | Notes |
|---|---|---|
| Coding | Med–High | Python/TypeScript scripts, prototypes, eval harnesses; less large-system ownership |
| LLM depth (prompting, evals, model behaviour) | High | The core of the job |
| Customer / communication | High | Workshops, exec readouts, technical advising |
| Sales support | Med | Pre-sales technical validation, POCs, pricing conversations |
| Product feedback | Med–High | Turning field patterns into product and model requests |
| Infra / data engineering | Low–Med | Enough to reason about integration, not usually to build it |

**Career paths.** Senior / Staff Applied AI Engineer; Applied AI or Solutions team lead / manager; move to product management for the API or developer platform; move to research engineering or model behaviour / evals teams; FDE or founding engineer at an AI startup.

---

## Typical interview loop

Loops vary by company and change often. The table shows a typical/reported shape, not a guarantee.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Motivation, why customer-facing, why this lab/company | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) |
| Technical screen / practical coding | Clean Python, API calls, data handling; sometimes classic DS&A | [02-coding](../../topics/02-coding/README.md) |
| Prompt-optimisation exercise | Diagnosing and improving a real-looking prompt against examples | This file, "Prompt optimisation" |
| Live LLM coding with API access (reported at some labs) | Calling the API, structured output, tool use, concurrency, quick evals | This file, "Live LLM coding"; [06-ai-llm](../../topics/06-ai-llm/README.md) |
| Take-home + deep dive (common) | RAG / agent / eval harness, then defending choices | [06-ai-llm section J](../../topics/06-ai-llm/ANSWERS-part2.md) |
| Use-case / applied system design | Designing a customer deployment with real constraints | [03-system-design](../../topics/03-system-design/ANSWERS.md) |
| Customer scenario / presentation | Explaining trade-offs, handling objections, exec communication | [09-customer-scenarios](../../topics/09-customer-scenarios/ANSWERS.md) |
| Hiring manager / values | Ownership, judgment, safety views (labs) | [10-behavioral](../../topics/10-behavioral/ANSWERS.md), [11-company-specific](../../topics/11-company-specific/ANSWERS.md) |

---

## Common rounds — use the shared sections

- [06-ai-llm](../../topics/06-ai-llm/README.md) — the most important shared section. Prioritise B (prompting), E (evaluation), G (production: rate limits, cost, latency, deprecation) and H (safety). This file assumes you know those fundamentals and goes one step further into customer-specific situations.
- [11-company-specific](../../topics/11-company-specific/README.md) — the OpenAI and Anthropic sections; read the company's own docs, cookbooks and model release notes before the loop.
- [03-system-design](../../topics/03-system-design/README.md) — section B (AI/LLM system design) and the follow-up questions in D.
- [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) — scoping, difficult situations, business acumen; the pricing and objection questions matter more here than in many FDE loops.
- [02-coding](../../topics/02-coding/README.md) — practical / incremental problems; rate limiter and concurrency questions come up because API work needs them.
- [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) — section C (LLM-specific troubleshooting).
- [10-behavioral](../../topics/10-behavioral/README.md) and [14-questions-to-ask](../../topics/14-questions-to-ask/README.md).

---

## Role-specific questions

### Prompt optimisation on a customer's prompt
- [ ] Live exercise: here is a customer's production prompt and 20 examples where it fails. Improve it in 30 minutes.
- [ ] How do you diagnose why a prompt fails before you change anything?
- [ ] The customer's prompt is 6,000 tokens of rules added over a year. How do you refactor it?
- [ ] How do you prove to the customer that your prompt change is an improvement and not a regression?
- [ ] The customer's prompt was tuned for another provider's model. How do you port it?
- [ ] When do you tell a customer that prompting will not fix their problem?

### Building evals for a customer
- [ ] The customer wants to go live in three weeks and has no evals. What do you build in week one?
- [ ] How do you get the customer's domain experts to label data efficiently?
- [ ] Design an eval for a contract-clause extraction use case.
- [ ] How do you write an LLM-as-judge rubric that the customer's domain experts trust?
- [ ] How do you turn a customer's vague success criteria into numbers both sides sign off on?
- [ ] Eval scores look good but the customer's end users are unhappy. What is going on?
- [ ] The customer needs the same use case in eight languages. How do you evaluate and tune quality across languages?

### Model selection and migration
- [ ] A customer asks which model they should use. Walk through how you decide.
- [ ] A new model version has been released. Plan a customer's migration from the old version.
- [ ] After a migration, outputs are longer and the formatting changed. What do you do?
- [ ] When would you route part of a workflow to a smaller, cheaper model?
- [ ] The customer wants extended reasoning ("thinking") turned on for every request. How do you respond?
- [ ] The customer must consume models through a cloud marketplace (Bedrock, Vertex AI, Azure). How does that affect your advice?

### Cost and latency trade-offs
- [ ] Estimate the monthly cost of a customer's workload and present it.
- [ ] The customer says the API is too expensive. Walk through the levers.
- [ ] A chat feature has a p95 latency of 12 seconds. How do you bring it down?
- [ ] How do you restructure a prompt to get prompt-cache hits?
- [ ] When would you move a workload to a batch API?
- [ ] The customer is planning a launch. How do you explain rate limits and capacity planning to them?

### Live LLM coding with API access
- [ ] Live: classify 500 support tickets into categories with structured output, handle API errors, and report accuracy against labels.
- [ ] Live: implement a tool-use loop where the model answers questions by calling a lookup function.
- [ ] Live: build a minimal harness that compares two prompts on a dataset, running calls concurrently.
- [ ] Live: stream a response to a user and handle a failure mid-stream.
- [ ] If AI coding assistants are allowed in the interview, how do you use them well?

### Cookbooks, reference implementations and enablement
- [ ] You are asked to write a cookbook for a common pattern (for example RAG with citations). How do you structure it?
- [ ] What makes a reference implementation that a customer's team will actually adopt?
- [ ] Several customers hit the same problem. How do you turn that into product feedback or a reusable asset?
- [ ] Run a technical workshop for a customer engineering team that is new to LLMs. What is the agenda?

### Enterprise adoption blockers
- [ ] The customer's security team blocks the rollout over data retention and training on their data. How do you unblock it?
- [ ] Legal is worried about hallucinations in customer-facing output. How do you address it?
- [ ] The POC worked but nobody uses it. How do you diagnose low adoption?
- [ ] An executive wants "AI everywhere" but has no use case. How do you pick the first one?
- [ ] A customer asks for a capability the model or platform does not support. How do you respond?

### Presentation and behavioral (role-specific)
- [ ] Presentation round: present your take-home to a mixed technical and executive audience.
- [ ] Tell me about a time you improved an AI or ML system's quality through measurement.
- [ ] Tell me about a time you told a customer the technology was not the right fit.

---

## Model answers

### Prompt optimisation on a customer's prompt

#### Live exercise: here is a customer's production prompt and 20 examples where it fails. Improve it in 30 minutes.
Spend the time in this order, and narrate it:
1. **Read the task and 3–4 failures first (5 min).** Restate what correct output is. Ask: is there a ground truth for each example?
2. **Group the failures (5 min).** E.g. 8 wrong format, 6 missed an edge case the prompt never mentions, 4 contradictory instructions, 2 arguably correct labels. Grouping tells you what to fix and protects you from fixing one example at a time.
3. **Fix causes, not symptoms (10 min).** Typical fixes: state the task and audience in the first lines; remove contradictions; replace "don't do X" lists with a clear description of the desired behaviour; add 2–3 diverse examples covering the failure groups; separate instructions from input data with tags; specify the output schema exactly or use structured output; allow an "unknown" option instead of forcing a guess.
4. **Re-run all 20 plus some that previously passed (5 min).** Report before/after counts per failure group.
5. **Say what you would do next (5 min).** Grow the eval set, check variance across runs, confirm the 2 disputed labels with the customer.

What interviewers look for: a hypothesis before editing, measurement after, and not overfitting to the 20 examples.

#### How do you diagnose why a prompt fails before you change anything?
- **Reproduce.** Run the failing input several times at the customer's settings (model, temperature, max tokens, system prompt). Is it consistent or intermittent?
- **Check the inputs, not just the prompt.** Many failures are retrieval returning the wrong context, truncated input, broken templating (empty variables, escaped characters) or a token limit cutting output.
- **Read the full request as the model sees it.** Log the rendered prompt; look for contradictions, buried instructions, and examples that teach the wrong pattern.
- **Ask the model to explain.** Reasoning output or a "why did you choose this" follow-up often reveals a misread instruction.
- **Classify the failure:** missing knowledge (needs context/RAG), ambiguous spec (needs a decision from the customer), capability limit (needs a different model or decomposition), formatting (needs schema/structured output), or label error (the "expected" answer is wrong).
- Only then change one thing at a time and measure.

#### The customer's prompt is 6,000 tokens of rules added over a year. How do you refactor it?
1. **Freeze a baseline.** Build an eval set from production traffic and the incidents that caused each rule, and score the current prompt.
2. **Inventory the rules.** Tag each as task definition, policy, formatting, edge case or patch-for-old-model. Find duplicates and contradictions; ask the owner which wins.
3. **Restructure:** role and goal, context about users, core instructions, policies, output format, a few examples. Put long reference material (policies, catalogues) in clearly tagged sections or move it to retrieval or tools if only some of it is needed per request.
4. **Delete aggressively, then measure.** Many rules were patches for older models. Remove a group, re-run evals, keep it removed if scores hold.
5. **Consider splitting** into a router plus specialised prompts if the prompt is really several tasks.
6. **Put it under version control** with the eval run attached to each change, and name an owner.

Result to report: shorter prompt, equal or better scores, lower cost and latency, and a process so it does not grow back.

#### How do you prove to the customer that your prompt change is an improvement and not a regression?
- Agree the eval set and metric **before** showing results, so you are not seen to pick favourable examples.
- Run old and new prompts on the same set, same model and settings, several times if outputs vary; report mean and spread, not one run.
- Show results per segment (case type, language, customer tier), because an average can hide a regression in an important slice.
- Include a **regression set** of cases that already worked, not only the failures you targeted.
- Show a handful of side-by-side examples, including any that got worse, and explain why.
- For subjective quality, use blind pairwise comparison by the customer's experts, or a validated LLM judge.
- Recommend a staged rollout (shadow or small traffic share) with the online metric they care about, and an easy rollback.

#### The customer's prompt was tuned for another provider's model. How do you port it?
- **Do not translate word for word.** Start from the task description and the eval set, not the old prompt's quirks.
- Remove provider-specific workarounds: odd capitalised warnings, repeated instructions, hacks for output formats the old model struggled with.
- Use the target model's documented conventions: system prompt use, tags or delimiters for structure, its structured-output or tool-use features, its guidance on examples and reasoning.
- Check parameter differences: max output tokens, temperature defaults, stop sequences, how tools and JSON mode are specified.
- Run the shared eval set on both models and compare per category. Expect some categories to improve and some to drop; fix the drops individually.
- Watch non-quality differences: verbosity, refusal behaviour, latency, token counts (tokenisers differ, so cost estimates must be redone).
- Keep the integration behind an abstraction so the customer can A/B and roll back.

#### When do you tell a customer that prompting will not fix their problem?
When the evidence points to a cause prompting cannot reach:
- **Missing knowledge**: the answer depends on private or fresh data the model does not have. Fix: retrieval or tools.
- **Ambiguous spec**: experts disagree on the right answer. Fix: a policy decision and labelling guidelines, not a better prompt.
- **Capability ceiling**: scores plateau across many prompt variants and the errors are reasoning errors. Fix: a stronger model, decomposing the task, or adding deterministic code for the hard part (maths, lookups, rules).
- **Volume or latency constraints** that only a smaller fine-tuned model or a different architecture can meet.
- **Bad inputs**: OCR noise, truncated documents.

Say it with data: "We tried N variants; scores moved from 71% to 74% and the remaining errors are all X. The next gain comes from Y." Being direct early saves the customer weeks.

### Building evals for a customer

#### The customer wants to go live in three weeks and has no evals. What do you build in week one?
- **Day 1: define success with the owner.** What does a correct output look like, what is a harmful one, and what error rate is acceptable to launch?
- **Days 1–2: collect 100–200 real inputs** (production logs, support tickets, historical documents), sampled across the important segments, plus known hard cases and adversarial inputs.
- **Days 2–3: label a first set** with 1–2 domain experts using a short guideline. Even 50 high-quality labels beat 1,000 guessed ones.
- **Day 3: write cheap automated checks first**: schema validity, required fields, exact-match fields, forbidden content, length.
- **Days 3–4: add an LLM judge** only for the subjective parts, and check it against the expert labels.
- **Day 5: a runnable harness** (script or eval tool) that scores any prompt/model version in minutes and outputs a per-category table. Baseline the current system.

Weeks two and three then become "improve the score and close the launch-blocking gaps", and the same set becomes the regression gate after launch.

#### How do you get the customer's domain experts to label data efficiently?
- **Respect their time:** expert hours are the scarcest resource on the project. Ask for short sessions, not a big batch.
- **Write a one-page guideline** with definitions and 5–10 worked examples; update it as disagreements come up.
- **Pre-fill with the model.** Experts correct a draft label instead of writing from scratch; it is much faster. Watch for anchoring bias by having them label a small sample blind.
- **Use a simple tool** they already know (a spreadsheet with dropdowns works) or a lightweight labelling UI. Avoid making them learn new software.
- **Measure agreement.** Have two experts label an overlap of 30–50 items. Low agreement means the task definition needs work, not more labels.
- **Prioritise what to label:** cases where the model is uncertain, segments with few examples, production failures.
- Show them the impact ("your labels found the issue that moved accuracy 8 points") to keep them engaged.

#### Design an eval for a contract-clause extraction use case.
**Task:** from a contract, extract clauses (e.g. termination, liability cap, governing law, renewal terms) with the text span and normalised values.

**Dataset:** 100–300 contracts across templates, counterparties, lengths, scan quality and languages. Lawyers label each clause: present/absent, span, normalised value (e.g. "liability cap: 12 months' fees").

**Metrics per clause type:**
- Detection: precision and recall on presence. Recall usually matters more (a missed indemnity clause is worse than a false flag), so agree the trade-off with legal.
- Span: overlap or exact-match of quoted text; check the quote exists verbatim in the source (catches fabrication).
- Normalised value: exact match for structured fields (dates, amounts, jurisdiction), judge or expert review for free text.
- "Not present" correctness: the model must say absent, not invent a clause.

**Slices:** long contracts (clause near the end), amendments that override earlier clauses, scanned PDFs.

**Report:** a table per clause type, and a list of errors for lawyers to review. Set launch thresholds per clause type, with human review for the low-confidence ones.

#### How do you write an LLM-as-judge rubric that the customer's domain experts trust?
- **Write it with them.** Start from how experts grade today: what makes an answer wrong, acceptable or excellent. Use their words.
- **Make criteria specific and separate.** One judgment per criterion (factually correct vs. the source; follows policy; complete; tone), each pass/fail or a small scale with anchored descriptions. Avoid "rate quality 1–10".
- **Give the judge what it needs:** the source context, a reference answer if one exists, and the policy text.
- **Ask for a short reasoning before the verdict**, and return structured output.
- **Validate:** run the judge on 50–100 expert-labelled items. Report agreement (accuracy or Cohen's kappa) per criterion. Iterate until agreement is close to expert-to-expert agreement.
- **Check known biases:** position bias in pairwise comparisons (swap order), length bias, self-preference.
- **Keep auditing:** experts spot-check a sample of judge decisions every release. Show them disagreements; trust grows when they see the judge is checked, not assumed.

#### How do you turn a customer's vague success criteria into numbers both sides sign off on?
Start from "it should be accurate and helpful" and work to something testable:
1. **Ask what decision or action the output drives** and what a mistake costs. That tells you which errors matter (false negatives in fraud, wrong amounts in finance, tone in support).
2. **Define the unit of evaluation** (a document, a ticket, a conversation) and what "correct" means for it, with examples.
3. **Pick 2–4 metrics:** a primary quality metric, a safety metric with a hard threshold, and operating constraints (p95 latency, cost per item).
4. **Baseline the current process** (human accuracy, time per item). "Better than today" is a fairer bar than "perfect".
5. **Write thresholds down:** e.g. "≥ 92% field-level accuracy on the agreed 300-item set, 0 critical errors in the red-team set, p95 < 4 s, ≤ $0.03 per document; items below confidence 0.7 go to human review."
6. **Get sign-off from the business owner and the risk owner**, and agree how the set will be refreshed.

#### Eval scores look good but the customer's end users are unhappy. What is going on?
The eval is measuring something different from what users experience. Check:
- **Distribution shift:** the eval set was built from old or clean data; production inputs are messier, longer, in other languages, or about new topics. Sample recent production traffic and score it.
- **Wrong metric:** the eval checks correctness but users care about speed, length, tone, or the answer being actionable.
- **Missing failure modes:** refusals, "I don't know" too often, slow responses, UI issues, or retrieval gaps for questions no one put in the set.
- **Judge drift or leakage:** the LLM judge is too lenient, or prompts were tuned on the eval set until it was overfit.
- **Expectation gap:** users were promised more than the system was designed to do.

Actions: read 50 real complaints and conversations, label them, add the failures to the eval set, and add user-facing metrics (thumbs-down rate, escalations, task completion) to the dashboard alongside offline scores.

#### The customer needs the same use case in eight languages. How do you evaluate and tune quality across languages?
- **Do not assume English results transfer.** Quality, refusal behaviour, tone and token counts (and so cost and latency) all vary by language.
- **Build a per-language eval slice:** real inputs from each market where possible; translated English cases only as a stopgap, because translation removes local phrasing, names, formats (dates, addresses, currency) and code-switching.
- **Native-speaker labels and review** for each language, at least for a calibration sample. Validate any LLM judge per language, not only in English.
- **Report per language** against the same thresholds; a good average can hide one weak market.
- **Decide prompt strategy by evals:** usually one English system prompt with "reply in the user's language" works well; add language-specific examples or terminology lists (product names, legal terms) where a language underperforms.
- **Check the full pipeline:** retrieval and embeddings in each language (or cross-language retrieval), OCR or speech recognition quality, and whether source documents exist in that language.
- **Plan the rollout by language,** starting where quality and volume are highest, with a human fallback in weaker languages.

### Model selection and migration

#### A customer asks which model they should use. Walk through how you decide.
Do not answer from a leaderboard. Ask, then test:
1. **Task and quality bar:** classification, extraction, long-document reasoning, coding, agents, multimodal? What accuracy is required?
2. **Constraints:** latency target (interactive vs. batch), volume and budget, context length, data residency, deployment route (direct API vs. cloud marketplace), compliance needs.
3. **Shortlist 2–3 candidates** across tiers: the most capable model, a mid-tier model, and a small fast model.
4. **Run the customer's eval set** on each with a reasonable prompt per model. Record quality per category, p50/p95 latency, and cost per task using real token counts.
5. **Pick the cheapest model that meets the bar**, or a combination: a small model for easy cases and a large one for hard cases.
6. **Plan for change:** models are updated and deprecated, so keep the model ID in config, keep the eval set, and re-run it when new versions ship.

Present the result as a small table (quality, latency, cost) with a recommendation and the reason.

#### A new model version has been released. Plan a customer's migration from the old version.
1. **Read the release and migration notes**: behaviour changes, new or removed parameters, pricing, and the deprecation date of the old version.
2. **Pin versions.** Make sure production uses a pinned model ID, not an alias that may change underneath.
3. **Run the existing eval set** on old and new with the current prompt. Compare per category, latency, output length and cost.
4. **Adjust prompts** only where scores dropped or behaviour changed; remove workarounds the new model no longer needs.
5. **Test the integration:** structured output parsing, tool calls, stop sequences, streaming, max-token limits, any changed defaults.
6. **Roll out gradually:** shadow traffic or a small percentage, compare online metrics (errors, user feedback, escalations), then ramp.
7. **Keep rollback ready** until the old version is retired, and record the decision with the eval results.

Give the customer a timeline that finishes well before the deprecation date.

#### After a migration, outputs are longer and the formatting changed. What do you do?
- **Quantify it:** compare output token distributions and format validity rates between versions on the eval set. Check whether quality also changed or only style.
- **Check for broken downstream code:** parsers, UI layouts, length limits (SMS, voice), cost estimates.
- **Fix at the right layer:**
  - Use structured output or tool/function schemas where the output feeds code; do not rely on prose formatting.
  - State the length and format explicitly ("Answer in at most three sentences", "Use no headings") and give a short example of the desired output.
  - Set a sensible max output token limit as a safety net, not as the main control (it truncates mid-sentence).
  - Check whether new reasoning or verbosity settings exist for the new version and set them deliberately.
- **Add format and length checks to the eval suite** so the next migration catches this automatically.
- Tell the customer what changed and why, so it does not look like a random regression.

#### When would you route part of a workflow to a smaller, cheaper model?
When the work is high-volume and some of it is easy. Common patterns:
- **Task split:** a small model does classification, extraction, routing, summarising short text or formatting; a larger model handles the reasoning-heavy step.
- **Cascade:** try the small model first; escalate to the larger model when confidence is low, validation fails, or the input matches known hard categories.
- **Pre-filtering:** a small model or plain code removes irrelevant documents before the expensive step.

Conditions: you have an eval set to prove the small model is good enough per category; the routing signal is reliable (a bad router silently sends hard cases to the weak model); and the savings justify the extra complexity. Measure the blended quality, latency and cost, not just the small model in isolation. Sometimes a single mid-tier model is simpler and nearly as cheap.

#### The customer wants extended reasoning ("thinking") turned on for every request. How do you respond?
Treat it as a hypothesis to test, not a yes or no.
- **Reasoning helps** on multi-step problems: complex analysis, maths, planning, code, hard classification with many rules, agentic tasks.
- **It costs** more output tokens (higher cost) and more time to first useful token, which can hurt interactive UX. On simple tasks it may add nothing.
- **Test it:** run the eval set with reasoning off and with one or two reasoning budgets. Compare quality per category against latency and cost.
- **Typical outcome:** enable it for the categories where it moves the score, or let a router decide; keep it off for simple, latency-sensitive steps.
- If the product is interactive, stream the answer and consider showing progress so the wait feels shorter.

Say it in their terms: "On your hard cases accuracy goes from 81% to 90%; on the easy 70% of traffic it is unchanged but responses take twice as long. So we turn it on for the hard path only."

#### The customer must consume models through a cloud marketplace (Bedrock, Vertex AI, Azure). How does that affect your advice?
Customers often do this to use existing cloud commitments, procurement, IAM, networking and compliance controls. Effects to check:
- **Model and feature availability:** a model version, region, or feature (some beta tools, batch, caching options, fine-tuning) may arrive later or differ from the provider's direct API. Check the platform's current docs, not memory.
- **Regions and quotas:** throughput limits and quota increase processes are set by the cloud platform. Plan capacity requests early.
- **API differences:** request format, SDK, auth (IAM roles instead of API keys), model ID naming. Keep code behind an adapter.
- **Pricing:** may differ in structure (on-demand vs. provisioned throughput) and is billed through the cloud.
- **Support path:** issues may need to go through the cloud provider's support.

Advice stays the same at the core (evals decide), but the plan must include availability checks and quota lead time.

### Cost and latency trade-offs

#### Estimate the monthly cost of a customer's workload and present it.
Use a simple model with stated assumptions; check current prices before quoting.

```
requests/month       R = 2,000,000  (e.g. 65k/day)
input tokens/request I = 3,000      (system prompt 1,500 + context 1,200 + user 300)
output tokens/req    O = 400
price per 1M input   Pin, per 1M output Pout (from the price list)

cost = R * (I * Pin + O * Pout) / 1,000,000
```
Then show sensitivity, because the assumptions are uncertain:
- With prompt caching on the 1,500-token static prefix, the cached part is billed at a much lower read rate.
- Routing 70% of requests to a smaller model.
- Batch processing for non-urgent work, typically at a large discount.
- Growth: what happens at 3x volume.

Present a table: baseline, optimised, worst case. Add cost per business unit (per ticket resolved, per document processed) and compare to the current cost of that unit; executives decide on that number, not on token prices. State what you need to validate: real token counts from a pilot.

#### The customer says the API is too expensive. Walk through the levers.
First measure where the money goes: tokens per request split into system prompt, context, history and output, by feature. Then, roughly in order of effort:
1. **Prompt caching** for repeated prefixes (system prompts, long documents, tool definitions).
2. **Trim input:** shorter system prompt, fewer or shorter retrieved chunks, summarise long conversation history, remove unused tool definitions.
3. **Control output:** ask for concise answers; structured output instead of prose; sensible max tokens.
4. **Right-size the model:** smaller model for easy steps, routing or cascades.
5. **Batch API** for anything not interactive.
6. **Application caching** of identical or near-identical requests where answers are stable.
7. **Fix waste:** retries on errors, duplicate calls, agents looping, calls for users who left.
8. **Commercial options:** committed-use or volume pricing — involve the account team.

Report savings against the eval set so nobody saves money by quietly losing quality.

#### A chat feature has a p95 latency of 12 seconds. How do you bring it down?
**Measure the breakdown first** with traces: retrieval, other API calls, time to first token, generation time, post-processing. Look at p95, not the average.
- **Perceived latency:** stream tokens to the UI; time to first token often matters more than total time.
- **Fewer output tokens:** generation time scales with output length. Ask for shorter answers.
- **Fewer sequential calls:** merge steps, run independent calls in parallel, remove unnecessary "rewrite the query" or "check the answer" steps from the hot path.
- **Smaller or faster model** for parts that do not need the largest one; turn off extended reasoning where it does not help.
- **Prompt caching** reduces time to first token for long repeated prefixes.
- **Faster retrieval:** cache embeddings, tune the index, cut re-ranking depth.
- **Tail causes:** retries after 429s or timeouts, cold starts, very long inputs. Fix rate-limit headroom and set timeouts with fallbacks.
Set a target (e.g. first token < 1.5 s, p95 complete < 6 s) and track it per release.

#### How do you restructure a prompt to get prompt-cache hits?
Prompt caching reuses work for an **identical prefix** of the request, so order content from most static to most dynamic:
1. Tool definitions and system instructions (static).
2. Large shared context: policy documents, product catalogue, a long document the user is asking several questions about.
3. Conversation history (grows, but earlier turns are stable).
4. The new user message and per-request data (dynamic) last.

Common mistakes that break hits: putting a timestamp, user name or request ID at the top of the system prompt; reordering tools or retrieved documents between calls; small edits to the static text. Also check the provider's rules: minimum cacheable length, cache lifetime (minutes, often refreshed on use), whether caching is automatic or needs explicit cache markers, and how cache writes and reads are priced. Measure hit rate from the usage fields in API responses and report the saving on cost and time to first token.

#### When would you move a workload to a batch API?
When results are not needed immediately. Batch APIs from major providers take a file or list of requests, process them asynchronously within a stated window (commonly up to 24 hours) and are priced at a significant discount (often around half price; check the current terms). Good fits:
- Back-filling or reprocessing a document archive.
- Nightly classification, tagging or summarisation jobs.
- Running large eval suites.
- Generating synthetic data or embeddings in bulk.

Not a fit: anything a user is waiting for, or pipelines that need a step's output within minutes to continue. Design for it: idempotent request IDs so you can match results, handling of partial failures (some requests error; resubmit those), and storage of results. Batch also avoids competing with interactive traffic for rate limits.

#### The customer is planning a launch. How do you explain rate limits and capacity planning to them?
Explain the units: limits are usually per minute on requests and on tokens (input and output, sometimes separately), set per organisation and model, and increase with usage tier or by agreement.

Then plan with numbers:
- Expected peak traffic, not the daily average (launch spikes can be 5–10x).
- Tokens per request × peak requests per minute = required tokens per minute. Compare with current limits.
- Request a limit increase or reserved/provisioned capacity **weeks ahead**; involve the account team.

Engineering side: retry 429s and 5xx with exponential backoff and jitter, respect retry-after headers, queue non-urgent work, degrade gracefully (smaller model, cached answers, "busy" message), and alert on 429 rate. Load test before launch at the expected peak. Write down who to call on launch day.

### Live LLM coding with API access

The snippets below use the Anthropic Python SDK as an example; the same structure works with any provider's SDK. Keep the model ID in an environment variable and check the current docs for exact parameters.

#### Live: classify 500 support tickets into categories with structured output, handle API errors, and report accuracy against labels.
Plan out loud: load data, define a schema, call concurrently with bounded parallelism, retry, score, print a confusion summary.

```python
import asyncio, csv, os, collections
from anthropic import AsyncAnthropic

CATS = ["billing", "bug", "account", "feature_request", "other"]
client = AsyncAnthropic(max_retries=5)          # SDK retries 429/5xx with backoff
MODEL = os.environ["MODEL"]
TOOL = {"name": "classify", "description": "Record the ticket category.",
        "input_schema": {"type": "object",
                         "properties": {"category": {"type": "string", "enum": CATS}},
                         "required": ["category"]}}

async def classify(text, sem):
    async with sem:
        try:
            r = await client.messages.create(
                model=MODEL, max_tokens=100, tools=[TOOL],
                tool_choice={"type": "tool", "name": "classify"},
                system="Classify the support ticket. Use 'other' if unsure.",
                messages=[{"role": "user", "content": f"<ticket>{text}</ticket>"}])
            return next(b.input["category"] for b in r.content if b.type == "tool_use")
        except Exception as e:                   # record, don't crash the batch
            return f"ERROR:{type(e).__name__}"

async def main():
    rows = list(csv.DictReader(open("tickets.csv")))   # columns: text,label
    sem = asyncio.Semaphore(10)
    preds = await asyncio.gather(*(classify(r["text"], sem) for r in rows))
    ok = sum(p == r["label"] for p, r in zip(preds, rows))
    print(f"accuracy {ok/len(rows):.1%}, errors {sum(p.startswith('ERROR') for p in preds)}")
    print(collections.Counter((r["label"], p) for p, r in zip(preds, rows) if p != r["label"]).most_common(10))

asyncio.run(main())
```
Then discuss: per-class precision/recall, the most common confusions, and whether the label set itself is ambiguous.

#### Live: implement a tool-use loop where the model answers questions by calling a lookup function.
```python
import json, os
from anthropic import Anthropic
client, MODEL = Anthropic(), os.environ["MODEL"]

ORDERS = {"A100": {"status": "shipped", "eta": "2026-10-24"}}
def lookup_order(order_id: str) -> dict:
    return ORDERS.get(order_id, {"error": "order not found"})

TOOLS = [{"name": "lookup_order",
          "description": "Get status and ETA for an order by its ID, e.g. A100.",
          "input_schema": {"type": "object",
                           "properties": {"order_id": {"type": "string"}},
                           "required": ["order_id"]}}]
HANDLERS = {"lookup_order": lookup_order}

def ask(question, max_steps=5):
    messages = [{"role": "user", "content": question}]
    for _ in range(max_steps):                       # hard cap on loop
        r = client.messages.create(model=MODEL, max_tokens=1024,
                                   tools=TOOLS, messages=messages)
        messages.append({"role": "assistant", "content": r.content})
        if r.stop_reason != "tool_use":
            return "".join(b.text for b in r.content if b.type == "text")
        results = []
        for b in r.content:
            if b.type == "tool_use":
                try:
                    out, err = HANDLERS[b.name](**b.input), False
                except Exception as e:                   # unknown tool or bad arguments
                    out, err = {"error": str(e)}, True
                results.append({"type": "tool_result", "tool_use_id": b.id,
                                "content": json.dumps(out), "is_error": err})
        messages.append({"role": "user", "content": results})
    return "Sorry, I could not complete that."

print(ask("Where is order A100?"))
```
Points to mention: step limit, errors returned to the model as tool results rather than raised, clear tool descriptions, and logging each step for debugging.

#### Live: build a minimal harness that compares two prompts on a dataset, running calls concurrently.
```python
import asyncio, json, os, statistics, time
from anthropic import AsyncAnthropic
client, MODEL = AsyncAnthropic(max_retries=5), os.environ["MODEL"]
PROMPTS = {"A": open("prompt_a.txt").read(), "B": open("prompt_b.txt").read()}
DATA = [json.loads(l) for l in open("eval.jsonl")]   # {"input":..., "expected":...}

def score(output, expected):                         # replace with task metric or judge
    return float(expected.lower() in output.lower())

async def run_one(name, item, sem):
    async with sem:
        t = time.perf_counter()
        r = await client.messages.create(model=MODEL, max_tokens=300, system=PROMPTS[name],
                                         messages=[{"role": "user", "content": item["input"]}])
        out = "".join(b.text for b in r.content if b.type == "text")
        return {"prompt": name, "score": score(out, item["expected"]),
                "latency": time.perf_counter() - t, "out_tokens": r.usage.output_tokens}

async def main():
    sem = asyncio.Semaphore(8)
    res = await asyncio.gather(*(run_one(p, d, sem) for p in PROMPTS for d in DATA))
    for p in PROMPTS:
        rs = [x for x in res if x["prompt"] == p]
        print(p, f"score={statistics.mean(x['score'] for x in rs):.2%}",
              f"p50_latency={statistics.median(x['latency'] for x in rs):.2f}s",
              f"avg_out_tokens={statistics.mean(x['out_tokens'] for x in rs):.0f}")
asyncio.run(main())
```
Extensions to offer: catch per-item API errors (as in the classification example) so one failure does not abort `gather`, save every output to a file for review, run each item several times to measure variance, add per-category breakdowns, and a paired comparison listing items where A and B disagree.

#### Live: stream a response to a user and handle a failure mid-stream.
```python
import os, anthropic
client, MODEL = anthropic.Anthropic(), os.environ["MODEL"]

# log_error and retry_once_or_fallback are app helpers, omitted for brevity
def stream_answer(messages, send):            # send() pushes text to the client (SSE/websocket)
    received = []
    try:
        with client.messages.stream(model=MODEL, max_tokens=800, messages=messages) as s:
            for text in s.text_stream:
                received.append(text)
                send(text)
            final = s.get_final_message()
            if final.stop_reason == "max_tokens":
                send("\n[Answer truncated]")
    except (anthropic.APIConnectionError, anthropic.APIStatusError) as e:
        partial = "".join(received)
        log_error(e, partial_len=len(partial))
        if not partial:
            return retry_once_or_fallback(messages)       # nothing shown yet: safe to retry
        send("\n[Connection lost. Tap retry to continue.]")  # don't silently restart
```
Explain the design choices: retry automatically only if nothing has been shown (otherwise the user sees duplicated or different text); if partial output was shown, either let the user retry or ask the model to continue from the partial text; set client and server timeouts; log the partial response and request ID; and on the UI side, mark incomplete answers clearly.

#### If AI coding assistants are allowed in the interview, how do you use them well?
Check the rules first and say what you will use. Then:
- **Stay in charge of the design.** State your plan before generating code; interviewers want to see your thinking, not the tool's.
- **Use it for boilerplate** (argument parsing, CSV loading, SDK call shapes) and keep the important logic (scoring, retry policy, data handling) under your control.
- **Read every line it produces** and explain it. Catch wrong SDK parameters or invented functions; check against the docs when unsure.
- **Run code early and often**; small working steps beat one large generated block.
- **Narrate verification:** "It wrote the loop without a step limit; I'm adding one."

At an AI company this round partly tests whether you are an effective, critical user of the tools you will recommend to customers.

### Cookbooks, reference implementations and enablement

#### You are asked to write a cookbook for a common pattern (for example RAG with citations). How do you structure it?
- **Start with the problem and the result:** one paragraph on when to use the pattern, and a short example of the final output (an answer with citations).
- **Prerequisites:** SDK version, API key, sample data that ships with the cookbook (small, licence-safe).
- **Steps in runnable order:** load and chunk documents, embed and index, retrieve, build the prompt with numbered sources, ask for citations in a structured format, validate that each citation points to a real retrieved chunk.
- **Evaluate:** a small eval cell (retrieval recall, citation accuracy) so readers learn to measure, not just build.
- **Production notes:** cost, latency, chunking choices and when to change them, failure modes, security (permissions on documents).
- **Keep it short and current:** one pattern per cookbook, minimal dependencies, pinned versions, and a CI job that runs it so it does not rot when the API changes.

Test it by giving it to someone who has never seen it and watching where they get stuck.

#### What makes a reference implementation that a customer's team will actually adopt?
- **It fits their stack:** their language, cloud, auth and deployment tooling. A perfect Python demo is ignored by a Java shop.
- **It solves their real use case** with their (sanitised) data, not a toy.
- **It is small and readable:** clear structure, few dependencies, no clever abstractions. They must own and change it after you leave.
- **Production basics included:** config not hard-coded, retries and timeouts, logging and tracing, secrets handling, tests, and an eval script with their data.
- **Documented decisions:** a README explaining why each choice was made and what to change at scale.
- **Handover built in:** pair-programming sessions while building it, a walkthrough recording, and a named owner on their side.

Measure adoption: is it in their repository, deployed, and being changed by their engineers a month later?

#### Several customers hit the same problem. How do you turn that into product feedback or a reusable asset?
1. **Collect evidence:** which customers, what they were trying to do, the workaround, and the cost to them (time, revenue at risk, blocked launches). Write it up in the format the product team uses.
2. **Separate the need from the requested solution.** Customers ask for a feature; the underlying need might be met better another way.
3. **Short term:** publish a reusable workaround — a cookbook, a code sample, a doc fix, an internal FAQ for other customer-facing engineers.
4. **Long term:** file a clear product request with a priority argument and link it to accounts; offer to be a design partner and to bring customers to user research.
5. **Close the loop:** tell customers what happened, and update the asset when the product changes.

Good field teams are a major source of product signal; showing you did this before is a strong interview point.

#### Run a technical workshop for a customer engineering team that is new to LLMs. What is the agenda?
Half day or full day, hands-on, built around **their** use case.
1. **Goals and use case (15 min):** what they want to build; agree success criteria.
2. **How models behave (30 min):** tokens, context window, non-determinism, hallucination, what models are good and bad at. Short, practical.
3. **Hands-on 1 — first calls and prompting (45 min):** API basics, system prompts, structure, examples, structured output, on their sample data.
4. **Hands-on 2 — evals (45 min):** build a 30-item eval set and a scoring script; compare two prompts. This is the habit that matters most.
5. **Patterns (30 min):** retrieval, tool use, when an agent is and is not appropriate.
6. **Production (30 min):** cost, latency, rate limits, safety, security, monitoring.
7. **Next steps (15 min):** a prototype plan, owners, and the next check-in.

Prepare notebooks and API keys in advance; a workshop that spends 40 minutes on setup fails.

### Enterprise adoption blockers

#### The customer's security team blocks the rollout over data retention and training on their data. How do you unblock it?
- **Meet them early and treat them as a partner,** not an obstacle. Ask for their specific questions or questionnaire.
- **Give documented facts, not reassurance:** the provider's commercial terms on training on customer data, data retention periods, zero-data-retention options where offered, encryption, sub-processors, certifications (e.g. SOC 2, ISO 27001), data residency and region options. Use the official trust/security documentation and involve the provider's security team for anything you are not sure of.
- **Address the architecture:** what data leaves their network, PII redaction before the API if needed, logging on their side, access controls, deployment via their cloud provider if that fits their controls better.
- **Reduce scope for a first phase:** start with a use case on less sensitive data to prove value while the full review continues.
- **Track open items** in a shared list with owners and dates.

Never overstate a guarantee to close a review; it will come back.

#### Legal is worried about hallucinations in customer-facing output. How do you address it?
Acknowledge the risk is real, then show controls and evidence:
- **Ground the answers:** retrieval from approved content only, instructions to answer only from sources, citations that link to the source, and "I don't know / let me connect you" when sources do not cover the question.
- **Constrain high-risk content:** no free generation of prices, legal terms, medical or financial advice; pull these from systems of record or templates.
- **Validate outputs:** check that cited passages exist, policy filters for forbidden claims, structured output for anything that becomes an action.
- **Human review** for high-stakes outputs, at least at first.
- **Measure:** a faithfulness/groundedness eval with a red-team set, results shared with legal, and production monitoring with sampling.
- **Disclose:** tell users they are talking to AI and how to reach a human.

Offer legal a role in setting thresholds and reviewing samples; that usually turns a blocker into an owner.

#### The POC worked but nobody uses it. How do you diagnose low adoption?
Look at data and talk to users.
- **Funnel:** how many people have access, tried it once, came back, use it weekly? Where is the drop?
- **Workflow fit:** is it a separate tool people must remember to open? Adoption improves dramatically when the feature sits inside the tool they already use (CRM, ticketing, IDE, email).
- **Trust:** one or two bad early answers can stop usage. Check early sessions for failures; add citations and confidence signals.
- **Value:** does it save meaningful time on a frequent task, or was the use case chosen because it demoed well?
- **Enablement and incentives:** were users trained? Do managers expect its use? Does it conflict with how they are measured?
- **Friction:** login, latency, permissions, missing data sources.

Interview 5–10 users, watch them work, fix the top two issues, and agree a usage metric with the sponsor.

#### An executive wants "AI everywhere" but has no use case. How do you pick the first one?
Run a short discovery, then score candidates.
1. **Collect ideas** from 4–6 teams: repetitive, text-heavy work, with volume (support, sales ops, finance ops, legal review, internal knowledge search, engineering).
2. **Score each** on business value (hours or money, measurable), feasibility (data available, clear right answer, integration effort), risk (customer-facing or internal, regulated or not), and sponsor (a named owner who wants it).
3. **Pick the first one** with high value, high feasibility, low-to-medium risk, an internal audience, and a baseline you can measure against. Internal knowledge assistants and support or operations triage are common first choices.
4. **Define a 4–8 week plan** with a success metric and a decision at the end.
5. **Keep a backlog** of the others so the executive sees a programme, not a one-off.

The first success buys trust and budget for harder use cases.

#### A customer asks for a capability the model or platform does not support. How do you respond?
- **Be honest and quick:** "That is not supported today." Do not hint at roadmap dates you cannot commit to.
- **Understand the underlying need:** what are they trying to achieve, and how important is it to the deal or launch?
- **Offer workarounds:** e.g. combine features, add a deterministic step in their code, use a different model for that step, or restructure the task. Show a small prototype if it is feasible.
- **Log it properly** as product feedback with business impact, and tell the customer you have done so.
- **Follow up** when anything changes.

Interviewers check that you do not over-promise, that you look for a way to still deliver value, and that you route the signal back to product.

### Presentation and behavioral (role-specific)

#### Presentation round: present your take-home to a mixed technical and executive audience.
Structure (about 20 minutes plus questions):
1. **Problem and outcome (2 min):** what the user needs, and the result in one sentence with the headline metric.
2. **Demo (4 min):** one happy path and one hard case. Rehearse; have a recording as backup.
3. **Approach (5 min):** a simple architecture diagram, the key choices and alternatives you rejected.
4. **Evaluation (4 min):** how you built the eval set, results table, error analysis with real examples of failures.
5. **Production view (3 min):** cost per request, latency, risks and mitigations, what changes at 100x scale.
6. **Next steps (2 min):** what you would do with another week.

Keep technical depth in backup slides; go there when engineers ask. Speak to the executive in outcomes and risk, to engineers in trade-offs. Admit weaknesses before they find them. See also [06-ai-llm section J](../../topics/06-ai-llm/ANSWERS-part2.md) for take-home follow-ups.

#### Tell me about a time you improved an AI or ML system's quality through measurement.
**What they look for:** you defined "better" before changing things, built or used an evaluation, made changes based on error analysis, and can quote numbers.

**STAR outline:**
- **Situation:** [system, users, what was wrong, how people noticed].
- **Task:** [your responsibility; the target].
- **Action:** [how you built the eval set; the error categories you found; the changes you made in priority order; how you validated them].
- **Result:** [metric before → after; business effect; what process stayed in place].

**Example (adapt to your own experience):** "Our internal support-answer assistant had complaints about wrong answers. I sampled 200 recent questions, had two support leads label them, and found 45% of failures came from retrieval missing the right article, not from the model. I added hybrid search and re-ranking and rewrote the prompt to say when sources did not cover the question. Correct-answer rate on the set went from 68% to 84%, and the eval became the gate for every later change."

#### Tell me about a time you told a customer the technology was not the right fit.
**What they look for:** honesty over short-term revenue, evidence-based reasoning, and that you kept the relationship and found another path.

**STAR outline:**
- **Situation:** [customer, what they wanted the technology to do].
- **Task:** [your role; why the decision mattered].
- **Action:** [the test or evidence that showed the gap; how you explained it; the alternative you proposed].
- **Result:** [what the customer did; effect on the relationship; what you learned].

**Example (adapt to your own experience):** "A customer wanted an LLM to calculate insurance premiums directly from free-text applications. In a one-week test the model extracted fields well but the calculations were wrong in about 6% of cases, which was unacceptable. I showed the error examples and proposed using the model only to extract fields into a structured form, with their existing rules engine doing the calculation. They agreed; the project launched with that design and the customer later expanded the use of extraction to other forms."

---

## Prep checklist

1. Build two or three small projects on the target company's API: structured-output classification, a tool-use loop, and an eval harness comparing two prompts. Be able to write each from memory in 20–30 minutes.
2. Read the company's prompt engineering guide, cookbooks and the release / migration notes for its last two model versions; note the behavioural changes.
3. Practise the prompt-optimisation exercise: take a weak prompt, write 20 test cases, improve it in 30 minutes with before/after numbers. Repeat with two different tasks.
4. Memorise the current price list structure, batch and caching discounts, rate-limit concepts and deprecation policy for your target provider (check the live docs the week of the interview).
5. Prepare a one-page cost model template and practise a cost estimate out loud with stated assumptions.
6. Rehearse a 20-minute take-home presentation with a demo, eval results and a failure analysis.
7. Prepare stories on: improving quality with measurement, saying no to a customer, unblocking a security/legal review, and turning field feedback into product change ([10-behavioral](../../topics/10-behavioral/ANSWERS.md)).
8. Re-read [06-ai-llm](../../topics/06-ai-llm/README.md) sections B, E, G and H and answer each question out loud.
9. Prepare questions about how the team splits time between customers, assets and product feedback ([14-questions-to-ask](../../topics/14-questions-to-ask/README.md)).
