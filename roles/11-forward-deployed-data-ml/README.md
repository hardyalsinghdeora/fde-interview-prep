# Forward Deployed Data Scientist / ML Engineer

Data scientists and ML engineers who work inside customer environments: they frame the business problem, wrangle the customer's real data, build and validate models, prove value with the customer, and hand over something the customer can run.

## The role

**Day to day**

- Discovery with business and technical stakeholders: what decision will the model improve, and how will we know?
- Get access to and understand the customer's data: messy exports, data warehouses, legacy systems, spreadsheets.
- Profile and clean data; engineer features; build baselines and models; validate them honestly.
- Explain results, trade-offs and uncertainty to non-technical users and executives.
- Design pilots and experiments to measure business impact.
- Deploy models into the customer's environment (their cloud, on-prem, or the vendor's platform) and set up monitoring.
- Hand over to the customer's data or IT team: code, documentation, runbooks, training.
- Increasingly, build LLM-based pipelines (extraction, classification, RAG) alongside classical models.

**Common titles and aliases:** Forward Deployed Data Scientist, Forward Deployed ML Engineer, Customer-Facing Data Scientist, Field Data Scientist, Applied Data Scientist, Customer Data Engineer, Data Science Solutions Architect, ML Solutions Architect, Resident Solutions Architect (data/ML), Applied AI Engineer, Data Science Consultant. Titles vary more than the work does.

**Organisations with customer-facing data/ML roles (examples; titles differ):** ML and data platform vendors such as Databricks, Snowflake, DataRobot, Dataiku, H2O.ai and Domino Data Lab; enterprise AI companies such as Palantir and C3 AI; AI labs and AI-native startups with applied AI teams; and consulting analytics groups such as QuantumBlack (McKinsey) and BCG X. Check each job description: some roles are mostly pre-sales, others are long-term delivery.

**How it differs from FDE**

| | Forward Deployed Data / ML | Forward Deployed Engineer (general) |
|---|---|---|
| Core output | Models, analyses, data pipelines, experiments | Integrations, applications, platform deployments |
| Key skills | Statistics, ML, feature engineering, evaluation | Software engineering, systems, integration |
| Main risk | Model does not create business value; data cannot support it | Integration fails; system does not fit the workflow |
| Proof of success | Measured lift in a business metric | Working deployment, adoption |
| Customer conversations | Metrics, thresholds, uncertainty, trust in predictions | Requirements, architecture, timelines |

The two overlap heavily, and many FDE teams include people with a data science background.

**Who it suits:** data scientists who want their models used, not just presented; people comfortable with messy data and ambiguity, who enjoy explaining statistics in plain language and can write production-quality code.

**Skills mix**

| Skill | Weight |
|---|---|
| Coding (Python, SQL, pipelines, deployment) | High |
| Statistics and ML | High |
| Customer communication | High |
| Sales / commercial | Low to Medium (higher in pre-sales roles) |
| Product feedback | Medium |
| Data engineering | Medium to High |

**Career paths:** senior / staff applied scientist or ML engineer, ML solutions architect, FDE lead or engagement manager, data science manager, ML platform engineer, product manager for ML products, head of data at a customer company, founder.

## Typical interview loop

Typical/reported shape; varies by company.

| Round | What it tests | How to prepare |
|---|---|---|
| Recruiter screen | Motivation, travel, customer-facing fit | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) |
| Technical screen | SQL, Python/pandas, statistics basics | [07-data-and-sql](../../topics/07-data-and-sql/README.md) (A, B, F) |
| Take-home on a messy dataset | Cleaning, framing, modelling, honesty about limits | Questions below |
| Take-home presentation | Explaining results to a mixed technical/business panel | Questions below |
| ML fundamentals / case study | Metrics, validation, leakage, model choice | [06-ai-llm section I](../../topics/06-ai-llm/README.md) and questions below |
| ML system design for a customer | Deployment, monitoring, retraining under customer constraints | [03-system-design](../../topics/03-system-design/README.md), [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) |
| Customer role-play | Problem framing, explaining a model, handling pushback | [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) |
| Coding (some companies) | Practical Python or DS&A | [02-coding](../../topics/02-coding/README.md) |
| Behavioral | Ownership, ambiguity, customer impact | [10-behavioral](../../topics/10-behavioral/README.md) |

## Common rounds — use the shared sections

- [07-data-and-sql](../../topics/07-data-and-sql/README.md) — highest priority: SQL prompts (A), pipelines and data quality (D), pandas wrangling (F). Entity resolution and PII handling come up with customer data.
- [06-ai-llm](../../topics/06-ai-llm/README.md) — section I (classic ML) for fundamentals; E (evaluation) and C (RAG) if the role includes LLM work.
- [04-decomposition](../../topics/04-decomposition/README.md) — framing a vague business problem into an analytics plan.
- [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) — "the data is worse than expected", "the customer wants a guarantee the AI will never be wrong", explaining technical ideas to executives.
- [03-system-design](../../topics/03-system-design/README.md) and [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) — deploying into a customer's cloud or on-prem environment.
- [10-behavioral](../../topics/10-behavioral/README.md) — customer focus and failure stories.

## Role-specific questions

### Take-home on a messy customer dataset

