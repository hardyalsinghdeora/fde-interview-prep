# Azure

Many enterprise customers, especially in banking, government and large Indian and global corporates, standardise on Microsoft, so engineers with an AWS background are often asked whether they can work in Azure without a long ramp-up. Forward Deployed Engineer, Solutions/Software Architect and Agentic-AI Architect roles ask most, frequently because Azure OpenAI is the customer's approved LLM route.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [AI / LLM](../topics/06-ai-llm/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Explain the hierarchy: tenant, management groups, subscriptions, resource groups, resources.
- [ ] What is Entra ID (formerly Azure AD), and how does it differ from AWS IAM?
- [ ] Azure RBAC: role definitions, assignments and scopes. How does inheritance down the hierarchy work?
- [ ] Managed identities: system-assigned vs. user-assigned. What is the AWS equivalent?
- [ ] Map these to AWS: VNet, NSG, Blob Storage, Key Vault, Azure Monitor, Front Door.
- [ ] App Service vs. Container Apps vs. AKS vs. Functions: when is each the right default? *(verify)*
- [ ] Azure SQL Database vs. Azure Database for PostgreSQL vs. Cosmos DB: what each is for.
- [ ] Service Bus queues and topics vs. Storage Queues vs. Event Grid vs. Event Hubs.

## B. Working depth
- [ ] How do service principals, app registrations and managed identities relate to each other?
- [ ] Private endpoints vs. service endpoints in a VNet. Which one keeps traffic off the public internet?
- [ ] Key Vault: secrets vs. keys vs. certificates, and access via RBAC vs. access policies *(verify)*.
- [ ] AKS: node pools, workload identity, Azure CNI vs. kubenet, and how upgrades work *(verify)*.
- [ ] App Service deployment slots: how do you do a zero-downtime swap and roll back?
- [ ] Azure Functions hosting plans (Consumption, Premium, Flex, Dedicated): cold starts and VNet access *(verify)*.
- [ ] Cosmos DB consistency levels and partition keys. How do request units affect cost?
- [ ] Service Bus sessions, dead-lettering and duplicate detection: how do they compare with SQS FIFO?
- [ ] How do you write infrastructure as code for Azure: Bicep, ARM templates or Terraform, and why?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Azure OpenAI vs. calling a model provider directly: data residency, content filtering, quotas, private networking, why enterprises prefer it *(verify)*.
- [ ] Azure OpenAI throughput: tokens-per-minute quotas vs. provisioned throughput, and how you handle 429s at scale *(verify)*.
- [ ] Availability zones and paired regions in Azure: how do they differ from AWS AZs and multi-region design?
- [ ] Azure Policy and management groups as guardrails: how do they compare with AWS SCPs?
- [ ] A customer insists on Azure but your product runs on AWS. Port, run multi-cloud, or connect cross-cloud? Trade-offs.
- [ ] Conditional Access, MFA and SSO via Entra ID for an enterprise app: what does the customer's IT team control?
- [ ] Cost management in Azure: reservations, savings plans, dev/test pricing and the usual surprises *(verify)*.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design a RAG assistant on Azure for a bank: Azure OpenAI, a search index, Blob Storage, private endpoints, Entra ID permissions.
- [ ] Deploy your containerised Node.js service into a customer's Azure subscription where public endpoints are forbidden.
- [ ] Design an event-driven order pipeline on Azure using Service Bus, Functions and Azure SQL with retries and idempotency.
- [ ] Translate an AWS reference architecture (ALB, ECS, RDS, SQS, S3, CloudFront) into Azure and explain what changes.
- [ ] A customer's security team must approve your Azure deployment. What do you prepare for them?

## E. Hands-on checks (things to be able to DO)
- [ ] Use the Azure CLI to create a resource group, a container app or App Service, and assign a managed identity.
- [ ] Grant that identity read access to one Key Vault secret and one Blob container, and nothing else.
- [ ] Write a Bicep or Terraform file for a VNet, a private endpoint and an Azure Database for PostgreSQL.
- [ ] Call an Azure OpenAI deployment from code using Entra ID auth instead of an API key *(verify)*.
- [ ] Find a failed request in Application Insights and trace it to the dependency that caused it.

---

**Good answers show:**
- A clean AWS-to-Azure mental map, plus the places where the mapping breaks (identity model, resource groups, regions).
- Identity-first thinking: Entra ID, managed identities and RBAC scopes, not keys in config.
- Private networking by default for enterprise customers, and knowing what that costs in setup time.
- Realism about Azure OpenAI quotas, regional availability and content filters when designing LLM features.
- Saying plainly where your Azure experience is thinner and how you would close the gap quickly.

**Prove it:** Port a small service you already run on AWS to Azure in one or two evenings: Container Apps or App Service with a managed identity, Key Vault, Azure Database for PostgreSQL behind a private endpoint, and one Azure OpenAI call, all in Bicep or Terraform. Write a short AWS-to-Azure mapping note of what changed.
