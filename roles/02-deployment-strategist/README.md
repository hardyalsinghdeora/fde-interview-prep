# Deployment Strategist (and other non-coding forward deployed roles)

A business-side forward deployed role that owns the customer's problem and outcome: framing what to solve, aligning stakeholders, proving value, and steering the engineers who build it.

## The role

**Day to day.** A Deployment Strategist works on the same accounts as the forward deployed engineers but owns the "what and why" rather than the code:

- **Problem framing:** working out which operational decision or workflow to change, and what success looks like.
- **Stakeholder work:** mapping who matters at the customer, getting executive sponsorship, winning over frontline users, and handling blockers.
- **Delivery steering:** turning the problem into a prioritised plan with the engineers, running workshops and user sessions, testing what was built with users, and driving adoption.
- **Value and growth:** measuring impact, building business cases, and finding the next use case so the account expands.
- **Writing and presenting:** memos, executive updates, and pitches, often to very senior people.

Travel and on-site time are typically similar to FDEs on the same account.

**Common titles / aliases:** Deployment Strategist (Palantir; widely reported internal nickname "Echo", with FDSEs reported as "Delta"), Forward Deployed Product Manager, Deployment Lead, Engagement Lead / Engagement Manager, AI Strategist, AI Deployment Manager, Solutions Strategist, Business Deployment roles at AI companies. Exact titles vary widely; read the job description rather than the title.

**Companies that hire for it:** Palantir is the best-known example (Deployment Strategist). Many AI labs, AI-native startups and enterprise software companies with forward deployed engineering teams also hire business-side counterparts under the titles above; check current postings, since these teams are new and change often.

**How it differs from FDE:**

| | Deployment Strategist | Forward Deployed Engineer |
|---|---|---|
| Owns | Problem, outcome, stakeholders, adoption, expansion | Technical design, build, integrations, reliability |
| Writes production code | Usually no (some light SQL, spreadsheets, or no-code platform configuration) | Yes |
| Interview core | Case / decomposition, behavioural, writing, communication | Coding, system design, decomposition, customer scenarios |
| Typical background | Consulting, operations, product, military/government, finance, analytics, quantitative degrees | Software engineering, data engineering, CS degrees |

Compared with management consulting: a Deployment Strategist stays until the solution is in use and produces measurable results on a software platform, rather than handing over recommendations.

**Who it suits:** people who like messy real-world problems, are comfortable with very senior and very frontline people in the same day, write and speak clearly, are numerate and curious about data and technology, and want ownership of outcomes rather than just advice.

**Skills mix:**

| Skill | Weight |
|---|---|
| Problem structuring / case solving | High |
| Stakeholder management & communication | High |
| Writing | High |
| Business acumen / ROI | High |
| Data and technical literacy | Med |
| Sales / account growth | Med |
| Coding | Low (none to light SQL/Python) |

**Career paths:** senior strategist → account or engagement lead → business or vertical lead; product management; general management or operations leadership (sometimes at a former customer); strategy roles; founding a company; venture capital.

## Typical interview loop

Loops change often; the rounds below are **typical / candidate-reported**, not guaranteed. Ask the recruiter what each round assesses.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Motivation, "why this role and company", travel, background | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md), this file |
| Case / decomposition interviews (often more than one) | Structuring a vague real-world problem, data reasoning, prioritisation | [04-decomposition](../../topics/04-decomposition/README.md), this file (cases) |
| Behavioural / values | Ownership, ambiguity, influence, resilience | [10-behavioral](../../topics/10-behavioral/README.md) |
| Customer role-play or stakeholder scenario (reported at some companies) | Discovery, handling pushback, expectation management | [09-customer-scenarios](../../topics/09-customer-scenarios/README.md), this file |
| Written or presentation exercise (reported at some companies) | Clear writing, synthesis, recommending under uncertainty | This file (writing exercises) |
| Light data / analytics (sometimes) | Reading a dataset, spotting issues, basic metrics | [07-data-and-sql](../../topics/07-data-and-sql/README.md) sections on SQL basics |
| Hiring manager / final | Fit, mission, judgment | [11-company-specific](../../topics/11-company-specific/README.md), [14-questions-to-ask](../../topics/14-questions-to-ask/README.md) |

Palantir's Deployment Strategist loop is commonly reported to centre on decomposition-style case interviews plus behavioural and "why Palantir" discussion; it does not usually include coding.

## Common rounds — use the shared sections

- [04 Decomposition](../../topics/04-decomposition/ANSWERS.md): the most important shared section. Practise every prompt out loud, focusing on the user, the decision and the data, not the technology.
- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): sections A (scoping), B (difficult situations), C (communication) and E (business acumen) map almost directly to this role.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md) and [00 story bank](../../topics/00-overview/ANSWERS.md): prepare stories for influence without authority, ambiguity, owning an outcome and handling conflict.
- [01 Recruiter & motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md): adapt "Why FDE?" into "Why a forward deployed business role?".
- [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md): for AI-company roles, know the concepts (RAG, agents, evals, hallucination) well enough to explain trade-offs to executives; skip implementation detail.
- [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md): basic SQL (joins, group by) and data quality concepts are enough.
- Skip or skim: [02 Coding](../../topics/02-coding/README.md), [03 System design](../../topics/03-system-design/README.md), [13 CS fundamentals](../../topics/13-cs-fundamentals/README.md).

## Role-specific questions

### Role understanding and motivation
- [ ] What does a Deployment Strategist do, and how is it different from a management consultant?
- [ ] Why a Deployment Strategist role rather than a forward deployed engineer, product manager or consultant?
- [ ] You will not write production code. How will you earn credibility with engineers and with the customer's technical team?