- [ ] You get a take-home with a messy customer CSV and a vague business question. Walk us through your approach.
- [ ] What data quality checks do you run in the first hour with a new customer dataset?
- [ ] How do you spot target leakage in a customer's dataset?
- [ ] How do you handle missing values, and when does missingness itself carry signal?
- [ ] Two customer systems disagree about the same records. How do you decide which to trust for modelling?
- [ ] How do you present take-home results to a mixed technical and business panel?

### Framing the problem with the customer

- [ ] A customer asks you to "build a model to predict churn." What do you ask before touching the data?
- [ ] How do you decide whether a customer's problem needs machine learning at all?
- [ ] How do you choose the evaluation metric together with a business stakeholder?
- [ ] How do you estimate the business value of a model before building it?
- [ ] The customer wants 99% accuracy on a dataset where 0.5% of cases are fraud. How do you respond?

### Feature engineering from customer data

- [ ] How do you engineer features from a customer's transaction or event data?
- [ ] How do you avoid leakage when building time-based features?
- [ ] How do you handle high-cardinality categorical variables such as product or store IDs?
- [ ] How do you validate a model on time-ordered or grouped customer data?

### Explaining models to business users

- [ ] Explain to a non-technical operations manager why the model flagged this case.
- [ ] How do you explain precision, recall and the choice of threshold to a business stakeholder?
- [ ] The customer's domain expert says the model is wrong because it contradicts their experience. What do you do?
- [ ] How do you communicate forecast uncertainty to executives?
- [ ] When would you choose an interpretable model over a more accurate black-box model?

### Experiment design and proving value

- [ ] Design an experiment to measure whether the model improves the customer's business outcome.
- [ ] The customer cannot run a randomised experiment. How else do you measure impact?
- [ ] How do you estimate sample size and test duration for a pilot?
- [ ] The pilot shows a lift, but the operations team chose which cases to use the model on. What do you do?

### Deployment, monitoring and MLOps handover

- [ ] The customer's data cannot leave their environment. How do you build and deploy the model?
- [ ] How do you monitor a model running in a customer's environment?
- [ ] Model performance dropped three months after deployment at the customer. How do you investigate?
- [ ] Batch scoring or real-time serving: how do you choose for a customer?
- [ ] How do you decide when and how to retrain a customer's model?
- [ ] What do you hand over so the customer's team can run the model without you?

### LLMs in forward deployed data work

- [ ] When would you use an LLM instead of a classical model for a customer problem such as classifying support tickets?
- [ ] How do you evaluate an LLM-based extraction pipeline on a customer's documents?

### Role-specific behavioral

- [ ] Tell me about a time a model you built did not deliver the expected business value.
- [ ] Tell me about a time you had to tell a customer their data could not support what they wanted.
- [ ] Why a forward deployed data or ML role rather than an internal data science role?

## Model answers

### Take-home on a messy customer dataset

#### You get a take-home with a messy customer CSV and a vague business question. Walk us through your approach.

Graders usually care more about judgment and communication than model score.

1. **Restate the question as a decision.** "Which customers should the retention team call each week?" Write down assumptions you had to make.
2. **Profile the data:** shape, types, missingness, duplicates, ranges, date coverage, target distribution. Note every issue found.
3. **Clean with a log:** each fix, why, and how many rows it affected. Keep raw data untouched.
4. **Check for leakage** and for what would actually be known at prediction time.
5. **Baseline first:** a simple rule or logistic regression; then one stronger model (for example gradient boosting). Do not try ten models.
6. **Validate properly:** time-based split if the data is temporal; a metric linked to the decision.
7. **Translate results:** "Calling the top 10% captures 45% of churners, vs. 10% at random."
8. **State limits and next steps:** data you would want, questions for the customer, how you would deploy and monitor.

Deliver a reproducible notebook or script (fixed seeds, requirements file) plus a short summary at the top. Time-box: a clear, honest, simple solution beats an over-engineered one.

#### What data quality checks do you run in the first hour with a new customer dataset?

```python
df.shape, df.dtypes
df.isna().mean().sort_values(ascending=False)    # missingness per column
df.duplicated().sum(); df[key].duplicated().sum() # full and key duplicates
df.describe(include="all")                        # ranges, odd min/max
df[date_col].min(), df[date_col].max()            # coverage
df.groupby(df[date_col].dt.to_period("M")).size() # gaps, sudden volume changes
df[target].value_counts(normalize=True)           # balance
```

What to look for:

- **Types:** numbers stored as strings, mixed date formats, locale decimals ("1.234,50").
- **Sentinel values:** -1, 0, 999, "N/A", "1900-01-01" meaning missing.
- **Impossible values:** negative ages, future dates, totals that do not add up.
- **Key integrity:** IDs unique where they should be; joins that multiply rows.
- **Time coverage:** missing months, a sudden change in volume (system migration?).
- **Category inconsistency:** "NY", "New York", "new york".
- **Target sanity:** plausible rate; does it match what the customer told you?

Then **ask the customer** about every surprise. Many "data errors" are business rules nobody wrote down.

#### How do you spot target leakage in a customer's dataset?

Leakage = a feature contains information that would not be available at prediction time, often derived from the outcome itself.

Warning signs:

- **Results too good:** AUC 0.99 on a hard business problem is a red flag.
- **One feature dominates importance,** especially with names like `status`, `closed_date`, `resolution_code`, `refund_amount`, `last_contact_reason`.
- **Timestamps after the prediction point:** fields updated when the outcome happened (for example `account_closed_flag` for churn).
- **Rows duplicated across train and test** (same customer in both).
- **Preprocessing on the full dataset** before splitting (scaling, target encoding, imputation with global statistics).

How to check:

1. For each feature, ask the customer **when and how it is populated** in their system.
2. Rebuild features as of a fixed **prediction date** using only data before it.
3. Train without the suspicious feature and compare performance.
4. Validate on a later time period.

Explain to the customer why a lower but honest score is the useful one.

#### How do you handle missing values, and when does missingness itself carry signal?

First understand **why** values are missing:

- **Missing completely at random:** a logging glitch. Simple imputation is usually fine.
- **Missing at random (depends on other observed fields):** model-based imputation can help.
- **Missing not at random:** missingness depends on the value itself or the outcome, for example income left blank by people with low income, or "last purchase date" empty because the customer never bought. Here missingness is signal.

Practical options:

- Add an **indicator column** (`x_missing`) plus a simple imputation (median, or a constant). This often works well with tree models.
- Use models that handle missing values natively (for example many gradient boosting libraries).
- Drop a column only if it is mostly missing and not informative; drop rows only if few and random.
- Compute imputation values on the **training data only**.

Also check that missingness is stable over time: if a field became mandatory last year, the model may learn "missing = old record", which will not hold in production. Ask the customer how each field is collected.

#### Two customer systems disagree about the same records. How do you decide which to trust for modelling?

Example: CRM and billing system disagree on customer start dates and status.

1. **Quantify the disagreement:** what percent of records, which fields, any pattern (a region, a time period, a migration date).
2. **Understand each system's role:** which is the system of record for which field? Billing is usually authoritative for money and dates of payment; CRM for contact and sales activity.
3. **Ask the data owners** about known issues: manual entry, sync jobs, migrations, timezone handling.
4. **Check against ground truth** for a sample: invoices, contracts, or the business user's knowledge.
5. **Define rules per field** (source precedence, most recent update wins, or flag conflicts) and document them.
6. **Use what will be available at prediction time.** If production scoring will read from CRM, train on CRM data, even if billing is cleaner, or the model will see different inputs in production.
7. **Add a conflict flag** as a feature if it is informative, and report data-quality issues to the customer as a separate deliverable.

For entity matching across systems, see entity resolution in [07-data-and-sql](../../topics/07-data-and-sql/README.md).

#### How do you present take-home results to a mixed technical and business panel?

Structure for 15–20 minutes:

1. **The answer first (1 slide):** the recommendation and expected impact in business terms.
2. **The problem as you framed it,** with assumptions.
3. **Data:** what it contained, key quality issues and how you handled them. Panels value honesty here.
4. **Approach:** baseline vs. final model, validation method, why this metric.
5. **Results in business terms:** a lift or gains chart, cost/benefit at the chosen threshold, examples of predictions.
6. **Limitations and risks:** leakage checks, bias concerns, data gaps.
7. **Next steps:** deployment, monitoring, pilot design, what data you would request.

Tips: one message per slide; technical details in an appendix for the engineers' questions; prepare for "why not model X?" and "how would this work in production?". Speak to the business people first, then let technical questions go deeper.

### Framing the problem with the customer

#### A customer asks you to "build a model to predict churn." What do you ask before touching the data?

- **Decision:** what will you do differently with the prediction? Call customers, offer discounts, change onboarding? If nothing changes, the model has no value.
- **Definition:** what counts as churn? Cancellation, non-renewal, no purchase in 90 days? For contract vs. subscription vs. usage-based businesses it differs.
- **Timing:** how far ahead do you need to know, and how often will predictions be used (weekly list, real-time)?
- **Capacity:** how many customers can the team act on per week? This sets the precision needed at the top of the list.
- **Success metric:** retained revenue, churn rate in the treated group vs. control.
- **Current process:** how are at-risk customers found today? That is the baseline to beat.
- **Data:** what history exists, where, how far back, and who owns it.
- **Constraints:** privacy, which fields may be used, where the model must run.
- **Stakeholders:** who will use the output, and who must trust it.

Better still, predict who will respond to an intervention (uplift), not just who will churn; mention this as a possible next step.

#### How do you decide whether a customer's problem needs machine learning at all?

ML is worth it when all of these are true:

- There is a **repeated decision** at scale, made often enough that automation or ranking helps.
- **Patterns are too complex** for simple rules, and rules have been tried or are hard to maintain.
- **Historical data with outcomes exists** (labels) or can be created affordably.
- **Errors are tolerable** or can be caught by a human review step.
- Someone will **act on the output** and the workflow can change.

Try simpler options first: a SQL report, a business rule, a dashboard, a heuristic score, or fixing a data process. Build a rule-based baseline anyway; if it captures most of the value, ship it. Say clearly to the customer when ML is not the answer. This builds trust, and interviewers want to see that you prefer solving the problem over using a technique.

#### How do you choose the evaluation metric together with a business stakeholder?

Start from the decision and the costs of errors, then map them to a metric.

