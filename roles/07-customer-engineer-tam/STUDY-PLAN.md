# Study Plan — Customer Engineer / Technical Account Manager (TAM)

For engineers moving into post-sale account ownership (TAM, Customer Success Engineer, post-sale Customer Engineer); assumes 6 weeks at about 10–12 hours a week, or the 2-week crash plan if an interview is already booked.

Role overview, loop and model answers: [README.md](./README.md). Questions every role gets: [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).

---

## Before you start

Spend 30 minutes rating yourself. Anything you rate 4 or 5, skim in the plan below; anything at 1–2 gets extra time.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Account health and adoption | | You can name five health signals (usage, tickets, sponsor engagement, renewal date, open escalations) and turn a "300 of 1,000 seats active" problem into a plan without notes. |
| QBRs and executive communication | | You can build a five-slide QBR from raw account data and present it to a VP in 10 minutes, leading with outcomes, not features. |
| Escalation management | | You can run a Sev 1 on a bridge call: roles, update cadence, customer comms, internal chasing, and the incident summary afterwards. |
| Cloud and networking breadth | | You can explain compute, storage, networking, IAM and the support model of one cloud at whiteboard level, and debug a 502 or a DNS issue out loud. |
| Commercial judgment | | You use ARR, gross and net retention, churn and expansion correctly and know where the TAM's role stops and the account executive's starts. |
| Book-of-business management | | You can segment 15 accounts into high-, low- and tech-touch and defend how you spend a week across them. |
| Written communication | | You can write a customer-facing incident summary and an escalation email in 20 minutes that a CTO would forward unchanged. |
| Behavioral stories | | You have four ready stories: Sev 1 escalation, at-risk account saved, saying no to a customer, influencing product. |

---

## 6-week plan

### Week 1 — The role, the loop and account health

**Goals:** understand what a TAM is measured on, and be able to talk about account health and adoption fluently.

