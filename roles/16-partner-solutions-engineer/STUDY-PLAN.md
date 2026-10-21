# Study Plan — Partner Solutions Engineer

For engineers preparing for Partner Solutions Engineer, Partner Solutions Architect, Alliances Engineer or Integrations Partner Engineer interviews; assumes about 10–12 hours a week for six weeks, or a two-week crash version.

---

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim any area where you are already a 4 or 5, and put the saved time into your two lowest scores.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| API and integration design | | You can explain OAuth 2.0 flows, webhook delivery and signatures, idempotency, pagination, rate limiting and API versioning, and have built at least one integration in production. |
| Connector coding | | You can build a small sync job between two APIs in a few hours, with retries, incremental state, tests and a clear README. |
| Cloud platform knowledge | | You can draw a reference architecture on one major cloud with private networking, IAM roles, encryption keys and logging, and explain each choice. |
| Cloud marketplaces and partner programs | | You can explain how a SaaS marketplace listing works (landing page, lifecycle events, entitlements, metering), private offers and co-sell programs. |
| Enablement and teaching | | You can teach a technical topic to a mixed audience in 20 minutes, with a demo, and handle questions you cannot answer. |
| Partner and stakeholder management | | You have concrete stories of influencing teams outside your reporting line, handling conflict between two parties, and saying no while keeping the relationship. |
| Commercial awareness | | You can explain partner-sourced versus partner-influenced revenue, deal registration, channel conflict and joint business plans. |
| Behavioral stories | | You have 8–10 STAR stories written down and can tell each in under three minutes. |

---

## 6-week plan

Assumes 10–12 hours a week. Each week has study items, a hands-on deliverable, a mock interview and an end-of-week check.

### Week 1 — Role, ecosystem and integration fundamentals

**Goals:** understand what the role is and how partner ecosystems work; refresh API and integration fundamentals.

