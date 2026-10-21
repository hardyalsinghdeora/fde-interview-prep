# Implementation / Integration Engineer

The engineer who takes a signed customer from contract to a live, integrated, adopted system: scoping the work, configuring the product, migrating data, building integrations, and handing a stable customer over to support.

---

## The role

**Day to day.** Implementation and integration engineers deliver onboarding projects for new customers, usually several at once. A typical week includes:

- Running project calls: kickoff, requirements and design workshops, weekly status.
- Configuring the product to the customer's processes (workflows, fields, roles, rules, templates).
- Mapping and migrating customer data from legacy systems: extract, cleanse, transform, load, validate.
- Building or configuring integrations with the customer's ERP, CRM, HRIS and identity systems (Salesforce, SAP, Workday, NetSuite, Okta/Entra ID) through APIs, files or an iPaaS.
- Managing scope: statements of work (SOWs), change requests, timelines, risks and dependencies.
- Testing with the customer (system integration testing, user acceptance testing), running go-live and hypercare, then handing over to support.

**Titles and aliases.** Implementation Engineer, Implementation Consultant, Integration Engineer, Onboarding Engineer, Professional Services Consultant / Engineer, Technical Consultant, Solutions Consultant (post-sale meaning at some companies), Delivery Consultant, Integration Specialist.

**Who hires (examples, titles vary).** SaaS platform vendors with professional services teams (for example Salesforce, ServiceNow, Workday, Oracle NetSuite, SAP, HubSpot, Stripe, Zendesk); vertical SaaS companies (healthcare, fintech, logistics, HR, payroll) where every customer needs data migration and integration; and system integrators and consultancies (for example Accenture, Deloitte, Capgemini, Infosys, and specialist partner firms) that implement those platforms for clients.

**How it differs from an FDE.**

| | FDE | Implementation / Integration Engineer |
|---|---|---|
| Problem shape | Open-ended: find the use case, build new software | Defined: deliver what the SOW describes, on a plan |
| Main output | Custom code and new workflows at the customer | Configured product, migrated data, working integrations |
| Coding | Daily, production software | Medium: scripts, transformations, API integrations, SQL, sometimes platform-specific languages (Apex, SuiteScript) |
| Methods | Iterate fast, prototype | Project discipline: phases, sign-offs, change control |
| Product influence | High: field work becomes product | Low–Med: repeat requests become templates or product features |
| Commercial | Pilots and expansion | SOW-bound; change requests and services revenue |

**Who it suits.** People who like finishing things, enjoy structure, are detail-oriented about data, and like seeing many different customer businesses. A good route into FDE, solutions architecture or technical programme management.

**Skills mix.**

| Skill | Weight |
|---|---|
| Coding / scripting (Python, SQL, JavaScript, platform languages) | Med |
| Data mapping, migration and quality | High |
| Integration (REST, SOAP, files/SFTP, webhooks, iPaaS) | High |
| Customer communication and workshops | High |
| Project management (scope, timelines, risks) | High |
| Sales | Low (change requests and services upsell) |
| Product influence | Low–Med |

**Career paths.** Senior / Lead Implementation Engineer; Solutions Architect or Technical Architect; Engagement or Project Manager; Professional Services management; Solutions Engineer (pre-sales); Forward Deployed Engineer; Product Manager for integrations or platform; independent consultant for a specific platform.

---

## Typical interview loop

Loops vary by company and level. The rounds below are typical or reported, not guaranteed.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Delivery experience, platforms you know, travel, why post-sale delivery | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) |
| Hiring manager | Projects delivered, how you handle scope, timelines and difficult customers | [10-behavioral](../../topics/10-behavioral/README.md), this file |
| Technical: SQL and data | Joins, deduplication, reconciliation queries, data modelling | [07-data-and-sql](../../topics/07-data-and-sql/README.md) |
| Technical: integration and APIs | REST, auth, webhooks, files, error handling, idempotency, ERP/CRM knowledge | [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md), this file |
| Practical exercise or take-home | Map a CSV to a schema, write a load script, design an integration | [02-coding](../../topics/02-coding/README.md) (practical), this file |
| Case study / scenario | Plan an implementation, handle a change request, go/no-go decision | [04-decomposition](../../topics/04-decomposition/README.md), [09-customer-scenarios](../../topics/09-customer-scenarios/README.md), this file |
| Debugging | Failed integration, missing records, sync errors | [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) |
| Behavioral / values | Ownership, conflict, delivering under pressure | [10-behavioral](../../topics/10-behavioral/README.md) |

---

## Common rounds — use the shared sections

