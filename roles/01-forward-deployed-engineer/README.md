# Forward Deployed Engineer (FDE / FDSE)

A software engineer embedded with customers who builds and ships production solutions on the company's platform, owns the technical side of the account, and feeds field learnings back into the product.

## The role

**Day to day.** An FDE spends the week moving between three modes:

- **Building:** writing integrations, data pipelines, LLM/agent workflows, and small apps on top of the company's platform, often in the customer's environment.
- **Customer-facing:** discovery calls, on-site working sessions, demos, status updates, and unblocking access, security reviews and data issues.
- **Product feedback:** writing up gaps, building reusable components, and pushing patterns seen across customers into the core product.

Travel varies by company and account: some FDEs spend multiple days a week on-site; others are mostly remote with periodic visits. Ask the recruiter for the real number.

**Common titles / aliases:** Forward Deployed Engineer (FDE), Forward Deployed Software Engineer (FDSE, Palantir), Forward Deployed AI Engineer, Applied AI Engineer, AI Deployment Engineer, Field Engineer, Customer Engineer (some companies), Founding/Deployment Engineer at startups.

**Companies that hire for it (examples):** Palantir (FDSE, the role's origin), OpenAI, Anthropic, Scale AI, Databricks, Salesforce (Agentforce), Ramp, Sierra, Decagon, Harvey, Glean, Cohere, Anduril, and many AI-native startups. See [11-company-specific](../../topics/11-company-specific/README.md).

**How it differs from nearby roles:**

| Role | Difference from FDE |
|---|---|
| Core SWE | Builds one product for many users; rarely owns a specific customer outcome. FDE builds for a specific customer, then generalises. |
| Solutions / Sales Engineer | Mostly pre-sale demos and technical validation. FDE writes production code and stays after the contract is signed. |
| Deployment Strategist | Business/problem owner on the same account; usually does not ship production code. See [02-deployment-strategist.md](../02-deployment-strategist/README.md). |
| Consultant | Bills hours on client-owned systems. FDE builds on and improves their own company's platform. |

**Who it suits:** engineers who like seeing users use what they built, are comfortable with ambiguity and messy data, enjoy learning new domains quickly, can write production code without a large team around them, and can hold a room with a customer executive.

**Skills mix:**

| Skill | Weight |
|---|---|
| Coding / production engineering | High |
| Data engineering / integration | High |
| Customer communication & scoping | High |
| Product sense (what to generalise) | Med–High |
| Sales / commercial | Low–Med |
| Deep research / algorithms | Low–Med |

**Career paths:** senior/lead FDE → FDE manager or head of a vertical; product manager (you already know customer pain); core engineering (often on the platform features you used in the field); solutions architecture leadership; founder (a common Palantir alumni path).

## Typical interview loop

Loops vary by company and change often; the rounds below are **typical / candidate-reported**, not guaranteed. Confirm with the recruiter.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | "Why FDE not SWE?", travel, customer fit | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) |
| Coding (DS&A or practical/incremental) | Clean, working code under changing requirements | [02-coding](../../topics/02-coding/README.md) |
| Take-home on the company's API (common at AI labs) | Building something real; then defending it | [06-ai-llm](../../topics/06-ai-llm/README.md), this file (exercises) |
| Decomposition (Palantir, Databricks, others) | Breaking a vague problem into a plan | [04-decomposition](../../topics/04-decomposition/README.md) |
| Learning round (Palantir) | Learning something new fast | [05-learning-round](../../topics/05-learning-round/README.md) |
| Applied system design | Architecture in a customer's real constraints | [03-system-design](../../topics/03-system-design/README.md) |
| Debugging / re-engineering | Unfamiliar code, root-causing | [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) |
| Customer role-play / demo | Scoping, pushback, presenting | [09-customer-scenarios](../../topics/09-customer-scenarios/README.md), this file |
| Hiring manager / values | Ownership, ambiguity, motivation | [10-behavioral](../../topics/10-behavioral/README.md) |

## Common rounds — use the shared sections

- [01 Recruiter & motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md): nail the 60-second "Why FDE?" and the travel answer first; this screen rejects many strong engineers.
- [02 Coding](../../topics/02-coding/ANSWERS.md): prioritise practical/incremental and API-style problems over hard DS&A, unless you are interviewing at Palantir or Scale.
- [03 System design](../../topics/03-system-design/ANSWERS.md): focus on section A (applied, customer-context prompts): legacy systems, on-prem, regulated data.
- [04 Decomposition](../../topics/04-decomposition/ANSWERS.md): essential for Palantir FDSE; useful everywhere for the "vague customer ask" round.
- [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md): essential for AI-lab FDE roles; evals and production reliability matter most.
- [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md) and [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md): the daily work; expect follow-ups on auth, networking and messy data.
- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): the generic role-plays (angry exec, scope creep, bad data). This file goes deeper on FDE-only situations.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md) and [00 story bank](../../topics/00-overview/ANSWERS.md): prepare stories for ownership, ambiguity and customer impact.

## Role-specific questions

### Travel, on-site work and field conditions
- [ ] What does a good first week on-site at a new customer look like for you?
- [ ] How do you stay effective when you are travelling 50% or more of the time?
- [ ] How do you build trust with a customer team you are embedded in when you are not their employee?
- [ ] You are the only person from your company on-site and a customer engineer offers you their admin credentials "to save time". What do you do?
- [ ] How do you work in a restricted environment: locked-down laptop, no internet, air-gapped or classified network?

