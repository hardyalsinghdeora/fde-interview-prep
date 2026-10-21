# Solutions Engineer / Sales Engineer / Pre-sales Engineer

The technical half of a sales team: you run discovery, demos and proofs of concept, answer every technical question in a deal, and secure the "technical win" so the account executive can close.

## The role

**Day to day.** You are paired with one or more account executives (AEs). A typical week mixes discovery calls, tailored demos, POC/pilot work (building sample integrations, helping the prospect's engineers get to first success), answering RFPs and security questionnaires, writing technical follow-ups, building demo assets, and feeding field feedback to product. You rarely ship production code for the customer; you prove that the product solves their problem, then hand over to post-sales (customer success, professional services, or an FDE/implementation team).

**Common titles.** Solutions Engineer, Sales Engineer, Pre-sales Engineer, Pre-sales Consultant, Technical Account Executive (rare), Customer Engineer (Google Cloud's name for pre-sales), and at some companies "Solutions Architect" is used for the same pre-sales job. Seniority: SE, Senior SE, Principal SE, SE Manager / Director of Solutions Engineering.

**Who hires for it.** Almost every B2B software company with an enterprise sales motion. Well-known examples: Stripe, Snowflake, Datadog, MongoDB, Databricks, Salesforce, Elastic, HashiCorp, Okta, Twilio, Cloudflare, and AI labs such as OpenAI and Anthropic (titles there vary between Solutions Engineer, Solutions Architect and Applied AI roles; check each posting). Exact titles and team structures change, so read the job description.

**How it differs from FDE.**

| | Solutions Engineer | Forward Deployed Engineer |
|---|---|---|
| When in the lifecycle | Before the contract (pre-sales) | After the contract (deployment) |
| Main goal | Technical win, then closed revenue | Customer outcome in production |
| Output | Demos, POCs, answers, proposals | Production code, pipelines, integrations |
| Time per customer | Days to weeks, many deals in parallel | Weeks to months, few customers |
| Measured on | Team quota, win rate, technical wins | Deployment success, adoption, expansion |
| Compensation | Usually base + variable tied to sales target (often around 70/30 or 80/20, varies) | Usually salary + equity, little or no sales variable |

**Who it suits.** Engineers who enjoy explaining more than building, like variety and pace, are comfortable with a number attached to their work, and can stay calm with a skeptical audience. Former developers, consultants, support engineers and data engineers commonly move into it.

**Skills mix.**

| Skill | Weight |
|---|---|
| Coding (scripts, sample apps, API integrations) | Med |
| System design / architecture | Med |
| Customer communication and presenting | High |
| Sales process (qualification, deal strategy) | High |
| Product depth (your product and competitors) | High |
| Production engineering / on-call | Low |

**Career paths.** Senior/Principal SE (bigger, more complex deals; specialist SE for one product area), SE management, solutions architecture, product management (field insight is valued), developer relations, moving into the AE role, or founding a company (SEs see many customer problems).

## Typical interview loop

Loops vary by company; this is the typical reported shape, not a guarantee.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Motivation for pre-sales, comfort with variable comp and travel, product interest | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| Hiring manager (SE manager) | Past deals, how you work with AEs, sales-process fluency | This file (sales process, behavioral) + [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |
| Technical screen | Product-domain fundamentals (APIs, databases, cloud, observability, payments, LLMs depending on company); sometimes a light coding or SQL exercise | [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/ANSWERS.md), [07-data-and-sql](../../topics/07-data-and-sql/ANSWERS.md), [02-coding](../../topics/02-coding/ANSWERS.md) |
| Discovery role-play | Questioning, listening, qualification | This file (Discovery) + [09-customer-scenarios](../../topics/09-customer-scenarios/ANSWERS.md) |
| Demo / presentation round (often the deciding round) | Storytelling, tailoring, handling interruptions and questions; commonly you demo their product after a few days with a trial account, or a product you know | This file (Demo) |
| Objection handling / panel with AE | Composure, honesty, competitive positioning | This file (Objections, Competitive) |
| Architecture or POC exercise (some companies) | Designing a customer solution, scoping success criteria | [03-system-design](../../topics/03-system-design/ANSWERS.md) + this file (POC) |
| Values / cross-functional | Collaboration with AE, CS, product | [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |

## Common rounds — use the shared sections

- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): highest priority. Scoping, discovery, "demo breaks", "sales promised something", competitor comparisons, security review blocking. Most of the role-play mechanics are there.
- [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md): APIs, auth (OAuth, SSO, SCIM), webhooks, compliance acronyms and security questionnaires come up in every technical deep dive.
- [01 Recruiter & motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md): adapt "why this role" to pre-sales; be ready to say why you want a quota-carrying team.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md): influence without authority, conflict, failure stories; reframe them around deals.
- [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md): essential if you are interviewing at an AI vendor (evals, RAG, safety, pricing by tokens).
- [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md): for data-platform vendors (Snowflake, Databricks, MongoDB).
- [03 System design](../../topics/03-system-design/ANSWERS.md): for architecture-style rounds; keep it at reference-architecture depth rather than deep internals.
- [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md): add SE-specific ones (SE:AE ratio, how technical wins are tracked, quota attainment of the team).

## Role-specific questions

### Role and motivation
- [ ] Why solutions engineering rather than software engineering or forward deployed engineering?
- [ ] What separates a great solutions engineer from an average one?
- [ ] How do you divide responsibilities with the account executive on a deal?
- [ ] What metrics should a solutions engineer be measured on?

### Discovery
- [ ] Discovery role-play: a VP of Engineering at a mid-size retailer has agreed to a 30-minute call. Run the discovery.
- [ ] What questions do you ask to uncover pain, business impact and the current technical state?
- [ ] How do you technically qualify an opportunity, and when would you recommend walking away?
- [ ] Explain MEDDPICC and where the solutions engineer contributes most.
- [ ] How do you find and develop a technical champion?

### Demo and presentation
- [ ] Demo round: you have 20 minutes to demo our product to a technical buyer. How do you structure it?
- [ ] How do you tailor a demo to what you learned in discovery?
- [ ] Your demo environment fails in the middle of a live demo. What do you do?
- [ ] How do you demo to a mixed audience of executives and engineers?
- [ ] How do you build and maintain demo environments?
- [ ] Explain our product's core technical idea to a non-technical buyer in two minutes.

### Proof of concept and technical win
- [ ] How do you scope and run a proof of concept so that it ends in a technical win?
- [ ] Write success criteria for a proof of concept of an observability product.
- [ ] The prospect asks for a free, open-ended 90-day pilot. How do you respond?
- [ ] A proof of concept is going badly at the halfway point. What do you do?
- [ ] What is a technical win and how do you know you have it?
- [ ] Hands-on exercise: build a small integration with our API in an hour and walk us through it.

### Objection handling
- [ ] What framework do you use to handle objections?
- [ ] Objection role-play: "This is too expensive. We could build it ourselves."
- [ ] Objection role-play: "We already use your competitor. Why would we switch?"
- [ ] Objection role-play: "Your product will not scale to our volume."
- [ ] How do you handle a technical objection you cannot answer on the spot?

### Competitive positioning
- [ ] How do you position against a competitor without criticising them?
- [ ] How do you build and use a competitive battlecard?
- [ ] The prospect sends a feature comparison spreadsheet written by a competitor. How do you handle it?

### RFPs, security and procurement
- [ ] How do you approach an RFP, and do you respond to every one?
- [ ] How do you handle a 300-question security questionnaire?
- [ ] The prospect asks whether you are SOC 2 Type II, HIPAA and GDPR compliant and where their data will be stored. How do you answer?
- [ ] Procurement or legal is stalling a deal after the technical win. What can the solutions engineer do?

### Sales process and collaboration
- [ ] Walk through the stages of a typical enterprise sales cycle and the solutions engineer's role at each.
- [ ] How do you prioritise across many deals with limited time?
- [ ] The account executive wants to promise a roadmap feature that is not committed. What do you do?
- [ ] How do you hand over a closed deal to post-sales?
- [ ] How do you get field feedback turned into product changes?
- [ ] How do you build a business value or ROI case that the economic buyer will accept?

### AI vendor specifics
- [ ] How do you handle a prospect's concerns about hallucination, data privacy and training on their data when selling an AI product?
- [ ] How would you run a bake-off where a prospect compares our LLM API against another provider?
- [ ] A prospect asks what our LLM API will cost them per month. How do you estimate it?

### Role-specific behavioral
- [ ] Tell me about a deal where your technical work made the difference.
- [ ] Tell me about a deal you lost and what you learned.
- [ ] Tell me about a time you had to say no to a customer or to your account executive.

## Model answers

### Role and motivation

#### Why solutions engineering rather than software engineering or forward deployed engineering?

Give three honest reasons and tie each to evidence from your past.

- **I like the moment of translation.** "The part of engineering I enjoy most is understanding someone's problem and showing them how technology solves it. In my last role I [ran internal demos / wrote integration guides / joined customer calls] and that was the work I looked forward to."
- **Breadth and pace.** An SE sees many customers, industries and architectures in a quarter. That suits me better than going deep on one codebase.
- **Direct line to impact.** A technical win is measurable and tied to revenue; I like seeing a clear result.

Versus FDE: "FDEs own long deployments after the sale. I want to be earlier in the cycle, shaping what the customer buys and why. I still want to stay hands-on: build POCs, write sample code, go deep on the product."

Avoid "I'm tired of coding" or "I want to earn commission" as the lead reason. It is fine to say you are comfortable with variable compensation tied to a team target.

#### What separates a great solutions engineer from an average one?

- **Discovery over demo.** Average SEs show features; great SEs spend the first meetings understanding the business problem and then show only what matters.
- **They control the technical process.** They set POC success criteria in writing up front instead of letting evaluations drift.
- **Credibility through honesty.** They say "no, we don't do that, here is the workaround" quickly. Buyers trust them, so their "yes" carries weight.
- **They multiply the AE.** They know the deal strategy, the decision process and the competition, not just the product.
- **Depth on demand.** They can go from an executive pitch to a packet capture or an API trace in the same meeting.
- **They scale themselves.** Reusable demos, templates, internal docs, and product feedback that actually lands.
- **They know when to walk away** from a deal that is a bad fit, protecting time and the company's reputation.

#### How do you divide responsibilities with the account executive on a deal?

The AE owns the deal: relationship with the economic buyer, pricing, commercial terms, forecast, and overall strategy. The SE owns the technical side: technical discovery, solution design, demos, POC, security and architecture questions, and getting the technical win.

In practice it is a partnership:
- **Before each call** we agree the goal, who leads which part, and what we want the customer to commit to next.
- **During calls** the AE frames the business context; I take technical questions. I flag business signals I hear (budget, timeline, competitors) and the AE flags technical risks.
- **After calls** we debrief and update the CRM together (technical win status, risks, next steps).
- **Shared deal plan** using a framework such as MEDDPICC; I own the technical fields (decision criteria, technical champion, technical risks).

If we disagree, for example on overpromising, I raise it privately and early, not in front of the customer.

#### What metrics should a solutions engineer be measured on?

Most SE teams use a mix of outcome and activity metrics:

- **Team or pooled quota attainment**: revenue closed by the AEs you support. This drives most variable pay.
- **Technical win rate**: percentage of opportunities where you secured the technical win, and how often a technical win converts to a closed deal.
- **POC conversion rate** and POC cycle time.
- **Deal velocity**: time from first technical meeting to technical win.
- **Losses by reason**: how many deals were lost on technical grounds versus price or timing.
- **Activity indicators**: demos, POCs, RFPs completed; used as context, not as goals.
- **Post-sale health**: whether customers you sold are adopting and renewing, which shows you sold a real fit.
- **Enablement and assets**: reusable demos, docs, product feedback adopted.

Good answer to add: "I'd want to be judged mainly on technical win rate and quality of the fit, because activity metrics reward busy work."

### Discovery

#### Discovery role-play: a VP of Engineering at a mid-size retailer has agreed to a 30-minute call. Run the discovery.

Structure out loud, then do it.

1. **Open (2 min):** thank them, confirm time, state the agenda: "I'd like to understand your goals and current setup, then decide together if a deeper session makes sense. OK?"
2. **Situation (5 min):** "Tell me about your platform today: stack, teams, scale." "What prompted you to take this call now?"
3. **Pain (8 min):** "What breaks or slows you down most?" "When did that last happen and what did it cost?" Dig with "Can you give me an example?" and "What happens if you do nothing?"
4. **Impact (5 min):** quantify: hours lost, revenue at risk during peak season, engineer headcount. "How does this affect your goals for this year?"
5. **Decision context (5 min):** "Who else cares about this? How have you bought tools like this before? What would a good outcome by Q4 look like? What else are you evaluating?"
6. **Close (5 min):** summarise in their words, confirm accuracy, propose a next step with a date (tailored demo with their architect).

Talk less than 30% of the time. Do not demo or pitch in the first call.

#### What questions do you ask to uncover pain, business impact and the current technical state?

**Current state:** What does the architecture look like today? What tools are you using for this? How many users, services, transactions, data volume? What cloud and deployment model? What integrations must this work with?

**Pain:** What is not working? When did it last cause a problem? How do you handle it today, and what does that workaround cost? Who is most affected?

**Impact:** What does that cost in time, money, risk or customers? How is this tied to a company goal or initiative this year? What happens if nothing changes in 12 months?

**Future state:** If this were solved, what would be different? How would you measure success?

**Decision:** Who else is involved? What are your must-haves versus nice-to-haves? What has stopped you solving this before? What is your timeline and why?

Use open questions, follow up on vague words ("slow", "a lot"), and ask for numbers. Summarise back and confirm.

#### How do you technically qualify an opportunity, and when would you recommend walking away?

Technical qualification answers: can we solve their problem well, and can they succeed with us?

Check:
- **Fit:** the core use case matches what the product does well, not edge features.
- **Environment:** deployment model, cloud, data residency, integrations and scale are supported.
- **Must-haves:** any hard requirement we cannot meet (on-prem only, a certification we lack, a missing connector).
- **Skills and resources:** do they have people to implement it?
- **Access:** can we reach the technical decision-makers and real data for a POC?

Recommend walking away (or deprioritising) when: a hard requirement is unmet and not on the roadmap; the evaluation is clearly a "column fodder" exercise where a competitor is already chosen; they will not define success criteria or give access; or the use case would lead to a failed deployment and churn. Explain it to the AE with evidence, suggest what would change your view, and keep the relationship warm.

#### Explain MEDDPICC and where the solutions engineer contributes most.

MEDDPICC is a qualification framework for complex B2B deals:

- **M**etrics: the quantified business outcome the buyer expects.
- **E**conomic buyer: the person who can approve the spend.
- **D**ecision criteria: the technical and business criteria used to choose.
- **D**ecision process: the steps and people involved in deciding.
- **P**aper process: legal, procurement, security review, contract steps.
- **I**dentify pain: the business problem driving urgency.
- **C**hampion: an insider with influence who sells for you.
- **C**ompetition: alternatives, including build-it-yourself and doing nothing.

The SE contributes most to **Decision criteria** (shaping them toward your strengths, ideally writing them into POC success criteria), **Metrics** (building the business case from technical impact), **Identify pain** (technical discovery), the **technical Champion**, and **Competition** (technical differentiation). The SE also de-risks the **Paper process** by handling security reviews early.

Related frameworks you may hear: BANT, MEDDIC (without the second P and C), SPICED, Command of the Message.

#### How do you find and develop a technical champion?

A champion has influence, has personal interest in the outcome, and will sell for you when you are not in the room. A friendly contact who has no influence is a "coach", not a champion.

**Finding one:** look for the engineer or architect who feels the pain most and asks detailed questions; check their influence by asking "How would you get this approved?" and seeing if they can get you meetings with others.

**Developing them:**
- Make them successful: give fast responses, hands-on help in the POC, early access, direct line to you.
- Tie the project to their personal win (promotion, less on-call pain, a visible initiative).
- Equip them: one-page summaries, architecture diagrams, the business case in their company's language, answers to likely internal objections.
- Test them: ask them to do something (set up the executive meeting, share the decision process). A champion who will not act is not a champion.

Keep more than one champion where possible, so the deal survives if one leaves.

### Demo and presentation

#### Demo round: you have 20 minutes to demo our product to a technical buyer. How do you structure it?

Clarify the scenario first: who is the persona, what was learned in discovery, what do they care about. If not given, state your assumptions.

Structure (a common approach is "tell-show-tell"):
1. **Context (2 min):** recap their situation and pain in their words. "You said incidents during peak take 3 hours to diagnose."
2. **Agenda and goal (1 min):** "I'll show three things that address that, then leave time for questions."
3. **Lead with the outcome (2 min):** show the end result first (the dashboard that pinpoints the failing service), not the setup screens.
4. **Three key capabilities (10 min):** each tied to a stated pain; for each, say why it matters, show it, then say what it means for them.
5. **Technical depth on request (3 min):** API, architecture, security, how it integrates with their stack.
6. **Summary and next step (2 min):** recap value, confirm it addresses their problem, propose a POC or deeper session.

Check in often ("Is this how your team would work?"), keep a backup recording, and resist showing every feature.

#### How do you tailor a demo to what you learned in discovery?

- **Use their data or a lookalike:** their industry, their naming, realistic volumes. A retailer sees orders and checkout services, not "foo-service".
- **Follow their pain ranking:** the first thing you show solves the top pain they named.
- **Mirror their workflow:** walk through how their on-call engineer, analyst or developer would actually use it, end to end.
- **Use their stack:** show the integrations they use (their cloud, their CI, their identity provider).
- **Cut what they don't need:** fewer features, deeper.
- **Persona-specific framing:** executives get outcomes and metrics; engineers get APIs, config and edge cases.
- **Prepare for their objections:** if they mentioned scale concerns, show a large-volume environment.

Confirm the agenda with your champion before the meeting: "These are the three things I plan to show. Anything I'm missing?"

#### Your demo environment fails in the middle of a live demo. What do you do?

Prevent first: rehearse, use a stable dedicated environment, test an hour before, have a backup recording, screenshots, and a second environment or account.

When it fails:
1. **Stay calm and name it briefly:** "That's not what should happen; give me 30 seconds."
2. **One quick attempt at recovery** (refresh, re-login, switch to the backup environment). Do not debug live for minutes.
3. **Switch to the backup:** recorded video or slides of the same flow, and keep narrating the value.
4. **Turn it into credibility if possible:** if it is a real error, briefly show how the product reports and handles errors, which buyers often appreciate.
5. **Follow up:** offer a live session within a day or two, send a recording, and explain the cause if it relates to the product.

Afterwards, do a short post-mortem and fix the root cause in the demo environment. See also [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md).

#### How do you demo to a mixed audience of executives and engineers?

- **Before the meeting:** learn who is attending and what each cares about; agree with your champion what each person needs to see.
- **Start with the executive story:** problem, outcome, business impact, in under five minutes. Executives often leave early, so give them what they need first.
- **Then layer depth:** "For the engineering team, here is how it works under the hood." Keep the deep dives short and offer a separate technical session for detailed questions.
- **Translate as you go:** after each technical point, state the business meaning ("this means fewer escalations during peak").
- **Manage the room:** if an engineer pulls you into a deep detail, answer briefly and park the rest: "Great question; let's cover that in the architecture session on Thursday."
- **End with a summary for both:** business outcome plus the technical next step.

#### How do you build and maintain demo environments?

- **Story first:** design the environment around 2–3 standard customer stories per segment or industry, with realistic data.
- **Reproducible:** infrastructure as code (Terraform, scripts, seed-data generators) so anyone on the team can rebuild it. Store in version control.
- **Live-looking data:** scripts that generate continuous traffic or events so dashboards are not empty or stale.
- **Isolated and stable:** dedicated accounts or tenants, pinned versions; a separate sandbox for trying new features.
- **Reset button:** a script that returns the environment to a known state after each demo.
- **Monitoring and ownership:** health checks, an owner for each environment, a schedule for updating after product releases.
- **Security:** no real customer data, no shared personal credentials, rotate keys.
- **Backups:** recorded walkthroughs of each story in case the live environment fails.

#### Explain our product's core technical idea to a non-technical buyer in two minutes.

Before the interview, prepare this for the specific company. Structure:

1. **The problem in their world** (20 sec): a familiar pain, no jargon.
2. **The idea in one sentence** (20 sec), with an analogy.
3. **How it works, simply** (40 sec): three steps maximum.
4. **What it means for them** (30 sec): outcome and a number.
5. **Check understanding** (10 sec).

Example for an observability product (adapt to the company): "When your website slows down, your engineers currently search through many separate systems to find out why, like trying to find a fault in a building by checking every room. Our product puts sensors in every room and draws one map showing where the problem started. It collects signals from your servers and applications, connects them, and alerts the right person with the likely cause. Teams typically use this to cut the time to fix incidents, which matters most on your busiest sales days. Does that match the problem you described?"

Avoid unverified claims; use the company's published case-study numbers if you cite any.

### Proof of concept and technical win

#### How do you scope and run a proof of concept so that it ends in a technical win?

A POC should confirm a decision, not start one. Before agreeing to it:

1. **Qualify:** is there a real budget, timeline and decision? Can a demo or trial answer the question instead?
2. **Write success criteria together** (3–5, measurable, tied to their pain), and get the decision-maker to agree: "If we meet these, will you move forward with us?"
3. **Fix scope and time:** typically 2–4 weeks, a specific use case, specified data and environments, named people on both sides.
4. **Mutual plan:** kick-off date, milestones, check-ins, end date, final readout meeting with the decision-maker.

Running it:
- Get to first value in the first days (install, first data flowing).
- Weekly check-ins against the criteria; share a written status.
- Track risks openly; escalate blockers (access, security approvals) early through the champion.
- Make their engineers do some of the hands-on work so they own the result.

Close with a readout: each criterion, evidence, result, and the agreed next step (commercial proposal). Document it so the champion can share it internally.

#### Write success criteria for a proof of concept of an observability product.

Good criteria are specific, measurable, tied to pain, and achievable in the POC window. Example for a retailer with slow incident response (adapt numbers to discovery):

| # | Criterion | Measure | Owner |
|---|---|---|---|
| 1 | Coverage | Agent deployed on the 3 checkout services in staging and 1 production cluster, with traces, logs and metrics correlated | SE + customer platform team |
| 2 | Faster diagnosis | In 3 replayed past incidents, the on-call engineer identifies the root-cause service in under 15 minutes (baseline: about 2 hours) | Customer SRE |
| 3 | Integrations | Alerts routed to their existing paging tool and team chat; SSO through their identity provider | SE |
| 4 | Performance overhead | Agent adds less than an agreed CPU/latency threshold, measured under load test | Customer |
| 5 | Security approval | Data-handling review passed with the security team | AE + SE + security |

Also state what is **out of scope** (all 200 services, cost optimisation features), the timeline, and the decision that follows if criteria are met.

#### The prospect asks for a free, open-ended 90-day pilot. How do you respond?

Do not refuse outright; understand and reshape.

1. **Ask why:** "What do you need to learn in 90 days?" Often the real reasons are internal approval cycles, uncertainty about effort, or no clear criteria.
2. **Explain the risk to them:** open-ended pilots lose momentum; people get pulled away and the evaluation ends with no decision.
3. **Offer a structured alternative:** a 2–4 week POC with agreed success criteria, named owners, and a decision meeting at the end. If they truly need longer (e.g., seasonal data), break it into milestones with checkpoints.
4. **Commercial options (AE's decision):** a paid pilot credited toward the contract, or a contract with an early exit clause, signals commitment from both sides.
5. **Get commitment:** "If we meet these criteria in three weeks, are you ready to move to a commercial discussion?"

If they refuse any structure, that is a qualification signal; discuss with the AE whether to invest SE time.

#### A proof of concept is going badly at the halfway point. What do you do?

1. **Diagnose honestly:** which criteria are at risk and why? Product gap, environment problem (access, network, data), customer not engaged, or criteria that were unrealistic?
2. **Tell the AE and your manager immediately**, with facts and options. No surprises at the readout.
3. **Fix what is fixable:** bring in specialists or product engineering for technical blockers; ask the champion to unblock access; run working sessions instead of waiting for the customer.
4. **Reset with the customer openly:** "We are behind on criteria 2 because of X. Here is our plan; can we extend by one week or adjust this criterion?" Changing criteria needs the decision-maker's agreement in writing.
5. **If it is a real product gap:** say so, show the roadmap or workaround honestly, and let the customer decide. A lost POC is better than a failed deployment.

Afterwards, run a retrospective and improve your qualification and scoping.

#### What is a technical win and how do you know you have it?

A technical win means the prospect's technical evaluators have concluded your solution meets their requirements and is their preferred choice (or at least acceptable) on technical grounds. The remaining decision is commercial.

Signs you have it:
- POC success criteria were met and the customer agreed in a readout.
- The technical champion or evaluation lead confirms, ideally in writing, that you are the recommended or approved vendor.
- Technical decision criteria are closed: security review passed, architecture approved, integration questions answered.
- The customer moves to procurement steps: asks for an order form, pricing, contract redlines.
- Their engineers start planning the rollout with you.

Signs you do not: vague praise ("looks great"), no access to the decision-maker, new technical questions keep appearing, or the competitor is still running a POC. Record the technical win status and the evidence in the CRM.

#### Hands-on exercise: build a small integration with our API in an hour and walk us through it.

What they test: can you read docs fast, get to something working, write clean code, and explain it to a customer.

Approach:
1. **Clarify the use case** (2 min) and pick the simplest flow that shows value.
2. **Read the quickstart, auth and one or two endpoints**; use the official SDK if one exists.
3. **Get a "hello world" call working early**, then build the scenario.
4. **Production touches if time allows:** API key from environment variables, error handling, retries with backoff on 429/5xx, idempotency keys for writes, pagination, webhook signature verification.
5. **Walk-through like a customer session:** what it does, show it running, key code, what you would add for production.

Example skeleton (payments-style API; adapt to the real SDK):

```python
import os, requests
API = "https://api.example.com/v1"
H = {"Authorization": f"Bearer {os.environ['API_KEY']}"}

def create_payment(order_id, amount_cents, currency="usd"):
    r = requests.post(f"{API}/payments", headers={**H, "Idempotency-Key": order_id},
                      json={"amount": amount_cents, "currency": currency}, timeout=10)
    r.raise_for_status()
    return r.json()
```

See [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md) for retries, idempotency and webhooks.

### Objection handling

#### What framework do you use to handle objections?

A simple, widely used loop:

1. **Listen fully** without interrupting or getting defensive.
2. **Acknowledge** the concern as reasonable: "That's a fair concern; others have raised it."
3. **Clarify / isolate:** ask questions to find the real issue. "When you say too complex, is that setup, daily use, or maintenance?" "Is this the main thing stopping you, or are there others?"
4. **Respond** with evidence: a demo, a reference customer, a benchmark, documentation, or an honest "we don't do that; here's how customers handle it."
5. **Confirm:** "Does that address your concern?"

Principles: never argue, never bluff, separate real objections (product gap) from smoke screens (no budget, no urgency), and log recurring objections for product and enablement. Many teams teach variants of this (e.g., LAER: Listen, Acknowledge, Explore, Respond).

#### Objection role-play: "This is too expensive. We could build it ourselves."

Acknowledge, then compare total cost honestly.

"That's a fair option; you have a strong engineering team. Can I ask a few questions to compare fairly?"

- **Scope:** "Which parts would you build? How long would the first version take, and how many engineers?"
- **Total cost of ownership:** engineer salaries for the build, plus ongoing maintenance, on-call, security, compliance, upgrades and scaling. Build a simple model together: e.g., 3 engineers for 6 months plus 1 engineer ongoing, and compare with the subscription.
- **Opportunity cost:** "What would those engineers build for your customers instead?"
- **Time to value:** weeks with a product versus months for a build.
- **Risk:** features you would have to replicate (edge cases, integrations, certifications).

Then: "If your use case is narrow and core to your differentiation, building may be right. If it's not your core business, most teams find buying cheaper over three years. Shall we build that comparison with your numbers?"

Pricing and discounting is the AE's call; the SE supplies the value and cost model.

#### Objection role-play: "We already use your competitor. Why would we switch?"

"That makes sense; switching has a real cost, so it needs a good reason. What's working well with them? And what made you take this meeting?"

- **Find the gap:** their reason for talking to you is usually pain (cost, scale, missing feature, support). Explore it: when, how often, what impact.
- **Differentiate on what matters to them,** not on a full feature list. "You mentioned X. Here's how we approach X differently," then show it.
- **Address switching cost directly:** migration tools, how long a typical migration takes, a phased approach (start with one team or workload), running side by side.
- **Proof:** a reference customer who switched, a focused POC on the pain area.
- **Respect the competitor:** "They're a solid product; where we tend to win is..."

If there is no real pain, it is fine to say: "It may not be the right time to switch; can we stay in touch for when you renew or expand?"

#### Objection role-play: "Your product will not scale to our volume."

Treat it as a technical question, not an attack.

1. **Clarify numbers:** "What volume do you mean: events per second, data size, users, regions? Peak or average? Growth over 3 years?"
2. **Understand the source:** a past bad experience, a competitor's claim, or an architectural assumption?
3. **Answer with evidence:** architecture explanation (how it scales horizontally, limits, quotas), published limits and benchmarks, reference customers at similar or larger scale (only ones you can name publicly), and any internal benchmark the product team can share.
4. **Offer to prove it:** a scoped load test in the POC with a clear success criterion ("sustain 50k events per second with p99 ingestion latency under X").
5. **Be honest about limits:** if they are beyond known tested scale, say so and involve product engineering.

Close: "If we show this in a load test with your traffic profile, would that resolve the concern?"

#### How do you handle a technical objection you cannot answer on the spot?

- **Do not guess.** A wrong answer damages credibility for the whole deal.
- **Say so clearly:** "I want to give you an accurate answer rather than a guess. Let me confirm with our engineering team."
- **Clarify the question** so you get it right: what exactly do they need, and why does it matter to them?
- **Answer what you can:** related facts you are sure of.
- **Commit to a time:** "I'll come back to you by Thursday," write it down, and do it.
- **Follow up in writing** with the answer, links to docs, and, if useful, an engineer on a call.
- **Close the loop internally:** add the question to the FAQ or knowledge base so the team has it next time.

This often builds trust because the buyer sees you will not bluff.

### Competitive positioning

#### How do you position against a competitor without criticising them?

- **Know them well:** use their product (trials, docs), read their public pricing and limits, and learn from win/loss notes.
- **Lead with your strengths for this customer,** not their weaknesses.
- **Set traps, not attacks:** shape decision criteria where you are strong. "When you evaluate, I'd test how each product handles X at your volume." The customer discovers the difference themselves.
- **Acknowledge where they are good:** it builds credibility.
- **Use facts that can be verified:** documentation, public benchmarks with method, reference customers. Never spread rumours or out-of-date claims.
- **Talk about approach differences:** "They take approach A, which works well for Y; we chose approach B because of Z, which matters for your case."
- **Offer a direct comparison test** in the POC.

Criticising a competitor often backfires, especially if the buyer chose that product before.

#### How do you build and use a competitive battlecard?

A battlecard is a short, internal reference for one competitor. Typical sections:

- **Overview:** who they are, target customers, pricing model.
- **Where they win / where we win:** honest, based on win/loss data.
- **Key differentiators:** 3–5 points with proof (demo, doc, customer).
- **Common claims they make and our factual response.**
- **Landmine (trap-setting) questions** to ask the prospect to reveal gaps that matter.
- **Objection responses.**
- **Proof points:** public case studies, references who switched.
- **Last updated date and owner.**

Use: review it before discovery when you know the competitor is in the deal, use the questions in discovery, and shape POC criteria. Keep it current after their releases, and share field updates with the product marketing team that usually owns it. Never share it with customers.

#### The prospect sends a feature comparison spreadsheet written by a competitor. How do you handle it?

1. **Stay calm and thank them.** It tells you a competitor is engaged and is shaping criteria.
2. **Check each claim** against current facts; correct errors politely with evidence (docs links, screenshots), not opinions.
3. **Reframe:** "Many of these items don't relate to the problems you described. Can we focus on the five that matter most to your team?" Re-anchor on their success criteria from discovery.
4. **Add missing rows** that matter to their use case and where you are strong, with proof.
5. **Offer to test, not argue:** "Let's validate the key ones in the POC."
6. **Be honest** about real gaps and explain workarounds or roadmap (only committed items).
7. **Tell the AE and product marketing**: competitor tactics matter for the team.

Do not reply with a counter-spreadsheet full of attacks; keep it factual and short.

### RFPs, security and procurement

#### How do you approach an RFP, and do you respond to every one?

Not every one. An RFP you did not help shape often has a preferred vendor already. Bid/no-bid check with the AE:
- Did we know about this before the RFP? Do we have a relationship and a champion?
- Do the requirements fit our strengths? Any must-haves we cannot meet?
- Is the deal size worth the effort (RFPs can take days to weeks)?
- Can we meet the deadline with good quality?

If we respond:
1. **Kick-off:** owner, timeline, internal reviewers (legal, security, product).
2. **Ask clarifying questions** through the allowed channel; this can also influence the buyer.
3. **Reuse an answer library** (many teams use RFP tools), but tailor key answers to the buyer's situation.
4. **Answer honestly:** "Yes", "Yes with configuration", "Roadmap (with no committed date unless approved)", "No, workaround: ...". Never mark "Yes" for something partial.
5. **Executive summary** focused on their goals, with differentiators and proof.
6. **Review** for consistency, then submit on time and request a debrief either way.

#### How do you handle a 300-question security questionnaire?

- **Start early:** security review is often the longest step in the paper process; ask about it in discovery.
- **Lead with standard documents:** SOC 2 Type II report (under NDA), ISO 27001 certificate, penetration test summary, a pre-filled standard questionnaire such as SIG or CAIQ, a trust-centre page. Many buyers accept these instead of, or alongside, their own form.
- **Use the answer library** maintained by the security/GRC team; do not invent answers.
- **Triage:** answer standard items from the library, route unusual ones to security, legal or engineering with a deadline.
- **Be precise and honest:** if a control is partially met, say how and what compensating controls exist.
- **Offer a call** between their security team and yours for complex topics (data flow, encryption, sub-processors).
- **Track** status, owners and the deadline, and update the library with new answers.

The SE coordinates and translates; the security team owns the accuracy of the answers.

#### The prospect asks whether you are SOC 2 Type II, HIPAA and GDPR compliant and where their data will be stored. How do you answer?

Answer only with verified facts, and explain the terms precisely:

- **SOC 2 Type II** is an audit report on controls over a period (often 6–12 months). Answer: "Yes, our latest report covers [period]; we can share it under NDA." Or say honestly if only Type I is available.
- **HIPAA** has no official certification. The right question is: "Will you sign a Business Associate Agreement (BAA), and which services are covered?" Answer with the company's actual position.
- **GDPR** is a regulation, not a certification. Talk about the Data Processing Agreement (DPA), sub-processor list, lawful transfer mechanisms (e.g., Standard Contractual Clauses), data subject request support, and data residency options.
- **Data location:** name the regions where data is stored and processed, whether customers can choose a region, where backups and support access happen, and retention and deletion policies.

Point them to the trust centre and offer a session with the security team. Never say "fully compliant" loosely. See also [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md).

#### Procurement or legal is stalling a deal after the technical win. What can the solutions engineer do?

The AE leads the commercial process, but the SE can remove technical blockers:

- **Find out exactly what is stuck:** security review findings, data processing terms, architecture approval, an internal budget step.
- **Answer technical items in contracts quickly:** SLA definitions, uptime history, data handling, sub-processors, exit and data export.
- **Arm the champion:** a one-page summary of the POC results and business case they can forward to procurement and the economic buyer.
- **Bring experts together:** a call between their security or legal team and ours often resolves weeks of email.
- **Keep urgency real:** connect the timeline to the pain uncovered in discovery ("your peak season starts in October; onboarding takes four weeks").
- **Keep the technical team warm:** plan the implementation together so they push internally.

Prevent this next time by mapping the paper process early (the P in MEDDPICC).

### Sales process and collaboration

#### Walk through the stages of a typical enterprise sales cycle and the solutions engineer's role at each.

Stage names vary by company; a common flow:

| Stage | What happens | SE role |
|---|---|---|
| Prospecting / lead | Marketing or SDR books first meeting | Occasionally help with technical outreach or events |
| Discovery / qualification | Understand pain, decision process | Technical discovery, current architecture, fit check |
| Solution / demo | Show how it solves their problem | Tailored demos, architecture sessions |
| Technical validation (POC/trial) | Prove it works in their environment | Scope criteria, run POC, technical win |
| Business case / proposal | Pricing and ROI | Supply value model, solution design, sizing |
| Negotiation / paper process | Legal, procurement, security | Security questionnaires, technical contract terms |
| Closed won / handover | Contract signed | Handover to CS/professional services/FDE |
| Post-sale / expansion | Adoption, renewal, upsell | Join expansion discovery, help with new use cases |

Typical enterprise cycles take a few months or longer; self-serve and mid-market deals are shorter.

#### How do you prioritise across many deals with limited time?

- **Align with the AEs and the forecast:** prioritise by deal size, stage and probability, and close date. Late-stage deals with a technical blocker come first.
- **Qualify hard:** avoid investing POC time in deals without pain, budget or access.
- **Match effort to deal size:** smaller deals get standard demos, recorded content and self-serve trials; large strategic deals get custom work.
- **Scale yourself:** reusable demos, templates, office hours, partner or specialist SEs for deep topics.
- **Time-block:** protect time for preparation and follow-ups, not just meetings.
- **Be transparent:** weekly review with your manager and AEs on where your time goes; say no or "next week" explicitly rather than doing everything poorly.

#### The account executive wants to promise a roadmap feature that is not committed. What do you do?

1. **Talk to the AE privately, before the customer meeting** if possible. Explain the risk: if it slips, we lose trust, may face contract disputes, and the customer may churn.
2. **Find what the customer actually needs:** often a workaround, an integration, or partial capability solves the problem.
3. **Get facts from product:** status, confidence, and the official way to discuss roadmap (many companies require NDA and "no commitments" language).
4. **Offer safe wording:** "This is on our roadmap; it isn't committed with a date. Here's how customers handle it today."
5. **If the feature is a hard requirement,** escalate to product leadership; any commitment must be approved and properly documented, not promised verbally.
6. **If the AE promises it anyway in front of the customer,** clarify gently in the moment or immediately after, and involve your manager.

Your credibility is the SE's main asset; protect it.

#### How do you hand over a closed deal to post-sales?

A poor handover causes slow onboarding and early churn. Good practice:

- **Written handover document:** business goals and metrics the customer bought for, success criteria, stakeholders (champion, economic buyer, technical owners), architecture and integrations, POC results and configuration, open risks, promises made (and what was not promised), timelines.
- **Internal handover meeting** with customer success, professional services or the implementation/FDE team.
- **Customer-facing transition call:** introduce the post-sales team, restate goals, agree the onboarding plan.
- **Transfer assets:** POC code, environments, scripts, diagrams.
- **Stay available** for a few weeks for questions and join the first milestone review.

The aim is that the customer never has to repeat themselves.

#### How do you get field feedback turned into product changes?

- **Make it specific:** which customers, their use case, what they tried, the gap, and the workaround.
- **Quantify it:** revenue blocked or at risk, number of deals affected, win/loss data. Product teams prioritise on impact.
- **Use the official channel:** the product feedback tool or CRM field, so requests are aggregated, not lost in chat.
- **Group similar requests** across SEs to show a pattern.
- **Bring customers to product managers** for interviews when the need is strategic.
- **Propose, don't only complain:** suggest the smallest version that would unblock deals.
- **Close the loop** with the customer and your team when something ships.

#### How do you build a business value or ROI case that the economic buyer will accept?

Build it with the champion, from the customer's own numbers, so it is their case rather than your pitch.

1. **Start from the pain found in discovery** and the metric the economic buyer is measured on (cost, revenue, risk, time to market).
2. **Quantify the current cost:** for example, engineer hours lost per incident × incidents per month × loaded hourly cost, plus revenue lost per hour of downtime. Use their figures; mark every assumption.
3. **Estimate the improvement conservatively,** grounded in POC results or published case studies, not best-case marketing claims.
4. **Include total cost:** subscription, implementation, training, internal staff time, and any tools retired (which may offset cost).
5. **Show a range and payback period** (conservative, expected), and which assumption matters most.
6. **Have the champion validate it** with finance before it reaches the economic buyer, and keep it to one page plus a backup spreadsheet.

Pricing and discounts stay with the AE; the SE owns the credibility of the numbers. An inflated case is easy for finance to dismiss and damages trust in the rest of the evaluation.

### AI vendor specifics

#### How do you handle a prospect's concerns about hallucination, data privacy and training on their data when selling an AI product?

**Hallucination:** be honest that models can produce wrong answers. Explain mitigations: grounding with retrieval (RAG) and citations, constrained outputs, tool use for facts and calculations, human review for high-risk steps, and evaluations on their own data to measure accuracy before launch. Frame it as risk management for a specific use case. See [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md).

**Data privacy and training:** answer only with the company's actual published policies: whether API or enterprise data is used for training by default, data retention periods, zero-data-retention options if offered, encryption, regions, and certifications. Point to the official terms and trust documentation.

**Deployment options:** direct API, or via cloud platforms (e.g., Amazon Bedrock, Google Cloud Vertex AI, Microsoft Azure) if the vendor's models are offered there, so data stays in their cloud agreement.

**Governance:** usage policies, audit logs, access controls, and safety features.

End with: "Let's design an evaluation on your data so you see the accuracy yourself."

#### How would you run a bake-off where a prospect compares our LLM API against another provider?

1. **Define the task and success metrics with them:** accuracy on their real cases, latency (p50/p95), cost per task, format compliance, safety behaviour.
2. **Build a representative eval set:** 100–500 real or realistic examples with expected outputs or grading rubrics, including hard and edge cases.
3. **Fair setup:** comparable model tiers, same inputs, and prompts tuned reasonably for each model (each model may need its own prompt style).
4. **Grading:** automated checks where possible (exact match, schema validation), LLM-as-judge with a rubric validated against human labels, plus human review of a sample.
5. **Measure cost and latency** under realistic load, including caching or batching options.
6. **Report:** results per metric and per category, failure examples, and the trade-offs.

Be honest if the other model wins on some cases. Recommend the eval set become their regression suite. See [06 AI / LLM evals](../../topics/06-ai-llm/ANSWERS-part2.md).

#### A prospect asks what our LLM API will cost them per month. How do you estimate it?

Give a range built from their workload, not a guess, and say what would change it.

1. **Get the workload shape:** requests per day (average and peak), input tokens per request (system prompt, retrieved context, conversation history), output tokens per request, and how many model calls each user task makes (agents and multi-step chains multiply this).
2. **Compute:** monthly requests × (input tokens × input price + output tokens × output price) ÷ 1,000,000, using the current published prices for the candidate model(s).
3. **Apply the levers that fit:** prompt caching on repeated prefixes, batch processing for non-urgent work, a smaller model for easy steps.
4. **Present baseline, optimised and growth scenarios,** plus cost per business unit (per ticket, per document), which is what buyers compare with today's cost.
5. **Validate in the POC:** measure real token counts, since early estimates are often off by a large factor.

Leave pricing agreements and discounts to the AE. See [05 Applied AI Engineer](../05-applied-ai-engineer/README.md) for a worked cost template.

### Role-specific behavioral

#### Tell me about a deal where your technical work made the difference.

**What they look for:** you understood the customer's real problem, you took ownership of the technical process, your specific action changed the outcome, and you can quantify it.

**STAR outline:**
- **S:** [Customer type, deal size range, competitors, what was at stake]
- **T:** [Your role; the technical blocker or risk]
- **A:** [Discovery insight, custom demo/POC, how you set success criteria, how you worked with the AE and champion]
- **R:** [Technical win, closed deal, timeline, what the customer achieved after; what you reused later]

**Example (adapt to your own experience):** "A logistics prospect was evaluating us against an incumbent and doubted our API could handle their peak load. I proposed a POC criterion of sustaining their peak volume with an agreed latency, built a load-test harness with their engineers, and found a batching setting that met the target. The readout showed the criteria met, the champion presented it internally, and the deal closed that quarter. I turned the harness into a reusable asset for other SEs."

If you have no sales experience, use an internal "sale": convincing a team to adopt a tool or design.

#### Tell me about a deal you lost and what you learned.

**What they look for:** honest ownership (not blaming product or price), a real root-cause analysis, and a changed behaviour.

**STAR outline:**
- **S:** [Deal context, competitor]
- **T:** [Your role in the technical evaluation]
- **A:** [What you did; what you missed, such as weak discovery, no access to the decision-maker, criteria not agreed]
- **R:** [Loss reason from the debrief; what you now do differently, with a later example]

**Example (adapt to your own experience):** "We lost a deal where our POC met every technical criterion, but the decision went to a competitor. In the debrief we learned the economic buyer cared most about consolidating vendors, which we had never discussed because we only talked to engineers. Since then I ask in discovery how the decision will be made and who signs, and I ask my champion to set up an early meeting with the economic buyer. In the next quarter that question surfaced a similar consolidation priority early, and we positioned for it."

#### Tell me about a time you had to say no to a customer or to your account executive.

**What they look for:** integrity under pressure, clear communication, and an alternative offered, while keeping the relationship.

**STAR outline:**
- **S:** [Request: promise a feature, skip a security review, unrealistic POC scope, custom build]
- **T:** [Why it was wrong or risky]
- **A:** [How you said no (privately, with evidence), the alternative you offered, who you involved]
- **R:** [Outcome for the deal and the relationship; trust gained]

**Example (adapt to your own experience):** "Near quarter-end, an AE wanted me to tell a prospect we supported on-premise deployment, which we did not. I explained privately that the customer would discover it in implementation and we would face churn and a contract dispute. I proposed showing our private-networking option instead and asked product for a session on their security concerns. The customer accepted the private-network design; the deal closed a month later than hoped, and the AE later brought me into his largest deal because the customer trusted our answers."

## Prep checklist

- [ ] Sign up for the company's free trial or developer account and build a 15–20 minute demo around one realistic customer story; rehearse it out loud at least three times, with a recorded backup.
- [ ] Write a two-minute non-technical explanation and a ten-minute technical deep dive of the product.
- [ ] Learn MEDDPICC (or the company's framework) well enough to fill it in for a hypothetical deal.
- [ ] Prepare a discovery question bank and practise a 30-minute discovery role-play with a friend acting as a skeptical buyer.
- [ ] Map the top 2–3 competitors: use their trials and docs, and write a one-page honest comparison.
- [ ] Prepare answers to the five most likely objections (price, build vs buy, incumbent, scale, security).
- [ ] Draft sample POC success criteria and a mutual plan for one use case.
- [ ] Read the company's trust/security page: certifications, data residency, BAA/DPA position.
- [ ] Prepare three deal-style stories (win, loss, saying no) using the STAR outlines above; if you lack sales experience, use internal adoption or customer-facing stories.
- [ ] Prepare questions about SE:AE ratio, compensation split, how technical wins are tracked, and team quota attainment ([14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md)).
