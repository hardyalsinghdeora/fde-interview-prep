# 00 — Overview: The FDE Role, Frameworks, and Study Plan

## What is a Forward Deployed Engineer?

An FDE is a software engineer who works directly with customers: embedded with a client team, building and shipping solutions on top of the company's platform, and feeding what they learn back to the core product team. The role mixes:

- **Engineering**: writing production code, integrations, data pipelines, LLM apps
- **Consulting**: scoping vague problems, defining success, managing stakeholders
- **Product**: spotting repeated patterns across customers and turning them into product features

Common titles: Forward Deployed Engineer, Forward Deployed Software Engineer (FDSE, Palantir), Deployment Strategist (Palantir, non-engineering sibling), Solutions Engineer, AI Engineer (customer-facing), Applied AI Engineer, Customer Engineer, Field Engineer, Implementation Engineer.

## What interviewers score

| Signal | What it looks like |
|---|---|
| Technical depth | Writes working code fast, knows trade-offs, debugs calmly |
| Problem decomposition | Asks clarifying questions, defines success, breaks into MECE parts |
| Customer empathy | Thinks about the end user, not just the tech |
| Communication | Explains technical ideas to non-technical people; thinks out loud |
| Ownership & bias to action | Ships under ambiguity; owns outcomes end to end |
| Learning speed | Picks up a new domain/API in minutes, not weeks |
| Judgment | Knows when to say no, when to hack, when to build properly |

## Frameworks

### STAR(+L) for behavioral
**S**ituation → **T**ask → **A**ction (what *you* did) → **R**esult (numbers) → **L**earning (what you'd do differently).

### Decomposition (Palantir style)
1. **Clarify**: Who is the user? What decision are they trying to make? What does success look like? What exists today?
2. **Scope**: Pick the most valuable slice. State assumptions explicitly.
3. **Break down**: Split into MECE sub-problems (data, logic, interface, operations).
4. **Data**: What data exists, where, how messy, how fresh, who owns it?
5. **Solution sketch**: End-to-end, simplest thing first (v0 → v1 → v2).
6. **Risks & trade-offs**: Privacy, scale, adoption, failure modes.
7. **Measure**: Metrics, feedback loop, iteration plan.

### System design
Requirements (functional + non-functional) → API → data model → high-level design → deep dives (scaling, consistency, failure) → trade-offs → monitoring.

### AI system design add-on
Use case & success metric → data/knowledge sources → model choice → prompt/RAG/agent design → evals (offline + online) → guardrails & safety → latency/cost → monitoring & feedback loop.

### Customer scenario (role-play)
Listen → acknowledge → clarify the real need → align on outcome → propose options with trade-offs → agree next steps → follow up in writing.

## 4-week study plan

| Week | Focus |
|---|---|
| 1 | Coding (arrays, strings, hashing, graphs, intervals) daily + behavioral story bank (10 stories) |
| 2 | System design + data/SQL + cloud/integration basics |
| 3 | AI/LLM: build a small RAG app + agent + eval harness; decomposition practice |
| 4 | Company-specific prep, mock interviews, customer role-plays, questions to ask |

## Story bank checklist (prepare one story for each)

- [ ] Biggest technical achievement
- [ ] Shipped something under extreme ambiguity
- [ ] Worked directly with a customer/user to solve their problem
- [ ] Disagreed with a stakeholder/manager
- [ ] Failure / mistake and what you learned
- [ ] Learned something new very fast
- [ ] Pushed back on a customer request / said no
- [ ] Production incident you handled
- [ ] Influenced without authority
- [ ] Prioritized under competing deadlines
- [ ] Turned a one-off solution into something reusable
- [ ] Explained something technical to a non-technical audience
