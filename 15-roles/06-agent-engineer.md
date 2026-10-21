# Agent Engineer

Engineer who designs, builds, launches and keeps improving customer-facing AI agents (chat, email and voice) for a brand's support and service workflows.

---

## The role

**Day to day.** You own one or more brands' agents end to end. You map their support workflows and policies into agent instructions and journeys, integrate the agent with their systems (order management, CRM, ticketing, billing, identity), set guardrails and escalation rules, build regression tests and simulations, launch in stages, and then spend much of your time reading conversations, finding failures and improving resolution rate and customer satisfaction. You work closely with the brand's customer-experience (CX) leaders, support operations, and their engineers.

**Common titles / aliases.** Agent Engineer, AI Deployment Engineer, Forward Deployed Engineer (agents), Solutions Engineer / Implementation Engineer (AI agents), Conversation Designer (more design-focused), Agent Product Manager (adjacent, less coding). Exact titles vary by company.

**Who hires for it (examples; well-known facts only).**
- **Sierra** — conversational AI agent platform for customer experience, co-founded by Bret Taylor and Clay Bavor.
- **Decagon** — AI agents for customer support.
- **Intercom** — its AI customer-service agent is called Fin; Intercom publicly prices Fin per resolution.
- **Voice-agent companies** — e.g. PolyAI (voice assistants for enterprise customer service) and developer platforms for voice agents such as Vapi and Retell AI.
- **Large platforms with agent products** — e.g. Salesforce (Agentforce), Zendesk, and other customer-service software vendors; also in-house CX teams at large brands.

**How it differs from FDE.** The domain is narrower and deeper: customer service conversations, measured by business metrics (resolution, CSAT, handle time, cost per contact). Integration work is real but usually follows repeatable patterns (commerce, CRM, ticketing, telephony). There is more conversation design, policy interpretation and operational iteration than a general FDE role, and the agent is live with the brand's customers, so quality and brand risk are immediate. A general FDE may build any kind of system; an Agent Engineer builds and runs one kind very well.

**Who it suits.** Engineers who like product and people problems as much as code, read transcripts with curiosity, care about tone and edge cases, and enjoy fast iteration against clear metrics. Support or operations experience is a real advantage.

**Skills mix.**

| Skill | Weight | Notes |
|---|---|---|
| Coding | Med–High | Integrations, tools, tests, scripts; typically Python/TypeScript |
| LLM / agent design | High | Instructions, tools, guardrails, evals, simulation |
| Customer / CX communication | High | Working with CX leaders and support ops weekly |
| Conversation design | Med–High | Flows, tone, clarification, escalation wording |
| Sales support | Low–Med | Pilots and business cases; less pre-sales than solutions roles |
| Analytics | Med | Resolution, CSAT, funnel and transcript analysis |

**Career paths.** Senior / Lead Agent Engineer owning larger brands; engineering or deployment manager; product manager for the agent platform (tools, evals, voice); platform engineering on the core agent framework; founding engineer at a vertical agent startup.

---

## Typical interview loop

Loops vary and change. Treat this as typical/reported, not guaranteed.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter / motivation | Why agents, why customer-facing, comfort with travel and live customers | [01-recruiter-and-motivation](../01-recruiter-and-motivation/README.md) |
| Coding (practical) | Clean code, APIs, JSON handling, incremental requirements | [02-coding](../02-coding/README.md) |
| Agent design case | Designing an agent for a brand's workflows, tools, guardrails, escalation, metrics | This file; [03-system-design B](../03-system-design/ANSWERS.md) |
| Live "build an agent" exercise (reported at agent companies) | Building a working agent with tools and tests in a time box | This file, "Live build"; [06-ai-llm D and J](../06-ai-llm/README.md) |
| Customer role-play / presentation | Launch plan, handling a CX leader, explaining failures | [09-customer-scenarios](../09-customer-scenarios/ANSWERS.md) |
| Debugging / metrics investigation | Diagnosing a drop in resolution or a misbehaving agent | [12-debugging-troubleshooting C](../12-debugging-troubleshooting/ANSWERS.md) |
| Hiring manager / values | Ownership, customer obsession, judgment | [10-behavioral](../10-behavioral/ANSWERS.md) |

---

## Common rounds — use the shared sections

- [06-ai-llm](../06-ai-llm/README.md) — prioritise D (agents and tool use), E (evaluation), G (production: latency, rate limits, fallbacks) and H (safety, prompt injection). Section J's support-bot take-home is close to this role.
- [03-system-design](../03-system-design/README.md) — section B, especially the support chatbot, refund/CRM agent and voice scheduling agent prompts.
- [08-cloud-devops-integration](../08-cloud-devops-integration/README.md) — APIs, webhooks, auth (OAuth, API keys), idempotency; agents live or die on integrations.
- [09-customer-scenarios](../09-customer-scenarios/README.md) — difficult situations and communication with non-technical stakeholders.
- [12-debugging-troubleshooting](../12-debugging-troubleshooting/README.md) — LLM-specific troubleshooting.
- [11-company-specific](../11-company-specific/README.md) — "Other companies" notes for Sierra, Decagon and Intercom; do the research checklist and try the products.
- [10-behavioral](../10-behavioral/README.md) and [14-questions-to-ask](../14-questions-to-ask/README.md).

---

## Role-specific questions

### Designing an agent for a brand's support workflows
- [ ] Case: an online retailer wants an agent to handle order status, returns and exchanges. Design it.
- [ ] How do you decide which intents the agent handles at launch and which go straight to humans?
- [ ] How do you turn a brand's SOPs, macros and help-centre articles into agent instructions?
- [ ] One general agent or several specialised agents/journeys for a support workflow: how do you decide?
- [ ] How do you capture the brand's voice and tone, and test that the agent keeps it?
- [ ] How should the agent verify a customer's identity before account-specific actions?

### Conversation design
- [ ] Design the conversation flow for a refund request that requires policy checks.
- [ ] How should the agent handle ambiguous or multi-intent messages?
- [ ] How should the agent handle an angry customer, or one who keeps asking for a human?
- [ ] How do you balance asking clarifying questions against making assumptions?
- [ ] How does conversation design differ between chat, email and voice?

