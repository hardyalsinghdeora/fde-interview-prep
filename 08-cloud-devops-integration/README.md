# 08 — Cloud, DevOps & Integration Questions

FDEs deploy into **the customer's** environment, which may be AWS/Azure/GCP, on-prem, air-gapped, or heavily locked down by IT and security.

---

## A. APIs & integration
- [ ] REST vs. GraphQL vs. gRPC vs. SOAP — when would you use each?
- [ ] Design a REST API for X (resources, verbs, status codes, pagination, versioning).
- [ ] HTTP status codes: 200, 201, 204, 301, 302, 304, 400, 401, 403, 404, 409, 422, 429, 500, 502, 503, 504.
- [ ] Idempotency: what is it, and how do you implement idempotency keys?
- [ ] Pagination: offset vs. cursor-based.
- [ ] Webhooks vs. polling vs. WebSockets vs. Server-Sent Events.
- [ ] How do you integrate with an API that has no docs?
- [ ] How do you integrate with a legacy system that only offers SFTP/CSV drops or a mainframe?
- [ ] How do you handle a flaky third-party API (retries, backoff, circuit breakers, timeouts)?
- [ ] How do you version an API without breaking customers?
- [ ] Synchronous vs. asynchronous integration — trade-offs.
- [ ] Message queues (SQS, RabbitMQ, Kafka, Pub/Sub) — when and why?
- [ ] How do you integrate with Salesforce / SAP / ServiceNow / Workday / Zendesk?
- [ ] iPaaS / integration tools (MuleSoft, Zapier, Workato) — when would you use one?

## B. Authentication, authorization & security
- [ ] Authentication vs. authorization.
- [ ] OAuth 2.0 flows (authorization code + PKCE, client credentials, device code).
- [ ] OIDC vs. SAML; SSO integration with Okta / Azure AD (Entra ID).
- [ ] JWTs — structure, validation, risks.
- [ ] API keys vs. OAuth tokens vs. mTLS.
- [ ] RBAC vs. ABAC vs. ReBAC.
- [ ] SCIM user provisioning.
- [ ] How do you store secrets (Vault, AWS Secrets Manager, KMS)?
- [ ] Encryption at rest vs. in transit; TLS handshake basics.
- [ ] OWASP Top 10 (injection, XSS, CSRF, SSRF, broken access control...).
- [ ] CORS — what is it and why does it fail?
- [ ] Compliance: SOC 2, HIPAA, GDPR, FedRAMP, ISO 27001, PCI-DSS — what do they mean for a deployment?
- [ ] The customer's security team blocks your deployment. How do you get it approved?
- [ ] How do you do a security questionnaire / vendor review with a customer?

## C. Cloud
- [ ] Core services you'd use on AWS/Azure/GCP for compute, storage, DB, queueing, networking.
- [ ] VPC, subnets (public/private), security groups, NACLs, NAT gateways.
- [ ] VPC peering vs. PrivateLink vs. VPN vs. Direct Connect / ExpressRoute.
- [ ] How do you connect your SaaS to a customer's private network securely?
- [ ] IAM: roles, policies, least privilege, cross-account access.
- [ ] Serverless (Lambda/Cloud Functions) vs. containers vs. VMs — trade-offs.
- [ ] Object storage (S3/GCS/Blob) — consistency, lifecycle policies, presigned URLs.
- [ ] Managed DBs (RDS, Aurora, DynamoDB, Cosmos DB, BigQuery, Snowflake) — when each.
- [ ] High availability and disaster recovery: multi-AZ, multi-region, RPO/RTO.
- [ ] Autoscaling strategies.
- [ ] Cost optimisation in the cloud.
- [ ] Deploying LLMs via Azure OpenAI / AWS Bedrock / Google Vertex AI — why do enterprises prefer these?

## D. Containers, orchestration & IaC
- [ ] What is Docker? Image vs. container. Write a Dockerfile for a Python service.
- [ ] Multi-stage builds; reducing image size.
- [ ] Kubernetes: pod, deployment, service, ingress, configmap, secret, statefulset, daemonset.
- [ ] How do you debug a pod in CrashLoopBackOff?
- [ ] Helm charts — what and why?
- [ ] Liveness vs. readiness probes.
- [ ] Horizontal Pod Autoscaler.
- [ ] Terraform: state, plan/apply, modules, drift.
- [ ] How do you deploy your product into a customer's Kubernetes cluster?
- [ ] Air-gapped deployment: how do you ship images, updates and models with no internet?

## E. CI/CD & operations
- [ ] Design a CI/CD pipeline for a customer-facing service.
- [ ] Blue-green vs. canary vs. rolling deployments.
- [ ] Feature flags — uses and risks.
- [ ] How do you roll back a bad deploy?
- [ ] Database migrations with zero downtime.
- [ ] Observability: logs vs. metrics vs. traces. Tools (Datadog, Prometheus, Grafana, OpenTelemetry).
- [ ] What SLIs/SLOs/SLAs would you define for a customer deployment?
- [ ] On-call and incident response — what's your process?
- [ ] How do you manage configuration across many customer deployments?
- [ ] How do you upgrade 50 customer deployments that are on different versions?

## F. Linux & scripting
- [ ] Common commands: `grep`, `awk`, `sed`, `find`, `xargs`, `curl`, `jq`, `ps`, `top`, `lsof`, `netstat/ss`, `df`, `du`, `tail -f`.
- [ ] How do you find which process is using a port?
- [ ] Disk is full — how do you find what's using it?
- [ ] File permissions and `chmod`/`chown`.
- [ ] Write a bash script to process logs.
- [ ] SSH tunnelling / port forwarding (getting into a customer's network).
- [ ] Cron jobs and systemd services.