### Case interviews and problem framing
- [ ] Case: a regional hospital network says emergency department waits are too long. How would you approach it?
- [ ] Case: an airline wants to reduce flight delays caused by maintenance. Where do you start?
- [ ] Case: a government agency wants to "use AI" across its benefits-processing operations. Frame the problem.
- [ ] How do you structure an ambiguous problem when you only have a few minutes to think?
- [ ] The customer lists ten problems they want solved. How do you decide which to tackle first?
- [ ] How do you know you have found the root problem rather than a symptom?
- [ ] You are staffed on a customer in an industry you know nothing about and will be on-site in two weeks. How do you get up to speed?

### Stakeholder management
- [ ] How do you build a stakeholder map for a new account, and what do you do with it?
- [ ] The executive sponsor and the frontline users want different things. How do you resolve it?
- [ ] A powerful middle manager is quietly blocking the deployment. What do you do?
- [ ] How do you manage a customer executive who changes priorities every week?
- [ ] Tell me about a time you got people with conflicting incentives to agree on a decision.

### Business cases and ROI
- [ ] Build a business case for a predictive-maintenance deployment at a manufacturer. Walk through the numbers.
- [ ] How do you estimate value when the customer has no baseline data?
- [ ] The pilot delivered less value than promised. How do you present it to the sponsor?
- [ ] How do you prioritise use cases across an account by value and feasibility?
- [ ] Estimation: roughly how many staff hours a year does a mid-sized insurer spend on manual claims document review?

### Workshop facilitation and discovery
- [ ] Design a half-day discovery workshop with a customer's operations leadership.
- [ ] How do you run a workshop where the most senior person dominates the room?
- [ ] How do you turn a user-shadowing session into requirements engineers can build from?
- [ ] Role-play: run the first 15 minutes of a discovery meeting with a skeptical COO.

### Technical and data literacy
- [ ] How do you work with engineers to turn a business need into a build plan?
- [ ] A dashboard shows a 30% drop in throughput. How do you check the data is right before acting on it?
- [ ] What do you ask about the data before committing to a use case?
- [ ] Explain to a customer executive what a data integration layer or ontology is and why it matters.
- [ ] How would you explain what an LLM can and cannot reliably do in a customer's workflow?
- [ ] Exercise: you are given a small spreadsheet of orders and shipments. What would you analyse, and what could you conclude?

### Writing and presenting
- [ ] Exercise: write a one-page memo recommending whether a customer should expand a deployment.
- [ ] How do you write an executive update that gets read and acted on?
- [ ] Exercise: present a recommendation in five minutes to a panel playing the customer's leadership team.

### Account growth and outcome ownership
- [ ] How do you turn a successful first use case into expansion across the customer's organisation?
- [ ] How do you measure whether a deployment is succeeding, beyond "it is live"?
- [ ] The deployment is behind schedule because the customer's own team is not delivering. How do you handle it?
- [ ] Tell me about a time you owned an outcome you did not fully control.
- [ ] When would you recommend stopping a use case or walking away from a deployment?

### Company and mission
- [ ] Palantir works with defence, intelligence and government customers. How do you think about the ethics of that work?
- [ ] What do you know about the company's platform, and how would you position it for a new customer?

## Model answers

### Role understanding and motivation

#### What does a Deployment Strategist do, and how is it different from a management consultant?
A Deployment Strategist owns a customer's problem and outcome from framing to measurable results, working alongside engineers who build the solution on the company's platform.

Day to day that means: finding the decision or workflow worth changing, getting the right people at the customer committed, prioritising what the engineers build, testing it with users, driving adoption, measuring value and growing the account.

**Differences from consulting:**
- **Delivery, not recommendations.** A consultant often hands over a deck; a strategist stays until the software is used and the metric moves.
- **A product, not bespoke advice.** The work is built on a platform that the company maintains, so what works at one customer can be reused at the next.
- **Small, hands-on teams.** A strategist and a few engineers, not a large pyramid; you do the analysis, write the memos and sit with users yourself.
- **Incentives:** success is renewals and expansion because the customer got value, not billable hours.

The overlap is real: structured problem solving, executive communication and stakeholder management transfer directly from consulting.

#### Why a Deployment Strategist role rather than a forward deployed engineer, product manager or consultant?
Structure: what you enjoy, evidence you are good at it, and why this role fits better than the alternatives. Make it specific to you.

- **vs. FDE:** "My strongest contribution is framing the problem, aligning people and driving adoption, not writing production code. I am technical enough to work closely with engineers, but my leverage is on the business side."
- **vs. product manager:** "I want to be close to specific customers and operations, solving real problems in the field, rather than managing a roadmap from a distance. I also like that patterns from the field feed the product."
- **vs. consulting:** "In consulting I [or: people I've worked with] often stopped at the recommendation. I want to own whether it actually works."

Back it with one example: a time you went from problem to measured outcome. Finish with why this company: the customers, the mission, or the platform you have looked at.

#### You will not write production code. How will you earn credibility with engineers and with the customer's technical team?
Engineers respect people who make their work easier and more valuable, and who understand enough to avoid bad decisions.

- **Learn the platform hands-on:** go through the training, build simple things yourself (pipelines, dashboards, no-code apps) so I understand effort and constraints.
- **Bring clear problems:** well-defined user needs, priorities and acceptance criteria, rather than vague requests or shifting scope.
- **Protect their time:** shield them from churn in customer priorities; handle stakeholder noise; get access and data requests moving.
- **Be data-literate:** read SQL, check data myself, ask good questions about quality and edge cases.
- **Respect estimates and trade-offs:** ask "what would make this cheaper?" rather than pushing dates.
- **With the customer's technical team:** show I understand their systems and constraints, never overpromise what the platform does, and bring an engineer when the question needs one.

Credibility comes over the first few weeks from being useful, not from claiming technical depth.

### Case interviews and problem framing

#### Case: a regional hospital network says emergency department waits are too long. How would you approach it?
**Clarify (first 3–5 minutes):** which waits (arrival to triage, triage to doctor, decision to admit, total length of stay)? How long are they now versus target? Which hospitals? Who is asking, and what decision can they change: staffing, bed management, patient flow?

