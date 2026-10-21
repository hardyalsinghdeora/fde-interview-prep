# 03 — System Design: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are compact reference designs, not scripts. Adapt them to your own experience and the interviewer's constraints, and practise saying them out loud: clarify first, sketch the boxes, then go deep on the two or three things that actually matter for that customer.

Each design follows the same skeleton: **Clarify → Requirements → Architecture → Data model → Deep dives → Trade-offs → Customer/deployment → Metrics.** In a real interview, spend the first 3-5 minutes on clarifying questions and requirements before drawing anything.

---

## A. Applied / customer-context design prompts

### Design a system to integrate our platform with a customer's legacy ERP/CRM (SAP, Salesforce, Oracle).

**Clarify:** Which objects (customers, orders, invoices)? Read-only or write-back? Freshness needed (nightly vs minutes)? What interfaces exist (SAP IDoc/BAPI/OData, Salesforce REST/Bulk/CDC, Oracle DB views)? Who owns the system and what's their change process? Network path (VPN, on-prem agent)?

**Requirements:** Reliable sync of N objects, near-real-time for key ones, no data loss, idempotent writes, audit trail, must not overload the ERP (API quotas, batch windows), survive schema changes.

**Architecture:**
```
ERP/CRM --(CDC / events / polling)--> Connector (per system) --> Queue (Kafka/SQS)
   --> Transform + canonical mapping --> Our platform DB / API
Our platform --> Outbox --> Write-back worker --> ERP API (rate-limited, idempotent)
```
- One connector per source using the best native interface: Salesforce Change Data Capture + Bulk API for backfill; SAP OData/IDoc via SAP Integration Suite or an on-prem agent; Oracle via read-only views or Debezium/GoldenGate CDC.
- A **canonical data model** in the middle so the platform never speaks SAP-ese directly.
- Mapping config (field maps, code tables) lives in versioned config, not code.

**Data model:** `external_ref(system, object_type, external_id, internal_id, last_synced_at, hash)` for ID crosswalk; sync_runs and dead-letter tables.

**Deep dives:** Idempotency via external IDs/upsert keys; conflict policy (system of record per field); watermarking for incremental pulls; backfill in pages within maintenance windows; DLQ with replay UI.

**Trade-offs:** iPaaS (MuleSoft, Boomi, Workato) is faster to ship but adds licensing and less control; custom connectors give control but you own maintenance. Polling is simple but laggy and quota-hungry; CDC is fresher but needs admin setup.

**Customer/deployment:** Get a sandbox early, a named ERP admin, a service account with least privilege, and agreement on who is system of record. Often deploy a small on-prem agent that dials out (no inbound firewall holes).

**Metrics:** sync lag, records/sec, error and DLQ rate, API quota usage, reconciliation diffs (row counts and checksums vs source).

### Design a data platform that ingests from 500 heterogeneous data sources.

**Clarify:** Source types mix (DBs, SaaS APIs, files/SFTP, streams)? Volume per source and in total? Freshness SLA? Who consumes (BI, ML, ops apps)? Team size to maintain 500 connectors?

**Requirements:** Add a new source in hours, not weeks, through config; isolate failures per source; schema evolution; lineage; data quality checks; cost control.

**Architecture:**
```
Sources -> Ingestion layer (Fivetran/Airbyte for SaaS, Debezium for DBs,
           custom Python for long tail, SFTP/file landing)
        -> Raw zone (object storage, immutable, partitioned by source/date)
        -> Bronze -> Silver (cleaned, typed, conformed) -> Gold (marts)
           via Spark/dbt on a lakehouse (Delta/Iceberg) or warehouse
        -> Catalog + lineage (Unity/DataHub), quality (Great Expectations/dbt tests)
        -> Orchestrator (Airflow/Dagster) driven by a source registry
```
- **Source registry** (YAML/DB): connection, schedule, owner, schema contract, SLA. Pipelines are generated from it; 500 sources should mean 5-10 connector *types*, not 500 bespoke jobs.

**Data model:** raw tables keep source schema plus metadata (`_source`, `_ingested_at`, `_batch_id`); silver conforms to canonical entities; gold is star schemas per use case.

**Deep dives:** Schema drift: land everything in raw, detect drift, auto-add nullable columns, alert on breaking changes. Per-source backpressure and retries so one bad source cannot block others. Data quality gates between layers.

**Trade-offs:** Buy connectors for common SaaS (time) vs build (cost and control). Lakehouse is cheaper and flexible; warehouse is simpler for SQL-only teams.

**Customer/deployment:** Prioritise the 20 sources that drive 80% of value; onboard the rest in waves. Data owners per source sign off on definitions.

**Metrics:** sources healthy %, freshness per source vs SLA, failed runs, quality test pass rate, cost per source, time to onboard a new source.

### Design a real-time dashboard for a hospital to track bed capacity across 50 hospitals.

**Clarify:** Source of truth for bed status (EHR ADT feed like Epic/Cerner HL7, manual entry)? "Real-time" = seconds or minutes? Users (command centre, regional ops, ED charge nurses)? Bed types (ICU, med-surg, isolation, staffed vs physical)? PHI needed on screen?

**Requirements:** Capacity per hospital/unit/bed type within ~1 minute; drill-down; alerts on thresholds (ICU > 90%); historical trends; high availability during surges; HIPAA.

**Architecture:**
```
EHR ADT (HL7v2 A01/A02/A03) / FHIR -> Interface engine (Mirth/Rhapsody) per site
  -> Message bus (Kafka) -> Stream processor (bed state machine)
  -> Current-state store (Postgres/Redis) + history (warehouse)
  -> API + WebSocket push -> Dashboard (map + tables) ; Alerting service
```
- Stream processor maintains a **bed state machine** (available, occupied, dirty/cleaning, blocked) from admit/transfer/discharge events.
- Manual-entry UI for sites without feeds and for staffed-bed overrides.

**Data model:** `hospital`, `unit`, `bed(bed_id, unit_id, type, status, patient_ref?, updated_at)`, `bed_event` (append-only), plus aggregated `capacity_snapshot(hospital, unit_type, ts, total, staffed, occupied)`.

**Deep dives:** Out-of-order/duplicate HL7 messages: use event timestamps and message IDs; periodic full census reconciliation each hour to fix drift. Aggregate-only views for most users so no PHI leaves the site.

**Trade-offs:** Push (WebSocket) vs poll every 30s: polling is simpler and fine at this scale (hundreds of users). Strict accuracy vs freshness: show "last updated" per hospital.

**Customer/deployment:** Integration with 50 EHR interfaces is the real work; start with sites on the same EHR. BAA with cloud provider, SSO via hospital IdP.

**Metrics:** feed lag per site, % sites reporting, reconciliation mismatch rate, dashboard uptime, alert time-to-acknowledge.

### Design a pipeline that ingests hospital records and flags patients at risk of readmission.

**Clarify:** Which readmission (30-day all-cause)? When must the flag appear (at discharge planning, daily)? Who acts on it (case managers)? Data available (EHR, claims, labs, social determinants)? Is there an existing model (LACE, HOSPITAL score)?

**Requirements:** Daily or near-real-time risk scores for current inpatients; explainable top factors; surfaced in the clinician workflow; HIPAA, audit; fairness monitoring; model governance.

**Architecture:**
```
EHR (FHIR/HL7, nightly extracts) + claims -> Ingestion -> De-identified/limited
feature store (encounters, diagnoses, labs, meds, prior utilisation)
  -> Training pipeline (offline, versioned) -> Model registry
  -> Scoring job (daily + on ADT events) -> Risk table
  -> EHR integration (FHIR write-back / CDS Hooks / worklist) + dashboard
  -> Outcome tracker (actual readmissions) -> monitoring + retraining
```

**Data model:** `patient`, `encounter`, `features(encounter_id, as_of, ...)` with point-in-time correctness, `risk_score(encounter_id, model_version, score, top_factors, scored_at)`, `intervention` log.

**Deep dives:** Start with a baseline (LACE index or logistic regression), then gradient boosting (XGBoost) with SHAP explanations. Avoid label leakage: features only as of scoring time. Calibrate so 0.3 means 30%. Pick thresholds by case-manager capacity (top 10% flagged), not by an arbitrary cutoff.

**Trade-offs:** Accuracy vs explainability (clinicians reject black boxes). Real-time vs daily batch (daily is usually enough for discharge planning).

**Customer/deployment:** Clinical champion, IRB/governance review, silent-mode pilot first (score but do not show) to validate. Deploy in the hospital's cloud tenant or on-prem.

**Metrics:** AUROC, calibration, precision at capacity, subgroup performance (fairness), % flagged patients receiving intervention, actual readmission rate change.

### Design a system to track military/logistics assets in real time with intermittent connectivity.

**Clarify:** Number of assets, update frequency, transport (satcom, radio, cellular, none for hours)? Bandwidth limits? Classification level? Who consumes (HQ common operating picture, field units)? Must field nodes work fully disconnected?

**Requirements:** Latest known position plus status per asset; graceful degradation when disconnected; eventual consistency; prioritised sync over thin links; strong security; operate at the edge.

**Architecture:**
```
Asset trackers -> Edge node (vehicle/forward base): local store + local UI
     <--store-and-forward sync (priority queues, compression)-->
Regional node <--> HQ node (full picture, analytics)
```
- Every node runs the same stack (local DB, sync agent, API, map UI) so it works standalone.
- Sync is **append-only event logs** with per-node sequence numbers; nodes exchange "what have you not seen" (vector clocks / high-water marks).

**Data model:** `asset`, `position_report(asset_id, ts, lat, lon, source, node_id, seq)`, `status_event`. Latest state derived by max(ts) per asset, so merges are trivial and commutative.

**Deep dives:** Bandwidth: delta + binary encoding (protobuf), drop or downsample stale positions, priority lanes (alerts > positions > bulk). Conflicts: last-writer-wins on timestamps for positions; CRDTs or human resolution for editable records. Clock skew: GPS time.

**Trade-offs:** Consistency vs availability: choose AP; show staleness ("last seen 14 min ago"). Edge compute adds hardware/ops burden.

**Customer/deployment:** Air-gapped/classified networks, rugged hardware, accreditation (ATO), cross-domain guards between classification levels. Containers on k3s at the edge, updates shipped as signed bundles.

**Metrics:** data age per asset, sync backlog per link, bytes per update, % assets reporting, sync conflict rate.

### Design a supply-chain visibility platform for a manufacturer with 200 suppliers.

**Clarify:** What visibility (PO status, shipments, inventory at supplier, tier-2 risk)? How do suppliers share data today (EDI, email, portals, spreadsheets)? Key decisions it should drive (expedite, re-source)? ERP in use?

**Requirements:** Unified view of POs, ASNs, shipments, ETAs; exception alerts (late shipment, shortage risk); supplier self-service onboarding; multiple ingestion channels; supplier sees only its data.

**Architecture:**
```
ERP (POs, BOMs, MRP) ----\
Suppliers: EDI 850/856, API, portal upload, email+CSV -> Ingestion + normalisation
Carriers/visibility APIs (project44, FourKites) ----/      -> Canonical model
   -> Event store + current state -> Exception engine (rules + ETA model)
   -> Buyer UI / supplier portal / alerts
```

**Data model:** `supplier`, `part`, `purchase_order`, `po_line`, `shipment(asn, carrier, eta, status)`, `inventory_snapshot`, `event` log, `exception(type, severity, owner, status)`.

**Deep dives:** Supplier onboarding: tiered approach (big suppliers by EDI/API, small via portal or emailed spreadsheet templates). Matching shipments to PO lines with fuzzy matching. Exception engine: "PO line due in 5 days, no ASN" or "ETA after MRP need date"; link to BOM to show which production lines are at risk.

**Trade-offs:** Data completeness vs supplier burden: start with what suppliers already send. Rules first, ML ETA prediction later.

**Customer/deployment:** The hardest part is supplier adoption; pick the top 20 suppliers by spend/risk for the pilot. Multi-tenant portal with row-level security per supplier.

**Metrics:** % PO lines with current status, supplier data freshness, exceptions caught before line stoppage, on-time-in-full, buyer hours saved.

### Design a fraud-detection system for a bank's card transactions.

**Clarify:** Volume (e.g. 5k TPS peak)? Latency budget in the authorisation path (typically < 100 ms)? Decline vs step-up (3DS/OTP) vs review? Labels available (chargebacks, confirmed fraud)? Existing rules engine?

**Requirements:** Real-time score per transaction within budget, high availability (fail-open/closed policy), rules + ML, analyst case management, explainability for regulators, feedback loop.

**Architecture:**
```
Card network auth -> Auth service -> Fraud scoring API (p99 < 50 ms)
   features: online feature store (Redis) <- stream aggregations (Flink/Kafka)
   model (GBM) + rules engine -> decision (approve/decline/step-up/review)
 -> events to Kafka -> case management UI + label store -> offline training
```

**Data model:** `transaction`, velocity features keyed by card/merchant/device (`txn_count_1h`, `amount_sum_24h`, `distinct_countries_7d`), `decision(txn_id, score, model_version, rules_fired)`, `label(txn_id, fraud, source, date)`.

**Deep dives:** Streaming features with consistent online/offline definitions (feature store) to avoid training-serving skew. Labels arrive late (chargebacks 30-90 days), so train on matured windows. Heavy class imbalance: use precision-recall metrics, cost-weighted thresholds (fraud loss vs customer friction). Graph features (shared devices/merchants) for rings.

**Trade-offs:** Fail-open on timeout (customer experience) vs fail-closed (loss); usually fail-open with rules fallback. Complex models vs latency.

**Customer/deployment:** PCI DSS: tokenised PANs, network segmentation. Model risk management (SR 11-7) documentation. Shadow mode before enforcement; champion/challenger.

**Metrics:** fraud loss bps, detection rate, false positive rate / decline rate on good customers, p99 latency, analyst queue size, model drift (PSI).

### Design an anti-money-laundering (AML) alert triage system.

**Clarify:** Current state: rules-based transaction monitoring generating how many alerts/day, with what false-positive rate (often 95%+)? Analyst team size? Regulator expectations? Data: transactions, KYC, sanctions, adverse media?

**Requirements:** Prioritise and enrich alerts, reduce analyst time per alert, never silently drop alerts that rules generate (regulators), full audit trail, explainable decisions, SAR filing workflow.

**Architecture:**
```
Core banking txns + KYC + watchlists -> Data platform (entity-resolved customers)
Rules engine (existing TM system) -> Alerts
   -> Enrichment (customer profile, network graph, history, adverse media)
   -> Risk scoring model (prioritise) + LLM narrative summary
   -> Case management (queues, L1/L2, SAR drafting) -> Audit store
   -> Feedback (dispositions) -> model retraining
```

**Data model:** `customer`/`party` (resolved), `account`, `transaction`, `alert(rule_id, score, priority, status)`, `case`, `disposition(analyst, decision, reason, ts)`, graph edges (party-account-counterparty).

**Deep dives:** Scoring prioritises but does not auto-close at first; auto-close low-risk only after validation and regulator buy-in, with sampling QA. Graph analytics to find networks (shared addresses, circular flows). LLM generates draft case narratives grounded in evidence, analyst edits and approves.

**Trade-offs:** Recall is sacred; optimise analyst efficiency, not alert suppression. Explainability over raw accuracy.

**Customer/deployment:** Model validation team review, on-prem or bank's private cloud, strict data residency. Run in parallel with the current process to prove no missed SARs.

**Metrics:** time per alert, alerts per analyst/day, SAR conversion rate by priority band, % high-priority SARs in top decile, QA miss rate, backlog age.

### Design a predictive maintenance system for aircraft/factory equipment using sensor data.

**Clarify:** Assets and sensor count, sampling rate (1 Hz vs 10 kHz vibration)? Failure history labels available? Action: alert, schedule work order, auto-stop? Connectivity at plant/aircraft (offload at gate)? Existing historian (OSIsoft PI) or CMMS (SAP PM, Maximo)?

**Requirements:** Detect anomalies and predict remaining useful life (RUL); alert early enough to schedule maintenance; low false alarms; integrate with work orders; edge processing for high-frequency data.

**Architecture:**
```
Sensors/PLCs -> Edge gateway (OPC-UA/MQTT; downsample, FFT features)
  -> Stream (Kafka/IoT Hub) -> Time-series store (Timescale/InfluxDB) + lake
  -> Feature pipelines (rolling stats, spectral features)
  -> Models: anomaly detection (unsupervised) + RUL/failure classifiers
  -> Alert service -> CMMS work orders + engineer dashboard -> feedback labels
```