### Prototype-to-production ownership
- [ ] Walk me through taking a demo you hacked together in a week to a production system the customer relies on.
- [ ] What concretely changes between your prototype code and your production code?
- [ ] Who owns the code you wrote for a customer after you rotate off the account, and how do you prepare for that?
- [ ] How do you monitor and support a deployment running in a customer environment you cannot access around the clock?
- [ ] Tell me about a time you shipped something to real users under a hard deadline and had to decide what to cut.

### Field-to-product feedback
- [ ] How do you write a product feedback ticket from the field so the core team actually acts on it?
- [ ] You built a workaround in the field that three customers now depend on. How do you get it into the product?
- [ ] The core product team disagrees with your feedback. How do you make the case?
- [ ] Tell me about a time you influenced a product or roadmap decision you did not own.
- [ ] What signals tell you a request is a product gap rather than a one-customer need?

### Technical ownership of a customer account
- [ ] What does it mean to be the technical owner of a customer account?
- [ ] How do you split responsibilities with the account executive, customer success manager and deployment strategist on the same account?
- [ ] A customer VP asks you privately whether they should renew or expand. How do you answer?
- [ ] What goes into a technical account plan, and how do you keep it current?
- [ ] You spot a serious risk on your account (a security gap, falling adoption) that the customer has not noticed. What do you do?

### Building on the company's platform
- [ ] When do you build with the platform's own primitives and when do you write custom code beside it?
- [ ] Palantir: how would you model a customer's supply chain as a Foundry ontology, and what would you build on top of it with AIP?
- [ ] OpenAI / Anthropic: a customer wants an LLM agent inside a regulated workflow. Walk through your build on the API from first prompt to production.
- [ ] You hit a platform limitation in the middle of a deployment. What do you do?
- [ ] How do you learn an internal platform fast enough to be useful to a customer in your first month?
- [ ] How do you keep a customer deployment working through platform releases and model version changes?

### Multi-customer context switching
- [ ] You cover three customers. Walk me through how you plan your week.
- [ ] Two of your customers have production incidents at the same time. What do you do?
- [ ] How do you make sure nothing from one customer (data, code, credentials, context) leaks into another's deployment?
- [ ] How do you reuse work across customers without breaking contracts or confidentiality?

### Hack versus productize
- [ ] Give a concrete rule you use to decide whether to hack something now or build it properly.
- [ ] How do you pay down the technical debt you created in the field?
- [ ] When, if ever, is it right to fork the product for one customer?

### Role-specific exercises
- [ ] Exercise: live demo. Present a working prototype to a mock customer executive who interrupts with objections.
- [ ] Exercise: customer architecture whiteboard. Sketch how our platform fits into this customer's existing stack.
- [ ] Exercise: live triage. A customer integration broke overnight; diagnose it with incomplete information while keeping the customer informed.
- [ ] Exercise: take-home build on our API, then defend your design choices to an engineer playing the customer's CTO.
- [ ] Exercise: write a one-page summary of a six-week pilot for the customer's executive sponsor.

## Model answers

### Travel, on-site work and field conditions

#### What does a good first week on-site at a new customer look like for you?
The goal of week one is access, context and one small visible win, not code volume.

- **Day 1:** meet the executive sponsor and the day-to-day champion. Confirm the outcome they are paying for and how it will be measured. Start access requests (accounts, VPN, data, laptops) immediately; they are the longest lead-time item.
- **Days 1–2:** shadow the actual end users. Watch them do the job the deployment is meant to improve. Note the spreadsheets, workarounds and manual steps.
- **Days 2–3:** inventory systems and data: sources, owners, formats, refresh rates, quality. Identify the IT and security contacts.
- **Days 3–4:** build a thin end-to-end slice on real (or realistic) data, even if ugly: one source in, one useful output.
- **Day 5:** show it to the champion and users, collect feedback, and send a written recap: what we learned, what we built, risks, asks (access, data, people), plan for next two weeks.

The written recap and the demo on day 5 build more trust than any slide deck.

#### How do you stay effective when you are travelling 50% or more of the time?
Treat travel as a scheduling and energy problem, not something to push through.

- **Protect deep work.** On-site days are for things that need the customer: workshops, user shadowing, debugging with their engineers, demos. Remote days and flights are for focused building and writing.
- **Make the environment portable.** Reproducible dev setup (containers, dotfiles, scripts), offline docs, a checklist for the customer's VPN and access, a travel kit (adapters, hotspot).
- **Keep a single plan.** One running task list per customer and one weekly plan, so switching cities does not mean losing the thread.
- **Write things down on-site.** Notes and decisions go into a shared doc the same day; memory fades over a flight.
- **Be honest about sustainability.** Sleep, exercise and blocking travel-free weeks matter; burned-out FDEs make bad calls in front of customers.

If asked, give a real example of how you have handled travel or a heavy schedule, and say what travel level you are comfortable with.

#### How do you build trust with a customer team you are embedded in when you are not their employee?
Trust comes from being useful, reliable and on their side about the outcome.

- **Ship something early** that makes a real user's day easier. Credibility comes from results.
- **Do what you said, when you said.** Small commitments kept (sending notes, fixing a bug by Friday) compound.
- **Learn their work.** Sit with operators, use their vocabulary, understand their incentives and pressures.
- **Make their people look good.** Credit the champion and their engineers in front of leadership; teach rather than hoard.
- **Be candid.** Say "I don't know, I'll find out", flag risks early, and do not oversell the platform. Honesty about limits is what makes your positive claims believable.
- **Respect their rules.** Follow their security, change-control and working norms even when slower.

