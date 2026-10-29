# AWS

AWS is the default cloud for most product companies hiring senior backend and platform engineers, so interviewers use it to test whether you can turn requirements into a secure, cost-aware architecture rather than recite service names. Principal/Staff Engineer, Software Architect and Forward Deployed Engineer loops ask about it most, often inside a system design round.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [System design](../topics/03-system-design/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Regions vs. Availability Zones vs. edge locations: what fails independently, and what does that mean for your design?
- [ ] IAM users vs. roles vs. groups vs. policies. Why should workloads almost never use long-lived access keys?
- [ ] How is an IAM request evaluated? Explain explicit deny, identity policies, resource policies and permission boundaries.
- [ ] VPC basics: public vs. private subnets, route tables, internet gateway, NAT gateway.
- [ ] Security groups vs. network ACLs: stateful vs. stateless, and when you would touch NACLs at all.
- [ ] EC2 vs. ECS (Fargate or EC2) vs. EKS vs. Lambda: one sentence on when each is the right default.
- [ ] S3 storage classes and lifecycle rules. What is the consistency model today? *(verify)*
- [ ] SQS standard vs. FIFO; SNS; EventBridge. When do you reach for each?
- [ ] RDS vs. Aurora vs. DynamoDB: what kind of workload fits each?

## B. Working depth
- [ ] How do presigned URLs work for S3 uploads and downloads, and what are the security pitfalls?
- [ ] CloudFront in front of S3: origin access control, cache keys, invalidations vs. versioned object names.
- [ ] SQS visibility timeout, dead-letter queues and redrive. How do you make a consumer idempotent?
- [ ] Lambda cold starts, concurrency limits, reserved vs. provisioned concurrency, and timeouts *(verify)*.
- [ ] RDS Multi-AZ vs. read replicas: which solves availability and which solves read scale?
- [ ] DynamoDB partition keys, sort keys, GSIs and hot partitions. How do you model access patterns first?
- [ ] VPC endpoints (gateway vs. interface / PrivateLink): why use them for S3, SQS or Secrets Manager?
- [ ] Secrets Manager vs. SSM Parameter Store vs. KMS: what each is for.
- [ ] How do you give an ECS task or EKS pod its own IAM role?
- [ ] Autoscaling: target tracking vs. step scaling vs. scheduled; what metric would you scale an SQS worker on?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Walk through the six Well-Architected pillars and name one trade-off you have made between two of them.
- [ ] Multi-account strategy: why separate accounts per environment or team? Organisations, SCPs and a landing zone.
- [ ] How do you design for an AZ outage? What about a full region outage, and what RPO/RTO does each option buy?
- [ ] Your AWS bill grew 40% in a quarter. How do you find the cause and what are the usual culprits (NAT data transfer, idle capacity, logs, cross-AZ traffic)?
- [ ] Savings Plans vs. Reserved Instances vs. Spot: how do you decide the commitment level?
- [ ] Aurora failover: what does the application see, and how do connection pooling and DNS caching affect recovery?
- [ ] When does DynamoDB become the wrong choice, and what does migrating off it cost?
- [ ] EKS vs. ECS for a team of 10 engineers: operational cost, hiring, ecosystem, lock-in.
- [ ] Service quotas and soft limits: how have they bitten teams in production, and how do you plan for them?
- [ ] How do you detect and respond to leaked AWS credentials? GuardDuty, CloudTrail, Config, Security Hub.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design a media upload and delivery pipeline (direct-to-S3 upload, processing, CloudFront delivery) for 1M users.
- [ ] Design an order-processing backend using SQS/SNS/EventBridge that tolerates duplicate and out-of-order messages.
- [ ] A customer requires all traffic to stay private between their VPC and your SaaS. What are your options?
- [ ] Migrate a monolith on EC2 with a self-managed MySQL to managed services with minimal downtime.
- [ ] Design a multi-tenant SaaS on AWS: tenant isolation at the account, VPC, database and IAM levels.
- [ ] Design the account structure, network and guardrails for a 50-engineer company starting fresh.

## E. Hands-on checks (things to be able to DO)
- [ ] Write a least-privilege IAM policy that lets one service read one S3 prefix and send to one SQS queue.
- [ ] Write Terraform (or CDK) for a VPC with two private subnets, an ECS Fargate service and an ALB.
- [ ] Use CloudTrail or IAM Access Analyzer to find out why a call returns AccessDenied.
- [ ] Set up a CloudWatch alarm on SQS queue age and wire it to scale a worker service.
- [ ] Read a Cost Explorer breakdown and name the top three savings you would make.

---

**Good answers show:**
- Starting from requirements (latency, data sensitivity, team size, budget) before naming services.
- Explicit failure modes: what happens when an AZ, a queue consumer or a dependency fails.
- Cost as a design input, with rough numbers and the biggest line items called out.
- Security by default: roles not keys, private networking, encryption, least privilege, audit trail.
- Awareness of operational cost: who runs it at 3 a.m. and what the team can realistically support.
- Honest trade-offs about managed-service lock-in versus speed of delivery.

**Prove it:** Build a small file-processing service: direct-to-S3 upload via presigned URL, S3 event to SQS, a Fargate or Lambda worker with a DLQ, CloudFront for delivery, all in Terraform with least-privilege roles, then write a one-page cost estimate and failure-mode table.