- [ ] Read the role overview and loop in [this role's README](README.md#the-role) and the [typical interview loop](README.md#typical-interview-loop).
- [ ] Study the model answers under [Role and partner ecosystem](README.md#role-and-partner-ecosystem).
- [ ] Work through [APIs & integration](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration) and [Authentication, authorization & security](../../topics/08-cloud-devops-integration/ANSWERS.md#b-authentication-authorization--security).
- [ ] Read the Tier 1 questions in [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md) and draft answers to "about you and the role".
- [ ] Draft your "why partner engineering" answer using [01 Recruiter and motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md#motivation--role-fit).

**Deliverable:** a one-page map of one real company's partner ecosystem (from public pages): its app directory or marketplace, top technology partners, SI partners and which cloud marketplaces it lists on.

**Mock interview:** a 30-minute recruiter and hiring-manager style screen with a peer: why this role, your integration experience, and one partner or vendor story.

**End of week check:** you can explain the difference between SIs, ISVs, resellers, marketplaces and cloud providers as partners, and describe OAuth authorisation code versus client credentials without notes.

### Week 2 — Building integrations and connectors

**Goals:** be able to design and build a reliable connector; handle versioning, webhooks and rate limits.

- [ ] Study [Building integrations and connectors](README.md#building-integrations-and-connectors) model answers.
- [ ] Review practical coding patterns in [02 Coding](../../topics/02-coding/ANSWERS.md) (API-style problems, error handling).
- [ ] Review [Production troubleshooting scenarios](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios) for integration failures.
- [ ] Read one major platform's integration documentation end to end (for example Salesforce REST and Bulk APIs, or Stripe's webhooks and API versioning docs).

**Deliverable:** build and publish a small connector: incremental sync between two public APIs with pagination, retries with backoff, idempotent upserts, unit tests with recorded responses and a README explaining design decisions.

**Mock interview:** a 60-minute practical coding session: a peer gives you an API brief and you build a sync script live, talking through decisions.

**End of week check:** you can design a connector framework and a webhook system on a whiteboard in 30 minutes each, and your connector runs twice without creating duplicates.

### Week 3 — Cloud marketplaces and reference architectures

**Goals:** understand marketplace listings and metering; design joint reference architectures.

- [ ] Study [Cloud marketplaces](README.md#cloud-marketplaces) and [Joint reference architectures and ecosystem design](README.md#joint-reference-architectures-and-ecosystem-design).
- [ ] Read the official seller documentation for AWS Marketplace SaaS products (or the Azure or Google Cloud equivalent if the company lists there).
- [ ] Review [Cloud](../../topics/08-cloud-devops-integration/ANSWERS.md#c-cloud) and the method in [03 System design](../../topics/03-system-design/ANSWERS.md).
- [ ] Skim [04 Solutions Architect](../04-solutions-architect/README.md) for reference architecture and security answers.

**Deliverable:** a joint reference architecture document: your product (or an imagined SaaS product) on a cloud provider for a regulated customer, with a diagram, private connectivity, SSO, customer-managed keys, audit logging and a shared responsibility table.

**Mock interview:** a 45-minute architecture whiteboard with a peer playing a partner's architect who challenges security and data flow choices.

**End of week check:** you can walk through a marketplace SaaS integration (landing page, lifecycle events, entitlements, metering) and explain private offers and committed spend in plain language.

### Week 4 — Enablement, certification and documentation

**Goals:** be ready for the enablement or presentation round; design programs that scale.

- [ ] Study [Partner enablement and certification](README.md#partner-enablement-and-certification).
- [ ] Review teaching techniques in [05 Learning round](../../topics/05-learning-round/ANSWERS.md) and [Communication](../../topics/09-customer-scenarios/ANSWERS.md#c-communication).
- [ ] Read [10 Developer Relations](../10-developer-relations/README.md) for documentation and sample-code practices.

**Deliverables:**
1. A 20-minute enablement session (slides plus live demo) teaching a product or integration to partner engineers. Record it.
2. A one-page certification program outline for SI partners: levels, tracks, assessment method and firm-level tiers.

**Mock interview:** deliver the enablement session to two peers who interrupt with hard questions; get written feedback on clarity and pacing.

**End of week check:** you can teach a technical topic in 20 minutes, stay on time, and handle a question you cannot answer gracefully.

### Week 5 — Co-selling, alliances and partner scenarios

**Goals:** handle commercial conversations and partner role-plays confidently.

- [ ] Study [Co-selling and alliances](README.md#co-selling-and-alliances) and [Joint customer issues and feedback](README.md#joint-customer-issues-and-feedback).
- [ ] Work through [Difficult situations](../../topics/09-customer-scenarios/ANSWERS.md#b-difficult-situations) and [Business acumen](../../topics/09-customer-scenarios/ANSWERS.md#e-business-acumen).
- [ ] Read the public partner program pages of the company's main cloud provider (co-sell eligibility, technical validation).

**Deliverable:** a two-page technical section of a joint business plan for an imagined partner: target solutions, integration milestones, enablement targets, support model and metrics.

**Mock interview:** three 15-minute role-plays with a peer: channel conflict, a partner demanding a roadmap commitment, and a joint customer escalation after a partner release broke an integration.

**End of week check:** in each role-play you protected the customer, stayed fair to the partner, avoided promising roadmap dates, and ended with a clear next step.

### Week 6 — Behavioral, company research and full mock loop

**Goals:** polish stories, tailor to the company, and rehearse the whole loop.

- [ ] Study [Behavioral (role-specific)](README.md#behavioral-role-specific) and [Conflict & collaboration](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration) in 10 Behavioral.
- [ ] Finalise 8–10 stories in [your story bank](../../my-prep/STORY-BANK.md), mapped to the role-specific behavioral questions.
- [ ] Do the [company research checklist](../../topics/11-company-specific/ANSWERS.md#company-research-checklist-do-for-every-company) for each target company, focused on its partner ecosystem.
- [ ] Prepare questions from [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md) (for example how the partner team is measured and how it works with sales).
- [ ] Re-run the [Prep checklist](README.md#prep-checklist).

**Deliverable:** a one-page "30-60-90 day plan" for the partner engineering role at your top target company.

**Mock interview:** a full mock loop over one or two days: hiring manager (45 min), integration technical (60 min), architecture whiteboard (45 min), enablement presentation (30 min), behavioral (45 min).

**End of week check:** you can do every round in the loop without notes, and every behavioral answer is under three minutes with a measurable result.

---

## 2-week crash plan

For an interview that is already scheduled. About 2–3 hours a day.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and motivation | Read the role overview; write your "why partner engineering" answer and 2-minute introduction | [README](README.md#the-role), [01 Motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| 2 | API fundamentals | OAuth flows, webhooks, idempotency, pagination, rate limits | [08 APIs & integration](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration) |
| 3 | Connectors | Study connector answers; start building a small sync script | [Building integrations](README.md#building-integrations-and-connectors) |
| 4 | Connectors | Finish the script with retries, tests and README | [02 Coding](../../topics/02-coding/ANSWERS.md) |
| 5 | Marketplaces | Marketplace answers; read one provider's SaaS listing docs | [Cloud marketplaces](README.md#cloud-marketplaces) |
| 6 | Reference architecture | Draw a joint reference architecture from memory twice | [Joint reference architectures](README.md#joint-reference-architectures-and-ecosystem-design), [03 System design](../../topics/03-system-design/ANSWERS.md) |
| 7 | Mock | Technical mock: connector design plus architecture whiteboard | [Model answers](README.md#model-answers) |
| 8 | Enablement | Build a 20-minute enablement session | [Partner enablement](README.md#partner-enablement-and-certification) |
| 9 | Enablement | Deliver it twice (recorded and to a peer); fix weak parts | [05 Learning round](../../topics/05-learning-round/ANSWERS.md) |
| 10 | Co-sell | Co-sell, channel conflict and roadmap answers | [Co-selling and alliances](README.md#co-selling-and-alliances) |
| 11 | Scenarios | Role-play three partner scenarios out loud | [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md) |
| 12 | Behavioral | Finalise stories; practise the four role-specific behavioral questions | [Behavioral](README.md#behavioral-role-specific), [Story bank](../../my-prep/STORY-BANK.md) |
| 13 | Company | Research the company's ecosystem; prepare questions to ask | [11 Company specific](../../topics/11-company-specific/ANSWERS.md), [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md) |
| 14 | Full mock and rest | One full mock loop in the morning; light review only after | [Prep checklist](README.md#prep-checklist) |

---

## Daily routine

A realistic 90-minute day:

| Time | Activity |
|---|---|
| 10 min | Review yesterday's notes and flashcards (OAuth flows, marketplace APIs, partner terms). |
| 30 min | Study one section of the model answers or a shared topic; write your own short version of two answers. |
| 30 min | Hands-on: work on the week's deliverable (connector, architecture, enablement deck). |
| 15 min | Say one answer or one behavioral story out loud, timed; record it once a week. |
| 5 min | Write down what you learned and one thing to fix tomorrow. |

---

## Mock interview schedule

| When | Mock | With whom |
|---|---|---|
| Week 1 | Recruiter and hiring-manager screen (30 min) | A peer or friend in tech |
| Week 2 | Practical coding: build a sync script live (60 min) | A peer engineer, or a practice platform such as Pramp or interviewing.io |
| Week 3 | Architecture whiteboard (45 min) | A peer who has worked with a cloud provider; Exponent has system design practice as an option |
| Week 4 | Enablement presentation (20–30 min) | Two peers who interrupt, plus a recording you review yourself |
| Week 5 | Three partner role-plays (15 min each) | A peer playing the partner, ideally someone with sales or partner experience |
| Week 6 | Full mock loop (4–5 hours over one or two days) | Several peers, each taking one round |
| Ongoing | One recorded behavioral answer a day | Yourself; review the recording for length and clarity |

Ask every mock partner for one thing you did well and one thing to fix, and track fixes across mocks.

---

## Fast track for the repo owner's profile

- **Skim week 2's fundamentals:** contract-first integration design, an API gateway with JWT auth and rate limiting, and an aggregation (BFF) layer are core strengths; spend the time on connector-framework and webhook design at partner scale instead.
- **Map CRM/ERP integration work to the technical round:** the CRM and accounting-system integrations are direct evidence for "build an integration with a major SaaS platform" and the joint customer triage question.
- **Map the bidirectional MySQL to Postgres migration bridge** (idempotency guards, ordering constraints, reconciliation across six product lines) to the connector take-home and to webhook reliability questions.
- **Map delivery leadership** (onboarding plans, mentoring, monthly programme reports) to the enablement and certification questions and to "something others reused at scale".
- **Biggest gap: external partner and customer work.** Most stakeholder experience is internal (sales, finance, academic teams). Spend extra time on week 5 role-plays and frame internal stories as cross-team influence without authority.
- **Second gap: cloud marketplaces and partner programs.** AWS experience is evidenced only for S3 and SQS; do all of week 3 and consider studying toward a cloud associate-level certification.
- **Third gap: live presentation to external audiences.** Record the enablement session in week 4 at least twice.
- **Public code portfolio gap:** publish the week 2 connector in a public repository; it becomes evidence for the take-home round.
- Use [your story bank](../../my-prep/STORY-BANK.md) for STAR versions of these stories, and confirm which languages you will use in coding rounds.

---

## Resources

- **Official docs:** AWS Marketplace Seller Guide (SaaS products), Microsoft commercial marketplace documentation (SaaS fulfilment and metering APIs), Google Cloud Marketplace partner documentation.
- **Partner programs:** AWS Partner Network (APN) program guides, Microsoft AI Cloud Partner Program, Google Cloud Partner Advantage. Read the current versions; requirements change often.
- **Integration references:** Salesforce developer documentation (REST API, Bulk API 2.0, Change Data Capture), Stripe API documentation (versioning, webhooks, idempotency keys), the OAuth 2.0 specification (RFC 6749) and OAuth 2.0 Security Best Current Practice.
- **Books:** *Designing Data-Intensive Applications* by Martin Kleppmann (reliability, idempotency, data movement); *API Design Patterns* by JJ Geewax; *The Trusted Advisor* by Maister, Green and Galford (partner relationships).
- **Certifications (optional structure for study):** AWS Certified Solutions Architect – Associate, or the Azure or Google Cloud architect equivalent; some vendors also publish free partner training paths.
- **Related roles in this repo:** [04 Solutions Architect](../04-solutions-architect/README.md), [08 Implementation / Integration Engineer](../08-implementation-integration-engineer/README.md), [10 Developer Relations](../10-developer-relations/README.md).