1. **Ask what a mistake costs.** A missed fraud case costs the transaction amount; a false alarm costs an analyst's 10 minutes and maybe customer friction.
2. **Ask about capacity.** If the team reviews 200 cases a day, the relevant metric is **precision at 200** (or recall at that volume), not overall accuracy.
3. **Choose:**
   - Ranking for limited capacity: precision@k, recall@k, lift.
   - Rare-event detection: precision-recall curve / average precision rather than accuracy.
   - Forecasting: MAPE is easy to explain but breaks near zero; WAPE or MAE in units the business uses.
   - Expected value: sum of (benefit of true positives − cost of false positives).
4. **Agree a business KPI** too (revenue retained, losses prevented) that the pilot will measure.
5. **Write it down** before modelling to avoid moving goalposts.

Keep a technical metric for model development (for example AUC) but report the business metric.

#### How do you estimate the business value of a model before building it?

Do a back-of-envelope calculation with the customer's numbers:

```
Value per period = (cases acted on) × (hit rate) × (value per correct action)
                   − (cases acted on) × (cost per action)
```

Example (illustrative numbers): retention team calls 500 customers a month. Today 8% of those called would have churned; a model might raise that to 25%. If a call saves 30% of true churners and each saved customer is worth 1,200 per year:

- Today: 500 × 0.08 × 0.30 × 1,200 = 14,400 of annual customer value saved per monthly batch.
- With model: 500 × 0.25 × 0.30 × 1,200 = 45,000 per monthly batch.
- Uplift ≈ 30,600 per batch, roughly 367,000 over 12 monthly batches.

Then compare with cost: build, data work, infrastructure and maintenance. Use ranges (pessimistic, likely, optimistic), check the assumptions with the customer, and test the key assumption (hit rate) early with a quick baseline on historical data. If the value is small even in the optimistic case, say so before starting.

#### The customer wants 99% accuracy on a dataset where 0.5% of cases are fraud. How do you respond?

Explain that accuracy is the wrong target, with their own numbers:

"A model that says 'not fraud' for every transaction is 99.5% accurate and catches zero fraud. So accuracy will look great while being useless."

Then reframe:

1. **What they really want:** catch as much fraud as possible while keeping false alarms manageable for the review team.
2. **Propose metrics:** recall (share of fraud caught) and precision (share of alerts that are real fraud), or precision at the team's daily review capacity, plus value of fraud prevented.
3. **Show a trade-off table:** "At this threshold, the team reviews 300 alerts a day, 1 in 5 is real fraud, and we catch 70% of fraud value."
4. **Technical handling of imbalance:** class weights, appropriate metrics, stratified or time-based validation; resampling only on training data. Calibrated probabilities help set thresholds.

Respect the intent behind "99%": they want a reliable system. Agree a target on the right metric instead.

### Feature engineering from customer data

#### How do you engineer features from a customer's transaction or event data?

Build features per entity (customer, account, device) **as of a prediction date**, from events before that date.

Common families:

- **Recency, frequency, monetary (RFM):** days since last purchase, number of orders in last 30/90/365 days, total and average spend.
- **Trends:** spend in last 30 days vs. previous 30; slope of weekly activity. Changes often predict better than levels.
- **Windows at several scales:** 7, 30, 90 days.
- **Diversity and behaviour:** number of distinct products, categories, channels; share of activity on weekends; time between events.
- **Lifecycle:** tenure, plan type, days to renewal, onboarding milestones completed.
- **Service signals:** support tickets, complaints, failed payments, returns.
- **Ratios:** discounts / spend, refunds / orders.
- **Domain features from experts:** ask the customer's staff what they look at; their heuristics often become the strongest features.

In SQL, window functions and date filters do most of this (see [07-data-and-sql](../../topics/07-data-and-sql/README.md)). Keep the feature code in one function or SQL model used for both training and scoring, to avoid training/serving skew.

#### How do you avoid leakage when building time-based features?

- **Fix a cutoff (prediction) time** per training example and compute features using only events strictly before it; the label comes from a window after it.

```
|---- feature window ----| cutoff |---- label window (e.g. next 60 days) ----|
```

- **Point-in-time joins:** when joining slowly changing data (plan, address, status), use the value valid at the cutoff, not today's value. Many customer tables only store the current value, which leaks the future; ask whether history or audit tables exist.
- **Watch late-arriving data:** if a transaction is recorded three days late in production, simulate that delay in training.
- **Time-based validation:** train on earlier cutoffs, test on later ones.
- **Fit preprocessing on training data only** (encoders, scalers, imputers), inside a pipeline.
- **Group splits:** the same customer should not be in both training and test when that would let the model memorise them.

Write a short test: for a sample of rows, confirm every source timestamp is before the cutoff.

#### How do you handle high-cardinality categorical variables such as product or store IDs?

Options, with trade-offs:

- **Frequency or count encoding:** replace category with how common it is. Simple and robust.
- **Target encoding:** replace with the mean target for that category, with smoothing toward the global mean and computed out-of-fold to avoid leakage. Powerful but easy to leak.
- **Group rare categories** into "other" below a minimum count.
- **Use the hierarchy:** store → region → country; product → category. Higher levels generalise better and handle new IDs.
- **Describe the entity instead of the ID:** store size, location type, product price band. This also handles new stores and products at scoring time.
- **Native categorical support** in some gradient boosting libraries (for example LightGBM, CatBoost).
- **Embeddings** for very large cardinality with lots of data, typically in neural models.
- **One-hot** only for low cardinality.

