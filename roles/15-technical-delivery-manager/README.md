# Technical Delivery Manager (Engagement Manager, Technical Program Manager)

A customer-facing delivery role that gets complex technical deployments live on time, on budget and with the agreed outcome, by planning across teams, managing risks and dependencies, controlling scope, and running governance with customer executives.

## The role

**Day to day.** A technical delivery manager (TDM) owns the plan and the health of one or more customer deployments:

- **Planning:** turning a statement of work (SOW) or deal into a delivery plan with milestones, a critical path, resourcing and a budget, across internal engineering, forward deployed engineers, the customer's IT teams, and often a system integrator (SI) or other vendors.
- **Risk, issue and dependency management:** keeping a RAID log (risks, assumptions, issues, dependencies) and acting on it before problems become delays.
- **Status and governance:** weekly status reports, RAG (red / amber / green) ratings, working group meetings, and steering committees with customer and internal executives.
- **Scope, change and commercials:** change control against the SOW, budget tracking and forecasting, and working with account teams on change orders.
- **Escalations:** raising problems early to the right level, with options and a recommendation.
- **Go-live and handover:** cutover planning, hypercare, handover to support and the customer's operations team, and lessons learned.

**Common titles / aliases:** Technical Delivery Manager, Engagement Manager, Delivery Lead, Technical Program Manager (TPM; note that "TPM" also means technical product manager elsewhere), Technical Project Manager, Program Manager – Customer Deployments, Professional Services Project Manager, Customer Delivery Manager. In some AI companies, Engagement Manager is the business lead on forward deployed teams.

**Companies that hire for it (examples):** cloud providers (AWS Professional Services, Microsoft Industry Solutions, Google Cloud Professional Services), enterprise software vendors with services arms (Salesforce, ServiceNow, SAP, Workday, Snowflake, Databricks), Palantir and AI companies running large deployments, and consultancies and SIs (Accenture, Deloitte, Thoughtworks). Big tech TPM roles (Amazon, Google, Meta) are similar but usually internal-facing. Loop details below are typical / candidate-reported.

**How it differs from FDE:**

| | Technical Delivery Manager | Forward Deployed Engineer |
|---|---|---|
| Owns | Plan, risks, dependencies, budget, governance, delivery outcome | Technical design and build |
| Writes code | Rarely (reads designs, may script or query) | Yes |
| Success measure | On time, on budget, agreed scope, customer satisfied, value delivered | Working solution in production, customer uses it |
| Main tools | Plans, RAID logs, status reports, steering committees | Code, integrations, data pipelines |
| Interview core | Scenario and behavioural questions, planning exercises, stakeholder role-play | Coding, system design, decomposition |

It also differs from a product manager, who decides what the product should be across all customers; a TDM delivers an agreed outcome for a specific customer or programme.

**Who it suits:** organised people who like making many moving parts converge; who are calm in escalations; who write crisp updates; and who are technical enough to challenge estimates and spot integration risk. Former engineers and tech leads often move into this role when they enjoy coordination more than writing code.

**Skills mix:**

| Skill | Weight |
|---|---|
| Planning, scheduling and dependency management | High |
| Risk and issue management | High |
| Stakeholder management and executive communication | High |
| Written status reporting | High |
| Commercial awareness (SOW, budget, change control) | Med–High |
| Technical understanding (architecture, integration, cloud, data) | Med |
| Agile and scaled agile practice | Med |
| Coding | Low |

**Career paths:** senior delivery manager → programme director or head of delivery / professional services → VP services or customer operations; engagement or account leadership; chief of staff; operations leadership; product management for those who enjoy shaping the product; moving into big tech technical program management.

## Typical interview loop

Loops vary by company; the rounds below are **typical / candidate-reported**. Ask the recruiter which rounds you will have.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Motivation, experience with programme size, customer-facing delivery, travel | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md), this file (role and motivation) |
| Hiring manager | Delivery approach, biggest programmes you ran, how you handle failure | This file, [10-behavioral](../../topics/10-behavioral/README.md) |
| Delivery scenario / case | Planning a multi-team deployment, risks, dependencies, critical path | This file (planning, RAID), [04-decomposition](../../topics/04-decomposition/README.md) |
| Stakeholder or steering committee role-play (common) | Presenting a red project, handling an angry sponsor, scope disputes | This file (governance, escalations), [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) |
| Technical round (lighter than for engineers) | Understanding architecture, integration, cloud, cutover risk | [03-system-design](../../topics/03-system-design/README.md), [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) |
| Written exercise (sometimes) | Status report, RAID log, escalation email, project plan from a brief | This file (status reporting exercises) |
| Behavioural / values | Ownership, conflict, delivering bad news, rescuing projects | [10-behavioral](../../topics/10-behavioral/README.md) |
| Final / bar raiser | Judgment, leadership, culture fit | [11-company-specific](../../topics/11-company-specific/README.md), [14-questions-to-ask](../../topics/14-questions-to-ask/README.md) |

Commonly reported patterns: Amazon TPM and AWS Professional Services loops use Leadership Principle behavioural questions heavily, with a technical or system design round for TPMs; consultancies and software services teams often add a case or a presentation of a past programme. Certifications such as PMP or PRINCE2 are often listed as "preferred" rather than required.

## Common rounds — use the shared sections

- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): sections A (scoping), B (difficult situations) and C (communication) map closely to delivery role-plays.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md) and [00 story bank](../../topics/00-overview/ANSWERS.md): ownership, conflict, prioritisation under pressure and failure stories are the core of this loop.
- [04 Decomposition](../../topics/04-decomposition/ANSWERS.md): use the structure to break a large deployment into workstreams and phases.
- [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md): sections A, C and E so you can discuss integrations, environments and release processes credibly.
- [03 System design](../../topics/03-system-design/ANSWERS.md): for TPM loops with a technical round, practise high-level designs and their delivery risks.
- [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md): section D (pipelines and data quality) for data migration programmes.
- [12 Debugging](../../topics/12-debugging-troubleshooting/ANSWERS.md): section B, to understand how incidents are triaged during hypercare.
- [01 Recruiter & motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) and [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md).
- Skip or skim: [02 Coding](../../topics/02-coding/README.md), [13 CS fundamentals](../../topics/13-cs-fundamentals/README.md), [05 Learning round](../../topics/05-learning-round/README.md).
- Shared questions for every role: [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).

