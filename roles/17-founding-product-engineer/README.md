# Founding / Product Engineer

One of the first engineers at an early-stage startup (often an AI startup), who owns whole features or the whole product from infrastructure to UI, talks to users every week, and ships fast with a small team and high ambiguity.

---

## The role

**Day to day.** At a seed or Series A company with a handful of people, a founding or product engineer typically:

- Ships features end to end: database schema, backend, LLM calls and prompts, frontend, deployment and monitoring, often several times a week.
- Talks to users: joins sales and onboarding calls with the founders, watches session recordings, reads support messages, and turns what they hear into what to build next.
- Makes product decisions alongside the founders: what to build, what to cut, what "good enough" means.
- Owns operations: on-call, incidents, cloud costs, security basics, and answering enterprise security questionnaires.
- Helps hire the next engineers and sets the early engineering culture and standards.

**Titles and aliases.** Founding Engineer, Founding Software Engineer, Founding AI Engineer, Product Engineer, Full-stack Product Engineer, Early Engineer, "Member of Technical Staff" at some AI startups. "Product Engineer" is also used at larger companies (for example PostHog, Intercom and Ramp have used the title) for engineers who own product outcomes, not just tickets.

**Who hires (examples).** Early-stage startups, especially those backed by accelerators and seed investors (Y Combinator's "Work at a Startup" board lists many founding engineer roles), AI-native startups building agents, copilots and vertical AI products, and product-led companies that use the Product Engineer title. Loop details below are typical or reported, not guaranteed; startups vary more than any other employer.

**How it differs from an FDE.**

| | Founding / Product Engineer | Forward Deployed Engineer |
|---|---|---|
| What you build | The company's own product, for all customers | Deployments and custom work for specific customers |
| Customer contact | Frequent but in service of the product: user interviews, early customer calls | Embedded with a customer, daily |
| Scope | Everything: infra, backend, frontend, AI, ops, hiring | The customer deployment and its integrations |
| Team and support | Tiny team, few processes, little existing code | Larger company with a platform behind you |
| Risk and reward | Lower salary, meaningful equity, high company risk | Market salary, smaller equity share, lower risk |
| Product influence | Very high: you shape the product directly | High: field learnings feed the product |

At very early AI startups the line blurs: a founding engineer may spend weeks forward deployed with the first design-partner customers.

**Who it suits.** Strong generalists who like owning outcomes, are comfortable with ambiguity and messy code written for speed, enjoy talking to users, and accept financial and career risk in return for scope, learning and equity.

**Skills mix.**

| Skill | Weight |
|---|---|
| Coding speed and full-stack breadth | High |
| Product sense and user empathy | High |
| System design for small teams (simple, managed, cheap) | Med to High |
| AI / LLM application engineering (at AI startups) | Med to High |
| Customer communication (calls, demos, support) | Med to High |
| Operations and security basics | Med |
| Sales involvement | Low to Med (joins founder-led sales calls) |

**Career paths.** Lead engineer or head of engineering as the company grows; CTO or VP Engineering; product leadership; staff engineer owning a product area; or founding your own company. If the startup fails, the breadth of experience usually transfers well to FDE, applied AI and product engineering roles.

---

## Typical interview loop

Early-stage loops are short and informal, often a week or less from first call to offer. Typical or reported shape:

| Round | What it tests | Prepare with |
|---|---|---|
| Founder intro call | Motivation, why startups, why this problem, what you have built | This file + [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| Technical conversation or past-project deep dive | Depth on something you built end to end; trade-offs you made | This file + [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |
| Take-home or live build (often 2–4 hours live, or up to a 48-hour take-home) | Shipping a working product quickly, scoping, code quality, product judgement | This file (Take-home and build exercises) + [02-coding](../../topics/02-coding/ANSWERS.md) |
| Practical system design | Pragmatic design for a small team; what changes at scale | This file + [03-system-design](../../topics/03-system-design/ANSWERS.md) |
| Product sense | Prioritisation, user problems, metrics, AI feature quality | This file + [04-decomposition](../../topics/04-decomposition/ANSWERS.md) |
| AI / LLM (at AI startups) | RAG, agents, evals, cost and latency | [06-ai-llm](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md) |
| Work trial (some companies) | Paid days or a week working with the team on real work | This file (paid work trial question) |
| Founder or team fit, offer conversation | Values, working style, equity and risk | This file + [14-questions-to-ask](../../topics/14-questions-to-ask/ANSWERS.md) |

Some startups still run a DS&A screen; many skip it in favour of a practical build.

---

## Common rounds — use the shared sections

- [02 Coding](../../topics/02-coding/ANSWERS.md): practical, product-style coding matters more than puzzles, but keep DS&A warm in case.
- [03 System design](../../topics/03-system-design/ANSWERS.md): especially the applied and AI/LLM prompts; always state the simple version first.
- [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md): RAG, agents, evaluation and production LLM engineering for AI startups; part 2 includes take-home build exercises.
- [04 Decomposition](../../topics/04-decomposition/ANSWERS.md): the product decomposition section is close to a product sense round.
- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): scoping, difficult customers, product feedback.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md): ownership, ambiguity and bias for action are the most relevant sections.
- [12 Debugging](../../topics/12-debugging-troubleshooting/ANSWERS.md): you will be on call for everything.
- [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md): due diligence on the company matters more here than anywhere else.
- Related roles: [01 Forward Deployed Engineer](../01-forward-deployed-engineer/README.md), [05 Applied AI Engineer](../05-applied-ai-engineer/README.md).

---

## Role-specific questions

### Role, fit and risk
- [ ] Why do you want to be a founding engineer at an early-stage startup rather than at a large company?
- [ ] How is a founding engineer different from a forward deployed engineer and a senior engineer at a large company?
- [ ] What do you look for in founders and an early-stage company before joining?
- [ ] How do you think about equity, salary and risk in an early-stage offer?

### Product sense
- [ ] Pick a product you use often. What would you change, and how would you measure it?
- [ ] You have ten customer requests and one engineer. How do you decide what to build next?
- [ ] Most users sign up but never return after the first day. How do you investigate and fix it?
- [ ] How do you talk to users to find out what they actually need?
- [ ] How do you decide when an AI feature is good enough to ship?

### Shipping fast and technical judgement
- [ ] How do you balance shipping speed against code quality and technical debt at an early-stage startup?
- [ ] What stack would you choose to build a new AI product from scratch, and why?
- [ ] How do you design a system so that a team of two can run it in production?
- [ ] When would you build, buy, or use an open-source tool at a startup?
- [ ] How do you ship safely without a QA team?
- [ ] How do you set up monitoring, on-call and incident response when you are one of two engineers?

### Take-home and build exercises
- [ ] Take-home: build a working product from a short brief in 48 hours. How do you plan and scope it?
- [ ] What makes a take-home submission stand out to founders?
- [ ] Live build: add a feature to an unfamiliar codebase in 90 minutes. How do you approach it?
- [ ] Build exercise: create a small AI app end to end, such as a support-ticket triage tool. Walk through your design.
- [ ] How do you present and defend your take-home in the follow-up review?
- [ ] How do you approach a paid work trial with the team?

### Customers and owning everything
- [ ] A large prospective customer asks for a custom feature that does not fit the product. What do you do?
- [ ] On a sales call with the founder, the customer asks a technical question you cannot answer. What do you do?
- [ ] How would you personally onboard the first ten customers?
- [ ] A customer says the product gave a wrong answer and they are angry. What do you do in the next 24 hours?
- [ ] How do you handle security questionnaires and enterprise requirements such as SSO and SOC 2 at a seed-stage company?

### System design for startups
- [ ] Design the first version of a multi-tenant backend for an AI product, then explain what changes at 100 times the usage.
- [ ] Design an LLM-powered feature with evaluation, cost control and fallbacks that one engineer can build in two weeks.
- [ ] Design usage metering and billing for an early usage-based AI product.

### Founder interview and working style
- [ ] What would you do in your first 30 days here?
- [ ] How do you work with a non-technical founder, or with a technical founder who still writes code?
- [ ] How would you help hire the next engineers and set the engineering culture?

### Behavioral (role-specific)
- [ ] Tell me about something you built and shipped end to end on your own.
- [ ] Tell me about a time you shipped something quickly that turned out to be wrong.
- [ ] Tell me about a time you worked with high ambiguity and no clear owner.
- [ ] Tell me about a time talking to users changed what you built.

---

## Model answers

### Role, fit and risk

#### Why do you want to be a founding engineer at an early-stage startup rather than at a large company?

**What they look for:** a real reason that survives hard months, not "startups are exciting". Founders worry about hiring someone who will leave when it gets messy or when the money runs low.

**Structure your answer:**
1. **What you want to do:** own outcomes end to end, ship weekly, talk to users, and make product decisions, not just implement tickets.
2. **Evidence you have done it:** a time you owned something from idea to production, ideally with users, and enjoyed the ambiguity.
3. **Why this company specifically:** the problem, the users, the founders, and why now. Show you have used the product or researched the market.
4. **Honest about the trade-off:** you understand the risk and lower salary, and have thought about it.

**Example (adapt to your own experience):** "At my current company I'm happiest when I own a problem end to end, like [project], where I talked to the users, designed it and ran it in production. In a large company that's the exception; here it's the job. I'm interested in [problem] because [personal reason or evidence], and after using your product and reading your customers' reviews I think [insight]. I've thought through the financial risk and I'm comfortable with it for the next few years."

#### How is a founding engineer different from a forward deployed engineer and a senior engineer at a large company?

- **Senior engineer at a large company:** deep ownership of a part of a mature system, with product managers, designers, QA, SRE and platform teams around them. Success is quality and impact within a well-defined scope.
- **Forward deployed engineer:** builds and deploys solutions with specific customers on top of an existing platform, with a larger company behind them.
- **Founding engineer:** builds the platform itself, with almost nothing around them. They are the product manager, the QA team, the SRE, sometimes the designer and the support team. Success is whether the company finds product-market fit before money runs out.

Practical differences: decisions are reversible and fast, so speed matters more than perfection; the codebase is small, so architectural choices have outsized long-term effect; and the work changes every few months as the company changes. A founding engineer also shapes culture and hiring.

At AI startups the founding engineer often does FDE-style work too, sitting with design-partner customers to make the first deployments succeed.

#### What do you look for in founders and an early-stage company before joining?

You are making an investment with your time, so do due diligence:

- **Founders:** do they know the problem deeply (domain experience or obsessive research)? Do they listen and change their minds with evidence? How do they handle disagreement? Talk to people who have worked with them.
- **Customers and traction:** who pays or uses the product today, how often, and what retention looks like. Ask to join a customer call or read anonymised feedback.
- **Runway and funding:** how many months of money are left at the current burn, who the investors are, and the plan for the next round. It is normal to ask this before accepting an offer.
- **Team and culture:** who else is there, how decisions are made, working hours and expectations, remote or in person.
- **Your role:** what you would own, how success is measured, and how the role might change as the company grows.
- **Equity terms:** see the next answer.

**Red flags:** vague answers about runway, no real users after a long time, founders who dismiss all criticism, or pressure to accept quickly without information.

#### How do you think about equity, salary and risk in an early-stage offer?

**Understand what you are offered** (this is general information, not tax or legal advice; terms differ by country):
- **Type:** stock options (the right to buy shares at a fixed strike price) or restricted stock. Ask the number of shares and the total fully diluted shares, so you know your percentage.
- **Vesting:** commonly four years with a one-year cliff.
- **Exercise terms:** the strike price, whether early exercise is allowed, and how long you have to exercise after leaving (often 90 days, sometimes extended to years).
- **Dilution and preferences:** future funding rounds dilute your percentage; investors' liquidation preferences are paid before common shareholders in a sale.

**Think about it realistically:**
- Most startups fail, so treat equity as a lottery ticket with better odds if the company is strong; do not count on it for living costs.
- Decide your minimum salary based on your real expenses and how many years you can sustain it.
- Model a few outcomes: company fails (equity worth zero), modest exit, strong exit.

**Negotiate:** many startups offer a trade-off between more salary and more equity. Ask for the details in writing, and ask a trusted advisor to review.

### Product sense

#### Pick a product you use often. What would you change, and how would you measure it?

**What they look for:** user empathy, clear prioritisation, a concrete idea, and a sensible way to measure it, not a long list of features.

**Structure:**
1. **The product and its users:** who uses it and for what main job. Pick a specific user segment.
2. **The problem:** one pain point you have seen yourself or in reviews, and why it matters (frequency, severity).
3. **Options:** two or three ways to solve it, and why you chose one (impact, effort, risk).
4. **The change:** describe it concretely, including the smallest version you could ship first.
5. **Measurement:** one primary metric tied to the user's job (for example task completion rate or time to complete), guardrail metrics (nothing else gets worse), and how you would test it (A/B test, or before-and-after with a cohort if traffic is low).
6. **Risks:** who might be hurt by the change.

**Tips:** choose a product you know well, and avoid ones the interviewer's company competes with unless you know them deeply. Show a trade-off you consciously made.

#### You have ten customer requests and one engineer. How do you decide what to build next?

1. **Go back to the company's current goal.** At an early stage it is usually one thing: activation, retention, or closing a specific set of design partners. Requests that do not move it wait.
2. **Find the problem behind each request.** Customers ask for solutions; several requests are often the same underlying problem.
3. **Score them roughly:** how many customers or how much revenue it affects, how painful it is (blocker or nice-to-have), effort, and whether it fits the product's direction or is a one-off.
4. **Look for cheap wins and blockers:** a one-day fix that unblocks a paying customer usually goes first.
5. **Say no or "not yet" clearly** to the rest, with a reason, and keep a visible list so customers feel heard.
6. **Ship small and check:** build the smallest version, ask the requesters if it solved their problem, and use usage data to decide whether to go further.

Mention that you would make the decision with the founders, quickly, and revisit it weekly; at a startup a good decision today beats a perfect one next month.

#### Most users sign up but never return after the first day. How do you investigate and fix it?

**Investigate:**
1. **Define the funnel:** sign-up, first key action (the "aha" moment), second session. Measure where users drop off with product analytics.
2. **Segment:** by acquisition channel, user type and device. Often one segment is fine and another is the problem (for example users from a launch post who were curious, not in need).
3. **Watch real sessions** (session recordings) and look at where people get stuck.
4. **Talk to users:** short calls with users who dropped off and users who stayed. Ask what they were trying to do and what happened.

**Typical causes and fixes:**
- Users never reach the first value: shorten onboarding, use sample data or templates, remove required setup steps.
- The product does not fit their need: an acquisition or positioning problem, not an onboarding one.
- No reason to come back: notifications, scheduled outputs or integrations into tools they already use.

**Fix one thing at a time,** measure day-1 and day-7 retention by sign-up cohort, and keep going. Be explicit that small samples at an early stage make metrics noisy, so combine numbers with conversations.

#### How do you talk to users to find out what they actually need?

- **Ask about the past, not the future.** "Tell me about the last time you did X" gives facts; "Would you use a feature that does Y?" gives polite guesses. This is the core idea of *The Mom Test* by Rob Fitzpatrick.
- **Focus on their problem and workflow,** not your product: what they do today, what it costs them, what they have already tried, and what they pay for.
- **Ask for specifics:** numbers, frequency, who else is involved, what happens when it goes wrong.
- **Listen for commitment,** not compliments: time, money, introductions, or data access are real signals.
- **Watch them work** when you can; screen-shares often reveal steps users would never mention.
- **Talk to the right people:** both the buyer and the daily user, and users who churned.
- **Take notes and share them** in one place so the whole team can see patterns; review across interviews, not one at a time.

As an engineer, combine conversations with data: interviews tell you why, usage data tells you how many.

#### How do you decide when an AI feature is good enough to ship?

**Define "good enough" for this use case before building:** what an error costs. A wrong summary of a meeting is cheap; a wrong drug dose is not. The bar and the design follow from that.

**Build an evaluation set early:** 50–200 real or realistic examples with expected outputs or grading criteria, covering common cases and known hard cases. Score automatically where possible (exact checks, an LLM-as-judge validated against human labels) and review a sample by hand.

**Set ship criteria:** for example a target accuracy on the eval set, no critical failures on the safety cases, and acceptable latency and cost per request.

**Design for imperfection:**
- Show sources or reasoning so users can check outputs.
- Keep a human in the loop where mistakes are costly (drafts, approvals).
- Let users correct outputs easily, and log those corrections as new eval data.
- Fall back gracefully ("I don't know") instead of guessing.

**Ship to a small group first** (design partners or a feature flag), monitor real usage and feedback, and expand when the numbers hold. See [Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important).

### Shipping fast and technical judgement

#### How do you balance shipping speed against code quality and technical debt at an early-stage startup?

**Principle:** at an early stage the biggest risk is building something nobody wants, so speed of learning wins. But some debt is cheap to avoid and expensive to fix later.

**Where I move fast:** UI and features that may be thrown away, internal tools, scripts, experiments behind feature flags. Simple code, minimal abstraction, manual steps are acceptable.

**Where I am careful from day one:**
- **Data model and data integrity:** schemas, migrations, backups. Bad data is hard to fix.
- **Security and tenant isolation:** auth, secrets management, access checks.
- **Billing and anything involving money.**
- **Public APIs** customers integrate with, because changing them is slow.

**Habits that keep speed sustainable:** small pull requests, a few high-value tests on critical paths, CI that deploys automatically, and a short list of known debt with the reason each item was accepted.

**Paying it down:** when a piece of debt slows down every change or causes incidents, fix it, ideally as part of the feature that touches it. Explain the trade-off in terms founders care about: "This costs two days now and saves us a day every week."

#### What stack would you choose to build a new AI product from scratch, and why?

**Principle:** choose boring, well-known tools the team already knows, managed services over self-hosting, and one language across as much of the stack as possible. Novelty should go into the product, not the infrastructure.

**A reasonable default (adapt to the team):**
- **Language and frameworks:** TypeScript with a full-stack framework such as Next.js, or Python (FastAPI) for the backend if the AI work is Python-heavy, with a TypeScript frontend.
- **Database:** managed PostgreSQL, which also handles vector search through the pgvector extension until you outgrow it.
- **Auth:** a managed auth provider, so SSO can be added later without rebuilding.
- **Hosting:** a managed platform (for example Vercel, Render or Fly.io) or a single cloud provider's managed services; containers for anything long-running.
- **Background jobs:** a simple managed queue or a Postgres-backed job queue.
- **LLMs:** call hosted model APIs through a thin internal wrapper so you can switch or mix models, log prompts and responses, and track cost.
- **Observability:** error tracking, logs and LLM tracing from day one.

**Say what would change the choice:** customer requirements (on-premises, data residency), the team's existing skills, and scale. The reasoning matters more than the brand names.

#### How do you design a system so that a team of two can run it in production?

- **Few moving parts:** one deployable application (a modular monolith), one database, one queue. Microservices multiply operational work.
- **Managed services everywhere possible:** database, auth, hosting, queues, email. Pay money to save engineering time.
- **Automate deploys:** every merge to main runs tests and deploys; rollbacks are one command.
- **Infrastructure as code or at least documented setup,** so the system can be rebuilt.
- **Observability that pages only for real problems:** error tracking, a few key alerts (site down, error rate up, queue backing up, LLM provider failing), and dashboards for usage and cost.
- **Idempotent background jobs with retries,** so failures recover without manual work.
- **Backups with tested restores.**
- **Runbooks** for the five most likely incidents.
- **Limits and guardrails:** rate limits, spending caps and alerts on cloud and LLM costs, so one bug or customer cannot cause a large bill.

Design question to ask yourself: "If this breaks at 3 a.m., can one tired person find and fix it in 30 minutes?"

#### When would you build, buy, or use an open-source tool at a startup?

**Build** when it is core to what makes the product different, when customers pay you for it, or when nothing available fits and the problem is small. Every line of code you write is code you maintain.

**Buy (managed or SaaS)** for anything that is not your differentiation and has a good vendor: auth, payments, email, error tracking, analytics, hosting, LLM APIs. The cost is usually far less than an engineer's time, and vendors handle security and compliance.

**Open source** when it is mature, widely used and well maintained, especially for libraries and frameworks. Self-hosting open-source infrastructure (search, queues, vector databases) adds operational work; prefer a managed version early on.

**Questions I ask:**
- Is this core to our product or a commodity?
- What does it cost in money versus engineering time, now and at ten times the scale?
- How hard is it to switch later (data lock-in, API surface)?
- Does it meet customer requirements (data location, security reviews)?

Revisit decisions as you grow: buying often makes sense early and building later, when scale or differentiation justifies it.

#### How do you ship safely without a QA team?

- **Automated tests where bugs are expensive:** auth and permissions, billing, data writes, and the main user path. A small set of end-to-end tests on critical flows beats high coverage everywhere.
- **Continuous integration:** tests, type checks and linting on every pull request; deploy automatically after merge.
- **Feature flags:** ship code dark, enable for internal users, then a few customers, then everyone. Turn features off without a deploy.
- **Small, frequent deploys:** a small change is easy to review and easy to roll back.
- **Fast rollback:** one-command rollback, and database migrations that are backward compatible (add columns first, remove later).
- **Preview environments** for each pull request so founders can click through changes.
- **Monitoring after deploy:** watch error rates and key metrics for the first 30 minutes.
- **Dogfooding:** the team uses the product daily.
- **Code review,** even with two people, for anything risky.

For AI features, add an eval suite that runs on prompt or model changes, because a prompt edit can break behaviour without any code failing.

#### How do you set up monitoring, on-call and incident response when you are one of two engineers?

**Monitoring:**
- Error tracking with alerts on new or spiking errors.
- Uptime checks on the main user paths and APIs.
- A few business alerts: sign-ups or key actions dropping to zero, background queue growing, LLM provider errors or latency rising, daily cost above a threshold.
- Structured logs with request IDs, and LLM call tracing.

**On-call:**
- Share it explicitly (for example alternate weeks), with a backup.
- Page only on customer-impacting problems; send everything else to a channel reviewed daily. Alert fatigue is the main risk.
- Agree expectations with founders: response times during and outside working hours.

**Incident response:**
1. Acknowledge, assess impact, tell affected customers early (a status page or direct message).
2. Mitigate first: roll back, disable the feature flag, switch model provider.
3. Find the root cause after service is restored.
4. Write a short blameless postmortem and fix the cause, plus the alert that should have caught it earlier.

See [12 Debugging](../../topics/12-debugging-troubleshooting/ANSWERS.md).

### Take-home and build exercises

#### Take-home: build a working product from a short brief in 48 hours. How do you plan and scope it?

**First hour: understand and scope.**
- Read the brief twice; list the user, the core job, and what the founders most likely want to see (product judgement, working software, code quality).
- Send clarifying questions early if allowed; otherwise write down your assumptions.
- Choose one core user flow and make it excellent. List "out of scope" items explicitly.
- Respect any time guidance; if they say "spend about four hours", do not spend twenty and hide it.

**Build in layers:**
1. A thin end-to-end slice working and deployed early (by the end of the first session), even if ugly.
2. Make the core flow correct and robust: validation, error states, empty states.
3. Polish what users touch first: clear UI, sensible defaults, sample data.
4. Tests for the core logic, and a seed script.

**Use tools sensibly:** familiar stack, managed services, and AI coding assistants if allowed (say so and own every line).

**Finish with:** a deployed link or one-command local setup, a README with decisions, trade-offs, known limitations and what you would do next, and a short demo video if appropriate.

**Leave buffer:** stop building several hours before the deadline to test from a clean setup and write the README.

#### What makes a take-home submission stand out to founders?

- **It works on first try:** a live link or a one-command setup, seed data, and no missing environment variables. Many submissions fail here.
- **Product judgement:** you chose a sensible scope, handled the real user's main problem, and added one or two thoughtful touches (good empty states, helpful errors, a useful default) rather than many half-finished features.
- **A clear README:** what you built, how to run it, key decisions and trade-offs, what you cut and why, and what you would do next with more time. Founders read this to judge how you think.
- **Clean, simple code:** readable structure, consistent naming, no dead code, sensible commits.
- **Tests where they matter:** the core logic, not boilerplate.
- **Honesty:** known bugs and limitations listed; use of AI tools disclosed if they asked.
- **A short demo video** (two to three minutes) walking through the product and one interesting decision.
- **Going one step beyond the brief only where it shows judgement,** such as noticing an edge case the brief missed.

What hurts: overengineering (microservices for a CRUD app), an unclear README, and spending much more time than asked without saying so.

#### Live build: add a feature to an unfamiliar codebase in 90 minutes. How do you approach it?

**Orient (10–15 minutes):** run the app and tests first. Read the README, the folder structure, and trace one existing feature similar to the one you will build, from UI to database. Say what you are learning out loud.

**Plan (5 minutes):** restate the feature, ask clarifying questions, and describe the smallest version you will deliver first, plus stretch goals.

**Build (50–60 minutes):**
- Follow the codebase's existing patterns rather than introducing your own.
- Work in small steps and run the app or tests after each one.
- Commit or checkpoint working states.
- Use the interviewer as a teammate: ask where things live instead of spending ten minutes searching.

**Finish (10 minutes):** demo the feature, add or update a test, and explain what you would improve (edge cases, refactors, performance).

**What they watch:** how quickly you get productive in someone else's code, whether you keep the app working, communication, and product sense (for example noticing a confusing UI state). Finishing a smaller version beats an unfinished ambitious one.

#### Build exercise: create a small AI app end to end, such as a support-ticket triage tool. Walk through your design.

**Scope:** incoming tickets are classified by category and urgency, a draft reply is suggested with sources, and an agent approves or edits it.

**Design:**
- **Ingestion:** a webhook or polling job from the help desk; store tickets in Postgres.
- **Classification:** an LLM call with a clear prompt, a fixed list of categories and structured output (JSON schema), plus a confidence or "unsure" option. Low confidence goes to a human queue.
- **Reply drafting with RAG:** embed help-centre articles and past resolved tickets; retrieve the top matches and ask the model to answer only from them, citing sources.
- **UI:** a simple queue showing category, urgency, draft reply and sources, with approve, edit and reject buttons.
- **Feedback loop:** store agent edits and corrections as labelled data.

**Quality:** a labelled eval set of 100 past tickets; measure classification accuracy and review draft quality with a rubric; run evals on every prompt change.

**Production concerns:** retries and timeouts on the model API, cost per ticket, logging prompts and responses with personal data handling in mind, and a manual fallback when the model is down.

**Show in the demo:** the working flow, eval numbers and what you would improve next. See [06 AI / LLM](../../topics/06-ai-llm/ANSWERS-part2.md#j-take-home--build-exercises-reported).

#### How do you present and defend your take-home in the follow-up review?

**Prepare a 10-minute walkthrough:**
1. The problem and the user, in one sentence.
2. A live demo of the main flow.
3. Architecture in one simple diagram, and the two or three key decisions with their trade-offs.
4. What you cut and why, and what you would do next.

**During questions:**
- **Know every line of your code,** including anything generated with AI tools.
- **Defend decisions with reasons, not stubbornness:** "I chose X because of Y; if Z were true I'd choose differently."
- **Agree openly when they find a real flaw,** and say how you would fix it. Founders are testing whether you are coachable and honest, not whether the code is perfect.
- **Expect extension questions:** "How would this handle ten times the users?", "What breaks first?", "How would you add a feature?" Answer with the simplest change that works.
- **Expect product questions:** who the user is, why this flow, how you would measure success.

End by asking for their feedback; it signals that you want to learn and gives you information about how they work.

#### How do you approach a paid work trial with the team?

Some startups replace part of the loop with a paid trial of a few days to a week of real work. It tests how you work, not just what you know, and lets you test them too.

**Before:** clarify the scope, expected hours, pay, access (code, tools, data), who you will work with and how the decision will be made. Check that your current employment contract allows it and avoid any conflict of interest.

**During:**
- **Ship something real early,** even small, in the first day or two. Momentum matters.
- **Ask good questions quickly** instead of being stuck silently; also show you can find answers yourself.
- **Communicate:** a short written update each day (what I did, what is next, what is blocking me).
- **Show product sense:** talk to a user or join a customer call if you can, and suggest improvements you notice.
- **Be a good teammate:** review others' code, follow team conventions, be easy to work with.

**Evaluate them as well:** how decisions are made, code quality, working hours, how founders treat people under pressure.

**End:** a short summary of what you shipped, what you learned and what you would do next, then ask for honest feedback.

### Customers and owning everything

#### A large prospective customer asks for a custom feature that does not fit the product. What do you do?

**Understand before deciding:** join the call or ask the founder for details. What problem is the feature solving? How important is it to the deal? Is it a blocker or a wish? How many other customers might need the same thing?

**Options, from best to worst usually:**
1. **Solve it with the existing product:** configuration, a workaround, or an integration.
2. **Generalise it:** build a version that serves this customer and fits the product direction (for example a configurable rule instead of a hard-coded behaviour).
3. **Build an extension point:** an API, webhook or export so the customer (or a partner) can build it themselves.
4. **Build a one-off,** only with clear eyes: time-boxed, isolated from the core, paid for if possible, and with an agreed maintenance plan.
5. **Say no,** if it would pull the product away from its direction.

**Decide with the founders,** making the cost explicit: engineering weeks, opportunity cost, long-term maintenance and the precedent set. Early revenue from a large logo can be worth a detour, but a startup that builds every custom request becomes a consulting company.

Communicate the decision quickly and honestly to the customer.

#### On a sales call with the founder, the customer asks a technical question you cannot answer. What do you do?

**Be honest and specific:** "I don't want to guess on that. Let me check and get back to you by tomorrow." Guessing wrong on a technical or security question damages trust far more than a short delay.

**Show you understood the question:** restate it, and ask a follow-up to learn why they are asking (for example a compliance requirement or an integration they depend on). The reason often matters more than the answer.

**Answer what you can:** "Here's what I know today: we do X; I'll confirm whether Y is supported."

**Follow up on time:** write down the exact question, find the answer (code, docs, vendor), and send a clear written response, copying the founder.

**Afterwards:** if the question is likely to come up again, add it to an FAQ or security document, and if it reveals a product gap, raise it.

Also agree roles with the founder before calls: who leads, who answers technical questions, and a signal for "let me take this one".

#### How would you personally onboard the first ten customers?

**Treat the first ten as a learning program, not just onboarding.**

- **Before go-live:** a kickoff call to understand their goals, current workflow, success criteria and data. Agree on what "success in 30 days" means for them.
- **Do it with them, not for them:** set up the product on a shared call, connect their data, and get them to the first meaningful result in the first session if at all possible.
- **Watch closely:** a shared chat channel, usage tracking per account, and weekly check-ins at first.
- **Fix fast:** bugs and small requests found by the first customers get fixed in days, which builds loyalty and improves the product for everyone.
- **Record everything:** where they got stuck, what they asked, what they never used. Look for patterns across the ten.
- **Turn manual steps into product:** after doing the same onboarding step three times by hand, automate or simplify it.
- **Ask for outcomes:** after 30 days, review results against their success criteria, ask for a reference or case study, and ask what would make them recommend it.

This is close to forward deployed work: the first customers teach you what the product must be.

#### A customer says the product gave a wrong answer and they are angry. What do you do in the next 24 hours?

**First hour:**
- Respond quickly and personally: acknowledge the impact, do not argue, say you are investigating and when they will hear back.
- Get specifics: the input, output, time, and what they expected. Find the request in logs or traces.

**Investigate (same day):**
- Reproduce it. For an AI feature, check what the model received: retrieved context, prompt version, model version, and any recent change.
- Classify the cause: bad or missing data, retrieval failure, prompt or model behaviour, a bug in surrounding code, or a misunderstanding of what the product does.
- Check whether other customers are affected.

**Mitigate:** fix the data, roll back a prompt or model change, add a guardrail, or turn off the feature for that case.

**Communicate (within 24 hours):** a clear explanation of what happened, what was fixed, and what will prevent it. Founders should be informed early, and may want to call the customer themselves.

**Prevent:** add the case to the evaluation set, add monitoring for that failure type, and improve how the product shows uncertainty or sources.

Handled well, an incident like this often increases a customer's trust.

#### How do you handle security questionnaires and enterprise requirements such as SSO and SOC 2 at a seed-stage company?

**Get the basics right early, because they are cheap now and expensive later:** SSO-ready auth (a managed provider), encryption in transit and at rest, least-privilege cloud access, secrets management, audit logs, backups, dependency updates, and MFA for the team.

**Questionnaires:**
- Build a reusable security document and a library of standard answers, kept honest and current.
- Answer truthfully; "planned for Q3" is acceptable, a false "yes" is not and can create contractual risk.
- Offer a call with the customer's security team; it often resolves more than the form.

**SSO and SCIM:** add SAML or OIDC SSO when the first enterprise customer needs it; a managed auth provider makes this quick.

**SOC 2:** customers increasingly ask for it. A common path is to use a compliance automation platform, implement the controls, get a SOC 2 Type I report first (controls designed at a point in time) and then Type II (controls operating over a period, often several months). Start when enterprise deals depend on it; it takes months and has real cost.

**Data and AI questions:** be ready to explain which model providers process customer data, retention, and whether data is used for training.

### System design for startups

#### Design the first version of a multi-tenant backend for an AI product, then explain what changes at 100 times the usage.

**Version 1 (a few customers, small team):**
- **One application** (modular monolith) behind a managed load balancer, deployed on a managed platform.
- **One managed PostgreSQL database** with a `tenant_id` on every tenant-owned table. Enforce isolation in one place: a data-access layer that always filters by tenant, or Postgres row-level security as a second safeguard.
- **Vector search** with pgvector in the same database, filtered by tenant.
- **Background jobs** (document ingestion, embeddings, long LLM tasks) on a simple queue with retries.
- **LLM calls** through an internal wrapper: timeouts, retries, logging, per-tenant usage tracking and cost limits.
- **Object storage** for uploaded files, with per-tenant prefixes.
- **Auth** through a managed provider, with organisations and roles.

**At 100 times the usage:**
- Database: read replicas, connection pooling, indexes tuned on real query patterns, partitioning large tables; move vectors to a dedicated store only if pgvector becomes the bottleneck.
- Separate worker pools by job type so a large ingestion does not block interactive requests.
- Per-tenant rate limits and fair queuing to handle noisy neighbours.
- Caching for repeated LLM and retrieval calls; model routing to cheaper models where quality allows.
- Dedicated deployments or databases for large enterprise tenants who need isolation or data residency.

Say explicitly that you would measure before scaling each part.

#### Design an LLM-powered feature with evaluation, cost control and fallbacks that one engineer can build in two weeks.

**Example feature:** "summarise this customer's account history and suggest next steps" inside a CRM-like product.

**Week 1: build the slice.**
- Collect inputs (notes, emails, tickets) for an account, trimmed to fit the context window, newest and most relevant first.
- Prompt with clear instructions, a structured output schema (summary, risks, next steps, cited sources) and examples.
- Show the result in the UI with sources and a feedback button (useful or not, with an optional comment).
- Behind a feature flag for internal users.

**Week 2: make it reliable.**
- **Evaluation:** 50–100 real accounts with reference summaries or a rubric (accuracy, coverage, no invented facts); an LLM judge checked against a sample of human grades; run on every prompt change.
- **Cost control:** cache results until new data arrives, use a cheaper model for long-input compression and a stronger one only for the final step, cap tokens per request, and track cost per tenant.
- **Fallbacks:** timeouts and retries, a second model provider or a smaller model if the primary fails, and a plain "unable to summarise" message instead of an error page.
- **Observability:** log prompts, outputs, latency, tokens and feedback.

Roll out to a few customers, watch feedback and cost, then expand.

#### Design usage metering and billing for an early usage-based AI product.

**Principles:** usage data must be accurate, auditable and never double-counted; use a billing provider instead of building invoicing.

**Design:**
- **Define billable units** customers understand and you can measure exactly, for example documents processed or tasks completed, rather than raw tokens, which customers cannot predict. Track tokens internally for cost and margin.
- **Usage events:** every billable action writes an event (tenant, unit, quantity, timestamp, idempotency key) to an append-only table in the same transaction as the work, or from a reliable queue.
- **Aggregation:** a scheduled job sums usage per tenant per period and sends it to the billing provider (Stripe Billing and similar tools support usage-based pricing), using idempotency keys so retries do not double-bill.
- **Plans and limits:** included quotas, overage pricing, hard or soft limits, and alerts to customers at 80% and 100%.
- **Customer visibility:** a usage dashboard matching the invoice.
- **Reconciliation:** compare internal usage totals with billed amounts each period; investigate gaps.

**Watch margins:** compare revenue per unit with model and infrastructure cost per unit per tenant. Usage-based AI products can lose money on heavy users if prices do not track model costs.

### Founder interview and working style

#### What would you do in your first 30 days here?

**What they look for:** that you will be productive quickly, learn the customer, and not try to rewrite everything.

**Shape of a good answer:**
- **Week 1: learn and ship something small.** Set up the environment, read the code, ship a small fix or feature to production in the first days. Join customer calls and read support messages.
- **Weeks 2–3: own a meaningful piece.** Take one important feature or problem from the founders' priority list and ship it end to end, talking to the users it affects.
- **Throughout: listen to customers.** Talk to several users directly and share what you learn.
- **Week 4: share observations and a proposal.** A short written note: what is working, the top risks you see (reliability, data model, security, tooling), and two or three improvements you suggest, prioritised by impact, not by your preferences.

**Tailor it:** reference something specific about their product, stage and stack, ideally from using the product yourself. Avoid promising a big rewrite or new processes before you understand why things are the way they are.

#### How do you work with a non-technical founder, or with a technical founder who still writes code?

**Non-technical founder:**
- Translate technical decisions into business terms: time, risk, cost and what users will notice.
- Give options with trade-offs rather than a single answer ("fast and limited by Friday, or complete in two weeks").
- Make progress visible: short demos every week rather than status reports.
- Respect their domain and customer knowledge; they often know the users better than you do.
- Earn trust by delivering what you estimate, and flag slips early.

**Technical founder who still codes:**
- Agree ownership areas so you do not collide, and keep code review mutual.
- Disagree on technical choices with evidence, then commit to the decision.
- Accept that they may change direction quickly; ask for the reason so you can make good decisions without them.
- Help them step back from code over time, by owning areas fully and reliably.

**In both cases:** regular one-to-one time, written decisions for important topics, and directness. Founders need someone who will tell them what they do not want to hear.

#### How would you help hire the next engineers and set the engineering culture?

**Hiring:**
- Define what the next hire must do in their first six months, and hire for that, not a generic profile.
- Design a short, practical loop that resembles the real work: a paid or short build exercise, a past-project deep dive, and time with founders. Respect candidates' time.
- Use a consistent scorecard and debrief, so decisions do not depend on mood.
- Use your network and the team's; early hires come mostly from referrals and direct outreach.
- Sell honestly: the mission, the scope, and the risks.

**Culture (set by what you do, not what you write):**
- Ship small and often; everyone deploys and is on call.
- Everyone talks to users.
- Write things down: short design notes for important decisions, and postmortems without blame.
- Code review for learning and risk, not gatekeeping.
- A few simple standards: tests on critical paths, CI, readable code.
- Direct, kind feedback, starting with yourself.

Early hires and habits shape the company for years, so it is worth being deliberate even at five people.

### Behavioral (role-specific)

#### Tell me about something you built and shipped end to end on your own.

**What they look for:** proof you can own everything: understanding the user, design, build, deploy, operate and iterate, without someone else filling the gaps.

**STAR outline:**
- **S:** [The problem and who had it]
- **T:** [What you decided to build and the constraints: time, resources, nobody else to help]
- **A:** [How you scoped it, the stack and key decisions, how you deployed and monitored it, how you got feedback from users and what you changed]
- **R:** [Usage or impact numbers, what happened next, what you would do differently]

**Example (adapt to your own experience):** "Our operations team spent hours each week reconciling records between two systems. I talked to two of them to understand the process, built a small service that compared the systems nightly and listed mismatches with suggested fixes, and deployed it with alerts. After the first week I added one-click fixes for the most common case, based on their feedback. Manual reconciliation time dropped substantially, and the tool became part of the team's daily routine."

#### Tell me about a time you shipped something quickly that turned out to be wrong.

**What they look for:** you are comfortable shipping fast, you notice quickly when you are wrong, you own it, and you learn something specific. Founders want speed and judgement, not fear of mistakes.

**STAR outline:**
- **S:** [The context and why speed mattered]
- **T:** [What you shipped and the assumption you made]
- **A:** [How you found out it was wrong (data, users, incident), how you responded, what you changed]
- **R:** [The outcome, the cost of the mistake, and what you now do differently]

**Example (adapt to your own experience):** "We shipped a bulk-import feature in two days to unblock a customer, assuming their files would match our template. Within a week other customers' files with different column orders caused silent mapping errors. I added validation with a preview step before import, fixed the affected records with the customers, and since then I add validation and a dry-run mode to anything that writes data in bulk, even in a rushed first version."

#### Tell me about a time you worked with high ambiguity and no clear owner.

**What they look for:** bias for action: you defined the problem, made reasonable assumptions, took ownership, and moved forward while keeping people informed.

**STAR outline:**
- **S:** [The unclear situation: vague goal, missing owner, conflicting signals]
- **T:** [Why it mattered and why someone needed to act]
- **A:** [How you clarified the goal, gathered information quickly, made and wrote down assumptions, took ownership, shipped a first step and adjusted]
- **R:** [What changed, measurable impact, and how the work was owned afterwards]

**Example (adapt to your own experience):** "Customers kept reporting slow page loads, but no team owned performance. I measured the slowest pages, found most of the time came from a few unindexed queries and a large unpaginated API response, and shared a one-page plan with the leads. I fixed the top three issues in a week and added a weekly performance report. Page load times on the worst pages dropped noticeably, and performance became an explicit part of our team's goals."

See also [Ambiguity & bias for action](../../topics/10-behavioral/ANSWERS.md#ambiguity--bias-for-action).

#### Tell me about a time talking to users changed what you built.

**What they look for:** you go to users directly, you listen for the real problem behind requests, and you are willing to change your plan when evidence says so.

**STAR outline:**
- **S:** [The feature or product and the original plan]
- **T:** [Why you talked to users and who you talked to]
- **A:** [What you asked, what you learned that surprised you, how you changed the plan, and how you validated the new direction]
- **R:** [The outcome compared with the original plan: adoption, time saved, work avoided]

**Example (adapt to your own experience):** "We planned a detailed reporting dashboard for finance users. Before building, I sat with three of them while they did their month-end work and saw they mostly exported data to spreadsheets anyway. They needed one reliable scheduled export with the right fields, not a dashboard. We built the export in a few days instead of a multi-week dashboard, it was used every month, and we reused the time on a feature customers had been waiting for."

---

## Prep checklist

1. Write and rehearse your "why a startup, why this company" answer, and be honest with yourself about the financial risk you can take.
2. Build and deploy one small full-stack AI app end to end (auth, database, LLM call with evals, UI, monitoring) in a public repository; it is practice for the take-home and your portfolio.
3. Do one timed take-home practice (4–8 hours) from a short brief, including a README and a two-minute demo video; ask a friend to review it as a founder would.
4. Practise a 90-minute live build in an unfamiliar open-source codebase: get it running, find a similar feature, and add a small one.
5. Prepare two product-sense answers (a product you would change, and a prioritisation example) and read *The Mom Test* for user interview technique.
6. Review the AI sections most likely to come up: [RAG](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation), [Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important) and [Production LLM engineering](../../topics/06-ai-llm/ANSWERS-part2.md#g-production-llm-engineering).
7. Practise startup-style system design: always present the simplest version first, then what changes with scale.
8. Use each target company's product, read its public material and founders' posts, and prepare specific suggestions.
9. Prepare due-diligence questions about runway, traction, team and equity terms using [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md).
10. Prepare STAR stories for end-to-end ownership, fast mistakes, ambiguity and user conversations in [10 Behavioral](../../topics/10-behavioral/ANSWERS.md).
11. Follow the [study plan](STUDY-PLAN.md) for a week-by-week schedule.