- [07-data-and-sql](../../topics/07-data-and-sql/README.md) — the most important technical section for this role. Prioritise deduplication, anti-joins, NULL behaviour, data modelling, schema drift, idempotent re-runs, backfills, data quality checks and entity resolution. See [answers](../../topics/07-data-and-sql/ANSWERS.md).
- [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) — section A (APIs and integration: idempotency, pagination, webhooks, legacy SFTP/CSV, flaky APIs, iPaaS, Salesforce/SAP/Workday) and section B (OAuth, SSO, SCIM, security reviews).
- [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) — section A (scoping, changing requirements, estimating) and section B (bad data with a fixed deadline, IT won't give access, sales over-promised, scope added without time).
- [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) — "data doesn't match the source system", "duplicates after a re-run", "webhook isn't arriving", "cron job didn't run".
- [02-coding](../../topics/02-coding/README.md) — practical coding (parsing files, calling APIs, transforming JSON) matters more than hard algorithms for this role.
- [10-behavioral](../../topics/10-behavioral/README.md) — prepare stories on missed deadlines, difficult stakeholders, and juggling several customers.
- [14-questions-to-ask](../../topics/14-questions-to-ask/README.md) — add questions about typical project length, concurrent projects per engineer, utilisation targets and how handoff to support works.

---

## Role-specific questions

### Onboarding and project delivery

- [ ] Walk me through how you run a customer implementation from kickoff to go-live.
- [ ] What happens in a kickoff meeting, and what must you leave with?
- [ ] How do you build a realistic implementation timeline?
- [ ] What do you check in a statement of work (SOW) before starting a project, and what are the red flags?
- [ ] Fixed-price vs. time-and-materials engagements: how does each change the way you work?
- [ ] How do you track risks, assumptions, issues and dependencies on an implementation?
- [ ] Exercise: write a one-page weekly status report for an implementation that is two weeks behind.
- [ ] You are running five implementations at once. How do you keep each on track without dropping anything?
- [ ] How would you reduce the average time to go-live across the implementation team?

### Requirements and solution design

- [ ] How do you run a requirements or design workshop for a SaaS implementation?
- [ ] Configuration vs. customization: how do you decide, and why does it matter?
- [ ] A customer insists their current process must be replicated exactly in the new system. How do you respond?
- [ ] What goes into a solution design document?

### Scope and change control

- [ ] Mid-project, the customer asks for an integration that is not in the SOW. Walk me through the change request process.
- [ ] How do you handle small scope creep that does not seem worth a formal change request?
- [ ] How do you estimate effort for a change request?

### Data migration and mapping

- [ ] Walk me through planning a data migration from a legacy system to a SaaS product.
- [ ] Exercise: map this customer CSV export to our product's schema and list the questions you would ask.
- [ ] How do you cleanse and deduplicate customer data before a migration?
- [ ] How do you validate that a data migration is complete and correct?
- [ ] Write SQL to find source records that did not migrate or migrated with different values.
- [ ] How do you plan cutover when the legacy system stays live until go-live?
- [ ] How much historical data should you migrate, and how do you treat open transactions?

### ERP, CRM and HRIS integrations

- [ ] Design a two-way sync between our product and Salesforce. What can go wrong?
- [ ] What are the options for integrating with SAP ECC or S/4HANA?
- [ ] How do you get employee data from Workday into our product?
- [ ] How do you integrate with NetSuite?
- [ ] How do you decide which system is the system of record for each field?
- [ ] Exercise: whiteboard an order-to-cash integration between a CRM, our product and an ERP.
- [ ] Code: write an idempotent script that loads records from a CSV into our REST API.

### Testing, go-live, hypercare and handoff

- [ ] What testing phases do you run before go-live, and who owns each?
- [ ] What is on your go-live checklist?
- [ ] Go/no-go meeting: UAT has three open defects. How do you decide?
- [ ] What is hypercare, and how do you run it?
- [ ] How do you hand a customer over from implementation to support?

### Partners and stakeholders

- [ ] How do you work with a system integrator (SI) that is leading the customer's programme?
- [ ] The customer's project manager is not completing their tasks. How do you handle it?
- [ ] How do you enable customer administrators so they can run the system after you leave?

### Behavioral (role-specific)

- [ ] Tell me about a time an implementation went off track and how you recovered it.
- [ ] Tell me about a time you pushed back on a customization request.
- [ ] Tell me about a time a data migration or go-live went wrong.

---

## Model answers

### Onboarding and project delivery

#### Walk me through how you run a customer implementation from kickoff to go-live.

I run it in phases with a sign-off at the end of each, so problems surface early.

1. **Pre-kickoff:** read the SOW, sales notes and contract; get the internal handoff from sales; identify gaps and risks before meeting the customer.
2. **Kickoff:** align on goals, scope, roles, timeline, governance and the customer's dependencies.
3. **Discovery and design (1–4 weeks):** requirements workshops, current-state processes, data sources, integrations. Output: a solution design the customer signs off.
4. **Build:** configure the product, build integrations, prepare migration scripts. Show progress to the customer in short demos, not just at the end.
5. **Data migration rehearsals:** at least one trial load into a test environment, with reconciliation.
6. **Testing:** system integration testing (SIT) by us, then user acceptance testing (UAT) by the customer against agreed scenarios.
7. **Training and readiness:** admin and end-user training, runbooks, support contacts.
8. **Go-live:** go/no-go decision, cutover plan, final migration, smoke tests.
9. **Hypercare (typically 2–4 weeks):** fast response to issues, daily check-ins at first.
10. **Handoff:** to support and the account team, with documentation and a formal project closure.

Throughout: weekly status reports, a risk log, and change control for anything outside scope.

#### What happens in a kickoff meeting, and what must you leave with?

The kickoff sets expectations for the whole project; most later problems trace back to something unclear here.

**Agenda (60–90 minutes):**
- Introductions and roles on both sides: executive sponsor, project manager, technical owners, data owners, admins, our team.
- Business goals and success criteria: why they bought, what "done" looks like.
- Scope walkthrough from the SOW, including what is explicitly **out** of scope.
- Approach and timeline: phases, milestones, target go-live, key dates such as fiscal year-end or blackout periods.
- Customer responsibilities and dependencies: access, environments, data extracts, SMEs' time, decisions.
- Governance: weekly status call, escalation path, change request process, tools (shared tracker, documents).
- Risks already known.

**What I must leave with:**
- Named owners for each customer dependency, with dates.
- Agreed meeting cadence and escalation contacts.
- Confirmed go-live target and the assumptions it depends on.
- Dates for the first workshops.
- Written follow-up within 24 hours summarising all of the above.

#### How do you build a realistic implementation timeline?

Build it from work and dependencies, not from the date sales promised.

1. **Break scope into work packages:** configuration areas, each integration, each data object to migrate, testing, training.
2. **Estimate each** using past projects and three-point estimates (best, likely, worst). Integrations and data migration carry the most uncertainty.
3. **Map dependencies,** especially on the customer: access, data extracts, SME availability, security review, third-party vendors. Customer dependencies are the most common cause of delay.
4. **Find fixed dates:** go-live constraints (contract renewals of the old system, fiscal periods, peak seasons) and holiday periods.
5. **Add buffers** at phase boundaries, not hidden inside every task. 15–25% is common for integration-heavy projects.
6. **Plan at least one migration rehearsal** and a UAT window long enough for fix-and-retest cycles.
7. **Validate with the customer:** walk them through their tasks and confirm they have the people.

If the result does not meet the expected date, I present options: reduce scope for phase 1, add resources, or move the date. I do not compress testing to make the plan fit.

#### What do you check in a statement of work (SOW) before starting a project, and what are the red flags?

The SOW defines what we owe the customer and what they owe us. I read it before kickoff and raise problems internally first.

**What I check:**
- Scope: specific deliverables, number of integrations, data objects and record volumes, environments, training sessions.
- Out-of-scope list and assumptions.
- Customer responsibilities and dependencies.
- Acceptance criteria and sign-off process.
- Timeline, milestones and payment milestones.
- Commercial model (fixed price or time and materials) and hours budget.
- Change request process.

**Red flags:**
- Vague scope: "integrate with the customer's ERP" with no system, direction or objects named.
- "Unlimited" or undefined data migration ("migrate all historical data").
- No customer responsibilities or no assumptions section.
- Acceptance based on subjective satisfaction rather than testable criteria.
- Timeline set before discovery, especially with a fixed price.
- Features promised that the product does not have.

If I find red flags, I clarify with sales and my manager, and then confirm interpretation with the customer in writing early, ideally at kickoff, so it does not become a dispute at sign-off.

#### Fixed-price vs. time-and-materials engagements: how does each change the way you work?

| | Fixed price | Time and materials (T&M) |
|---|---|---|
| Who carries overrun risk | The vendor | The customer |
| Scope | Must be tightly defined | Can evolve |
| Change control | Strict: every addition is a change request | Lighter, but still track scope against budget |
| Reporting | Progress against deliverables and milestones | Hours burned against budget, plus progress |

**Fixed price:** I am strict about the documented scope and assumptions from day one. Any request outside scope goes through change control, otherwise margin disappears. I watch effort against the estimate weekly and raise variance early internally.

**T&M:** the customer pays for time, so I make burn visible weekly (hours used, remaining, forecast to completion) so there are no surprises. I still prioritise ruthlessly because budgets are usually capped in practice.

Either way, the customer experience should feel the same: clear plan, regular updates, and no surprises. Many projects mix them, for example a fixed-price core implementation plus a T&M bucket for change requests.

#### How do you track risks, assumptions, issues and dependencies on an implementation?

I keep a RAID log: Risks, Assumptions, Issues, Dependencies. It is a shared table reviewed in every weekly status call.

| Type | Meaning | Example |
|---|---|---|
| Risk | Might happen and would hurt | Customer SAP team may not be available in August |
| Assumption | Believed true, not confirmed | Customer data extract will be UTF-8 CSV |
| Issue | Happening now | Sandbox access still not provided, 5 days late |
| Dependency | Something we need from someone else | Okta admin to configure SSO app by 15 March |

Each entry has an owner, a due date, impact (on timeline, scope or cost), and a status or mitigation.

Practices that make it work:
- **Validate assumptions early;** each unconfirmed assumption is a hidden risk.
- **Escalate by rule:** an issue that blocks the critical path for more than a few days goes to the sponsor level.
- **Link it to the plan:** show which milestone each item threatens.
- **Keep it short and current;** a 200-row log nobody reads is useless.

#### Exercise: write a one-page weekly status report for an implementation that is two weeks behind.

What they are testing: honesty, clarity, and whether you bring a recovery plan rather than just bad news.

```
Project: Acme Corp implementation      Week 7 of 14      Overall: AMBER (was GREEN)

Summary
Go-live is at risk of a two-week slip (from 12 May to 26 May) because the customer
ERP extract arrived 9 days late and contained unexpected product codes. We propose a
recovery plan below that keeps 12 May for phase 1 if approved by Friday.

Progress this week
- Configuration: 90% complete, demo held with operations team on Tuesday.
- Salesforce integration: built and passing SIT.
- ERP data migration: first trial load done; 6% of order lines rejected (unmapped codes).

Next week
- Agree mapping for 340 unmapped product codes (needs customer data owner).
- Second trial migration on Thursday.

Decisions needed (by Friday)
1. Option A: go live 12 May with Salesforce integration and open orders only; migrate
   closed history by 26 May. Option B: move full go-live to 26 May.

Top risks / issues
- ISSUE: Product code mapping owner not named. Owner: customer PM. Due: Monday.
- RISK: UAT testers on holiday week of 5 May. Mitigation: confirm backups.

Budget: 62% of hours used, 55% of scope delivered. Forecast: within budget with option A.
```

Key points: status colour changes honestly, one-sentence summary, cause stated without blame, options with a decision deadline.

#### You are running five implementations at once. How do you keep each on track without dropping anything?

Concurrent projects are normal in this role; the skill is a system, not heroics.

- **One view of everything:** a portfolio tracker with each project's phase, next milestone, go-live date, RAG status and the top blocker. I review it every Monday and plan the week from it.
- **Know where each project is in its life cycle.** Projects in discovery or cutover need intense attention; projects waiting on customer data need a nudge and little else. Stagger go-lives so two cutovers never land in the same week if I can influence it.
- **Fixed rhythms per project:** a standing weekly status call and a weekly written report, so each customer knows when they will hear from me and I am not reacting to every message.
- **Batch and template:** reuse kickoff decks, mapping templates, test scripts and runbooks; block focus time for build work instead of fragmenting it between calls.
- **Push work to the right owner:** customer tasks tracked in the RAID log with dates; partners or colleagues for specialist work.
- **Raise capacity problems early:** if two projects hit critical phases at once, I tell my manager with dates and options (move a milestone, add help) before quality slips.

Interviewers often follow up with "what did you drop?". Have an honest example ready of something you deprioritised and how you told the customer.

#### How would you reduce the average time to go-live across the implementation team?

Treat it as a measurable process problem: find where time goes across many projects, then remove the biggest delays.

1. **Measure:** time in each phase across recent projects (contract signed → kickoff → design sign-off → first data load → UAT → go-live), plus waiting time on customer dependencies. Usually most of the delay is waiting, not building.
2. **Fix the start:** a better sales-to-implementation handoff, a pre-kickoff questionnaire, and requesting access, data extracts and SSO setup before kickoff, since those are the longest customer dependencies.
3. **Standardise:** packaged implementation templates per segment or industry (pre-built configuration, standard integrations, mapping templates, test scripts), so each project starts from about 80% done.
4. **Tooling:** reusable migration scripts and validation reports, connectors for the most common ERPs and HRIS, a self-service data-load tool for simple customers.
5. **Scope discipline:** a smaller phase 1 that goes live sooner, with the rest in phase 2.
6. **Feed product:** the configuration and customizations that every project repeats are candidates for product features or defaults.

Track the result (median time to go-live, and time to first value) per quarter, and watch quality too: faster go-lives with more hypercare issues are not a win.

### Requirements and solution design

#### How do you run a requirements or design workshop for a SaaS implementation?

Run it around the customer's real processes, and design to the product's standard capabilities wherever possible.

**Before:** send an agenda and pre-read; ask for sample data, current process documents and screenshots; make sure the right people attend (process owners and people who do the work, not only managers).

**During (2–3 hours per process area):**
1. **Current state:** walk through the process step by step with real examples, including exceptions and volumes.
2. **Pain points and goals:** what must improve.
3. **Show the standard product flow** for that process, ideally configured with their sample data.
4. **Fit-gap:** for each requirement, mark it as fits as-is, fits with configuration, needs a process change, needs an integration or extension, or is out of scope.
5. **Capture decisions and open questions** live on screen so attendees see what is recorded.

**After:** send notes within a day with decisions, open questions with owners, and a draft of the requirements list. Requirements get IDs so they can be traced through design, build and UAT.

The key skill is asking "why" behind each requirement and steering toward standard features without dismissing real needs.

#### Configuration vs. customization: how do you decide, and why does it matter?

**Configuration** uses the product's built-in settings: fields, workflows, rules, roles, templates, layouts. It is supported, survives upgrades, and customer admins can maintain it.

**Customization** adds custom code or extensions: scripts (for example Apex in Salesforce, SuiteScript in NetSuite), custom integrations, custom UI. It is more flexible but costs more to build, test, maintain and upgrade, and often needs developers the customer may not have.

How I decide:
1. Can the requirement be met by configuration, maybe with a small process change? Prefer that.
2. If not, does it support real business value or a legal or regulatory need? If it is only "the way we have always done it", challenge it.
3. If custom work is justified, is it isolated (an extension or integration using public APIs) rather than modifying core behaviour?
4. Who will own and maintain it after go-live? Is it in scope and priced?
5. Is this a common need across customers? Then raise it as a product request.

It matters because heavy customization is a major cause of late projects, costly upgrades and customers who cannot maintain their own system. A useful rule: "adopt standard, adapt where needed, customize only when justified".

#### A customer insists their current process must be replicated exactly in the new system. How do you respond?

Understand first, then guide. Sometimes they are right; often the old process exists because of limits in the old system.

1. **Ask why for each step:** is it required by regulation, by contract, by a downstream system, or by habit or an old workaround?
2. **Separate the outcome from the steps.** "Managers must approve spend over 10,000" is a requirement; "approvals happen through a spreadsheet emailed on Fridays" is not.
3. **Show the standard flow** with their data and walk through how it achieves the same outcome.
4. **Quantify the cost of replicating exactly:** build effort, timeline impact, upgrade and maintenance burden, and loss of future product features.
5. **Find a compromise:** standard flow plus a small configuration, or phase it (go live standard, revisit after 60 days of use).
6. **Escalate the decision, not the conflict:** if the process owner still insists, present the options and costs to the sponsor and let them decide. Document the decision.

I also involve end users, who are often more open to a better process than the manager defending the old one.

#### What goes into a solution design document?

The solution design document (sometimes called a configuration workbook or technical design) records what will be built, so the customer can sign it off and support can maintain it later.

Typical contents:
- **Overview:** goals, scope, phase, related documents (SOW, requirements list).
- **Business process flows:** future-state processes with diagrams.
- **Configuration design:** objects, fields, picklists, workflows, approval rules, roles and permissions, notifications, templates.
- **Data model and migration:** which objects are migrated, source-to-target mapping reference, volumes, transformation rules, history scope.
- **Integration design:** for each integration: systems, direction, trigger (real-time, scheduled, file), objects and fields, authentication, error handling, retry, volumes, monitoring and owners.
- **Security and access:** SSO, user provisioning, data access rules.
- **Non-functional requirements:** performance, availability, data retention, compliance.
- **Custom components:** any code, with justification.
- **Assumptions, open decisions and traceability** back to requirement IDs.

I keep it as a living document during build, then freeze a final version at go-live and include it in the support handoff.

### Scope and change control

#### Mid-project, the customer asks for an integration that is not in the SOW. Walk me through the change request process.

Treat the request positively, but make the impact explicit before anyone starts work.

1. **Acknowledge and capture:** "That makes sense; it is not in the current scope, so let me assess it properly." Log it as a change request with an ID.
2. **Clarify requirements:** systems, direction, objects, volumes, frequency, who owns the other system, deadline, and why it is needed now.
3. **Assess impact:** effort, timeline, cost, dependencies and risk to the current go-live. Check whether it can be phase 2.
4. **Propose options:** add now with a new date and cost; deliver after go-live as phase 2; trade it for a lower-priority item already in scope; or a lighter interim solution (for example a scheduled file export).
5. **Get written approval** from the customer's authorised person (usually the sponsor) and our side (project lead, sometimes sales or finance) before work starts.
6. **Update the plan:** SOW amendment or change order, timeline, RAID log, status report.

Never start work on a verbal "yes" or slip it in quietly. Unapproved scope is how fixed-price projects lose money and how customers end up surprised by a late go-live.

#### How do you handle small scope creep that does not seem worth a formal change request?

Small items add up; ten "quick" changes can cost a week. I still track them, but with a lighter process.

- **Log every request** in a scope or change log, even if it takes 30 minutes.
- **Agree a small-change allowance** at kickoff or in the SOW: for example, minor changes under two hours are absorbed up to a total budget, beyond which a formal change request is needed.
- **Show the running total** in the weekly status report, so the customer sees what has been absorbed.
- **Check the source:** if many small requests come from one area, requirements were probably incomplete; schedule a short workshop to close the gap.
- **Hold the line on risk:** a "small" change that touches an integration or the data model is not small, because it needs retesting.
- **Batch:** group minor items into a post-go-live backlog when they are not needed for launch.

Being flexible on genuinely small items builds goodwill; being transparent about them keeps that goodwill from turning into an expectation that everything is free.

#### How do you estimate effort for a change request?

Break it down, estimate the parts, include the hidden work, and state assumptions.

1. **Decompose:** design, configuration or build, data changes, testing, documentation, training, deployment and project overhead.
2. **Estimate each part** using similar past work. For integrations, consider: number of objects and fields, direction (one-way or two-way), authentication setup, error handling, volumes and the customer system's API quality.
3. **Add the work people forget:**
   - Regression testing of affected areas.
   - Changes to migration scripts or mappings.
   - Customer-side effort (their SME time, their IT team's work).
   - Coordination with third parties.
4. **Use a range** (for example 3–5 days) with a confidence level, and state the assumptions ("assumes the ERP exposes an order API; if file-based, add 2 days").
5. **Show impact on the plan,** not just effort: does it move go-live, or can it run in parallel?
6. **Review with a peer** for anything over a few days.

Then I track actuals against the estimate, which improves future estimates.

### Data migration and mapping

#### Walk me through planning a data migration from a legacy system to a SaaS product.

Treat migration as its own workstream with rehearsals, not a task the week before go-live.

1. **Inventory and scope:** which objects (customers, contacts, products, open orders, history), volumes, source systems and owners. Decide history scope early.
2. **Extract approach:** API, database export, report or file, and who runs it. Get a full sample extract early.
3. **Profile the data:** row counts, nulls, duplicates, formats, invalid values, orphan records. This usually changes the plan.
4. **Mapping:** source-to-target field mapping with transformation rules, default values and lookup tables (statuses, codes, users), signed off by the customer's data owner.
5. **Cleansing:** agree who fixes what (customer in the source, or us in transformation).
6. **Build the pipeline:** extract, transform, load in dependency order (parents before children), using external IDs so loads are re-runnable.
7. **Rehearse:** at least one or two full trial migrations into a test environment, timed, with reconciliation and customer validation.
8. **Cutover plan:** freeze window, final extract, delta handling, load sequence, validation, rollback criteria.
9. **Sign-off:** reconciliation report and customer acceptance.

The most common failures are late extracts, poor data quality discovered late, and no rehearsal.

#### Exercise: map this customer CSV export to our product's schema and list the questions you would ask.

What they are testing: attention to detail, understanding of data types and relationships, and asking good questions rather than guessing.

How I approach it:
1. **Profile the file:** count rows and columns, check the header, encoding (UTF-8 vs. Windows-1252), delimiter and quoting, date formats, empty values, duplicates of what looks like the key.
2. **Build a mapping table:**

| Source column | Target field | Transformation | Notes |
|---|---|---|---|
| `CUST_NO` | `customer.external_id` | trim | Check uniqueness |
| `NAME` | `customer.name` | trim, collapse spaces | Required |
| `STATUS` | `customer.status` | `A`→`active`, `I`→`inactive` | Unknown values? |
| `CREATED` | `customer.created_at` | parse `DD/MM/YYYY` to ISO 8601 | Time zone? |
| `PHONE` | `contact.phone` | normalise to E.164 | Contact or account level? |
| `REGION_CD` | `customer.region_id` | lookup table | Need code list |

3. **Flag gaps:** required target fields with no source, source columns with no target.

**Questions I would ask:**
- Which column is the unique key, and is it stable over time?
- What do the codes mean (status, region, type)? Is there a reference list?
- What does an empty value mean: unknown, not applicable, or zero?
- Which time zone are timestamps in?
- Are inactive or deleted records included, and should they migrate?
- Is this a full extract or a subset? How will we get the final one?
- Who signs off the mapping?

#### How do you cleanse and deduplicate customer data before a migration?

The principle: the customer owns their data quality decisions; I provide tooling, reports and rules.

**Cleansing:**
- **Profile and report:** invalid emails, missing required fields, bad formats, orphan records (orders with no customer), out-of-range values.
- **Classify each issue:** fix automatically in transformation (trimming, case, phone and date formats), fix in the source by the customer (wrong business data), or exclude (test or obsolete records) with the customer's approval.
- **Track decisions** in the mapping document so the rules are repeatable in every rehearsal.

**Deduplication:**
1. **Define a match rule** with the customer: exact key match, or fuzzy matching on name plus address, email domain or tax ID.
2. **Find candidates** with SQL or a script (normalise first: lower-case, remove punctuation and legal suffixes like "Ltd" or "Inc").
3. **Agree survivorship rules:** which record wins (most recent, most complete, from the main system) and which fields merge.
4. **Customer review** of uncertain matches; never auto-merge low-confidence matches.
5. **Keep a cross-reference** from old IDs to the surviving ID, so child records (contacts, orders) re-link correctly and history is traceable.

Cleansing in the source system before migration is usually better, but only if the customer has time; otherwise the transformation layer does it.

#### How do you validate that a data migration is complete and correct?

Validate at several levels, automate it, and have the customer sign off on the results.

1. **Completeness:** record counts per object, source vs. target, accounting for records excluded by agreed rules. Every source record is either loaded or in a reject file with a reason.
2. **Accuracy:** field-level comparison for a sample, plus full comparison of key fields (IDs, amounts, statuses) by joining source and target extracts.
3. **Financial and control totals:** sums of amounts, quantities, open balances by period or by customer must reconcile.
4. **Relationships:** children linked to the right parents (no orphan contacts or orders).
5. **Business validation:** the customer's users check known records and run real tasks: "open customer X's last five orders", "run the month-end report".
6. **Functional checks:** migrated data works in the product's workflows (open items can be processed).

I produce a reconciliation report each rehearsal and for the final load, with counts, totals, rejects and reasons, and the customer signs it as part of go-live acceptance.

#### Write SQL to find source records that did not migrate or migrated with different values.

Assume the source extract and a target export are both loaded into staging tables, linked by an external ID.

```sql
-- 1. Records in source that are missing in the target (anti-join)
SELECT s.source_id, s.customer_name
FROM stg_source_customers s
LEFT JOIN stg_target_customers t
       ON t.external_id = s.source_id
WHERE t.external_id IS NULL;

-- 2. Records that migrated but key values differ
--    IS DISTINCT FROM treats NULLs correctly (PostgreSQL, Snowflake, BigQuery and others)
SELECT s.source_id,
       s.mapped_status AS source_status, t.status AS target_status,
       s.credit_limit AS source_limit, t.credit_limit AS target_limit
FROM stg_source_customers s
JOIN stg_target_customers t
  ON t.external_id = s.source_id
WHERE s.mapped_status IS DISTINCT FROM t.status
   OR s.credit_limit IS DISTINCT FROM t.credit_limit;

-- 3. Control totals by a business dimension
SELECT COALESCE(s.region, t.region) AS region,
       s.total AS source_total, t.total AS target_total
FROM (SELECT region, SUM(open_balance) AS total FROM stg_source_customers GROUP BY region) s
FULL OUTER JOIN
     (SELECT region, SUM(open_balance) AS total FROM stg_target_customers GROUP BY region) t
  ON s.region = t.region
WHERE s.total IS DISTINCT FROM t.total;
-- NULL regions never match with '='; COALESCE(region, '(none)') in both subqueries keeps them comparable.
```

Points to mention: compare against the **mapped** value (after transformation), not the raw source value; normalise types and trimming before comparing; and also check the reverse direction (records in the target that have no source, which suggests duplicates or test data).

#### How do you plan cutover when the legacy system stays live until go-live?

The challenge is that data keeps changing while you migrate. Common approaches:

**Option 1: Big-bang with a freeze (simplest).**
- Agree a freeze window (for example Friday 18:00 to Monday 08:00) when the legacy system is read-only.
- Final full extract, load, validate, go/no-go, open the new system.
- Works when volumes allow a full load within the window.

**Option 2: Initial load plus delta (for large volumes or short windows).**
- Load the bulk of historical or static data days or weeks earlier.
- At cutover, extract only records created or changed since the initial load (using last-modified timestamps or change logs) and apply them.
- Needs reliable change detection, including deletes, and an idempotent upsert by external ID.

**Plan details either way:**
- A minute-by-minute cutover runbook with owners, timings from the rehearsal, and checkpoints.
- Business continuity during the freeze: manual workaround, or a backlog to enter after go-live.
- Go/no-go criteria and a **rollback plan**: if validation fails, the legacy system reopens.
- Communication to users about the freeze and the switch.
- Decommission or read-only access for the legacy system afterwards.

#### How much historical data should you migrate, and how do you treat open transactions?

Migrate what the business needs to operate and to meet legal requirements, not "everything because we can". More history means more mapping, more cleansing, longer loads and more risk.

**Questions to decide history scope:**
- What do users actually look up, and how far back (usually 1–3 years)?
- Legal and regulatory retention (tax, finance, healthcare) — can it be met by archiving the old data elsewhere?
- Do reports need year-over-year comparisons in the new system?
- Does the new product's data model even fit old records (statuses or products that no longer exist)?

**Common pattern:**
- **Master data** (customers, products, employees): migrate active records, plus inactive ones still referenced.
- **Open transactions** (open orders, unpaid invoices, active tickets, open opportunities): always migrate, in full detail, because the business must continue working on them. These need the most careful validation.
- **Closed history:** migrate a limited period in summary or full form, or archive it in a read-only store or data warehouse with a link from the new system.

Open transactions also need a clear rule at cutover, for example "orders created before Friday 18:00 are finished in the old system if they ship by Monday; others migrate".

### ERP, CRM and HRIS integrations

#### Design a two-way sync between our product and Salesforce. What can go wrong?

**Design:**
- **Scope per field:** for each object (Account, Contact, Opportunity) decide which system owns each field. Two-way sync should mean "some fields flow each way", rarely "both systems can edit everything".
- **Authentication:** a dedicated integration user with a least-privilege profile or permission set, connected through OAuth (for server-to-server, the JWT bearer flow or client credentials through a connected app; Salesforce is moving new integrations to External Client Apps, so check current guidance (verify)).
- **Salesforce to us:** Change Data Capture or Platform Events for near-real-time changes, or polling with `SystemModstamp` for simpler setups.
- **Us to Salesforce:** REST API upserts using an **External ID** field holding our ID, so retries do not create duplicates; Bulk API 2.0 for large loads and the initial sync.
- **ID cross-reference:** store the Salesforce ID in our product and our ID in Salesforce.
- **Error handling:** retries with backoff, a dead-letter queue, alerts, and a reconciliation job that compares both sides nightly.

**What can go wrong:**
- **Infinite loops:** a change synced to Salesforce triggers a change event that syncs back. Fix: ignore changes made by the integration user, or compare values before writing.
- **Conflicts:** both sides edited the same record; you need a rule (system of record or last-writer-wins by timestamp).
- **API limits:** Salesforce enforces daily API request limits per org; use Bulk API and batching.
- **Validation rules and triggers** in the customer's org reject or modify records.
- **Picklist mismatches** and required fields added later by the customer's admins.
- **Deletes and merges** in Salesforce that are not propagated.

#### What are the options for integrating with SAP ECC or S/4HANA?

SAP landscapes are usually owned by a dedicated customer SAP team (or an SI), so the first step is to meet them and learn what they already use.

Main options:
- **IDocs** (Intermediate Documents) over ALE: SAP's long-standing asynchronous message format for master data and transactions (for example material masters, orders, invoices). Reliable and familiar to SAP teams.
- **BAPIs / RFCs:** function calls into SAP business logic, called through SAP connectors or middleware.
- **OData services:** REST-style APIs, exposed through SAP Gateway. S/4HANA provides many standard APIs, documented on the SAP Business Accelerator Hub.
- **Middleware:** SAP Integration Suite (formerly Cloud Platform Integration) or a third-party iPaaS such as MuleSoft or Boomi, which many SAP customers already run.
- **Files:** scheduled flat-file or CSV exchange over SFTP; still common and often fastest to agree on.

How I choose: prefer what the customer's SAP team already supports and monitors; prefer standard APIs over custom ABAP development; consider volume and latency (batch IDocs or files for bulk, OData for real-time lookups); and plan for their change process, since SAP transports and testing cycles can take weeks. Build that lead time into the plan.

#### How do you get employee data from Workday into our product?

Common options, from simplest to most flexible:

1. **Through the identity provider:** many customers already provision users from Workday into Okta or Microsoft Entra ID. If our product supports SCIM, we receive users and basic attributes (name, email, department, manager) from the identity provider. Simple and secure, but limited to attributes the identity provider carries.
2. **Reports-as-a-Service (RaaS):** the customer's Workday team builds a custom report with exactly the fields we need and exposes it as a web service. We call it on a schedule and receive JSON, XML or CSV.
3. **Enterprise Interface Builder (EIB) or a scheduled file:** Workday generates an outbound file and delivers it to SFTP on a schedule.
4. **Workday Web Services (SOAP) or REST APIs:** for richer or real-time needs; more work.

Practical points:
- Access is through an **Integration System User (ISU)** with a security group that only allows the fields needed. The customer's Workday team configures this.
- Agree how to handle **future-dated hires and terminations**, rehires, contractors vs. employees, and leave.
- Handle **deactivation**: terminated workers must lose access promptly; this is often a security requirement.
- Treat HR data as sensitive PII: minimise fields, encrypt, restrict access, and document it for the customer's privacy review.

#### How do you integrate with NetSuite?

NetSuite offers several integration methods; the choice depends on volume, latency and what the customer's NetSuite admins support.

- **SuiteTalk REST web services:** record operations and **SuiteQL** queries over REST. Good default for new integrations.
- **SuiteTalk SOAP web services:** older and very complete, still used by many existing connectors, but Oracle has announced its retirement (verify the current timeline), so avoid it for new builds.
- **RESTlets:** custom REST endpoints written in SuiteScript and deployed in the customer's account. Useful when you need custom logic or to combine several operations in one call.
- **CSV import** and saved searches exported on a schedule, for simple batch needs or initial migrations.
- **iPaaS connectors** (for example Celigo, Boomi, Workato) that many NetSuite customers already use.

Practical points:
- **Authentication:** OAuth 2.0 (preferred for new integrations) or token-based authentication (TBA), which Oracle is phasing out for new integrations (verify), with a dedicated integration role that has only the permissions needed.
- **Concurrency limits:** NetSuite limits concurrent requests per account; design for queuing and retries.
- **Customizations:** customers often have custom fields, records and scripts (user event scripts can change or reject records you send); test against their sandbox.
- **Subsidiaries, currencies and accounting periods** matter for financial records; closed periods will reject postings.
- Use **external IDs** for idempotent upserts.

#### How do you decide which system is the system of record for each field?

The system of record (SoR) is the one authoritative source for a piece of data; other systems receive copies. Deciding it per entity, and sometimes per field, prevents conflicts and data fights.

How I decide, with the customer's process owners:
1. **Where is the data created** in the business process? Customer accounts are often created in the CRM; invoices in the ERP; employees in the HRIS.
2. **Who is accountable** for its accuracy? The team that owns the process usually owns the data.
3. **Which system enforces the rules** (validation, approvals, compliance)? Credit limits and tax data typically live in the ERP.
4. **Regulatory needs:** financial and HR data usually must be governed in the ERP or HRIS.

Then I document it in a **data ownership matrix**: entity or field, SoR, systems that consume it, sync direction, frequency, and conflict rule. For example: Account name (CRM, flows to ERP), Credit limit (ERP, flows to CRM, read-only there), Employee department (Workday, read-only in our product).

Fields owned elsewhere should be read-only in the consuming system where possible, so users cannot create conflicting edits.

#### Exercise: whiteboard an order-to-cash integration between a CRM, our product and an ERP.

What they are testing: clear data flows, ownership, error handling, and asking about the customer's process first.

Assume our product handles quoting or fulfilment, Salesforce is the CRM and NetSuite or SAP is the ERP.

```
 CRM (Salesforce)            Our product                     ERP (NetSuite / SAP)
 ----------------            -----------                     --------------------
 Account, Opportunity  --->  Customer, Order created   --->  Sales order created
 (closed-won event)          (validates, enriches)           Customer master (if new)
                                     |
                                     |  fulfilment status
                                     v
 Order status (read)   <---  Shipment / delivery       --->  Invoice created in ERP
                                                              |
 Invoice + payment     <------------------------------------  Payment received
 status (read)                                                (ERP is SoR for money)
```

**Key design points to state:**
- **Systems of record:** CRM owns accounts and opportunities; our product owns order fulfilment status; ERP owns customer financial data, invoices and payments.
- **Triggers:** event-driven where possible (CRM change events, our webhooks), scheduled batch for ERP if it is file-based.
- **IDs:** external IDs and a cross-reference so each order is traceable in all three systems.
- **Idempotency:** retrying "create sales order" must not create two orders.
- **Error handling:** retries, dead-letter queue, alert to the customer's ops team, and a dashboard of failed messages with a re-run button.
- **Reconciliation:** daily comparison of order counts and totals across systems.
- **Middleware:** if the customer has an iPaaS, route through it.

Questions I would ask first: who creates customers in the ERP, tax and currency handling, partial shipments, cancellations and returns, and volumes.

#### Code: write an idempotent script that loads records from a CSV into our REST API.

Idempotent means re-running it gives the same result: no duplicates. The key is an upsert by external ID, plus logging what failed so it can be re-run.

```python
import csv, json, sys, time
import requests

API = "https://api.example.com/v1/customers"
HEADERS = {"Authorization": "Bearer <token-from-secret-store>",
           "Content-Type": "application/json"}
RETRYABLE = {429, 500, 502, 503, 504}

def transform(row):
    return {
        "external_id": row["CUST_NO"].strip(),
        "name": " ".join(row["NAME"].split()),
        "status": {"A": "active", "I": "inactive"}[row["STATUS"].strip()],
    }

def backoff(resp, attempt):
    try:
        return int(resp.headers.get("Retry-After", ""))  # seconds form only
    except (AttributeError, ValueError):
        return min(2 ** attempt, 60)

def upsert(record, session, max_retries=5):
    # PUT to a resource keyed by external_id = create or replace, safe to repeat
    url = f"{API}/by-external-id/{record['external_id']}"
    resp = None
    for attempt in range(max_retries):
        try:
            resp = session.put(url, json=record, headers=HEADERS, timeout=30)
        except (requests.ConnectionError, requests.Timeout):
            resp = None                      # network blip: retry
        if resp is not None and resp.status_code not in RETRYABLE:
            return resp
        time.sleep(backoff(resp, attempt))
    return resp                              # last response, or None if never reached

def main(path):
    ok, failed = 0, []
    with requests.Session() as s, open(path, newline="", encoding="utf-8-sig") as f:
        for line_no, row in enumerate(csv.DictReader(f), start=2):
            try:
                record = transform(row)
            except (KeyError, ValueError, AttributeError) as e:
                failed.append({"line": line_no, "row": row, "error": f"transform: {e!r}"})
                continue
            resp = upsert(record, s)
            if resp is not None and resp.status_code in (200, 201, 204):
                ok += 1
            else:
                failed.append({"line": line_no, "row": row,
                               "status": getattr(resp, "status_code", None),
                               "error": resp.text[:500] if resp is not None else "no response"})
    with open("failed_rows.json", "w") as out:
        json.dump(failed, out, indent=2)
    print(f"loaded={ok} failed={len(failed)} (see failed_rows.json)")

if __name__ == "__main__":
    main(sys.argv[1])
```

Points to mention: if the API only offers POST, look up by external ID first or send an idempotency key; use a bulk endpoint for large files; never hard-code secrets; add a dry-run mode; respect rate limits; produce a reconciliation count at the end; and treat `failed_rows.json` as sensitive, since it holds raw customer rows.

### Testing, go-live, hypercare and handoff

#### What testing phases do you run before go-live, and who owns each?

| Phase | Purpose | Owner |
|---|---|---|
| Unit / build testing | Each configuration item, script or integration component works | Implementation team |
| System integration testing (SIT) | End-to-end flows across our product and the customer's systems, with realistic data, including error cases | Implementation team, with customer IT for their systems |
| Data migration testing | Trial loads, reconciliation, performance of the load | Implementation team; customer data owners validate |
| User acceptance testing (UAT) | Business users confirm the solution supports their processes, using agreed test scenarios | Customer (business users), supported by us |
| Performance / volume testing | Expected peak volumes, batch windows, API limits | Implementation team (when in scope) |
| Security / access testing | Roles and permissions, SSO, data visibility | Implementation team and customer security |
| Regression testing | After fixes or change requests, nothing else broke | Implementation team |

Good practice: write UAT scenarios with the customer early, traced to requirement IDs; agree defect severity definitions and exit criteria (for example, no open critical or high defects) before UAT starts; and track defects in one shared tool. UAT must be done by the customer's real users, otherwise it is not acceptance.

#### What is on your go-live checklist?

A go-live checklist turns cutover into a set of verified items with owners. Typical items:

**Readiness (before go/no-go):**
- UAT signed off; no open critical or high defects; known issues have workarounds documented.
- Final migration rehearsal passed with reconciliation signed off, and timing fits the cutover window.
- Production configuration matches the tested configuration (configuration migrated and checked).
- Integrations pointed at production endpoints, with production credentials stored in the secret store, and monitoring and alerts enabled.
- SSO and user provisioning working; users and roles loaded.
- Training completed; user guides and admin runbooks published.
- Support model ready: hypercare rota, contact list, escalation path, support team briefed.
- Communication sent to users (dates, freeze, what changes, where to get help).
- Rollback plan documented with decision criteria and a decision deadline.

**Cutover:**
- Legacy system frozen; final extract and load; validation and reconciliation.
- Smoke tests of key processes in production by named customer users.
- Go-live decision confirmed by the sponsor; announcement sent.

**After:**
- Monitor integrations and error queues closely in the first days; daily hypercare check-ins.

#### Go/no-go meeting: UAT has three open defects. How do you decide?

The number matters less than what the defects are. I would evaluate each one against criteria agreed before UAT.

For each defect:
1. **Severity and business impact:** does it stop a critical process (cannot invoice, cannot pay employees), cause wrong data, or create a compliance risk? Or is it cosmetic or a rare edge case?
2. **Workaround:** is there a practical, documented workaround the users can follow for a few weeks?
3. **Fix timeline and risk:** can it be fixed and retested before go-live, or soon after without destabilising the system?
4. **Frequency:** how many users or transactions does it affect?

Then decide:
- **No-go** if any defect blocks a critical process with no workaround, risks data integrity, or creates a legal or compliance problem.
- **Go with conditions** if the defects are medium or low, have workarounds, and fixes are scheduled during hypercare. Record them as known issues, communicate them to users, and get the sponsor to accept them in writing.

I also consider the cost of delay: frozen legacy systems, contract end dates, and team availability. The sponsor makes the final call; my job is to give a clear recommendation with the risks stated plainly.

#### What is hypercare, and how do you run it?

Hypercare is a period of intensified support right after go-live, usually 2–4 weeks, when users are new to the system and hidden issues surface. The implementation team stays engaged, rather than handing over immediately to standard support.

How I run it:
- **Clear entry and exit criteria** agreed in advance: for example, exit when no critical issues are open, ticket volume has fallen below an agreed level for a week, and month-end or first payroll has completed successfully.
- **Fast triage channel:** one intake route (a shared channel or support queue with a hypercare tag), with faster response targets than standard support.
- **Daily stand-up** with the customer's key users and IT in the first week, then less often.
- **Monitoring:** integration error queues, failed jobs, data quality checks, usage and login rates.
- **Issue log** with categories: defect, training gap, data issue, enhancement request. Training gaps are fixed with quick guides, enhancements go to a backlog, not into hypercare.
- **Watch first business cycles:** first invoice run, first month-end close, first payroll.
- **Exit:** summary report of issues, open items and their owners, then a formal handoff to support.

#### How do you hand a customer over from implementation to support?

A good handoff means support can help the customer without calling me. I treat it as a deliverable with a checklist.

**Handoff package:**
- Customer overview: goals, key contacts, sponsor, admins, communication preferences.
- Final solution design: configuration, integrations, data flows, custom components and why they exist.
- Environment details: instances, integration endpoints, credentials location (never the secrets themselves), monitoring dashboards.
- Runbooks: how to re-run failed integrations, common errors and fixes, scheduled jobs.
- Known issues, workarounds and open defects with their ticket references.
- Open enhancement requests and phase 2 scope.
- Hypercare summary: issues seen and how they were resolved.

**Process:**
- A handoff meeting with the support lead and the account manager or TAM to walk through it.
- A joint call with the customer to introduce the support team, explain how to raise tickets, severity levels and response targets.
- A short overlap period where I remain available to support for questions.
- Formal project closure: sign-off, lessons learned, and an update to internal templates if something new was built.

### Partners and stakeholders

#### How do you work with a system integrator (SI) that is leading the customer's programme?

In many enterprise projects the customer hires an SI (for example a large consultancy) to run the overall programme, and we deliver or support our product within it. The SI can be a strong ally or a source of friction, depending on how roles are set up.

- **Clarify roles early, in writing:** who configures our product, who builds integrations, who owns data migration, who tests, who trains. A RACI chart avoids gaps and duplicated work.
- **Respect their programme governance:** attend their status meetings, use their plan and RAID log, rather than running a parallel project.
- **Enable them:** give their consultants product training, best-practice guides and access to our experts, so they configure the product the right way.
- **Protect product quality:** review their design for our product, especially customizations, and raise concerns early with evidence. Their incentive may be to build more; ours is a supportable solution.
- **Keep a direct line to the customer** without going around the SI: a regular check-in with the customer's sponsor, agreed with the SI.
- **Escalate jointly** on issues, not against each other in front of the customer.

Good SI relationships also bring future projects, so I treat them as long-term partners.

#### The customer's project manager is not completing their tasks. How do you handle it?

Assume good intent first: they are often overloaded with their day job, or blocked internally.

1. **Make the impact visible and specific:** show in the RAID log which tasks are late and which milestone each one threatens. "The data extract is 8 days late, which moves the migration rehearsal and puts go-live at risk."
2. **Talk privately:** ask what is blocking them. Offer help: templates, doing part of the work ourselves if appropriate, breaking tasks into smaller pieces, or joining a meeting with their internal teams.
3. **Agree new dates** and confirm them in writing.
4. **Make dependencies part of the weekly status report,** so the sponsor sees them without it feeling like a complaint.
5. **Escalate if it continues:** a short meeting with the customer's sponsor, framed around the project, not the person: "we need more customer capacity or a revised timeline; here are the options".
6. **Protect our side contractually:** if the SOW states customer responsibilities, document delays, because they affect timeline and cost commitments.

The goal is to get the project moving, not to win the argument; the PM is someone you will work with for months.

#### How do you enable customer administrators so they can run the system after you leave?

Plan enablement from the start, not in the last week, so admins learn by doing during the project.

- **Identify admins early** and include them in design workshops, so they understand why things are configured the way they are.
- **Pair during build:** let them configure parts of the system with our guidance (for example new fields, roles or reports). People learn much more by doing than by watching.
- **Role-based training:** separate sessions for admins (configuration, user management, integrations monitoring) and end users (daily tasks).
- **Train-the-trainer** for large rollouts: customer champions train their own teams, with our materials.
- **Documentation tailored to their system:** admin runbook for common tasks (add a user, change a workflow, re-run a failed sync), with screenshots of their configuration rather than generic docs.
- **Certification or product learning paths** if the vendor offers them.
- **Check readiness:** before go-live, ask admins to perform key tasks without help, for example onboarding a new user or diagnosing a failed integration.

A system the customer cannot maintain becomes a support burden and a renewal risk.

### Behavioral (role-specific)

#### Tell me about a time an implementation went off track and how you recovered it.

**What they are looking for:** early detection, honest communication, a structured recovery plan, and learning that changed how you run projects.

**STAR outline:**
- **Situation:** [project, customer, what went off track: late dependencies, poor data, scope change, under-estimation; how far behind]
- **Task:** [your role and the constraint: fixed go-live, fixed price]
- **Action:** [how you found the root cause, re-planned, presented options to the sponsor, changed scope or resourcing, and tightened tracking]
- **Result:** [go-live outcome, customer satisfaction, cost impact]
- **Learning:** [what you now do on every project]

**Example (adapt to your own experience):** On a [N]-week implementation, the first trial migration showed that about 20% of the customer's product records had invalid unit codes, which would delay go-live by at least three weeks. I set the project to amber the same day, produced a profiling report, and offered the sponsor two options: go live on time with the active product catalogue only, or delay the whole launch. They chose phased go-live; we went live on the original date, and the remaining records were cleaned and loaded two weeks later. I now run data profiling in week one of every project instead of waiting for the first trial load.

#### Tell me about a time you pushed back on a customization request.

**What they are looking for:** that you protect the customer's long-term interest and supportability, use evidence rather than opinion, and still find a way to meet the underlying need.

**STAR outline:**
- **Situation:** [the request, who wanted it, why: often replicating an old process]
- **Task:** [why it was risky: cost, upgrade impact, maintainability, timeline]
- **Action:** [how you found the underlying need, showed a standard alternative, quantified the cost, and escalated the decision appropriately]
- **Result:** [what was decided and the outcome later]
- **Learning:** [how you handle such requests now]

**Example (adapt to your own experience):** A finance manager asked for a custom approval engine to copy their old five-level spreadsheet approval process, estimated at three weeks of build. In a workshop I asked why each level existed and found that two levels were no longer required by policy and the rest could be met by the product's standard approval rules with amount thresholds. I demonstrated the standard flow with their real data and showed the build and upgrade cost of the custom version. They accepted the standard flow; approvals took less time for users and there was no custom code to maintain.

#### Tell me about a time a data migration or go-live went wrong.

**What they are looking for:** calm incident handling, ownership without blame, clear customer communication, and concrete process changes afterwards.

**STAR outline:**
- **Situation:** [what went wrong: missing records, wrong mapping, integration failure on day one, load too slow for the cutover window]
- **Task:** [your role and the business impact]
- **Action:** [immediate containment (pause, rollback or fix forward), communication to the customer, root cause analysis, fix and validation]
- **Result:** [how quickly it was resolved, data impact, customer relationship afterwards]
- **Learning:** [checklist or rehearsal change you introduced]

**Example (adapt to your own experience):** After a go-live, users reported that about 300 customer records had the wrong payment terms. I paused the outbound integration to the ERP so the wrong terms would not reach invoices, and told the customer within the hour what we knew and when we would update them. The cause was a lookup table updated by the customer after our final rehearsal, which our cutover did not re-validate. We corrected the records with a script, reconciled against the source, and resumed the integration the same day. Since then, I freeze mapping tables at the last rehearsal and include a checksum check of every lookup table in the cutover runbook.

---

## Prep checklist

1. Practise mapping a messy CSV (mixed date formats, codes, duplicates, blank values) to a target schema and write the mapping table and the questions you would ask.
2. Write the reconciliation SQL from this file from memory: anti-join for missing records, `IS DISTINCT FROM` comparisons, control totals with a full outer join.
3. Write and run a small idempotent CSV-to-API loader in Python against a public test API, with retries, a failed-rows file and a dry-run flag.
4. Learn the integration options for at least two of Salesforce, SAP, Workday and NetSuite well enough to explain them on a whiteboard, including authentication and limits.
5. Draft a sample SOW scope section, a change request, a RAID log and a weekly status report for a fictional project; these artefacts often come up as exercises.
6. Prepare a go-live checklist and a cutover runbook template, and be ready to walk through a go/no-go decision.
7. Prepare three stories: a project you recovered, a customization you pushed back on, and a migration or go-live problem you handled.
8. If the company implements a specific platform, take its free admin or implementation learning path (for example Salesforce Trailhead) and learn its standard configuration options.
9. Review [07-data-and-sql](../../topics/07-data-and-sql/README.md) sections A, B and D and [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) sections A and B.
10. Prepare questions about project length, concurrent projects, utilisation targets, use of SIs, and how handoff to support works.