### Tool and API integration with customer systems
- [ ] Design the tools the agent needs to work with a commerce platform, a CRM and a ticketing system.
- [ ] The brand's order API is slow and sometimes fails. How does the agent cope mid-conversation?
- [ ] How do you make write actions such as refunds and cancellations safe and idempotent?
- [ ] The brand's backend has no API for an action the agent needs. What are your options?
- [ ] How do you handle secrets, authentication and data access when the agent calls the brand's systems?

### Guardrails and escalation to humans
- [ ] What guardrails would you put on a support agent that can issue refunds?
- [ ] Design the handoff from the agent to a human. What context is passed and how?
- [ ] How do you stop the agent from promising things the brand's policy does not allow?
- [ ] How do you protect a customer-facing agent from prompt injection and abuse?
- [ ] How should the agent handle sensitive situations: self-harm, legal threats, regulated advice, payment card data?

### Voice agents
- [ ] Walk through the latency budget of one turn in a voice agent.
- [ ] How do you handle interruptions (barge-in) and end-of-turn detection?
- [ ] How do you capture names, email addresses and order numbers reliably over a phone line?
- [ ] Cascaded speech-to-text, LLM and text-to-speech vs. a speech-to-speech model: what are the trade-offs?

### Measuring resolution rate, CSAT and business impact
- [ ] How do you define and measure resolution rate honestly?
- [ ] Containment, resolution and CSAT: what is the difference, and what can go wrong with each?
- [ ] Resolution rate dropped from 62% to 55% this week. Investigate.
- [ ] How do you measure customer satisfaction with AI conversations when few customers answer surveys?
- [ ] How do you build the ROI case for a brand's agent deployment?

### Agent regression testing
- [ ] How do you build a regression test suite for a customer-service agent?
- [ ] How do you use simulated customers to test an agent?
- [ ] The brand changes its return policy tomorrow. How do you ship the change safely?
- [ ] How do you review production conversations at scale to find failures?

### Live "build an agent" exercise
- [ ] Live: build a support agent with lookup_order and create_return tools in 60 minutes.
- [ ] Live follow-up: add escalation to a human and write tests for it.
- [ ] Role-play: present the launch plan for a new agent to the brand's VP of Customer Experience.

### Behavioral (role-specific)
- [ ] Tell me about a time something you launched to customers misbehaved in production.
- [ ] Tell me about a time you pushed back on a customer's request for quality or safety reasons.

---

## Model answers

### Designing an agent for a brand's support workflows

#### Case: an online retailer wants an agent to handle order status, returns and exchanges. Design it.
**Clarify first:** channels (chat, email, voice), volume, top contact reasons from ticket data, return policy and exceptions, systems (commerce platform, warehouse/shipping, CRM, helpdesk), languages, and target metrics.

**Design:**
- **Identity:** verify the customer (logged-in session token, or order number + email, or one-time code) before showing order details.
- **Journeys:** order status (read-only), return (eligibility check → reason → method → label), exchange (eligibility, stock check, create exchange order). Each journey has written policy, required data, allowed actions and exit conditions.
- **Tools:** `get_customer`, `list_orders`, `get_order`, `check_return_eligibility`, `create_return`, `check_stock`, `create_exchange`, `handoff_to_human`. Typed inputs, clear descriptions, deterministic eligibility in code, not in the prompt.
- **Guardrails:** refund/exchange limits, confirmation before any write, no policy invention, escalation triggers (fraud signals, high value, angry customer, repeated failure).
- **Handoff:** to the helpdesk with a summary, verified identity, actions taken.
- **Evaluation:** regression conversations per journey, simulated customers, launch on a small share of traffic, then ramp.
- **Metrics:** resolution rate, CSAT, escalation rate, wrong-action rate, cost per contact.

#### How do you decide which intents the agent handles at launch and which go straight to humans?
Use the brand's ticket data. Pull 3–6 months of contacts, cluster them by reason, and score each intent:
- **Volume:** share of contacts.
- **Automatable:** is there a clear policy and the data/APIs to act on it? Read-only intents (order status, FAQs) are easier than write actions.
- **Risk:** cost of a mistake (money, legal, safety, brand). Fraud claims, legal threats, and medical or safety issues start with humans.
- **Complexity:** number of steps, exceptions, systems.
- **Customer emotion:** complaints about serious failures may need a human early.

Launch with high-volume, low-risk, well-defined intents (often 40–60% of volume is in a handful of intents), and route the rest to humans with a good handoff. Agree a roadmap: as tests and metrics prove the agent on one intent, expand to the next. Write the decision down with the CX lead so "why doesn't it handle X?" has an agreed answer.

#### How do you turn a brand's SOPs, macros and help-centre articles into agent instructions?
- **Collect everything:** SOPs, macros (saved replies), help-centre articles, internal wikis, and examples of great human conversations. Interview senior agents about the unwritten rules.
- **Find conflicts and gaps:** help-centre says 30 days, an internal SOP says 45 days for members. Get a decision from the policy owner; never let the model choose.
- **Split by type:**
  - Policies and decision rules that need exact application (eligibility windows, limits) → code inside tools where possible, or precise structured instructions.
  - Procedures → journey steps: what to collect, which tools, in what order, when to escalate.
  - Knowledge (product details, FAQs) → retrieval from the knowledge base.
  - Tone → a short brand voice section with examples.
- **Write instructions as a human trainer would:** goal, steps, exceptions, examples, what not to do.
- **Test each rule** with conversation tests derived from it, and link instructions to their source so policy changes can be traced.

#### One general agent or several specialised agents/journeys for a support workflow: how do you decide?
**One agent with all tools and instructions** is simpler, keeps context in one place, and handles topic switches naturally. It struggles when instructions grow very long, tools number in the dozens, and policies for different journeys interfere with each other.

**Specialised journeys or sub-agents** (a router plus focused agents for returns, billing, technical support) give shorter, clearer instructions, smaller toolsets, independent testing and ownership, and safer changes (editing returns cannot break billing). Costs: routing errors, handoffs between parts losing context, and more moving pieces.

