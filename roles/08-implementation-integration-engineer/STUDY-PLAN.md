# Study Plan — Implementation / Integration Engineer

For engineers preparing for post-sale delivery roles (Implementation Engineer, Integration Engineer, Professional Services Consultant); assumes 6 weeks at about 10–12 hours a week, or the 2-week crash plan if an interview is already booked.

Role overview, loop and model answers: [README.md](./README.md). Questions every role gets: [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).

---

## Before you start

Spend 30 minutes rating yourself. Skim what you rate 4–5; spend extra time on 1–2.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Project delivery (kickoff to go-live) | | You can walk through phases, sign-offs and a realistic timeline for a 12-week implementation without notes. |
| Scope and change control | | You can read a SOW, spot red flags, and run a change request from request to signed estimate. |
| Data mapping and migration | | Given a messy CSV, you produce a field mapping table, cleansing rules and a list of customer questions in 30 minutes. |
| Reconciliation SQL | | You write anti-joins, `IS DISTINCT FROM` comparisons and control totals with a full outer join from memory. |
| API integration and idempotency | | You can write a CSV-to-REST loader with retries, idempotency keys, a failed-rows file and a dry-run flag. |
| ERP / CRM / HRIS knowledge | | You can whiteboard integration options, authentication and limits for at least two of Salesforce, SAP, Workday and NetSuite. |
| Testing, go-live and hypercare | | You can run a go/no-go meeting with three open UAT defects and justify the decision. |
| Customer workshops and stakeholders | | You can run a requirements workshop and handle a customer who wants their old process copied exactly. |

---

## 6-week plan

### Week 1 — How implementations run, end to end

**Goals:** speak the language of delivery: phases, SOWs, RAID logs, status reports.

