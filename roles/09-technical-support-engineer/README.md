# Technical Support Engineer

Customer-facing engineers who diagnose, reproduce and resolve technical problems in a product, escalate real bugs to engineering, and write clearly to customers under time pressure. A common entry route into Forward Deployed Engineering.

## The role

**Day to day**

- Work a queue of tickets (email, portal, chat, Slack Connect) against response and resolution SLAs.
- Triage: decide severity, ask for the missing information, and route or own each ticket.
- Reproduce customer issues: build a minimal repro, read logs, traces, HAR files and config, and isolate customer error vs. product bug.
- Escalate confirmed bugs to engineering with a clean bug report, then keep the customer updated until the fix ships.
- Join incident calls for Sev1s; write status updates.
- Write and maintain knowledge-base (KB) articles, runbooks and internal notes; feed patterns back to product and docs.
- Write small scripts and tools (log parsers, API checkers, data fixes) to speed up the team.
- Take on-call or follow-the-sun shifts in many companies.

**Common titles and aliases:** Technical Support Engineer (TSE), Support Engineer, Product Support Engineer, Escalation Engineer, Technical Solutions Engineer, Technical Services Engineer, Cloud Support Engineer, Production Support Engineer, Application Support Engineer, Customer Reliability Engineer, Tier 2/Tier 3 Support.

**Companies that hire for it (examples):** cloud providers (AWS Cloud Support Engineer, Google Cloud Technical Solutions Engineer, Microsoft Support / Escalation Engineer), developer-infrastructure and SaaS companies such as Stripe, Datadog, MongoDB (Technical Services Engineer), Elastic, GitLab, Cloudflare, Atlassian, Twilio, Snowflake and Databricks. Titles and scope vary a lot between companies; check the job description for whether the role is Tier 1 (routing) or Tier 2/3 (deep debugging).

**How it differs from FDE**

| | Technical Support Engineer | Forward Deployed Engineer |
|---|---|---|
| Trigger for work | Customer reports a problem | Customer has a goal to achieve |
| Unit of work | Ticket / incident (hours to days) | Deployment / project (weeks to months) |
| Builds new things? | Small tools, scripts, repros | Production integrations, apps, pipelines |
| Main output | Resolution, bug report, KB article | Working system at the customer |
| Customers per week | Many (tens of tickets) | Few (one to three accounts) |
| Success metrics | SLA attainment, CSAT, time to resolution | Customer outcome, adoption, expansion |

The skills overlap heavily: debugging unfamiliar systems, reading logs, explaining technical issues to non-engineers, and staying calm with unhappy customers. Many FDEs and solutions engineers started in support.

**Who it suits:** people who enjoy puzzles in systems they did not build, like variety, write well, and get satisfaction from unblocking someone quickly. Less suited to people who want long uninterrupted build time.

**Skills mix**

| Skill | Weight |
|---|---|
| Coding (scripting, reading code, small tools) | Medium |
| Debugging / troubleshooting / log reading | High |
| Customer communication (especially written) | High |
| Systems knowledge (networking, Linux, HTTP, databases, cloud) | High |
| Sales / commercial | Low |
| Product feedback and documentation | Medium |

**Career paths:** senior / staff support engineer, escalation engineer, support engineering manager, site reliability engineer (SRE), solutions engineer / solutions architect, technical account manager, forward deployed engineer, software engineer (often via an internal transfer after fixing bugs in the codebase), product manager for developer tools.

## Typical interview loop

Rounds vary by company; this is the typical/reported shape, not a guarantee.

| Round | What it tests | How to prepare |
|---|---|---|
| Recruiter screen | Motivation, shift/on-call availability, written English | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) |
| Technical screen | Linux, networking, HTTP, DNS, TLS, SQL basics, "how does X work" | [13-cs-fundamentals](../../topics/13-cs-fundamentals/README.md), [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) |
| Troubleshooting / log-reading round | Hypothesis-driven debugging, reading logs and stack traces | [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) and the questions below |
| Ticket triage exercise (live or take-home) | Prioritisation, severity judgment, first actions | Questions below |
| Written reply exercise | Tone, clarity, accuracy, de-escalation in writing | Questions below |
| Light coding / scripting (some companies) | Parse a log, call an API, write a small script | [02-coding](../../topics/02-coding/README.md) (practical problems), [08 Linux & scripting](../../topics/08-cloud-devops-integration/README.md) |
| Customer role-play (some companies) | Live call with an "angry customer" interviewer | [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) |
| Hiring manager / behavioral | Ownership, empathy, working with engineering | [10-behavioral](../../topics/10-behavioral/README.md) |

## Common rounds — use the shared sections

- [12-debugging-troubleshooting](../../topics/12-debugging-troubleshooting/README.md) — the most important shared section. Prioritise section B (production troubleshooting) and C (LLM troubleshooting if the product is AI).
- [13-cs-fundamentals](../../topics/13-cs-fundamentals/README.md) — networking (DNS, TCP, TLS, HTTP status codes) and operating systems come up as rapid-fire questions.
- [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) — APIs, auth (OAuth, API keys, SSO/SAML), Linux & scripting.
- [07-data-and-sql](../../topics/07-data-and-sql/README.md) — SQL concepts and basic queries; support engineers often query internal data to investigate.
- [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) — difficult situations and communication; most map directly onto support.
- [10-behavioral](../../topics/10-behavioral/README.md) — customer focus, conflict, failure.
- [02-coding](../../topics/02-coding/README.md) — only the practical problems (parsing, string handling, API calls); deep DS&A is rare for support roles.

## Role-specific questions

### Ticket triage and severity

