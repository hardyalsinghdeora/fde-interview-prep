# 08 — Cloud, DevOps & Integration: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers, not scripts. Adapt them to your own experience and the stack you have actually used, and practise saying them out loud. For FDE roles, always tie the answer back to "what this means when I'm deploying into a customer's environment".

---

## A. APIs & integration

### REST vs. GraphQL vs. gRPC vs. SOAP — when would you use each?

| Style | Strengths | Weaknesses | Use when |
|---|---|---|---|
| **REST** (JSON over HTTP) | Universal, cacheable, easy to debug with `curl`, great tooling (OpenAPI) | Over/under-fetching, many round trips for nested data | Default for public and partner APIs, CRUD-shaped resources |
| **GraphQL** | Client picks exact fields, one round trip for nested data, strong schema | Harder HTTP caching, N+1 resolver problems, query-cost abuse, auth per field | Many front-ends with different data needs (web, mobile), aggregating several back-ends |
| **gRPC** (Protobuf over HTTP/2) | Fast binary encoding, strict contracts, streaming both ways, codegen | Poor browser support (needs gRPC-Web), harder to inspect, not cache-friendly | Internal service-to-service calls, low latency, polyglot microservices |
| **SOAP** (XML + WSDL) | Formal contracts, WS-Security, transactions | Verbose, heavy tooling | Only when the customer's system already speaks it (banks, SAP, older government systems) |

FDE angle: you rarely choose — the customer's system chooses for you. Expose REST for your own public API, use gRPC internally if latency matters, and write adapters for whatever SOAP or GraphQL endpoints the customer already has.

### Design a REST API for X (resources, verbs, status codes, pagination, versioning).

Worked example — X = a document-processing service.

- **Clarify:** who calls it (customer back-ends, UI), volumes, sync vs. async processing, auth model.
- **Resources (nouns, plural):** `/v1/documents`, `/v1/documents/{id}`, `/v1/documents/{id}/extractions`, `/v1/jobs/{id}`.
- **Verbs:**
  - `POST /v1/documents` → create (201 + `Location` header). Accept `Idempotency-Key`.
  - `GET /v1/documents?status=done&limit=50&cursor=...` → list.
  - `GET /v1/documents/{id}` → 200 or 404.
  - `PATCH /v1/documents/{id}` → partial update (200); `PUT` for full replace.
  - `DELETE /v1/documents/{id}` → 204.
  - Long work: `POST /v1/documents/{id}/extractions` → 202 Accepted + `/v1/jobs/{id}` to poll, or a webhook on completion.
- **Status codes:** 200/201/202/204 success; 400 malformed, 401 no/invalid auth, 403 forbidden, 404, 409 conflict (version mismatch), 422 validation, 429 rate limit with `Retry-After`; 5xx server.
- **Errors:** consistent body, e.g. RFC 9457 problem+json: `{"type","title","status","detail","request_id"}`.
- **Pagination:** cursor-based, response `{"data":[...],"next_cursor":"..."}`.
- **Filtering/sorting:** `?status=`, `?sort=-created_at`.
- **Versioning:** URL major version (`/v1`), additive changes only within a version, deprecation headers before removal.
- **Cross-cutting:** OAuth2 bearer tokens, rate limits per client, `ETag`/`If-Match` for optimistic concurrency, request IDs for tracing, OpenAPI spec as the contract.

### HTTP status codes: 200, 201, 204, 301, 302, 304, 400, 401, 403, 404, 409, 422, 429, 500, 502, 503, 504.

- **200 OK** — success with a body.
- **201 Created** — resource created; include `Location`.
- **204 No Content** — success, no body (DELETE, some PUTs).
- **301 Moved Permanently** — clients/search engines should update the URL; cacheable.
- **302 Found** — temporary redirect (use 307/308 when the method must be preserved).
- **304 Not Modified** — reply to a conditional GET (`If-None-Match`/`If-Modified-Since`); client uses its cache.
- **400 Bad Request** — malformed request (bad JSON, missing param).
- **401 Unauthorized** — really "unauthenticated": missing/invalid/expired credentials.
- **403 Forbidden** — authenticated but not allowed.
- **404 Not Found** — no such resource (also used to hide existence from unauthorised callers).
- **409 Conflict** — state conflict: duplicate, version mismatch, idempotency key reused with a different body.
- **422 Unprocessable Content** — syntactically valid but fails validation rules.
- **429 Too Many Requests** — rate limited; send `Retry-After`.
- **500 Internal Server Error** — unhandled bug on the server.
- **502 Bad Gateway** — proxy/load balancer got an invalid response from upstream (upstream crashed, wrong port).
- **503 Service Unavailable** — overloaded or in maintenance; temporary, often with `Retry-After`.
- **504 Gateway Timeout** — proxy timed out waiting for upstream.

Debugging tip: 502/504 point at the hop between load balancer and app (health, timeouts, port), not necessarily at app code.

### Idempotency: what is it, and how do you implement idempotency keys?

**Idempotent** = doing the operation N times has the same effect as doing it once. GET, PUT, DELETE are idempotent by spec; POST is not. It matters because networks fail after the server acted but before the client heard back, so clients retry — without idempotency you double-charge or double-create.

**Idempotency keys** (Stripe pattern):
1. Client generates a unique key (UUID) per logical operation and sends `Idempotency-Key: <uuid>`; reuses it on retries.
2. Server stores `(client_id, key) → request hash, status, response` with a unique constraint and a TTL (e.g. 24h).
3. First request: insert row as `in_progress`, do the work, save the response in the same transaction as the side effect where possible.
4. Repeat with same key + same body: return the stored response. Still in progress: 409 (or wait). Same key, different body: 422/409.

```python
def create_payment(req, key, client_id):
    body_hash = sha256(req.body).hexdigest()
    with db.transaction():
        row = db.execute(
            "INSERT INTO idem_keys (client_id, key, body_hash, status) "
            "VALUES (%s,%s,%s,'in_progress') ON CONFLICT DO NOTHING RETURNING key",
            (client_id, key, body_hash)).fetchone()
        if row is None:  # key already seen
            prev = db.execute("SELECT body_hash, status, response FROM idem_keys "
                              "WHERE client_id=%s AND key=%s", (client_id, key)).fetchone()
            if prev.body_hash != body_hash: return 422, {"error": "key reused with different body"}
            if prev.status == "in_progress": return 409, {"error": "request in progress"}
            return 200, prev.response
        result = charge(req)  # side effect
        db.execute("UPDATE idem_keys SET status='done', response=%s WHERE client_id=%s AND key=%s",
                   (json.dumps(result), client_id, key))
    return 201, result
```

Also apply idempotency to consumers of queues/webhooks (dedupe on event ID), since at-least-once delivery is the norm.

### Pagination: offset vs. cursor-based.

- **Offset/limit** (`?offset=200&limit=50` → `LIMIT 50 OFFSET 200`)
  - Pros: simple, jump to page N, total counts easy.
  - Cons: slow on deep pages (DB scans and discards offset rows); unstable — inserts/deletes during paging cause skipped or duplicated items.
- **Cursor/keyset** (`?cursor=eyJpZCI6MTIzfQ`, decoded to `WHERE (created_at, id) < (:ts, :id) ORDER BY created_at DESC, id DESC LIMIT 50`)
  - Pros: constant-time per page with an index, stable under concurrent writes, scales to huge tables.
  - Cons: no random page jumps, cursor must be on a unique, sortable key (add `id` as tiebreaker), harder total counts.
- Make cursors **opaque** (base64 of the sort key) so you can change the implementation.
- Recommendation: cursor for APIs and feeds/sync jobs; offset is fine for small admin tables with page numbers.

### Webhooks vs. polling vs. WebSockets vs. Server-Sent Events.

- **Polling:** client asks "anything new?" every N seconds. Simple, works through any firewall, but wasteful and adds latency. Long-polling reduces waste. Good fallback and for reconciliation.
- **Webhooks:** server POSTs to the client's URL when an event happens. Efficient server-to-server. Needs a public endpoint (problem in locked-down customer networks), signature verification (HMAC), retries with backoff, idempotent handling, and a reconciliation poll for missed events.
- **WebSockets:** full-duplex persistent connection. Use for interactive, two-way, low-latency (chat, collaborative editing, live control). Costs: stateful connections, sticky load balancing, proxies may break them.
- **SSE:** one-way server → client stream over plain HTTP (`text/event-stream`), auto-reconnect with `Last-Event-ID`. Ideal for live dashboards and **streaming LLM tokens**. Simpler than WebSockets, proxy friendly.

Rule of thumb: server-to-server events → webhooks + periodic reconciliation; browser needs live updates one way → SSE; true two-way → WebSockets; can't receive inbound traffic → polling.

### How do you integrate with an API that has no docs?

1. **Ask first:** customer admins, the vendor's support, existing integrations, Postman collections, old scripts in the customer's repos — someone has called it before.
2. **Observe real traffic:** use the vendor's UI with browser DevTools (Network tab) or a proxy (mitmproxy, Charles, Burp) to capture endpoints, headers, auth and payloads.
3. **Look for machine-readable specs:** `/swagger.json`, `/openapi.json`, `/api-docs`, WSDL (`?wsdl`), GraphQL introspection.
4. **Probe carefully in a sandbox/non-prod tenant** with read-only calls; never experiment with writes in production.
5. **Write down what you learn** as an OpenAPI spec and contract tests — the tests catch silent changes later.
6. **Build defensively:** tolerant parsing (ignore unknown fields), strict validation of what you depend on, retries, logging of raw responses (minus PII) for debugging.
7. **Get it agreed in writing** with the system owner, since undocumented APIs can change without notice — flag that risk to the customer.

### How do you integrate with a legacy system that only offers SFTP/CSV drops or a mainframe?

**SFTP/CSV:**
- Agree a **file contract**: naming (`orders_YYYYMMDD_HHMM.csv`), encoding (UTF-8 vs. Windows-1252), delimiter, quoting, header row, date formats, schedule, full vs. delta.
- **Completeness signal**: write to a temp name then rename, or a `.done`/manifest file with row count and checksum — never process half-written files.
- Pipeline: poll SFTP (key-based auth, IP allowlist) → copy raw file to object storage (immutable audit trail) → validate schema and row counts → load to staging → upsert into the target idempotently (keyed on business ID + file name) → move file to `processed/` or `error/`.
- Alert on missing files ("no file by 6am"), quarantine bad rows rather than failing the whole file, and keep reprocessing easy.
- Outbound: generate files the same way, with a manifest.

**Mainframe:**
- Usually you don't touch it directly. Options in order of preference: an existing API layer (z/OS Connect, MQ), **IBM MQ** messages, CDC tools (e.g. IBM InfoSphere, Precisely) replicating DB2/VSAM to a modern DB, or batch file extracts (often EBCDIC, fixed-width, COBOL copybooks — parse with a copybook-aware library).
- Respect batch windows and MIPS cost; involve the mainframe team early because their change cycles are slow.

### How do you handle a flaky third-party API (retries, backoff, circuit breakers, timeouts)?

