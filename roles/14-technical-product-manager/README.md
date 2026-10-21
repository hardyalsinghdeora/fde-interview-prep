# Technical Product Manager (Platform PM, Forward Deployed PM, AI Product Manager)

A customer-facing product role that decides what gets built and why: turning customer problems and field evidence into prioritised, well-specified products that engineering can ship and customers will pay for.

## The role

**Day to day.** A technical product manager (TPM in some companies; note that "TPM" also means technical program manager elsewhere) owns a product area's outcomes rather than its code:

- **Discovery:** talking to customers, reading field notes from forward deployed engineers and solutions teams, analysing usage data, and deciding which problems are worth solving.
- **Prioritisation and roadmap:** choosing what to build this quarter and what not to build, and explaining those choices to engineering, sales, leadership and customers.
- **Specification:** writing PRDs, user stories and acceptance criteria; agreeing API contracts and non-functional requirements with engineering.
- **Execution:** working with an engineering manager and designer on scope, trade-offs and sequencing; unblocking; running betas and launches.
- **Measurement:** defining success metrics, instrumenting them, and deciding what to change based on results.
- **For AI products:** defining quality through evaluations, choosing models and trade-offs between cost, latency and accuracy, and setting pricing that covers variable inference cost.

**Common titles / aliases:** Product Manager (Technical), Technical Product Manager, Platform Product Manager, API Product Manager, Developer Platform PM, Forward Deployed Product Manager, AI Product Manager, Product Manager – Applied AI, Group PM for platform areas. Some companies call the field-embedded version a Deployment Strategist (see [02 Deployment Strategist](../02-deployment-strategist/README.md)).

**Companies that hire for it (examples):** Google, Microsoft, Amazon (titled "Product Manager – Technical", PM-T), Stripe, Twilio, Atlassian, Datadog, Snowflake, Databricks, AI labs such as OpenAI and Anthropic, and most B2B SaaS and AI-native startups. Loop details below are typical / candidate-reported and change often.

**How it differs from FDE:**

| | Technical Product Manager | Forward Deployed Engineer |
|---|---|---|
| Owns | What to build and why, across many customers | Making it work for specific customers |
| Writes production code | No (may prototype, write SQL, read code) | Yes |
| Success measure | Product adoption, revenue, retention, quality metrics | Customer deployed, live and getting value |
| Scope | A product area, over quarters | A set of accounts, over weeks to months |
| Interview core | Product sense, prioritisation, metrics, execution, technical depth, behavioural | Coding, system design, decomposition, customer scenarios |

FDEs often move into PM because they already see which customer problems repeat, which workarounds are painful, and which features would remove whole classes of custom work. The shift is from "solve it for this customer" to "solve it once for every customer, and decide what not to solve".

**Who it suits:** people who enjoy deciding rather than building, who are comfortable being accountable without direct authority, who write clearly, who like data, and who can say no without damaging relationships. Former engineers do well on platform, API and AI products because they can judge feasibility and talk to developers as peers.

**Skills mix:**

| Skill | Weight |
|---|---|
| Product sense / customer empathy | High |
| Prioritisation and trade-off judgment | High |
| Written and verbal communication | High |
| Metrics and analytics | High |
| Technical depth (APIs, architecture, AI) | Med–High (High for platform / AI PM) |
| Stakeholder management / influence | High |
| Coding | Low (reading code and SQL helps) |
| Sales / commercial | Med (pricing, packaging, customer calls) |

**Career paths:** PM → senior PM → group PM / director of product → VP product or chief product officer; general management; founding a company (FDE + PM experience is a common founder profile); moving back to engineering leadership or forward deployed leadership.

## Typical interview loop

Loops vary by company; the rounds below are **typical / candidate-reported**. Ask the recruiter what each round tests.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Motivation, "why PM", background, level | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md), this file (role and motivation) |
| Product sense / product design | Users, problems, solutions, prioritisation within a design | This file (product sense), [04-decomposition](../../topics/04-decomposition/README.md) section E |
| Analytical / metrics | North Star, metric trees, root-causing a metric change, A/B tests | This file (metrics), [07-data-and-sql](../../topics/07-data-and-sql/README.md) |
| Execution / prioritisation | Roadmaps, trade-offs, launches, handling slips | This file (prioritisation, PRDs and execution) |
| Technical round (common for technical / platform PM) | System design at a PM level, APIs, AI concepts | [03-system-design](../../topics/03-system-design/README.md), [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md), [06-ai-llm](../../topics/06-ai-llm/README.md) |
| Strategy / estimation (sometimes) | Market sizing, build vs buy, competition | This file (strategy) |
| Written exercise or take-home (sometimes) | PRD, one-pager, product teardown | This file (PRD exercise) |
| Behavioural / leadership | Influence, conflict, failure, saying no | [10-behavioral](../../topics/10-behavioral/README.md) |
| Hiring manager / final | Fit, judgment, product taste | [11-company-specific](../../topics/11-company-specific/README.md), [14-questions-to-ask](../../topics/14-questions-to-ask/README.md) |

Commonly reported patterns: Google PM loops centre on product design, analytical, technical and leadership interviews; Amazon PM-T loops use Leadership Principle behavioural questions plus a technical round and often a written component; Meta-style loops use "product sense" and "analytical thinking" (execution) interviews. Smaller companies often add a take-home PRD or a case presented to the team.

## Common rounds — use the shared sections

- [04 Decomposition](../../topics/04-decomposition/ANSWERS.md): section E (abstract / product decomposition) is close to a product sense interview; practise framing users and problems before solutions.
- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): section A (scoping), D (product feedback and scale) and E (business acumen) map directly to PM work.
- [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md): for AI PM roles, know sections A, C, D, E (evaluation) and G (production) well enough to discuss trade-offs; you do not need to implement them.
- [03 System design](../../topics/03-system-design/ANSWERS.md): for technical PM rounds, practise drawing a high-level design and discussing scale, reliability and cost; skip deep internals.
- [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md): section A (APIs and integration) for platform and API PM roles.
- [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md): basic SQL and metrics reasoning; some loops ask you to write a query.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md) and [00 story bank](../../topics/00-overview/ANSWERS.md): influence without authority, conflict, failure and prioritisation stories.
- [01 Recruiter & motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) and [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md).
- Skip or skim: [02 Coding](../../topics/02-coding/README.md), [13 CS fundamentals](../../topics/13-cs-fundamentals/README.md), [12 Debugging](../../topics/12-debugging-troubleshooting/README.md).
- Shared questions for every role: [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).