The failure mode to avoid is acting like a vendor who parachutes in, criticises their systems and leaves.

#### You are the only person from your company on-site and a customer engineer offers you their admin credentials "to save time". What do you do?
Decline politely and get proper access, even if it costs a day.

- **Why:** shared credentials break the customer's audit trail, may violate their security policy and your contract, and make you personally responsible for anything done under that account. If something goes wrong, nobody can tell who did what.
- **What I say:** "Thanks, I appreciate it, but I need to use my own account so everything I do is traceable to me. Can we raise a request together for the access I actually need?"
- **Then:** work out the minimum access needed (least privilege: which systems, read or write, how long), file the request with their IT/security owner, and escalate through the sponsor if it blocks the timeline.
- **Meanwhile:** keep moving with what I can do: sample or synthetic data, building the pipeline against a schema, pairing with their engineer who drives on their own account.

If it is a pattern, mention it to my lead and the account team so the access process is fixed for future deployments.

#### How do you work in a restricted environment: locked-down laptop, no internet, air-gapped or classified network?
Plan for it before arriving; you cannot download your way out on-site.

- **Ask early:** OS and tooling allowed, package mirrors (internal PyPI/npm/Artifactory), container registry, how software is approved and transferred, whether a cleared person must accompany you.
- **Bring dependencies in an approved way:** vendored or pinned dependencies, offline wheels, container images scanned and transferred through their process, offline documentation.
- **Design for it:** no calls to external APIs or SaaS; self-hosted models if AI is needed; configuration via files, not cloud consoles; logs and metrics kept inside the boundary.
- **Adapt the workflow:** develop and test on an unclassified mirror with synthetic data, then promote builds through the customer's transfer process. Keep releases small because each transfer is costly.
- **Follow the rules strictly:** no personal devices, no copying data out, no discussing details outside cleared spaces.

Mention any experience with on-prem, regulated or offline systems. See [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/ANSWERS.md) for on-prem and air-gapped deployment detail.

### Prototype-to-production ownership

#### Walk me through taking a demo you hacked together in a week to a production system the customer relies on.
I treat it as a staged promotion with explicit gates, agreed with the customer.

1. **Confirm the demo solved the right problem.** Get users to use it on real data for a week or two; measure against the success metric agreed up front.
2. **Decide what to keep.** Usually the data model and core logic survive; the glue, hard-coded config and manual steps do not.
3. **Harden:** tests on core logic and data contracts, input validation, error handling and retries, configuration and secrets out of the code, idempotent pipelines, permissions mapped to the customer's identity system.
4. **Operate:** logging, metrics, alerts on freshness and failures, a runbook, a defined support path and SLA.
5. **Pass their gates:** security review, data protection review, change-control and UAT. Start these early; they are usually the critical path.
6. **Roll out gradually:** one team or site first, with a fallback to the old process, then expand.
7. **Hand over or own:** decide who runs it (customer team, our support, or me for a period) and document it.

Throughout, keep the sponsor updated on what "production" means and why it takes longer than the demo.

#### What concretely changes between your prototype code and your production code?
A checklist I would actually apply:

- **Correctness:** unit tests on business logic; tests on data contracts (schema, nulls, ranges); a regression set for LLM features (evals).
- **Failure handling:** timeouts, retries with backoff, idempotency, clear behaviour on bad input, dead-letter or quarantine for bad records.
- **Config and secrets:** no hard-coded paths, keys or customer IDs; secrets in a vault or the platform's secret store.
- **Security:** least-privilege service accounts, permissions that respect the customer's access controls, audit logging, PII handling.
- **Observability:** structured logs, metrics (latency, error rate, row counts, cost), alerts, dashboards.
- **Performance and cost:** tested on production-sized data; known limits; cost per run or per request.
- **Deployability:** CI, versioned releases, a rollback path, infrastructure as code where the customer allows it.
- **Maintainability:** readable code, a README, a runbook, and an owner.

I do not do all of this for every prototype; the point of a prototype is speed. I do all of it before anything becomes a dependency for the customer's operations.

#### Who owns the code you wrote for a customer after you rotate off the account, and how do you prepare for that?
Ownership must be decided explicitly and early, because "the FDE who left" is the most common reason field deployments rot.

Typical options, agreed with the customer and my team:
- **The customer's team owns it.** Then I build it in their repo and conventions, pair with their engineers from early on, and have them make changes before I leave.
- **Our company owns it** (support team or a successor FDE). Then it must follow our standards, live in our repos, and have a proper handover.
- **It becomes product.** The best outcome: the core team absorbs it and it is supported like any other feature.

Preparation regardless of owner: README with architecture and data flow, runbook for common failures, list of credentials and integrations (not the secrets themselves), known issues and open decisions, recorded walkthrough, and an overlap period where the new owner runs things while I am still reachable. The test: can the new owner deploy a change and handle an alert without asking me?

#### How do you monitor and support a deployment running in a customer environment you cannot access around the clock?
Design the deployment so the customer's own operations can detect and handle common problems, and agree an escalation path for the rest.

- **Monitoring inside their boundary:** health checks, data freshness and volume checks, error rates and job failures, wired into their existing tools (their Datadog, Splunk, CloudWatch or email/on-call) rather than something only I watch.
- **Telemetry out, if allowed:** some customers permit sending anonymised health metrics back to us; many regulated ones do not. Agree this in writing.
- **Runbook for their team:** symptoms, likely causes, safe actions (restart, rerun, rollback), and when to call us.
- **Escalation path and SLA:** who to contact, through what channel, expected response time, consistent with the contract.
- **Self-healing where cheap:** retries, idempotent reruns, automatic backfill of missed windows.
- **Review regularly:** a short weekly or monthly check of alerts and incidents with the customer to find recurring issues.