Always ask: what happens when a new store or product appears in production? The encoding must handle unseen values.

#### How do you validate a model on time-ordered or grouped customer data?

Random K-fold cross-validation overestimates performance on this kind of data.

- **Time-ordered data:** train on the past, test on the future. Use a rolling or expanding window (for example scikit-learn `TimeSeriesSplit`), with a **gap** between training and test periods equal to the label window, so labels do not overlap.
- **Grouped data** (several rows per customer, store or patient): use group-based splits (`GroupKFold`) so a group appears in only one fold.
- **Both:** split by time and check that new groups also appear in the test set, if production will see new customers.
- **Hold out the most recent period** as a final test that you look at once.
- **Check stability:** performance per time period and per segment (region, product line), not just one average.
- **Match production:** if the customer will retrain monthly and score weekly, simulate exactly that schedule in a backtest.

Report the backtest results by period so the customer sees how performance varies, not one optimistic number.

### Explaining models to business users

#### Explain to a non-technical operations manager why the model flagged this case.

Use a local explanation (for example SHAP values) but translate it into the manager's language and keep it to the top few reasons.

Example: "The model gave claim #4812 a high risk score, 0.82, which puts it in the top 3% of claims this week. The three biggest reasons are:

1. The repair amount is about four times the typical amount for this type of damage.
2. The claim was filed two days after the policy started.
3. The same repair shop appears on several other high-risk claims this month.

Things that made it look less risky: the customer has a long, clean history.

This doesn't mean it is fraud; it means it is worth a reviewer's time. Last quarter, about 1 in 4 claims with scores this high turned out to need adjustment."

Principles: give the score meaning (rank, what happens next), list reasons in business terms, include factors on both sides, state what the model does not know, and remind them the person makes the decision. Do not imply that feature contributions are causes.

#### How do you explain precision, recall and the choice of threshold to a business stakeholder?

Use their workflow and numbers, not definitions.

"Imagine the model gives every transaction a score, and we choose a cut-off. Everything above it goes to your review team.

- **Precision** answers: of the cases we send your team, how many are real problems? Low precision means your team wastes time.
- **Recall** answers: of all the real problems, how many did we catch? Low recall means losses slip through.

Moving the cut-off trades one for the other."

Then show a small table:

| Cut-off | Alerts per day | Real fraud in alerts | Fraud caught |
|---|---|---|---|
| High | 100 | 40% | 50% |
| Medium | 300 | 20% | 75% |
| Low | 800 | 9% | 90% |

"Your team can handle about 300 a day, so the medium cut-off catches three quarters of fraud value. Do you want to discuss staffing or keep the cut-off there?" Then let them choose; the threshold is a business decision.

#### The customer's domain expert says the model is wrong because it contradicts their experience. What do you do?

Treat it as information, not opposition. Experts are often right, and the model needs their trust to be used.

1. **Ask for specific cases:** "Can you show me two or three examples where it got it wrong?"
2. **Investigate together:** look at the data for those cases. Common findings:
   - A data issue (wrong field, stale value, a join error). Fix it.
   - Missing information the expert uses but the model cannot see. Consider adding it as a feature.
   - The model found a real pattern the expert did not know. Show the evidence (historical outcomes for similar cases).
   - A genuine model weakness for a segment. Report it and constrain usage there.
3. **Compare on outcomes:** in a blind test on historical cases, how do the expert and the model do? Or combine them.
4. **Involve the expert** in feature design and evaluation going forward.
5. **Design the workflow** so experts can override the model and their overrides are logged; that becomes training data.

Never dismiss the expert in front of their colleagues; adoption depends on them.

#### How do you communicate forecast uncertainty to executives?

- **Give a range, not a point:** "We expect 42,000 units next quarter, most likely between 38,000 and 46,000 (80% interval)."
- **Explain what the range means** in plain words: "In roughly 8 out of 10 quarters like this, the actual result falls inside this range."
- **Link to decisions:** "If you stock for 46,000, the risk of stock-outs is about 10%; at 42,000 it is about 50%. The cost of each is ..." Executives decide on risk, not on the point estimate.
- **Show past accuracy:** a chart of previous forecasts vs. actuals builds calibrated trust.
- **Name the main drivers of uncertainty:** promotions, a new competitor, a data change.
- **Use scenarios** for things the model cannot know (a recession, a big new customer).
- **Visualise** with a fan chart or shaded band.
- **Update regularly** and explain changes.

Avoid false precision such as "42,137 units". Check that intervals are well calibrated on historical data before presenting them.

#### When would you choose an interpretable model over a more accurate black-box model?

Choose an interpretable model (logistic regression, small decision tree, scorecards, generalised additive models) when:

- **Regulation requires explanations,** such as credit decisions, insurance pricing or some healthcare uses; reasons may need to be given to individuals.
- **Users must trust and act** on each prediction and will not adopt a black box.
- **The accuracy gap is small.** On many tabular business problems, a well-engineered simple model is close to gradient boosting. Measure the gap in business value, not only in AUC.
- **Data is small or noisy,** where complex models overfit.
- **Debugging and handover matter:** the customer's team must maintain it.
- **Fairness review** is needed and must be transparent.

Choose the more complex model when the extra accuracy has clear business value and post-hoc explanations (for example SHAP) plus monitoring are acceptable to stakeholders and regulators.

