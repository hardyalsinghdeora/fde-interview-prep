# Study Plan — Technical Product Manager

For engineers and forward deployed engineers preparing for technical, platform, API or AI product manager interviews; assumes about 10–12 hours a week for six weeks, or a two-week crash plan if an interview is already booked.

## Before you start

Spend 30 minutes rating yourself honestly on each area. Skip or skim the weeks where you are already a 4 or 5, and spend the saved time on your weakest two areas.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Product sense / product design | | You can take "design X for Y" and in 25 minutes pick a user segment, rank their problems, propose and prioritise solutions, and name a metric, without rambling. |
| Prioritisation and roadmaps | | You can apply RICE, MoSCoW and cost of delay to a real backlog and explain what you would not build and why. |
| Metrics and analytics | | You can define a North Star with input metrics and guardrails, and root-cause a metric drop in a structured order. You can write a basic SQL query with joins and group by. |
| Experimentation | | You can explain sample size, power, peeking and when not to A/B test. |
| Execution and PRDs | | You have written a PRD with goals, non-goals, testable acceptance criteria and a rollout plan, and can handle a slipping launch. |
| Technical depth (APIs, architecture) | | You can draw a system at a high level, discuss scale and reliability trade-offs, and explain API versioning and deprecation. |
| AI product knowledge | | You can explain RAG, fine-tuning, agents and evals in plain English, define quality for an LLM feature, and discuss AI pricing. |
| Strategy and estimation | | You can size a market in five minutes with clear assumptions and argue build versus buy. |
| Behavioural stories | | You have six or more STAR stories with numbers, including saying no, failure and influence without authority. |
| Customer research | | You can run a non-leading discovery interview and synthesise themes. |

## 6-week plan

### Week 1 — The PM role, product sense foundations and your stories

**Goals:** understand what PM interviewers score, learn one product sense structure, and start your story bank.