## Role-specific questions

### Role and motivation
- [ ] What does a technical delivery manager do, and how is it different from a project manager, a product manager and a forward deployed engineer?
- [ ] Why do you want a delivery management role rather than staying an engineer?
- [ ] How do you stay technical enough to be credible with engineers and customer architects?

### Planning multi-team deployments
- [ ] Plan the deployment of a platform for a customer with five business units, three internal teams and a system integrator. Where do you start?
- [ ] How do you build a delivery plan and critical path when the team cannot estimate the work well?
- [ ] How do you run a kickoff with a new enterprise customer?
- [ ] How do you plan a production cutover for a go-live weekend?

### RAID, risk and dependency management
- [ ] What is a RAID log, and how do you keep it useful rather than a document nobody reads?
- [ ] How do you identify, score and manage risks on a customer deployment?
- [ ] How do you manage dependencies on teams you do not control?
- [ ] A critical dependency on the customer's IT team slips by three weeks. What do you do?

### Status reporting and governance
- [ ] How do you write a weekly status report, and how do you decide the RAG status?
- [ ] How do you set up governance for a large engagement: steering committee, working groups and meeting cadence?
- [ ] Exercise: you are presenting to the steering committee and the project is red. Walk me through it.
- [ ] How do you spot and fix a "watermelon" project that reports green but is really red?

### Scope and change control
- [ ] How do you manage scope creep on a fixed-price engagement?
- [ ] Walk me through a change control process from request to signed change order.
- [ ] The customer insists a requirement was always in scope, but it is not in the SOW. How do you handle it?

### Budgets and commercials
- [ ] How do you track a project's budget and forecast the cost at completion? Explain the basics of earned value.
- [ ] Your forecast shows the project will overrun its budget by 20%. What do you do?
- [ ] How do fixed-price and time-and-materials contracts change how you run delivery?

### Agile and scaled delivery
- [ ] How do you run agile delivery when the customer expects a fixed plan and date?
- [ ] Explain SAFe at a high level, including PI planning and agile release trains, and when it helps or hurts.
- [ ] How do you coordinate several scrum teams working towards one release?
- [ ] Which delivery metrics do you track, and how do you stop them being gamed?

### Escalations and difficult situations
- [ ] When and how do you escalate, and what does a good escalation look like?
- [ ] The customer's executive sponsor leaves halfway through the project. What do you do?
- [ ] A key engineer resigns two months before go-live. How do you respond?
- [ ] Two senior customer stakeholders give you conflicting direction. How do you resolve it?
- [ ] A severity-1 production incident happens during hypercare. What is your role?

### Vendors, system integrators and partners
- [ ] How do you coordinate a system integrator and third-party vendors on one deployment?
- [ ] A partner SI is consistently late and its quality is poor. How do you handle it?

### Go-live, handover and closure
- [ ] How do you run hypercare and hand over to support and the customer's operations team?
- [ ] How do you run a lessons-learned review that actually changes behaviour?

### Behavioral
- [ ] Tell me about a time you rescued a failing project.
- [ ] Tell me about a time you delivered bad news to a senior stakeholder.
- [ ] Tell me about a time you managed competing priorities across several teams.

## Model answers

### Role and motivation

#### What does a technical delivery manager do, and how is it different from a project manager, a product manager and a forward deployed engineer?
A technical delivery manager is accountable for getting a technical outcome live for a customer: the agreed scope, on time, within budget, and actually working for the business. The job is planning, coordinating teams, managing risks and dependencies, controlling scope, and keeping executives informed and aligned.

- **vs. a general project manager:** the "technical" part means understanding architecture, integrations, environments, data migration and release processes well enough to challenge estimates, spot technical risk early and talk to engineers and customer architects as a peer.
- **vs. product manager:** a PM decides what the product should be across many customers. A TDM delivers an agreed solution for a specific customer or programme, and feeds product gaps back to the PM.
- **vs. FDE:** the FDE designs and builds the solution. The TDM makes sure the FDEs, internal teams, customer IT and partners are working on the right things in the right order, removes blockers, and manages the commercial and governance side.

One line for interviews: "Engineers build it; I make sure it lands: on time, on budget, with no surprises for the customer."

#### Why do you want a delivery management role rather than staying an engineer?
**What they look for:** a positive pull towards coordinating and owning outcomes, evidence you already do it, and realism about what changes.

**Structure:**
- **What you noticed:** "On the projects I worked on, the biggest delays were rarely about code. They came from unclear scope, dependencies on other teams, and late decisions."
- **Evidence:** times you ran sprint planning, built a release plan, managed a cross-team dependency, wrote programme reports for leadership, handled an escalation, or coordinated a go-live.
- **Why delivery, honestly:** "I get most satisfaction from making a complex effort come together and seeing the customer use it. I accept I will write less code; I will use my technical background to make better plans and catch risks early."
- **What you bring:** credibility with engineers, realistic estimates, and an understanding of integration and cutover risk.
- **Why this company:** its customers, the scale and type of deployments, and how the delivery team works with FDEs and product.

Avoid framing it as leaving engineering because you dislike coding.

#### How do you stay technical enough to be credible with engineers and customer architects?
- **Understand the architecture of what you deliver:** be able to draw the solution, its integrations and data flows, environments and release path from memory.
- **Join technical sessions:** attend design reviews and stand-ups; ask "what could go wrong at cutover?" and "what are we assuming about the customer's systems?"
- **Read, do not write:** read design documents, pull requests at a high level, logs during incidents, and test results. Run simple queries to check data yourself.
- **Keep learning the stack:** cloud fundamentals, integration patterns (APIs, events, file transfers), identity and single sign-on, data migration and CI/CD. A cloud practitioner-level certification can help structure this.
- **Respect engineering judgment:** ask for estimates as ranges with assumptions, and ask what would reduce them rather than pushing dates.
- **Know when to bring an expert:** never bluff to a customer architect; say "let me bring our lead engineer" and follow up quickly.