The aim is that a 2 a.m. failure is either fixed automatically or handled by their on-call using my runbook, not waiting for me to land.

#### Tell me about a time you shipped something to real users under a hard deadline and had to decide what to cut.
**What they are looking for:** prioritisation by user value, explicit trade-offs agreed with stakeholders, and quality where it matters.

**STAR outline:**
- **Situation:** [project, users, the fixed deadline and why it was fixed].
- **Task:** [what you owned; what "shipped" meant].
- **Action:** how you listed scope, ranked it by user value and risk, what you cut or deferred, who you agreed this with, and what you refused to cut (for example correctness or security).
- **Result:** [shipped on date; usage or impact numbers; what happened to the deferred items].
- **Learning:** [what you would scope differently].

**Example (adapt to your own experience):** "A logistics client needed a shipment-exception dashboard before peak season, four weeks out. I listed twelve requested features and ranked them with the operations lead by how often each would be used in peak. We shipped the four core views, a daily CSV export instead of a live ERP write-back, and manual user provisioning. I did not cut data validation, because wrong numbers would have killed trust. It launched on time to 40 planners; the write-back shipped six weeks later. I learned to agree the cut list in writing on day one."

### Field-to-product feedback

#### How do you write a product feedback ticket from the field so the core team actually acts on it?
Product teams act on feedback that is specific, evidenced and tied to value. A good field ticket has:

- **Problem, not solution:** "Users at [customer] cannot join streaming sensor data to maintenance records because X", not "add feature Y".
- **Who and how many:** which customers and users are affected, how often, and whether other FDEs have seen it.
- **Impact:** revenue at stake (renewal, expansion, deal blocked), hours lost, workaround cost.
- **Evidence:** reproduction steps, screenshots, logs, a minimal example, a link to the workaround code.
- **Current workaround** and why it does not scale.
- **Suggested approach** (optional, clearly marked as a suggestion).
- **Urgency and deadline**, with the real reason.

Then follow through: tag the right product owner, raise it in the field-to-product sync, aggregate duplicates from other FDEs into one ticket, and update it when new customers hit it. One well-written ticket with three customers attached beats ten vague complaints.

#### You built a workaround in the field that three customers now depend on. How do you get it into the product?
Three customers on the same workaround is the signal that it is a product gap.

1. **Generalise the evidence:** document the three use cases, what is common and what differs, and the cost of maintaining three copies.
2. **Make the business case:** usage, revenue tied to those accounts, other prospects likely to need it, and the support risk of field-maintained code.
3. **Bring a design, not just a request:** propose the generic version (API, configuration points, what stays customer-specific), ideally reviewed with a core engineer.
4. **Offer to do the work:** many FDE teams can contribute code to the product with core-team review. Follow their standards, tests and release process.
5. **Plan migration:** move each customer from the workaround to the product feature, with tests that prove identical behaviour, then delete the workaround.

Meanwhile, stabilise the workaround as a shared, versioned module rather than three diverging copies, so the risk does not grow while the product decision is made.

#### The core product team disagrees with your feedback. How do you make the case?
First, assume they may be right: they see all customers and the roadmap; I see a few accounts deeply.

- **Understand their reasoning:** ask what they would need to see to change priority. Often the objection is about cost, design or strategy, not whether the problem exists.
- **Strengthen the evidence:** more customers affected, quantified impact (revenue at risk, hours lost), user quotes, data from usage. Get other FDEs and account owners to add their cases.
- **Reduce their cost:** propose a smaller version, offer to build it with their review, or find an extension point that solves it without a big feature.
- **Escalate cleanly if the stakes justify it:** a joint write-up to the product lead with both views, not a back-channel complaint.
- **Accept the decision and close the loop with the customer:** explain honestly what will and will not be built, and maintain a supported workaround if needed.

Interviewers want to see that you push with data and still disagree-and-commit.

#### Tell me about a time you influenced a product or roadmap decision you did not own.
**What they are looking for:** influence without authority, evidence over opinion, understanding the other team's constraints, and a concrete result.

**STAR outline:**
- **Situation:** [product or team you did not own; the gap you saw from users].
- **Task:** [why it mattered to you or your users].
- **Action:** how you gathered evidence (users affected, data, cost), who you talked to, how you framed it in their terms, what you offered to do yourself.
- **Result:** [decision made, shipped feature, measurable impact].
- **Learning:** [what you would do differently in influencing].

**Example (adapt to your own experience):** "Our analytics customers kept exporting data to Excel because our product had no pivot view. I collected export logs showing 60% of active users exported weekly, wrote a one-page proposal with five customer quotes, and built a rough prototype over a weekend. The product manager had deprioritised it for a different feature; I showed that the prototype cost two weeks to finish and offered to pair on it. It shipped the next quarter and exports dropped by half. I learned to bring a prototype and a cost estimate, not just a request."

#### What signals tell you a request is a product gap rather than a one-customer need?
Signals that it is a product gap:
- **Repetition:** more than one customer (or prospect) asks independently, or other FDEs have built the same workaround.
- **Generic problem:** it is about a common workflow, data type or integration (SSO, a popular ERP, a standard file format), not a customer's unique process.
- **Workaround cost:** customers or FDEs are spending real time maintaining something fragile.
- **Deal impact:** it blocks sales or renewals.
- **Fits the product's direction:** it aligns with the roadmap and target market.

