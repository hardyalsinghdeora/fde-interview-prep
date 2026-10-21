# 03 — System Design Questions

FDE system design is **applied**: expect real customer constraints (legacy systems, on-prem, regulated data, messy inputs, small team, tight deadline) instead of pure "design Twitter at Google scale".

---

## A. Applied / customer-context design prompts
- [ ] Design a system to integrate our platform with a customer's legacy ERP/CRM (SAP, Salesforce, Oracle).
- [ ] Design a data platform that ingests from 500 heterogeneous data sources.
- [ ] Design a real-time dashboard for a hospital to track bed capacity across 50 hospitals.
- [ ] Design a pipeline that ingests hospital records and flags patients at risk of readmission.
- [ ] Design a system to track military/logistics assets in real time with intermittent connectivity.
- [ ] Design a supply-chain visibility platform for a manufacturer with 200 suppliers.
- [ ] Design a fraud-detection system for a bank's card transactions.
- [ ] Design an anti-money-laundering (AML) alert triage system.
- [ ] Design a predictive maintenance system for aircraft/factory equipment using sensor data.
- [ ] Design an inventory forecasting system for a retail chain.
- [ ] Design a system to unify customer records from 10 systems (entity resolution / master data).
- [ ] Design an incident/case management system for a police department or emergency services.
- [ ] Design a 911 dispatch optimisation system using call, traffic, and ambulance GPS data.
- [ ] Design a document processing pipeline for insurance claims (OCR → extraction → review).
- [ ] Design an audit-logging system for a regulated customer (immutable, queryable).
- [ ] Design a multi-tenant SaaS deployment where some customers require single-tenant or on-prem.
- [ ] Design a system that works in an air-gapped environment.
- [ ] Design a data-sharing system between organisations with fine-grained access control.
- [ ] Design a migration from a customer's on-prem database to the cloud with zero downtime.
- [ ] Design a reporting system that the customer's analysts can self-serve.
- [ ] Design a CDC (change data capture) pipeline from Postgres to a warehouse.
- [ ] Design a webhook delivery system with retries and guaranteed delivery.
- [ ] Design an integration layer that syncs data bi-directionally between two SaaS tools.
- [ ] Design a permissioning model (RBAC/ABAC) for a platform used by multiple agencies.
- [ ] Design a system for a field-sales team that must work offline and sync later.

## B. AI / LLM system design prompts
- [ ] Design a customer-support chatbot for an enterprise using its knowledge base (RAG).
- [ ] Design an internal enterprise search / "ask your docs" assistant with permissions.
- [ ] Design a RAG system over 10M documents; how does it change at 100x data?
- [ ] Design an AI agent that books travel / processes refunds / updates CRM records.
- [ ] Design a multi-agent system for a complex workflow (research → draft → review).
- [ ] Design a coding assistant for a company's internal codebase.
- [ ] Design a contract review / legal document analysis assistant.
- [ ] Design a call-centre copilot that listens live and suggests answers.
- [ ] Design a voice agent for appointment scheduling (latency budget!).
- [ ] Design a document-extraction system (invoices, forms) with LLMs and human review.
- [ ] Design an LLM evaluation platform / eval harness for a customer.
- [ ] Design an LLM gateway: routing between models, rate limits, caching, cost tracking, fallbacks.
- [ ] Design a system that classifies and routes 1M support tickets/day with an LLM.
- [ ] Design a text-to-SQL analytics assistant over a company's warehouse.
- [ ] Design a content-moderation pipeline using LLMs.
- [ ] Design a personalised recommendation system with LLM re-ranking.
- [ ] Design guardrails for an LLM app in a regulated industry (healthcare, finance).
- [ ] Design a fine-tuning pipeline for a customer's domain data.
- [ ] Design a semantic cache for LLM responses.
- [ ] Design an MCP server that exposes a customer's internal tools to Claude/ChatGPT.
- [ ] Design a batch-processing system that runs an LLM over 50M records cheaply.
- [ ] Design observability for an LLM application (traces, costs, quality).

## C. Classic system design (still asked)
- [ ] URL shortener
- [ ] Rate limiter (distributed)
- [ ] Notification system (email/SMS/push)
- [ ] Chat / messaging app (WhatsApp/Slack)
- [ ] News feed (Twitter/Instagram)
- [ ] Web crawler / search engine
- [ ] Typeahead / autocomplete
- [ ] Distributed key-value store
- [ ] Distributed cache
- [ ] File storage / sync (Dropbox/Google Drive)
- [ ] Video streaming (YouTube/Netflix)
- [ ] Ride sharing (Uber)
- [ ] Food delivery (DoorDash)
- [ ] Payment system / ledger
- [ ] Ticket booking (Ticketmaster) with concurrency on seats
- [ ] Hotel/flight reservation system
- [ ] Logging & metrics system
- [ ] Job scheduler / distributed cron
- [ ] Message queue (Kafka-like)
- [ ] Collaborative document editor (Google Docs)
- [ ] Leaderboard
- [ ] Metrics/alerting system
- [ ] API gateway

## D. Follow-up questions to expect
- [ ] What are the functional and non-functional requirements?
- [ ] Estimate scale: users, QPS, storage, bandwidth.
- [ ] What does the data model look like? SQL or NoSQL? Why?
- [ ] Where are the bottlenecks? How do you scale reads vs. writes?
- [ ] How do you handle failures, retries, idempotency?
- [ ] Consistency vs. availability trade-off for this system?
- [ ] How do you secure it (authn, authz, encryption, PII)?
- [ ] How do you monitor it? What alerts would you set?
- [ ] How would you deploy it at a customer with no cloud / strict compliance (HIPAA, FedRAMP, GDPR, SOC 2)?
- [ ] What would you build in week 1 vs. month 3?
- [ ] How would you explain this design to the customer's CTO? To an end user?
- [ ] What would you cut if you had half the time?
- [ ] How do you handle schema changes in upstream data sources?
- [ ] How do you backfill historical data?
- [ ] What happens when a downstream dependency is down?
- [ ] Cost estimate: what's expensive, and how do you reduce it?