A practical approach: build both, show the customer the difference in value, and decide together. Often the simple model goes first and the complex one later once trust exists.

### Experiment design and proving value

#### Design an experiment to measure whether the model improves the customer's business outcome.

Example: churn model guiding a retention team.

1. **Hypothesis:** customers contacted based on model scores churn less than customers chosen by the current process.
2. **Unit of randomisation:** customer (or account manager / region if contacts interfere with each other).
3. **Design:** score everyone, then randomly assign eligible customers to:
   - Treatment: retention team contacts the top-scored customers.
   - Control: current process (or no contact for a holdout of high-scored customers, if ethical and agreed).
   A useful variant: randomise within the high-score group to measure the effect of the intervention itself.
4. **Primary metric:** churn rate (or retained revenue) at 60/90 days. Secondary: contact rate, cost per save. Guardrails: complaints, discount spend.
5. **Sample size and duration** from the baseline rate and the smallest effect worth detecting.
6. **Pre-register** the metric, analysis and duration with the customer; avoid stopping early when results look good.
7. **Check execution:** did the team actually follow the assignment? Log every contact.
8. **Analyse:** difference with a confidence interval; segment results only as exploration.

Agree in advance what result leads to rollout.

#### The customer cannot run a randomised experiment. How else do you measure impact?

Use the strongest available quasi-experimental method, and be clear about its assumptions:

- **Staggered rollout / difference-in-differences:** roll out to some regions or branches first; compare the change over time vs. regions not yet using it. Assumes parallel trends; check pre-period trends.
- **Synthetic control:** build a weighted combination of untreated units that matches the treated unit before launch.
- **Regression discontinuity:** if action is taken above a score cut-off, compare cases just above and just below it.
- **Matching / propensity scores:** compare treated cases with similar untreated ones. Only adjusts for observed differences.
- **Interrupted time series:** before/after with trend and seasonality modelled; weakest, because other things change at the same time.
- **Backtesting on historical data:** shows what the model would have flagged; measures predictive value, not the effect of acting on it.

Also look for small randomisation opportunities: a holdout of a few percent, or randomising the order of a work queue. Report results with ranges and caveats.

#### How do you estimate sample size and test duration for a pilot?

Inputs:

- **Baseline rate** of the metric (for example churn 10% per quarter).
- **Minimum detectable effect (MDE):** the smallest change worth acting on, agreed with the business (for example 2 percentage points).
- **Significance level** (usually 5%) and **power** (usually 80%).

A rough rule for two equal groups with a binary metric at 5% significance and 80% power:

```
n per group ≈ 16 × p × (1 − p) / MDE²
            ≈ 16 × 0.10 × 0.90 / 0.02² ≈ 3,600
```

Use a proper calculator or `statsmodels` (`NormalIndPower`) for the final number.

Duration = sample size ÷ eligible units per week, plus the time for the outcome to occur (a 90-day churn label means waiting 90 days after the last enrolment). Also run whole weekly or monthly cycles to cover seasonality.

If the required sample is unrealistic, discuss options: a larger MDE, a faster proxy metric (with evidence it predicts the real outcome), pooling across sites, or a decision based on a lower confidence level, stated explicitly.

#### The pilot shows a lift, but the operations team chose which cases to use the model on. What do you do?

The result is probably biased: selection bias. The team may have used the model on the easiest cases, or on cases they would have handled well anyway.

1. **Do not report the lift as proven.** Explain to the sponsor, neutrally, why the comparison is unreliable.
2. **Understand why they chose:** distrust of the model, workload, unclear instructions. That is useful feedback.
3. **Salvage what you can:** compare treated vs. untreated cases with similar scores and characteristics (matching), check whether chosen cases differed systematically, and present the result as indicative with clear caveats.
4. **Redesign the next phase:** random assignment done by the system (for example a randomised work queue), simple instructions, a logged override option with reasons.
5. **Involve the operations team** in the redesign so they understand why it matters.

Do not blame the team; the experiment design did not account for how they work.

### Deployment, monitoring and MLOps handover

#### The customer's data cannot leave their environment. How do you build and deploy the model?

Bring the work to the data:

- **Work inside their environment:** their cloud account (VPC), data platform workspace, or on-prem servers, using their identity and access controls.
- **Agree access** early: accounts, least-privilege roles, approved tools and package mirrors (many secure environments have no internet access, so prepare dependencies in advance, for example a vetted container image).
- **Use their infrastructure** for training and serving where possible (their Kubernetes, ML platform, or scheduler), so they can run it after you leave.
- **Develop with synthetic or masked data** outside, then run on real data inside if development tools are limited.
- **Package everything:** container image, pinned dependencies, infrastructure as code, config separate from code.
- **Logs and monitoring stay inside;** only share aggregated metrics if agreed.
- **Handle PII:** minimise fields, mask where possible, follow their data-protection policies.
- **If federated or on-device learning is proposed,** be cautious: it adds complexity and is rarely needed for a single customer.

Plan extra time; access approval is often the longest step. See [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) for on-prem and air-gapped deployment patterns.

#### How do you monitor a model running in a customer's environment?

Monitor four layers, and agree who receives alerts (customer team, you, or both):