Signals it is one-customer:
- It encodes a specific internal policy, legacy system or org structure.
- Nobody else has asked, and the target market does not look like this customer.
- It conflicts with how most customers use the product.

The useful middle ground: one-customer logic built on a generic **extension point** (configuration, plugin, API). If many customers need extension points in the same place, that is a product gap too.

### Technical ownership of a customer account

#### What does it mean to be the technical owner of a customer account?
It means I am accountable for the customer's technical outcome, not just the code I personally write.

- **Know the landscape:** their systems, data, integrations, security constraints and the people who own each.
- **Own the technical plan:** what we are building, in what order, why, and the risks.
- **Own the deployment's health:** reliability, performance, incidents and their follow-up.
- **Be the technical voice:** to the customer (honest about what the platform can and cannot do) and to my company (field feedback, risks to renewal).
- **Make technical decisions** and document them, while involving the customer's engineers so they understand and can challenge them.
- **Coordinate:** pull in specialists, core engineering or support when needed, rather than doing everything alone.

It does not mean owning the commercial relationship; that sits with the account executive or deployment strategist. But I make sure they never learn about a technical problem from the customer before they hear it from me.

#### How do you split responsibilities with the account executive, customer success manager and deployment strategist on the same account?
Agree the split explicitly at the start, because overlaps cause dropped balls and mixed messages to the customer.

| Role | Typically owns |
|---|---|
| Account executive | Contract, pricing, renewals, expansion deals, executive commercial relationship |
| Customer success manager | Adoption, training, health scores, renewal readiness (at companies that have CSMs) |
| Deployment strategist / engagement lead | Problem framing, business outcomes, stakeholder map, roadmap with the customer |
| FDE | Technical design and delivery, data and integrations, reliability, technical risk |

Working norms that matter: one shared account plan; a weekly internal sync; one voice to the customer on commercial topics; no promises about pricing or roadmap from me; no technical commitments from sales without checking with me. When a customer asks me a commercial question, I answer what I can factually and loop in the owner the same day.

#### A customer VP asks you privately whether they should renew or expand. How do you answer?
Answer honestly about the value and risks from a technical viewpoint, and do not make commercial commitments or pressure them.

- **Use facts:** "Here is what is in production, how it is used, and the impact we have measured against the goals we agreed: [numbers]."
- **Be candid about gaps:** "These two use cases are not delivering yet, and here is why and what it would take."
- **Tie expansion to value, not to selling:** "If you want X, these are the use cases I think are ready, and these are not."
- **Leave pricing and contract terms to the account owner:** "I will ask [AE] to follow up on the commercial side."
- **Tell my account team the same day** what was asked and what I said.

Overselling to win a renewal destroys the trust that makes the next renewal possible. If my honest view is that they are not getting value, the right move is to say so internally first and fix the deployment, not to mislead the customer.

#### What goes into a technical account plan, and how do you keep it current?
A short living document (one or two pages plus links) that anyone covering the account could pick up.

- **Business goals and success metrics**, agreed with the sponsor.
- **Stakeholder map:** sponsor, champion, users, IT/security, blockers, with what each cares about.
- **Current state:** what is in production, usage, health, open incidents.
- **Architecture and data:** systems integrated, data flows, environments, access, security constraints.
- **Roadmap:** next 1–3 use cases, with status and owners.
- **Risks and mitigations:** technical, adoption, people, commercial.
- **Open asks** on both sides (access, data, decisions, product gaps filed).
- **Decision log** with dates.

I update it after every significant meeting or release, and review it in the weekly account sync. If it is not used in that sync, it goes stale; tying it to a recurring meeting is what keeps it current.

#### You spot a serious risk on your account (a security gap, falling adoption) that the customer has not noticed. What do you do?
Raise it promptly, with evidence and a proposed fix, through the right channel.

**Security gap (for example, an over-permissioned integration or exposed data):**
1. If there is active exposure, contain it first within my authority (revoke a token I control, disable a job) and follow my company's incident process.
2. Tell my lead and our security team immediately, then the customer's security contact, with facts: what, since when, what data, what we have done.
3. Agree remediation and document it. Never sit on a security issue to avoid an awkward conversation.

**Falling adoption:**
1. Find out why from data and conversations with users: wrong workflow, performance, trust in the output, training, a reorganisation.
2. Tell my account team, then bring it to the champion with the data and a plan (fix the top usability issue, retrain, re-scope).

In both cases, the customer should hear about risks from us before they find them, and with a plan attached.

### Building on the company's platform

#### When do you build with the platform's own primitives and when do you write custom code beside it?
Default to the platform; write custom code only where the platform clearly cannot do the job.

**Use platform primitives when** they cover the need reasonably well. The customer gets upgrades, security, support and permissions for free; other FDEs can maintain it; and usage shows the product team what customers value. Building around the platform also weakens the case for renewal.

**Write custom code when:**
- the platform has no capability for it (an unusual protocol, a specialised algorithm);
- performance or scale requirements exceed what the primitive supports;
- the customer's constraints require it (a system that must stay in their stack).

**How to do custom code well:** use the platform's official extension points (SDKs, functions, APIs, plugins) rather than bypassing it; keep the custom part small and well-defined; document why it exists; and file a product ticket for the gap so the custom code can eventually be removed.

A useful test: "If I leave tomorrow, can a standard support engineer understand and maintain this?" Platform-native usually passes; clever side systems usually do not.

#### Palantir: how would you model a customer's supply chain as a Foundry ontology, and what would you build on top of it with AIP?
Start from the decisions users make, then model the objects those decisions are about.