Credibility comes from asking the questions that expose real risk, not from knowing every detail.

### Planning multi-team deployments

#### Plan the deployment of a platform for a customer with five business units, three internal teams and a system integrator. Where do you start?
1. **Understand the outcome and constraints:** read the SOW and deal notes; meet the sponsor to confirm business goals, success criteria, fixed dates (contract, regulatory, fiscal year), budget and what "done" means.
2. **Map stakeholders and teams:** sponsor, business unit leads, customer IT and security, our three teams, the SI. Agree roles with a RACI (responsible, accountable, consulted, informed).
3. **Break the work into workstreams:** for example platform setup and security, integrations, data migration, configuration per business unit, testing, training and change management, cutover and support.
4. **Choose a rollout strategy:** usually a pilot business unit first (simplest or most willing), then waves. This reduces risk and creates a reusable playbook.
5. **Build the integrated plan:** milestones per workstream, dependencies between teams, critical path, resourcing, and environments.
6. **Set up governance and RAID:** steering committee, weekly working group, RAID log, status reporting.
7. **Confirm and baseline:** review the plan with all parties, get sign-off, and baseline it for change control.

Mention early access requests, security reviews and data access as typical long-lead items to start in week one.

#### How do you build a delivery plan and critical path when the team cannot estimate the work well?
- **Break work down** until items are small enough to reason about (a few days to two weeks). Uncertainty usually hides in large items.
- **Estimate in ranges:** best, likely and worst case (three-point estimates). The spread shows where uncertainty is. A PERT-style weighted average, (best + 4 × likely + worst) ÷ 6, gives a planning figure.
- **Run spikes for the unknowns:** time-boxed investigations into the riskiest items (an unfamiliar customer system, data quality) before committing to dates.
- **Map dependencies** and find the critical path: the longest chain of dependent tasks, which sets the end date. Focus management attention there.
- **Add explicit contingency** sized to the risk, held at project level rather than hidden in every task.
- **Use reference data:** how long similar deployments took; past velocity.
- **Plan in rolling waves:** detailed for the next four to six weeks, milestone-level beyond, refined as you learn.
- **Communicate confidence:** "We are 80% confident of the end of March; the main risk is the identity integration."

Re-plan on evidence, not optimism.

#### How do you run a kickoff with a new enterprise customer?
**Before:** align internally on scope, team, risks and commercial position; agree the agenda with the customer sponsor; make sure the right people attend (sponsor, business leads, IT, security, the SI).

**Agenda (typically half a day to a day):**
1. **Introductions and roles,** including who makes which decisions.
2. **Objectives and success criteria:** business goals and how we will measure them; confirm with the sponsor in the room.
3. **Scope:** what is in and, explicitly, what is out, as per the SOW.
4. **Approach and plan:** phases, milestones, rollout waves, high-level timeline.
5. **Governance:** steering committee and working group cadence, status reporting, escalation path, change control.
6. **Dependencies and customer responsibilities:** access, environments, data, subject matter experts' time, decisions needed and when.
7. **Initial risks and assumptions:** start the RAID log together.
8. **Ways of working:** communication channels, tools, meeting rhythm.
9. **Next steps** with owners and dates.

**After:** send a written summary within 24 hours with decisions, actions and the RAID log. A kickoff that makes customer responsibilities explicit prevents most later disputes.

#### How do you plan a production cutover for a go-live weekend?
- **Write a cutover runbook:** every step in order with owner, start time, duration, dependencies, verification check, and contact details. Include pre-cutover tasks (freeze, backups, communications) and post-cutover checks.
- **Define go / no-go criteria and checkpoints:** for example, testing signed off, no open severity-1 or severity-2 defects, data reconciliation passed, support ready. Hold a go / no-go meeting with named decision makers.
- **Plan the rollback:** exact steps, the point of no return, and how long rollback takes. Decide in advance which failures trigger it.
- **Rehearse:** at least one full dress rehearsal in a production-like environment, timed, including data migration and rollback.
- **Data migration and reconciliation:** record counts, totals and sample checks agreed with the business.
- **Staffing and communication:** command channel or bridge call, shift rota, escalation contacts at the customer, SI and vendors; scheduled status updates to stakeholders.
- **Business verification:** key users run test transactions before the system is declared live.
- **Hypercare** plan ready to start immediately.

Keep a timestamped log during the cutover for the post-go-live review.

### RAID, risk and dependency management

#### What is a RAID log, and how do you keep it useful rather than a document nobody reads?
A RAID log records **Risks** (things that might happen and would affect the project), **Assumptions** (things we treat as true but have not confirmed), **Issues** (problems happening now) and **Dependencies** (things we need from others, or others need from us). Some teams use "D" for decisions as well; keeping a separate decision log is good practice.

Each entry has: description, owner, date raised, impact, likelihood (for risks), mitigation or action, due date, status.

**Keeping it useful:**
- **Review it weekly** in the working group, focusing on new, high and overdue items, not reading every line.
- **One named owner per item,** with a date, never "the team".
- **Write specific entries:** "Customer SSO configuration not started; if not done by 15 March, user acceptance testing slips two weeks" rather than "SSO risk".
- **Validate assumptions:** each has a date by which it must be confirmed; unconfirmed assumptions become risks.
- **Link it to reporting:** the top risks and issues appear in the status report and steering pack.
- **Close items** when resolved, and turn materialised risks into issues.
- **Keep it in a shared tool** the customer can see where appropriate.

#### How do you identify, score and manage risks on a customer deployment?
**Identify:** risk workshop at kickoff with engineering, the customer and the SI; review the SOW assumptions; use a checklist from past deployments (access and security approvals, data quality, integration with legacy systems, key person dependency, customer resource availability, change in sponsor, scope ambiguity, performance at scale); ask each workstream lead "what keeps you up at night?"