1. **Service health:** job success, latency, errors, throughput, resource use.
2. **Input data quality:** schema changes, missing-value rates, value ranges, row counts, category values never seen before, data freshness.
3. **Drift:** distribution of key features and of predictions vs. a training baseline, using measures such as the population stability index (PSI) or KS tests. Prediction drift is a cheap early warning.
4. **Performance:** once labels arrive, the real metric (precision at k, MAE) per period and per segment. Labels are often delayed, so track proxies until then.
5. **Business outcome:** the KPI the model was meant to move.

Practical details: a dashboard the customer's team can read; alert thresholds tuned to avoid noise; log every prediction with model version and input snapshot for debugging; review meeting cadence (for example monthly). Distinguish **data drift** (inputs change) from **concept drift** (relationship between inputs and outcome changes), since they need different responses.

#### Model performance dropped three months after deployment at the customer. How do you investigate?

1. **Confirm the drop is real:** label delays, a change in how labels are recorded, small sample size in the latest period, a metric computed differently.
2. **Check the data pipeline first:** most drops are data problems. Schema changes, a source system upgrade, a field now always null, unit or currency changes, a join that started dropping rows. Compare feature distributions now vs. training.
3. **Check for training/serving skew:** are production features computed the same way as in training?
4. **Check the business:** new products, pricing changes, a marketing campaign, seasonality, a policy change in how cases are handled. **Feedback loops** also matter: if the model's alerts change behaviour, future data differs.
5. **Segment the drop:** one region, one product, new customers only?
6. **Act:**
   - Data bug: fix and backfill.
   - Data drift with stable relationships: retrain on recent data.
   - Concept drift: rework features or the model with domain experts.
7. **Communicate** with the customer: what happened, impact, fix, and the monitoring improvement that would catch it earlier.

#### Batch scoring or real-time serving: how do you choose for a customer?

Decide based on when the decision is made and when the input data is available.

| | Batch | Real-time |
|---|---|---|
| Use when | Decisions are periodic (daily call lists, weekly forecasts) | Decision happens during an event (payment authorisation, web session) |
| Inputs | Data warehouse tables | Request payload plus fresh features |
| Complexity | Low: scheduled job writing a table | Higher: API service, low-latency feature store, scaling, uptime |
| Cost | Low | Higher |
| Customer skills needed | SQL, scheduler | Service operations, on-call |

Default to batch unless the use case needs immediate decisions; it is easier for the customer to run and debug. Intermediate options: micro-batch (every few minutes), or precomputed scores looked up in real time. If real-time is needed, make sure features computed online match the training features, and agree latency and availability targets with the customer's engineering team.

#### How do you decide when and how to retrain a customer's model?

**When:**

- **Scheduled:** based on how fast the domain changes, measured by a backtest: train on older data, test on successive later periods and see how quickly performance decays. If it decays noticeably after three months, retrain monthly or quarterly.
- **Triggered:** when monitoring shows sustained drift or performance below an agreed threshold.
- **Event-driven:** after known business changes (new product line, policy change).

**How:**

1. Automated pipeline: pull data with a fixed window, build features with the same code, train, evaluate.
2. **Compare the new model with the current one** on the same recent holdout; promote only if it is better or equal on the agreed metric and passes checks (no segment much worse, sensible feature importance, calibration).
3. Register versions with metadata (data window, metrics, code version) in a model registry.
4. Support rollback.
5. Human approval for promotion at first; automate once the customer trusts the process.

Retraining too often adds risk and cost without benefit; let the backtest decide.

#### What do you hand over so the customer's team can run the model without you?

- **Code in their repository:** training, feature, scoring and evaluation code, with tests and pinned dependencies.
- **Reproducible environment:** container image or environment file; infrastructure as code for jobs and endpoints.
- **Pipelines and schedules** running on their platform, not on your laptop or your company's account.
- **Model card / documentation:** purpose, intended use and limits, training data and period, features, metrics by segment, known failure modes, fairness checks.
- **Runbook:** how to retrain, how to deploy a new version, how to roll back, what each alert means and what to do, who to contact.
- **Monitoring dashboard** and alerts routed to their team.
- **Data dictionary** and lineage for the features.
- **Training sessions:** walkthroughs, and a supervised retrain done by their team while you watch.
- **Handover plan:** shadowing period, an agreed support window, and success criteria for "they can run it".

Ask early who will own it. If nobody at the customer can, the project is at risk, and that should be raised before building.

### LLMs in forward deployed data work

#### When would you use an LLM instead of a classical model for a customer problem such as classifying support tickets?

**LLM (prompted, possibly with a few examples) is a good fit when:**
- There is little or no labelled data, and you need to start quickly.
- Categories change often or are many and descriptive.
- Inputs are long, varied free text, multilingual, or need reasoning.
- You also need extracted fields or summaries, not only a label.

**Classical model (for example TF-IDF + logistic regression, or a fine-tuned small transformer) is better when:**
- There are many labelled examples and stable categories.
- Volume is very high and cost or latency matter.
- Data must stay on modest on-prem hardware.
- Strict consistency and simple auditing are needed.

A practical path: start with an LLM to deliver value and to **create labels** (reviewed by humans), measure on a labelled test set, then consider distilling into a cheaper classifier if volume justifies it. Compare options on the same test set for accuracy, cost per 1,000 tickets and latency. See [06-ai-llm](../../topics/06-ai-llm/README.md) for prompting and fine-tuning details.

#### How do you evaluate an LLM-based extraction pipeline on a customer's documents?

