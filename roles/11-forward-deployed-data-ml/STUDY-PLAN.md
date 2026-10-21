# Study Plan — Forward Deployed Data Scientist / ML Engineer

For data scientists, ML engineers and data-heavy backend engineers preparing for customer-facing data and ML roles; assumes 6 weeks at about 10–12 hours a week, or the 2-week crash plan if an interview is already booked.

Role overview, loop and model answers: [README.md](./README.md). Questions every role gets: [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).

---

## Before you start

Spend 30 minutes rating yourself. Skim what you rate 4–5; spend extra time on 1–2.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| SQL and pandas wrangling | | You write window functions, deduplication and cohort queries, and the pandas equivalents, without looking anything up. |
| Data profiling and quality | | Your first-hour checks on a new dataset (types, nulls, duplicates, ranges, keys, time coverage) are automatic. |
| ML fundamentals | | You can explain bias-variance, regularisation, tree ensembles, class imbalance and calibration, and choose a baseline. |
| Validation and leakage | | You spot target leakage, use time-based or grouped splits, and build features with a cutoff date. |
| Problem framing with the customer | | You turn "predict churn" into a decision, an action, a metric and a value estimate before touching data. |
| Explaining models | | You explain precision, recall and threshold choice with a cost table to a non-technical manager, and a single prediction with SHAP. |
| Experiment design | | You can size a pilot, pick a randomisation unit, and fall back to difference-in-differences when randomisation is impossible. |
| Deployment and MLOps | | You can sketch batch vs real-time serving, monitoring, drift detection, retraining and handover inside a customer's cloud. |
| LLMs for data work | | You can compare an LLM against a classical classifier on the same test set for cost and accuracy. |

---

## 6-week plan

### Week 1 — SQL, pandas and first-hour data checks

**Goals:** pass the technical screen (SQL, Python/pandas, statistics basics) and make data profiling automatic.

