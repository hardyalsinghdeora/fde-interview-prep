# Study Plan — Deployment Strategist

For candidates preparing for a Deployment Strategist (or forward deployed PM / engagement lead) loop; assumes about 10–12 hours a week for six weeks, with no production coding required.

Read the role overview first: [README.md — The role](./README.md#the-role) and the [typical interview loop](./README.md#typical-interview-loop). This loop is mostly case interviews, behavioural, writing and stakeholder role-play. There is usually no coding round, so this plan spends its time on structured thinking, numbers, discovery and clear writing instead.

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim areas you rate 4 or 5.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Case structuring / decomposition | | Given a vague operational problem, you clarify the user, the decision and the success metric, then produce a structured, prioritised plan in 30–40 minutes. |
| Root-cause thinking | | You can separate symptom from cause and name the data that would confirm it. |
| Business cases and estimation | | You build a value estimate with explicit assumptions and a sensitivity check, and do the arithmetic aloud without errors. |
| Stakeholder management | | You can draw a stakeholder map for a new account and explain how you would handle a sponsor and users who want different things. |
| Discovery and workshop facilitation | | You can run 15 minutes of discovery with a skeptical executive and end on a concrete next step. |
| Data and technical literacy | | You write simple SQL joins and group-bys, spot data quality issues, and explain RAG, ontology and LLM limits in plain words. |
| Writing | | You can write a one-page recommendation memo that leads with the answer and survives being cut by a third. |
| Presenting | | You can present a recommendation in five minutes to senior people and handle hostile questions. |
| Behavioural stories | | You have stories for influence without authority, conflict, owning an outcome you did not control, and bad news. |

## 6-week plan

Assumes ~10–12 hours a week. Cases are the core of this loop: do at least two timed cases every week from Week 2 onwards, whatever the week's theme.

### Week 1 — The role, motivation and a case method

**Goals:** understand what the role owns, explain why you want it, and settle on one repeatable case structure.

- [ ] Read [Role understanding and motivation — model answers](./README.md#role-understanding-and-motivation-1).
- [ ] Read the decomposition intro and frameworks: [04 Decomposition — README](../../topics/04-decomposition/README.md) and the framework section in [00 Overview](../../topics/00-overview/README.md#frameworks).
- [ ] Adapt the motivation answers for a business role: [01 Recruiter and motivation — answers](../../topics/01-recruiter-and-motivation/ANSWERS.md#motivation--role-fit).
- [ ] Read [How do you structure an ambiguous problem when you only have a few minutes to think?](./README.md#how-do-you-structure-an-ambiguous-problem-when-you-only-have-a-few-minutes-to-think)
- [ ] Work Tier 1 of [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md#tier-1--asked-in-almost-every-loop), skipping the technical items.

**Deliverables:**
1. A one-page case template you will reuse: clarify (user, decision, metric) → map the current workflow → hypotheses → data needed → phased plan → risks.
2. A recorded 60-second "why a forward deployed business role, and why not consulting or PM?" answer.

**Mock:** one 30-minute case with a friend using the [hospital wait-times case](./README.md#case-a-regional-hospital-network-says-emergency-department-waits-are-too-long-how-would-you-approach-it); focus only on clarifying well.

**End of week check:** you ask at least three clarifying questions before structuring, every time.

### Week 2 — Case reps across sectors

**Goals:** volume. Get comfortable with very different domains.

- [ ] Do the README cases: [airline maintenance](./README.md#case-an-airline-wants-to-reduce-flight-delays-caused-by-maintenance-where-do-you-start) and [government benefits AI](./README.md#case-a-government-agency-wants-to-use-ai-across-its-benefits-processing-operations-frame-the-problem).
- [ ] Do one prompt each from [04 — A. Public sector](../../topics/04-decomposition/ANSWERS.md#a-public-sector--government--defense), [B. Healthcare](../../topics/04-decomposition/ANSWERS.md#b-healthcare) and [C. Commercial](../../topics/04-decomposition/ANSWERS.md#c-commercial--enterprise).
- [ ] Read [prioritising ten problems](./README.md#the-customer-lists-ten-problems-they-want-solved-how-do-you-decide-which-to-tackle-first) and [finding the root problem](./README.md#how-do-you-know-you-have-found-the-root-problem-rather-than-a-symptom).
- [ ] Rehearse [04 — F. Typical push-backs](../../topics/04-decomposition/ANSWERS.md#f-typical-interviewer-follow-ups--push-backs).

**Deliverable:** four timed case write-ups (one page each), each ending with "first thing I would do on Monday".

**Mock:** a 40-minute case with a peer who pushes back hard ("that will not work here", "you have half the budget").

**End of week check:** you finish a case on time with a prioritised first step rather than a list of everything.

### Week 3 — Numbers: business cases, ROI and estimation

**Goals:** make the arithmetic fast and the assumptions visible.

- [ ] Read [Business cases and ROI — model answers](./README.md#business-cases-and-roi-1), including the [predictive-maintenance case](./README.md#build-a-business-case-for-a-predictive-maintenance-deployment-at-a-manufacturer-walk-through-the-numbers) and the [claims estimation](./README.md#estimation-roughly-how-many-staff-hours-a-year-does-a-mid-sized-insurer-spend-on-manual-claims-document-review).
- [ ] Study [09 Customer scenarios — E. Business acumen](../../topics/09-customer-scenarios/ANSWERS.md#e-business-acumen).
- [ ] Read [presenting a pilot that under-delivered](./README.md#the-pilot-delivered-less-value-than-promised-how-do-you-present-it-to-the-sponsor).
- [ ] Add supply chain to your sector reps: read [15-supply-chain-ai](../../topics/15-supply-chain-ai/README.md) sections [A (fundamentals)](../../topics/15-supply-chain-ai/README.md#a-supply-chain-fundamentals), [G (case prompts)](../../topics/15-supply-chain-ai/README.md#g-decomposition--case-prompts) and [H (customer scenarios)](../../topics/15-supply-chain-ai/README.md#h-customer-scenarios), with [answers](../../topics/15-supply-chain-ai/ANSWERS.md) and [part 2](../../topics/15-supply-chain-ai/ANSWERS-part2.md).

**Deliverables:**
1. A spreadsheet business case for one use case with a base, low and high scenario and a one-line sensitivity summary ("the case depends mostly on X").
2. Two estimation questions solved out loud and recorded.

**Mock:** an estimation question followed by a business-case question (40 minutes); the partner challenges every assumption.

**End of week check:** you can state the three assumptions that drive your number and how wrong each could be before the case falls apart.

### Week 4 — Stakeholders, discovery and workshops

**Goals:** handle people, not just problems.

- [ ] Read [Stakeholder management — model answers](./README.md#stakeholder-management-1).
- [ ] Read [Workshop facilitation and discovery — model answers](./README.md#workshop-facilitation-and-discovery-1), especially the [skeptical COO role-play](./README.md#role-play-run-the-first-15-minutes-of-a-discovery-meeting-with-a-skeptical-coo).
- [ ] Study [09 — A. Scoping and requirements](../../topics/09-customer-scenarios/ANSWERS.md#a-scoping--requirements) and [B. Difficult situations](../../topics/09-customer-scenarios/ANSWERS.md#b-difficult-situations).
- [ ] Behavioural: [10 — Conflict and collaboration](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration) and [Ambiguity](../../topics/10-behavioral/ANSWERS.md#ambiguity--bias-for-action).

**Deliverables:**
1. A stakeholder map for an imagined account (sponsor, blockers, champions, frontline users) with one action per person.
2. A half-day discovery workshop agenda with timings, exercises and the outputs you want.

**Mock:** a 20-minute discovery role-play with a friend playing a skeptical COO, followed by a 20-minute "middle manager is blocking you" scenario.

**End of week check:** you listen more than you talk in discovery, and you close every role-play on a dated next step.

### Week 5 — Data literacy, technical fluency and writing

**Goals:** credibility with engineers, and writing that executives act on.

- [ ] Read [Technical and data literacy — model answers](./README.md#technical-and-data-literacy-1), including the [orders and shipments spreadsheet exercise](./README.md#exercise-you-are-given-a-small-spreadsheet-of-orders-and-shipments-what-would-you-analyse-and-what-could-you-conclude).
- [ ] Basic SQL: [07 — A. SQL coding prompts](../../topics/07-data-and-sql/ANSWERS.md#a-sql-coding-prompts) (joins and group-bys only) and [D. Pipelines and data quality](../../topics/07-data-and-sql/ANSWERS.md#d-pipelines--data-quality).
- [ ] LLM concepts for executives: [06 AI/LLM — A. Fundamentals](../../topics/06-ai-llm/ANSWERS.md#a-llm-fundamentals) and [C. RAG](../../topics/06-ai-llm/ANSWERS.md#c-rag-retrieval-augmented-generation), at concept level only.
- [ ] Read [Writing and presenting — model answers](./README.md#writing-and-presenting-1) and [09 — C. Communication](../../topics/09-customer-scenarios/ANSWERS.md#c-communication).

**Deliverables:**
1. A one-page memo recommending whether a customer should expand a deployment, then cut by a third.
2. A short analysis of a public sample dataset (orders, flights or similar) in SQL or a spreadsheet, with three findings and two data quality caveats.

**Mock:** a five-minute recommendation presentation to two or three friends playing the customer's leadership team, with five minutes of hostile questions.

**End of week check:** your memo's first sentence is the recommendation, and you can explain "what an LLM can and cannot reliably do" to a non-technical person in two minutes.

### Week 6 — Outcome ownership, company and mission, full loop

**Goals:** tie it together and walk in with a clear view on the company.

- [ ] Read [Account growth and outcome ownership — model answers](./README.md#account-growth-and-outcome-ownership-1).
- [ ] Read [Company and mission](./README.md#company-and-mission-1) and the target company in [11 Company-specific](../../topics/11-company-specific/ANSWERS.md), including the [research checklist](../../topics/11-company-specific/ANSWERS.md#company-research-checklist-do-for-every-company).
- [ ] Behavioural polish: [10 — Ownership and impact](../../topics/10-behavioral/ANSWERS.md#ownership--impact), [Values, ethics and culture](../../topics/10-behavioral/ANSWERS.md#values-ethics--culture).
- [ ] Prepare questions: [14 — About the role](../../topics/14-questions-to-ask/ANSWERS.md#about-the-role) and [To the hiring manager](../../topics/14-questions-to-ask/ANSWERS.md#to-the-hiring-manager).
- [ ] Tick off the role's [Prep checklist](./README.md#prep-checklist).

**Deliverable:** a half-page note on the company's mission and customers, including any ethical questions and your honest position, and a two-minute plain-language explanation of its platform.

**Mock:** a full mock loop: two cases (40 minutes each), a behavioural interview (45), and a written exercise (30 minutes to write a memo from a short brief).

**End of week check:** you can do two cases back to back without your structure slipping, and give your ethics view calmly.

## 2-week crash plan

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and motivation | Read the role; record "why this role" | [README — role](./README.md#the-role), [Role understanding answers](./README.md#role-understanding-and-motivation-1) |
| 2 | Case method | Build your case template; one case untimed | [00 — Frameworks](../../topics/00-overview/README.md#frameworks), [hospital case](./README.md#case-a-regional-hospital-network-says-emergency-department-waits-are-too-long-how-would-you-approach-it) |
| 3 | Cases | Two timed cases | [04 — B](../../topics/04-decomposition/ANSWERS.md#b-healthcare), [04 — C](../../topics/04-decomposition/ANSWERS.md#c-commercial--enterprise) |
| 4 | Cases | Two timed cases + push-backs | [04 — A](../../topics/04-decomposition/ANSWERS.md#a-public-sector--government--defense), [04 — F](../../topics/04-decomposition/ANSWERS.md#f-typical-interviewer-follow-ups--push-backs) |
| 5 | Numbers | One business case, two estimations | [Business cases answers](./README.md#business-cases-and-roi-1) |
| 6 | Stories | Six STAR stories | [10 Behavioral](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration), [my story bank](../../my-prep/STORY-BANK.md) |
| 7 | Mock day | Two cases with a partner | [Mock schedule](#mock-interview-schedule) |
| 8 | Stakeholders | Stakeholder map; sponsor vs users | [Stakeholder answers](./README.md#stakeholder-management-1) |
| 9 | Discovery | Skeptical COO role-play | [Workshop and discovery answers](./README.md#workshop-facilitation-and-discovery-1), [09 — A](../../topics/09-customer-scenarios/ANSWERS.md#a-scoping--requirements) |
| 10 | Data literacy | Basic SQL; spreadsheet exercise | [Technical and data literacy](./README.md#technical-and-data-literacy-1), [07 — A](../../topics/07-data-and-sql/ANSWERS.md#a-sql-coding-prompts) |
| 11 | Writing | One memo, one executive update | [Writing and presenting](./README.md#writing-and-presenting-1) |
| 12 | Presenting | Five-minute recommendation to a panel | [Presentation exercise](./README.md#exercise-present-a-recommendation-in-five-minutes-to-a-panel-playing-the-customers-leadership-team) |
| 13 | Company | Mission, ethics, platform explanation; full mock | [Company and mission](./README.md#company-and-mission-1), [11](../../topics/11-company-specific/ANSWERS.md) |
| 14 | Rest and review | Re-read template, stories and memo | [README checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–35 | One case or decomposition prompt, timed and spoken out loud (record it twice a week). |
| 35–50 | Mental maths or one estimation question; write the assumptions down. |
| 50–75 | Writing: rewrite yesterday's case as a half-page memo, or read one model answer and summarise it in five lines. |
| 75–90 | One behavioural story or one stakeholder scenario out loud; note one improvement. |

Weekends: the week's deliverable and the mock.

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | One case, clarifying only | A friend |
| 2 | Case with hard push-back | Someone with consulting, product or operations experience |
| 3 | Estimation + business case | A numerate friend; case-interview partners from consulting prep communities also work |
| 4 | Discovery role-play + blocker scenario | A friend playing a senior skeptic |
| 5 | Five-minute presentation with Q&A | Two or three people at once |
| 6 | Full loop with written exercise | Two different partners; Exponent and similar platforms offer product and case mocks |

Two timed cases a week from Week 2 is the minimum. Record at least one per week and check you clarified before you solved.

## Fast track for the repo owner's profile

- **Skip or skim:** the SQL and data literacy material in Week 5; deep MySQL and PostgreSQL experience goes well past what this loop asks. Keep only the "explain it to an executive" practice.
- **Skim:** technical credibility questions such as [how will you earn credibility with engineers](./README.md#you-will-not-write-production-code-how-will-you-earn-credibility-with-engineers-and-with-the-customers-technical-team); about 15 years of engineering answers this directly.
- **Biggest gap 1 — case interviews.** There is no evidence of formal case or consulting practice. Give Weeks 2–3 extra time and do more timed cases than the plan asks.
- **Biggest gap 2 — external customer and executive work.** Most stakeholder work has been with internal sales, finance and academic teams. Use it, but practise discovery and presentation with people playing outside executives.
- **Biggest gap 3 — motivation.** Expect "why leave engineering for a non-coding role?". Prepare an honest answer; if the real interest is building, a forward deployed engineer role may fit better (see [01 FDE](../01-forward-deployed-engineer/README.md)).
- **Business case and ROI rounds:** the order/subscription/commerce platform work (discount approval rules engine, invoicing and payments tied to a cash-collection metric) shows linking systems to a business metric.
- **Stakeholder and outcome-ownership rounds:** the large data migration across six product lines with reconciliation, and coordinating sales, finance and academic teams, are good stories for conflicting incentives and owning an outcome.
- **Writing round:** monthly programme reports, release plans and onboarding plans are real writing evidence; bring one anonymised structure as a talking point.
- **AI literacy:** the speech-to-text provider evaluation (a contribution, not sole authorship) with a buy-vs-build cost model and data-protection constraints is a strong example of explaining AI trade-offs to decision makers.
- Keep all stories in [../../my-prep/STORY-BANK.md](../../my-prep/STORY-BANK.md).

## Resources

- *Case in Point* by Marc Cosentino — case interview structures and practice cases.
- *Case Interview Secrets* by Victor Cheng — hypothesis-driven case thinking.
- *The Pyramid Principle* by Barbara Minto — leading with the answer in memos and presentations.
- *The Mom Test* by Rob Fitzpatrick — asking discovery questions that get honest answers.
- *Measure What Matters* by John Doerr — defining outcome metrics with customers.
- The target company's own website, documentation and published customer case studies (for Palantir, its public Foundry and AIP documentation).
- Mock platforms with case and product mocks: Exponent; consulting case-practice communities for partners.