## Role-specific questions

### Role and motivation
- [ ] What does a technical product manager do, and how is it different from a forward deployed engineer and a project manager?
- [ ] Why do you want to move from engineering or forward deployed work into product management?
- [ ] How do you work with engineering so that you are trusted rather than seen as overhead?

### Product sense and design
- [ ] Product sense: design a product to help small businesses get paid faster. Walk me through it.
- [ ] Product sense: improve the onboarding experience for a developer API platform.
- [ ] Product sense: pick a product you use often and tell me how you would improve it.
- [ ] How do you decide whether a customer request should become a product feature?

### Prioritisation and roadmaps
- [ ] Explain RICE and when you would use it instead of MoSCoW or Kano.
- [ ] You have ten candidate features, three engineers and one quarter. How do you decide what to build?
- [ ] Your largest customer demands a feature that no other customer has asked for. What do you do?
- [ ] How do you build and communicate a roadmap when priorities keep changing?
- [ ] How do you decide how much engineering capacity goes to technical debt versus new features?

### Customer research and discovery
- [ ] How do you run customer discovery interviews without leading the customer?
- [ ] How do you use input from forward deployed engineers and solutions teams without becoming a feature factory for one account?
- [ ] How do you validate a product idea before engineering builds it?

### PRDs and execution
- [ ] Exercise: outline a PRD for a new feature. What goes in it?
- [ ] How do you write requirements and acceptance criteria that engineers can build from?
- [ ] A launch date is at risk two weeks before release. How do you handle it?
- [ ] How do you run a launch from private beta to general availability?

### Metrics and analytics
- [ ] How do you choose a North Star metric and its input metrics?
- [ ] Metrics: weekly active users of a product dropped 10% week over week. How do you investigate?
- [ ] How do you design and read an A/B test, and when should you not run one?
- [ ] Which metrics would you track for a B2B SaaS product?

### API and platform products
- [ ] How is a platform or API product different from an end-user product?
- [ ] How do you version a public API and deprecate an endpoint without breaking customers?
- [ ] What does a good developer experience look like, and how do you measure it?
- [ ] Product exercise: define the product for an internal developer platform used by 200 engineers.

### AI product management
- [ ] How do you define and measure product quality for an LLM feature?
- [ ] How would you price an AI product with high and variable inference costs?
- [ ] How do you decide whether a problem should be solved with AI at all?
- [ ] How do you scope and launch an AI agent feature that will sometimes be wrong?
- [ ] Technical round: explain RAG, fine-tuning and evals as a PM, including when you would choose each.

### Strategy and business
- [ ] Should we build, buy or partner for a new capability? How do you decide?
- [ ] Estimation: estimate the annual market for an AI coding assistant sold to enterprises.
- [ ] How do you take competitors into account when setting product strategy?

### Behavioral
- [ ] Tell me about a time you said no to a stakeholder or customer.
- [ ] Tell me about a time you shipped something that did not work. What did you learn?
- [ ] Tell me about a time you influenced a decision without having authority.

## Model answers

### Role and motivation

#### What does a technical product manager do, and how is it different from a forward deployed engineer and a project manager?
A technical product manager is accountable for the outcomes of a product area: which problems it solves, for whom, in what order, and whether the result moves the business. The "technical" part means the product itself is technical (APIs, platforms, infrastructure, AI) or the PM works closely enough with engineering to make architecture-level trade-offs.

- **vs. FDE:** an FDE makes the product work for specific customers, often by building custom integrations and workflows. A PM looks across all customers, spots the repeated needs, and turns them into product so that the next FDE does not have to build them again. The FDE's question is "how do we make this customer succeed?"; the PM's is "what should we build so that most customers succeed?"
- **vs. project manager:** a project manager owns delivery of an agreed scope on time and budget. A product manager owns deciding the scope in the first place and whether it was the right thing to build. On a good team the PM owns "what and why", the engineering lead owns "how", and a delivery or program manager (if any) owns "when and who".

A one-line version for interviews: "I decide what is worth building, make sure we build it well, and prove it worked."

#### Why do you want to move from engineering or forward deployed work into product management?
**What they look for:** a real pull towards product decisions (not an escape from coding), evidence you already do PM-like work, and awareness of what you will give up.

**Structure:** what you have noticed, evidence, why PM, why this company.

- **What you noticed:** "Working with customers, I kept seeing that the biggest gains came from deciding what to build, not from building it faster."
- **Evidence:** times you shaped scope, wrote a spec, chose between options using data, or turned a repeated customer request into a reusable feature. Concrete examples: a requirements document you wrote, a prioritisation call you made, a metric you defined.
- **Why PM, honestly:** "I want to own the outcome across many customers. I accept that I will no longer be the person writing the code, and that my work will be judged on results I influence rather than control."
- **What you bring:** technical depth, so you can judge feasibility and cost, talk to developer customers as a peer, and earn engineering trust quickly.
- **Why here:** the product, its users, and why your background fits this specific area (for example, an API platform or an AI product).

Avoid "I am tired of coding" or "I want to be the CEO of the product".

#### How do you work with engineering so that you are trusted rather than seen as overhead?
Engineers trust PMs who make their work more valuable and less wasteful.

- **Bring problems, not solutions:** explain the user problem, the evidence and the success metric, then design the solution with the engineering lead. Engineers often find cheaper options than the PM would.
- **Be clear and stable:** written requirements, explicit priorities, and a short list of what is out of scope. Do not change direction mid-sprint without a strong reason, and explain the reason when you do.
- **Respect estimates:** ask "what would make this smaller?" rather than "can you do it faster?". Treat estimates as information, not negotiation.
- **Protect focus:** absorb stakeholder noise and ad hoc requests; batch them into planning.
- **Share context:** invite engineers to customer calls, share usage data and feedback, celebrate impact, not just shipping.
- **Own the hard calls:** make the trade-off decision when the team is stuck, and take responsibility when it is wrong.
- **Give space for engineering health:** agree a standing share of capacity for technical debt and reliability.

