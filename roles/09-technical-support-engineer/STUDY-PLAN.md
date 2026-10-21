# Study Plan — Technical Support Engineer

For engineers preparing for Tier 2/3 support, escalation and technical solutions roles; assumes 6 weeks at about 10–12 hours a week, or the 2-week crash plan if an interview is already booked.

Role overview, loop and model answers: [README.md](./README.md). Questions every role gets: [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).

---

## Before you start

Spend 30 minutes rating yourself. Skim what you rate 4–5; spend extra time on 1–2.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Ticket triage and severity | | You can triage ten mixed tickets in 15 minutes and explain severity vs priority and your order. |
| Networking and HTTP fundamentals | | You answer rapid-fire DNS, TCP, TLS and status-code questions correctly and explain what happens when a browser loads a URL. |
| Log reading and reproduction | | Given a log excerpt or a HAR file, you find the failing request and form two testable hypotheses within five minutes. |
| Command-line toolkit | | You use `curl -v`, `dig`, `openssl s_client`, `jq` and `grep | sort | uniq -c` without looking up flags. |
| Auth and API debugging | | You can diagnose intermittent 401s and missing webhooks and prove which side is at fault. |
| Written customer replies | | You write a calm, accurate reply to a furious customer in 15 minutes that a reviewer would send unchanged. |
| Escalation and bug reports | | Your bug reports have repro steps, expected vs actual, impact and evidence, and engineering does not need to ask a follow-up. |
| Knowledge base and process | | You can write a KB article in a standard structure and explain how you would reduce ticket volume. |

---

## 6-week plan

### Week 1 — Fundamentals for the technical screen

**Goals:** answer rapid-fire systems questions confidently; this round filters most candidates.