**Score:** likelihood × impact, each 1–5, giving a priority. Describe impact in concrete terms: days of delay, cost, or effect on a business outcome.

**Respond (four standard options):**
- **Avoid:** change the plan to remove the risk (for example, drop a risky integration from phase one).
- **Reduce (mitigate):** lower likelihood or impact (early spike, extra testing, backup person).
- **Transfer:** move it to another party (contract terms, vendor responsibility, insurance).
- **Accept:** consciously accept, with contingency, when mitigation costs more than the impact.

**Manage:** owner and trigger for each risk; review weekly; escalate risks above an agreed threshold to the steering committee; track contingency usage.

The aim is fewer surprises: most risks are visible early if you look.

#### How do you manage dependencies on teams you do not control?
- **Make dependencies explicit early:** list them in the plan and RAID log with what is needed, by when, from whom, and what happens if it is late.
- **Agree them, do not assume them:** meet the other team's lead, explain why it matters, and get a committed date. For customer dependencies, write them into the plan and kickoff summary; for contractual ones, the SOW should list customer responsibilities.
- **Understand their priorities:** what are they measured on, what else competes for their time? Make it easy for them: clear specifications, ready-to-use templates, a named contact.
- **Track with leading indicators:** check progress well before the due date, not on the day.
- **Build buffers and alternatives:** plan non-dependent work in parallel; consider mocks, stubs or temporary workarounds so testing can continue.
- **Escalate early and constructively** when a dependency is at risk: to their manager or the steering committee, with impact and options, not blame.

Relationships matter: help the other teams when you can, and they will prioritise you when it counts.

#### A critical dependency on the customer's IT team slips by three weeks. What do you do?
1. **Confirm the facts:** talk to the IT lead; find out why (capacity, competing priority, approvals, technical blocker) and whether three weeks is firm or could grow.
2. **Assess impact:** is the item on the critical path? What does three weeks mean for milestones, go-live date, cost (our team waiting) and the business outcome?
3. **Look for options:**
   - Help unblock: offer our engineers to do part of the work, or provide scripts and documentation.
   - Re-sequence: bring forward work that does not depend on it; use mock interfaces to continue testing.
   - Reduce scope for the first release, so go-live does not wait for this item.
   - Get the customer to re-prioritise: their sponsor can move IT capacity.
   - Accept the delay with a re-baselined plan, and a change request if our costs rise because of it.
4. **Escalate if needed** through the customer sponsor, with impact and options.
5. **Update the plan, RAID log and status report,** and document that the delay is a customer dependency, which matters for commercial discussions.

Stay factual and collaborative; the goal is the joint outcome, not winning an argument.

### Status reporting and governance

#### How do you write a weekly status report, and how do you decide the RAG status?
**Structure (one page):**
1. **Overall status (RAG) and a two-line summary:** what changed and what matters.
2. **Status by workstream** or milestone with RAG and trend (improving, stable, worsening).
3. **Progress this week** against plan, and plan for next week.
4. **Key milestones:** baseline date, forecast date, status.
5. **Top risks and issues** with owner, action and date.
6. **Decisions and help needed,** with the person and the deadline.
7. **Budget** (if relevant): spent, forecast at completion versus budget.

**RAG definitions (agree them at kickoff):**
- **Green:** on track for scope, schedule and budget; risks under control.
- **Amber:** at risk; there is a recovery plan within the team's control, or a decision is needed soon.
- **Red:** will miss a committed milestone, budget or scope without intervention beyond the team.

**Principles:** no surprises (a project should rarely go from green to red in one week); be honest; lead with what the reader needs to do; keep the format consistent so trends are visible.

#### How do you set up governance for a large engagement: steering committee, working groups and meeting cadence?
**Steering committee** (monthly, or every two to four weeks in critical phases):
- Members: customer executive sponsor (chair), business unit leads, customer IT leader, our delivery and account executives, SI lead.
- Purpose: confirm direction and priorities, decide on escalations, approve major changes to scope, budget and timeline, remove organisational blockers.
- Inputs: a short pack with overall status, milestones, top risks, decisions needed, and benefits tracking.

**Programme working group** (weekly): workstream leads from all parties; review plan, RAID, dependencies and actions; resolve what can be resolved below steering level.

**Workstream meetings and stand-ups** (daily or several times a week): delivery teams.

**Design authority** (as needed): technical decisions and standards across teams.

**Also define:** a RACI, a decision log, the escalation path (team → working group → steering committee) with expected response times, change control, and reporting formats.

Keep governance proportionate: a small engagement may only need a weekly check-in and a monthly sponsor review.

#### Exercise: you are presenting to the steering committee and the project is red. Walk me through it.
**Before the meeting:** brief the sponsor privately so they are not surprised; align with our account executive; prepare options with costs and a recommendation.

**In the meeting (about 15 minutes):**
1. **Headline first:** "The project is red. On the current plan, go-live moves from 1 June to 29 June. I need a decision today on one of three options."
2. **Cause, briefly and factually:** for example, data quality in the source system is worse than assumed; cleansing needs three more weeks. No blame.
3. **Impact:** on dates, cost, business outcomes and dependent plans (training, a marketing launch).
4. **Options with trade-offs:**
   - A: move go-live four weeks; extra cost X.
   - B: go live on 1 June with three business units, add the other two in July.
   - C: go live on time with a reduced data scope and manual workarounds for one process.
5. **Recommendation and why:** for example B, because it protects the most valuable outcome with lower risk.
6. **Decision and actions:** confirm the decision, owners, and when the committee will see the updated plan.

**After:** send minutes with the decision within a day, update the plan and RAID, and raise a change request if needed.

#### How do you spot and fix a "watermelon" project that reports green but is really red?
A watermelon project is green on the outside and red inside: status reports look fine while the real situation is deteriorating.