Trust is earned in the first few weeks by being useful: removing one blocker, making one hard prioritisation call, and writing one clear spec.

### Product sense and design

#### Product sense: design a product to help small businesses get paid faster. Walk me through it.
Use a simple structure and say it out loud: goal, users, problems, solutions, prioritise, metrics, risks.

- **Clarify the goal:** reduce days sales outstanding (time from invoice to cash) for small businesses; business goal for us, perhaps payments volume.
- **Users:** segment, for example sole traders who invoice occasionally, service businesses with recurring clients, and small B2B suppliers with net-30 terms. Pick one: service businesses with 5–50 clients, where late payment hurts cash flow most.
- **Problems (ranked):** invoices sent late or with errors; clients forget; paying is inconvenient (bank transfer details, no card option); chasing feels awkward; no visibility of who owes what.
- **Solutions:** invoice from a template right after the job is done; one-click pay links supporting cards and bank payments; automatic, polite reminders with escalation; a dashboard of overdue invoices; optional early-payment discount or invoice financing.
- **Prioritise:** start with pay links plus automatic reminders: highest impact on time-to-cash, low effort, and easy to measure.
- **Metrics:** median days to payment (primary), share of invoices paid by due date, pay-link conversion; guardrails: unsubscribes from reminders, customer complaints.
- **Risks:** payment fees reduce adoption; reminders annoy the business's clients; fraud on pay links.

Finish with a one-sentence recommendation and what you would test first.

#### Product sense: improve the onboarding experience for a developer API platform.
**Goal:** more developers reach first value quickly. Define "activated": for example, a successful production API call within 7 days of sign-up, or the first live transaction.

**Users:** an individual developer evaluating the API; a startup engineer integrating for real; an enterprise team with security reviews. Focus on the individual developer, who decides whether the platform is worth adopting.

**Find the drop-offs:** funnel from sign-up → API key created → first test call → first successful integration → production use. Look at where most users stop, read support tickets and community questions, and watch five developers try it.

**Likely problems and fixes:**
- Time to first call is long → a quickstart that works in under five minutes, keys shown immediately, copy-paste examples in popular languages, and an interactive API explorer.
- Confusing errors → clear error messages with links to fixes.
- Hard to test safely → sandbox mode with test data.
- Unclear next step → sample apps for common use cases and a checklist from test to production.
- Enterprise friction → self-serve security documentation and SSO.

**Metrics:** time to first successful call (median), activation rate within 7 days, 30-day retention of activated developers; guardrail: support tickets per new developer.

**First bet:** the five-minute quickstart plus better error messages, since they affect every new developer.

#### Product sense: pick a product you use often and tell me how you would improve it.
**What they look for:** a product you genuinely know, clear thinking about users and their problems, a prioritised improvement, and metrics, not a list of features you would like.

**Structure:**
1. **The product and its mission** in one sentence, and why you chose it.
2. **Users and segments,** then pick one segment whose needs are underserved.
3. **Pain points** for that segment, with evidence (your own use, reviews, observed behaviour).
4. **Two or three solutions,** from small to bold.
5. **Prioritise** by impact on the segment and the business versus effort and risk; choose one.
6. **Success metrics** and a guardrail.
7. **Risks** and how you would test before full build.

**Example shape (adapt to a product you actually use):** a calendar app for people who manage both work and personal calendars. Pain point: double-booking across calendars and time spent copying events. Solutions: cross-calendar busy blocking; smart "find a time" that respects both; weekly planning view. Choose cross-calendar busy blocking: high value, moderate effort. Metric: reduction in conflicting events per user; guardrail: privacy complaints. Test with an opt-in beta.

Show taste: say what you would not change and why.

#### How do you decide whether a customer request should become a product feature?
I treat the request as evidence of a problem, not a specification.

1. **Understand the underlying problem:** ask why they need it, what they do today, and what it costs them. Often the real need is different or smaller.
2. **Check how widespread it is:** search other customer feedback, FDE and support notes, sales loss reasons, and usage data. One loud customer is a data point; the same need across many accounts is a pattern.
3. **Check strategic fit:** does it serve our target segment and product direction, or pull us towards a niche?
4. **Estimate value and cost:** revenue at risk or unlocked, retention effect, engineering effort, and the long-term cost of maintaining it.
5. **Consider alternatives:** an existing feature with better documentation, a configuration option, an extension point (API, webhook, plug-in) that lets the customer or FDE build it, or a partner.
6. **Decide and close the loop:** tell the customer what we will do and why, including "not now". Track the request so that if more customers ask, the decision can be revisited.

A useful rule: build into the product when the need is general; provide extension points when the need is specific.

### Prioritisation and roadmaps

#### Explain RICE and when you would use it instead of MoSCoW or Kano.
- **RICE** (popularised by Intercom): score = Reach × Impact × Confidence ÷ Effort. Reach is how many users or accounts are affected in a period; Impact is a scale (for example 0.25 to 3); Confidence is a percentage reflecting evidence quality; Effort is person-weeks or months. Good for comparing many candidate features on a backlog in a consistent way, and for forcing an honest view of confidence.
- **MoSCoW:** Must, Should, Could, Won't (this time). Good for fixing scope for a release or a project with a deadline, especially with customers, because it makes "won't" explicit.
- **Kano:** classifies features as basic expectations (absence causes dissatisfaction), performance features (more is better) and delighters (unexpected, high appeal). Good for understanding how customers perceive features, usually from surveys, and for making sure basics are not neglected in favour of shiny items.

Others worth naming: value versus effort 2×2 for quick workshops, cost of delay / WSJF for sequencing work by urgency, and opportunity solution trees for discovery.

Key point for the interview: frameworks support judgment, they do not replace it. I use scores to start a discussion, then sanity-check against strategy, dependencies and risk.

