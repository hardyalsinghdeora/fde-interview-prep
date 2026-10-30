# 16 — AWS: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers at the **service level**: how a specific AWS service behaves, its limits and the trade-offs between neighbouring services. Architecture-level questions (VPC layout, HA/DR, cost strategy, landing zones, Well-Architected) are answered in [topic 08 section C](../08-cloud-devops-integration/ANSWERS.md#c-cloud) and the [Solutions Architect role file](../../roles/04-solutions-architect/README.md#model-answers), and are linked rather than repeated. AWS quotas and features change often: anything marked *(verify)* should be checked against current AWS documentation before you quote it. In the interview, "roughly" and "last time I checked" are fine for quotas; being confidently wrong is not.

---

## A. Compute

### What are the EC2 instance families and purchase options, and how do you choose?

Read the name: `m7g.2xlarge` = family **m** (general purpose), generation **7**, attribute **g** (Graviton/ARM), size **2xlarge**. Other attribute letters: `i` Intel, `a` AMD, `d` local NVMe, `n` enhanced networking, `e` extra memory or storage.

| Family | Optimised for | Typical workload |
|---|---|---|
| **T** (burstable) | Cheap baseline CPU plus credits | Dev boxes, low-traffic services; watch CPU credit exhaustion (unlimited mode costs extra) |
| **M** (general) | Balanced CPU:memory (about 1:4) | Default for app servers |
| **C** (compute) | High CPU (about 1:2) | Batch, encoding, busy stateless APIs |
| **R / X / z** (memory) | 1:8 and above | Caches, in-memory databases, SAP HANA (X) |
| **I / D** (storage) | Local NVMe SSD / HDD | Self-managed databases, Kafka, search |
| **P / G / Inf / Trn** (accelerated) | NVIDIA GPUs (P for training, G for inference/graphics), Inferentia, Trainium | ML training and inference |

