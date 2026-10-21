# Study Plan — Founding / Product Engineer

For engineers preparing for founding engineer or product engineer roles at early-stage (often AI) startups; assumes about 10–12 hours a week for six weeks, or a two-week crash version.

---

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim any area where you are already a 4 or 5, and put the saved time into your two lowest scores.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Full-stack shipping speed | | You can build and deploy a small web app with auth, a database and a working UI in a day, using a stack you know well. |
| Product sense | | You can pick a product, name a specific user problem, propose a change, and say how you would measure it in five minutes. |
| AI / LLM application engineering | | You have built a RAG or agent feature with an evaluation set, and can explain cost, latency and failure handling. |
| Startup system design | | You can design a simple multi-tenant backend that two people can run, and explain what changes at 100 times the usage. |
| Operations | | You have set up CI/CD, error tracking and alerts, and handled a production incident end to end. |
| Talking to users | | You have interviewed users about their workflow, avoided leading questions, and changed a plan based on what you heard. |
| Take-home and live build | | You have done a timed build from a brief and delivered working software with a clear README and demo. |
| Startup judgement and equity | | You can explain options, vesting, strike price, dilution and runway, and know what questions to ask founders. |

---

## 6-week plan

Assumes 10–12 hours a week. Each week has study items, a hands-on deliverable, a mock interview and an end-of-week check.

### Week 1 — Role, motivation and startup due diligence

**Goals:** be clear about why you want this, what risk you can accept, and how to evaluate startups.