**Data model:** `asset`, `sensor`, `reading(sensor_id, ts, value)` in time-series DB, `feature_window`, `failure_event`/`work_order`, `prediction(asset_id, ts, risk, rul, model_version)`.

**Deep dives:** Labels are rare: start with physics thresholds and unsupervised anomaly detection (isolation forest, autoencoders), move to supervised as failures are logged. Per-asset baselines since machines differ. Alert hysteresis and grouping to prevent alarm fatigue.

**Trade-offs:** Edge vs cloud compute (bandwidth vs flexibility). Sensitivity vs false alarms; cost of an unplanned failure sets the threshold.

**Customer/deployment:** OT/IT network separation, often on-prem; aviation requires regulatory approvals and maintenance engineers in the loop. Start with one asset class with known failure modes.

**Metrics:** precision/recall of failure prediction, lead time before failure, false alarms per asset per month, unplanned downtime reduction, maintenance cost.

### Design an inventory forecasting system for a retail chain.

**Clarify:** SKUs x stores (e.g. 50k x 1,000 = 50M series)? Forecast horizon and granularity (daily per store-SKU, weekly)? Used for replenishment orders automatically or by planners? Promotions, seasonality, new products? Data history?

**Requirements:** Daily forecasts for all store-SKUs within a nightly window, prediction intervals (for safety stock), handle promotions/holidays/stockouts, planner overrides, accuracy tracking.

**Architecture:**
```
POS sales, inventory, promos, prices, calendar, weather -> Warehouse
  -> Feature pipeline (lags, rolling means, promo flags, holidays)
  -> Training (weekly): global GBM (LightGBM) across series + quantile loss
  -> Batch inference (nightly, Spark/Ray) -> forecasts table
  -> Replenishment engine (order qty = forecast + safety stock - on hand - on order)
  -> Planner UI (overrides, exceptions) -> ERP purchase orders
```

**Data model:** `sales(store, sku, date, units, price, promo_flag)`, `inventory_snapshot`, `forecast(store, sku, date, p50, p90, model_version, run_id)`, `override`.

**Deep dives:** Stockouts censor demand: impute true demand when on-hand = 0. One global model beats 50M per-series ARIMA models in cost and cold-start. Hierarchical reconciliation (store vs region totals). Intermittent demand for slow movers (Croston or zero-inflated models).

**Trade-offs:** Accuracy vs explainability for planners; a simple baseline (seasonal naive) must always be beaten. Automation vs planner control.

**Customer/deployment:** Planner trust is everything: show accuracy vs their current method, allow overrides and measure them.

**Metrics:** WAPE/MAPE by category, bias, forecast value-add vs baseline, stockout rate, inventory turns, waste for perishables.

### Design a system to unify customer records from 10 systems (entity resolution / master data).

**Clarify:** Volume (e.g. 50M records)? Which identifiers exist (email, phone, tax ID, address)? Golden record consumers (CRM, analytics, compliance)? Batch or real-time match on create? Who resolves ambiguous matches (data stewards)?

**Requirements:** Match records across systems into a single entity with a persistent ID; survivorship rules for golden attributes; explainable merges; un-merge; incremental updates; crosswalk back to source IDs.

**Architecture:**
```
10 sources -> Ingest -> Standardise (names, addresses via libpostal, phones E.164)
  -> Blocking (candidate pairs by keys: email, phone, name+zip soundex)
  -> Pairwise scoring (rules + ML, e.g. Splink/Fellegi-Sunter)
  -> Clustering (connected components with thresholds)
  -> Golden record (survivorship) + crosswalk -> MDM store/API
  -> Steward review queue for grey-zone matches
```

**Data model:** `source_record(source, source_id, attrs, hash)`, `match_pair(a, b, score, decision)`, `entity(entity_id, golden attrs)`, `entity_member(entity_id, source, source_id)`, `merge_history`.

**Deep dives:** Blocking keeps it from being O(n^2). Thresholds: auto-match above high, steward review in middle, no-match below. Transitive over-merging (A~B, B~C but A≠C): guard by cluster coherence checks. Stable entity IDs across reruns. Survivorship: most recent, most trusted source, or most complete per attribute.

**Trade-offs:** Precision vs recall: false merges are worse (privacy, wrong person) than missed merges. Buy (Reltio, Informatica, Tamr) vs build (Splink + warehouse).

**Customer/deployment:** Data stewards and agreed match rules signed off by business. GDPR: right to erasure must cascade through crosswalk.

**Metrics:** precision/recall on a labelled sample, duplicate rate, steward queue size, % records matched, merge/un-merge rate.

### Design an incident/case management system for a police department or emergency services.

**Clarify:** Which workflows (incident reports, investigations, evidence)? Integration with CAD (dispatch), RMS, body-cam, court systems? Users and roles? CJIS compliance? Mobile use in vehicles?

**Requirements:** Create/update cases, link people/vehicles/locations/evidence, search, assignment and status workflow, chain of custody, strict access control and audit, offline-capable mobile, retention rules, reporting (NIBRS).

**Architecture:**
```
CAD events -> Case service (create from incident) <-> Web + mobile clients
Case service: Postgres (cases, entities, links) + object store (evidence, WORM)
Search (OpenSearch, permission-filtered) ; Workflow engine (states, assignments)
Audit log (append-only) ; Integrations: RMS, court, NIBRS export, LPR
```

**Data model:** `case(id, type, status, classification)`, `person`, `vehicle`, `location`, `case_entity(case_id, entity_id, role)`, `evidence(hash, storage_uri, custody_chain)`, `note`, `access_log`. A graph view over entities aids investigations.

**Deep dives:** Access: ABAC with case sensitivity (juvenile, sealed, internal affairs), need-to-know. Evidence integrity: SHA-256 hashes, WORM storage, custody events. Entity resolution for people across cases. Offline mobile drafts synced later.

**Trade-offs:** Configurable workflows vs simplicity; full-text search must not leak restricted cases (filter at index query time, not post-hoc).

**Customer/deployment:** CJIS Security Policy (MFA, encryption, background-checked admins), GovCloud or on-prem. Migration from legacy RMS is a major workstream.

**Metrics:** case cycle time, time to create report, search latency, audit anomalies, uptime, adoption by officers.

### Design a 911 dispatch optimisation system using call, traffic, and ambulance GPS data.

**Clarify:** Optimise what: response time, coverage, crew workload? Recommend or auto-dispatch (almost always recommend)? Data sources: CAD calls, AVL GPS every 5-10 s, traffic (HERE/Google/TomTom), hospital status? Scale (city vs county)?

**Requirements:** Recommend the best unit within ~1 s of call creation; live unit map; dynamic post/move-up recommendations to maintain coverage; very high availability with a manual fallback; full audit.

**Architecture:**
```
CAD (call, location, priority) --\
AVL GPS stream -> Kafka -> Unit state service (position, status: available/en route/at hospital)
Traffic API -> ETA service (road graph + live speeds, cached travel-time matrix)
   -> Recommender: rank available units by ETA + capability (ALS/BLS) + policy
   -> Dispatcher console (accept/override) ; Coverage optimiser (every minute)
```

**Data model:** `call(id, location, priority, created_at)`, `unit(id, type, status, last_position, ts)`, `assignment(call_id, unit_id, recommended_rank, accepted, ts)`, travel-time matrix cache.

**Deep dives:** ETA quality matters more than fancy optimisation: use routing engine (OSRM/Valhalla) with live speeds, fallback to historical speeds per time-of-day. Coverage: maximal covering location model to suggest move-ups. Stale GPS handling: if unit position older than 60 s, flag.

**Trade-offs:** Greedy nearest-unit is explainable and robust; global optimisation over pending calls can be better but harder to trust. Humans keep final say.

**Customer/deployment:** CAD vendor integration (often proprietary), CJIS, on-prem with redundant sites. Must degrade to existing manual process if the system is down.

**Metrics:** response time (median/90th), % calls meeting target, recommendation acceptance rate, ETA error, coverage %.

### Design a document processing pipeline for insurance claims (OCR → extraction → review).

**Clarify:** Document types (claim forms, medical bills, police reports, photos)? Volume (e.g. 50k docs/day)? Fields needed and accuracy target? Turnaround SLA? Existing claims system (Guidewire)?

**Requirements:** Ingest from email/portal/scan; classify; extract fields with confidence; human review for low confidence; push to claims system; audit; PII protection.

**Architecture:**
```
Intake (email, portal, scanner) -> Object store + queue
 -> Preprocess (dedupe, split multi-doc PDFs, deskew)
 -> Classify (doc type) -> OCR/layout (Textract/Azure DI/Tesseract)
 -> Extraction (templates for fixed forms; LLM/layout model for variable docs)
 -> Validation (business rules, cross-checks vs policy DB)
 -> Confidence router: auto-accept | human review UI
 -> Claims system API ; feedback -> training data
```

**Data model:** `document(id, claim_id, type, storage_uri, status)`, `page`, `extraction(doc_id, field, value, confidence, bbox, source=model|human)`, `review_task`, `audit_event`.

**Deep dives:** Field-level confidence plus rule validation (dates in range, policy number exists, totals sum) decides routing. Review UI shows bounding boxes beside values. Corrections become labelled data for evaluation and fine-tuning. Idempotent processing per document hash.

**Trade-offs:** Straight-through processing rate vs error rate; set per-field thresholds by cost of error (payment amount vs claimant middle name).

**Customer/deployment:** PII/PHI: encryption, region-locked processing, retention policy. Start with the top 3 document types that make up most volume.

**Metrics:** STP rate, field accuracy (sampled QA), review time per doc, end-to-end latency, cost per doc.

### Design an audit-logging system for a regulated customer (immutable, queryable).

**Clarify:** What events (user actions, data access, admin changes)? Volume? Retention (7 years for SOX/FINRA)? Who queries (auditors, security, customers)? Tamper-evidence or tamper-proof required? Real-time alerting?

**Requirements:** Every event captured with who/what/when/where/outcome; no loss; immutable and tamper-evident; query by user/resource/time in seconds for recent data; long-term cheap retention; access to logs is itself audited.

**Architecture:**
```
Services -> audit SDK (structured events, sync write to local outbox)
  -> Kafka (replicated, acks=all) -> Writers:
       - Hot store: OpenSearch/ClickHouse (90 days, fast query)
       - Cold store: S3 with Object Lock (compliance mode, WORM), Parquet
       - Hash chain service: per-partition chained hashes, periodic signed digests
  -> Query API (auth, row-level filtering) + SIEM export
```

**Data model:** `event(event_id, ts, actor, actor_type, action, resource_type, resource_id, tenant_id, source_ip, outcome, request_id, before/after hash, prev_hash)`.

**Deep dives:** Tamper evidence: hash chaining plus periodically publishing signed root hashes (like CloudTrail digest files); verification tool for auditors. Guaranteed capture: transactional outbox so a business action and its audit event commit together. Schema versioning for events.

**Trade-offs:** Sync logging (no loss, adds latency) vs async (fast, small loss risk). Use outbox for critical actions. Hot vs cold cost.

**Customer/deployment:** Separate account/role so admins cannot delete logs; KMS keys controlled by customer; retention per regulation; GDPR conflicts handled by pseudonymising actor IDs.

**Metrics:** ingestion lag, events dropped (target 0, measured by sequence gaps), query latency, verification job pass/fail.

### Design a multi-tenant SaaS deployment where some customers require single-tenant or on-prem.

**Clarify:** How many tenants and of which tiers? Why single-tenant (data residency, noisy neighbour, compliance, custom keys)? On-prem with internet or air-gapped? Release cadence expectations? Who operates on-prem?

**Requirements:** One codebase and one artifact set for all modes; tenant isolation; per-tenant config and feature flags; automated deployment to many environments; observability across them; upgrades without bespoke work.

**Architecture:**
- **Pooled multi-tenant:** shared app tier, `tenant_id` on every row with row-level security, or schema-per-tenant; shared Kubernetes cluster.
- **Single-tenant (dedicated):** same Helm charts/Terraform modules instantiated per tenant in its own VPC/account, managed by us.
- **On-prem/customer cloud:** same containers packaged as a Helm chart or Replicated/KOTS bundle; customer runs the cluster; we ship versioned releases.
- A **control plane** (tenant registry, licensing, release orchestration) vs **data planes** (where tenant data lives). Data planes phone home only metadata/telemetry, if allowed.

**Deep dives:** Abstract cloud dependencies behind interfaces (S3 API via MinIO, Postgres not Aurora-only features, Kafka or NATS). Config-driven feature flags. Support at most N-2 versions on-prem. Migrations must be backward compatible for rolling upgrades.

**Trade-offs:** Pooled is cheapest and easiest to operate; dedicated costs more but sells to enterprises (charge for it). On-prem slows velocity and support; limit what is supported.

**Customer/deployment:** Clear support matrix (K8s versions, DBs), install preflight checks, support bundles for debugging without access.

**Metrics:** deployment frequency per mode, version skew across fleet, time to provision a new tenant, cost per tenant, incidents per mode.

### Design a system that works in an air-gapped environment.

**Clarify:** Fully disconnected or one-way data diode? Classification level? Hardware provided? How are updates delivered (physical media, cross-domain solution)? Who operates it? What external services does the app currently depend on (SaaS APIs, LLM APIs, package registries)?

**Requirements:** Zero runtime internet dependencies; self-contained install and upgrade; local identity, monitoring, logging; reproducible, verifiable releases; operate with minimal vendor access.

**Architecture:**
- Bundle everything: container images in a local registry (Harbor), Helm charts, OS packages mirror, models/weights, docs. Delivered as a signed tarball/ISO.
- Platform: Kubernetes (RKE2/k3s/OpenShift) or plain VMs, local Postgres, MinIO for object storage, Keycloak for identity (integrate with customer LDAP/AD/CAC), Prometheus/Grafana/Loki for observability.
- No external calls: replace SaaS (maps tiles, geocoding, LLM APIs) with local equivalents (self-hosted tile server, open-weight models on local GPUs).

**Deep dives:** Release process: SBOM, vulnerability scan, signatures (cosign), checksums verified on import. Upgrade runbook and rollback; database migrations tested on prod-like data. Support: "support bundle" export that customers review and carry out. Licensing offline (signed license files).

**Trade-offs:** Slower update cadence (quarterly) means more careful testing; cannot hotfix quickly. Local models may be weaker than hosted ones.

**Customer/deployment:** Accreditation (ATO, STIG hardening, FIPS crypto), on-site engineers with clearances, training customer admins.

**Metrics:** install/upgrade success rate and duration, time from release to deployed, number of support escalations, uptime.

### Design a data-sharing system between organisations with fine-grained access control.

**Clarify:** Who shares with whom (hospitals and researchers, agencies, supply partners)? Granularity (dataset, row, column, purpose)? Copy data or query in place? Legal agreements (DUAs)? De-identification required? Revocation needs?

**Requirements:** Data owners define policies; consumers access only permitted rows/columns for permitted purposes; revocation is effective; complete audit; minimise copies; support aggregated or privacy-preserving access where raw is not allowed.

**Architecture:**
```
Producer data -> Sharing layer:
   catalog (datasets, schema, owner, sensitivity tags)
   policy engine (ABAC: org, role, purpose, clearance -> row filters, column masks)
   access via governed views/queries (Delta Sharing, Snowflake secure shares, Trino + Ranger/OPA)
   request/approval workflow (consumer requests -> owner approves -> grant with expiry)
   audit log of every query
```

**Data model:** `dataset`, `column_tag(sensitivity)`, `policy(dataset, subject_attrs, purpose, row_filter, column_mask, expires_at)`, `grant`, `access_request`, `query_audit`.

**Deep dives:** Enforce at the query engine, not the app, so every path is covered. Query-in-place avoids copies and makes revocation real; when copies are unavoidable, watermark and contract-limit them. For sensitive data: aggregate-only access with minimum cell sizes, differential privacy, or clean rooms.

**Trade-offs:** Federation (fresh, no copies, slower and coupled to producer uptime) vs replication (fast, harder to revoke).

**Customer/deployment:** Legal agreements map directly into policy objects. Each org keeps its own IdP; federate identity via SAML/OIDC.

**Metrics:** time from request to access, policy violations blocked, % data access through governed paths, audit completeness.