**Structure the drivers (flow of a patient):**
- **Demand:** arrival volume and timing, acuity mix, avoidable visits.
- **Process in the ED:** triage, waiting for a clinician, tests and imaging turnaround.
- **Exit block:** admitted patients waiting for an inpatient bed; often the largest cause, driven by discharge timing.
- **Capacity:** staff rosters versus arrival patterns, beds, cubicles.

**Data:** ED timestamps per patient, bed management system, staffing rosters, lab and imaging turnaround, discharge times.

**Hypothesis and first slice:** if most delay is "decision to admit → bed", focus on bed visibility and earlier discharges: a live view of beds and expected discharges for bed managers, with predicted admissions from the ED.

**Success metric:** median and 90th percentile time from decision-to-admit to bed, then total ED length of stay; watch for unintended effects (readmissions).

**Pilot:** one hospital, 8–12 weeks, then scale. Mention adoption: involve bed managers and nurses from day one.

#### Case: an airline wants to reduce flight delays caused by maintenance. Where do you start?
**Clarify:** how much of total delay is maintenance-related (minutes, cost, share of flights)? Which fleet and stations? Scheduled or unscheduled maintenance? What is the cost of a delay versus a cancellation?

**Break down maintenance delays:**
- **Unscheduled defects:** components failing unexpectedly → predictive maintenance using sensor and fault data.
- **Parts availability:** right part not at the right station → inventory positioning.
- **Labour and scheduling:** technicians not available when the aircraft is on the ground → shift and task planning.
- **Process and paperwork:** sign-offs, deferral decisions, communication between maintenance control and operations.

**Data:** delay codes, maintenance logs and work orders, aircraft sensor and fault messages, parts inventory, technician rosters, flight schedule.

**Prioritise:** quantify delay minutes by cause from delay codes. Suppose parts availability causes 35%: start with a tool that predicts likely parts needs per station from defect history and the next day's schedule, and shows maintenance control where shortfalls are.

**Measure:** maintenance-coded delay minutes per 100 departures, cancellations, and parts expediting cost. Start with one fleet type and two hub stations.

#### Case: a government agency wants to "use AI" across its benefits-processing operations. Frame the problem.
Reframe "use AI" into "which outcomes, for whom, measured how".

**Clarify:** what does the agency want: faster decisions, lower backlog, fewer errors, better citizen experience, lower cost? What are the legal constraints (who may make a determination, appeal rights, privacy, records retention)? Where are the bottlenecks today?

**Map the process:** application intake → document collection → eligibility checks → determination → payment → appeals and fraud review. For each stage: volume, time, error rate, staff hours.

**Identify candidate use cases and score them** on value (backlog hours saved, error reduction), feasibility (data available, digitised documents), and risk (impact on citizens if wrong, legal limits):
- Classifying and extracting data from submitted documents: high value, moderate risk with human review.
- Summarising case files for caseworkers: high value, low risk.
- Automated eligibility decisions: high value but high risk and often legally restricted; keep humans deciding.

**Recommendation:** start with assistive AI (extraction and summarisation) that keeps a caseworker making the decision, with measured accuracy, audit logs and an equity check across groups. Metric: time to determination and backlog size, with error and appeal rates as guardrails.

#### How do you structure an ambiguous problem when you only have a few minutes to think?
Use a repeatable sequence and say it aloud so the interviewer can follow:

1. **Restate the goal** in one sentence and ask 2–3 clarifying questions: who the user is, what decision they make, what success means.
2. **Pick a structure that fits:** a process flow (steps from start to end) for operations problems; a driver tree (metric = A × B, then break down A and B) for metric problems; stakeholders and their needs for adoption problems.
3. **Keep branches mutually exclusive and collectively exhaustive,** at two levels; do not go deeper yet.
4. **Form a hypothesis** about where the biggest lever is, and name the data that would confirm or reject it.
5. **Choose a first slice,** say what you would deliver in weeks, not months, and how you would measure it.
6. **Name risks** (data, adoption, ethics) and next steps.

