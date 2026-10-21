# Customer Engineer / Technical Account Manager (TAM)

The technical owner of a customer relationship: keeps accounts healthy, drives adoption, manages escalations and protects renewals, using enough technical depth to be trusted by the customer's engineers.

---

## The role

**Day to day.** A TAM or Customer Success Engineer (CSE) owns a "book" of existing customers. A typical week mixes:

- Scheduled cadence calls with each account (weekly or bi-weekly for the largest, monthly or quarterly for the rest).
- Reviewing account health: usage, support tickets, incidents, open feature requests, upcoming renewals.
- Working open escalations: chasing support and engineering, joining incident bridges, writing customer updates.
- Technical advisory: architecture reviews, best-practice reviews, launch or peak-event planning, cost optimisation.
- Preparing and running quarterly business reviews (QBRs) with the customer's leadership.
- Internal advocacy: writing feature requests, briefing account executives (AEs) on risk and expansion, feeding product teams.

**Titles and aliases.** Technical Account Manager, Customer Success Engineer, Customer Success Architect, Customer Engineer, Enterprise Support TAM, Customer Success Account Manager (Microsoft), Success Architect. Titles vary: read the job description.

> **Note on "Customer Engineer" at Google Cloud.** At Google Cloud, "Customer Engineer" (CE) is mainly a **pre-sales** technical role that works alongside sales on new workloads and opportunities, closer to a Solutions Engineer. Google Cloud also has **Technical Account Managers** who work post-sale with Premium Support customers. This file focuses on the post-sale account-ownership work; if you are interviewing for a pre-sales CE role, also prepare [03-solutions-engineer.md](03-solutions-engineer.md) and [04-solutions-architect.md](04-solutions-architect.md).

**Who hires (examples, titles vary).** AWS (TAMs in Enterprise Support), Google Cloud (Customer Engineers, TAMs), Microsoft (Customer Success Account Managers, Cloud Solution Architects), and many SaaS and infrastructure companies with post-sale technical teams, such as Datadog, Okta, Cloudflare, MongoDB and Atlassian.

**How it differs from an FDE.**

| | FDE | TAM / CSE |
|---|---|---|
| Main output | Working software deployed at the customer | Healthy, growing, renewing accounts |
| Coding | Daily, production code | Occasional: scripts, queries, reproductions, small tools |
| Number of customers | 1–3 deep engagements | 5–30+ accounts (depends on segment) |
| Time horizon | Project (weeks to months) | Relationship (years, renewal cycles) |
| Success metric | Use case live, value delivered | Gross/net retention, adoption, CSAT, escalation outcomes |
| Commercial exposure | Low | Medium: renewals and expansion signals |

**Who it suits.** People who like breadth over depth, enjoy relationships and long-term ownership, stay calm in escalations, and are good at organising many threads at once. It suits engineers who prefer advising and troubleshooting to writing large codebases, and support engineers ready to move toward strategy.

**Skills mix.**

| Skill | Weight |
|---|---|
| Coding | Low–Med (scripting, SQL, reading logs and configs) |
| Technical breadth (cloud, networking, the product) | High |
| Customer communication and relationships | High |
| Sales / commercial awareness | Med (renewals, expansion, risk) |
| Product influence | Med (feature requests, voice of customer) |
| Project / programme management | Med–High |

**Career paths.** Senior / Principal TAM; TAM or Customer Success manager; Solutions Architect or Solutions Engineer (move pre-sales); Product Manager (strong voice-of-customer background); Account Executive or Account Manager (move fully commercial); Support or Professional Services leadership; FDE if you build up coding.

---

## Typical interview loop

Loops vary by company and level. The rounds below are typical or reported, not guaranteed.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Motivation for post-sale work, customer experience, travel, compensation | [01-recruiter-and-motivation](../01-recruiter-and-motivation/README.md) |
| Hiring manager | Book-of-business management, escalation stories, prioritisation | [10-behavioral](../10-behavioral/README.md), this file |
| Technical / role-related knowledge | Cloud and networking fundamentals, product area depth, troubleshooting out loud. Google reportedly calls this "Role-Related Knowledge" (RRK) | [08-cloud-devops-integration](../08-cloud-devops-integration/README.md), [12-debugging-troubleshooting](../12-debugging-troubleshooting/README.md), [13-cs-fundamentals](../13-cs-fundamentals/README.md) |
| Problem solving / general cognitive ability | Structuring an ambiguous problem (Google reportedly calls this GCA) | [04-decomposition](../04-decomposition/README.md) |
| Customer role-play | Escalation call, angry executive, renewal-risk conversation | [09-customer-scenarios](../09-customer-scenarios/README.md), this file |
| QBR or presentation exercise | Turning account data into a story for executives | This file (QBR exercise) |
| Written exercise | Incident summary, escalation email, ticket triage | This file, [12-debugging-troubleshooting](../12-debugging-troubleshooting/ANSWERS.md) |
| Values / culture | Googleyness & Leadership (Google), Leadership Principles (Amazon), company values elsewhere | [10-behavioral](../10-behavioral/README.md), this file |

---

## Common rounds — use the shared sections

