# Terraform & infrastructure as code

Infrastructure as code is how serious teams make cloud environments reviewable, repeatable and auditable, so senior candidates are expected to have opinions on state, module design and safe change in CI. Platform, Staff/Principal backend and Forward Deployed Engineer roles ask most, since FDEs often have to stand up or extend infrastructure inside a customer's cloud account.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [System design](../topics/03-system-design/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What problem does infrastructure as code solve compared with clicking in the console or running scripts?
- [ ] Declarative vs. imperative IaC — where does Terraform sit, and where do Pulumi and CDK sit?
- [ ] What is Terraform state, what is in it, and why does it matter?
- [ ] Walk through `init`, `plan`, `apply` and `destroy`. What does `plan` actually compare?
- [ ] Providers, resources, data sources, variables, outputs and locals — what is each for?
- [ ] Remote state backends and state locking — why are they needed for a team?
- [ ] What is drift, and how do you detect it?
- [ ] What is a module, and when should you write one instead of copying resources?

## B. Working depth
- [ ] `count` vs. `for_each` — why does `count` cause resources to be destroyed when a list item is removed?
- [ ] How do implicit and explicit dependencies (`depends_on`) work, and when is `depends_on` a smell?
- [ ] Lifecycle settings: `create_before_destroy`, `prevent_destroy`, `ignore_changes` — when would you use each?
- [ ] How do you bring existing hand-made resources under Terraform (`import` blocks, `terraform import`)? *(verify)*
- [ ] Refactoring safely: `moved` blocks, `terraform state mv` and `state rm` — what are the risks? *(verify)*
- [ ] Structuring multiple environments: workspaces vs. separate directories/state per environment vs. a wrapper tool such as Terragrunt.
- [ ] Pinning provider and module versions, and how you upgrade a major provider version safely. *(verify)*
- [ ] Handling secrets: why secrets end up in state, and how you limit exposure (secret managers, sensitive values, encrypted backends).
- [ ] Running Terraform in CI: plan on pull request, saved plan files, manual approval and apply on merge.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you split state across a large estate (by layer, team, blast radius), and how do stacks share outputs safely?
- [ ] Designing a reusable module library: interfaces, versioning, opinionated defaults vs. flexibility, and avoiding "god modules".
- [ ] Policy as code: OPA/Conftest, Sentinel or cloud-native policies — what would you enforce at plan time, and what at the organisation level? *(verify)*
- [ ] CI identity and permissions: OIDC federation to AWS/GCP instead of static keys, and separating plan and apply roles. *(verify)*
- [ ] A plan shows a database will be replaced. How do you stop that class of accident reaching production?
- [ ] Terraform vs. Pulumi vs. AWS CDK vs. Crossplane — how would you choose for a given organisation? *(verify)*
- [ ] Licensing and ecosystem shifts (Terraform vs. OpenTofu): what would you consider before standardising? *(verify)*
- [ ] How do you combine IaC with Kubernetes and GitOps without two tools fighting over the same resources?
- [ ] Cost visibility in IaC: tagging/labelling standards and cost estimates on pull requests. *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A team has years of console-built AWS infrastructure. Plan the move to Terraform without downtime or surprise deletions.
- [ ] Someone changed a security group in the console during an incident and the next `apply` will revert it. What do you do now, and what process do you change?
- [ ] The state file is corrupted or a lock is stuck after a CI job was killed mid-apply. Walk through recovery.
- [ ] Design the repository layout, environments and pipeline for a product that runs in dev, staging and production across two clouds.
- [ ] You must deploy your product into fifty customer cloud accounts with slightly different settings. How do you structure the code and roll out changes?
- [ ] A security review asks for proof that no public S3 or Cloud Storage buckets can be created. How do you enforce and evidence it?

## E. Hands-on checks (things to be able to DO)
- [ ] Write a module for a VPC/network, a managed PostgreSQL instance and an object-storage bucket, with variables, outputs and sensible defaults.
- [ ] Configure a remote backend with locking and migrate local state into it.
- [ ] Set up a CI pipeline that posts the plan on a pull request and applies only after approval, authenticating via OIDC.
- [ ] Import an existing resource, then refactor it into a module with a `moved` block and a clean plan. *(verify)*
- [ ] Write one policy-as-code rule (for example "all buckets must block public access") and show it failing a plan.

---

**Good answers show:**
- Treating state as critical, sensitive data: remote, locked, encrypted, access-controlled and split by blast radius.
- A safe change process: small plans, review of every destroy/replace, approvals, and guardrails in CI.
- Module design that balances reuse with clarity, with versioned interfaces.
- Identity without static credentials and least-privilege roles for plan vs. apply.
- Pragmatism on tooling choice, tied to team skills and existing estate rather than fashion.
- Experience-based answers on drift, imports and recovery, not just the happy path.

**Prove it:** Codify a small but real stack (network, managed PostgreSQL, Redis, object storage with CDN, a container service) in Terraform for two environments, with a remote backend, a module library, a GitHub Actions or similar pipeline using OIDC, and two policy-as-code rules. Then deliberately cause drift and a risky replace, and document how the pipeline caught or handled each.
