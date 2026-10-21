# 12 — Debugging & Troubleshooting Questions

FDEs are the first responders when something breaks at a customer. Some loops include a **debugging round** (Scale AI, Palantir's re-engineering round, Stripe-style bug bash): you get an unfamiliar codebase with failing tests or bugs and must fix them.

---

## A. Live debugging round formats
- [ ] Given a repo with failing unit tests, make them pass and explain each bug.
- [ ] Given buggy code, find all bugs (off-by-one, null handling, mutation, concurrency, wrong types).
- [ ] Re-engineer: improve the readability/performance of a working-but-messy program.
- [ ] Read a stack trace and identify the root cause.
- [ ] Find the memory leak / performance bottleneck in this code.
- [ ] Fix a race condition in this multi-threaded code.
- [ ] This API integration returns wrong results — find out why.
- [ ] Code review: what would you comment on in this PR?

## B. Production troubleshooting scenarios
- [ ] The customer says "the app is slow." How do you investigate?
- [ ] The website is down. Walk me through your first 15 minutes.
- [ ] An API is returning 500 errors intermittently. How do you debug?
- [ ] An API is returning 502/504. What could cause it?
- [ ] The app works locally but fails in the customer's environment.
- [ ] It worked yesterday and nobody changed anything. What do you check?
- [ ] Database queries suddenly became 10x slower.
- [ ] A batch job that usually takes 1 hour now takes 8 hours.
- [ ] Memory usage keeps growing until the service crashes.
- [ ] CPU is at 100% on one server.
- [ ] Disk filled up on a production machine.
- [ ] Users in one region can't reach the service (DNS? CDN? firewall?).
- [ ] SSL/TLS certificate errors at the customer site.
- [ ] Auth/SSO login fails only for some users.
- [ ] Data in the dashboard doesn't match the source system.
- [ ] Duplicate records appeared after a pipeline re-run.
- [ ] Kafka consumer lag keeps growing.
- [ ] Kubernetes pod is in CrashLoopBackOff / OOMKilled / ImagePullBackOff.
- [ ] Deploy succeeded but the new feature isn't showing.
- [ ] A webhook from a third party isn't arriving.
- [ ] A cron job didn't run.

## C. LLM-specific troubleshooting
- [ ] The LLM suddenly gives worse answers. How do you investigate?
- [ ] Responses are getting cut off mid-answer (max tokens? stop sequences?).
- [ ] The model ignores part of the system prompt.
- [ ] The model returns invalid JSON 5% of the time.
- [ ] The RAG system answers from the wrong document.
- [ ] Latency jumped from 2s to 15s.
- [ ] Getting lots of 429 rate-limit errors in production.
- [ ] Costs doubled this month. Find out why.
- [ ] The agent gets stuck in a loop calling the same tool.
- [ ] The agent calls the wrong tool or invents tool arguments.
- [ ] Output differs between staging and production with the same prompt.
- [ ] Streaming breaks behind the customer's proxy/load balancer.

## D. Debugging process questions
- [ ] Walk me through your general debugging process.
- [ ] How do you debug something you can't reproduce?
- [ ] How do you debug in a customer environment where you have limited access?
- [ ] What tools do you use (debuggers, profilers, logs, tracing, `strace`, `tcpdump`)?
- [ ] How do you use `git bisect`?
- [ ] How do you decide between a quick fix and a root-cause fix during an incident?
- [ ] What goes into a good post-mortem?
- [ ] How do you communicate with the customer during an outage?
- [ ] How do you prevent the same bug from happening again?
