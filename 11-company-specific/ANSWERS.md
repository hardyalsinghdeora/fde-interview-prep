# 11 — Company-Specific Notes: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers — adapt them to your own experience and say them out loud. Many questions here repeat material from other sections; those get a concise, company-flavoured answer plus a link to the full version. Product and policy facts were accurate to the best of our knowledge in 2026 but change quickly: **re-check each company's website, blog and recent news the week before your interview**. Items marked *(verify)* are ones we are less sure of.

---

## Palantir — Forward Deployed Software Engineer (FDSE)

### Decomposition: reduce 911 response times (call, traffic, ambulance GPS data).

**Clarify:** Which part of response time — call answer, call processing (triage), dispatch, travel, or on-scene? Which agency (police/fire/EMS)? What target (e.g., NFPA 1710 suggests ~8 min for ALS arrival on 90% of calls)? Who is the user — dispatchers, EMS ops managers, city planners?

**Break the timeline down** (measure each segment first; you can't improve what isn't split):
1. Call received → answered (staffing, queueing).
2. Answered → incident created (triage protocol, location accuracy).
3. Created → unit dispatched (unit recommendation).
4. Dispatched → en route (crew turnout).
5. En route → on scene (routing, traffic, unit positioning).

**Data:** CAD (computer-aided dispatch) incident logs with timestamps; 911 call metadata (ANI/ALI, wireless location); AVL/GPS pings from ambulances; traffic feeds (city sensors, commercial APIs); road network (OSM); hospital status/diversion; historic incidents by time and place.

**Ontology:** Incident, Call, Unit, Station, Crew, Hospital, RoadSegment; links Incident → assigned Unit → Hospital.

**v0 (2–4 weeks):** clean and join CAD + GPS; a dashboard showing each segment's median and 90th percentile by district and hour. This often reveals the biggest win (e.g., turnout time or bad addresses) without any ML.

**v1:** real-time **closest-available-unit recommendation** using live GPS and traffic-aware travel time (not straight-line distance); address/geocoding fixes.
**v2:** **dynamic posting / move-up** — forecast demand by area and hour, reposition idle units to cover gaps; post-incident analytics for staffing.

