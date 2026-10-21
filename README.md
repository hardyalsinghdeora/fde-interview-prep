# Forward Deployed Engineer (FDE) Interview Prep

A structured question bank for **Forward Deployed Engineer / Forward Deployed Software Engineer (FDSE) / Deployment Strategist-adjacent** interviews at companies such as Palantir, OpenAI, Anthropic, Scale AI, Databricks, Ramp, Sierra, Decagon, Harvey, Glean, Cohere, Mistral, Salesforce (Agentforce), and AI-native startups.

The goal is coverage: every base question that can come up in an FDE loop is listed here, grouped by round. Use each file as a checklist. Answer every question out loud and tick it off.

> Sources: candidate reports (Glassdoor, Blind, 1point3acres), prep guides (Exponent/Aced, IGotAnOffer, Educative, techinterview.org, GSD Council), and the common structure of FDE loops. Interview content changes and is often under NDA, so treat company-specific items as *reported*, not guaranteed.

---

## Which role are you applying for?

FDE is one of a family of customer-facing technical roles. **[15-roles](roles/README.md)** covers 11 of them: Forward Deployed Engineer, Deployment Strategist, Solutions Engineer, Solutions Architect, Applied AI Engineer, Agent Engineer, Customer Engineer/TAM, Implementation Engineer, Technical Support Engineer, Developer Relations, and Forward Deployed Data/ML. It includes:

- [Role map](roles/README.md): how the roles differ and which interview rounds each one gets
- [Common questions](roles/COMMON-QUESTIONS.md): asked in every role, linked to their answers
- One file per role with role-specific questions and model answers

## What an FDE loop usually looks like

| Stage | What it tests | File |
|---|---|---|
| Recruiter / "Why FDE?" screen | Motivation, travel, customer-facing fit | [01-recruiter-and-motivation](topics/01-recruiter-and-motivation/README.md) |
| Coding screen (LeetCode-style or practical) | DS&A, clean code, changing requirements | [02-coding](topics/02-coding/README.md) |
| System design (applied) | Architecture under real customer constraints | [03-system-design](topics/03-system-design/README.md) |
| Decomposition (Palantir, Databricks, many AI labs) | Breaking a vague problem into a plan | [04-decomposition](topics/04-decomposition/README.md) |
| Learning round (Palantir) | Picking up a new concept/API fast | [05-learning-round](topics/05-learning-round/README.md) |
| AI / LLM use-case & take-home (OpenAI, Anthropic, Scale, etc.) | RAG, agents, evals, prompting, production LLM | [06-ai-llm](topics/06-ai-llm/README.md) |
| Data / SQL | Pipelines, modeling, SQL fluency | [07-data-and-sql](topics/07-data-and-sql/README.md) |
| Cloud / DevOps / Integration | Deploying into a customer's environment | [08-cloud-devops-integration](topics/08-cloud-devops-integration/README.md) |
| Customer scenario / role-play | Scoping, expectation management, communication | [09-customer-scenarios](topics/09-customer-scenarios/README.md) |
| Behavioral / Hiring manager / Values | Ownership, ambiguity, conflict, impact | [10-behavioral](topics/10-behavioral/README.md) |
| Debugging round | Reading unfamiliar code, finding failures | [12-debugging-troubleshooting](topics/12-debugging-troubleshooting/README.md) |
| CS fundamentals (rapid-fire) | Networking, OS, DBs, concurrency, security | [13-cs-fundamentals](topics/13-cs-fundamentals/README.md) |
| Your questions for them | Judgment and curiosity | [14-questions-to-ask](topics/14-questions-to-ask/README.md) |

Company-by-company notes: [11-company-specific](topics/11-company-specific/README.md)
Role overview, frameworks and a study plan: [00-overview](topics/00-overview/README.md)

---

## Repository structure

```
fde-interview-prep/
├── README.md                          ← you are here
├── 00-overview/                       ← what an FDE is, answer frameworks, 4-week plan
│   (every folder: README.md = questions, ANSWERS.md = model answers)
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
├── 14-questions-to-ask/               ← questions for the interviewer
└── 15-roles/                          ← role map, common questions, role-specific Q&A
```

## Model answers

Every folder has an `ANSWERS.md` next to its `README.md` with a model answer for each question, in the same order. Coding and AI/LLM are split into two answer files each.

| Section | Questions | Answers |
|---|---|---|
| 00 Overview (story bank) | [README](topics/00-overview/README.md) | [ANSWERS](topics/00-overview/ANSWERS.md) |
| 01 Recruiter & motivation | [README](topics/01-recruiter-and-motivation/README.md) | [ANSWERS](topics/01-recruiter-and-motivation/ANSWERS.md) |
| 02 Coding | [README](topics/02-coding/README.md) | [ANSWERS](topics/02-coding/ANSWERS.md) · [DS&A topics](topics/02-coding/ANSWERS-dsa-topics.md) |
| 03 System design | [README](topics/03-system-design/README.md) | [ANSWERS](topics/03-system-design/ANSWERS.md) |
| 04 Decomposition | [README](topics/04-decomposition/README.md) | [ANSWERS](topics/04-decomposition/ANSWERS.md) |
| 05 Learning round | [README](topics/05-learning-round/README.md) | [ANSWERS](topics/05-learning-round/ANSWERS.md) |
| 06 AI / LLM | [README](topics/06-ai-llm/README.md) | [ANSWERS (A–E)](topics/06-ai-llm/ANSWERS.md) · [Part 2 (F–J)](topics/06-ai-llm/ANSWERS-part2.md) |
| 07 Data & SQL | [README](topics/07-data-and-sql/README.md) | [ANSWERS](topics/07-data-and-sql/ANSWERS.md) |
| 08 Cloud, DevOps & integration | [README](topics/08-cloud-devops-integration/README.md) | [ANSWERS](topics/08-cloud-devops-integration/ANSWERS.md) |
| 09 Customer scenarios | [README](topics/09-customer-scenarios/README.md) | [ANSWERS](topics/09-customer-scenarios/ANSWERS.md) |
| 10 Behavioral | [README](topics/10-behavioral/README.md) | [ANSWERS](topics/10-behavioral/ANSWERS.md) |
| 11 Company-specific | [README](topics/11-company-specific/README.md) | [ANSWERS](topics/11-company-specific/ANSWERS.md) |
| 12 Debugging | [README](topics/12-debugging-troubleshooting/README.md) | [ANSWERS](topics/12-debugging-troubleshooting/ANSWERS.md) |
| 13 CS fundamentals | [README](topics/13-cs-fundamentals/README.md) | [ANSWERS](topics/13-cs-fundamentals/ANSWERS.md) |
| 14 Questions to ask | [README](topics/14-questions-to-ask/README.md) | [ANSWERS](topics/14-questions-to-ask/ANSWERS.md) |
| 15 Roles | [Role map](roles/README.md) · [Common questions](roles/COMMON-QUESTIONS.md) | Answers inside each role file |

Behavioral answers are **templates with labelled examples**, not real stories. Replace them with your own experience.

## How to use

1. Read [00-overview](topics/00-overview/README.md) for frameworks (STAR, decomposition, system design).
2. Go through each folder and answer every question out loud or in writing first, then compare with `ANSWERS.md`.
3. Mark questions with `- [x]` as you finish them. Fork the repo and keep your own answers in an `answers/` folder.
4. In the last week, run timed mocks: 1 coding, 1 decomposition, 1 AI design, 1 behavioral per day.

## Contributing

Heard a new question? Add it to the right file as a `- [ ]` item. If it's company-specific, also add it to that company's section in `11-company-specific`.
