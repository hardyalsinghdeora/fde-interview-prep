# Study Plan — Agent Engineer

For engineers preparing for an Agent Engineer (customer-support or voice-agent deployment) loop; assumes about 10–12 hours a week for six weeks.

Read the role overview first: [README.md — The role](./README.md#the-role) and the [typical interview loop](./README.md#typical-interview-loop). This role is narrower and deeper than a general FDE: one kind of system (a customer-facing agent for a brand), judged by business metrics (resolution, CSAT, cost per contact). The loop usually includes an agent design case, a live "build an agent" exercise and a metrics investigation. This plan has you build one support agent across the six weeks and add tests, guardrails, metrics and a launch plan to it each week.

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim areas you rate 4 or 5.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Agent design for support workflows | | In 30 minutes you scope intents, identity checks, tools, guardrails, escalation, tests, rollout and metrics for a retailer's agent. |
| Conversation design | | You can design a refund flow with policy checks, handle multi-intent and angry customers, and adapt it for voice. |
| Tool and API integration | | You design tools for commerce, CRM and ticketing systems, with safe, idempotent write actions and graceful handling of slow APIs. |
| Guardrails and escalation | | You enforce policy and identity in code, not only in the prompt, and design a handoff that passes full context to a human. |
| Agent testing | | You have built regression tests and a simulated-customer harness and can explain how you ship a policy change safely. |
| Metrics and analytics | | You can define resolution honestly, explain containment versus resolution versus CSAT, and investigate a metric drop. |
| Voice (if the role includes it) | | You can walk through a voice turn's latency budget and explain barge-in and end-of-turn detection. |
| Practical coding | | You can build a working tool-using agent with tests in 60 minutes in Python or TypeScript. |
| CX stakeholder communication | | You can present a launch plan to a VP of Customer Experience and explain a failure without jargon. |

## 6-week plan

Assumes ~10–12 hours a week. Pick one imaginary brand (for example an online clothing retailer) and use it all six weeks so your agent grows into a single portfolio piece.

### Week 1 — Product immersion and agent fundamentals

**Goals:** know what good and bad support agents look like, and refresh agent fundamentals.

- [ ] Read the role overview and [how it differs from FDE](./README.md#the-role).
- [ ] Study [06 AI/LLM — D. Agents and tool use](../../topics/06-ai-llm/ANSWERS.md#d-agents-and-tool-use) and [B. Prompt engineering](../../topics/06-ai-llm/ANSWERS.md#b-prompt-engineering).
- [ ] Read [turning SOPs, macros and help-centre articles into agent instructions](./README.md#how-do-you-turn-a-brands-sops-macros-and-help-centre-articles-into-agent-instructions).
- [ ] Company research: [11 — Other companies](../../topics/11-company-specific/ANSWERS.md#other-companies-hiring-fdes-prepare-the-general-sections) and the [research checklist](../../topics/11-company-specific/ANSWERS.md#company-research-checklist-do-for-every-company).
- [ ] Motivation: [01 — AI-company specific motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md#ai-company-specific-motivation).

**Deliverable:** a teardown of five real customer-service AI agents on brand websites (and the target company's public demos, if any): what they handle, when they escalate, how they fail, and one improvement each.

**Mock:** a 30-minute recruiter screen: "why agents", "why customer-facing", how you feel about live customers seeing your work.

**End of week check:** you can describe, from your teardown, three concrete ways support agents fail and what control would prevent each.

### Week 2 — The agent design case and conversation design

**Goals:** a reusable design template and a confident 30-minute design case.

- [ ] Read [Designing an agent for a brand's support workflows — model answers](./README.md#designing-an-agent-for-a-brands-support-workflows-1), especially the [retailer case](./README.md#case-an-online-retailer-wants-an-agent-to-handle-order-status-returns-and-exchanges-design-it) and [choosing launch intents](./README.md#how-do-you-decide-which-intents-the-agent-handles-at-launch-and-which-go-straight-to-humans).
- [ ] Read [Conversation design — model answers](./README.md#conversation-design-1), including the [refund flow](./README.md#design-the-conversation-flow-for-a-refund-request-that-requires-policy-checks).
- [ ] Study [03 System design — B. AI / LLM prompts](../../topics/03-system-design/ANSWERS.md#b-ai--llm-system-design-prompts) (the support chatbot and refund/CRM agent prompts).

**Deliverables:**
1. A one-page agent design template: intents and scope, identity, tools, guardrails, escalation, tests, rollout, metrics.
2. The template filled in for your brand, plus a conversation flow diagram for a refund request.

**Mock:** a 30-minute agent design case: the partner plays the brand's head of support and gives you the retailer prompt.

**End of week check:** you decide which intents go to humans at launch and justify it with risk and volume, not gut feel.

### Week 3 — Tools and integrations: build the agent

**Goals:** a working support agent with a tool layer that enforces identity and policy in code.

- [ ] Read [Tool and API integration with customer systems — model answers](./README.md#tool-and-api-integration-with-customer-systems-1), especially [safe, idempotent write actions](./README.md#how-do-you-make-write-actions-such-as-refunds-and-cancellations-safe-and-idempotent) and [slow or failing order APIs](./README.md#the-brands-order-api-is-slow-and-sometimes-fails-how-does-the-agent-cope-mid-conversation).
- [ ] Read [identity verification](./README.md#how-should-the-agent-verify-a-customers-identity-before-account-specific-actions).
- [ ] Study [08 — A. APIs and integration](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration) and [B. Authentication, authorization and security](../../topics/08-cloud-devops-integration/ANSWERS.md#b-authentication-authorization--security).
- [ ] Read the [live build exercise](./README.md#live-build-a-support-agent-with-lookup_order-and-create_return-tools-in-60-minutes).
- [ ] Practical coding: two JSON and API-style problems from [02 Coding — A](../../topics/02-coding/ANSWERS.md#a-reported-fde-coding-problems).

**Deliverable:** a support agent for your brand with `lookup_order`, `create_return` and a policy-check tool against a mock backend; identity and return-window rules enforced in the tool layer; idempotency keys on write actions; timeouts and a fallback message for slow APIs.

**Mock:** a 45-minute practical coding mock focused on JSON handling and API calls with requirements added midway.

**End of week check:** you can rebuild the core agent loop and two tools in 45 minutes.

### Week 4 — Guardrails, escalation and regression testing

**Goals:** make the agent safe to put in front of a brand's customers, and prove it.

- [ ] Read [Guardrails and escalation to humans — model answers](./README.md#guardrails-and-escalation-to-humans-1), including the [handoff design](./README.md#design-the-handoff-from-the-agent-to-a-human-what-context-is-passed-and-how) and [prompt injection and abuse](./README.md#how-do-you-protect-a-customer-facing-agent-from-prompt-injection-and-abuse).
- [ ] Read [Agent regression testing — model answers](./README.md#agent-regression-testing-1), including [simulated customers](./README.md#how-do-you-use-simulated-customers-to-test-an-agent) and [shipping a policy change safely](./README.md#the-brand-changes-its-return-policy-tomorrow-how-do-you-ship-the-change-safely).
- [ ] Study [06 — E. Evaluation](../../topics/06-ai-llm/ANSWERS.md#e-evaluation-most-important) and [part 2 — H. Safety, security and responsible AI](../../topics/06-ai-llm/ANSWERS-part2.md#h-safety-security-and-responsible-ai).
- [ ] Read the [escalation follow-up exercise](./README.md#live-follow-up-add-escalation-to-a-human-and-write-tests-for-it).

**Deliverables:**
1. Escalation to a human added to your agent, with a structured handoff summary, plus 15–20 regression tests (including policy edge cases and an injection attempt).
2. A simulated-customer harness that runs 10 personas (angry, multi-intent, wrong order number, asks for a human, tries injection) and scores each conversation.

**Mock:** a 60-minute live "build an agent" session from a blank file, followed by 20 minutes adding escalation and tests.

**End of week check:** a return-policy change is a one-line config change that your tests catch if done wrong.

### Week 5 — Metrics, debugging and voice

**Goals:** speak the CX metrics language and investigate drops calmly.

- [ ] Read [Measuring resolution rate, CSAT and business impact — model answers](./README.md#measuring-resolution-rate-csat-and-business-impact-1), especially the [62% to 55% investigation](./README.md#resolution-rate-dropped-from-62-to-55-this-week-investigate) and [the ROI case](./README.md#how-do-you-build-the-roi-case-for-a-brands-agent-deployment).
- [ ] Study [12 — C. LLM-specific troubleshooting](../../topics/12-debugging-troubleshooting/ANSWERS.md#c-llm-specific-troubleshooting) and [B. Production troubleshooting](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios).
- [ ] Read [reviewing production conversations at scale](./README.md#how-do-you-review-production-conversations-at-scale-to-find-failures).
- [ ] If the role includes voice: read [Voice agents — model answers](./README.md#voice-agents-1) and [06 part 2 — G. Production LLM engineering](../../topics/06-ai-llm/ANSWERS-part2.md#g-production-llm-engineering) for latency.

**Deliverables:**
1. A written investigation for "resolution dropped from 62% to 55%": hypotheses in order, the data you would pull, the SQL or script for one check, and the customer update you would send.
2. A metrics glossary in your own words (resolution, containment/deflection, CSAT, first-contact resolution, average handle time, cost per contact, repeat contact rate), with one pitfall each. Voice roles: also a measured latency breakdown of one turn on a voice-agent platform.

**Mock:** a 40-minute metrics investigation: the partner reveals data one piece at a time ("the drop is only on returns", "a policy changed on Tuesday").

**End of week check:** you can explain why containment can rise while customers get worse service.

### Week 6 — Launch plan, CX stakeholders and full loop

**Goals:** the customer-facing round and a full rehearsal.

- [ ] Read the [launch plan role-play](./README.md#role-play-present-the-launch-plan-for-a-new-agent-to-the-brands-vp-of-customer-experience).
- [ ] Read [Behavioral (role-specific) — model answers](./README.md#behavioral-role-specific-1).
- [ ] Study [09 — B. Difficult situations](../../topics/09-customer-scenarios/ANSWERS.md#b-difficult-situations) and [C. Communication](../../topics/09-customer-scenarios/ANSWERS.md#c-communication).
- [ ] Behavioural: [10 — Failure and learning](../../topics/10-behavioral/ANSWERS.md#failure--learning) and [Customer focus](../../topics/10-behavioral/ANSWERS.md#customer-focus).
- [ ] Read about public customer-service AI incidents (the README checklist mentions the 2024 Air Canada chatbot ruling) and note the control that would have prevented each.
- [ ] Prepare questions: [14 — About customers](../../topics/14-questions-to-ask/ANSWERS.md#about-customers).
- [ ] Tick off the role's [Prep checklist](./README.md#prep-checklist).

**Deliverable:** a 10-minute launch plan for your brand's agent aimed at a VP of Customer Experience: scope, staged rollout, guardrails, metrics and targets, review cadence, and what happens when it goes wrong.

**Mock:** a full mock loop: practical coding (45), agent design case (45), live build (60), launch plan role-play (30), behavioural (30).

**End of week check:** you can tell a CX leader why the agent will sometimes fail and how you will catch it, without jargon and without over-promising.

## 2-week crash plan

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and products | Read the role; try three brand support agents | [README — role](./README.md#the-role), [11 — Other companies](../../topics/11-company-specific/ANSWERS.md#other-companies-hiring-fdes-prepare-the-general-sections) |
| 2 | Agent fundamentals | Tool use, agent loops | [06 — D](../../topics/06-ai-llm/ANSWERS.md#d-agents-and-tool-use) |
| 3 | Design case | Retailer case with the template | [Retailer case](./README.md#case-an-online-retailer-wants-an-agent-to-handle-order-status-returns-and-exchanges-design-it) |
| 4 | Conversation design | Refund flow; angry and multi-intent customers | [Conversation design answers](./README.md#conversation-design-1) |
| 5 | Build | Agent with lookup_order and create_return | [Live build exercise](./README.md#live-build-a-support-agent-with-lookup_order-and-create_return-tools-in-60-minutes) |
| 6 | Integration | Idempotent writes, slow APIs, identity | [Tool integration answers](./README.md#tool-and-api-integration-with-customer-systems-1), [08 — A](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration) |
| 7 | Mock day | Design case + live build mock | [Mock schedule](#mock-interview-schedule) |
| 8 | Guardrails | Refund guardrails; injection; handoff | [Guardrails answers](./README.md#guardrails-and-escalation-to-humans-1) |
| 9 | Testing | 15 regression tests; simulated customers | [Regression testing answers](./README.md#agent-regression-testing-1) |
| 10 | Metrics | Resolution vs containment vs CSAT | [Metrics answers](./README.md#measuring-resolution-rate-csat-and-business-impact-1) |
| 11 | Investigation | The 62% to 55% drop, out loud | [Investigation answer](./README.md#resolution-rate-dropped-from-62-to-55-this-week-investigate), [12 — C](../../topics/12-debugging-troubleshooting/ANSWERS.md#c-llm-specific-troubleshooting) |
| 12 | Voice (if relevant) or launch plan | Latency budget, barge-in; or launch plan | [Voice answers](./README.md#voice-agents-1), [Launch plan role-play](./README.md#role-play-present-the-launch-plan-for-a-new-agent-to-the-brands-vp-of-customer-experience) |
| 13 | Stories | Incident story, pushback story; full mock | [Behavioral answers](./README.md#behavioral-role-specific-1), [my story bank](../../my-prep/STORY-BANK.md) |
| 14 | Rest and review | Rebuild the agent core once; re-read template | [README checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–40 | Build on your agent: one tool, guardrail, test or harness feature per day. |
| 40–60 | Read 10–20 conversations (your simulated ones or a public support dataset) and label failures; this is a core skill of the job. |
| 60–80 | One README model answer, then answer a sibling question out loud as if to a CX leader. |
| 80–90 | Update your design template, metrics glossary or mistakes log. |

Weekends: the week's deliverable and the mock.

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Recruiter screen | A friend |
| 2 | Agent design case | A peer, ideally someone with support or operations experience playing the head of support |
| 3 | Practical coding (JSON, APIs) | A peer engineer, or Pramp / interviewing.io |
| 4 | 60-minute live build + escalation follow-up | A peer engineer; record your screen |
| 5 | Metrics investigation | A friend who reveals data in stages from a script you give them |
| 6 | Full loop including launch plan role-play | Two or three partners, one non-technical; Exponent for behavioural practice |

From Week 3, rebuild the agent core from a blank file once a week, timed.

## Fast track for the repo owner's profile

- **Skim:** tool and API integration ([08 — A](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration), [B](../../topics/08-cloud-devops-integration/ANSWERS.md#b-authentication-authorization--security)); contract-first integrations, an API gateway with JWT auth and rate limiting, and CRM/ERP integrations cover most of what agent tools need.
- **Skim:** idempotent write actions; the migration work with idempotency guards and ordering constraints answers the [safe refunds question](./README.md#how-do-you-make-write-actions-such-as-refunds-and-cancellations-safe-and-idempotent) from experience.
- **Biggest gap 1 — production LLM and agent experience.** No evidence of shipped agents, evals or simulated testing. Weeks 3 and 4 are the priority; build the agent and its tests in public.
- **Biggest gap 2 — conversation design and CX metrics.** New vocabulary (resolution, containment, CSAT, handle time). Do the Week 1 teardown and Week 5 glossary carefully.
- **Biggest gap 3 — live coding speed.** DS&A is rusty and only PHP and SQL are evidenced; confirm the language early and rebuild the agent core weekly until it takes under 45 minutes.
- **Agent design case (tools for commerce, CRM, ticketing):** the order/subscription/commerce platform (SKU catalogue, discount approval rules engine, invoicing and payments) is close to a brand's order and refund backend; use it to reason about policy rules and refunds.
- **Guardrails and data access:** row-level access control maps to "the agent only sees this customer's data".
- **Voice round (if any):** the speech-to-text provider evaluation (a contribution, not sole authorship) with a buy-vs-build cost model and data-protection constraints is directly relevant; describe your part precisely.
- **Behavioural (incident, pushback, working with non-technical stakeholders):** delivery leadership of a squad of about four engineers and work with sales, finance and academic teams; the legacy modernisation is a good "launched change, handled fallout" story.
- Keep all stories in [../../my-prep/STORY-BANK.md](../../my-prep/STORY-BANK.md).

## Resources

- Anthropic documentation on tool use (https://docs.anthropic.com) and OpenAI platform documentation (https://platform.openai.com/docs) for function calling.
- Anthropic's engineering article "Building effective agents".
- The τ-bench (tau-bench) paper from Sierra, on evaluating tool-using agents in customer-service domains with simulated users.
- *Conversational Design* by Erika Hall, and *Designing Voice User Interfaces* by Cathy Pearl.
- Documentation of voice-agent platforms such as Vapi, Retell AI or LiveKit Agents, for turn detection, barge-in and latency.
- Public product pages and documentation for Sierra, Decagon and Intercom Fin, to learn how these companies describe resolution and pricing.
- Mock platforms: Pramp, interviewing.io and Exponent.