Practical approach: start with one agent and a modest set of journeys loaded as needed; split when evals show interference, when different teams own different areas, or when different risk levels need different guardrails or models. Whatever the structure, the customer should experience one consistent agent, and shared context (identity, conversation summary) must travel across components.

#### How do you capture the brand's voice and tone, and test that the agent keeps it?
- **Source material:** brand guidelines, top-rated human conversations, macros, marketing copy. Ask the CX team for 10 replies they love and 10 they dislike.
- **Write a concise voice section:** 4–6 traits with do/don't examples (e.g. "warm, not gushing; no exclamation marks; use the customer's first name once; never blame the customer"). Specify formality, emoji use, sign-offs, apology style, length limits per channel.
- **Include examples** of good replies in a few situations (delay, denial, apology).
- **Test it:**
  - An LLM judge with a rubric per trait, validated against the CX team's ratings on 50 replies.
  - Hard cases: denying a refund politely, an angry customer, a joke from the customer.
  - Checks for banned phrases and length.
- **Review with the brand** in a side-by-side session before launch, and sample production conversations weekly.

Tone must never override policy accuracy; a friendly wrong answer is still wrong.

#### How should the agent verify a customer's identity before account-specific actions?
Match the verification level to the risk of the action.
- **Best case: authenticated channel.** The customer is logged in to the app or website; the widget passes a signed token (e.g. a JWT signed by the brand's backend) and the agent's tools act only for that customer ID. The model never decides who the customer is.
- **Unauthenticated chat/email:** order number plus the email or postcode on the order; for higher-risk actions, a one-time code sent to the email or phone on file.
- **Voice:** caller ID is a weak signal (it can be spoofed), so add knowledge or one-time code checks for sensitive actions.

Rules:
- Verification is done by tools and code, which store a verified state; tools check that state, not the model's claim.
- Read-only status may need less than changing addresses, issuing refunds or revealing personal data.
- Limit attempts; escalate after failures.
- Never read back full personal data; confirm partially ("ending in 42").

### Conversation design

#### Design the conversation flow for a refund request that requires policy checks.
1. **Understand the request:** which order and item, and why. Ask only for what is missing.
2. **Verify identity** (if not already verified).
3. **Fetch facts with tools:** order date, delivery date, item category, price, prior refunds.
4. **Check eligibility in code:** `check_refund_eligibility(order, item, reason)` returns eligible / not eligible / needs review, with the reason and allowed options (refund, store credit, exchange).
5. **Explain the outcome clearly:**
   - Eligible: state amount, method and timing, and ask for explicit confirmation.
   - Not eligible: explain the specific reason plainly, offer allowed alternatives, offer a human if the customer disputes it.
   - Needs review (high value, suspicious, damaged item needing photos): collect what is needed and hand off with context.
6. **Act:** call `create_refund` with an idempotency key; confirm the result from the tool, never from assumption.
7. **Close:** summarise what happened, next steps, reference number; check if anything else is needed.

Tests cover each branch, plus the customer changing their mind mid-flow and the tool failing.

#### How should the agent handle ambiguous or multi-intent messages?
- **Ambiguous** ("my order is wrong"): if one interpretation is far more likely given context (one recent order, delivered yesterday), proceed with it and state the assumption. Otherwise ask one focused question with options ("Was an item missing, damaged, or not what you ordered?").
- **Multi-intent** ("cancel my subscription and why was I charged twice?"): acknowledge both, handle them in a sensible order (usually the time-sensitive or money issue first), and confirm at the end that each was handled. Keep track of open intents in the conversation state so none is lost.
- **Topic switches mid-flow:** handle the new question, then offer to return to the unfinished task.
- **Out-of-scope intents:** say what you can help with and hand off for the rest.

In testing, include multi-intent and switching scenarios specifically; they are a common source of "the agent forgot my second question" complaints.

#### How should the agent handle an angry customer, or one who keeps asking for a human?
**Angry customer:**
- Acknowledge the specific problem briefly and sincerely; avoid generic over-apologising.
- Move quickly to action: what the agent can do now, with concrete outcomes and times.
- Do not argue or defend the brand; do not mirror the tone.
- Escalation triggers: strong anger, threats, repeated dissatisfaction, a vulnerable customer, or the second failed attempt at resolution.

**Customer asks for a human:**
- Brand policy decides, but a good default is: if asked clearly, offer the handoff promptly. Optionally offer once to solve it quickly if the request is simple ("I can check your order status right now, or connect you to a teammate. Which would you prefer?"). Never trap people in loops.
- If no humans are available (out of hours), say so honestly and give the alternative: callback, ticket, expected response time.

Measure these: escalation requests, repeated requests for a human, and CSAT on those conversations.

#### How do you balance asking clarifying questions against making assumptions?
Weigh the cost of a wrong assumption against the cost of an extra question.
- **Read-only, low-risk, easily corrected** (which order to show the status of): assume the most likely option and state it ("Your latest order, #1234, is out for delivery. Did you mean a different order?").
- **Actions with consequences** (refunds, cancellations, address changes): always confirm the details explicitly before acting.
- **Use available context first:** account data, order history, the page the customer is on. Do not ask for information the agent can look up.
- **Ask one question at a time**, with options where possible; avoid long forms in chat and especially in voice.
- **Cap clarification rounds**; if the agent still cannot understand after two, offer a human.

Evaluate with test conversations that measure both errors from wrong assumptions and turns wasted on unnecessary questions.

#### How does conversation design differ between chat, email and voice?
- **Chat:** short turns, back-and-forth, links and buttons allowed, typing indicators, customers may leave and return. Keep replies to a few sentences; use quick-reply options for choices.
- **Email:** asynchronous and often one-shot. Read the whole thread, answer all questions in one complete, well-structured reply, include next steps and reference numbers. Clarification costs hours, so infer more from context and ask all needed questions at once.
- **Voice:** no visual channel and real-time. Very short sentences, one question at a time, no lists, URLs or markdown. Read numbers in a speakable way; confirm critical details by reading them back. Latency, interruptions and speech recognition errors dominate the experience.

Same policies and tools across channels; different response styles and length limits per channel, set in the instructions and tested separately.

### Tool and API integration with customer systems

#### Design the tools the agent needs to work with a commerce platform, a CRM and a ticketing system.
Design tools around **customer tasks**, not raw API endpoints, so the model makes few, clear choices.
- **Commerce:** `get_orders(customer_id, limit)`, `get_order(order_id)` (status, items, tracking, dates), `check_return_eligibility(order_id, item_ids)`, `create_return(order_id, item_ids, reason, method, idempotency_key)`, `update_shipping_address(order_id, address)` (only before fulfilment).
- **CRM:** `get_customer_profile(customer_id)` (tier, preferences, flags such as VIP or fraud risk), `log_interaction(summary, outcome)`.
- **Ticketing:** `create_ticket(category, priority, summary, transcript)`, `handoff_to_human(queue, summary)`, `get_open_tickets(customer_id)` so the agent knows about existing cases.

Principles:
- The tool layer is a service you own: it holds credentials, enforces the customer scope from the verified session, validates inputs, applies business rules and limits, and returns compact, model-friendly JSON (only fields the agent needs, no huge raw payloads).
- Clear names and descriptions, typed parameters, enums where possible.
- Errors returned as readable messages ("order not found", "return window closed on 3 Oct").
- Every call logged with conversation ID for audit and debugging.

#### The brand's order API is slow and sometimes fails. How does the agent cope mid-conversation?
**In the tool layer:**
- Timeouts per call (e.g. 3–5 s for chat, shorter for voice), retries with backoff for idempotent reads only, and a circuit breaker if the API is down.
- Caching for data that changes slowly (order details for a few minutes).
- Prefetching: when the customer is identified, fetch recent orders in the background.

**In the conversation:**
- For a slow call, keep the customer informed ("Let me check that for you"); in voice, play a short filler phrase rather than silence.
- On failure, the tool returns a clear error; the agent says it cannot access order information right now and offers options: try again shortly, a ticket with a follow-up, or a human.
- Never invent the data the tool failed to return. Test this explicitly.

**Operationally:** alert on error rate and latency per tool, track how many conversations failed due to tool errors, and share the numbers with the brand's engineering team.

#### How do you make write actions such as refunds and cancellations safe and idempotent?
- **Explicit confirmation** of the exact action and amount before calling a write tool.
- **Idempotency keys:** derive a key from conversation ID + action + order/item, and pass it to the backend (or keep a record in your tool layer) so retries and repeated model calls cannot refund twice.
- **Server-side validation:** the tool re-checks eligibility and limits regardless of what the model says. The model proposes; code decides.
- **Limits:** per-action and per-customer caps (amount, count per day); above them, human approval.
- **Scope:** the tool acts only on the verified customer's orders.
- **Confirm results from the system:** the agent reports the backend's response (reference number), not its own assumption.
- **Audit log:** who, what, when, conversation ID, input and response.
- **Reversibility:** prefer reversible actions or a short delay window for high-value ones.
- **Tests:** duplicate calls, timeouts after the backend actually succeeded (check status before retrying), and attempts to act on another customer's order.

#### The brand's backend has no API for an action the agent needs. What are your options?
In rough order of preference:
1. **Ask whether an API exists elsewhere:** an admin API, a platform's standard API (many commerce and helpdesk platforms have one), or an integration platform the brand already uses.
2. **Small service built by the brand's engineers** exposing just the needed action; you write the spec and tests.
3. **Asynchronous handoff:** the agent collects all required details, creates a structured ticket or queue item, and a human or back-office process completes it. The customer gets a clear timeline. This often covers the gap well at launch.
4. **Workflow tools / RPA / browser automation** against the internal UI: possible as a temporary bridge, but fragile and needs monitoring and an exit plan.
5. **Database access:** usually a last resort and only read-only, because it bypasses business logic.

Choose based on volume, risk and timeline, and document the decision. Launching with option 3 for low-volume actions is common and honest.

#### How do you handle secrets, authentication and data access when the agent calls the brand's systems?
- **Credentials live in the tool layer**, in a secrets manager, never in prompts or model-visible text. Rotate them and use separate credentials per environment.
- **Least privilege:** service accounts or OAuth scopes limited to the actions the agent needs; read-only where possible.
- **Customer scoping:** every tool call is bound to the verified customer ID from the session, enforced in code. The model cannot pass a different customer ID to get someone else's data.
- **Data minimisation:** return only fields the agent needs; mask sensitive data (full card numbers are never returned; partial addresses where possible).
- **Network:** allow-listed IPs, TLS, webhook signature verification for inbound events.
- **Logging and retention:** redact PII in logs per the brand's policy; define retention; respect deletion requests.
- **Compliance review:** data processing agreement, regions, sub-processors, and the brand's security questionnaire. See [08-cloud-devops-integration](../08-cloud-devops-integration/ANSWERS.md) for auth patterns.

### Guardrails and escalation to humans

#### What guardrails would you put on a support agent that can issue refunds?
Layers, from strongest to weakest:
1. **Code-enforced rules in tools:** eligibility, maximum amount per refund and per customer per period, only original payment method, only verified customer's orders, idempotency.
2. **Approval thresholds:** above a value or for flagged customers (fraud score, many recent refunds), create a request for human approval instead of acting.
3. **Confirmation step:** the customer confirms the exact action.
4. **Instructions:** clear policy, no exceptions without a tool that allows them, never promise outcomes before the tool confirms.
5. **Output checks:** detect replies that promise money, discounts or policy exceptions not backed by a tool result.
6. **Monitoring:** refund rate and total value per day vs. baseline, alerts on spikes, sampled review of refund conversations, abuse patterns (same customer trying many phrasings).
7. **Kill switch:** disable the refund tool quickly without taking the whole agent down.

The principle to state: the model can be persuaded; the code cannot. Put money limits in code.

#### Design the handoff from the agent to a human. What context is passed and how?
**When:** customer asks; policy requires it (high value, legal, safety); the agent fails twice; low confidence; strong negative sentiment; tool outage.

**How:**
- Use the brand's helpdesk or contact-centre integration to transfer the conversation (or create a ticket out of hours), routed to the right queue by intent, language and priority.
- Tell the customer what happens next and the expected wait; do not hand off into silence.

**Context passed:**
- Verified identity and verification method.
- A short structured summary: intent, key facts (order, item, dates), what the customer wants, what the agent did and the tool results, why it escalated.
- Customer sentiment and any promises already made.
- Full transcript link.

**Good practice:** the human should never ask the customer to repeat themselves. Measure it: time to human, repeat-question rate, CSAT after handoff, and human feedback on summary quality. Review escalated conversations to find intents the agent could learn to handle.

#### How do you stop the agent from promising things the brand's policy does not allow?
This risk is real and public: in Moffatt v. Air Canada (2024), a Canadian tribunal held the airline responsible for incorrect refund information given by its website chatbot.

Controls:
- **Ground policy answers** in retrieved, approved policy text and tool results; instruct the agent not to state policy it cannot cite.
- **Decisions in code:** eligibility and compensation come from tools; the agent can only offer what the tool returns as allowed options.
- **Forbidden promises list:** compensation, discounts, delivery guarantees, legal statements — only via tools or humans.
- **Output check** before sending for high-risk claims (amounts, "guarantee", "will refund"), flagging or blocking unsupported ones.
- **Tests:** adversarial customers pushing for exceptions ("the last agent promised me..."), outdated policies, and edge dates.
- **Monitoring:** sample conversations with money or policy language for review.
- **Policy freshness:** a process so policy changes update the knowledge source and tests on the same day.

#### How do you protect a customer-facing agent from prompt injection and abuse?
**Threats:** customers trying to make the agent ignore instructions, reveal its prompt, give unauthorised refunds or produce offensive content; indirect injection through content the agent reads (emails, order notes, web pages, uploaded files).

**Defences:**
- **Authority in code, not in the prompt:** tools enforce identity scope and limits, so a successful injection cannot move money or expose other customers' data.
- **Separate untrusted content:** wrap user and retrieved content in clear tags and instruct the model to treat it as data.
- **Input and output classifiers** for abuse, jailbreak patterns and policy violations; the brand's content standards on output.
- **No secrets in prompts.** Assume the system prompt can leak.
- **Rate limits and abuse detection:** per user, per IP; block automated abuse.
- **Red-team before launch** with a library of injection and abuse attempts, and keep them in the regression suite.
- **Logging and alerts** for blocked attempts; review patterns.

See [06-ai-llm section H](../06-ai-llm/ANSWERS-part2.md) for prompt injection basics.

#### How should the agent handle sensitive situations: self-harm, legal threats, regulated advice, payment card data?
Define these with the brand's legal and CX teams before launch, then implement as detection plus fixed handling:
- **Self-harm or safety risk:** respond with care, give appropriate crisis resources for the customer's region as agreed with the brand, and escalate to a trained human immediately. Do not continue the support flow as if nothing happened.
- **Legal threats or regulatory complaints:** acknowledge, do not argue or admit liability, route to the specified team, tag the ticket.
- **Regulated advice (medical, financial, legal):** give only approved general information, direct to a qualified professional, never personalised advice outside policy.
- **Payment card data:** never ask for or accept full card numbers in chat or transcripts; send customers to a secure, PCI-compliant payment flow; detect and redact card numbers in logs if a customer types them.
- **Vulnerable customers, harassment, discrimination claims:** escalate per policy.

Test each category with scripted conversations and keep them in the regression suite. These rules should be simple and deterministic where possible.

### Voice agents

#### Walk through the latency budget of one turn in a voice agent.
Conversation feels natural when the agent starts replying roughly within a second of the caller finishing. A typical cascaded pipeline budget (illustrative numbers; measure your own):

| Step | Typical range |
|---|---|
| End-of-turn detection (silence / turn model) | 200–600 ms |
| Final speech-to-text result | 100–300 ms (streaming STT) |
| LLM time to first token | 200–800 ms, depending on model and prompt length |
| Text-to-speech time to first audio | 100–300 ms (streaming TTS) |
| Network and telephony | 50–200 ms |

**Ways to reduce it:** stream everything (STT partials, LLM tokens into TTS sentence by sentence); use a fast model and short prompts, with prompt caching; better turn detection instead of fixed long silence timeouts; co-locate services in one region; prefetch customer data at call start; keep tool calls fast or play a short natural filler phrase while a slow tool runs; keep replies short so speaking time is short. Track p50 and p95 per step in production.

#### How do you handle interruptions (barge-in) and end-of-turn detection?
**Barge-in:** keep listening while the agent speaks (with echo cancellation so the agent does not hear itself). When the caller starts speaking meaningfully, stop TTS playback quickly, cancel or discard the rest of the planned reply, and record in the conversation history what was actually spoken before the interruption, so the model does not think the caller heard everything. Ignore backchannels ("mm-hm", "yeah") and background noise, otherwise the agent stops constantly.

**End-of-turn detection:** pure silence timeouts are a trade-off — short timeouts cut people off mid-thought (especially when reading numbers), long ones feel slow. Better: combine voice activity detection with a semantic or audio turn-detection model that predicts whether the utterance is complete; allow longer pauses when the agent has just asked for a number or address.

Test with real recordings: accents, noisy environments, speakerphone, people who pause a lot.

#### How do you capture names, email addresses and order numbers reliably over a phone line?
Speech recognition is weakest exactly on these: rare names, letters that sound alike (B/D/P, M/N), and long digit strings.
- **Avoid asking when possible:** use caller ID to look up the account, or identify by a known order and confirm.
- **Constrain recognition:** use STT features such as keyword boosting or custom vocabulary, and expected formats (order numbers are 8 digits starting with 1).
- **Validate against the system:** look up the captured order number; if it does not exist, try close matches against the customer's orders.
- **Read back and confirm**, digit by digit or in chunks, and use the phonetic alphabet for spelling ("B as in bravo").
- **Ask in small parts:** the email's username, then the domain; offer common domains.
- **Fallback:** send an SMS link to type the detail, or hand off to a human after two failed attempts.

Measure capture accuracy and the number of turns spent on it.

#### Cascaded speech-to-text, LLM and text-to-speech vs. a speech-to-speech model: what are the trade-offs?
**Cascaded (STT → LLM → TTS):**
- Pros: choose best-in-class components separately; full text transcripts for logging, evals and compliance; mature tool calling and guardrails on text; easier to debug which stage failed; flexible voices.
- Cons: more latency from multiple hops; loses tone and emotion from the caller's voice; more parts to operate.

**Speech-to-speech (one model takes audio in and produces audio out):**
- Pros: potentially lower latency, more natural prosody and interruption handling, can pick up the caller's tone.
- Cons: fewer model choices; text-based guardrails, testing and auditing are harder (you need transcripts from a side channel); tool use and instruction following may lag behind the best text models; cost and vendor lock-in vary.

Choose by requirement: regulated, tool-heavy support workflows often favour cascaded pipelines for control and auditability; experiences where naturalness matters most may favour speech-to-speech. Evaluate both on the brand's real call scenarios before deciding, since the field changes quickly.

### Measuring resolution rate, CSAT and business impact

#### How do you define and measure resolution rate honestly?
Agree the definition with the brand in writing, because vendors and teams define it differently.

A strict, defensible definition: **a conversation is resolved by the agent if the customer's issue was handled without a human, and the customer did not come back about the same issue within a set window (e.g. 72 hours to 7 days) on any channel.**

Measurement:
- **Denominator:** all conversations the agent handled, including those where the customer abandoned. Excluding abandonment inflates the number.
- **Signals:** no handoff; no repeat contact on the same issue (match by customer and topic); explicit confirmation ("Did that solve it?") as a supporting signal, not the only one; for action intents, the action actually completed in the backend.
- **Audit:** LLM or human review of a sample to classify resolved / not resolved / deflected without help; report the precision of the automated metric.
- **Segment** by intent, channel and language.

Report resolution next to CSAT and wrong-action rate, so nobody improves resolution by making escalation harder.

#### Containment, resolution and CSAT: what is the difference, and what can go wrong with each?
- **Containment (deflection):** share of conversations that did not reach a human. Risk: it counts customers who gave up or were blocked from reaching a human as successes. High containment can mean a bad experience.
- **Resolution:** share of conversations where the issue was actually solved by the agent. Risk: loose definitions (no handoff = resolved), ignoring repeat contacts, or self-reported "yes" from customers who just wanted to leave.
- **CSAT:** survey score from customers. Risks: low response rates and response bias (very happy or very angry customers answer); scores reflect the outcome (a denied refund scores low even when handled perfectly) and the brand overall, not just the agent.

Use them together with guardrail metrics: repeat contact rate, escalation-after-frustration rate, wrong-action rate, and a sampled quality review. If one rises while the others fall, investigate before celebrating.

#### Resolution rate dropped from 62% to 55% this week. Investigate.
1. **Check the measurement first:** did the definition, tracking, or data pipeline change? Did a channel or tag change?
2. **Segment the drop:** by intent, channel, language, time of day, customer type. Usually one or two segments explain most of it.
3. **Check what changed:** agent releases (instructions, model version, tools), brand changes (new policy, product launch, promotion, outage, shipping delay), knowledge base edits, API errors or latency on a tool.
4. **Check the mix:** a spike in a hard intent (e.g. a delivery delay causing "where is my order" complaints the agent cannot fix) lowers the rate without the agent getting worse.
5. **Read conversations** from the affected segment — 30 to 50 — and label failure reasons.
6. **Act:** roll back a bad release, fix the tool, update knowledge, or explain to the brand that the drop reflects an operational issue (and help them message it).
7. **Add regression tests** for the failure, and report the cause, fix and recovery to the brand.

#### How do you measure customer satisfaction with AI conversations when few customers answer surveys?
- **Improve the survey:** ask one short question in the conversation at the end, not by email later; test wording and timing.
- **Correct for bias:** compare respondents and non-respondents on intent and outcome; weight results by segment.
- **Use behavioural signals for every conversation:** repeat contact, asking for a human, abandonment mid-flow, negative phrases ("this is useless"), escalation after frustration, completion of the action.
- **Model-based quality scoring:** an LLM judge, validated against human ratings and the surveyed conversations, rates each conversation for resolution, accuracy and tone. Report it as an estimate with its agreement level.
- **Compare with human agents** on the same intents using the same measures.
- **Qualitative:** weekly review of a sample with the CX team.

Present a dashboard with surveyed CSAT, estimated CSAT for all conversations, and the behavioural signals together.

#### How do you build the ROI case for a brand's agent deployment?
Use the brand's own numbers and be conservative.
- **Baseline:** contacts per month by intent and channel, cost per human-handled contact (fully loaded), current CSAT, response times, after-hours coverage.
- **Projected impact:** resolution rate per intent (start from pilot data, not marketing claims) × volume = contacts resolved by the agent; savings = resolved contacts × human cost per contact − agent cost (platform, usage, internal staff to maintain it).
- **Other benefits:** 24/7 coverage, instant response, handling seasonal peaks without hiring, consistency, multilingual support, freeing human agents for complex cases. Quantify where possible (e.g. revenue saved by faster cancellation-save offers), otherwise list as qualitative.
- **Costs and risks:** implementation time, integration work, the brand's team time, risk of lower CSAT if quality is poor.
- **Show a range:** conservative, expected, optimistic, and the break-even resolution rate.
- **Plan to verify:** a pilot with a control group (some traffic to humans only) so the result is measured, not assumed.

### Agent regression testing

#### How do you build a regression test suite for a customer-service agent?
- **Sources:** each journey's policy rules (one or more tests per rule), real production conversations (anonymised), past incidents and complaints, red-team attempts, and edge cases from senior support agents.
- **Test format:** a scenario with customer profile, mocked tool data (order states, dates), the customer's goal and messages, and expected outcomes: tools called (and not called) with correct arguments, final state (return created or not), required statements (policy reason, reference number), forbidden statements (unsupported promises), escalation when required.
- **Checks:** deterministic assertions on tool calls and state; an LLM judge with a rubric for tone and explanation quality.
- **Determinism:** fixed tool mocks and clock; run each test several times and track pass rate, because model output varies.
- **Run** on every change to instructions, tools, knowledge or model version, in CI, with a dashboard by journey. Block release on critical failures.
- **Maintain:** every production failure becomes a test; retire tests when policies change.

#### How do you use simulated customers to test an agent?
A simulated customer is an LLM given a persona and a goal that talks to the agent for a full multi-turn conversation.
- **Scenario spec:** goal ("return a damaged blender ordered 20 days ago"), persona (impatient, vague, non-native speaker, tries to get a refund outside policy), hidden facts the simulator reveals only when asked, and success criteria.
- **Environment:** tools backed by mocked or sandbox data matching the scenario.
- **Run many variations** of each scenario (personas, phrasings) to cover the long tail; this finds issues scripted tests miss, like losing track over many turns.
- **Score** each conversation: goal achieved, correct tools and final state, policy followed, turns taken, tone — using deterministic checks plus a judge.
- **Watch for simulator problems:** simulated customers can be too cooperative or too articulate. Calibrate personas against real transcripts, and review a sample manually.

Use simulation for coverage and pre-launch confidence; keep real-transcript tests as the anchor.

#### The brand changes its return policy tomorrow. How do you ship the change safely?
1. **Get the exact policy in writing**, including the effective date and time, what happens to orders placed before the change, and exceptions.
2. **Update the source of truth:** eligibility logic in the tool (with date-based rules for old vs. new orders), knowledge base articles, and agent instructions.
3. **Update tests:** change expected outcomes for affected cases, add tests at the boundary (orders on the cut-over date, day 29 vs. day 31) and for customers quoting the old policy.
4. **Run the full regression suite and simulations** to check nothing else broke.
5. **Deploy with a scheduled switch** at the effective time (feature flag or date logic), not a manual edit at midnight.
6. **Monitor closely after the change:** return-related conversations, escalations, complaints. Review a sample in the first hours.
7. **Tell the brand's human team** what the agent now says, so humans and agent stay consistent.

Point out to the brand that a one-day notice is risky and agree a standard lead time for future policy changes.

#### How do you review production conversations at scale to find failures?
- **Automatic tagging:** run an LLM classifier over every conversation for intent, outcome, escalation reason, sentiment, and suspected issues (wrong information, policy violation, loop, tool error, unnecessary escalation). Validate the classifier against human labels.
- **Prioritised sampling:** human review focuses on high-risk categories (money, safety, complaints), low-scoring conversations, new intents, and a random sample for unbiased quality estimates.
- **Clustering** failed or escalated conversations to find recurring themes and new intents.
- **Dashboards:** failure categories over time, by journey and release.
- **Workflow:** each finding becomes a ticket with an owner and a regression test; fixes are verified on the original conversation.
- **Include the brand:** a weekly review with the CX team; their senior agents spot policy and tone issues quickly.

Respect privacy: redact PII in review tools, restrict access, and follow retention rules.

### Live "build an agent" exercise

#### Live: build a support agent with lookup_order and create_return tools in 60 minutes.
**Plan (say it first):** 10 min tools and data, 20 min agent loop, 15 min tests, 15 min guardrails and polish. Example in Python with the Anthropic SDK; the shape is the same with any provider.

```python
import json, os, datetime as dt
from anthropic import Anthropic
client, MODEL = Anthropic(), os.environ["MODEL"]
TODAY = dt.date(2026, 10, 21)
ORDERS = {"A1": {"customer": "c1", "delivered": "2026-10-10", "items": ["mug"], "returned": False}}

def lookup_order(ctx, order_id):
    o = ORDERS.get(order_id)
    if not o or o["customer"] != ctx["customer_id"]:   # scope enforced in code
        return {"error": "order not found"}
    return {"order_id": order_id, **{k: o[k] for k in ("delivered", "items", "returned")}}

def create_return(ctx, order_id, reason):
    o = ORDERS.get(order_id)
    if not o or o["customer"] != ctx["customer_id"]: return {"error": "order not found"}
    if o["returned"]: return {"status": "already_returned"}           # idempotent
    if (TODAY - dt.date.fromisoformat(o["delivered"])).days > 30:
        return {"error": "return window (30 days) has closed"}
    o["returned"] = True
    return {"status": "created", "return_id": f"R-{order_id}"}

TOOLS = [
 {"name": "lookup_order", "description": "Get details of the customer's order by ID.",
  "input_schema": {"type": "object", "properties": {"order_id": {"type": "string"}}, "required": ["order_id"]}},
 {"name": "create_return", "description": "Create a return. Only call after the customer confirms.",
  "input_schema": {"type": "object", "properties": {"order_id": {"type": "string"},
                   "reason": {"type": "string"}}, "required": ["order_id", "reason"]}}]
FUNCS = {"lookup_order": lookup_order, "create_return": create_return}
SYSTEM = ("You are the support agent for an online shop. Help with order status and returns. "
          "Use tools for all facts; never guess order details or policy. "
          "Confirm with the customer before creating a return. Be brief and friendly.")

def agent_turn(ctx, messages, max_steps=6):
    for _ in range(max_steps):
        r = client.messages.create(model=MODEL, max_tokens=800, system=SYSTEM,
                                   tools=TOOLS, messages=messages)
        messages.append({"role": "assistant", "content": r.content})
        if r.stop_reason != "tool_use":
            return "".join(b.text for b in r.content if b.type == "text")
        results = [{"type": "tool_result", "tool_use_id": b.id,
                    "content": json.dumps(FUNCS[b.name](ctx, **b.input))}
                   for b in r.content if b.type == "tool_use"]
        ctx.setdefault("calls", []).extend(b.name for b in r.content if b.type == "tool_use")
        messages.append({"role": "user", "content": results})
    return "Let me connect you with a teammate."
```
Then demo a status question, a return with confirmation, and an out-of-window return. Points to mention: identity scope and policy are in code, tool errors go back to the model, the loop has a step cap, and state lives outside the model.

#### Live follow-up: add escalation to a human and write tests for it.
Add a tool and instructions:

```python
def handoff_to_human(ctx, reason, summary):
    ctx["handoff"] = {"reason": reason, "summary": summary}   # real version: helpdesk API
    return {"status": "queued", "expected_wait_minutes": 5}

TOOLS.append({"name": "handoff_to_human",
  "description": "Transfer to a human agent. Use when the customer asks for a person, is very upset, "
                 "or the request is outside order status and returns.",
  "input_schema": {"type": "object", "properties": {"reason": {"type": "string"},
                   "summary": {"type": "string"}}, "required": ["reason", "summary"]}})
FUNCS["handoff_to_human"] = handoff_to_human
```

Tests (run each several times because outputs vary; assert on tool calls and state, not exact wording):

```python
def run(msg, customer="c1"):
    ctx, msgs = {"customer_id": customer}, [{"role": "user", "content": msg}]
    return agent_turn(ctx, msgs), ctx

def test_explicit_human_request():
    _, ctx = run("I want to talk to a real person please")
    assert "handoff" in ctx and ctx["handoff"]["summary"]

def test_out_of_scope_escalates():
    _, ctx = run("I want to dispute a charge with my bank, who do I talk to?")
    assert "handoff" in ctx

def test_status_does_not_escalate():
    _, ctx = run("What's the status of order A1?")
    assert "handoff" not in ctx and "lookup_order" in ctx["calls"]

def test_cannot_see_other_customers_order():
    reply, ctx = run("Return order A1, wrong item", customer="c2")
    assert ORDERS["A1"]["returned"] is False
```
Also mention: a deterministic fallback that escalates after repeated tool failures or after the step cap, and checking the summary quality with a judge.

#### Role-play: present the launch plan for a new agent to the brand's VP of Customer Experience.
Speak in their terms: customer experience, risk, team impact, metrics.
1. **Goal:** "Resolve order-status and return contacts (about 45% of volume) instantly, 24/7, at or above today's CSAT."
2. **Scope at launch:** which intents the agent handles, which go to humans, and why.
3. **Safety:** policy enforced in systems, not by the AI's judgement; refund limits; clear escalation with full context; sensitive topics go to people.
4. **Testing done:** number of test scenarios, simulation results, internal pilot results with their team.
5. **Rollout:** internal → 5% of chat traffic → 25% → 100% over N weeks, with go/no-go criteria at each step (resolution, CSAT, wrong-action rate, escalation quality).
6. **Measurement:** the agreed metric definitions and a weekly dashboard and review.
7. **Their team:** how human agents receive handoffs, how they give feedback, how roles shift to complex cases.
8. **Asks:** a policy owner for fast decisions, access to systems, the weekly review slot.

Expect questions like "What if it says something wrong?" — answer with the specific controls and the rollback plan.

### Behavioral (role-specific)

#### Tell me about a time something you launched to customers misbehaved in production.
**What they look for:** fast, calm response; protecting customers first; honest communication; root-cause fix plus a process change so it does not recur.

**STAR outline:**
- **Situation:** [what you launched, who used it, what went wrong and how you found out].
- **Task:** [your responsibility in the response].
- **Action:** [immediate mitigation (rollback, feature flag, disable a tool); how you informed the customer/stakeholders; the root cause; the fix; the test or monitor you added].
- **Result:** [impact contained, time to fix, what changed in the process].

**Example (adapt to your own experience):** "After an update to a support assistant's knowledge base, it started quoting an expired promotion. A support lead flagged it within an hour. I rolled back the knowledge change, confirmed the old behaviour returned, and told the client's CX lead what had happened and how many conversations were affected (about 40). The cause was an old article that the update had re-published. We added an expiry field to articles, a test for promotional claims, and an alert on spikes in discount-related language."

#### Tell me about a time you pushed back on a customer's request for quality or safety reasons.
**What they look for:** you protect the customer's long-term interest and their end users, you use evidence, you offer an alternative, and the relationship survives.

**STAR outline:**
- **Situation:** [the request, who asked, the pressure (deadline, executive)].
- **Task:** [why it was your call or your responsibility to raise].
- **Action:** [the evidence you gathered (tests, data, examples); how you explained the risk in their terms; the alternative you proposed].
- **Result:** [decision taken, outcome, relationship].

**Example (adapt to your own experience):** "A client wanted the support agent to approve all refund requests under a set value automatically, with no checks, to raise the resolution rate before a quarterly review. I pulled data showing that a small group of accounts generated a large share of refund requests, and estimated the cost of fraud under the proposed rule. I proposed automatic approval for customers with a normal refund history and human review for flagged accounts. The client agreed; resolution still rose noticeably and refund losses did not increase."

---

## Prep checklist

1. Use the products: talk to customer-service AI agents on several brands' websites and, where available, the target company's public demos. Note what works, where they escalate, and how they fail.
2. Build a small support agent (order lookup, returns, handoff) with a tool layer that enforces identity and policy in code, plus 15–20 regression tests. Be able to rebuild the core in 45 minutes.
3. Write a simulated-customer harness that runs 10 personas against your agent and scores the conversations.
4. Prepare a one-page agent design template: intents and scope, identity, tools, guardrails, escalation, tests, rollout, metrics. Practise the retailer case out loud in 30 minutes.
5. Learn the metric vocabulary and its pitfalls: resolution, containment/deflection, CSAT, first-contact resolution, average handle time, cost per contact, repeat contact rate.
6. If the role includes voice, build or try a basic voice agent and measure each stage's latency; read about turn detection and barge-in in your chosen platform's docs.
7. Read a few public customer-service AI incidents (for example the 2024 Air Canada chatbot ruling) and prepare what controls would have prevented them.
8. Re-read [06-ai-llm](../06-ai-llm/README.md) sections D, E and H and [03-system-design](../03-system-design/README.md) section B.
9. Prepare stories for: a production incident, pushing back on a customer, improving a metric through iteration, and working with non-technical stakeholders ([10-behavioral](../10-behavioral/ANSWERS.md)).