1. **Build a gold set:** a representative sample of documents (by type, source, quality, language, including scans and edge cases), with fields labelled by the customer's experts. Start with 100–300 documents; check agreement between labellers.
2. **Define correctness per field:** exact match for IDs, normalised match for dates and amounts, semantic match for free text. Decide how to score missing vs. wrong values.
3. **Metrics:** field-level precision and recall, document-level "all critical fields correct", and the rate of "not found" when the value exists.
4. **Segment results** by document type and quality; averages hide failure modes.
5. **Check business impact:** which errors are costly (a wrong amount) vs. harmless.
6. **Add confidence and routing:** validation rules (totals add up, dates valid), and send low-confidence documents to human review; measure the share routed and accuracy of the rest.
7. **Regression testing:** rerun the gold set on every prompt or model change.
8. **Monitor in production:** sample reviews and human corrections feed the gold set.

See evaluation in [06-ai-llm](../../topics/06-ai-llm/README.md) for LLM-as-judge and eval tooling.

### Role-specific behavioral

#### Tell me about a time a model you built did not deliver the expected business value.

**What they are looking for:** you measure real outcomes rather than model metrics, you can diagnose why value did not appear (framing, adoption, data, workflow), and you learned something that changed how you work.

**STAR outline:**
- **Situation:** [the customer or business problem, the model, the expected value].
- **Task:** [your role and what success was supposed to be].
- **Action:** [how you discovered the gap, what you investigated (offline vs. online results, user adoption, process), what you changed].
- **Result:** [what happened next, numbers if possible, and the lesson you now apply, such as defining the decision and pilot design before modelling].

**Example (adapt to your own experience):** I built a demand-forecasting model that beat the existing method clearly in backtests, but store stock levels did not improve after rollout. Interviews with store managers showed they ordered in fixed case sizes and overrode forecasts they did not understand. I worked with them to show forecasts as recommended case quantities with a short reason, and added their override reasons as feedback. Adoption rose and stock-outs fell in the next pilot. Since then I map the full decision workflow before starting a model.

#### Tell me about a time you had to tell a customer their data could not support what they wanted.

**What they are looking for:** honesty early, evidence rather than opinion, and a constructive alternative that keeps the customer moving.

**STAR outline:**
- **Situation:** [what the customer wanted, the data they had].
- **Task:** [deliver a realistic assessment without losing the relationship].
- **Action:** [the analysis that showed the gap (too few labels, missing history, leakage, poor quality), how you presented it, the alternatives you offered (data collection plan, simpler first step, different target)].
- **Result:** [customer decision, what was delivered, how the relationship developed].

**Example (adapt to your own experience):** A customer wanted to predict equipment failures, but their maintenance logs held only a handful of recorded failures over two years and no consistent sensor history. I showed a short analysis: number of failure events, the data gaps, and what a model trained on it would and would not be able to tell us. I proposed two things: a rules-based alert from the existing sensor thresholds that maintenance staff had described, and a data collection plan so a predictive model would be possible in a year. They accepted both, and the rules alone reduced unplanned downtime noticeably in the first months.

#### Why a forward deployed data or ML role rather than an internal data science role?

Answer with genuine reasons and show you understand the trade-offs:

- **Impact you can see:** models used in real customer operations, measured in business outcomes.
- **Variety:** different industries, datasets and problems; you learn quickly.
- **Full lifecycle ownership:** framing, data, modelling, deployment, adoption, not only the modelling step.
- **Customer work energises you:** discovery, explaining, persuading.
- **Trade-offs you accept:** messy data, less time for methodological depth, travel, context switching, and the need to hand over rather than keep perfecting.

Example framing (adapt to your own experience): "In my current team, my best work was the project where I sat with the operations team, rebuilt the problem definition, and saw the model change how they worked. I want that every time: owning the problem end to end with the customer. A forward deployed role gives me that, plus exposure to many domains. I'm comfortable with imperfect data; I see making sense of it as part of the job."

## Prep checklist

- [ ] Do a timed practice take-home: pick a messy public dataset (for example a city open-data portal export), write a decision-framed question, and deliver a notebook plus a 10-slide presentation in four to six hours.
- [ ] Practise data profiling and cleaning in pandas and SQL until the first-hour checks are automatic ([07-data-and-sql](../../topics/07-data-and-sql/README.md) section F).
- [ ] Build a leakage-safe feature pipeline with a cutoff date and a time-based backtest for an event dataset.
- [ ] Prepare a one-page explanation of precision, recall and thresholds using a cost table, and rehearse it for a non-technical listener.
- [ ] Practise a SHAP-based explanation of a single prediction in plain English.
- [ ] Review experiment design: sample size estimation, randomisation units, difference-in-differences and selection bias.
- [ ] Sketch a deployment and monitoring architecture for a model inside a customer's cloud with no data egress, including retraining and rollback ([03-system-design](../../topics/03-system-design/README.md), [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md)).
- [ ] Write a model card and a handover runbook for one of your past projects.
- [ ] Prepare an LLM vs. classical model comparison for a text classification task, with cost and accuracy on the same test set ([06-ai-llm](../../topics/06-ai-llm/README.md)).
- [ ] Prepare four STAR stories: a model that created value, a model that did not, telling a customer "no", and winning over a skeptical domain expert.