### Design a migration from a customer's on-prem database to the cloud with zero downtime.

**Clarify:** Engine and size (e.g. Oracle 10 TB to Postgres/Aurora, or same engine)? Write rate? Acceptable cutover window (seconds of read-only)? How many apps connect? Network bandwidth to cloud? Rollback requirement?

**Requirements:** No data loss, near-zero downtime, verifiable correctness, rollback path, minimal app changes.

**Plan:**
1. **Assess:** schema/feature compatibility (AWS SCT/ora2pg), dependencies, connections inventory.
2. **Provision** target, network (Direct Connect/VPN), security.
3. **Initial load:** snapshot/bulk export, or physical (backup restore) for same engine.
4. **CDC replication:** Debezium/AWS DMS/GoldenGate/native logical replication from source log position at snapshot time; catch up and keep lag at seconds.
5. **Validate:** row counts, checksums per table/partition, sampled row comparisons, shadow-read testing.
6. **Cutover:** brief write freeze or switch via connection proxy/DNS; wait for lag = 0; flip apps; enable reverse replication (cloud to on-prem) for rollback.
7. **Decommission** after a bake period.

**Deep dives:** Alternative to freeze: dual-writes with idempotent upserts behind a feature flag, but these are error-prone; prefer CDC + quick flip. Sequences/identity columns must be advanced on target. Large LOBs and tables without PKs need special handling.

**Trade-offs:** Same-engine lift (fast, low risk) vs re-platform (cost savings, more risk). Do one at a time when possible.

**Customer/deployment:** Rehearse cutover in staging multiple times; runbook with go/no-go criteria; DBA and app owners on the call.

**Metrics:** replication lag, validation mismatch count, cutover duration, error rates post-cutover, rollback readiness.

### Design a reporting system that the customer's analysts can self-serve.

**Clarify:** Analyst skill (SQL vs Excel only)? Data sources? Number of users? Existing BI tools/licences? Governance: who defines metrics? Row-level security needs?

**Requirements:** Analysts build their own reports without engineers; trustworthy, consistent metric definitions; performance on large data; access control; discoverability.

**Architecture:**
```
Sources -> ELT (Fivetran/Airbyte) -> Warehouse (Snowflake/BigQuery/Postgres)
  -> dbt models: staging -> marts (documented, tested)
  -> Semantic layer (dbt metrics/Cube/LookML): certified metrics & dimensions
  -> BI tool (Looker/Power BI/Superset/Metabase) with RLS
  -> Catalog/docs + training ; certified vs sandbox spaces
```

**Data model:** star schemas: facts (orders, events) with conformed dimensions (customer, product, date); wide denormalised tables for common analyses.

**Deep dives:** The semantic layer is what stops "five definitions of revenue". Certified dashboards vs personal sandboxes. Performance: aggregate tables, materialised views, warehouse caching, query cost limits. RLS by region/business unit mapped from IdP groups.

**Trade-offs:** Flexibility vs governance; too strict and analysts go back to Excel exports. Pick a BI tool they already licence unless it's blocking.

**Customer/deployment:** Train power users as champions; office hours; migrate their top 10 existing reports first to prove parity.

**Metrics:** weekly active analysts, % reports self-built, engineering ticket volume for reports, query p95 latency, warehouse cost, metric discrepancy incidents.

### Design a CDC (change data capture) pipeline from Postgres to a warehouse.

**Clarify:** Tables and change rate? Latency target (minutes vs hourly)? Need full history (SCD2) or current state? Deletes? Warehouse (Snowflake/BigQuery/Redshift)? Managed tools allowed (Fivetran, Airbyte, Datastream)?

**Requirements:** Capture inserts/updates/deletes reliably with ordering per key; handle schema changes; initial snapshot plus streaming; exactly-once effect in warehouse; low impact on source.

**Architecture:**
```
Postgres (wal_level=logical, publication, replication slot)
  -> Debezium (Kafka Connect) -> Kafka topics per table (key = PK) + Schema Registry
  -> Sink: land raw change events into warehouse staging (append-only)
  -> MERGE job (dbt/Snowflake tasks) every N minutes -> current-state tables (+ SCD2 history)
```

**Data model:** change event: `op (c/u/d), before, after, source.lsn, ts_ms`. Raw table append-only; final table keyed by PK with `_lsn` to apply only newer changes.

**Deep dives:** Idempotent merge: dedupe by PK keeping max LSN, so replays are safe. Deletes as tombstones (soft delete flag or delete in merge). Replication slot growth: if the consumer stops, WAL accumulates and can fill the source disk; alert on slot lag and set `max_slot_wal_keep_size`. Schema changes: Avro + registry; additive changes auto-propagate, breaking ones alert. Initial snapshot: Debezium incremental snapshots without long locks.

**Trade-offs:** Kafka adds ops but decouples and allows multiple consumers; simpler: Fivetran or direct Debezium Server to cloud storage. Micro-batch merges every 5 min balance cost and freshness.

**Customer/deployment:** Need DBA to enable logical replication and grant permissions; run from a read replica only if it supports logical decoding (PG16+ on standby).

**Metrics:** end-to-end lag, replication slot size, events/sec, merge failures, row count reconciliation.

### Design a webhook delivery system with retries and guaranteed delivery.

**Clarify:** Volume (e.g. 10k events/sec)? Ordering required per subscription? At-least-once is acceptable? Max retry window (e.g. 3 days)? Customers need replay and delivery logs?

**Requirements:** At-least-once delivery, retries with backoff, isolation so one slow endpoint does not hurt others, signatures, delivery visibility, manual replay, disable failing endpoints.

**Architecture:**
```
Producers -> Event written in same DB txn as business change (outbox)
 -> Relay -> Kafka/SQS -> Fan-out (subscriptions matching event type)
 -> Delivery tasks queue (per-endpoint partitioning)
 -> Workers: HTTP POST (timeout 10 s, HMAC signature, event id header)
     2xx -> mark delivered ; else schedule retry (exp backoff + jitter)
 -> After max attempts -> DLQ + notify customer ; dashboard + replay API
```

**Data model:** `subscription(id, tenant, url, secret, event_types, status)`, `event(id, type, payload, created_at)`, `delivery(id, event_id, subscription_id, attempt, status, next_attempt_at, response_code, latency)`.

**Deep dives:** "Guaranteed" really means at-least-once plus idempotency on the receiver (event IDs, documented). Per-endpoint concurrency limits and circuit breakers so a dead endpoint's backlog does not starve others. SSRF protection: block private IPs, resolve DNS and validate. Signatures with timestamp to prevent replay. Ordering: per-key sequential delivery only if required (costly); otherwise include sequence numbers.

**Trade-offs:** Strict ordering vs throughput. Long retry windows vs storage.

**Metrics:** delivery success rate, time to first successful delivery, retry backlog per endpoint, DLQ size, disabled endpoints.

### Design an integration layer that syncs data bi-directionally between two SaaS tools.

**Clarify:** Which tools and objects (e.g. Salesforce accounts and HubSpot companies)? Which fields flow which way? System of record per field? Latency expectation? Webhooks available? API rate limits?

**Requirements:** Changes in either system propagate within minutes; no infinite loops; conflict handling; ID mapping; idempotent; handle rate limits; observable and replayable.

**Architecture:**
```
Tool A webhooks/poll --> Change capture A --\
                                             -> Sync engine -> writer B (rate-limited)
Tool B webhooks/poll --> Change capture B --/   (mapping, conflict rules, loop guard)
Mapping store: id_map(a_id, b_id), field state hashes, last sync versions
```

**Data model:** `id_map(object_type, a_id, b_id)`, `field_state(object, field, last_value_hash, last_written_by, version, ts)`, `sync_log`.

**Deep dives:** **Loop prevention:** when the engine writes to B, record the value hash; when B's webhook echoes it back, recognise it as our own write and skip. Also use a dedicated integration user and filter its changes. **Conflicts:** field-level ownership (A owns billing, B owns marketing fields) avoids most; otherwise last-writer-wins by timestamp with an alert queue. **Creates:** dedupe via matching before creating to avoid duplicates. Webhooks are lossy, so add periodic polling reconciliation by `updated_at`.

**Trade-offs:** Buy (Workato, Tray, Census for reverse ETL) vs build; building gives control over conflicts. Field-level vs record-level sync (field-level is more complex but loses less data).

**Customer/deployment:** Agree on ownership matrix in writing with both teams before building.

**Metrics:** sync lag, conflict rate, error rate per object, API quota usage, reconciliation drift.

### Design a permissioning model (RBAC/ABAC) for a platform used by multiple agencies.

**Clarify:** How many agencies and users? Data sensitivity levels? Sharing between agencies? Need-to-know/case-based access? Each agency's own IdP? Delegated admin?

**Requirements:** Agencies isolated by default; controlled cross-agency sharing; role-based for common access plus attribute-based for sensitivity, purpose and clearance; delegated administration; explainable decisions ("why can I see this?"); full audit.

**Design:**
- **Identity:** federate each agency's IdP (SAML/OIDC); SCIM for provisioning; user attributes: agency, clearance, roles, groups.
- **Model:** hierarchy `platform -> agency -> unit/team -> project/case`. RBAC roles (viewer, analyst, editor, admin) granted at a scope. ABAC conditions layered on top: `resource.classification <= user.clearance`, `resource.agency == user.agency OR resource shared with user.agency`, `purpose in allowed_purposes`.
- **Relationship-based** (Zanzibar-style, e.g. OpenFGA/SpiceDB) for sharing graphs: "user is member of group that is viewer on case".
- **Enforcement:** central policy decision point (OPA/Cedar or OpenFGA) called by services; data-layer enforcement (row-level security, search filters) so lists and searches are correct.

**Data model:** `subject`, `role`, `role_binding(subject, role, scope)`, `resource(attrs: owner_agency, classification, markings)`, `share(resource, target_agency, permissions, expires)`, `policy`.

**Trade-offs:** Pure RBAC causes role explosion; pure ABAC is hard to audit. Hybrid: RBAC for capabilities, ABAC/ReBAC for data visibility.

**Deployment:** Agency admins manage their own users; platform admins cannot read data by default (break-glass with audit).

**Metrics:** authz latency, denied requests, access reviews completed, time to onboard a new agency.

### Design a system for a field-sales team that must work offline and sync later.

**Clarify:** What do reps do offline (view accounts, create orders, capture notes/photos)? Data volume per rep? Conflicts likely (shared accounts)? Devices (iPad, Android)? Backend CRM?

**Requirements:** Full functionality for assigned territory offline; reliable sync when connected; conflict handling; secure local storage (lost devices); small sync payloads over weak networks.

**Architecture:**
```
Mobile app: local DB (SQLite/Realm/WatermelonDB) + outbox of mutations
   <-> Sync API (delta pull since cursor; push mutations batch)
   <-> Backend service + Postgres -> CRM/ERP integration
```
- **Pull:** server returns changes since the device's last cursor, scoped to the rep's territory.
- **Push:** client sends queued operations with client-generated UUIDs (idempotent) and the base version they edited.

**Data model:** each record has `id (UUID)`, `version`, `updated_at`, `deleted` (tombstone). Server keeps a change log with monotonically increasing sequence for cursors. Outbox: `op_id, entity, op, payload, base_version`.

**Deep dives:** Conflicts: field-level merge where possible; server-authoritative for prices and inventory (validate orders on sync, return rejections to the user); last-writer-wins for notes or keep both. CRDTs only if collaborative editing really matters. Photos uploaded separately, resumable. Encrypt local DB, remote wipe via MDM.

**Trade-offs:** Off-the-shelf sync (Couchbase Lite, PowerSync, Realm/Atlas Device Sync, Firebase) vs custom. Larger local datasets help offline but slow first sync.

**Metrics:** sync success rate, pending ops age, conflict rate, rejected orders, time to first sync.

## B. AI / LLM system design prompts

### Design a customer-support chatbot for an enterprise using its knowledge base (RAG).

**Clarify:** Channels (web, in-app, email)? Volume? KB sources (Zendesk, Confluence, PDFs) and how often they change? Answer only or take actions (order status, refunds)? Escalation path to humans? Languages? Success metric (deflection, CSAT)?

**Requirements:** Accurate, grounded answers with citations; refuse or escalate when unsure; handoff with context; latency < 3 s to first token; no hallucinated policies; PII safe; measurable quality.

**Architecture:**
```
KB sources -> Ingestion (sync, clean HTML, chunk by headings, metadata) -> embed
  -> Vector index + keyword index (hybrid)
User msg -> Orchestrator: guardrails (PII, abuse) -> query rewrite (with chat history)
  -> retrieve (hybrid, filter by product/locale) -> rerank -> LLM answer w/ citations
  -> post-checks (groundedness, policy) -> response | escalate to agent (ticket + transcript)
Logs/traces -> eval set + feedback -> improvements
```

**Data model:** `chunk(id, doc_id, text, embedding, product, locale, updated_at, url)`, `conversation`, `message`, `feedback`, `escalation`.

**Deep dives:** Chunking by semantic sections with titles; hybrid BM25 + vector beats either alone for product names and error codes. System prompt: answer only from context, cite sources, say "I don't know" otherwise. Tool calls for account-specific questions (order status) with auth. Eval set of 200-500 real questions with expected answers; LLM-as-judge plus human review.

**Trade-offs:** Bigger model = better answers, higher cost/latency; route simple FAQs to a smaller model. Aggressive deflection vs CSAT.

**Customer/deployment:** KB quality is usually the bottleneck; the project often surfaces outdated articles. Launch to a % of traffic, with easy human handoff.

**Metrics:** resolution/deflection rate, CSAT, escalation rate, groundedness score, hallucination incidents, cost per conversation, p95 latency.

### Design an internal enterprise search / "ask your docs" assistant with permissions.

**Clarify:** Sources (Google Drive, SharePoint, Confluence, Slack, Jira)? Number of users and docs? Permission models per source? Freshness needed? Data residency?

**Requirements:** Users only ever see content they can access in the source system; fresh within minutes; answers with citations; search plus chat; audit.

**Architecture:**
```
Connectors per source (API + webhooks/incremental crawl)
  -> doc + ACL extraction (users/groups allowed) -> identity mapping (source groups -> IdP users)
  -> chunk + embed -> index with ACL fields (allowed_principals)
Query: user identity -> expand groups -> retrieve with ACL filter (pre-filter in index)
  -> rerank -> LLM answer with citations -> audit log
```

**Data model:** `document(id, source, url, acl_principals[], updated_at)`, `chunk(doc_id, text, embedding, acl_principals[])`, `principal_map(source_principal -> idp_user/group)`, group membership cache.

**Deep dives:** **Permissions are the whole problem.** Index ACLs alongside chunks and filter at query time (pre-filter, not post-filter, or results go empty or leak). ACL changes must propagate quickly: subscribe to permission-change events, recompute on group membership change. Optionally re-check permissions against the source API at answer time for the top-k docs (defence in depth). Never cache answers across users.

**Trade-offs:** Index-time ACLs (fast, can be stale) vs query-time checks (accurate, slow, API rate limits); use both. Buy (Glean, Copilot, Vertex Search) vs build.

**Customer/deployment:** Security review will focus on ACL fidelity and data leaving the tenant; deploy in the customer's cloud, use their LLM endpoint (Bedrock/Azure/Vertex).

**Metrics:** permission-leak tests (must be zero), freshness lag, answer helpfulness, search click-through, adoption (WAU).

### Design a RAG system over 10M documents; how does it change at 100x data?

**Clarify:** Doc size (10M docs might be 200M chunks)? Update rate? QPS? Latency target? Multi-tenant? Query types (lookup vs analytical)?

**At 10M docs (~100-200M chunks):**
- **Ingestion:** distributed parsing and embedding (Ray/Spark batch), dedupe by hash, incremental updates via change feed.
- **Index:** sharded ANN (HNSW or IVF-PQ) in a vector DB (Milvus, Vespa, OpenSearch, pgvector only if smaller); hybrid with BM25; metadata filters.
- **Query:** query rewrite -> hybrid retrieve top 100 -> cross-encoder rerank top 10 -> LLM.
- Storage: 200M x 1024 dims x 4 bytes ≈ 800 GB raw; use quantisation (int8/PQ) to fit in memory.