**Warning signs:**
- Status stays green for weeks while milestones are quietly re-dated or "percent complete" stalls at 80–90%.
- Testing keeps being compressed; defect counts rise.
- Vague updates ("progressing well") without evidence.
- Team members or customer users are uneasy in informal conversations.
- Reporting tracks activity (tasks done) rather than outcomes (working features accepted).

**Fixing it:**
- **Measure with evidence:** demo of working software, accepted user stories, test pass rates, burn-up charts and forecast dates, not self-reported percentages.
- **Agree objective RAG criteria** tied to milestones and budget.
- **Make it safe to report bad news:** thank people for raising problems early; never punish honesty.
- **Do independent checks:** health checks or a quality assurance review of the plan.
- **Reset openly:** if you inherit one, re-baseline with the facts, tell the sponsor clearly, and show a credible recovery plan.

Honest amber early is far better than red late.

### Scope and change control

#### How do you manage scope creep on a fixed-price engagement?
On fixed price, unmanaged scope directly erodes the margin and then threatens delivery.

- **Start with a clear baseline:** SOW with deliverables, acceptance criteria, assumptions, exclusions and customer responsibilities. Walk through it at kickoff so everyone has the same understanding.
- **Trace requirements to the SOW:** every requirement maps to a scoped item. Anything new is identified as a change, even if small.
- **Have a visible change process** agreed at kickoff, so raising a change is normal and not a confrontation.
- **Train the team:** engineers and FDEs should not agree to extras informally; they log requests and refer them to the delivery manager.
- **Offer trade-offs:** swap a new item for something of similar effort that matters less, defer it to a later phase, or price it as a change order.
- **Track small items:** many "small favours" add up; log them, and use them in negotiation.
- **Be reasonable:** a little flexibility on genuinely small items builds goodwill; agree an explicit budget for minor changes if helpful.

Communicate in terms of the customer's goals: "We can add this; here is the effect on date and cost, or here is what we would swap."

#### Walk me through a change control process from request to signed change order.
1. **Request:** anyone raises a change request (CR) in a standard form: description, reason, requester, urgency.
2. **Log:** recorded in the change log with an ID and status.
3. **Impact assessment:** delivery and engineering estimate effort, cost, schedule impact, risks, and effect on other work and on contract terms. Usually within a few days for small changes.
4. **Options:** do it now with extra cost and time; do it in exchange for removing something; defer to a later phase; reject.
5. **Decision:** the agreed authority approves. Small changes within a threshold may be approved by the working group; larger ones by the steering committee or the contract owners.
6. **Contract update:** for changes affecting cost or scope, a written change order or SOW amendment, signed by both parties before work starts (involve the account team and, where needed, legal and finance).
7. **Implement:** update the plan, budget, RAID log and requirements baseline; inform the team.
8. **Track:** report changes in status reports and steering packs, including cumulative impact.

The process should be light for small items and rigorous for large ones, but always written down.

#### The customer insists a requirement was always in scope, but it is not in the SOW. How do you handle it?
1. **Listen first:** understand what they believe was agreed and why. Sometimes it was promised in the sales process, or it is implied by a business goal.
2. **Check the record:** SOW, requirements documents, kickoff summary, emails and sales notes. Talk to our account team about what was said.
3. **Discuss facts calmly:** walk through the SOW with the customer lead: "Here is what is listed; this item is not mentioned, and the assumptions say X."
4. **Separate the relationship from the commercial question:** acknowledge their need and that the misunderstanding is frustrating.
5. **Offer options:** a change order at fair cost; a trade with lower-value scoped items; phase it later; a workaround; or, if our side clearly implied it, absorb part of the cost and record it as a goodwill decision.
6. **Escalate if needed** to the sponsor and our account executive, so it does not poison the working relationship.
7. **Prevent repeats:** clarify ambiguous items in the remaining scope now, and feed back to sales about how scope is described.

The aim is a fair, documented resolution without damaging trust.

### Budgets and commercials

#### How do you track a project's budget and forecast the cost at completion? Explain the basics of earned value.
**Tracking:** record actual hours and costs weekly by workstream (timesheets, vendor invoices, cloud and licence costs). Compare with the planned spend for that point in time and with work actually completed. Spend alone says nothing about progress.

**Earned value basics:**
- **Planned value (PV):** budgeted cost of the work scheduled to be done by now.
- **Earned value (EV):** budgeted cost of the work actually completed.
- **Actual cost (AC):** what the completed work actually cost.
- **Cost performance index (CPI) = EV ÷ AC.** Below 1 means over budget for the work done.
- **Schedule performance index (SPI) = EV ÷ PV.** Below 1 means behind schedule.
- **Estimate at completion (EAC):** a common formula is budget at completion ÷ CPI, assuming current efficiency continues.

**Example:** budget $1,000,000; by now we planned $400,000 of work (PV), completed $300,000 worth (EV), and spent $375,000 (AC). CPI = 0.8, SPI = 0.75, EAC ≈ $1,250,000. We are over budget and behind.

In practice, also do a bottom-up forecast of remaining work with the team and compare it with the formula; differences start a useful conversation.

#### Your forecast shows the project will overrun its budget by 20%. What do you do?
1. **Validate the forecast:** check the data and assumptions with workstream leads; find the causes (underestimated work, scope changes not charged, rework from quality issues, customer delays, team inefficiency).
2. **Separate causes by who owns them:** customer-caused delays and unapproved changes may be recoverable through change orders; our estimation errors usually are not.
3. **Find recovery options:**
   - Raise change requests for valid scope changes and customer-caused delays.
   - Reduce cost: adjust team mix, remove waste (waiting, rework), use reusable assets.
   - Reduce or defer scope with customer agreement.
   - Accept the overrun where it protects the outcome and the relationship.
4. **Escalate early internally:** delivery leadership and finance need to know about margin impact now, not at the end. Present the cause, options and recommendation.
5. **Agree the customer conversation:** with the account team, decide what to raise and how; be transparent if the customer is affected.
6. **Update the forecast and track weekly** against the recovery plan.

Never hide an overrun hoping to recover it later; it usually grows.

