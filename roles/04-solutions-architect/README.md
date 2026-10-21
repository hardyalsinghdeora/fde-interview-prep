# Solutions Architect

A customer-facing architect who designs how a customer should build on a platform (a cloud provider or a vendor product), reviews their designs, and guides migrations and adoption, mostly through whiteboards, reviews and advice rather than writing their production code.

## The role

**Day to day.** Architecture sessions with customer engineering teams; whiteboarding reference architectures; Well-Architected or design reviews; migration and modernisation planning; cost-optimisation reviews; building proofs of concept and sample code; presenting recommendations to CTOs and engineering leaders; writing blog posts, reference architectures and workshops; and bringing customer requirements back to product teams. At cloud providers SAs are usually aligned to a set of accounts (by segment or industry) or to a specialty (data, security, networking, AI/ML, migration).

**Common titles.** Solutions Architect (SA), Cloud Solutions Architect (Microsoft), Customer Engineer (Google Cloud's pre-sales role), Partner Solutions Architect (works with consulting and technology partners), Specialist SA (e.g., data/analytics, generative AI), Enterprise Architect (customer-side or consulting), and at AI labs "Solutions Architect, Applied AI" or "Applied AI Engineer". At some vendors "Solutions Architect" means the same job as a pre-sales Solutions Engineer; see [03 Solutions Engineer](../03-solutions-engineer/README.md).

**Who hires for it.** Cloud providers (AWS, Microsoft Azure, Google Cloud, Oracle Cloud), data and infrastructure vendors (Databricks, Snowflake, MongoDB, Confluent, Elastic, HashiCorp, Stripe), AI labs (OpenAI and Anthropic have posted Solutions Architect / Applied AI roles), consulting firms and systems integrators (Accenture, Deloitte and AWS/Azure/GCP partners), and large enterprises with internal SA teams. Titles change, so read each job description.

**How it differs from FDE.**

| | Solutions Architect | Forward Deployed Engineer |
|---|---|---|
| Main output | Designs, reviews, recommendations, reference code | Production code and running systems |
| Who builds | The customer (or a partner) builds; SA guides | FDE builds alongside or for the customer |
| Breadth vs depth | Many customers, broad platform knowledge | Few customers, deep in their data and systems |
| Commercial link | Often pre-sales or consumption-growth aligned | Post-sale deployment and outcome |
| Typical artefacts | Architecture diagrams, ADRs, review reports, migration plans | Pipelines, integrations, apps, deployments |

**Who it suits.** Engineers with broad infrastructure and design experience who enjoy advising, teaching and presenting more than owning a codebase, and who like constant variety in problems.

**Skills mix.**

| Skill | Weight |
|---|---|
| Coding (sample code, POCs, IaC) | Med |
| System design / cloud architecture | High |
| Customer communication and presenting | High |
| Sales / commercial awareness | Med (High in pre-sales SA roles) |
| Platform breadth (compute, data, network, security) | High |
| Product / domain depth in one specialty | Med to High |

**Career paths.** Senior / Principal SA, Specialist SA in a domain, SA manager, Field CTO or chief architect, product management, consulting partner, or engineering leadership on the customer side.

## Typical interview loop

Typical reported shape; varies by company and level.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Motivation, background, travel, customer exposure | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| Hiring manager / technical phone screen | Breadth of cloud knowledge, a past architecture you designed, leadership-principle style questions | This file + [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/ANSWERS.md) |
| Architecture whiteboard (customer scenario) | Requirements gathering, reference architecture, trade-offs, HA/DR, cost, security | This file + [03-system-design](../../topics/03-system-design/ANSWERS.md) |
| Technical deep dive in a domain | Depth in networking, databases, data analytics, security, containers or AI/ML | [13-cs-fundamentals](../../topics/13-cs-fundamentals/ANSWERS.md), [07-data-and-sql](../../topics/07-data-and-sql/ANSWERS.md), [06-ai-llm](../../topics/06-ai-llm/ANSWERS.md) |
| Presentation round (commonly reported at AWS and others) | Present a past architecture or a prepared topic to a panel acting as a customer; handle challenge questions | This file (Presenting) |
| Behavioral (at AWS, structured around Leadership Principles; Amazon also includes a "Bar Raiser" interviewer) | Ownership, customer focus, diving deep, disagreement | This file + [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |
| Coding (some companies, especially AI labs) | Practical coding, API use, scripting | [02-coding](../../topics/02-coding/ANSWERS.md) |

## Common rounds — use the shared sections

- [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md): highest priority. VPC basics, connectivity options, IAM, managed databases, HA/DR, autoscaling, cost, Kubernetes, Terraform. This file goes beyond into multi-account, migration and Well-Architected.
- [03 System design](../../topics/03-system-design/ANSWERS.md): the method for any whiteboard; practise drawing with named managed services.
- [09 Customer scenarios](../../topics/09-customer-scenarios/ANSWERS.md): pushback, explaining trade-offs to executives, competitor comparisons, security reviews.
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md): prepare at least two stories per leadership principle you expect.
- [13 CS fundamentals](../../topics/13-cs-fundamentals/ANSWERS.md): networking (DNS, TCP, TLS, load balancing), databases, consistency.
- [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md): required for Applied AI / generative AI SA roles.
- [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md): for data/analytics specialist SA roles.

## Role-specific questions

### Role and customer engagement
- [ ] How is a solutions architect different from a solutions engineer and a forward deployed engineer?
- [ ] How do you earn the trust of a customer's CTO in the first meeting?
- [ ] How do you gather requirements before drawing any architecture?

### Reference architecture whiteboards
- [ ] Whiteboard: design a highly available web application on AWS for a retailer expecting 10x traffic on Black Friday.
- [ ] Whiteboard: design a serverless, event-driven order-processing pipeline.
- [ ] Whiteboard: design a cloud analytics platform for a customer leaving an on-premises Hadoop cluster.
- [ ] Whiteboard: design a multi-tenant SaaS architecture and explain silo, pool and bridge models.
- [ ] Whiteboard: design hybrid connectivity between a customer's data centres and the cloud.
- [ ] When would you recommend Kubernetes over a managed container service or serverless?

### Well-Architected and architecture reviews
- [ ] Name the six pillars of the AWS Well-Architected Framework and give a design principle for each.
- [ ] How do you run a Well-Architected review with a customer, and what do you deliver?
- [ ] How do the Azure and Google Cloud architecture frameworks compare with AWS's?
- [ ] Architecture review exercise: a customer runs its app and MySQL on one public EC2 instance, stores uploads in a public S3 bucket, and has access keys in the code. Critique the design.

### Migration and modernisation
- [ ] Explain the 7 Rs of migration and how you choose between them.
- [ ] Plan the migration of 500 virtual machines and a 20 TB Oracle database to the cloud.
- [ ] How do you migrate a production database with minimal downtime?
- [ ] A customer wants to lift-and-shift everything in three months and modernise later. What do you advise?

### Multi-account and landing zones
- [ ] Why use a multi-account strategy, and how would you structure organisational units?
- [ ] What goes into a landing zone?
- [ ] How do you implement guardrails with preventive and detective controls?
- [ ] How do you design networking across hundreds of accounts?
- [ ] How do you centralise identity, logging and security in a multi-account environment?

### High availability and disaster recovery
- [ ] Explain the four common disaster recovery strategies and their RTO, RPO and cost trade-offs.
- [ ] A customer needs an RPO of 15 minutes and an RTO of 1 hour for its core database. Design it.
- [ ] When is multi-region worth the cost and complexity compared with multi-AZ?
- [ ] How do you test disaster recovery and prove it works?

### Cost optimisation
- [ ] A customer's cloud bill has doubled in six months. How do you find and remove the waste?
- [ ] Compare Savings Plans, Reserved Instances and Spot Instances, and say when you would use each.
- [ ] How do you design for cost from the start and help a customer adopt FinOps practices?

### Security and compliance
- [ ] Explain the shared responsibility model and how you would design a HIPAA or PCI DSS workload in the cloud.
- [ ] Design encryption and key management for a regulated workload.
- [ ] A customer must keep EU personal data in the EU. How does that shape the architecture?

### Applied AI solutions architecture
- [ ] A customer wants to put an LLM application into production. Which architecture and decisions do you walk them through?
- [ ] How do you help a customer choose between prompting, RAG and fine-tuning?
- [ ] How do you estimate and control token cost and latency for an LLM workload?

### Presenting and influencing
- [ ] Presentation round: present an architecture to the customer's CTO in 15 minutes. How do you structure it?
- [ ] The customer's CTO disagrees with your recommended architecture. How do you handle it?
- [ ] How do you write an architecture decision record or design document for a customer?
- [ ] The customer wants a service your platform lacks but a competitor offers. What do you do?

### Leadership-principle style behavioral
- [ ] Tell me about a time you put the customer's long-term interest ahead of a short-term win.
- [ ] Tell me about a time you dove deep to find the root cause of a customer's problem.
- [ ] Tell me about a time you disagreed with a decision and then committed to it.
- [ ] Tell me about a time you simplified a customer's architecture or invented a new approach.

## Model answers

### Role and customer engagement

#### How is a solutions architect different from a solutions engineer and a forward deployed engineer?

- **Solutions engineer (SE):** pre-sales, paired with account executives, focused on the technical win for a specific deal: discovery, demos, POCs, RFPs. Success is closed revenue.
- **Solutions architect (SA):** designs how a customer should use the platform, often across the whole relationship (pre- and post-sale). Runs architecture and Well-Architected reviews, migration planning, cost and security guidance, and reference architectures. Success is often measured by customer adoption, consumption or workload launches, and customer satisfaction. At cloud providers SAs usually own technical relationships with a set of accounts.
- **Forward deployed engineer (FDE):** writes and ships production code inside or alongside the customer's environment to deliver a specific outcome.

A short way to say it: the SE proves it can work, the SA designs how it should work, the FDE builds it so it does work. The boundaries blur: some vendors call pre-sales engineers SAs, and many SAs build POCs. Mention how this company defines it, based on the job description.

#### How do you earn the trust of a customer's CTO in the first meeting?

- **Prepare:** read their public engineering blog, earnings or strategy statements, job postings (which reveal the stack), and any account history.
- **Lead with their goals, not your product:** "What are the two or three outcomes your team must deliver this year?"
- **Listen more than you talk;** reflect back their priorities precisely.
- **Show relevant expertise briefly:** one specific insight or pattern from similar companies, without naming other customers' confidential details.
- **Be honest about trade-offs and limits,** including where your platform is not the best fit. CTOs are used to being sold to; candour stands out.
- **Respect their team:** never criticise their current architecture in front of their engineers; ask why decisions were made.
- **Offer something useful immediately:** a follow-up architecture session, a relevant reference architecture, a specialist contact.
- **Follow through** on every commitment within the stated time. Trust is built on the second and third meetings.

#### How do you gather requirements before drawing any architecture?

Ask before drawing; state assumptions out loud if the interviewer won't answer.

- **Business:** what problem, for whom, what success looks like, deadline, budget constraints.
- **Functional:** main user flows, integrations, data sources and sinks.
- **Non-functional:** traffic (average and peak), data volume and growth, latency targets, availability target (e.g., 99.9% vs 99.99%), RPO/RTO, regions and users' locations.
- **Security and compliance:** data classification, regulations (HIPAA, PCI DSS, GDPR, data residency), identity provider.
- **Constraints:** existing tools and contracts, team skills (a small team favours managed and serverless services), on-premises dependencies, licensing (e.g., Oracle, Windows).
- **Operations:** who runs it, monitoring standards, release frequency.
- **Cost sensitivity.**

Then summarise: "So the priorities are availability and time to market, then cost," and design for those priorities explicitly. In the interview this takes about 5 minutes of a 45-minute session.

### Reference architecture whiteboards

#### Whiteboard: design a highly available web application on AWS for a retailer expecting 10x traffic on Black Friday.

Clarify: baseline and peak traffic, read/write ratio, regions, checkout availability target, team skills.

Design (AWS names; equivalent services exist on Azure and GCP):
- **Edge:** Route 53 DNS, CloudFront CDN for static assets and cacheable pages, AWS WAF and Shield for protection.
- **Compute:** Application Load Balancer across at least 3 Availability Zones; stateless app tier on ECS/Fargate, EKS or EC2 Auto Scaling groups. Scale on request count or CPU, and **pre-scale** (scheduled scaling) before the event; warm up capacity rather than relying only on reactive scaling.
- **Sessions:** externalised in ElastiCache (Redis) or DynamoDB.
- **Data:** Aurora (Multi-AZ) with read replicas for catalogue reads; ElastiCache for hot product data; DynamoDB for cart if very high write scale.
- **Decoupling:** order placement writes to SQS; workers process payment, inventory and email asynchronously, so spikes are buffered.
- **Static and media:** S3 behind CloudFront.
- **Operations:** CloudWatch dashboards and alarms, load testing at 10x+ beforehand, check service quotas early, runbooks, game day.
- **Degradation plan:** feature flags to turn off expensive features (recommendations) under load; queue-based waiting room if needed.

Mention cost: scale down after the event; Savings Plans for the baseline, on-demand or Spot for burst where safe.

#### Whiteboard: design a serverless, event-driven order-processing pipeline.

Flow (AWS example):
1. **API Gateway** receives `POST /orders`; a **Lambda** validates the request, writes the order to **DynamoDB** with status `PENDING` using an idempotency key, and returns 202.
2. **DynamoDB Streams** or an **EventBridge** event `OrderCreated` triggers the workflow.
3. **Step Functions** orchestrates the steps: reserve inventory, charge payment, confirm order, with retries, timeouts and compensating actions (release inventory if payment fails, i.e., a saga).
4. **EventBridge** publishes `OrderConfirmed`; consumers (email via SES, analytics to Kinesis Data Firehose to S3, shipping integration) subscribe independently.
5. **SQS** queues in front of slower or third-party integrations, with **dead-letter queues** and alarms.

Design points:
- At-least-once delivery means every consumer must be **idempotent**.
- Lambda concurrency limits and reserved concurrency to protect downstream systems.
- Observability: structured logs, X-Ray tracing, correlation IDs through events.
- Security: least-privilege IAM role per function, secrets in Secrets Manager.
- Trade-offs: pay-per-use and no servers to manage, but cold starts, execution time limits (Lambda max 15 minutes) and harder local testing.

#### Whiteboard: design a cloud analytics platform for a customer leaving an on-premises Hadoop cluster.

Clarify: data volume, sources, batch vs streaming needs, users (analysts with SQL, data scientists), existing Spark/Hive jobs, governance needs.

Target architecture (lakehouse pattern, AWS example):
- **Ingestion:** AWS DMS or CDC tools for databases; Kinesis or MSK (Kafka) for streams; AWS DataSync or Transfer Family for files.
- **Storage:** S3 as the data lake in zones (raw, cleaned, curated), using an open table format (Apache Iceberg, Delta Lake or Hudi) for ACID updates and time travel.
- **Processing:** EMR or AWS Glue for existing Spark jobs (minimal code change), or Databricks; orchestration with Airflow (Amazon MWAA) or Step Functions.
- **Catalogue and governance:** Glue Data Catalog plus Lake Formation for fine-grained access.
- **Serving:** Athena for ad hoc SQL, Redshift (or Snowflake) for warehouse workloads, QuickSight or the customer's BI tool.

Migration approach: move HDFS data to S3 (DataSync, or Snowball for very large offline transfers), port Hive metastore, re-point Spark jobs, run in parallel and reconcile outputs, then decommission. Key benefits: separate storage and compute, pay for what runs, no cluster capacity planning.

#### Whiteboard: design a multi-tenant SaaS architecture and explain silo, pool and bridge models.

- **Silo:** each tenant gets dedicated resources (separate database, stack or even account). Strongest isolation, easier per-tenant compliance and noisy-neighbour protection, simple cost attribution; but higher cost and operational overhead for many tenants.
- **Pool:** tenants share infrastructure; data separated by a `tenant_id` and enforced with controls (e.g., database row-level security, or IAM policies scoped by tenant). Most cost-efficient and easiest to operate at scale; needs careful isolation, throttling and per-tenant metering.
- **Bridge (hybrid):** mix by layer or by tier, e.g., shared app tier with separate databases, or pooled for standard customers and silo for enterprise customers who pay for it.

Core components regardless of model: tenant onboarding service, identity with tenant context in tokens (e.g., Cognito or Auth0 claims), tenant-aware routing, per-tenant rate limits and quotas, metering for billing, tenant-level observability, and automated deployment that updates all tenants.

Recommendation: pool by default for scale, silo for regulated or very large tenants, decided by isolation requirements, cost and number of tenants.

#### Whiteboard: design hybrid connectivity between a customer's data centres and the cloud.

Clarify: bandwidth, latency needs, availability target, number of VPCs/regions, encryption requirements, existing network vendor.

Options (AWS names; Azure ExpressRoute and Google Cloud Interconnect are equivalents):
- **Site-to-Site VPN** over the internet: quick to set up, encrypted, limited throughput and variable latency. Good for start or backup.
- **Direct Connect:** private dedicated connection with consistent performance. Lead time can be weeks. Not encrypted by default; add MACsec (on supported connections) or run VPN over it if required.
- **Resilience:** two Direct Connect connections at two different locations for high resilience, with VPN as a backup path.
- **Hub:** Transit Gateway (or Cloud WAN) to connect many VPCs and the on-premises network in a hub-and-spoke; Direct Connect Gateway to reach multiple regions.
- **Routing:** BGP, avoid overlapping CIDR ranges (plan IP addressing early with the network team, use IPAM).
- **DNS:** Route 53 Resolver inbound and outbound endpoints so on-premises and cloud names resolve both ways.
- **Inspection:** centralised inspection VPC with firewall appliances or AWS Network Firewall if required.

#### When would you recommend Kubernetes over a managed container service or serverless?

Choose the simplest platform that meets the requirements; Kubernetes is powerful but the customer pays for it in operations and skills.

- **Serverless functions (AWS Lambda, Azure Functions, Cloud Run functions):** event-driven or spiky workloads, small teams, fast time to market. Limits: execution time, cold starts, less control over the runtime.
- **Managed container service (ECS/Fargate, Cloud Run, Azure Container Apps):** long-running containerised services without managing a cluster's control plane, add-ons and upgrades. A good default for many web and API workloads.
- **Kubernetes (EKS, GKE, AKS):** when the customer needs portability across clouds or on-premises, already has Kubernetes skills and tooling, runs many services that benefit from its ecosystem (operators, service mesh, GitOps), needs fine-grained scheduling (GPUs, batch, mixed instance types), or is building an internal platform for many teams.

Questions to ask: team size and existing skills, number of services, portability or on-premises requirements, compliance, and who will run upgrades and security patching. A common recommendation is managed containers or serverless for most workloads, with Kubernetes only where its benefits are needed, and a managed Kubernetes service rather than self-managed clusters.

### Well-Architected and architecture reviews

#### Name the six pillars of the AWS Well-Architected Framework and give a design principle for each.

1. **Operational excellence:** perform operations as code; make frequent, small, reversible changes; learn from operational failures.
2. **Security:** implement a strong identity foundation (least privilege, centralised identity); apply security at all layers; enable traceability; protect data in transit and at rest.
3. **Reliability:** automatically recover from failure; test recovery procedures; scale horizontally; stop guessing capacity.
4. **Performance efficiency:** use managed and serverless services; go global in minutes; experiment more often; choose the right resource type for the workload.
5. **Cost optimisation:** adopt a consumption model; measure overall efficiency; stop spending on undifferentiated heavy lifting; analyse and attribute expenditure.
6. **Sustainability** (added in 2021): understand your impact; maximise utilisation; use managed services; reduce downstream impact.

Mention that the framework also has "lenses" for specific workloads (e.g., Serverless, SaaS, Machine Learning, Generative AI) and the free Well-Architected Tool for recording reviews.

#### How do you run a Well-Architected review with a customer, and what do you deliver?

1. **Prepare:** agree scope (one workload, not the whole estate), attendees (workload owner, engineers, security, operations), and a 2–4 hour session or a series of shorter ones.
2. **Set the tone:** it is a learning conversation, not an audit; no blame.
3. **Walk the pillars** using the framework's questions in the Well-Architected Tool, asking how they do things and asking for evidence (dashboards, runbooks, IAM policies).
4. **Record risks** as high or medium with notes; focus on the riskiest areas rather than every question.
5. **Prioritise together:** impact vs effort, aligned to their business goals.

Deliverables:
- A report of findings by pillar with high-risk issues (HRIs).
- An improvement plan: top 5–10 actions, owners, dates, with "quick wins" first.
- Links to reference architectures and the relevant lenses.
- A follow-up review date to measure progress (the tool records milestones).

Often the review uncovers projects where the SA can help further (a DR design, a cost review, a migration).

#### How do the Azure and Google Cloud architecture frameworks compare with AWS's?

All three cover the same core concerns with slightly different structure:

- **AWS Well-Architected Framework:** six pillars (operational excellence, security, reliability, performance efficiency, cost optimisation, sustainability), plus workload lenses and the Well-Architected Tool.
- **Microsoft Azure Well-Architected Framework:** five pillars (reliability, security, cost optimisation, operational excellence, performance efficiency), with an assessment tool and Azure Advisor recommendations. It sits alongside the Cloud Adoption Framework (CAF), which covers strategy, governance and landing zones.
- **Google Cloud Well-Architected Framework** (previously called the Architecture Framework): pillars covering operational excellence; security, privacy and compliance; reliability; cost optimisation; performance optimisation; and sustainability, plus cross-pillar "perspectives" for areas such as AI/ML (verify the current pillar list).

In an interview, say they are broadly equivalent: use the framework of the platform the customer runs on, and the principles (automation, least privilege, design for failure, measure cost) transfer. Check the current versions before the interview, as these frameworks are revised regularly.

#### Architecture review exercise: a customer runs its app and MySQL on one public EC2 instance, stores uploads in a public S3 bucket, and has access keys in the code. Critique the design.

Prioritise by risk, then propose fixes in phases.

**Critical (security):**
- **Access keys in code:** rotate and revoke them now, scan git history, use an IAM role on the instance (instance profile) instead; secrets in Secrets Manager.
- **Public S3 bucket:** turn on Block Public Access, serve uploads via CloudFront with origin access control or presigned URLs; enable encryption and versioning.
- **Database on a public instance:** move it to a private subnet; restrict security groups.

**High (reliability):**
- **Single instance, single AZ:** any failure means full outage and possible data loss. Move to an Auto Scaling group across multiple AZs behind a load balancer.
- **Database co-located with the app:** move to RDS or Aurora MySQL with Multi-AZ, automated backups and point-in-time recovery.

**Medium:** no monitoring/alarms (add CloudWatch), manual deployments (add CI/CD and IaC), no WAF, no cost tagging.

Present as: what we found, the risk in business terms, the fix, effort. Start with key rotation the same day.

### Migration and modernisation

#### Explain the 7 Rs of migration and how you choose between them.

- **Retire:** switch off applications nobody needs (discovery often finds a meaningful share).
- **Retain:** keep on-premises for now (recent hardware investment, compliance, hard dependency).
- **Rehost (lift and shift):** move VMs as they are, e.g., with AWS Application Migration Service. Fastest; little optimisation.
- **Relocate:** move a whole platform without changes, e.g., VMware workloads to a VMware-based cloud offering (check which are currently sold, as these changed after Broadcom's VMware acquisition).
- **Repurchase:** replace with SaaS (e.g., an on-premises CRM to a SaaS CRM).
- **Replatform (lift, tinker and shift):** small changes for managed benefits, e.g., database to RDS, app to containers on a managed service.
- **Refactor / re-architect:** redesign for cloud-native (microservices, serverless). Highest effort and benefit; reserve for business-critical apps that need agility or scale.

Choose per application using: business value and roadmap, technical fit and dependencies, licensing (e.g., Oracle or Windows), team skills, timeline drivers (data-centre exit date), and risk. Typical pattern: retire and rehost the long tail quickly; replatform or refactor the few applications where the business benefit justifies it.

#### Plan the migration of 500 virtual machines and a 20 TB Oracle database to the cloud.

Use a phased approach (AWS calls it assess, mobilise, migrate and modernise):

1. **Assess:** discovery tools to inventory servers, utilisation and dependencies; build the business case (TCO); decide the R for each application.
2. **Mobilise:** landing zone (accounts, network, identity, security baseline), hybrid connectivity, migration factory team and runbooks; pilot with 2–3 low-risk apps.
3. **Migrate in waves:** group VMs by application and dependency (things that talk a lot move together); 20–50 servers per wave; rehost with block-level replication tools (e.g., AWS Application Migration Service), right-size on the way; test, cut over in a maintenance window, keep rollback.
4. **Oracle database (20 TB):** decide between Oracle on RDS or EC2 (licence considerations) and conversion to PostgreSQL (schema conversion tool plus effort to rewrite PL/SQL). Initial load via physical backup/restore or Data Pump (Snowball for offline bulk if bandwidth is limited), then ongoing replication with CDC (AWS DMS, or Oracle GoldenGate/Data Guard) to keep in sync; cut over with a short downtime window after data validation.
5. **Operate and optimise:** monitoring, backups, cost optimisation, then modernise.

Governance: wave plan, success metrics, communication plan, and decommissioning on-premises servers to realise savings.

#### How do you migrate a production database with minimal downtime?

Pattern: **full load plus change data capture (CDC), then a short cutover.**

1. **Prepare target:** schema (converted if engines differ), sizing, parameters, security.
2. **Initial full load** from a consistent snapshot or native backup.
3. **Continuous replication** of changes using CDC (native replication such as MySQL binlog or PostgreSQL logical replication, or a tool like AWS DMS).
4. **Validate:** row counts, checksums, sample queries; run application tests against the target; monitor replication lag.
5. **Cutover:** put the app into read-only or briefly stop writes; wait for lag to reach zero; switch the connection string (ideally via DNS or config, not code); smoke-test.
6. **Rollback plan:** keep reverse replication or keep the source untouched for a defined period.

Watch for: large objects, sequences/identity columns, triggers, character sets, time zones, and application connection pools caching DNS. Downtime is typically minutes, limited to the cutover step. For zero downtime, use dual writes or blue-green database features where supported, at the cost of complexity.

#### A customer wants to lift-and-shift everything in three months and modernise later. What do you advise?

Support the speed goal but reduce the risks.

- **Understand the driver:** a data-centre contract end date or hardware refresh is a valid reason to rehost fast.
- **Agree that rehosting first is often right:** it reduces time in two environments, and modernising in the cloud is easier afterwards.
- **But challenge the plan in four areas:**
  - **Feasibility:** three months for a full estate is aggressive; validate with a discovery of server count and dependencies, and plan waves with buffer.
  - **Retire first:** shutting down unused apps reduces scope.
  - **Right-size during migration** instead of copying over-provisioned VMs, or cloud costs may be higher than on-premises.
  - **Foundations:** do not skip the landing zone, security baseline, backups and monitoring.
- **Quick replatform wins:** moving databases to managed services is often low effort and high value.
- **Make "later" real:** agree a modernisation backlog with owners and dates, funded by the savings.

Present a clear plan with risks and a decision point, and let the customer decide.

### Multi-account and landing zones

#### Why use a multi-account strategy, and how would you structure organisational units?

Accounts are a strong isolation boundary for security, quotas, billing and blast radius. Benefits: separate production from development, contain incidents, apply different controls per environment, clear cost attribution, and independent team autonomy.

Typical structure in AWS Organizations (based on AWS's published guidance; adapt to the customer):
- **Management account:** billing and organisation management only; no workloads.
- **Security OU:** log archive account (centralised, immutable logs) and security tooling/audit account (GuardDuty, Security Hub administrator).
- **Infrastructure OU:** shared networking (Transit Gateway, egress, inspection), shared services (directory, CI/CD).
- **Workloads OU:** with child OUs for production and non-production; one or more accounts per application or team per environment.
- **Sandbox OU:** experimentation with spending limits and fewer integrations.
- **Policy staging, Suspended** OUs for testing controls and closed accounts.

Apply controls (SCPs) at the OU level so new accounts inherit them. Use an account vending process so teams get compliant accounts on demand.

#### What goes into a landing zone?

A landing zone is a pre-configured, secure, multi-account environment that workloads land into. Components:

- **Account structure:** Organizations, OUs, account vending (AWS Control Tower Account Factory, or Account Factory for Terraform).
- **Identity:** single sign-on via IAM Identity Center federated with the corporate IdP (e.g., Entra ID, Okta); permission sets; no long-lived IAM users; break-glass access.
- **Guardrails:** SCPs and Control Tower controls (preventive, detective, proactive).
- **Logging:** organisation-wide CloudTrail and AWS Config to a central log archive account with restricted access.
- **Security services:** GuardDuty, Security Hub, IAM Access Analyzer, delegated to the security account.
- **Networking:** IP address plan, hub-and-spoke with Transit Gateway, centralised egress and inspection, DNS, hybrid connectivity.
- **Baselines:** encryption defaults, backup policies, tagging policies, budgets and cost alerts.
- **Automation:** all of it as code (CloudFormation/CDK or Terraform) with a pipeline.

Azure equivalents: Azure Landing Zones from the Cloud Adoption Framework, using management groups, Azure Policy and subscriptions.

#### How do you implement guardrails with preventive and detective controls?

- **Preventive** controls stop non-compliant actions before they happen:
  - SCPs (deny leaving the organisation, deny disabling CloudTrail, restrict regions, deny creating IAM users with access keys).
  - Permission boundaries for delegated IAM administration.
  - S3 Block Public Access at the account level.
  - IaC policy checks in the pipeline (e.g., CloudFormation Guard, Open Policy Agent, Checkov), and proactive controls (CloudFormation hooks).
- **Detective** controls find drift after it happens:
  - AWS Config rules and conformance packs, Security Hub standards (e.g., CIS, AWS Foundational Security Best Practices), GuardDuty threat findings, Access Analyzer.
- **Corrective / responsive:** automated remediation (Config remediation, EventBridge plus Lambda) and ticketing.

Principles: prevent the high-risk, well-understood things; detect the rest so teams are not blocked; test SCPs in a staging OU first; document exceptions with expiry dates; measure compliance over time.

Example SCP snippet (deny actions outside approved regions; real policies exempt global services):

```json
{"Effect": "Deny", "NotAction": ["iam:*", "organizations:*", "sts:*", "support:*"],
 "Resource": "*",
 "Condition": {"StringNotEquals": {"aws:RequestedRegion": ["eu-west-1", "eu-central-1"]}}}
```

#### How do you design networking across hundreds of accounts?

- **IP address management first:** non-overlapping CIDR plan across all regions, accounts and on-premises (Amazon VPC IPAM can allocate automatically).
- **Hub-and-spoke:** AWS Transit Gateway per region (or AWS Cloud WAN for global), with spokes for workload VPCs, shared across accounts with AWS RAM. Route tables segment environments (production cannot reach development).
- **Alternative: shared VPCs** (VPC sharing) where a central team owns the VPC and workload accounts deploy into shared subnets, reducing the number of VPCs.
- **Centralised egress:** NAT gateways and inspection (AWS Network Firewall or third-party) in a network account, rather than NAT in every VPC; trade-off is cost of data processing vs management.
- **Private access to services:** VPC endpoints, centralised where it makes sense; PrivateLink for service-to-service exposure without full network connectivity.
- **DNS:** Route 53 Resolver rules shared across accounts; private hosted zones.
- **Hybrid:** Direct Connect / VPN terminates on the hub.
- **Everything as code,** with network changes owned by a platform team.

#### How do you centralise identity, logging and security in a multi-account environment?

**Identity:**
- Federate the corporate identity provider to IAM Identity Center; manage access by groups and permission sets; SCIM provisioning.
- No IAM users with long-lived keys for humans; workloads use IAM roles; cross-account access via role assumption.
- Break-glass accounts with strong MFA and alerts on use.

**Logging:**
- Organisation CloudTrail (all regions, all accounts) and AWS Config to a central log archive account; S3 with Object Lock or restricted deletion, encryption with KMS.
- VPC Flow Logs, DNS query logs, load balancer logs sent centrally; application logs to a central observability platform or SIEM.

**Security:**
- Delegated administrator for GuardDuty, Security Hub, Macie, Inspector, Access Analyzer in the security tooling account; auto-enable for new accounts.
- Findings aggregated and routed to the SIEM and ticketing; automated responses for common findings.

Separation of duties: workload teams cannot alter or delete logs or disable security services, enforced by SCPs.

### High availability and disaster recovery

#### Explain the four common disaster recovery strategies and their RTO, RPO and cost trade-offs.

RPO = how much data you can lose (time since last recoverable point). RTO = how long until service is restored.

| Strategy | How it works | Typical RPO / RTO | Cost |
|---|---|---|---|
| Backup and restore | Back up data (and IaC) to another region; rebuild everything on disaster | Hours / hours to a day | Lowest |
| Pilot light | Core data replicated live (e.g., database replica) in the DR region; compute off or minimal; scale up on failover | Minutes / tens of minutes to hours | Low to medium |
| Warm standby | Scaled-down but fully working copy running in the DR region; scale up on failover | Seconds to minutes / minutes | Medium to high |
| Multi-site active/active | Full capacity serving traffic in two or more regions | Near zero / near zero | Highest |

Choose by business impact: classify applications into tiers, set RPO/RTO per tier with the business, and pick the cheapest strategy that meets them. Note that high availability (surviving an AZ failure, e.g., Multi-AZ) is different from DR (surviving a region-level event, a bad deployment, or data corruption). Data corruption and ransomware need point-in-time and immutable backups, which replication alone does not give.

#### A customer needs an RPO of 15 minutes and an RTO of 1 hour for its core database. Design it.

Clarify: engine, size, write rate, whether the threat is AZ failure, region failure, or corruption.

Design (AWS, relational):
- **In-region HA:** Aurora or RDS Multi-AZ; failover in about a minute or two for instance or AZ failure.
- **Cross-region DR:** a cross-region read replica, or Aurora Global Database (replication lag typically about a second). This meets a 15-minute RPO for regional events with margin. Pilot light or warm standby for the application tier in the DR region, deployed by IaC.
- **Corruption / accidental deletion:** automated backups with point-in-time restore (restorable to within about five minutes of the latest changes), plus AWS Backup copies to another region and account (with vault lock for ransomware protection).
- **Failover process:** documented, mostly automated runbook: promote replica, update DNS (Route 53 with health checks or the application's endpoint config), scale app tier, verify. Target well under one hour.
- **Monitoring:** alarms on replication lag versus RPO, backup job failures.
- **Test:** regular failover drills to measure actual RTO.

State cost: the replica and standby compute run all the time; compare with business cost of an hour's outage.

#### When is multi-region worth the cost and complexity compared with multi-AZ?

Multi-AZ (within one region) protects against single data-centre failures and is the default for production. It is cheap and simple because AZs are close enough for synchronous replication.

Multi-region adds protection against regional outages and can reduce latency for global users, but costs more: duplicated infrastructure, cross-region data transfer, harder data consistency (asynchronous replication or conflict handling), more complex deployments, testing and operations.

Multi-region is worth it when:
- The cost of a regional outage (revenue, safety, regulatory penalties) exceeds the extra spend.
- Regulations or contracts require geographic separation or resilience.
- Users are global and latency matters (active/active or read-local patterns).
- Data residency requires serving users from specific regions (which is a different reason: separate stacks per region).

Otherwise, multi-AZ plus backups copied to another region (backup-and-restore or pilot light) is often enough. Decide per application tier, not for the whole estate.

#### How do you test disaster recovery and prove it works?

- **Define what success means:** measured RTO and RPO against targets, and data integrity checks.
- **Test levels, increasing in realism:**
  - Backup restore tests (automated, for example weekly restore to a test account and validate).
  - Tabletop exercises of the runbook with the people on call.
  - Component failover tests (database failover, AZ evacuation).
  - Full regional failover drills in a planned window, ideally serving real traffic from the DR region.
  - Chaos engineering with fault injection (e.g., AWS Fault Injection Service) in pre-production, then carefully in production.
- **Automate the runbook** so failover is repeatable and not dependent on one expert.
- **Record results:** actual times, issues found (expired credentials, missing DNS records, quota limits in the DR region, config drift), and fix them.
- **Repeat on a schedule** and after major architecture changes.

A DR plan that has never been tested should be treated as not working.

### Cost optimisation

#### A customer's cloud bill has doubled in six months. How do you find and remove the waste?

1. **Find where it grew:** Cost Explorer (or Azure Cost Management, GCP billing reports) by service, account, region, usage type and tag; Cost and Usage Report for detail. Separate real business growth from waste.
2. **Common causes and fixes:**
   - **Idle or forgotten resources:** unattached EBS volumes, old snapshots, idle load balancers, unused Elastic IPs, dev environments running nights and weekends (schedule them off).
   - **Over-provisioning:** right-size with Compute Optimizer and utilisation metrics; move to newer instance generations or Graviton (ARM) where compatible.
   - **Data transfer:** cross-AZ chatter, NAT gateway processing (use VPC endpoints for S3/DynamoDB), internet egress (use a CDN).
   - **Storage:** S3 lifecycle policies and Intelligent-Tiering; gp2 to gp3 volumes; log retention limits in CloudWatch.
   - **Pricing model:** Savings Plans/Reserved Instances for steady baseline; Spot for fault-tolerant workloads.
   - **Managed service settings:** over-sized databases, provisioned capacity left high.
3. **Quick wins first,** then structural changes.
4. **Prevent recurrence:** tagging policy, budgets and anomaly detection alerts, cost owner per team, monthly review.

#### Compare Savings Plans, Reserved Instances and Spot Instances, and say when you would use each.

- **Savings Plans:** commit to a constant spend per hour (e.g., $10/hour) for 1 or 3 years in exchange for lower rates. **Compute Savings Plans** apply flexibly across EC2 instance families, regions, Fargate and Lambda; **EC2 Instance Savings Plans** give a deeper discount but are tied to an instance family in a region. Use for the steady baseline of compute.
- **Reserved Instances (RIs):** commitment to a specific instance configuration (Standard or Convertible) for 1 or 3 years. For EC2, Savings Plans are generally recommended now for flexibility. RIs (or reserved nodes) are still offered for services like RDS, ElastiCache, Redshift and OpenSearch, and AWS has added Savings Plans for some managed databases too (verify current coverage before advising). Zonal RIs also reserve capacity.
- **Spot Instances:** spare capacity at a large discount, but can be reclaimed with a two-minute warning. Use for fault-tolerant, stateless or flexible workloads: batch, CI, big data, containers with mixed instance types, some training jobs. Diversify instance types and use capacity-optimised allocation.
- **On-demand:** for spiky, short-lived or unknown workloads.

Typical approach: right-size first, then cover a share of the steady baseline with commitments, use Spot for suitable workloads, and leave the variable top on demand. Review coverage and utilisation monthly.

#### How do you design for cost from the start and help a customer adopt FinOps practices?

**Design for cost:**
- Choose managed and serverless services where usage is spiky or low (pay per use).
- Make cost a non-functional requirement: estimate cost per request or per customer (unit economics) in the design, using the pricing calculator.
- Autoscaling and scheduling; storage tiering and lifecycle from day one; minimise cross-AZ and egress data transfer; caching.
- Tag everything (team, application, environment, cost centre) and enforce with tag policies.

**FinOps practices** (the FinOps Foundation describes phases of inform, optimise, operate):
- **Inform:** visibility and allocation: dashboards per team, showback or chargeback, budgets and anomaly alerts.
- **Optimise:** right-sizing, commitments, Spot, removing waste; a regular recommendations review.
- **Operate:** cost ownership by engineering teams, cost checks in architecture reviews and CI (e.g., Infracost for Terraform), KPIs such as cost per transaction, and a cross-functional FinOps team (finance, engineering, product).

The cultural point: engineers make cost decisions daily, so give them the data and the responsibility.

### Security and compliance

#### Explain the shared responsibility model and how you would design a HIPAA or PCI DSS workload in the cloud.

**Shared responsibility:** the provider is responsible for security *of* the cloud (physical data centres, hardware, hypervisor, and the managed parts of managed services); the customer is responsible for security *in* the cloud (data, identity and access, configuration, OS patching on VMs, network rules, encryption choices, application code). The split shifts with the service: more customer responsibility on EC2, less on Lambda or S3.

**Designing a regulated workload:**
- **Contract and scope:** for HIPAA, sign the provider's Business Associate Addendum (BAA) and use only HIPAA-eligible services for protected health information. For PCI DSS, use services covered by the provider's PCI attestation and minimise cardholder data scope (tokenise via a payment provider where possible).
- **Isolation:** dedicated accounts for the regulated environment, segmented networks, no public endpoints for data stores.
- **Encryption:** in transit (TLS) and at rest with KMS customer-managed keys.
- **Access:** least privilege, MFA, just-in-time access, logging of all access.
- **Logging and monitoring:** CloudTrail, Config, GuardDuty, Security Hub with compliance standards, log retention as required.
- **Evidence:** AWS Artifact for provider reports; Audit Manager or similar for collecting control evidence.

Compliance is shared: the cloud being certified does not make the workload compliant.

#### Design encryption and key management for a regulated workload.

- **At rest:** encrypt everything (EBS, S3, RDS, DynamoDB, backups) by default using AWS KMS **customer-managed keys** for regulated data, so the customer controls key policy, rotation and audit. Enforce with SCPs or Config rules (e.g., deny unencrypted volumes).
- **Key separation:** separate keys per application, environment and data classification; keys in a dedicated security account where appropriate, with cross-account grants.
- **Key policies:** separate key administrators from key users (separation of duties); least privilege; condition keys to restrict which services can use the key.
- **Rotation:** automatic annual rotation (configurable) for KMS keys; rotate application secrets via Secrets Manager.
- **Audit:** every key use logged in CloudTrail; alarms on key deletion scheduling or policy changes.
- **Higher assurance:** CloudHSM or KMS custom key stores if regulations require single-tenant HSMs or customer control of key material; external key stores for keys held outside AWS (with availability trade-offs).
- **In transit:** TLS 1.2+ everywhere, ACM for certificates, mTLS for service-to-service where required.
- **Application-level:** envelope encryption or field-level encryption for highly sensitive fields (e.g., card numbers, national IDs), or tokenisation.

#### A customer must keep EU personal data in the EU. How does that shape the architecture?

Clarify the requirement first with their legal or privacy team: is it a legal obligation, a contract term or a preference; which data counts (personal data, backups, logs, support access); and whether access from outside the EU (for example by support staff) is allowed.

Architecture choices:
- **Region selection:** deploy workloads, databases and backups only in EU regions; enforce with preventive controls (for example an SCP or Azure Policy restricting regions) and detect drift.
- **Data flows:** map every place data goes: logs, monitoring, analytics, CDN caches, email and SaaS sub-processors, AI model endpoints. Each must be EU-hosted or receive only non-personal data.
- **Global services:** identity, DNS, CDN and some management services are global; check what data they store and where.
- **Encryption and keys:** customer-managed keys held in the EU; for stronger sovereignty requirements, external or customer-held keys, or sovereign-cloud offerings.
- **Access:** restrict operator and support access by location and role, with audit logs.
- **Multi-region users:** separate regional stacks (EU and non-EU) with data partitioned by user's home region, rather than one global database.
- **Contracts:** data processing agreements, sub-processor lists and transfer mechanisms (for example Standard Contractual Clauses) where any transfer happens.

Mention that residency (where data is stored) is not the same as sovereignty (who can be compelled to access it); the second may need sovereign-cloud options and legal advice.

### Applied AI solutions architecture

#### A customer wants to put an LLM application into production. Which architecture and decisions do you walk them through?

1. **Use case and success metrics:** what task, what quality is acceptable, cost per task, latency target, risk level.
2. **Model access:** direct API from the model provider or via their cloud (Amazon Bedrock, Azure, Google Cloud Vertex AI) for existing contracts, private networking, IAM and data residency. Model choice by quality on their evals, latency and cost; route easy tasks to smaller models.
3. **Context and knowledge:** prompt design, RAG over their documents (ingestion, chunking, embeddings, vector store, permissions-aware retrieval), tools/function calling for live data and actions.
4. **Application layer:** orchestration, conversation state, structured outputs with schema validation, streaming responses.
5. **Safety and governance:** input/output filtering, PII handling, prompt-injection defences (especially with tools), human approval for risky actions, audit logs.
6. **Evaluation:** offline eval sets before launch; online monitoring, user feedback, regression tests for prompt or model changes.
7. **Operations:** rate limits and retries, fallback models, caching (including prompt caching), cost dashboards, observability with traces of each call.
8. **Rollout:** pilot with a small group, measure, expand.

Details: [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md).

#### How do you help a customer choose between prompting, RAG and fine-tuning?

Start simple and move up only when evals show the need.

- **Prompting (with examples):** first step for almost every use case. Cheap, fast to iterate. Good when the model already has the knowledge and skill and you need to define format, tone or task.
- **RAG:** when the model needs knowledge it does not have or that changes often (internal documents, policies, product data), or when answers must cite sources and respect user permissions. Updating knowledge is just re-indexing.
- **Fine-tuning:** when you need consistent style, format or a narrow specialised behaviour that prompting cannot achieve reliably, or to make a smaller, cheaper model perform like a larger one on a narrow task. Needs high-quality labelled examples, an eval set, and a retraining process; it is not a good way to add frequently changing facts.

They combine: RAG plus a well-designed prompt covers most enterprise cases; fine-tuning is added for specific gains. Decide using an eval set on the customer's real data: measure quality, latency and cost for each option. Also consider tools/agents when the task needs live data or actions.

#### How do you estimate and control token cost and latency for an LLM workload?

**Estimate cost:** requests per day × (input tokens × input price + output tokens × output price). Example: 100,000 requests/day, 3,000 input tokens (system prompt plus retrieved context) and 300 output tokens each = 300M input and 30M output tokens a day (about 9B and 0.9B a month); multiply by the provider's current per-million-token prices. Include retries, agent loops (multiple calls per task), embeddings and evaluation runs.

**Control cost:**
- Use the smallest model that passes the evals; route by difficulty.
- Trim context: better retrieval (fewer, more relevant chunks), shorter system prompts, summarise long histories.
- Prompt caching for repeated prefixes (system prompts, documents), where the provider supports it.
- Batch APIs for non-urgent work, often at a discount.
- Cap output length; cache full responses for repeated questions.
- Per-user or per-tenant budgets and dashboards.

**Latency:** time to first token plus generation time (proportional to output tokens). Reduce with streaming, smaller models, shorter outputs, parallel calls, caching, and regions close to users. Measure p50 and p95 under realistic load and account for rate limits (provisioned throughput if needed).

### Presenting and influencing

#### Presentation round: present an architecture to the customer's CTO in 15 minutes. How do you structure it?

Many SA loops (commonly reported at AWS) include a presentation of a past project or a prepared scenario to a panel acting as the customer, with questions throughout.

Structure:
1. **Context and goal (2 min):** the business problem, constraints, and what success means. "You need to handle 10x peak traffic without increasing the operations team."
2. **Recommendation first (1 min):** the one-sentence answer, so the CTO knows where you are going.
3. **Architecture (5 min):** one clear diagram, explained as a request or data flow; highlight only the key components.
4. **Key decisions and trade-offs (3 min):** two or three choices, the alternatives you rejected and why (cost, risk, team skills).
5. **Risks, cost and timeline (2 min):** a cost estimate range, main risks with mitigations, phased plan.
6. **Ask (2 min):** the decision or next step you need.

Tips: speak in business terms, keep a backup slide for deep dives, pause for questions, say "I don't know, I'll find out" when true, and practise to time. If presenting a past project, be clear about your own contribution versus the team's.

#### The customer's CTO disagrees with your recommended architecture. How do you handle it?

1. **Listen and understand the reason:** "Help me understand your concern." The CTO may know constraints you don't (a team skill gap, a vendor contract, a past failure, a strategic direction).
2. **Restate their position fairly** to show you understood it.
3. **Find the decision criteria together:** cost, speed, risk, operability. Disagreements often come from weighting criteria differently.
4. **Compare options against those criteria with evidence:** data, benchmarks, references, a small prototype or proof test.
5. **Be clear about risks** of their preferred option in business terms, in writing if they are significant.
6. **Accept that it is their decision.** If their option is reasonable, commit fully to making it succeed; if it is unsafe (e.g., a security risk), escalate within your organisation and document the advice.
7. **Record it** in an architecture decision record so the reasoning is visible later.

It is fine to be persuaded. Changing your view when given new facts builds credibility.

#### How do you write an architecture decision record or design document for a customer?

**ADR (one decision, one page):**
- **Title and status** (proposed, accepted, superseded).
- **Context:** the problem, constraints and forces.
- **Decision:** what was chosen.
- **Options considered:** 2–4 alternatives with pros and cons.
- **Consequences:** trade-offs accepted, risks, follow-up actions.
- Date, decision-makers.

**Design document (whole solution):**
1. Executive summary: goal, recommendation, cost range, timeline.
2. Requirements: functional, non-functional (scale, availability, RPO/RTO, security, compliance), assumptions and out-of-scope items.
3. Architecture: diagram(s), components, data flow, integrations.
4. Key decisions (link ADRs) and alternatives.
5. Security, operations (monitoring, backups, DR), and cost estimate.
6. Implementation plan, risks and open questions.

Write for two audiences: executives read the summary; engineers read the details. Keep it versioned and reviewed with the customer.

#### The customer wants a service your platform lacks but a competitor offers. What do you do?

- **Understand the real need:** the outcome they want, not the product name. Often an existing service, a combination of services, or a partner product meets it.
- **Be honest:** if there is a real gap, say so. Do not claim equivalence that isn't there.
- **Offer options:** a native alternative or pattern; a marketplace or partner solution; running the open-source equivalent on managed infrastructure; or a hybrid/multi-cloud design where that component runs elsewhere, with its trade-offs (data transfer, latency, operations, security).
- **Quantify trade-offs** so the customer decides on facts.
- **Feed it back:** log the request with product teams, with the business impact; share roadmap only through approved channels and without commitments.
- **Keep the customer's interest first:** recommending the best solution for them, even when part of it is not yours, builds long-term trust.

### Leadership-principle style behavioral

#### Tell me about a time you put the customer's long-term interest ahead of a short-term win.

**What they look for (Customer Obsession at Amazon, similar values elsewhere):** you started from the customer's need, accepted a short-term cost (lower revenue, more work, a harder conversation), and the customer was better off.

**STAR outline:**
- **S:** [Customer, what they were asking for or about to do]
- **T:** [What a short-term win would have looked like, and why it was not right for them]
- **A:** [What you recommended, how you explained it, who you had to convince internally]
- **R:** [Customer outcome with numbers; effect on trust and the long-term relationship]

**Example (adapt to your own experience):** "A customer asked for a large, always-on cluster for a nightly batch job. Selling it would have increased spend, but their usage pattern showed it would sit idle about 20 hours a day. I proposed an on-demand job cluster using Spot capacity, which cut their projected cost by more than half. My account team was initially uneasy about the lower spend, but the customer moved two additional workloads to the platform within the year because they trusted our advice."

#### Tell me about a time you dove deep to find the root cause of a customer's problem.

**What they look for (Dive Deep):** you did not stop at the first explanation, you used data, you understood details at several layers, and you fixed the cause, not the symptom.

**STAR outline:**
- **S:** [Customer problem: latency spikes, intermittent failures, cost spike]
- **T:** [Your responsibility; why it mattered to the business]
- **A:** [Hypotheses, data you gathered (metrics, logs, traces, packet captures, billing data), what you ruled out, the root cause]
- **R:** [Fix, measured improvement, prevention (alerts, docs, product feedback)]

**Example (adapt to your own experience):** "A customer reported random timeouts after moving to the cloud. Application logs only showed generic connection errors. I correlated timestamps with load balancer and NAT gateway metrics and saw errors aligned with idle connections being dropped after a timeout. Their connection pool kept idle connections open longer than the network's idle timeout. We set keep-alives and pool idle timeouts below the limit; errors dropped to zero, and I wrote a short guide our team reused for other migrations."

See also [12 Debugging](../../topics/12-debugging-troubleshooting/ANSWERS.md).

#### Tell me about a time you disagreed with a decision and then committed to it.

**What they look for (Have Backbone; Disagree and Commit):** you raised the disagreement respectfully, with data, at the right time; once the decision was made you supported it fully and did not undermine it; you learned from the outcome.

**STAR outline:**
- **S:** [Decision being made, who made it]
- **T:** [Why you disagreed, what was at stake]
- **A:** [How you presented your view (data, alternatives), the decision, and how you then committed: what you did to make it succeed and how you reduced the risks you had flagged]
- **R:** [Outcome; what you learned; whether you were right or wrong]

**Example (adapt to your own experience):** "My team decided to standardise on a single Kubernetes-based platform for all customer POCs. I argued serverless would be faster for small POCs and showed setup-time data. The lead chose Kubernetes for consistency. I committed: I built a template that cut our cluster setup time, which reduced my main concern. Consistency did pay off in easier handovers, and I learned to weigh team-wide benefits more than my own speed."

#### Tell me about a time you simplified a customer's architecture or invented a new approach.

**What they look for (Invent and Simplify):** you saw unnecessary complexity or an unsolved problem, proposed a simpler or novel solution, and delivered measurable benefit.

**STAR outline:**
- **S:** [The existing architecture or process and its problems: cost, fragility, operational load]
- **T:** [What you were asked to do or decided to take on]
- **A:** [The simpler design or new approach, how you validated it (prototype, data), how you got buy-in]
- **R:** [Measured result: fewer components, lower cost, faster deployments, fewer incidents; reuse elsewhere]

**Example (adapt to your own experience):** "A customer ran a dozen VMs with cron jobs and custom scripts to move files between systems, and failures were found days later. I proposed replacing it with an event-driven flow: uploads to object storage triggered serverless functions, with a queue and dead-letter queue for failures and alerts on errors. A one-week prototype convinced their team. They retired the VMs, failures became visible within minutes, and the operations team stopped spending time on manual reruns."

## Prep checklist

- [ ] Read the AWS Well-Architected Framework pillar summaries (and the Azure or Google Cloud equivalent if relevant) and be able to name design principles for each.
- [ ] Practise five whiteboard scenarios out loud with named managed services, using a timer: requirements (5 min), diagram (15 min), trade-offs, HA/DR, security and cost (15 min).
- [ ] Learn the multi-account / landing zone reference (AWS Organizations, Control Tower, SCPs, IAM Identity Center, Transit Gateway) and draw it from memory.
- [ ] Memorise the 7 Rs, the four DR strategies with RTO/RPO, and the pricing models (on-demand, Savings Plans, Reserved, Spot).
- [ ] Prepare a 15-minute presentation of an architecture you designed, with one diagram, key trade-offs, cost and results; rehearse with someone who interrupts.
- [ ] For AWS roles, prepare two STAR stories for each Leadership Principle you expect (especially Customer Obsession, Dive Deep, Ownership, Earn Trust, Have Backbone; Disagree and Commit), using [10 Behavioral](../../topics/10-behavioral/ANSWERS.md).
- [ ] Do a hands-on build in a free-tier account: a multi-AZ web app with IaC, and a serverless pipeline, so your answers come from practice.
- [ ] Consider a relevant certification as structured study (e.g., AWS Certified Solutions Architect – Associate or Professional, or the Azure or Google Cloud architect equivalents); it is often valued but rarely required.
- [ ] For Applied AI SA roles, build a small RAG application with an eval set and a cost estimate, and review [06 AI / LLM](../../topics/06-ai-llm/ANSWERS.md).
