# 04 — Decomposition: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers, not scripts. In the real round you will be talking with the interviewer, so ask the clarifying questions out loud and let their answers change your plan. Adapt the examples to domains you know, and practise saying each one in 10–15 minutes.

Every prompt in A–E uses the same seven steps, so you can follow the structure at a glance:

1. **Clarify**: who the users are, what decision they make, what success looks like, and what exists today.
2. **Scope / first slice**: the narrowest slice that is still valuable, with assumptions stated.
3. **MECE breakdown**: sub-problems that do not overlap and that together cover the problem.
4. **Data & quality**: real sources, how they join, and how messy they are.
5. **Solution v0 / v1 / v2**: simplest useful thing first.
6. **Risks & ethics**
7. **Metrics & iteration**

A habit that works in every prompt: **say what decision you are improving before you mention any model.** Most v0s are a joined dataset plus a ranked list or map that someone already uses every day. Models come in v1, once there is trust and a feedback loop.

---

## A. Public sector / government / defense

### A city wants to reduce 911 response times. You have call data, traffic data, and ambulance GPS. What do you do?

**Clarify**
- Users: the EMS dispatcher (assigns units in real time), the EMS operations chief (plans posting locations and shifts), and the city budget office.
- Decision: which unit to send to a call now, and where idle units should wait.
- Success: 90th-percentile response time (call received to unit on scene), not the mean, broken down by priority (cardiac arrest vs non-urgent) and by neighbourhood. Equity matters: an average that hides a slow district is a failure.
- Ask: is the problem in call-taking, in dispatch, or in travel? I would not assume travel.

**Scope.** First, break response time into its segments and find the largest one. Then improve unit posting (where ambulances wait) for life-threatening calls in one district.

**MECE breakdown.** Response time = (1) call answer time + (2) call processing / triage + (3) dispatch-to-wheels-rolling (turnout) + (4) travel time. Then add (5) unit availability: is the closest unit busy, or stuck at the hospital waiting for handover?

**Data & quality**
- CAD (computer-aided dispatch) records: timestamps for each stage, priority, address. Timestamps are often keyed in by hand, and "at scene" is pressed late or never.
- Ambulance AVL/GPS pings every 5–30 seconds. Expect gaps in tunnels and clock drift against CAD.
- Traffic: Google/HERE/INRIX speeds or city loop detectors.
- Hospital handover times, often in a separate system.
- Join on unit ID + time window. Geocode addresses, and expect 5–10% to fail (rural addresses, apartment complexes).

**Solution**
- **v0 (week 1–2):** a response-time waterfall dashboard by segment, priority and district. This often shows that a non-travel segment dominates, e.g. 40-minute hospital "wall time" means units are not available.
- **v1:** travel-time isochrones from GPS history by time of day, plus a coverage map showing which areas are more than 8 minutes from any available unit right now. Add a move-up recommendation ("reposition Medic 12 to Station 4").
- **v2:** demand forecasting by hex cell and hour, with dynamic posting optimisation (maximal covering location model). Use GPS-based ETA (actual drive times) instead of straight-line distance.

**Risks & ethics.** Optimising the average can shift coverage away from low-call-volume poor areas, so add equity constraints. Dispatcher alert fatigue. Repositioning wears out crews, so cap moves per shift. Never automate the dispatch decision for high-acuity calls; the dispatcher keeps it.

**Metrics & iteration.** P90 response time by priority and district; percentage of the city covered within 8 minutes; move-ups accepted vs rejected and why. Pilot in one district against a comparable control and review weekly with dispatch supervisors.

### Help a police department decide where to deploy officers.

**Clarify**
- Users: patrol shift commanders (daily beat assignments) and the chief / city council (staffing levels).
- Decision: how many officers per beat per shift, and what proactive vs reactive mix.
- Success: this has to be defined with the community, not only the department. Options are response time to priority-1 calls, violent crime victimisation, officer workload balance, and community trust (survey).

**Scope.** Start with workload-based beat staffing (calls for service volume and time spent), not crime prediction. It is less controversial and directly useful.

**MECE breakdown.** (1) Demand: calls for service by place, time and type. (2) Supply: officers available after leave, court and training. (3) Allocation: beats and shift schedules. (4) Proactive strategy: hot-spot policing, problem-oriented work. (5) Outcome and fairness monitoring.

**Data & quality**
- CAD calls for service: less biased than arrests because citizens start them, though still uneven.
- RMS incident reports, arrests and stops: these reflect where police already patrol, which creates a feedback loop.
- Staffing and rostering system; GIS beat boundaries; 311 data; victimisation surveys.
- Quality issues: call type recoded after arrival, duplicate calls, time spent on scene missing.

**Solution**
- **v0:** a workload heatmap by beat × hour (call volume × average time consumed), compared with current staffing. Find beats with 90% utilisation, where officers cannot respond quickly.
- **v1:** a staffing model (queueing model, e.g. Erlang-C style) that recommends officers per beat/shift to hit a response-time target with 30–40% uncommitted time.
- **v2:** combine with non-police alternatives: route mental-health calls to crisis teams and quantify the officer hours freed.

**Risks & ethics.** Predictive policing on arrest data reinforces over-policing of minority neighbourhoods. Avoid arrest/stop data as a target and avoid person-level prediction. Publish the method; audit the impact on stops by demographic; keep the civilian oversight board involved. Explainability: commanders must be able to say why a beat got more officers.

**Metrics & iteration.** P1 response time, utilisation balance across beats, crisis-team diversion rate, stop rates by demographic (should not rise), community survey. Review quarterly with the oversight body.

### Help a government agency detect benefits fraud.

**Clarify**
- Users: fraud investigators (limited caseload), eligibility caseworkers, the policy team.
- Decision: which cases to investigate or pause, and which process holes to close.
- Distinguish fraud (intentional), error (claimant or agency mistake) and organised rings (identity theft at scale). They need different responses.
- Success: recovered or prevented dollars per investigator hour, with **false positives that harm legitimate claimants kept very low**.

**Scope.** Start with organised identity fraud in one programme (e.g. unemployment insurance), where rings leave strong signals: shared bank accounts, devices and addresses. Individual-level scoring of poor claimants comes later, if ever.

**MECE breakdown.** (1) Identity: is this a real person? (2) Eligibility: are they entitled (income, employment, residency)? (3) Duplication across programmes and states. (4) Organised networks. (5) Investigation workflow and outcomes.

**Data & quality**
- Application records: name, SSN, DOB, address, bank routing, email, phone.
- Login logs: IP address, device fingerprint.
- Wage records (state UI wage files), the death master file, prison records, the National Directory of New Hires, cross-state matching (e.g. NASWA Integrity Data Hub).
- Past investigation outcomes. Labels are scarce and biased towards what was already investigated.
- Entity resolution is the main problem: misspelt names, reused emails like john.doe+1@.

**Solution**
- **v0:** rule-based checks plus link analysis. Flag more than 5 claims to one bank account or address, dead or incarcerated identities, and claims from datacentre IPs. Show investigators a graph view of the linked cluster.
- **v1:** entity resolution (e.g. Splink or Senzing) to build a claimant graph; network features and an anomaly score; rank clusters (not individuals) for review.
- **v2:** supervised model on investigation outcomes, with feedback from case dispositions; identity proofing (ID.me / Login.gov) at intake for high-risk scores.

**Risks & ethics.** Robodebt (Australia) and Michigan MiDAS (about 93% false fraud findings) are the cautionary tales. **No automated denial**: a human decides and the claimant has due process and notice. Test for disparate impact by race, language and disability. Keep the data use within its legal authority (data-sharing agreements, Privacy Act).

**Metrics & iteration.** Precision of investigated flags (confirmed fraud rate), dollars prevented, time to pay legitimate claims (must not rise), appeals overturned. Retrain on new dispositions monthly, because fraudsters adapt.

### Help the military track and maintain its vehicle/aircraft fleet.

**Clarify**
- Users: the maintenance officer / crew chief (daily work orders), the logistics / supply officer (parts), the commander (how many aircraft are mission-capable tomorrow).
- Decision: which aircraft fly, which get maintenance first, and which parts to order or move.
- Success: **mission-capable (MC) rate** and aircraft available for the planned flying schedule; fewer "not mission capable – supply" (NMCS) days.

**Scope.** One airframe type at one wing or base. Build a single source of truth for each tail number's status and its parts blockers.

**MECE breakdown.** (1) Asset status: what is up or down and why. (2) Maintenance demand: scheduled inspections plus unscheduled failures. (3) Parts supply: inventory, lead time, cannibalisation. (4) Labour: qualified maintainers and shifts. (5) Planning: the flying schedule against availability.

**Data & quality**
- Maintenance information systems (e.g. GCSS-Army, IMDS / REMIS for the USAF, NALCOMIS for the Navy): work orders and fault codes, often free text with inconsistent codes.
- Supply systems (ERP such as SAP-based GCSS, DLA requisitions).
- Flight hours and sensor / health and usage monitoring (HUMS) data.
- Data is on classified or air-gapped networks with different enclaves; joins are on tail number / serial number and NSN (part number).

**Solution**
- **v0:** a fleet status board: each tail, status, open work orders, and parts awaited with ETA. This replaces the whiteboard and the spreadsheet emailed around every morning.
- **v1:** parts prediction. Forecast demand for high-failure NSNs from flight hours and failure history, and recommend pre-positioning or lateral transfer from other bases. Track cannibalisation.
- **v2:** predictive maintenance from HUMS/sensor data on specific components (e.g. engines), and schedule optimisation that balances the inspection calendar against flying-hour plans.

**Risks & ethics.** Safety: predictions must never defer mandated inspections. Classification and air-gapped deployment (no cloud LLM; on-prem). Data accuracy depends on maintainers' data entry, so make the tool save them time or entry quality will not improve.

**Metrics & iteration.** MC rate, NMCS days, average time awaiting parts, forecast accuracy for top-50 parts, maintainer time spent on status reporting. Iterate with the morning maintenance meeting as the feedback loop.

### Help coordinate disaster response after a hurricane/earthquake (shelters, supplies, rescue).

**Clarify**
- Users: the emergency operations centre (EOC) incident commander, logistics section chief, shelter managers, search-and-rescue team leads, and NGOs (Red Cross).
- Decisions: where to send rescue teams, which shelters to open, and where supplies go.
- Success: lives saved / time to rescue, unmet needs closed, and supply delivered where needed (not piled at the wrong depot).
- Phases matter: the first 72 hours (rescue) are a different problem from weeks 1–4 (shelter and supply).

**Scope.** A common operating picture: one shared map of needs and resources, updated by field teams. Coordination failure is usually the bottleneck, not optimisation.

**MECE breakdown.** (1) Situational awareness: damage, needs, hazards. (2) Rescue tasking. (3) Shelter capacity and population. (4) Supply logistics: inventory, transport, last mile. (5) Communication with the public and agencies.

**Data & quality**
- 911 / 311 calls; social media (noisy); FEMA damage assessments.
- Satellite / aerial imagery (Maxar, NOAA), with building-damage models (e.g. the xView2 dataset).
- Census population and vulnerable-population lists (dialysis, oxygen users from utility and HHS emPOWER data).
- Shelter rosters; warehouse inventory; road closures (DOT, Waze); power outage maps.
- Quality: very low early on, duplicated, and stale within hours. Every record needs a timestamp and source.

**Solution**
- **v0:** a shared map (ArcGIS or a Foundry-style app) with layers for requests, shelters (capacity and occupancy), road status and supplies. A simple mobile form so field teams update status offline and sync later.
- **v1:** request deduplication and triage (priority by medical vulnerability, wait time); rescue team assignment suggestions; supply allocation by shelter population × days.
- **v2:** imagery-based damage estimates to prioritise areas with no calls (no calls can mean no phones); routing that respects road status.

**Risks & ethics.** Connectivity is down, so design offline-first with sync. Data about vulnerable people needs strict access controls. Inequity: affluent areas generate more online requests than poor ones. Information overload for commanders.

**Metrics & iteration.** Time from request to assignment and resolution, open-request backlog, shelter overflow events, supply stock-outs. Run a hot-wash each operational period (12 hours) and change the tool overnight.

### Help a city reduce homelessness using data from shelters, hospitals and police.

**Clarify**
- Users: the Continuum of Care (CoC) coordinator, outreach workers, housing case managers, and the city's homelessness office.
- Decision: who gets scarce housing resources first (coordinated entry prioritisation), and where outreach goes.
- Success: fewer people entering homelessness, shorter time homeless, fewer returns to homelessness, more housing placements. Shelter-bed counts alone are not success.

**Scope.** Focus on the high-utiliser population that cycles through ER, jail and shelter. It is small, costly, and well served by permanent supportive housing (a "frequent users" initiative). The first slice is a shared, consented by-name list.

**MECE breakdown.** (1) Inflow: prevention of people at risk. (2) Stock: who is homeless now and their needs. (3) Outflow: housing placement and the match process. (4) Returns: retention after housing. (5) System capacity: beds, vouchers, case managers.

**Data & quality**
- HMIS (Homeless Management Information System): shelter stays, assessments (e.g. VI-SPDAT).
- Hospital ED visits (HIPAA); jail bookings (police); 211 calls; eviction filings (court records); point-in-time count.
- No shared ID across systems, aliases, missing DOB. Use privacy-preserving record linkage (hashed identifiers, linked by a trusted third party).
- Legal: HIPAA, 42 CFR Part 2 (substance-use records), and consent requirements limit sharing.