**Risks:** dispatchers must stay in control (recommend, don't auto-dispatch); GPS gaps and clock skew; equity across neighbourhoods; CJIS/HIPAA data rules; system must keep working if the platform is down (CAD is the system of record).

**Metrics:** 90th-percentile response time by segment and district, % calls meeting standard, recommendation acceptance rate, unit utilisation.

Full framework: [../04-decomposition/ANSWERS.md](../04-decomposition/ANSWERS.md).

### Decomposition: pipeline that ingests hospital records and flags readmission risk.

**Clarify:** Which readmissions — all-cause **30-day** (the CMS Hospital Readmissions Reduction Program measure) or specific conditions (heart failure, pneumonia, COPD)? Who acts on the flag — discharge nurses, care coordinators? What can they actually do (follow-up calls, home visits, med reconciliation)? When is the score needed — at admission, daily, at discharge?

**Data:** EHR (Epic/Cerner) via HL7 v2 ADT feeds and FHIR APIs; diagnoses (ICD-10), procedures, labs, meds, vitals, prior admissions, length of stay, discharge disposition; claims; social determinants (lives alone, housing).

**Pipeline:** ingest (HL7/FHIR/batch extracts) → raw zone → patient identity matching (MPI, fuzzy matching across systems) → cleaned encounter-level tables → feature tables → model scoring → results written back to the care team's worklist (ideally inside the EHR) → feedback (did they intervene, was the patient readmitted).

**Model:** start with a known rule score (**LACE index**: Length of stay, Acuity, Comorbidities, ED visits) as the v0 baseline; v1 gradient-boosted trees on richer features; calibrated probabilities, explainable top factors per patient.

**Risks:** label leakage (using data from after discharge); bias across demographic groups; alert fatigue (tie the threshold to care-team capacity: "top 20 patients per day"); HIPAA — minimum necessary access, audit logs, BAAs; model drift.

**Metrics:** model (AUROC, calibration, precision at top-k); outcome (30-day readmission rate in the flagged group vs control, ideally a staged rollout); adoption (% flagged patients with an intervention).

Full framework: [../04-decomposition/ANSWERS.md](../04-decomposition/ANSWERS.md).

### Decomposition: military asset tracking.

**Clarify:** What assets (vehicles, aircraft, munitions, spare parts, people)? What decisions — readiness reporting, logistics resupply, maintenance, operational planning? Classification level and network (unclassified vs SIPR/JWICS, disconnected edge)? Which echelon is the user?

**Data:** ERP/logistics systems (e.g., GCSS-Army), maintenance records, RFID/barcode scans at depots, GPS/telemetry (where allowed), Blue Force Tracking, supply requests, readiness reports, unit structure.

**Ontology:** Asset, Unit, Location, MaintenanceEvent, Part, SupplyRequest, Mission; links Asset → Unit, Asset → MaintenanceEvent → Part.

**v0:** a single trusted inventory — unify asset IDs across systems, show "what do we have, where, and is it mission-capable" with data freshness shown on every record.
**v1:** readiness dashboard and alerts (asset not mission-capable > N days, part shortages); actions to update status from the field.
**v2:** predictive maintenance and resupply recommendations; scenario planning ("can we support operation X?").

**Key constraints:** works disconnected and syncs later (edge deployment, conflict resolution); strict classification and need-to-know access controls with row/column-level security and audit; data from the edge is late and conflicting — model confidence and last-seen time. Deployment through something like Apollo to air-gapped networks.

**Metrics:** inventory accuracy, time to produce a readiness report (days → minutes), mission-capable rate, stockout frequency.

### Decomposition: build a knowledge graph to connect entities across datasets.

**Clarify:** What entities (people, companies, vehicles, accounts), which datasets, and what questions should the graph answer (fraud rings, supplier risk, investigations)? Freshness and scale?

**Steps:**
1. **Schema / ontology:** entity types, properties, relationship types with provenance (which source, when, confidence).
2. **Ingest & standardise** each source: normalise names, addresses (libpostal), phone numbers, dates.
3. **Entity resolution** — the hard part: blocking (only compare candidates sharing a key such as postcode + name prefix) → pairwise similarity (Jaro-Winkler, embeddings, exact IDs) → rule or ML match scoring → clustering into canonical entities. Keep a human review queue for uncertain matches, and never destroy source records (link, don't merge destructively).
4. **Build edges** from explicit relationships (transactions, ownership) and derived ones (shared address/phone).
5. **Store & query:** graph DB (Neo4j, Neptune) or an ontology layer over a lakehouse; search index for lookups.
6. **Apps:** entity profile pages, link analysis, path finding, alerts on new connections.

**Risks:** false merges are worse than missed merges in investigations; access control per source (some users can't see some edges); scale of all-pairs comparison; keeping it incremental.

**Metrics:** match precision/recall on a labelled sample, analyst time per investigation, coverage of entities.

### Design a data platform for 500 data sources.

**Clarify:** source types (databases, SaaS APIs, files, streams), volumes, latency needs (batch vs real-time), users (analysts, apps, ML), compliance, cloud vs on-prem.

**Design:**
- **Ingestion layer** using a connector framework, not 500 bespoke scripts: CDC (Debezium) for databases, managed connectors (Fivetran/Airbyte) for SaaS, Kafka for streams, landing files in object storage. Each source is a **config entry** (owner, schedule, schema, SLA, PII tags).
- **Storage:** lakehouse on object storage with an open table format (Delta/Iceberg); bronze (raw, immutable) → silver (cleaned, conformed) → gold (business-level) layers.
- **Transformation:** Spark/dbt/SQL, orchestrated by Airflow/Dagster; code in git with CI.
- **Metadata & governance:** catalog with lineage, ownership, classification (Unity Catalog, DataHub); role- and attribute-based access; PII masking.
- **Quality:** schema-change detection, freshness and volume checks, data contracts with source owners; alert the owner, quarantine bad batches.
- **Serving:** warehouse/SQL endpoints, semantic layer or ontology, APIs, feature store.
- **Operations:** per-source health dashboard — at 500 sources something is always broken, so make failures visible, isolated and self-service to fix.

**Rollout:** onboard the 10–20 sources that drive the first use case, prove value, then industrialise onboarding (templates, a request form, automated tests).

Full treatment: [../03-system-design/ANSWERS.md](../03-system-design/ANSWERS.md).

### Coding: palindrome checks; path on a grid with walls; standard LeetCode mediums.

```python
from collections import deque

def is_palindrome(s: str) -> bool:
    """Ignore non-alphanumerics and case. O(n) time, O(1) space."""
    i, j = 0, len(s) - 1
    while i < j:
        if not s[i].isalnum(): i += 1
        elif not s[j].isalnum(): j -= 1
        elif s[i].lower() != s[j].lower(): return False
        else: i += 1; j -= 1
    return True

def shortest_path(grid: list[str], start, end) -> int:
    """BFS on a grid; '#' = wall. Returns steps or -1. O(R*C) time and space."""
    R, C = len(grid), len(grid[0])
    q, seen = deque([(start, 0)]), {start}
    while q:
        (r, c), d = q.popleft()
        if (r, c) == end:
            return d
        for nr, nc in ((r+1, c), (r-1, c), (r, c+1), (r, c-1)):
            if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] != '#' and (nr, nc) not in seen:
                seen.add((nr, nc)); q.append(((nr, nc), d + 1))
    return -1

print(is_palindrome("A man, a plan, a canal: Panama"))       # True
print(shortest_path(["..#", ".##", "..."], (0, 0), (2, 2)))   # 4
```

Variants to expect: longest palindromic substring (expand around centre, O(n²)); "can delete one char" palindrome; grid with weighted cells (Dijkstra) or "can break k walls" (BFS over state (r, c, k)). No AI tools in Palantir rounds — practise writing these by hand, narrating, and testing on edge cases (empty input, start == end, unreachable). More: [../02-coding/ANSWERS.md](../02-coding/ANSWERS.md).

### Re-engineering / debugging: fix and improve a piece of buggy code.

**How to run the round:**
1. **Read before touching** — understand intent; ask what the code is supposed to do.
2. **Run it / its tests** and reproduce the failure. If there are no tests, write a quick one for the expected behavior.
3. **Fix bugs one at a time**, smallest change first, explaining each: off-by-one, mutable default arguments, integer vs float division, shadowed variables, wrong comparisons (`is` vs `==`), unhandled empty input/`None`, resource leaks, shared mutable state, exception swallowing.
4. **Then improve**: naming, splitting long functions, removing duplication, better data structures (list lookup → set/dict for O(1)), complexity fixes, error handling, type hints.
5. **Prioritise out loud**: correctness → clarity → performance. Don't rewrite everything; they want judgement about what matters.

Full checklist: [../12-debugging-troubleshooting/ANSWERS.md](../12-debugging-troubleshooting/ANSWERS.md).

### Learning: learn a concept/API in 60 minutes and apply it.

Palantir weights this heavily for FDSE. They score learning behaviour: targeted questions, playing back your understanding ("so if I understand correctly..."), applying it, staying calm, and admitting gaps. Use the interviewer as a resource. Primers on likely topics (Bloom filters, consistent hashing, Raft, double-entry bookkeeping, ontologies, Git internals) and a full playbook: [../05-learning-round/ANSWERS.md](../05-learning-round/ANSWERS.md).

### Why Palantir? Are you comfortable with government/defense work?

**What they want:** a real reason tied to the FDSE work (not "big data"), knowledge of the products, and an honest, thought-through answer on defense — Palantir is open that it works with militaries, intelligence agencies and law enforcement, and it expects you to have considered that.

**Answer shape:**
- **The work:** "I want to sit with users who run critical operations — a hospital, a factory, a military unit — and ship software that changes how they make decisions. Forward-deployed engineering at Palantir is the purest form of that: you own the problem end to end."
- **The product:** "Foundry's ontology and AIP turn messy data into something operators and AI can act on. I like that the platform compounds — each deployment improves the product."
- **The people / bar:** small teams, high ownership, hard problems.
- **Defense:** answer honestly. If yes: "Yes. I think democratic governments need the best technology, and I'd rather it be built by people who care about privacy and civil liberties controls, which Palantir builds into the platform (granular access controls, audit logs, purpose-based access). I'd want to understand the safeguards on any specific program." If you have limits, say so calmly; they prefer honesty to a fake yes. Some teams are commercial-only.

**Pitfalls:** only talking about money or the stock; not knowing what Foundry/Gotham are; hand-waving the ethics question.

### What do you know about Foundry, Gotham, AIP, Apollo?

- **Gotham** — Palantir's original platform (2000s), built for intelligence, defense and law-enforcement analysts: integrates disparate data into a graph of entities (people, places, events), supports search, link analysis, geospatial/temporal analysis, and targeting and mission planning workflows. Strong security model (classification, need-to-know, audit).
- **Foundry** — the commercial (and also government) data operating system: data integration (connectors, Pipeline Builder, Code Repositories for Python/Spark/SQL transforms), versioned datasets with lineage, the **Ontology**, analytics tools (Contour, Quiver, notebooks) and app building (Workshop, Slate), plus model integration. Used across manufacturing, energy, healthcare, finance, government.
- **AIP (Artificial Intelligence Platform)** — launched 2023; connects LLMs (multiple providers, including private/self-hosted models) to the Ontology so AI can reason over an organisation's data and take actions with guardrails, permissions and human review. Components include AIP Logic (no-code LLM-powered functions), AIP Agent Studio, and evals tooling. Sold heavily through hands-on **AIP Bootcamps** (customers build a working use case in days). The strongest recent commercial growth driver *(verify latest figures in earnings)*.
- **Apollo** — the continuous delivery and deployment platform that ships and updates Palantir software (and customers' software) across any environment: public clouds, on-prem, classified/air-gapped networks and edge devices, managing versions, constraints and rollbacks autonomously.
- Others worth knowing: **Maven Smart System** (AI-enabled targeting/C2 for the US DoD) and the **Ontology SDK (OSDK)** for building external apps on the ontology.

### What's an ontology in Foundry and why does it matter?

**What it is:** a semantic, operational layer that maps raw datasets to the real-world things a business cares about:
- **Object types** (e.g., Aircraft, Work Order, Customer), backed by datasets, each with a primary key.
- **Properties** (attributes) and **link types** (relationships: Work Order → Aircraft).
- **Actions** — governed ways to write back changes (e.g., "Reassign technician"), with validation, permissions and audit, which can sync to source systems.
- **Functions** — business logic (TypeScript/Python) and models bound to objects.

**Why it matters:**
- **One shared language** — operators, analysts, developers and AI all work with "aircraft" rather than `tbl_ac_mstr_v2`.
- **Decisions, not just dashboards** — actions close the loop: people make and record decisions in the same system, and those decisions become data.
- **Reuse** — build an object once; many apps use it.
- **Security** — permissions are applied at object/property level and inherited everywhere.
- **AI grounding** — AIP agents query objects and call actions with the same permissions as the user, which makes LLMs useful and safe in operations.

Interview one-liner: "The ontology is a digital twin of the organisation — nouns are objects, verbs are actions — and it's what lets Palantir go from data integration to operational decisions and AI."

### Palantir values / mission questions.

**What we know (public statements; verify current wording on palantir.com):**
- Palantir describes its purpose as building software that helps institutions — especially Western governments and their allies, and large companies — make better decisions with their data; leadership (CEO Alex Karp) is explicit about supporting the US and allied defense.
- **Privacy and civil liberties** are framed as a core commitment, with a dedicated PCL engineering team and features such as granular access control, purpose-based access and immutable audit logs.
- Culture: very high ownership, flat structure, forward-deployed with customers, intense work, comfort with controversy, mission over comfort.

**Likely questions and how to answer:**
- *"What would you not build?"* — have a thoughtful line, e.g., something that clearly violates law or human rights, and explain how you'd raise concerns.
- *"How do you balance customer needs and civil liberties?"* — technical safeguards (least privilege, audit, data retention limits), escalation, transparency.
- *"Tell me about a time you owned an outcome with little direction"* — ownership story (see [../10-behavioral/ANSWERS.md](../10-behavioral/ANSWERS.md)).
- *"What do you think of Palantir's public positioning?"* — show you've read Karp's letters / *The Technological Republic* (2025) or at least recent shareholder letters; agree or disagree specifically, respectfully.

## OpenAI — Forward Deployed Engineer

### Why FDE and not SWE?

**What they want:** you actively want the customer-facing, ambiguous, end-to-end part — not "FDE as a backdoor to SWE".

**Answer shape:**
- "I'm happiest when I'm close to the problem: talking to the users, seeing the messy data, and shipping something they use next week. As an SWE I'd improve a product for everyone a little; as an FDE I make it work for a specific customer completely, and I bring what I learn back to the product."
- "With frontier models, the gap between what the model can do and what companies actually get from it is mostly deployment work — data, integration, evals, change management. That's the work I want."
- Evidence: [a time you worked directly with users/customers, scoped something vague, and shipped].
- Acknowledge the trade-offs: travel, context switching, less deep ownership of one codebase — and why you're fine with them.

More: [../01-recruiter-and-motivation/ANSWERS.md](../01-recruiter-and-motivation/ANSWERS.md).

### Take-home: RAG system / agent / eval harness on OpenAI APIs.

**What wins take-homes:**
- **Scope sharply** and write it down: the user, the task, what's in/out.
- **Working end to end beats feature breadth.** A small RAG over real documents that answers with citations > a half-built multi-agent system.
- **Include evals** — even 20–50 hand-labelled questions with an automatic scorer (exact match, LLM-as-judge with a rubric, retrieval hit rate). This is what separates strong candidates.
- **Production touches:** config for model names, retries with backoff, structured outputs (JSON schema), logging of prompts/latency/tokens/cost, basic tests, a one-command run (`make run` / Docker).
- **README:** design decisions, trade-offs, what you'd do with more time, eval results with honest failure cases.

Typical stack: OpenAI Responses API (or Chat Completions), `text-embedding-3-small/large` for embeddings, a vector store (pgvector, FAISS, or OpenAI's hosted file search), FastAPI or a simple CLI. For agents: the Agents SDK with function tools, guardrails and tracing. Check the current API docs — model names and recommended APIs change (OpenAI has said the Assistants API is being deprecated in favour of the Responses API) *(verify)*.

Details on RAG, agents and evals: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

### Why this chunking strategy? Why not a different retrieval method?

**Defend choices with the data and with evals:**
- **Chunking:** "The docs are structured manuals, so I chunked by section headings, ~500–800 tokens with ~10–15% overlap, and kept the section title and document name in each chunk's metadata. Fixed-size chunks were splitting tables and procedures mid-way; heading-based chunks raised retrieval recall@5 from X to Y on my eval set."
- Alternatives you considered: fixed-size token windows (simple, breaks structure), semantic chunking (embedding-based boundaries; more compute), small-to-big / parent-document retrieval (retrieve small chunks, pass the parent section), late chunking, per-row chunks for tables.
- **Retrieval:** "I used **hybrid** retrieval — BM25 plus dense embeddings combined with reciprocal rank fusion — because users search for exact part numbers that embeddings miss, then a re-ranker on the top 20." Alternatives: dense only (fine for natural language), keyword only (fine for codes), query rewriting / HyDE, metadata filters, long-context (stuff whole docs in if the corpus is small — sometimes the right answer).
- Always end with: "I'd change it if the eval showed X."

### What would you change if the dataset was 100x larger?

- **Ingestion:** batch/parallel embedding with the Batch API (lower cost), incremental re-indexing via content hashes so only changed docs are re-embedded, a queue-based pipeline.
- **Index:** move from in-memory/FAISS flat to an ANN index (HNSW/IVF) in a managed or scalable vector DB (pgvector with HNSW, Qdrant, Weaviate, OpenSearch); shard; quantise vectors to cut memory.
- **Retrieval quality:** more documents means more near-duplicates and distractors — add metadata filtering (by product, date, tenant), deduplication, hybrid search and a re-ranker; evaluate recall again since it often drops at scale.
- **Cost/latency:** cache embeddings of frequent queries and responses; smaller embedding dimensions; route easy queries to cheaper models.
- **Ops:** access control per document (permission-aware retrieval), monitoring of index freshness, backfills, versioned indexes so you can roll back.
- **Evals:** larger, stratified eval set; sample production queries.

### How do you know your AI system is actually working?

- **Define success with the customer** in task terms (e.g., "correct, cited answer to policy questions", "ticket resolved without escalation").
- **Offline evals:** a golden set of real, representative inputs with expected outputs or rubrics; score retrieval (recall@k, MRR) separately from generation (correctness, faithfulness to sources, format validity). Use deterministic checks where possible, LLM-as-judge (calibrated against human labels) where not. Run on every prompt/model change as a regression test.
- **Online signals:** thumbs up/down, escalation rate, edit distance on drafts, task completion, latency, cost per task.
- **Human review:** sample production traces weekly, label failures, categorise them (retrieval miss, hallucination, wrong tool), and add them to the eval set.
- **Business metric:** the KPI the customer cares about (handle time, deflection, analyst hours saved), ideally via A/B or staged rollout.

Full answer: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

### Rate limiting, retries, batching, caching, latency debugging with the API.

- **Rate limits:** limits are per organisation/project on requests and tokens per minute; read the `x-ratelimit-*` response headers; use a client-side token bucket so you shape traffic before you hit 429s.
- **Retries:** retry 429 and 5xx/timeouts with **exponential backoff plus jitter**, capped attempts, honour `retry-after`; don't retry 400s; make side-effecting tool calls idempotent.
- **Batching:** the Batch API for offline jobs (cheaper, async, completes within a 24h window); batch embedding inputs in one request; concurrency with a bounded semaphore for online fan-out.
- **Caching:** response caching for identical requests (keyed by model + prompt hash + params); embedding cache; **prompt caching** — put the long, static part of the prompt (system prompt, tools, documents) first so the provider can reuse it automatically, which cuts latency and cost.
- **Latency debugging:** measure time-to-first-token vs total time; stream responses; reduce output tokens (output dominates latency); shorten prompts; use a smaller/faster model for simple steps; parallelise independent calls; check reasoning effort settings on reasoning models; trace each step of an agent to find the slow one; compare client vs server timing to rule out your own network/proxy.

Code for a retry wrapper and rate limiter: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md) and [../02-coding/ANSWERS.md](../02-coding/ANSWERS.md).

### Design a customer deployment of ChatGPT Enterprise / Assistants / Agents SDK.

**Clarify:** which use cases (general productivity vs a specific workflow like support or contract review), how many users, data sensitivity and residency, identity provider, systems to integrate, success metric.

**Two layers to the answer:**

1. **ChatGPT Enterprise rollout (buy, configure):**
   - SSO (SAML) and SCIM provisioning from the identity provider; workspace admin roles; domain verification.
   - Data controls: enterprise data isn't used for training by default; retention settings; data residency options where offered *(verify current regions)*; Compliance API export to the customer's eDiscovery/DLP tools.
   - Connectors to internal knowledge (SharePoint, Google Drive, etc.) respecting existing permissions; custom GPTs for common workflows, with an internal review/publishing process.
   - Change management: champions network, prompt training, use-case library; usage analytics to find adoption gaps.

2. **Custom workflow on the API (build):**
   - Architecture: the customer's app → backend service → OpenAI Responses API / **Agents SDK** (agents with tools, handoffs between specialised agents, guardrails, tracing) → tools that call internal APIs (CRM, ticketing) via a service layer that enforces the end user's permissions.
   - Retrieval over internal docs (hosted file search or own vector DB), structured outputs for anything downstream systems consume.
   - Guardrails: input/output moderation, PII redaction, allow-listed actions, human approval for high-risk actions (refunds, emails to customers).
   - Ops: evals in CI, tracing and logging, rate-limit handling, cost dashboards, fallbacks.
   - Note: the Assistants API is being replaced by the Responses API; new builds should use Responses/Agents SDK *(verify current status)*.

**Rollout:** pilot group → measure against baseline → expand; security review and DPIA up front. Metrics: weekly active users, tasks automated, time saved, CSAT, cost per task.

### How would you help a Fortune 500 customer move from POC to production?

POCs fail to reach production for predictable reasons — no owner, no eval, no integration, no security sign-off, unclear ROI. Address each:
1. **Re-confirm the business case:** one measurable KPI, an executive sponsor and an operational owner who will use it daily.
2. **Build an eval set** from real data with the users; agree the quality bar ("90% correct on the golden set, zero critical errors") before scaling.
3. **Production architecture:** real integrations (SSO, source systems, write-backs), not CSV uploads; error handling, retries, monitoring, logging; cost model at production volume.
4. **Security, legal, compliance early:** data flow diagram, DPA, retention, access controls, model risk review — start in week 1, not at the end.
5. **Human-in-the-loop design** for risky steps; clear fallback when the AI is unsure.
6. **Staged rollout:** shadow mode → small pilot group → expand, measuring the KPI against a control.
7. **Operational handover:** runbooks, on-call ownership, dashboards, a process for updating prompts/evals, training the customer's team.
8. **Communicate:** weekly status with metrics, risks and decisions needed.

More: [../09-customer-scenarios/ANSWERS.md](../09-customer-scenarios/ANSWERS.md).

### Tell me about a time you owned a customer outcome.

**What they're looking for:** you took responsibility for the *outcome* (not just your ticket), handled ambiguity and setbacks, and measured the result.

**STAR template:**
- **S:** [Customer, their problem, stakes — revenue, renewal, an executive deadline].
- **T:** [What success meant for them, and your role].
- **A:** [How you went beyond your lane: talked to end users, found the real problem, coordinated other teams, unblocked data/security, shipped iteratively, kept stakeholders informed, handled a setback].
- **R:** [Measured outcome; what the customer said/did (renewed, expanded); what you fed back into the product].

**Example (adapt to your own experience):** "A logistics customer's integration was three weeks late and they were threatening to churn. I took over as the single point of contact, visited their ops team, and found the real issue wasn't our API but their address data — 18% of records failed geocoding. I wrote a cleaning step and a review queue for their team, set up a daily 15-minute call, and got us live in two weeks. Failed deliveries dropped 12% in the first month, they renewed, and the cleaning step became a product feature."

**Pitfalls:** "we" with no clear personal contribution; outcome = "shipped" with no impact; blaming the customer.

See also: [../10-behavioral/ANSWERS.md](../10-behavioral/ANSWERS.md).

### What do you think of OpenAI's mission? AGI benefits?

**Facts:** OpenAI's mission is "to ensure that artificial general intelligence benefits all of humanity." The 2018 **Charter** commits to broadly distributed benefits, long-term safety, technical leadership and a cooperative orientation (including a pledge to stop competing with and assist a value-aligned, safety-conscious project that gets close to AGI first). In 2025 OpenAI restructured: the non-profit (now the OpenAI Foundation) controls a for-profit public benefit corporation (OpenAI Group PBC) *(verify current details)*. Its strategy is **iterative deployment** — releasing progressively more capable systems so society can adapt and the company can learn about safety from real use.

**How to answer (have a real view):**
- What you find compelling: "Deployment is how benefits actually reach people — the FDE role is literally the mechanism for 'benefits all of humanity' inside real organisations: healthcare, government, education, not just tech."
- Concrete benefits you believe in: scientific acceleration, expert-level help widely available, productivity in under-served sectors.
- Show nuance: acknowledge risks (misuse, reliability, concentration of power, labour disruption) and say how you'd contribute — honest evals with customers, safe deployment patterns, not overselling capability.
- Avoid both hype and cynicism; avoid getting into internal politics.

## Anthropic — Forward Deployed Engineer / Applied AI

### Use-case: design a Claude deployment for a customer with tools via MCP.

**Example scenario:** an insurance company wants Claude to help claims handlers — summarise claim files, look up policy coverage, and draft decisions.

**Clarify:** users and volume, systems (claims system, policy DB, document store), what Claude may *do* vs only *read*, compliance (PII, regulated decisions), deployment surface (Claude API direct, AWS Bedrock, Google Vertex AI — often chosen for data/cloud alignment), success metric.

**Architecture:**
- **Client/host:** an internal web app (or Claude for Work/Enterprise with connectors) that runs the agent loop with the Claude API — tool use, system prompt with role and policies, prompt caching for the long static parts.
- **MCP servers** (Model Context Protocol, an open standard Anthropic introduced in Nov 2024 for connecting models to tools and data): one per system — `policy-server` (tools: `get_policy`, `check_coverage`), `claims-server` (`get_claim`, `list_documents`, `add_note`), `docs-server` (resources for claim documents, search tool). Each exposes **tools** (actions), **resources** (readable data) and **prompts** (templates). Transport: stdio locally, Streamable HTTP for remote servers, with OAuth-based auth.
- **Permissions:** MCP servers act **with the end user's identity** (OAuth token passed through), so Claude can never see more than the handler could. Write tools (`add_note`, `set_decision`) require human confirmation.
- **Tool design:** few, well-named tools with clear descriptions and examples, typed input schemas, concise structured outputs (IDs + key fields, pagination), helpful error messages the model can recover from.
- **Long documents:** use Claude's large context with citations, or retrieval for very large files.
- **Safety & ops:** audit log of every tool call; PII handling; evals on real claims; tracing; rate-limit and retry handling; fallback to human.

**Rollout:** read-only assistant first → drafting with human approval → limited automation for low-risk claims. Metrics: handling time, accuracy vs reviewer, override rate.

More on agents and tool use: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

### How do you make long-context workflows reliable?

- **Structure the input:** put long documents **at the top** of the prompt and the question/instructions at the end (Anthropic's guidance; this measurably improves quality); wrap each document in XML tags with metadata (`<document><source>...</source><document_content>...</document_content></document>`).
- **Ground the answer:** ask Claude to first extract relevant quotes, then answer from them; use the Citations feature so every claim points to a source span; let it say "not found in the documents".
- **Don't rely on context alone:** if the corpus is larger than needed, retrieve the relevant parts first; trim irrelevant content — more tokens can add distraction, cost and latency.
- **Decompose:** split big jobs into steps (map-reduce summarisation over sections, then synthesis) or sub-agents with fresh contexts; validate intermediate results.
- **Manage agent context over long runs:** summarise/compact history, keep state in files or a scratchpad/memory tool rather than an ever-growing transcript, clear stale tool results.
- **Prompt caching** for the large static prefix to cut cost and latency on repeated calls.
- **Structured outputs** with schema validation and retry on failure.
- **Evaluate specifically for long context:** test with the answer placed at different positions ("needle" style tests built from the customer's real docs), check citation accuracy, and monitor in production.

### Coding: rate limiter with per-user and global limits (reported, unconfirmed).

Token bucket per user plus one global bucket; a request must pass both. Check both before consuming either so a global rejection doesn't burn the user's token.

```python
import time
from collections import defaultdict

class TokenBucket:
    def __init__(self, rate: float, capacity: float, now: float):
        self.rate, self.capacity = rate, capacity      # tokens/sec, burst size
        self.tokens, self.last = capacity, now

    def refill(self, now: float) -> None:
        self.tokens = min(self.capacity, self.tokens + (now - self.last) * self.rate)
        self.last = now

class RateLimiter:
    def __init__(self, user_rate, user_cap, global_rate, global_cap, clock=time.monotonic):
        self.clock = clock
        self.user_rate, self.user_cap = user_rate, user_cap
        self.glob = TokenBucket(global_rate, global_cap, clock())
        self.users: dict[str, TokenBucket] = {}

    def allow(self, user: str, cost: float = 1.0) -> bool:
        now = self.clock()
        ub = self.users.get(user)
        if ub is None:
            ub = self.users[user] = TokenBucket(self.user_rate, self.user_cap, now)
        ub.refill(now); self.glob.refill(now)
        if ub.tokens >= cost and self.glob.tokens >= cost:
            ub.tokens -= cost; self.glob.tokens -= cost
            return True
        return False

t = [0.0]
rl = RateLimiter(user_rate=1, user_cap=2, global_rate=10, global_cap=3, clock=lambda: t[0])
print([rl.allow("a") for _ in range(3)])   # [True, True, False]  user limit
print(rl.allow("b"), rl.allow("b"))        # True False           global cap of 3 hit
t[0] = 1.0
print(rl.allow("a"))                        # True (refilled)
```

O(1) per call; O(U) memory for U users. Follow-ups to expect: inject the clock for testing (done), evict idle users (LRU/TTL), thread safety (a lock per bucket), distributed version (Redis with a Lua script for atomic check-and-decrement), sliding-window log/counter alternatives, returning a retry-after time.

### Coding: progressive/incremental problem with changing requirements (e.g., in-memory DB, bank system).

**How these rounds work:** several levels (e.g., level 1 basic CRUD → level 2 filtering/scans → level 3 TTL/timestamps → level 4 history/rollback or merges). Unit tests unlock the next level. They score working code under time pressure and how well your design absorbs new requirements.

**Strategy:**
- Read all of the current level's spec carefully; pass the tests before polishing.
- Keep a clean core data model and small methods so later levels slot in (e.g., store `timestamp` from the start if any method takes one).
- Don't over-engineer level 1 for imagined level 4 — but avoid choices that are expensive to change (put all state behind one class).
- Handle edge cases the spec mentions explicitly (missing keys return `""`/`None`, etc.); return types exactly as specified.
- Use Claude if allowed in that round, but you must understand and verify everything.

**Sketch (in-memory DB with TTL):**

```python
class InMemoryDB:
    def __init__(self):
        self.data: dict[str, dict[str, tuple[str, float | None]]] = {}  # key -> field -> (value, expires_at)

    def _alive(self, rec, ts):
        return rec[1] is None or ts < rec[1]

    def set(self, ts: int, key: str, field: str, value: str, ttl: int | None = None) -> None:
        self.data.setdefault(key, {})[field] = (value, ts + ttl if ttl else None)

    def get(self, ts: int, key: str, field: str) -> str | None:
        rec = self.data.get(key, {}).get(field)
        return rec[0] if rec and self._alive(rec, ts) else None

    def delete(self, ts: int, key: str, field: str) -> bool:
        rec = self.data.get(key, {}).get(field)
        if rec and self._alive(rec, ts):
            del self.data[key][field]; return True
        return False

    def scan_prefix(self, ts: int, key: str, prefix: str) -> list[str]:
        fields = self.data.get(key, {})
        return [f"{f}({v})" for f, (v, e) in sorted(fields.items())
                if f.startswith(prefix) and self._alive((v, e), ts)]
```

Bank-system variant: accounts dict, `deposit/transfer/withdraw` with validation, then top-spenders ranking (sort by outgoing total, ties by id), scheduled payments (a min-heap by timestamp processed at the start of each call), and account merges. More: [../02-coding/ANSWERS.md](../02-coding/ANSWERS.md).

### Coding: LRU cache (reported in general Anthropic loops).

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cap, self.d = capacity, OrderedDict()

    def get(self, key):
        if key not in self.d:
            return -1
        self.d.move_to_end(key)          # mark most recently used
        return self.d[key]

    def put(self, key, value) -> None:
        if key in self.d:
            self.d.move_to_end(key)
        self.d[key] = value
        if len(self.d) > self.cap:
            self.d.popitem(last=False)   # evict least recently used

c = LRUCache(2); c.put(1, 1); c.put(2, 2); c.get(1); c.put(3, 3)
print(c.get(2), c.get(1), c.get(3))      # -1 1 3
```

O(1) get/put, O(capacity) space. Be ready to implement it without `OrderedDict` (hash map + doubly linked list with sentinel head/tail) and to discuss follow-ups: TTL expiry, thread safety, LFU, size-by-bytes, and a distributed cache. Full version: [../02-coding/ANSWERS.md](../02-coding/ANSWERS.md).

### How would you measure whether an agent actually works?

- **Define the task and success criteria** with the customer: final outcome (ticket resolved correctly, report accurate), constraints (no forbidden actions, within N steps/cost/time).
- **Build an eval suite of realistic tasks** in a sandboxed environment with real-looking tools/data; grade the **end state** (e.g., database state after the agent runs) rather than the exact path, because good agents take different valid paths.
- **Graders:** code-based checks where possible (state assertions, schema validity, unit tests), LLM-as-judge with a clear rubric for open-ended output (calibrated against human labels), human review for a sample.
- **Metrics:** task success rate; **pass^k / consistency** across repeated runs (agents are stochastic — run each task several times); steps, tokens, latency and cost per task; tool-call error rate; safety violations; escalation rate.
- **Trajectory analysis:** read transcripts, categorise failures (wrong tool, bad args, gave up, looped, hallucinated a result), fix tool descriptions/prompts, add the cases to the suite.
- **In production:** tracing, outcome feedback, human-override rate, A/B or staged rollout vs the old process, business KPI.

More: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

### Prompt engineering: improve this prompt; get structured output reliably.

**Improve a prompt — checklist (Anthropic's documented techniques):**
1. **Be clear and direct:** state the task, audience, and what good looks like; explain *why* constraints exist.
2. **Give context and a role** in the system prompt.
3. **Separate parts with XML tags** (`<instructions>`, `<document>`, `<examples>`).
4. **Few-shot examples** — 3–5 varied, representative ones, including edge cases.
5. **Let it think** for complex tasks (extended thinking, or ask for reasoning in `<thinking>` before the answer).
6. **Specify output format** precisely; say what to do when information is missing ("return `null`, don't guess").
7. **Chain** complex tasks into steps.
8. **Test against an eval set**, change one thing at a time, keep a prompt changelog.

**Reliable structured output:**
- Best: **tool use / structured outputs with a JSON schema** — define a tool whose input schema is your output schema and force it with `tool_choice`, or use the API's structured-output feature where available (constrained decoding guarantees valid JSON) *(check current Claude API docs)*.
- Validate with Pydantic/JSON Schema; on failure, retry with the validation error in the prompt.
- Keep schemas simple (flat where possible, enums for categories, explicit nullability).
- Older technique: prefill the assistant turn with `{` — note this is not supported on some newer models, so prefer tools/structured outputs.
- Set low temperature for extraction tasks.

### What is Constitutional AI? What's your view on AI safety?

**Constitutional AI (CAI)** — Anthropic's method (paper: *"Constitutional AI: Harmlessness from AI Feedback"*, Bai et al., December 2022) for training a helpful and harmless assistant using a written set of principles (a "constitution") instead of large amounts of human harmfulness labels:
1. **Supervised phase:** the model generates responses to harmful prompts, then **critiques and revises** its own responses according to constitutional principles; the model is fine-tuned on the revised responses.
2. **RL phase (RLAIF — RL from AI Feedback):** the model compares pairs of responses using the principles; those AI preferences train a preference model, which is used as the reward for reinforcement learning.

Benefits: scalable oversight (less human exposure to harmful content), **transparency** (the principles are written down and published), and a less evasive model — it explains objections rather than refusing flatly. The original constitution drew on sources like the UN Universal Declaration of Human Rights and other published principles. In January 2026 Anthropic published a new, much longer constitution for Claude, written mainly as explanation of values and reasoning (prioritising being broadly safe, broadly ethical, compliant with Anthropic's guidelines and genuinely helpful) rather than a list of rules *(verify details)*.

**Your view on AI safety (have a genuine one):** e.g., "I think capabilities are moving fast enough that safety has to be engineered in now, not later — both near-term (reliability, misuse, privacy, honest communication of limits) and longer-term (alignment and control of much more capable systems). As an FDE my part is concrete: build deployments with evals, least-privilege tools, human approval for consequential actions, monitoring, and be honest with customers about what the model can't do. I also think safety and usefulness aren't opposites — customers trust and adopt systems that behave predictably."

### Why Anthropic? Discuss the Responsible Scaling Policy.

**Why Anthropic — answer shape:**
- **Mission:** Anthropic's mission is to ensure the world safely makes the transition through transformative AI. It is a public benefit corporation with a Long-Term Benefit Trust that elects part of the board. Say why that matters to you specifically.
- **Approach:** a safety-focused lab that still builds frontier models, on the bet that you need to be at the frontier to do safety research and to set industry norms ("race to the top"). Mention research you've read: interpretability, CAI, the RSP.
- **Product/role:** Claude's strengths for enterprise work (coding, long documents, agentic tool use), MCP as an open standard, Claude Code; FDEs make deployments succeed and feed learnings back.
- **Personal fit:** honesty and care about getting it right, not just shipping.

**Responsible Scaling Policy (RSP):**
- First published **September 2023**; major update (version 2) in **October 2024**, with further revisions since *(verify current version)*.
- Core idea: **if-then commitments** — as models reach certain dangerous **capability thresholds**, stronger safeguards must be in place before training further or deploying.
- **AI Safety Levels (ASL-1, 2, 3, 4...)**, modelled loosely on biosafety levels. Each level has **deployment standards** (preventing misuse, e.g., classifiers against CBRN weapons uplift) and **security standards** (protecting model weights from theft).
- Thresholds focus on areas like **CBRN weapons** (chemical, biological, radiological, nuclear) uplift and **autonomous AI R&D**; models are evaluated before and after deployment.
- **May 2025:** Anthropic activated **ASL-3** protections with the launch of Claude Opus 4 as a precaution.
- Governance: a Responsible Scaling Officer, internal and external review, published capability and safeguards reports.
- Discussion points: strengths (concrete, testable commitments; influenced other labs' frontier safety frameworks and policy); critiques (self-governance, thresholds and evaluations are hard to define, commercial pressure). Having a balanced view is better than reciting.

### Values: honesty, handling uncertainty, saying no to customers.

**Honesty:** "I tell customers what the model can and can't do, including failure rates from our evals. Overselling wins a POC and loses the account. If I made a mistake, I say so early with a fix plan."

**Handling uncertainty:** say what you know, what you don't, and how you'll find out: "I'm not sure whether long-context or retrieval will work better for your corpus; I'll run both on 50 of your real questions by Thursday." Calibrated confidence — distinguish "verified" from "I believe".

**Saying no:** explain the reason, offer an alternative, escalate when needed. "We shouldn't let the agent send payments without approval — the error rate isn't zero and the cost of one mistake is high. What we can do is have it prepare the payment and route it to your team with one-click approval, which still saves most of the time." Also no to requests that violate the usage policy or put end users at risk — firmly, without lecturing.

**STAR template for "a time you said no to a customer":** S [request], T [why it was risky/wrong], A [how you explained, the alternative you offered, who you involved], R [outcome, relationship preserved]. *Example (adapt to your own experience):* a customer wanted to skip evaluation to hit a launch date; you proposed a 3-day minimal eval of 100 cases, which caught a critical failure mode, and the launch slipped one week instead of failing publicly.

See: [../10-behavioral/ANSWERS.md](../10-behavioral/ANSWERS.md).

## Scale AI — Forward Deployed Engineer

### OA: find time intervals where several people are all free.

Classic "employee free time". Collect everyone's busy intervals, sort, merge, and the gaps (within the working window) are times when everyone is free. If the question is "at least k people free", use a sweep line.

```python
def common_free(schedules, day_start, day_end):
    """schedules: list of per-person lists of [start, end) busy intervals.
    Returns intervals where ALL people are free. O(N log N) time, O(N) space."""
    busy = sorted(iv for person in schedules for iv in person)
    free, cur = [], day_start
    for s, e in busy:
        if s > cur:
            free.append((cur, min(s, day_end)))
        cur = max(cur, e)
        if cur >= day_end:
            break
    if cur < day_end:
        free.append((cur, day_end))
    return [(a, b) for a, b in free if a < b]

def free_for_at_least(schedules, k, day_start, day_end):
    """Intervals where at least k of the n people are free (sweep line). O(N log N)."""
    n, events = len(schedules), []
    for person in schedules:
        for s, e in person:
            events += [(s, 1), (e, -1)]           # busy count changes
    events.sort()                                 # at equal time, -1 (end) before +1 (start)
    res, busy, prev = [], 0, day_start
    for t, delta in events + [(day_end, 0)]:
        t = min(max(t, day_start), day_end)
        if t > prev and n - busy >= k:
            if res and res[-1][1] == prev:
                res[-1] = (res[-1][0], t)         # extend contiguous interval
            else:
                res.append((prev, t))
        busy += delta
        prev = max(prev, t)
    return res

sched = [[(9, 10), (12, 13)], [(9, 11)], [(14, 15)]]
print(common_free(sched, 9, 17))           # [(11, 12), (13, 14), (15, 17)]
print(free_for_at_least(sched, 2, 9, 17))  # [(10, 17)]
print(free_for_at_least(sched, 3, 9, 17))  # [(11, 12), (13, 14), (15, 17)]
```

Clarify: are intervals inclusive or half-open, minimum meeting length, time zones, already sorted per person (then a heap merge gives O(N log P) for P people).

### Debugging round: fix a broken codebase / failing tests.

- **Run the test suite first** and read the failures; group them — one root cause often breaks many tests.
- **Read the failing test** to understand intended behaviour; don't "fix" tests to match buggy code unless the test is clearly wrong (and say so).
- **Reproduce minimally**, form a hypothesis, check with prints/debugger, fix, re-run. Narrate each step.
- Common planted bugs: off-by-one, wrong boundary comparisons, mutable default args, incorrect sort keys, timezone/date handling, missing `await`, swallowed exceptions, wrong dict key, stale cache, integer division, None handling.
- Prioritise: fix the bugs blocking the most tests first; keep commits/changes small; mention anything risky you noticed but didn't change.

Full method: [../12-debugging-troubleshooting/ANSWERS.md](../12-debugging-troubleshooting/ANSWERS.md).

### System design: data labelling pipeline; LLM eval platform; RLHF data collection system.

**Data labelling pipeline**
- Flow: customer uploads data (S3/GCS, API) → **task creation** (split into units, attach instructions/ontology of labels) → **routing** to labellers by skill, language, clearance, and quality score → labelling UI (bounding boxes, text classification, ranking) → **quality control** → delivery via API/export.
- **Quality:** gold/honeypot tasks with known answers, consensus (multiple labellers, agreement metrics like Cohen's/Krippendorff's alpha), reviewer tiers, per-labeller accuracy tracking, calibration tasks; disagreements go to experts.
- **ML assist:** model pre-labels, humans correct (faster); active learning picks the most informative items.
- **Infra:** task queue (Kafka/SQS) with leasing and timeouts so abandoned tasks return; Postgres for task state; object storage for assets; idempotent submission; audit trail; PII redaction; throughput and SLA dashboards.

**LLM eval platform**
- Entities: **datasets** (versioned prompts with references/rubrics), **models/endpoints**, **graders** (exact match, code execution, LLM-as-judge, human), **runs**, **results**.
- Run engine: fan out (dataset × model × sample) to workers with rate limiting per provider, retries, caching of identical calls; store raw outputs and scores.
- Human eval: route to expert raters with blind side-by-side comparison; inter-rater agreement.
- Analysis: leaderboards, slicing by category, regression diffs between model versions, statistical significance (bootstrap CIs), contamination checks; private held-out sets.
- Security: customer models and data isolated per tenant; CI integration to gate deployments.

**RLHF data collection system**
- Collect **preference data**: show a prompt and 2+ model responses, raters choose/rank and explain; also supervised fine-tuning demonstrations (raters write ideal responses) and red-teaming data.
- Prompt sourcing and coverage (task taxonomy, difficulty, languages); generate responses from the customer's checkpoints via API.
- Rater quality: expert pools (coding, math, medicine), guidelines, calibration, agreement checks, spot audits, fraud/AI-generated-answer detection.
- Output: versioned datasets with provenance (rater, guideline version, model version) delivered to the customer for reward-model training / DPO; feedback loop on which data improves the model.
- Scale and SLAs: throughput forecasting, workforce scheduling, cost per comparison.

More: [../03-system-design/ANSWERS.md](../03-system-design/ANSWERS.md).

### Credo questions: Scale values; government/defense customers.

**Facts (verify on scale.com before the interview — values wording changes):** Scale AI provides data for AI (labelling, RLHF and evaluation data through its Data Engine and contributor platforms such as Remotasks/Outlier), evaluation and safety work (including public leaderboards via its research lab, SEAL), and AI applications for enterprise and government (e.g., the **Scale Donovan** platform and defense programs such as **Thunderforge** with the US DoD) *(verify)*. In June 2025 **Meta invested about $14 billion for a ~49% non-voting stake**; founder Alexandr Wang left to lead Meta's superintelligence effort, and **Jason Droege** became CEO. Several AI-lab customers reportedly reduced work with Scale afterwards, so government and enterprise matter even more.

**Values:** Scale publishes a short list of company values; candidates report themes like **ambition**, **ownership**, **quality / attention to detail**, **customer obsession / urgency**, and a 2025 hiring principle Wang called **"MEI" (merit, excellence, intelligence)** *(exact current wording uncertain — check the careers page)*. Prepare one story per value.

**Defense question:** Scale is openly pro-national-security (it has worked with the DoD and argues the US must lead in AI). Answer honestly and specifically, e.g., "Yes — I think it's important that democratic governments have reliable, well-evaluated AI, and data quality and evaluation are exactly where safety is decided. I'd want to understand the guardrails and the human-in-the-loop design on any program."

### How would you evaluate a model for a customer?

1. **Start from the use case:** what decisions/tasks will the model perform, what does a costly error look like, what's the baseline (current process or current model)?
2. **Build an eval set** from the customer's real, representative data, including edge cases and adversarial inputs; hold out a private test set; label with domain experts and measure inter-rater agreement.
3. **Choose metrics** per task: accuracy/F1 for classification, exact match / pass@k for code, rubric scores (LLM-judge calibrated against humans) for open-ended output, factuality/faithfulness for RAG, calibration, latency and cost per task.
4. **Safety & robustness:** red-teaming, jailbreak and prompt-injection tests, bias checks across groups, refusal appropriateness.
5. **Compare candidates** (models, prompts, fine-tunes) on the same set with confidence intervals; slice results by category to find where each fails.
6. **Report** in business terms: "Model B solves 87% vs 79% for the current system, fails mainly on scanned documents, costs $0.02 per task." Recommend with caveats.
7. **Keep evaluating in production:** monitor drift, sample outputs for human review, re-run the suite on every model change.

See: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

## Databricks — Forward Deployed / AI Engineer (FDE)

### SQL: window functions, joins, aggregations.

Know these cold (practise on Databricks SQL / Spark SQL):

```sql
-- Top 3 products by revenue per category (window + ranking)
SELECT category, product_id, revenue
FROM (
  SELECT p.category, o.product_id, SUM(o.amount) AS revenue,
         DENSE_RANK() OVER (PARTITION BY p.category ORDER BY SUM(o.amount) DESC) AS rnk
  FROM orders o JOIN products p ON o.product_id = p.product_id
  GROUP BY p.category, o.product_id
) t
WHERE rnk <= 3;

-- 7-day rolling average and day-over-day change
SELECT day, revenue,
       AVG(revenue) OVER (ORDER BY day ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS avg_7d,
       revenue - LAG(revenue) OVER (ORDER BY day) AS dod_change
FROM daily_revenue;

-- Customers with no orders (anti-join)
SELECT c.customer_id FROM customers c
LEFT JOIN orders o ON o.customer_id = c.customer_id
WHERE o.customer_id IS NULL;
```

Key points: `ROW_NUMBER` vs `RANK` vs `DENSE_RANK`; `ROWS` vs `RANGE` frames; `WHERE` vs `HAVING`; join fan-out duplicating sums; NULL behaviour in joins and `COUNT(col)`; Databricks extras: `QUALIFY` to filter on window results, `MERGE INTO` for upserts on Delta tables. More: [../07-data-and-sql/ANSWERS.md](../07-data-and-sql/ANSWERS.md).

### Explain the entire transformer architecture.

- **Input:** text → tokens (BPE/SentencePiece) → token embeddings + **positional information** (sinusoidal, learned, or rotary RoPE in modern LLMs).
- **Self-attention:** each token projects to Q, K, V; Attention(Q,K,V) = softmax(QKᵀ/√d_k)V. **Multi-head** attention runs h heads in parallel on lower-dim projections and concatenates them. Decoders use a **causal mask** so tokens only see earlier tokens.
- **Block:** attention → add residual → layer norm → position-wise feed-forward (two linear layers with a nonlinearity like GELU/SwiGLU, ~4x hidden size) → add residual → layer norm. Modern LLMs use **pre-norm** (often RMSNorm). Stack N blocks.
- **Output:** final hidden state → linear layer to vocabulary → softmax → next-token probabilities; sample with temperature/top-p.
- **Variants:** original 2017 "Attention Is All You Need" encoder–decoder (translation); encoder-only (BERT, embeddings/classification); decoder-only (GPT, Llama, Claude) for generation. Mixture-of-Experts replaces the FFN with routed experts.
- **Training:** pre-training on next-token prediction; then supervised fine-tuning and preference tuning (RLHF/DPO).
- **Inference:** **KV cache** stores past keys/values; attention is O(n²) in sequence length — hence FlashAttention, GQA/MQA, sliding window attention.

Full answer: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

### RAG design; training strategies for domain models.

**RAG on Databricks:** ingest documents to Delta tables (Unity Catalog governed) → parse/chunk → embeddings via Model Serving / Foundation Model APIs → **Mosaic AI Vector Search** index that syncs from a Delta table → retrieval (hybrid + re-rank) → LLM via Model Serving → chain/agent logged with **MLflow**, evaluated with Mosaic AI Agent Evaluation (LLM judges + human review app), served as an endpoint with inference tables for monitoring *(product names change; verify)*. Permissions via Unity Catalog.

**Training strategies for domain models (cheapest first):**
1. **Prompting + RAG** — usually enough for knowledge; cheapest, easy to update.
2. **Fine-tuning** (SFT, often parameter-efficient like LoRA) — for style, format, task specialisation, or a smaller/cheaper model distilled from a larger one.
3. **Preference tuning** (DPO/RLHF) — align outputs with expert preferences.
4. **Continued pre-training** on large domain corpora — new vocabulary/knowledge (legal, biomedical); expensive.
5. **Pre-training from scratch** — rarely justified.
Decide with evals: baseline → RAG → fine-tune only if the gap remains. Watch data quality, catastrophic forgetting, and licensing.

See: [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md).

### MLOps and Spark / data science questions.

**MLOps:**
- **MLflow:** experiment tracking (params, metrics, artifacts), model registry (in Unity Catalog, with aliases like `@champion`), model packaging and serving.
- Feature engineering with feature tables (online/offline consistency, point-in-time joins to avoid leakage).
- CI/CD for ML: code in git, Databricks Asset Bundles for deployment, dev/staging/prod workspaces, automated tests and validation gates.
- Monitoring: data drift, prediction drift, model quality (Lakehouse Monitoring), retraining triggers.

**Spark:**
- Lazy evaluation; **transformations** (narrow: `map`, `filter`; wide: `groupBy`, `join` cause a **shuffle**) vs **actions** (`count`, `write`) that trigger jobs → stages → tasks.
- Performance: avoid shuffles; **broadcast joins** for small tables; handle **data skew** (salting, AQE skew join); partitioning and file sizes (`OPTIMIZE`, Z-ORDER/liquid clustering in Delta); cache only when reused; prefer DataFrame/SQL over UDFs (or vectorised pandas UDFs); Photon engine.
- Delta Lake: ACID transactions on object storage, time travel, `MERGE`, schema enforcement/evolution.
- Structured Streaming for incremental pipelines; Auto Loader for files.

**Data science basics:** bias–variance, train/validation/test splits and leakage, cross-validation, class imbalance (precision/recall, AUC-PR), A/B test significance.

More: [../07-data-and-sql/ANSWERS.md](../07-data-and-sql/ANSWERS.md), [../08-cloud-devops-integration/ANSWERS.md](../08-cloud-devops-integration/ANSWERS.md).

### Decomposition: vague business goal → plan on the Lakehouse.

**Example:** "A retailer wants to reduce customer churn."

1. **Clarify:** how is churn defined (no purchase in 90 days? cancelled subscription?), which customers, current rate, value at stake, who will act and how (retention offers, service outreach), timeline.
2. **Success metric:** churn rate in the targeted segment vs control; retained revenue; campaign ROI.
3. **Data:** transactions, customer profile, web/app events, support tickets, marketing interactions → land in **bronze** Delta tables (Auto Loader, Lakeflow/DLT pipelines, partner connectors).
4. **Model the data:** silver (cleaned, deduped, conformed customer IDs) → gold (customer-level features: recency, frequency, monetary value, ticket counts, engagement trends). Govern with **Unity Catalog** (PII tags, row/column masking).
5. **v0 (2 weeks):** a Databricks SQL / AI/BI dashboard of churn by segment and cohort — often reveals quick wins.
6. **v1:** churn prediction model (gradient boosting) tracked with MLflow, scored daily by a Databricks Job, results pushed to the CRM/marketing tool (reverse ETL); explanations of top drivers.
7. **v2:** uplift modelling (who will respond to an offer), GenAI summaries of support tickets as features, A/B tested campaigns.
8. **Risks:** label definition, leakage, data quality across systems, privacy/consent, adoption by the marketing team.
9. **Plan & team:** milestones, who owns each piece, how it's handed over.

Full framework: [../04-decomposition/ANSWERS.md](../04-decomposition/ANSWERS.md).

### Business use-case discussion and career history (HM round).

**Career history:** a 2-minute narrative that connects your roles to *why FDE at Databricks now*: [role] → [what you built, with impact numbers] → [the thread: data/ML + customer-facing + delivery] → [why this role is the next step]. Prepare to go deep on 2 projects: problem, your architecture, trade-offs, what went wrong, measurable outcome.

**Business use-case discussion:** they want to see you connect technology to value. For any use case, cover: the business problem and who owns it, the value (revenue, cost, risk), the data needed, the solution on the platform, how you'd measure success, risks, and how you'd get to production. Know a few common Databricks use cases by industry: retail (demand forecasting, personalisation), financial services (fraud, risk, regulatory reporting), healthcare/life sciences (real-world evidence, genomics), manufacturing (predictive maintenance, IoT), and GenAI (document intelligence, support agents).

Also prepare questions for the hiring manager: [../14-questions-to-ask/ANSWERS.md](../14-questions-to-ask/ANSWERS.md).

## Other companies hiring FDEs (prepare the general sections)

No checklist questions here. Map each company to the section to prepare: customer-support agents and voice latency → [../06-ai-llm/ANSWERS.md](../06-ai-llm/ANSWERS.md); permissions-aware enterprise search and connectors → [../03-system-design/ANSWERS.md](../03-system-design/ANSWERS.md); on-prem / air-gapped deployment → [../08-cloud-devops-integration/ANSWERS.md](../08-cloud-devops-integration/ANSWERS.md); practical coding and take-homes → [../02-coding/ANSWERS.md](../02-coding/ANSWERS.md); customer scenarios → [../09-customer-scenarios/ANSWERS.md](../09-customer-scenarios/ANSWERS.md).

## Company research checklist (do for every company)

### What does the product do? Who are the main customers?

Write a one-paragraph answer you could say in 30 seconds: **product** (what it does in plain words), **buyer** (which role signs the contract), **user** (who uses it daily), **top customer segments/logos** (from case studies and earnings calls), **business model** (seat licences, usage-based API, platform contract), and **main competitors**. Sources: homepage, docs, customer case studies, earnings call transcripts / S-1 for public companies (Palantir), funding announcements for private ones. Example for Palantir: "Foundry/AIP/Gotham sold to large enterprises and governments; US government and US commercial are the biggest segments; big multi-year contracts expanded through bootcamps."

### What does an FDE there actually ship?

Find out what "forward deployed" means at that company, because it varies:
- **Palantir:** configures and builds on Foundry/AIP for a customer — pipelines, ontology, Workshop apps, AIP workflows — usually on-site, end to end.
- **OpenAI / Anthropic:** custom deployments of models for strategic customers — agents, RAG, evals, integrations, prompt/tool design — and feedback into product and research.
- **Scale:** data and eval programs for labs and government customers, plus GenAI applications.
- **Databricks:** customer lakehouse and GenAI solutions on the platform.

Sources: job descriptions, the company's engineering blog posts about FDEs, LinkedIn posts from current FDEs, and ask the recruiter: "What did the last FDE hired ship in their first 90 days?"

### Recent launches, blog posts, earnings/funding news.

- Read the **last 3–6 months** of the company blog, product changelog/release notes and docs "what's new" page.
- Public companies (Palantir, Snowflake, MongoDB; Databricks was private at time of writing — check for an IPO): latest **earnings call** highlights — growth, segment trends, what the CEO is emphasising (for Palantir, AIP and US commercial growth).
- Private companies: latest **funding round**, valuation, major partnerships, leadership changes (e.g., Scale's 2025 Meta investment and CEO change).
- Pick **one launch you have an opinion on** and prepare: what it is, who it's for, one thing you'd improve, how an FDE would deploy it.

### Mission and values: map 1 story to each value.

- Copy the company's mission and values from its careers/about page.
- Build a table: **value → one STAR story → the specific behaviour it shows → result**. Reuse stories across companies but re-angle the framing.
- Example mapping: Anthropic "honesty" → the time you told a customer the POC wasn't ready; Palantir "ownership" → the time you took a stalled integration end to end; Scale "quality" → the time you built a validation step that caught bad data; OpenAI "benefits all of humanity" → a project that helped non-technical users.
- Prepare a genuine view on the mission, including one nuance or tension, rather than repeating the slogan.

See story bank: [../10-behavioral/ANSWERS.md](../10-behavioral/ANSWERS.md).

### Try the product/API yourself and build something small.

Nothing signals interest like a working demo. In a weekend:
- **OpenAI / Anthropic:** a small RAG or tool-using agent with an eval set of 20 questions; for Anthropic, build an MCP server for a data source you use.
- **Palantir:** use the free Foundry/AIP developer tier if available *(verify)*, or read the Ontology/OSDK docs and model a domain you know as objects, links and actions.
- **Databricks:** the Free Edition / free trial — ingest a public dataset into Delta, build a pipeline, a dashboard and a model logged in MLflow.
- **Scale:** read SEAL leaderboards and its eval methodology; build a mini eval harness comparing two models.
Write down what was confusing or broken — it's great material for "what would you improve?" and for questions to the interviewer.