**Purchase options:**
- **On-Demand:** no commitment; spiky, short-lived or unknown workloads.
- **Savings Plans / Reserved Instances:** 1- or 3-year commitment for the steady baseline. Trade-offs are in the [Solutions Architect answer](../../roles/04-solutions-architect/README.md#compare-savings-plans-reserved-instances-and-spot-instances-and-say-when-you-would-use-each); don't repeat them all here.
- **Spot:** spare capacity at a large discount, reclaimed with a two-minute warning; fault-tolerant work.
- **Dedicated Hosts / Instances:** per-socket licensing or compliance.
- **On-Demand Capacity Reservations / Capacity Blocks for ML:** guarantee capacity (often GPUs) without a discount *(verify current Capacity Blocks terms)*.

**How to choose:** find the workload's bottleneck (CPU, memory, disk, network, GPU), benchmark on a current generation, prefer Graviton when the software runs on ARM (commonly quoted as up to ~40% better price-performance *(verify)*), then right-size with Compute Optimizer after a couple of weeks of real metrics. Interviewers listen for "I'd measure, not guess", Graviton, and Spot for interruptible work.

### What are AMIs, user data and launch templates, and how do they fit together?

- **AMI (Amazon Machine Image):** root volume snapshot(s) plus metadata (architecture, block device mapping, launch permissions). AMIs are regional: copy them to other regions, share them with accounts (an encrypted AMI also needs its KMS key shared).
- **User data:** a script or cloud-init config that runs **once, on first boot**, as root by default. Used to bootstrap: install an agent, pull config, join a cluster. Limited to 16 KB; output in `/var/log/cloud-init-output.log`. It is **not a secret store**: anyone who can describe the instance can read it.
- **Launch template:** a versioned definition of how to launch: AMI, instance type(s), security groups, IAM instance profile, user data, EBS volumes, metadata options, tags. Used by Auto Scaling groups, EC2 Fleet and Spot. Launch configurations are legacy.

**Bake vs. bootstrap:**
- **Golden AMI** (built in a pipeline with EC2 Image Builder or Packer): fast, reproducible boots, patched centrally. Best for autoscaling.
- **Bootstrap with user data:** flexible, but slower, and scale-out fails if a package mirror is down at the worst moment.
- Common middle ground: bake the OS and dependencies, use user data only for environment-specific settings, and fetch secrets at runtime from Secrets Manager or Parameter Store.

Always require **IMDSv2** (session-token metadata) in the launch template; it blocks the classic SSRF-to-instance-credentials attack. What they listen for: immutable infrastructure (replace, don't patch in place) and why secrets don't belong in user data.

### How do Auto Scaling groups work: scaling policies, health checks, lifecycle hooks and warm pools?

An ASG keeps the number of instances between **min** and **max** at a **desired** capacity, spread across the subnets (AZs) you give it, launched from a launch template. It rebalances across AZs and replaces unhealthy instances.

| Policy | How it works | Use when |
|---|---|---|
| **Target tracking** | Keeps a metric at a target (CPU 50%, ALB requests per target 1,000) | Default; simplest to get right |
| **Step scaling** | Adds or removes N instances depending on how far an alarm is breached | Different reactions to small and large spikes |
| **Scheduled** | Changes min/desired at set times | Known peaks (09:00 weekdays, Black Friday) |
| **Predictive** | Forecasts load from history and scales ahead of it | Regular daily or weekly cycles with slow boot times |

For queue workers, target-track a custom metric "backlog per instance" (visible SQS messages divided by running instances) rather than CPU.

- **Health checks:** EC2 status checks by default. Turn on **ELB health checks** so an instance that runs but returns 500s gets replaced, and set a **health check grace period** that covers boot time.
- **Lifecycle hooks:** pause instances in `Pending:Wait` or `Terminating:Wait` to finish bootstrap, register with something, or drain connections and ship logs before termination.
- **Warm pools:** keep pre-initialised instances stopped (or running/hibernated) so scale-out takes seconds instead of minutes; stopped instances cost only their EBS. Useful when boot or model loading is slow.
- Also: **instance refresh** for rolling AMI updates, **mixed instances policy** (several instance types, an On-Demand base plus Spot), **capacity rebalancing** for Spot, and default instance warm-up to avoid flapping.

General autoscaling signals and pitfalls: [topic 08](../08-cloud-devops-integration/ANSWERS.md#autoscaling-strategies).

### ECS vs. EKS vs. Fargate vs. App Runner — when would you use each?

Two separate choices: the **orchestrator** (ECS or EKS) and **where containers run** (EC2 you manage, or Fargate).

| Option | What it is | Choose when | Watch out for |
|---|---|---|---|
| **ECS** | AWS's own orchestrator (task definitions, services) | AWS-only team that wants simplicity and tight IAM/ALB integration | AWS-specific; smaller ecosystem than Kubernetes |
| **EKS** | Managed Kubernetes control plane | Portability (same Helm charts in customer clouds and on-prem), existing Kubernetes skills, operators and ecosystem | Per-cluster fee, version upgrades roughly yearly to stay in standard support *(verify)*, add-ons, ingress and node management (EKS Auto Mode reduces this *(verify)*) |
| **Fargate** | Serverless compute for ECS tasks or EKS pods | No nodes to manage, per-task isolation, small or spiky services | Higher unit price, no GPUs *(verify)*, no DaemonSets on EKS, slower start than warm nodes |
| **EC2 capacity** (ECS on EC2, EKS node groups, Karpenter) | You run the nodes | GPUs, high steady utilisation, Spot, custom AMIs | Patching and capacity are yours |
| **App Runner** | Source code or image to an HTTPS URL with autoscaling, no cluster | Simple web apps and APIs, small team | Limited networking and configuration; check its current status *(verify: AWS has been steering new users towards ECS Express Mode)* |

FDE angle: if the product must also run in a customer's Kubernetes cluster, standardise on **EKS** so one artefact deploys everywhere. For a single AWS-only service, **ECS on Fargate** is the least operational work. Architecture-level comparison: [Solutions Architect answer](../../roles/04-solutions-architect/README.md#when-would-you-recommend-kubernetes-over-a-managed-container-service-or-serverless).

### How does Lambda work: execution model, cold starts, concurrency and limits?

**Execution model:** each request runs in an **execution environment** (a Firecracker microVM) that handles **one request at a time**. After the request the environment is frozen and reused for the next one (a warm start); anything created outside the handler (SDK clients, DB connections) survives between invocations.

Invocation types:
- **Synchronous:** API Gateway, ALB, function URLs, SDK calls. The caller waits and handles retries.
- **Asynchronous:** S3, SNS, EventBridge. Lambda queues the event, retries twice, then sends it to an on-failure destination or DLQ.
- **Event source mapping:** Lambda polls SQS, Kinesis, DynamoDB Streams or Kafka and invokes your function with batches.

**Cold start:** a new environment has to download code, start the runtime and run init code. Typically a few hundred ms for Python or Node, more for Java/.NET or large packages *(verify)*. VPC attachment no longer adds a big penalty. Reduce it with smaller packages, lazy imports, more memory (which also means more CPU), **SnapStart** (Java, Python, .NET *(verify)*) or provisioned concurrency.

**Concurrency** = requests in flight ≈ requests per second × average duration in seconds. 100 rps × 0.5 s = 50 concurrent.
- **Account limit:** 1,000 concurrent executions per region by default, raisable *(verify)*.
- **Reserved concurrency:** reserves **and caps** concurrency for one function. Free. Use it to guarantee capacity or to **protect a downstream** database (cap at 50). Setting it to 0 is a kill switch.
- **Provisioned concurrency:** keeps N environments initialised so there are no cold starts; billed whether used or not. Use for latency-sensitive APIs, scheduled with Application Auto Scaling.

Limits to know *(verify current values)*:

| Limit | Value |
|---|---|
| Timeout | 15 minutes (default 3 seconds) |
| Memory | 128 MB to 10,240 MB; CPU scales with memory, up to 6 vCPU |
| Synchronous payload | 6 MB request and response (response streaming allows more) |
| Asynchronous payload | 256 KB historically, raised to 1 MB in 2025 *(verify)* |
| `/tmp` storage | 512 MB default, up to 10 GB |
| Deployment package | 50 MB zipped upload, 250 MB unzipped, 10 GB container image |

What they listen for: the concurrency formula, the reserved vs. provisioned distinction (the most common confusion), and the fact that Lambda can overwhelm a database.

### When should you not use Lambda, and when is AWS Batch the better fit?

Don't use Lambda when:
- **Work runs longer than 15 minutes** and can't be split into steps (split with Step Functions, or use ECS tasks or Batch).
- **Load is steady and high:** containers or EC2 on Savings Plans are usually cheaper. Do the maths with your request rate, duration and memory.
- **Latency must be consistently low** and you won't pay for provisioned concurrency.
- **You need GPUs**, large models or big local state.
- **You need long-lived connections** (a WebSocket server, gRPC streams, stateful Kafka consumers).
- **Many short calls hit a relational database** and you have no RDS Proxy.
- **The same code must run in customer clouds or on-prem.**

AWS has also launched Lambda Managed Instances (Lambda functions on EC2 capacity you choose), which changes some of the cost and hardware trade-offs *(verify current scope)*.

**AWS Batch** runs containerised jobs from **job queues** on **compute environments** (EC2, Spot, Fargate or EKS). It handles provisioning, scheduling, priorities, retries, **array jobs** (10,000 copies of a job, each with an index) and job dependencies. Batch itself is free; you pay for the compute, and Spot fits well because jobs can retry. Use it for hours-long jobs, HPC, genomics, rendering, nightly ML scoring, GPU jobs: anything shaped like "many independent jobs, done by morning".

Rule of thumb: event glue and short tasks → Lambda; long or heavy queued jobs → Batch (or an ECS task started by EventBridge for a handful of jobs); multi-step workflows → Step Functions orchestrating any of them.

## B. Storage

### S3 storage classes and Intelligent-Tiering — how do you choose?

| Class | Retrieval | Minimum storage duration | Use for |
|---|---|---|---|
| **Standard** | Milliseconds | None | Hot data, the default |
| **Intelligent-Tiering** | Milliseconds (optional archive tiers take hours) | None | Unknown or changing access patterns |
| **Standard-IA** | Milliseconds, per-GB retrieval fee | 30 days | Rarely read but needed quickly (backups) |
| **One Zone-IA** | Milliseconds, one AZ only | 30 days | Data you can re-create (thumbnails, secondary copies) |
| **Glacier Instant Retrieval** | Milliseconds, higher retrieval fee | 90 days | Archives read about once a quarter (medical images) |
| **Glacier Flexible Retrieval** | Minutes to hours | 90 days | Archives, DR copies |
| **Glacier Deep Archive** | Within 12–48 hours | 180 days | 7–10 year compliance retention; cheapest |
| **Express One Zone** | Single-digit ms, one AZ, directory buckets | None *(verify)* | Very high request rates, ML training data, latency-sensitive analytics |

**Intelligent-Tiering** moves objects automatically: Frequent → Infrequent after 30 days without access → Archive Instant after 90 days, plus optional asynchronous Archive and Deep Archive tiers you opt into. No retrieval fees, but a small monthly monitoring fee per object, and objects under 128 KB are never moved. A good default for data lakes and user content where you can't predict access.

**Gotchas that cost money:**
- Minimum duration charges: delete a Glacier object after a week and you still pay for 90 days.
- Minimum billable object size (128 KB) in the IA classes.
- Lifecycle transitions are charged per object: moving millions of tiny objects can cost more than it saves, so compact them first.
- Retrieval fees on IA and Glacier.

Lifecycle and presigned URL basics: [topic 08](../08-cloud-devops-integration/ANSWERS.md#object-storage-s3gcsblob--consistency-lifecycle-policies-presigned-urls). What they listen for: you ask about **access pattern and object size** before choosing a class.

### How do you secure an S3 bucket: bucket policies vs. IAM policies vs. ACLs, Block Public Access and Object Ownership?

- **IAM (identity-based) policies:** what a principal in *your* account can do, across many buckets. Use them for your own workloads.
- **Bucket policies (resource-based):** who can access *this* bucket, including **other accounts** and services (CloudFront via OAC), with conditions such as `aws:SourceVpce` (only through a VPC endpoint), `aws:PrincipalOrgID` (only our organisation), `aws:SecureTransport` (deny non-TLS) or a required KMS key.
- **ACLs:** legacy per-object grants. **Disable them** by setting **Object Ownership = Bucket owner enforced** (the default for new buckets since April 2023). The bucket owner then owns every object, including ones uploaded by other accounts, which removes the old "I own the bucket but can't read the object" problem.
- **Block Public Access:** four settings, at account and bucket level, that override any policy or ACL that would make data public. On by default for new buckets. Turn it on at the **account** level (or enforce with an SCP) and treat exceptions as reviewed decisions; serve public content through CloudFront with OAC instead.
- **Encryption:** SSE-S3 is applied to all new objects by default. Use **SSE-KMS** with a customer managed key when you need key-level access control and audit, and enable **S3 Bucket Keys** to cut KMS request costs.
- **Detect:** IAM Access Analyzer (buckets shared outside the account or organisation), Macie (sensitive data), CloudTrail data events, server access logs.

Same-account access works if *either* the IAM policy or the bucket policy allows it and nothing denies it; cross-account access needs *both*. See [IAM policy evaluation](#how-does-aws-evaluate-iam-policies-explicit-deny-scps-permission-boundaries-resource-policies-and-session-policies).

### S3 versioning, replication and Object Lock — what problem does each solve?

- **Versioning:** every overwrite keeps the previous version, and a delete just adds a **delete marker**. Protects against accidental deletes and overwrites. Once enabled it can be suspended but not removed. Add lifecycle rules that expire **noncurrent versions** after N days, or the bill grows silently. **MFA Delete** adds protection against version deletion.
- **Replication** (versioning required on both buckets):
  - **Cross-Region Replication (CRR)** for DR, latency or compliance copies; **Same-Region Replication (SRR)** for aggregating logs or copying to another account.
  - Only new objects replicate by default; use **S3 Batch Replication** for existing ones.
  - **Replication Time Control** offers 99.99% of objects replicated within 15 minutes, with metrics *(verify)*.
  - Can replicate to another account and change object ownership; replicating delete markers is optional.
  - **Replication is not backup:** a bad overwrite or ransomware encryption replicates too.
- **Object Lock** (WORM, write once read many): blocks deletion or overwrite of an object version for a retention period.
  - **Governance mode:** users with a special permission can override it. **Compliance mode:** nobody, including root, can shorten or remove it.
  - **Legal hold:** an indefinite lock until someone removes it.
  - Used for regulated records retention (SEC 17a-4-style) and **ransomware-resistant backups** (AWS Backup Vault Lock is the backup equivalent).

What they listen for: "replication is not a backup", and noncurrent version expiry.

### How do you get high performance from S3 (request rates, multipart upload, byte-range fetches, Transfer Acceleration)?

- **Request rates:** at least **3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix**, and S3 scales by partitioning prefixes automatically. Spread hot workloads across prefixes (for example `date/shard/...`); random hash prefixes are no longer needed. A sudden 10x jump on one prefix can briefly return **503 Slow Down** while S3 scales, so retry with backoff.
- **Multipart upload:** required above 5 GB per object upload, recommended from about 100 MB. Parts of 5 MB to 5 GB, up to 10,000 parts, uploaded in parallel and retried individually. Add a lifecycle rule to **abort incomplete multipart uploads**: they are billed but invisible in normal listings. Maximum object size was 5 TB for years *(verify: AWS announced 50 TB objects in late 2025)*.
- **Byte-range fetches:** download parts of a large object in parallel, or read only the part you need (a Parquet footer).
- **Transfer Acceleration:** uploads enter at the nearest edge location and travel over the AWS backbone. Helps long-distance uploads from users around the world; costs extra; test with the speed comparison tool first.
- **Other levers:** columnar formats and fewer, larger files (roughly 100 MB to 1 GB) for analytics; **Express One Zone** for single-digit-ms access next to compute; **Mountpoint for Amazon S3** or the CRT-based SDK transfer clients for high-throughput reads; an S3 gateway endpoint so traffic doesn't pass through NAT; compute in the same region.
- **Small-files problem:** millions of tiny objects make listing, lifecycle transitions and analytics slow and expensive. Batch or compact them.

### EBS volume types and snapshots — how do you choose and protect volumes?

| Type | Profile | Use for |
|---|---|---|
| **gp3** (SSD) | 3,000 IOPS and 125 MB/s baseline at any size; extra IOPS and throughput provisioned separately *(verify current maximums, raised in 2025)* | The default for almost everything |
| **gp2** (SSD, older) | IOPS tied to size (3 IOPS per GB, burst credits) | Migrate to gp3: usually ~20% cheaper, no downtime |
| **io2 Block Express** | Up to 256,000 IOPS, sub-millisecond latency, higher durability, Multi-Attach | Large Oracle/SQL Server, latency-critical databases |
| **st1** (HDD) | Throughput-optimised | Large sequential reads: logs, streaming, warehouses on EC2 |
| **sc1** (HDD) | Cheapest | Cold, rarely accessed data |

Key facts: a volume lives in **one AZ**, attaches to instances in that AZ, and is replicated only within it. The instance type also has a maximum EBS bandwidth, so a fast volume on a small instance is capped by the instance.

**Snapshots:** point-in-time and **incremental** (only changed blocks), stored regionally by AWS.
- Use them to back up, copy across regions or accounts for DR, create a volume in another AZ, or build an AMI.
- Automate with **Data Lifecycle Manager** or **AWS Backup** (central policies, cross-account copies, Vault Lock).
- Restored volumes load blocks lazily, so first reads are slow. Use **Fast Snapshot Restore** or initialise the volume before a database goes live.
- Snapshots are **crash-consistent**. For application consistency, flush or freeze the application first, or use pre/post scripts (VSS on Windows).
- Turn on EBS encryption by default for the account; to encrypt an old unencrypted volume, copy its snapshot with encryption.
- **Recycle Bin** protects against accidental snapshot deletion; the snapshot archive tier is cheaper for long-term retention.

### EBS vs. EFS vs. FSx vs. instance store — when each?

| Option | Type | Shared? | Durability / scope | Use for |
|---|---|---|---|---|
| **EBS** | Block | One instance (Multi-Attach on io1/io2 only) | Persistent, one AZ | Boot disks, databases on EC2 |
| **Instance store** | Block, NVMe physically attached | No | **Ephemeral**: lost on stop, terminate or host failure | Caches, scratch space, data replicated at the application level |
| **EFS** | NFS file system (Linux) | Thousands of clients across AZs | Regional (or One Zone), grows automatically | Shared content, home directories, shared files for ECS, EKS and Lambda |
| **FSx for Windows File Server** | SMB with Active Directory | Yes | Multi-AZ option | Windows file shares, lift-and-shift of file servers |
| **FSx for Lustre** | Parallel HPC file system linked to S3 | Yes | Scratch or persistent | ML training, HPC, high throughput |
| **FSx for NetApp ONTAP** | NFS, SMB and iSCSI with NetApp features | Yes | Multi-AZ | Migrating NetApp estates, multi-protocol access |
| **FSx for OpenZFS** | NFS, low latency | Yes | Single or Multi-AZ | Migrating ZFS or Linux NAS |
| **S3** | Object | Via API | Regional, 11 nines durability | Anything that doesn't need POSIX file semantics |

How to choose: Does more than one machine need it at the same time (EFS/FSx rather than EBS)? Which protocol (NFS, SMB, block)? Can you afford to lose it (instance store)? Price: EFS Standard costs several times more per GB than gp3 *(verify)*, so use its lifecycle to Infrequent Access and don't run databases on it. Often the right answer is "put it in S3 instead".

### How do you move large amounts of data into AWS: DataSync vs. Storage Gateway vs. Snowball vs. Transfer Family?

Do the arithmetic first: 100 TB over a 1 Gbps link at 80% utilisation is 800,000 Gb ÷ 0.8 Gbps = 1,000,000 seconds, **about 12 days**. If that fits the timeline and doesn't starve the customer's link, transfer online.

| Tool | What it does | Use when |
|---|---|---|
| **DataSync** | Agent-based online transfer from NFS, SMB, HDFS, object stores and other clouds into S3, EFS or FSx; incremental, verified, scheduled, bandwidth-limited | One-off or recurring migration of file data over the internet or Direct Connect |
| **Storage Gateway** | On-prem virtual appliance that presents S3 as NFS/SMB (**S3 File Gateway**), iSCSI volumes (**Volume Gateway**) or a virtual tape library (**Tape Gateway**), with a local cache | Ongoing hybrid access; replacing physical tape backups; apps that must stay on-prem but store in AWS |
| **Snowball Edge** | Rugged physical device you load and ship back | Tens of TB to petabytes over slow links; edge compute at disconnected sites *(verify: AWS has retired several Snow devices, so check what is still offered)* |
| **Transfer Family** | Managed SFTP, FTPS, FTP and AS2 endpoints backed by S3 or EFS | Partners who send files over SFTP; legacy integrations |
| **DMS** | Database migration with change data capture | Databases, not files: see the [Solutions Architect answer](../../roles/04-solutions-architect/README.md#how-do-you-migrate-a-production-database-with-minimal-downtime) |

Typical pattern for a large migration: bulk copy with Snowball (or DataSync over Direct Connect), then DataSync incremental syncs until cut-over. Where this sits in a migration plan: [the 7 Rs](../../roles/04-solutions-architect/README.md#explain-the-7-rs-of-migration-and-how-you-choose-between-them).

## C. Networking & content delivery

### ALB vs. NLB vs. Gateway Load Balancer.

| | ALB | NLB | GWLB |
|---|---|---|---|
| Layer | 7 (HTTP/HTTPS, HTTP/2, gRPC, WebSocket) | 4 (TCP, UDP, TLS) | 3 (IP packets, GENEVE on port 6081) |
| Routing | Host, path, header, query string, method; weighted target groups | By port | Sends traffic through an appliance fleet and back |
| Targets | Instances, IPs, Lambda functions, containers | Instances, IPs, an ALB | Virtual appliances (firewalls, IDS/IPS) |
| Static IP | No (use the DNS name, or Global Accelerator in front) | **Yes**, one per AZ, Elastic IPs allowed | n/a |
| Client IP | In `X-Forwarded-For` | Preserved (for instance targets) | Preserved |
| Extras | WAF, OIDC/Cognito authentication, redirects, fixed responses, sticky sessions, mTLS | Very high throughput and low latency, **PrivateLink endpoint services**, TLS pass-through or termination | Transparent inline inspection with third-party firewalls |

How to choose:
- Web apps, APIs, microservices with path routing → **ALB**.
- Non-HTTP protocols, static IPs for a customer's firewall allowlist, extreme throughput, or exposing a service through **PrivateLink** → **NLB** (an NLB can sit in front of an ALB to get both).
- Central inspection through a security vendor's appliances → **GWLB**, usually in an inspection VPC behind a Transit Gateway.

Debugging: an ALB **502** means the target closed the connection or sent an invalid response (often the app's keep-alive timeout is shorter than the ALB idle timeout, 60 s by default); **503** means no healthy targets; **504** means the target timed out. More in [502/504 debugging](../12-debugging-troubleshooting/ANSWERS.md#an-api-is-returning-502504-what-could-cause-it).

### Route 53 routing policies and health checks.

Route 53 provides DNS (public and private hosted zones), domain registration and health checks. **Alias records** point the zone apex (`example.com`) at AWS resources such as an ALB, CloudFront or an S3 website: no CNAME-at-apex problem and no charge for those queries.

| Policy | Behaviour | Use for |
|---|---|---|
| **Simple** | One record (several values returned in random order), no health checks | A single resource |
| **Weighted** | Splits answers by weight (90/10) | Canary or blue-green between stacks, A/B tests |
| **Latency-based** | Answers with the region that has the lowest latency for the user | Multi-region active-active |
| **Failover** | Primary/secondary based on a health check | Active-passive DR |
| **Geolocation** | By the user's continent, country or US state | Data residency, localised content, licensing |
| **Geoproximity** | By distance, with a bias to grow or shrink a region's share | Gradually shifting traffic between regions |
| **Multivalue answer** | Up to 8 healthy records at random | Simple client-side spreading |
| **IP-based** | By the client's CIDR | Routing specific networks or ISPs |

**Health checks:**
- **Endpoint checks:** HTTP, HTTPS or TCP from checkers in several regions, optionally matching a string in the response.
- **Calculated checks:** combine child checks ("healthy if 2 of 3 are healthy").
- **CloudWatch alarm checks:** for private resources external checkers can't reach.
Attach them to records so unhealthy answers are withdrawn.

Gotchas: failover is only as fast as the **TTL** (use about 60 s on failover records), and some clients cache longer than the TTL; firewalls must allow the health checkers; Application Recovery Controller gives tightly controlled failover for critical apps. DR strategies themselves: [topic 08](../08-cloud-devops-integration/ANSWERS.md#high-availability-and-disaster-recovery-multi-az-multi-region-rporto).

### How does CloudFront work (origins, caching, Origin Access Control, signed URLs), and when would you use Global Accelerator instead?

**CloudFront** is AWS's CDN: hundreds of edge locations plus regional edge caches *(verify current count)*.
- **Origins:** S3, ALB/EC2, API Gateway, Lambda function URLs, any HTTP server. **Origin groups** fail over to a secondary origin; **Origin Shield** adds a central cache layer to protect the origin.
- **Cache behaviours** per path pattern: `/static/*` cached for a day, `/api/*` not cached and forwarded to the ALB.
- **Cache key (cache policy):** which headers, cookies and query strings create separate cache entries. Including too much (every cookie) destroys the hit ratio. **Origin request policies** forward extra values to the origin without adding them to the cache key.
- **Invalidations** take time and cost money beyond a free allowance; prefer **versioned file names** (`app.3f9a.js`).
- **OAC (Origin Access Control):** CloudFront signs its requests to a private S3 bucket, and the bucket policy allows only that distribution (`AWS:SourceArn`). The bucket stays private. OAC replaces the legacy OAI and supports SSE-KMS.
- **Signed URLs vs. signed cookies:** both use a key group (public key registered in CloudFront, private key in your app) plus an expiry or policy. A signed URL protects one file (a download link); signed cookies cover many files (a video stream, a members' area) without changing URLs. These are different from S3 presigned URLs, which bypass CloudFront.
- **Edge code:** **CloudFront Functions** (lightweight JavaScript for header rewrites, redirects, simple token checks) vs. **Lambda@Edge** (Node/Python, network calls, more time and cost).
- **Security:** TLS at the edge with ACM certificates (which must be in **us-east-1**), WAF, Shield Standard included, geo-restriction.

**Global Accelerator** gives you two **static anycast IP addresses**. Traffic enters at the nearest edge and travels over the AWS backbone to regional endpoints (ALB, NLB, EC2, Elastic IP). It does not cache. Use it for **TCP/UDP traffic** (gaming, VoIP, IoT), when customers need **fixed IPs to allowlist**, or for **fast regional failover** without waiting for DNS caches. Use CloudFront for cacheable HTTP content and for APIs that benefit from edge TLS termination.

### Gateway vs. interface VPC endpoints.

Both let resources in private subnets reach AWS services **without an internet gateway or NAT**, keeping traffic on the AWS network.

| | Gateway endpoint | Interface endpoint (PrivateLink) |
|---|---|---|
| Services | **S3 and DynamoDB only** | Most AWS services (SQS, KMS, Secrets Manager, ECR, STS, Bedrock, CloudWatch...), plus your own and SaaS services |
| Mechanism | Route table entry pointing at a prefix list | Network interfaces with private IPs in your subnets |
| Cost | **Free** | Per hour per AZ plus per GB |
| Usable from on-prem or peered VPCs | No | **Yes** (over VPN, Direct Connect or Transit Gateway) |
| Controls | Endpoint policy | Endpoint policy plus security groups |
| DNS | Public service names, routed privately | Private DNS makes the public name resolve to the endpoint's IPs |

Practical points:
- Always add the free S3 and DynamoDB **gateway endpoints**; they remove NAT data processing charges for S3-heavy workloads.
- A private EKS or ECS cluster with no NAT needs interface endpoints for ECR (API and Docker), STS, CloudWatch Logs and others, plus the S3 gateway endpoint for image layers. Missing ones are a classic cause of `ImagePullBackOff`.
- Use **endpoint policies** to restrict which buckets or accounts are reachable (an exfiltration control), and `aws:SourceVpce` in bucket policies to require the endpoint.
- In multi-account estates, centralise interface endpoints in a shared-services VPC with Route 53 Resolver rules: see the [Solutions Architect networking answer](../../roles/04-solutions-architect/README.md#how-do-you-design-networking-across-hundreds-of-accounts).

### Transit Gateway vs. VPC peering vs. PrivateLink — the short version.

- **VPC peering:** one-to-one, non-transitive, CIDRs must not overlap, no hourly charge (data transfer only). Fine for a handful of VPCs; a full mesh needs n(n−1)/2 connections, which doesn't scale.
- **Transit Gateway:** a regional **hub** for VPCs, VPNs and Direct Connect gateways. Its route tables let you segment traffic (prod can't reach dev; everything passes through inspection). Charged per attachment-hour and per GB; peers across regions. The default above roughly ten VPCs or with hybrid connectivity. **Cloud WAN** adds a global, policy-driven network on top.
- **PrivateLink:** exposes **one service**, in one direction, and overlapping CIDRs are fine. Best for SaaS-to-customer and cross-account service access without network-level trust.

Full comparison including VPN and Direct Connect: [topic 08](../08-cloud-devops-integration/ANSWERS.md#vpc-peering-vs-privatelink-vs-vpn-vs-direct-connect--expressroute). Multi-account network design: [Solutions Architect](../../roles/04-solutions-architect/README.md#how-do-you-design-networking-across-hundreds-of-accounts).

### "I can't connect to my instance" — how do you debug security groups, NACLs and routes?

Follow the packet path in both directions. Concepts are in [topic 08](../08-cloud-devops-integration/ANSWERS.md#vpc-subnets-publicprivate-security-groups-nacls-nat-gateways).

1. **Is the target up and listening?** Instance status checks; on the box, `ss -tlnp`: is the app bound to `0.0.0.0` rather than `127.0.0.1`? Is an OS firewall (iptables, firewalld, Windows Firewall) blocking it?
2. **Right address?** Did the public IP change after a stop/start (no Elastic IP)? Does the private hosted zone resolve?
3. **Target security group:** an inbound rule for the port **from the source** (CIDR or source security group). Security groups are stateful, so replies are allowed automatically. Check the source's outbound rules too.
4. **NACLs on both subnets:** stateless, so you need the inbound port **and** outbound **ephemeral ports (1024–65535)** for the reply, and the mirror image on the other subnet. A missing ephemeral rule is the classic "the security group looks fine but it still hangs".
5. **Route tables:** a public subnet needs `0.0.0.0/0` to an internet gateway *and* the instance needs a public IP; a private subnet uses NAT; peering or Transit Gateway routes must exist in **both** VPCs.
6. **Timeout or refused?** A **timeout** means something dropped packets (security group, NACL, route). **Connection refused** means you reached the host and nothing is listening on that port.
7. **Tools:** **VPC Reachability Analyzer** models the path and names the blocking component; **VPC Flow Logs** show `REJECT` records; `nc -vz host port`, `curl -v`, `traceroute`.
8. **For shell access**, use **Session Manager** instead of SSH: no inbound port at all. If it fails, check the SSM agent, the instance role and reachability of the SSM endpoints.

What they listen for: a layer-by-layer method, stateful vs. stateless, and Reachability Analyzer or Flow Logs instead of guessing.

## D. Databases & analytics

### RDS Multi-AZ vs. read replicas.

| | Multi-AZ instance deployment | Multi-AZ DB cluster | Read replicas |
|---|---|---|---|
| Purpose | **High availability** | High availability plus some read scaling | **Read scaling** (and DR if cross-region) |
| Replication | **Synchronous** to one standby in another AZ | Semi-synchronous to two readable standbys *(verify engine support)* | **Asynchronous**, so replicas lag |
| Readable standby? | No | Yes | Yes |
| Failover | Automatic; the DNS endpoint moves; typically 60–120 s *(verify)* | Typically under 35 s *(verify)* | Manual promotion to a standalone database |
| Regions | Same region | Same region | Same or another region |

Points to make:
- Multi-AZ protects against instance, storage and AZ failure and makes maintenance less disruptive. It is **not** a read-scaling feature and **not** protection against bad writes, which replicate instantly; that's what automated backups and point-in-time recovery (up to 35 days) are for.
- Read replicas offload reporting and read traffic. The application must send reads to replica endpoints and tolerate stale data; read-your-own-writes needs the primary.
- Applications must reconnect after failover: avoid long DNS caching (the JVM is a common culprit) and use **RDS Proxy** to pool connections and shorten failover.
- A cross-region read replica is a cheap DR option (promote it in a disaster). For faster cross-region DR, use Aurora Global Database.

### How is Aurora different from RDS: storage architecture, Serverless v2 and Global Database?

**Storage architecture:** compute and storage are separate. The cluster volume is a distributed, log-structured store with **six copies across three AZs**. Writes need 4 of 6 copies and reads 3 of 6, so it tolerates losing an AZ and still serves reads with one more copy gone. The database sends only **redo log records** to storage rather than full pages, which is a large part of why it outperforms standard MySQL/PostgreSQL on EBS. Storage grows automatically (up to 128 TiB, raised for some versions *(verify)*) and you pay for what you use.
- **Replicas** (up to 15) share the same volume, so lag is typically milliseconds and failover is fast (often under 30 s *(verify)*). Endpoints: cluster (writer), reader (load-balanced across replicas), and custom endpoints.
- Extras: fast copy-on-write clones, Backtrack (MySQL), **I/O-Optimized** pricing for I/O-heavy workloads (worth it when I/O is a large share of the bill, about 25% or more *(verify)*), zero-ETL integration with Redshift.

**Serverless v2:** capacity in **ACUs** (each about 2 GiB of memory plus matching CPU) that scale in small steps within seconds between a minimum and maximum. Serverless and provisioned instances can be mixed in one cluster, and it can scale down to 0 ACUs with auto-pause *(verify)*. Good for spiky, unpredictable, dev and test workloads; at steady high load, provisioned instances are cheaper.

**Global Database:** one primary region and several read-only secondary regions *(verify current maximum)*, replicated at the storage layer with typical lag **under one second**. **Switchover** for planned moves with no data loss; **failover** for outages with an RPO of seconds and an RTO of about a minute or two *(verify)*. Write forwarding lets secondary regions accept writes and pass them to the primary. Use it for cross-region DR and low-latency global reads.

AWS also offers **Aurora DSQL**, a distributed serverless PostgreSQL-compatible database with active-active multi-region writes *(verify: newer service with PostgreSQL compatibility limits)*.

Choose Aurora over RDS for higher availability, many replicas, global DR and fast failover. Choose RDS when cost matters more, the workload is small, or you need an engine Aurora doesn't offer (Oracle, SQL Server, Db2). The wider database landscape: [topic 08](../08-cloud-devops-integration/ANSWERS.md#managed-dbs-rds-aurora-dynamodb-cosmos-db-bigquery-snowflake--when-each).

### How do you design a DynamoDB table: partition and sort keys, single-table design and hot partitions?

**Start from access patterns, not entities.** List every query ("get order by ID", "a customer's orders, newest first", "open orders for a warehouse") with expected rates, then design keys so each is a `GetItem` or `Query`, never a `Scan`.

- **Partition key:** hashed to choose the physical partition. It needs **high cardinality and even access**: `customerId`, `orderId`, `deviceId`. Bad choices: `status`, `date`, `country`.
- **Sort key:** orders items within a partition and supports range conditions (`begins_with`, `between`, `>`). Composite sort keys model hierarchies: `ORDER#2026-10-28#123`, `COUNTRY#UK#CITY#LEEDS`.
- **Item collection:** all items sharing a partition key; one `Query` can return a customer and their orders together.

**Single-table design:** several entity types in one table with generic key names (`PK = CUSTOMER#42`, `SK = PROFILE` or `ORDER#...`), plus overloaded GSIs for other access patterns.
- Benefits: related items in one request (no joins), fewer tables to operate.
- Costs: harder to read and evolve, analytics needs exports, and a new access pattern can mean a backfill.
- Use it for high-scale, well-understood access patterns. Use **several tables** when patterns are unknown, the team is new to DynamoDB, or the entities are unrelated. Many teams take a middle path.

**Hot partitions:** one partition supports up to **3,000 RCU and 1,000 WCU** per second. Adaptive capacity and split-for-heat spread load automatically, but **a single key** can't exceed the partition limit. Fixes:
- **Write sharding:** add a suffix (`PK = DATE#2026-10-28#7`, with 0–9 chosen randomly or from a hash), then read all shards and merge.
- Cache hot reads in **DAX** or ElastiCache.
- Avoid time-based partition keys for writes; "today" is always hot.
- Spread bulk loads across keys instead of writing in key order.

What they listen for: "access patterns first", cardinality, and a concrete sharding fix.

### DynamoDB GSI vs. LSI, and on-demand vs. provisioned capacity.

| | Global secondary index (GSI) | Local secondary index (LSI) |
|---|---|---|
| Keys | Any partition and sort key | Same partition key as the table, different sort key |
| Created | Any time | **Only when the table is created** |
| Consistency | Eventually consistent only | Strongly consistent reads available |
| Capacity | Its own | Shared with the table |
| Limits | 20 per table by default *(verify)* | 5 per table; each item collection limited to **10 GB** |

Use **GSIs** for almost everything: new access patterns, inverted indexes (swap PK and SK), and **sparse indexes** that contain only items with a given attribute (for example only open orders). Use an **LSI** only when you need a strongly consistent alternate sort order within a partition and can accept the 10 GB limit. A throttled GSI **throttles writes to the base table**, so give GSIs enough capacity and project only the attributes you need.

**Capacity modes:**
- **On-demand:** pay per request, no planning, absorbs sudden increases (instantly up to about double the previous peak *(verify)*); you can set a maximum throughput. AWS cut on-demand prices by about half in late 2024 *(verify)*, which makes it the sensible default for new, spiky or unpredictable tables.
- **Provisioned:** you set RCU/WCU, usually with auto scaling on a target utilisation; cheaper for steady, predictable traffic, especially with reserved capacity. Auto scaling reacts in minutes, so sharp spikes throttle.
- You can switch between modes, with limits on how often *(verify)*.

Units: 1 RCU = one strongly consistent read per second of up to 4 KB (or two eventually consistent reads); 1 WCU = one write per second of up to 1 KB; transactional reads and writes cost double.

### DynamoDB consistency, transactions and Streams.

- **Consistency:** reads are **eventually consistent** by default (usually consistent within a second, at half the cost). `ConsistentRead=true` gives **strongly consistent** reads on the table and LSIs, not on GSIs. **Global tables** replicate asynchronously between regions with last-writer-wins by default; a multi-region strong consistency option was added in 2025 *(verify)*.
- **Conditional writes:** `ConditionExpression` such as `attribute_not_exists(PK)` for idempotent creates or `version = :v` for optimistic locking. Most "transaction" needs are really a conditional write on one item.
- **Transactions:** `TransactWriteItems` and `TransactGetItems`: all-or-nothing across up to 100 items *(verify)* in one region, with conditions. They cost twice the capacity, conflicts raise `TransactionCanceledException`, and a `ClientRequestToken` makes retries idempotent. Use them for "debit one account and credit another" or "create the order and reserve stock".
- **Streams:** an ordered (per item) log of item-level changes, kept for **24 hours**, with a choice of keys only, new image, old image, or both. Consumed by Lambda through an event source mapping. Uses: triggering workflows (order placed → send email), maintaining aggregates or a search index, audit and change data capture into the data lake. **Kinesis Data Streams for DynamoDB** is the alternative when you need longer retention or more consumers.
- **Handling stream failures:** Lambda retries a failing batch until it succeeds or the records expire, which blocks that shard. Set `BisectBatchOnFunctionError`, a maximum retry count and an on-failure destination, and make the handler idempotent.
- Also: **TTL** deletes expired items at no cost (the deletes appear in the stream, which is handy for expiry events), point-in-time recovery (35 days), on-demand backups and export to S3.

### ElastiCache Redis (Valkey) vs. Memcached, and which caching patterns do you use?

| | Valkey / Redis OSS | Memcached |
|---|---|---|
| Data types | Strings, hashes, lists, sets, sorted sets, streams, geospatial, JSON and vector search *(verify per engine and version)* | Strings only |
| Durability and replication | Replicas, Multi-AZ automatic failover, snapshots | None: losing a node loses its data |
| Scaling | Cluster mode shards plus read replicas | Multi-threaded nodes; add nodes with client-side hashing |
| Extras | Pub/sub, Lua scripts, transactions, leaderboards, rate limiting, locks | Simplicity, multiple cores per node |

Choose **Valkey/Redis** almost always (AWS prices Valkey below Redis OSS *(verify)*). Choose **Memcached** only for a simple, large, throwaway object cache where multi-threading matters and durability doesn't. **ElastiCache Serverless** removes node sizing. **MemoryDB** is the durable, Redis-compatible option when you want it as a *primary* database.

**Caching patterns:**
- **Cache-aside (lazy loading):** read the cache; on a miss, read the database and write the result with a TTL. The most common pattern; caches only what is used; first reads are slow; data is stale until the TTL expires.
- **Write-through:** update the cache whenever you write the database. Fresher data, but caches things nobody reads, so still use a TTL.
- **Write-behind:** write to the cache and flush to the database asynchronously. Fast, but risks data loss; rarely appropriate.
- **TTL with jitter** so keys don't all expire at once; **stampede protection** (a lock or "single flight" on a miss, or serve stale while refreshing).
- **Invalidate on write** (delete the key) rather than trying to update it in place.
- Other uses: session store, rate limiter (`INCR` plus `EXPIRE`), distributed locks (carefully), leaderboards (sorted sets), semantic caching of LLM responses.

What they listen for: what is cached, how stale it may be, and what happens when the cache is empty or down (the database must survive a cold cache).

### Redshift vs. Athena vs. EMR — when would you use each, and where do Glue and Lake Formation fit?

| | Redshift | Athena | EMR |
|---|---|---|---|
| What | Columnar MPP data warehouse (provisioned RA3 nodes or Serverless) | Serverless SQL (Trino-based) directly over S3 | Managed Spark, Hive, Trino, Flink, HBase on EC2, EKS or Serverless |
| Pricing | Node-hours or RPU-hours | Per TB scanned (about $5/TB *(verify)*) or provisioned capacity | Instance-hours plus an EMR fee; Spot works well |
| Best for | Frequent, concurrent BI dashboards, complex joins, predictable performance | Ad-hoc queries, log analysis (CloudTrail, ALB logs), occasional reports | Large custom ETL, feature engineering, streaming, migrating Hadoop or Spark |
| Watch out for | Paying for idle capacity; table design (distribution and sort keys, though much is automatic now) | Cost and speed depend on file layout: partitioning, Parquet, compaction | You own cluster configuration and job tuning |

**Glue:**
- **Data Catalog:** the serverless, Hive-compatible metastore used by Athena, Redshift Spectrum and EMR.
- **Crawlers** that infer schemas and partitions.
- **ETL jobs:** serverless Spark or Python, with job bookmarks for incremental loads; also data quality rules and DataBrew for visual preparation.

**Lake Formation:** governance on top of the Glue Data Catalog:
- **Fine-grained permissions** at database, table, column, row and cell level.
- **LF-tags** for tag-based access control at scale.
- Cross-account data sharing.
Instead of writing S3 bucket policies per team, you grant "analysts can select these columns" and Athena, Redshift Spectrum and EMR enforce it.

Typical lake: raw → cleaned → curated zones in S3 as Parquet or Apache Iceberg tables (S3 Tables provide managed Iceberg *(verify)*), the Glue Data Catalog plus Lake Formation for access, Athena for ad-hoc work, Redshift for BI, Glue or EMR for heavy transforms. Hadoop migration design: [Solutions Architect whiteboard](../../roles/04-solutions-architect/README.md#whiteboard-design-a-cloud-analytics-platform-for-a-customer-leaving-an-on-premises-hadoop-cluster).

### Kinesis Data Streams vs. Data Firehose vs. MSK vs. SQS.

| | Kinesis Data Streams | Data Firehose | MSK (Kafka) | SQS |
|---|---|---|---|---|
| Model | Ordered, replayable stream of shards | Managed delivery pipe | Ordered, replayable log of partitions | Queue: each message goes to one consumer and is then deleted |
| Consumers | Many, each tracking its own position | You don't write consumers; it delivers | Many consumer groups | Competing consumers |
| Retention | 24 hours by default, up to 365 days | Buffers briefly before delivery | Configurable; tiered storage for long retention | Up to 14 days |
| Ordering | Per shard (by partition key) | n/a | Per partition | FIFO queues only |
| Scaling | Shards (each 1 MB/s or 1,000 records/s in, 2 MB/s out) or on-demand mode | Automatic | Brokers and partitions, or MSK Serverless | Automatic (standard queues are nearly unlimited) |
| Use for | Real-time analytics, clickstreams, IoT, several consumers reading the same data | Landing data in S3, Redshift, OpenSearch, Splunk or Snowflake with batching, Parquet conversion and Lambda transforms | Kafka compatibility, migrating existing Kafka, very high throughput, Kafka Connect | Decoupling, task queues, buffering |

How to decide:
- "Put events into S3 or a warehouse" → **Firehose** (least code).
- "Several applications process the same stream in real time, with replay" → **Kinesis Data Streams** (AWS-native, simpler) or **MSK** (the customer already runs Kafka, needs Kafka APIs and connectors, or wants portability).
- "Background jobs, each done once" → **SQS**.
- A common chain: producers → Kinesis or MSK → Lambda or Managed Service for Apache Flink → Firehose → S3.

Queues and streams in general: [topic 08](../08-cloud-devops-integration/ANSWERS.md#message-queues-sqs-rabbitmq-kafka-pubsub--when-and-why).

### When would you use OpenSearch, and how does it compare with other options for vector search?

**Amazon OpenSearch Service** (managed OpenSearch, the Elasticsearch fork) is the choice for:
- **Full-text search** with relevance scoring, fuzzy matching, facets and autocomplete (product or document search).
- **Log and observability analytics** with OpenSearch Dashboards, using UltraWarm and cold storage for older data.
- **Vector search** (k-NN with HNSW or IVF) and **hybrid search** (BM25 keyword scores plus vectors), which improves RAG retrieval quality.

Deployment: managed **domains** (you size nodes, shards and replicas, use dedicated master nodes and three AZs) or **OpenSearch Serverless** collections (search, time series, vector) with no sizing but a minimum capacity charge that surprises small projects *(verify current minimum)*. Operational gotchas: shard sizing (roughly 10–50 GB per shard is the usual guidance *(verify)*), JVM memory pressure, mapping explosions from uncontrolled fields. It is not a primary database: keep the source of truth elsewhere and be able to rebuild the index.

**Vector search options on AWS:**

| Option | Choose when |
|---|---|
| OpenSearch (domain or Serverless) | Hybrid keyword plus vector search, rich filters, large scale |
| Aurora or RDS PostgreSQL with pgvector | Already on PostgreSQL, moderate scale, want joins and transactions alongside vectors |
| S3 Vectors | Very large, cost-sensitive, latency-tolerant vector storage *(verify: newer service)* |
| MemoryDB or ElastiCache vector search | Lowest latency, smaller indexes |
| Third party (Pinecone, MongoDB Atlas) | The customer's existing standard |

Most of these can back a Bedrock Knowledge Base: see [section H](#how-do-bedrock-knowledge-bases-work-for-rag).

## E. Application integration

### SQS standard vs. FIFO queues.

| | Standard | FIFO |
|---|---|---|
| Delivery | **At least once** (occasional duplicates) | **Exactly-once processing** within a 5-minute deduplication window |
| Order | Best effort | Strict order **per message group ID** |
| Throughput | Nearly unlimited | 300 messages/s per API action, 3,000 with batching; much higher in high-throughput mode *(verify)* |
| Name | Anything | Must end in `.fifo` |

How FIFO works: each message carries a `MessageGroupId` (for example `customerId`). Order is guaranteed within a group, and different groups are processed in parallel. While a message from a group is in flight, the next message in that group is held back, so a poison message **blocks its whole group** until it moves to the DLQ. Duplicates are removed using a `MessageDeduplicationId` or content-based deduplication.

Choose **standard** by default, with idempotent consumers. Choose **FIFO** when order matters per entity (ledger updates per account, stock changes per SKU) and throughput per group is modest. Pick the group ID carefully: one group for everything turns the queue into a single-threaded pipe.

Other limits *(verify)*: maximum message size was 256 KB for years and was raised to 1 MiB in 2025; for larger payloads use the extended client library (payload in S3, pointer in the message). Retention is 1 minute to 14 days (default 4 days); delivery delay up to 15 minutes.

### Explain SQS visibility timeout, long polling and dead-letter queues — and how do you handle at-least-once delivery?

Lifecycle: a producer sends a message → a consumer calls `ReceiveMessage` → the message becomes **invisible** for the visibility timeout → the consumer processes it and calls `DeleteMessage`. If it doesn't delete in time, the message becomes visible again and another consumer receives it.

- **Visibility timeout:** default 30 seconds, maximum 12 hours. Set it comfortably above processing time; with a Lambda trigger, AWS recommends at least six times the function timeout. For long jobs, extend it as a heartbeat with `ChangeMessageVisibility`. Too short means duplicate processing; too long means slow retries after a crash.
- **Long polling:** `WaitTimeSeconds` up to **20 seconds**, so the call waits for messages instead of returning empty straight away. Fewer empty responses, lower cost, lower latency. Always use it (set `ReceiveMessageWaitTimeSeconds` on the queue).
- **Dead-letter queue:** after `maxReceiveCount` receives (say 5) without deletion, the message moves to the DLQ. Alarm on DLQ depth above zero, inspect, fix, then **redrive** messages back to the source queue. DLQ retention should be longer than the source queue's.

**At-least-once means duplicates will happen** (standard queues, expired visibility timeouts, a consumer crashing after the side effect but before the delete). Make consumers idempotent:
- Deduplicate on a business ID or message ID using a conditional write (DynamoDB `attribute_not_exists`) with a TTL.
- Make the side effect itself idempotent (upserts; an idempotency key on the payment API call).
- Delete the message only after success.
- With Lambda, return **partial batch failures** (`ReportBatchItemFailures`) so one bad message doesn't retry the whole batch.

Idempotency key design in general: [topic 08](../08-cloud-devops-integration/ANSWERS.md#idempotency-what-is-it-and-how-do-you-implement-idempotency-keys). What they listen for: sizing the timeout to processing time, DLQ plus alarm plus redrive, and that idempotency is the consumer's job.

### How does SNS fan-out work, and why pair SNS with SQS?

**SNS** is publish/subscribe: a publisher sends one message to a **topic**, and SNS pushes a copy to every **subscription** (SQS, Lambda, HTTP/S endpoints, Firehose, email, SMS, mobile push).

**SNS + SQS fan-out:** one topic, one SQS queue per consuming service.
- Each consumer gets its own copy, buffer, processing speed, retries and DLQ, so a slow or broken consumer doesn't affect the others.
- New consumers subscribe without any change to the producer.
- SQS absorbs spikes so consumers work at their own pace. Pushing directly from SNS to Lambda or HTTP endpoints gives you only SNS's delivery retry policy.
- Example: an `order-placed` topic with queues for billing, fulfilment, email and analytics.

Details worth mentioning:
- **Subscription filter policies** (on message attributes or the body) so each queue receives only the events it cares about.
- **Raw message delivery** to avoid the SNS JSON envelope arriving in SQS.
- The SQS **queue policy** must allow the topic to send to it; if the queue is encrypted with a customer managed KMS key, the key policy must allow SNS too (a common silent failure).
- **FIFO topics** feeding FIFO queues give ordered fan-out.
- Use a **DLQ on the subscription** for messages SNS can't deliver.

SNS vs. EventBridge is the next question.

### EventBridge vs. SNS vs. SQS — and what are EventBridge Pipes and Scheduler?

- **SQS:** a queue. **Pull**-based, buffers work, one logical consumer per queue. Use it to decouple and smooth load.
- **SNS:** a topic. **Push** to many subscribers, high throughput, low latency, attribute and body filtering. Use it for fan-out of your own events.
- **EventBridge event buses:** an **event router** with rich content-based **rules** (match on any field; prefix, numeric and exists matching), several targets per rule (up to 5 *(verify)*), input transformation, and native sources: AWS service events (EC2 state changes, GuardDuty findings), **SaaS partner events** (Salesforce, Zendesk, Datadog), cross-account and cross-region buses, **archive and replay**, a schema registry, and **API destinations** (call any HTTP API with auth and rate limiting). Latency is typically higher than SNS *(verify typical figures)*, and throughput has regional quotas.

| Need | Choose |
|---|---|
| Buffer work for a pool of workers | SQS |
| Fast fan-out of high-volume messages | SNS (plus SQS) |
| Route events from many sources by content; react to AWS or SaaS events; event-driven integration across teams and accounts | EventBridge |

**EventBridge Pipes:** point-to-point integration of **source → filter → enrich → target** without glue Lambdas. Example: DynamoDB Stream → only `status = SHIPPED` → enrich through an API → start a Step Functions workflow. Sources include SQS, Kinesis, DynamoDB Streams, MSK/Kafka and Amazon MQ.

**EventBridge Scheduler:** a managed scheduler for **one-off and recurring** schedules (cron, rate, at), with time zones, flexible time windows, retries and DLQs, and direct targets across a large number of AWS APIs; it is built to handle millions of schedules *(verify)*. Use it instead of a cron server and instead of older scheduled EventBridge rules. A good fit for per-user timers ("send a reminder in three days").

### Step Functions: Standard vs. Express, error handling, and when to use them instead of chaining Lambdas?

**Step Functions** runs state machines defined in Amazon States Language (with JSONPath or JSONata): Task, Choice, Parallel, Map, Wait, Pass, Succeed and Fail states. Tasks can call a large number of AWS services directly through **SDK integrations** (DynamoDB, SQS, Bedrock, ECS, Glue...), often with no Lambda at all.

| | Standard | Express |
|---|---|---|
| Maximum duration | 1 year | 5 minutes |
| Semantics | Exactly-once workflow execution | At-least-once (asynchronous) or at-most-once (synchronous) |
| Pricing | Per state transition | Per request plus duration |
| History | Visual execution history kept for 90 days | CloudWatch Logs only |
| Use for | Long-running business processes, human approvals, ML pipelines | High-volume, short event processing (IoT, stream transforms), synchronous API back-ends |

**Error handling:** each state can have `Retry` (which errors, interval, backoff rate, maximum attempts, jitter) and `Catch` (route to a fallback or compensating state). Patterns:
- **Saga** with compensating steps (refund the payment if shipping fails).
- Timeouts and heartbeats on tasks.
- **`.waitForTaskToken`** callbacks to wait for a person or external system.
- **`.sync`** to wait for a Batch, ECS or Glue job to finish.
- **Distributed Map** to process millions of S3 objects in parallel.

**Why not chain Lambdas** (function A calls function B)?
- You pay for A while it waits for B, and timeouts compound.
- Retries, error handling and state are buried in code that operators can't see.
- There is no visual record of where a run failed and no easy resume.

Step Functions makes the workflow explicit, retries declarative, and waiting cheap (Standard charges per transition, not per second of waiting). Use **choreography** (EventBridge or SNS events) when loosely coupled services belong to different teams; use **orchestration** (Step Functions) when one process needs ordering, compensation and visibility. AWS also introduced Lambda durable functions for code-first long-running workflows *(verify)*. Example design: [serverless order pipeline](../../roles/04-solutions-architect/README.md#whiteboard-design-a-serverless-event-driven-order-processing-pipeline).

### API Gateway REST vs. HTTP vs. WebSocket APIs, throttling and authorisers.

| | REST API | HTTP API | WebSocket API |
|---|---|---|---|
| Cost and latency | Higher | Cheaper (often quoted as up to ~70% less *(verify)*) and lower latency | Per message plus connection minutes |
| Features | API keys and **usage plans**, request validation, request/response mapping, caching, **WAF**, private APIs, edge-optimised endpoints, X-Ray, mock integrations | JWT authorisers, Lambda, HTTP and private (VPC Link) integrations, CORS, automatic deployments | Persistent two-way connections; routes chosen by message content; `@connections` API to push to clients |
| Choose when | Public or partner APIs that need per-customer quotas, validation, WAF or private endpoints | Most new internal or simple APIs in front of Lambda or an ALB | Chat, live dashboards, notifications |

**Throttling** uses token buckets at several levels: account per region (10,000 requests per second with a 5,000 burst by default *(verify)*), stage and method settings, and **per client through usage plans** (rate, burst and quota tied to API keys). API keys are for metering and quotas, **not authentication**. Throttled calls get **429**. The integration timeout is 29 seconds by default (REST regional and private APIs can request more *(verify)*) and the payload limit is 10 MB.

**Authorisers:**
- **IAM (SigV4):** callers that have AWS credentials: other services, other accounts.
- **Cognito user pool authoriser** (REST) or **JWT authoriser** (HTTP, any OIDC provider: Cognito, Okta, Entra ID, Auth0).
- **Lambda authoriser:** custom logic on a token or request parameters, returning an IAM policy (REST) or a simple yes/no (HTTP). Cache results by token with a TTL to save cost and latency.
- Plus **resource policies** (limit by source IP, VPC endpoint or account) and **mutual TLS** on custom domains.

Gotchas: the 29-second timeout bites long LLM calls, so use response streaming where supported *(verify for REST APIs)*, an asynchronous job pattern, or an ALB or Lambda function URL instead; and handle CORS for browser clients.

## F. Security & identity

### How does AWS evaluate IAM policies (explicit deny, SCPs, permission boundaries, resource policies and session policies)?

The default is **implicit deny**: a request succeeds only if something allows it and nothing denies it. Simplified order for a request inside one account:

1. **Explicit deny** in any applicable policy → **denied**. Nothing overrides it.
2. **Organisations SCPs** (and **RCPs**, resource control policies) must allow the action for that account. They never grant permissions; they set the **maximum**.
3. **Resource-based policy** (bucket policy, KMS key policy, SQS queue policy, a role's trust policy): if it allows the principal, that can grant access (with nuances for role sessions, and KMS always needs its key policy).
4. **Identity-based policies** on the user or role must allow it otherwise.
5. **Permission boundary**, if set, must also allow it; it caps what identity policies can grant.
6. **Session policy**, if passed when assuming a role or federating, must also allow it; it caps that session.

In short: effective permissions are the intersection of identity policy, boundary, SCP/RCP and session policy, plus resource policy grants within the account, minus any explicit deny.

**Identity-based vs. resource-based:**
- Identity-based policies attach to a principal ("what can I do"). Resource-based policies attach to a resource ("who can touch me"), have a `Principal` element, and are the way to grant access to **another account's principals** and to AWS services.
- **Same account:** either one allowing is enough, provided nothing denies (KMS is the exception: the key policy must allow or delegate to IAM).
- **Cross-account:** **both** must allow: the resource policy in account B trusts the principal in account A, *and* account A's identity policy lets that principal make the call.

Typical uses: SCPs for organisation-wide rules ("only approved regions"), permission boundaries so developers can create roles without escalating privilege, session policies to scope temporary credentials per tenant. Debugging: many AccessDenied messages now say which policy type denied; use the IAM policy simulator, the CloudTrail `errorCode` and IAM Access Analyzer. Basics: [topic 08](../08-cloud-devops-integration/ANSWERS.md#iam-roles-policies-least-privilege-cross-account-access); guardrail design: [Solutions Architect](../../roles/04-solutions-architect/README.md#how-do-you-implement-guardrails-with-preventive-and-detective-controls).

### How do STS AssumeRole and external IDs work, and what is the confused deputy problem?

**AssumeRole:** a principal calls `sts:AssumeRole` with a role ARN. STS checks the role's **trust policy** (who may assume it) and that the caller is allowed to call `sts:AssumeRole`, then returns **temporary credentials** (access key, secret key, session token) valid from 15 minutes up to the role's maximum of 12 hours (default 1 hour; role chaining is limited to 1 hour). Permissions come from the role's policies, optionally narrowed by a session policy.

Variants:
- `AssumeRoleWithWebIdentity`: OIDC tokens, used by EKS IRSA and by GitHub Actions or other CI systems so no keys are stored in CI.
- `AssumeRoleWithSAML`: corporate federation.
- **Session tags** and `sts:SourceIdentity` record who really acted, in CloudTrail.

**The confused deputy:** your SaaS (the deputy) is allowed to assume roles in many customer accounts. An attacker who is also your customer gives you **another customer's role ARN** during onboarding. If you only check the ARN, your service assumes the victim's role and acts on the attacker's behalf.

**External ID** fixes it: you, the vendor, generate a unique, unguessable ID per customer and show it in your onboarding UI; the customer adds an `sts:ExternalId` condition to their role's trust policy. You always pass *that tenant's* ID when assuming *that tenant's* role, so nobody can make you use someone else's role. The external ID isn't really a secret; it binds a tenant to a role. Trust policy example: [topic 08](../08-cloud-devops-integration/ANSWERS.md#iam-roles-policies-least-privilege-cross-account-access).

For AWS services acting on your behalf, the equivalent protection is the `aws:SourceArn` and `aws:SourceAccount` conditions in resource policies. What they listen for: who generates the external ID (the vendor), and temporary credentials instead of access keys.

### What is IAM Identity Center, and why use it instead of IAM users?

**IAM Identity Center** (formerly AWS SSO) is the recommended way for **people** to access AWS accounts and applications.
- Connect the corporate identity provider (Entra ID, Okta, Google Workspace) over SAML, with **SCIM** to sync users and groups automatically, or use its built-in directory.
- Define **permission sets** (bundles of policies such as `ReadOnly`, `DataEngineer`, `Admin`) and assign them to groups for specific accounts. Identity Center creates the matching roles in each account.
- Users sign in once to the access portal and choose an account and role. The CLI uses `aws configure sso` and `aws sso login` for short-lived credentials, so there are no access keys on laptops.
- Joiners and leavers are handled in the identity provider: disable someone in Okta and their AWS access disappears everywhere.
- Supports MFA, attribute-based access control (user attributes passed as session tags), and trusted identity propagation to services such as Redshift and Amazon Q.

**Why not IAM users:** long-lived passwords and access keys per account, manual off-boarding, no central view across 100 accounts, and keys that leak. Keep IAM users only for the rare tool that can't use roles (prefer IAM Roles Anywhere for on-prem servers), and lock down the root user with MFA and no access keys.

Workloads use **roles** (instance profiles, ECS task roles, EKS Pod Identity or IRSA), not Identity Center. Multi-account identity design: [Solutions Architect](../../roles/04-solutions-architect/README.md#how-do-you-centralise-identity-logging-and-security-in-a-multi-account-environment).

### How does KMS envelope encryption work, and what are key policies?

**Envelope encryption:** KMS keys never leave KMS's HSMs, and `Encrypt` accepts only 4 KB. For real data:
1. Call `GenerateDataKey`. KMS returns a **plaintext data key** and the same key **encrypted under your KMS key**.
2. Encrypt the data locally with the plaintext data key (AES-256-GCM), then wipe the plaintext key from memory.
3. Store the **encrypted data key alongside the ciphertext**.
4. To decrypt, send the encrypted data key to KMS `Decrypt` (permission checked and logged in CloudTrail), get the plaintext key back, and decrypt locally.

Benefits: large data is encrypted quickly and locally, only small keys travel to KMS, and access to the data becomes access to `kms:Decrypt`, which you can audit and revoke. S3, EBS and RDS do this for you; in your own code use the AWS Encryption SDK (with data key caching to reduce KMS calls).

**Key types:** AWS owned keys (invisible to you), AWS managed keys (`aws/s3` and similar, no policy control), and **customer managed keys** (your key policy, rotation settings, disable or schedule deletion, cross-account use). For stricter requirements: imported key material, multi-Region keys, CloudHSM-backed custom key stores and external key stores.

**Key policies:** every KMS key has exactly one resource-based key policy, and it is the **primary** control. IAM policies work only if the key policy delegates to the account (the default statement with `"Principal": {"AWS": "arn:aws:iam::111122223333:root"}` does this). Good practice:
- Separate **key administrators** (manage the key, can't use it) from **key users** (encrypt and decrypt).
- Use conditions such as `kms:ViaService` (only through S3 in this region) and `kms:EncryptionContext`.
- For cross-account use, allow the other account in the key policy *and* grant its principals in their IAM policies.
- **Grants** give temporary, programmatic permissions (EBS and RDS use them).

Automatic rotation for customer managed keys (yearly by default, period configurable *(verify)*) keeps old key material so existing data still decrypts. Regulated workload design: [Solutions Architect](../../roles/04-solutions-architect/README.md#design-encryption-and-key-management-for-a-regulated-workload).

### Secrets Manager vs. Parameter Store.

| | Secrets Manager | Systems Manager Parameter Store |
|---|---|---|
| Purpose | Secrets: database passwords, API keys, OAuth client secrets | Configuration, plus some secrets as `SecureString` |
| Rotation | **Built in** (managed for RDS, Aurora, Redshift, DocumentDB; a Lambda function for anything else) | None; do it yourself |
| Cross-region replication | Yes | No |
| Cross-account access | Resource policy | Advanced tier only, through resource sharing *(verify)* |
| Size | 64 KB | 4 KB standard, 8 KB advanced |
| Cost | Per secret per month plus per API call *(verify current prices)* | Standard parameters free; advanced charged |
| Versions | Staging labels (`AWSCURRENT`, `AWSPREVIOUS`) | Version numbers and labels |

How to choose:
- **Secrets Manager** for credentials that need rotation, auditing, and cross-account or cross-region use. RDS can manage its master password in Secrets Manager automatically.
- **Parameter Store** for configuration values, feature flags, AMI IDs and endpoints, and for low-cost secrets that never rotate. Hierarchies (`/prod/payments/db_host`) and `GetParametersByPath` are handy.
- Both encrypt with KMS and log to CloudTrail, and Parameter Store can reference Secrets Manager secrets so applications use one API.

Practical points: cache secrets in the application (AWS caching clients or the Lambda extension) rather than fetching on every request; inject them into ECS through task definitions or into EKS with the Secrets Store CSI driver; during rotation, applications must cope with the old and new credentials both being valid. Secrets handling in general: [topic 08](../08-cloud-devops-integration/ANSWERS.md#how-do-you-store-secrets-vault-aws-secrets-manager-kms).

### Cognito user pools vs. identity pools.

- **User pools = authentication.** A user directory and OIDC/OAuth 2.0 identity provider: sign-up and sign-in, password policies, MFA, account recovery, managed login pages, federation with social and enterprise identity providers (Google, Apple, SAML, OIDC), and Lambda triggers to customise flows (pre-sign-up, pre-token generation). Output: **JWTs** (ID, access and refresh tokens) that your API validates, for example with API Gateway Cognito or JWT authorisers or ALB authentication.
- **Identity pools = authorisation to AWS resources.** They exchange a token (from a user pool, a social provider, SAML, or even an unauthenticated guest) for **temporary AWS credentials** via STS, mapped to IAM roles by rules or attributes. This lets a mobile app upload straight to `s3://bucket/${cognito-identity.amazonaws.com:sub}/*` without a back-end in the middle.

Typical set-up: a user pool for login and JWTs for your API; most apps need nothing more. Add an identity pool only when the client itself must call AWS services.

When not to use Cognito: if the customer already runs Okta, Entra ID or Auth0, federate to that instead of creating a new user store. Cognito is strongest for **customer-facing identity (CIAM)** on AWS. Watch for limits on customising some flows, user migration (use the migration Lambda trigger) and the pricing tiers *(verify current tiers)*. OAuth flows themselves: [topic 08](../08-cloud-devops-integration/ANSWERS.md#oauth-20-flows-authorization-code--pkce-client-credentials-device-code).

### WAF, Shield, GuardDuty, Security Hub, Inspector and Macie — what does each do?

| Service | What it does | One-line pitch |
|---|---|---|
| **AWS WAF** | Layer 7 web ACLs on CloudFront, ALB, API Gateway, AppSync and Cognito: managed rule groups (common exploits, known bad inputs, bot control, account takeover prevention), rate-based rules, IP and geo rules, CAPTCHA | "Block bad HTTP requests" |
| **Shield** | DDoS protection. **Standard** is free and automatic for layer 3/4. **Advanced** is paid: layer 7 mitigation with WAF, the Shield Response Team, and cost protection for scaling during attacks | "Absorb DDoS" |
| **GuardDuty** | Threat **detection** from CloudTrail, VPC Flow Logs and DNS logs, plus protection plans for S3, EKS, RDS logins, Lambda, runtime monitoring and malware scanning; uses threat intelligence and ML (crypto mining, credential exfiltration, calls from Tor) | "Is someone attacking us or already inside?" |
| **Inspector** | **Vulnerability** scanning of EC2 instances, ECR container images and Lambda functions for CVEs and network exposure; SBOM export | "What's unpatched?" |
| **Macie** | Discovers and classifies **sensitive data** (PII, credentials, financial data) in S3; flags public or unencrypted buckets | "Where is our PII?" |
| **Security Hub** | **Aggregates** findings from the services above and partners across accounts, runs posture checks against standards (AWS Foundational Security Best Practices, CIS, PCI DSS), automation rules *(verify: Security Hub was substantially reworked in 2025 around correlated exposures)* | "One place for findings and a compliance score" |

Related services: **Detective** (investigate a finding through graph analysis), **IAM Access Analyzer** (external and unused access), **Config** (resource compliance), **Firewall Manager** (central WAF, security group and Network Firewall policies), **Network Firewall** (stateful inspection at VPC level).

Interview framing: **prevent** (WAF, Shield, SCPs) → **detect** (GuardDuty, Inspector, Macie, Config) → **aggregate and respond** (Security Hub plus EventBridge to raise a ticket or run automated remediation), all enabled organisation-wide with a delegated administrator in a security account.

### A developer's access keys were pushed to a public GitHub repository. What do you do?

Assume they have been used: automated scanners find keys on GitHub within minutes. AWS may also detect the leak, attach the `AWSCompromisedKeyQuarantine` managed policy to the user and open a support case *(verify current policy version name)*.

**Contain (minutes):**
1. **Deactivate** the access key. Don't delete it yet; you want it for the investigation. Give the legitimate workload a replacement, ideally a role rather than a new key.
2. If the user had broad rights, attach an explicit **deny-all** policy, and revoke active sessions on any roles the user could have assumed ("Revoke active sessions" denies tokens issued before that moment).
3. Remove the secret from the repository **and its history** (`git filter-repo` or BFG), but treat it as compromised regardless.

**Investigate:**
4. Search **CloudTrail** for the access key ID in **every region**: what was called, from which IPs, since when. Look for persistence (`CreateUser`, `CreateAccessKey`, `CreateLoginProfile`, `CreateRole`, `AttachUserPolicy`, `UpdateAssumeRolePolicy`), resource abuse (`RunInstances` with GPU types in regions you don't use, new Lambda functions), and data access (`GetObject` on sensitive buckets, `PutBucketPolicy`).
5. Check GuardDuty findings, Cost Explorer for unexpected spend, and Config for changed resources.

**Eradicate and recover:**
6. Delete everything the attacker created (users, keys, roles, instances, functions, key pairs), rotate any other secrets the user could read, then delete the leaked key.
7. Work with AWS Support (they can help with fraudulent charges) and follow your incident process: notify security, and legal or the DPO if personal data may have been accessed.

**Prevent:** no long-lived keys (Identity Center for people, roles and OIDC federation for CI), GitHub secret scanning with push protection, pre-commit hooks (gitleaks), SCPs restricting regions and expensive instance types, GuardDuty and budget alerts across the organisation, and a blameless post-mortem.

What they listen for: deactivate first, CloudTrail across all regions, hunt for persistence, and the systemic fix of removing access keys entirely.

## G. Operations, observability & IaC

### How do you use CloudWatch metrics, logs, alarms and Logs Insights together?

- **Metrics:** time series in namespaces with dimensions. AWS services publish automatically (EC2 every 5 minutes, or every minute with detailed monitoring). Memory and disk usage need the **CloudWatch agent**. Publish custom metrics with `PutMetricData` or, more cheaply, the **Embedded Metric Format** (structured log lines CloudWatch turns into metrics). Every unique combination of dimensions is a separate billed metric, so high-cardinality dimensions get expensive.
- **Logs:** log groups and streams. **Set retention**: the default is never expire, a classic cost leak. Metric filters turn log patterns into metrics (count `ERROR` lines); subscription filters stream logs to Lambda, Firehose or OpenSearch; the Infrequent Access log class is cheaper for logs you rarely query.
- **Logs Insights:** a query language over log groups, charged per GB scanned, so narrow the time range and log groups. Structured JSON logs make it far more useful.

  ```
  fields @timestamp, @message
  | filter status >= 500
  | stats count(*) as errors by bin(5m), route
  | sort errors desc
  ```

- **Alarms:** on a metric or metric maths expression, with OK, ALARM and INSUFFICIENT_DATA states and actions to SNS, Auto Scaling, EC2 or Systems Manager. **Anomaly detection** alarms suit metrics with daily patterns; **composite alarms** reduce noise ("errors high AND latency high"); decide deliberately how to treat missing data.
- **Beyond the basics:** dashboards, **Synthetics** canaries (scripted user journeys), RUM for real users, Container Insights, and Application Signals for service-level SLOs *(verify current feature names)*.

Alert on **symptoms users feel** (error rate, p99 latency, age of the oldest queued message) rather than every CPU spike. Observability in general: [topic 08](../08-cloud-devops-integration/ANSWERS.md#observability-logs-vs-metrics-vs-traces-tools-datadog-prometheus-grafana-opentelemetry).

### CloudTrail vs. CloudWatch vs. AWS Config.

| | CloudTrail | CloudWatch | AWS Config |
|---|---|---|---|
| Question it answers | **Who did what, when, from where?** (API calls) | **How is it performing?** (metrics, logs, alarms) | **What is this resource's configuration, how has it changed, and is it compliant?** |
| Data | Management events (recorded by default, 90-day event history), data events (S3 object access, Lambda invokes; extra cost), Insights events for unusual API activity | Metrics, application and service logs | Configuration items and a timeline per resource, plus relationships |
| Typical use | Security investigations, audit, "who deleted the bucket?" | Operations, alerting, dashboards | Compliance rules ("all EBS volumes encrypted", "no security group open to 0.0.0.0/0 on port 22"), conformance packs, automatic remediation, change history |

Baseline in any serious environment:
- An **organisation trail** to a locked-down S3 bucket in a log archive account, with log file validation; optionally CloudTrail Lake for SQL queries.
- Config enabled in all accounts and regions, with an aggregator.
- CloudWatch alarms or EventBridge rules on critical CloudTrail events: root login, `StopLogging`, SCP changes.

See the [Solutions Architect answer on centralising logging](../../roles/04-solutions-architect/README.md#how-do-you-centralise-identity-logging-and-security-in-a-multi-account-environment). Quick recall: CloudTrail = **audit**, CloudWatch = **monitor**, Config = **inventory and compliance**.

### How do you trace requests across services with X-Ray and ADOT?

- **Distributed tracing:** every request gets a trace ID passed between services (the `X-Amzn-Trace-Id` header or W3C `traceparent`), and each hop records **spans** with timing, errors and metadata. You get a service map and can see where latency comes from ("80% of the time is one DynamoDB call").
- **AWS X-Ray:** AWS's tracing back-end: service map, trace analytics, sampling rules (don't trace 100% of high-volume traffic), and integrations with API Gateway, Lambda (active tracing), ECS, EKS, SNS, SQS and Step Functions.
- **ADOT (AWS Distro for OpenTelemetry):** AWS-supported OpenTelemetry SDKs, auto-instrumentation agents and Collector. Instrument once with **OpenTelemetry** and export traces to X-Ray/CloudWatch *and/or* Datadog, Honeycomb, Jaeger or Grafana. AWS is moving X-Ray instrumentation to OpenTelemetry, with the X-Ray SDKs and daemon heading for maintenance and end of support *(verify dates)*, so recommend OpenTelemetry for new work. CloudWatch Application Signals and Transaction Search build on it *(verify)*.
- **Practical steps:** auto-instrumentation (Java, Python, Node) or Lambda layers; propagate context across asynchronous hops (SQS message attributes); add business attributes (tenant, order ID) to spans; put the trace ID in every log line so logs and traces join up.

What they listen for: OpenTelemetry for portability (important for FDEs, whose customers often have their own observability stack), sampling, and log-trace correlation.

### CloudFormation vs. CDK vs. Terraform — which do you choose, and how do you handle drift and failed stack updates?

| | CloudFormation | CDK | Terraform / OpenTofu |
|---|---|---|---|
| Language | YAML or JSON templates | TypeScript, Python, Java, Go, C#; synthesises CloudFormation | HCL |
| State | Held by AWS (the stack) | Same (CloudFormation) | A state file you store and lock (for example S3) |
| Scope | AWS (plus registry extensions) | AWS | Multi-cloud plus SaaS providers (Datadog, GitHub, Snowflake, Okta) |
| Strengths | No state to manage, automatic rollback, StackSets for organisation-wide deployment | A real programming language, high-level constructs with secure defaults, loops, unit tests | Huge provider ecosystem, readable `plan`, one tool across clouds |
| Weaknesses | Verbose, little logic | Abstractions hide what is created; still bound by CloudFormation limits | State management; providers occasionally lag new AWS features |

How to choose: follow the customer's existing standard first. Terraform when several clouds or SaaS resources are involved (common for FDEs); CDK for AWS-native application teams who want code; plain CloudFormation for StackSets, Control Tower customisations and Service Catalog. Terraform specifics: [topic 08](../08-cloud-devops-integration/ANSWERS.md#terraform-state-planapply-modules-drift).

**Drift** (someone changed a resource in the console): CloudFormation **drift detection** shows the differences per resource (not every resource type or property is covered). Fix it by updating the template to match, or by re-applying the template; prevent it by limiting console write access and with Config rules. In Terraform, `plan` shows drift.

**Failed stack updates:**
- Read the **stack events** for the *first* failure; later failures are often cancellations. Common causes: missing permissions for the deployment role, a resource name that already exists, a service quota, an invalid property, or a custom resource Lambda that never responds (the stack can hang for up to an hour).
- CloudFormation **rolls back** automatically. `UPDATE_ROLLBACK_FAILED` usually means something was changed outside CloudFormation: fix it by hand, then **continue update rollback**, skipping resources if needed.
- Prevention: **change sets** to preview replacements ("Replacement: True" on a database is a red flag), `DeletionPolicy: Retain` or `Snapshot` and `UpdateReplacePolicy` on stateful resources, stack policies, termination protection, smaller stacks, linting (`cfn-lint`, `cdk diff`), and disabling rollback while debugging in dev.

### What does Systems Manager give you (Session Manager, Patch Manager, Run Command)?

**AWS Systems Manager (SSM)** manages EC2 and on-prem servers through the **SSM Agent** (preinstalled on Amazon Linux and many AMIs). Requirements: an instance role with `AmazonSSMManagedInstanceCore` (or the Default Host Management Configuration), and network access to the SSM endpoints, either through NAT or through interface endpoints for `ssm`, `ssmmessages` and `ec2messages` *(verify)*.

- **Session Manager:** a shell on instances from the browser or CLI **with no SSH keys, no bastion and no inbound ports**. IAM decides who can connect (for example by instance tag); sessions are logged to S3 or CloudWatch and in CloudTrail; port forwarding lets you tunnel to a private RDS database for debugging. This is the answer to "how do you access private instances securely" in most security reviews.
- **Patch Manager:** patch baselines (which patches, auto-approval delays), patch policies, **maintenance windows** and compliance reporting for Linux and Windows.
- **Run Command:** run a script or document across a fleet selected by tag, with rate control and error thresholds ("restart the service on 500 instances, 10% at a time").
- **Automation:** runbooks for routine and remediation tasks (AMI patching pipelines, fixing a Config rule violation, incident response steps).
- Also: **State Manager** (keep configuration in place), **Inventory** (installed software), **Fleet Manager**, **Parameter Store** (see [Secrets Manager vs. Parameter Store](#secrets-manager-vs-parameter-store)), and **hybrid activations** to manage on-prem servers the same way. Some SSM capabilities have been renamed, moved or retired over time *(verify current list)*.

Interview framing: Systems Manager replaces SSH, bastions and ad-hoc scripts with audited, IAM-controlled operations, which is a strong point to make to a customer's security team.

### Which AWS cost tools do you use, and how do you make cost visible?

| Tool | Use |
|---|---|
| **Cost Explorer** | Interactive analysis by service, account, region, usage type and tag; forecasts; Savings Plans and RI coverage and utilisation |
| **AWS Budgets** | Alerts on actual or forecast cost, usage or coverage; **budget actions** (apply an SCP or IAM policy, stop instances) |
| **Cost Anomaly Detection** | ML-based alerts on unusual spend per service, account or tag; catches overnight spikes |
| **Cost and Usage Report (CUR 2.0) / Data Exports** | Line-item billing data in S3, queried with Athena or QuickSight; the basis for chargeback and FinOps dashboards |
| **Cost allocation tags** | User-defined tags (`team`, `env`, `customer`) **must be activated** in Billing before they show up; enforce with tag policies and SCPs; group with cost categories |
| **Compute Optimizer** | Right-sizing recommendations from utilisation for EC2, Auto Scaling groups, EBS, Lambda, ECS on Fargate and more *(verify coverage)* |
| **Trusted Advisor / Cost Optimization Hub** | Idle and unattached resources, consolidated savings recommendations |
| **Pricing Calculator** | Estimates before you build |

Making cost visible:
- Tag everything at creation (defaults in IaC) and split accounts by team and environment: the cleanest cost boundary.
- Send budget and anomaly alerts to the **owning team**, not only to finance.
- Review the top movers monthly.
- In multi-tenant SaaS, track unit costs (cost per tenant, per 1,000 requests, per AI answer).

Strategy-level answers: [topic 08](../08-cloud-devops-integration/ANSWERS.md#cost-optimisation-in-the-cloud) and the [Solutions Architect bill review](../../roles/04-solutions-architect/README.md#a-customers-cloud-bill-has-doubled-in-six-months-how-do-you-find-and-remove-the-waste).

## H. AI/ML on AWS

### What does Amazon Bedrock provide, and how do on-demand, batch and provisioned throughput differ?

**Bedrock** is a managed, serverless API for foundation models from several providers (Anthropic Claude, Amazon Nova, Meta Llama, Mistral, Cohere, DeepSeek, OpenAI open-weight models and others *(verify current catalogue per region)*), plus building blocks: Knowledge Bases, Agents and AgentCore, Guardrails, evaluations, fine-tuning and distillation, prompt management.

- **APIs:** `InvokeModel` (provider-specific request body) and the **Converse API** (one request format across models, with tool use and streaming). Recommend Converse for portability between models.
- **Model access:** originally you enabled each model per account and region in the console; AWS has since made most serverless models available by default, with some providers still requiring a use-case form or Marketplace subscription *(verify current process)*. Control which models can be used with IAM and SCPs on model ARNs.
- **Cross-region inference profiles:** route requests to other regions within a geography (US, EU, APAC) or globally, for more throughput. This matters for **data residency**, so check which regions a profile can use *(verify)*.

| Mode | How you pay | Use when |
|---|---|---|
| **On-demand** | Per input and output token, within per-account quotas (tokens and requests per minute) | The default; variable traffic |
| **Batch inference** | Submit JSONL in S3 and collect results in S3, usually within hours; around 50% of the on-demand price *(verify)* | Offline work: classifying a backlog, summarising documents, running evals |
| **Provisioned throughput** | Model units bought for a term (no commitment, 1 month or 6 months *(verify)*), billed hourly | Guaranteed capacity at steady high volume; needed to serve many custom (fine-tuned) models *(verify)* |
| **Service tiers / latency-optimised inference** | Priority and flex tiers, latency-optimised versions of some models *(verify)* | Trading cost against latency |

Cost levers: **prompt caching** for long repeated context, smaller models for routing and classification, intelligent prompt routing, and batch for anything that isn't interactive. FDE action: check model availability in the customer's region and **request quota increases on day one**; quota is a common go-live blocker. Why enterprises choose Bedrock at all: [topic 08](../08-cloud-devops-integration/ANSWERS.md#deploying-llms-via-azure-openai--aws-bedrock--google-vertex-ai--why-do-enterprises-prefer-these).

### How do Bedrock Knowledge Bases work for RAG?

A **Knowledge Base** is managed RAG: ingestion, chunking, embedding, storage and retrieval.

- **Data sources:** S3 (the most common), a web crawler, Confluence, SharePoint, Salesforce, or custom sources through the API. Structured data can be queried with natural language to SQL against Redshift *(verify)*.
- **Ingestion (a sync job):** parses documents (optionally with a foundation model or Bedrock Data Automation for PDFs with tables and images), **chunks** them (fixed size, hierarchical, semantic, or custom through a Lambda function), embeds them with an embedding model (Titan Text Embeddings, Cohere Embed), and writes vectors and metadata to a **vector store**: OpenSearch Serverless, Aurora PostgreSQL with pgvector, Neptune Analytics (GraphRAG), Pinecone, Redis, MongoDB Atlas or S3 Vectors *(verify current list)*.
- **Query:** `Retrieve` returns chunks with scores and sources, and you build the prompt; `RetrieveAndGenerate` also calls the model and returns an answer with **citations**. Options include the number of results, **hybrid search** (with OpenSearch), **metadata filters** (`department = HR`, a tenant ID), **reranking** models and query decomposition.
- **Sync** is incremental, but scheduling it is your job (EventBridge Scheduler, or S3 events triggering `StartIngestionJob`).

Trade-offs to say out loud:
- Fastest route to a working prototype, with less control over chunking, retrieval logic and latency than a custom pipeline. Using `Retrieve` with your own prompt gives back some control.
- **Permissions:** don't assume per-user document permissions from SharePoint or Confluence carry through. Enforce them with metadata filters built from the user's groups, or separate knowledge bases per tenant *(verify current ACL support)*.
- Evaluate retrieval separately from generation (see the next question). RAG concepts: [topic 06](../06-ai-llm/ANSWERS.md#explain-rag-end-to-end).

### What do Bedrock Guardrails and model evaluation do?

**Guardrails** are configurable policies applied to inputs and outputs. You attach them to model calls, Agents and Knowledge Bases, or call the standalone **`ApplyGuardrail` API** on any text, including output from models outside Bedrock.
- **Content filters:** hate, insults, sexual content, violence, misconduct, plus **prompt attack** (jailbreak and prompt injection) detection, each with a strength setting.
- **Denied topics:** topics defined in natural language ("investment advice") that are blocked.
- **Word filters:** profanity and custom word lists.
- **Sensitive information filters:** detect and **block or mask PII** (built-in entity types plus custom regex) in prompts and responses.
- **Contextual grounding check:** scores whether a response is supported by the retrieved sources and relevant to the question, which catches RAG hallucinations.
- **Automated Reasoning checks:** validate answers against formal rules derived from a policy document *(verify availability)*.
- Guardrails are versioned and report which policy intervened, for logging and metrics. IAM condition keys can force every call to use a given guardrail *(verify)*.

**Model evaluation:**
- **Automatic evaluation** on built-in or your own datasets, with metrics for accuracy, robustness and toxicity.
- **LLM-as-a-judge** evaluation with judge models and rubrics (correctness, completeness, helpfulness, harmfulness).
- **Human evaluation** with your own reviewers or an AWS-managed team.
- **RAG evaluation** for Knowledge Bases: retrieval metrics (context relevance and coverage) and generation metrics (faithfulness, citation quality).

What to say: guardrails are one layer. Keep application-level controls too (authorisation, tool restrictions, output validation), and **evaluate on the customer's own golden dataset** before and after every model or prompt change. Guardrail design for regulated chatbots: [topic 06](../06-ai-llm/ANSWERS-part2.md#what-guardrails-would-you-add-for-a-healthcarefinance-chatbot).

### Bedrock Agents vs. Bedrock AgentCore — what is each for?

**Bedrock Agents** (a managed agent configuration): you define instructions, a model, **action groups** (tools implemented as Lambda functions with an OpenAPI or function schema, or "return of control" to your application), Knowledge Bases and Guardrails. Bedrock runs the reasoning loop, session state and traces. It supports multi-agent collaboration (a supervisor with sub-agents). Quick to build; limited control over the loop.

**Bedrock AgentCore** *(verify current scope: launched in 2025 and changing quickly)*: infrastructure to **run agents built with any framework** (Strands Agents, LangGraph, CrewAI, LlamaIndex, OpenAI Agents SDK) using any model, inside or outside Bedrock. Its modules:
- **Runtime:** serverless hosting with each session isolated in its own microVM, long-running sessions (up to hours *(verify)*) and streaming.
- **Memory:** short-term and long-term memory stores.
- **Gateway:** turns APIs, Lambda functions and OpenAPI specs into **MCP** tools, with authentication and tool discovery.
- **Identity:** identities for agents, and OAuth access to third-party services on behalf of users (a token vault).
- **Code Interpreter** and **Browser** tools running in sandboxes.
- **Observability:** OpenTelemetry traces of agent steps in CloudWatch.
- Newer additions such as **Policy** (deterministic rules on tool calls) and **Evaluations** *(verify)*.

How to choose: Bedrock Agents for a straightforward AWS-managed agent with a few tools and RAG; AgentCore when the team already has framework code, needs its own loop, MCP tools or several models, or needs production features such as per-session isolation, delegated identity and observability. A common pattern is to build with Strands or LangGraph locally and deploy on AgentCore Runtime.

What they listen for: you separate the **agent logic** (framework) from the **runtime concerns** (isolation, identity, memory, tool access, observability), and you put guardrails on **actions**, not only on text.

### What does SageMaker provide for training and inference, and which endpoint type do you choose?

**Amazon SageMaker AI** (renamed in late 2024, when "SageMaker" also became the umbrella for Unified Studio across data and AI *(verify naming)*) covers the ML lifecycle: Studio notebooks, **training jobs**, **Pipelines**, **Model Registry**, **Feature Store**, **Model Monitor**, **Clarify** and **JumpStart**.

**Training jobs:** you supply a container (a built-in algorithm, a framework image or your own), data in S3 or FSx, and an instance type and count. SageMaker provisions the instances, runs the job, writes the model artefact to S3 and tears everything down. Options include **managed Spot training** with checkpoints (large savings), distributed training libraries, hyperparameter tuning, and **HyperPod** for resilient large training clusters.

**Inference options:**

| Option | How it works | Use when |
|---|---|---|
| **Real-time endpoint** | Always-on instances behind an HTTPS endpoint, with autoscaling; multi-model endpoints and inference components pack several models onto shared instances | Low latency, steady traffic, GPUs |
| **Serverless inference** | Scales to zero, pay per use, cold starts, CPU only, limited memory (up to about 6 GB *(verify)*) | Intermittent traffic, small models |
| **Asynchronous inference** | Requests queued with payloads in S3 (up to about 1 GB), processing up to about an hour, results to S3 with an SNS notification; can scale to zero *(verify limits)* | Large payloads, long processing (video, big documents) |
| **Batch transform** | An offline job over a dataset in S3; no endpoint | Nightly scoring of a full dataset |

**JumpStart:** a model hub of pre-trained and open-weight foundation models (Llama, Mistral and others) that you deploy to **your own** endpoints in **your** VPC with a click or an SDK call, plus fine-tuning recipes.

What they listen for: matching the endpoint type to latency, traffic shape and payload size, and knowing that real-time endpoints bill by the hour even when idle.

### Bedrock vs. SageMaker vs. self-hosting on EC2/EKS — how do you choose?

| | Bedrock | SageMaker (JumpStart, endpoints) | Self-hosting on EC2/EKS (vLLM, TGI, Triton) |
|---|---|---|---|
| Models | Proprietary (Claude, Nova) and popular open models through an API | Any open-weight or custom model | Anything |
| Operational burden | None (serverless) | Medium: instance choice, scaling | High: GPUs, drivers, serving stack, scaling, patching |
| Pricing | Per token (or provisioned throughput) | Per instance-hour | Per instance-hour (Spot and Savings Plans possible) |
| Control | Limited (no weights, no custom serving) | High | Full (quantisation, batching, custom kernels) |
| Scale to zero | Built in | Serverless and async options only | Hard |
| Best for | Most generative AI apps, frontier models, fast delivery | Custom or fine-tuned open models, classic ML, MLOps teams | Very high steady volume, portability (the same stack on-prem or air-gapped), special hardware needs |

Decision questions:
1. **Do you need a proprietary frontier model?** Then Bedrock: you can't self-host Claude.
2. **A custom or fine-tuned open model?** Bedrock Custom Model Import *(verify supported architectures)* or SageMaker.
3. **Volume and utilisation:** per-token pricing wins at low or variable utilisation; dedicated GPUs pay off only at high sustained utilisation. Work out the break-even with real traffic numbers.
4. **Portability or air-gap:** if the same deployment must run in a customer's on-prem cluster, self-host on EKS with the same images.
5. **Team skills** for operating GPUs.

FDE answer: "Start on Bedrock to prove value in weeks. Move specific high-volume, narrow tasks to a smaller fine-tuned model on SageMaker or EKS only when the numbers justify it." Production LLM architecture: [Solutions Architect](../../roles/04-solutions-architect/README.md#a-customer-wants-to-put-an-llm-application-into-production-which-architecture-and-decisions-do-you-walk-them-through).

### How do you keep customer data private when using Bedrock?

What AWS states *(verify current terms in the Bedrock data protection documentation and service terms)*:
- Prompts and completions are **not used to train** Amazon or third-party models and are **not shared with model providers**. Models run in AWS-operated deployment accounts that providers can't access.
- Bedrock doesn't store prompts or responses by default. **Model invocation logging** is **off** until you turn it on (to your own S3 bucket or CloudWatch).
- Fine-tuning creates a private copy of the model, and training data stays under your account's control.

What you design:
- **Network:** call Bedrock from private subnets through **interface VPC endpoints (PrivateLink)** for `bedrock-runtime` (and `bedrock-agent-runtime`). Endpoint policies restrict which models and accounts can be used; SCPs or IAM conditions can deny calls that don't come through the endpoint (`aws:SourceVpce`).
- **Identity:** an IAM role per application with least privilege on specific model ARNs. Bedrock also supports API keys *(verify)*; restrict or disable them with SCPs if the customer doesn't want them.
- **Encryption:** TLS in transit; **customer managed KMS keys** for Knowledge Bases, agent sessions, custom models, logs and source buckets.
- **Residency:** choose regions and inference profiles that keep processing in the required geography. Geographic cross-region profiles stay within that geography; a global profile can route anywhere *(verify)*.
- **Logging and audit:** CloudTrail records API calls. If you enable invocation logging for debugging or evaluation, treat those logs as sensitive data: KMS, a restricted bucket, retention limits, PII redaction.
- **Guardrails** sensitive information filters mask PII in prompts and responses.
- **Compliance:** confirm Bedrock and the specific models are covered by the programmes the customer needs (HIPAA eligibility, SOC, ISO, FedRAMP, GovCloud availability) *(verify per model and region)*.

What they listen for: you separate **provider guarantees** (no training, no retention) from **controls you implement** (network, IAM, KMS, logging), and you check terms per model and region instead of assuming.

### What is Amazon Q, and where does it fit?

*(verify: this product line has changed names and scope several times.)*
- **Amazon Q Developer:** an AI coding assistant in IDEs and the CLI (code generation, chat, code transformation such as Java version upgrades, security scanning) and in the AWS console (explaining errors, troubleshooting resources, answering cost questions). AWS also launched **Kiro**, an agentic IDE, alongside it *(verify)*.
- **Amazon Q Business:** an enterprise assistant over company data with many connectors (SharePoint, Confluence, Salesforce, S3) that respects source permissions, plus lightweight apps and actions. In 2025 AWS launched **Amazon Quick Suite**, which took over much of Q Business together with QuickSight *(verify)*.
- **Q inside other services:** QuickSight (natural-language BI) and Amazon Connect (contact centre agent assistance).

Where it fits: Q is a **finished application** for internal productivity; buy it when the need is "chat over our documents" or "help our developers". Bedrock is the **platform** for building custom applications. If a customer asks "why build on Bedrock when we have Q?", the answer is control over the user experience, workflow integration, data sources, evaluation and cost.

### Walk me through a production RAG application on AWS, end to end.

Example: an internal assistant over policy documents for 5,000 employees.

1. **Ingestion:** documents arrive in S3 (from SharePoint through a connector, DataSync or uploads). An S3 event goes to EventBridge, which starts a Step Functions workflow: parse (Textract or Bedrock Data Automation for PDFs, tables and scans), clean, attach metadata (department, access groups, version, date), **chunk**, embed with a Bedrock embedding model, and upsert into the vector store. Alternatively, use a **Bedrock Knowledge Base** for ingestion and retrieval if its chunking and connectors fit.
2. **Storage:** OpenSearch Serverless (for hybrid search) or Aurora PostgreSQL with pgvector; raw and parsed documents versioned in S3; DynamoDB for conversation history and feedback.
3. **Front end and API:** users sign in through Cognito or IAM Identity Center federated with Entra ID; the web app is served from S3 through CloudFront; calls go through API Gateway (JWT authoriser) or an ALB to Lambda or an ECS/Fargate service (Fargate suits long streaming responses).
4. **Query path:** rewrite the question → retrieve top-k with **metadata filters derived from the user's groups** (document-level security) → rerank → build the prompt with citations → call Bedrock **Converse** with streaming and a **Guardrail** (PII masking, contextual grounding, prompt-attack filter) → return the answer with links to sources.
5. **Security:** everything in private subnets; interface VPC endpoints for Bedrock and an S3 gateway endpoint; customer managed KMS keys; least-privilege roles per component; WAF at the edge; Secrets Manager for any third-party keys.
6. **Observability and evaluation:** CloudWatch metrics for latency, tokens, cost per request and guardrail interventions; OpenTelemetry traces across retrieval and generation; user feedback stored in DynamoDB; offline evaluation on a golden set (Bedrock RAG evaluation or your own harness) run in CI before any prompt or model change.
7. **Cost and scale:** prompt caching for the system prompt, a smaller model for query rewriting, batch inference for re-embedding, quotas requested early, budgets and per-department cost tags.
8. **Operations:** IaC (CDK or Terraform), a re-ingestion schedule, deletion propagation (remove a document, remove its vectors), runbooks for model version deprecations.

What they listen for: **document-level access control** in retrieval, evaluation, data privacy and cost per answer. Generic RAG design: [topic 06](../06-ai-llm/ANSWERS.md#explain-rag-end-to-end) and the [RAG build exercise](../06-ai-llm/ANSWERS-part2.md#build-a-rag-system-over-a-document-set-using-our-api-include-an-eval).

## I. Scenarios & troubleshooting

### A Lambda function times out when it calls an RDS database. How do you debug it?

Separate **can't connect** from **too slow** from **too many connections**.

1. **Logs and metrics:** in the function's CloudWatch logs, does it time out while connecting or during the query? Compare duration with the configured timeout (the default is only 3 seconds).
2. **Networking (the most common cause):** to reach a private RDS instance, the function must be **attached to the VPC**, in subnets that can route to the database, and the **database security group must allow the function's security group** on 5432 or 3306. A timeout while connecting points to the network path (see ["I can't connect"](#i-cant-connect-to-my-instance--how-do-you-debug-security-groups-nacls-and-routes)).
3. **The VPC function can no longer reach AWS APIs or the internet:** if the same function also calls Secrets Manager, S3 or a third-party API, that call now hangs. A Lambda function in a VPC has **no public IP**, so it needs a **NAT gateway** (from private subnets) or **VPC endpoints** (a Secrets Manager interface endpoint, the S3 gateway endpoint). The classic "it hangs after I put it in the VPC".
4. **Connection exhaustion:** every concurrent execution environment opens its own connection, so a burst to 500 concurrent executions exhausts `max_connections`. Symptoms: "too many connections" errors or slow connects. Fixes: **RDS Proxy** (pools and multiplexes connections, shortens failover, supports IAM authentication), open the connection **outside the handler** so it's reused, and cap the function with **reserved concurrency**.
5. **Slow queries:** Performance Insights or Database Insights, missing indexes, locks, database CPU and I/O metrics.
6. **Death by small delays:** a cold start, DNS lookups and fetching the secret on every call add up. Cache the secret and increase memory.

Answer shape: "Find out whether it's the network, connections or query time. The fixes are security groups, subnets and endpoints; RDS Proxy plus reserved concurrency; and query tuning, respectively."

### An application gets 403 Access Denied from S3. How do you debug it?

Work through the policy layers (see [IAM policy evaluation](#how-does-aws-evaluate-iam-policies-explicit-deny-scps-permission-boundaries-resource-policies-and-session-policies)):

1. **Read the error and confirm the caller.** Newer AccessDenied messages often say which policy type denied the request *(verify for S3)*. Run `aws sts get-caller-identity` from the same environment: often it isn't the role you think (wrong profile, missing instance profile, IRSA or Pod Identity not wired up).
2. **Identity policy:** `s3:GetObject` must be on the object ARN `arn:aws:s3:::bucket/*` and `s3:ListBucket` on the bucket ARN `arn:aws:s3:::bucket`. Mixing these up is the most common bug. Without `ListBucket`, a missing object returns **403 instead of 404**, which sends people looking in the wrong place.
3. **Bucket policy:** explicit denies, for example requiring TLS, a specific VPC endpoint (`aws:SourceVpce`), encryption headers, or membership of the organisation.
4. **KMS:** if objects use **SSE-KMS**, the caller needs `kms:Decrypt` (and `kms:GenerateDataKey` for writes), and the **key policy** must allow it. Cross-account access almost always breaks here.
5. **Cross-account:** the bucket policy and the caller's identity policy must both allow it. Objects uploaded by another account while ACLs were enabled may not belong to the bucket owner; set Object Ownership to bucket owner enforced.
6. **Guardrails:** SCPs and RCPs, permission boundaries, session policies, and **VPC endpoint policies** (a gateway endpoint policy that allows only certain buckets).
7. **Block Public Access:** if the caller is anonymous (a public website), BPA is doing its job. Serve through CloudFront with OAC instead.
8. **Presigned URLs:** expired, signed by a principal without permission, or signed with temporary credentials that expired before the URL did.

Tools: CloudTrail data events (`errorCode: AccessDenied` with the principal), the IAM policy simulator, IAM Access Analyzer.

### An EC2 instance in a private subnet can't reach the internet. What do you check?

1. **The private subnet's route table:** `0.0.0.0/0` must point to a **NAT gateway**, not the internet gateway (an instance without a public IP can't use an internet gateway). Check the route table is associated with the *right* subnet.
2. **The NAT gateway:** state Available, in a **public** subnet whose route table sends `0.0.0.0/0` to the **internet gateway**, with an Elastic IP. Use one NAT gateway per AZ; if an AZ's NAT gateway goes, that AZ loses egress.
3. **Security group outbound rules:** the default allows all outbound, but hardened groups may not allow 443 or 80.
4. **NACLs** on both the private and public subnets: outbound 443 and inbound **ephemeral ports** for the replies.
5. **DNS:** `enableDnsSupport` on the VPC; a custom DHCP options set pointing at an unreachable DNS server; Route 53 Resolver DNS Firewall blocking the domain.
6. **IPv6:** needs an **egress-only internet gateway** rather than NAT.
7. **Centralised egress:** in enterprise accounts the default route often goes to a **Transit Gateway** and an inspection VPC, Network Firewall or proxy that blocks the domain. Ask the network team, and check `HTTP_PROXY` settings.
8. **Test:** `curl -v https://example.com`, `dig`, Reachability Analyzer, Flow Logs on the instance's network interface and the NAT gateway's.

Also ask whether it really needs the internet. For AWS services, **VPC endpoints** are cheaper and safer than NAT (the S3 and DynamoDB gateway endpoints are free). Background: [topic 08](../08-cloud-devops-integration/ANSWERS.md#vpc-subnets-publicprivate-security-groups-nacls-nat-gateways).

### A DynamoDB table is throttling. How do you diagnose and fix it?

1. **Which operations, and where:** CloudWatch `ThrottledRequests`, `ReadThrottleEvents` and `WriteThrottleEvents` on the table **and on each GSI**, and consumed vs. provisioned capacity. **CloudWatch Contributor Insights for DynamoDB** shows the most accessed and most throttled keys.
2. **Causes and fixes:**
   - **Provisioned capacity too low, or auto scaling too slow** for spikes: raise the minimum, schedule scaling before known peaks, or switch to **on-demand**.
   - **An on-demand table scaling too fast** (traffic more than doubling its previous peak in a short time) *(verify current behaviour)* or hitting a configured maximum: pre-warm with the warm throughput setting *(verify)* or raise the limit.
   - **A hot partition or key** above about 1,000 WCU or 3,000 RCU: write sharding, caching with DAX, or a key redesign (see [DynamoDB key design](#how-do-you-design-a-dynamodb-table-partition-and-sort-keys-single-table-design-and-hot-partitions)).
   - **A throttled GSI pushing back on the base table:** give the GSI enough capacity and project fewer attributes.
   - **Scans** consuming capacity: replace them with queries or GSIs, use exports for analytics, and rate-limit batch jobs.
   - **Large items:** a 400 KB item costs 400 WCUs per write, so move large attributes to S3.
3. **Client side:** SDKs retry throttled requests with backoff; make sure retries are enabled and jittered, and that `UnprocessedItems` from batch writes are retried.
4. **Quotas:** check per-table and account throughput quotas *(verify current defaults)*.

What they listen for: checking GSIs and hot keys before "just add capacity".

### API latency spiked right after a deploy. What do you do?

1. **Mitigate first.** If the timing matches the deploy and users are affected, **roll back**: CodeDeploy automatic rollback on alarms, the ECS deployment circuit breaker, moving a Lambda alias back, or flipping the weighted target group. Investigate once users are fine. See [rolling back a bad deploy](../08-cloud-devops-integration/ANSWERS.md#how-do-you-roll-back-a-bad-deploy).
2. **Confirm the scope.** Every endpoint or one? All AZs and instances, or only the new ones? p50 or only p99? Use ALB `TargetResponseTime` by target group and compare traces (X-Ray or OpenTelemetry) from before and after.
3. **The usual suspects after a deploy on AWS:**
   - **New code:** N+1 queries, a removed cache, a new synchronous call, bigger payloads, debug logging.
   - **Cold starts:** a new Lambda version without provisioned concurrency on the new alias, a larger package, JVM warm-up in fresh containers.
   - **Capacity:** new tasks with less CPU or memory, fewer tasks during a rolling deploy, connection pools reset all at once (a connection storm on the database).
   - **Configuration:** an endpoint in the wrong region (cross-region calls), timeout and retry settings causing retry storms, a feature flag switched on, a missing VPC endpoint sending traffic through NAT.
   - **Database:** a migration that took locks or dropped an index; changed query plans.
   - **Dependencies:** Bedrock or third-party throttling (429s and retries) showing up as latency.
4. **Prevent it next time:** canary or linear deployments with automatic rollback on latency and error alarms, load tests in the pipeline, SLO-based alerts. See [blue-green vs. canary](../08-cloud-devops-integration/ANSWERS.md#blue-green-vs-canary-vs-rolling-deployments) and [latency debugging](../12-debugging-troubleshooting/ANSWERS.md#latency-jumped-from-2s-to-15s).

### The AWS bill spiked overnight. How do you find out why?

1. **Cost Explorer** at daily granularity for the last week: group by **service**, then **linked account**, **region**, **usage type** and **tag** until you find the line that jumped. Check **Cost Anomaly Detection**, which often names the cause already.
2. **Map the usage type to a cause:**
   - `BoxUsage` in an unusual region: instances someone launched; check CloudTrail `RunInstances`. GPU instances in regions you don't use usually mean **compromised credentials** (see [leaked access keys](#a-developers-access-keys-were-pushed-to-a-public-github-repository-what-do-you-do)).
   - `NatGateway-Bytes` or `DataTransfer-Out`: a job moving data through NAT or out to the internet. Add VPC endpoints; find the source with VPC Flow Logs.
   - Lambda or API Gateway requests: a **recursive loop** (a function writes to the bucket that triggers it; SQS → Lambda → the same queue). Lambda's recursive loop detection stops some of these *(verify coverage)*.
   - CloudWatch Logs ingestion: debug logging left on, or a crash loop flooding logs.
   - S3 requests: a crawler or a misconfigured client listing millions of objects.
   - Bedrock tokens: a retry loop or a runaway agent.
   - DynamoDB on-demand: a scan or a load test.
3. **Stop the bleeding:** scale down, disable the trigger, set reserved concurrency to 0, or deactivate keys; then fix it properly.
4. **Prevent:** budgets with alerts (and actions) per account, anomaly detection subscriptions that reach owners, SCPs blocking unused regions and large instance types, tagging, and limits on concurrency and maximum Auto Scaling group size.

More: [costs doubled](../12-debugging-troubleshooting/ANSWERS.md#costs-doubled-this-month-find-out-why) and the [Solutions Architect bill review](../../roles/04-solutions-architect/README.md#a-customers-cloud-bill-has-doubled-in-six-months-how-do-you-find-and-remove-the-waste).

### Design a serverless image-processing pipeline.

Clarify first: volume (1,000 a day or a million an hour?), image sizes, outputs needed (thumbnails, moderation, OCR, AI captions), latency expectations, who uploads (the public?), retention.

1. **Upload:** the client asks the API (API Gateway and Lambda, authenticated with Cognito) for an **S3 presigned PUT or POST** with content-type and size limits, then uploads **directly to S3** (an `uploads` bucket). No image bytes pass through the API.
2. **Trigger:** an S3 event notification to **EventBridge** or directly to an **SQS queue** in front of the processing function, for buffering, retries and a **DLQ**.
3. **Processing:** a Lambda function with enough memory (1–3 GB for image libraries such as Pillow or sharp, packaged in a layer or container image) creates thumbnails and WebP versions, strips EXIF metadata and writes to a **separate output bucket**. Never write back to the bucket or prefix that triggers the function: that is the infinite loop. For several steps (resize → Rekognition moderation → Bedrock caption → Textract OCR), use **Step Functions** with retries per step.
4. **Metadata:** a DynamoDB table (`imageId` → owner, status, variants, labels); the client learns the status by polling, a WebSocket API or AppSync subscriptions.
5. **Delivery:** CloudFront in front of the output bucket with **OAC**; signed URLs or cookies for private images; sensible cache headers.
6. **Reliability:** idempotent processing (deterministic output keys, conditional DynamoDB writes), partial batch failure reporting, reserved concurrency to protect downstream quotas (Rekognition, Bedrock), alarms on DLQ depth and queue age.
7. **Security and cost:** private buckets, KMS, malware scanning (GuardDuty Malware Protection for S3), lifecycle rules (delete originals after N days or move them to Glacier Instant Retrieval), Graviton (arm64) functions.
8. **When to leave Lambda:** very large images or video go to ECS/Fargate tasks or AWS Batch (15-minute and memory limits); bulk reprocessing of existing images uses S3 Batch Operations or a Step Functions Distributed Map.

What they listen for: presigned uploads, the recursive trigger trap, queue plus DLQ, idempotency, and knowing when to move off Lambda. A related design: [serverless order pipeline](../../roles/04-solutions-architect/README.md#whiteboard-design-a-serverless-event-driven-order-processing-pipeline).

### Migrate a server that runs a dozen cron jobs to AWS.

1. **Inventory first:** `crontab -l` for every user, `/etc/cron.*` and systemd timers. For each job record what it does, its schedule, run time, CPU and memory, inputs and outputs (files, databases, APIs), credentials, what happens if it runs twice or not at all, and who is alerted today (often nobody). Cron basics: [topic 08](../08-cloud-devops-integration/ANSWERS.md#cron-jobs-and-systemd-services).
2. **Classify and choose a target per job:**

   | Job type | Target |
   |---|---|
   | Short (under 15 minutes), light, stateless | **EventBridge Scheduler → Lambda** |
   | Longer, or needs a container or OS tools | **EventBridge Scheduler → ECS task on Fargate** (or AWS Batch for heavy jobs) |
   | Several steps with dependencies ("B after A") | **Step Functions** started by Scheduler |
   | Operations commands on servers that remain | **Systems Manager** maintenance windows or State Manager |
   | Can't be changed yet | Rehost on one small EC2 instance first, modernise later |

3. **Remove hidden dependencies:** local disk → S3 or EFS; a local database → RDS; hard-coded passwords → Secrets Manager; the server's time zone → set the time zone explicitly in Scheduler; partner IP allowlists → NAT with an Elastic IP.
4. **Make jobs safe to retry and to overlap:** idempotent outputs, and a lock for "only one at a time" (neither Scheduler nor ECS prevents overlapping runs; use a DynamoDB lock or Step Functions); retries and a DLQ on each schedule.
5. **Observability, the big upgrade:** logs in CloudWatch, alarms on failures **and on missed runs** (publish a "job succeeded" heartbeat metric and alarm when it's missing), and notifications to the owning team.
6. **Cut over job by job:** run old and new in parallel (dry-run, or read-only jobs), compare outputs, switch, and decommission the server at the end.
7. **IaC:** define schedules, functions and alarms in CDK or Terraform so the old "cron server" becomes reviewable code.

What they listen for: inventory and classification before tools, idempotency and overlap, and alerting on jobs that silently don't run. Related: [the 7 Rs](../../roles/04-solutions-architect/README.md#explain-the-7-rs-of-migration-and-how-you-choose-between-them) and [a cron job didn't run](../12-debugging-troubleshooting/ANSWERS.md#a-cron-job-didnt-run).