#### How do fixed-price and time-and-materials contracts change how you run delivery?
**Fixed price:** the customer pays an agreed amount for defined deliverables; the supplier carries the cost risk.
- Scope definition and change control are critical; every ambiguity is a financial risk.
- Detailed upfront planning and contingency.
- Acceptance criteria and sign-off steps must be explicit, because payment often depends on milestones.
- Tension with agile: you fix scope and price, so flexibility must come through trade-offs and change orders.

**Time and materials (T&M):** the customer pays for time spent at agreed rates; the customer carries the cost risk.
- Focus shifts to transparency: regular reporting of hours, burn rate and value delivered.
- Scope can evolve more easily, which suits agile and discovery-heavy work.
- Often has a cap ("not to exceed") that must be tracked closely.
- Risk of the customer feeling they pay for slow progress; show progress and outcomes clearly.

**Hybrids:** fixed-price phases after a T&M discovery phase; capped T&M; outcome-based fees. Many AI and forward deployed engagements start with a paid pilot or discovery, then a fixed-scope or subscription-based deployment.

### Agile and scaled delivery

#### How do you run agile delivery when the customer expects a fixed plan and date?
Combine a fixed outer frame with agile execution inside it.

- **Fix the date and budget, flex the scope:** agree must-have outcomes up front (MoSCoW), with "should" and "could" items that fill remaining capacity.
- **Build a release plan:** a backlog estimated at a high level, mapped to sprints and milestones, showing the date and what is likely by then. Update it each sprint from actual velocity.
- **Show progress with working software:** sprint reviews with customer users every two weeks build more confidence than Gantt charts.
- **Report in the customer's language:** milestones, burn-up charts with forecast ranges, risks and decisions needed, mapped to their plan format if required.
- **Agree how change works:** new items go into the backlog and displace lower-priority ones, or become a change order if they exceed the frame.
- **Keep a few fixed-plan elements:** dependencies with other teams, cutover, training and regulatory dates must be planned precisely.

Explain it to the customer as reducing risk: they see real progress every two weeks and can steer, instead of finding out at the end.

#### Explain SAFe at a high level, including PI planning and agile release trains, and when it helps or hurts.
SAFe (Scaled Agile Framework) is a framework for coordinating many agile teams.

- **Agile release train (ART):** a long-lived team of agile teams (often 50–125 people) that plan and deliver together towards a shared mission, on a common cadence.
- **Program increment (PI):** a fixed timebox, usually 8–12 weeks (often four development iterations plus an innovation and planning iteration).
- **PI planning:** a one- or two-day event where all teams of the ART plan the next increment together: business context and vision, team breakouts, a dependency board showing cross-team dependencies, risks reviewed and handled (often with the ROAM categories: resolved, owned, accepted, mitigated), and a confidence vote on the PI objectives.
- **Roles:** release train engineer (coordinator, similar to a chief scrum master), product management, system architect, business owners.

**Helps when:** many teams build one product or solution with heavy dependencies; the organisation needs predictable, aligned planning; large enterprises and government programmes.

**Hurts when:** applied to small or independent teams; treated as process compliance rather than delivering value; it can add overhead, reduce team autonomy and slow change.

As a delivery manager, use PI planning ideas (joint planning, visible dependencies) even without full SAFe.

#### How do you coordinate several scrum teams working towards one release?
- **Shared release goal and plan:** a release roadmap with milestones, owned jointly by the product owners and delivery manager.
- **Joint planning:** a quarterly or release-level planning session with all teams to identify dependencies, then align sprint cadences so teams plan and review at the same time.
- **Dependency management:** a visible dependency board; explicit agreements on interfaces (API contracts agreed early, mocks for testing).
- **Scrum of scrums:** a short meeting (two or three times a week) of team representatives covering progress, blockers and dependencies.
- **Integration early and often:** continuous integration, shared test environments, regular end-to-end tests, a single definition of done that includes integration.
- **Single backlog view** for the release, even if teams have their own backlogs.
- **Release readiness:** a checklist and go / no-go review covering testing, performance, security, documentation and support.
- **Shared metrics:** a release burn-up showing combined progress and forecast.

My focus is the seams between teams, where most problems appear.

#### Which delivery metrics do you track, and how do you stop them being gamed?
**Useful metrics:**
- **Predictability:** milestone dates hit versus baseline; sprint commitment reliability (planned versus delivered).
- **Flow:** cycle time, throughput, work in progress, blocked time.
- **Scope:** release burn-up showing completed versus total scope, with scope changes visible.
- **Quality:** escaped defects, defects found in user acceptance testing, test pass rate, production incidents after release.
- **Engineering health:** deployment frequency, lead time for changes, change failure rate and time to restore (the DORA metrics).
- **Cost:** budget burn, CPI, forecast at completion.
- **Outcome:** user adoption, business metrics, customer satisfaction.

**Avoiding gaming:**
- Use metrics for learning and conversation, not individual performance ratings.
- Never compare teams on velocity; story points are team-specific.
- Track balanced sets (speed with quality) so improving one at the cost of another is visible.
- Prefer outcome metrics and evidence (working software, accepted stories) over self-reported progress.
- Review metric trends with the team and ask what is behind them.

