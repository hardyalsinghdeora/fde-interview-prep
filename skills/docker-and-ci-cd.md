# Docker and CI/CD

Shipping safely and often is what separates senior platform engineers from people who only write features, so interviewers probe how you build images, design pipelines and get changes into production without outages. Principal/Staff Engineer, Engineering Manager and Forward Deployed Engineer roles ask most, because they own release processes or deploy into customer environments.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Image vs. container vs. registry. What actually is a container at the Linux level (namespaces, cgroups)?
- [ ] How do image layers and the build cache work? Why does instruction order in a Dockerfile matter?
- [ ] What is a multi-stage build and why does it shrink images?
- [ ] `CMD` vs. `ENTRYPOINT`; shell form vs. exec form, and why PID 1 and signal handling matter.
- [ ] Continuous integration vs. continuous delivery vs. continuous deployment.
- [ ] Blue/green vs. canary vs. rolling deployments: one sentence on each.
- [ ] What is a feature flag, and how is it different from a deployment?
- [ ] What is GitOps?

## B. Working depth
- [ ] Choosing a base image: distroless vs. Alpine vs. slim Debian. What breaks with musl?
- [ ] Why run as a non-root user, and how do you do it in a Dockerfile?
- [ ] How do you pass build-time secrets without baking them into a layer?
- [ ] Tags vs. digests. Why is deploying `latest` dangerous?
- [ ] How do you keep a Node.js image small and the build fast (lockfile caching, `npm ci`, pruning dev deps)?
- [ ] Image scanning: what do tools like Trivy or Grype find, and how do you handle a critical CVE with no fix? *(verify)*
- [ ] What stages belong in a pipeline for a backend service, and which should block a merge?
- [ ] How do you keep CI fast as the repo grows: caching, parallelism, test splitting, affected-only builds?
- [ ] How do you run database migrations in a pipeline without breaking the old version still serving traffic?
- [ ] How do you promote the same artefact through dev, staging and production instead of rebuilding?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Canary analysis: which metrics decide promote vs. roll back, and how long do you wait?
- [ ] When is rollback impossible (destructive migrations, external side effects), and how do you design for roll-forward?
- [ ] Expand/contract migrations: walk through renaming a column on a live table with zero downtime.
- [ ] Feature flag debt: how do you stop thousands of stale flags, and who owns cleaning them up?
- [ ] Monorepo vs. polyrepo for CI/CD: build times, versioning, ownership, blast radius.
- [ ] Supply chain: signing images, SBOMs, provenance and pinned dependencies. What is worth the effort for a mid-size team? *(verify)*
- [ ] Push-based CD vs. pull-based GitOps (Argo CD, Flux): trade-offs in security and drift detection.
- [ ] How do you measure delivery performance (DORA metrics) without turning them into targets that get gamed?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design the CI/CD platform for 30 microservices behind an API gateway, from pull request to production.
- [ ] A deploy at 6 p.m. raised error rates by 3%. Walk through detection, decision and rollback.
- [ ] Your pipeline takes 45 minutes and engineers batch changes. How do you get it under 10?
- [ ] Design a release process for an on-prem or air-gapped customer who cannot pull from your registry.
- [ ] Roll out a risky billing change to 1% of tenants first, with a kill switch.

## E. Hands-on checks (things to be able to DO)
- [ ] Write a multi-stage Dockerfile for a TypeScript service that runs as non-root and is under 150 MB.
- [ ] Write a GitHub Actions (or GitLab CI) workflow: lint, test, build, scan, push by digest, deploy to staging.
- [ ] Use `docker history` or a layer inspector to explain why an image is 1 GB.
- [ ] Implement a percentage-based feature flag with per-tenant overrides.
- [ ] Write a Kubernetes rolling update or canary with readiness probes and show a rollback.

---

**Good answers show:**
- Treating the build artefact as immutable and promoting it, not rebuilding per environment.
- Deploy and release kept separate: flags, canaries and quick rollback as standard practice.
- A clear plan for schema changes and other irreversible steps, not just "we roll back".
- Security built into the pipeline (scanning, signing, least-privilege deploy credentials) at a cost the team can sustain.
- Pipeline speed and developer experience treated as product metrics.
- Concrete numbers from experience: build time, deploy frequency, change failure rate.

**Prove it:** Take a small TypeScript API, write a multi-stage non-root Dockerfile, a CI workflow that tests, scans, signs and pushes by digest, and a canary deploy to a local Kubernetes cluster (kind or k3d) with an automated rollback when an error-rate check fails. Write up before/after image size and pipeline time.
