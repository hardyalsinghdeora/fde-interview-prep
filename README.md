# Forward Deployed Engineer (FDE) Interview Prep

A structured question bank for **Forward Deployed Engineer / Forward Deployed Software Engineer (FDSE) / Deployment Strategist-adjacent** interviews at companies such as Palantir, OpenAI, Anthropic, Scale AI, Databricks, Ramp, Sierra, Decagon, Harvey, Glean, Cohere, Mistral, Salesforce (Agentforce), and AI-native startups.

The goal is coverage: every base question that can come up in an FDE loop is listed here, grouped by round. Use each file as a checklist. Answer every question out loud and tick it off.

> Sources: candidate reports (Glassdoor, Blind, 1point3acres), prep guides (Exponent/Aced, IGotAnOffer, Educative, techinterview.org, GSD Council), and the common structure of FDE loops. Interview content changes and is often under NDA, so treat company-specific items as *reported*, not guaranteed.

---

## What an FDE loop usually looks like

| Stage | What it tests | File |
|---|---|---|
| Recruiter / "Why FDE?" screen | Motivation, travel, customer-facing fit | [01-recruiter-and-motivation](01-recruiter-and-motivation/README.md) |
| Coding screen (LeetCode-style or practical) | DS&A, clean code, changing requirements | [02-coding](02-coding/README.md) |
| System design (applied) | Architecture under real customer constraints | [03-system-design](03-system-design/README.md) |
| Decomposition (Palantir, Databricks, many AI labs) | Breaking a vague problem into a plan | [04-decomposition](04-decomposition/README.md) |
| Learning round (Palantir) | Picking up a new concept/API fast | [05-learning-round](05-learning-round/README.md) |
| AI / LLM use-case & take-home (OpenAI, Anthropic, Scale, etc.) | RAG, agents, evals, prompting, production LLM | [06-ai-llm](06-ai-llm/README.md) |
| Data / SQL | Pipelines, modeling, SQL fluency | [07-data-and-sql](07-data-and-sql/README.md) |
| Cloud / DevOps / Integration | Deploying into a customer's environment | [08-cloud-devops-integration](08-cloud-devops-integration/README.md) |
| Customer scenario / role-play | Scoping, expectation management, communication | [09-customer-scenarios](09-customer-scenarios/README.md) |
| Behavioral / Hiring manager / Values | Ownership, ambiguity, conflict, impact | [10-behavioral](10-behavioral/README.md) |
| Debugging round | Reading unfamiliar code, finding failures | [12-debugging-troubleshooting](12-debugging-troubleshooting/README.md) |
| CS fundamentals (rapid-fire) | Networking, OS, DBs, concurrency, security | [13-cs-fundamentals](13-cs-fundamentals/README.md) |
| Your questions for them | Judgment and curiosity | [14-questions-to-ask](14-questions-to-ask/README.md) |

Company-by-company notes: [11-company-specific](11-company-specific/README.md)
Role overview, frameworks and a study plan: [00-overview](00-overview/README.md)

---

## Repository structure

```
fde-interview-prep/
├── README.md                          ← you are here
├── 00-overview/                       ← what an FDE is, answer frameworks, 4-week plan
├── 01-recruiter-and-motivation/       ← "Why FDE not SWE?", resume walk-through
├── 02-coding/                         ← DS&A by topic + practical/OOP + reported problems
├── 03-system-design/                  ← classic + applied + AI system design prompts
├── 04-decomposition/                  ← open-ended problem breakdown prompts
├── 05-learning-round/                 ← learn-on-the-spot topics
├── 06-ai-llm/                         ← LLM fundamentals, RAG, agents, evals, MCP, safety
├── 07-data-and-sql/                   ← SQL, data modeling, pipelines, Spark
├── 08-cloud-devops-integration/       ← cloud, APIs, auth, deployment, on-prem
├── 09-customer-scenarios/             ← role-plays and client-situation questions
├── 10-behavioral/                     ← STAR questions by theme
├── 11-company-specific/               ← Palantir, OpenAI, Anthropic, Scale, Databricks, ...
├── 12-debugging-troubleshooting/      ← production triage, debugging prompts
├── 13-cs-fundamentals/                ← networking, OS, DB, concurrency, security
└── 14-questions-to-ask/               ← questions for the interviewer
```

## How to use

1. Read [00-overview](00-overview/README.md) for frameworks (STAR, decomposition, system design).
2. Go through each folder and answer every question out loud or in writing.
3. Mark questions with `- [x]` as you finish them. Fork the repo and keep your own answers in an `answers/` folder.
4. In the last week, run timed mocks: 1 coding, 1 decomposition, 1 AI design, 1 behavioral per day.

## Contributing

Heard a new question? Add it to the right file as a `- [ ]` item. If it's company-specific, also add it to that company's section in `11-company-specific`.
