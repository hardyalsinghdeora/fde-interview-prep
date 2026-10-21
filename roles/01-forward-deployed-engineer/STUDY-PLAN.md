# Study Plan — Forward Deployed Engineer

For software engineers preparing for an FDE / FDSE loop; assumes about 10–12 hours a week for six weeks, or a full two weeks of evenings if the interview is already booked.

Read the role overview first: [README.md](./README.md#the-role) and the [typical interview loop](./README.md#typical-interview-loop). FDE loops are the broadest in this repo: coding, applied system design, an LLM build, decomposition, debugging and customer role-play can all appear in one loop. This plan covers all of them, weighted towards what most often decides the offer: working code under changing requirements and calm customer judgment.

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim any area you rate 4 or 5; put the time saved into your two lowest areas.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Practical coding (incremental, API-style) | | You can build a working solution in 45 minutes, then extend it twice as requirements change, with tests, without rewriting from scratch. |
| Classic DS&A | | You solve most medium problems on hashing, graphs, intervals and heaps in under 30 minutes and state complexity unprompted. |
| Applied system design | | You can design for a customer with legacy systems, on-prem data or regulated data, and say what you would ship in week one versus month three. |
| Data engineering and SQL | | You write window functions and joins from memory and can explain how you would make a messy nightly load idempotent and reconciled. |
| Integration, auth and cloud | | You can explain OAuth flows, webhooks, retries with backoff and idempotency keys, and debug a 401 versus a 403 versus a timeout. |
| LLM / RAG / agents / evals | | You have built an LLM app with tools and an eval set, and can explain how you measured whether a change helped. |
| Decomposition | | Given "help a hospital reduce waits", you clarify the user and decision, produce a phased plan and name the data you need, in 30 minutes. |
| Customer communication and demo | | You can run a 10-minute demo while being interrupted, and say "no, but here is what we can do" without losing the room. |
| Behavioral stories | | You have 8–10 STAR stories written, each under two minutes, covering ownership, ambiguity, failure, conflict and customer impact. |

## 6-week plan

Assumes ~10–12 hours a week. Each week ends with a mock interview and a short check. If a check fails, carry that item into the next week rather than skipping ahead.

### Week 1 — Role fit, story bank and coding warm-up

**Goals:** get through the recruiter screen cleanly, start the story bank, and restart daily coding.

- [ ] Read the whole role overview and loop: [README.md — The role](./README.md#the-role), [Typical interview loop](./README.md#typical-interview-loop).
- [ ] Work through Tier 1 of [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md#tier-1--asked-in-almost-every-loop).
- [ ] Study motivation and logistics answers: [01 Recruiter and motivation — answers](../../topics/01-recruiter-and-motivation/ANSWERS.md#motivation--role-fit) and [logistics](../../topics/01-recruiter-and-motivation/ANSWERS.md#logistics).
- [ ] Read the FDE travel questions and model answers: [Travel, on-site work and field conditions](./README.md#travel-on-site-work-and-field-conditions-1).
- [ ] Fill the story bank checklist: [00 Overview — story bank](../../topics/00-overview/ANSWERS.md#story-bank-checklist-prepare-one-story-for-each) and the [coverage map](../../topics/00-overview/ANSWERS.md#coverage-map-which-story-to-use-when).
- [ ] Do five warm-up problems from [02 Coding — DS&A topics](../../topics/02-coding/ANSWERS-dsa-topics.md#arrays--strings) (arrays, strings, hashing).

**Deliverables:**
1. A recorded 60-second "Why FDE, not SWE?" answer plus an honest travel answer (watch it back once).
2. Eight STAR stories drafted in your own story bank ([../../my-prep/STORY-BANK.md](../../my-prep/STORY-BANK.md)).

**Mock:** a 30-minute recruiter-screen mock with a friend: "tell me about yourself", "why FDE", travel, salary expectations.

**End of week check:** you can answer "why FDE" in under 60 seconds without notes, and you can name which story you would use for ownership, ambiguity and failure.

### Week 2 — Practical coding and messy data

**Goals:** be fluent at incremental, API-style problems and at the data work FDEs do daily.

- [ ] Work the reported FDE problems in [02 Coding — A. Reported FDE coding problems](../../topics/02-coding/ANSWERS.md#a-reported-fde-coding-problems); do at least six, timed.
- [ ] Do two low-level design problems from [02 Coding — C. Object-oriented / low-level design](../../topics/02-coding/ANSWERS.md#c-object-oriented--low-level-design-code-it).
- [ ] Practise the questions in [02 Coding — D. Questions asked during/after coding](../../topics/02-coding/ANSWERS.md#d-questions-asked-duringafter-coding).
- [ ] SQL reps: [07 Data and SQL — A. SQL coding prompts](../../topics/07-data-and-sql/ANSWERS.md#a-sql-coding-prompts).
- [ ] Pipelines: [07 Data and SQL — D. Pipelines and data quality](../../topics/07-data-and-sql/ANSWERS.md#d-pipelines--data-quality) and [F. Pandas / Python data wrangling](../../topics/07-data-and-sql/ANSWERS.md#f-pandas--python-data-wrangling).
- [ ] If interviewing at Palantir or Scale, add [graphs](../../topics/02-coding/ANSWERS-dsa-topics.md#graphs) and [intervals](../../topics/02-coding/ANSWERS-dsa-topics.md#intervals).

**Deliverable:** a small ingestion script that pulls from a paginated public API (or a messy CSV), deduplicates, loads into SQLite or Postgres idempotently, and has five tests. Push it to a public repo; this starts your portfolio.

**Mock:** a 60-minute incremental coding mock where the partner adds a new requirement every 15 minutes.

**End of week check:** you can finish an incremental problem with working code and tests, and explain how you would rerun a failed load without duplicating rows.

### Week 3 — Applied system design and integration

**Goals:** design inside a customer's real constraints, not a greenfield whiteboard.

- [ ] Work at least four prompts from [03 System design — A. Applied / customer-context prompts](../../topics/03-system-design/ANSWERS.md#a-applied--customer-context-design-prompts).
- [ ] Prepare the follow-ups in [03 System design — D. Follow-up questions to expect](../../topics/03-system-design/ANSWERS.md#d-follow-up-questions-to-expect).
- [ ] Integration and security: [08 — A. APIs and integration](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration) and [B. Authentication, authorization and security](../../topics/08-cloud-devops-integration/ANSWERS.md#b-authentication-authorization--security).
- [ ] Operations: [08 — E. CI/CD and operations](../../topics/08-cloud-devops-integration/ANSWERS.md#e-cicd--operations).
- [ ] Read the FDE model answers on [Prototype-to-production ownership](./README.md#prototype-to-production-ownership-1) and [Building on the company's platform](./README.md#building-on-the-companys-platform-1).
- [ ] Rehearse the [customer architecture whiteboard exercise](./README.md#exercise-customer-architecture-whiteboard-sketch-how-our-platform-fits-into-this-customers-existing-stack).

**Deliverable:** a two-page design doc for one applied prompt (for example an on-prem, regulated customer), with a diagram, a week-one versus month-three scope, failure modes and a monitoring plan.

**Mock:** a 45-minute applied system design mock where the partner plays the customer's head of security and keeps adding constraints.

**End of week check:** you can draw your design in 15 minutes and defend three trade-offs, including what you would cut to ship in two weeks.

### Week 4 — Build an LLM application on the target company's API

**Goals:** have one real, demoable LLM project with tools and an eval set; this is the most common take-home at AI-lab FDE roles.

- [ ] Fundamentals refresh: [06 AI/LLM — C. RAG](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation) and [D. Agents and tool use](../../topics/06-ai-llm/ANSWERS.md#d-agents-and-tool-use).
- [ ] The most-asked area: [06 AI/LLM — E. Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important).
- [ ] Production concerns: [06 part 2 — G. Production LLM engineering](../../topics/06-ai-llm/ANSWERS-part2.md#g-production-llm-engineering) and [H. Safety and security](../../topics/06-ai-llm/ANSWERS-part2.md#h-safety-security-and-responsible-ai).
- [ ] Read the reported take-homes in [06 part 2 — J. Take-home / build exercises](../../topics/06-ai-llm/ANSWERS-part2.md#j-take-home--build-exercises-reported).
- [ ] Read the FDE model answer for the [regulated-workflow LLM agent](./README.md#openai--anthropic-a-customer-wants-an-llm-agent-inside-a-regulated-workflow-walk-through-your-build-on-the-api-from-first-prompt-to-production) and the [take-home defence exercise](./README.md#exercise-take-home-build-on-our-api-then-defend-your-design-choices-to-an-engineer-playing-the-customers-cto).
- [ ] Study [03 System design — B. AI / LLM prompts](../../topics/03-system-design/ANSWERS.md#b-ai--llm-system-design-prompts) (two prompts).

**Deliverable:** a RAG or tool-using agent over a small real document set, with 25–40 labelled eval cases, a script that reports accuracy and latency, and a README that lists known failure modes. Keep it to what you can explain line by line.

**Mock:** a 45-minute take-home defence: a partner reads your repo for 10 minutes, then plays the customer's CTO asking "why this design?", "what breaks at 100x?", "how do you know it works?".

**End of week check:** you can show a before/after eval number for one change you made and explain why the change helped.

### Week 5 — Decomposition, learning round and live debugging

**Goals:** handle the vague-problem rounds and the "something is broken, fix it" rounds.

- [ ] Do four decomposition prompts out loud, timed, from [04 Decomposition — C. Commercial / enterprise](../../topics/04-decomposition/ANSWERS.md#c-commercial--enterprise) and [D. AI-company prompts](../../topics/04-decomposition/ANSWERS.md#d-ai-company-decomposition-prompts); then rehearse the [push-backs](../../topics/04-decomposition/ANSWERS.md#f-typical-interviewer-follow-ups--push-backs).
- [ ] Palantir candidates: add [A. Public sector](../../topics/04-decomposition/ANSWERS.md#a-public-sector--government--defense) and the [learning round](../../topics/05-learning-round/ANSWERS.md#possible-topics-you-could-be-taught).
- [ ] Debugging: [12 — A. Live debugging round formats](../../topics/12-debugging-troubleshooting/ANSWERS.md#a-live-debugging-round-formats), [B. Production troubleshooting](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios) and [C. LLM-specific troubleshooting](../../topics/12-debugging-troubleshooting/ANSWERS.md#c-llm-specific-troubleshooting).
- [ ] Rehearse the [live triage exercise](./README.md#exercise-live-triage-a-customer-integration-broke-overnight-diagnose-it-with-incomplete-information-while-keeping-the-customer-informed).
- [ ] Read [Field-to-product feedback](./README.md#field-to-product-feedback-1) and [Hack versus productize](./README.md#hack-versus-productize-1).

**Deliverables:**
1. A sample product feedback ticket for a gap you hit in Week 4's build (problem, evidence, customers affected, proposed fix).
2. A one-page pilot summary for an imaginary six-week pilot ([exercise](./README.md#exercise-write-a-one-page-summary-of-a-six-week-pilot-for-the-customers-executive-sponsor)).

**Mock:** two back-to-back mocks: a 40-minute decomposition and a 30-minute live triage where the partner feeds you logs one at a time.

**End of week check:** in decomposition you clarify the user and decision before proposing anything; in triage you state a hypothesis, test it, and send a customer update within the first 10 minutes.

### Week 6 — Customer role-play, demo and full loop

**Goals:** close the customer-facing gap and rehearse the full day.

- [ ] Customer scenarios: [09 — B. Difficult situations](../../topics/09-customer-scenarios/ANSWERS.md#b-difficult-situations), [C. Communication](../../topics/09-customer-scenarios/ANSWERS.md#c-communication) and [role-play tips](../../topics/09-customer-scenarios/ANSWERS.md#role-play-tips).
- [ ] FDE-only situations: [Technical ownership of a customer account](./README.md#technical-ownership-of-a-customer-account-1) and [Multi-customer context switching](./README.md#multi-customer-context-switching-1).
- [ ] Rehearse the [live demo exercise](./README.md#exercise-live-demo-present-a-working-prototype-to-a-mock-customer-executive-who-interrupts-with-objections).
- [ ] Company research: [11 Company-specific](../../topics/11-company-specific/ANSWERS.md) for your target, plus the [research checklist](../../topics/11-company-specific/ANSWERS.md#company-research-checklist-do-for-every-company).
- [ ] Behavioral polish: [10 — Ownership and impact](../../topics/10-behavioral/ANSWERS.md#ownership--impact), [Customer focus](../../topics/10-behavioral/ANSWERS.md#customer-focus), [Hiring-manager round topics](../../topics/10-behavioral/ANSWERS.md#hiring-manager-round-topics).
- [ ] Prepare questions: [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md#about-customers).
- [ ] Tick off the role's [Prep checklist](./README.md#prep-checklist).

**Deliverable:** a 10-minute recorded demo of your Week 4 project for a non-technical executive, plus a backup recording and a list of three likely objections with answers.

**Mock:** a full mock loop over one or two days: practical coding (60), applied design (45), customer role-play with demo (45), behavioral (45). Use different partners if you can.

**End of week check:** you can give the demo while being interrupted and still finish on time with a clear next step.

## 2-week crash plan

For an interview already scheduled. Assumes about 2 hours a day; protect the mocks.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role fit | "Why FDE" and travel answers recorded; skim the loop | [README loop](./README.md#typical-interview-loop), [01 answers](../../topics/01-recruiter-and-motivation/ANSWERS.md#motivation--role-fit) |
| 2 | Stories | Write 6 STAR stories; map them to rounds | [Story bank checklist](../../topics/00-overview/ANSWERS.md#story-bank-checklist-prepare-one-story-for-each), [my story bank](../../my-prep/STORY-BANK.md) |
| 3 | Practical coding | Two reported FDE problems, timed | [02 — A](../../topics/02-coding/ANSWERS.md#a-reported-fde-coding-problems) |
| 4 | Practical coding | One more problem + one low-level design | [02 — C](../../topics/02-coding/ANSWERS.md#c-object-oriented--low-level-design-code-it) |
| 5 | Applied design | Two applied prompts, 45 minutes each | [03 — A](../../topics/03-system-design/ANSWERS.md#a-applied--customer-context-design-prompts) |
| 6 | Integration and data | Auth, retries, idempotency; two SQL prompts | [08 — A](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration), [07 — A](../../topics/07-data-and-sql/ANSWERS.md#a-sql-coding-prompts) |
| 7 | Mock day | Coding mock + design mock | [Mock schedule](#mock-interview-schedule) |
| 8 | LLM build | Start a minimal tool-using app on the target API | [06 — D](../../topics/06-ai-llm/ANSWERS.md#d-agents-and-tool-use) |
| 9 | Evals | Add 20 eval cases and a scoring script | [06 — E](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important) |
| 10 | Decomposition | Two prompts out loud, timed | [04 — C](../../topics/04-decomposition/ANSWERS.md#c-commercial--enterprise), [04 — F](../../topics/04-decomposition/ANSWERS.md#f-typical-interviewer-follow-ups--push-backs) |
| 11 | Debugging | One live triage rehearsal; LLM troubleshooting | [README triage exercise](./README.md#exercise-live-triage-a-customer-integration-broke-overnight-diagnose-it-with-incomplete-information-while-keeping-the-customer-informed), [12 — C](../../topics/12-debugging-troubleshooting/ANSWERS.md#c-llm-specific-troubleshooting) |
| 12 | Customer | Demo your build; two role-plays | [README demo exercise](./README.md#exercise-live-demo-present-a-working-prototype-to-a-mock-customer-executive-who-interrupts-with-objections), [09 — B](../../topics/09-customer-scenarios/ANSWERS.md#b-difficult-situations) |
| 13 | Company | Company research, questions to ask, full mock loop | [11 Company-specific](../../topics/11-company-specific/ANSWERS.md), [14](../../topics/14-questions-to-ask/ANSWERS.md) |
| 14 | Rest and review | Re-read stories and your design doc; sleep | [README checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–40 | One coding problem, timed, talking out loud. Alternate DS&A and practical/incremental days. |
| 40–65 | The week's main topic (design, LLM build, decomposition), reading one model answer and then answering a sibling question without notes. |
| 65–80 | One story or one customer question out loud, recorded on your phone. |
| 80–90 | Log what went wrong in a mistakes file; pick tomorrow's problem. |

Use weekends for the longer pieces: the week's deliverable and the mock.

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Recruiter screen (30 min) | A friend or colleague |
| 2 | Incremental coding (60 min) | A peer engineer, or a platform such as Pramp, interviewing.io or Exponent |
| 3 | Applied system design (45 min) | A senior engineer; interviewing.io and Exponent also offer design mocks |
| 4 | Take-home defence (45 min) | An engineer who reads your repo first |
| 5 | Decomposition (40 min) + live triage (30 min) | A peer; for decomposition, someone with consulting or product experience is ideal |
| 6 | Full loop (3–4 hours) and demo role-play | Two or three different people; record the demo |

Aim for at least one coding mock every week from Week 2, even when the week's theme is something else. Record any mock you cannot do with a person and review it the next day.

## Fast track for the repo owner's profile

- **Skim:** API design and integration fundamentals ([08 — A](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration), [B](../../topics/08-cloud-devops-integration/ANSWERS.md#b-authentication-authorization--security)) and data pipelines ([07 — D](../../topics/07-data-and-sql/ANSWERS.md#d-pipelines--data-quality)): contract-first integrations, an API gateway with JWT auth and rate limiting, and large data migrations are already strengths.
- **Skim:** SQL and data modelling; deep MySQL and PostgreSQL experience covers most of [07](../../topics/07-data-and-sql/ANSWERS.md). Do a few window-function reps only.
- **Biggest gap 1 — production LLM, RAG, agent and eval experience.** Give Week 4 double time and make the build public; this is the round most likely to decide an AI-lab FDE offer.
- **Biggest gap 2 — DS&A is rusty.** Start daily coding from Week 1 and keep one coding mock every week; practical/incremental problems first, then classic DS&A if targeting Palantir or Scale.
- **Biggest gap 3 — external customer-facing work and live demos.** Most stakeholder work has been internal (sales, finance, academic teams). Frame those honestly as customer work, and do extra demo and role-play reps in Week 6.
- **Applied system design and take-home defence:** the bidirectional legacy MySQL to new Postgres bridge across six product lines (idempotency guards, ordering constraints, reconciliation) is a strong legacy-integration design story.
- **Prototype-to-production and debugging rounds:** the legacy modernisation (PHP 5.3 to 7.3 user migration) and the platform of about 13 services give concrete examples of shipping safely and root-causing in production.
- **Customer role-play and decomposition:** the order/subscription/commerce design (SKU catalogue, discount approval rules engine, invoicing tied to cash collection) and CRM/ERP integrations show turning business needs into systems.
- **Behavioral / hiring manager:** leading a squad of about four engineers, release planning, monthly programme reports and mentoring cover ownership and leadership; the speech-to-text provider evaluation (a contribution, not sole authorship) is an honest AI story covering buy-vs-build and data-protection constraints.
- Keep all stories in [../../my-prep/STORY-BANK.md](../../my-prep/STORY-BANK.md). Confirm which languages to code in before Week 2; only the stack above is evidenced.

## Resources

- *Designing Data-Intensive Applications* by Martin Kleppmann — the standard reference for data systems, replication and consistency.
- *Cracking the Coding Interview* by Gayle Laakmann McDowell, plus LeetCode or NeetCode problem lists for timed DS&A practice.
- *System Design Interview* (volumes 1 and 2) by Alex Xu — useful for the classic design method before applying it to customer contexts.
- Official API documentation for your target: Anthropic docs (https://docs.anthropic.com) and OpenAI platform docs (https://platform.openai.com/docs), including their cookbooks and prompt engineering guides.
- *AI Engineering* by Chip Huyen — building and evaluating applications on foundation models.
- Palantir's public Foundry and AIP documentation and learning resources, if interviewing at Palantir.
- Mock platforms: Pramp, interviewing.io and Exponent.