- [ ] Here are eight tickets in the queue. Triage them: what order, what severity, and what first action for each?
- [ ] How do you define severity levels (Sev1 to Sev4), and what decides the level?
- [ ] What is the difference between severity and priority?
- [ ] Explain SLA, SLO, first-response time and time to resolution. How do you protect SLAs when the queue spikes?
- [ ] A ticket just says "your product is broken." What information do you ask for first?
- [ ] Two Sev1 tickets arrive at the same time and you are the only engineer on shift. What do you do?

### Reproducing bugs and reading logs

- [ ] Walk me through how you reproduce a bug that you cannot reproduce on your machine.
- [ ] Here is a log excerpt. What happened, and what do you tell the customer?
- [ ] What is a HAR file, how do you read one, and what can it tell you?
- [ ] A customer's API calls fail with 401 errors some of the time. How do you diagnose it?
- [ ] A customer says webhooks are not arriving. How do you prove whether the problem is on our side or theirs?
- [ ] Which command-line tools do you use most in support work, and for what?
- [ ] How do you follow one customer request across several services to find where it failed?
- [ ] A customer says performance got worse after upgrading to the new version. How do you investigate?
- [ ] Users at one customer suddenly cannot log in through SAML single sign-on. How do you troubleshoot it?

### Escalating to engineering

- [ ] When do you escalate to engineering, and when do you keep working the ticket yourself?
- [ ] Write the bug report you would send to engineering for a confirmed bug.
- [ ] Engineering says the behaviour is "working as intended" but the customer is blocked. What do you do?
- [ ] Engineering is slow to respond on an escalation and the SLA is about to breach. What do you do?
- [ ] How do you handle customer communication during an active incident?

### Written customer communication

- [ ] Write a reply to a customer whose production system is down and who is furious.
- [ ] Write a reply telling a customer that the feature they need does not exist.
- [ ] How do you de-escalate an angry customer in writing?
- [ ] How do you adjust a reply for a non-technical reader vs. a senior engineer?
- [ ] A customer demands a fix date you cannot commit to. How do you reply?
- [ ] The customer caused the problem through their own misconfiguration. How do you tell them?
- [ ] When do you move from the ticket to a call or screen-share?

### Knowledge base, metrics and process

- [ ] Write a knowledge-base article for a common issue. What structure do you use?
- [ ] How do you decide which issues deserve a KB article or a docs change?
- [ ] How do you reduce ticket volume rather than just closing tickets faster?
- [ ] Which support metrics matter, and how can they mislead?
- [ ] How do you write a handoff note for the next shift in a follow-the-sun team?
- [ ] A customer pastes credentials or personal data into a ticket. What do you do?
- [ ] How do you use AI tools in support work, and where do you not trust them?

### Role-specific behavioral

- [ ] Tell me about a time you solved a hard technical problem for a customer.
- [ ] Tell me about a time you turned a recurring support issue into a product or docs fix.
- [ ] Tell me about a time you made a mistake in a customer communication.
- [ ] Tell me about a time you turned an angry customer around.
- [ ] Why support engineering, and where do you want it to lead?

## Model answers

### Ticket triage and severity

#### Here are eight tickets in the queue. Triage them: what order, what severity, and what first action for each?

Say your method out loud before sorting, then apply it ticket by ticket.

1. **Scan everything first** (one minute). Do not answer the first ticket you open.
2. **Score each on impact and urgency:** is production down or data at risk? How many users? Is there a workaround? Is it a paid tier with a contractual SLA? How close is the SLA breach?
3. **Look for clusters.** Three tickets saying "login fails" from different customers in the last 20 minutes is probably one incident, not three tickets. Raise an incident and link them.
4. **Order:** Sev1s and possible incidents first, then tickets near SLA breach, then Sev2, then how-to questions and feature requests.
5. **First action per ticket**, for example:
   - Outage cluster: declare incident, page on-call, send a holding reply to all affected.
   - Data-loss report: acknowledge within minutes, ask for IDs and time window, escalate.
   - Single-user bug with workaround: send workaround, set Sev3.
   - "How do I...?": link docs or KB, close or set Sev4.
   - Feature request: thank, log in the feedback tool, set expectations.
   - Billing question: route to billing team with context.
6. **Acknowledge everyone** within the response SLA, even if it is "we are looking at this".

Interviewers score the reasoning and the clustering, not one exact order.

#### How do you define severity levels (Sev1 to Sev4), and what decides the level?

Severity measures **business impact on the customer**. A common scheme:

| Level | Definition | Example |
|---|---|---|
| Sev1 / Critical | Production down or unusable, data loss or security issue, no workaround | All API calls return 500; data being deleted |
| Sev2 / High | Major function impaired, significant users affected, workaround is poor | Exports fail for one large workspace |
| Sev3 / Medium | Partial or minor loss of function, workaround exists | One report shows wrong totals; manual export works |
| Sev4 / Low | Question, cosmetic issue, feature request | "How do I rotate API keys?" |

What decides it: scope (how many users, which environment), whether production or data is affected, whether a workaround exists, and security exposure. Customers often over-state severity; check the facts politely and re-classify with an explanation, but never downgrade a possible data-loss or security issue until it is ruled out. Severity can also be raised later as you learn more.

#### What is the difference between severity and priority?

- **Severity** = how bad the impact is (a property of the problem).
- **Priority** = the order in which we work on it (a decision by the team).

They usually move together but not always:

- High severity, lower priority: a crash in a deprecated feature used by one trial account, with a known workaround.
- Low severity, high priority: a typo in the checkout page for the largest customer before their launch, or a cosmetic bug a strategic account's CEO is complaining about.

Priority also takes into account SLA deadlines, customer tier, number of tickets affected, and engineering capacity. Keeping the two separate stops arguments: you can agree on the facts (severity) and then discuss the order (priority).

#### Explain SLA, SLO, first-response time and time to resolution. How do you protect SLAs when the queue spikes?