- **Timeouts on every call:** connect (~1–3s) and read timeouts sized to the endpoint's p99. No timeout = threads pile up and your service dies with theirs.
- **Retry only safe things:** network errors, 429, 502/503/504. Not 400/401/403/422. Only retry non-idempotent calls when using idempotency keys.
- **Exponential backoff with jitter**, capped attempts and total time; honour `Retry-After`.
- **Circuit breaker:** after N failures, open the circuit and fail fast for a cool-down period, then half-open to test. Protects both you and them.
- **Bulkheads / rate limiting:** separate connection pools per dependency; client-side rate limit below their quota.
- **Go async where possible:** put calls on a queue with a retry policy and a dead-letter queue, so user requests don't block on the vendor.
- **Fallbacks:** cached data, degraded mode, clear error to the user.
- **Observability:** per-dependency latency, error-rate and circuit-state metrics; alerts; evidence to send the vendor.

```python
import random, time, requests
RETRYABLE = {429, 502, 503, 504}
def call(url, attempts=5, base=0.5, cap=20):
    for i in range(attempts):
        try:
            r = requests.get(url, timeout=(3, 10))
            if r.status_code not in RETRYABLE: return r
            ra = r.headers.get("Retry-After", "")
            if ra.isdigit():                 # seconds form; honour it, don't jitter below it
                time.sleep(int(ra)); continue
        except (requests.ConnectionError, requests.Timeout):
            pass
        time.sleep(random.uniform(0, min(cap, base * 2**i)))  # exponential backoff, full jitter
    raise RuntimeError(f"gave up on {url}")
```
In production use a library (`tenacity`, `pybreaker`, Resilience4j, or the service mesh).

### How do you version an API without breaking customers?

- **Prefer not breaking:** additive changes (new optional fields, new endpoints) are non-breaking if clients ignore unknown fields — document that rule ("tolerant reader").
- **Breaking changes** (removing/renaming fields, changing types or semantics, new required params) get a new version.
- **Scheme:** URL major version (`/v1/`, `/v2/`) is most visible and easy to route; header/media-type versioning (`Accept: application/vnd.x.v2+json`) or date-based versions per account (Stripe: `Stripe-Version: 2024-06-20`, with the account pinned to its first version) are alternatives.
- **Run versions side by side** with a translation layer: core speaks latest, adapters convert old shapes — avoids forking the codebase.
- **Deprecation process:** announce with timeline, `Deprecation`/`Sunset` headers, changelog, migration guide, usage metrics per client to see who is still on v1, direct outreach to stragglers (FDEs often do this), then remove.
- **Contract tests** (OpenAPI diff in CI, consumer-driven tests like Pact) to catch accidental breaks.

### Synchronous vs. asynchronous integration — trade-offs.

**Synchronous (request/response, e.g. REST call):**
- Pros: simple, immediate result, easy to reason about and debug.
- Cons: temporal coupling — if the downstream is slow or down, so are you; latencies add up across chains; hard to absorb spikes.
- Use for: reads the user is waiting for, validation, short operations.

**Asynchronous (queue, events, webhooks, 202 + polling):**
- Pros: decoupled availability, absorbs bursts, retries built in, fan-out to multiple consumers, better for long tasks (LLM batch jobs, document processing).
- Cons: eventual consistency, more moving parts, harder debugging (need correlation IDs/tracing), must handle duplicates and ordering, need DLQs and monitoring of lag.
- Use for: long-running work, cross-system workflows, high volume, integrating with unreliable systems.

Common hybrid: accept synchronously (validate, persist, return 202 with a job ID), process asynchronously, notify via webhook/SSE.

### Message queues (SQS, RabbitMQ, Kafka, Pub/Sub) — when and why?

**Why:** decouple producers from consumers, buffer spikes, retry failed work, fan-out, and smooth load.

- **AWS SQS:** fully managed, near-infinite scale, at-least-once (Standard) or FIFO with dedupe. Visibility timeout + DLQ. Best "just give me a work queue" on AWS. No replay.
- **RabbitMQ:** classic broker with flexible routing (exchanges: direct, topic, fanout), priorities, per-message acks. Good for task queues and complex routing; self-hosted or managed (Amazon MQ, CloudAMQP). Moderate throughput.
- **Kafka:** distributed, partitioned, **durable log**. Consumers track offsets, so you can replay; ordering per partition; very high throughput. Use for event streaming, CDC, analytics pipelines, event sourcing. Heavier ops (use MSK, Confluent).
- **Google Pub/Sub:** managed pub/sub with push or pull, global, at-least-once (exactly-once option), ordering keys, limited replay via seek. Default on GCP. (Azure equivalents: Service Bus for queues, Event Hubs for Kafka-style streams.)

Design rules regardless of choice: consumers must be idempotent, use DLQs, monitor queue depth/consumer lag, and set message size limits (put big payloads in object storage, send a pointer).

### How do you integrate with Salesforce / SAP / ServiceNow / Workday / Zendesk?

General playbook: use the vendor's **official APIs and auth**, get a **dedicated integration user** with least privilege, test in a **sandbox**, respect **rate/API limits**, prefer **events/CDC over polling**, and map the customer's custom fields (every instance is customised).

- **Salesforce:** REST/SOQL for queries, Bulk API 2.0 for large loads, Platform Events / Change Data Capture for change streams, OAuth (JWT bearer flow for server-to-server via a Connected App). Watch daily API limits and custom objects (`__c`).
- **SAP:** S/4HANA exposes OData APIs (via SAP API Business Hub); older ECC uses BAPIs/RFC and IDocs. Often must go through SAP BTP/Integration Suite or the customer's middleware. Expect long lead times and the SAP Basis team as gatekeeper.
- **ServiceNow:** Table API (REST) for incidents/CMDB, Scripted REST APIs, outbound Business Rules/Flow Designer to call your webhook; OAuth or basic auth for an integration user; MID Server for reaching on-prem.
- **Workday:** SOAP Web Services and REST APIs, RaaS (Reports-as-a-Service — custom reports exposed as REST/XML, very common for HR data extracts), Workday Studio/EIBs. Integration System User (ISU) with security groups. Data is sensitive (HR/PII).
- **Zendesk:** REST API with API token or OAuth, incremental export endpoints for sync (cursor-based), webhooks/triggers for events, rate limits by plan.

Always: build a connector abstraction, store sync state (cursors/watermarks), handle deletes, log per-record failures.

### iPaaS / integration tools (MuleSoft, Zapier, Workato) — when would you use one?

- **What they are:** hosted platforms with prebuilt connectors, auth handling, triggers, mapping and retries.
  - **Zapier / Make:** no-code, business users, simple low-volume automations.
  - **Workato / Tray / Boomi:** enterprise automation, recipes, governance, mid-to-high volume.
  - **MuleSoft (Anypoint):** API-led integration platform for large enterprises, often owned by a central integration team.