Signpost often ("I'll look at three areas: demand, process, capacity") and check in with the interviewer. Being structured and explicit matters more than being right first time. See the framework in [00-overview](../../topics/00-overview/README.md#decomposition-palantir-style).

#### The customer lists ten problems they want solved. How do you decide which to tackle first?
Score them on a few explicit criteria with the customer, not alone.

- **Value:** money, time, risk or mission impact if solved; how many people benefit.
- **Feasibility:** is the data available and usable, can we integrate in weeks, does the platform support it?
- **Time to visible impact:** can a real user see a difference within one or two months?
- **Sponsorship:** is there an owner at the customer who wants it and will make users adopt it?
- **Strategic fit:** does it build foundations (data, integrations) that make the next problems cheaper?

Plot value against feasibility; start with a high-value, feasible problem with a committed owner, even if it is not the biggest problem on the list. An early win earns trust and access for the harder ones. Agree the ranking with the sponsor, write it down, and revisit it at a set interval rather than every week.

#### How do you know you have found the root problem rather than a symptom?
Use several checks:

- **Ask "why" repeatedly** and follow the chain to something the customer can act on. "Reports are late" → "data arrives late" → "suppliers email spreadsheets" → "no shared system with suppliers".
- **Go and see:** watch the work. Stated problems in meetings are often symptoms; observed workarounds point at causes.
- **Test with data:** if this is the cause, what else should be true? For example, if staffing causes delays, delays should rise when staffing drops. Check it.
- **Check across stakeholders:** if different groups describe the same underlying issue in different words, it is more likely a root cause.
- **Ask the counterfactual:** "If we fixed this perfectly, would the outcome metric move enough?" If not, it is not the main cause.

Root cause is often organisational (incentives, ownership, information flow) rather than technical; say so if the evidence points there.

#### You are staffed on a customer in an industry you know nothing about and will be on-site in two weeks. How do you get up to speed?
Aim to understand how the customer makes money (or delivers its mission), its key operational metrics and its vocabulary, not to become an expert.

- **Days 1–3, structure:** read the customer's annual report or budget documents, two or three industry primers, and recent news. Write a one-page "how this business works": value chain, main costs, regulators, the metrics leaders are judged on.
- **Days 3–7, people:** talk to colleagues who have worked with similar customers, read past account plans and deployment write-ups, and ask the sponsor for a short call with someone who can explain their operations.
- **Days 7–10, data and vocabulary:** look at any sample data or reports the customer can share; build a glossary of their terms and acronyms.
- **Before arriving:** write down your hypotheses and the questions you need answered, so the first week on-site is spent testing them.
- **On-site:** shadow frontline work early; it fills the gaps no document covers.

Be open with the customer that you are new to their industry and confident about the method; they value someone who asks good questions over someone who pretends.

### Stakeholder management

#### How do you build a stakeholder map for a new account, and what do you do with it?
**Build it in the first two weeks** from the sponsor, the champion, org charts and every meeting.

For each person or group, capture:
- **Role in the deployment:** decision-maker, budget holder, champion, end user, technical gatekeeper (IT, security, data owners), influencer, potential blocker.
- **What they care about:** their metrics, pressures and what success means for them personally.
- **Influence and stance:** high or low influence; supportive, neutral or opposed.
- **Relationship owner** on our side and last contact.

**What I do with it:**
- Plan engagement: keep high-influence supporters close and visible; convert high-influence neutrals with a win that matters to them; understand opponents' concerns early.
- Spot gaps: no executive sponsor, no IT contact, no end-user voice.
- Reduce single points of failure: if everything depends on one champion, build a second relationship.
- Share it with the account team and update it as people change roles.

Keep it internal and respectful; it describes interests, not personal judgments.

#### The executive sponsor and the frontline users want different things. How do you resolve it?
Find the shared outcome, then design so both get what matters most to them.

1. **Understand each side precisely.** The executive typically wants a metric (cost, throughput, visibility); users want their day to be easier and not to be monitored or blamed.
2. **Look for the overlap.** Usually the executive's metric improves only if users adopt the tool, and users adopt it only if it helps them. That is the argument to make to both.
3. **Make it concrete:** design the first release around a user pain point that also produces the data or result the executive needs. For example, a scheduling tool that saves planners time and gives leadership the live visibility they wanted as a by-product.
4. **Be transparent:** if the executive's request (for example, individual performance tracking) will kill adoption, tell the executive directly, with evidence, and suggest an alternative such as team-level metrics.
5. **Escalate the trade-off to the sponsor** if it cannot be reconciled; they own the decision, but should make it with the adoption risk understood.

#### A powerful middle manager is quietly blocking the deployment. What do you do?
Start by understanding, not going over their head.

1. **Find out why.** Meet one-to-one and ask about their concerns. Common reasons: fear of losing control or headcount, a past failed project, workload, being left out of the decision, or a real flaw in the plan.
2. **Address the real concern.** Involve them in design; give their team an early win; credit them visibly; adjust the plan if their objection is valid.
3. **Make the work easy for them:** small asks, clear timelines, doing the preparation for their team.
4. **Make progress visible** to the sponsor through normal updates, factually, including where things are blocked and why.
5. **Escalate only if needed,** and openly: "I'd like to bring this to [sponsor] together so we can agree a way forward." Going around them secretly usually creates a permanent enemy.

In the interview, show empathy for the manager's position as well as determination to deliver.

#### How do you manage a customer executive who changes priorities every week?
Make the cost of change visible and create a predictable rhythm for deciding.

- **Find the cause:** is the business genuinely changing, are they reacting to their own boss, or is there no agreed goal? Each needs a different fix.
- **Anchor on agreed outcomes:** revisit the original goals with them: "Does this new request serve goal X, or is X changing?"
- **Show trade-offs explicitly:** "We can start this, which delays Y by three weeks. Which matters more?" Executives usually decide well when the trade-off is clear.
- **Set a cadence:** a fortnightly priority review where changes are decided, with urgent exceptions only for genuine emergencies.
- **Protect the team:** keep the engineers working on the agreed priorities between reviews.
- **Write decisions down** and share them, so the history is clear.

If the changes reflect a real shift in their business, adapt quickly; that is the job. The aim is controlled change, not no change.

#### Tell me about a time you got people with conflicting incentives to agree on a decision.
**What they are looking for:** understanding each party's incentives, finding common ground, structured facilitation, and a decision that actually stuck.

**STAR outline:**
- **Situation:** [the groups involved, the decision, why their incentives conflicted].
- **Task:** [your role; why it was your job to get agreement].
- **Action:** how you learned each side's interests (one-to-ones), framed the shared goal, used data or options with trade-offs, facilitated the decision meeting, and documented the outcome.
- **Result:** [the decision, what happened next, measurable outcome].
- **Learning:** [what you would do differently].

**Example (adapt to your own experience):** "Sales wanted a new reporting tool to show pipeline daily; finance wanted to keep the monthly close process unchanged to avoid reconciliation errors. I met each lead separately and found both cared most about forecast accuracy. I proposed a daily view from the CRM marked as provisional, reconciled to finance figures monthly, and showed a mock-up with both definitions side by side. Both signed off in one meeting; forecast variance fell from 12% to 7% over two quarters. I learned that separate conversations first make the joint meeting short."

### Business cases and ROI

#### Build a business case for a predictive-maintenance deployment at a manufacturer. Walk through the numbers.
State assumptions, compute, then test sensitivity. Numbers below are illustrative.

**Baseline:**
- 50 critical machines, each with about 40 hours of unplanned downtime a year → 2,000 hours.
- Cost of an hour of unplanned downtime (lost margin, overtime, expediting): about $5,000 → **$10M a year**.

**Benefit:**
- Assume the model flags 30% of failures early enough to plan the repair → 600 hours affected.
- Planned repairs take about a third of the time and avoid knock-on costs → roughly 70% of those hours saved → **420 hours × $5,000 ≈ $2.1M a year**.
- Fewer emergency parts orders and overtime: perhaps **$0.3M**.
- Total ≈ **$2.4M a year**.

**Costs (year 1):** platform and deployment ≈ $0.8M; sensors and connectivity ≈ $0.2M; customer staff time ≈ $0.2M → **$1.2M**, lower in later years.

**Result:** year-1 net ≈ $1.2M; payback in about 6 months.

**Sensitivity:** if detection is 15% instead of 30%, benefit halves to about $1.2M and year 1 roughly breaks even. That tells you what to validate in the pilot: detection rate and lead time on two or three machine types, measured against maintenance logs. Present a range, not a single number.

#### How do you estimate value when the customer has no baseline data?
Create a baseline rather than skipping it.

- **Measure now, quickly:** time-and-motion sampling for a week (shadow users, log how long tasks take), or extract partial data from emails, tickets and spreadsheets.
- **Use proxies:** overtime hours, backlog size, error or rework counts, customer complaints, write-offs.
- **Ask experts structured questions:** "How many hours a week does this take? What happens when it goes wrong, and how often?" Take the range from several people, not one estimate.
- **Use external benchmarks** carefully, as a sanity check.
- **Run a controlled comparison in the pilot:** one site or team using the tool and a similar one not, or before and after over the same period.

Be explicit about uncertainty: present a range, show the assumptions, and agree with the sponsor up front how value will be measured during the pilot. That agreement matters more than the precision of the first estimate.

#### The pilot delivered less value than promised. How do you present it to the sponsor?
Lead with the facts, explain why, and come with a recommendation.

1. **Headline honestly:** "We targeted a 30% reduction in processing time; we achieved 12%."
2. **Explain the gap with evidence:** for example, adoption reached only 40% of users because of a missing integration; on the teams that used it fully, the reduction was 28%.
3. **Separate what worked from what did not:** this distinguishes "the approach is wrong" from "the rollout was incomplete".
4. **Recommend clearly:** fix the integration and extend the pilot by six weeks with a specific target; or narrow the scope to where it worked; or stop. Give the cost and expected value of each.
5. **Own our part:** if we over-estimated or under-delivered, say so.

Never inflate numbers or switch metrics after the fact; sponsors remember. Telling the truth early, with a plan, often strengthens the relationship. Before the meeting, brief the account team so nobody is surprised.

#### How do you prioritise use cases across an account by value and feasibility?
Build a simple, shared scoring framework and use it with the customer.

| Criterion | Questions |
|---|---|
| Value | Money, time, risk or mission impact; number of users; strategic importance to leadership |
| Feasibility | Data availability and quality; integration effort; platform fit; regulatory constraints |
| Adoption | Committed business owner; user appetite; change-management effort |
| Time to value | Weeks to first measurable result |
| Foundation | Does it reuse data and integrations already built, or create new ones others can reuse? |

Score each use case 1–5 per criterion, plot value against feasibility, and discuss rather than just add up scores. Typical sequence: a quick win first to build trust; then use cases that share the same data foundation, so each one gets cheaper; and plan the large, hard, high-value use case once the foundation and trust exist. Review the portfolio quarterly with the sponsor.

#### Estimation: roughly how many staff hours a year does a mid-sized insurer spend on manual claims document review?
State a structure, make round assumptions, compute, and sanity-check.

**Structure:** claims per year × share needing manual document review × average review time.

**Assumptions (illustrative):**
- A mid-sized insurer handles about 500,000 claims a year.
- About 40% need someone to read documents (medical reports, invoices, photos, police reports) → 200,000 claims.
- Average review time across simple and complex claims: about 30 minutes, some reviewed more than once → say 0.5 hours.

**Calculation:** 200,000 × 0.5 = **100,000 hours a year**, about **55 full-time staff** at around 1,800 working hours each.

**Sanity check:** a mid-sized insurer might have several hundred claims staff; document review being 10–20% of their time is plausible.

**So what:** if AI-assisted extraction and summarisation cut review time by 40%, that frees roughly 40,000 hours, or about 20 staff, for complex claims. Mention which assumption matters most (review time per claim) and how you would check it (a one-week time sample).

### Workshop facilitation and discovery

#### Design a half-day discovery workshop with a customer's operations leadership.
**Before:** agree the goal with the sponsor (for example, "agree the top three problems and a first pilot"), invite 6–10 people covering operations, frontline supervisors, IT/data and finance, interview two or three attendees in advance, and gather basic data so the discussion is grounded.

**Agenda (about 4 hours):**
1. **Opening (15 min):** sponsor states why this matters; I state the goal, agenda and ground rules.
2. **Current state (45 min):** walk through the end-to-end process on a whiteboard; attendees mark pain points, delays and workarounds.
3. **Problem deep dive (45 min):** for the top pain points, discuss causes, frequency, cost and who is affected. Use data where available.
4. **Break (15 min).**
5. **Opportunity ideas (45 min):** small groups propose what would change if they had the right information or tools; capture data needed.
6. **Prioritise (45 min):** score opportunities on value and feasibility; dot-vote; discuss the top three.
7. **Close (30 min):** agree the first pilot, success metric, owners, data access and next meeting.

**After:** send a written summary within 48 hours with the decisions and owners. The follow-up is where most workshops succeed or fail.

#### How do you run a workshop where the most senior person dominates the room?
Use structure so that good input does not depend on people speaking up against their boss.

- **Prepare the senior person:** before the session, explain that the goal is to hear the frontline view and ask them to speak last on key topics. Most leaders agree when asked privately.
- **Silent writing first:** everyone writes ideas on sticky notes or a shared board before discussion, so ideas are captured independently.
- **Round-robins** and small-group breakouts, each reporting back through a non-senior member.
- **Anonymous voting** for prioritisation.
- **Redirect politely in the room:** "That's helpful. I'd like to hear from the people who run this process daily. [Name], what does this look like on the night shift?"
- **Use data** to settle disagreements rather than seniority.

Afterwards, follow up one-to-one with quieter participants; they often share the most useful information privately.

#### How do you turn a user-shadowing session into requirements engineers can build from?
1. **Capture during the session:** each step the user takes, the systems and documents used, the decisions they make and what information each decision depends on, time taken, and every workaround (spreadsheets, phone calls, sticky notes). Ask "why did you do that?" after, not during, busy moments.
2. **Write it up the same day:** a simple process map with pain points marked, and quotes.
3. **Turn pain points into user stories** with acceptance criteria: "As a dispatcher, I need to see which crews are within 30 minutes of a job so I can assign the nearest one. Done when: the list updates within 1 minute of crew GPS changes and matches the radio log on test days."
4. **List data needs** for each story: source system, fields, freshness, owner.
5. **Prioritise** with the business owner and mark the minimum slice.
6. **Review with engineers,** who will spot data and feasibility issues, then validate the first prototype with the same users.

The goal is that an engineer understands the user's decision and constraints, not just a feature list.

#### Role-play: run the first 15 minutes of a discovery meeting with a skeptical COO.
**What they score:** listening, credibility without overselling, and leaving with a concrete next step.

**Shape of the 15 minutes:**
1. **Open briefly (1–2 min):** thank them, state the purpose ("understand your priorities and see if there's a problem where we can show value quickly, or tell you if there isn't"), and confirm the time.
2. **Ask about their priorities (5–7 min):** "What are the two or three operational metrics you're under most pressure on this year?" "Where do you lack the information you need to decide?" Follow up on specifics and numbers.
3. **Acknowledge skepticism directly:** "You've probably seen technology projects that didn't deliver. What went wrong?" Use the answer to shape the approach.
4. **Reflect back (2 min):** summarise what you heard and check it.
5. **Propose a small, low-risk next step (3 min):** for example, a two-hour session with their planning team and access to a sample of data, with a defined outcome in four weeks.

Avoid pitching features; talk about their problems. If you do not know something, say so and follow up.

### Technical and data literacy

#### How do you work with engineers to turn a business need into a build plan?
- **Start with the user and decision,** not the solution: share the user research, process map and success metric so engineers understand the "why".
- **Co-design the approach:** engineers propose how; I bring constraints from the customer (deadlines, users, policies) and challenge scope against value.
- **Break into thin slices:** each slice delivers something a user can try. Agree the first slice and its acceptance criteria.
- **Surface dependencies early:** data access, security approvals, customer IT resources. Owning these is usually my job.
- **Trade-offs explicitly:** when an estimate is large, ask what would make it smaller and what we would lose.
- **Set a rhythm:** short regular check-ins, demos to users every week or two, and a written plan with owners.

My role is to make sure the team builds the right thing and is not blocked; theirs is how to build it. Neither should make the other's decisions unilaterally.

#### A dashboard shows a 30% drop in throughput. How do you check the data is right before acting on it?
Treat a sudden large change as a possible data problem until shown otherwise.

- **Is the data complete?** Check row counts and the latest timestamp per source; a late or failed feed often looks like a drop.
- **Did a definition change?** New filters, a changed status code, a new site added or removed, time zone or calendar changes (a holiday, a month with fewer working days).
- **Is it everywhere or in one segment?** Break down by site, product, shift. A drop concentrated in one site whose feed changed points to data; a broad drop points to reality.
- **Cross-check another source:** shipping records, finance figures, or ask the site manager directly.
- **Check recent changes** to the pipeline or source systems.

If the data checks out, investigate causes with operations. Either way, tell stakeholders what you found and how confident you are, and add a data-freshness check to the dashboard if one was missing.

#### What do you ask about the data before committing to a use case?
- **Existence:** does the data needed for the decision exist? Where, in which systems?
- **Access:** who owns it, what approvals are needed, how long will access take?
- **Granularity and history:** is it at the right level (per transaction, per asset) and how many months or years exist?
- **Quality:** missing values, duplicates, inconsistent IDs across systems, manual entry errors. Ask for a sample and look at it.
- **Freshness:** how often is it updated versus how quickly the decision is made?
- **Joinability:** is there a common key to link sources (asset ID, customer ID), or will we need matching?
- **Sensitivity:** personal, health, financial, classified? Legal constraints on use?
- **Meaning:** what do the fields actually mean, and who can explain them?

If the answers are poor, either pick a different first use case or make fixing the data part of the plan and timeline, explicitly.

#### Explain to a customer executive what a data integration layer or ontology is and why it matters.
Plain-language version:

"Today your information is spread across many systems: orders in the ERP, maintenance in another tool, staffing in spreadsheets. Each uses its own codes and names, so answering a simple question like 'which customer orders are at risk because a machine is down?' needs someone to pull and match data by hand.

An integration layer connects those systems and keeps the data flowing in. An ontology goes one step further: it describes your business the way your people think about it, as real things such as plants, machines, orders and suppliers, and how they relate. The data from every system is attached to those things.

Why it matters: your teams can ask questions and take actions on the business, not on tables; new applications and AI tools can be built on the same foundation in weeks instead of starting from scratch each time; and permissions and audit apply consistently."

Then tie it to their problem with one concrete example from their operations.

#### How would you explain what an LLM can and cannot reliably do in a customer's workflow?
**Can do well:** read and summarise documents; extract fields from unstructured text; draft letters and responses; classify and route requests; answer questions grounded in a provided set of documents; help staff search large knowledge bases.

**Cannot do reliably on its own:** guarantee factual accuracy (it can produce confident errors, called hallucinations); perform precise calculations or apply complex rules without tools; know your internal facts unless given them; take legally binding decisions; explain its reasoning in a way that satisfies an audit.

**What that means for the workflow:**
- Ground it in your documents and show sources so staff can check.
- Keep a human approving anything with real consequences; start with drafting and assisting.
- Measure accuracy on your own examples before go-live, and keep monitoring.
- Use rules and systems for calculations and policy checks, and the model for language.

Close with: "The question is not whether it is perfect but whether staff plus the model are faster and at least as accurate as staff alone, which we can measure in a pilot." See [06-ai-llm](../../topics/06-ai-llm/ANSWERS.md) for more detail.

#### Exercise: you are given a small spreadsheet of orders and shipments. What would you analyse, and what could you conclude?
**Approach out loud:**
1. **Understand the data:** columns, one row per what, date range, keys linking orders to shipments.
2. **Check quality:** missing dates, duplicate orders, shipments without orders, negative or zero quantities, odd outliers.
3. **Core metrics:** on-time delivery rate (shipped or delivered date versus promised date), average and 90th percentile lead time, fill rate (shipped quantity versus ordered), partial shipments.
4. **Segment:** by customer, product, warehouse, carrier, month. Problems are usually concentrated.
5. **Look for drivers:** do late orders correlate with a warehouse, carrier, large orders, or end-of-month peaks?

In SQL terms, a typical first query (one order can have several shipments, so take the last delivery per order first; orders with no delivery count as not on time):
```sql
WITH last_delivery AS (
  SELECT order_id, MAX(delivered_date) AS delivered_date
  FROM shipments
  GROUP BY order_id
)
SELECT o.warehouse,
       COUNT(*) AS orders,
       AVG(CASE WHEN d.delivered_date <= o.promised_date THEN 1.0 ELSE 0 END) AS on_time_rate
FROM orders o
LEFT JOIN last_delivery d ON d.order_id = o.order_id
GROUP BY o.warehouse
ORDER BY on_time_rate;
```
**Conclusions:** state findings with appropriate caution ("Warehouse C has a 62% on-time rate versus 90% elsewhere, concentrated in large orders; this suggests a capacity issue, but I would confirm with the site"), and say what decision this supports and what further data you need.

### Writing and presenting

#### Exercise: write a one-page memo recommending whether a customer should expand a deployment.
**Structure:**

1. **Recommendation first (2–3 sentences):** "Recommend expanding the scheduling deployment from 2 to all 9 plants in Q1, at an estimated cost of $X, for an expected $Y in annual savings. Decision needed by [date]."
2. **Evidence:** results from the current deployment against the original targets (3–4 numbers with baselines), adoption figures, and one user quote.
3. **Why expand now:** what the remaining plants have in common with the pilot plants; what is different and how that is handled.
4. **Costs and resources:** money, customer staff time, IT work, timeline.
5. **Risks and mitigations:** for example, two plants run an older ERP version (integration risk; mitigation: start them last).
6. **Alternatives considered:** do nothing; expand to three plants first. Why the recommendation beats them.
7. **Next steps** with owners and dates.

**Style:** short paragraphs, specific numbers, plain words, no jargon, a clear "ask". In the interview, state the recommendation even if the data is mixed, and be explicit about what would change your mind. Interviewers look for synthesis and judgment, not length.

#### How do you write an executive update that gets read and acted on?
- **Bottom line first:** status in one line (on track / at risk / off track) and the single most important thing.
- **Decisions or help needed** next, explicitly: "Need: approval of X by Friday." Executives act on asks, not narratives.
- **Progress against the agreed metrics,** in numbers, compared with last update and target.
- **Risks** with owner and mitigation; bad news included, early.
- **Next milestones** with dates.
- **Keep it short:** readable in two minutes on a phone; details in links or an appendix.
- **Regular cadence:** same day, same format, so they know where to look.
- **Write for forwarding:** assume it will be sent to their boss, so it must stand alone and avoid internal jargon.

Test: if they read only the subject line and first two lines, do they know the status and what you need from them?

#### Exercise: present a recommendation in five minutes to a panel playing the customer's leadership team.
**Structure for five minutes:**
1. **Recommendation (30 sec):** what you recommend and the decision you need.
2. **Why it matters (60 sec):** the problem in their terms, with one or two numbers.
3. **Evidence (90 sec):** what you found, the key analysis, what the pilot showed.
4. **Plan, cost and risks (60 sec):** phases, resources, top two risks and mitigations.
5. **Ask and next steps (30 sec):** repeat the decision needed, owners, dates.

**Delivery:** one idea per slide or none at all; numbers rounded; speak to their priorities (cost, service, risk). Leave time for questions; that is where they assess you.

**Handling questions:** answer directly first, then give the reason; if challenged, acknowledge the point and explain what would change your view; if you do not know, say so and how you would find out. Do not get defensive and do not abandon your recommendation at the first push unless the new information genuinely changes it.

### Account growth and outcome ownership

#### How do you turn a successful first use case into expansion across the customer's organisation?
- **Prove and publicise the first win:** measured results, a user champion who tells the story, credit to the customer team, presented to the sponsor's peers.
- **Find adjacent problems** that reuse the same data and integrations: the next use case should be cheaper and faster, which strengthens the case.
- **Map new sponsors:** other business units with similar problems; ask the current sponsor for introductions.
- **Run short discovery sessions** with those teams, using the first use case as a concrete demo.
- **Build a multi-use-case roadmap** with the customer showing the cumulative value of a shared platform rather than isolated projects.
- **Coordinate with the account executive** on the commercial side; my job is to make sure the value is real and visible.

Expansion should follow value. Pushing new use cases before the first one is adopted spreads the team thin and risks the whole account.

#### How do you measure whether a deployment is succeeding, beyond "it is live"?
Measure on three levels, agreed with the customer before go-live:

1. **Adoption:** active users against target users, frequency of use, which features are used, whether old workarounds (spreadsheets, emails) have stopped.
2. **Operational outcome:** the metric the deployment was meant to move, against a baseline: time to decision, throughput, cost, errors, backlog.
3. **Business or mission value:** money saved or earned, risk reduced, service improved, expressed the way the sponsor reports to their leadership.

Also watch **health indicators**: data freshness, incidents, user satisfaction, and whether the customer's team is starting to build or request new things themselves (a strong signal of ownership).

Report these regularly. If adoption is high but outcomes do not move, the tool may be solving the wrong problem; if outcomes improve where it is used but adoption is low, the problem is change management.

#### The deployment is behind schedule because the customer's own team is not delivering. How do you handle it?
1. **Get specific:** which deliverables are late (data access, test users, IT changes), by how much, and the impact on milestones.
2. **Understand why:** competing priorities, unclear ownership, missing skills, or disagreement with the plan. Talk to the people involved directly.
3. **Make it easy for them:** do as much of the preparation as possible; offer our engineers to pair with theirs; reduce the ask.
4. **Make dependencies visible in a neutral way:** a shared plan with owners and dates on both sides, reviewed in the regular status meeting, so it is not "blame".
5. **Escalate to the sponsor with facts and options** if it continues: "We are three weeks behind because of X. Options: assign another person, reduce scope, or move the date."
6. **Protect the relationship:** focus on the shared goal, not fault.

Meanwhile, keep our team productive on work not blocked by the dependency.

#### Tell me about a time you owned an outcome you did not fully control.
**What they are looking for:** ownership mindset, influence through others, proactive handling of dependencies, and honesty about results.

**STAR outline:**
- **Situation:** [goal and the parts outside your control: other teams, a client, a vendor].
- **Task:** [the outcome you were accountable for].
- **Action:** how you mapped dependencies, aligned people, tracked progress, removed blockers, escalated with options, and kept stakeholders informed.
- **Result:** [measurable outcome, including partial success if so].
- **Learning:** [what you would do differently].

**Example (adapt to your own experience):** "I was responsible for launching a new supplier onboarding process across three regions, but each regional procurement team reported to a different director. I agreed a shared timeline with each director, set up a weekly 20-minute check-in, and created a tracker showing each region's status. When one region fell behind because of a staffing gap, I trained two of their analysts myself and moved their go-live by two weeks with the sponsor's agreement. All three regions went live within a month of plan, and onboarding time fell from 21 days to 9. I learned to agree escalation rules up front."

#### When would you recommend stopping a use case or walking away from a deployment?
When continuing costs the customer (and your team) more than it is likely to return, and you have already tried the reasonable fixes.

Signals:
- **No owner:** nobody at the customer is accountable for the outcome or will make users change how they work.
- **No path to value:** the data does not exist or cannot be accessed within a reasonable time, or the pilot metric has not moved after a fair test.
- **Mis-scoped problem:** the real issue is organisational or policy-related, and software will not change it.
- **Unacceptable risk:** ethical, legal or safety concerns that controls cannot address.

How to do it: bring evidence and options to your own team first, then to the sponsor ("narrow to the part that worked, pause until X is in place, or stop"); write down what was learned; and redirect the effort to a better use case on the account where possible. Stopping a weak use case early protects trust and frees the team for one that works, which is usually better for the account than letting a failing project drag on.

### Company and mission

#### Palantir works with defence, intelligence and government customers. How do you think about the ethics of that work?
Interviewers want a considered, honest view, not enthusiasm or avoidance. A reasonable structure:

- **Your position, stated clearly:** whether and why you are comfortable supporting government, defence and law-enforcement customers, and which uses you would find difficult.
- **Recognise the trade-offs:** these institutions do important work and decisions are better with good information; the same tools can also affect civil liberties and privacy, and the company's work has attracted public criticism.
- **How you would act in the role:** respect legal and policy constraints, insist on access controls and audit trails, raise concerns through internal channels if a use seems wrong, and be willing to escalate or step off an account.
- **Show you have done your homework:** read the company's own public statements about which customers it works with, and its approach to privacy and civil liberties, and form your own view.

Do not pretend to have no reservations if you do; a thoughtful candidate who can articulate where their lines are is more credible. If you cannot support any of this work, this is not the right company.

#### What do you know about the company's platform, and how would you position it for a new customer?
**Know the basics, in your own words** (for Palantir: Foundry for integrating and modelling an organisation's data and building operational applications on an ontology; AIP for using LLMs and agents on top of that data with security controls; Gotham for government and defence analysis; Apollo for deploying and updating software across environments). For an AI company, know its models, API, enterprise product and agent tooling. Check the company's current site, since products are renamed and expanded often.

**Positioning for a new customer:**
1. **Start from their problem,** not the product: "You want to cut unplanned downtime and your data sits in five systems."
2. **Explain the value in outcomes:** time to first use case (weeks), ability to act and not only see dashboards, reuse of the same foundation for the next use cases, and security and governance for sensitive data.
3. **Be honest about fit:** what it does not do, what the customer must provide (data access, a business owner, users' time), and what it costs in effort.
4. **Offer proof:** a short pilot with a defined metric, and relevant case studies the company has published.

Avoid reciting features; show you understand why a customer would buy it.

## Prep checklist

- [ ] Practise six to eight case or decomposition prompts out loud, timed (30–40 minutes each), from [04-decomposition](../../topics/04-decomposition/README.md) and the cases above; record yourself and check you clarify before solving.
- [ ] Prepare two business cases with explicit assumptions and a sensitivity check, and two estimation questions, until the arithmetic is quick and clean.
- [ ] Write one one-page recommendation memo and one executive update, then cut each by a third.
- [ ] Prepare stories for influence without authority, conflicting stakeholders, owning an outcome you did not control, delivering bad news, and learning a new domain fast ([story bank](../../topics/00-overview/ANSWERS.md)).
- [ ] Do a discovery role-play with a friend playing a skeptical executive; practise listening and closing on a concrete next step.
- [ ] Build basic data fluency: write simple SQL joins and group-bys, and practise spotting data quality problems in a sample dataset.
- [ ] Learn the company's platform at a high level from its own website, documentation and published case studies; prepare a two-minute plain-language explanation.
- [ ] Form a clear, honest view on the company's mission and customers, including any ethical concerns, and be ready to discuss it.
- [ ] Prepare questions on how strategists and engineers split work, accounts per person, travel, and how success is measured ([14-questions-to-ask](../../topics/14-questions-to-ask/README.md)).