#### You have ten candidate features, three engineers and one quarter. How do you decide what to build?
1. **Start from the goal:** what outcome is this quarter about? For example, improving activation or reducing churn in a segment. Features that do not serve the goal drop down immediately.
2. **Clarify each feature's problem and evidence:** who asked, how many customers, data supporting it. Merge duplicates.
3. **Size roughly with engineering:** T-shirt sizes are enough. Three engineers for a quarter is roughly 30 engineer-weeks, minus around 20–30% for support, bugs and technical debt.
4. **Score:** RICE or value versus effort, with honest confidence levels.
5. **Check dependencies and risk:** some features unblock others; some carry technical risk better tackled early.
6. **Choose a small number:** usually two or three features done well beats seven half-finished. Put one high-confidence win first to build momentum and learning.
7. **Define success metrics** for each.
8. **Communicate:** share the chosen list, what was not chosen and why, and when it will be reconsidered.

Mention that you would check the plan mid-quarter and adjust based on results.

#### Your largest customer demands a feature that no other customer has asked for. What do you do?
Take it seriously without letting one account set the roadmap.

1. **Understand the need:** meet the customer with the account team; find the underlying problem and the business impact (renewal risk, expansion, compliance deadline).
2. **Quantify both sides:** revenue at stake versus engineering cost, opportunity cost (what else does not get built), and long-term maintenance.
3. **Look for generalisation:** could a more general version serve other customers or our target market? Often a niche request points to a broader need described in customer-specific language.
4. **Consider alternatives:** configuration, an API or extension point, an FDE or professional services build on top of the product, a partner, or a workaround.
5. **Decide with stakeholders:** involve sales leadership and engineering; make the trade-off explicit ("building this delays X by six weeks").
6. **Communicate clearly to the customer:** what we will do, when, and what we will not do, without vague promises.

If we do build a custom item, isolate it behind a flag or extension so it does not complicate the core product.

#### How do you build and communicate a roadmap when priorities keep changing?
- **Make it outcome-based:** organise around problems and goals ("reduce onboarding time") rather than a list of features with dates. This survives changes in the specific solution.
- **Use time horizons:** Now (committed, high confidence), Next (likely, being explored), Later (direction, low confidence). Only "Now" carries dates.
- **Separate audiences:** engineering needs detail; leadership needs outcomes and trade-offs; customers need external versions without firm dates unless committed.
- **Have a change process:** new requests go through the same prioritisation; when something comes in, something else goes out, and the trade-off is written down.
- **Review on a rhythm:** monthly or quarterly roadmap review with stakeholders, so changes happen at known points rather than every week.
- **Explain the why:** every roadmap update states what changed and why (new data, customer learning, strategy change).

If priorities change every week, that is a signal: find out whether the strategy is unclear, and raise it with leadership rather than re-planning endlessly.

#### How do you decide how much engineering capacity goes to technical debt versus new features?
- **Agree a default:** many teams reserve a fixed share (often 15–30%) for technical debt, reliability and maintenance, adjusted by team. A standing allocation avoids negotiating every item.
- **Make debt visible in business terms:** engineering identifies the debt; together we describe its cost: slower delivery, incidents, on-call load, security risk, inability to build a planned feature.
- **Prioritise debt the same way as features:** debt that blocks roadmap items or causes customer-facing incidents goes first.
- **Use signals:** rising incident rates, deploy frequency dropping, lead time growing, or engineers repeatedly working around the same component are signs to increase the share temporarily.
- **Bundle when sensible:** combine refactoring with a feature in the same area to deliver both.

As a PM I treat a healthy platform as part of the product. Neglecting it shows up later as missed dates and outages that customers notice.

### Customer research and discovery

#### How do you run customer discovery interviews without leading the customer?
- **Prepare a goal:** what decision will these interviews inform? Write 5–8 open questions, not a feature pitch.
- **Ask about past behaviour, not hypotheticals:** "Tell me about the last time you had to reconcile invoices. What did you do?" beats "Would you use a reconciliation feature?" People are poor at predicting what they will use.
- **Use open questions and follow-ups:** "What happened next?", "Why was that hard?", "What did it cost you?", "What have you tried?"
- **Do not pitch or describe your solution** until the end, if at all. When you show a concept, watch reactions and ask what they would do with it.
- **Listen for workarounds and spending:** spreadsheets, manual steps and money already spent on alternatives are the strongest signals of real pain.
- **Talk to the right people:** users, buyers and admins have different problems; include churned and non-customers, not only fans.
- **Record and synthesise:** notes or recordings (with permission), tag themes, count how many interviews mention each, and share a short summary with the team.

The book *The Mom Test* by Rob Fitzpatrick covers this approach well.

#### How do you use input from forward deployed engineers and solutions teams without becoming a feature factory for one account?
Field teams are the best early signal of what customers need; the risk is that the loudest account drives the roadmap.

- **One intake channel:** a shared form or tracker where FDEs log requests with the customer problem, workaround used, effort spent, revenue at stake, and how many customers they have seen it at.
- **Tag and aggregate:** group requests by underlying problem, not by wording. Review regularly to see patterns across accounts.
- **Watch for repeated custom work:** if FDEs have built the same integration or script three times, that is a strong product candidate because it is proven and costed.
- **Regular field reviews:** a monthly session with FDE and solutions leads to walk through themes and give decisions back.
- **Provide extension points:** APIs, webhooks, templates and plug-ins let FDEs solve customer-specific needs without changes to the core product.
- **Close the loop:** tell field teams what was decided and why, so they keep sending good signal.
- **Spend time in the field myself:** join deployments and customer calls to see context first-hand.

Weight requests by strategic fit and breadth, not by who shouts loudest.

#### How do you validate a product idea before engineering builds it?
Validate the riskiest assumption first, as cheaply as possible. Common risks are value (do they want it?), usability (can they use it?), feasibility (can we build it?) and viability (does it work for the business?).