- **SLA (Service Level Agreement):** contractual commitment, often with penalties, e.g. "first response to Sev1 within 1 hour, 24x7."
- **SLO (Service Level Objective):** internal target, usually tighter than the SLA, e.g. "respond to Sev1 within 15 minutes."
- **First-response time (FRT):** time until the first human, useful reply. Most support SLAs are written on this.
- **Time to resolution (TTR):** time until the issue is solved. Rarely a contractual guarantee, because fixes can depend on engineering.

Protecting SLAs during a spike:

1. Check if the spike is one incident; if so, link tickets to a single incident and post one status-page update.
2. Sort by time-to-breach, not by arrival time.
3. Send meaningful first responses fast (acknowledge, state what you need, give next update time).
4. Use macros/templates for repeated issues, but personalise the first line.
5. Ask for help early: pull in another region or a swarming channel.
6. Afterwards, review why the spike happened and what would prevent it.

#### A ticket just says "your product is broken." What information do you ask for first?

Reply quickly, with a short, specific list. Asking for ten things causes delays, so ask for what changes your next step:

1. **What were you trying to do, and what happened instead?** Exact error message or screenshot.
2. **When did it start?** Did it ever work? Anything change (deploy, upgrade, config, network)?
3. **Scope:** all users or some? One environment or all? Which region?
4. **Identifiers:** account/workspace ID, request ID, timestamp with timezone, affected object IDs.
5. **Environment:** version, SDK/language, browser/OS if UI.
6. **Impact:** is production blocked? This decides severity.

Before replying, check what you can find yourself: account health, recent errors in our logs for that account, status page, known issues. A reply that says "I can see 4xx errors on your account starting 10:42 UTC; can you confirm whether you changed your API key around then?" is much better than a generic questionnaire.

#### Two Sev1 tickets arrive at the same time and you are the only engineer on shift. What do you do?

1. **Acknowledge both immediately** with a short holding reply and a time for the next update.
2. **Check whether they are related.** Same region, same feature, same time? If so, it is one incident: page on-call engineering and work it once.
3. **If unrelated, get help.** Page the on-call escalation, support manager or another region per the escalation policy. Being alone on shift does not mean handling two Sev1s alone.
4. **Prioritise by impact** while help arrives: confirmed data loss or security beats degraded performance; a full outage beats partial.
5. **Keep both customers informed** at the promised cadence even if one is waiting.
6. Write clear notes so whoever joins can pick one up immediately.

What the interviewer wants: no silent waiting, use of the escalation path, and impact-based priority.

### Reproducing bugs and reading logs

#### Walk me through how you reproduce a bug that you cannot reproduce on your machine.

"Works for me" means my environment differs from theirs. Find the difference.

1. **Collect the exact inputs:** request payload, request ID, timestamps, account settings, version, SDK, region, browser, feature flags.
2. **Check our telemetry for their request** before trying to reproduce: logs, traces, error tracking. Often the answer is there.
3. **Match their environment:** same product version and plan, same feature flags, same data shape (size, unicode, nulls, time zone), same SDK version, same region.
4. **Change one variable at a time** until it breaks: data size, concurrency, auth type, network path (proxy, VPN, corporate TLS inspection).
5. **Consider timing and state:** race conditions, caches, rate limits, eventual consistency.
6. **If still no repro,** ask for a screen-share, HAR file, debug logs or a minimal script from the customer.
7. **Write the minimal repro** once found: smallest set of steps that fails every time. This is what engineering needs.

If you never reproduce it, escalate with all evidence and say clearly "not reproduced; evidence suggests X".

#### Here is a log excerpt. What happened, and what do you tell the customer?

Example exercise:

```
2026-03-02T10:41:58Z INFO  req=7f3a account=acme POST /v1/exports size=1.2GB
2026-03-02T10:42:01Z WARN  req=7f3a worker=exp-4 memory=91% of 2048MiB
2026-03-02T10:42:03Z ERROR req=7f3a worker=exp-4 killed: OOM
2026-03-02T10:42:03Z INFO  req=7f3a retry=1 worker=exp-2
2026-03-02T10:42:07Z ERROR req=7f3a worker=exp-2 killed: OOM
2026-03-02T10:42:07Z ERROR req=7f3a status=500 msg="export failed after 2 attempts"
```

How to read it aloud:

1. Follow one request ID through time.
2. Note the first abnormal line (memory 91%), not just the last error.
3. Cause: a 1.2 GB export exceeds the worker memory limit; retries fail the same way, so it is deterministic.
4. Check scope: are other accounts with large exports affected?

Customer reply: confirm you found the cause ("exports above roughly 1 GB currently fail because of a memory limit"), give a workaround (split the export by date range), say you have raised it with engineering, and give the next update time. Internally: bug report with the request ID, and possibly a docs note on the size limit.

#### What is a HAR file, how do you read one, and what can it tell you?

A HAR (HTTP Archive) file is a JSON recording of every network request a browser made: URL, method, headers, cookies, request and response bodies, status codes and timing. Customers export it from browser developer tools (Network tab, "Save all as HAR").

How to read it: open it in a HAR viewer or the browser's dev tools import, then:

- Filter for failed requests (4xx/5xx, or status 0 / blocked).
- Find the first failure in time; later ones are often consequences.
- Check timing columns: DNS, connect, TLS, waiting (server time), download.
- Check request headers: auth header present? Correct domain? Cookies sent?
- Check response bodies for error messages and request IDs you can look up in our logs.

What it reveals: CORS errors, blocked third-party scripts, expired sessions, proxies stripping headers, slow server responses vs. slow client network.

Important: a HAR file contains cookies and tokens. Ask customers to sanitise it or tell them it will be handled securely, and delete it when done.

#### A customer's API calls fail with 401 errors some of the time. How do you diagnose it?

Intermittent 401 means some requests are authenticated and some are not. Look for what differs between the passing and failing ones.