- [ ] Read [The role](./README.md#the-role) and [Typical interview loop](./README.md#typical-interview-loop) in this role's README, including the note on Google Cloud's pre-sales "Customer Engineer" title.
- [ ] Read the [Tier 1 questions](../COMMON-QUESTIONS.md#tier-1--asked-in-almost-every-loop) in COMMON-QUESTIONS.md.
- [ ] Work through [Account health and adoption](./README.md#account-health-and-adoption-1) model answers, especially [the 1,000-seat question](./README.md#a-customer-bought-1000-seats-but-only-300-are-active-after-six-months-what-do-you-do) and [building a success plan](./README.md#how-do-you-build-a-success-plan-for-a-new-enterprise-account).
- [ ] Read [what is the difference between a CSM, a TAM and a pre-sales CE](./README.md#what-is-the-difference-between-a-csm-a-tam-and-a-pre-sales-customer-engineer) so you can answer "why this role" precisely.
- [ ] Read [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) and its [answers](../../topics/01-recruiter-and-motivation/ANSWERS.md); rewrite "Why FDE?" as "Why post-sale, why TAM?".

**Deliverables:**
1. A one-page account-health scorecard for a fictional SaaS customer: 6–8 signals, a red/amber/green rule for each, and the action each colour triggers.
2. A 60-second "why TAM" answer, recorded on your phone.

**Mock:** 30-minute recruiter-screen mock with a peer: motivation, book size you expect, travel, and "tell me about a customer you supported".

**End of week check:** you can explain how you would measure the health of an account and what you would do about a low-adoption account, in under three minutes each.

### Week 2 — Technical breadth: one cloud plus networking

**Goals:** pass the technical or "role-related knowledge" round. Pick one cloud (or the company's product) and learn it at whiteboard level.

- [ ] Read [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) sections [A (APIs)](../../topics/08-cloud-devops-integration/README.md#a-apis--integration), [B (auth)](../../topics/08-cloud-devops-integration/README.md#b-authentication-authorization--security), [C (cloud)](../../topics/08-cloud-devops-integration/README.md#c-cloud) and [E (CI/CD and operations)](../../topics/08-cloud-devops-integration/README.md#e-cicd--operations), with [answers](../../topics/08-cloud-devops-integration/ANSWERS.md).
- [ ] Read the [Networking](../../topics/13-cs-fundamentals/README.md#networking) section of 13-cs-fundamentals and its [answers](../../topics/13-cs-fundamentals/ANSWERS.md): DNS, TLS, HTTP, load balancing.
- [ ] Study [Technical troubleshooting and advisory](./README.md#technical-troubleshooting-and-advisory-1): the [502 behind a load balancer](./README.md#a-customer-reports-intermittent-502-errors-behind-a-google-cloud-external-application-load-balancer-walk-through-troubleshooting-it-with-them-on-a-call) and [Compute Engine vs GKE vs Cloud Run](./README.md#explain-to-a-customer-when-to-choose-compute-engine-gke-or-cloud-run-for-a-workload) answers.
- [ ] Read [the shared responsibility model answer](./README.md#explain-the-shared-responsibility-model-and-how-it-shapes-a-conversation-after-a-customer-security-incident).

**Deliverables:**
1. A one-page cheat sheet for your chosen cloud: compute, storage, networking, IAM, support tiers and severity definitions.
2. A whiteboard diagram (photo is fine) of a typical three-tier customer workload on that cloud, with where 502s, 504s and IAM errors come from.

**Mock:** 45-minute technical mock: a peer reads you the 502 scenario and plays the customer's engineer while you troubleshoot out loud. Record it.

**End of week check:** you can explain your chosen cloud's core services in 10 minutes and walk a customer through a load-balancer error without guessing.

### Week 3 — Escalations and troubleshooting with the customer on the call

**Goals:** run a Sev 1 calmly, write customer-facing incident communication, and decide when and to whom to escalate.

- [ ] Work through [Escalation management](./README.md#escalation-management-1) model answers, especially [managing a Sev 1 for a strategic customer](./README.md#walk-me-through-how-you-manage-a-severity-1-escalation-for-a-strategic-customer) and [the customer emails your CEO](./README.md#a-customer-emails-your-ceo-complaining-about-repeated-outages-how-do-you-respond).
- [ ] Read [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) sections [B](../../topics/12-debugging-troubleshooting/README.md#b-production-troubleshooting-scenarios) and [D](../../topics/12-debugging-troubleshooting/README.md#d-debugging-process-questions), with [answers](../../topics/12-debugging-troubleshooting/ANSWERS.md).
- [ ] Do the [eight-ticket triage exercise](./README.md#exercise-triage-a-queue-of-eight-open-tickets-across-your-accounts-and-explain-your-order) with a 15-minute timer before reading the model answer.
- [ ] Read [the role-play where your data says the customer's configuration caused the outage](./README.md#role-play-a-frustrated-customer-says-your-platform-caused-their-outage-but-your-data-suggests-their-configuration-did).

**Deliverables:**
1. A customer-facing incident summary: take a real public post-mortem from any cloud or SaaS status page and rewrite it for a customer CTO (what happened, impact, root cause, what we are changing, what you should do).
2. A one-page Sev 1 escalation runbook: roles, update cadence, internal escalation path, and the first three messages you send.

**Mock:** 30-minute role-play: a peer plays an angry VP after a second outage this quarter. Then 20 minutes writing the follow-up email under a timer.

**End of week check:** you can run a Sev 1 bridge in role-play without losing the customer's trust and produce a clean incident summary in 20 minutes.

### Week 4 — QBRs, executives and commercial judgment

**Goals:** turn account data into an executive story, and handle renewal and expansion conversations without overstepping the account executive.

- [ ] Work through [QBRs and executive relationships](./README.md#qbrs-and-executive-relationships-1), including the [10-minute QBR exercise](./README.md#exercise-present-a-10-minute-qbr-for-a-mock-account-using-the-data-provided).
- [ ] Work through [Renewals, expansion and commercial judgment](./README.md#renewals-expansion-and-commercial-judgment-1), especially [the red account 90 days from renewal](./README.md#a-strategic-renewal-is-90-days-out-and-the-account-is-red-what-is-your-plan) and [the discount request](./README.md#a-customer-asks-for-a-discount-at-renewal-because-they-are-under-using-the-product-how-do-you-respond).
- [ ] Read [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) sections [B (difficult situations)](../../topics/09-customer-scenarios/README.md#b-difficult-situations) and [E (business acumen)](../../topics/09-customer-scenarios/README.md#e-business-acumen), with [answers](../../topics/09-customer-scenarios/ANSWERS.md).
- [ ] Learn the vocabulary: ARR, gross and net revenue retention, churn, expansion, consumption commitment, renewal forecast.

**Deliverables:**
1. A two-page success plan and a five-slide QBR for a fictional account (usage trend, outcomes delivered, open risks, roadmap asks, next-quarter goals).
2. A 10-minute recording of yourself presenting that QBR.

**Mock:** 45-minute QBR mock: present to a peer playing a sceptical executive sponsor who was not involved in the purchase; they interrupt with "why should we renew?".

**End of week check:** you can present a QBR in 10 minutes and answer a discount request at renewal without promising anything you do not own.

### Week 5 — Book of business, product feedback and behavioral

**Goals:** show you can prioritise many accounts and influence product, and have your stories ready.

- [ ] Work through [Managing a book of business](./README.md#managing-a-book-of-business-1), especially [15 accounts, one week](./README.md#you-own-15-accounts-how-do-you-decide-where-to-spend-your-week) and [two Sev 1s at once](./README.md#two-of-your-accounts-have-severity-1-issues-at-the-same-time-what-do-you-do).
- [ ] Read [writing a feature request a PM will act on](./README.md#how-do-you-write-a-feature-request-that-a-product-manager-will-actually-act-on) and [09 section D (product feedback)](../../topics/09-customer-scenarios/README.md#d-product-feedback--scale).
- [ ] Work through [Googleyness, values and behavioral](./README.md#googleyness-values-and-behavioral-role-specific-1) model answers.
- [ ] Read [10-behavioral](../../topics/10-behavioral/README.md) sections [Customer focus](../../topics/10-behavioral/README.md#customer-focus), [Conflict and collaboration](../../topics/10-behavioral/README.md#conflict--collaboration) and [Prioritisation and pressure](../../topics/10-behavioral/README.md#prioritisation--pressure), with [answers](../../topics/10-behavioral/ANSWERS.md).
- [ ] Add your stories to [STORY-BANK.md](../../my-prep/STORY-BANK.md), using the [story bank checklist](../../topics/00-overview/README.md#story-bank-checklist-prepare-one-story-for-each).

**Deliverables:**
1. A written feature request for a real gap you have seen in a product you use: customer impact, how many customers, workaround, business value.
2. Four STAR stories written out (Sev 1, account saved, said no, influenced product), each under two minutes spoken.

**Mock:** 45-minute hiring-manager mock focused on behavioral and book-of-business questions. For Google, use the Googleyness prompts; for AWS, map each story to a Leadership Principle.

**End of week check:** you can defend how you spend a week across 15 accounts and tell all four stories without notes.

### Week 6 — Company-specific prep and full loop rehearsal

**Goals:** tailor everything to the target company and rehearse the whole loop.

- [ ] Run the [company research checklist](../../topics/11-company-specific/README.md#company-research-checklist-do-for-every-company) for your target company: product, support tiers, severity definitions, TAM tier contents.
- [ ] Read the problem-solving prompts in [04-decomposition section C](../../topics/04-decomposition/README.md#c-commercial--enterprise) for the "general cognitive ability" style round.
- [ ] Do the [inherited account with no notes](./README.md#exercise-you-inherit-an-account-whose-previous-tam-left-no-notes-run-the-first-call) exercise out loud.
- [ ] Read the [Prep checklist](./README.md#prep-checklist) and tick off anything left.
- [ ] Pick 5 questions from [14-questions-to-ask](../../topics/14-questions-to-ask/README.md#about-customers) and add questions on book size, segmentation and how TAMs are measured.

**Deliverables:**
1. A one-page "first 90 days as a TAM here" plan for the target company.
2. A final cheat sheet: your four stories, your cloud summary, your QBR structure, your questions.

**Mock:** a full mock loop over two days: technical troubleshooting (45 min), customer role-play (30 min), QBR presentation (30 min), behavioral (45 min). Use different people for each if you can.

**End of week check:** you have done every round type at least twice and can name your weakest round and what you did about it.

---

## 2-week crash plan

For an interview already on the calendar. About 2 hours a day.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and loop | Read the role and loop; write "why TAM" and record it | [README](./README.md#the-role), [01 answers](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| 2 | Account health | Study health and adoption answers; draft a health scorecard | [Account health](./README.md#account-health-and-adoption-1) |
| 3 | Cloud basics | Cheat sheet for one cloud: compute, storage, network, IAM, support model | [08 section C](../../topics/08-cloud-devops-integration/README.md#c-cloud) |
| 4 | Networking troubleshooting | DNS, TLS, 502/504, timeouts; do the 502 scenario out loud | [13 Networking](../../topics/13-cs-fundamentals/README.md#networking), [502 answer](./README.md#a-customer-reports-intermittent-502-errors-behind-a-google-cloud-external-application-load-balancer-walk-through-troubleshooting-it-with-them-on-a-call) |
| 5 | Escalations | Sev 1 answer; write an incident summary in 20 minutes | [Escalation management](./README.md#escalation-management-1) |
| 6 | Mock 1 | Angry-executive role-play with a peer; review recording | [09 section B](../../topics/09-customer-scenarios/README.md#b-difficult-situations) |
| 7 | Rest / catch-up | Fill gaps from days 1–6 | — |
| 8 | QBR | Build a five-slide QBR for a fictional account | [QBR exercise](./README.md#exercise-present-a-10-minute-qbr-for-a-mock-account-using-the-data-provided) |
| 9 | Commercial | Renewal, discount, competitor and roadmap-date answers; learn the vocabulary | [Renewals](./README.md#renewals-expansion-and-commercial-judgment-1) |
| 10 | Book of business | 15 accounts, two Sev 1s, ticket triage exercise | [Book of business](./README.md#managing-a-book-of-business-1) |
| 11 | Stories | Write four STAR stories and say each aloud twice | [10-behavioral](../../topics/10-behavioral/README.md#customer-focus), [STORY-BANK.md](../../my-prep/STORY-BANK.md) |
| 12 | Mock 2 | QBR presentation plus behavioral mock | [Googleyness answers](./README.md#googleyness-values-and-behavioral-role-specific-1) |
| 13 | Company research | Support tiers, severity definitions, values; questions to ask | [11 checklist](../../topics/11-company-specific/README.md#company-research-checklist-do-for-every-company), [14](../../topics/14-questions-to-ask/README.md) |
| 14 | Light review | Re-read cheat sheets, sleep well | [Prep checklist](./README.md#prep-checklist) |

---

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–15 | One model answer from this role's README, read then answered aloud without looking |
| 15–45 | Main study item for the week (cloud topic, escalation, QBR, commercial) |
| 45–70 | Hands-on: work on the week's deliverable (scorecard, incident summary, QBR slides) |
| 70–85 | Speak one behavioral story or one role-play answer into a recorder, then listen back |
| 85–90 | Note one thing to fix tomorrow |

On weekends, use one longer block for the week's mock interview.

---

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Recruiter screen (30 min) | Peer or friend |
| 2 | Technical troubleshooting: 502 / DNS / IAM (45 min) | Peer with cloud experience; record it |
| 3 | Escalation role-play plus written follow-up (50 min) | Peer playing an angry executive |
| 4 | QBR presentation (45 min) | Peer, ideally someone in sales or customer success |
| 5 | Hiring manager and behavioral (45 min) | Peer, or a paid coach on a platform such as Exponent or interviewing.io |
| 6 | Full loop (four rounds over two days) | Mix of peers; record at least one round |

Free and paid options: practise with friends or ex-colleagues in customer success; record yourself on video for the QBR; platforms such as Pramp, interviewing.io and Exponent offer peer or coached mocks (check which ones cover customer-facing roles). Do at least one mock per week and at least two role-plays in total.

---

## Fast track for the repo owner's profile

- **Skim:** API design, authentication (JWT) and rate limiting in [08 section B](../../topics/08-cloud-devops-integration/README.md#b-authentication-authorization--security), and basic SQL for usage data. You have built these; spend the time on explaining them to customers instead.
- **Skim:** delivery and prioritisation questions. Leading a squad, sprint and release planning, and monthly programme reports already give you answers for "how do you organise many threads".
- **Biggest gap 1 — external customers:** most of your stakeholder work has been internal (sales, finance, academic teams). Frame these as "internal customers" honestly, and do extra role-plays with an external-customer framing.
- **Biggest gap 2 — executive presentation:** live demo and presentation practice with external customers is a known gap. Do the Week 4 QBR recording twice and get feedback from someone outside engineering.
- **Biggest gap 3 — cloud breadth and certification:** your evidenced cloud use is AWS S3 and SQS. Pick AWS as your "one cloud" for Week 2 and consider a foundational AWS or Google Cloud certification if the target company values it.
- **Biggest gap 4 — commercial vocabulary:** learn retention and expansion terms in Week 4. Your work on invoicing and payments tied to a cash-collection metric is a good bridge into renewal conversations.
- **Round mapping — escalation and troubleshooting:** the bidirectional legacy-to-new database bridge across six product lines (idempotency guards, ordering constraints, reconciliation) is your strongest "hard problem under pressure" story.
- **Round mapping — QBR / executive:** monthly programme reports map directly to the QBR exercise: you already turn delivery data into a story for leadership.
- **Round mapping — technical advisory:** your contribution to the speech-to-text provider evaluation (buy-vs-build cost model, data-protection constraints) fits "architecture review" and "cost optimisation" questions.
- **Round mapping — behavioral:** mentoring and onboarding plans, plus row-level access control work, give "influenced product or engineering" and "said no for the customer's good" stories. Keep them in [STORY-BANK.md](../../my-prep/STORY-BANK.md).

---

## Resources

- *The Customer Success Professional's Handbook* by Ashvin Vaidyanathan and Ruben Rabago — account health, success plans, QBRs.
- *Customer Success* by Nick Mehta, Dan Steinman and Lincoln Murphy — retention and the business side of post-sale work.
- *Site Reliability Engineering* (Google) — incident management and post-mortem chapters; free online at https://sre.google/books/
- AWS Well-Architected Framework (https://aws.amazon.com/architecture/well-architected/) or the Google Cloud Architecture Framework — the basis for best-practice reviews.
- AWS Skill Builder and Google Cloud Skills Boost — official free and paid cloud learning paths, including foundational certifications.
- Public status pages and post-incident reports from major cloud and SaaS providers — practice material for incident summaries.