- **Problem validation:** discovery interviews and data showing the problem is frequent and costly.
- **Demand tests:** a landing page or waitlist, a "fake door" button that measures clicks before the feature exists (followed by an honest message), or asking customers to commit to a pilot or letter of intent.
- **Prototype tests:** clickable mock-ups or a no-code version shown to five to eight users to test usability.
- **Concierge or "Wizard of Oz":** deliver the outcome manually behind the scenes to learn what matters before automating. Common for AI features: humans do the task first, then the model.
- **Technical spikes:** a short engineering investigation for feasibility risk, for example whether a model reaches acceptable accuracy on real data.
- **Pricing tests:** discuss price early in interviews; test willingness to pay with design partners.

Set the success threshold before the test, so the result changes a decision.

### PRDs and execution

#### Exercise: outline a PRD for a new feature. What goes in it?
A PRD (product requirements document) should be short and focused on the problem. Typical outline:

1. **Summary:** one paragraph on what and why.
2. **Problem and evidence:** who has the problem, how often, what it costs, quotes and data.
3. **Goals and non-goals:** what success looks like and what is explicitly out of scope.
4. **Success metrics:** primary metric with target, secondary metrics, guardrails.
5. **Users and use cases:** personas and key user stories or jobs to be done.
6. **Requirements:** functional requirements prioritised (must / should / could), with acceptance criteria; non-functional requirements: performance, availability, security, privacy, compliance, accessibility.
7. **User experience:** flows, mock-ups or links to designs.
8. **Technical considerations:** dependencies, API changes, data, migrations; usually written with engineering or linked to a design doc.
9. **Rollout plan:** beta, feature flags, migration, documentation, support and sales enablement.
10. **Risks and open questions,** with owners.
11. **Timeline and milestones** at a high level.

For an AI feature, add an evaluation plan: test set, quality bar, failure modes and human fallback. In the exercise, fill sections with a concrete example rather than describing the template.

#### How do you write requirements and acceptance criteria that engineers can build from?
- **Start from user stories:** "As a [user], I want [capability] so that [outcome]." This keeps the why attached to the what.
- **Make acceptance criteria testable:** use Given/When/Then: "Given an invoice is 7 days overdue, when the daily job runs, then a reminder email is sent to the billing contact, and the invoice shows 'reminder sent' with a timestamp."
- **Cover edge cases and errors:** empty states, permissions, limits, invalid input, timeouts, retries, partial failure.
- **State non-functional requirements as numbers:** "p95 response under 300 ms at 500 requests per second", "available 99.9% monthly", "data retained 90 days".
- **Separate must-haves from nice-to-haves.**
- **Do not over-specify the solution:** describe behaviour and constraints, and let engineering choose the implementation.
- **Review together:** walk through with engineers and QA before the sprint; their questions reveal gaps. Keep the document updated as decisions are made.

Good requirements mean an engineer could build it and a tester could verify it without asking you what you meant.

#### A launch date is at risk two weeks before release. How do you handle it?
1. **Get the facts quickly:** with the engineering lead, find what is late, why, and how much remains. Distinguish between known remaining work and unknowns.
2. **Check what the date is tied to:** a customer commitment, a marketing event, a contract, or an internal target. This determines how fixed it is.
3. **Lay out options:**
   - Cut scope: ship the must-haves and move the rest to a fast follow.
   - Launch to a smaller audience: beta or a subset of customers behind a feature flag.
   - Move the date: if quality or safety would suffer.
   - Add help: only if the work can be split without slowing the team.
4. **Never trade away quality silently:** security, data integrity and reliability are not optional cuts.
5. **Decide with stakeholders:** present options with consequences; get a decision within a day or two.
6. **Communicate early:** to sales, support, marketing and any committed customers, with the new plan.
7. **Learn:** after launch, review why it slipped (estimation, scope creep, dependencies) and adjust.

#### How do you run a launch from private beta to general availability?
- **Private beta / alpha:** a small number of design partners who agreed to give feedback; feature flagged; direct channel with the team. Goal: does it solve the problem, and what breaks?
- **Define exit criteria for each stage:** for example, key workflow completed by most beta users, no severity-1 bugs open, performance targets met, support documentation ready.
- **Public beta:** wider opt-in audience; measure adoption and quality at scale; label as beta in the product and docs; set expectations on support and change.
- **Launch readiness:** documentation and API reference, pricing and packaging, sales and support enablement (FAQ, demo, known issues), monitoring and alerts, rollback plan, legal and security review, marketing announcement.
- **General availability:** gradual rollout by percentage or segment, watch metrics and errors, a launch room or channel for the first days.
- **After launch:** compare against success metrics at 30 and 90 days; collect feedback; plan follow-ups; write a short launch review.

For API products, GA also means a stability commitment: breaking changes after GA must follow the deprecation policy.

### Metrics and analytics

#### How do you choose a North Star metric and its input metrics?
A North Star metric captures the core value customers get from the product and predicts long-term business success. It should be measurable, move with customer value (not just revenue), and be something the team can influence.

**Steps:**
1. Ask what value the product delivers and to whom.
2. Find the action that represents that value being received. Examples: for a payments API, successful payment volume; for a collaboration tool, weekly active teams creating content; for an AI assistant, tasks completed that users accept.
3. Check it against "would increasing this ever be bad for customers?" If yes, add guardrails.

**Input metrics** are the levers the team works on, forming a metric tree. For a payments API: new accounts activated × payments per active account × success rate. Teams own inputs; the North Star shows whether the combined effect works.

**Guardrails** protect against harmful optimisation: latency, error rate, fraud rate, support contacts, churn.

Avoid vanity metrics such as total sign-ups or page views, which grow without value being delivered.

#### Metrics: weekly active users of a product dropped 10% week over week. How do you investigate?
1. **Check the data first:** did tracking, a definition, or a data pipeline change? Was there a release that touched instrumentation? Compare with other sources (server logs, revenue).
2. **Check external causes:** seasonality (holiday, end of school term), last year's same week, marketing campaigns ending, competitor launches, news, outages.
3. **Segment the drop:** by platform (web, iOS, Android), country, new versus returning users, acquisition channel, customer tier, product version. A drop concentrated in one segment points to a cause; for example, only Android new users after a release suggests an app bug or onboarding change.
4. **Walk the funnel:** sign-up, activation, core action. Find the step where the drop appears.
5. **Check internal changes:** releases, experiments, pricing changes, emails, notifications, rate limits, outages or latency.
6. **Form and test hypotheses** with data; talk to support and to a few affected users.
7. **Act and communicate:** fix or roll back, then share findings and prevention (monitoring, alerts).