1. **Pull failing and successful request IDs** in the same time window and compare them.
2. **Common causes:**
   - Token expiry: OAuth access tokens expire (often after an hour) and the client does not refresh, or refreshes too late.
   - Several keys in use: one server or one environment has an old or revoked key.
   - Load-balanced client fleet: one instance has a stale config.
   - Clock skew: signed requests or JWTs rejected because the client clock is off.
   - Key rotation: key rotated without updating all services.
   - Wrong environment: test key sent to production endpoint.
   - Proxy stripping the `Authorization` header on some paths.
3. **Check our side:** auth service errors, recent deploys, regional issues.
4. **Ask the customer** for the failing request IDs, which hosts send them, and their token refresh logic.

Reply with what you found in their traffic (for example, "all failures come from tokens older than 60 minutes"), the fix, and a link to the auth docs.

#### A customer says webhooks are not arriving. How do you prove whether the problem is on our side or theirs?

Use our delivery logs as evidence. For each missing event:

1. **Was the event created?** If not, the problem is upstream (the trigger did not happen, or the event type is not subscribed).
2. **Did we attempt delivery?** Check the webhook attempt log: timestamp, URL, response code, latency.
3. **What did their endpoint return?**
   - No attempt: subscription disabled, wrong event types, endpoint auto-disabled after repeated failures.
   - Timeout: their endpoint is too slow; advise returning 2xx fast and processing asynchronously.
   - 4xx/5xx: their server rejected it; often signature verification fails (wrong secret, or body parsed before verifying).
   - TLS/connection errors: expired certificate, firewall, IP allowlist missing our IP ranges.
   - 2xx: we delivered; the issue is in their processing.
4. **Test with them:** send a test event, or point the webhook temporarily at a request-inspection endpoint they control.

Reply with facts: "We attempted 42 deliveries between 09:00 and 10:00 UTC; all received a 403 from your endpoint." That ends blame quickly and kindly.

#### Which command-line tools do you use most in support work, and for what?

- `curl -v` / `curl -w` — reproduce API calls, see headers, TLS handshake and timing. Example: `curl -sv -o /dev/null -w '%{http_code} %{time_total}\n' https://api.example.com/health`
- `dig` / `nslookup` — DNS resolution; `dig +short CNAME app.customer.com`.
- `openssl s_client -connect host:443 -servername host` — certificate chain and expiry.
- `nc -vz host port`, `traceroute`/`mtr` — reachability and network path.
- `grep`, `awk`, `sort | uniq -c | sort -rn` — count errors by type in logs.
- `jq` — filter JSON logs and API responses: `jq 'select(.status>=500) | .request_id' app.log`.
- `tail -f`, `less +F` — follow logs live.
- `kubectl logs/describe/get events` — if the product is deployed into the customer's Kubernetes.
- `psql`/`mysql` — read-only queries for investigation.

The point to make: pick the tool that gives you evidence quickly, and share the exact command with the customer so they can run it too.

#### How do you follow one customer request across several services to find where it failed?

1. **Get a correlation ID.** Most systems return a request ID or trace ID in a response header (for example `X-Request-Id`). Ask the customer for it, or find it by account and timestamp.
2. **Search centralised logs** (Datadog, Splunk, Elasticsearch, CloudWatch) for that ID across all services.
3. **Use distributed tracing** (OpenTelemetry, Jaeger, Datadog APM) if available: the trace shows each service span, its duration and the first span with an error.
4. **Find the first failure,** not the last; downstream errors often just report that something upstream failed.
5. **Check dependencies** around that span: database, queue, third-party API, timeouts.
6. **If there is no trace ID,** narrow by account, endpoint and a tight time window, then line up timestamps (watch for timezone and clock differences).

If correlation is hard, note it as feedback: propagating a request ID end to end is a cheap, high-value fix for support.

#### A customer says performance got worse after upgrading to the new version. How do you investigate?

1. **Quantify it.** Which operations, how much slower (p50 vs. p95/p99), since when exactly? Get before/after numbers or request IDs.
2. **Confirm the correlation.** Did the slowdown start at the upgrade time, or did something else change (data volume, traffic, infrastructure)?
3. **Read the release notes and changelog** for the version: new defaults, removed caches, changed query paths, new dependency versions.
4. **Reproduce:** same operation on old vs. new version with the same data. If you can show a clear difference, you have a regression.
5. **Profile where time goes:** server-side timing, slow-query logs, client-side timing, network.
6. **Check other customers** on the same version for the same symptom.
7. **Offer mitigation:** a config flag that restores the old behaviour, or a rollback path if supported.
8. **Escalate as a regression** with before/after numbers, version numbers and a repro. Regressions usually get high priority.

Keep the customer updated with the facts you have, and do not promise a rollback that the product cannot support.

#### Users at one customer suddenly cannot log in through SAML single sign-on. How do you troubleshoot it?

"Suddenly" and "all users at one customer" point to something that changed in the SSO configuration, usually on one side of the trust.

1. **Scope:** all users or some? IdP-initiated, SP-initiated or both? Since when? Did anything change (certificate, IdP app, domain, our release)?
2. **Get the exact error** from our side (SSO logs for that account) and from the user's browser. A SAML tracer extension or a HAR file captures the SAML response; decode it (Base64) to read the assertion.
3. **Check the usual causes:**
   - **Expired or rotated signing certificate:** the IdP (Okta, Entra ID) rolled its certificate and our side still has the old one. This is the most common "suddenly, everyone" cause.
   - **Clock skew:** assertion `NotBefore` / `NotOnOrAfter` outside the allowed window.
   - **Audience or ACS URL mismatch:** entity ID or reply URL changed, for example after a domain change.
   - **NameID or attribute change:** the IdP now sends a different identifier (email vs. employee ID), so users no longer match existing accounts.
   - **Users not assigned** to the app in the IdP, or a conditional access policy now blocks them.