**Ontology (object types and links):**
- Objects: Supplier, Part, Purchase Order, Shipment, Plant, Inventory Position, Customer Order.
- Links: Supplier supplies Part; PO contains Parts and is fulfilled by Shipments; Shipment arrives at Plant; Plant holds Inventory Positions; Customer Order consumes Parts.
- Properties: lead times, ETAs, quantities, risk scores, statuses. Time-series for inventory and sensor data where needed.
- **Actions:** expedite shipment, reallocate inventory, change supplier, with permissions and an audit trail, written back to the ERP where allowed.

**Data:** pipelines (Pipeline Builder or Code Repositories) that ingest ERP, supplier portals and logistics feeds, clean and resolve entities (the same supplier under several IDs), and populate the ontology.

**Apps and AIP:** a Workshop app showing at-risk customer orders; AIP Logic functions or an agent that explains why an order is at risk and proposes actions ("reallocate 200 units from Plant B"), with a human approving each Action. Evaluate suggestions against planners' past decisions before relying on them.

Mention you would confirm current product names and capabilities; the platform evolves quickly.

#### OpenAI / Anthropic: a customer wants an LLM agent inside a regulated workflow. Walk through your build on the API from first prompt to production.
Example: an agent that drafts responses to insurance claim queries.

1. **Scope and risk:** which decisions the agent may make versus only suggest; human review required for anything binding; data classification and residency; the customer's compliance requirements (audit, retention, model risk).
2. **Eval set first:** 100–300 real, anonymised cases with expected outcomes and graded criteria (correct policy cited, no invented facts, tone). Agree pass thresholds with the business.
3. **Prototype:** a system prompt with policy, tools defined with JSON schemas (look up policy, fetch claim, draft letter), structured outputs, retrieval over policy documents with citations. With Claude, tools can be exposed via MCP.
4. **Iterate on evals:** prompt changes, retrieval tuning, model choice; track accuracy, refusal, latency and cost per case.
5. **Guardrails:** input and output checks, PII handling, restricted tool permissions, maximum steps, escalation to a human when uncertain.
6. **Production:** retries and rate-limit handling, prompt caching for long shared context, logging of every prompt, tool call and output for audit, version pinning of models and prompts, monitoring and sampled human review.
7. **Rollout:** shadow mode, then assistive with human approval, then wider scope as metrics hold.

Details: [06-ai-llm](../../topics/06-ai-llm/ANSWERS.md).

#### You hit a platform limitation in the middle of a deployment. What do you do?
1. **Confirm it is real:** check docs, internal channels and the core team; often there is an undocumented way or a newer feature.
2. **Size the impact:** does it block the outcome, slow it down, or only affect a nice-to-have? Is there a deadline at risk?
3. **Find options:** a workaround with platform extension points; a different design that avoids the limit; a temporary custom component; asking the core team for a fix or timeline.
4. **Tell people early:** my account team immediately; the customer once I have options, framed as "here is the constraint, here are three ways forward, here is what I recommend".
5. **File the product ticket** with the customer impact (see the feedback answer above).
6. **Build the workaround so it is easy to remove** when the product catches up.

What I avoid: hiding the limitation, or quietly building a large side system that becomes permanent.

#### How do you learn an internal platform fast enough to be useful to a customer in your first month?
Learn by building, aimed at the customer's actual use case.

- **Week 1:** official training and docs end to end, quickly; then build a small realistic project end to end (ingest data, model it, build a simple app or API on top). Breadth first.
- **Read existing deployments:** the best way to learn how the platform is really used is to read other FDEs' code and designs for similar customers.
- **Find the experts:** identify who knows each part of the platform and ask specific questions after trying first.
- **Shadow:** join senior FDEs on customer calls to learn the patterns and the common traps.
- **Build for my customer early:** a prototype on the customer's data (or a realistic mock) by week 2–3, reviewed by a senior FDE.
- **Keep notes:** a personal "how do I" document; share the useful parts back as internal docs.

Mention how you have done this before with a new framework or codebase, with a timeline. See also [05-learning-round](../../topics/05-learning-round/ANSWERS.md).

#### How do you keep a customer deployment working through platform releases and model version changes?
Treat upgrades as a planned, tested activity, not a surprise.

- **Pin versions** where the platform allows: model snapshots rather than aliases that silently move, SDK and library versions, platform release channels.
- **Regression tests:** automated tests on integrations and data pipelines, and an eval suite for LLM features, run against the new version before switching.
- **Watch release notes and deprecation notices;** track model retirement dates in the account plan.
- **Upgrade in a staging environment first,** compare outputs (for LLMs, eval scores, latency, cost and a sample of outputs side by side), then roll out with a rollback path.
- **Communicate:** tell the customer what is changing and when, especially if outputs or UI will differ.
- **Reduce coupling:** keep prompts, configuration and model choice outside the code so a change is a configuration update, not a rewrite.

For LLM features specifically, a newer model is not automatically better on a given task; the eval set decides.

### Multi-customer context switching

#### You cover three customers. Walk me through how you plan your week.
- **Monday, 30 minutes:** review each account plan; list each customer's top priority for the week, deadlines, and anything blocked on me. Rank across customers by impact and urgency (production issues, committed deadlines, then roadmap work).
- **Block time by customer,** not by task: for example customer A on-site Tuesday–Wednesday, B and C split across Thursday–Friday, rather than switching every hour.
- **Fixed touchpoints:** a weekly status update to each customer on a set day, and the internal account sync.
- **Buffer:** leave around 20% of the week unplanned for incidents and urgent requests.
- **End of each block:** write a short note on where I stopped and the next step, so switching back costs minutes, not an hour.
- **Friday:** send updates, update account plans, and flag to my lead if one customer is crowding out the others.