- [ ] Read the [role overview](README.md#the-role) and the [typical interview loop](README.md#typical-interview-loop).
- [ ] Study the model answers under [Role, fit and risk](README.md#role-fit-and-risk).
- [ ] Draft your motivation answer with [01 Recruiter and motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md#motivation--role-fit).
- [ ] Read the Tier 1 questions in [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).
- [ ] Prepare due-diligence questions using [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md#about-the-role).

**Deliverables:**
1. A personal "startup criteria" page: minimum salary, risk you can accept, stage and domain you want, and your red flags.
2. A shortlist of 10–15 target startups with a note on each one's product, stage and what you would build there.

**Mock interview:** a 30-minute founder intro call with a peer playing the founder: why startups, why this company, what you have built.

**End of week check:** you can explain why you want this role in two minutes, and explain options, vesting and dilution in plain language.

### Week 2 — Shipping fast: stack, practices and operations

**Goals:** have a default stack and a way of shipping safely that you can describe and demonstrate.

- [ ] Study [Shipping fast and technical judgement](README.md#shipping-fast-and-technical-judgement).
- [ ] Review [CI/CD & operations](../../topics/08-cloud-devops-integration/ANSWERS.md#e-cicd--operations) and [Software engineering practices](../../topics/13-cs-fundamentals/ANSWERS.md#software-engineering-practices).
- [ ] Review [Production troubleshooting scenarios](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios).

**Deliverable:** start a small full-stack AI app (for example a document Q&A or ticket triage tool) on your default stack: auth, database, one LLM call, CI that deploys on merge, error tracking and one alert. Publish it in a public repository.

**Mock interview:** a 45-minute technical conversation with a peer: walk through your stack choices and how two engineers would run it in production.

**End of week check:** your app is deployed with CI/CD and monitoring, and you can justify each stack choice in one sentence.

### Week 3 — AI product engineering

**Goals:** be able to build and evaluate an AI feature properly, which most AI startups will test.

- [ ] Study [RAG](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation), [Agents and tool use](../../topics/06-ai-llm/ANSWERS.md#d-agents-and-tool-use) and [Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important).
- [ ] Study [Production LLM engineering](../../topics/06-ai-llm/ANSWERS-part2.md#g-production-llm-engineering) and [Take-home / build exercises](../../topics/06-ai-llm/ANSWERS-part2.md#j-take-home--build-exercises-reported).
- [ ] Review the AI questions in this role's [Product sense](README.md#product-sense) and [System design for startups](README.md#system-design-for-startups) answers.

**Deliverable:** finish the AI app: retrieval or tool use, an evaluation set of at least 50 examples with a score you track, cost per request, a fallback when the model fails, and a two-minute demo video.

**Mock interview:** a 45-minute AI system design with a peer: "Design an LLM feature with evaluation, cost control and fallbacks that one engineer can build in two weeks."

**End of week check:** you can show an eval score before and after a prompt change, and explain how you would decide the feature is good enough to ship.

### Week 4 — Take-homes and live builds

**Goals:** be comfortable building under time pressure, in your own code and someone else's.

- [ ] Study [Take-home and build exercises](README.md#take-home-and-build-exercises).
- [ ] Review practical coding patterns in [02 Coding](../../topics/02-coding/ANSWERS.md) (especially the object-oriented and practical sections).
- [ ] Keep DS&A warm with a few problems from [DS&A topics](../../topics/02-coding/ANSWERS-dsa-topics.md), in case a startup still asks.

**Deliverables:**
1. A timed take-home: give yourself a brief (or ask a friend for one) and build it in 6–8 hours, with a README covering decisions, trade-offs and next steps.
2. Two 90-minute live builds in unfamiliar open-source codebases: get them running and add a small feature.

**Mock interview:** a take-home review with a peer acting as the founder: 10-minute walkthrough, then 30 minutes of challenge questions.

**End of week check:** your take-home runs from a clean setup with one command, and you can defend every decision in it.

### Week 5 — Product sense, users and customers

**Goals:** think like a product owner and handle customer situations founders will throw at you.

- [ ] Study [Product sense](README.md#product-sense) and [Customers and owning everything](README.md#customers-and-owning-everything).
- [ ] Work through [Abstract / product decomposition](../../topics/04-decomposition/ANSWERS.md#e-abstract--product-decomposition) and [Product feedback & scale](../../topics/09-customer-scenarios/ANSWERS.md#d-product-feedback--scale).
- [ ] Read *The Mom Test* (short book on user interviews).

**Deliverables:**
1. Interview three real users of your AI app or of a product you use (friends or colleagues are fine), using past-behaviour questions; write one page on what you learned and what you would change.
2. Two written product-sense answers: a product you would change, and a prioritisation of ten requests with one engineer.

**Mock interview:** a 45-minute product sense round with a peer, plus two short customer role-plays (custom feature request, angry customer after a wrong AI answer).

**End of week check:** you can answer a product-sense question with a specific user, problem, solution and metric in under ten minutes.

### Week 6 — System design, founder rounds, behavioral and full mock

**Goals:** polish founder-facing rounds and rehearse the whole loop.

- [ ] Study [System design for startups](README.md#system-design-for-startups) and [Founder interview and working style](README.md#founder-interview-and-working-style).
- [ ] Review the method in [03 System design](../../topics/03-system-design/ANSWERS.md) and practise stating the simplest version first.
- [ ] Study [Behavioral (role-specific)](README.md#behavioral-role-specific) and [Ownership & impact](../../topics/10-behavioral/ANSWERS.md#ownership--impact) in 10 Behavioral.
- [ ] Finalise stories in [your story bank](../../my-prep/STORY-BANK.md).
- [ ] Re-run the [Prep checklist](README.md#prep-checklist).

**Deliverable:** a written "first 30 days" plan for your top target startup, based on using its product.

**Mock interview:** a full mock loop over one or two days: founder intro (30 min), past-project deep dive (45 min), live build (90 min), startup system design (45 min), product sense (45 min), founder fit and offer conversation (30 min).

**End of week check:** you can do every round without notes, and you have a clear set of questions to judge each startup before accepting an offer.

---

## 2-week crash plan

For an interview that is already scheduled. About 2–3 hours a day.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Motivation and company | Write your "why startup, why this company" answer; use their product | [Role, fit and risk](README.md#role-fit-and-risk), [01 Motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| 2 | Shipping fast | Stack, testing, deploys, operations answers | [Shipping fast](README.md#shipping-fast-and-technical-judgement) |
| 3 | AI basics | RAG and evaluation | [RAG](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation), [Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important) |
| 4 | Build | Start a small AI app on your default stack | [Build exercises](../../topics/06-ai-llm/ANSWERS-part2.md#j-take-home--build-exercises-reported) |
| 5 | Build | Finish it: eval set, deploy, README, demo video | [Take-home answers](README.md#take-home-and-build-exercises) |
| 6 | Live build | One 90-minute live build in an unfamiliar codebase | [02 Coding](../../topics/02-coding/ANSWERS.md) |
| 7 | Mock | Take-home review mock with a peer | [Model answers](README.md#model-answers) |
| 8 | Product sense | Two product-sense answers out loud, timed | [Product sense](README.md#product-sense), [04 Decomposition](../../topics/04-decomposition/ANSWERS.md) |
| 9 | Customers | Customer and ownership answers; two role-plays | [Customers and owning everything](README.md#customers-and-owning-everything) |
| 10 | System design | Multi-tenant AI backend and LLM feature design, twice each | [System design for startups](README.md#system-design-for-startups), [03 System design](../../topics/03-system-design/ANSWERS.md) |
| 11 | Founder rounds | First 30 days, working with founders, hiring | [Founder interview](README.md#founder-interview-and-working-style) |
| 12 | Behavioral | Finalise four role-specific stories | [Behavioral](README.md#behavioral-role-specific), [Story bank](../../my-prep/STORY-BANK.md) |
| 13 | Due diligence | Runway, traction and equity questions; offer basics | [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md) |
| 14 | Full mock and rest | One full mock loop in the morning; light review only after | [Prep checklist](README.md#prep-checklist) |

---

## Daily routine

A realistic 90-minute day:

| Time | Activity |
|---|---|
| 10 min | Use a target company's product or read one founder post; note one idea. |
| 25 min | Study one section of the model answers or a shared topic; write your own short version of two answers. |
| 40 min | Build: work on the week's app, take-home or live-build practice. Shipping something every day is the habit this role rewards. |
| 10 min | Say one answer or behavioral story out loud, timed. |
| 5 min | Write down what you shipped and learned, and one thing to fix tomorrow. |

---

## Mock interview schedule

| When | Mock | With whom |
|---|---|---|
| Week 1 | Founder intro call (30 min) | A peer, ideally someone who has worked at or founded a startup |
| Week 2 | Stack and operations conversation (45 min) | A peer engineer |
| Week 3 | AI system design (45 min) | A peer with LLM experience; Exponent and similar platforms have system design practice as an option |
| Week 4 | Take-home review (40 min) and one live build (90 min) | A peer acting as founder; Pramp or interviewing.io are options for general coding practice |
| Week 5 | Product sense (45 min) and two customer role-plays | A product manager friend if possible |
| Week 6 | Full mock loop (4–5 hours over one or two days) | Several peers, each taking one round |
| Ongoing | One recorded answer a day | Yourself; review for length, clarity and specifics |

Ask every mock partner for one thing you did well and one thing to fix, and track fixes across mocks.

---

## Fast track for the repo owner's profile

- **Skim backend and API design study:** contract-first integrations, an API gateway with JWT auth and rate limiting, a BFF layer and about 13 platform services are strong evidence for startup system design; focus instead on presenting the simplest version first.
- **Map commerce platform design** (SKU catalogue, discount approval rules engine, invoicing and payments) to the usage metering and billing design question.
- **Map the bidirectional MySQL to Postgres migration bridge** (idempotency guards, ordering, reconciliation) and the PHP 5.3 to 7.3 user migration to "shipping without breaking things" and data-integrity questions.
- **Map delivery leadership** (squad of about four engineers, sprint planning, onboarding plans, mentoring) to "how would you hire and set culture" and "first 30 days".
- **Biggest gap: production LLM/RAG/eval experience.** Do all of week 3 and make the AI app the centrepiece of your portfolio; contribution to a speech-to-text provider evaluation (buy-vs-build cost model, DPDP Act constraints) is a useful buy-versus-build story but say clearly that you contributed rather than led it.
- **Second gap: public code portfolio and recent hands-on speed.** The week 2–4 app and timed take-home are the most important deliverables for this profile.
- **Third gap: external users.** Most stakeholders have been internal (sales, finance, academic teams); do the week 5 user interviews and frame internal users as customers in stories.
- **Confirm your languages and frontend experience** before choosing a default stack; pick the stack you can ship fastest in.
- Use [your story bank](../../my-prep/STORY-BANK.md) for STAR versions of these stories.

---

## Resources

- **Books:** *The Mom Test* by Rob Fitzpatrick (user interviews); *Inspired* by Marty Cagan (product thinking); *Shape Up* by Ryan Singer (free online from Basecamp; scoping and shipping in fixed time); *Designing Data-Intensive Applications* by Martin Kleppmann (data and reliability).
- **Startup material:** Y Combinator's Startup School and library (free), Paul Graham's essays (for example "Do Things That Don't Scale"), and YC's "Work at a Startup" board for finding roles.
- **Equity:** Holloway's *Guide to Equity Compensation* (detailed, partly free online); always confirm with a tax advisor in your country.
- **AI engineering:** official documentation of the major model providers (for example Anthropic and OpenAI docs on prompting, tool use and evaluation); *AI Engineering* by Chip Huyen.
- **Product engineering:** PostHog's public handbook and blog on product engineers (publicly available, describes how a product-engineer culture works).
- **Related roles in this repo:** [01 Forward Deployed Engineer](../01-forward-deployed-engineer/README.md), [05 Applied AI Engineer](../05-applied-ai-engineer/README.md), [06 Agent Engineer](../06-agent-engineer/README.md).