- [ ] Read [The role](./README.md#the-role) and [Typical interview loop](./README.md#typical-interview-loop).
- [ ] Read [07-data-and-sql](../../topics/07-data-and-sql/README.md) sections [A (SQL prompts)](../../topics/07-data-and-sql/README.md#a-sql-coding-prompts), [B (concepts)](../../topics/07-data-and-sql/README.md#b-sql-concepts), [D (pipelines and data quality)](../../topics/07-data-and-sql/README.md#d-pipelines--data-quality) and [F (pandas)](../../topics/07-data-and-sql/README.md#f-pandas--python-data-wrangling), with [answers](../../topics/07-data-and-sql/ANSWERS.md).
- [ ] Work through [the first-hour data quality checks](./README.md#what-data-quality-checks-do-you-run-in-the-first-hour-with-a-new-customer-dataset), [missing values](./README.md#how-do-you-handle-missing-values-and-when-does-missingness-itself-carry-signal) and [two systems that disagree](./README.md#two-customer-systems-disagree-about-the-same-records-how-do-you-decide-which-to-trust-for-modelling).

**Deliverables:**
1. A reusable profiling notebook or script that, given any CSV, reports types, null rates, duplicates, key uniqueness, value ranges and time coverage.
2. Ten SQL and ten pandas practice problems solved (window functions, deduplication, cohort retention, joins with NULLs).

**Mock:** 45-minute technical screen: 20 minutes SQL, 20 minutes pandas on a shared notebook, 5 minutes statistics questions.

**End of week check:** you can profile a new dataset in under 30 minutes and explain every check you ran.

### Week 2 — ML fundamentals, validation and leakage

**Goals:** pass the ML fundamentals / case study round.

- [ ] Read [06-ai-llm section I (classic ML)](../../topics/06-ai-llm/README.md#i-classic-ml-sometimes-asked), with [answers](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md).
- [ ] Work through [Feature engineering from customer data](./README.md#feature-engineering-from-customer-data-1): [features from event data](./README.md#how-do-you-engineer-features-from-a-customers-transaction-or-event-data), [leakage in time-based features](./README.md#how-do-you-avoid-leakage-when-building-time-based-features), [high-cardinality categoricals](./README.md#how-do-you-handle-high-cardinality-categorical-variables-such-as-product-or-store-ids) and [validation on time-ordered data](./README.md#how-do-you-validate-a-model-on-time-ordered-or-grouped-customer-data).
- [ ] Read [spotting target leakage](./README.md#how-do-you-spot-target-leakage-in-a-customers-dataset).

**Deliverables:**
1. A leakage-safe feature pipeline on a public event or transaction dataset, with an explicit cutoff date and a time-based backtest.
2. A one-page comparison of a simple baseline vs a gradient-boosted model on that dataset, with the right metric for the problem.

**Mock:** 45-minute ML case mock: a peer describes a dataset with a hidden leak; you find it, choose a validation scheme and pick a metric.

**End of week check:** you can find leakage in a described dataset and justify your split and metric.

### Week 3 — The take-home: framing, modelling and presenting

**Goals:** deliver a timed take-home on a messy dataset and present it to a mixed panel.

- [ ] Work through [Take-home on a messy customer dataset](./README.md#take-home-on-a-messy-customer-dataset-1), especially [the approach](./README.md#you-get-a-take-home-with-a-messy-customer-csv-and-a-vague-business-question-walk-us-through-your-approach) and [presenting to a mixed panel](./README.md#how-do-you-present-take-home-results-to-a-mixed-technical-and-business-panel).
- [ ] Work through [Framing the problem with the customer](./README.md#framing-the-problem-with-the-customer-1): ["build a model to predict churn"](./README.md#a-customer-asks-you-to-build-a-model-to-predict-churn-what-do-you-ask-before-touching-the-data), [does it need ML at all](./README.md#how-do-you-decide-whether-a-customers-problem-needs-machine-learning-at-all), [choosing the metric with the business](./README.md#how-do-you-choose-the-evaluation-metric-together-with-a-business-stakeholder) and [estimating value before building](./README.md#how-do-you-estimate-the-business-value-of-a-model-before-building-it).
- [ ] Read [04-decomposition](../../topics/04-decomposition/README.md) sections [C (commercial)](../../topics/04-decomposition/README.md#c-commercial--enterprise) and [F (follow-ups)](../../topics/04-decomposition/README.md#f-typical-interviewer-follow-ups--push-backs), with [answers](../../topics/04-decomposition/ANSWERS.md).

**Deliverables:**
1. A timed practice take-home (four to six hours): a messy public dataset (for example a city open-data export), a decision-framed question, a clean notebook and a 10-slide presentation.
2. A one-page value estimate for that model: the decision it changes, cost of errors, expected benefit range.

**Mock:** 30-minute take-home presentation to two peers, one technical and one playing a business owner, followed by 15 minutes of questions on limits and next steps.

**End of week check:** you can deliver a take-home in six hours and present it in 15 minutes, leading with the decision, not the model.

### Week 4 — Explaining models and handling pushback

**Goals:** pass the customer role-play: explain predictions, thresholds and uncertainty, and handle sceptical experts.

- [ ] Work through [Explaining models to business users](./README.md#explaining-models-to-business-users-1): [why the model flagged this case](./README.md#explain-to-a-non-technical-operations-manager-why-the-model-flagged-this-case), [precision, recall and thresholds](./README.md#how-do-you-explain-precision-recall-and-the-choice-of-threshold-to-a-business-stakeholder), [the domain expert who disagrees](./README.md#the-customers-domain-expert-says-the-model-is-wrong-because-it-contradicts-their-experience-what-do-you-do), [forecast uncertainty](./README.md#how-do-you-communicate-forecast-uncertainty-to-executives) and [interpretable vs black-box](./README.md#when-would-you-choose-an-interpretable-model-over-a-more-accurate-black-box-model).
- [ ] Read [99% accuracy on 0.5% fraud](./README.md#the-customer-wants-99-accuracy-on-a-dataset-where-05-of-cases-are-fraud-how-do-you-respond).
- [ ] Read [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) sections [B (difficult situations)](../../topics/09-customer-scenarios/README.md#b-difficult-situations) and [C (communication)](../../topics/09-customer-scenarios/README.md#c-communication), with [answers](../../topics/09-customer-scenarios/ANSWERS.md).

**Deliverables:**
1. A one-page explainer of precision, recall and threshold choice built around a cost table, written for an operations manager.
2. A SHAP-based explanation of one prediction from your Week 3 model, written in three plain-English sentences.

**Mock:** 30-minute role-play: a peer plays a domain expert who says the model is wrong, then an executive who asks for a guarantee the model will never be wrong.

**End of week check:** you can explain a threshold choice in business terms and turn a sceptical expert into a collaborator.

### Week 5 — Proving value: experiments and pilots

**Goals:** design a credible pilot and measure impact honestly.

- [ ] Work through [Experiment design and proving value](./README.md#experiment-design-and-proving-value-1): [designing the experiment](./README.md#design-an-experiment-to-measure-whether-the-model-improves-the-customers-business-outcome), [no randomisation possible](./README.md#the-customer-cannot-run-a-randomised-experiment-how-else-do-you-measure-impact), [sample size and duration](./README.md#how-do-you-estimate-sample-size-and-test-duration-for-a-pilot) and [the operations team chose the cases](./README.md#the-pilot-shows-a-lift-but-the-operations-team-chose-which-cases-to-use-the-model-on-what-do-you-do).
- [ ] Review statistics for experiments: power, minimum detectable effect, randomisation units, difference-in-differences, selection bias.
- [ ] Read [09 section E (business acumen)](../../topics/09-customer-scenarios/README.md#e-business-acumen) for ROI conversations.

**Deliverables:**
1. A two-page pilot design for your Week 3 model: hypothesis, unit, sample size, duration, guardrail metrics, and a fallback design without randomisation.
2. A small simulation (notebook) showing how selection bias inflates an apparent lift.

**Mock:** 45-minute experiment design case: a peer gives a customer scenario and pushes on "we cannot randomise" and "we need results in two weeks".

**End of week check:** you can size a pilot on paper and explain why a biased pilot's lift should not be trusted.

### Week 6 — Deployment, MLOps handover, LLMs and full loop

**Goals:** pass the ML system design round and rehearse the full loop.

- [ ] Work through [Deployment, monitoring and MLOps handover](./README.md#deployment-monitoring-and-mlops-handover-1): [no data egress](./README.md#the-customers-data-cannot-leave-their-environment-how-do-you-build-and-deploy-the-model), [monitoring](./README.md#how-do-you-monitor-a-model-running-in-a-customers-environment), [performance dropped after three months](./README.md#model-performance-dropped-three-months-after-deployment-at-the-customer-how-do-you-investigate), [batch vs real-time](./README.md#batch-scoring-or-real-time-serving-how-do-you-choose-for-a-customer), [retraining](./README.md#how-do-you-decide-when-and-how-to-retrain-a-customers-model) and [handover](./README.md#what-do-you-hand-over-so-the-customers-team-can-run-the-model-without-you).
- [ ] Read [03-system-design section A](../../topics/03-system-design/README.md#a-applied--customer-context-design-prompts) and [08-cloud-devops-integration section C (cloud)](../../topics/08-cloud-devops-integration/README.md#c-cloud).
- [ ] Work through [LLMs in forward deployed data work](./README.md#llms-in-forward-deployed-data-work-1) and read [06-ai-llm section E (evaluation)](../../topics/06-ai-llm/README.md#e-evaluation-most-important).
- [ ] Work through [Role-specific behavioral](./README.md#role-specific-behavioral-1) and write four stories into [STORY-BANK.md](../../my-prep/STORY-BANK.md); tick off the [Prep checklist](./README.md#prep-checklist).

**Deliverables:**
1. An architecture sketch for deploying and monitoring a model inside a customer's cloud with no data egress, including retraining and rollback, plus a model card and a handover runbook.
2. An LLM vs classical comparison on one text classification task: same test set, accuracy and cost per 1,000 items.

**Mock:** full mock loop over two days: SQL/pandas (45 min), ML case (45 min), take-home presentation (30 min), ML system design (45 min), behavioral (45 min).

**End of week check:** you can design a deployment under customer constraints and you have done every round type at least twice.

---

## 2-week crash plan

About 2 hours a day.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | SQL | Window functions, dedup, cohorts | [07 A, B](../../topics/07-data-and-sql/README.md#a-sql-coding-prompts), [07 answers](../../topics/07-data-and-sql/ANSWERS.md) |
| 2 | pandas and profiling | First-hour checks script | [07 F](../../topics/07-data-and-sql/README.md#f-pandas--python-data-wrangling), [first-hour checks](./README.md#what-data-quality-checks-do-you-run-in-the-first-hour-with-a-new-customer-dataset) |
| 3 | ML fundamentals | Metrics, imbalance, model choice | [06 I](../../topics/06-ai-llm/README.md#i-classic-ml-sometimes-asked) |
| 4 | Leakage and validation | Time-based features and splits | [Feature engineering](./README.md#feature-engineering-from-customer-data-1) |
| 5–6 | Take-home | Timed take-home on a public messy dataset plus slides | [Take-home](./README.md#take-home-on-a-messy-customer-dataset-1) |
| 7 | Mock 1 | Present the take-home to a peer | [Presenting results](./README.md#how-do-you-present-take-home-results-to-a-mixed-technical-and-business-panel) |
| 8 | Framing | Churn questions, need for ML, metric, value | [Framing](./README.md#framing-the-problem-with-the-customer-1) |
| 9 | Explaining | Thresholds with a cost table; SHAP in plain English | [Explaining models](./README.md#explaining-models-to-business-users-1) |
| 10 | Experiments | Pilot design, no-randomisation fallback, sample size | [Experiments](./README.md#experiment-design-and-proving-value-1) |
| 11 | Deployment | No-egress deployment, monitoring, retraining, handover | [Deployment](./README.md#deployment-monitoring-and-mlops-handover-1) |
| 12 | Mock 2 | Customer role-play plus ML system design | [09 B](../../topics/09-customer-scenarios/README.md#b-difficult-situations) |
| 13 | LLMs and stories | LLM vs classical; four STAR stories | [LLMs](./README.md#llms-in-forward-deployed-data-work-1), [STORY-BANK.md](../../my-prep/STORY-BANK.md) |
| 14 | Light review | Checklist, cheat sheet | [Prep checklist](./README.md#prep-checklist) |

---

## Daily routine

| Minutes | Activity |
|---|---|
| 0–20 | One SQL or pandas problem, timed |
| 20–50 | Main study item for the week (ML concepts, framing, explaining, experiments, deployment) |
| 50–75 | Hands-on: notebook work on the week's deliverable |
| 75–85 | Explain one concept aloud to an imaginary non-technical manager (threshold, leakage, uncertainty) |
| 85–90 | Log what to fix tomorrow |

---

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | SQL plus pandas screen (45 min) | Peer data scientist; or SQL practice platforms |
| 2 | ML case with hidden leakage (45 min) | Peer with ML experience |
| 3 | Take-home presentation (45 min) | One technical and one non-technical peer |
| 4 | Domain-expert and executive role-play (30 min) | Peer playing the customer |
| 5 | Experiment design case (45 min) | Peer with analytics or experimentation background |
| 6 | Full loop (five rounds over two days) | Mix of peers; optionally a coached mock on interviewing.io or Exponent |

Options: Pramp and interviewing.io cover SQL and coding screens; Exponent has data science and ML interview content. Present your take-home to a non-technical friend at least once.

---

## Fast track for the repo owner's profile

- **Skim:** SQL, data quality and reconciliation. The bidirectional legacy MySQL to new Postgres bridge across six product lines, with idempotency guards, ordering constraints and reconciliation, covers most of Week 1 and the "two systems disagree" question.
- **Skim:** deployment constraints and data protection. Row-level access control and the India DPDP Act constraints from the speech-to-text evaluation map to "data cannot leave the customer's environment".
- **Biggest gap 1 — statistics and ML:** classical ML, validation and leakage are not evidenced in your profile. Weeks 2 and 5 are your highest-value weeks; consider adding a third week on ML basics.
- **Biggest gap 2 — Python and pandas:** your evidenced stack is PHP and SQL. Confirm your Python level; if it is low, start pandas practice on day one.
- **Biggest gap 3 — production LLM and evaluation experience:** do the Week 6 LLM vs classical comparison and read [06-ai-llm section E](../../topics/06-ai-llm/README.md#e-evaluation-most-important) carefully.
- **Biggest gap 4 — presenting to external customers:** practise the take-home presentation and role-plays with people outside engineering.
- **Round mapping — value estimation and framing:** your contribution to the speech-to-text provider evaluation and its buy-vs-build cost model maps to "estimate the business value before building".
- **Round mapping — data engineering and pipelines:** the migration bridge, CRM/ERP integrations and AWS S3/SQS work map to pipeline and deployment design questions.
- **Round mapping — business metrics:** invoicing and payments tied to a cash-collection metric show you can tie technical work to a business outcome, the core of "proving value". Keep these stories in [STORY-BANK.md](../../my-prep/STORY-BANK.md).

---

## Resources

- *Data Science for Business* by Foster Provost and Tom Fawcett — framing problems, expected value and evaluation.
- *Hands-On Machine Learning with Scikit-Learn, Keras and TensorFlow* by Aurélien Géron — practical ML fundamentals.
- *Trustworthy Online Controlled Experiments* by Ron Kohavi, Diane Tang and Ya Xu — experiment design and pitfalls.
- *Interpretable Machine Learning* by Christoph Molnar — free online; SHAP and model explanation.
- *Designing Machine Learning Systems* by Chip Huyen — deployment, monitoring, drift and retraining.
- *Causal Inference: The Mixtape* by Scott Cunningham — difference-in-differences and other non-randomised methods.
- scikit-learn documentation (https://scikit-learn.org/stable/) and pandas documentation (https://pandas.pydata.org/docs/).
