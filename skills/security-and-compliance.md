# Security and compliance

At senior and principal level you are expected to design secure systems by default and to talk credibly with a customer's security and compliance teams, not hand everything to a separate security group. Forward Deployed Engineer, Software Architect and Agentic-AI Architect roles ask most, especially in regulated industries and wherever LLM agents touch real data or tools.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [AI / LLM](../topics/06-ai-llm/README.md) · [Customer scenarios](../topics/09-customer-scenarios/README.md) · [System design](../topics/03-system-design/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Authentication vs. authorisation, and where each happens in a request through an API gateway.
- [ ] Walk through the OWASP Top 10 and give one real example of the top three *(verify)*.
- [ ] SQL injection, XSS, CSRF and SSRF: how each works and the main defence for each.
- [ ] Hashing vs. encryption vs. encoding. How should passwords be stored?
- [ ] Encryption at rest vs. in transit; what does TLS actually protect?
- [ ] OAuth 2.0 vs. OIDC vs. SAML: what problem does each solve?
- [ ] JWTs: what is inside one, how is it validated, and what are the common mistakes?
- [ ] What is least privilege, and what is defence in depth?

## B. Working depth
- [ ] Session cookies vs. bearer tokens for a web app: security trade-offs, cookie flags, token storage.
- [ ] RBAC vs. ABAC vs. relationship-based access. How do you enforce tenant isolation in a multi-tenant database?
- [ ] Broken object-level authorisation (IDOR): how do you find it and prevent it systematically?
- [ ] Secrets management: where secrets live, how they reach the app, rotation, and what to do after a leak.
- [ ] Envelope encryption with KMS: how does it work and why use it?
- [ ] Rate limiting and abuse prevention for public APIs and login endpoints.
- [ ] Software supply chain: dependency pinning, lockfiles, SCA scanning, typosquatting, SBOMs.
- [ ] Secure SDLC: what security checks belong in design review, code review, CI and production?
- [ ] Audit logging: what to log, what never to log (PII, secrets), and how to make logs tamper-evident.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Run a threat model (for example STRIDE) for a new feature. Who takes part and what comes out of it?
- [ ] SOC 2 Type I vs. Type II and ISO 27001: what do they require from engineering day to day?
- [ ] GDPR and India's DPDP Act basics: lawful basis or consent, data subject rights, breach notification, cross-border transfer *(verify)*.
- [ ] How do you implement "right to erasure" when data sits in backups, logs, analytics and a vector index?
- [ ] Prompt injection, direct and indirect: why is it hard to fix, and what controls reduce the impact?
- [ ] An LLM agent can call tools that write data or spend money. How do you limit what it can do (scoped credentials, allow-lists, human approval, output validation)?
- [ ] Data leakage through LLMs: sensitive data in prompts, logs, fine-tuning and retrieval across tenants.
- [ ] Balancing security against delivery speed: how do you push back on a risky shortcut without blocking the team?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] An engineer committed a production database password to a public repo an hour ago. What do you do, in order?
- [ ] Design authentication and authorisation for a B2B SaaS with SSO, SCIM provisioning, roles and per-tenant admins.
- [ ] A bank's security team sends a 300-question vendor questionnaire. How do you handle it and what gaps usually appear?
- [ ] Design a RAG assistant that respects document-level permissions and resists indirect prompt injection.
- [ ] Your company needs SOC 2 in six months. What does engineering need to change?

## E. Hands-on checks (things to be able to DO)
- [ ] Find and fix an IDOR and an SQL injection in a small Express or similar API.
- [ ] Validate a JWT correctly: signature, algorithm, issuer, audience, expiry, key rotation via JWKS.
- [ ] Set up secret scanning and dependency scanning in CI and triage the first report.
- [ ] Write a prompt-injection test set for an agent and show which mitigations stop which attacks.

---

**Good answers show:**
- Security designed in from the start, with threat models tied to real assets and attackers.
- Defence in depth with clear priorities, not a checklist of every control.
- Compliance treated as evidence of good practice, with a realistic view of the engineering work it creates.
- For LLM systems: accepting that prompt injection cannot be fully prevented, so limiting what the agent is allowed to do.
- Calm, ordered incident handling: contain, rotate, assess impact, notify, fix the root cause.
- Being able to explain risk to non-security stakeholders in business terms.

**Prove it:** Build a small multi-tenant API with OIDC login, per-tenant RBAC and an audit log, add secret and dependency scanning to CI, then add an LLM tool-calling endpoint and a red-team test set for prompt injection. Write a one-page threat model with the mitigations you shipped.