If a metric becomes a target, people optimise the number rather than the result (Goodhart's law).

### Escalations and difficult situations

#### When and how do you escalate, and what does a good escalation look like?
**When:** escalate when a problem threatens a committed milestone, budget, scope or quality and cannot be solved at your level within the time available, or needs a decision or resources you do not control. Escalate early: as soon as you know, not when it is too late to act. Agree thresholds and the escalation path at kickoff so escalation is expected rather than personal.

**How:**
- Try to resolve it at the working level first, and tell the people involved that you will escalate if needed, so nobody is surprised.
- Escalate to the right level: the person who can actually decide or unblock.
- Escalate in writing and in conversation.

**A good escalation message contains:**
1. **The issue in one sentence.**
2. **Impact:** on dates, cost, customer, outcome, with numbers.
3. **What has been tried.**
4. **Options** with trade-offs.
5. **Recommendation.**
6. **The specific decision or help needed, and by when.**

Keep it factual and blame-free. Afterwards, close the loop: confirm the decision, act on it, and report the result. Escalation is a normal management tool, not a failure.

#### The customer's executive sponsor leaves halfway through the project. What do you do?
A lost sponsor is one of the biggest risks to an engagement: budget, priority and decision-making can all disappear.

1. **Act quickly:** log it as a high risk; inform our account executive and delivery leadership.
2. **Find out who is taking over:** ask the departing sponsor (if possible) and the customer's leadership who will own the programme. Ask the departing sponsor to introduce us and endorse the work.
3. **Onboard the new sponsor:** meet them early; understand their priorities and how they are measured; present the business case, progress, value delivered so far, and upcoming decisions. Be ready for them to question the project.
4. **Reconfirm objectives and scope:** the new sponsor may want changes. Use change control rather than resisting.
5. **Broaden support:** strengthen relationships with other senior stakeholders and business unit leads so the project does not depend on one person.
6. **Show value quickly:** highlight a near-term win that matters to the new sponsor.

If no sponsor is appointed, escalate through our account leadership: a project without a sponsor should be paused or re-planned rather than drift.

#### A key engineer resigns two months before go-live. How do you respond?
1. **Assess the impact:** what does this person own, what knowledge exists only in their head, and what is on the critical path?
2. **Use the notice period well:** agree a knowledge transfer plan immediately: documentation of designs and decisions, recorded walk-throughs, pairing with named successors on live work, handover of access and credentials through proper channels.
3. **Find cover:** move someone from inside the team, borrow from another team, or bring in a contractor or partner; prioritise people already familiar with the codebase or customer.
4. **Re-plan:** adjust the plan for ramp-up time; consider reducing scope for go-live or moving non-critical work to after launch.
5. **Communicate:** inform delivery leadership and, where it affects the customer, tell the customer with the mitigation plan and any impact on dates. Do not hide it.
6. **Look after the team:** check workload and morale of those taking on the work.
7. **Reduce future key-person risk:** pairing, rotation, documentation standards; log key-person dependency as a standard risk.

#### Two senior customer stakeholders give you conflicting direction. How do you resolve it?
1. **Do not pick a side or act on either instruction yet.** Pause work that would be wasted, and tell both that you need alignment.
2. **Understand each position:** meet each separately; find out what they are trying to achieve, what constraints they face and what they would accept. Often the conflict is about priorities or underlying goals, not the specific request.
3. **Clarify decision rights:** check the RACI and governance. Who is accountable for this decision? If unclear, that is the real problem to fix.
4. **Lay out the options neutrally:** a short document with each option, impacts on scope, schedule, cost and outcome, and your recommendation based on the agreed project objectives.
5. **Bring them together:** a joint meeting, facilitated by you, focused on the shared objective.
6. **Escalate to the sponsor or steering committee** if they cannot agree, with the options paper.
7. **Record the decision** in the decision log and communicate it to the team.

This keeps you as a trusted, neutral party and avoids being caught in organisational politics.

#### A severity-1 production incident happens during hypercare. What is your role?
As delivery manager I am usually not the person fixing it; I make sure the response is organised, fast and well communicated.

- **Mobilise:** confirm an incident lead (often the senior engineer or support lead) and get the right engineers, customer IT and vendors on a bridge call quickly. Follow the agreed incident process and severity definitions.
- **Protect the fixers:** shield engineers from repeated status requests; I handle stakeholders.
- **Communicate:** regular updates to customer and internal stakeholders at an agreed interval (for example every 30 minutes for severity 1): what is affected, business impact, current actions, next update time. Keep wording factual.
- **Support decisions:** help decide on rollback, workaround or fix-forward, with the business impact in view; get customer approval where needed.
- **Track:** timeline of events, actions and decisions for the review.
- **After resolution:** confirm service is restored with business users; organise a blameless post-incident review with root cause and actions; add actions to the plan and RAID log; report in the next steering meeting.

Also check whether the incident affects hypercare exit criteria and the handover date.

### Vendors, system integrators and partners

#### How do you coordinate a system integrator and third-party vendors on one deployment?
- **Clear contracts and responsibilities:** understand who is contracted to whom (the customer often contracts the SI directly). Build a responsibility matrix across all parties, especially at the boundaries: integrations, environments, testing and defect ownership.
- **One integrated plan:** each party's plan rolls up into a single programme plan with shared milestones and dependencies, reviewed together weekly.
- **Shared governance:** the SI and vendors attend the working group and relevant steering meetings; one RAID log and one defect tracker across parties.
- **Agreed interfaces:** API contracts, data formats and environment access agreed early and documented; changes go through change control.
- **Common definitions:** what "done", "tested" and each defect severity mean.
- **Joint testing:** end-to-end test plans that cross vendor boundaries, with named owners for each part.
- **Relationship:** treat partners as part of the team, share context and credit, and resolve issues at working level first; escalate through contract owners when needed.

My role is often to be the neutral integrator of plans, even when I do not control the other parties' contracts.

#### A partner SI is consistently late and its quality is poor. How do you handle it?
1. **Gather evidence:** missed milestones against agreed dates, defect rates, rework, examples of quality issues, and their impact on the programme.
2. **Talk to the SI's delivery lead directly:** share the evidence, ask for their view of the causes (staffing, unclear requirements, dependencies on us or the customer). Sometimes part of the cause is on our side.
3. **Agree a recovery plan:** specific actions, owners, dates, and measures (for example, defect rates in the next two drops, named senior resources added). Review weekly.
4. **Adjust the programme:** add quality gates (code reviews, acceptance tests before handover), extra testing of their deliverables, and re-plan the critical path to reduce dependence on their late items.
5. **Escalate if no improvement:** through the contract owner (often the customer) and the steering committee, with the evidence. Possible outcomes include contract remedies, replacing their staff, or moving scope to another party.
6. **Stay professional:** keep the relationship workable and avoid blaming them in front of the customer; the customer wants a joint solution.

Document everything, because commercial consequences may follow.

### Go-live, handover and closure

#### How do you run hypercare and hand over to support and the customer's operations team?
**Hypercare** is a period after go-live (often two to six weeks) of extra support while the system stabilises.

- **Plan it before go-live:** duration, team and rota, support hours, severity definitions, response targets, communication channels, and exit criteria.
- **Run it actively:** daily triage of issues with the customer; fast fixes; tracking of incidents, defects, adoption and performance; daily or twice-weekly reports.
- **Exit criteria** (agreed in advance): for example, no open severity-1 or severity-2 issues, incident volume below an agreed level for two weeks, key business processes completed successfully (month-end, for instance), and support team trained.

**Handover:**
- Documentation: architecture, configuration, runbooks, known issues and workarounds, monitoring and alerting, contacts and escalation paths.
- Knowledge transfer sessions with the support team and the customer's operations team; shadowing, then reverse shadowing.
- Transfer of open defects and backlog to the owning teams.
- Formal sign-off by the customer and the receiving support team.

Then close the project: final report, benefits check, lessons learned and commercial closure.

#### How do you run a lessons-learned review that actually changes behaviour?
- **Run it soon:** at the end of each phase, not only at project end, so people remember and the lessons can still help this project.
- **Make it safe and blameless:** focus on systems and decisions, not individuals; a neutral facilitator helps.
- **Include all parties:** our team, FDEs, the customer and the SI where possible.
- **Use data:** timeline of the project, planned versus actual dates, scope changes, defects, incidents and budget.
- **Ask simple questions:** what went well, what did not, what surprised us, what would we do differently.
- **Turn lessons into specific actions:** for example, "add a security review task in week one of every plan template", with an owner and due date. A lesson without an action is just a note.
- **Change the standard assets:** update templates, checklists, estimation data, SOW language and onboarding materials. This is how lessons reach future projects.
- **Follow up:** check actions in later reviews; share key lessons across the delivery team.

Measure success by whether the next project avoids the same problem.

### Behavioral

#### Tell me about a time you rescued a failing project.

**What they look for:** clear diagnosis, decisive actions, honest communication with stakeholders, and a measurable turnaround.

**STAR outline:**
- **S:** [Project, why it was failing: dates missed, budget, quality, morale, customer unhappy]
- **T:** [Your role; what was at stake]
- **A:** [How you diagnosed it (facts, conversations), what you changed: scope, plan, team, governance, communication; how you reset expectations]
- **R:** [Outcome with numbers: go-live date, budget, quality, customer relationship; what you learned]

**Example (adapt to your own experience):** "I took over a data migration programme that had slipped twice. The team was migrating everything at once, and reconciliation failures were found late. I split the migration into waves by product line, added automated reconciliation checks after each wave, and reset the plan with the sponsor using the real defect data. The first wave went live within six weeks, and the remaining waves followed the new plan."

#### Tell me about a time you delivered bad news to a senior stakeholder.

**What they look for:** you raised it early, were honest and clear, brought options, took ownership, and kept trust.

**STAR outline:**
- **S:** [Stakeholder, the news: delay, overrun, failure]
- **T:** [Why it was your responsibility to tell them]
- **A:** [How you prepared (facts, impact, options), how you delivered it (headline first, no blame), how you handled their reaction]
- **R:** [Decision made, outcome, and effect on the relationship]

**Example (adapt to your own experience):** "Three weeks before a planned release, testing showed an integration with the finance system could double-post invoices in some cases. I told the business head the same day: the release would slip two weeks unless we shipped without that integration. I gave two options with impacts and recommended releasing the rest on time and the integration two weeks later. They agreed, and later said they valued hearing it early rather than at the deadline."

#### Tell me about a time you managed competing priorities across several teams.

**What they look for:** a transparent way of deciding, alignment with business goals, stakeholder buy-in, and follow-through.

**STAR outline:**
- **S:** [The teams, the competing demands, the limited resource (people, environments, release slots)]
- **T:** [Your responsibility]
- **A:** [How you gathered requests, the criteria you used (business impact, deadlines, dependencies), how you involved stakeholders, how you communicated the decision and handled those who lost out]
- **R:** [What was delivered, measurable result, what you would do differently]

**Example (adapt to your own experience):** "Sales, finance and operations each wanted changes in the same quarterly release, and together they needed about twice the team's capacity. I listed every request with its business impact and deadline, met the three leads together, and proposed criteria: regulatory and revenue-affecting items first. We agreed a release plan, deferred the rest with dates, and published it. All committed items shipped, and the deferred ones were delivered the following quarter."

## Prep checklist

1. Prepare a two-minute summary of the largest programme you have run: scope, teams, budget or size, your role, the hardest problem and the result.
2. Build a sample RAID log for a fictional multi-team deployment with ten realistic entries, each with owner, date and action.
3. Write a one-page weekly status report and a five-slide steering committee pack for a project that has turned red, with options and a recommendation.
4. Practise the "plan a deployment" scenario out loud in 20 minutes: outcome, stakeholders, workstreams, rollout waves, critical path, governance and risks.
5. Learn earned value (PV, EV, AC, CPI, SPI, EAC) and work through two numerical examples.
6. Learn the basics of scrum, kanban and SAFe (ART, PI planning, ROAM) well enough to say when each fits.
7. Write a change request and an escalation email for a realistic scenario, each under one page.
8. Prepare a cutover runbook outline with go / no-go criteria and a rollback plan for a system you know.
9. Review enough architecture and integration knowledge to discuss delivery risks: [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md) and [03 System design](../../topics/03-system-design/ANSWERS.md).
10. Prepare six to eight STAR stories covering a failing project, bad news, conflict, competing priorities, an escalation, a vendor problem and a scope dispute; use [10 Behavioral](../../topics/10-behavioral/ANSWERS.md).
11. Decide whether a certification is worth it for your target employers (for example PMP, PRINCE2, Certified ScrumMaster or SAFe); it is often preferred but rarely required.
12. Prepare questions about delivery models, team structure and how delivery works with FDEs and product, using [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md).