**At 100x (1B docs, ~20B chunks):**
- Memory cost dominates: aggressive quantisation (PQ, binary embeddings + rescoring), disk-based ANN (DiskANN), smaller embedding dimensions (Matryoshka).
- **Partition by tenant/domain/time** and route queries to relevant shards instead of searching everything.
- **Hierarchical retrieval:** document-level index first (summaries/titles), then chunk retrieval within top docs.
- Re-embedding the corpus becomes a multi-week, very expensive job: version embeddings, migrate gradually, avoid changing models casually.
- Ingestion pipeline needs backpressure, idempotency, and cost per doc optimisation (cheap embedding models, batch APIs).
- Tiering: hot recent data in memory, cold on disk; caching popular queries.

**Trade-offs:** Recall vs cost (quantisation and partitioning lose recall; measure it). Freshness vs index rebuild cost.

**Metrics:** recall@k on a labelled set, p95 latency, cost per 1k queries, index build time, ingestion lag, answer quality evals.

### Design an AI agent that books travel / processes refunds / updates CRM records.

**Clarify:** Which actions and their blast radius (refund $10 vs $10k)? Policies the agent must follow? Systems and APIs available? Human approval requirements? Volume?

**Requirements:** Complete multi-step tasks via tools; follow business policy; never take irreversible actions without proper authorisation; audit every step; graceful handoff to humans.

**Architecture:**
```
User/channel -> Agent orchestrator (LLM with tool use, state machine for workflow)
  tools: lookup_order, check_refund_policy, issue_refund, search_flights, book, update_crm
  -> Tool gateway: authz per user + per tool, input validation, limits, idempotency keys
  -> Approval service (human-in-the-loop for high-risk actions)
  -> Systems of record ; Trace/audit store ; Eval harness
```

**Deep dives:** Keep tools narrow and typed (JSON schema), never "run arbitrary SQL". Policy enforcement in code, not only in the prompt: `issue_refund` itself checks amount limits and eligibility. Read tools free, write tools gated: confirm with the user before booking, require human approval above thresholds. Idempotency keys so retries don't double-refund. Structured workflow (state machine) for well-known processes; free-form agent loop only where needed. Max steps and timeouts.

**Trade-offs:** Autonomy vs control: start with "agent drafts, human approves", raise autonomy as evals and production data show reliability.

**Customer/deployment:** Sandbox/staging APIs for testing; per-action audit for compliance; prompt-injection defences when tools read untrusted text (emails, web).

**Metrics:** task success rate, % needing human handoff, policy violations (target 0), wrong-action rate, steps and cost per task, time saved.

### Design a multi-agent system for a complex workflow (research → draft → review).

**Clarify:** What output (report, proposal, code)? Quality bar and who reviews? Sources allowed? Latency (minutes OK)? Cost budget per run?

**Requirements:** Decompose task, gather evidence, produce draft, critique and revise, cite sources, bounded cost and time, observable steps, human checkpoints.

**Architecture:**
```
Orchestrator (planner) -> creates plan + subtasks
  -> Research agents (parallel; web/RAG/tools) -> evidence store (notes + citations)
  -> Writer agent (draft from evidence only)
  -> Reviewer agent (rubric: accuracy vs evidence, completeness, style) -> issues list
  -> Writer revises (max N rounds) -> Human review -> final
State: shared workspace (task graph, artifacts, messages) persisted per run
```

**Deep dives:** Start with the simplest thing: often one agent with good tools, or a fixed pipeline, beats free-form multi-agent chat. Use multiple agents when there is real parallelism (independent research subtopics) or context isolation benefits. Structured handoffs (JSON artifacts), not long chat transcripts. Reviewer uses a different prompt and ideally checks claims against evidence. Termination criteria and budgets (tokens, steps, wall-clock). Durable execution (Temporal or a job queue) so long runs survive failures.

**Trade-offs:** More agents = more cost, latency, and error compounding; benefits must be shown by evals. Parallel research vs coherence.

**Customer/deployment:** Show intermediate artifacts to users for trust; allow editing the plan before research.

**Metrics:** rubric-scored quality vs single-agent baseline, citation accuracy, cost and time per run, revision rounds, human edit distance.

### Design a coding assistant for a company's internal codebase.

**Clarify:** Features (chat Q&A, autocomplete, PR review, agentic changes)? Repo size and languages? IDEs? Code can leave the network? Model hosted where?

**Requirements:** Answers grounded in internal code and docs; respects repo permissions; low latency for completion (< 300 ms); secure; measurable productivity.

**Architecture:**
```
Repos (Git) -> Indexer: parse (tree-sitter), symbol graph, chunk by function/class,
  embeddings + keyword/code search (Zoekt) ; docs/ADRs/runbooks too
IDE plugin / CLI / PR bot -> Gateway (auth, repo ACLs, logging)
  -> Context builder: open files, cursor, retrieved symbols, call graph neighbours
  -> LLM (large for chat/agent, small fast for completion)
  -> Agent mode: tools (read file, grep, run tests in sandbox, open PR)
```

**Deep dives:** Retrieval: combine exact search (symbols, grep) with semantic search; code understanding benefits more from symbol graph than pure embeddings. Agentic changes run in sandboxed containers with tests; output is a PR for human review, never a direct push. Secrets scanning on context and outputs. Incremental re-index on each commit.

**Trade-offs:** Buy (Copilot, Claude Code, Cursor, Sourcegraph Cody) vs build; usually buy and add internal context via MCP/indexes. Self-hosted model (privacy) vs API (quality).

**Customer/deployment:** Legal/security review on code egress; enterprise agreements with zero data retention; rollout to pilot teams.

**Metrics:** acceptance rate of suggestions, PRs merged from agent, time-to-merge, developer satisfaction, defect rate of AI-authored code.

### Design a contract review / legal document analysis assistant.

**Clarify:** Contract types (NDAs, MSAs, leases)? Task: extract clauses, compare to playbook, redline, summarise? Users (in-house lawyers, paralegals)? Volume? Confidentiality requirements?

**Requirements:** Extract key terms and clauses with exact citations; flag deviations from company playbook with risk level; suggest fallback language; lawyers make final decisions; strong confidentiality; audit.

**Architecture:**
```
Upload / CLM integration (Ironclad, DocuSign CLM) -> parse (DOCX/PDF, OCR, structure)
 -> Clause segmentation + classification (indemnity, limitation of liability, termination...)
 -> Per-clause LLM analysis vs playbook (standard position, acceptable fallbacks, red lines)
 -> Issues list with citations + suggested redlines -> Word add-in / review UI
 -> Lawyer accepts/edits -> feedback store
```

**Data model:** `contract`, `clause(contract_id, type, text, span)`, `playbook_rule(clause_type, standard, fallbacks, red_flags)`, `finding(clause_id, rule_id, risk, explanation, suggested_text, status)`.

**Deep dives:** Long documents: process clause by clause with the definitions section included as context; use long-context models for cross-references. Every finding must quote the exact span (verifiable). Playbook is structured data owned by legal, not buried in prompts. Evaluate against lawyer-annotated contracts.

**Trade-offs:** Recall over precision for risk flags (missing an uncapped indemnity is worse than an extra flag), but too many flags kills adoption.

**Customer/deployment:** Privilege and confidentiality: zero-retention model endpoints or private deployment; access control by matter. Integrate into Word, where lawyers live.

**Metrics:** clause extraction F1, issue recall on annotated set, review time per contract, suggestion acceptance rate.

### Design a call-centre copilot that listens live and suggests answers.

**Clarify:** Telephony platform (Genesys, Amazon Connect, Twilio)? Concurrent calls? Languages? Suggestions: knowledge answers, next best action, compliance prompts, after-call summary? Recording consent?

**Requirements:** Real-time transcription with speaker separation; suggestions within 1-2 s of a customer question; non-intrusive UI; post-call summary and CRM notes; PII redaction; scale to thousands of concurrent calls.

**Architecture:**
```
Telephony media fork (SIPREC / streaming API) -> Streaming ASR (per channel, diarised)
  -> Transcript stream (Kafka/WebSocket) -> Copilot service per call:
       intent/question detection (small model, every utterance)
       -> RAG over KB + customer context (CRM lookup)
       -> suggestion (LLM, streamed) -> agent desktop UI (WebSocket)
       compliance checker (required disclosures said?)
  -> End of call: summary + disposition + CRM write
```

**Deep dives:** Don't call a big LLM on every word; trigger on detected questions/intents and debounce. Pre-fetch customer context at call start. Latency budget: ASR partials ~300 ms, retrieval 100 ms, LLM first tokens ~500 ms. PII redaction in transcripts (card numbers, using pause/resume recording for PCI).

**Trade-offs:** Suggestion frequency vs distraction; agents ignore noisy copilots. Cloud ASR (accuracy) vs on-prem (data control).

**Customer/deployment:** Pilot with one team, A/B against control; train agents; union/works-council concerns about monitoring.

**Metrics:** suggestion usage rate, average handle time, after-call work time, first-call resolution, CSAT, ASR word error rate.

### Design a voice agent for appointment scheduling (latency budget!).

**Clarify:** Inbound phone, outbound, or both? Scheduling system (EHR, calendar API)? Languages? Volume? Edge cases: rescheduling, cancellations, insurance questions? When to transfer to a human?

**Requirements:** Natural conversation with < ~800 ms voice-to-voice response; book/reschedule/cancel correctly; confirm details; handle interruptions (barge-in); transfer to human; HIPAA if healthcare.

**Architecture:**
```
Phone (SIP/Twilio) -> Media server (WebRTC/RTP)
  -> VAD + streaming ASR (partials) -> Turn detection
  -> Dialogue LLM (streaming, tools: check_availability, book, cancel, lookup_patient)
  -> Streaming TTS -> back to caller
Scheduling API (cached availability) ; transcripts + audit
```

**Latency budget (target ~700-800 ms after user stops speaking):**
- End-of-turn detection: 200-300 ms
- ASR final: ~100 ms (streaming partials)
- LLM time to first token: 200-350 ms (small/fast model, short prompt, prompt caching)
- TTS first audio: ~100 ms (streaming)
- Network: ~50 ms (co-locate all services in one region)

**Deep dives:** Stream everything and start TTS on the first sentence. Pre-fetch availability at call start; use filler ("let me check that") when a tool call is slow. Barge-in: stop TTS immediately when the caller speaks. Confirm critical info (date, time, name) explicitly; spell back. Speech-to-speech models reduce latency but are harder to control with tools.

**Trade-offs:** Bigger model = better reasoning, worse latency. Strict scripted flows vs free conversation.

**Metrics:** voice-to-voice latency p50/p95, task completion rate, transfer rate, booking errors, call duration, caller satisfaction.

### Design a document-extraction system (invoices, forms) with LLMs and human review.

**Clarify:** Document variety (100 vendor layouts vs 10 fixed forms)? Volume and SLA? Fields (header + line items)? Downstream (ERP AP posting)? Accuracy target?

**Requirements:** Extract structured JSON with schema validation; confidence per field; human review for uncertain cases; learn from corrections; integrate with ERP; cost per page acceptable.

**Architecture:**
```
Ingest (email/upload/SFTP) -> classify -> OCR/layout (text + bboxes) or vision LLM
 -> LLM extraction with JSON schema (structured output) + few-shot per doc type/vendor
 -> Validators: schema, arithmetic (line totals = subtotal; subtotal+tax = total),
    master data (vendor exists, PO matches), duplicates
 -> Confidence routing: auto-post | review queue (UI with doc + highlighted fields)
 -> ERP integration ; corrections -> eval set + few-shot examples per vendor
```

**Data model:** `document`, `extraction(field, value, confidence, source_span)`, `validation_result`, `review(user, corrections)`, `vendor_profile(few_shot_examples)`.

**Deep dives:** LLM confidence is poorly calibrated; derive confidence from validators, agreement between two extraction passes/models, and OCR quality. Arithmetic and 3-way match (PO, receipt, invoice) catch most errors cheaply. Store source spans so reviewers verify quickly. Vendor-specific examples improve accuracy for frequent vendors.

**Trade-offs:** Vision LLM end-to-end (simpler, handles layout) vs OCR + text LLM (cheaper, auditable). Cost vs accuracy: small model first, escalate to a larger one on validation failure.

**Metrics:** field-level accuracy, STP rate, review time, cost per document, duplicate payments caught.

### Design an LLM evaluation platform / eval harness for a customer.

**Clarify:** What apps (RAG, agents, extraction)? Who runs evals (engineers in CI, domain experts)? What does "good" mean for them? Existing labelled data? Models compared?

**Requirements:** Versioned datasets; run evals for any change (prompt, model, retrieval); multiple scorers; compare runs; human review UI; CI gating; production sampling.

**Architecture:**
```
Datasets (golden sets, versioned; from prod logs + expert-written + synthetic)
 -> Runner: config = {app version, prompt, model, params} x dataset ; parallel, cached
 -> Scorers: exact/regex/JSON schema, programmatic checks, LLM-as-judge (rubrics),
    retrieval metrics (recall@k), human ratings
 -> Results store -> comparison UI (diffs per example, regressions) -> CI gate
 -> Online: sample prod traces -> async scoring -> dashboards + new test cases
```

**Data model:** `dataset(version)`, `example(input, expected, tags)`, `run(config, git_sha, ts)`, `result(run_id, example_id, output, scores, cost, latency)`, `human_label`.

**Deep dives:** LLM judges must be validated against human labels (agreement rate) and use specific rubrics, pairwise comparisons, and position-swap for bias. Slice results by tags (topic, difficulty) since averages hide regressions. Statistical significance: report confidence intervals; small sets are noisy. Feed production failures into the dataset continuously.

**Trade-offs:** Build vs adopt (Braintrust, LangSmith, Promptfoo, Inspect). Judge cost vs coverage.

**Customer/deployment:** Domain experts define rubrics; make labelling easy for them. Keep data in their environment.

**Metrics:** eval coverage of use cases, judge-human agreement, time to run suite, regressions caught before prod.

### Design an LLM gateway: routing between models, rate limits, caching, cost tracking, fallbacks.

**Clarify:** Number of internal apps/teams? Providers (Anthropic, OpenAI, Bedrock, Azure, self-hosted)? Peak RPS and token volume? Need streaming? Chargeback per team? Data policies (PII, region)?

**Requirements:** Single API for all apps; authn per app/team; quotas; provider failover; caching; cost attribution; logging with PII controls; < 20 ms added latency; streaming support.

**Architecture:**
```
Apps -> Gateway (stateless, horizontally scaled; LiteLLM/Envoy/Kong or custom)
  1. Auth (API key/JWT -> team, app) 2. Policy (allowed models, PII redaction, region)
  3. Rate limit (Redis token buckets: RPM + TPM per team/model)
  4. Cache lookup (exact hash; optional semantic) 5. Router (model alias -> provider/deployment,
     weights, health) 6. Provider adapters (normalised schema, streaming passthrough)
  7. Fallback on 429/5xx/timeout (retry with backoff, then alt provider/model)
  -> async: usage events -> Kafka -> cost DB (tokens x price table) -> dashboards/budgets
```

**Deep dives:** Token-based rate limiting needs estimating tokens before the call and reconciling after. Model aliases ("fast", "smart") decouple apps from vendor names and allow migrations. Fallback across providers requires prompt/feature compatibility (tool schemas differ). Circuit breakers per provider. Streaming: proxy SSE without buffering; count tokens from final usage events. Leverage provider prompt caching for long shared prefixes.

**Trade-offs:** Central gateway is a single point of failure: deploy multi-AZ, keep it stateless. Logging full prompts aids debugging but is a privacy risk: configurable per app, redaction, retention limits.

**Metrics:** added latency, error rate per provider, fallback rate, cache hit rate, cost per team/app, quota rejections.

### Design a system that classifies and routes 1M support tickets/day with an LLM.

**Clarify:** Number of categories/queues (20 vs 500)? Labels from history? Latency (seconds vs minutes)? Multi-label? Languages? Cost budget?

**Scale:** 1M/day ≈ 12/s average, ~50/s peak. At ~1k input tokens per ticket that's 1B input tokens/day, so model choice drives cost.

**Requirements:** Assign category, priority, team, language, sentiment; high accuracy on routing; low cost; explainable; handle new categories quickly.