- [09-customer-scenarios](../09-customer-scenarios/README.md) — the core of this role. Prioritise section B (difficult situations: angry executive, champion leaves, users won't adopt, sales over-promised) and section E (ROI, expansion, churn, prioritising five customers). See [answers](../09-customer-scenarios/ANSWERS.md).
- [12-debugging-troubleshooting](../12-debugging-troubleshooting/README.md) — section B (production scenarios) and section D (limited access, communicating during an outage, post-mortems). TAMs are judged on how they run troubleshooting with the customer on the call.
- [08-cloud-devops-integration](../08-cloud-devops-integration/README.md) — sections A, B, C and E: APIs, auth/SSO, cloud networking, HA/DR, SLIs/SLOs/SLAs. For cloud-provider TAM roles, know one cloud in depth.
- [10-behavioral](../10-behavioral/README.md) — customer focus, conflict, prioritisation and pressure. Have at least two escalation stories and one churn-save story ready.
- [13-cs-fundamentals](../13-cs-fundamentals/README.md) — networking (DNS, TLS, HTTP, load balancing) comes up in technical screens for cloud TAM roles.
- [07-data-and-sql](../07-data-and-sql/README.md) — basic SQL for pulling usage and cost data; section A prompts are enough.
- [01-recruiter-and-motivation](../01-recruiter-and-motivation/README.md) — adapt "Why FDE?" into "Why post-sale, why TAM?".
- [14-questions-to-ask](../14-questions-to-ask/README.md) — add questions about book size, segmentation and how TAMs are measured.

---

## Role-specific questions

### Account health and adoption

- [ ] How do you measure the health of a customer account?
- [ ] What leading indicators tell you an account is at risk of churning?
- [ ] A customer bought 1,000 seats but only 300 are active after six months. What do you do?
- [ ] How do you build a success plan for a new enterprise account?
- [ ] How do you drive adoption of a feature the customer paid for but is not using?
- [ ] What is the difference between a CSM, a TAM, and a pre-sales Customer Engineer?

### QBRs and executive relationships

- [ ] How do you prepare and run a quarterly business review (QBR)?
- [ ] Exercise: present a 10-minute QBR for a mock account using the data provided.
- [ ] How do you show value to an executive sponsor who was not involved in the purchase?
- [ ] How do you build multi-threaded relationships in an account?

### Escalation management

- [ ] Walk me through how you manage a Severity 1 escalation for a strategic customer.
- [ ] A customer emails your CEO complaining about repeated outages. How do you respond?
- [ ] How do you decide when to escalate a support case internally, and to whom?
- [ ] How do you write a customer-facing incident summary after an outage?
- [ ] Role-play: a frustrated customer says your platform caused their outage, but your data suggests their configuration did.

### Technical troubleshooting and advisory

- [ ] A customer reports intermittent 502 errors behind a Google Cloud external Application Load Balancer. Walk through troubleshooting it with them on a call.
- [ ] A customer's BigQuery bill tripled this month. How do you help them?
- [ ] Exercise: triage a queue of eight open tickets across your accounts and explain your order.
- [ ] How do you run an architecture or best-practice review for an existing customer?
- [ ] A customer has a major launch or peak event in six weeks. How do you prepare them?
- [ ] A customer wants to migrate 200 on-premises VMs to Google Cloud in six months. How do you help as their TAM?

### Renewals, expansion and commercial judgment

- [ ] A strategic renewal is 90 days out and the account is red. What is your plan?
- [ ] How do you work with the account executive on expansion, and where does your role stop?
- [ ] A customer asks for a discount at renewal because they are under-using the product. How do you respond?
- [ ] A customer is evaluating a competitor for a new workload. What do you do?
- [ ] A customer asks you to commit to a date for a roadmap feature. How do you handle it?

### Managing a book of business

- [ ] You own 15 accounts. How do you decide where to spend your week?
- [ ] How do you segment a book of business into high-touch, low-touch and tech-touch?
- [ ] Two of your accounts have Severity 1 issues at the same time. What do you do?
- [ ] What tools and data do you use to run a book of business?
- [ ] How do you write a feature request that a product manager will actually act on?
- [ ] Exercise: you inherit an account whose previous TAM left no notes. Run the first call.

### Googleyness, values and behavioral (role-specific)

- [ ] Explain to a customer when to choose Compute Engine, GKE or Cloud Run for a workload.
- [ ] Explain the shared responsibility model and how it shapes a conversation after a customer security incident.
- [ ] Googleyness: tell me about a time you did the right thing for a customer even though it cost your company in the short term.
- [ ] Googleyness: tell me about a time you worked with a team whose goals conflicted with yours.
- [ ] Tell me about a time you saved an at-risk account.
- [ ] Tell me about a time you told a customer something they did not want to hear about their own setup.

---

## Model answers

### Account health and adoption

#### How do you measure the health of a customer account?

Use a small, explainable score built from four groups of signals, and always pair it with your own judgment.

- **Product usage:** active users vs. licensed seats, depth of use (key features adopted), consumption trend vs. commitment (for usage-based products), week-over-week trend rather than a single number.
- **Support experience:** open ticket count, Sev 1/2 volume, ticket age, repeat issues, CSAT on closed tickets.
- **Relationship:** do we have an engaged executive sponsor, a technical champion, and contacts in more than one team? Did they attend the last QBR? How quickly do they reply?
- **Commercial and outcome:** progress against the success plan goals, renewal date, budget changes, reorganisations, M&A, competitor activity.

I colour each group green / amber / red with a one-line reason, then roll up to an overall status. The reason matters more than the colour: "amber — sponsor left in March, usage flat, two P2s open over 30 days" tells the AE and my manager what to do. I review scores weekly for top accounts and monthly for the rest, and I sanity-check them: a quiet account with no tickets is often disengaged, not healthy.

#### What leading indicators tell you an account is at risk of churning?

Lagging indicators (a cancellation notice, a failed renewal) arrive too late. Leading indicators I watch:

- **Usage decline**: active users or consumption falling for 4+ weeks, or a key workload being moved off.
- **Champion or sponsor change**: the person who bought us leaves, or a new CIO/CTO arrives (new leaders review vendors).
- **Engagement drop**: cancelled cadence calls, no QBR attendance, slow replies.
- **Support pain**: repeated escalations on the same issue, long-open tickets, low CSAT, "this keeps happening" language.
- **Business signals**: layoffs, budget freezes, acquisition, cost-cutting announcements.
- **Competitive signals**: RFPs, questions about data export, a competitor's logo at their events, procurement asking for usage reports.
- **Unmet outcomes**: the goals in the success plan have not moved.

One signal is a prompt to ask questions; two or three together means I flag the account as at risk, tell the AE and my manager, and start a save plan with a named owner and dates.

#### A customer bought 1,000 seats but only 300 are active after six months. What do you do?

First diagnose, then act, then agree the target with the customer.

1. **Segment the data.** Which teams, regions or roles are active? Is it 300 heavy users and 700 never-logged-in, or 1,000 light users? Is SSO/SCIM provisioning even done for the other 700?
2. **Talk to both groups.** Interview active users (what value do they get?) and non-users (blocker: access, training, workflow fit, competing tool, no mandate?).
3. **Classify blockers** into technical (provisioning, integrations, performance), enablement (no training, no champions), and organisational (no executive mandate, old tool still running).
4. **Build an adoption plan** with the sponsor: fix technical blockers first, run role-based training, recruit champions per team, publish internal success stories, and agree on retiring the old tool.
5. **Set measurable targets** such as 60% weekly active in 90 days, reviewed fortnightly.

I raise it early and honestly with the sponsor rather than hoping it improves before renewal. If some seats will never be used, I would rather discuss right-sizing openly than lose trust and the whole account at renewal.

#### How do you build a success plan for a new enterprise account?

A success plan is a short shared document, owned jointly with the customer, that connects their business goals to how they will use the product.

- **Start from the sales handoff:** why they bought, what was promised, who signed, which use cases were sold. Confirm all of it with the customer; handoffs lose detail.
- **Business objectives:** 2–4 outcomes in their language with a metric (for example "cut incident detection time from 30 to 5 minutes").
- **Use cases and milestones:** what goes live when, with technical prerequisites (SSO, integrations, data onboarding).
- **Stakeholders:** executive sponsor, technical lead, admins, end-user groups, and our side (TAM, AE, support, professional services).
- **Risks and dependencies:** security review, migrations, competing priorities.
- **Cadence:** weekly or bi-weekly working call, quarterly business review.
- **Success measures:** adoption and outcome metrics we will report at each QBR.

I keep it to one or two pages, review it at every QBR, and update it when goals change. If the customer will not engage with the plan, that itself is a health signal.

#### How do you drive adoption of a feature the customer paid for but is not using?

First check that the feature actually maps to a goal the customer has; pushing a feature they do not need wastes their time and mine.

If it does map to a goal:

1. **Find the blocker.** Ask the owning team directly: is it unaware, unsure how to configure it, blocked by security, or does it compete with an existing tool?
2. **Show it on their data.** A 30-minute working session that configures it for one real use case beats any generic demo.
3. **Start with one team.** Pick a motivated team, get a quick visible result, and measure it.
4. **Make it repeatable.** Write a short internal guide, run enablement sessions, and help them create internal champions.
5. **Report the outcome** at the next QBR in business terms ("alerts reduced by 40%"), which builds pull from other teams.

I track adoption of key features in my health score, so I can see whether the push worked and stop pushing if it is not delivering value.

#### What is the difference between a CSM, a TAM, and a pre-sales Customer Engineer?

Titles vary by company, but the usual split is:

| | Customer Success Manager (CSM) | Technical Account Manager (TAM) | Pre-sales Customer Engineer / SE |
|---|---|---|---|
| Stage | Post-sale | Post-sale | Pre-sale |
| Focus | Business outcomes, adoption, renewal, relationship | Technical health, escalations, architecture guidance, operational readiness | Winning new deals and workloads: discovery, demos, POCs, technical validation |
| Typical counterpart | Business owners, executives | Engineers, platform and operations teams | Buyers and technical evaluators |
| Measured on | Retention, adoption, NPS | Escalation outcomes, technical health, CSAT, retention contribution | Technical win rate, pipeline, revenue |

At Google Cloud the "Customer Engineer" title is mainly pre-sales; TAMs are post-sale. At many smaller SaaS companies one person does both CSM and TAM work. In an interview, I ask how the role is split there and say where my strengths fit.

### QBRs and executive relationships

#### How do you prepare and run a quarterly business review (QBR)?

A QBR is for the customer's leaders to see value and agree priorities, not for us to read out dashboards.

**Prepare (2–3 weeks before):**
- Re-read the success plan and last QBR's actions.
- Pull data: adoption trends, outcomes achieved, support and incident summary, cost or consumption.
- Pre-brief the customer champion: what do their leaders care about this quarter? Any surprises to avoid?
- Align internally with the AE (commercial topics), support (incident history) and product (roadmap you can share).

**Structure (45–60 minutes):**
1. Their goals and what changed in their business (5 min, let them talk).
2. Value delivered against goals, in their metrics (10 min).
3. Honest review of problems: incidents, open escalations, what we did about them (10 min).
4. Recommendations and roadmap relevant to them (10 min).
5. Agreed priorities and success plan for next quarter, with owners and dates (10 min).

**After:** send a written summary and action list within 48 hours and track actions in the cadence calls. Good QBRs end with decisions, not applause.

#### Exercise: present a 10-minute QBR for a mock account using the data provided.

What they are testing: can you turn raw data into a short, honest story with a clear ask, and can you handle executive questions.

A 10-minute structure:

1. **Headline (30 seconds):** one sentence summary. "Usage is up 35% and you hit your detection-time goal, but two outages in August hurt confidence, and we have a plan to address it."
2. **Their goals recap (1 minute):** the 2–3 objectives from the success plan.
3. **Value delivered (3 minutes):** 2–3 charts at most, each with one takeaway written as the slide title. Convert usage into outcome (time saved, cost avoided, incidents reduced).
4. **What did not go well (2 minutes):** incidents, adoption gaps, open escalations. Own it, show root cause and fix.
5. **Recommendations (2 minutes):** three specific actions, for example enable a feature, run training for a lagging team, architecture review before peak season.
6. **Asks and next steps (1.5 minutes):** what you need from them (sponsor time, a decision) and the date of the next check-in.

Tips: lead with the headline, do not hide bad news (the interviewer usually plants some in the data), quantify, and stop early to leave time for questions. If a number looks wrong in the data, say so rather than presenting it.

#### How do you show value to an executive sponsor who was not involved in the purchase?

A new sponsor has no attachment to the decision and may see us as a cost line to cut. I treat it like a fresh sale, based on evidence.

1. **Learn their priorities first.** Ask for a 30-minute introduction and spend most of it asking about their goals for the year, pressures and how they measure success.
2. **Map our impact to those priorities**, not to the old sponsor's. If they care about cost, show cost avoided; if about reliability, show incident and uptime trends.
3. **Use their numbers.** A one-page value summary: what the product does for them today, which teams depend on it, measured outcomes, and what it would take to replace it.
4. **Bring a credible next step**: a recommendation that helps their agenda, such as a cost optimisation review.
5. **Include our leadership** at the right level if appropriate, peer-to-peer.

I also check who else in their team uses and values the product, because a sponsor listens more to their own people than to a vendor.

#### How do you build multi-threaded relationships in an account?

Single-threaded accounts (one champion) are fragile: if that person leaves, the account is at risk.

- **Map the account:** an org chart of everyone who buys, uses, operates, secures or pays for the product, and who influences each.
- **Aim for coverage at three levels:** executive sponsor, technical leads in each using team, and day-to-day operators and admins.
- **Give each a reason to talk to you:** executives get value summaries and peer access; technical leads get architecture reviews and early roadmap previews; operators get fast help and training.
- **Expand through value:** every new team onboarded or every escalation resolved is a chance to meet new people. Ask your champion "who else should I be talking to?".
- **Coordinate internally** so the AE, TAM, support and executives are not all calling the same person.

I track relationship coverage as part of the health score and review it each quarter: if all my contacts are in one team, that is a risk to fix before it matters.

### Escalation management

#### Walk me through how you manage a Severity 1 escalation for a strategic customer.

My role in a Sev 1 is coordination and communication, so engineers can focus on the fix.

1. **Confirm and size it (first 15 minutes):** what is broken, business impact (revenue, users, regulatory), when it started, what changed. Make sure a Sev 1 support case exists with the right priority.
2. **Mobilise:** get the right support and engineering owners on a bridge; page the on-call through the formal process, not side channels. Inform my manager and the AE.
3. **Set communication rhythm:** one customer-facing channel, updates at a fixed interval (for example every 30–60 minutes) even when there is no news, each with status, impact, current actions, next update time.
4. **Protect the customer's interests internally:** push for workarounds and mitigation first (rollback, failover, traffic shift), root cause second.
5. **Track facts:** a timeline of what happened and what was tried; it will feed the incident report.
6. **Close properly:** confirm with the customer it is resolved, agree monitoring, then deliver an incident summary with root cause and preventive actions by an agreed date.
7. **Follow up:** track preventive actions to completion and review at the next cadence call.

What I avoid: guessing at root cause in front of the customer, promising ETAs engineering has not given, and going silent.

#### A customer emails your CEO complaining about repeated outages. How do you respond?

Treat it as a symptom of lost trust, not just a complaint to answer.

1. **Respond fast and in a coordinated way.** Agree internally who replies (usually an executive, drafted by me) so the customer does not get three different answers. Acknowledge within hours: we have received it, we take it seriously, here is who owns it, here is when you will hear from us next.
2. **Gather facts:** list every incident in the period, impact, root causes, status of preventive actions. Check whether incidents were ours, theirs or shared.
3. **Meet them:** propose a call with their leader and the right executive from our side. Listen first; let them describe the business impact in their words.
4. **Present a plan:** a reliability review of their architecture and our platform, specific preventive actions with owners and dates, and a regular check-in until trust is rebuilt. Discuss service credits if the SLA applies; the AE and contract owner handle that.
5. **Deliver and report:** close the actions and report progress visibly.

Internally I also ask why it reached the CEO: did our escalation path fail, or did they not know it? Then fix that so the next issue reaches me first.

#### How do you decide when to escalate a support case internally, and to whom?

I escalate based on impact and trajectory, not on how loud the customer is, and I use the formal path before favours.

Escalate when:
- **Business impact is high:** production down, revenue or data at risk, a regulatory deadline, a launch blocked.
- **The case is stuck:** no meaningful progress within the expected time for its priority, or ping-pong between teams.
- **It is a pattern:** the same issue across several customers or recurring at this customer.
- **The relationship is at risk:** a renewal is near, or an executive is involved.

To whom, in order:
1. Support duty manager or escalation manager, to raise priority and get the right engineer.
2. Engineering on-call or the product team owning the component, through their documented process.
3. My manager and the AE, when commercial or executive attention is needed.
4. Executive escalation only when the earlier levels have not worked and the impact justifies it.

Every escalation I raise includes customer impact, what has been tried, what I need, and by when. Escalations that are clear and rare get answered quickly; constant escalation devalues them.

#### How do you write a customer-facing incident summary after an outage?

Clear, factual, blameless and focused on what the customer needs to know. A typical structure:

1. **Summary:** two or three sentences on what happened, impact and duration.
2. **Customer impact:** which services, regions or features; start and end times with time zone; what users experienced.
3. **Timeline:** detection, key actions, mitigation, resolution.
4. **Root cause:** in plain language, at the level of detail the customer needs; avoid internal names and jargon.
5. **Resolution:** what fixed it.
6. **Preventive actions:** specific changes, owners (by team) and target dates.
7. **Recommendations for the customer** (if any): for example, configuring retries or multi-region failover.

Rules: get it reviewed by engineering for accuracy and by legal/communications if the company requires it; do not speculate; do not blame individuals or third parties by name; send within the agreed timeframe; and follow up when preventive actions complete. An honest summary that admits fault usually builds more trust than a defensive one.

#### Role-play: a frustrated customer says your platform caused their outage, but your data suggests their configuration did.

What the interviewer is testing: can you stay calm, avoid blame, and lead toward the facts without damaging the relationship.

How I would run it:

1. **Acknowledge impact first.** "I understand your service was down for two hours and that hurt your customers. I want to get to the bottom of this with you."
2. **Do not argue the cause yet.** Ask for their timeline and what they observed. Listen for details that do or do not match our data.
3. **Share data, not conclusions.** "Our logs show the health checks to your backends started failing at 14:02, right after a firewall rule change in your project. Can we look at that together?"
4. **Make it joint.** Offer to walk through it on a screen-share with their engineer. Let the data lead them to the conclusion.
5. **Own our part if there is any.** Maybe our error messages were unclear or our documentation was weak; say so and file the improvement.
6. **Help prevent it.** Offer a configuration review, alerts on the relevant change, or a guard rail.
7. **Confirm in writing** a neutral summary of findings.

Avoid saying "this was your fault". The goal is a shared understanding and a fix, not winning the argument.

### Technical troubleshooting and advisory

#### A customer reports intermittent 502 errors behind a Google Cloud external Application Load Balancer. Walk through troubleshooting it with them on a call.

Narrate the plan so the customer can follow, and gather facts before guessing.

1. **Scope:** what percentage of requests, since when, which URLs or backends, any recent change (deploy, autoscaling, firewall, config).
2. **Read the load balancer logs** in Cloud Logging. The `jsonPayload.statusDetails` field explains who generated the 502, for example `failed_to_pick_backend` (no healthy backend), `backend_connection_closed_before_data_sent_to_client` or `backend_timeout`.
3. **Check backend health:** is the health check passing on all instances? Firewall rules must allow Google's health-check ranges (`35.191.0.0/16` and `130.211.0.0/22`).
4. **Check keepalive mismatch:** if the backend web server closes idle connections sooner than the load balancer expects, the LB can reuse a closed connection and return a 502. Google's guidance is to set the backend keepalive timeout longer than the LB's (600 seconds for the global external Application Load Balancer); for nginx, raise `keepalive_timeout`.
5. **Check backend timeout** (default 30 seconds) against slow requests.
6. **Check capacity:** instances restarting, OOM, autoscaler scaling in under load, connection draining settings.
7. **Confirm the fix** with a metric (502 rate) and document it for the customer.

If it looks like a platform issue, I open a support case with the evidence collected so far.

#### A customer's BigQuery bill tripled this month. How do you help them?

Find what drove the change, fix the cause, then add guard rails.

1. **Identify the cost type:** query compute (on-demand pricing bills bytes processed; capacity/editions pricing bills slots), storage, or streaming inserts. Use the billing export or Billing reports grouped by SKU.
2. **Find the top queries and users** with `INFORMATION_SCHEMA.JOBS`:

```sql
SELECT user_email,
       SUM(total_bytes_billed) / POW(1024, 4) AS tib_billed,
       COUNT(*) AS queries
FROM `region-us`.INFORMATION_SCHEMA.JOBS
WHERE creation_time > TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 30 DAY)
  AND job_type = 'QUERY'
GROUP BY user_email
ORDER BY tib_billed DESC
LIMIT 20;
```

3. **Typical causes:** a new dashboard refreshing every few minutes, `SELECT *` on wide tables, queries on unpartitioned tables, a scheduled query or pipeline accidentally running more often.
4. **Fixes:** partition and cluster large tables, require partition filters, select only needed columns, materialise or cache common results, fix the schedule.
5. **Guard rails:** set maximum bytes billed on queries, custom quotas per user or project, budget alerts, and consider capacity pricing if workloads are steady.

I present findings as a short report with savings estimates. Helping a customer spend less builds trust and protects the renewal.

#### Exercise: triage a queue of eight open tickets across your accounts and explain your order.

What they are testing: a consistent, explainable prioritisation method, not one right order.

My method:
1. **Severity and business impact first:** production down or data loss beats everything; then degraded production; then blocked projects; then questions.
2. **Then time pressure:** a launch tomorrow or an SLA about to breach moves up.
3. **Then account context:** strategic accounts and accounts near renewal or already at risk get more attention when impact is otherwise similar.
4. **Then effort:** quick wins I can close in minutes I batch together so they do not sit.
5. **Patterns:** if two tickets describe the same symptom at different customers, I group them and alert support, because it may be a platform issue.

For each ticket I state an action, not just a rank: escalate to engineering, reply with a workaround, ask for missing logs, schedule a call, or redirect to documentation. I also flag any ticket where the priority the customer chose looks wrong, and correct it with them politely. Finally I explain what I would communicate to customers whose tickets I put lower, so nobody is left in silence.

#### How do you run an architecture or best-practice review for an existing customer?

A structured review finds risks before they become incidents. On cloud platforms I base it on the provider's framework (Google Cloud Architecture Framework, AWS Well-Architected).

1. **Scope:** one critical workload, not the whole estate. Agree goals with the customer (reliability before a launch, cost, security).
2. **Gather:** architecture diagram, configs, monitoring, recent incidents, and a working session with their engineers.
3. **Assess by pillar:** reliability (single points of failure, zones/regions, backups, tested restores), security (IAM least privilege, public exposure, key management), performance and scaling, cost, and operations (monitoring, alerting, runbooks, change process).
4. **Report:** findings ranked by risk and effort, each with a concrete recommendation and link to documentation. Separate "fix now" from "plan".
5. **Follow through:** track remediation in the cadence calls and repeat the review yearly or before major changes.

I keep the tone collaborative: their engineers know constraints I do not, and a review that feels like an audit gets ignored.

#### A customer has a major launch or peak event in six weeks. How do you prepare them?

Treat it as a small project with a checklist and a dedicated support plan (AWS offers this formally as Infrastructure Event Management for some support tiers).

- **Week 6–5: understand the event.** Expected traffic and growth multiple, critical user journeys, timeline, freeze dates, business impact of failure.
- **Week 5–4: architecture and capacity review.** Single points of failure, autoscaling limits, database capacity, caching, third-party dependencies. Check **quotas and limits** and request increases early, since some take time.
- **Week 4–2: load test** at the expected peak plus margin; fix bottlenecks; rehearse failover and rollback.
- **Week 2–1: operational readiness.** Dashboards and alerts on key journeys, runbooks, on-call rota on both sides, escalation contacts, change freeze.
- **Event day:** I am available; support is pre-briefed with the case details; we have a shared channel.
- **After:** review what happened, capture lessons, and feed improvements into the success plan.

The most common failure I would watch for is a forgotten quota or a downstream dependency nobody load-tested.

#### A customer wants to migrate 200 on-premises VMs to Google Cloud in six months. How do you help as their TAM?

As TAM I am not doing the migration myself; I make sure it is planned well, staffed and de-risked.

1. **Clarify goals and constraints:** why migrate (data centre exit, cost, modernisation), hard deadline, compliance, downtime tolerance.
2. **Assess the estate:** use discovery tooling (Google Cloud's Migration Center) to inventory VMs, dependencies, utilisation and licensing. Group applications into waves.
3. **Choose a strategy per application:** rehost (lift and shift, for example with Migrate to Virtual Machines), replatform (to managed databases, GKE), or retire. Most 6-month programmes rehost most VMs and modernise later.
4. **Set up foundations first:** landing zone, network connectivity (Cloud VPN or Interconnect), IAM, logging, billing structure.
5. **Bring in the right people:** Google Cloud professional services or a partner for execution; the AE for migration programmes or incentives that may be available.
6. **Run in waves:** a low-risk pilot wave first, then larger waves, each with test and rollback plans.
7. **Govern:** weekly status, risk log, cost tracking against forecast.

My ongoing job is to remove blockers (quotas, support cases), keep stakeholders aligned and make sure post-migration operations are ready.

### Renewals, expansion and commercial judgment

#### A strategic renewal is 90 days out and the account is red. What is your plan?

Ninety days is late but workable. Run it as a save plan with the AE, with weekly reviews.

**Week 1: diagnose honestly.**
- Why is it red? Usage, unresolved technical pain, sponsor change, budget, competitor? Talk to the champion and, if possible, the decision-maker directly.
- Who decides, by when, and what is their procurement process?

**Weeks 2–6: fix what we can control.**
- Close or credibly plan every open escalation, with engineering commitments in writing.
- Rapid adoption push on the use cases tied to their goals.
- Build a value summary: outcomes delivered, usage, what replacing us would cost them in migration effort and risk.
- Get executive alignment: our executive meets theirs to acknowledge issues and commit to fixes.

**Weeks 6–12: shape the renewal.**
- With the AE, agree the commercial approach: right-size, change term, bundle services. That is the AE's decision; I bring the technical and usage facts.
- Agree a success plan for the next term with checkpoints.

Throughout: tell my manager and the AE the true risk level each week. Over-optimistic forecasting is worse than a loss you saw coming.

#### How do you work with the account executive on expansion, and where does your role stop?

The AE owns the commercial relationship and the number; I own technical health and trust. Expansion works best when we share information early and play our own parts.

**What I bring:**
- Expansion signals from technical conversations: a new team asking for access, workloads approaching plan limits, a new project the product could support.
- Technical validation: whether the use case actually fits, what it would take to implement, and risks.
- Credibility: customers trust a TAM who is not selling to them, so I protect that.

**What I do not do:**
- Negotiate pricing, discounts or contract terms.
- Push products that do not fit the customer's goals.
- Promise roadmap features or dates to help close a deal.

**How we work:** a regular account sync (weekly for top accounts), a shared account plan, and agreement on who speaks for what in customer meetings. If I think an expansion would hurt the customer or the relationship (for example, they are not ready operationally), I tell the AE directly and suggest a sequence that works.

#### A customer asks for a discount at renewal because they are under-using the product. How do you respond?

Pricing is the AE's decision, so I do not negotiate alone, but I can shape the conversation.

1. **Acknowledge and take it seriously.** Their concern is legitimate: they are paying for value they are not getting.
2. **Understand why usage is low.** Was it over-bought, or blocked by onboarding issues, poor fit or a reorganisation? The answer changes the response.
3. **Bring options to the AE:**
   - If adoption was blocked by us or by fixable issues: a time-bound adoption plan (training, technical help, maybe professional services) so they reach the value they paid for.
   - If they genuinely over-bought: right-sizing may be the honest answer. Keeping a smaller, happy customer is better than a full-price customer who churns next year.
   - Alternatives such as changing product mix or term length.
4. **Respond jointly** with the AE, with data on usage and a plan.

I avoid becoming defensive or dismissing the request, and I avoid promising a discount I cannot give. I also take the lesson internally: the under-use should have shown up in my health score months earlier.

#### A customer is evaluating a competitor for a new workload. What do you do?

Do not panic and do not attack the competitor. Find out why and compete on fit and value.

1. **Ask directly and calmly:** what is the workload, what are their requirements, and why are they looking elsewhere? Sometimes it is a different team that does not know we offer it; sometimes it is price, a missing feature, or a past bad experience with us.
2. **Tell the AE immediately** and agree a plan; this is a competitive opportunity.
3. **Assess fit honestly.** If we fit, bring a pre-sales engineer or specialist to run a proper technical evaluation with their criteria. If we do not fit, say so; credibility on the existing account matters more.
4. **Use our advantages in their terms:** existing integration, security approvals already done, skills their team already has, consolidated billing and support.
5. **Address past pain** if it is the reason: show what was fixed.

If they still choose the competitor for that workload, I stay helpful and protect the existing relationship. A customer using two vendors is common and not a failure.

#### A customer asks you to commit to a date for a roadmap feature. How do you handle it?

I never commit to a date I do not control. Roadmaps change, and a missed promise hurts trust more than a clear "I cannot commit".

How I respond:
1. **Understand the need behind the feature:** what problem, how urgent, what happens if it is late. Often a workaround or another feature solves it.
2. **Share what I can, accurately:** whether it is on the roadmap and, if the company allows, the current target with clear caveats ("planned, not committed, may change"). Some companies require an NDA or product manager involvement for roadmap discussions.
3. **Raise it internally properly:** a feature request with customer name, business impact and revenue context, linked to the product backlog. Arrange a call with the product manager if the need is significant.
4. **Offer interim options:** workaround, partner solution, or professional services.
5. **Follow up** with updates on status so they do not have to chase.

If a contract depends on the feature, that becomes a formal commercial commitment for the AE, legal and product leadership to decide, not a TAM promise.

### Managing a book of business

#### You own 15 accounts. How do you decide where to spend your week?

I protect a structure so that urgent work does not consume all strategic work.

1. **Tier the accounts** by revenue, strategic importance and growth potential. Top tier gets weekly contact; others bi-weekly or monthly.
2. **Overlay risk and events:** a red account, an upcoming renewal, a launch or a migration moves an account up temporarily.
3. **Plan the week** roughly as: reactive work (escalations, tickets, ~30–40%), scheduled cadence calls and QBR preparation (~30–40%), proactive work such as reviews, adoption plans and documentation (~20–30%).
4. **Review on Monday:** health dashboard, open escalations, renewals in the next 120 days, actions from last week.
5. **Scale what repeats:** templates, one-to-many webinars or office hours for common topics, reusable best-practice documents.

I also make the trade-offs visible to my manager. If one account is consuming 50% of my time for weeks, that is a resourcing conversation, not something to absorb silently.

#### How do you segment a book of business into high-touch, low-touch and tech-touch?

Segment by value and need, then design a different engagement model for each.

| Segment | Typical criteria | Engagement |
|---|---|---|
| High-touch (named TAM) | Largest revenue, strategic logos, complex architecture, high expansion potential | Dedicated TAM, weekly or bi-weekly calls, QBRs, proactive reviews, escalation ownership |
| Low-touch (pooled) | Mid-size accounts | Shared team of TAMs, scheduled check-ins quarterly, help on request, programmatic reviews |
| Tech-touch (scaled) | Many smaller accounts | Automated onboarding emails, in-product guidance, webinars, community, documentation, health alerts that trigger human outreach |

Rules I would add:
- Use more than revenue: an account with complex architecture or strategic importance (reference customer, new market) may need more touch.
- Allow accounts to move segments temporarily when risk or opportunity changes.
- Review segmentation at least yearly and measure outcomes (retention, adoption) per segment to see whether the model works.

#### Two of your accounts have Severity 1 issues at the same time. What do you do?

I cannot be fully present on two bridges, so I make sure both are owned and communicated.

1. **Confirm both are true Sev 1s** with real production impact, and that each has a proper support case with on-call engineers working it.
2. **Compare impact:** customer business impact, number of users, data risk, contractual obligations. I personally attend the higher-impact one.
3. **Get coverage for the other:** ask my manager or a peer TAM to act as point of contact, give them a quick briefing (contacts, context, history), and agree they will message me with updates.
4. **Keep both customers informed:** each gets regular updates with clear owners. The second customer should not feel less important; they get a named person and a fixed update rhythm.
5. **Check whether they are related:** two Sev 1s at once may be one platform incident. If so, raise it immediately so it is handled as one incident.
6. **After:** follow-ups and incident summaries for both, and a team discussion about coverage for simultaneous escalations.

#### What tools and data do you use to run a book of business?

Typical stack (names vary by company):

- **CRM (for example Salesforce):** account, contacts, opportunities, renewal dates; the shared source of truth with the AE.
- **Customer success platform (for example Gainsight, Totango, ChurnZero):** health scores, success plans, playbooks, task reminders.
- **Support system (for example Zendesk, ServiceNow, Salesforce Service Cloud):** ticket volume, priorities, ageing, CSAT.
- **Product usage data:** internal dashboards or a warehouse with SQL access; I want the ability to query usage myself rather than wait for reports.
- **Incident and status tooling:** status page, incident management tool, post-incident reports.
- **Collaboration:** shared Slack or Teams channels with key customers, meeting notes, and a per-account document with architecture and history.

What matters is the habit: a single account plan per customer, notes after every call, actions with owners and dates, and a weekly review of the health dashboard. That also means a colleague could take over an account without starting from zero.

#### How do you write a feature request that a product manager will actually act on?

Product managers prioritise by impact and evidence. A good request makes the case easy.

- **Problem, not solution:** describe what the customer is trying to do and what blocks them. Include their proposed solution separately, as one option.
- **Who and how many:** customer names, segment, number of users affected; link other customers with the same need.
- **Business impact:** revenue at risk or opportunity, renewal date, competitive pressure, or cost of the workaround.
- **Current workaround** and why it is not enough.
- **Urgency:** a real deadline, if there is one, and why.
- **Evidence:** quotes, screenshots, ticket links, usage data.
- **Offer access:** "the customer's lead engineer is happy to join a call".

Then follow up: I link new customer requests to the same item instead of opening duplicates, and close the loop with the customer when the decision is made, including when the answer is no.

#### Exercise: you inherit an account whose previous TAM left no notes. Run the first call.

What they are testing: structured discovery, humility, and turning a weak handoff into a plan.

**Before the call:** read CRM history, contract and renewal date, support tickets for 12 months, usage trends, past QBR decks, and talk to the AE and support. Draft hypotheses about health.

**On the call (30–45 minutes):**
1. **Introduce and reset:** "I am your new TAM. I have read your history, but I want to hear it from you."
2. **Their goals:** what are they trying to achieve this year, and how does our product fit?
3. **Current state:** which teams use it, for what, what works well, what frustrates them.
4. **Open issues:** anything unresolved, promised or pending from the previous TAM. Write each down.
5. **People:** who else should I meet: sponsor, other teams, admins.
6. **Working model:** how often to meet, preferred channels, escalation path.

**After the call:** send a summary with what I heard, open actions with owners and dates, and a proposed cadence. Within 30 days: a draft success plan and a health assessment shared with the AE. Never blame the previous TAM in front of the customer.

### Googleyness, values and behavioral (role-specific)

#### Explain to a customer when to choose Compute Engine, GKE or Cloud Run for a workload.

Frame it as a trade-off between control and operational effort.

| | Compute Engine | GKE | Cloud Run |
|---|---|---|---|
| What it is | Virtual machines | Managed Kubernetes | Serverless containers |
| You manage | OS, patching, scaling setup, runtime | Kubernetes workloads and config (Autopilot mode removes node management) | Just the container and its settings |
| Best for | Lift-and-shift, legacy apps, special OS or licensing needs, GPUs with full control | Many microservices, teams already using Kubernetes, complex networking, portability | Stateless HTTP services, APIs, event-driven jobs, variable traffic |
| Scaling | Managed instance groups | Pod and node autoscaling | Automatic, including scale to zero |
| Cost model | Pay for VMs while running | Pay for nodes (Standard) or pod resources (Autopilot) | Pay per use (request or instance time) |

My recommendation: start with the most managed option that meets the requirements. Stateless web service with variable traffic: Cloud Run. Large platform team already on Kubernetes: GKE. An application that needs a specific OS or cannot be containerised: Compute Engine. Then confirm with their constraints: team skills, existing tooling, compliance, and latency needs.

#### Explain the shared responsibility model and how it shapes a conversation after a customer security incident.

The shared responsibility model says the cloud provider secures the underlying infrastructure (data centres, hardware, the hypervisor, the managed service itself), and the customer secures what they put on it and how they configure it: identities and IAM permissions, data, network exposure, application code, and keys. The split moves with the service: on VMs the customer also patches the OS; on fully managed services the provider does more. Google also describes this as "shared fate", offering secure defaults, blueprints and guidance to help customers on their side.

After a customer incident, for example a leaked service account key used to access storage:
- **Lead with help, not blame.** Help contain it first: revoke the key, review audit logs, scope the data accessed, engage the provider's security support.
- **Be clear and factual about responsibility** when it comes up: the investigation will show which layer failed.
- **Help prevent recurrence:** least-privilege IAM, avoiding long-lived keys (use workload identity), organisation policy constraints, alerts on unusual access.
- **Involve the right teams:** incident response, legal and communications follow their own processes; the TAM coordinates and does not make statements about liability.

#### Googleyness: tell me about a time you did the right thing for a customer even though it cost your company in the short term.

**What they are looking for:** integrity, long-term thinking, customer focus, and that you made the decision transparently with your team rather than going rogue. Google has described "Googleyness" as qualities such as comfort with ambiguity, a bias to action, collaboration, and doing the right thing; treat that as reported, not an official rubric.

**STAR outline:**
- **Situation:** [customer, what they were paying for, what you noticed, e.g. over-provisioning or an unsuitable product]
- **Task:** [your responsibility and the tension between short-term revenue and the customer's interest]
- **Action:** [how you validated the data, aligned with the AE and manager, and presented the recommendation to the customer]
- **Result:** [customer outcome, cost to your company, long-term outcome such as renewal, expansion or a reference]
- **Learning:** [what you now do routinely]

**Example (adapt to your own experience):** During a cost review I found a customer was paying for a large reserved capacity commitment they used at about 40%. I showed the data to my AE, and we agreed to recommend right-sizing at the next term, reducing annual revenue from that account by roughly [X]%. The customer renewed for three years instead of one and later moved an additional workload to us. Since then I include cost-efficiency findings in every QBR.

#### Googleyness: tell me about a time you worked with a team whose goals conflicted with yours.

**What they are looking for:** collaboration, empathy for the other team's incentives, and finding a shared goal rather than escalating early.

**STAR outline:**
- **Situation:** [the other team (engineering, sales, support) and the conflicting goals, e.g. engineering focused on a roadmap release while your customer needed a fix]
- **Task:** [what you needed and why it mattered]
- **Action:** [how you understood their constraints, quantified customer impact, proposed a compromise, and escalated only if needed]
- **Result:** [outcome for both teams and the customer; any process change]
- **Learning:** [what you would do earlier next time]

**Example (adapt to your own experience):** A customer needed a bug fix before their launch, but the owning engineering team was in a release freeze. I met their lead, learned the freeze protected a major release, and found that a configuration workaround could cover the customer for six weeks. I documented the workaround, got engineering to review it, and the fix shipped in the next release. The customer launched on time, and we added "pending fix with workaround" as a standard escalation status.

#### Tell me about a time you saved an at-risk account.

**What they are looking for:** early detection, honest diagnosis, a structured save plan, cross-functional coordination, and a measurable result. Also, what you would do differently.

**STAR outline:**
- **Situation:** [account, why at risk: usage drop, sponsor change, outages, competitor, and how you noticed]
- **Task:** [your goal, e.g. renewal in N days, and your role vs. the AE's]
- **Action:** [diagnosis conversations, fixes delivered, executive engagement, adoption plan, internal escalation]
- **Result:** [renewal outcome, health metrics after, relationship change]
- **Learning:** [the signal you now watch earlier]

**Example (adapt to your own experience):** A mid-size customer's weekly active users dropped 35% over two months after their platform lead left. I met the new lead, learned their team had never been trained and that an integration had broken after their SSO change. We fixed the integration within a week, ran two training sessions, and set a fortnightly check-in. Usage recovered within six weeks and they renewed. I added "key contact change" as an automatic trigger for an outreach call.

#### Tell me about a time you told a customer something they did not want to hear about their own setup.

**What they are looking for:** courage with tact, data-based recommendations, and keeping the relationship intact.

**STAR outline:**
- **Situation:** [the customer's architecture or practice, and the risk you saw, e.g. single-zone deployment before a peak season, or no tested backups]
- **Task:** [why it was your job to raise it, and why it was awkward: their architect designed it, budget, timing]
- **Action:** [how you prepared evidence, chose the audience and setting, framed it in terms of their goals, and offered options with effort and cost]
- **Result:** [what they decided, what happened later]
- **Learning:** [how you raise such issues now]

**Example (adapt to your own experience):** Before a customer's busiest season, I noticed their production database ran in a single zone with untested backups. Their architect was proud of the design, so I raised it privately first, with the incident history of similar setups and a short list of options and costs. He agreed to a restore test, which failed, and the team moved to a high-availability configuration two weeks before peak. I now include a backup-restore question in every architecture review.

---

## Prep checklist

1. Pick one cloud (or the company's product) and be able to explain its core compute, storage, networking, IAM and support model clearly, at whiteboard level.
2. Write a two-page mock success plan and a five-slide mock QBR for a fictional account, then present it out loud in 10 minutes.
3. Prepare four stories: a Sev 1 escalation you coordinated, an at-risk account you saved, a time you said no to a customer, and a time you influenced product or engineering.
4. Practise two role-plays with a friend: an angry executive after repeated outages, and a customer asking for a discount at renewal.
5. Learn the support model of the company: severity definitions, response-time targets, escalation paths, and what premium support or TAM tiers include.
6. Practise troubleshooting out loud on networking basics: DNS, TLS, load balancer 502/504, timeouts, IAM permission errors.
7. Write a sample customer-facing incident summary for a real public post-mortem, rewritten for a customer audience.
8. Know the commercial vocabulary: ARR, gross and net revenue retention, churn, expansion, consumption commitment, renewal forecast.
9. For Google, read the reported loop format (RRK, GCA, Googleyness & Leadership) and prepare structured answers; for AWS, map stories to the Leadership Principles.
10. Prepare questions about book size, segmentation, how TAMs are measured, and how TAMs and AEs split responsibilities.
