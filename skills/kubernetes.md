# Kubernetes

Kubernetes is the default runtime for containerised services in most product and platform teams, so senior candidates are expected to reason about it beyond `kubectl apply`: scheduling, failure modes, upgrades, security and cost. Platform, Staff/Principal backend and Forward Deployed Engineer roles ask most, because FDEs often deploy into a customer's cluster they do not control.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [System design](../topics/03-system-design/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What problem does Kubernetes solve that Docker Compose or plain VMs with an autoscaling group do not?
- [ ] Pod vs. container vs. Deployment vs. ReplicaSet — how do they relate, and why do you rarely create a bare Pod?
- [ ] Walk through the control plane: API server, etcd, scheduler, controller manager, and what the kubelet does on each node.
- [ ] Service types: ClusterIP, NodePort, LoadBalancer, headless — when would you use each?
- [ ] Ingress vs. a LoadBalancer Service vs. the Gateway API — what does each give you? *(verify)*
- [ ] ConfigMap vs. Secret: how are they mounted, and why is a Secret not encrypted by default?
- [ ] Liveness vs. readiness vs. startup probes — what happens when each one fails?
- [ ] Resource requests vs. limits; what are the QoS classes (Guaranteed, Burstable, BestEffort)?
- [ ] Deployment vs. StatefulSet vs. DaemonSet vs. Job/CronJob — give one real use for each.
- [ ] What is a namespace for, and what does it not isolate?

## B. Working depth
- [ ] How does a rolling update work (`maxSurge`, `maxUnavailable`), and how do you roll back?
- [ ] How does the Horizontal Pod Autoscaler decide to scale, and why does it need requests set? What about custom or external metrics?
- [ ] What happens when a container exceeds its memory limit vs. its CPU limit? Why do some teams avoid CPU limits?
- [ ] PersistentVolume, PersistentVolumeClaim and StorageClass — how does dynamic provisioning work, and what does `ReclaimPolicy` change?
- [ ] How does DNS-based service discovery work inside a cluster (`svc.namespace.svc.cluster.local`)?
- [ ] How do taints/tolerations, node affinity and pod anti-affinity differ? When would you use topology spread constraints?
- [ ] What is a PodDisruptionBudget, and what goes wrong during node drains without one?
- [ ] RBAC: Role vs. ClusterRole, RoleBinding vs. ClusterRoleBinding, and ServiceAccounts for workloads.
- [ ] Helm: what are charts, values and releases? What are the pain points of Helm vs. Kustomize?
- [ ] How does graceful shutdown work (SIGTERM, `terminationGracePeriodSeconds`, preStop hooks), and why do Node.js services drop requests during deploys?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How does pod networking work (CNI, one IP per pod), and what changes with an overlay network vs. VPC-native IPs on EKS/GKE? *(verify)*
- [ ] NetworkPolicies: default-deny, egress control, and why policies silently do nothing without a CNI that enforces them.
- [ ] Multi-tenancy: namespace-per-tenant vs. cluster-per-tenant vs. virtual clusters — isolation, blast radius and cost trade-offs.
- [ ] How do you manage secrets properly: external secret operators, cloud secret managers, envelope encryption of etcd, workload identity (IRSA / GKE Workload Identity)? *(verify)*
- [ ] Rollout strategies beyond rolling: blue-green and canary with a service mesh or progressive delivery controller — what does each need?
- [ ] How do you run a cluster version upgrade with zero downtime, including deprecated API removals? *(verify)*
- [ ] Right-sizing and cost: bin-packing, cluster autoscaler vs. Karpenter-style node provisioning, spot nodes, and over-provisioned requests. *(verify)*
- [ ] Managed (EKS/GKE/AKS) vs. self-managed: what do you still own on a managed control plane? GKE Autopilot vs. Standard? *(verify)*
- [ ] Should a stateful database run in Kubernetes? Argue both sides with operators, storage and failover in mind.
- [ ] GitOps (Argo CD / Flux) vs. push-based CI deploys — drift, audit and rollback trade-offs.
- [ ] Pod security: running as non-root, read-only root filesystem, Pod Security Standards, image signing and admission control. *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A pod is in `CrashLoopBackOff`. Walk through how you find the cause, step by step.
- [ ] Pods are `Pending` for ten minutes after a deploy. What are the possible reasons, and how do you tell them apart?
- [ ] Latency spikes every few minutes on a Node.js service in Kubernetes with no error logs. What do you check (CPU throttling, GC, DNS, probes)?
- [ ] Design the move of a set of Docker Compose / ECS microservices behind an API gateway onto EKS or GKE: what changes, in what order, and what is the rollback plan?
- [ ] Design a multi-tenant SaaS platform on Kubernetes where a few enterprise customers need dedicated compute.
- [ ] You must ship your product into a customer's locked-down or air-gapped cluster. How do you package, configure and upgrade it?
- [ ] A queue-driven worker needs to scale on SQS or Kafka lag rather than CPU. How would you design that?

## E. Hands-on checks (things to be able to DO)
- [ ] Write a Deployment + Service + HPA manifest from memory, with probes, requests/limits and a PodDisruptionBudget.
- [ ] Use `kubectl describe`, `logs --previous`, `get events`, `exec` and `port-forward` to debug a failing pod.
- [ ] Create a Role, RoleBinding and ServiceAccount that let a CI job deploy to one namespace only.
- [ ] Package a service as a Helm chart with per-environment values files, then `helm upgrade` and `helm rollback` it.
- [ ] Write a default-deny NetworkPolicy plus an allow rule for one service-to-service call, and prove it works.
- [ ] Spin up a local cluster (kind or minikube), install an ingress controller and route two hostnames to two services.

---

**Good answers show:**
- Reasoning from the reconciliation loop (desired vs. actual state) rather than reciting object names.
- Failure-mode thinking: what happens on node loss, bad probes, OOM kills, drains and control-plane outages.
- Clear cost awareness: requests drive scheduling and spend; over-provisioning is the usual waste.
- Security as default: least-privilege RBAC, workload identity instead of static keys, network policies, non-root images.
- Honest scoping of when Kubernetes is overkill and a managed container service (ECS, Cloud Run) is the better call.
- Operational maturity: GitOps, upgrade plans, PDBs, and observability wired in from day one.

**Prove it:** Take an existing Node.js + PostgreSQL + Redis service and run it on a local kind cluster, then on a small managed cluster: Helm chart, readiness/liveness probes, HPA on a custom metric, a NetworkPolicy, secrets from a cloud secret manager, and a scripted canary rollout with rollback. Write a short runbook of three failures you caused on purpose and how you diagnosed each.