**Architecture:**
```
Ticketing system (Zendesk/ServiceNow) webhook -> Queue (SQS/Kafka)
 -> Preprocess (strip signatures, truncate, detect language, PII mask)
 -> Tier 1: fast classifier (fine-tuned small model / embeddings + logistic regression)
       confident -> route
 -> Tier 2: LLM (small/mid model, structured output w/ taxonomy + few-shot) for uncertain
 -> Rules overlay (VIP customers, legal keywords -> priority)
 -> Write back to ticketing API (idempotent) ; low-confidence -> triage queue
 -> Agent re-routes = labels -> retraining & eval
```

**Deep dives:** Cascade: a cheap classifier handles ~70-80% confidently, LLM handles the rest; or distil LLM labels into a small model. Taxonomy in the prompt with clear definitions and examples; constrained output (enum). Use batch APIs for non-urgent tickets (≈50% cheaper) and prompt caching for the static taxonomy prefix. Measure with re-route rate as an implicit label.

**Trade-offs:** LLM on everything (simple, costly) vs cascade (cheaper, more components). Latency vs batch discounts.

**Metrics:** routing accuracy (vs agent final queue), re-route rate, time to first response, cost per 1k tickets, throughput, % to triage.

### Design a text-to-SQL analytics assistant over a company's warehouse.

**Clarify:** Warehouse and size (hundreds vs thousands of tables)? Users (execs, analysts)? Existing semantic layer or dbt docs? Row-level security? Accuracy expectations (financial numbers)?

**Requirements:** Natural language to correct SQL, executed safely with user's permissions; show the SQL and assumptions; charts/tables; handle ambiguity by asking; never mutate data; cost controls.

**Architecture:**
```
User question -> Orchestrator
  -> Schema retrieval: relevant tables/columns via embeddings over dbt docs, column
     descriptions, sample values, and past verified queries
  -> (prefer) semantic layer: map question -> metrics/dimensions (dbt SL/Cube)
  -> else LLM generates SQL with retrieved schema + few-shot verified examples
  -> Validate: parse (sqlglot), allow-list SELECT only, table permissions, LIMIT, cost estimate (dry run)
  -> Execute as the user (RLS) with timeout -> results -> LLM summary + chart
  -> Self-correct on errors (max 2 retries) ; feedback -> verified query library
```

**Deep dives:** Accuracy comes from context: curated metric definitions, column descriptions, join paths, and a library of verified question-SQL pairs. Generating against a semantic layer is far more reliable than raw SQL over 2,000 tables. Read-only service user plus user-scoped credentials. Show the SQL and a plain-English interpretation so users can catch wrong assumptions.

**Trade-offs:** Flexibility (raw SQL) vs correctness (semantic layer). Agentic exploration (query to inspect values) improves accuracy but costs time.

**Metrics:** execution accuracy on a golden set of questions, % answered without analyst, user thumbs-up rate, query cost, latency.

### Design a content-moderation pipeline using LLMs.

**Clarify:** Content types (text, images, video, live chat)? Volume and latency (pre-publish blocking vs post-hoc)? Policy categories? Jurisdictions (DSA, local laws)? Appeals process?

**Requirements:** Enforce a written policy consistently; block clear violations fast; send borderline content to humans; low false positives; appeals; audit and transparency reporting; adapt quickly to new abuse.

**Architecture:**
```
Content -> Fast path: hash matching (PhotoDNA/PDQ for known CSAM/terror), keyword/regex,
           small classifiers (toxicity, spam) -> clear-block / clear-allow
 -> Uncertain -> LLM policy reasoner (policy text in prompt, structured output:
           category, severity, policy clause, confidence, rationale)
 -> Decision engine (thresholds per category + user reputation) -> allow | limit | remove
 -> Human review queues (priority by severity & reach) -> decisions -> labels
 -> Appeals -> second reviewer ; Audit + transparency reports
```

**Deep dives:** Tiered cascade for cost: LLMs only on the ambiguous ~5-10%. Policy-as-prompt makes policy changes fast, but every change needs an eval on a labelled set. Adversarial users: obfuscation, image text (OCR), multilingual; red-team regularly. Reviewer wellbeing: blur, rotation, limits for graphic content.

**Trade-offs:** Precision vs recall varies per category (zero tolerance for CSAM; lenient for mild profanity). Pre-publish latency vs safety.

**Metrics:** prevalence of violating content, precision/recall per category, appeal overturn rate, time to action, reviewer throughput, cost per 1k items.

### Design a personalised recommendation system with LLM re-ranking.

**Clarify:** Domain (e-commerce, content, jobs)? Catalogue size and users? Latency (< 200 ms for page load)? Signals available? Why LLM re-ranking (cold start, explanations, natural-language intent)?

**Requirements:** Relevant, diverse recommendations; real-time context; explanations; cold start for new items/users; within latency and cost budgets.

**Architecture:**
```
Offline: interaction logs -> train two-tower embeddings + ranking model (GBM/DNN)
Online:
  1. Candidate generation (ANN over item embeddings, co-visitation, popular, recent)  ~1000
  2. Ranking model with features (user history, item stats, context)                 ~100
  3. LLM re-rank (top 20-50): user profile summary + intent + item descriptions
     -> reorder, diversify, generate short explanations
  4. Business rules (inventory, compliance, dedupe) -> serve
Feature store (online Redis) ; logging -> A/B testing
```

**Deep dives:** LLM is too slow and costly for every request: apply it only on top-k, cache per user segment or precompute nightly for email/home feed, use small fast models. LLM-generated item descriptions/attributes improve cold-start embeddings offline. Precompute user profile summaries asynchronously. Guard against position bias and hallucinated item IDs (constrain output to given IDs).

**Trade-offs:** Quality vs latency/cost; must prove LLM stage adds lift in an A/B test over the classic ranker.

**Metrics:** CTR, conversion, revenue per session, diversity/novelty, coverage, latency p99, cost per 1k recommendations.

### Design guardrails for an LLM app in a regulated industry (healthcare, finance).

**Clarify:** What does the app do (patient Q&A, advisor assistant)? Who are users (clinicians, consumers)? Regulations (HIPAA, FINRA, SEC marketing rules, EU AI Act, FDA SaMD)? What must it never do (diagnose, give personalised investment advice)?

**Requirements:** Prevent prohibited outputs, PII/PHI leakage, hallucinated facts; resist prompt injection; required disclaimers; human oversight; full audit and record-keeping; demonstrable testing.

**Layered design:**
- **Input guardrails:** authn/authz, PII detection/redaction (Presidio), prompt-injection/jailbreak classifier, topic/scope classifier (out-of-scope -> polite refusal).
- **Context controls:** retrieval only from approved, versioned sources; permission-filtered; untrusted content clearly delimited.
- **Model layer:** system prompt with policy; low temperature; structured outputs; tool permissions least-privilege.
- **Output guardrails:** groundedness check vs retrieved sources, policy classifiers (advice, diagnosis, promissory language), PII scan, mandatory disclaimers, confidence-based escalation to a human.
- **Human-in-the-loop:** for high-risk actions or categories; clinician/advisor approval.
- **Audit:** immutable log of inputs, retrieved context, model version, outputs, guardrail decisions (retention per FINRA 17a-4 / HIPAA).

**Deep dives:** Guardrails are tested like features: red-team suites, adversarial eval sets, regression runs on every change. Deterministic checks where possible (regex, allow-lists) over LLM judges. Fail closed for high-risk categories.

**Trade-offs:** Each layer adds latency and false refusals; tune with eval data. Over-blocking hurts usefulness.

**Customer/deployment:** BAA with model provider, zero retention, in-region processing; model risk management documentation; compliance sign-off.

**Metrics:** violation rate on red-team set, false refusal rate, groundedness, escalations, incidents.

### Design a fine-tuning pipeline for a customer's domain data.

**Clarify:** Why fine-tune (format/style, domain vocabulary, latency/cost via smaller model) vs prompting/RAG? Data volume and quality? Model allowed (open-weight self-hosted vs provider fine-tuning API)? Compute budget? How often to retrain?

**Requirements:** Reproducible pipeline from raw data to deployed model; data governance (PII removal, consent); evaluation gates vs baseline; versioning; rollback.

**Architecture:**
```
Raw data (tickets, documents, expert answers) -> PII scrub + dedupe + quality filter
 -> Formatting into instruction/response pairs (+ synthetic data from a stronger model, reviewed)
 -> Split train/val/test (by time/customer to avoid leakage) -> dataset version (DVC/lakeFS)
 -> Training: LoRA/QLoRA SFT (HF TRL, Axolotl) on GPUs, or provider fine-tune API; then optional DPO with preference pairs
 -> Eval: task metrics + general capability regression + safety -> registry (MLflow)
 -> Deploy (vLLM/TGI, LoRA adapters) with canary -> monitor -> new data loop
```

**Deep dives:** Data quality matters more than quantity; 1-5k excellent examples often beat 100k noisy ones. Always compare against strong prompting + RAG baseline first. Watch catastrophic forgetting and safety regressions. LoRA adapters allow per-customer variants on a shared base model.

**Trade-offs:** Fine-tuning locks you to a base model version; when a better base model ships, you redo it. Knowledge belongs in RAG; behaviour/format in fine-tuning.

**Customer/deployment:** Data stays in customer environment; model weights are customer IP; document data lineage for compliance.

**Metrics:** task eval score vs baseline, cost/latency per request vs larger model, regression on safety set, training cost.

### Design a semantic cache for LLM responses.

**Clarify:** Traffic pattern (many repeated FAQs?) Answers personalised or user-specific? Freshness requirements? Tolerance for a "close but wrong" cached answer?

**Requirements:** Return cached answers for semantically equivalent queries; never serve another user's private data; avoid stale answers; measurable hit rate and quality; low lookup latency (< 20 ms).

**Architecture:**
```
Request -> normalise -> exact-match cache (hash of model+prompt+params) -> hit? return
  -> embed query (small fast embedding model) -> ANN search in cache index
     (scoped by tenant, model, prompt version, locale)
  -> similarity >= threshold (+ optional cheap verifier) -> return cached
  -> else call LLM -> store {embedding, query, response, metadata, ttl}
```

**Data model:** `cache_entry(id, scope_key, query_text, embedding, response, model, prompt_version, source_doc_versions, created_at, ttl, hit_count)`.

**Deep dives:** Threshold tuning is everything: too low returns wrong answers ("cancel order" vs "cancel subscription"). Tune on labelled pairs; consider a cross-encoder verifier. Only cache non-personalised, non-tool-dependent answers. Invalidate on KB document updates (track source versions) and prompt/model changes. Use provider prompt caching for long static prefixes; that's a different, complementary saving.

**Trade-offs:** Hit rate vs correctness. Redis with vector search, or a vector DB plus Redis for exact hits.

**Metrics:** hit rate (exact vs semantic), false hit rate (sampled review), latency saved, cost saved, staleness incidents.

### Design an MCP server that exposes a customer's internal tools to Claude/ChatGPT.

**Clarify:** Which internal systems (ticketing, CRM, DB, internal APIs)? Read vs write? Which clients (Claude Desktop, Claude Code, custom agent)? Local (stdio) or remote (HTTP)? User-level auth needed?

**Requirements:** Expose well-designed tools, resources, and prompts over the Model Context Protocol; per-user authentication and authorisation; safe write operations; audit; rate limits; easy to add tools.

**Architecture:**
```
MCP client (Claude) --Streamable HTTP + OAuth 2.1--> MCP server (remote, behind gateway)
   tools: search_tickets, get_customer, create_ticket, run_report(...)
   resources: docs, schemas ; prompts: common workflows
   -> AuthZ layer (user token -> internal permissions; on-behalf-of token exchange)
   -> Internal APIs / DBs (least-privilege service accounts)
   -> Audit log ; rate limiting ; observability
```

**Deep dives:** **Tool design:** few, task-oriented tools with clear names, descriptions, typed JSON schema inputs and concise outputs (pagination, truncation) beat 200 thin API wrappers. Return helpful error messages the model can act on. **Auth:** MCP's OAuth flow with the corporate IdP; act as the user, not a superuser. **Safety:** read-only by default; write tools flagged so clients prompt for confirmation; validate inputs server-side; treat any text returned from internal systems as untrusted (prompt-injection risk). Version tools; log every call.

