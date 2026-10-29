# Google Cloud (GCP)

Many data- and AI-heavy organisations run on Google Cloud, and interviewers want to see that an engineer strong on AWS can map concepts across and spot where GCP genuinely differs (projects and IAM, global networking, BigQuery, Pub/Sub). Forward Deployed Engineer, Agentic-AI Architect and platform-leaning Staff/Principal roles ask most, especially when customers bring their own GCP estate.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [AI / LLM](../topics/06-ai-llm/README.md) · [System design](../topics/03-system-design/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Explain the resource hierarchy: organisation, folders, projects and resources. How does it compare with AWS accounts and Organizations?
- [ ] IAM basics: principals, roles (basic, predefined, custom) and policy bindings. Why are basic roles discouraged?
- [ ] What is a service account, and how is it different from a user account?
- [ ] Compute choices: Compute Engine, GKE, Cloud Run, Cloud Functions, App Engine — when would you pick each?
- [ ] Map these AWS services to GCP: S3, CloudFront, SQS/SNS, RDS, DynamoDB, Lambda, ECS/Fargate, EKS, Redshift, Secrets Manager. *(verify)*
- [ ] What makes a GCP VPC different from an AWS VPC (global VPC, regional subnets)?
- [ ] Cloud Storage classes and lifecycle rules; signed URLs vs. IAM-based access.
- [ ] Cloud SQL vs. AlloyDB vs. Spanner vs. Firestore vs. Bigtable — the one-line use case for each. *(verify)*

## B. Working depth
- [ ] Workload Identity (GKE) and Workload Identity Federation (CI, other clouds): why they beat downloaded service-account keys. *(verify)*
- [ ] Cloud Run: concurrency per instance, cold starts, min instances, and how it scales on requests. *(verify)*
- [ ] Pub/Sub: push vs. pull subscriptions, acknowledgement deadlines, ordering keys, dead-letter topics and exactly-once delivery. *(verify)*
- [ ] Cloud SQL connectivity: private IP, the Auth Proxy or connectors, connection limits, and HA failover behaviour. *(verify)*
- [ ] BigQuery: partitioning and clustering, on-demand vs. capacity pricing, and how to keep a query from scanning a whole table. *(verify)*
- [ ] Load balancing: global external Application Load Balancer vs. regional options; Cloud CDN and Cloud Armor. *(verify)*
- [ ] Shared VPC vs. VPC peering vs. Private Service Connect — when would you use each? *(verify)*
- [ ] Secret Manager, Cloud KMS and CMEK — what does each protect, and who holds the keys?
- [ ] Cloud Logging, Monitoring and Trace: log sinks, log-based metrics and alerting policies.
- [ ] Vertex AI basics: model access through the Model Garden/Gemini APIs, endpoints, and when to use it instead of calling a model provider directly. *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Design a landing zone: folder structure per environment and team, organisation policies, billing accounts and audit logging.
- [ ] Spanner vs. AlloyDB vs. sharded Cloud SQL for a globally used transactional system — consistency, latency and cost. *(verify)*
- [ ] GKE Autopilot vs. Standard vs. Cloud Run for a microservices estate — control, cost and operational load. *(verify)*
- [ ] VPC Service Controls: what exfiltration risk do they address, and what tends to break when you turn them on? *(verify)*
- [ ] Cost control: committed-use and sustained-use discounts, BigQuery slot reservations, egress costs, budgets and labels. *(verify)*
- [ ] Multi-region and DR on GCP: regional vs. multi-regional services, RPO/RTO for Cloud SQL, Spanner and Cloud Storage. *(verify)*
- [ ] Pub/Sub vs. managed Kafka on GCP for an event backbone — ordering, replay, retention and ecosystem. *(verify)*
- [ ] Data residency and compliance for customers in India and the EU: region choice, CMEK, access transparency. *(verify)*
- [ ] Running a hybrid AWS + GCP estate: identity federation, networking between clouds, and avoiding duplicated platforms.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A customer runs on GCP and wants your SaaS deployed into their project. How do you design access, networking and upgrades?
- [ ] Migrate a Node.js + PostgreSQL + Redis + SQS + S3 + CloudFront stack from AWS to GCP. Map each piece, sequence the cut-over and name the risks.
- [ ] Design a RAG-based support assistant on GCP using Cloud Run, a vector-capable database and Vertex AI, with per-tenant data isolation. *(verify)*
- [ ] A BigQuery bill jumped tenfold last month. How do you find the cause and stop it recurring?
- [ ] Design an event-driven order pipeline with Pub/Sub, Cloud Run and Cloud SQL that tolerates duplicate deliveries.
- [ ] A Cloud Run service returns intermittent 5xx errors when calling Cloud SQL under load. Walk through the diagnosis.

## E. Hands-on checks (things to be able to DO)
- [ ] Use `gcloud` to create a project, a least-privilege service account and IAM bindings for one service.
- [ ] Deploy a container to Cloud Run with a private Cloud SQL connection, Secret Manager secrets and min instances set.
- [ ] Set up Workload Identity Federation so a CI pipeline deploys without a service-account key. *(verify)*
- [ ] Create a Pub/Sub topic with a push subscription to Cloud Run and a dead-letter topic, and test redelivery.
- [ ] Load a dataset into a partitioned, clustered BigQuery table and show the bytes-scanned difference between two queries.

---

**Good answers show:**
- Fluent AWS-to-GCP mapping, plus the real differences (projects, global VPC, IAM model, BigQuery economics) rather than a one-to-one table.
- Identity first: service accounts per workload, no long-lived keys, organisation policies and least privilege.
- Choosing the simplest managed service that fits (often Cloud Run) and saying when GKE is justified.
- Cost literacy: egress, query scanning, idle instances, and discounts tied to usage patterns.
- Awareness of enterprise controls (VPC Service Controls, CMEK, data residency) that customer security teams will raise.
- Clear failure-mode and DR reasoning per service, not just "it's managed".

**Prove it:** Deploy a small multi-service app on GCP end to end with Terraform: Cloud Run services, Cloud SQL over private IP, Pub/Sub between services, Secret Manager, a BigQuery export of events and a budget alert, with CI deploying through Workload Identity Federation. Write a one-page AWS-to-GCP mapping from the build, noting every place the mapping was not one-to-one.