4. **Unblock while you fix:** if the product allows it, a break-glass admin login (password or backup method) so the customer's admin can update settings.
5. **Reply with evidence:** "The assertion is signed with a certificate that expired at 00:00 UTC; please upload the new certificate in Settings > SSO."

Afterwards suggest prevention: certificate-expiry reminders, metadata URL auto-refresh if supported, and a KB article.

### Escalating to engineering

#### When do you escalate to engineering, and when do you keep working the ticket yourself?

Escalate when:

- You have **evidence of a product bug** (reproducible behaviour that contradicts the docs or spec).
- It needs **access or changes you do not have**: database fixes, code changes, infrastructure.
- It is a **Sev1, security or data-integrity** issue; escalate early, in parallel with your own investigation.
- You have **exhausted your runbooks** and a reasonable time-box (set by team policy) has passed.

Keep working it when:

- It is a configuration, usage or integration problem on the customer side.
- The answer is in docs, KB or past tickets.
- You have not yet collected the basic information engineering will ask for.

A good rule: escalate the problem, not the ticket. Arrive with a repro, logs, scope and your hypothesis. Engineering trusts support engineers who escalate rarely but with complete evidence, and those escalations get handled faster.

#### Write the bug report you would send to engineering for a confirmed bug.

```
Title: CSV export returns 500 for exports > ~1 GB (OOM in export worker)

Severity: Sev2 — 3 customers affected, workaround exists (split by date)
Customers: acme (ticket #48121), globex (#48133), initech (#48140)
First seen: 2026-03-02 10:42 UTC (after release 4.18.0 on 2026-03-01)

Summary
Exports larger than roughly 1 GB fail after two retries. Export workers
are killed by OOM; worker memory limit is 2048 MiB.

Steps to reproduce
1. Workspace with ≥ 5M rows in table `events` (staging workspace ws_test_77)
2. POST /v1/exports {"format":"csv","table":"events"}
3. Observe 500 after ~10s

Expected: export completes (worked in 4.17.x for the same data)
Actual:   500 "export failed after 2 attempts"

Evidence
- request IDs: 7f3a, 81bc, 9d02
- logs: worker exp-4 memory 91% then "killed: OOM"
- 4.18.0 changelog mentions "export now buffers rows for sorting"

Hypothesis: new buffering loads full result into memory.
Ask: confirm cause; ETA for fix; is there a flag to disable buffering?
```

Key properties: one issue per report, reproducible steps, expected vs. actual, scope and impact, evidence, and a clear ask.

#### Engineering says the behaviour is "working as intended" but the customer is blocked. What do you do?

1. **Understand the intent.** Ask engineering why it was designed this way. There may be a good reason (security, data consistency) that you can explain to the customer.
2. **Check the docs.** If the docs say otherwise, it is at least a docs bug, and the customer was reasonably misled; say so internally.
3. **Find a workaround** with engineering: different API, config, script, or a supported alternative.
4. **Make the business case** if many customers hit it: count tickets, name affected accounts and revenue, and take it to the product manager as a feature request, not as an argument with engineering.
5. **Tell the customer honestly:** this is current designed behaviour, here is why, here is the workaround, and you have logged their need with product. Do not blame engineering in front of the customer.
6. **Escalate through account management** if a key account is truly blocked and the workaround is not acceptable.

#### Engineering is slow to respond on an escalation and the SLA is about to breach. What do you do?

1. **Check the escalation is complete.** If it lacks repro steps or impact, engineering may have deprioritised it; fix that first.
2. **Make the impact visible:** update severity if justified, state the SLA deadline and customer impact in the escalation.
3. **Use the defined path:** ping the on-call engineer, then the engineering manager or support escalation manager. Follow the policy rather than messaging random engineers.
4. **Keep the customer informed.** The response SLA is usually about communication, so send an honest update: what you know, what is happening, next update time. That keeps the SLA even if the fix is not ready.
5. **Keep investigating in parallel;** a workaround you find yourself may unblock the customer.
6. **Afterwards,** raise the pattern in a retro if escalations regularly stall, with data, not blame.

#### How do you handle customer communication during an active incident?

- **First update fast:** acknowledge the issue publicly (status page) and to affected tickets within minutes, even before the cause is known.
- **One source of truth:** status page plus a single incident ticket that others link to, so updates are consistent.
- **Fixed cadence:** for example every 30 minutes for Sev1, even if the update is "no change, still investigating".
- **Each update contains:** what is affected, what customers see, what we are doing, workaround if any, next update time.
- **Avoid speculation** about root cause or ETA until confirmed. Say "we are investigating a database issue" only if true.
- **Closing message:** resolution time, impact window, whether action is needed by the customer, and that a post-incident report will follow if that is policy.
- **Follow-up:** send the root-cause summary, and proactively contact the most affected customers.

Coordinate with the incident commander; support owns customer communication, engineering owns the fix.

### Written customer communication

#### Write a reply to a customer whose production system is down and who is furious.

```
Hi Dana,

I understand your checkout has been failing for the last 40 minutes and
this is costing you orders. I am treating this as our highest priority
(Sev1) and I am working on it now with our on-call engineering team.

What we know so far:
- Since 14:05 UTC, payment requests from your account return 502.
- Other customers in the EU region are seeing the same errors, so this
  looks like an issue on our side, not your configuration.
- Status page: https://status.example.com/incidents/1234

What you can do now:
- Requests can be retried safely with the same idempotency key.

I will update you by 15:00 UTC, or sooner if anything changes. If it helps,
I can join a call with your team; reply here or call the Sev1 line.

Alex
Support Engineering
```

Why it works: acknowledges impact in their terms, takes ownership, gives facts not excuses, offers a workaround, commits to a next update time, and offers a call. No "sorry for any inconvenience" filler, no blame, no promise of a fix time you do not know.

#### Write a reply telling a customer that the feature they need does not exist.