- [ ] Read the role overview and loop in [this role file](./README.md) and the comparison with other roles in [the roles index](../README.md).
- [ ] Read the "Role and motivation" and "Product sense and design" model answers in [this file's model answers](./README.md#model-answers).
- [ ] Read [04 Decomposition questions](../../topics/04-decomposition/README.md) section E and its [answers](../../topics/04-decomposition/ANSWERS.md).
- [ ] Read [01 Recruiter & motivation answers](../../topics/01-recruiter-and-motivation/ANSWERS.md) and adapt "why this role" into "why product management".
- [ ] Read [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md) and mark which apply.
- [ ] Start your stories in [the story bank](../../my-prep/STORY-BANK.md) using [00 Overview story checklist](../../topics/00-overview/README.md).

**Hands-on deliverables:**
- Write a two-minute "why PM, why now" answer and record yourself.
- Do a written product teardown (one page) of a product you use daily: users, jobs, three problems, one improvement with a metric.

**Mock interview:** one 30-minute product sense mock with a peer ("improve a product you use").

**End of week check:** you can answer "why PM?" in two minutes and run a product sense question end to end using a consistent structure.

### Week 2 — Prioritisation, roadmaps and customer discovery

**Goals:** be fluent with prioritisation frameworks and discovery techniques, and be able to say no well.

- [ ] Read "Prioritisation and roadmaps" and "Customer research and discovery" in [this file's model answers](./README.md#model-answers).
- [ ] Read [09 Customer scenarios](../../topics/09-customer-scenarios/README.md) sections A, D and E with [answers](../../topics/09-customer-scenarios/ANSWERS.md).
- [ ] Read the prioritisation and conflict sections of [10 Behavioral answers](../../topics/10-behavioral/ANSWERS.md).
- [ ] Read *The Mom Test* (short book) or a summary of it.

**Hands-on deliverables:**
- Take a backlog of ten features (from your current work, sanitised, or invented) and score them with RICE; write a one-page quarterly plan with what is in, what is out and why.
- Write a 30-minute customer discovery interview script with eight non-leading questions, and run it with one friend or colleague about a real problem they have.

**Mock interview:** a 45-minute execution / prioritisation mock: "ten features, three engineers, one quarter" and "largest customer demands a niche feature".

**End of week check:** you can apply three frameworks to the same backlog and explain how judgment overrides the scores; you can describe how you would handle field requests without becoming a feature factory.

### Week 3 — Metrics, analytics and experimentation

**Goals:** handle analytical rounds confidently.

- [ ] Read "Metrics and analytics" in [this file's model answers](./README.md#model-answers).
- [ ] Read [07 Data & SQL questions](../../topics/07-data-and-sql/README.md) sections A and B and the matching [answers](../../topics/07-data-and-sql/ANSWERS.md).
- [ ] Learn the B2B SaaS metric definitions: ARR, net and gross revenue retention, CAC payback, activation, cohort retention.
- [ ] Learn A/B testing basics: hypothesis, randomisation unit, sample size, power, sample ratio mismatch, peeking.

**Hands-on deliverables:**
- Draw metric trees (North Star, inputs, guardrails) for three products: a payments API, a team chat tool and an AI support assistant.
- Write five SQL queries on any public sample dataset: daily active users, 7-day retention by cohort, funnel conversion, top accounts by usage, week-over-week change.

**Mock interview:** two 30-minute analytical mocks: "metric dropped 10%" and "choose success metrics for feature X".

**End of week check:** you can investigate a metric drop in a clear order (data, external, segment, funnel, internal changes) and design an A/B test, including when not to run one.

### Week 4 — Execution, PRDs, launches and technical depth

**Goals:** show you can turn a problem into shippable work and hold your own in a technical round.

- [ ] Read "PRDs and execution" and "API and platform products" in [this file's model answers](./README.md#model-answers).
- [ ] Read [03 System design questions](../../topics/03-system-design/README.md) section A and skim [answers](../../topics/03-system-design/ANSWERS.md) for structure.
- [ ] Read [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/README.md) section A with [answers](../../topics/08-cloud-devops-integration/ANSWERS.md).
- [ ] Study one public API's documentation, changelog and versioning policy (for example Stripe or Twilio).

**Hands-on deliverables:**
- Write a full PRD (two pages maximum) for one feature: problem and evidence, goals and non-goals, metrics, prioritised requirements with Given/When/Then acceptance criteria, rollout plan, risks.
- Write a one-page developer experience critique of the API you studied, with three prioritised improvements and how you would measure them.

**Mock interview:** a 45-minute technical PM mock: "design the system behind feature X at a high level, then discuss trade-offs and how you would version its API".

**End of week check:** you can write a PRD an engineer could build from and explain an architecture and an API deprecation plan without hand-waving.

### Week 5 — AI product management and strategy

**Goals:** be convincing on AI product questions and strategy/estimation.

- [ ] Read "AI product management" and "Strategy and business" in [this file's model answers](./README.md#model-answers).
- [ ] Read [06 AI / LLM questions](../../topics/06-ai-llm/README.md) sections A, C, D, E and G, with [answers](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md).
- [ ] Read [11 Company-specific](../../topics/11-company-specific/README.md) for any AI company you target.

**Hands-on deliverables:**
- Build a small LLM feature (for example, answering questions over ten documents) using any model API or no-code tool, with a 30-example eval set and a measured cost per answer. Write one page on its quality, failure modes and launch threshold.
- Draft a pricing model for that feature: per seat, usage-based and hybrid options, with margin at light, median and heavy usage.

**Mock interview:** a 45-minute AI PM mock: "define quality and launch plan for an AI agent feature", followed by one market sizing question.

**End of week check:** you can explain prompting versus RAG versus fine-tuning with a decision order, define an eval-based quality bar, and reason about AI pricing and margins.

### Week 6 — Behavioural polish, company research and full mock loops

**Goals:** convert preparation into consistent performance.

- [ ] Re-read the "Behavioral" model answers in [this file](./README.md#model-answers) and [10 Behavioral answers](../../topics/10-behavioral/ANSWERS.md).
- [ ] Finalise six to eight stories in [the story bank](../../my-prep/STORY-BANK.md) with numbers.
- [ ] Prepare questions from [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md).
- [ ] Use the target company's product; read its docs, changelog and recent announcements.

**Hands-on deliverables:**
- A one-page "improvement idea" for the target company's product, with user, problem, solution, metric and risks, ready to discuss in the final round.
- A recorded 10-minute walk-through of your PRD or AI feature as if presenting to a hiring panel.

**Mock interview:** two full mock loops (four 45-minute rounds each: product sense, analytical, execution/technical, behavioural) with different interviewers.

**End of week check:** you can do four rounds back to back, keep to time, and finish each answer with a clear recommendation.

## 2-week crash plan

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and motivation | Read the role file; write and record "why PM"; list your top six stories | [README](./README.md), [01 answers](../../topics/01-recruiter-and-motivation/ANSWERS.md), [story bank](../../my-prep/STORY-BANK.md) |
| 2 | Product sense | Learn the structure; do two product sense prompts out loud, timed | [Model answers](./README.md#model-answers), [04 Decomposition](../../topics/04-decomposition/ANSWERS.md) |
| 3 | Prioritisation | Frameworks; score a ten-item backlog; practise "say no" | [Model answers](./README.md#model-answers) |
| 4 | Metrics | North Star and metric trees for three products; two "metric dropped" drills | [Model answers](./README.md#model-answers), [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md) |
| 5 | Experimentation and SQL | A/B testing basics; five SQL queries | [07 Data & SQL](../../topics/07-data-and-sql/README.md) |
| 6 | Execution | Write a short PRD; practise "launch at risk" | [Model answers](./README.md#model-answers) |
| 7 | Mock day | One product sense and one analytical mock with a peer; review recordings | [COMMON-QUESTIONS](../COMMON-QUESTIONS.md) |
| 8 | Technical depth | High-level system design; API versioning and developer experience | [03 System design](../../topics/03-system-design/ANSWERS.md), [08 Integration](../../topics/08-cloud-devops-integration/ANSWERS.md) |
| 9 | AI product | RAG, fine-tuning, agents, evals; AI quality and pricing answers | [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) |
| 10 | Strategy | Two market sizings; build versus buy; competition | [Model answers](./README.md#model-answers) |
| 11 | Behavioural | Rehearse six stories out loud, two minutes each | [10 Behavioral](../../topics/10-behavioral/ANSWERS.md) |
| 12 | Company research | Use the product; write one improvement idea; prepare questions | [11 Company-specific](../../topics/11-company-specific/README.md), [14 Questions](../../topics/14-questions-to-ask/ANSWERS.md) |
| 13 | Full mock loop | Four rounds back to back with feedback | This plan's mock schedule |
| 14 | Light review | Re-read notes and weakest answers; rest | [Prep checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute day:

| Time | Activity |
|---|---|
| 10 min | Warm-up: one estimation or metric definition from memory |
| 30 min | Study the week's topic (read model answers and shared sections, take notes in your own words) |
| 30 min | Practise out loud: one product sense, analytical or execution question with a timer; record it |
| 15 min | Review the recording against the structure; write one improvement |
| 5 min | Add to your story bank or plan tomorrow |

On weekends, use one longer block (two to three hours) for the week's hands-on deliverable and a mock.

## Mock interview schedule

| Weeks | Mock | Frequency | With whom |
|---|---|---|---|
| 1–2 | Product sense, prioritisation | Once a week each | A peer or a PM friend; record yourself |
| 3 | Analytical / metrics | Twice | A peer, or a paid PM mock platform such as Exponent or interviewing.io (check current offerings) |
| 4 | Technical PM / system design | Once | An engineer friend who can push on trade-offs |
| 5 | AI PM and estimation | Once | A peer working in AI, or a mock platform |
| 6 | Full loop (four rounds) | Twice | Different people each time; at least one experienced PM |
| Throughout | Behavioural | Weekly, 20 minutes | A friend, or Pramp-style peer practice (check current offerings), or recording |

After every mock, write down the two biggest gaps and fix them before the next one.

## Fast track for the repo owner's profile

- **Skim:** API design, contract-first integrations, platform services and data migration topics. As a principal engineer who designed an API gateway with JWT authentication and rate limiting, an aggregation layer, and contract-first integrations, you can go fast through the "API and platform products" answers and the technical PM round.
- **Skim:** execution and delivery. You already run sprint planning, release plans, monthly programme reports and onboarding plans; focus only on the PM framing (outcomes and metrics rather than delivery dates).
- **Biggest gap: product sense and discovery.** Most of your stakeholder work has been with internal teams (sales, finance, academic teams). Spend extra time in weeks 1–2 on product sense prompts and non-leading customer interviews, and practise speaking about users outside your company.
- **Biggest gap: metrics and experimentation.** Practise North Star metric trees, A/B testing and "metric dropped" questions in week 3; these are rarely part of an engineering role.
- **Gap: AI product depth.** Your AI exposure is a contribution to a speech-to-text provider evaluation; do the week 5 LLM feature with evals and a cost model so you can speak from hands-on experience.
- **Product sense and pricing round:** your order, subscription and commerce platform work (SKU catalogue, discount approval rules engine, invoicing and payments tied to a cash-collection metric) maps well to product sense, B2B metrics and pricing questions. Practise describing it as "the problem, the users, the metric we moved".
- **Strategy / build versus buy round:** use the speech-to-text provider evaluation with its buy-versus-build cost model and data-protection (India DPDP Act) constraints; be clear that you contributed to it rather than led it alone.
- **Technical round:** use the bidirectional legacy MySQL and new Postgres bridge across six product lines (idempotency guards, ordering constraints, reconciliation) as your trade-off story, and the PHP 5.3 to 7.3 user migration for "how do you change a platform without breaking users", which maps to API deprecation questions.
- **Behavioural round:** leading a squad of about four engineers and tech-leading business-critical modules gives stories for influence, prioritisation and saying no; draft them in [the story bank](../../my-prep/STORY-BANK.md).
- **Prepare a confident answer to "why PM?"** that explains a deliberate move from principal engineer to product, not a step away from engineering.

## Resources

- *Inspired* and *Empowered* by Marty Cagan (product discovery and product teams).
- *Cracking the PM Interview* by Gayle Laakmann McDowell and Jackie Bavaro (interview formats and practice questions).
- *Decode and Conquer* by Lewis C. Lin (product sense frameworks).
- *The Mom Test* by Rob Fitzpatrick (customer interviews).
- *Continuous Discovery Habits* by Teresa Torres (discovery and opportunity solution trees).
- *Trustworthy Online Controlled Experiments* by Ron Kohavi, Diane Tang and Ya Xu (A/B testing).
- *Escaping the Build Trap* by Melissa Perri (outcome-based product management).
- *Accelerate* by Nicole Forsgren, Jez Humble and Gene Kim (DORA metrics, for platform PM).
- Stripe API documentation and versioning guide (https://docs.stripe.com) as an example of strong developer experience.
- Exponent (paid PM interview course and mock platform); Lenny's Newsletter and podcast (product management practice).
- Model provider documentation on evaluations and prompt engineering, such as the Anthropic documentation (https://docs.anthropic.com) and the OpenAI platform documentation.