If all three genuinely need more than one person, I raise it as a staffing problem rather than quietly underserving one.

#### Two of your customers have production incidents at the same time. What do you do?
1. **Triage both quickly (minutes):** what is broken, how many users, business impact (for example, a hospital's operations versus a weekly report), data loss or security risk, and whether a workaround exists.
2. **Prioritise by impact and severity,** not by who shouts loudest. Safety, security and data integrity first.
3. **Get help immediately:** page our support or on-call, ask another FDE or my lead to take one incident. Two incidents at once is the reason escalation paths exist.
4. **Communicate to both customers right away:** acknowledge, give the next update time, even for the one I am not personally working on yet.
5. **Contain before fixing:** rollback, disable a failing job, switch to a fallback.
6. **Afterwards:** a written post-incident review for each, and check whether the incidents share a cause (a platform release, a shared dependency).

See [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/ANSWERS.md) for incident triage detail.

#### How do you make sure nothing from one customer (data, code, credentials, context) leaks into another's deployment?
Separation by default, enforced by tooling rather than memory.

- **Data:** never copy customer data to my laptop or shared drives unless contractually allowed; no customer data in demos for other customers; synthetic or public data for reusable examples.
- **Environments and credentials:** separate environments, accounts, repos and secrets per customer; separate browser profiles or machines if the customer requires; no shared service accounts.
- **Code:** customer-specific code in customer-specific repos; reusable code generalised and cleaned of names, schemas and logic that reveal the customer before it goes into shared libraries.
- **AI tools:** check what is allowed: do not paste one customer's data or code into an external AI tool unless approved.
- **Conversation:** do not name customers or share their details with other customers unless they have agreed to be a reference.
- **Check before reuse:** a quick review step before anything moves from a customer repo to a shared one.

Confidentiality failures end contracts and careers; interviewers listen for whether you treat this as non-negotiable.

#### How do you reuse work across customers without breaking contracts or confidentiality?
- **Know the IP terms:** contracts differ on who owns code written during a deployment. Check with the account team or legal before reusing anything built on customer time.
- **Reuse patterns, not artefacts:** the design, approach and lessons are usually fine to reuse; customer data, business rules and identifiable configurations are not.
- **Generalise deliberately:** extract the reusable part into a shared library or product feature, stripped of customer specifics, with tests on synthetic data.
- **Build reusable from the start when you can see the pattern:** for example, a generic SAP connector configured per customer, rather than writing it inside one customer's project.
- **Document templates internally:** reference architectures, onboarding checklists and runbooks that any FDE can use.

The best outcome is that reusable work moves into the product, so it is covered by the standard licence and every customer benefits.

### Hack versus productize

#### Give a concrete rule you use to decide whether to hack something now or build it properly.
My rule: **hack when the main risk is "we are building the wrong thing"; build properly when the main risk is "this will break something people depend on".**

In practice:
- **Hack** for demos, experiments and first versions used by a few friendly users, where learning speed matters and failure is cheap.
- **Build properly** as soon as any of these is true: real users depend on it daily; it writes to a system of record; it handles sensitive data; it will run unattended; or a second customer needs it.
- **Never hack** security, permissions, or data correctness, even in a prototype touching real data.

Two habits make the rule safe: label hacks clearly (in code and in what I tell the customer: "this is a prototype"), and put a date or trigger on the rewrite in the account plan, so the hack does not quietly become production.

#### How do you pay down the technical debt you created in the field?
- **Make it visible:** keep a debt list per deployment with impact (incidents, time cost, risk) rather than hiding it.
- **Prioritise by risk and cost:** fix what causes incidents or blocks new work first; leave ugly-but-stable code alone.
- **Attach it to work the customer values:** refactor the area you are already changing for a new feature; schedule a hardening phase before scaling to more users and explain it to the sponsor as reliability work.
- **Use the product:** often the best payoff is replacing custom code with a platform feature that now exists.
- **Prevent new debt:** templates, shared libraries, and a minimum standard (tests on core logic, config not hard-coded) even for fast work.

Be honest in interviews that FDE work creates debt by design; the signal they want is that you manage it deliberately.

#### When, if ever, is it right to fork the product for one customer?
Rarely, and only with the cost made explicit.

A fork means that customer stops getting upgrades and fixes cheaply, and someone must maintain a separate version indefinitely. It is almost always better to use configuration, extension points, or a customer-specific module built on the standard product.

It can be justified when:
- regulation or classification requires a separately built and certified version (for example, an air-gapped environment with its own release process);
- the customer is strategic enough to fund the maintenance, and the commercial team has agreed;
- it is temporary, with a defined plan to merge back.

If I think a fork is needed, I bring it to my lead and the product team as a decision with costs, rather than drifting into one by accumulating local patches.

### Role-specific exercises

#### Exercise: live demo. Present a working prototype to a mock customer executive who interrupts with objections.
**What they score:** clarity, tying the demo to business value, calm under interruption, and honesty.

**Structure (10–15 minutes):**
1. **Open with their problem and the outcome,** not the technology: "Your planners spend six hours a week reconciling X; this shows how that becomes ten minutes."
2. **Show one end-to-end workflow** as the user would do it, on realistic data. Skip setup screens.
3. **Point to the result** (time saved, risk caught) and what changes for their team.
4. **Close with next steps:** pilot scope, what you need from them, timeline.

**Handling objections:**
- "This won't work with our data": "What is different about your data? Let's take one example and check."
- "Our team won't use another tool": ask what they use today; show how it fits into or writes back to that.
- "What about accuracy / security?": give facts, admit limits, describe controls.
- Demo breaks: stay calm, explain briefly, switch to a backup (recording or screenshots), and follow up.

Prepare a backup recording, short scripted path, and two or three likely objections in advance.

#### Exercise: customer architecture whiteboard. Sketch how our platform fits into this customer's existing stack.
**Approach:**
1. **Ask before drawing:** what systems exist (ERP, CRM, data warehouse, identity provider), where the data lives, cloud or on-prem, network and security constraints, and who the users are.
2. **Draw their current state first,** with their names for things. This shows you listened.
3. **Place the platform:** data sources → ingestion (connectors, batch versus streaming) → platform (data model, logic, AI) → user-facing apps → write-back to systems of record.
4. **Cross-cutting concerns:** identity (SSO, SCIM), permissions mapped from source systems, networking (private link, VPN, on-prem agent), audit logging, monitoring.
5. **Phase it:** v0 read-only from one source; v1 more sources and users; v2 write-back and automation.
6. **Call out risks and decisions** they need to make: data ownership, security review, which team runs it.

Keep the diagram simple and talk through one record's journey end to end. See [03-system-design](../../topics/03-system-design/ANSWERS.md) for applied design prompts.

#### Exercise: live triage. A customer integration broke overnight; diagnose it with incomplete information while keeping the customer informed.
**What they score:** structured diagnosis, communication, and not guessing.

1. **Acknowledge and set expectations** (to the interviewer playing the customer): "I'm on it. I'll update you in 30 minutes even if I don't have the answer yet."
2. **Scope:** what exactly is failing, since when, for whom, any error messages, and whether data is missing or wrong (worse) versus delayed.
3. **What changed?** Deploys on our side, platform release, customer changes (password rotation, firewall rule, schema change, expired certificate, API version), volume spikes.
4. **Follow the data path:** source → connector → transformation → output. Check logs and metrics at each step to find where it stops.
5. **Contain:** pause downstream jobs that might publish bad data; communicate impact.
6. **Fix and verify,** then backfill the missed window and confirm with the customer.
7. **Follow up:** a short written incident summary with cause, fix and prevention (for example an alert on credential expiry).

Think aloud, state hypotheses and how you would test each. See [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/ANSWERS.md).

#### Exercise: take-home build on our API, then defend your design choices to an engineer playing the customer's CTO.
**For the build:**
- Scope tightly; a small, working, well-tested system beats an ambitious broken one.
- Include an eval or test set, error handling (rate limits, retries, timeouts), a README with how to run it, design decisions and what you would do next.
- Track cost and latency for AI builds.

**For the defence:**
- Open with the problem, your approach, and results on your evals in two minutes.
- For every major choice, state the alternatives considered and why you chose this one ("I used hybrid search because the documents have many product codes that embedding search alone missed: recall went from X to Y").
- Know your weaknesses and say them first: what breaks at 100x data, what you did not test.
- When challenged, do not get defensive; treat it as a design discussion: "That's a fair point; here's how I'd change it."
- Translate for the CTO: security, cost, maintainability, how it would run in their environment.

See [06-ai-llm](../../topics/06-ai-llm/ANSWERS.md) for the technical depth interviewers probe.

#### Exercise: write a one-page summary of a six-week pilot for the customer's executive sponsor.
Executives read the top third. Structure:

1. **Headline (2–3 sentences):** outcome against the agreed goal. "The pilot reduced average claim triage time from 3 days to 4 hours for 25 adjusters, against a target of 1 day."
2. **Results:** 3–5 metrics with baseline, target and actual; one short user quote.
3. **What worked and what did not,** honestly, including misses and why.
4. **Risks and open issues:** data quality, adoption, security approvals, dependencies on their teams.
5. **Recommendation and decision needed:** for example, "expand to all 300 adjusters in Q1; decision needed by [date] on [budget / IT resource]".
6. **Next steps** with owners and dates.

Rules: plain language, no jargon, numbers over adjectives, one page. Put technical detail in an appendix for their engineers. Sending this clearly and on time is itself evidence of the ownership the role needs.

## Prep checklist

- [ ] Write and rehearse a 60-second "Why FDE?" answer and an honest answer on how much travel you can do.
- [ ] Build one small end-to-end project on the target company's platform or API (a Foundry learning environment if available, or an OpenAI/Anthropic API app with tools and an eval set), and be ready to demo it in 10 minutes.
- [ ] Prepare a demo script with a backup recording and three likely objections, and practise it with a friend interrupting you.
- [ ] Prepare stories for: shipping under a deadline, influencing a product decision, owning a customer or user outcome, handling an incident, and learning a new system fast. Use the [story bank](../../topics/00-overview/ANSWERS.md).
- [ ] Practise one customer architecture whiteboard and one live-triage exercise out loud, timed.
- [ ] Write a sample one-page pilot summary and a sample product feedback ticket; reuse them as talking points.
- [ ] Read the target company's FDE job post, engineering blog and customer case studies; note two or three deployments you could discuss.
- [ ] Review [09-customer-scenarios](../../topics/09-customer-scenarios/ANSWERS.md) and [11-company-specific](../../topics/11-company-specific/ANSWERS.md) for the target company.
- [ ] Prepare questions about travel, accounts per FDE, how field feedback reaches product, and on-call expectations ([14-questions-to-ask](../../topics/14-questions-to-ask/README.md)).