- [ ] Read [The role](./README.md#the-role) and [Typical interview loop](./README.md#typical-interview-loop).
- [ ] Read the [Tier 1 questions](../COMMON-QUESTIONS.md#tier-1--asked-in-almost-every-loop) in COMMON-QUESTIONS.md.
- [ ] Work through [Onboarding and project delivery](./README.md#onboarding-and-project-delivery-1): [kickoff to go-live](./README.md#walk-me-through-how-you-run-a-customer-implementation-from-kickoff-to-go-live), [the kickoff meeting](./README.md#what-happens-in-a-kickoff-meeting-and-what-must-you-leave-with), [checking a SOW](./README.md#what-do-you-check-in-a-statement-of-work-sow-before-starting-a-project-and-what-are-the-red-flags) and [fixed price vs time and materials](./README.md#fixed-price-vs-time-and-materials-engagements-how-does-each-change-the-way-you-work).
- [ ] Read [tracking risks, assumptions, issues and dependencies](./README.md#how-do-you-track-risks-assumptions-issues-and-dependencies-on-an-implementation).
- [ ] Read [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) and adapt your answer to "why post-sale delivery?".

**Deliverables:**
1. A sample SOW scope section and a RAID log for a fictional 12-week implementation of a SaaS HR product.
2. The [weekly status report exercise](./README.md#exercise-write-a-one-page-weekly-status-report-for-an-implementation-that-is-two-weeks-behind): one page, for a project two weeks behind.

**Mock:** 30-minute recruiter plus hiring-manager mock: "walk me through a project you delivered end to end".

**End of week check:** you can explain the full implementation lifecycle in five minutes and name three SOW red flags.

### Week 2 — Data mapping, migration and reconciliation SQL

**Goals:** pass the SQL and data round, which is the most important technical round for this role.

- [ ] Read [07-data-and-sql](../../topics/07-data-and-sql/README.md) sections [A (SQL prompts)](../../topics/07-data-and-sql/README.md#a-sql-coding-prompts), [B (concepts)](../../topics/07-data-and-sql/README.md#b-sql-concepts) and [D (pipelines and data quality)](../../topics/07-data-and-sql/README.md#d-pipelines--data-quality), with [answers](../../topics/07-data-and-sql/ANSWERS.md).
- [ ] Work through [Data migration and mapping](./README.md#data-migration-and-mapping-1): [planning a migration](./README.md#walk-me-through-planning-a-data-migration-from-a-legacy-system-to-a-saas-product), [cleansing and deduplication](./README.md#how-do-you-cleanse-and-deduplicate-customer-data-before-a-migration) and [validating completeness](./README.md#how-do-you-validate-that-a-data-migration-is-complete-and-correct).
- [ ] Write the [reconciliation SQL](./README.md#write-sql-to-find-source-records-that-did-not-migrate-or-migrated-with-different-values) yourself before reading the answer, then compare.
- [ ] Read [cutover with the legacy system live](./README.md#how-do-you-plan-cutover-when-the-legacy-system-stays-live-until-go-live) and [how much history to migrate](./README.md#how-much-historical-data-should-you-migrate-and-how-do-you-treat-open-transactions).

**Deliverables:**
1. The [CSV mapping exercise](./README.md#exercise-map-this-customer-csv-export-to-our-products-schema-and-list-the-questions-you-would-ask) done on a real messy CSV (mixed date formats, codes, duplicates, blanks): mapping table, cleansing rules, customer questions.
2. A SQL file with three reconciliation queries (missing records, changed values, control totals), run against a local Postgres or SQLite database.

**Mock:** 45-minute SQL mock with a peer: deduplication, anti-join, NULL behaviour, then "how would you prove the migration is complete?".

**End of week check:** you can write reconciliation SQL from memory and explain a cutover plan with a frozen delta window.

### Week 3 — APIs, idempotent loaders and integration debugging

**Goals:** handle the integration and practical-coding rounds.

- [ ] Read [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) sections [A (APIs and integration)](../../topics/08-cloud-devops-integration/README.md#a-apis--integration) and [B (OAuth, SSO, SCIM)](../../topics/08-cloud-devops-integration/README.md#b-authentication-authorization--security), with [answers](../../topics/08-cloud-devops-integration/ANSWERS.md).
- [ ] Study [the idempotent CSV-to-REST loader](./README.md#code-write-an-idempotent-script-that-loads-records-from-a-csv-into-our-rest-api) and the practical problems in [02-coding section A](../../topics/02-coding/README.md#a-reported-fde-coding-problems) (parsing files, calling APIs, transforming JSON).
- [ ] Read [12-debugging-troubleshooting section B](../../topics/12-debugging-troubleshooting/README.md#b-production-troubleshooting-scenarios) for "data doesn't match the source", "duplicates after a re-run", "webhook isn't arriving", with [answers](../../topics/12-debugging-troubleshooting/ANSWERS.md).

**Deliverables:**
1. A working Python loader that reads a CSV and posts to a public test API with retries and backoff, idempotency keys, a failed-rows file and a `--dry-run` flag. Push it to a public repo.
2. A one-page debugging runbook for "records missing after the nightly sync".

**Mock:** 60-minute practical coding mock: a peer gives you a CSV and an API spec; you write the loader live and explain your error handling.

**End of week check:** you can write an idempotent loader in under 45 minutes and explain why re-running it does not create duplicates.

### Week 4 — ERP, CRM and HRIS integrations and solution design

**Goals:** whiteboard enterprise integrations and design documents.

- [ ] Work through [ERP, CRM and HRIS integrations](./README.md#erp-crm-and-hris-integrations-1): [two-way Salesforce sync](./README.md#design-a-two-way-sync-between-our-product-and-salesforce-what-can-go-wrong), [SAP](./README.md#what-are-the-options-for-integrating-with-sap-ecc-or-s4hana), [Workday](./README.md#how-do-you-get-employee-data-from-workday-into-our-product), [NetSuite](./README.md#how-do-you-integrate-with-netsuite) and [system of record per field](./README.md#how-do-you-decide-which-system-is-the-system-of-record-for-each-field).
- [ ] Work through [Requirements and solution design](./README.md#requirements-and-solution-design-1), especially [configuration vs customization](./README.md#configuration-vs-customization-how-do-you-decide-and-why-does-it-matter) and [what goes into a solution design document](./README.md#what-goes-into-a-solution-design-document).
- [ ] Read [03-system-design section A](../../topics/03-system-design/README.md#a-applied--customer-context-design-prompts) for customer-context design prompts.
- [ ] If the target company implements a specific platform, start its free admin learning path (for example Salesforce Trailhead).

**Deliverables:**
1. The [order-to-cash whiteboard](./README.md#exercise-whiteboard-an-order-to-cash-integration-between-a-crm-our-product-and-an-erp): a diagram plus a one-page note on system of record, sync direction, error handling and reconciliation.
2. A three-page solution design document for the same integration.

**Mock:** 45-minute integration design mock: a peer asks you to design a two-way CRM sync and pushes on conflicts, rate limits and deletes.

**End of week check:** you can explain integration options and limits for two enterprise platforms and defend which system owns each field.

### Week 5 — Scope, testing, go-live and difficult stakeholders

**Goals:** handle the case-study round: change requests, go/no-go, hypercare, and stakeholders who do not deliver.

- [ ] Work through [Scope and change control](./README.md#scope-and-change-control-1): [integration not in the SOW](./README.md#mid-project-the-customer-asks-for-an-integration-that-is-not-in-the-sow-walk-me-through-the-change-request-process), [small scope creep](./README.md#how-do-you-handle-small-scope-creep-that-does-not-seem-worth-a-formal-change-request) and [estimating a change request](./README.md#how-do-you-estimate-effort-for-a-change-request).
- [ ] Work through [Testing, go-live, hypercare and handoff](./README.md#testing-go-live-hypercare-and-handoff-1), including the [go/no-go with three open defects](./README.md#gono-go-meeting-uat-has-three-open-defects-how-do-you-decide).
- [ ] Work through [Partners and stakeholders](./README.md#partners-and-stakeholders-1).
- [ ] Read [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) sections [A (scoping)](../../topics/09-customer-scenarios/README.md#a-scoping--requirements) and [B (difficult situations)](../../topics/09-customer-scenarios/README.md#b-difficult-situations), with [answers](../../topics/09-customer-scenarios/ANSWERS.md).

**Deliverables:**
1. A written change request with an effort estimate, impact on timeline and options for the customer.
2. A go-live checklist and a cutover runbook template (hour-by-hour, owners, rollback point).

**Mock:** 45-minute case mock: a peer plays the customer's project manager who adds an integration two weeks before go-live and has not finished UAT.

**End of week check:** you can say no to scope politely, offer options, and make and justify a go/no-go call.

### Week 6 — Behavioral, company-specific prep and full loop

**Goals:** have your delivery stories ready and rehearse the whole loop.

- [ ] Work through [Behavioral (role-specific)](./README.md#behavioral-role-specific-1): a project you recovered, a customization you pushed back on, a migration or go-live that went wrong.
- [ ] Read [10-behavioral](../../topics/10-behavioral/README.md) sections [Ownership and impact](../../topics/10-behavioral/README.md#ownership--impact), [Failure and learning](../../topics/10-behavioral/README.md#failure--learning) and [Prioritisation and pressure](../../topics/10-behavioral/README.md#prioritisation--pressure), with [answers](../../topics/10-behavioral/ANSWERS.md).
- [ ] Write your stories into [STORY-BANK.md](../../my-prep/STORY-BANK.md).
- [ ] Run the [company research checklist](../../topics/11-company-specific/README.md#company-research-checklist-do-for-every-company): which platform, typical customers, partner ecosystem.
- [ ] Read the [Prep checklist](./README.md#prep-checklist); prepare questions on project length, concurrent projects and utilisation from [14-questions-to-ask](../../topics/14-questions-to-ask/README.md#about-the-role).

**Deliverables:**
1. Three STAR stories written and timed (under two minutes each).
2. A one-page "how I would run my first implementation here" note for the target company.

**Mock:** full mock loop over two days: SQL (45 min), integration design (45 min), case study (45 min), behavioral (45 min).

**End of week check:** you have done every round type at least twice and your loader and SQL are in a repo you can share.

---

## 2-week crash plan

About 2 hours a day.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and lifecycle | Read role and loop; explain kickoff-to-go-live aloud | [README](./README.md#the-role), [lifecycle answer](./README.md#walk-me-through-how-you-run-a-customer-implementation-from-kickoff-to-go-live) |
| 2 | SOW and RAID | SOW red flags; draft a RAID log and status report | [Onboarding and delivery](./README.md#onboarding-and-project-delivery-1) |
| 3 | SQL | Dedup, anti-joins, NULLs, control totals | [07 sections A, B](../../topics/07-data-and-sql/README.md#a-sql-coding-prompts), [07 answers](../../topics/07-data-and-sql/ANSWERS.md) |
| 4 | Migration | Plan, cleanse, validate, cutover; do the CSV mapping exercise | [Data migration](./README.md#data-migration-and-mapping-1) |
| 5 | APIs | Idempotency, pagination, webhooks, OAuth | [08 sections A, B](../../topics/08-cloud-devops-integration/README.md#a-apis--integration) |
| 6 | Code | Write the idempotent CSV-to-API loader | [Loader answer](./README.md#code-write-an-idempotent-script-that-loads-records-from-a-csv-into-our-rest-api) |
| 7 | Mock 1 | SQL plus loader mock with a peer | — |
| 8 | ERP/CRM/HRIS | Salesforce two-way sync, system of record, one of SAP/Workday/NetSuite | [Integrations](./README.md#erp-crm-and-hris-integrations-1) |
| 9 | Design | Order-to-cash whiteboard; config vs customization | [Order-to-cash](./README.md#exercise-whiteboard-an-order-to-cash-integration-between-a-crm-our-product-and-an-erp) |
| 10 | Scope and go-live | Change request, go/no-go, hypercare, handoff | [Scope](./README.md#scope-and-change-control-1), [Go-live](./README.md#testing-go-live-hypercare-and-handoff-1) |
| 11 | Debugging | Missing records, duplicates after re-run, webhooks | [12 section B](../../topics/12-debugging-troubleshooting/README.md#b-production-troubleshooting-scenarios) |
| 12 | Mock 2 | Case study: scope added late plus go/no-go | [09 section B](../../topics/09-customer-scenarios/README.md#b-difficult-situations) |
| 13 | Stories and company | Three STAR stories; company research; questions | [Behavioral](./README.md#behavioral-role-specific-1), [STORY-BANK.md](../../my-prep/STORY-BANK.md) |
| 14 | Light review | Re-read checklist and cheat sheets | [Prep checklist](./README.md#prep-checklist) |

---

## Daily routine

| Minutes | Activity |
|---|---|
| 0–20 | One SQL problem or one reconciliation query, written from scratch |
| 20–50 | Main study item for the week (migration, integrations, scope, go-live) |
| 50–75 | Hands-on: work on the week's artefact (loader, mapping table, RAID log, runbook) |
| 75–85 | Say one model answer or one behavioral story aloud |
| 85–90 | Log what to fix tomorrow |

---

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Recruiter plus "walk me through a project" (30 min) | Peer |
| 2 | SQL and data round (45 min) | Peer who writes SQL daily; or a SQL practice platform |
| 3 | Practical coding: CSV-to-API loader (60 min) | Peer; record your screen |
| 4 | Integration design whiteboard (45 min) | Peer with integration or architecture experience |
| 5 | Case study: change request and go/no-go (45 min) | Peer playing customer PM |
| 6 | Full loop (four rounds over two days) | Mix of peers; optionally a paid mock on interviewing.io or Exponent |

Options: ex-colleagues from professional services or delivery teams make the best case-study interviewers. Pramp, interviewing.io and Exponent offer peer and coached mocks for the coding and SQL rounds. Aim for one mock a week.

---

## Fast track for the repo owner's profile

This is one of the closest fits to your background.

- **Skip or skim:** data migration theory, idempotency, reconciliation and ordering constraints. The bidirectional legacy MySQL to new Postgres bridge across six product lines is exactly what Week 2 teaches; just practise telling it in interview form.
- **Skim:** CRM/ERP integration basics and contract-first API design. You have built CRM/ERP integrations and an API gateway with JWT auth and rate limiting.
- **Skim:** delivery mechanics. Sprint planning, release plans, monthly programme reports and onboarding plans cover most of Week 1.
- **Biggest gap 1 — external SOW and change-control vocabulary:** your projects were internal. Learn SOW, change request, fixed price vs time and materials, utilisation and hypercare terms so your stories sound like customer delivery.
- **Biggest gap 2 — specific enterprise platforms:** study Salesforce plus one of SAP, Workday or NetSuite in Week 4; take the free learning path for the target company's platform.
- **Biggest gap 3 — scripting language:** your evidenced stack is PHP, SQL and REST. Confirm whether you are comfortable writing the Week 3 loader in Python; if not, add extra practice time.
- **Round mapping — SQL and data:** the migration bridge and its reconciliation work.
- **Round mapping — integration design:** CRM/ERP integrations, the API gateway and the aggregation (BFF) layer across about 13 services.
- **Round mapping — case study / configuration vs customization:** the discount approval rules engine and SKU catalogue in the order and subscription platform. The legacy modernisation (PHP 5.3 to 7.3 user migration) fits cutover and go-live questions.
- **Round mapping — behavioral:** leading a small squad through these projects. Write the stories into [STORY-BANK.md](../../my-prep/STORY-BANK.md).

---

## Resources

- *Enterprise Integration Patterns* by Gregor Hohpe and Bobby Woolf — the standard vocabulary for messaging and integration design.
- *Designing Data-Intensive Applications* by Martin Kleppmann — chapters on replication, consistency and batch processing for sync and migration questions.
- Salesforce Trailhead (https://trailhead.salesforce.com) — free admin, integration and data-loading modules.
- Vendor developer docs for Workday, SAP and NetSuite integration options (REST, SOAP, file-based, iPaaS).
- iPaaS learning paths such as MuleSoft or Workato academies — for integration-platform roles.
- *Making Things Happen* by Scott Berkun — practical project management for engineers.
- PostgreSQL documentation (https://www.postgresql.org/docs/) — joins, `IS DISTINCT FROM`, window functions.