**Solution**
- **v0:** a by-name list dashboard from HMIS (inflow, outflow, time to placement), plus a linked count of high utilisers across ER, jail and shelter, built under a data-sharing agreement.
- **v1:** prioritisation support: rank by vulnerability and system use. The case conference makes the final match. Prevention: flag eviction filings for rental-assistance outreach.
- **v2:** a risk model for first-time homelessness using eviction, benefits-loss and hospital-discharge data, evaluated by randomised rollout (as Los Angeles' California Policy Lab did).

**Risks & ethics.** Very sensitive data, so use minimal linkage, role-based access, and no police access to health data. Using police data can criminalise. Prioritisation tools like VI-SPDAT have shown racial bias, so audit. Consent and transparency with clients.

**Metrics & iteration.** HUD system performance measures: length of time homeless, returns within 6–24 months, placements per month, inflow vs outflow. Review the by-name list monthly with providers.

### Help a country distribute vaccines during a pandemic.

**Clarify**
- Users: the national immunisation programme manager, regional / district health officers, vaccination site managers, and the cold-chain logistics team.
- Decisions: allocation per region per week; site placement; who is eligible next.
- Success: coverage of priority groups (elderly, health workers) and overall, doses wasted (expiry, cold-chain breaks), equity across regions and income, and time from arrival to arm.

**Scope.** Weekly allocation of doses to regions plus stock visibility at district stores. Visibility comes before optimisation.

**MECE breakdown.** (1) Supply: inbound shipments and schedule. (2) Storage and distribution: cold chain, transport. (3) Demand: eligible population, uptake, hesitancy. (4) Administration: sites, staff, appointments. (5) Tracking: doses given, second doses, adverse events.

**Data & quality**
- Manufacturer and donor shipment schedules (COVAX).
- National logistics MIS (e.g. DHIS2 / OpenLMIS / VIMS), plus temperature loggers (e.g. Nexleaf).
- Census and population registries, which are often outdated. Use WorldPop / Facebook population density.
- Vaccination records (often paper in rural areas, digitised later); sites and staff rosters.

**Solution**
- **v0:** a stock-and-coverage dashboard. Stock by store with days-of-supply and expiry dates; doses given vs target by district. Daily SMS / USSD stock reporting where there is no internet.
- **v1:** an allocation model proportional to eligible priority population and absorption capacity (doses given last week), with expiry-aware redistribution ("move 5k doses from A to B before they expire in 10 days").
- **v2:** micro-planning: site placement by travel time (catchment analysis), and outreach targeting for low-uptake areas; second-dose reminders by SMS.

**Risks & ethics.** Political pressure on allocation, so publish the transparent formula. Equity: urban bias in data. Privacy of the vaccination registry. Misinformation. Cold-chain failure in remote areas.

**Metrics & iteration.** Coverage by priority group and region, wastage %, stock-outs, days doses sit in stores, dose-2 completion. Re-run allocation weekly with actual uptake.

### Help a border agency process immigration cases faster.

**Clarify**
- Users: case officers / adjudicators, supervisors (allocating work), and applicants (indirectly).
- Which case types: visas, asylum, citizenship? Each has very different legal standards.
- Decision: the officer still decides. Our lever is how fast a case reaches a ready-to-decide state.
- Success: median and P90 time to decision, backlog size, decision quality (appeal overturn rate), consistency across officers.

**Scope.** One high-volume, rule-based case type (e.g. a visa renewal or work permit). Measure where time is spent before automating anything.

**MECE breakdown.** (1) Intake: completeness of applications. (2) Waiting: queue time between steps. (3) Evidence gathering: background / security checks, document verification. (4) Officer review time. (5) Requests for evidence (RFE) and rework loops.

**Data & quality**
- The case management system (status history with timestamps) for process mining.
- Document uploads (scans, often poor quality).
- External check results (biometrics, watchlists) and officer allocation records.
- Status history is often incomplete; free-text notes hold the real reasons for delay.

**Solution**
- **v0:** process mining (e.g. Celonis or pm4py) over the status logs, to show where cases wait. Often 80% of elapsed time is queueing or waiting for checks, not officer work.
- **v1:** intake completeness checks (catch missing documents before submission, which reduces RFE loops); document classification and extraction (OCR plus an LLM) to pre-fill a case summary; triage of simple cases into a fast lane.
- **v2:** workload balancing across offices; a "case ready" packet that brings all checks together for the officer.

**Risks & ethics.** Life-changing decisions: no automated refusals, and explainability is legally required (reasons for decisions). Bias by nationality must be audited (the UK Home Office visa "streaming tool" was withdrawn in 2020 after a legal challenge). Security data access is strictly controlled.

**Metrics & iteration.** Cycle time by stage, backlog, RFE rate, first-time-right rate, appeal overturn rate, officer hours per case. Pilot in one office, A/B by case allocation.

### Help a tax authority find under-reported income.

**Clarify**
- Users: audit selection teams and auditors.
- Decision: which returns to audit, given limited auditor hours, and which segment (self-employed, cash businesses, high net worth) to focus on.
- Success: additional tax assessed per audit hour, the "no-change" audit rate (audits that find nothing, which waste time and burden honest taxpayers), and voluntary compliance (deterrence).

**Scope.** Third-party information mismatch for one segment, e.g. self-employed income vs payment-card (1099-K) and bank data. Income with little or no third-party reporting (cash, self-employment, gig work) has by far the highest misreporting rate in IRS tax-gap estimates.

**MECE breakdown.** (1) Income visibility: third-party reported vs not. (2) Matching: reported vs third-party data. (3) Risk scoring for income not covered by third-party reports (lifestyle and industry norms). (4) Audit selection and workflow. (5) Feedback from audit outcomes.

**Data & quality**
- Tax returns; third-party information returns (W-2, 1099s, 1099-K card payments).
- Property and vehicle registries; company registries; cross-border data (CRS / FATCA).
- Past audit results: labels biased towards previously selected cases (selection bias); the IRS uses random audits (NRP) for unbiased estimates.
- Entity matching across taxpayer IDs and businesses.

**Solution**
- **v0:** mismatch reports. 1099-K receipts much greater than reported gross receipts; sort by dollar gap and send automated notices (the cheapest intervention).
- **v1:** a risk model trained on random audit data plus industry benchmarks (e.g. a restaurant's reported margin vs peers); ranked audit list with reasons shown.
- **v2:** network analysis for linked entities and shell companies; behavioural "nudge" letters tested by RCT.

**Risks & ethics.** Audit burden should not fall disproportionately on low-income filers. A 2023 Stanford study found Black taxpayers were audited about 3–5x as often because of EITC-focused selection algorithms, so test for this. Taxpayer confidentiality (IRC 6103). Explainability for appeals.

**Metrics & iteration.** Yield per audit hour, no-change rate, collection rate, distribution of audits by income band. Keep a random-audit holdout for unbiased model evaluation.

### Help a city plan public transport routes.

**Clarify**
- Users: transit agency service planners, the board (approves network changes), and riders.
- Decision: route alignment, frequency per route, and stop placement, within a fixed operating budget (bus hours).
- Success: ridership, or coverage (access for everyone)? This is the classic ridership vs coverage trade-off (Jarrett Walker), and it is a political choice the agency must make explicitly. Also: jobs reachable within 45 minutes, and on-time performance.

**Scope.** A network redesign analysis for one corridor or district, focused on frequency (frequent networks drive ridership).

**MECE breakdown.** (1) Demand: where people travel, origin–destination. (2) Supply: current routes, frequency, running times. (3) Performance: ridership, reliability, crowding. (4) Constraints: budget, fleet, garages, road geometry. (5) Equity and access.

**Data & quality**
- GTFS (scheduled) and GTFS-realtime / AVL (actual).
- APC (automatic passenger counters) and farecard tap-ins. Tap-offs are often missing, so infer destinations from the next tap-in.
- Census LODES (home–work flows); mobile phone / location data (Replica, StreetLight).
- Land use, and demographics for equity (Title VI analysis in the US).

**Solution**
- **v0:** route performance scorecards: boardings per revenue hour, on-time %, crowding, coverage. Highlight low-performing segments and duplicated routes.
- **v1:** an OD matrix from farecard inference; accessibility analysis (Conveyal / R5 / OpenTripPlanner) comparing jobs reachable within 45 minutes for current vs scenario networks; scenario tool for planners (Remix-style).
- **v2:** frequency optimisation under the budget; dynamic schedule adjustments from realtime running times.

**Risks & ethics.** Cuts fall on transit-dependent, low-income riders. Equity analysis is required. Data from farecards under-represents cash riders. Community engagement: route changes produce strong backlash.

**Metrics & iteration.** Ridership, access to jobs (total and for equity populations), on-time %, cost per boarding. Before/after evaluation over 6–12 months.

### Help track illegal fishing / sanctions evasion using ship AIS data.

**Clarify**
- Users: maritime analysts at a coast guard, sanctions enforcement (OFAC, Treasury), NGOs (Global Fishing Watch), or a bank / insurer screening vessels.
- Decision: which vessels to inspect, board, deny port access to, or block payments for.
- Success: precision of leads (confirmed violations per lead investigated), and time from suspicious activity to alert.

**Scope.** Detect AIS "dark periods" (transponder switched off) and ship-to-ship transfers in one high-risk region (e.g. West Africa for fishing, or tankers near sanctioned ports).

**MECE breakdown.** (1) Vessel identity: IMO / MMSI, flag, ownership. (2) Behaviour: track, speed, loitering, gaps. (3) Encounters with other vessels. (4) Location context: EEZs, marine protected areas, ports. (5) Ownership network and risk scoring.

**Data & quality**
- AIS from terrestrial and satellite receivers (Spire, exactEarth, MarineTraffic). It is self-reported and spoofable: fake positions, swapped MMSIs, gaps from satellite coverage.
- SAR satellite imagery (Sentinel-1) and VIIRS night lights to find dark vessels that are not broadcasting.
- Vessel registries (IHS / S&P Sea-web, Lloyd's List); fishing licences; sanctions lists (OFAC SDN, EU, UN); port call records.

**Solution**
- **v0:** rule-based alerts on tracks: AIS gaps over 12 hours near sensitive zones, two vessels loitering together at under 2 knots and under 500 m apart (a likely transfer), and position jumps that are impossible (spoofing). Map UI with vessel history.
- **v1:** fishing behaviour classification from track patterns (trawling vs longline vs transit). Match SAR detections against AIS to find dark vessels. Ownership graph to sanctioned entities.
- **v2:** a risk score per vessel combining behaviour, ownership and flag-hopping history; daily ranked lead list.

**Risks & ethics.** Coverage gaps cause false "dark" alerts. Adversaries adapt (spoofing). Misidentifying a legitimate vessel costs its owner money (denied insurance). Keep human analyst review and an evidence trail.

**Metrics & iteration.** Lead precision, inspections leading to violations, detection latency, coverage of the area of interest. Analyst feedback labels each alert.

### Help an intelligence analyst find connections between people across many datasets.

**Clarify**
- Users: an all-source analyst with a specific question ("who is connected to this suspect?").
- Decision: what to report or recommend, and which leads to pursue.
- Success: time to answer the question, recall of real connections, and fewer missed links. Also: auditable provenance of every link.
- Legal authorities: what each dataset may be used for under what authority (e.g. Executive Order 12333 / FISA in the US).

**Scope.** One investigation workflow: start from a seed entity and expand to its linked entities with sources. Build entity resolution across 3–4 key datasets first.

**MECE breakdown.** (1) Ingestion and normalisation. (2) Entity resolution: same person across sources. (3) Relationship extraction: explicit (phone calls, transactions) vs inferred (co-location). (4) Search and graph exploration UI. (5) Provenance, access control and audit.

**Data & quality**
- Structured: phone records, travel bookings, financial transactions, company registries.
- Unstructured: reports and intercepts (need NER plus translation).
- Transliterated names (Mohammed / Muhammad), aliases, missing identifiers; conflicting sources; classification levels per record.

**Solution**
- **v0:** an ontology (Person, Phone, Account, Organisation, Event) and ingestion; deterministic plus fuzzy entity resolution with confidence scores; graph search from a seed, every edge linking to its source document.
- **v1:** NER and relation extraction from text with an LLM, with the analyst confirming. Link suggestions ("these two accounts share 3 devices"). Timeline and map views.
- **v2:** pattern queries (find people who shared phones with X within 2 hops); alerts when new data touches a watched entity.

**Risks & ethics.** False links ruin innocent people's lives. Show confidence; never merge entities silently; analysts must verify. Access control per data authority and classification; full audit log. Guard against mission creep on domestic data.

**Metrics & iteration.** Time to produce an assessment, ER precision and recall on a hand-labelled sample, analyst-accepted vs rejected link suggestions. Embed with analysts during live investigations.

### Help a school district reduce dropout rates.

**Clarify**
- Users: counsellors, teachers, principals, the district office.
- Decision: which students get which intervention (mentoring, tutoring, attendance outreach), and when.
- Success: graduation rate and on-track rates (credits, attendance), measured against a comparison group. The key is to **intervene early**: the ABC indicators (Attendance, Behaviour, Course performance) predict dropout from grade 6–9.

**Scope.** A 9th-grade early-warning system for one or two high schools. 9th-grade on-track status strongly predicts graduation (Chicago research).

**MECE breakdown.** (1) Identification: who is at risk. (2) Diagnosis: why (academic, attendance, family, health). (3) Intervention matching and capacity. (4) Follow-up and case management. (5) Evaluation.

**Data & quality**
- SIS (PowerSchool, Infinite Campus): attendance, grades, credits, discipline.
- Assessments; demographics; transfers (a student who transfers is not a dropout, but records often misclassify it).
- Intervention records, often in spreadsheets or not recorded at all.
- Data refresh is weekly at best; attendance coding varies by school.

**Solution**
- **v0:** a weekly early-warning list using transparent ABC thresholds (attendance under 90%, any F in a core course, more than one suspension) built into the counsellor workflow, with reasons shown.
- **v1:** a predictive model (gradient boosting) calibrated on past cohorts, only if it beats the simple indicators. Log interventions so you can track who got what.
- **v2:** intervention effectiveness analysis; staffing allocation by risk load per school.

**Risks & ethics.** Labelling students can lower teacher expectations, so share the list only with counsellors and use language like "needs support". Bias from discipline data (disproportionately applied to Black students). FERPA privacy. A list without intervention capacity is pointless.

**Metrics & iteration.** 9th-grade on-track rate, attendance improvement after intervention, graduation rate of flagged students vs a matched comparison, counsellor usage. Iterate each semester.

### Help a city manage its water supply during a drought.

**Clarify**
- Users: utility operations (reservoir releases, pumping), the conservation / demand-management team, city leadership (restriction stages), and customers.
- Decision: when to trigger restriction stages, which supply source to draw, and whom to target for conservation.
- Success: keep storage above the critical threshold through the drought, meet the demand-reduction target (e.g. 20%), keep revenue stable, and avoid service interruption.

**Scope.** Demand-side first: identify the biggest discretionary users (outdoor irrigation), plus a weekly supply-vs-demand projection for leadership.

**MECE breakdown.** (1) Supply: reservoir, groundwater, imports, recycled. (2) Demand: residential indoor/outdoor, commercial, agricultural. (3) Losses: leaks, non-revenue water (often 15–30%). (4) Policy levers: restrictions, pricing, enforcement. (5) Forecasting: weather and inflows.

**Data & quality**
- Reservoir levels; streamflow (USGS); weather forecasts (NOAA).
- AMI smart meters (hourly) or monthly billing reads (much coarser).
- SCADA (pumps, pressure); GIS (parcels, irrigated area from imagery); customer accounts; leak reports.

**Solution**
- **v0:** a storage runway dashboard ("days of supply at current demand and inflow scenarios"), plus a top-users list by parcel (outdoor use = summer use minus winter baseline).
- **v1:** AMI-based leak detection (continuous overnight flow) with customer alerts; targeted outreach and enforcement for irrigation violations; district metered area analysis to locate main leaks.
- **v2:** demand forecasting with weather, and an optimisation of source mix and restriction stage timing; tiered pricing scenario modelling.

**Risks & ethics.** Equity: per-capita targets penalise large households, and pricing hits low-income customers. Privacy of hourly meter data (it reveals occupancy). Revenue drops when conservation succeeds.

**Metrics & iteration.** Demand vs target (gallons per capita per day), storage trajectory vs plan, non-revenue water %, leak alerts resolved. Update forecasts weekly.

### Help a fire department predict which buildings are at highest fire risk.

**Clarify**
- Users: the fire prevention bureau (inspection scheduling) and inspectors.
- Decision: which commercial buildings to inspect this month, given limited inspectors. (Residential outreach such as smoke-alarm installs is a related use case.)
- Success: fires and fire deaths prevented, or proxied by violations found per inspection and fires in inspected vs uninspected high-risk buildings.

**Scope.** Re-rank the existing commercial inspection list. This is what Atlanta's Firebird project and NYC FDNY's FireCast did.

**MECE breakdown.** (1) Building inventory: what exists. (2) Risk factors: physical, occupancy, history. (3) Inspection history and capacity. (4) Prioritisation. (5) Outcome feedback.

**Data & quality**
- Property / tax assessor records (age, construction type, use, size); permits; code violations; past fire incidents (NFIRS).
- Business licences (occupancy type); 311 complaints; utility shut-offs; vacancy.
- Census (for residential risk: elderly, poverty).
- Joining needs address / parcel ID standardisation, which is often the biggest task. Many buildings are missing from the inspection inventory entirely (Firebird found thousands).

**Solution**
- **v0:** a reconciled building inventory (find uninspected commercial properties), plus a simple points-based risk score from known factors.
- **v1:** a gradient boosted model predicting fire incidents in the next year per building; calibrated; top reasons shown; monthly inspection list in the inspectors' scheduling tool.
- **v2:** residential targeting for smoke-alarm canvassing using census-block risk (American Red Cross approach); feedback from inspections into the model.

**Risks & ethics.** Fires are rare, so the labels are noisy; evaluate on precision@k for top-N. Avoid only targeting poor neighbourhoods for enforcement; frame it as prevention (outreach, not fines) for residential. Inspectors may distrust it, so let them add local knowledge.

**Metrics & iteration.** Fires in top-decile buildings captured (recall@k), violations found per inspection, inventory coverage. Retrain yearly; compare to the old list.

## B. Healthcare

### A hospital wants to reduce patient readmissions. Build a solution.

**Clarify**
- Users: discharge planners / transitional care nurses (decide who gets a follow-up call, home visit or pharmacist review), hospitalists, and the quality department.
- Which readmissions? 30-day all-cause, or the CMS HRRP conditions (heart failure, AMI, pneumonia, COPD, hip/knee, CABG), which carry penalties.
- Success: 30-day readmission rate for the targeted cohort, compared with the baseline / control, without increased ED revisits or mortality.

**Scope.** Heart failure patients at one hospital: high volume, penalised, and with known interventions (follow-up within 7 days, medication reconciliation, weight monitoring).

**MECE breakdown.** (1) Risk identification before discharge. (2) Root causes: medication, follow-up access, social factors, clinical deterioration. (3) Intervention capacity and matching. (4) Post-discharge monitoring. (5) Measurement.

**Data & quality**
- EHR (Epic / Cerner): diagnoses, labs, vitals, meds, prior admissions, discharge disposition.
- ADT feeds (admit/discharge/transfer) in real time via HL7; claims data (catches readmissions to other hospitals, which the EHR misses); social determinants (often missing).
- Epic has a built-in readmission risk model; LACE / HOSPITAL scores are simple baselines.

**Solution**
- **v0:** a daily discharge worklist ranked by the LACE score (Length of stay, Acuity, Comorbidities, ED visits), showing whether a follow-up appointment is booked and meds reconciled. This alone closes gaps.
- **v1:** a calibrated ML model (gradient boosting on EHR features) inside Epic, only if it beats LACE on AUROC and precision@top-20%. Track interventions given.
- **v2:** post-discharge remote monitoring (scales, symptoms by text) with escalation; target "impactable" patients rather than just the highest risk (some highest-risk patients will be readmitted whatever you do).

**Risks & ethics.** Using cost as a proxy for need introduced racial bias in a well-known algorithm (Obermeyer et al., 2019), so audit by race. HIPAA. Alert fatigue. Gaming, e.g. observation stays not counted as readmissions.

**Metrics & iteration.** Readmission rate (stepped-wedge or pre/post with a comparison unit), intervention completion rate, follow-up within 7 days, model calibration. Monthly review with the care transitions team.

### A hospital's ER is overcrowded. How would you help?

**Clarify**
- Users: the ED charge nurse (real-time flow), the house supervisor / bed manager, and hospital leadership.
- Is the problem input (too many arrivals), throughput (slow in the ED), or output (admitted patients **boarding** in the ED waiting for an inpatient bed)? Output is usually the main cause.
- Success: door-to-provider time, ED length of stay, left-without-being-seen (LWBS) rate, boarding hours.

**Scope.** Measure the three components first. If boarding dominates, focus on inpatient discharge timing ("discharge before noon"), which is outside the ED.

**MECE breakdown.** Input (arrivals, acuity mix, ambulance diversion) / Throughput (triage, labs, imaging, consults, decision to admit) / Output (bed assignment, boarding, inpatient discharges).

**Data & quality**
- ED tracking board timestamps (arrival, triage, provider, decision, departure) from Epic ASAP / Cerner FirstNet.
- Lab and imaging turnaround times; bed management system (bed requests, assignments, cleaning status, e.g. TeleTracking); inpatient discharge orders vs actual discharge times.
- Timestamps are fairly good but "decision to admit" is often entered late.

**Solution**
- **v0:** a patient flow dashboard: time spent in each segment, boarding hours by unit, arrivals vs discharges by hour (often shows inpatient discharges peak at 3–5pm while ED arrivals peak at 11am).
- **v1:** arrival forecasting by hour to match staffing; a predicted admission at triage (ML on triage data) so the bed request starts early; a discharge-readiness list for inpatient units.
- **v2:** a hospital-wide capacity command centre (as at Johns Hopkins) with predicted occupancy 24–72 hours out and escalation playbooks.

**Risks & ethics.** Pressure to discharge early can harm patients, so watch readmissions. Predictions that bias triage. Staff overwhelmed by another screen.

**Metrics & iteration.** Door-to-provider, ED LOS for admitted vs discharged patients, LWBS %, boarding hours, discharge-before-noon %. Daily huddle review.

### Help a hospital network manage bed capacity across sites.

**Clarify**
- Users: the network transfer centre / capacity command centre, site bed managers, and the CNO.
- Decision: where to place a new admission or transfer; when to open surge beds; when to divert ambulances; elective surgery scheduling.
- Success: fewer declined transfers, lower boarding, occupancy balanced (no site at 105% while another sits at 75%), fewer cancelled surgeries.

**Scope.** Real-time network-wide bed census with staffed beds (not just physical beds), for adult med/surg and ICU.

**MECE breakdown.** (1) Current state: occupied, available, staffed, dirty/clean beds. (2) Demand: ED admits, transfers, elective surgery. (3) Supply: discharges, staffing. (4) Placement decisions. (5) Forecasting and surge planning.

**Data & quality**
- ADT feeds from each site's EHR (possibly different EHRs after mergers, so HL7 / FHIR normalisation).
- Bed board systems; staffing / scheduling (Kronos / UKG); OR schedule; transfer centre logs.
- Bed definitions differ across sites (what counts as "available"?). Standardise this first.

**Solution**
- **v0:** a unified capacity view across sites: staffed beds by unit type, pending admits and expected discharges today. Refreshed every 15 minutes from ADT.
- **v1:** 24–72 hour occupancy forecast (arrivals by hour, length-of-stay prediction per patient); a transfer placement recommendation considering service line and distance.
- **v2:** elective surgery smoothing (move scheduled cases to balance inpatient demand); surge triggers and playbooks.

**Risks & ethics.** Moving patients away from family and their usual doctors; clinicians must agree to transfers. Staffing is the real constraint, so do not show beds that cannot be staffed. Data latency causing double-booking.

**Metrics & iteration.** Occupancy variance across sites, transfer acceptance rate, ED boarding hours, surgery cancellations for capacity, forecast error (MAPE). Daily capacity huddle feedback.

### Help a pharma company speed up clinical trial recruitment.

**Clarify**
- Users: clinical operations (site selection and feasibility), site coordinators (screen patients), and the study team.
- Decision: which sites to open, which patients to approach, which protocol criteria to relax.
- Success: time to first patient in and to last patient in, screen-fail rate, enrolment per site per month, and diversity of enrolled patients (FDA diversity action plan requirements; verify current guidance status).

**Scope.** Site selection for one upcoming trial in one indication: an often-cited industry figure is that about 80% of trials miss enrolment timelines, frequently because of poorly chosen sites.

**MECE breakdown.** (1) Protocol feasibility: are the criteria too strict? (2) Site selection. (3) Patient identification at sites. (4) Patient engagement and retention. (5) Operational monitoring.

**Data & quality**
- Historical site performance from the CTMS (Veeva, Medidata): enrolment, screen fail.
- Real-world data: claims (IQVIA, Komodo), EHR networks (TriNetX, Flatiron for oncology), which show where eligible patients actually are.
- ClinicalTrials.gov (competing trials); protocol inclusion/exclusion criteria as free text.
- RWD is de-identified and only approximates the criteria (labs are often missing).

**Solution**
- **v0:** a site scorecard: historical enrolment rate in the indication, startup time, competing trials nearby, and eligible patient counts from RWD by geography.
- **v1:** an LLM to parse inclusion/exclusion criteria into structured queries; run them against RWD to estimate the eligible population and show which criterion removes the most patients (protocol optimisation). At sites, EHR-based pre-screening lists for coordinators.
- **v2:** recruitment forecasting with monitoring that alerts when a site underperforms early, so you can add sites; decentralised trial elements for retention.

**Risks & ethics.** HIPAA and patient consent for EHR screening; IRB approval. Diversity: optimising for speed picks academic centres in affluent areas. Do not market to patients deceptively.

**Metrics & iteration.** Enrolment rate vs plan, screen-fail %, cycle time to site activation, diversity of enrolment. Compare forecasts with actuals per trial.

### Help a health insurer detect fraudulent claims.

**Clarify**
- Users: the Special Investigations Unit (SIU), pre-payment review nurses / coders, provider relations.
- Type: provider fraud (billing for services not rendered, upcoding, unbundling) is far larger than member fraud. Waste and abuse (FWA) overlap.
- Decision: pay, hold for review, or investigate a provider.
- Success: dollars recovered or prevented per reviewer hour, false-positive rate (held legitimate claims anger providers and breach prompt-pay laws).

**Scope.** Post-payment provider-level outlier analysis in one specialty with known fraud patterns (e.g. DME, labs, behavioural health, physical therapy).

**MECE breakdown.** (1) Rule violations: impossible billing (e.g. more than 24 hours of services in a day, procedures inconsistent with gender). (2) Statistical outliers vs peers. (3) Networks: providers, patients and referrers colluding. (4) Pre-payment vs post-payment workflow. (5) Case management and recovery.

**Data & quality**
- Claims (837 format): CPT/HCPCS, ICD-10, provider NPI, member, dates, amounts.
- Provider registries (NPPES), OIG exclusion list, licence boards; prior SIU case outcomes (few labels).
- Claims are clean structurally but lag; codes change yearly.

**Solution**
- **v0:** rule-based edits plus peer-comparison outlier reports (e.g. this provider bills 99215, the highest office-visit level, for 80% of visits vs a 20% peer average). Ranked provider list for SIU with evidence.
- **v1:** an unsupervised anomaly model at provider level; network analysis of shared patients / referral rings; a supervised model as SIU outcomes accumulate.
- **v2:** pre-payment scoring of high-risk claims with a hold queue, and an LLM to compare medical records against billed codes for reviewers.

**Risks & ethics.** Denying legitimate care. Unusual is not fraud (specialists treat sicker patients), so risk-adjust. Providers must get due process. HIPAA.

**Metrics & iteration.** Hit rate of SIU referrals, recovered dollars, savings vs cost, provider abrasion (appeals overturned). Feedback from case outcomes monthly.

### Help a hospital reduce nurse burnout / optimise staffing schedules.

**Clarify**
- Users: nurse managers (build schedules), the staffing office (daily adjustments, float pool), nurses themselves, and the CNO.
- Decision: the 6-week schedule, daily staffing levels, float / agency use, and shift-swap approvals.
- Success: turnover and vacancy rate, overtime and agency hours, nurse satisfaction (e.g. a burnout survey), mandatory overtime instances, patient safety indicators (falls, pressure injuries).

**Scope.** One department: replace the manual scheduling spreadsheet with a schedule that respects nurse preferences, and measure staffing against acuity-adjusted demand.

**MECE breakdown.** (1) Demand: census and patient acuity by unit and shift. (2) Supply: nurses, skills, availability, preferences. (3) Scheduling: the 6-week plan. (4) Daily adjustment: floats, call-offs. (5) Workload and well-being signals.

**Data & quality**
- Scheduling / timekeeping (UKG Kronos, API Healthcare, ShiftWizard); payroll overtime.
- ADT census and acuity scores; HR data (turnover, exit interview reasons); engagement surveys; EHR audit logs (documentation time, after-shift charting).
- Preferences are usually informal (texts and notes to the manager).

**Solution**
- **v0:** a workload dashboard: nurse-to-patient ratio by shift vs target, overtime and consecutive shifts per nurse, highlighting unfair distribution (who always gets weekends and nights).
- **v1:** a scheduling optimiser (constraint programming with OR-Tools / CP-SAT) with hard rules (ratios, rest between shifts, skill mix) and soft rules (preferences, fairness of weekends); nurse self-scheduling app; census forecast to plan floats 1–2 weeks ahead.
- **v2:** burnout risk signals (overtime streaks, missed breaks, after-hours charting) for proactive managers; documentation burden reduction (ambient AI scribes).

**Risks & ethics.** Monitoring individual nurses feels like surveillance, so aggregate and let nurses see their own data. Union contracts and state ratio laws. Opaque algorithms rejected by staff; managers must be able to override.

**Metrics & iteration.** Turnover, overtime/agency spend, preference fulfilment %, nurse satisfaction pulse, safety indicators. Per scheduling period, ask nurses what the optimiser got wrong.

### Help a hospital manage its medical supply inventory.

**Clarify**
- Users: supply chain / materials managers, unit nurses (who hoard supplies because they do not trust the system), OR inventory staff, finance.
- Decision: par levels per unit, reorder quantities, substitutions during shortages, expiry management.
- Success: stock-outs of critical items, inventory carrying cost / days on hand, expired write-offs, nurse time hunting for supplies.

**Scope.** The OR, where high-value implants and preference cards drive large cost and waste, or high-turnover med/surg supplies on 3 units.

**MECE breakdown.** (1) Consumption measurement. (2) Stock visibility: central vs units vs hidden hoards. (3) Replenishment policy: par levels, reorder points. (4) Supplier risk and shortages. (5) Waste and expiry.

**Data & quality**
- ERP / MMIS (Workday, Infor, Oracle); PAR scanning / automated dispensing (Omnicell, Pyxis).
- EHR charge capture (what was used on which patient); OR preference cards (often out of date: items opened but not used); GPO contracts; supplier backorder notices (often email).
- Consumption is invisible where items are not scanned.

**Solution**
- **v0:** a stock visibility and stock-out report; compare par levels with actual usage (most par levels are set once and never revisited).
- **v1:** dynamic par levels from usage forecasts (by unit, day of week, census); expiry alerts with rotation; preference card clean-up from the record of items opened vs used.
- **v2:** shortage early warning from supplier and FDA shortage lists, with suggested clinical substitutes; network-level balancing across hospitals.

**Risks & ethics.** A stock-out in surgery is a patient-safety event, so keep safety stock for critical items. Nurses hoard if trust is low; that is a change-management issue. Recalls must be traceable to patient (UDI).

**Metrics & iteration.** Stock-outs, inventory value / days on hand, expired write-offs, $ saved on preference cards, nurse survey on supply availability. Monthly par review.

### Help doctors spend less time on documentation.

**Clarify**
- Users: physicians in a specific setting (primary care is the biggest pain: one widely cited time-motion study (Sinsky et al., 2016) found nearly 2 hours of EHR and desk work per hour of direct patient time, plus "pajama time" charting at night), plus nurses and coders downstream.
- Which documentation: visit notes, in-basket messages, prior auth letters, discharge summaries?
- Success: time in notes per visit and after-hours EHR time (from EHR audit logs, e.g. Epic Signal), note quality, burnout scores, patient experience.

**Scope.** Ambient AI scribe for outpatient visits in one specialty clinic, about 20 physician volunteers. Vendors already sell this (e.g. Microsoft Dragon Copilot / DAX, Abridge, Ambience; the landscape changes quickly, so verify).

**MECE breakdown.** (1) Capture: audio, consent. (2) Generation: transcript to structured note (SOAP / HPI). (3) Review and edit by the physician. (4) EHR integration: note, orders, codes. (5) Downstream: coding, patient instructions, in-basket replies.

**Data & quality**
- Visit audio (consent required; noisy; multiple speakers; accents; interpreters).
- EHR context (problem list, meds) for grounding; specialty note templates; physician edit diffs (excellent quality signal); EHR audit logs for time-in-notes.

**Solution**
- **v0:** buy rather than build if a vendor fits; pilot with 20 doctors, measure baseline vs after for 4–6 weeks.
- **v1 (if building):** ASR (e.g. Whisper fine-tuned on medical vocabulary) → LLM note generation with templates per specialty, grounded on the transcript; every statement traceable to a transcript span; physician signs off; integrate via Epic FHIR / note APIs.
- **v2:** draft in-basket replies, order suggestions, and coding (ICD-10 / E&M level) for coder review.

**Risks & ethics.** Hallucinations (an invented exam finding is a medico-legal risk), so the physician must review and the UI should highlight uncertain items. Omissions. Patient consent for recording; data retention; BAA with the vendor. Upcoding risk from AI coding.

**Metrics & iteration.** Time in notes per visit, after-hours EHR time, edit rate per note, physician satisfaction / burnout survey, error audit of a sample of notes by clinicians, adoption (percentage of visits using it after 8 weeks).

## C. Commercial / enterprise

### An airline has frequent flight delays. How do you help?

**Clarify**
- Users: the operations control centre (OCC): duty manager, crew schedulers, maintenance control, and station managers at hubs.
- Decision: during the day (swap aircraft, hold or release connecting flights, call reserve crew), or in planning (schedule turn times, buffers)?
- Success: D0/D15 on-time performance (departures within 15 minutes), delay minutes per departure, cancellations, misconnected passengers, cost of disruption (crew overtime, compensation such as EU261).

**Scope.** Root-cause the delays first, then pick the largest controllable category, often **reactionary (knock-on) delays** from tight turns at a hub.

**MECE breakdown.** IATA delay codes split cleanly into (1) airline-controllable: turnaround / ground handling, crew, maintenance, boarding. (2) Reactionary: late inbound aircraft, usually the biggest share. (3) External: ATC, weather, airport. Then planning vs day-of-operations levers.

**Data & quality**
- Flight operations (actual out/off/on/in times via ACARS); delay codes (entered by the station, inconsistently).
- Crew rosters (Sabre, Jeppesen); maintenance logs (AMOS / TRAX); turnaround milestone data (often manual or missing; computer vision on the ramp from Assaia gives it).
- Weather (METAR / TAF), ATC flow programmes (FAA ATCSCC); passenger connection data (PSS).

**Solution**
- **v0:** a delay root-cause analysis: propagate delays along aircraft rotations to find the flights that start chains, and turn-time vs scheduled-turn by station.
- **v1:** a planning fix: re-time schedule buffers where scheduled turns are less than the P70 actual turn; a day-of-operations delay prediction 2–4 hours ahead, so OCC can pre-emptively swap tails or call reserve crew.
- **v2:** an integrated disruption recovery optimiser (aircraft, crew and passengers together), as offered by vendors such as Sabre or built in-house.

**Risks.** Safety and crew legality (rest rules) are hard constraints. Gaming of delay codes. Adding buffer costs aircraft utilisation, so quantify the trade-off.

**Metrics & iteration.** D15 OTP, reactionary delay minutes, misconnects, disruption cost; A/B by schedule season.

### A manufacturer has too many defects on its production line. Help them.

**Clarify**
- Users: the quality engineer (root cause), line supervisors / operators (real-time adjustments), plant manager.
- Which defects, where detected (in-line, end-of-line, customer returns)? What are the cost and volume per defect type?
- Success: first-pass yield, scrap/rework cost, defects per million (DPMO), customer returns.

**Scope.** Pareto the defects: usually 2–3 defect types on one line cause most of the cost. Take the top one.

**MECE breakdown.** The 6Ms (Ishikawa): Machine (process parameters), Material (supplier lots), Method, Man (shift / operator), Measurement (is the inspection itself right?), Mother nature (temperature, humidity). Then detection vs prevention.

**Data & quality**
- MES (production records, serial or lot genealogy); quality management system (defect logs, often free text).
- Machine sensors / PLC via SCADA or historian (OSIsoft PI / AVEVA); supplier lot data; inspection images.
- Key challenge: **traceability**, linking a defective part back to the exact machine, time and material lot. Clocks are often unsynchronised.

**Solution**
- **v0:** a joined genealogy dataset: defects by lot / machine / shift / parameter window; simple statistical comparisons and SPC charts. This often finds the cause (e.g. one supplier lot, or a temperature drift on the night shift).
- **v1:** a model of defect probability from process parameters (with SHAP for the drivers); real-time alerts when parameters drift into risky ranges; computer vision inspection where defects are visual.
- **v2:** closed-loop recommendations for parameter set-points; supplier quality scorecards.

**Risks.** Correlation is not causation, so confirm with designed experiments (DOE) on the line. Operator blame culture. Edge deployment and OT network security.

**Metrics & iteration.** First-pass yield, scrap cost, time to detect drift. Weekly quality review; each confirmed root cause becomes a rule.

### A retailer keeps running out of stock on popular items. Help them.

**Clarify**
- Users: merchandise planners / buyers (orders from suppliers), allocation and replenishment analysts (store-level), store managers.
- Where do the stock-outs happen: in the DC (supplier problem) or on the shelf while the store has stock in the backroom (execution / phantom inventory)?
- Success: on-shelf availability (OSA), lost sales, without inflating inventory (weeks of supply, markdowns).

**Scope.** The top 500 SKUs by revenue (A items) in one region, store-level replenishment.

**MECE breakdown.** (1) Forecasting demand. (2) Replenishment policy (reorder point, safety stock, order frequency). (3) Supply: supplier fill rate, lead time variability. (4) Store execution: backroom to shelf, phantom inventory. (5) Allocation between stores.

**Data & quality**
- POS sales (censored: zero sales during a stock-out is not zero demand, so you must unconstrain demand).
- Inventory positions (perpetual inventory is often wrong by 60%+ of SKUs per store, per research); purchase orders and supplier ASNs; promotions calendar; shelf scans or images.

**Solution**
- **v0:** a stock-out diagnosis: for each out-of-stock event classify the cause (forecast miss, supplier late, store had stock, inventory record wrong). Plus a daily "likely phantom inventory" list (system shows stock but zero sales for N days) for store staff to check.
- **v1:** a better forecast (gradient boosting with promotions, seasonality, weather) on unconstrained demand; safety stock set by service-level target per SKU class.
- **v2:** multi-echelon inventory optimisation; supplier collaboration (shared forecasts); computer vision shelf monitoring.

**Risks.** More stock means more working capital and markdowns; set the target by item class. Store staff adoption of the task list.

**Metrics & iteration.** OSA %, lost sales estimate, inventory turns, forecast WAPE, phantom-inventory corrections. Pilot vs control stores.

### A bank wants to detect money laundering.

**Clarify**
- Users: AML analysts / investigators (review alerts and file SARs), the MLRO / compliance officer, regulators (indirectly).
- Today: rule-based transaction monitoring (e.g. Actimize, SAS AML) produces huge alert volumes with **95–99% false positives**.
- Success: fewer false positives without missing true cases, higher SAR conversion rate, investigator hours per alert, regulatory findings.

**Scope.** Alert triage: rank existing rule alerts so analysts spend time on the riskiest first. The regulator accepts this more easily than replacing rules (model risk management, SR 11-7).

**MECE breakdown.** (1) Customer risk: KYC/CDD. (2) Transaction monitoring: scenarios. (3) Network: relationships among accounts. (4) Investigation workflow and case management. (5) Reporting (SARs) and feedback.

**Data & quality**
- Core banking transactions; wires (SWIFT); cards; KYC data (often stale); sanctions / PEP lists; adverse media.
- Alert and case history with dispositions (labels, but biased towards what rules catch); entity resolution across products and counterparties.

**Solution**
- **v0:** alert prioritisation model trained on past dispositions (SAR filed vs closed), with reason codes; analysts work the queue top-down; low-risk alerts still reviewed (initially) to prove safety.
- **v1:** a customer-level network view (graph of counterparties); features like rapid movement of funds, structuring just under thresholds, fan-in/fan-out; LLM summaries of the case narrative for the investigator (drafting SARs).
- **v2:** detection beyond rules: graph ML for mule networks; continuous KYC refresh.

**Risks.** Regulatory: models must be explainable, validated and documented; never auto-close an alert without an agreed policy. De-risking that excludes legitimate customers. Data privacy and tipping-off rules.

**Metrics & iteration.** SAR conversion rate, false positive rate, alerts per investigator, regulatory exam outcomes; below-the-line testing to catch misses.

### A shipping company wants to optimise routes and reduce fuel costs.

**Clarify**
- Is this ocean shipping (vessels) or trucking / parcel delivery? Ask. Assume ocean.
- Users: voyage planners / chartering, the fleet performance team, and captains on board.
- Decision: route (weather routing), speed (slow steaming), trim and hull cleaning timing, bunkering port.
- Success: fuel consumption per nautical mile at a given speed, $ fuel per voyage, on-time arrival, CO2 intensity (IMO CII rating, EU ETS costs).

**Scope.** Speed optimisation on one trade lane: fuel burn per hour rises roughly with the cube of speed (per nautical mile, roughly the square), so arriving "just in time" rather than sailing fast and waiting at anchor saves the most.

**MECE breakdown.** (1) Voyage planning: route, speed, ETA. (2) Vessel efficiency: hull fouling, trim, engine. (3) Port operations: waiting time, berth scheduling. (4) Fuel procurement: where and when to bunker. (5) Emissions compliance.

**Data & quality**
- Noon reports (manual, daily: speed, fuel, weather), notoriously noisy; high-frequency sensor data (fuel flow meters, shaft power) on newer ships.
- AIS tracks; weather / ocean currents (ECMWF, NOAA); port congestion data; charter party terms (speed and consumption warranties).

**Solution**
- **v0:** a fleet performance dashboard: fuel vs speed curves per ship, and waiting time at anchor per port call (often several days, which is pure waste).
- **v1:** speed recommendations from berth availability (virtual arrival); a hull fouling model to time cleaning; weather routing (vendor or in-house).
- **v2:** a fleet-wide optimiser with bunker price planning; emissions reporting.

**Risks.** Commercial contracts may require specific speeds; safety in heavy weather overrides; crew trust in shore recommendations.

**Metrics & iteration.** Fuel per nautical mile normalised for weather, anchor waiting hours, CII rating, savings vs a baseline voyage. Monthly per-vessel review.

### An energy company wants to predict equipment failures on the grid.

**Clarify**
- Users: asset managers (replacement capital plan), maintenance planners (inspection schedules), and the control room (real-time).
- Which assets: transformers, poles, lines, breakers? Failure modes differ.
- Decision: what to inspect, repair or replace this year with a fixed budget.
- Success: SAIDI / SAIFI (outage duration / frequency), failures avoided, wildfire ignitions (in fire-prone regions), capital efficiency.

**Scope.** Distribution transformers, or wood poles in one region: high volume and data available.

**MECE breakdown.** (1) Asset registry and condition. (2) Stressors: load, weather, vegetation, age. (3) Failure history. (4) Risk = probability × consequence (customers affected, critical loads, fire risk). (5) Intervention planning.

**Data & quality**
- GIS asset registry (often incomplete: wrong install dates, missing assets); work orders / outage management system (OMS) history.
- AMI smart meter voltage data (detects failing transformers); SCADA for substations; dissolved gas analysis for large transformers; inspection photos (drones / LiDAR for vegetation); weather.
- Failures are rare and the cause field in outage records is unreliable.

**Solution**
- **v0:** a risk-ranked asset list from a transparent score (age, condition rating, load, past outages) mapped by circuit; compare with the current replacement plan.
- **v1:** a survival / gradient boosting model predicting failure probability in 12 months; AMI voltage anomaly detection for transformers; computer vision on inspection photos for defects.
- **v2:** a risk-based capital optimiser (maximise risk reduction per dollar); real-time wildfire risk combining weather and asset condition for public safety power shutoffs.

**Risks.** Safety and wildfire liability (PG&E); regulators review capital plans, so explainability matters; selection bias in labels.

**Metrics & iteration.** SAIDI / SAIFI on targeted circuits, failure rate of top-decile assets (precision@k), inspections that find defects. Compare predicted vs actual failures annually.

### An oil & gas company wants to improve drilling efficiency.

**Clarify**
- Users: drilling engineers (well plans), the rig team / driller (real-time parameters), the remote operations centre, and the drilling superintendent.
- Success: days per well / feet per day (rate of penetration, ROP), non-productive time (NPT, often 15–25% of rig time), cost per foot, safety incidents.

**Scope.** Reduce non-productive and "invisible lost time" on one basin's pad-drilling programme, comparing performance across rigs and crews.

**MECE breakdown.** (1) Planning: well design, bit and BHA selection. (2) On-bottom drilling performance (ROP optimisation). (3) Flat time: connections, tripping, casing. (4) Non-productive time: stuck pipe, equipment failure, waiting. (5) Learning across wells.

**Data & quality**
- Rig sensor data at 1 Hz (WITSML): hookload, weight on bit, RPM, torque, flow, pressure.
- Daily drilling reports (DDR, free text with NPT codes, inconsistent); offset well data; geology / formation tops; bit records.
- Sensor data is noisy and calibration varies by rig.

**Solution**
- **v0:** rig state classification from sensor data (drilling, connection, tripping); benchmark connection times and tripping speeds across crews: best-crew vs average is often a large, cheap gain.
- **v1:** ROP optimisation recommendations per formation (WOB/RPM) learned from offset wells; NPT text mining of reports for recurring causes; stuck-pipe early warning.
- **v2:** closed-loop drilling automation advisory with the driller in control.

**Risks.** Well control and safety: advisory only, never override. Crew pushback on benchmarking, so frame it as coaching. Remote rigs have limited bandwidth, so run edge processing.

**Metrics & iteration.** Feet per day, connection time, NPT %, cost per foot. Learning curve across successive wells on the pad.

### A car manufacturer wants to reduce warranty claims.

**Clarify**
- Users: quality engineers (field quality), warranty analysts, supplier quality, and design engineers.
- Decision: which emerging issues to investigate, when to issue a fix (production change, service campaign, recall), and supplier chargebacks.
- Success: warranty cost per vehicle, incidents per thousand vehicles (IPTV), time from first claim to detection of an emerging issue, recall cost.

**Scope.** Early warning of emerging issues: detecting a problem at 50 claims instead of 500 saves the most money.

**MECE breakdown.** (1) Detection of emerging issues. (2) Root cause: design, supplier, assembly plant / build date. (3) Fix and containment. (4) Claim validity: warranty fraud by dealers. (5) Feedback to design.

**Data & quality**
- Warranty claims from dealers: part codes, labour codes, technician comments (free text, multilingual), mileage.
- Vehicle build data (VIN to plant, build date, part supplier lots); telematics / DTC fault codes from connected vehicles; NHTSA complaints; social media and forums; call-centre notes.
- Claim reporting lags (weeks), and maturity varies (new vehicles have fewer months in service), so normalise by exposure.

**Solution**
- **v0:** exposure-normalised claim rate trends by part / model / build month, with statistical alerts (e.g. control charts on claims per 1,000 vehicles in service).
- **v1:** NLP / LLM clustering of technician comments and NHTSA complaints into issues ("water leak at A-pillar"); link to build data to isolate supplier lot or build window.
- **v2:** connected-vehicle telematics to predict failures before the customer notices; dealer claim anomaly detection.

**Risks.** Regulatory safety obligations (TREAD Act reporting): if you detect a safety defect you must act, so involve legal. Customer data privacy for telematics.

**Metrics & iteration.** Detection lead time, cost per vehicle, IPTV, recall size (smaller is better: caught earlier). Weekly field quality meeting uses the tool.

### A telecom wants to reduce customer churn.

**Clarify**
- Users: the retention / marketing team (campaigns), call-centre agents (save offers), the network team.
- Which segment: prepaid vs postpaid, consumer vs business? Churn is defined differently (prepaid has no contract cancellation, just inactivity).
- Decision: whom to target with what offer, and what root causes to fix.
- Success: **incremental** retention (saved customers who would otherwise have left) minus offer cost, not raw churn prediction accuracy.

**Scope.** Postpaid consumers approaching contract end, with an uplift-based retention campaign.

**MECE breakdown.** (1) Who churns (prediction). (2) Why: price, network quality, service experience, competitor offers, moving house. (3) Who can be saved (uplift / persuadability). (4) What offer. (5) Structural fixes (network coverage, billing errors).

**Data & quality**
- Billing / CRM (tenure, plan, contract end, payments); usage (CDRs, data usage); network quality per customer (dropped calls, throughput from network probes).
- Customer care interactions (call transcripts, complaints); port-out requests (the churn label, with the competitor it went to).

**Solution**
- **v0:** churn drivers analysis and a simple propensity model; a list for the retention team of high-value customers near contract end with warning signs (e.g. complaints + network issues).
- **v1:** uplift model trained from a randomised campaign (treatment vs control) so offers go to persuadable customers, not "sure things" or "lost causes"; offer optimisation by margin.
- **v2:** root-cause fixes fed to other teams: coverage gaps hurting retention by cell site; billing shock alerts; LLM analysis of care transcripts.

**Risks.** Discounts given to people who would stay anyway (wasted margin); training customers to threaten to leave; fairness in offers; privacy of usage data (GDPR consent for marketing).

**Metrics & iteration.** Incremental retention vs control, net revenue saved, churn rate by segment. Always keep a holdout group.

### An insurance company wants to process claims faster.

**Clarify**
- Users: claims adjusters / handlers, the FNOL (first notice of loss) intake team, and customers.
- Which line: auto, property, health, workers' comp? Assume personal auto or home.
- Success: cycle time (FNOL to payment), customer NPS, loss adjustment expense per claim, leakage (overpayment), fraud caught.

**Scope.** Simple, low-severity claims (e.g. windscreen, minor auto damage) for straight-through or fast-track processing; complex claims stay with adjusters.

**MECE breakdown.** (1) Intake / FNOL. (2) Triage: complexity, fraud risk, coverage. (3) Assessment: damage estimation. (4) Decision and payment. (5) Communication with the customer and waiting time.

**Data & quality**
- Claims system (Guidewire ClaimCenter, Duck Creek): status history for process mining; policy data (coverage, deductibles).
- Photos, documents, repair estimates (CCC, Mitchell, Audatex); adjuster notes (free text); third-party data (police reports, weather for property claims).

**Solution**
- **v0:** process mining of status history: where time goes (often waiting for documents or adjuster assignment). Quick fixes: auto-request missing docs, better assignment rules.
- **v1:** triage model at FNOL (complexity and fraud score) routing simple claims to fast track; document extraction with OCR + LLM; photo-based damage estimate (e.g. Tractable) for adjuster approval.
- **v2:** straight-through processing with auto-payment for clearly covered, low-risk, low-value claims within authority limits.

**Risks.** Fraud exploits automation; regulatory fair claims handling rules (unfair claims practices acts); explainable denials; bias by geography.

**Metrics & iteration.** Cycle time, % fast-tracked, leakage audits on automated claims, NPS, adjuster capacity freed. Audit a sample of automated decisions monthly.

### A restaurant chain wants to reduce food waste.

**Clarify**
- Users: kitchen managers (daily prep and orders), the regional ops / supply team, the menu team.
- Where does waste happen: over-ordering (spoilage), over-prep (cooked but unsold), plate waste, or trim?
- Success: food cost % of sales, waste $ per store, without more stock-outs ("86'd" menu items) that lose sales.

**Scope.** Daily prep forecasting for the top 20 prepped items in 10 pilot stores. Over-prep is usually the largest controllable waste.

**MECE breakdown.** (1) Demand forecasting by item, store, daypart. (2) Prep planning. (3) Ordering and inventory (par levels, shelf life). (4) Waste measurement. (5) Menu design (ingredient overlap, portions).

**Data & quality**
- POS sales by item and hour; recipes / bill of materials; inventory counts (manual, weekly, error-prone); supplier orders.
- Waste logs (rarely kept; may need a tablet or a smart-scale like Winnow / Leanpath); weather, local events, promotions.

**Solution**
- **v0:** measure waste: a simple waste log on a tablet plus theoretical vs actual food cost by store (the gap is waste and shrink).
- **v1:** item-level forecasts by daypart (gradient boosting with weather and events) turned into a prep sheet for kitchen managers, editable; order suggestions from forecast and shelf life.
- **v2:** network-level menu engineering, cross-utilising ingredients; donation logistics for surplus.

**Risks.** Kitchen staff ignore the sheet if it is wrong on a busy day; food safety must not be compromised; stock-outs anger customers.

**Metrics & iteration.** Waste $ / % of food purchased, food cost %, stock-outs, prep sheet adherence. Pilot vs control stores for 8 weeks.

### A logistics company's warehouse is slow. Help them.

**Clarify**
- Users: the warehouse / shift manager, pickers and packers, the inventory control team.
- "Slow" how: order cycle time (order received to shipped), missed carrier cut-offs, units per labour hour?
- Success: on-time shipping %, units per hour (UPH), cost per order, order accuracy.

**Scope.** Measure the process first. Picking is usually 50–60% of warehouse labour cost, so start with pick travel time.

**MECE breakdown.** Inbound (receiving, put-away) / Storage (slotting) / Outbound (wave planning, picking, packing, shipping) / Labour planning / Systems (WMS logic, equipment).

**Data & quality**
- WMS (Manhattan, Blue Yonder, SAP EWM) task logs: scan events with timestamps per picker; order profiles; SKU velocity; location map; labour management system data.
- Idle time between scans is hard to attribute.

**Solution**
- **v0:** a time study from scan logs: travel vs pick vs idle by zone and hour; order arrival curve vs labour roster.
- **v1:** re-slotting: fast movers near pack-out (ABC velocity slotting), items commonly ordered together placed close; batch / cluster picking; labour forecasting per hour.
- **v2:** dynamic wave planning by carrier cut-off; automation business case (AMRs like Locus, goods-to-person).

**Risks.** Re-slotting disrupts operations, so do it gradually; worker surveillance concerns with productivity tracking (some US states regulate quotas, e.g. California AB 701).

**Metrics & iteration.** UPH, on-time ship %, travel distance per pick, accuracy. Before/after per zone.

### A company has data spread across 50 systems and nobody trusts the numbers. Help them.

**Clarify**
- Users: who suffers? Usually the exec team arguing about whose revenue number is right, plus analysts spending most of their time reconciling.
- Which decisions are blocked by distrust? Pick the 1–2 KPIs that matter most (revenue, customer count, inventory).
- Success: one agreed definition per key metric, a reconciled number that finance signs off, analyst time spent reconciling, and trust (do leaders use the dashboard in meetings?).

**Scope.** Do **not** try to integrate all 50 systems. Pick one high-value metric (e.g. monthly revenue by region) and trace it end to end through the 4–6 systems it touches.

**MECE breakdown.** (1) Definitions: what the metric means (business glossary). (2) Lineage: which systems feed it. (3) Integration: ingest, entity resolution (customer master). (4) Quality: tests, monitoring. (5) Ownership and governance. (6) Consumption: one dashboard.

**Data & quality**
- Typical sources: ERP (SAP / Oracle), CRM (Salesforce), billing, spreadsheets.
- Distrust usually has specific causes: different customer IDs, different revenue recognition timing, currency conversions, manual adjustments in Excel.

**Solution**
- **v0:** a metric definition workshop with finance and sales; trace one number; build an ELT pipeline (Fivetran / Airbyte → Snowflake / Databricks → dbt) with tests (unique, not null, reconciliation to the GL total); show where systems disagree and why.
- **v1:** a customer master (entity resolution); a semantic layer of certified metrics; data quality monitors (Great Expectations / dbt tests / Monte Carlo); a named owner per dataset.
- **v2:** extend domain by domain; data catalogue with lineage; self-serve.

**Risks.** Politics: owners of each system defend their numbers, so get an exec sponsor and finance as arbiter. Boil-the-ocean data lake projects that never deliver.

**Metrics & iteration.** Number of certified metrics adopted, reconciliation breaks over time, data test failures, time analysts spend reconciling, exec dashboard usage.

### A CEO says "we want to use AI." Where do you start?

**Clarify**
- Why now: board pressure, competitor moves, cost-cutting, or a specific pain? What did they try already?
- What are the company's top 3 business goals this year? AI must serve one of them.
- Success: a measurable business outcome within 90 days, not "an AI strategy deck".

**Scope.** Run a short discovery (2–3 weeks) to produce a ranked use-case portfolio, then deliver one use case to production in 8–12 weeks.

**MECE breakdown.** (1) Use-case discovery: interview department heads and front-line staff, map high-volume, repetitive, text-heavy or prediction-heavy work. (2) Prioritisation: value × feasibility (data available, risk level, integration effort). (3) Foundation: data access, security, approved LLM platform, governance. (4) Delivery of first use case. (5) Scaling: playbook, internal team, change management.

**Data & quality.** For each candidate: does the data exist, is it accessible, is it labelled? Text-based use cases (support, documents, internal knowledge) often have data ready; prediction use cases need history.

**Solution**
- **v0:** use-case inventory with value estimates (hours saved × cost, revenue impact), risk tier; pick a "lighthouse" with clear metric, willing business owner, low risk (e.g. support agent assist, document processing).
- **v1:** build it with evals and a baseline; measure real outcome; parallel workstream for safe enterprise LLM access (e.g. Claude via a governed gateway) for general productivity.
- **v2:** AI governance (policy, risk review, model inventory), a centre of excellence, and the next 3 use cases.

**Risks.** Solutions looking for problems; pilot purgatory (many demos, none in production); data security and leakage; employee fear, so communicate that AI augments work.

**Metrics & iteration.** Business KPI of the lighthouse (e.g. handle time), adoption, cost per task, number of use cases in production (not in pilot). Quarterly portfolio review with the CEO.

### A sales team wants to know which leads to prioritise.

**Clarify**
- Users: SDRs / account executives (who to call today), sales managers (territory and routing), marketing (which campaigns produce good leads).
- What is a "good lead": converts to opportunity? Closed-won? Revenue? Pick a target that has enough labels: closed-won may be too rare, so use "sales-qualified opportunity within 90 days".
- Success: conversion rate of worked leads, pipeline $ per rep hour, speed-to-lead.

**Scope.** Inbound leads for one product / region, scored and routed in the CRM.

**MECE breakdown.** (1) Fit: firmographic (industry, size, tech stack). (2) Intent: behaviour (website visits, pricing page, product sign-up). (3) Timing / engagement recency. (4) Routing and SLA. (5) Feedback from outcomes.

**Data & quality**
- CRM (Salesforce / HubSpot): leads, opportunities, outcomes. Data is messy: duplicates, stage hygiene, reps not logging activities.
- Marketing automation (Marketo); product usage (for PLG); enrichment (ZoomInfo, Clearbit); intent data (Bombora, G2).

**Solution**
- **v0:** clean up and a rules-based score agreed with sales (ICP fit + key behaviours); speed-to-lead SLA for top tier.
- **v1:** an ML model (logistic regression / gradient boosting) on historical conversions, with top reasons shown on the lead record; calibrate scores into tiers.
- **v2:** account-level scoring; next-best-action; LLM-generated research summaries per account for reps.

**Risks.** Self-fulfilling prophecy (low-scored leads never get called, so they never convert), so keep a random sample worked; rep distrust if no explanation.

**Metrics & iteration.** Conversion by score tier (should be monotonic), pipeline per rep, response time; A/B test routing; retrain quarterly.

### A company wants to reduce employee attrition.

**Clarify**
- Users: HR business partners, people managers, the CHRO.
- Which attrition: regretted voluntary attrition among high performers or critical roles? Early-tenure (first year) attrition?
- Decision: which teams need intervention, which policies to change (pay, career paths, managers).
- Success: regretted attrition rate, especially in critical roles; cost of turnover; engagement scores.

**Scope.** Aggregate driver analysis at team / segment level rather than individual flight-risk scores (individual scoring is ethically fraught and legally risky).

**MECE breakdown.** (1) Who leaves: segments. (2) Why: pay, manager, growth, workload, commute / remote policy. (3) When: tenure points, after reviews, after bonus payout. (4) Levers. (5) Measurement.

**Data & quality**
- HRIS (Workday / SuccessFactors): tenure, role, pay, promotions, manager, exits.
- Engagement surveys (Culture Amp, Glint); exit interviews (often not candid); pay benchmarks (Radford); performance ratings.

**Solution**
- **v0:** attrition analytics by segment, manager, tenure, pay position vs market; survival curves; key finding usually a few managers or a pay band out of market.
- **v1:** driver model (survival analysis) at aggregate level; manager dashboards with team-level engagement; targeted programmes (pay adjustment, career paths) tested.
- **v2:** NLP on survey comments for themes; stay interviews.

**Risks.** Privacy and trust: individual flight-risk scores shared with managers can lead to discrimination or self-fulfilling exits; GDPR / works councils in Europe; protected attributes.

**Metrics & iteration.** Regretted attrition by segment, time-to-fill, engagement, intervention effect vs comparison groups. Quarterly.

### A construction company keeps going over budget on projects.

**Clarify**
- Users: project managers, estimators (bid stage), the commercial / cost control team, executives.
- Over budget where: bad estimates at bid (structural), or cost overruns during execution (change orders, rework, delays, material price)?
- Success: cost variance at completion, margin erosion, early-warning lead time (detecting overrun at 30% complete, not 90%).

**Scope.** An early warning system for active projects: earned value metrics and a forecast at completion, for the current project portfolio.

**MECE breakdown.** (1) Estimating accuracy. (2) Scope changes / change orders. (3) Productivity: labour, rework, delays. (4) Materials and subcontractor costs. (5) Cost reporting and forecasting.

**Data & quality**
- Estimates and budgets; ERP job costing (Viewpoint, Sage, CMiC); schedules (Primavera P6, MS Project); change orders and RFIs (Procore); daily reports; timesheets.
- Cost coding is inconsistent across projects; committed costs lag actual.

**Solution**
- **v0:** portfolio cost dashboard: earned value (CPI, SPI), committed vs budget by cost code, pending change orders; flag projects with CPI below 0.95.
- **v1:** estimate vs actual analysis across completed projects by cost code to correct the estimating database; forecast-at-completion model; change-order tracking to recover costs from the client.
- **v2:** risk model at bid using project features; LLM analysis of contracts for risk clauses and RFIs.

**Risks.** PMs hide bad news, so make the tool help them, not punish them; data entry burden on site.

**Metrics & iteration.** Margin fade, CPI distribution, estimate accuracy, change order recovery rate. Monthly project reviews use the tool.

### A farm wants to increase crop yield.

**Clarify**
- Users: the farm owner / agronomist (planting, inputs, irrigation decisions), equipment operators.
- Which crop, how many acres, irrigated or dryland? Is the goal yield or profit? (Yield at any cost is not success: margin per acre is.)
- Success: yield per acre, input cost per bushel, profit per acre.

**Scope.** Variable-rate input management (seed, nitrogen) using zones from historical yield maps, in one field or crop.

**MECE breakdown.** (1) Soil: fertility, texture, drainage. (2) Water: irrigation, rainfall. (3) Inputs: seed variety, density, fertiliser, crop protection. (4) Timing: planting and harvest. (5) Pests and disease.

**Data & quality**
- Yield monitor data from combines (John Deere Operations Center, Climate FieldView): noisy, needs cleaning for edges and speed changes.
- Soil samples (sparse); satellite imagery (Sentinel-2 / Planet NDVI); weather; as-applied maps from equipment; soil moisture sensors.

**Solution**
- **v0:** clean multi-year yield maps; management zones (consistently high / low areas); compare to inputs applied.
- **v1:** variable-rate prescriptions by zone; in-season NDVI monitoring to flag stressed areas for scouting; irrigation scheduling from soil moisture and ET.
- **v2:** on-farm trials (strip trials of varieties and rates) analysed statistically; yield forecasting for marketing grain.

**Risks.** Weather variance swamps effects in a single year, so use trials and multiple years; data ownership with equipment vendors; overfitting.

**Metrics & iteration.** Yield and margin per acre by zone vs prior years and vs check strips; input cost per bushel. Each season is one iteration, so design trials carefully.

### A bank wants to reduce loan defaults.

**Clarify**
- Users: credit risk / underwriting policy team, underwriters (manual review), collections team.
- Which product: credit cards, personal loans, SME, mortgages?
- Levers: approve / decline and pricing at origination; limit management; early collections intervention.
- Success: default rate / expected loss at a given approval rate (the trade-off), risk-adjusted return, fairness metrics.

**Scope.** Early-delinquency intervention for existing personal loans: catching customers before they miss payments is often faster to deliver than changing origination models (which need regulatory validation).

**MECE breakdown.** (1) Origination: scorecards, affordability. (2) Account management: limits, pricing. (3) Early warning. (4) Collections strategy. (5) Monitoring and model governance.

**Data & quality**
- Application data; credit bureau (Experian, Equifax, TransUnion); internal transactions (salary deposits stopping is a strong signal); repayment history; open banking data; collections outcomes.
- Labels take 12+ months to mature; reject inference (no outcome for declined applicants).

**Solution**
- **v0:** early warning list: customers with income disruption or rising utilisation; proactive outreach with payment plans.
- **v1:** behavioural scoring model (gradient boosting with monotonic constraints, explainable reason codes) for collections prioritisation and contact strategy; champion/challenger test.
- **v2:** origination model upgrade with cash-flow data, through model risk management (SR 11-7) validation.

**Risks.** Fair lending (ECOA / Reg B, adverse action reasons required); disparate impact; proxies for protected classes; treating customers in hardship fairly (FCA Consumer Duty in UK).

**Metrics & iteration.** Roll rates (current to 30 to 60 days past due), cure rate, loss rate at constant approval, fairness metrics. Champion/challenger continuously.

### An e-commerce company has too many returns.

**Clarify**
- Users: category managers / merchandisers, the product content team, the returns / reverse logistics team, customer service.
- Which categories? Apparel return rates are often 20–40%; electronics much lower.
- Why: wrong size, not as described, damaged, changed mind, or fraud ("wardrobing").
- Success: return rate and return cost per order, **net** of lost sales (stricter policies can reduce conversion), customer satisfaction.

**Scope.** Size-related apparel returns in one category (e.g. women's jeans), often the largest reason.

**MECE breakdown.** Pre-purchase (product information, sizing, images) / Fulfilment (wrong item, damage) / Post-purchase (customer behaviour, policy, fraud) / Reverse logistics (cost of processing, resale value).

**Data & quality**
- Orders and returns with reason codes (customers choose the first option, so codes are unreliable); free-text return comments and reviews.
- Product attributes; customer size history; warehouse inspection outcomes; carrier damage data.

**Solution**
- **v0:** return rate dashboard by SKU, reason and customer; top-20 SKUs with abnormal return rates go to merchandising (often wrong size chart or misleading photo).
- **v1:** LLM classification of return comments and reviews into precise reasons ("runs small"); fit recommendation ("customers like you bought size M"); flag product pages to fix.
- **v2:** serial returner / fraud detection with policy tweaks; return routing to resale vs liquidation.

**Risks.** Policies that penalise legitimate customers; return fraud detection false positives; discrimination by body size or geography.

**Metrics & iteration.** Return rate, cost per return, conversion (must not drop), size-related return share. A/B test fit recommendations.

## D. AI-company decomposition prompts

### A customer wants to automate 50% of its customer support with an LLM. Plan it.

**Clarify**
- Users: end customers (get answers), support agents (handle what remains), the support ops leader (owns the metric).
- "50%" of what: tickets fully resolved without a human (containment), or 50% of agent time? Which channels: chat, email, voice?
- Success: automated resolution rate **with** CSAT at or above the human baseline and low re-contact rate within 7 days. Deflection that just annoys customers is not success.

**Scope.** Pull 3 months of tickets and cluster by intent. Usually 10–20 intents make up 60%+ of volume. Start with the top 3–5 intents that are informational or simple actions (order status, password reset, refund policy).

**MECE breakdown.** (1) Intent coverage: which intents to automate. (2) Knowledge: help centre, policies (accuracy and freshness). (3) Actions: tool / API access (look up order, issue refund within limits). (4) Handoff: escalation to a human with context. (5) Evaluation and monitoring. (6) Agent-assist for the rest.

**Data & quality**
- Helpdesk tickets (Zendesk, Salesforce Service Cloud, Intercom) with resolutions, macros, CSAT; knowledge base (often outdated or contradictory); order / account APIs.
- Old tickets contain PII, so redact before using them for evals.

**Solution**
- **v0 (2–4 weeks):** agent-assist first: the LLM drafts replies with citations from the knowledge base, and the agent edits and sends. This is safe, builds an eval set from edits, and finds knowledge gaps.
- **v1:** customer-facing bot for the top intents with RAG over the KB plus read-only tools (order status), clear escalation ("talk to a human") and a confidence threshold. Offline eval set of 300–500 real tickets graded on correctness, policy compliance and tone (LLM-as-judge calibrated with human labels).
- **v2:** write actions (refunds, address changes) with business-rule limits and confirmation; expand intents one by one, each gated on eval scores; voice channel.

**Risks.** Hallucinated policy (Air Canada was held liable for its chatbot's invented refund policy), so ground answers in the KB and refuse outside scope. Prompt injection via customer messages that trigger tools. PII. Agent job concerns, so involve the support team from day one.

**Metrics & iteration.** Automated resolution rate, CSAT for bot vs human, 7-day re-contact rate, escalation rate, cost per resolution, hallucination rate in sampled review. Weekly review of failed conversations, then fix the KB, prompts or tools.

### A law firm wants to use LLMs to review contracts. How do you scope it?

**Clarify**
- Users: associates and paralegals doing first-pass review; partners (sign-off, risk appetite); knowledge management team.
- Which task: due diligence on hundreds of contracts in M&A (find change-of-control, assignment, exclusivity clauses), negotiating the firm's own templates (redline against a playbook), or contract summarisation?
- Success: hours saved per matter, recall of risky clauses (missing one is the expensive error), partner acceptance.

**Scope.** M&A due diligence for one contract type (e.g. commercial supply agreements), extracting 10–15 defined clause types into a review table. This is bounded, high-volume and easy to evaluate.

**MECE breakdown.** (1) Document ingestion: PDFs, scans, OCR. (2) Clause extraction and classification. (3) Risk assessment against the firm's playbook. (4) Review UX: citations, the lawyer confirms. (5) Evaluation. (6) Security and confidentiality.

**Data & quality**
- Data room documents (scanned, poor-quality, amendments that override originals); the firm's playbook and precedent clauses; past diligence memos as gold labels.
- Client confidentiality: zero data retention, no training on client data, possibly a private deployment (AWS Bedrock / Vertex / Azure in the firm's tenancy).

**Solution**
- **v0:** a 50-contract gold set annotated by associates; LLM extraction with quoted text spans and page links; measure recall and precision per clause type versus the gold set.
- **v1:** a review table UI (contract × clause) where every cell links to the source text; the lawyer accepts or edits; edits become new eval data. Integrate with the DMS (iManage / NetDocuments). Alternatively buy (Harvey, Kira / Litera, Luminance) and focus on integration and playbook.
- **v2:** playbook-based redlining for the firm's templates; cross-document reasoning (amendments).

**Risks.** Missed clauses (low recall) are worse than false flags, so tune for recall and have humans review everything. Unauthorised practice / professional responsibility: lawyers remain accountable. Confidentiality and privilege. Hallucinated case law (attorneys have been sanctioned) if the scope creeps into legal research.

**Metrics & iteration.** Recall per clause type (target 95%+), review hours per contract, lawyer acceptance rate, write-offs on matters. Add clause types only once each passes eval.

### A bank wants an internal AI assistant for its 10,000 employees. What do you build first?

**Clarify**
- Who are the 10,000: tellers, call-centre staff, relationship managers, operations, engineers, compliance? Their needs differ greatly.
- What are the expensive, frequent questions today? Usually "what is the policy / procedure for X?", which ties up internal helpdesks.
- Constraints: data residency, model risk management, regulators, information barriers (e.g. research vs trading).
- Success: time to answer, helpdesk ticket deflection, adoption (weekly active users), zero data incidents.

**Scope.** First build a **policy and procedure Q&A assistant for one population** (e.g. 1,500 contact-centre and branch staff) over approved, public-to-employees documents, plus a governed general-purpose chat (drafting, summarising) for everyone on an approved enterprise platform.

**MECE breakdown.** (1) Platform: secure LLM gateway, SSO, logging, DLP. (2) Knowledge: document sources, ownership, freshness. (3) Access control: answers respect document permissions. (4) Use cases by persona. (5) Evaluation and model risk. (6) Change management and training.

**Data & quality**
- SharePoint / Confluence policies (duplicates, outdated versions; many conflicting); procedure manuals; product terms; helpdesk tickets with frequent questions.
- Permissions in Entra ID / AD groups. Customer data excluded at first.

**Solution**
- **v0:** gateway with audit logs and PII / DLP filters; RAG over 200–500 curated, owned documents with citations and "last reviewed" dates; eval set of 200 real questions from the helpdesk, graded by policy owners.
- **v1:** permission-aware retrieval across more sources; feedback buttons routed to document owners; usage analytics; expand to RMs (product info, meeting prep from CRM, read-only).
- **v2:** agentic workflows with system access (e.g. drafting credit memos, KYC summaries) under model risk governance; integration into Teams.

**Risks.** Data leakage across permission boundaries; outdated or conflicting policies giving wrong answers (fix content ownership); regulatory obligations (record-keeping of communications); over-reliance.

**Metrics & iteration.** Weekly active users, answer accuracy on the eval set and sampled production answers, helpdesk ticket deflection, thumbs-down themes, security incidents (zero). Monthly content gap review.

### A customer's RAG pilot works in demos but fails in production. How do you approach it?

**Clarify**
- "Fails" how: wrong answers, "I don't know" too often, slow, expensive, or users don't use it? Get 20–50 concrete failing examples.
- Who uses it and for what questions? Demo questions were probably written by the builders; real users ask differently (vague, multi-part, use internal jargon).
- Success: answer correctness on real questions, task completion, user adoption.

**Scope.** Do not rebuild. Build an **evaluation set from production logs** (100–300 real queries with correct answers from domain experts) and diagnose by stage.

**MECE breakdown of failure causes.** Each failing example goes into exactly one bucket:
1. **Content gap:** the answer is not in the corpus.
2. **Ingestion / parsing:** tables, PDFs, scans parsed badly; chunks split mid-thought.
3. **Retrieval:** the right chunk exists but is not in the top-k (vocabulary mismatch, acronyms, wrong filters, stale index).
4. **Generation:** the right context was retrieved but the model ignored it, mixed it up, or hallucinated.
5. **Query / UX:** ambiguous or multi-hop questions, missing conversation context.
6. **Non-functional:** latency, cost, permissions.

**Data.** Production query logs, retrieved chunks, final answers, user feedback. Make sure tracing is in place (Langfuse, LangSmith, Arize Phoenix); without it you cannot diagnose.

**Solution**
- **v0 (week 1):** add tracing; label 100 failures into the buckets; measure retrieval recall@k separately from answer correctness. Typically retrieval and parsing cause most failures.
- **v1 fixes by bucket:** better parsing (layout-aware, tables to markdown); chunking with headers and metadata; hybrid search (BM25 + embeddings) plus a reranker; query rewriting / acronym expansion; metadata filters (version, date, product); prompts that require citations and allow "I don't know"; fill content gaps with document owners.
- **v2:** regression eval in CI (every change runs the eval set); online feedback loop; monitoring for drift as documents change.

**Risks.** Overfitting to the eval set; fixing the wrong thing without measuring; stale indexes.

**Metrics & iteration.** Retrieval recall@k, answer correctness / faithfulness (LLM judge calibrated against humans), "I don't know" rate, thumbs-up %, latency P95. Report weekly progress per failure bucket to the customer.

### A customer wants an agent that automates procurement end to end. Break it down.

**Clarify**
- Users: requesters (employees who need things), procurement buyers, approvers / budget owners, AP (accounts payable), suppliers.
- "End to end" covers intake → sourcing → PO → receiving → invoice → payment. Which part hurts most? Typically intake chaos and invoice matching.
- Spend type: catalogue purchases, indirect services, direct materials? Systems: SAP Ariba, Coupa, Oracle?
- Success: cycle time from request to PO, buyer hours per PO, % spend under contract (maverick spend down), invoice exceptions.

**Scope.** Don't build a fully autonomous agent first. Start with **intake and triage of purchase requests**: conversational intake that produces a complete, policy-compliant request routed to the right path (catalogue, existing contract, new sourcing).

**MECE breakdown (by process stage).**
1. Intake: understand the need, check policy, capture required fields.
2. Sourcing: find existing contracts / catalogue items, or run RFQ with suppliers.
3. Approval: route by amount, budget, category.
4. PO creation in the ERP.
5. Receiving and invoice matching (3-way match: PO, receipt, invoice).
6. Payment and supplier management.

Plus cross-cutting: permissions, audit trail, exception handling, human approval gates.

**Data & quality**
- ERP / P2P system (POs, vendors, contracts, catalogues); procurement policy documents; past requests and approvals; invoices (PDF, need extraction); vendor master data (duplicates, outdated bank details).

**Solution**
- **v0:** intake assistant: chat in Slack / Teams → structured request with category, cost centre, matched contract; human buyer reviews. Read-only tools.
- **v1:** auto-create requisitions for catalogue and under-contract purchases below a threshold (e.g. $5k), with approval workflows unchanged; invoice extraction plus 3-way match exceptions explained for AP.
- **v2:** RFQ drafting and supplier comparison for buyers; negotiation support; more autonomy as accuracy is proven, always within spending limits and approval gates.

**Risks.** Payment fraud: never let the agent change vendor bank details or approve payments; prompt injection via supplier emails / documents; financial controls (SOX) require audit trails and segregation of duties; errors that create unwanted POs.

**Metrics & iteration.** Request-to-PO time, % of requests needing buyer rework, agent action accuracy (sampled audit), contract compliance, invoice exception rate. Expand autonomy step by step based on measured accuracy.

### A hospital wants AI to summarise patient records for doctors. What are the risks and steps?

**Clarify**
- Users: which doctors and which moment? E.g. a hospitalist admitting a patient with a 500-page history, an ED physician, or a specialist preparing for a referral visit.
- What decision does the summary support? A summary for admission is different from a discharge summary.
- Success: time to get oriented on a patient, clinically significant errors (omissions and fabrications) at near-zero, physician trust and usage.

**Scope.** Pre-visit summaries for one specialty clinic (e.g. new referrals to cardiology), read-only, where the doctor still has access to the full chart.

**MECE breakdown.** (1) Data access and assembly: notes, labs, meds, imaging reports. (2) Summarisation: structure per specialty. (3) Grounding: every statement linked to its source. (4) Review UX in the EHR. (5) Evaluation and safety monitoring. (6) Governance: privacy, regulatory, clinical sign-off.

**Data & quality**
- EHR via FHIR / Epic APIs: notes (copy-pasted, contradictory, outdated), problem lists (often not maintained), medication lists (reconciliation issues), scanned outside records (OCR errors).
- Temporal reasoning is hard: a resolved problem vs current one.

**Risks (the core of this question)**
- **Hallucination:** invented diagnoses, wrong medication doses.
- **Omission:** missing a critical allergy or a recent abnormal result. Often worse than hallucination because it is invisible.
- **Outdated info presented as current.**
- **Automation bias:** doctors stop checking the chart.
- **Privacy:** HIPAA, BAA with vendors, no training on PHI without approval, access logging.
- **Bias:** summaries that reproduce stigmatising language from notes.
- **Regulatory:** may be considered clinical decision support (FDA guidance); liability.

**Steps**
- **v0:** structured summary template designed with clinicians (problems, meds, allergies, recent results, key events) and generated with citations; offline eval on 100 charts where clinicians rate accuracy and omissions; safety-critical fields (allergies, meds) pulled from structured data, not generated.
- **v1:** silent pilot (clinicians compare summary vs chart, no reliance), then a live pilot with 10–20 doctors; in-EHR display with source links; feedback button for errors.
- **v2:** expand specialties after error rates are acceptable; continuous sampled audits.

**Metrics.** Clinician-rated error rate (fabrication, omission per summary), time-to-orientation, usage, reported incidents. Governance committee reviews monthly.

### A customer says the model is "hallucinating too much." What do you do?

**Clarify**
- Ask for examples: 20–50 concrete cases. "Hallucination" is used for many things: factual invention, wrong answer from correct docs, outdated info, formatting errors, or just answers the user disagrees with.
- Which use case and what is the cost of an error? Which model, prompt, retrieval setup?
- Success: an agreed, measurable error rate on a representative test set, below a threshold the business accepts.

**Scope.** Build a labelled eval set from their examples plus sampled production traffic, then classify the errors.

**MECE breakdown of causes.**
1. Knowledge not available to the model: no retrieval, or retrieval missed it, so it guesses.
2. Retrieved context is wrong or outdated.
3. The model contradicts or ignores the context (faithfulness failure).
4. The question is outside scope or ambiguous; the model answers instead of refusing or asking.
5. Task demands precise computation or lookups the model should not do from memory (numbers, dates).
6. Mislabelled "hallucinations" (actually correct, or a disagreement on policy).

**Solution**
- **v0:** measure baseline hallucination rate on the eval set by category.
- **v1 fixes by category:** ground with RAG and require citations; instruct and permit "I don't know"; lower temperature for factual tasks; use tools for calculations and lookups; structured outputs; a verification step (second pass checking every claim against the sources); better retrieval (see the RAG question); consider a stronger model.
- **v2:** guardrail checks in production (citation verification, groundedness scoring); UX that shows sources so users can check; human review for high-stakes outputs.

**Risks.** Over-correction: the model refuses too much and becomes useless. Track refusal rate together with error rate.

**Metrics & iteration.** Hallucination / faithfulness rate on the eval set, refusal rate, user-reported errors per 1,000 answers. Share a before/after report with the customer; add every new reported failure to the eval set.

### A customer wants to replace a team of 20 data-entry workers with AI. How do you approach it?

**Clarify**
- What exactly do the 20 people do? Shadow them for a day. Usually: read documents (invoices, forms, emails) → extract fields → validate against systems → enter into an ERP / CRM → handle exceptions.
- Volume, document variety, accuracy needed, and the cost of an error.
- What happens to the people? Redeploy to exception handling and quality checks? This affects adoption and should be decided by leadership up front.
- Success: cost and cycle time per document at equal or better accuracy than the human baseline (measure the human error rate too, often 1–4%).

**Scope.** Choose the highest-volume, most standard document type (e.g. supplier invoices) and aim for "human in the loop on exceptions", not 100% automation.

**MECE breakdown.** (1) Intake: email, scan, portal. (2) Classification: document type. (3) Extraction: fields and line items. (4) Validation: business rules, cross-checks with master data. (5) Entry: API or RPA into the system. (6) Exception handling: a human review queue. (7) Monitoring and QA.

**Data & quality**
- Historical documents with the entered values (ready-made labels if they can be paired); systems of record; document quality varies (scans, handwriting, multiple languages).

**Solution**
- **v0:** measure baseline (throughput, error rate per person). Build extraction with an LLM / vision model (or IDP tools like Azure Document Intelligence, AWS Textract) and run in **shadow mode** against human entries for 2–4 weeks to measure field-level accuracy.
- **v1:** straight-through processing for documents where all fields pass validation and confidence thresholds; others go to a review queue where humans correct pre-filled fields (much faster than typing).
- **v2:** expand document types; feed corrections back; team shrinks through attrition or moves to exceptions, vendor queries and QA.

**Risks.** Silent errors at scale (one wrong pattern repeated 10,000 times), so sample audits; system integration without APIs (RPA is brittle); morale and knowledge loss: these workers know the edge cases, so involve them.

**Metrics & iteration.** % straight-through, field-level accuracy, cost per document, cycle time, exception queue size. Raise automation thresholds as accuracy is proven.

### An enterprise has 5,000 internal documents and wants a Q&A bot by Friday. Plan it.

**Clarify (in one 30-minute call on day 1)**
- Who will use it on Friday and what are their top 20 questions? Is Friday a demo to leadership or a production launch?
- Where are the documents (SharePoint, Confluence, Google Drive), what format, and are there permission restrictions?
- Approved LLM / cloud? Any sensitive data?
- Success for Friday: a working bot that answers the top questions correctly with citations, for a pilot group.

**Scope.** Be honest: by Friday you can ship a **pilot** for 10–30 users over a curated subset with clear limitations, not a hardened enterprise system. Agree on that in writing.

**Plan**
- **Monday:** clarify; get access; collect 30–50 real questions with expected answers from 2–3 subject experts (this is the eval set); check documents for sensitivity.
- **Tuesday:** ingest. Parse PDFs / Word / HTML (e.g. Unstructured, Docling), chunk by section with titles and metadata, embed, index (pgvector, OpenSearch or a managed option like Azure AI Search / Bedrock Knowledge Bases). Exclude restricted documents if permissions cannot be enforced by Friday.
- **Wednesday:** retrieval + generation with citations and "I don't know"; run the eval set; fix the worst failures (parsing, chunking, hybrid search).
- **Thursday:** simple UI (Slack / Teams bot or web), SSO, logging, feedback buttons; re-run eval; pilot users test.
- **Friday:** launch to the pilot group with known limitations and a feedback channel; show eval results.

**MECE breakdown.** Content (which docs, freshness) / Ingestion / Retrieval / Generation / Interface / Security / Evaluation.

**Risks.** Permission leakage (biggest), stale or contradictory docs, scope creep, demo-only quality, so be explicit about what is a pilot.

**Metrics & iteration (week 2+).** Eval accuracy, thumbs up / down, unanswered question themes (content gaps), weekly active users. Week 2: permissions, more sources, incremental re-indexing.

### A customer wants to measure ROI of their AI deployment. How?

**Clarify**
- Which deployment, what was its intended outcome, and who wants the ROI (CFO for budget renewal? Business owner for expansion?)
- Was a baseline captured before launch? If not, reconstruct one from historical data or use a comparison group.
- Success: a defensible number the CFO accepts, plus leading indicators to manage it.

**MECE breakdown.**
- **Benefits:** (1) Cost: hours saved × loaded cost, but only real if capacity is redeployed or hiring avoided. (2) Revenue: conversion, retention, faster sales cycles. (3) Quality / risk: fewer errors, compliance, CSAT. (4) Speed: cycle time.
- **Costs:** model / API usage, infrastructure, licences, build and integration, maintenance and evals, human review, change management and training.

**Approach**
- **v0:** define the unit of value per use case (cost per ticket resolved, hours per contract reviewed); measure baseline vs current with a comparison: A/B or staggered rollout (team A gets the tool first), or difference-in-differences against a team not using it.
- **v1:** instrumented dashboard: adoption (active users, tasks completed with AI), task-level metrics (time, quality), and costs from billing (token spend per task).
- **v2:** quarterly ROI review that adds second-order effects and decides on scaling or stopping.

**Pitfalls to call out.** Self-reported time savings are inflated; "hours saved" that just become slack is not cash ROI; ignoring quality regressions; ignoring total cost of ownership (evals, maintenance).

**Metrics.** Net benefit / total cost, payback period, cost per unit of work, adoption rate, quality metrics held constant or improved.

## E. Abstract / product decomposition

### How would you build a product to help people find a parking spot?

**Clarify**
- Users: drivers in a dense city centre (the pain is there, not in suburbs). Secondary: parking operators and the city (pricing, enforcement).
- Which parking: on-street, garages, or both? Commuters (daily, predictable) or visitors (one-off, uncertain)?
- Decision: where to drive and whether to pay for a garage vs circle for street parking.
- Success: time to park (cruising time; studies suggest a large share of downtown traffic is cruising for parking), successful parks per suggestion, retention.

**Scope.** Off-street garages first: availability data exists and you can reserve (guaranteed spot). On-street prediction is harder and a v1.

**MECE breakdown.** (1) Supply: where spots exist, rules, price, hours. (2) Availability now. (3) Prediction of availability at arrival time. (4) Guidance / navigation and payment. (5) Two-sided: operators, cities.

**Data & quality**
- Garage occupancy feeds (operators, ParkWhiz / SpotHero APIs); city open data (meter locations, rules, sometimes sensor occupancy like SFpark).
- Payment-app transactions (ParkMobile: a proxy for occupancy); curb rules (often only on signs, so need to be digitised); users' own GPS (detect parking events from driving to walking transitions).
- On-street data is sparse and quickly stale.

**Solution**
- **v0:** search and reserve garage spots near a destination with price and walking time.
- **v1:** on-street probability heat map by block and time using payment transactions, user parking events and historical patterns; "drive here" routing that maximises probability of finding a spot.
- **v2:** city partnerships for sensor data and dynamic pricing; EV charging spots.

**Risks.** Location privacy; cold start in each city; dangerous app use while driving (voice / simple UI); stale data damages trust.

**Metrics.** Time to park (self-reported or GPS), prediction accuracy (spot found within N minutes), conversion to reservation, retention per city.

### How would you help a library decide which books to buy?

**Clarify**
- Users: collection development librarians with a fixed annual budget.
- Library type: public (community demand, equity) vs academic (curriculum and research support)?
- Decision: which titles, how many copies, print vs e-book vs audiobook, and what to weed (remove).
- Success: circulation per dollar spent, holds wait time, collection diversity and coverage of community needs, use of new purchases within 12 months.

**Scope.** For a public library system: copy-depth decisions for new popular titles (holds ratio), plus a demand signal for underserved topics.

**MECE breakdown.** (1) Demand: what people want (holds, requests, searches). (2) Supply: current collection, condition. (3) Cost: price per format, e-book licensing (often expires after 2 years or 26 loans). (4) Mission: diversity, local languages, underserved branches. (5) Weeding.

**Data & quality**
- ILS (integrated library system: Sierra, Polaris, Koha): circulation, holds, inventory.
- Catalogue searches with zero results; interlibrary loan requests; OverDrive / Libby e-book usage; publisher catalogues and pre-publication reviews; bestseller lists; community demographics by branch (languages spoken).

**Solution**
- **v0:** holds-to-copy ratio dashboard with automatic purchase suggestion (e.g. buy another copy for every 4 holds), and failed-search / ILL report showing demand not met.
- **v1:** demand forecasting for upcoming titles (author history, pre-pub buzz); format optimisation (print vs e-book cost per loan); branch-level allocation by local demand and language.
- **v2:** weeding recommendations (no circulation in 3 years, poor condition); collection gap analysis vs community demographics.

**Risks.** Popularity-only buying narrows the collection and neglects minority interests, so balance with mission goals; patron privacy (libraries are strict about reading records; aggregate only).

**Metrics.** Circulation per dollar, average holds wait time, percentage of new purchases circulated, ILL demand fulfilled, diversity of collection.

### How would you design a system to match volunteers to non-profits?

**Clarify**
- Users: two sides. Volunteers (want meaningful, convenient opportunities) and non-profit volunteer coordinators (need reliable people with specific skills at specific times).
- Type: skilled (pro bono: accounting, design) vs unskilled (food bank shifts)? One-off vs ongoing?
- Success: **completed** volunteer hours, not sign-ups (no-show rates are high), volunteer retention (second engagement), coordinator satisfaction.

**Scope.** Unskilled shift-based volunteering in one city, where no-shows and empty shifts hurt non-profits most.

**MECE breakdown.** (1) Supply: volunteer profiles, availability, location, skills, interests. (2) Demand: opportunities with requirements. (3) Matching and ranking. (4) Commitment and reminders (no-show reduction). (5) Feedback and retention. (6) Trust and safety (background checks for work with children).

**Data & quality**
- Volunteer sign-ups (self-reported, often sparse); opportunity listings (inconsistent formats); attendance records (coordinators may not record); existing platforms (VolunteerMatch, Idealist), Golden / Galaxy Digital for coordinators.

**Solution**
- **v0:** structured opportunity listings plus filters (location, time, cause); simple sign-up, calendar invites and SMS reminders (reminders alone reduce no-shows significantly).
- **v1:** ranking by fit (distance, availability overlap, interests) and reliability; coordinator dashboard to track fill rate and confirm attendance; suggest next opportunity after a completed shift.
- **v2:** skills-based matching for pro bono projects; corporate volunteering programmes as a demand channel.

**Risks.** Safeguarding vulnerable people; reliability scores that penalise people unfairly (e.g. those with unpredictable work schedules); concentration on popular non-profits.

**Metrics.** Shift fill rate, show rate, hours completed, volunteer 90-day retention, coordinator NPS.

### How would you help a university schedule its classes and rooms?

**Clarify**
- Users: the registrar's scheduling office, department schedulers, faculty (preferences), students (need conflict-free combinations of required courses).
- Decision: time slot and room for each section each term.
- Success: no hard conflicts; students can register for required courses (fewer time conflicts between required courses in the same programme); room utilisation and seat fill; faculty preference satisfaction; time to produce the schedule.

**Scope.** A conflict analysis and optimisation for one college's courses, roll-forward from last year's schedule with changes (universities rarely redo from scratch).

**MECE breakdown.** (1) Demand: enrolment forecasts per course, which courses students take together. (2) Resources: rooms (capacity, features, location), faculty availability. (3) Constraints: hard (no double-booking, capacity, accessibility) vs soft (preferences, spread of times). (4) Optimisation. (5) Change management with departments.

**Data & quality**
- SIS (Banner, PeopleSoft, Workday Student): course offerings, enrolments, waitlists; degree audit / programme requirements (to find course pairs that must not clash); room inventory (often outdated); faculty preferences (gathered by email).

**Solution**
- **v0:** clash analysis: course pairs commonly taken together that clash; rooms where enrolment exceeds or badly underfills capacity; utilisation by time of day (everyone wants 10am Tuesday).
- **v1:** an optimisation model (integer programming / constraint programming, e.g. OR-Tools, or commercial tools like Infosilem / Ad Astra) for room assignment first, then time slots, with schedulers able to lock assignments and re-solve.
- **v2:** enrolment forecasting from degree progress; student-level conflict minimisation.

**Risks.** Faculty politics; changes that break long-standing arrangements; data on accessibility needs.

**Metrics.** Required-course conflicts, seat fill %, room utilisation, waitlist size, faculty preference satisfaction, scheduling cycle time.

### How would you help a sports team scout players?

**Clarify**
- Users: scouts, the head of recruitment / sporting director, the coach (who must want the player). Which sport? Assume football (soccer).
- Decision: which players to watch in person, shortlist and buy, within a budget and given squad needs.
- Success: signed players who play minutes and perform, value for money (market value over time), scout time saved.

**Scope.** A shortlisting tool for one squad position (e.g. left back) across target leagues, based on the coach's playing style.

**MECE breakdown.** (1) Need: role profile from the coach. (2) Player pool: leagues covered. (3) Performance evaluation: event and tracking data, adjusted for league strength. (4) Non-performance factors: age, contract, wages, injury history, character. (5) Workflow: shortlist → live scouting → decision.

**Data & quality**
- Event data (Opta / Stats Perform, StatsBomb, Wyscout), tracking data (where available); video; contract and market data (Transfermarkt estimates, agents); injury history; scout reports (free text, subjective).
- League-strength adjustment is essential; small samples for young players.

**Solution**
- **v0:** a role-based player search: metrics per 90 minutes relevant to the role, filters (age, contract end, budget), percentiles vs league.
- **v1:** player similarity ("find players like X"), league-adjusted projections; scout report capture integrated, with LLM summaries of reports.
- **v2:** performance projections by age curves; injury risk; video tagging.

**Risks.** Over-reliance on stats that miss context (tactics, teammates); bias against leagues with poor data coverage; scouts feel replaced, so build it as their tool.

**Metrics.** Minutes played by signings, hit rate of signings (judged after 1–2 seasons), cost vs value, scout time per shortlist.

### How would you build a tool to track the spread of misinformation?

**Clarify**
- Users: who? Platform trust & safety team, fact-checkers, public health communicators, election officials, or researchers. Each needs different things. Assume a public health agency during an outbreak.
- Decision: which narratives to respond to, where, and how fast; whether to "prebunk".
- Success: time from emergence to detection of a harmful narrative, response before it peaks, reach of corrections. Tracking is not the goal; effective response is.

**Scope.** Narrative detection and tracking for one topic (e.g. vaccine safety) on 2–3 platforms with accessible data.

**MECE breakdown.** (1) Collection: sources and access. (2) Detection: identify claims, group into narratives. (3) Assessment: is it false (fact-check matching), harm, and spread velocity. (4) Attribution: coordinated inauthentic behaviour vs organic. (5) Response and measurement.

**Data & quality**
- Platform APIs (limited and expensive since 2023 for X / Reddit; Meta Content Library for researchers; TikTok research API); news, Telegram channels; fact-check databases (ClaimReview markup, Google Fact Check Tools API).
- Biased coverage (closed groups and WhatsApp are invisible); multilingual; images / video memes.

**Solution**
- **v0:** keyword and embedding-based monitoring of a topic; clustering of posts into narratives with daily volume trends and examples; match against known fact-checks.
- **v1:** LLM-based claim extraction and narrative labelling; velocity and reach alerts; analyst dashboard for triage; network analysis for coordinated accounts (shared links, posting synchronisation).
- **v2:** multimodal (OCR of images, video transcription); response tracking (did the correction reach the same audiences?).

**Risks.** Labelling contested or emerging science as "misinformation" (truth changes, e.g. early COVID guidance), so analysts decide; free speech and government overreach concerns; privacy of individuals; platform terms of service.

**Metrics.** Detection lead time, analyst precision (alerts judged relevant), response time, narrative reach before and after correction.

### Estimate how many X are in Y (Fermi questions), then build a tool to track it.

**How to answer.** Clarify the definition, break it into factors you can estimate, state assumptions with round numbers, sanity-check with a second method, then design the tracker. Worked example: **"How many electric cars are on the road in California, and build a tool to track it."**

**Clarify.** Battery-electric only, or plug-in hybrids too? Light-duty passenger vehicles registered and active. Assume BEV + PHEV.

**Estimate (top-down via the fleet)**
- Population about 39M; roughly 0.75 registered light-duty vehicles per person, so about 30M vehicles.
- New car sales about 1.8M per year. EV share of new sales roughly 5% around 2018, rising to about 20–25% recently.
- Sum EV sales over the last ~10 years: about 0.1 + 0.1 + 0.15 + 0.15 + 0.15 + 0.25 + 0.35 + 0.4 + 0.4 + 0.4 ≈ 2.45M cumulative sales. Subtract scrappage and cars that leave the state (many off-lease EVs are resold elsewhere; say 25–35%), giving **about 1.6–1.8M**, or about 5–6% of the fleet.

**Sanity check (bottom-up).** About 13M households, about 10–12% have an EV, some own two → about 1.5–1.7M. Both methods land in the same range. Then name a real check: California Energy Commission data shows cumulative ZEV sales of roughly 2M by the mid-2020s and a noticeably smaller on-road count, which is why the out-of-state adjustment matters (verify current figures).

**Tool to track it**
- **Users:** state energy planners, utilities (grid load), charging-network planners.
- **Data:** DMV registration snapshots (the authoritative count; quarterly, by ZIP code), new sales reports (CEC, Auto Innovators), utility EV rate enrolments and smart meter load signatures, charger network data (AFDC from the US DOE).
- **v0:** quarterly dashboard of registrations by ZIP, fuel type and model from DMV data, with growth trends.
- **v1:** join with chargers per ZIP to show charger-per-EV gaps; nowcasting between DMV updates from sales data.
- **v2:** forecast adoption by ZIP (income, housing type, incentives) to plan grid upgrades and charging.
- **Metrics:** forecast error vs next DMV release, use by planners in investment decisions.

The interviewer is checking structure, sensible assumptions and sanity checks, not the exact number. Say your units and round aggressively.

## F. Typical interviewer follow-ups / push-backs

Push-backs are a test of how you adapt, not proof that you are wrong. A good pattern: **acknowledge → restate what changed → adjust the plan out loud → say the trade-off.** The examples below use the hospital readmissions prompt (B) as a running case, so you can see how one plan bends under each push-back. They are illustrative, not real engagements: adapt them to your own experience.

### Who is the actual user, and what does their day look like?

**What they test:** end-user empathy. Do you design for a real person, or for "the hospital"?

**How to respond.** Name one primary user by role, then describe their day concretely: their tools, time pressure, and the exact moment your product is used.

**Example:** "The primary user is the transitional care nurse. She covers about 25 discharges a day across 3 units, works mostly in Epic, and starts at 7am with a huddle where discharges are planned. She has maybe 10 minutes per patient for discharge education and booking follow-ups. So my tool must be a worklist inside Epic that is ready before the 7am huddle, not a separate dashboard. The CMO is the buyer, but the nurse decides whether it is used."

**Tip:** separate the **buyer** (pays, owns the metric), the **user** (uses it daily) and the **affected party** (the patient). Ask to shadow the user in week 1.

### What's the first thing you'd build? Why that?

**What they test:** scoping judgment. Do you pick something small, valuable and learnable?

**How to respond.** Pick the thinnest end-to-end slice that (1) one real user uses in their real workflow, (2) produces a measurable result within weeks, and (3) builds the data foundation for later steps. Justify it in terms of value, feasibility and learning.

**Example:** "A daily ranked discharge worklist for heart failure patients using the simple LACE score, inside Epic. Why: it needs only data we already have, nurses can use it in week 2, and it starts logging which interventions were done, which is exactly the label data the ML model in v1 needs. I'd deliberately not start with the ML model, because without the workflow nobody acts on predictions."

### What if the data you assumed doesn't exist?

**What they test:** resourcefulness, and not stalling.

**How to respond.**
1. Ask whether it exists somewhere else under another name: a different system, a spreadsheet, paper forms, a vendor, or open data.
2. Look for a **proxy** (e.g. no social-needs data → use ZIP-level deprivation index; no tap-off data in transit → infer destination from next tap-in).
3. **Start collecting it** with the smallest possible change to someone's workflow (one dropdown in a form), and design v0 not to depend on it.
4. Re-scope to the part of the problem the available data supports.

**Example:** "If there's no record of which follow-up interventions were done, I'd add a one-click 'called / booked / declined' field in the worklist. Within 6 weeks we'd have the intervention data. Meanwhile the worklist works without it."

Also state the impact honestly: "Without it, we can predict risk but can't yet measure which intervention works."

### What if the data is 80% wrong/missing?

**What they test:** data reasoning and pragmatism.

**How to respond.**
- **Diagnose first:** which fields, which sources, and is it missing at random or systematically (e.g. one site never fills it in)? Systematic gaps are bias, not just noise.
- **Find the reliable 20%:** often one site, one time period or one field (e.g. timestamps from machines are fine, free text from people is not). Build v0 on that.
- **Use more reliable substitutes:** machine-generated data over manual entry (e.g. AVL GPS times instead of dispatcher-pressed status buttons).
- **Fix it at the source:** make data entry part of a workflow that benefits the person entering it; validation at entry; data owners.
- **Make quality visible:** show completeness in the product so users know how much to trust it.

**Example:** "If 80% of 'at scene' timestamps are missing, I'd derive arrival from GPS (the unit stops within 50 m of the address) and validate on the 20% that exist. And I'd show dispatch supervisors their own data quality, because their own numbers looking bad is motivating."

### The customer only has 2 weeks. What do you deliver?

**What they test:** ruthless scoping and expectation management.

**How to respond.** Agree on the decision the 2 weeks should support. Deliver one thin, useful, real thing plus a plan, not a polished demo on fake data.

**Typical 2-week plan:**
- Days 1–2: interviews with users, data access, define one metric and baseline.
- Days 3–8: build v0 on real data (joined dataset + one view or ranked list; no ML unless trivial).
- Days 9–10: put it in front of 3–5 real users, iterate.
- Days 11–14: handover: what works, what the data showed (often the most valuable output), measured baseline, and a costed v1 roadmap.

**Example:** "For readmissions in two weeks: a validated LACE-based worklist for one unit, a baseline readmission analysis showing which patient groups drive the rate, and a v1 plan. I'd say clearly that a validated predictive model isn't a two-week deliverable, and why."

### How would you know if your solution is working? What metric?

**What they test:** outcome thinking and experimental rigour.

**How to respond with three levels of metric:**
1. **Outcome (north star):** the business result, e.g. 30-day readmission rate for the cohort.
2. **Leading / behaviour:** are users acting on it? (percentage of flagged patients with a follow-up booked within 7 days).
3. **Model / system health:** calibration, precision@top-k, data freshness, uptime.
Plus **guardrails** that must not get worse: ED revisits, mortality, length of stay, nurse workload.

**Attribution:** how to know your tool caused the change. Use a comparison: randomised or stepped-wedge rollout across units, or difference-in-differences against a similar unit. Pre/post alone is confounded by seasonality.

**Example:** "If readmissions drop but follow-up bookings didn't change, the drop is probably not from our tool."

### What are the privacy / ethical concerns?

**What they test:** maturity and judgment, especially for Palantir-style public sector work.

**How to respond with a checklist, applied to the specific case (not generic):**
- **Data minimisation and access:** who sees what (role-based access, purpose limitation); audit logs; legal basis (HIPAA, GDPR, FERPA, data-sharing agreements).
- **Bias and fairness:** are labels or proxies biased (arrests ≠ crime, cost ≠ health need)? Test error rates by group.
- **Harm of errors:** what happens to a false positive (investigated, denied benefits) vs a false negative? Keep humans in the loop for consequential decisions, with appeal / due process.
- **Transparency:** can affected people understand and challenge the decision?
- **Feedback loops / mission creep:** the data used for help being used later for enforcement.
- **Security:** sensitive data concentration is a target.

**Example:** "For readmissions: the model must not use cost as a proxy for need, since that under-flagged Black patients in a well-known 2019 study. I'd audit flag rates and outcomes by race and payer and make sure the extra help goes to patients who would benefit, not only those who are already well supported."

### What happens when users don't trust your model's output?

**What they test:** empathy and adoption skills. Treat distrust as information.

**How to respond.**
1. **Find out why**: sit with them. Is the model wrong (then fix it), is it right but unexplained, or does it conflict with their experience, workflow or incentives?
2. **Explain:** show the top reasons per prediction and the underlying data, in their language.
3. **Prove it:** run it in shadow mode and review cases together; show its track record on their own past cases ("these 10 patients it flagged last month: 6 were readmitted").
4. **Let them override and capture why** (override reasons are your best feedback data).
5. **Start where the model clearly helps** (a decision support aid, not a replacement) and keep the human in charge.

**Example (adapt to your own experience):** "A nurse said the list flags patients she already knows are high risk. That's useful: the value is then in the patients she wouldn't have expected, so I'd highlight those, and add 'why flagged' reasons."

### The stakeholder disagrees with your priority. How do you handle it?

**What they test:** influence, listening, and not being stubborn or a pushover.

**How to respond.**
1. **Understand their reasoning:** what do they know that I don't (politics, a deadline, a past failure)? Their priority may be right.
2. **Make the trade-off explicit** with shared criteria: value, effort, risk, time to impact. Put both options side by side.
3. **Use data where possible:** "ICU readmissions are 3% of volume; heart failure is 25%."
4. **Find a combined path:** often you can do their priority in a way that also builds the foundation you wanted, or time-box a quick test.
5. **Disagree and commit:** they own the business decision. Write down the decision and what would make us revisit it.

**Example:** "The CMO wants the ICU first because of a recent bad event. I'd ask what happened, show the volume numbers, and propose building the shared data pipeline once, starting with a light ICU view to address the immediate concern, while the heart failure worklist ships next."

### How would this scale to 100 hospitals/cities/plants?

**What they test:** product thinking. Can you separate the reusable core from per-site configuration?

**How to respond.**
- **Standardise the data model** (an ontology / canonical model, e.g. FHIR for hospitals, a common asset model for plants) with per-site **connectors** that map local systems to it. Integration becomes the main per-site cost, so build connector templates for the common systems (Epic, Cerner, Meditech).
- **Configuration, not code:** thresholds, workflows and unit names set per site.
- **Models:** a shared base model with site-level calibration; monitor drift per site (different patient mix).
- **Operations:** automated deployment, multi-tenant or per-site isolation (data residency), monitoring, and support playbooks.
- **People:** an onboarding playbook and training so each site goes live in weeks, not months; site champions; benchmarking across sites becomes a new feature.

**Example:** "Site 1 takes 3 months, site 10 should take 3 weeks. I'd track time-to-value per new site as the scaling metric."

### What would make this project fail?

**What they test:** risk awareness and honesty. Do a pre-mortem.

**Common failure modes (name the 3–4 most relevant to your case):**
- **No adoption:** built outside the workflow, or users not involved.
- **Wrong problem:** optimised a metric nobody acts on.
- **Data access** delayed for months by legal / IT.
- **Trust lost** early from a visible error.
- **No owner** after the FDE leaves; nobody maintains it.
- **Scope creep / boil the ocean.**
- **Intervention capacity:** predictions without staff to act on them.
- **Ethical or legal incident.**

**Then say how you'd mitigate each:** early user involvement, a data access plan on day 1, shadow mode before go-live, a named business owner, written scope.

**Example:** "The biggest risk for readmissions is that the nurses have no time to act on the list. So I'd check capacity on day 1 and size the list to it (top 10 patients per day, not 100)."

### Which parts are generalisable into a product?

**What they test:** the FDE ↔ product feedback loop (central to Palantir, OpenAI, Anthropic FDE roles).

**How to respond.** Split what you built into three layers:
1. **Core platform (generalise):** connectors to common systems, the data model / ontology, entity resolution, the eval harness, the alerting and worklist framework, permissions and audit.
2. **Templates (configurable):** e.g. "readmission risk worklist" or "asset risk ranking", useful across customers in the same industry with configuration.
3. **Bespoke (keep custom):** local business rules, one-off integrations, site politics.

The test: **has a second customer needed it?** Build it the first time, notice it the second, productise it the third. Feed back to the product team with evidence (how many customers, how much time it would save).

**Example (adapt to your own experience):** "The Epic connector and the worklist UI were reused at the next two hospitals with only configuration, so those went to product. The specific nurse-call script stayed bespoke."

### Now add a constraint: no internet / no cloud / legacy mainframe. What changes?

**What they test:** adaptability and real-world deployment knowledge (defense, government, banks, hospitals).

**How to respond, constraint by constraint:**
- **No internet / air-gapped (defense, classified):** everything on-prem or at the edge; package with containers on Kubernetes (e.g. k3s / OpenShift) and ship via secure media transfer; open-weight models (Llama, Mistral, Qwen) served locally with vLLM, sized to the available GPUs; no SaaS APIs, so local vector DB (pgvector, Qdrant), local monitoring; offline updates and model refreshes; offline-first mobile apps that sync when connected (disaster response).
- **No cloud (policy):** run on the customer's data centre; capacity planning matters (no autoscaling); use the same stack on-prem for portability.
- **Legacy mainframe (banks, government, insurers):** do not replace it; integrate. Read via batch extracts (nightly files, COBOL copybooks to parse EBCDIC fixed-width records), change data capture (IBM CDC / Precisely), or MQ messages; write back via existing transactions (CICS) or a middleware API layer, or RPA as a last resort. Accept latency (daily data) and design v0 around it.

**Then say what shrinks:** smaller models, fewer real-time features, slower iteration cycles. And what you'd do first: confirm the hardware and the accreditation process (ATO) early, since it determines the timeline.

### How would you get adoption from users who resist change?

**What they test:** empathy and change-management skills, often the real FDE job.

**How to respond.**
1. **Understand the resistance:** fear for their job, past failed tools, extra work, the tool doesn't fit how they work, or they lose autonomy. Each needs a different answer.
2. **Co-design:** involve them from week 1; recruit a respected front-line champion; let them shape the tool.
3. **Remove work before adding any:** the first version should save them time (e.g. replace the morning spreadsheet), not add data entry.
4. **Fit the existing workflow:** inside the tools they already use, at the moment they need it.
5. **Quick wins and visible responsiveness:** fix their feedback within days and tell them you did.
6. **Keep them in control:** recommendations, not orders; easy overrides.
7. **Align incentives with leadership:** make sure managers measure what the tool helps, and don't use it to punish.
8. **Measure adoption** (weekly active users, % of recommendations acted on) and follow up on non-users personally.

**Example (adapt to your own experience):** "The crew chiefs ignored the fleet status board because they still had to email the morning report. Once the board generated the morning report automatically, everyone used it within two weeks."
