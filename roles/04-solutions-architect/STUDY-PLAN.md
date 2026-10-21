# Study Plan — Solutions Architect

For engineers preparing for a Solutions Architect loop at a cloud provider, data vendor or AI lab; assumes about 10–12 hours a week for six weeks.

Read the role overview first: [README.md — The role](./README.md#the-role) and the [typical interview loop](./README.md#typical-interview-loop). SA loops centre on a customer architecture whiteboard, a domain deep dive, a presentation round and structured behavioural questions (Leadership Principles at AWS). This plan builds cloud breadth first, then whiteboard method, then the specialist topics SA interviews love: Well-Architected, landing zones, migration, disaster recovery and cost.

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim areas you rate 4 or 5.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Core cloud services | | You can name and choose between the main compute, storage, database, messaging and networking services of one cloud, and explain VPCs, subnets and IAM. |
| Whiteboard method | | In 35 minutes you gather requirements, draw a design with named managed services, and cover HA, security and cost without being asked. |
| Well-Architected and reviews | | You can name the six pillars with a design principle each, and review a weak architecture into a prioritised list of fixes. |
| Multi-account and landing zones | | You can draw an organisation structure with OUs, guardrails, central logging and identity from memory. |
| Migration | | You can explain the 7 Rs and plan a large VM and database migration in waves, with minimal-downtime database cutover. |
| HA and disaster recovery | | You can match the four DR strategies to RTO, RPO and cost, and design for a stated RPO/RTO. |
| Cost optimisation | | You can find waste in a bill and explain Savings Plans, Reserved and Spot Instances. |
| Security and compliance | | You can explain shared responsibility, encryption and key management, and the shape of a HIPAA or PCI DSS workload. |
| Presenting | | You can present an architecture to a CTO in 15 minutes with one diagram and handle disagreement. |
| Behavioural (Leadership Principles) | | You have two STAR stories for each of the principles you expect, especially customer obsession, dive deep and disagree-and-commit. |

## 6-week plan

Assumes ~10–12 hours a week. Pick one cloud (usually the employer's) and use its service names everywhere; mention equivalents only when asked.

### Week 1 — Cloud foundations, hands-on

**Goals:** breadth across compute, storage, network, identity and databases, learned by building.

- [ ] Cloud fundamentals: [08 — C. Cloud](../../topics/08-cloud-devops-integration/ANSWERS.md#c-cloud) and [D. Containers, orchestration and IaC](../../topics/08-cloud-devops-integration/ANSWERS.md#d-containers-orchestration--iac).
- [ ] Networking and databases: [13 CS fundamentals — Networking](../../topics/13-cs-fundamentals/ANSWERS.md#networking) and [Databases](../../topics/13-cs-fundamentals/ANSWERS.md#databases).
- [ ] Read [Role and customer engagement — model answers](./README.md#role-and-customer-engagement-1), including [how an SA differs from an SE and FDE](./README.md#how-is-a-solutions-architect-different-from-a-solutions-engineer-and-a-forward-deployed-engineer).
- [ ] Work Tier 1 and Tier 2 of [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md#tier-2--most-technical-loops).

**Deliverable:** in a free-tier account, a multi-AZ web application behind a load balancer with a managed database, built with infrastructure as code (Terraform or the cloud's own tool), then torn down. Keep the code in a public repo.

**Mock:** a 30-minute technical phone screen with a peer covering breadth: "what happens when you type a URL", VPC basics, IAM, choosing a database.

**End of week check:** you can draw a VPC with public and private subnets across two AZs and explain each component's job.

### Week 2 — The whiteboard method

**Goals:** a repeatable whiteboard approach, used on the classic SA prompts.

- [ ] Method and follow-ups: [03 System design — A. Applied / customer-context prompts](../../topics/03-system-design/ANSWERS.md#a-applied--customer-context-design-prompts), [C. Classic system design](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked) and [D. Follow-up questions](../../topics/03-system-design/ANSWERS.md#d-follow-up-questions-to-expect).
- [ ] Read [How do you gather requirements before drawing any architecture?](./README.md#how-do-you-gather-requirements-before-drawing-any-architecture)
- [ ] Work the README whiteboards: [Black Friday web app](./README.md#whiteboard-design-a-highly-available-web-application-on-aws-for-a-retailer-expecting-10x-traffic-on-black-friday), [serverless order pipeline](./README.md#whiteboard-design-a-serverless-event-driven-order-processing-pipeline), [multi-tenant SaaS](./README.md#whiteboard-design-a-multi-tenant-saas-architecture-and-explain-silo-pool-and-bridge-models).

**Deliverable:** three clean architecture diagrams (one per whiteboard above), each with a five-line trade-off note. Re-draw one from memory at the end of the week.

**Mock:** a 45-minute whiteboard of the Black Friday prompt, timed: requirements (5), diagram (15), trade-offs, HA/DR, security and cost (15), questions (10).

**End of week check:** you ask about traffic, data, compliance, budget and team skills before drawing anything.

### Week 3 — Well-Architected, security and landing zones

**Goals:** review architectures, not just draw them; know the enterprise foundations.

- [ ] Read [Well-Architected and architecture reviews — model answers](./README.md#well-architected-and-architecture-reviews-1), including the [single-EC2 review exercise](./README.md#architecture-review-exercise-a-customer-runs-its-app-and-mysql-on-one-public-ec2-instance-stores-uploads-in-a-public-s3-bucket-and-has-access-keys-in-the-code-critique-the-design).
- [ ] Read [Multi-account and landing zones — model answers](./README.md#multi-account-and-landing-zones-1).
- [ ] Read [Security and compliance — model answers](./README.md#security-and-compliance-1) and [08 — B. Authentication, authorization and security](../../topics/08-cloud-devops-integration/ANSWERS.md#b-authentication-authorization--security).
- [ ] Read the official AWS Well-Architected pillar summaries (see Resources).

**Deliverables:**
1. A written review report for the single-EC2 exercise: findings ranked by risk, each with a fix and effort estimate.
2. A landing zone diagram (organisation, OUs, guardrails, identity, central logging, networking hub) drawn from memory.

**Mock:** a 45-minute architecture review: a partner describes a flawed architecture and you lead the review, then present the top five fixes.

**End of week check:** you can name the six pillars with one design principle each, and explain preventive versus detective guardrails.

### Week 4 — Migration, HA/DR and cost

**Goals:** the topics that separate SAs from general engineers.

- [ ] Read [Migration and modernisation — model answers](./README.md#migration-and-modernisation-1): the [7 Rs](./README.md#explain-the-7-rs-of-migration-and-how-you-choose-between-them), [500 VMs and a 20 TB Oracle database](./README.md#plan-the-migration-of-500-virtual-machines-and-a-20-tb-oracle-database-to-the-cloud) and [minimal-downtime database migration](./README.md#how-do-you-migrate-a-production-database-with-minimal-downtime).
- [ ] Read [High availability and disaster recovery — model answers](./README.md#high-availability-and-disaster-recovery-1).
- [ ] Read [Cost optimisation — model answers](./README.md#cost-optimisation-1).
- [ ] Hybrid connectivity: [the hybrid whiteboard](./README.md#whiteboard-design-hybrid-connectivity-between-a-customers-data-centres-and-the-cloud) and the [Hadoop analytics migration](./README.md#whiteboard-design-a-cloud-analytics-platform-for-a-customer-leaving-an-on-premises-hadoop-cluster).

**Deliverables:**
1. A two-page migration plan for the 500-VM scenario: discovery, wave plan, 7 Rs per workload group, database cutover, rollback and success measures.
2. A DR design for "RPO 15 minutes, RTO 1 hour" with a cost comparison against the next-cheaper strategy.

**Mock:** a 45-minute migration whiteboard where the partner adds "the CFO wants it done in three months" halfway through ([the lift-and-shift question](./README.md#a-customer-wants-to-lift-and-shift-everything-in-three-months-and-modernise-later-what-do-you-advise)).

**End of week check:** you can recite the four DR strategies with RTO, RPO and cost, and the pricing models, without notes.

### Week 5 — Applied AI architecture and the presentation round

**Goals:** handle the AI deep dive (now common) and prepare the presentation you will give.

- [ ] Read [Applied AI solutions architecture — model answers](./README.md#applied-ai-solutions-architecture-1).
- [ ] Study [06 AI/LLM — C. RAG](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation), [E. Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important) and [part 2 — G. Production LLM engineering](../../topics/06-ai-llm/ANSWERS-part2.md#g-production-llm-engineering).
- [ ] Do two prompts from [03 System design — B. AI / LLM prompts](../../topics/03-system-design/ANSWERS.md#b-ai--llm-system-design-prompts).
- [ ] Read [Presenting and influencing — model answers](./README.md#presenting-and-influencing-1), including [writing an ADR or design document](./README.md#how-do-you-write-an-architecture-decision-record-or-design-document-for-a-customer).

**Deliverables:**
1. A 15-minute presentation of an architecture you designed in real life: context, one diagram, key decisions and trade-offs, cost, results, what you would change.
2. A small RAG application on a managed model service with a 20-case eval set and a monthly token cost estimate (required for Applied AI / generative AI SA roles; optional otherwise).

**Mock:** a presentation round: 15 minutes presenting, 15 minutes of challenge questions from a partner playing a CTO who [disagrees with your recommendation](./README.md#the-customers-cto-disagrees-with-your-recommended-architecture-how-do-you-handle-it).

**End of week check:** you finish the presentation in 15 minutes and can defend one decision you would now make differently.

### Week 6 — Leadership Principles behavioural and full loop

**Goals:** structured behavioural answers and a full rehearsal.

- [ ] Read [Leadership-principle style behavioral — model answers](./README.md#leadership-principle-style-behavioral-1).
- [ ] Study [10 — Customer focus](../../topics/10-behavioral/ANSWERS.md#customer-focus), [Problem solving and technical judgment](../../topics/10-behavioral/ANSWERS.md#problem-solving--technical-judgment), [Conflict and collaboration](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration) and [Ownership and impact](../../topics/10-behavioral/ANSWERS.md#ownership--impact).
- [ ] Customer pushback: [09 — B. Difficult situations](../../topics/09-customer-scenarios/ANSWERS.md#b-difficult-situations) and [the competitor-service question](./README.md#the-customer-wants-a-service-your-platform-lacks-but-a-competitor-offers-what-do-you-do).
- [ ] Prepare questions: [14 — About the team and product](../../topics/14-questions-to-ask/ANSWERS.md#about-the-team--product).
- [ ] Tick off the role's [Prep checklist](./README.md#prep-checklist).

**Deliverable:** a story matrix: rows are the principles you expect (customer obsession, ownership, dive deep, earn trust, have backbone / disagree and commit, invent and simplify), columns are two stories each, with the measurable result for each story.

**Mock:** a full mock loop: technical screen (30), whiteboard (45), presentation (30), and a behavioural round run in the "bar raiser" style, where the interviewer keeps asking "what exactly did you do?" (45).

**End of week check:** every story has a clear "I" action and a result, and survives three follow-up "why" questions.

## 2-week crash plan

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and breadth | Read the role; refresh cloud basics | [README — role](./README.md#the-role), [08 — C](../../topics/08-cloud-devops-integration/ANSWERS.md#c-cloud) |
| 2 | Networking | VPCs, DNS, load balancing, hybrid connectivity | [13 — Networking](../../topics/13-cs-fundamentals/ANSWERS.md#networking), [hybrid whiteboard](./README.md#whiteboard-design-hybrid-connectivity-between-a-customers-data-centres-and-the-cloud) |
| 3 | Whiteboard method | Requirements questions; Black Friday prompt | [Requirements answer](./README.md#how-do-you-gather-requirements-before-drawing-any-architecture), [Black Friday](./README.md#whiteboard-design-a-highly-available-web-application-on-aws-for-a-retailer-expecting-10x-traffic-on-black-friday) |
| 4 | Whiteboards | Serverless pipeline; multi-tenant SaaS | [Reference architecture whiteboards](./README.md#reference-architecture-whiteboards-1) |
| 5 | Well-Architected | Six pillars; single-EC2 review | [Well-Architected answers](./README.md#well-architected-and-architecture-reviews-1) |
| 6 | Landing zones and security | Draw a landing zone; encryption and keys | [Landing zones](./README.md#multi-account-and-landing-zones-1), [Security](./README.md#security-and-compliance-1) |
| 7 | Mock day | Whiteboard mock + review mock | [Mock schedule](#mock-interview-schedule) |
| 8 | Migration | 7 Rs; database cutover | [Migration answers](./README.md#migration-and-modernisation-1) |
| 9 | HA/DR | Four strategies; RPO/RTO design | [HA and DR answers](./README.md#high-availability-and-disaster-recovery-1) |
| 10 | Cost | Bill doubled; pricing models | [Cost answers](./README.md#cost-optimisation-1) |
| 11 | AI architecture | LLM in production; prompting vs RAG vs fine-tuning | [Applied AI answers](./README.md#applied-ai-solutions-architecture-1), [06 — C](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation) |
| 12 | Presentation | Build and rehearse the 15-minute presentation | [Presenting answers](./README.md#presenting-and-influencing-1) |
| 13 | Behavioural | Story matrix; full mock loop | [LP behavioral answers](./README.md#leadership-principle-style-behavioral-1), [my story bank](../../my-prep/STORY-BANK.md) |
| 14 | Rest and review | Redraw three diagrams from memory | [README checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–15 | Flashcards: services, limits, DR strategies, pricing models, the 7 Rs. |
| 15–55 | One whiteboard prompt, timed, drawn on paper or a whiteboard tool and narrated out loud. |
| 55–75 | The week's topic: one README model answer, then a sibling question without notes. |
| 75–90 | One Leadership Principle story out loud; tighten the result line. |

Weekends: hands-on building in the free-tier account and the week's mock.

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Cloud breadth phone screen | A peer engineer with cloud experience |
| 2 | Timed whiteboard | A senior engineer or practising SA; interviewing.io and Exponent offer system design mocks |
| 3 | Architecture review | A peer who prepares a deliberately flawed design |
| 4 | Migration whiteboard with a changing constraint | A peer playing a CFO-pressured CTO |
| 5 | Presentation round | Two people: one technical, one executive |
| 6 | Full loop with bar-raiser-style behavioural | Two or three partners; Exponent has behavioural mocks in this style |

Do one whiteboard every week from Week 2, regardless of theme, and at least one recorded behavioural answer a week.

## Fast track for the repo owner's profile

- **Skim:** API, integration and data topics, and database migration with minimal downtime; the bidirectional legacy MySQL to new Postgres bridge across six product lines (idempotency guards, ordering constraints, reconciliation) is direct evidence. Use it as the [minimal-downtime migration](./README.md#how-do-you-migrate-a-production-database-with-minimal-downtime) answer.
- **Skim:** multi-tenant and access-control design; row-level access control experience covers much of the multi-tenant SaaS prompt.
- **Biggest gap 1 — cloud breadth and certifications.** Evidenced AWS use is S3 and SQS, and certifications are unknown. Give Weeks 1, 3 and 4 full time, and consider the AWS Solutions Architect – Associate exam as structured study.
- **Biggest gap 2 — landing zones, DR and cost at enterprise scale.** No evidence yet; learn them from the README and build in the free tier so answers come from practice.
- **Biggest gap 3 — external customer presentations.** Most stakeholder work has been internal. Rehearse the Week 5 presentation with people who interrupt.
- **Presentation round:** present the API gateway (JWT auth, rate limiting) with the aggregation/BFF layer across the platform of about 13 services; it has a diagram, real trade-offs and a clear story.
- **Whiteboard round:** the order/subscription/commerce platform (SKU catalogue, discount approval rules engine, invoicing tied to cash collection) is a ready-made example for the serverless order pipeline and HA prompts.
- **Behavioural (dive deep, ownership, invent and simplify):** the legacy modernisation (PHP 5.3 to 7.3 user migration), CRM/ERP integrations and delivery leadership of a squad of about four engineers.
- **Applied AI deep dive:** the speech-to-text provider evaluation (a contribution, not sole authorship) with a buy-vs-build cost model and data-protection constraints fits "help a customer choose an AI approach".
- Keep all stories in [../../my-prep/STORY-BANK.md](../../my-prep/STORY-BANK.md).

## Resources

- AWS Well-Architected Framework (https://aws.amazon.com/architecture/well-architected/) and the AWS Architecture Center.
- Microsoft Azure Well-Architected Framework and Google Cloud Architecture Framework, if interviewing with those providers.
- AWS Certified Solutions Architect – Associate and Professional exam guides (or the Azure and Google Cloud architect equivalents) as structured study.
- *Fundamentals of Software Architecture* by Mark Richards and Neal Ford.
- *Designing Data-Intensive Applications* by Martin Kleppmann.
- *System Design Interview* (volumes 1 and 2) by Alex Xu.
- Amazon's published Leadership Principles, for AWS behavioural rounds.
