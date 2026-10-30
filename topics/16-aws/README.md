# 16 — AWS

AWS service questions come up in Solutions Architect and Solutions Engineer loops at AWS itself and at AWS partners (consultancies, AWS Professional Services, the partner teams in [roles/16](../../roles/16-partner-solutions-engineer/README.md)). They also appear in FDE, customer engineer and implementation loops at companies whose customers mostly run on AWS: AI labs that sell through Bedrock, data platforms (Databricks, Snowflake) and enterprise SaaS vendors. Expect them in technical screens, "how would you build this on AWS" whiteboards and troubleshooting rounds.

This file covers the **service level**: how specific AWS services behave, their limits and the trade-offs between neighbouring services. **Architecture-level AWS questions are covered elsewhere and not repeated here:** VPC design, connectivity options, IAM basics, serverless vs. containers, HA/DR, autoscaling, cost optimisation and Bedrock vs. Azure OpenAI vs. Vertex are in [topic 08 section C](../08-cloud-devops-integration/README.md#c-cloud). Well-Architected reviews, the 7 Rs of migration, landing zones and SCP guardrails, DR strategies, Savings Plans vs. RIs vs. Spot, shared responsibility, KMS for regulated workloads, data residency and reference architecture whiteboards are in the [Solutions Architect role file](../../roles/04-solutions-architect/README.md#role-specific-questions). Answers link to those where they overlap.

> AWS changes fast. Limits, prices and product names marked *(verify)* in the answers should be checked against current AWS documentation before you rely on them.

Answers: [ANSWERS.md](ANSWERS.md)

---

## A. Compute
- [ ] What are the EC2 instance families and purchase options, and how do you choose?
- [ ] What are AMIs, user data and launch templates, and how do they fit together?
- [ ] How do Auto Scaling groups work: scaling policies, health checks, lifecycle hooks and warm pools?
- [ ] ECS vs. EKS vs. Fargate vs. App Runner — when would you use each?
- [ ] How does Lambda work: execution model, cold starts, concurrency and limits?
- [ ] When should you not use Lambda, and when is AWS Batch the better fit?

## B. Storage
- [ ] S3 storage classes and Intelligent-Tiering — how do you choose?
- [ ] How do you secure an S3 bucket: bucket policies vs. IAM policies vs. ACLs, Block Public Access and Object Ownership?
- [ ] S3 versioning, replication and Object Lock — what problem does each solve?
- [ ] How do you get high performance from S3 (request rates, multipart upload, byte-range fetches, Transfer Acceleration)?
- [ ] EBS volume types and snapshots — how do you choose and protect volumes?
- [ ] EBS vs. EFS vs. FSx vs. instance store — when each?
- [ ] How do you move large amounts of data into AWS: DataSync vs. Storage Gateway vs. Snowball vs. Transfer Family?

## C. Networking & content delivery
- [ ] ALB vs. NLB vs. Gateway Load Balancer.
- [ ] Route 53 routing policies and health checks.
- [ ] How does CloudFront work (origins, caching, Origin Access Control, signed URLs), and when would you use Global Accelerator instead?
- [ ] Gateway vs. interface VPC endpoints.
- [ ] Transit Gateway vs. VPC peering vs. PrivateLink — the short version.
- [ ] "I can't connect to my instance" — how do you debug security groups, NACLs and routes?

## D. Databases & analytics
- [ ] RDS Multi-AZ vs. read replicas.
- [ ] How is Aurora different from RDS: storage architecture, Serverless v2 and Global Database?
- [ ] How do you design a DynamoDB table: partition and sort keys, single-table design and hot partitions?
- [ ] DynamoDB GSI vs. LSI, and on-demand vs. provisioned capacity.
- [ ] DynamoDB consistency, transactions and Streams.
- [ ] ElastiCache Redis (Valkey) vs. Memcached, and which caching patterns do you use?
- [ ] Redshift vs. Athena vs. EMR — when would you use each, and where do Glue and Lake Formation fit?
- [ ] Kinesis Data Streams vs. Data Firehose vs. MSK vs. SQS.
- [ ] When would you use OpenSearch, and how does it compare with other options for vector search?

## E. Application integration
- [ ] SQS standard vs. FIFO queues.
- [ ] Explain SQS visibility timeout, long polling and dead-letter queues — and how do you handle at-least-once delivery?
- [ ] How does SNS fan-out work, and why pair SNS with SQS?
- [ ] EventBridge vs. SNS vs. SQS — and what are EventBridge Pipes and Scheduler?
- [ ] Step Functions: Standard vs. Express, error handling, and when to use them instead of chaining Lambdas?
- [ ] API Gateway REST vs. HTTP vs. WebSocket APIs, throttling and authorisers.

## F. Security & identity
- [ ] How does AWS evaluate IAM policies (explicit deny, SCPs, permission boundaries, resource policies and session policies)?
- [ ] How do STS AssumeRole and external IDs work, and what is the confused deputy problem?
- [ ] What is IAM Identity Center, and why use it instead of IAM users?
- [ ] How does KMS envelope encryption work, and what are key policies?
- [ ] Secrets Manager vs. Parameter Store.
- [ ] Cognito user pools vs. identity pools.
- [ ] WAF, Shield, GuardDuty, Security Hub, Inspector and Macie — what does each do?
- [ ] A developer's access keys were pushed to a public GitHub repository. What do you do?

## G. Operations, observability & IaC
- [ ] How do you use CloudWatch metrics, logs, alarms and Logs Insights together?
- [ ] CloudTrail vs. CloudWatch vs. AWS Config.
- [ ] How do you trace requests across services with X-Ray and ADOT?
- [ ] CloudFormation vs. CDK vs. Terraform — which do you choose, and how do you handle drift and failed stack updates?
- [ ] What does Systems Manager give you (Session Manager, Patch Manager, Run Command)?
- [ ] Which AWS cost tools do you use, and how do you make cost visible?

## H. AI/ML on AWS
- [ ] What does Amazon Bedrock provide, and how do on-demand, batch and provisioned throughput differ?
- [ ] How do Bedrock Knowledge Bases work for RAG?
- [ ] What do Bedrock Guardrails and model evaluation do?
- [ ] Bedrock Agents vs. Bedrock AgentCore — what is each for?
- [ ] What does SageMaker provide for training and inference, and which endpoint type do you choose?
- [ ] Bedrock vs. SageMaker vs. self-hosting on EC2/EKS — how do you choose?
- [ ] How do you keep customer data private when using Bedrock?
- [ ] What is Amazon Q, and where does it fit?
- [ ] Walk me through a production RAG application on AWS, end to end.

## I. Scenarios & troubleshooting
- [ ] A Lambda function times out when it calls an RDS database. How do you debug it?
- [ ] An application gets 403 Access Denied from S3. How do you debug it?
- [ ] An EC2 instance in a private subnet can't reach the internet. What do you check?
- [ ] A DynamoDB table is throttling. How do you diagnose and fix it?
- [ ] API latency spiked right after a deploy. What do you do?
- [ ] The AWS bill spiked overnight. How do you find out why?
- [ ] Design a serverless image-processing pipeline.
- [ ] Migrate a server that runs a dozen cron jobs to AWS.