Say what you would look at first and why; interviewers like a prioritised approach over a long list.

#### How do you design and read an A/B test, and when should you not run one?
**Design:**
- A clear hypothesis: "Showing a code sample on the sign-up page will increase 7-day activation."
- One primary metric, plus guardrails (errors, latency, revenue, unsubscribes).
- Randomisation unit: user, account or session; for B2B often account, to avoid users in the same company seeing different versions.
- Sample size from baseline rate, minimum detectable effect, significance level (often 5%) and power (often 80%); fix duration in advance and cover full weekly cycles.

**Read:**
- Check the split is balanced (sample ratio mismatch suggests a bug).
- Look at the confidence interval, not just the p-value; is the effect practically meaningful?
- Do not stop early when it looks significant ("peeking"), and be wary of testing many metrics or segments without correction.
- Watch for novelty effects and long-term effects.

**Do not run one when:** there is too little traffic (common in B2B); the change is clearly needed (bug fixes, legal requirements); the effect is long-term or network-wide; or the experiment would harm users. Use qualitative research, staged rollouts, or before/after analysis with care instead.

#### Which metrics would you track for a B2B SaaS product?
Group them so they tell a story from acquisition to revenue:

- **Acquisition and activation:** trials or sign-ups, activation rate (reached first value), time to value.
- **Engagement and adoption:** weekly active users and active accounts, feature adoption, depth (seats used versus purchased), stickiness (daily over monthly active users) where daily use makes sense.
- **Retention:** logo churn, gross revenue retention, net revenue retention (includes expansion; above 100% means existing customers grow), cohort retention curves.
- **Revenue:** annual recurring revenue (ARR), average contract value, expansion revenue, conversion from trial to paid.
- **Efficiency (company level):** customer acquisition cost, CAC payback period, lifetime value to CAC ratio.
- **Customer health and quality:** NPS or CSAT, support tickets per account, incidents and uptime, time to resolve.

Pick a few for the team's area and connect them to the North Star. For platform and API products add API success rate, latency and integrations live per account.

### API and platform products

#### How is a platform or API product different from an end-user product?
- **Users are developers (and their companies):** they judge the product by documentation, consistency, reliability and how quickly they can build, not by visual design.
- **Value is indirect:** customers create value by building on top; success is measured by integrations live, API volume and what partners build, not just logins.
- **Changes are expensive to undo:** once an API is public, customers depend on its behaviour. Breaking changes break their production systems, so design and versioning matter more.
- **Non-functional requirements are the product:** latency, uptime, rate limits, security, idempotency and clear errors are features.
- **Multiple audiences:** the developer who integrates, the buyer who pays, and internal platform consumers; their needs differ.
- **Generality versus specificity:** a platform PM must resist building one customer's workflow into the core and instead provide building blocks.
- **Ecosystem:** SDKs, sample apps, community, partner integrations and marketplace are part of the product.

As a PM I spend more time on API design reviews, documentation, deprecation policy and developer research than I would on an end-user product.

#### How do you version a public API and deprecate an endpoint without breaking customers?
**Versioning:**
- Prefer additive, backwards-compatible changes (new optional fields, new endpoints) so most changes need no new version.
- Define what counts as breaking: removing or renaming fields, changing types or meaning, new required parameters, changed error codes or defaults.
- Choose a scheme: version in the URL (/v1/), in a header, or date-based versions pinned per account (the model Stripe is known for). Publish the policy.

**Deprecation:**
1. Measure usage of the endpoint by customer from API logs.
2. Announce with a clear timeline (often 6–12 months for public APIs), the reason, and a migration guide with examples.
3. Mark as deprecated in docs and responses (for example a Deprecation or Sunset header), and in SDKs.
4. Contact the heaviest users directly; offer help, sometimes via FDEs or solutions engineers.
5. Track migration progress; send reminders; consider brief planned "brownouts" to surface forgotten callers.
6. Remove only when usage is near zero or the timeline ends, with leadership agreement for any remaining important customers.

The principle: never surprise a customer in production.

#### What does a good developer experience look like, and how do you measure it?
**What good looks like:**
- Fast start: sign up and make a first successful call in minutes, with keys and a sandbox available immediately.
- Clear, complete documentation: quickstarts, guides by use case, full API reference, examples in popular languages, and an explorer to try calls.
- Consistent API design: predictable naming, pagination, errors and authentication across endpoints.
- Helpful errors: messages that say what went wrong and how to fix it, with request IDs.
- Good tooling: SDKs, CLI, webhooks with test tools, logs and dashboards to debug.
- Reliability and transparency: status page, changelog, deprecation policy.
- Support: community, docs search, responsive support for paying customers.

**Measures:**
- Time to first successful call; activation rate.
- Time to production integration.
- API error rates by type (4xx caused by developer confusion are a DX signal).
- Documentation usage and search queries with no results.
- Support tickets per active developer, by topic.
- Developer satisfaction surveys or NPS; friction logs from watching developers use the product.

#### Product exercise: define the product for an internal developer platform used by 200 engineers.
Treat internal engineers as customers.

- **Goal:** help product teams ship safely and faster with less effort on infrastructure. Business outcomes: faster delivery, fewer incidents, lower cloud cost.
- **Users:** product engineers (want speed and simplicity), platform and SRE engineers (want reliability and standards), security and compliance (want guardrails), engineering leaders (want visibility).
- **Discovery:** interview teams, map the path from idea to production, measure where time is spent: environment setup, CI waits, deploy approvals, on-call toil.
- **Product scope ("golden paths"):** service templates with CI/CD, observability and security built in; self-service environments; a service catalogue showing owners and dependencies (Backstage is a common open-source option); secrets and access management; cost visibility per team.
- **Prioritise:** the biggest shared bottleneck first, often new service creation or deployment pipeline time.
- **Metrics:** the DORA metrics (deployment frequency, lead time for changes, change failure rate, time to restore), time to create a new service, adoption of golden paths, developer satisfaction survey.
- **Adoption strategy:** make the paved road easier than alternatives instead of mandating it; migrate a few willing teams first and publish results.