```
Hi Sam,

Thanks for explaining what you are trying to do: export audit logs
automatically to your S3 bucket every night.

Our product does not support scheduled exports to S3 today. I want to be
clear about that so you can plan.

Two options that customers in a similar situation use:
1. The Audit Log API (docs: <link>) with a small scheduled job.
   I have attached a 30-line Python example that runs as a cron job or
   AWS Lambda and writes to S3.
2. Our Splunk / Datadog integration, if you already use either.

I have also logged your request with our product team, including your use
case and the compliance requirement behind it. I cannot promise a date,
but I will update this ticket if it is planned.

Happy to help you set up option 1 if you'd like.

Priya
```

Principles: say "no" clearly and early, show you understood the goal, give a practical alternative, log the request honestly without implying a commitment.

#### How do you de-escalate an angry customer in writing?

- **Respond fast.** Silence makes anger worse.
- **Acknowledge the impact specifically:** "your team could not run payroll this morning", not "sorry for the inconvenience".
- **Do not argue with emotion or match tone.** Ignore insults; answer the problem.
- **Take ownership** of the next step even if the fault is not ours: "I will own this until it is resolved."
- **Facts and plan:** what you know, what you are doing, when they will hear next.
- **Give them control:** options, a workaround, a direct line, an offer to call.
- **Keep it short and readable:** bullets, no jargon, no long apologies.
- **Do not over-promise.** A broken promise turns an angry customer into a lost one.
- **Follow through** exactly at the time you said.
- **Know when to change channel:** if the thread is going in circles, offer a call.
- **Escalate internally** (account manager, support manager) if they threaten to churn or the relationship needs senior attention.

If the customer is abusive, follow company policy and involve your manager; empathy does not mean accepting abuse.

#### How do you adjust a reply for a non-technical reader vs. a senior engineer?

Same facts, different level of detail and framing.

**Non-technical reader (manager, business user):**
- Lead with impact and what they need to do: "Your reports from 1 to 3 March show lower totals than expected. No data was lost; it will be corrected by Friday. You don't need to do anything."
- Avoid jargon or explain it in one phrase.
- Short, no logs.

**Senior engineer:**
- Lead with the technical cause and evidence: "Aggregation job double-applied the timezone offset for UTC+X workspaces after release 4.18; affected rows are in `daily_rollups` for 2026-03-01 to 03-03. Backfill runs Thursday 02:00 UTC; idempotent."
- Include request IDs, versions, commands, links to API docs.
- They value precision and honesty about unknowns.

If the ticket has both audiences, write a two-line summary on top and technical details below. Ask early who will read it.

#### A customer demands a fix date you cannot commit to. How do you reply?

Do not invent a date and do not hide behind "no ETA". Give what you can commit to.

```
Hi Lee,

I understand you need to plan around this. I cannot give a reliable
fix date yet, because engineering is still confirming the root cause,
and I don't want to give you a date that later moves.

What I can commit to:
- I will update you every day at 16:00 UTC until it is fixed.
- As soon as engineering confirms the fix and target release, I will
  send you the date.
- In the meantime, the workaround below avoids the error for new jobs.

If a date matters for a decision on your side (for example a launch),
tell me the date and I will make sure engineering knows.
```

Commit to the process (updates, workaround), not the outcome. Asking for their deadline gives you useful information to raise priority internally.

#### The customer caused the problem through their own misconfiguration. How do you tell them?

Focus on the fix, not on fault. Make it easy for them to accept.

- Lead with the solution: "I found the cause, and it is a quick change."
- Describe the configuration neutrally: "The webhook secret in your settings doesn't match the one your server uses to verify signatures" rather than "you entered the wrong secret".
- Show the evidence so they can check it themselves.
- Give exact steps to fix and to verify.
- If our docs, UI or error messages made the mistake easy, say so internally and log a docs or product improvement; misconfigurations that repeat are product problems.
- Close warmly: "Let me know once it is updated and I'll confirm deliveries are succeeding."

Never write "user error" or anything that implies blame in a ticket the customer can see; tickets get forwarded to their managers.

#### When do you move from the ticket to a call or screen-share?

Move to a call when:

- **Back-and-forth is slow:** after two or three rounds without progress.
- **The environment is complex** and screenshots are not enough (custom network, many components).
- **The customer is very upset** or the account is at risk; voice de-escalates better.
- **It is a Sev1** and real-time coordination is faster.
- **You need to observe behaviour** you cannot reproduce.

Stay in writing when the answer is clear and documented, when the customer prefers async or is in a far timezone, or when an exact written record matters.

During a call: have an agenda, ask for screen-share, take notes, avoid making changes on their system yourself unless policy allows it, and afterwards **summarise in the ticket**: what you saw, what was changed, next steps and owners. The written record is what the next engineer and the customer will rely on.

### Knowledge base, metrics and process

#### Write a knowledge-base article for a common issue. What structure do you use?

Structure: title as the symptom the customer searches for, then environment, cause, resolution, verification.

```
# Webhook deliveries fail with "signature verification failed"

Applies to: Webhooks API, all plans. Last reviewed: 2026-03.

## Symptoms
- Your endpoint returns 400 or 401 to our webhooks.
- Your logs show "signature verification failed" or "invalid signature".

## Cause
The signature is computed over the raw request body. It fails when:
1. Your framework parses the JSON before you verify (body changes).
2. You use the secret of a different endpoint or environment.
3. Your server clock is more than 5 minutes off (timestamp check).

## Resolution
1. Verify using the raw body. Example for Express:
   app.post('/hooks', express.raw({type: 'application/json'}), handler)
2. Copy the secret for this endpoint from Settings > Webhooks.
3. Sync the clock with NTP.

## How to verify
Send a test event from Settings > Webhooks > Send test. Expect 2xx.

## Still failing?
Open a ticket with the event ID and your endpoint's response.
```