**Trade-offs:** Local stdio server (simple, runs with user's credentials on their machine) vs remote (central control, auth, auditing; better for enterprise).

**Metrics:** tool call success/error rate, latency, calls per user, denied calls, task success in evals.

### Design a batch-processing system that runs an LLM over 50M records cheaply.

**Clarify:** Task (classify, extract, summarise)? Tokens per record in/out? Deadline (days OK)? Accuracy needed? Data location and sensitivity?

**Estimate:** 50M records x 500 input tokens = 25B input tokens; plus 50 output tokens each = 2.5B output tokens. Cost varies by >10x across models, so model choice and batching dominate.

**Design:**
```
Source table -> Partitioner (chunks of 10k records, manifest with status)
  -> Preprocess: dedupe (exact + near-dup), truncate, skip records rules can handle
  -> Pilot: 1k sample across models -> eval vs labelled set -> choose cheapest passing model
  -> Submit via provider Batch APIs (~50% discount, 24h SLA) or self-hosted vLLM on spot GPUs
  -> Collect results -> validate (schema) -> retry failures -> write to output table
  -> Sampled QA ; checkpointed, resumable, idempotent per record ID
```

**Cost levers:** dedupe (often 20-40% fewer calls); smallest model that passes eval; prompt caching for shared instructions; batch API discount; pack multiple short records per request; constrain output length (JSON, enums); cascade (cheap model, escalate low-confidence to bigger model); distil into a fine-tuned small model if the task recurs.

**Deep dives:** Rate limits: concurrency control and backoff; job state table for resumability. Determinism: temperature 0, prompt and model versions stored with outputs.

**Metrics:** cost per 1k records, throughput, failure rate, sampled accuracy, total wall-clock time.

### Design observability for an LLM application (traces, costs, quality).

**Clarify:** App type (RAG, agent)? Volume? Existing stack (Datadog, Grafana)? Privacy constraints on logging prompts? Who consumes (engineers, PMs, domain experts)?

**Requirements:** End-to-end traces of each request including retrieval, tool calls, model calls; tokens, cost, latency per step; quality signals; searchable; alerting; privacy controls.

**Architecture:**
```
App instrumented with OpenTelemetry (GenAI semantic conventions) / SDK (Langfuse, LangSmith, Arize, Braintrust)
  spans: request -> retrieval (query, doc ids, scores) -> LLM call (model, prompt version,
         tokens in/out, cached tokens, latency, finish reason) -> tool calls -> response
  -> Collector -> trace store (+ PII redaction) -> dashboards
  -> Async evaluators on sampled traces (groundedness, relevance, toxicity, LLM-judge)
  -> User feedback joined by trace id -> Alerts -> curated failures into eval datasets
```

**Key metrics:** latency (TTFT and total, per step), error rates (provider errors, timeouts, refusals, schema failures), tokens and cost by feature/tenant/model, retrieval stats (empty results, low scores), quality scores over time, user feedback rate, agent step counts and loops.

**Deep dives:** Version everything (prompt, model, retrieval config) on the span so regressions can be attributed to a change. Sampling: 100% metadata, sampled full payloads. Redact PII before storage; retention limits. Drift: watch input topic distribution changes.

**Alerts:** cost spike per tenant, error-rate jump, latency p95, groundedness drop, refusal rate change.

**Trade-offs:** Full prompt logging vs privacy; vendor tool vs self-hosted (data residency).

## C. Classic system design (still asked)

### URL shortener

**Clarify:** Custom aliases? Expiry? Analytics? Scale: assume 100M new URLs/month, 100:1 read:write.

**Estimates:** ~40 writes/s, ~4k reads/s (peaks 10x). 5 years ≈ 6B URLs x ~500 bytes ≈ 3 TB. 7 base62 chars = 3.5 trillion codes, enough.

**Requirements:** Create short URL, redirect fast (< 50 ms), highly available reads, no collisions, abuse prevention.

**Architecture:**
```
Client -> LB -> API (create) -> ID generator -> DB (code -> long_url)
Client -> CDN/edge -> Redirect service -> Redis cache -> DB  ; click events -> Kafka -> analytics
```

**Data model:** `url(code PK, long_url, user_id, created_at, expires_at)`. Key-value access by code: DynamoDB/Cassandra or sharded Postgres.

**Deep dives:**
- **Code generation:** counter-based IDs (range allocation per server from a coordinator, or Snowflake IDs) encoded base62 — no collisions, no retries. Avoid sequential guessability by shuffling bits. Hash-based (MD5 truncated) needs collision checks.
- **Redirect:** 301 (cached by browsers, fewer hits, loses analytics) vs 302 (every click counted). Usually 302.
- **Caching:** hot links follow a power law; Redis LRU absorbs most reads.
- **Abuse:** rate limit creates, scan URLs against Safe Browsing.

**Trade-offs:** Counter needs coordination; hash avoids it but has collisions. NoSQL scales simply for key lookups.

**Metrics:** redirect p99, cache hit rate, create error rate, abuse reports.

### Rate limiter (distributed)

**Clarify:** Limit by user, API key, IP? Limits per endpoint? Hard or soft? Scale (e.g. 1M RPS across 100 gateway nodes)? Accuracy needed?

**Requirements:** Enforce limits across all nodes with low added latency (< 5 ms), fail gracefully, return 429 with `Retry-After` and rate headers, configurable rules.

**Algorithms:**
- **Token bucket:** allows bursts, smooth refill; most common (AWS, Stripe).
- **Leaky bucket:** smooth output rate.
- **Fixed window counter:** simple, but 2x burst at window edges.
- **Sliding window log:** exact but memory-heavy.
- **Sliding window counter:** weighted previous + current window; good approximation, cheap.

**Architecture:**
```
Client -> API gateway nodes -> rate limit middleware
  -> Redis cluster (key = limit:{api_key}:{endpoint}) with Lua script for atomic
     check-and-decrement (token bucket: tokens, last_refill_ts)
  -> Rules config (per plan/tier) cached locally, updated via pub/sub
```

**Deep dives:** Atomicity via Lua scripts (or `INCR` + `EXPIRE` for fixed window). Latency/hot keys: local in-memory buckets with periodic sync to Redis (approximate, much faster); shard keys across Redis cluster. Failure: if Redis is down, fail open with local limits (availability) unless protecting something critical. Multi-region: per-region limits or async global aggregation (accept slight overshoot).

**Trade-offs:** Exact global accuracy vs latency; most systems accept ~5-10% overshoot.

**Metrics:** 429 rate per client, limiter latency, Redis errors, false throttles.

### Notification system (email/SMS/push)

**Clarify:** Volume (e.g. 100M/day, bursts from marketing campaigns)? Transactional vs marketing? Channels and providers? User preferences and quiet hours? Delivery guarantees?

**Requirements:** Send via multiple channels; templating and localisation; user preferences and opt-outs (legal: CAN-SPAM, TCPA, GDPR); priority (OTP before marketing); retries and provider failover; dedup; tracking (sent, delivered, opened).

**Architecture:**
```
Services -> Notification API (validate, idempotency key) -> Kafka (by priority)
 -> Orchestrator: preferences/opt-out check, rate limit per user, template render, schedule/quiet hours
 -> Channel queues: email | SMS | push | in-app
 -> Channel workers -> providers (SES/SendGrid, Twilio, APNs/FCM) with failover
 -> Status webhooks -> event store -> analytics ; DLQ
```

**Data model:** `notification(id, user_id, type, channel, template_id, status, idempotency_key)`, `user_preferences(user_id, channel, category, opted_in, quiet_hours)`, `device_token`, `template(versioned, locale)`, `delivery_event`.

**Deep dives:** Separate queues by priority so a 50M-user marketing blast doesn't delay password-reset OTPs. Idempotency key prevents duplicates on retries. Provider failover and per-provider rate limits. Clean invalid device tokens on APNs/FCM feedback. Digesting/batching to avoid spamming users.

**Trade-offs:** At-least-once delivery means occasional duplicates; dedupe window per idempotency key. Build vs buy (Braze, Customer.io, Courier).

**Metrics:** send latency by priority, delivery rate per channel/provider, bounce and complaint rates, opt-out rate, queue lag.

### Chat / messaging app (WhatsApp/Slack)

**Clarify:** 1:1 and groups (max size)? Channels like Slack? Online presence, read receipts, media, search, E2E encryption? Scale: assume 100M DAU, 50 messages/user/day ≈ 5B msgs/day ≈ 60k/s average.

**Requirements:** Real-time delivery (< 200 ms when online), offline delivery on reconnect, ordering within a conversation, message persistence and sync across devices, high availability.

**Architecture:**
```
Clients <-> WebSocket gateways (stateful connections; millions per fleet)
  -> Chat service: assign per-conversation sequence number, persist, fan out
  -> Message store (Cassandra/ScyllaDB, partition by conversation_id, cluster by seq)
  -> Session registry (Redis: user -> gateway node) ; pub/sub between gateways
  -> Offline users -> push notification service
  -> Media -> object store + CDN (upload via pre-signed URLs)
```

**Data model:** `message(conversation_id, seq, message_id, sender_id, body, ts)`, `conversation_member(conversation_id, user_id, last_read_seq)`, `user_inbox` or per-device sync cursor.

**Deep dives:** Ordering: per-conversation monotonic sequence from a single leader per conversation (or partition). Delivery: client acks; server resends unacked; client dedupes by message_id (client-generated). Sync: devices request messages after their last seq per conversation. Large groups/channels: fan out on read (members pull from channel) rather than write to every inbox. Presence is expensive: heartbeats, debounced and lazily fetched.

**Trade-offs:** Fan-out on write (fast reads, expensive for big groups) vs on read. E2E encryption (Signal protocol) prevents server-side search.

**Metrics:** delivery latency p99, connection count, message loss (sequence gaps), reconnect rate.

### News feed (Twitter/Instagram)

**Clarify:** Chronological or ranked? Scale: 300M DAU, 200 follows average, some accounts with 100M followers. Feed load < 200 ms.

**Estimates:** posts ~500M/day (~6k/s); feed reads ~100k/s.

**Architecture:**
```
Post service -> posts DB + media CDN -> Kafka "new post"
  -> Fan-out service: for normal authors push post_id to each follower's feed cache (Redis list, last ~800)
Feed service (read): follower's precomputed feed + pull celebrity posts at read time
  -> merge -> ranking model -> hydrate (post, author, counts) -> client (cursor pagination)
```

**Data model:** `post(id, author_id, content, media_urls, ts)` (sharded by post id), `follow(follower_id, followee_id)` (graph store, both directions indexed), feed cache `feed:{user_id} -> [post_ids]`.

**Deep dives:** **Hybrid fan-out:** push for regular users (fast reads), pull for celebrities (avoid writing 100M entries per post). Skip fan-out to inactive users; rebuild their feed on login. Ranking: candidate posts -> ML model (engagement prediction) -> diversity rules. Counters (likes) via sharded counters and async aggregation. Pagination with cursors, not offsets.

**Trade-offs:** Write amplification vs read latency. Feed freshness vs cost: eventual consistency is fine.

**Metrics:** feed load latency, fan-out lag, cache hit rate, engagement (CTR, time spent).

### Web crawler / search engine

**Clarify:** Scope (whole web vs some domains)? Pages to crawl (1B/month)? Freshness? Search features (ranking, snippets)?

**Estimates:** 1B pages/month ≈ 400 pages/s; ~100 KB/page ≈ 100 TB/month raw (compress).

**Crawler architecture:**
```
Seed URLs -> URL frontier (priority queues + per-host politeness queues)
 -> Fetchers (async HTTP, DNS cache, robots.txt cache, per-host rate limits)
 -> Content store (object storage, compressed) -> Parser (links, text)
 -> Dedup (URL normalisation + seen set via Bloom filter; content fingerprint SimHash)
 -> new URLs back to frontier
```

**Search architecture:**
```
Parsed docs -> Indexer (tokenise, inverted index: term -> posting list (doc_id, positions))
  -> sharded by document; PageRank/link analysis offline
Query -> query parsing -> scatter to index shards -> top-k per shard (BM25 + signals)
  -> gather + re-rank (ML) -> snippets -> results ; cache popular queries
```

**Deep dives:** Politeness: one connection per host, respect robots.txt and crawl-delay. Prioritisation: PageRank estimate, change frequency for re-crawl scheduling. Traps: infinite URL spaces (calendars) → URL depth/pattern limits. Index sharding by doc (each shard full index of subset) is simpler for scale-out; replicate shards for QPS.

**Trade-offs:** Freshness vs coverage under fixed crawl budget. Doc-partitioned vs term-partitioned index.

**Metrics:** pages/sec, crawl errors, duplicate rate, index freshness, query latency, relevance (NDCG).

### Typeahead / autocomplete

**Clarify:** Corpus (search queries, products, users)? Personalised? Languages? Scale: 10k-100k QPS since every keystroke triggers a request; < 100 ms end to end.

**Requirements:** Top-k (e.g. 10) suggestions by prefix, ranked by popularity/recency, filtered for offensive terms, near-real-time trending updates.

**Architecture:**
```
Query logs -> aggregation job (hourly/daily counts with decay) -> build trie / prefix table
  -> distribute to suggestion servers (in-memory)
Client (debounce 100 ms, local cache) -> CDN/edge cache for short prefixes
  -> Suggestion service (in-memory trie with top-k cached at each node) -> results
Trending: stream processing (Flink) updates a small hot overlay
```

**Data model:** trie where each node stores top-k completions precomputed, or a simple `prefix -> [top suggestions]` map in Redis/in-memory (prefixes up to ~20 chars). Sharded by first characters if large.

**Deep dives:** Precomputing top-k per node makes lookups O(prefix length). Rebuild offline and swap atomically (blue/green). Personalisation: merge global suggestions with user history client-side or in a second lookup. Fuzzy matching for typos: edit-distance or n-gram index (Elasticsearch completion suggester). Filter blocklisted terms at build time.

**Trade-offs:** Freshness vs build cost; memory of top-k per node vs computing at query time.

**Metrics:** latency p99, suggestion CTR, keystrokes saved, coverage.

### Distributed key-value store

**Clarify:** Data size and QPS? Value sizes? Consistency needs (strong vs eventual)? Latency targets? Multi-region?

**Requirements:** `put(k, v)`, `get(k)`, `delete(k)`; horizontal scale; high availability; tunable consistency; durability; automatic recovery.

**Design (Dynamo-style, AP with tunable consistency):**
- **Partitioning:** consistent hashing with virtual nodes; easy rebalancing.
- **Replication:** N=3 replicas on successive nodes, across racks/AZs.
- **Quorums:** R + W > N for strong-ish reads (e.g. N=3, W=2, R=2); lower for latency.
- **Write path:** commit log (WAL) -> memtable -> flush to SSTables (LSM tree); compaction; Bloom filters for reads.
- **Conflict resolution:** vector clocks or last-write-wins timestamps; read repair.
- **Failure handling:** gossip membership + failure detection; hinted handoff for temporarily down nodes; Merkle trees for anti-entropy repair.

**Alternative (CP):** Raft per shard range (like etcd/TiKV/CockroachDB): leader per range gives linearisable reads/writes; unavailable for minority partitions.

**Trade-offs:** AP (always writable, conflicts possible) vs CP (consistent, unavailable during partitions). LSM (write-optimised) vs B-tree (read-optimised).

**Metrics:** p99 latency, replication lag, hinted handoffs pending, compaction backlog, disk usage per node.

### Distributed cache

**Clarify:** What is cached (DB rows, sessions, rendered pages)? Size (e.g. 10 TB) and QPS (1M/s)? Consistency tolerance? Eviction policy?

**Requirements:** Sub-ms gets, horizontal scale, high hit rate, tolerate node loss without stampeding the DB.

**Architecture:** Cluster of cache nodes (Redis Cluster / Memcached) with consistent hashing (client-side or proxy like Twemproxy/mcrouter); replicas for hot data/availability.

**Patterns:**
- **Cache-aside** (most common): read cache → miss → DB → populate with TTL. Writes: update DB, then delete cache key.
- **Write-through/write-behind** for specific cases.
- **Eviction:** LRU/LFU with TTLs.

**Deep dives:**
- **Thundering herd / stampede:** request coalescing (single-flight), locks on miss, early probabilistic refresh, stale-while-revalidate.
- **Hot keys:** replicate hot keys, local in-process L1 cache, key splitting.
- **Consistency:** delete-after-write races (stale value repopulated): use short TTLs, versioned values, or CDC-driven invalidation from the DB log.
- **Node failure:** consistent hashing limits remapping to 1/N of keys; warm-up.
- **Cache penetration** (keys that don't exist): cache negatives, Bloom filter.

**Trade-offs:** Freshness vs hit rate (TTL length). Redis (data structures, persistence, replication) vs Memcached (simple, multi-threaded).

**Metrics:** hit rate, latency, evictions, memory use, hot key distribution, DB load.

### File storage / sync (Dropbox/Google Drive)

**Clarify:** Max file size? Sync across devices, sharing, versioning, offline? Scale: 500M users, average 10 GB.

**Requirements:** Upload/download reliably (resumable), sync changes to all devices quickly, version history, sharing with permissions, deduplication, durability (11 nines).

**Architecture:**
```
Client (watcher, chunker, local DB of file state)
  -> Metadata service (files, folders, versions, chunks list) -> SQL (sharded by user/namespace)
  -> Block service: upload chunks (4 MB, content-hashed) via pre-signed URLs -> object storage (S3)
  -> Notification service (long poll/WebSocket): "namespace changed, cursor X"
  -> Other devices fetch metadata changes since cursor, download missing chunks
```

**Data model:** `file(id, namespace_id, path, latest_version)`, `version(file_id, version, chunk_hashes[], size, modified_by, ts)`, `chunk(hash, storage_key, refcount)`, `share(namespace, principal, role)`, `journal(namespace_id, seq, change)`.

**Deep dives:** Chunking enables resumable uploads, delta sync (only changed chunks), and dedup by hash across users. Conflicts: if two devices edit the same base version, keep both ("conflicted copy"). Journal with sequence numbers gives cursors for sync. Garbage-collect unreferenced chunks after retention.

**Trade-offs:** Fixed-size vs content-defined chunking (better for inserts). Cross-user dedup saves storage but leaks information (confirmation attacks); scope per user if sensitive.

**Metrics:** sync latency, upload success rate, storage dedup ratio, conflict rate.

### Video streaming (YouTube/Netflix)

**Clarify:** UGC uploads (YouTube) or curated catalogue (Netflix)? Live or VOD? Scale: 1B views/day; global.

**Requirements:** Upload and transcode; adaptive playback with low startup time and low rebuffering; global delivery; recommendations; DRM for premium content.

**Architecture:**
```
Upload: client -> resumable upload -> raw object store -> Kafka job
 -> Transcoding pipeline (split into segments, parallel encode to ladder:
    240p..4K, H.264/VP9/AV1) -> HLS/DASH segments + manifests -> origin storage
Playback: client -> API (metadata, auth, manifest URL, DRM license)
 -> CDN edge (Open Connect-like appliances in ISPs) -> segments
 -> ABR player picks bitrate by bandwidth/buffer
Analytics: view events -> stream -> counts, QoE, recommendations
```

**Data model:** `video(id, owner, title, status, duration)`, `rendition(video_id, codec, resolution, bitrate, manifest_url)`, view counts in a counter store, metadata in sharded SQL, search index.

**Deep dives:** Bandwidth is the cost: CDN caching with popularity-based pre-positioning; per-title encoding optimisation. Startup time: small first segments, low initial bitrate. Transcoding is embarrassingly parallel (segment-level). View counts: approximate, batched aggregation.

**Trade-offs:** More renditions/codecs = better QoE and bandwidth savings but higher storage/encode cost. Live adds latency trade-offs (LL-HLS).

**Metrics:** startup time, rebuffer ratio, average bitrate, CDN hit rate, transcode latency, cost per GB delivered.

### Ride sharing (Uber)

**Clarify:** Core flows: request ride, match driver, track trip, pricing, payments. Scale: 10M rides/day, 1M active drivers sending location every 4 s ≈ 250k location updates/s.

**Requirements:** Match rider to nearby driver within seconds; real-time location; ETA and surge pricing; trip state machine; payments; high availability.

**Architecture:**
```
Driver app -> location updates -> Location service -> geospatial index in memory
   (H3/geohash cells -> driver ids; Redis GEO or custom sharded by region)
Rider request -> Trip service -> Matching service: nearby available drivers
   -> ETA (routing engine) -> offer to best driver (timeout, next) -> trip created
Trip service (state machine: requested -> accepted -> arrived -> in_progress -> completed)
Pricing (surge by supply/demand per cell) ; Payments ; Notifications ; Kafka for events
```

**Data model:** `driver_location` (ephemeral, in-memory, TTL), `trip(id, rider, driver, state, pickup, dropoff, fare, ts)` in SQL sharded by city, `driver(status)`.

**Deep dives:** Geospatial indexing with H3 hexagons: query the rider's cell plus neighbouring rings. Shard by city/region (traffic is local). Avoid double-assigning drivers: lock/lease driver during offer (Redis SETNX with TTL) or single matcher per region. Location writes are lossy and ephemeral — don't put them in the trip DB; sample for history.

**Trade-offs:** Greedy nearest-driver vs batched matching (better global efficiency, small delay). Consistency for trip/payment state, availability for location.

**Metrics:** match time, ETA accuracy, pickup time, cancellation rate, driver utilisation.

### Food delivery (DoorDash)

**Clarify:** Three-sided: customers, restaurants, couriers. Scale: 5M orders/day, peaks at lunch/dinner. Scheduled orders? Grocery too?

**Requirements:** Browse restaurants/menus near me; place and pay; restaurant accepts and prepares; dispatch courier; live tracking; accurate ETAs; refunds/support.

**Architecture:**
```
Customer app -> Search/discovery (geo-filtered restaurants, Elasticsearch, cached menus)
 -> Cart/Order service (state machine) -> Payments (authorise, capture on delivery)
 -> Restaurant tablet/POS integration (accept, prep time)
 -> Dispatch service: batch assignment of orders to couriers every ~30 s
    (cost = ETA to restaurant vs food ready time, delivery time, batching two orders)
 -> Courier location service (geo index) ; Tracking via WebSocket ; Notifications
 -> ETA models (prep time, travel time) ; Kafka event bus
```

**Data model:** `restaurant`, `menu_item` (read-heavy, cached), `order(id, customer, restaurant, items, state, totals)`, `delivery(order_id, courier_id, pickup_eta, dropoff_eta, state)`, `courier(status, location)`.

**Deep dives:** Dispatch is an optimisation problem: courier should arrive when food is ready (minimise both courier wait and food sitting); solve as assignment problem per region every few seconds. Prep-time prediction from restaurant history. Menu availability consistency (86'd items). Order state machine with idempotent transitions and outbox events.

**Trade-offs:** Assign early (certainty) vs late (better matches). Batching orders lowers cost but slows delivery.

**Metrics:** delivery time, ETA accuracy, courier utilisation, order defect rate (late, missing items), cost per delivery.

### Payment system / ledger

**Clarify:** Merchant payments via card networks (Stripe-like), internal wallet, or both? Scale: 1k-10k TPS. Currencies? Settlement and payouts?

**Requirements:** Never lose or double-charge money; exactly-once effect; full auditability; reconciliation with external processors; strong consistency for balances; PCI compliance.

**Architecture:**
```
Client -> Payment API (idempotency key required) -> Payment service (state machine:
   created -> authorised -> captured -> settled / failed / refunded)
 -> PSP/processor adapters (card networks via Adyen/Stripe) with retries
 -> Ledger service: double-entry, append-only (Postgres, serialisable txns)
 -> Outbox -> Kafka -> notifications, analytics
 -> Reconciliation jobs: internal ledger vs PSP settlement files daily
```

**Data model:** `payment(id, idempotency_key UNIQUE, amount, currency, status)`, `ledger_entry(id, txn_id, account_id, debit/credit, amount, currency, ts)` — every transaction's entries sum to zero; balances derived (or materialised with versioned updates). Amounts as integers in minor units, never floats.

**Deep dives:** Idempotency keys stored with the response so retries return the same result. External calls are not transactional: record intent first, call PSP, record outcome; unknown outcomes (timeouts) are resolved by status queries and reconciliation, never by blindly retrying a charge. Ledger entries immutable; corrections are reversing entries. Card data tokenised; only the vault touches PANs (PCI scope reduction).

**Trade-offs:** Strong consistency (CP) over availability for the ledger. Synchronous vs async capture.

**Metrics:** payment success rate, reconciliation breaks (target 0), latency, duplicate charge incidents, PSP error rates.

### Ticket booking (Ticketmaster) with concurrency on seats

**Clarify:** Assigned seats or general admission? Peak: a big on-sale with 1M users for 50k seats in minutes. Hold time before payment? Bots?

**Requirements:** No double booking; fair access under extreme spikes; seat holds with expiry; payment; fast seat map.

**Architecture:**
```
Users -> CDN (static event pages) -> Virtual waiting room (queue tokens, admit N/min)
 -> Booking service: seat map (cached, read-heavy) -> hold seat(s)
 -> Seat inventory: Postgres with row-level locking or Redis holds with TTL
 -> Payment -> confirm booking (hold -> sold) -> ticket issuance ; expired holds released
```

**Data model:** `seat(event_id, seat_id, status: available|held|sold, hold_id, hold_expires_at, version)`, `hold(id, user, seats, expires_at)`, `booking(id, user, seats, payment_id)`.

**Deep dives:** **Concurrency:** atomic conditional update: `UPDATE seat SET status='held', hold_id=? WHERE event_id=? AND seat_id IN (...) AND status='available'` — check affected rows equals requested count, in one transaction (or optimistic locking with version). Alternative: Redis `SET seat_key hold_id NX EX 600` per seat. Holds expire via TTL and a sweeper. **Spikes:** waiting room protects the backend and gives fairness; precompute seat map snapshots; shard by event. Bot defence: CAPTCHA, verified fan, rate limits.

**Trade-offs:** Pessimistic locks (simple, contention) vs optimistic (retries under contention). Strong consistency on seats, eventual for seat map display (show "might be taken").

**Metrics:** double-booking incidents (0), checkout conversion, queue wait time, hold expiry rate, p99 hold latency.

### Hotel/flight reservation system

**Clarify:** Our own inventory or aggregating from GDS (Amadeus/Sabre) / hotel channel managers? Overbooking allowed? Scale of search vs booking (search 1000x more)?

**Requirements:** Search availability and prices fast; book without overselling (or within controlled overbooking); hold during checkout; cancellations/modifications; integration with external suppliers; payments.

**Architecture:**
```
Search: client -> Search service -> availability cache (precomputed per hotel/date/room type,
        Elasticsearch/Redis) + supplier APIs for live price check
Book: Reservation service -> inventory DB (Postgres) -> hold (TTL) -> payment -> confirm
      -> supplier booking API (for external inventory) -> confirmation number
Events -> notifications, cache invalidation, analytics
```

**Data model:** `room_inventory(hotel_id, room_type, date, total, booked, version)` — booking a 3-night stay decrements 3 rows atomically: `UPDATE ... SET booked = booked + 1 WHERE booked < total + overbook_allowance` for each date in one transaction. `reservation(id, guest, hotel, room_type, check_in, check_out, status, idempotency_key)`. Flights: `seat_inventory(flight_id, fare_class, available)`.

**Deep dives:** Search uses cached availability (stale OK); booking re-validates against source of truth. External suppliers are slow and flaky: timeouts, async confirmation, compensation (saga) if payment succeeded but supplier failed. Idempotency for retries. Overbooking is a business policy encoded as allowance.

**Trade-offs:** Cache freshness vs supplier API costs (look-to-book ratios). Strong consistency on inventory decrement.

**Metrics:** search latency, look-to-book ratio, booking failure rate, price mismatch at checkout, oversell incidents.

### Logging & metrics system

**Clarify:** Volume (e.g. 10 TB logs/day, 10M metric series)? Retention? Query patterns (search recent logs, dashboards, long-term trends)? Multi-tenant?

**Requirements:** Collect logs/metrics from thousands of hosts; low ingestion overhead; search logs within seconds of emission; dashboards; tiered retention; cost control.

**Architecture:**
```
Hosts/pods: agents (Fluent Bit/Vector, OpenTelemetry Collector, Prometheus exporters)
 -> Buffer: Kafka (absorbs spikes, decouples)
 -> Logs: indexers -> hot store (Elasticsearch/OpenSearch or Loki/ClickHouse) -> cold S3 (Parquet)
 -> Metrics: Prometheus (scrape) -> remote write to long-term TSDB (Mimir/Thanos/VictoriaMetrics)
 -> Query: Grafana/Kibana ; alerting rules
```

**Data model:** logs: structured JSON (`ts, service, level, trace_id, message, attrs`), partitioned by time and service. Metrics: series = name + labels, samples (ts, value), compressed (Gorilla encoding).

**Deep dives:** Cost is the main issue: sample debug logs, drop noisy logs at the agent, index only selected fields (Loki indexes labels only), compress cold data in object storage. Metric cardinality explosion (user_id as label) kills TSDBs: enforce label limits. Backpressure: agents buffer to disk; Kafka retains during indexer outages. Correlate logs/metrics/traces with trace IDs.

**Trade-offs:** Full-text indexing (fast search, expensive) vs label-only (cheap, grep-like scans). Retention vs cost.

**Metrics:** ingestion lag, dropped events, query latency, storage cost per GB, cardinality per tenant.

### Job scheduler / distributed cron

**Clarify:** Number of jobs (1M schedules?) and frequency (per minute)? Run duration? Exactly-once execution needed? Dependencies (DAGs)? Retries?

**Requirements:** Schedule cron/one-off jobs; trigger on time (within seconds); each run executes once (at least once + idempotency); retries; no lost jobs on node failure; history and monitoring.

**Architecture:**
```
API -> Job store (Postgres: job definitions, next_run_at indexed)
Scheduler nodes (partitioned by job_id hash, or leader-elected):
  poll due jobs: SELECT ... WHERE next_run_at <= now() FOR UPDATE SKIP LOCKED LIMIT 1000
  -> create run record (job_id, scheduled_time UNIQUE) -> enqueue to work queue (Kafka/SQS)
  -> compute next_run_at
Workers -> execute (lease with heartbeat) -> update run status ; retry with backoff ; DLQ
```

**Data model:** `job(id, cron_expr, timezone, payload, next_run_at, owner, retry_policy)`, `run(job_id, scheduled_time, status, attempt, worker, started_at, finished_at)` with UNIQUE(job_id, scheduled_time).

**Deep dives:** Unique constraint on (job, scheduled_time) prevents duplicate runs even if two schedulers race. Worker leases with heartbeats: if a worker dies, the lease expires and the run is retried — so jobs must be idempotent. Timezones and DST. Missed runs after outage: policy per job (catch up vs skip). Time wheel/in-memory timers for sub-second precision at huge scale.

**Trade-offs:** DB polling is simple and fine to ~10k jobs/s with partitioning; beyond that, sharded time-wheels. Exactly-once is impossible in general; aim for at-least-once + idempotent jobs. Alternatives: Temporal, Airflow (DAGs), Kubernetes CronJobs.

**Metrics:** schedule lag (actual start - scheduled), failure rate, queue depth, duplicate runs.

### Message queue (Kafka-like)

**Clarify:** Throughput (1M msgs/s)? Retention (days)? Ordering? Delivery semantics? Consumer groups and replay?

**Requirements:** Durable, ordered within partition, horizontally scalable, multiple independent consumer groups, replay from offset, high availability.

**Design:**
- **Topics split into partitions**; each partition is an append-only log of segment files on disk. Ordering only within a partition; producers choose partition by key hash.
- **Brokers** host partitions; each partition has a leader and followers (replication factor 3). In-sync replica (ISR) set; `acks=all` + `min.insync.replicas=2` for durability.
- **Metadata/controller:** consensus (Raft, like KRaft) for partition leadership and membership.
- **Consumers** pull in batches and track their own offsets per partition (committed to an internal topic). Consumer groups: each partition consumed by one member; rebalance on join/leave.
- **Performance:** sequential disk I/O, OS page cache, zero-copy sendfile, batching, compression.
- **Retention:** time/size-based deletion of old segments, or log compaction (keep latest per key).

**Delivery semantics:** at-most-once (commit before process), at-least-once (process then commit; default), exactly-once (idempotent producer + transactions, within the system).

**Trade-offs:** More partitions = more parallelism but more overhead and rebalancing cost. Pull model gives consumer control; push gives lower latency.

**Metrics:** consumer lag, under-replicated partitions, produce/fetch latency, broker disk usage, ISR shrinks.

### Collaborative document editor (Google Docs)

**Clarify:** Concurrent editors per doc (up to ~100)? Rich text? Offline editing? Comments, history, permissions?

**Requirements:** Real-time co-editing (changes visible in < 200 ms), convergence (all users see same doc), no lost edits, offline support, version history, presence/cursors.

**Architecture:**
```
Clients (local doc model, apply ops optimistically)
 <-> WebSocket -> Collaboration service (one authoritative session per doc, sticky routing)
      -> transform/merge ops, assign revision, broadcast
      -> Op log (append-only, per doc) + periodic snapshots -> DB/object storage
 Presence via pub/sub ; Comments/permissions via regular services
```

**Data model:** `document(id, owner, acl)`, `op(doc_id, revision, user, op, ts)`, `snapshot(doc_id, revision, content)`.

**Deep dives:** **OT (Operational Transformation):** central server orders ops and transforms concurrent ops against each other (Google Docs). Needs a central authority per doc. **CRDTs** (Yjs, Automerge): each char has a unique ID; merges are commutative, works peer-to-peer and offline; more metadata overhead. Either way: client sends ops with base revision; server transforms/merges and broadcasts. Snapshots every N ops for fast loading; history by replaying ops.

**Trade-offs:** OT is simpler with a server and compact; CRDT is better for offline/P2P. One session server per doc limits scale per doc but simplifies ordering.

**Metrics:** op round-trip latency, conflict/transform errors, doc load time, divergence incidents.

### Leaderboard

**Clarify:** Global, per game, per friends? Real-time? Number of players (100M) and score updates/s? Time-windowed (daily/weekly)?

**Requirements:** Update scores, get top-N, get a user's rank and neighbours, with low latency.

**Architecture:**
```
Game servers -> Score service (validate, anti-cheat) -> Redis sorted sets
   ZADD leaderboard:{game}:{period} score user_id ; ZREVRANGE for top N ; ZREVRANK for rank
 -> durable store (DB) for scores history ; Kafka for events
```

**Deep dives:** Redis sorted set gives O(log n) updates and rank queries; one set handles tens of millions of members. Beyond that: shard by score ranges (rank = count in higher shards + rank within shard) or approximate ranks for the long tail (percentile buckets), exact for top. Ties: composite score (score, then earlier timestamp) encoded into the float. Windowed boards: separate keys per period with TTL. Friends leaderboard: fetch friend scores and sort in app (small). Persist to DB to rebuild Redis on failure.

**Trade-offs:** Exact global rank for everyone is expensive at huge scale; users rarely need exact rank #3,451,229.

**Metrics:** update latency, query latency, Redis memory, cheating flags.

### Metrics/alerting system

**Clarify:** Metrics volume (series, samples/s)? Alert rules count? Notification channels? Multi-tenant? Overlap with logging question — focus on alerting.

**Requirements:** Ingest time series, evaluate alert rules continuously, notify the right people quickly with low noise, silence/inhibit, escalation, HA (alerting must work when other things fail).

**Architecture:**
```
Exporters/agents -> Prometheus/collectors -> TSDB (long-term: Mimir/VictoriaMetrics)
Rule evaluator (every 15-60 s): PromQL rules -> pending -> firing after "for" duration
 -> Alertmanager (HA pair, gossip): dedupe, group, inhibit, silence, route by labels
 -> PagerDuty/Opsgenie/Slack/email ; escalation policies ; status page
```

**Data model:** time series (name, labels, samples); `alert_rule(expr, for, severity, labels, runbook_url)`; `alert(fingerprint, state, starts_at, ends_at)`; routing tree config.

**Deep dives:** Alert on symptoms (SLO burn rate, error rate, latency) rather than causes (CPU high). Multi-window burn-rate alerts reduce noise. Grouping avoids 500 pages for one outage. Meta-monitoring: a separate system checks that the alerting pipeline itself works (dead man's switch alert). Every alert has a runbook and an owner.

**Trade-offs:** Sensitivity vs alert fatigue. Pull (Prometheus scrape) vs push (OTLP, StatsD) collection.

**Metrics:** time to detect, pages per on-call shift, % actionable alerts, false positive rate, alert evaluation lag.

### API gateway

**Clarify:** External, internal, or both? RPS (e.g. 100k)? Protocols (REST, gRPC, WebSocket)? Auth types? Multi-region?

**Requirements:** Single entry point; routing to services; authn (OAuth/JWT, API keys); rate limiting; request validation; TLS termination; observability; low added latency (< 5-10 ms); HA.

**Architecture:**
```
Client -> DNS/Anycast -> L4 LB -> Gateway fleet (stateless; Envoy/Kong/NGINX/AWS API GW)
  plugin chain: TLS -> auth (JWT verify with cached JWKS / key lookup) -> rate limit (Redis)
   -> request validation -> routing (path/host/header, canary weights) -> retries/timeouts/
   circuit breakers -> upstream service (service discovery) -> response transforms
  -> logs/metrics/traces ; config from control plane (versioned, hot reload)
```

**Deep dives:** Keep the gateway thin: cross-cutting concerns only, no business logic. JWT verification locally (no auth service call per request). Config via control plane with validation and gradual rollout (bad config can take everything down). Timeouts and circuit breakers protect upstreams. Canary routing for deployments. Backend-for-frontend layers if client-specific aggregation is needed.

**Trade-offs:** Centralisation (consistency, single point of failure) vs sidecar/service mesh. Managed vs self-hosted.

**Metrics:** added latency p99, 4xx/5xx per route, auth failures, rate-limit rejections, upstream health.

## D. Follow-up questions to expect

### What are the functional and non-functional requirements?

- **Functional requirements** = what the system does: the user-visible features and flows. Example (claims pipeline): ingest documents, extract fields, route low-confidence items to review, push results to the claims system.
- **Non-functional requirements** = how well it does it: latency (p99 < 200 ms), throughput (50k docs/day), availability (99.9%), durability (no data loss), consistency, security and compliance (HIPAA, audit), scalability, cost ceiling, operability, data residency, maintainability by the customer's team.

How to answer in an interview: list 3-5 functional requirements, explicitly say what is **out of scope**, then pick the 3-4 non-functional requirements that actually drive the design and give them numbers. For FDE contexts, always add the customer constraints: deployment environment (on-prem, cloud), existing systems to integrate with, team that will own it, and deadline. Write them on the board; refer back to them when making trade-offs.

### Estimate scale: users, QPS, storage, bandwidth.

Use round numbers and show the steps:
- **Users → QPS:** 10M DAU x 20 requests/day = 200M/day ÷ ~100k s/day ≈ 2k QPS average; peak ≈ 2-5x → ~10k QPS. Split read vs write (often 10:1 to 100:1).
- **Storage:** writes/day x size x retention. 1M new records/day x 2 KB = 2 GB/day ≈ 730 GB/year; x3 replication ≈ 2.2 TB/year. Add indexes (~30-50%).
- **Bandwidth:** QPS x response size: 10k QPS x 50 KB = 500 MB/s egress (CDN territory).
- **Memory for cache:** cache the hot 20% of daily reads.
- **LLM systems:** also estimate tokens: requests x tokens per request → cost per day and rate-limit headroom (TPM).

Useful anchors: 1 day ≈ 86,400 s; one Postgres node handles ~5-10k simple QPS; Redis ~100k ops/s per node. State conclusions: "so a single Postgres with read replicas is fine" — the point of estimation is to choose the design, not the arithmetic itself.

### What does the data model look like? SQL or NoSQL? Why?

Start from entities and access patterns: list the main entities, relationships, and the top 3-5 queries with their frequency.

- **Choose SQL (Postgres) when:** relational data with joins, transactions across entities (payments, inventory, bookings), evolving query patterns, strong consistency, moderate scale (a well-tuned Postgres handles TBs and thousands of QPS; scale with read replicas, partitioning, then sharding like Citus). It is the right default for most FDE projects.
- **Choose NoSQL when:** access is simple key-value or key-range at very high scale (DynamoDB, Cassandra for messages/time series), flexible documents (MongoDB), heavy write throughput with predictable partition keys, or specialised needs: time series (Timescale/Influx), graph (Neo4j) for relationship traversal, search (OpenSearch), vectors.
- Polyglot is normal: Postgres as system of record + Redis cache + search index + warehouse for analytics, synced via CDC.

Always mention the partition/shard key and how the main queries hit indexes.

### Where are the bottlenecks? How do you scale reads vs. writes?

**Finding bottlenecks:** walk the request path and ask where the limit is: database (connections, locks, slow queries, hot rows), single-threaded services, external APIs with rate limits (very common in customer integrations and LLM providers), network/egress, and fan-out amplification.

**Scaling reads:**
- Caching (CDN, Redis, application caches), with sensible TTLs and invalidation.
- Read replicas (accept replication lag for non-critical reads).
- Denormalised/materialised views and search indexes for complex queries.
- Precomputation (feeds, aggregates).

**Scaling writes:**
- Batching and async processing through queues (absorb spikes).
- Partitioning/sharding by a high-cardinality key (tenant, user); avoid hot partitions.
- Write-optimised stores (LSM: Cassandra), append-only logs instead of in-place updates.
- Reduce contention: avoid single hot counters (sharded counters), optimistic concurrency.

Stateless app tiers scale horizontally behind a load balancer; state is the hard part. Profile before optimising.

### How do you handle failures, retries, idempotency?

- **Assume everything fails:** networks, dependencies, nodes, and your own deploys.
- **Timeouts** on every remote call; budgets shorter than the caller's.
- **Retries** only for transient errors (5xx, 429, timeouts), with exponential backoff + jitter and a maximum; never retry non-idempotent operations blindly.
- **Idempotency:** client-supplied idempotency keys stored with the result (payments, webhooks), upserts with natural keys, unique constraints (job_id, scheduled_time), dedup by event ID in consumers. This makes at-least-once delivery safe.
- **Circuit breakers and bulkheads** to stop cascading failures; fallbacks (cached data, degraded mode).
- **Queues + DLQs** for async work, with replay tools.
- **Transactional outbox** to atomically update DB and publish events; **sagas** with compensating actions for multi-service workflows.
- **Unknown outcomes** (timeout after sending a charge): query status or reconcile, don't guess.
- Test it: chaos tests, failure injection, game days.

### Consistency vs. availability trade-off for this system?

CAP: during a network partition you choose consistency or availability; PACELC adds that even without partitions you trade latency for consistency.

How to answer: classify each piece of data by the cost of being wrong vs. unavailable.
- **Choose consistency (CP):** money, ledgers, inventory and seat booking, permissions/access revocation, unique usernames. Use single-leader databases, transactions, quorum/consensus (Raft). Users see an error rather than a wrong answer.
- **Choose availability (AP):** feeds, likes counts, recommendations, presence, dashboards, location updates, offline-first field apps. Use eventual consistency, show staleness ("updated 2 min ago"), and resolve conflicts (LWW, CRDTs, merge).

Most real systems are mixed: e.g. a booking system is strongly consistent on the seat decrement but eventually consistent on the seat-map display. Mention read-your-writes as a cheap, user-friendly middle ground.

### How do you secure it (authn, authz, encryption, PII)?

- **Authn:** SSO via the customer's IdP (SAML/OIDC), MFA; service-to-service via mTLS or workload identity; short-lived tokens; no shared passwords; secrets in a vault (AWS Secrets Manager, HashiCorp Vault).
- **Authz:** least privilege; RBAC for capabilities plus ABAC/row-level security for data; enforce at the data layer as well as the API; tenant isolation tested explicitly.
- **Encryption:** TLS 1.2+ in transit; at rest with KMS, customer-managed keys (BYOK) for enterprise; field-level encryption for highly sensitive fields.
- **PII:** data minimisation, classification and tagging, masking/tokenisation, pseudonymisation in analytics, retention and deletion (GDPR erasure), redaction before logs and LLM prompts.
- **Network:** private subnets, no public DBs, WAF, egress controls.
- **Audit and detection:** immutable audit logs, SIEM integration, anomaly alerts.
- **Supply chain/app sec:** dependency scanning, SBOM, SAST, pen tests. For LLM apps add prompt-injection defences and tool permission limits.

### How do you monitor it? What alerts would you set?

**Instrument:** the four golden signals (latency, traffic, errors, saturation) per service; RED for request paths, USE for resources; structured logs with request/trace IDs; distributed tracing (OpenTelemetry); business metrics (orders processed, documents extracted, sync lag).

**Define SLOs** with the customer, e.g. 99.9% of API requests succeed in < 500 ms; data freshness < 15 min.

**Alerts (page a human only when action is needed):**
- SLO burn-rate alerts (fast and slow windows) on errors and latency.
- Pipeline freshness/lag over threshold; queue depth or consumer lag growing; DLQ non-empty.
- Saturation: DB connections, disk > 80%, replication slot size.
- External dependency failures (ERP API errors, LLM provider 429s).
- Data quality failures (row counts drop 50%, null spikes).
- Security: auth failures spike, unusual access.

Tickets (not pages) for slow-burn issues. Every alert has an owner and a runbook. Dashboards: one overview for the customer, detailed ones for engineers.

### How would you deploy it at a customer with no cloud / strict compliance (HIPAA, FedRAMP, GDPR, SOC 2)?

**No cloud / on-prem:** package as containers on Kubernetes (OpenShift, RKE2) or VMs; Helm charts or Replicated/KOTS installer; replace managed services with self-hosted equivalents (Postgres, MinIO, Kafka/Redpanda, Keycloak); offline bundles with signed images and SBOMs; support bundles; documented upgrade/rollback.

**Compliance specifics:**
- **HIPAA:** BAA with every vendor touching PHI, encryption, access controls, audit logs, minimum necessary use.
- **FedRAMP:** deploy in authorised environments (AWS GovCloud, Azure Government) using FedRAMP-authorised services; NIST 800-53 controls; FIPS 140-validated crypto; continuous monitoring; US persons for operations.
- **GDPR:** lawful basis, data residency in EU regions, DPAs, data subject rights (access, erasure), DPIAs, minimisation.
- **SOC 2:** evidence of controls: change management, access reviews, logging, incident response, vendor management.

Practical: engage their security/compliance team in week 1, map controls to your architecture, reuse your existing compliance documentation, and design so data stays in their boundary.

### What would you build in week 1 vs. month 3?

**Week 1 (prove value and de-risk):**
- Access to real data and systems (often the slowest part: credentials, VPN, approvals).
- Thin end-to-end slice: one source → minimal processing → one output a real user can see, even if ugly.
- Baseline metrics and a small eval/test set; document assumptions and risks.
- Simple tech: managed services, a single Postgres, scripts run by a scheduler.

**Month 1:** harden the core path: error handling, retries, idempotency, monitoring, auth/SSO, the top 80% of cases, user feedback loop.

**Month 3 (production and scale):**
- Additional sources/use cases, automation of manual steps.
- Reliability: SLOs, alerting, runbooks, DR/backups, load testing.
- Security/compliance sign-off, audit logs, access reviews.
- Performance and cost optimisation.
- Handover: docs, training, customer team owns operations.

Principle: get to a working thing in front of users fast, then iterate on what they actually use.

### How would you explain this design to the customer's CTO? To an end user?

**CTO:** focus on outcomes, risk, and fit with their environment.
- One-sentence purpose and the business metric it moves.
- A simple box diagram: their systems, our components, data flows, where data lives.
- Key decisions and trade-offs ("we chose CDC over nightly batch so dashboards are 5 minutes fresh; cost is one more service to run").
- Security, compliance, and integration with their identity and network.
- Operations: who runs it, SLOs, cost, scaling path, exit/lock-in story.
- Risks and mitigations; timeline and asks of their team.

**End user:** no architecture. Explain what changes in their day: "When a claim comes in, the fields are already filled in; you only check the highlighted ones and click approve." Cover what it does, what it doesn't do, how to correct it, and who to contact. Demo with their own data. Avoid jargon in both; adapt depth to questions.

### What would you cut if you had half the time?

Cut scope, not quality on the critical path.

- **Keep:** the single most valuable end-to-end workflow, data correctness, security basics (auth, encryption, access control), and enough monitoring to know when it breaks.
- **Cut or defer:** secondary use cases and sources, real-time (use batch/hourly instead), custom UI (use an existing BI tool, spreadsheet export, or a simple admin interface), ML (use rules/heuristics first), multi-region and auto-scaling (single region, manual scaling), self-service configuration (hardcode for the first customer), polish and edge-case automation (route to manual handling).
- **Buy instead of build:** managed services, iPaaS connectors, hosted vector DB.

Say explicitly what technical debt you accept and the plan to pay it down, and confirm the trade with the customer: "We can deliver X by the date; Y moves to phase 2."

### How do you handle schema changes in upstream data sources?

**Prevent:** data contracts with source owners (schema, semantics, SLA, change notice); schema registry (Avro/Protobuf with compatibility rules) for event streams.

**Detect:** compare incoming schema against the expected one on every run; data quality tests (dbt tests, Great Expectations) on types, nulls, value ranges, row counts; alert on drift.

**Absorb:**
- Land raw data as-is (bronze layer, or JSON column) so nothing is lost even if transformations break.
- Auto-handle additive changes: new nullable columns propagate automatically.
- Breaking changes (rename, type change, removed column): quarantine affected records, alert, and fix the mapping; do not silently write nulls.
- Version transformations; mappings in config rather than code.
- Use explicit column selection downstream (never `SELECT *` into critical models).

**Recover:** after fixing, replay from raw to backfill the affected period. For APIs, pin API versions (Salesforce API version, Stripe version headers).

### How do you backfill historical data?

- **Design for it upfront:** idempotent, deterministic pipelines parameterised by time range (`run(start, end)`), partitioned outputs that can be overwritten, and raw data retained.
- **Plan:** size the data, estimate time and cost, check source system limits (API quotas, DB load); agree a window with the source owner.
- **Execute:**
  - Chunk by partition (day/month), process in parallel with concurrency limits, track progress in a state table so it can resume.
  - Use bulk interfaces (Salesforce Bulk API, DB snapshots/exports, read replicas) instead of the incremental path.
  - Write to a staging location and swap partitions atomically, or use MERGE/upsert so reruns are safe.
  - Run backfill separately from the live incremental pipeline, then reconcile the overlap using watermarks.
- **Validate:** row counts, checksums, sampled comparisons with the source, downstream metric sanity checks.
- **Point-in-time correctness** for ML features: use history as it was then, not current values.

Orchestrators like Airflow/Dagster support backfills natively over partitioned jobs.

### What happens when a downstream dependency is down?

First, classify the dependency: is it on the critical path for the user, or can work be deferred?

- **Detect fast:** timeouts, health checks, circuit breakers that open after repeated failures and stop sending traffic.
- **Degrade gracefully:** serve cached/stale data with an indicator, disable the feature (feature flags), use a fallback (secondary provider, simpler model, rules engine instead of ML scorer).
- **Buffer and retry:** for async writes, queue the work (Kafka/SQS) and drain later with backoff; DLQ after max attempts; make processing idempotent so replays are safe.
- **Protect yourself:** bulkheads (separate thread/connection pools per dependency) so one slow dependency does not exhaust shared resources; load shedding.
- **Communicate:** clear user-facing messages, status page, alerts with runbooks.
- **Recover:** drain backlog at a controlled rate so you don't overwhelm the dependency when it returns (thundering herd); reconcile.

Decide fail-open vs fail-closed per case: fraud scoring may fail open with rules; authorisation must fail closed.

### Cost estimate: what's expensive, and how do you reduce it?

**Estimate:** list major cost drivers with unit prices x volume: compute (instances/GPU hours), storage (GB-month x replication x retention), data transfer (egress, cross-AZ), managed services (warehouse credits, vector DB), LLM tokens (requests x tokens x price), third-party APIs and licences, and people/operations time.

**Usually expensive:** LLM/GPU inference, warehouse compute from unoptimised queries, egress and cross-region traffic, logging/observability volume, over-provisioned always-on clusters, high-cardinality metrics.

**Reduce:**
- LLMs: smaller models via routing/cascades, prompt caching, batch APIs, semantic caching, shorter prompts and outputs, distillation.
- Compute: autoscaling, spot instances for batch, right-sizing, scale to zero for dev.
- Storage: lifecycle tiers (hot → S3 IA → Glacier), compression (Parquet), retention policies.
- Warehouse: partitioning/clustering, incremental models, auto-suspend, query cost monitoring.
- Network: keep data and compute co-located, CDN.
- Observability: sampling, drop debug logs, limit cardinality.

Track cost per unit of value (per document, per query, per tenant) and put budgets and alerts on it.