- **Use one when:** the customer already has it and an integration team that owns it (go through them — it's the path of least resistance with IT); many SaaS-to-SaaS connections with standard objects; business users must maintain the flow; speed to a pilot matters more than control.
- **Build custom when:** high volume or low latency, complex logic or data transformations, it's core to your product, cost per task gets high, or you need strong testing/version control.
- FDE move: offer both — a clean REST API + webhooks for your product, plus a connector/recipe for the customer's iPaaS so their team can self-serve.

## B. Authentication, authorization & security

### Authentication vs. authorization.

- **Authentication (AuthN):** *who are you?* Proving identity — password + MFA, SSO via OIDC/SAML, API key, client certificate. Failure → **401**.
- **Authorization (AuthZ):** *what are you allowed to do?* Checking permissions for this action on this resource — roles, policies, ownership, tenant boundaries. Failure → **403** (or 404 to hide existence).
- Order: authenticate first, then authorize every request on the server side (never trust the UI to hide buttons).
- Example: an Okta login proves Alice is Alice (AuthN); your app checks Alice is an admin of workspace 42 before she can delete a document (AuthZ).
- OAuth 2.0 is an **authorization** (delegation) framework; OIDC adds the **authentication** layer on top (ID token).
- FDE angle: in enterprise deployments AuthN is usually delegated to the customer's IdP; AuthZ is often driven by IdP groups mapped to your app's roles.

### OAuth 2.0 flows (authorization code + PKCE, client credentials, device code).

- **Authorization Code + PKCE** — for a user granting access to an app (web, SPA, mobile, CLI).
  1. App creates random `code_verifier`, sends `code_challenge = SHA256(verifier)` with redirect to the authorization server.
  2. User logs in and consents; server redirects back with a short-lived `code`.
  3. App exchanges `code` + `code_verifier` at the token endpoint for access token (+ refresh token, + ID token with OIDC).
  - PKCE stops an intercepted code being redeemed. Now recommended for all clients, including confidential ones. Implicit and password grants are deprecated.
- **Client Credentials** — machine-to-machine, no user. Service authenticates with client ID + secret (or private-key JWT / mTLS) and gets a token scoped to the app itself. Typical for back-end integrations and cron jobs.
- **Device Authorization (device code)** — input-constrained devices or CLIs on headless servers. Device shows a URL + user code; user approves on a phone/laptop; device polls the token endpoint until approved.
- **Refresh tokens:** get new access tokens without re-login; rotate them and store them securely.
- Tokens: keep access tokens short-lived (5–60 min), request minimal scopes, validate `aud`.

### OIDC vs. SAML; SSO integration with Okta / Azure AD (Entra ID).

- **SAML 2.0:** XML-based, older, browser-redirect/POST flow. IdP sends a signed XML **assertion** to the app's ACS URL. Dominant in legacy enterprise apps. Authentication only, no API access tokens.
- **OIDC:** identity layer on OAuth 2.0, JSON/JWT based. Returns an **ID token** (who the user is) plus access tokens for APIs. Better for modern web, mobile and SPAs; simpler to implement.
- **Choose:** support both if you sell to enterprises; OIDC for new builds; SAML because many IT teams standardise on it.

**SSO integration steps (Okta / Entra ID):**
1. Customer admin creates an app registration (Entra) / app integration (Okta).
2. Exchange metadata: for SAML — your Entity ID + ACS URL, their IdP metadata XML (SSO URL, signing cert); for OIDC — redirect URIs, their issuer URL, client ID/secret.
3. Map attributes/claims: email, name, stable user ID (`sub`/NameID — don't key on email alone), **groups** for role mapping.
4. Decide JIT provisioning vs. SCIM; enforce SSO for the domain; keep a break-glass admin.
5. Test with a pilot group; handle cert rotation (SAML signing certs expire); IdP-initiated vs. SP-initiated login.
- Common gotchas: clock skew, wrong ACS/redirect URL, Entra group overage (more than ~200 groups in a JWT, or 150 in SAML, returns a link to fetch them instead of the list), missing group claims config.

### JWTs — structure, validation, risks.

**Structure:** `base64url(header).base64url(payload).signature`
- Header: `alg` (RS256, ES256, HS256), `kid` (which key).
- Payload (claims): `iss`, `sub`, `aud`, `exp`, `iat`, `nbf`, plus custom claims (scopes, roles, tenant).
- Signature over header+payload. JWTs are **signed, not encrypted** (JWE is the encrypted variant) — anyone can read the payload.

**Validation (every request):**
- Verify signature with the issuer's public key from its JWKS endpoint (cache, refresh on unknown `kid`).
- **Pin allowed algorithms**; reject `alg: none` and HS/RS confusion.
- Check `iss`, `aud` (token meant for *your* API), `exp`/`nbf` with small clock skew, required scopes/roles.

```python
import jwt  # PyJWT
from jwt import PyJWKClient
jwks = PyJWKClient("https://login.example.com/.well-known/jwks.json")
def verify(token):
    key = jwks.get_signing_key_from_jwt(token).key
    return jwt.decode(token, key, algorithms=["RS256"],
                      audience="api://my-api", issuer="https://login.example.com/")
```

**Risks:**
- Hard to revoke — keep them short-lived, use refresh tokens, or a deny-list for critical cases.
- Sensitive data in payload is readable.
- Stored in `localStorage` → stolen via XSS; prefer HttpOnly secure cookies for browsers (then handle CSRF).
- Weak HMAC secrets can be brute forced; token bloat from too many claims.

### API keys vs. OAuth tokens vs. mTLS.

- **API keys:** long-lived static secret in a header. Simple, good for server-to-server and developer onboarding. Cons: identify an app not a user, no built-in expiry or scopes, leak easily (logs, git). Mitigate: hash at rest, prefix for identification (`sk_live_...`), scopes, rotation, IP allowlists.
- **OAuth tokens:** short-lived, scoped, can represent a user (delegated) or a service (client credentials), revocable via the auth server. Standard for enterprise and user-delegated access. More moving parts.
- **mTLS:** both sides present X.509 certificates in the TLS handshake. Strong machine identity, credential never sent over the wire, can't be replayed. Cons: certificate issuance/rotation/PKI overhead, breaks at TLS-terminating proxies unless handled. Common in banks, service meshes (Istio/Linkerd), zero-trust networks; also used to bind OAuth tokens (RFC 8705).
- Often combined: mTLS for the channel + OAuth token for identity/permissions.
- FDE choice: API keys for quick pilots, OAuth client credentials for production integrations, mTLS when the customer's security team requires it.

### RBAC vs. ABAC vs. ReBAC.

- **RBAC (role-based):** users get roles (admin, editor, viewer); roles have permissions. Simple, auditable, maps well to IdP groups. Breaks down into "role explosion" when you need per-region/per-project variations.
- **ABAC (attribute-based):** decisions from attributes of user, resource, action and context — e.g. "allow if user.department == doc.department and doc.classification <= user.clearance and time is business hours". Very flexible, good for compliance rules; harder to audit and reason about. Policy engines: OPA/Rego, AWS IAM conditions, Cedar.
- **ReBAC (relationship-based):** permissions follow relationships in a graph — "user can view doc if user is member of a team that owns the folder containing the doc" (Google Zanzibar; SpiceDB, OpenFGA). Great for sharing/collaboration models with nested hierarchies.
- **In practice:** RBAC for coarse roles + ReBAC/ABAC for resource-level and data-level rules. For LLM/RAG products, enforce document-level permissions at retrieval time (filter by the user's ACLs) so the model never sees what the user couldn't.

### SCIM user provisioning.

- **SCIM 2.0** (System for Cross-domain Identity Management) is a standard REST API + schema for the **IdP to push user and group changes** to your app.
- Endpoints you implement: `/scim/v2/Users`, `/scim/v2/Groups` supporting `POST`, `GET` (with `filter=userName eq "..."`), `PUT`/`PATCH`, `DELETE`.
- Flow: admin assigns a user to your app in Okta/Entra → IdP calls `POST /Users`; changes attributes → `PATCH`; removes or deactivates → `PATCH active=false` (deprovisioning). Group membership changes drive role mapping.
- **Why it matters:** SSO alone means a fired employee can't log in, but their account, API tokens and data access still exist. SCIM gives automatic **deprovisioning** — a frequent security-questionnaire requirement.
- Versus JIT provisioning (create user on first SSO login): JIT is simpler but can't deprovision or pre-create users.
- Implementation tips: auth with a bearer token per tenant, idempotent creates, use `externalId` to map, soft-delete on deactivate, revoke sessions/tokens immediately, test with Okta's and Entra's SCIM validators (each has quirks in PATCH formats).

### How do you store secrets (Vault, AWS Secrets Manager, KMS)?

- **Never** in source code, images, plain env files in git, or logs. Scan repos (gitleaks, trufflehog) and rotate anything that leaked.
- **Secrets managers** store and serve secrets with access control, audit and rotation:
  - **AWS Secrets Manager** / SSM Parameter Store, **Azure Key Vault**, **GCP Secret Manager** — managed, IAM-integrated, automatic rotation for DB credentials.
  - **HashiCorp Vault** — cloud-agnostic, on-prem/air-gapped friendly, **dynamic secrets** (short-lived DB creds generated per lease), PKI, transit encryption.
- **KMS** (AWS KMS, Cloud KMS, Key Vault keys, HSMs) manages **encryption keys**, not secrets. Used for **envelope encryption**: a data key encrypts the data, KMS's master key encrypts the data key; keys never leave the HSM. Customer-managed keys (CMK/BYOK) let the customer revoke your access.
- **Delivery to apps:** workload identity (IAM roles for service accounts, managed identity) so the app has no bootstrap secret; Kubernetes via External Secrets Operator / Secrets Store CSI driver; note K8s Secrets are only base64 — enable etcd encryption.
- **Operations:** least-privilege access per service, rotation, audit logs, short TTLs, separate secrets per environment and per customer.

### Encryption at rest vs. in transit; TLS handshake basics.

- **At rest:** data on disk/storage encrypted — disk/volume encryption (EBS, LUKS), storage service encryption (S3 SSE-KMS), DB TDE, and application/field-level encryption for very sensitive fields (SSNs). Protects against stolen disks/snapshots and gives key-based access control (revoke the key = data unreadable). Key management is the hard part.
- **In transit:** TLS 1.2+ (prefer 1.3) for all network hops — client to LB, LB to service, service to DB. mTLS internally for zero trust. Protects against eavesdropping and tampering.
- Also "in use" (confidential computing, enclaves) for the most sensitive cases.

**TLS 1.3 handshake (simplified):**
1. **ClientHello:** supported versions, cipher suites, key share (ECDHE public key), SNI (hostname).
2. **ServerHello:** chosen cipher + its key share; both sides now derive the shared session keys (ECDHE → forward secrecy).
3. Server sends its **certificate** chain and a signature proving it owns the private key, then `Finished` — all encrypted.
4. Client **validates the certificate**: trusted CA chain, hostname matches, not expired/revoked. Optionally client certificate (mTLS).
5. Symmetric encryption (AES-GCM/ChaCha20) for application data. TLS 1.3 = 1 round trip (0-RTT resumption possible).

Common enterprise issue: corporate TLS-inspecting proxies re-sign traffic — you must trust the customer's internal CA bundle in your containers.

### OWASP Top 10 (injection, XSS, CSRF, SSRF, broken access control...).

The OWASP Top 10 is the standard list of web-app risks. The categories below follow the 2021 edition; a 2025 edition has since been published (verify the final list) that keeps broken access control at #1, folds SSRF into it, and adds software supply chain failures and mishandling of exceptional conditions. Key ones and fixes:
- **Broken access control (#1):** IDOR (`/invoices/124` showing another tenant's data), missing function-level checks. Fix: server-side authZ on every request, tenant scoping in queries, deny by default, tests.
- **Cryptographic failures:** plaintext data, weak algorithms. Fix: TLS everywhere, strong hashing for passwords (bcrypt/argon2), KMS.
- **Injection (SQL, command, LDAP):** untrusted input concatenated into queries. Fix: parameterised queries/ORM, no `shell=True`, input validation. (LLM-era cousin: **prompt injection** — treat model output as untrusted.)
- **XSS** (now under injection): attacker script runs in victims' browsers. Fix: output encoding, framework auto-escaping, Content Security Policy, HttpOnly cookies.
- **Insecure design, security misconfiguration** (default creds, open S3 buckets, verbose errors), **vulnerable components** (unpatched deps — use Dependabot, Trivy, SBOMs), **authentication failures** (no MFA, weak sessions), **software/data integrity failures** (unsigned updates, insecure CI), **logging & monitoring failures**.
- **SSRF:** server fetches a user-supplied URL, attacker points it at `169.254.169.254` (cloud metadata) or internal services. Fix: allowlist destinations, block private IP ranges after DNS resolution, IMDSv2, egress firewalls.
- **CSRF** (not its own item in 2021 but always asked): a malicious site makes the victim's browser send an authenticated request using its cookies. Fix: `SameSite=Lax/Strict` cookies, anti-CSRF tokens, check `Origin`; not an issue for bearer-token APIs that don't use cookies.

### CORS — what is it and why does it fail?

- Browsers enforce the **same-origin policy**: JS on `https://app.a.com` can't read responses from `https://api.b.com` (different scheme/host/port). **CORS** lets the server opt in by sending headers.
- **Simple requests** (GET/POST with simple headers): browser sends `Origin`; the server must reply `Access-Control-Allow-Origin: https://app.a.com`.
- **Preflight:** for PUT/DELETE, JSON content type, or custom headers like `Authorization`, the browser first sends `OPTIONS` with `Access-Control-Request-Method/Headers`; the server must answer with `Access-Control-Allow-Methods`, `-Headers`, and a 2xx.
- **Why it fails:**
  - Server/gateway doesn't handle `OPTIONS` (returns 401/404/405 because auth middleware runs first).
  - Origin mismatch (`http` vs `https`, trailing slash, port, `www`).
  - `Access-Control-Allow-Origin: *` with `credentials: include` — not allowed; must echo the specific origin and send `Access-Control-Allow-Credentials: true`.
  - Missing allowed header (e.g. `Authorization`, `X-Request-Id`).
  - Error responses (500) from a proxy/LB without CORS headers — the real error is hidden behind a "CORS error".
- Key point: CORS is enforced **by the browser only**; `curl` works fine, so "works in curl, fails in browser" is a classic CORS sign. It's not a security control for your API — authZ is.

### Compliance: SOC 2, HIPAA, GDPR, FedRAMP, ISO 27001, PCI-DSS — what do they mean for a deployment?

- **SOC 2 (Type I/II):** US auditor's attestation that your controls (security, availability, confidentiality, processing integrity, privacy) exist (Type I) and operated over a period (Type II). Means: access reviews, change management, logging, vendor management, incident response. Table stakes for selling B2B SaaS.
- **ISO 27001:** international certification of an information security management system (ISMS) — risk assessment, policies, Annex A controls. Common in Europe/Asia; similar evidence to SOC 2.
- **HIPAA (US health):** protects PHI. Need a **BAA** with the customer and with subprocessors (your cloud and LLM provider must sign one), encryption, access logging, minimum necessary access, breach notification.
- **GDPR (EU personal data):** lawful basis, **data residency/transfer** rules (EU region, SCCs), DPA with customers, data subject rights (access, deletion — your system must be able to find and delete a person's data, including in logs and vector indexes), subprocessor lists, breach notice within 72h.
- **FedRAMP (US federal cloud):** authorisation at Low/Moderate/High based on NIST 800-53 controls. Usually means deploying in **AWS GovCloud/Azure Government**, US-person access, FIPS 140-validated crypto, continuous monitoring. Very long process.
- **PCI-DSS (card data):** minimise scope — don't store card numbers; use a tokenising processor (Stripe). If in scope: network segmentation, encryption, quarterly scans, strict access.

Deployment impact: which region, which subprocessors (including LLM APIs), logging/retention, encryption/key ownership, who can access production, and whether the customer needs single-tenant or self-hosted.

### The customer's security team blocks your deployment. How do you get it approved?

1. **Understand the actual objection** — meet them, don't escalate around them. Ask for specific concerns: data leaving their boundary, a dependency, inbound ports, LLM training on their data, missing certification?
2. **Treat them as a stakeholder, not an obstacle** — their job is to say no to unknown risk. Make the risk known and small.
3. **Bring evidence:** SOC 2 report, pen test summary, architecture and data-flow diagram, list of subprocessors, encryption and key management details, the LLM provider's zero-retention/no-training terms.
4. **Offer mitigations / alternative architectures:** deploy in their VPC or their cloud account, private connectivity (PrivateLink) instead of public internet, outbound-only connections, customer-managed keys, SSO + SCIM, PII redaction, read-only least-privilege credentials, logs to their SIEM, a data-processing agreement.
5. **Reduce scope for a pilot:** synthetic or non-sensitive data, a limited user group, time-boxed with explicit exit criteria.
6. **Document decisions** and agreed controls; get the exception or approval in writing.
7. **Loop in your own security team** for anything you can't answer — never guess on security claims.
8. Keep the business sponsor informed so the timeline impact is visible, but escalate only after you've genuinely addressed concerns.

What the interviewer wants: empathy, technical depth on mitigations, and persistence without being adversarial.

### How do you do a security questionnaire / vendor review with a customer?

- **Expect it:** SIG Lite/Core, CAIQ, or the customer's custom spreadsheet (often 100–300 questions), plus a DPA, MSA security exhibit, maybe a pen test or architecture review.
- **Process:**
  1. Start early — it's often on the critical path to contract.
  2. Use a maintained **answer library** / trust portal (Vanta, Drata, SafeBase, Conveyor) with pre-approved answers and documents (SOC 2, ISO cert, pen test letter, policies).
  3. Triage questions: standard ones answered from the library; deployment-specific ones (data flows, regions, where LLM inference runs, retention) answered by the FDE with security/legal review.
  4. **Be accurate, never over-claim.** "Partially — compensating control is X" is better than a false "Yes"; answers become contractual.
  5. Offer a call with their security team to walk through architecture — often resolves dozens of questions at once.
  6. Track follow-ups and commitments (e.g. "we'll support SCIM by Q3") in the CRM and feed gaps into the product roadmap.
- **Hot topics now:** AI-specific questions — is our data used for training, retention of prompts, model provider subprocessors, prompt-injection controls, data residency of inference.

## C. Cloud

### Core services you'd use on AWS/Azure/GCP for compute, storage, DB, queueing, networking.

| Need | AWS | Azure | GCP |
|---|---|---|---|
| VMs | EC2 | Virtual Machines | Compute Engine |
| Managed Kubernetes | EKS | AKS | GKE |
| Serverless containers | ECS Fargate / App Runner | Container Apps | Cloud Run |
| Functions | Lambda | Functions | Cloud Functions (Cloud Run functions) |
| Object storage | S3 | Blob Storage | Cloud Storage (GCS) |
| Block / file | EBS / EFS | Managed Disks / Azure Files | Persistent Disk / Filestore |
| Relational DB | RDS, Aurora | Azure SQL, Database for PostgreSQL | Cloud SQL, AlloyDB, Spanner |
| NoSQL | DynamoDB | Cosmos DB | Firestore, Bigtable |
| Warehouse | Redshift | Synapse / Fabric | BigQuery |
| Queue / pub-sub | SQS, SNS, EventBridge | Service Bus, Event Grid | Pub/Sub, Eventarc |
| Streaming | Kinesis, MSK | Event Hubs | Pub/Sub, Managed Kafka |
| Networking | VPC, ALB/NLB, Route 53, CloudFront | VNet, App Gateway/Front Door, Azure DNS | VPC, Cloud Load Balancing, Cloud DNS, Cloud CDN |
| Identity / secrets | IAM, Secrets Manager, KMS | Entra ID, Key Vault | IAM, Secret Manager, Cloud KMS |
| LLMs | Bedrock | Azure OpenAI / AI Foundry | Vertex AI |
| Observability | CloudWatch, X-Ray | Azure Monitor, App Insights | Cloud Logging/Monitoring/Trace |

Typical FDE default stack: containers on managed Kubernetes or Cloud Run/Fargate, managed Postgres, object storage, a managed queue, secrets manager, and the cloud's managed LLM service — all in private subnets behind a load balancer.

### VPC, subnets (public/private), security groups, NACLs, NAT gateways.

- **VPC:** your isolated virtual network in a region with a CIDR range (e.g. `10.20.0.0/16`). Plan ranges to avoid overlap with the customer's networks — overlap blocks peering/VPN later.
- **Subnets:** slices of the VPC in one AZ. **Public** subnet = route table has a route to an **Internet Gateway** (load balancers, bastions). **Private** subnet = no direct internet route (apps, DBs). Use at least two AZs.
- **NAT gateway:** sits in a public subnet so private instances can make **outbound** connections (package downloads, API calls) without being reachable inbound. Costs per hour + per GB — VPC endpoints for S3/DynamoDB etc. avoid NAT charges.
- **Security groups:** **stateful** firewalls attached to instances/ENIs; allow rules only; return traffic allowed automatically; can reference other SGs ("DB SG allows 5432 from app SG").
- **NACLs:** **stateless** subnet-level rules, allow and deny, evaluated in numbered order; must allow ephemeral return ports explicitly. Used as a coarse extra layer (e.g. block a CIDR).
- Typical layout: ALB in public subnets → app in private subnets → DB in isolated subnets; egress via NAT or VPC endpoints.

### VPC peering vs. PrivateLink vs. VPN vs. Direct Connect / ExpressRoute.

- **VPC peering:** private routing between two VPCs (same or cross-account/region). Simple, no bandwidth bottleneck. Cons: non-transitive, CIDRs must not overlap, opens network-level reachability both ways (customer security teams dislike this). Transit Gateway scales it to many VPCs.
- **PrivateLink (AWS) / Private Endpoint (Azure) / Private Service Connect (GCP):** exposes **one service** (behind an NLB) to another VPC as a private endpoint. One-directional, consumer-initiated, overlapping CIDRs fine, no wider network exposure. **Best fit for SaaS-to-customer connectivity.**
- **Site-to-site VPN:** IPsec tunnel over the internet between cloud and on-prem/another cloud. Quick to set up (hours), cheap, encrypted; limited bandwidth per tunnel (about 1.25 Gbps for a standard AWS tunnel; verify current limits) and internet-dependent latency.
- **Direct Connect (AWS) / ExpressRoute (Azure) / Cloud Interconnect (GCP):** dedicated private circuit from the customer's data centre to the cloud. Predictable latency, high bandwidth, doesn't traverse the public internet; takes weeks to provision, costs more. Usually already exists at large enterprises — you ride on it.
- Rule of thumb: SaaS service to customer VPC → PrivateLink; customer on-prem → VPN (or their existing Direct Connect); full network merge between your own VPCs → peering/Transit Gateway.

### How do you connect your SaaS to a customer's private network securely?

Options from most to least preferred by security teams:
1. **Private connectivity endpoints:** customer consumes your service via PrivateLink/Private Service Connect, or you consume their data source via a PrivateLink they expose. No public internet, no network-level access.
2. **Outbound-only agent / connector** in the customer's network (like a Tailscale/Cloudflare tunnel, or your own lightweight agent): it initiates an outbound TLS (or mTLS) connection to your SaaS and pulls work. No inbound firewall rules — the easiest approval. Pattern used by GitHub Actions runners, Fivetran, Datadog, ServiceNow MID Server.
3. **Site-to-site VPN** to the customer's network, with tightly scoped firewall rules to specific hosts/ports.
4. **Public endpoint with IP allowlisting + mTLS/OAuth** — your static egress IPs (via NAT) allowlisted on their side, TLS + strong auth.
5. **Deploy in their environment** (their VPC/cluster) when data can't leave at all.

In every case: least-privilege credentials (read-only DB user, scoped service account), encryption in transit, audit logs, a data-flow diagram for the security review, and documented ownership of each component.

### IAM: roles, policies, least privilege, cross-account access.

- **Principals:** users, groups, **roles** (assumable identities with temporary credentials), service accounts.
- **Policies:** JSON documents of `Effect / Action / Resource / Condition`. Identity-based (attached to a principal) and resource-based (on an S3 bucket, KMS key). Default deny; explicit deny wins. Also permission boundaries and org-level SCPs.
- **Least privilege:** grant only the actions/resources needed; start narrow, use access analyzers and CloudTrail to refine; no long-lived access keys — use roles and workload identity (IRSA/EKS Pod Identity, Azure Managed Identity, GCP Workload Identity); separate roles per service and environment.
- **Cross-account access (AWS):** the customer creates a role in **their** account whose **trust policy** allows your account's principal to assume it, with an **`ExternalId`** condition (prevents the confused-deputy problem). Your service calls `sts:AssumeRole` and gets temporary credentials scoped by their permission policy. They can revoke anytime.

```json
{
  "Effect": "Allow",
  "Principal": {"AWS": "arn:aws:iam::111122223333:role/vendor-connector"},
  "Action": "sts:AssumeRole",
  "Condition": {"StringEquals": {"sts:ExternalId": "cust-8f3a..."}}
}
```
- Azure equivalent: multi-tenant app registration + role assignment in the customer's subscription; GCP: grant your service account a role on their project, or workload identity federation.

### Serverless (Lambda/Cloud Functions) vs. containers vs. VMs — trade-offs.

- **Serverless functions:** no servers to manage, scale to zero, pay per request, fast to ship. Limits: execution time (Lambda 15 min), cold starts, memory/package size, limited GPU, vendor lock-in, harder local testing, can get expensive at sustained high load. Great for event glue (S3 triggers, webhooks), spiky low-volume APIs, cron jobs.
- **Containers (Kubernetes, ECS, Cloud Run):** portable (same image runs on any cloud or on-prem — key for FDE deployments into customer environments), long-running processes, GPUs, fine control over runtime; good cost at steady load. Cost: orchestration complexity (less on Cloud Run/Fargate).
- **VMs:** full OS control, legacy software, special kernels/drivers, licensing tied to hosts, GPU-heavy model serving. Most ops burden (patching, scaling) and slowest to scale.
- FDE default: **containers**, because portability across customer clouds, on-prem and air-gapped is decisive. Serverless for glue on our own SaaS side.

### Object storage (S3/GCS/Blob) — consistency, lifecycle policies, presigned URLs.

- **Model:** flat namespace of buckets/containers and keys, effectively unlimited, very durable (S3: 11 nines), accessed via HTTP API. Not a filesystem — no atomic rename, listing can be slow at scale, design key prefixes deliberately.
- **Consistency:** S3 (since Dec 2020), GCS and Azure Blob are **strongly read-after-write consistent** for object puts/overwrites/deletes, and listings reflect writes. Concurrent writers to one key: last writer wins — use versioning or conditional writes (`If-None-Match`, GCS generation preconditions) to avoid clobbering.
- **Lifecycle policies:** rules by prefix/tag/age to transition to cheaper tiers (S3 Standard → Infrequent Access → Glacier; Hot/Cool/Archive in Azure) and expire objects or old versions; abort incomplete multipart uploads. Major cost lever and helps meet retention policies.
- **Presigned URLs:** a time-limited URL signed with your credentials that lets a client GET or PUT one object directly without cloud credentials. Use for browser uploads/downloads of large files so they bypass your app servers. Keep expiry short, restrict content type/size (presigned POST policies), and remember anyone holding the URL can use it.
- Security: block public access, bucket policies, SSE-KMS encryption, access logs, VPC endpoints.

### Managed DBs (RDS, Aurora, DynamoDB, Cosmos DB, BigQuery, Snowflake) — when each.

- **RDS (Postgres/MySQL/SQL Server/Oracle):** standard relational workloads, transactional app data, when you want the familiar engine with managed backups/patching/Multi-AZ. Default choice.
- **Aurora:** AWS's cloud-native MySQL/Postgres-compatible engine: storage replicated across 3 AZs, fast failover, up to 15 read replicas, Global Database, Serverless v2 autoscaling. Use for higher scale/availability; costs more.
- **DynamoDB:** key-value/document, single-digit ms at any scale, serverless pricing. Great when access patterns are known (single-table design), e.g. sessions, idempotency keys, high-volume events. Poor for ad-hoc queries and joins.
- **Cosmos DB:** Azure's globally distributed multi-model NoSQL with tunable consistency levels and multi-region writes. Use on Azure for global, low-latency apps. Watch RU costs.
- **BigQuery:** serverless analytical warehouse on GCP, SQL over petabytes, pay per scanned bytes or slots. Analytics/BI/ML, not OLTP.
- **Snowflake:** cloud-agnostic warehouse with separated storage/compute, virtual warehouses, data sharing. Very common as the customer's "source of truth" for analytics — FDEs often read from it.
- Also: Postgres + pgvector (or OpenSearch) for vector search in RAG; Redis/ElastiCache for caching.

Rule: OLTP → relational first; extreme scale with simple access → DynamoDB/Cosmos; analytics → BigQuery/Snowflake/Redshift.

### High availability and disaster recovery: multi-AZ, multi-region, RPO/RTO.

- **RPO (Recovery Point Objective):** how much data you can lose (time since last good copy). **RTO (Recovery Time Objective):** how long you can be down. Both are business decisions — ask the customer, then price the options.
- **Multi-AZ (HA):** run across 2–3 availability zones in one region — load balancer across AZs, multiple replicas, Multi-AZ DB with synchronous standby. Handles instance/AZ failures with near-zero RPO and minutes of RTO. Baseline for production.
- **Multi-region (DR):** protects against region outage. Strategies by cost/RTO:
  - **Backup & restore:** cross-region backups; RPO hours, RTO hours. Cheapest.
  - **Pilot light:** data replicated, minimal infra running; RTO tens of minutes.
  - **Warm standby:** scaled-down full stack running; RTO minutes.
  - **Active-active:** both regions serve traffic; near-zero RTO, but complex (data conflicts, global DBs like Aurora Global, Spanner, Cosmos).
- Must haves: automated backups with tested **restores** (an untested backup isn't a backup), IaC to recreate infra, DNS failover (Route 53 health checks), runbooks, regular DR drills, and checking dependencies (is your LLM endpoint or IdP also single-region?).

### Autoscaling strategies.

- **Horizontal** (more instances) preferred over **vertical** (bigger instances) for stateless services; vertical for DBs or when horizontal isn't possible.
- **Signals:**
  - CPU/memory utilisation — simple but lagging; fine for CPU-bound services.
  - Request rate / latency per instance — better for web APIs.
  - **Queue depth / consumer lag** — best for workers (KEDA on Kubernetes, SQS-based scaling).
  - GPU utilisation / tokens-per-second / in-flight requests for LLM inference.
- **Policies:** target tracking (keep CPU at 60%), step scaling, **scheduled scaling** for known peaks (business hours), predictive scaling.
- **Layers in Kubernetes:** HPA (pods) + Cluster Autoscaler/Karpenter (nodes) + VPA (right-size requests).
- **Pitfalls:** slow startup (cold models take minutes to load — keep warm minimum replicas), flapping (use cooldowns/stabilisation windows), scaling into a downstream bottleneck (DB connection limits — use pooling), and max limits so bugs don't cause bill shock.

### Cost optimisation in the cloud.

- **Visibility first:** tag everything (team, customer, env), cost explorer/billing exports, per-customer cost in multi-tenant setups, budget alerts and anomaly detection.
- **Right-size:** many instances run at 10% CPU; downsize, use Graviton/ARM, set realistic K8s requests.
- **Pricing models:** Savings Plans/Reserved Instances/committed-use discounts for steady baseload (30–70% off); **spot/preemptible** for fault-tolerant batch and CI.
- **Turn things off:** scale to zero for dev/staging at night, delete unattached volumes, old snapshots, idle load balancers, forgotten GPU instances.
- **Storage:** lifecycle tiers, compression, delete stale logs; columnar formats for analytics.
- **Network:** data transfer and **NAT gateway** charges are a classic surprise — use VPC endpoints, keep traffic in-AZ/region, use CDNs.
- **Managed vs. self-run trade-off:** managed costs more per unit but less engineering time.
- **LLM costs:** right-size the model (small model for routing/classification), prompt caching, batch APIs (often ~50% cheaper), shorter prompts, response caching, provisioned throughput only when utilisation is high.

### Deploying LLMs via Azure OpenAI / AWS Bedrock / Google Vertex AI — why do enterprises prefer these?

- **Data stays in their cloud boundary:** traffic via private endpoints/PrivateLink inside their tenant/VPC, no public internet; data isn't used for training; configurable or zero data retention.
- **Existing contracts and procurement:** billed through the existing cloud agreement and committed spend (MACC/EDP) — no new vendor onboarding, which can take months.
- **Compliance inheritance:** covered by the cloud provider's existing certifications (SOC 2, ISO, HIPAA BAA, FedRAMP, regional data residency) that the customer has already approved.
- **Identity and governance:** IAM/Entra ID auth, no API keys to manage, audit logs in CloudTrail/Azure Monitor, quota and policy controls, content filtering/guardrails.
- **Model choice:** Bedrock and Vertex offer multiple model families (including Claude), easing switching.
- **Capacity:** provisioned throughput for guaranteed capacity and predictable latency.
- **Trade-offs:** newest models/features can arrive later than on the model provider's own API, regional availability varies, quotas need requesting early, and APIs differ slightly — so build a thin model-gateway abstraction (or LiteLLM) to stay portable.
- FDE action item: check model availability in the customer's required region and request quota increases on day one.

## D. Containers, orchestration & IaC

### What is Docker? Image vs. container. Write a Dockerfile for a Python service.

- **Docker** packages an app with its runtime, libraries and config into a portable unit that runs the same on a laptop, CI, any cloud or on-prem. Uses Linux kernel features (namespaces for isolation, cgroups for resource limits) — lighter than VMs because containers share the host kernel.
- **Image:** immutable, layered, read-only template (built from a Dockerfile, stored in a registry, identified by tag and digest). **Container:** a running (or stopped) instance of an image with its own writable layer, process, network and filesystem view. Image is to container as class is to object.

```dockerfile
# Dockerfile for a FastAPI service
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1

WORKDIR /app

# Install deps first so this layer is cached when only code changes
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY src/ ./src/

# Run as non-root
RUN useradd --create-home --uid 10001 appuser
USER appuser

EXPOSE 8000
HEALTHCHECK CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/healthz')"
CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```
Plus a `.dockerignore` (`.git`, `.venv`, `__pycache__`, `.env`, tests). Good practices: pin base image versions (ideally by digest), non-root user, exec-form `CMD` so signals reach the process, no secrets in the image, config via env vars.

### Multi-stage builds; reducing image size.

**Multi-stage:** use one stage with compilers and build tools, then copy only the artefacts into a clean minimal runtime stage. Smaller image, smaller attack surface, no build tools in production.

```dockerfile
FROM python:3.12-slim AS build
RUN apt-get update && apt-get install -y --no-install-recommends build-essential \
    && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY requirements.txt .
RUN python -m venv /venv && /venv/bin/pip install --no-cache-dir -r requirements.txt

FROM python:3.12-slim AS runtime
COPY --from=build /venv /venv
WORKDIR /app
COPY src/ ./src/
USER 10001
ENV PATH="/venv/bin:$PATH"
CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Other ways to shrink:**
- Slim/distroless/Chainguard base images (Alpine can be slower for Python due to musl — wheels often don't exist).
- Combine `RUN` steps and clean package caches in the same layer; `--no-install-recommends`; `pip --no-cache-dir`.
- `.dockerignore` to keep the build context small.
- Order layers from least to most frequently changing for cache hits.
- Don't bake large models into the image unless air-gapped requirements demand it — mount from a volume or pull from object storage.
- Inspect with `docker history` or `dive`; scan with Trivy/Grype.

### Kubernetes: pod, deployment, service, ingress, configmap, secret, statefulset, daemonset.

- **Pod:** smallest deployable unit — one or more containers sharing network namespace (localhost) and volumes. Ephemeral; you rarely create them directly.
- **Deployment:** manages a ReplicaSet to keep N identical, stateless pods running; handles rolling updates and rollbacks.
- **Service:** stable virtual IP + DNS name (`api.ns.svc.cluster.local`) that load-balances across pods matched by label selector. Types: ClusterIP (internal), NodePort, LoadBalancer (cloud LB), headless.
- **Ingress:** HTTP(S) routing from outside the cluster to services by host/path, with TLS termination; needs an ingress controller (NGINX, ALB, Traefik). The Gateway API is the newer successor, and the community ingress-nginx controller has been announced for retirement (verify status), so new installs should lean towards Gateway API.
- **ConfigMap:** non-secret config as key-values or files, injected as env vars or mounted volumes.
- **Secret:** same for sensitive values — only base64-encoded by default; enable etcd encryption, restrict RBAC, or sync from Vault/cloud secret managers.
- **StatefulSet:** for stateful apps (databases, Kafka, vector DBs): stable pod names (`db-0`, `db-1`), ordered startup/shutdown, a persistent volume per pod that follows it.
- **DaemonSet:** one pod per node (or per selected node) — log shippers, monitoring agents, GPU device plugins, CNI.
- Also worth naming: Job/CronJob (batch), Namespace (isolation), PersistentVolumeClaim, ServiceAccount, HPA.

### How do you debug a pod in CrashLoopBackOff?

CrashLoopBackOff = the container keeps starting and exiting; Kubernetes restarts it with growing back-off. Work through it systematically:

```bash
kubectl get pods -n app -o wide                     # status, restarts, node
kubectl describe pod <pod> -n app                   # Events, Last State, exit code, reason (OOMKilled?), probe failures
kubectl logs <pod> -n app --previous                # logs from the crashed instance (key step)
kubectl logs <pod> -n app -c <container> --previous # multi-container pods / init containers
kubectl get events -n app --sort-by=.lastTimestamp
kubectl get pod <pod> -n app -o yaml                # actual spec: image, env, mounts, resources, command
```

**Read the exit code:** `1` app error (stack trace in logs); `137` SIGKILL — usually **OOMKilled** (raise memory limit or fix leak); `139` segfault; `143` SIGTERM; `127` command not found (wrong `CMD`/entrypoint); `126` permission denied.

**Common causes:** missing env var/ConfigMap/Secret; can't reach DB or dependency at startup; bad config or migration failure; **liveness probe killing a slow-starting app** (add a startupProbe or increase `initialDelaySeconds`); wrong command or architecture (arm64 image on amd64 node → `exec format error`); read-only filesystem or non-root permissions; customer's TLS-intercepting proxy breaking outbound calls.

**Get inside:**
```bash
kubectl debug -it <pod> -n app --image=busybox --target=<container>   # ephemeral debug container
kubectl run tmp --rm -it --image=nicolaka/netshoot -n app -- bash     # test DNS/network from same namespace
# or temporarily override the command to keep it alive and exec in:
#   command: ["sleep", "infinity"]   then kubectl exec -it <pod> -- sh
```
Then fix, redeploy, and add the missing check (config validation at startup, better error messages) so the next person diagnoses it faster.

### Helm charts — what and why?

- **Helm** is the package manager for Kubernetes. A **chart** is a versioned bundle of templated K8s manifests (`templates/`), default config (`values.yaml`), metadata (`Chart.yaml`) and dependencies (e.g. a Postgres or Redis subchart).
- **Why:**
  - One parameterised package instead of dozens of YAML files — customers override only `values.yaml` (domain, image registry, resources, which features are on, external DB vs. bundled).
  - **Releases with history:** `helm install`, `helm upgrade`, `helm rollback <release> <revision>`.
  - Distribution: push charts to an OCI registry; easy to mirror for air-gapped installs.
  - Hooks for pre-upgrade jobs like DB migrations.
- **Trade-offs:** Go templating gets messy; a values file can become a giant config surface — validate with `values.schema.json`; `helm template` + `helm lint` in CI. Alternatives/complements: Kustomize (overlays, no templating), operators for complex lifecycle, Argo CD/Flux for GitOps-driven Helm releases.
- FDE relevance: a well-designed Helm chart is usually *the* product artefact for self-hosted customer deployments.

```bash
helm upgrade --install myapp oci://registry.example.com/charts/myapp \
  --version 2.4.1 -n myapp --create-namespace -f customer-values.yaml --atomic --timeout 10m
```

### Liveness vs. readiness probes.

- **Liveness probe:** "is this container broken beyond repair?" If it fails, kubelet **restarts** the container. Use for deadlocks/hung processes. Keep it cheap and **don't check dependencies** — if the DB is down, restarting every pod makes things worse.
- **Readiness probe:** "can this pod receive traffic right now?" If it fails, the pod is **removed from Service endpoints** (no restart). Use during startup (loading a model, warming caches), when overloaded, or when a critical dependency is unavailable. Also gates rolling updates.
- **Startup probe:** for slow-starting apps (big ML models); disables liveness/readiness until it succeeds, preventing CrashLoopBackOff from liveness killing a still-booting app.

```yaml
startupProbe:   {httpGet: {path: /healthz, port: 8000}, failureThreshold: 60, periodSeconds: 5}
livenessProbe:  {httpGet: {path: /healthz, port: 8000}, periodSeconds: 10, failureThreshold: 3}
readinessProbe: {httpGet: {path: /ready,   port: 8000}, periodSeconds: 5,  failureThreshold: 2}
```
Common mistake: same endpoint for both, with liveness checking the DB → cascading restarts during a DB blip.

### Horizontal Pod Autoscaler.

- HPA adjusts a Deployment/StatefulSet's **replica count** based on metrics, looping roughly every 15s: `desired = ceil(current * currentMetric / targetMetric)`.
- **Metrics:** CPU/memory utilisation **relative to the pod's resource requests** (requests must be set), custom metrics (requests per second via Prometheus Adapter), external metrics (queue length). **KEDA** extends this for event sources (SQS, Kafka lag) and scale-to-zero.
- **Behaviour settings:** `minReplicas`/`maxReplicas`, scale-up/scale-down policies and stabilisation windows (default 5 min for scale-down) to avoid flapping.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata: {name: api}
spec:
  scaleTargetRef: {apiVersion: apps/v1, kind: Deployment, name: api}
  minReplicas: 3
  maxReplicas: 30
  metrics:
  - type: Resource
    resource: {name: cpu, target: {type: Utilization, averageUtilization: 65}}
```
- Pair with **Cluster Autoscaler/Karpenter** so new pods have nodes, readiness probes so new pods don't take traffic too early, and PodDisruptionBudgets. Don't combine HPA and VPA on the same CPU/memory metric.

### Terraform: state, plan/apply, modules, drift.

- **Terraform** = declarative IaC: describe desired infra in HCL; providers (AWS, Azure, GCP, Kubernetes, Okta...) make API calls to reach it.
- **State:** a file mapping your resources to real-world IDs plus attributes. Store it **remotely** (S3 + DynamoDB/native lockfile, GCS, Azure Blob, Terraform Cloud) with **locking** and encryption — it may contain secrets. Never edit by hand; use `terraform state mv/rm`, `import` blocks.
- **Plan/apply:** `terraform plan` diffs config vs. state vs. real infra and shows creates/updates/**destroys**; review it (in PRs via Atlantis/TF Cloud). `terraform apply` executes a saved plan. Watch for "forces replacement" on stateful resources; use `prevent_destroy` lifecycle rules for DBs.
- **Modules:** reusable, versioned building blocks (e.g. `vpc`, `eks-cluster`, `customer-tenant`). Compose per environment/customer with different inputs; pin versions.
- **Drift:** real infra changed outside Terraform (someone clicked in the console). Detect with scheduled `terraform plan -detailed-exitcode` (exit 2 = changes) or `plan -refresh-only`; then either codify the change or let apply revert it. Prevent by restricting console write access.
- FDE angle: ship customers a Terraform module for their side (IAM role, PrivateLink endpoint, buckets) so their team applies it in their own account — easier security approval than handing over admin access.

### How do you deploy your product into a customer's Kubernetes cluster?

1. **Discovery / pre-flight:** K8s version and distribution (EKS, AKS, GKE, OpenShift, Rancher), node sizes and GPUs, ingress controller, storage classes, registry access (can they pull from ours or must we mirror?), egress/proxy rules, DNS and TLS certs, IdP for SSO, external DB/object storage vs. bundled, security policies (Pod Security Standards, OPA/Kyverno, no root, read-only FS). Provide a **preflight check script** (e.g. Replicated `troubleshoot` preflights).
2. **Package:** a Helm chart (or an operator) with a clear `values.yaml`, signed images (cosign), SBOMs, minimal privileges (namespaced RBAC, no cluster-admin), resource requests/limits, network policies, OpenShift-compatible security contexts.
3. **Install:** into a dedicated namespace via their GitOps tooling (Argo CD/Flux) if they have it, otherwise `helm upgrade --install` run by their team or jointly over a screen share. Secrets via their secret store (External Secrets, Vault).
4. **Verify:** smoke tests (`helm test`), health dashboards, end-to-end with real SSO.
5. **Operate:** they usually own the cluster and we have limited/no access — so ship good logs/metrics (Prometheus endpoints, OpenTelemetry), a **support bundle** collector, runbooks, and an upgrade path (versioned charts, migration hooks, rollback docs).
6. **Handover:** documented architecture, ownership matrix (who patches nodes, who upgrades the app), escalation path.

### Air-gapped deployment: how do you ship images, updates and models with no internet?

- **Bundle everything into a signed release artefact:**
  - Container images saved as tarballs or an OCI layout (`docker save`, `skopeo copy`, `oras`), Helm charts, CLI binaries, OS packages/pip wheels if needed.
  - **Model weights** (often tens of GBs) plus tokenizer/config, with checksums.
  - Manifest with versions, SHA-256 digests, SBOM, and signatures (cosign/GPG).
- **Transfer:** customer downloads on a connected machine (or we ship encrypted drives), scans it with their AV/security tooling, and moves it across the gap ("sneakernet"/data diode) per their process.
- **Inside:** push images into their internal registry (Harbor, Artifactory, Nexus), charts into their chart repo, models into internal object storage (MinIO) or a PVC. Helm values point all `image.registry` fields at the internal registry.

```bash
# Connected side
skopeo copy docker://registry.example.com/app:2.4.1 oci-archive:app-2.4.1.tar
sha256sum *.tar > SHA256SUMS && cosign sign-blob --key cosign.key SHA256SUMS > SHA256SUMS.sig
# Air-gapped side: verify signature and checksums before importing
cosign verify-blob --key cosign.pub --signature SHA256SUMS.sig SHA256SUMS   # flags vary by cosign version
sha256sum -c SHA256SUMS
skopeo copy oci-archive:app-2.4.1.tar docker://harbor.internal/app:2.4.1
```
- **Design the product for it:** no runtime calls to the internet (telemetry off, no CDN fonts, no license-server phone-home — use offline license files), self-hosted LLM inference (vLLM/TGI) instead of APIs, internal CA support, NTP.
- **Updates:** versioned, cumulative bundles with migration scripts; delta bundles to reduce size; documented upgrade/rollback runbook; tested in our own air-gapped staging environment. Tools like Replicated (KOTS embedded/air-gap installs) and Zarf are built for this.
- **Support:** no remote access, so provide a support-bundle tool that collects logs/config (redacted) for the customer to export.

## E. CI/CD & operations

### Design a CI/CD pipeline for a customer-facing service.

**On every pull request (CI):**
1. Lint, format check, type check (ruff, mypy).
2. Unit tests + coverage; contract tests (OpenAPI diff, Pact).
3. Security: dependency scan (Dependabot/Snyk), SAST (Semgrep/CodeQL), secret scan (gitleaks), IaC scan (Checkov/tfsec).
4. Build the Docker image once, scan it (Trivy), tag with the git SHA.
5. Integration tests against ephemeral dependencies (docker compose / Testcontainers); `terraform plan` and `helm template` for infra changes.
6. For LLM features: a small eval suite on a golden set with pass thresholds.

**On merge to main (CD):**
1. Push the **immutable image** (by digest) to the registry; sign it (cosign) and generate an SBOM.
2. Deploy automatically to **staging** (GitOps: update the image tag in the env repo; Argo CD syncs). Run DB migrations (backward compatible) as a pre-deploy job.
3. Smoke + end-to-end tests on staging.
4. **Promote the same artefact** to production — no rebuilds. Manual approval gate if the customer/compliance requires it.
5. **Progressive rollout:** canary (5% → 25% → 100%) with automated analysis of error rate/latency (Argo Rollouts/Flagger); automatic rollback on SLO breach.
6. Post-deploy checks, release notes, notify the channel; deploy markers in Datadog/Grafana.

**Principles:** build once, deploy many; everything as code; fast feedback (<10 min CI); trunk-based development with feature flags; least-privilege deploy credentials via OIDC federation (no long-lived cloud keys in CI); audit trail for SOC 2 change management.

### Blue-green vs. canary vs. rolling deployments.

- **Rolling:** replace instances gradually (K8s default: `maxSurge`/`maxUnavailable`). Pros: no extra capacity, simple. Cons: old and new run together during rollout; rollback is another rolling update; bad versions reach users progressively without metric-based gating.
- **Blue-green:** run a full new environment (green) next to the current one (blue), test it, then **switch all traffic** at the LB/DNS. Pros: instant cut-over and instant rollback (switch back), test in a production-like environment. Cons: double capacity during switch; DB schema must work for both; all users hit the new version at once.
- **Canary:** send a small percentage of traffic (or specific tenants/internal users) to the new version, compare metrics against the baseline, gradually increase. Pros: limits blast radius, catches issues real traffic reveals. Cons: needs traffic splitting (service mesh, ingress weights, Argo Rollouts), good metrics, and handling of mixed versions; low-traffic services make statistical comparisons hard.
- **Choose:** rolling for low-risk internal services; canary for high-traffic customer-facing services; blue-green when you need instant rollback or a hard cut-over (e.g. some single-tenant customer deployments, stateful apps with maintenance windows).
- In all cases: backward-compatible DB changes and API compatibility between versions.

### Feature flags — uses and risks.

**Uses:**
- Decouple **deploy** from **release**: ship dark code to production, turn it on later.
- Progressive delivery: enable for internal users → one pilot customer → 10% → all.
- Per-customer/per-tenant features and entitlements (very common in FDE work: "turn on the new connector only for Customer X").
- **Kill switches** for risky features or expensive dependencies (disable an LLM feature if costs spike or the provider is down).
- A/B experiments.

**Tools:** LaunchDarkly, Unleash, Flagsmith, OpenFeature SDK, or a simple config table.

**Risks:**
- **Flag debt:** stale flags pile up, code paths multiply, untested combinations. Give each flag an owner and expiry; remove after full rollout.
- Testing complexity — test both states of critical flags.
- Flag service as a dependency: define safe defaults when unreachable; cache locally.
- Misconfiguration in production = outage; audit and permission flag changes like deploys.
- Leaking unreleased features via client-side flags; don't use flags as a security control.
- Doesn't help with schema changes — DB migrations still need expand/contract.

### How do you roll back a bad deploy?

1. **Decide fast:** if a deploy correlates with an SLO breach, roll back first and debug later. Mitigation beats root cause during an incident.
2. **Mechanisms:**
   - Flip the **feature flag** off (fastest, if the change was flagged).
   - Abort the canary / switch blue-green traffic back.
   - Redeploy the previous immutable image:
```bash
kubectl rollout undo deployment/api -n app            # previous ReplicaSet
kubectl rollout undo deployment/api -n app --to-revision=12
helm rollback myapp 41 -n app
# GitOps: git revert the image-tag commit and let Argo CD sync
```
3. **Database:** the hard part. Rollback only works if migrations are backward compatible (expand/contract) so the old code runs on the new schema. Avoid down-migrations that destroy data; prefer roll-forward fixes for data issues. Restore from backup/PITR only as a last resort.
4. **Verify:** metrics back to baseline, error rates normal, customer-facing confirmation.
5. **Follow up:** communicate status to customers, blameless postmortem, add the missing test/alert/canary check, and freeze further deploys until fixed.
- Pre-requisites: immutable versioned artefacts, recorded deploy history, rollback rehearsed, and runbooks — especially for customer-hosted deployments where *they* may have to run the rollback.

### Database migrations with zero downtime.

Core idea: during a deploy, old and new code run at the same time, so every schema state must work with both. Use **expand → migrate → contract**:

Example — rename `users.name` to `full_name`:
1. **Expand:** add nullable `full_name` column (no default rewrite, no long lock). Deploy.
2. **Dual write:** new code writes both columns, reads old (or with fallback). Deploy.
3. **Backfill** in small batches (e.g. 1–10k rows per transaction, throttled) to avoid long locks and replication lag.
4. **Switch reads** to `full_name`. Deploy.
5. **Contract:** stop writing `name`; after confidence, drop the column in a later release.

**Postgres-specific safety:**
- `CREATE INDEX CONCURRENTLY` (not inside a transaction).
- Add `NOT NULL` via a `CHECK ... NOT VALID` constraint then `VALIDATE CONSTRAINT`.
- Add foreign keys `NOT VALID`, then validate.
- Set `lock_timeout` (e.g. 5s) so a migration waiting on a lock doesn't block all traffic behind it.
- Avoid type changes that rewrite the table; create a new column instead.
- Big tables: tools like `pg-osc`, `gh-ost`/`pt-online-schema-change` for MySQL.

**Process:** migrations versioned in code (Alembic, Flyway, Liquibase), run as a separate step/job before the app rollout, tested against a production-sized copy, and never mixed with destructive changes in the same release.

### Observability: logs vs. metrics vs. traces. Tools (Datadog, Prometheus, Grafana, OpenTelemetry).

- **Logs:** timestamped records of discrete events, high detail. Answer "what exactly happened to this request?". Use **structured JSON** with request ID, tenant, user, trace ID; avoid logging PII/secrets/prompts unless approved. Expensive at volume — sample and set retention.
- **Metrics:** numeric time series aggregated over time (request rate, error rate, latency percentiles, queue depth, GPU utilisation, tokens/sec). Cheap, ideal for dashboards and **alerting**. Keep label cardinality low (no user IDs as labels). RED (Rate, Errors, Duration) for services, USE (Utilisation, Saturation, Errors) for resources.
- **Traces:** the path of one request across services as a tree of spans with timings. Answer "where did the 4 seconds go?" — e.g. 3.2s in the LLM call, 0.5s in vector search.
- Workflow: metric alert fires → dashboard narrows the service/time → trace shows the slow span → logs for that trace ID show the error.

**Tools:**
- **OpenTelemetry:** vendor-neutral standard and SDKs/Collector for traces, metrics and logs — instrument once, export anywhere. Very valuable for FDEs because each customer may use a different backend.
- **Prometheus:** pull-based metrics + PromQL + Alertmanager; K8s standard.
- **Grafana:** dashboards over Prometheus, Loki (logs), Tempo (traces).
- **Datadog** (also New Relic, Dynatrace, Splunk): commercial all-in-one SaaS; fast to adopt, expensive at scale.
- Cloud native: CloudWatch, Azure Monitor, Google Cloud Operations. LLM-specific: Langfuse, LangSmith, Arize for prompt/response traces and evals.

### What SLIs/SLOs/SLAs would you define for a customer deployment?

- **SLI** (indicator): a measured ratio of good events — e.g. % of requests that succeed within 1s.
- **SLO** (objective): internal target for the SLI over a window — e.g. 99.9% over 28 days. Leaves an **error budget** (0.1% ≈ 40 min/month) that governs release pace.
- **SLA** (agreement): contractual promise to the customer with credits/penalties — set **looser than the SLO** (e.g. 99.5%) so you breach internally first.

**Example set for an AI document-processing service:**
| User journey | SLI | SLO |
|---|---|---|
| API availability | non-5xx responses / all valid requests | 99.9% / 28d |
| Interactive latency | % of query requests answered < 2s (p95), time-to-first-token < 1s | 95% |
| Async pipeline freshness | % of documents processed within 15 min of upload | 99% |
| Correctness/quality | extraction accuracy on weekly sampled audit; % answers with valid citations | ≥ 95% (tracked, maybe not contractual) |
| Integration sync | % of scheduled syncs completing successfully | 99.5% |
| Support | P1 response within 1h | contractual |

Principles: measure at the edge close to the user, define what's excluded (customer-side outages, their LLM quota limits), agree on dependencies when the customer hosts it (you can't promise uptime for infra you don't run), and review SLOs with the customer quarterly.

### On-call and incident response — what's your process?

1. **Detect:** SLO-based alerts (burn-rate alerts, not every CPU spike) routed via PagerDuty/Opsgenie; every alert actionable with a runbook link.
2. **Triage & declare:** assess impact and set severity (SEV1 = customer down/data risk). Declare early — it's cheap to downgrade.
3. **Roles:** Incident Commander (coordinates, decides), technical lead(s) (investigate), communications lead (customer and internal updates). Dedicated Slack channel and bridge.
4. **Mitigate first:** roll back, flip a flag, fail over, scale up, rate-limit — restore service before root-causing.
5. **Communicate:** status page and direct customer updates on a regular cadence (e.g. every 30 min for SEV1): what's affected, what we're doing, next update time. FDEs are often the customer's point of contact here.
6. **Resolve & verify** metrics are back to normal; keep a timeline as you go.
7. **Blameless postmortem** within a few days: timeline, impact, root cause and contributing factors (5 whys), what went well, action items with owners and dates. Share a customer-facing RCA where appropriate.
8. **Healthy on-call:** reasonable rotation, handoff notes, reduce alert noise, follow-the-sun if global, compensate on-call.

### How do you manage configuration across many customer deployments?

- **Separate code from config:** one product codebase and artefact; everything customer-specific is config (12-factor). Avoid per-customer forks or branches at all costs.
- **Layered config:** product defaults → environment tier (prod/staging) → region → customer overrides. Implement as Helm values layering or Kustomize overlays.
- **Config as code in Git:** a "fleet" repo with one directory per customer (`customers/acme/values.yaml`), PR review, history, and **GitOps** (Argo CD ApplicationSets / Flux) reconciling each deployment to Git.
- **Schema + validation:** JSON Schema for config (`values.schema.json`), CI validation of all customer configs on every product change, so a new required field fails CI rather than a customer deploy.
- **Secrets** separately in each customer's secret store, referenced by name — never in the repo.
- **Feature flags/entitlements** for runtime toggles, not code branches.
- **Visibility:** an inventory/dashboard of each deployment's version, config hash, last deploy, health, and drift detection (live vs. Git).
- Keep the number of knobs small; every option is a test-matrix multiplier.

### How do you upgrade 50 customer deployments that are on different versions?

1. **Inventory:** know every deployment's current version, config, infra type (SaaS single-tenant, customer VPC, air-gapped), access model and contacts. Group into cohorts.
2. **Define a supported upgrade path:** e.g. support N-2 versions; allow skipping minors but require stepping through certain "stop" versions with key migrations (like Kubernetes). Make migrations idempotent and cumulative; test upgrade paths from every supported starting version in CI.
3. **Reduce drift first:** customers far behind get stepped up; deprecate unsupported versions with clear timelines.
4. **Automate:** GitOps or a fleet-management tool (Argo CD ApplicationSets, Replicated, custom controller) so an upgrade is a version bump in config plus automated pre-flight checks, backups, migration, health verification and auto-rollback.
5. **Wave rollout:** internal/staging → friendly early adopters → low-risk customers → large/regulated customers last, with soak time between waves and stop conditions.
6. **Customer-managed / air-gapped:** publish release bundles, upgrade guides and pre-flight scripts; schedule maintenance windows with them; FDE-assisted for the high-touch accounts.
7. **Communicate:** release notes, breaking-change notices, change-approval tickets (many enterprises require CAB approval).
8. **Track:** dashboard of version per customer and upgrade status; postmortems on failed upgrades feed back into the tooling.
Long-term: make deployments self-updating where customers allow it, and keep the version spread small.

## F. Linux & scripting

### Common commands: `grep`, `awk`, `sed`, `find`, `xargs`, `curl`, `jq`, `ps`, `top`, `lsof`, `netstat/ss`, `df`, `du`, `tail -f`.

```bash
grep -rin "timeout" /var/log/app/          # recursive, case-insensitive, line numbers
grep -E "ERROR|FATAL" app.log | grep -v healthz   # regex, exclude matches
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head   # top client IPs
awk -F, '$3 > 100 {sum += $3} END {print sum}' data.csv          # field filter + sum
sed -n '100,120p' app.log                  # print a line range
sed -i 's/old-host/new-host/g' config.yaml # in-place replace
find /var/log -name "*.log" -mtime +7 -size +100M   # old, big log files
find . -name "*.pyc" -print0 | xargs -0 rm -f        # safe with spaces in names
cat hosts.txt | xargs -P 8 -I{} curl -s -o /dev/null -w "{} %{http_code}\n" https://{}/healthz  # parallel
curl -sS -v -H "Authorization: Bearer $TOKEN" https://api.example.com/v1/items   # verbose: TLS, headers
curl -w "dns:%{time_namelookup} connect:%{time_connect} tls:%{time_appconnect} total:%{time_total}\n" -o /dev/null -s https://api.example.com
jq '.items[] | {id, status}' resp.json     # reshape JSON
jq -r '.items[] | select(.status=="failed") | .id' resp.json
ps aux --sort=-%mem | head                 # top memory processes
top / htop                                 # live CPU/mem; press 1 for per-core, M to sort by mem
lsof -p 1234 | wc -l                       # open files of a process (fd leaks)
ss -tulpn                                  # listening TCP/UDP ports with process (netstat -tulpn on old systems)
ss -tan state established | wc -l          # count established connections
df -h ; df -i                              # disk space and inodes per filesystem
du -sh /var/* 2>/dev/null | sort -h        # directory sizes
tail -f app.log | grep --line-buffered ERROR   # follow a log live
journalctl -u myapp -f --since "10 min ago"    # systemd service logs
```
Interview tip: explain *why* you'd reach for each — e.g. `curl -v` + `openssl s_client -connect host:443` for TLS issues, `ss` + `lsof` for "connection refused", `df -i` when disk shows space but writes fail.

### How do you find which process is using a port?

```bash
sudo ss -ltnp 'sport = :8080'      # modern: shows pid and process name
sudo lsof -i :8080                 # all processes with sockets on 8080 (listening and connected)
sudo lsof -iTCP:8080 -sTCP:LISTEN  # only the listener
sudo netstat -tulpn | grep :8080   # older systems
sudo fuser 8080/tcp                # prints pid; fuser -k 8080/tcp kills it
```
- Use `sudo` — without root you won't see other users' process info.
- Then inspect it: `ps -fp <pid>`, `ls -l /proc/<pid>/exe`, `systemctl status <pid>` (shows which unit owns it).
- In containers: the port may be held by `docker-proxy` — check `docker ps --format '{{.Names}} {{.Ports}}'`. In Kubernetes: `kubectl get svc -A | grep 8080` or check `hostPort`/NodePort usage.
- Stop it gracefully via its service manager (`systemctl stop`) rather than `kill -9`, which skips cleanup.

### Disk is full — how do you find what's using it?

```bash
df -h                                        # which filesystem is full
df -i                                        # inodes exhausted? (millions of tiny files)
sudo du -xh --max-depth=1 / 2>/dev/null | sort -h | tail   # -x: stay on this filesystem
sudo du -xh --max-depth=1 /var | sort -h | tail             # drill down level by level
sudo find / -xdev -type f -size +1G -exec ls -lh {} \; 2>/dev/null   # biggest files
sudo ncdu -x /                               # interactive, if installed
sudo lsof +L1                                # deleted files still held open by a process
```
- **Usual suspects:** `/var/log` (unrotated logs), `journalctl` (`journalctl --disk-usage`, `--vacuum-size=500M`), Docker (`docker system df`, `docker system prune`, container JSON logs in `/var/lib/docker/containers`), core dumps, `/tmp`, old model files/caches (`~/.cache/huggingface`), DB WAL files.
- **`df` says full but `du` doesn't add up** → a process holds a deleted file open (`lsof +L1`); restart that process or truncate via `/proc/<pid>/fd/<n>` (`: > /proc/<pid>/fd/<n>`).
- **Fix safely:** truncate (`: > big.log`) rather than delete files an app still writes to; compress or move to object storage; then prevent recurrence — logrotate, Docker log-size limits (`max-size`), retention policies, disk-usage alerts at 80%.

### File permissions and `chmod`/`chown`.

- `ls -l` shows `-rwxr-x---  1 alice devs  file`: type, then **user / group / other** each with read (4), write (2), execute (1).
- For directories: `r` = list names, `w` = create/delete entries, `x` = enter/traverse.

```bash
chmod 640 app.conf          # rw- r-- ---  owner rw, group r, others none
chmod 600 ~/.ssh/id_ed25519 # private keys must be owner-only or ssh refuses them
chmod 700 ~/.ssh
chmod 755 deploy.sh         # rwx r-x r-x  executable script
chmod u+x,g-w,o-rwx file    # symbolic form
chmod -R g+rX shared/       # capital X: execute only on dirs (and already-executable files)
chown appuser:appgroup /srv/app -R
chgrp devs file
umask 027                   # default perms for new files: 640, dirs: 750
```
- **Special bits:** setuid (4xxx — runs as file owner, e.g. `passwd`), setgid (2xxx — on dirs, new files inherit the group), sticky bit (1xxx — `/tmp`, only owners can delete their files).
- Beyond basic modes: ACLs (`setfacl`/`getfacl`), SELinux/AppArmor contexts (`ls -Z`) — a common cause of "permission denied" on RHEL even with correct modes.
- Containers: run as a non-root UID and make sure mounted volumes are writable by it (`fsGroup` in K8s `securityContext`). Never "fix" problems with `chmod 777`.

### Write a bash script to process logs.

Task: summarise an nginx access log — total requests, 5xx error rate, top endpoints with errors, slowest requests (assumes `$request_time` is the last field).

```bash
#!/usr/bin/env bash
# usage: ./logsummary.sh access.log [access.log.1.gz ...]
set -euo pipefail

[[ $# -ge 1 ]] || { echo "usage: $0 LOGFILE..." >&2; exit 1; }

# zcat -f reads both plain and gzipped files
read_logs() { zcat -f -- "$@"; }

echo "== Totals =="
read_logs "$@" | awk '
  { total++; if ($9 ~ /^5/) err++ }
  END { printf "requests=%d 5xx=%d rate=%.2f%%\n", total, err, total ? 100*err/total : 0 }'

echo "== Top 10 endpoints returning 5xx =="
read_logs "$@" | awk '$9 ~ /^5/ { split($7, p, "?"); print $9, p[1] }' \
  | sort | uniq -c | sort -rn | head -10

echo "== Requests per minute (last 5 buckets) =="
read_logs "$@" | awk '{ print substr($4, 2, 17) }' | sort | uniq -c | tail -5

echo "== 10 slowest requests =="
read_logs "$@" | awk '{ print $NF, $9, $7 }' | sort -rn | head -10
```
Key points to mention: `set -euo pipefail` for safety, quoting `"$@"`, handling rotated `.gz` files, streaming (works on multi-GB files in O(1) memory except the `sort` steps, which spill to disk), and for JSON logs swapping `awk` for `jq`. Complexity: O(n) to scan plus O(k log k) to sort the k distinct keys. If logic grows beyond ~50 lines, switch to Python.

### SSH tunnelling / port forwarding (getting into a customer's network).

```bash
# Local forward: reach a DB inside the customer network via a bastion.
# localhost:5433 on my laptop -> bastion -> db.internal:5432
ssh -N -L 5433:db.internal:5432 me@bastion.customer.com
psql -h localhost -p 5433 -U readonly appdb

# Jump host: SSH to a private server through a bastion
ssh -J me@bastion.customer.com me@10.0.3.15

# Remote forward: expose my local service (e.g. a webhook receiver on :8000) to the remote side
ssh -N -R 9000:localhost:8000 me@remote-host      # remote-host:9000 -> my laptop:8000

# Dynamic forward: SOCKS proxy to browse internal web UIs (Grafana, admin consoles)
ssh -N -D 1080 me@bastion.customer.com            # set browser SOCKS5 proxy to localhost:1080

# Keep a tunnel alive
autossh -M 0 -N -o ServerAliveInterval=30 -o ServerAliveCountMax=3 -L 5433:db.internal:5432 me@bastion
```
`~/.ssh/config` makes it repeatable:
```
Host cust-bastion
  HostName bastion.customer.com
  User me
  IdentityFile ~/.ssh/cust_ed25519
Host cust-db-tunnel
  HostName 10.0.3.15
  ProxyJump cust-bastion
  LocalForward 5433 db.internal:5432
```
- **Etiquette and security (what interviewers listen for):** only use access paths the customer has approved; use their bastion/VPN/PAM tools (CyberArk, Teleport, AWS SSM Session Manager — `aws ssm start-session` with port forwarding needs no open SSH port); key-based auth, no shared keys; least-privilege read-only accounts; bind forwards to `localhost`; close tunnels when done; log what you did. Never build a reverse tunnel out of a customer network without explicit permission — it bypasses their firewall and is a fireable offence at many companies.

### Cron jobs and systemd services.

**Cron:** time-based scheduler. Edit with `crontab -e`; format `min hour day-of-month month day-of-week command`.
```cron
# Nightly SFTP pull at 02:15, with lock to avoid overlap and logs captured
15 2 * * * flock -n /tmp/pull.lock /opt/etl/pull.sh >> /var/log/etl/pull.log 2>&1
*/5 * * * * /opt/bin/healthcheck.sh       # every 5 minutes
```
Gotchas: minimal `PATH` and env (use absolute paths, source config), timezone of the server, no output unless redirected, overlapping runs (`flock`), silent failures — add monitoring (healthchecks.io/dead-man's switch).

**systemd:** init system and service manager — supervises long-running processes with restarts, dependencies, logging (journald) and resource limits.
```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My API service
After=network-online.target
Wants=network-online.target

[Service]
User=appuser
WorkingDirectory=/opt/myapp
EnvironmentFile=/etc/myapp/env
ExecStart=/opt/myapp/venv/bin/uvicorn src.main:app --host 0.0.0.0 --port 8000
Restart=on-failure
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl daemon-reload && sudo systemctl enable --now myapp
systemctl status myapp ; journalctl -u myapp -f
```
- **systemd timers** (`.timer` units) are a modern alternative to cron: logs in journald, `Persistent=true` catches up missed runs after downtime, `systemctl list-timers` shows the schedule.
- In Kubernetes, the equivalents are a Deployment (service) and a CronJob (`concurrencyPolicy: Forbid` to prevent overlap).