Write in the customer's words, one issue per article, steps that can be copied, and a review date so it does not go stale.

#### How do you decide which issues deserve a KB article or a docs change?

- **Frequency:** the same question or fix appears in several tickets (a common threshold is three in a month). Use ticket tags or search to count.
- **Cost per ticket:** issues that take long investigation deserve an article even if rarer.
- **Self-serviceable:** the customer can fix it without our access. If only we can fix it, write an internal runbook instead.
- **Severity of getting it wrong:** security setup, data deletion, migrations.
- **Docs vs. KB:** if the product docs are wrong or missing, fix the docs (the source of truth). KB is for symptoms, errors, troubleshooting and workarounds.
- **Product fix instead:** if a clear error message or UI change would prevent the ticket, file that too; the article is a stopgap.

Measure: views, "was this helpful", and whether tickets on that topic go down after publishing.

#### How do you reduce ticket volume rather than just closing tickets faster?

Treat tickets as data about product defects.

1. **Tag and categorise** every ticket (feature area, root cause: bug, docs gap, UX confusion, customer config).
2. **Report the top drivers** monthly: "18% of tickets are SSO setup questions."
3. **Fix at the source:**
   - Docs gaps: write or fix docs, add examples.
   - Confusing UX or vague error messages: file product issues with ticket counts. Better error messages are one of the cheapest volume reducers.
   - Bugs: link tickets to bug reports so engineering sees the real impact.
4. **Self-service:** KB articles, in-product help, diagnostic tools (e.g. a "test connection" button).
5. **Proactive outreach:** contact customers affected by a known issue before they open tickets.
6. **Check results:** did the category shrink after the fix?

This is also the work that most often gets support engineers promoted or moved into engineering or FDE roles.

#### Which support metrics matter, and how can they mislead?

| Metric | What it shows | How it misleads |
|---|---|---|
| First-response time | Speed of acknowledgement | Gamed by empty "we're looking" replies |
| Time to resolution | Speed of solving | Pushes premature closure; reopened tickets hide it |
| CSAT | Customer happiness per ticket | Low response rate; biased to extremes; rates the outcome not the engineer |
| Backlog / ticket age | Queue health | Hides severity mix |
| Reopen rate | Quality of fixes | Depends on closure policy |
| Escalation rate | Self-sufficiency | Too low can mean sitting on real bugs |
| Tickets per customer / contact rate | Product quality | Grows with user base; normalise by active users |

Use several together, segment by severity and customer tier, and pair numbers with ticket review. A team that reduces total volume may see its averages get worse because only hard tickets remain, which is a good outcome.

#### How do you write a handoff note for the next shift in a follow-the-sun team?

The next engineer should be able to act without reading the whole thread.

```
HANDOFF — Ticket #48121 (acme, Sev2) — 2026-03-02 17:00 UTC

Status: Waiting on engineering (BUG-9921), workaround given.
Problem: CSV exports > ~1 GB fail with 500 (OOM in export worker).
Done so far:
- Confirmed cause in logs (req 7f3a). 2 other customers affected.
- Customer is splitting exports by month; confirmed working.
Customer expectations: daily update at 16:00 UTC; next due tomorrow.
Next actions:
1. Check BUG-9921 for fix version; tell customer.
2. If no engineering reply by 10:00 UTC, ping #export-oncall.
Watch out: customer CTO is cc'd; contract renewal this month.
Contacts: dana@acme (technical), eng owner: @rk
```

Include: current status, what was tried, what was promised and when, next action, risks, and links. Update the ticket itself too, not only chat.

#### A customer pastes credentials or personal data into a ticket. What do you do?

1. **Do not use or copy the secret.** Never test with a customer's production credentials unless policy explicitly allows it.
2. **Redact it** from the ticket using the ticketing tool's redaction feature, and from any attachments (HAR files, logs, screenshots), following company policy.
3. **Tell the customer** promptly and kindly: the secret was exposed in a ticket and they should rotate it now. Give the exact rotation steps.
4. **Check where else it went:** email notifications, Slack integrations, internal escalations. Redact there if possible.
5. **Report it** to your security or privacy team if policy requires (for example, personal data under GDPR, or payment card data, which must not be stored in tickets).
6. **Prevent repeats:** ticket form warnings, automatic secret detection, KB guidance on sanitising logs and HAR files.

Show that you know secrets in tickets are a real security incident, not a formality.

#### How do you use AI tools in support work, and where do you not trust them?

Use them to speed up the routine parts, and keep a human accountable for anything sent to a customer or changed in a system.

**Where they help:**
- Summarising long ticket threads and drafting handoff notes.
- Searching KB, docs and past tickets for similar issues.
- Drafting replies, which I then check for accuracy and tone.
- Explaining unfamiliar stack traces or code, and writing small scripts (log parsers, `jq` filters).
- Tagging tickets and spotting clusters for trend reports.

**Where I do not trust them blindly:**
- **Facts about our product:** they can invent settings, API fields or limits. Every technical claim is checked against docs, code or a test.
- **Commitments:** dates, SLAs, refunds or roadmap statements are never sent from an AI draft without a human decision.
- **Sensitive data:** only approved tools, and no customer secrets or personal data pasted into tools the company has not approved.
- **Root cause:** a plausible explanation is a hypothesis to test, not a conclusion.

If the company has an AI support agent for customers, I would watch its escalations and wrong answers, and turn them into KB fixes and evaluation cases.

### Role-specific behavioral

#### Tell me about a time you solved a hard technical problem for a customer.

**What they are looking for:** structured debugging under uncertainty, persistence, communication while investigating, and evidence that you went to the root cause.

**STAR outline:**
- **Situation:** [customer, product area, impact, why it was hard: intermittent / no repro / several systems].
- **Task:** [your responsibility; SLA or deadline].
- **Action:** [hypotheses you formed, data you collected, how you narrowed it down, who you involved, how you kept the customer updated].
- **Result:** [resolution, time saved, customer outcome, follow-up fix or KB article].