### AI product management

#### How do you define and measure product quality for an LLM feature?
For AI features, evaluations are the product specification: quality is defined by the test set and the bar it must meet.

1. **Define the task and what "good" means** with users and domain experts: correct, grounded in sources, complete, safe, right format and tone.
2. **Build an evaluation set:** real or realistic examples covering common cases, edge cases and known failure modes, with expected outputs or grading rubrics. Start with 50–200 examples and grow it from production failures.
3. **Choose graders:** exact or programmatic checks where possible; model-based grading (LLM as judge) for open-ended outputs, calibrated against human ratings; human review for high-stakes items.
4. **Set launch thresholds,** for example 90% correct on the core set, zero unsafe answers on the safety set, p95 latency under 3 seconds, cost per task under a target.
5. **Measure in production:** user signals (accept, edit, retry, thumbs down, escalation to a human), task completion, sampled human review, and drift.
6. **Run evals on every change** to prompts, models or retrieval, like regression tests.

As a PM I own the definition of quality and the thresholds; engineering owns building the evaluation pipeline. See [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) section E.

#### How would you price an AI product with high and variable inference costs?
**Understand the costs:** cost per task (tokens in and out, retrieval, tool calls, retries), how it varies by user and task type, and how it will fall as models get cheaper. Know your gross margin at heavy, median and light usage.

**Pricing options:**
- **Per seat:** simple and predictable for buyers; risk is heavy users destroying margin. Mitigate with fair-use limits or tiers.
- **Usage-based** (per request, token, credit): aligns price with cost; buyers dislike unpredictable bills, so offer committed-use discounts and budget alerts.
- **Outcome-based** (per resolved ticket, per document processed): aligns with customer value; requires agreed definitions and measurement.
- **Hybrid:** platform or seat fee plus included usage and overage. Common in practice.

**Levers to protect margin:** route simple tasks to smaller, cheaper models; cache repeated work; limit context sizes; set quotas.

**Approach:** price from customer value (time or cost saved), check it covers cost at heavy usage with healthy margin, test with design partners, and keep packaging simple. Revisit regularly as model prices change.

#### How do you decide whether a problem should be solved with AI at all?
Start from the problem, not the technology.

- **Is the problem worth solving?** Frequency, cost, and value of solving it.
- **Would a simpler solution work?** Rules, search, better forms, or a workflow change are cheaper, more predictable and easier to debug. If the logic can be written down as clear rules, use rules.
- **Does it suit AI?** Good fits involve unstructured input (text, documents, speech, images), language generation or understanding, many variations that rules cannot cover, and tasks where "mostly right with review" is useful.
- **What is the cost of an error?** If mistakes are expensive or irreversible, you need human review, confidence thresholds, or a narrower scope. If the required accuracy is near 100% and errors cannot be caught, AI may be the wrong choice today.
- **Is there data to evaluate it?** Without examples of good outputs you cannot measure quality.
- **Do unit economics work?** Inference cost per task versus value per task.
- **Can it be explained and governed?** Regulatory and privacy constraints.

A quick prototype on real examples with an eval set usually answers the question in days.

#### How do you scope and launch an AI agent feature that will sometimes be wrong?
- **Start narrow:** pick a well-defined task with clear success criteria and limited actions, for example drafting refund decisions for orders under a set value, rather than "handle all support".
- **Design for errors:** human in the loop for consequential actions (agent proposes, human approves); confidence thresholds with handover to a person; read-only tools first, write actions later; limits on spend and number of steps.
- **Make it visible:** show users what the agent did and why, with sources; easy undo; clear labelling that it is AI.
- **Define quality and safety bars** with an evaluation set including adversarial cases such as prompt injection, and agree acceptable error rates with stakeholders by error type.
- **Roll out gradually:** internal users, then design partners, then a percentage of traffic; monitor task success, escalation rate, user corrections and incidents.
- **Plan operations:** logging and traces for every run, a process to review failures and add them to evals, a kill switch, and an incident process.
- **Set expectations** with customers and sales about what it does and does not do.

Expand scope as data shows it is reliable.

#### Technical round: explain RAG, fine-tuning and evals as a PM, including when you would choose each.
- **Prompting:** instructions and examples in the request. Cheapest and fastest to change; always try first.
- **RAG (retrieval-augmented generation):** before answering, the system searches a knowledge source (documents, tickets, database) and puts the relevant passages into the prompt. Use it when answers depend on company-specific or frequently changing information, or when you need citations. Main risks: poor retrieval leads to wrong answers; access control must be respected in what is retrieved.
- **Fine-tuning:** further training a model on examples to change its behaviour, style or format, or to make a smaller model do a narrow task well. Use it when prompting cannot reach consistent behaviour, or to cut cost and latency at high volume. It is not the best way to add knowledge that changes, needs training data and maintenance, and must be repeated when base models change.
- **Evals:** test sets and grading that measure whether any of these works. Needed in every case; they turn "it seems better" into numbers and catch regressions.

PM decision order: prompt → add RAG for knowledge → consider fine-tuning for behaviour or cost, each step justified by eval results. See [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md).

### Strategy and business

#### Should we build, buy or partner for a new capability? How do you decide?
Key questions:

- **Is it core to our differentiation?** If customers choose us because of it, build and own it. If it is necessary but not differentiating (authentication, billing, email delivery), prefer buy.
- **Time to market:** buying or partnering is usually faster; does speed matter competitively?
- **Total cost of ownership:** build cost plus years of maintenance, on-call and upgrades, versus licence fees that grow with usage.
- **Quality and maturity:** can a vendor do it better than we could in the next year?
- **Control and risk:** vendor lock-in, pricing changes, data residency and privacy, the vendor's financial health, and roadmap dependence.
- **Integration effort:** how well it fits our architecture and customers' needs.
- **Partner option:** when a partner serves the need and both sides benefit commercially (integration, marketplace, reselling), with less control.