- [ ] Read [The role](./README.md#the-role) and [Typical interview loop](./README.md#typical-interview-loop); note whether your target role is Tier 1 or Tier 2/3.
- [ ] Read [13-cs-fundamentals](../../topics/13-cs-fundamentals/README.md) sections [Networking](../../topics/13-cs-fundamentals/README.md#networking), [Operating systems](../../topics/13-cs-fundamentals/README.md#operating-systems) and [Databases](../../topics/13-cs-fundamentals/README.md#databases), with [answers](../../topics/13-cs-fundamentals/ANSWERS.md).
- [ ] Read [08-cloud-devops-integration section F (Linux and scripting)](../../topics/08-cloud-devops-integration/README.md#f-linux--scripting) and [section B (auth)](../../topics/08-cloud-devops-integration/README.md#b-authentication-authorization--security), with [answers](../../topics/08-cloud-devops-integration/ANSWERS.md).
- [ ] Read [Which command-line tools do you use most](./README.md#which-command-line-tools-do-you-use-most-in-support-work-and-for-what).

**Deliverables:**
1. A personal flashcard set (50+ cards) on HTTP status codes, DNS record types, TLS handshake steps, OAuth flows and common Linux commands.
2. A "toolkit log": run `curl -v`, `dig`, `openssl s_client` and `jq` against three real public endpoints and write one line on what each output tells you.

**Mock:** 30-minute rapid-fire technical screen: a peer reads 25 fundamentals questions; you answer each in under a minute.

**End of week check:** you can explain what happens from typing a URL to seeing a page, including DNS, TCP, TLS and HTTP, in five minutes.

### Week 2 — Triage, severity and SLAs

**Goals:** pass the ticket triage exercise.

- [ ] Work through [Ticket triage and severity](./README.md#ticket-triage-and-severity-1): [the eight-ticket exercise](./README.md#here-are-eight-tickets-in-the-queue-triage-them-what-order-what-severity-and-what-first-action-for-each), [severity levels](./README.md#how-do-you-define-severity-levels-sev1-to-sev4-and-what-decides-the-level), [severity vs priority](./README.md#what-is-the-difference-between-severity-and-priority) and [SLAs and SLOs](./README.md#explain-sla-slo-first-response-time-and-time-to-resolution-how-do-you-protect-slas-when-the-queue-spikes).
- [ ] Read ["your product is broken"](./README.md#a-ticket-just-says-your-product-is-broken-what-information-do-you-ask-for-first) and [two Sev1s, one engineer](./README.md#two-sev1-tickets-arrive-at-the-same-time-and-you-are-the-only-engineer-on-shift-what-do-you-do).
- [ ] Read the [Tier 1 questions](../COMMON-QUESTIONS.md#tier-1--asked-in-almost-every-loop) and [Debugging and operations](../COMMON-QUESTIONS.md#debugging-and-operations) in COMMON-QUESTIONS.md.

**Deliverables:**
1. A self-made triage pack: ten mixed tickets (outage, data issue, how-to, feature request, billing, security) with your severity, order and first action for each.
2. A one-page severity matrix (Sev1 to Sev4) with examples and response targets.

**Mock:** 30-minute live triage: a peer gives you a different set of tickets; you triage in 15 minutes and explain your order for 15.

**End of week check:** you triage ten tickets in 15 minutes and can justify every severity.

### Week 3 — Reproducing bugs and reading logs

**Goals:** pass the troubleshooting / log-reading round with a hypothesis-driven method.

- [ ] Work through [Reproducing bugs and reading logs](./README.md#reproducing-bugs-and-reading-logs-1): [cannot reproduce](./README.md#walk-me-through-how-you-reproduce-a-bug-that-you-cannot-reproduce-on-your-machine), [the log excerpt](./README.md#here-is-a-log-excerpt-what-happened-and-what-do-you-tell-the-customer), [HAR files](./README.md#what-is-a-har-file-how-do-you-read-one-and-what-can-it-tell-you), [intermittent 401s](./README.md#a-customers-api-calls-fail-with-401-errors-some-of-the-time-how-do-you-diagnose-it) and [missing webhooks](./README.md#a-customer-says-webhooks-are-not-arriving-how-do-you-prove-whether-the-problem-is-on-our-side-or-theirs).
- [ ] Read [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) sections [A (round formats)](../../topics/12-debugging-troubleshooting/README.md#a-live-debugging-round-formats), [B (production scenarios)](../../topics/12-debugging-troubleshooting/README.md#b-production-troubleshooting-scenarios) and [D (process)](../../topics/12-debugging-troubleshooting/README.md#d-debugging-process-questions), with [answers](../../topics/12-debugging-troubleshooting/ANSWERS.md).
- [ ] If the company's product is AI, add [12 section C (LLM troubleshooting)](../../topics/12-debugging-troubleshooting/README.md#c-llm-specific-troubleshooting).

**Deliverables:**
1. Record a HAR file in browser dev tools on a real site; write a half-page analysis: slowest request, any failures, what you would tell a customer.
2. A small log-parsing script (Python or shell) that takes an access log and prints error counts by endpoint and status code. See the practical problems in [02-coding section A](../../topics/02-coding/README.md#a-reported-fde-coding-problems).

**Mock:** 45-minute log-reading mock: a peer gives you a log excerpt and a symptom; you think aloud, state hypotheses, ask for evidence and conclude.

**End of week check:** you can find the failing request in a HAR file and explain a 401 or webhook problem with evidence, not guesses.

### Week 4 — Written customer communication

**Goals:** pass the written reply exercise; tone matters as much as accuracy.

- [ ] Work through [Written customer communication](./README.md#written-customer-communication-1): [furious customer with production down](./README.md#write-a-reply-to-a-customer-whose-production-system-is-down-and-who-is-furious), [feature does not exist](./README.md#write-a-reply-telling-a-customer-that-the-feature-they-need-does-not-exist), [fix date you cannot commit to](./README.md#a-customer-demands-a-fix-date-you-cannot-commit-to-how-do-you-reply) and [the customer misconfigured it](./README.md#the-customer-caused-the-problem-through-their-own-misconfiguration-how-do-you-tell-them).
- [ ] Read [adjusting for non-technical vs senior readers](./README.md#how-do-you-adjust-a-reply-for-a-non-technical-reader-vs-a-senior-engineer) and [when to move to a call](./README.md#when-do-you-move-from-the-ticket-to-a-call-or-screen-share).
- [ ] Read [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) sections [B (difficult situations)](../../topics/09-customer-scenarios/README.md#b-difficult-situations) and [C (communication)](../../topics/09-customer-scenarios/README.md#c-communication), with [answers](../../topics/09-customer-scenarios/ANSWERS.md).

**Deliverables:**
1. Three timed replies (15 minutes each): furious customer with an outage, "feature doesn't exist", "you misconfigured it". Get a peer to mark tone, clarity and accuracy, then rewrite.
2. One incident status update series: first update, 30-minute update, resolution notice.

**Mock:** 30-minute angry-customer call role-play, followed by 15 minutes writing the follow-up email.

**End of week check:** your replies acknowledge impact, state facts, give a next step and a time for the next update, and never blame the customer.

### Week 5 — Escalation, bug reports and the knowledge base

**Goals:** show you work well with engineering and reduce ticket volume, not just close tickets.

- [ ] Work through [Escalating to engineering](./README.md#escalating-to-engineering-1): [when to escalate](./README.md#when-do-you-escalate-to-engineering-and-when-do-you-keep-working-the-ticket-yourself), [the bug report](./README.md#write-the-bug-report-you-would-send-to-engineering-for-a-confirmed-bug), ["working as intended"](./README.md#engineering-says-the-behaviour-is-working-as-intended-but-the-customer-is-blocked-what-do-you-do) and [SLA about to breach](./README.md#engineering-is-slow-to-respond-on-an-escalation-and-the-sla-is-about-to-breach-what-do-you-do).
- [ ] Work through [Knowledge base, metrics and process](./README.md#knowledge-base-metrics-and-process-1), including [KB article structure](./README.md#write-a-knowledge-base-article-for-a-common-issue-what-structure-do-you-use), [reducing ticket volume](./README.md#how-do-you-reduce-ticket-volume-rather-than-just-closing-tickets-faster) and [credentials pasted into a ticket](./README.md#a-customer-pastes-credentials-or-personal-data-into-a-ticket-what-do-you-do).
- [ ] Read [09 section D (product feedback)](../../topics/09-customer-scenarios/README.md#d-product-feedback--scale).

**Deliverables:**
1. One KB article and one bug report, in the README's formats, for a real issue you have hit.
2. A follow-the-sun handoff note for three open tickets, using [the handoff note answer](./README.md#how-do-you-write-a-handoff-note-for-the-next-shift-in-a-follow-the-sun-team) as a guide.

**Mock:** 30-minute mock where a peer plays an engineering lead who says "working as intended"; you negotiate a workaround and a path for the customer.

**End of week check:** an engineer could act on your bug report without asking you a question.

### Week 6 — Product hands-on, behavioral and full loop

**Goals:** know the company's product from a user's view and rehearse the whole loop.

- [ ] Sign up for the company's free tier, follow the quickstart, break something on purpose, and note the error messages and docs you get.
- [ ] Read the company's public status page history and two post-incident reports; run the [company research checklist](../../topics/11-company-specific/README.md#company-research-checklist-do-for-every-company).
- [ ] Work through [Role-specific behavioral](./README.md#role-specific-behavioral-1), including [why support engineering](./README.md#why-support-engineering-and-where-do-you-want-it-to-lead).
- [ ] Read [10-behavioral](../../topics/10-behavioral/README.md) sections [Customer focus](../../topics/10-behavioral/README.md#customer-focus) and [Failure and learning](../../topics/10-behavioral/README.md#failure--learning); write four stories into [STORY-BANK.md](../../my-prep/STORY-BANK.md).
- [ ] Tick off the [Prep checklist](./README.md#prep-checklist) and pick questions from [14-questions-to-ask](../../topics/14-questions-to-ask/README.md#about-the-team--product).

**Deliverables:**
1. A one-page "friction log" from your hands-on session with the product: what broke, what the error said, what the docs said.
2. Four STAR stories: hard technical fix, angry customer turned around, influenced a product fix, mistake made and corrected.

**Mock:** full mock loop over two days: fundamentals screen (30 min), log-reading (45 min), triage plus written reply (45 min), behavioral (45 min).

**End of week check:** you have done each round type at least twice and can talk about the company's product from first-hand use.

---

## 2-week crash plan

About 2 hours a day.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and networking | Read role and loop; DNS, TCP, TLS, HTTP flashcards | [README](./README.md#the-role), [13 Networking](../../topics/13-cs-fundamentals/README.md#networking) |
| 2 | Linux and auth | CLI toolkit on real endpoints; OAuth, API keys, SSO | [08 F](../../topics/08-cloud-devops-integration/README.md#f-linux--scripting), [08 B](../../topics/08-cloud-devops-integration/README.md#b-authentication-authorization--security) |
| 3 | Triage | Ten-ticket triage in 15 minutes; severity matrix | [Triage](./README.md#ticket-triage-and-severity-1) |
| 4 | Logs and HAR | Log excerpt answer; record and read a HAR file | [Reproducing and logs](./README.md#reproducing-bugs-and-reading-logs-1) |
| 5 | 401s and webhooks | Diagnose both scenarios aloud | [401 answer](./README.md#a-customers-api-calls-fail-with-401-errors-some-of-the-time-how-do-you-diagnose-it) |
| 6 | Mock 1 | Fundamentals rapid-fire plus log-reading mock | [12 section B](../../topics/12-debugging-troubleshooting/README.md#b-production-troubleshooting-scenarios) |
| 7 | Rest / catch-up | — | — |
| 8 | Written replies | Three timed replies; peer review | [Written communication](./README.md#written-customer-communication-1) |
| 9 | Escalation | Bug report, "working as intended", SLA breach | [Escalating](./README.md#escalating-to-engineering-1) |
| 10 | KB and metrics | Write a KB article; support metrics and how they mislead | [KB and process](./README.md#knowledge-base-metrics-and-process-1) |
| 11 | Product hands-on | Free tier, quickstart, break it, read status page | [11 checklist](../../topics/11-company-specific/README.md#company-research-checklist-do-for-every-company) |
| 12 | Mock 2 | Triage plus written reply plus angry-customer call | [09 section C](../../topics/09-customer-scenarios/README.md#c-communication) |
| 13 | Stories | Four STAR stories; answer on shifts and on-call | [Behavioral](./README.md#role-specific-behavioral-1), [STORY-BANK.md](../../my-prep/STORY-BANK.md) |
| 14 | Light review | Flashcards, checklist | [Prep checklist](./README.md#prep-checklist) |

---

## Daily routine

| Minutes | Activity |
|---|---|
| 0–15 | Flashcards: 20 fundamentals questions (networking, HTTP, Linux, auth) |
| 15–45 | Main study item for the week (triage, logs, replies, escalation) |
| 45–70 | Hands-on: CLI on a real endpoint, a HAR file, a log-parsing script, or a timed reply |
| 70–85 | One model answer or one story said aloud |
| 85–90 | Note one thing to fix tomorrow |

---

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Rapid-fire fundamentals (30 min) | Peer reading questions from 13-cs-fundamentals |
| 2 | Live ticket triage (30 min) | Peer, ideally someone who has worked a support queue |
| 3 | Log-reading and troubleshooting (45 min) | Peer engineer; record it |
| 4 | Angry-customer call plus written follow-up (45 min) | Peer, plus a second reader to mark the email |
| 5 | "Working as intended" negotiation (30 min) | Peer playing an engineering lead |
| 6 | Full loop (four rounds over two days) | Mix of peers; optionally a coached mock on interviewing.io or Exponent |

Options: support-engineering communities and ex-colleagues are the best source of realistic triage and reply exercises. Pramp, interviewing.io and Exponent can cover the light coding and technical screens. Have every written exercise read by someone else.

---

## Fast track for the repo owner's profile

- **Positioning first:** with about 15 years of experience and a Principal title, target senior, escalation or Tier 3 roles (or "Technical Solutions Engineer" style roles), and prepare a clear answer to "why support engineering, and where does it lead?".
- **Skip or skim:** SQL basics, REST API behaviour, JWT authentication and rate limiting. You built an API gateway with these, so 401 and 429 scenarios should be familiar.
- **Skim:** escalation and incident process. Leading a squad and owning business-critical modules gives you real escalation experience from the other side.
- **Biggest gap 1 — tool fluency under time pressure:** HAR files and the `curl`/`dig`/`openssl`/`jq` toolkit are not evidenced in your profile; do Weeks 1 and 3 in full.
- **Biggest gap 2 — timed written replies to external customers:** your stakeholders have been mostly internal. Do every Week 4 exercise with a timer and a reviewer.
- **Biggest gap 3 — queue work and shifts:** triage across many unrelated customers is different from owning a few modules. Practise the triage pack until it is automatic, and decide your honest answer on on-call and shifts.
- **Round mapping — troubleshooting:** the bidirectional database bridge (idempotency guards, ordering constraints, reconciliation) is your strongest "data doesn't match" and "duplicates after a re-run" story.
- **Round mapping — escalation and bug reports:** the legacy modernisation (PHP 5.3 to 7.3 user migration) and integration work with CRM/ERP systems give you concrete cross-team debugging stories.
- **Round mapping — knowledge base and process:** onboarding plans and mentoring map to "how do you reduce ticket volume" and "write a KB article". Keep all stories in [STORY-BANK.md](../../my-prep/STORY-BANK.md).

---

## Resources

- *Debugging: The 9 Indispensable Rules* by David J. Agans — a short, practical debugging method.
- MDN Web Docs on HTTP (https://developer.mozilla.org/en-US/docs/Web/HTTP) — status codes, headers, caching, CORS.
- Chrome DevTools documentation on the Network panel (https://developer.chrome.com/docs/devtools/) — recording and reading HAR files.
- *The Linux Command Line* by William Shotts — free online; shell, pipes and text processing.
- Julia Evans' zines (for example on networking, DNS and HTTP) — clear short explanations of tools you will be asked about.
- Knowledge-Centered Service (KCS) from the Consortium for Service Innovation — the common standard for support knowledge bases.
- *Site Reliability Engineering* (Google), incident management chapters — free at https://sre.google/books/