**Example (adapt to your own experience):** A customer's file uploads failed for about 5% of users with a generic network error. Nothing showed in our server logs for the failures. I asked for HAR files from two affected users and saw the requests never reached us; both users were on the same corporate VPN. Testing with the customer's IT team showed their proxy blocked uploads above 10 MB. I gave IT the exact domain and size rule to allowlist, sent a short explanation for their users, and wrote a KB article on proxy-related upload failures. The issue was resolved in two days, and similar tickets afterwards were closed in under an hour using the article.

#### Tell me about a time you turned a recurring support issue into a product or docs fix.

**What they are looking for:** noticing patterns, using data to persuade, influencing engineering and product without authority, and measuring the result.

**STAR outline:**
- **Situation:** [the recurring issue and how often it happened].
- **Task:** [why you decided it was worth fixing at the source].
- **Action:** [how you quantified it (ticket counts, time spent), who you took it to, what you proposed (error message, docs, UI change, tool), what you built or wrote yourself].
- **Result:** [ticket volume change, time saved, customer impact].

**Example (adapt to your own experience):** I noticed many tickets about failed API calls were caused by customers sending test-mode keys to the live endpoint, and the API returned only "401 Unauthorized". I tagged a month of tickets, found about 30, and estimated the team's time spent. I proposed to the API team that the error message say "This is a test-mode key; use a live key for this endpoint", and wrote the docs change. After it shipped, tickets in that category dropped sharply over the next quarter.

#### Tell me about a time you made a mistake in a customer communication.

**What they are looking for:** honesty, fast correction, ownership, and a process change so it does not repeat.

**STAR outline:**
- **Situation:** [what you sent and why it was wrong: wrong information, wrong customer, promised a date, wrong tone].
- **Task:** [limit the damage and restore trust].
- **Action:** [how quickly you noticed, how you corrected it with the customer, who you informed internally].
- **Result:** [customer reaction, what you changed: checklist, template, review step].

**Example (adapt to your own experience):** I told a customer a bug fix would ship "in Thursday's release" based on a comment in the engineering ticket. The fix was pulled from the release after a failed test. I emailed the customer on Thursday morning before they noticed, explained the change, gave the workaround again and the new target. They appreciated the early warning. Since then I only share release dates once the fix is merged and tagged for a release, and I phrase them as targets.

#### Tell me about a time you turned an angry customer around.

**What they are looking for:** empathy under pressure, ownership of the next step, clear communication, and a result you can point to, not just "they calmed down".

**STAR outline:**
- **Situation:** [the customer, why they were angry (outage, repeated issue, slow responses), how it showed (escalation to management, threat to churn)].
- **Task:** [your role: own the ticket, restore trust].
- **Action:** [how you acknowledged the impact, what facts you gathered, the plan and update cadence you committed to, who you involved, and how you kept every promise].
- **Result:** [issue outcome, the customer's reaction, any lasting change such as a KB article or a process fix].

**Example (adapt to your own experience):** A customer's admin opened his fourth ticket in two weeks about failed scheduled reports, writing in capitals and copying his VP. Each earlier ticket had been closed with a workaround by a different engineer. I took ownership of all four, called him to apologise for the repeated back-and-forth, and promised a daily update. I found that the failures matched a timeout on large reports, got engineering to confirm it as a bug, and set up a temporary schedule split that worked. The fix shipped ten days later; he replied to the closing ticket thanking us, and later agreed to be a beta tester. Since then I check a customer's ticket history before replying, so repeat issues get one owner.

#### Why support engineering, and where do you want it to lead?

Answer honestly in three parts:

1. **Why you like the work:** solving problems in real systems you did not build, variety, direct impact on customers, satisfaction of unblocking someone.
2. **Why this company:** the product and its users, technical depth of the support team (Tier 2/3, engineering-adjacent), how support feeds product.
3. **Where it leads:** it is fine to say you see it as a path to solutions engineering, FDE, SRE or software engineering, as long as you show you value support itself and will do it well for a meaningful period. Interviewers worry about people who want to leave in six months.

Example framing (adapt to your own experience): "I am at my best when debugging a problem in an unfamiliar system with a customer waiting. Your support team works directly with engineering and owns real fixes, which is the depth I want. Longer term I'd like to grow into a customer-facing engineering role such as solutions or forward deployed engineering, and I think the best preparation is seeing hundreds of real customer problems first."

## Prep checklist

- [ ] Practise a ticket triage exercise: write 8–10 mixed tickets (outage, data issue, how-to, feature request, billing) and triage them in 15 minutes, explaining your order out loud.
- [ ] Write three customer replies with a timer (15 minutes each): furious customer with an outage, "feature doesn't exist", and "you misconfigured it". Have someone critique tone and clarity.
- [ ] Learn to read a HAR file: record one in browser dev tools, find the slowest request and any failures.
- [ ] Practise the command-line toolkit: `curl -v`, `dig`, `openssl s_client`, `jq`, `grep | sort | uniq -c`, on real public endpoints.
- [ ] Study the company's product: sign up for a free tier, follow the quickstart, break something on purpose, and see what error messages and docs you get.
- [ ] Read the company's public status page history and a few post-incident reports to understand their incident communication style.
- [ ] Write one KB article and one bug report in the formats above for an issue you have actually hit.
- [ ] Review HTTP status codes, DNS, TLS, OAuth/API keys, and SSO basics from [13-cs-fundamentals](../../topics/13-cs-fundamentals/README.md) and [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md).
- [ ] Prepare four STAR stories: hard technical fix, turned an angry customer around, influenced a product fix, made and corrected a mistake.
- [ ] Prepare an honest answer on shifts, on-call and weekend coverage, and on your longer-term career direction.