Make the decision with a simple comparison table and a recommendation, plus a review point. In AI products this often appears as "build our own model versus use a model provider's API": most teams start with APIs and only train or host their own when cost, latency, privacy or differentiation demand it.

#### Estimation: estimate the annual market for an AI coding assistant sold to enterprises.
State assumptions clearly and round numbers; the structure matters more than the answer.

- **Developers:** assume roughly 25–30 million professional developers worldwide (public estimates vary; state the figure you use).
- **Share in enterprises that buy tools centrally:** assume about half, so roughly 13–15 million.
- **Price:** assume an enterprise seat costs about $20–40 per month; use $30, so $360 per developer per year.
- **Potential market:** 14 million × $360 ≈ $5 billion a year if every enterprise developer had a seat.
- **Realistic adoption over a few years:** assume 40–60% penetration, giving roughly $2–3 billion.

**Sanity checks:** compare with enterprise spending on other developer tools per head; consider upside from usage-based agent features priced above seats, and downside from price competition and bundled offerings.

Close with which assumption matters most (price per seat and adoption) and how you would refine it (vendor-reported seat counts, surveys, analyst reports).

#### How do you take competitors into account when setting product strategy?
- **Start with customers, not competitors:** strategy should come from the problems of our target customers and where we can win. Copying competitor features leads to a product without a point of view.
- **Know the landscape:** maintain a simple view of main competitors, their positioning, target segment, pricing, strengths and weaknesses; use win/loss interviews from sales, analyst reports, public docs and changelogs, and trying their products.
- **Identify table stakes versus differentiators:** match competitors on basics customers expect (otherwise we lose deals), and invest more in where we are or can be clearly better.
- **Watch for substitutes:** spreadsheets, internal tools, and in AI, foundation model providers adding the same capability natively.
- **Use win/loss data:** if we lose deals repeatedly for the same reason, that is a priority signal.
- **Do not react to every launch:** decide whether it changes customer behaviour before changing the roadmap.

Say "parity where required, differentiation where it matters, and focus on our segment".

### Behavioral

#### Tell me about a time you said no to a stakeholder or customer.

**What they look for:** you made a reasoned decision based on goals and evidence, explained it respectfully, offered an alternative, and kept the relationship.

**STAR outline:**
- **S:** [Who asked, what they wanted, why it mattered to them]
- **T:** [Why it conflicted with priorities, strategy, cost or quality]
- **A:** [How you understood their need, the data you used, how you explained the decision, the alternative you offered]
- **R:** [What happened, relationship afterwards, what the team could focus on instead]

**Example (adapt to your own experience):** "A sales lead asked my team to build a custom report for one prospect within two weeks, which would have delayed a billing fix affecting many customers. I met the prospect with sales to understand the actual need: they wanted a monthly export for their finance team. I showed that our existing export plus a saved filter covered most of it, and offered to add the remaining field in the next planned release. The prospect signed, and the billing fix shipped on time."

#### Tell me about a time you shipped something that did not work. What did you learn?

**What they look for:** ownership without blame, honest analysis, a fix, and a concrete change in how you work.

**STAR outline:**
- **S:** [What you shipped and the goal]
- **T:** [Your role and responsibility]
- **A:** [What went wrong, how you found out (metric, feedback), what you did to fix or roll back]
- **R:** [Outcome, and the specific lesson you now apply: for example, validating with users earlier, defining success metrics before launch]

**Example (adapt to your own experience):** "I led a feature for automated reminders to collect overdue payments. We built it from internal assumptions, and after launch usage was low: the finance team kept their manual process because the reminders could not be customised per client. I ran interviews, found the two settings that mattered, and we added them in a follow-up release, after which usage grew. Since then I run a prototype review with real users before committing engineering time."

#### Tell me about a time you influenced a decision without having authority.

**What they look for:** you understood other people's goals, used evidence, built allies, and changed a decision for a better outcome.

**STAR outline:**
- **S:** [The decision and who owned it]
- **T:** [Why you believed it should change]
- **A:** [How you learned their priorities, gathered data, built a coalition, framed the proposal in their terms, and handled objections]
- **R:** [Decision made, measurable outcome, relationship]

**Example (adapt to your own experience):** "Another team owned an internal API that my product depended on, and they planned to change its response format with little notice. I did not manage that team. I collected which consumers depended on the current format and the effort each would need, then proposed a versioned endpoint with a three-month migration window. Framing it as reducing their support load, not my convenience, won their lead's agreement; all consumers migrated without an incident."

## Prep checklist

1. Learn one product sense structure (goal, users, problems, solutions, prioritise, metrics, risks) and practise it out loud on five prompts with a timer of 25 minutes each.
2. Practise three metrics investigations ("metric X dropped") and three North Star questions, writing the metric tree each time.
3. Learn RICE, MoSCoW, Kano, cost of delay and value versus effort well enough to apply each to an example in two minutes.
4. Write a real one-to-two-page PRD for a feature in a product you know, including success metrics, non-goals and an evaluation plan if it uses AI.
5. Prepare a technical walk-through: draw the architecture of a system you worked on and explain trade-offs to a non-engineer and to an engineer.
6. For platform and API PM roles, study one well-known API's docs and versioning policy (for example Stripe or Twilio) and write a short critique of its developer experience.
7. For AI PM roles, build or review a small LLM feature with an eval set, measure cost per task, and draft a pricing model; review [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) sections C, D, E and G.
8. Prepare six behavioural stories covering saying no, failure, influence without authority, conflict with engineering, a data-driven decision and a customer insight; use [10 Behavioral](../../topics/10-behavioral/ANSWERS.md).
9. Do two estimation questions and one build-versus-buy question out loud.
10. Research the company's product: use it, read its changelog and docs, and prepare one improvement idea with metrics to mention in the final round.
11. Prepare questions about how product, engineering and field teams work together, using [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md).
