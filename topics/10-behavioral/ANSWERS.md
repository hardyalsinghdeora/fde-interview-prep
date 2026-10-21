# 10 — Behavioral Questions: Model Answers

Questions: [README.md](README.md)

**How to use these answers.** These are reference answers, not scripts. Every "Example" story is illustrative and must be replaced with a real story from your own experience — never present an invented story as your own. Use the STAR templates to structure your stories, then practise saying them out loud in about two minutes.

**Format for each "tell me about a time" question:** *What they're looking for* → *STAR template* (fill the brackets) → *Example (adapt to your own experience)* → *Pitfalls*. Non-STAR questions get a framework plus an example.

**Story bank tip:** most of these questions can be covered by 10–12 well-prepared stories. Tag each story with the themes it covers (ownership, ambiguity, customer, conflict, failure, technical depth, pressure, communication) and reuse.

---

## Ownership & impact

### Tell me about the project you're most proud of.

**What they're looking for:** What you value, the scope of your contribution, technical depth, and whether you can tie work to real outcomes. For FDE roles, bonus points if the pride comes from customer impact, not just clever code.

**STAR template:**
- **S:** [Team/company, customer, the problem and why it mattered]
- **T:** [Your specific role and the goal — "I owned X"]
- **A:** [3–4 key actions: technical decisions, people you aligned, obstacles you cleared]
- **R:** [Quantified outcome: time saved, revenue, adoption, reliability]
- **L:** [Why you are proud — what it says about how you work]

**Example (adapt to your own experience):**
"At a B2B analytics company, a logistics customer was reconciling shipment data across three systems by hand, which took their ops team about two days each week. I owned building the integration. I spent the first week on-site shadowing their analysts, then built an ingestion pipeline in Python and Airflow that pulled from their ERP, a carrier API and S3 drops, normalised them into Postgres, and flagged mismatches in a simple dashboard. The hardest part was entity matching — shipment IDs were formatted differently in each system — so I built a rules-plus-fuzzy-matching layer and had their analysts label 500 edge cases to tune it. Reconciliation went from two days to about 30 minutes, and the customer expanded the contract to two more regions. I'm proudest because I went from a vague complaint to something their team used every day."

**Pitfalls:**
- Picking a project where you can't explain your personal contribution ("we did...").
- All tech, no outcome — or all outcome, no technical depth.
- Choosing something so old or trivial it undersells you.

### Tell me about a time you owned something end to end.

**What they're looking for:** Whether you take full responsibility from problem definition through to adoption and maintenance — not just "my ticket is done." FDEs are expected to own outcomes, not tasks.

**STAR template:**
- **S:** [The need and who had it]
- **T:** [What "end to end" covered: discovery, design, build, deploy, support]
- **A:** [Actions at each stage, especially the unglamorous ones — docs, monitoring, training, follow-up]
- **R:** [Outcome and how it was sustained]
- **L:** [What owning the whole thing taught you]

**Example (adapt to your own experience):**
"Our sales team kept losing deals because prospects couldn't evaluate our API without a sandbox. Nobody owned it, so I proposed owning it. I interviewed four sales engineers to scope it, designed a sandbox that cloned a sanitised dataset per tenant, built it on Kubernetes with a nightly reset job, wrote the onboarding docs, and added Grafana alerts. After launch, I sat in on three customer demos to see where people got stuck and fixed two confusing error messages. Time-to-first-API-call for prospects went from about a week to under an hour, and sales attributed two closed deals to it in the first quarter. I also wrote a runbook and handed on-call to the platform team so it didn't depend on me."

**Pitfalls:**
- Stopping the story at "and then I shipped it" — show adoption, monitoring and handover.
- Implying you did everything alone and never asked for help; ownership includes pulling in the right people.

### Tell me about a time you went beyond your job description.

**What they're looking for:** Initiative, judgment about *which* extra work matters, and that you didn't drop your core responsibilities to do it.

**STAR template:**
- **S:** [Your role and its normal boundaries]
- **T:** [The gap you noticed that wasn't your job]
- **A:** [What you did, how you made time, who you looped in]
- **R:** [Impact on the team/customer/business]
- **L:** [How you decide when stepping outside your lane is worth it]

**Example (adapt to your own experience):**
"As a backend engineer, I noticed our support team was escalating about 40 tickets a week to engineering that were really data-lookup questions. That wasn't my area, but it was eating engineering time. I spent a couple of evenings and some slack time building a read-only internal admin tool on top of our existing API, with role-based access, and ran a 30-minute training for support. I checked with my manager first so it wasn't a surprise. Escalations dropped by about 60% within a month, and support's median resolution time fell from a day to a few hours. Support later asked for two more features, which we prioritised properly on the roadmap."

**Pitfalls:**
- Framing it as heroics that came at the expense of your actual job.
- Bypassing your manager or another team's ownership without communicating.

### Tell me about the most impactful thing you've built. How did you measure impact?

**What they're looking for:** Whether you think in outcomes, can define metrics before you build, and are honest about attribution.

**STAR template:**
- **S:** [Problem and baseline metric]
- **T:** [Goal and the metric you agreed on up front]
- **A:** [What you built; how you instrumented it; how you isolated its effect]
- **R:** [Before/after numbers, and any second-order effects]
- **L:** [What you learned about measuring impact]

**Example (adapt to your own experience):**
"At a fintech, manual review of loan applications was the bottleneck — about 30% of applications sat for over 48 hours. I built a document-extraction and rules service that pre-filled the reviewer's form and auto-approved clean, low-risk cases. Before building, I agreed with the ops lead on three metrics: median time-to-decision, reviewer throughput, and error rate on a weekly audited sample. I rolled it out to half the reviewers first so we had a comparison group. Median decision time dropped from 31 hours to 6, throughput per reviewer roughly doubled, and the audited error rate stayed flat. I was careful to report that a seasonal dip in volume explained part of the improvement — the A/B split let us show the tool accounted for most of it."

**Pitfalls:**
- Vanity metrics ("lines of code", "number of features") instead of business outcomes.
- Claiming 100% of a team result, or not knowing the numbers at all.

### Tell me about a time you took initiative without being asked.

**What they're looking for:** Bias for action, ability to spot problems before they're assigned, and good judgment about scope and communication.

**STAR template:**
- **S:** [What you noticed and why no one was handling it]
- **T:** [What you decided to do]
- **A:** [How you validated it was worth doing, built a small first version, got buy-in]
- **R:** [Result]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"During a customer deployment, I noticed every new customer onboarding needed someone to hand-write the same 200 lines of config YAML, and mistakes caused about one failed deployment a week. Nobody had asked for a fix. I spent a day building a CLI that generated the config from a short questionnaire and validated it against a JSON schema, then showed it to two other deployment engineers. They started using it immediately, and I turned it into a proper internal tool with tests. Config-related deployment failures went to near zero over the next two months, and onboarding time dropped by about half a day per customer."

**Pitfalls:**
- Initiative that ignored priorities or created work others had to clean up.
- Not mentioning how you validated the idea before investing heavily.

### Tell me about a time you shipped something that users loved.

**What they're looking for:** User empathy, how you learned what users wanted, and evidence (not just your feeling) that they loved it.

**STAR template:**
- **S:** [Users and their pain]
- **T:** [What you set out to build]
- **A:** [How you discovered needs, iterated with users, cut scope]
- **R:** [Evidence of love: adoption, retention, quotes, NPS, unsolicited feedback]
- **L:** [What made it land]

**Example (adapt to your own experience):**
"Field technicians at a utilities customer were filling out paper inspection forms and re-typing them into a web app at night. I built a mobile-first offline form app with photo capture and sync-on-reconnect. Before writing code, I rode along with two technicians for a day and learned they wear gloves and work in bad light, so I used large tap targets and high contrast, and dropped half the fields they never used. We piloted with 10 technicians and shipped three iterations in three weeks based on their feedback. Within two months, 90% of technicians had switched voluntarily, data entry time per inspection dropped from 15 minutes to 3, and the operations manager forwarded a message from a technician saying it 'gave him his evenings back.'"

**Pitfalls:**
- No evidence beyond "people liked it."
- Ignoring how you learned what users needed.

### Tell me about a time you had to deliver results with limited resources.

**What they're looking for:** Resourcefulness, ruthless prioritisation, and smart use of existing tools rather than building everything.

**STAR template:**
- **S:** [The constraint: time, people, budget, infra]
- **T:** [What had to be delivered]
- **A:** [How you prioritised, what you reused or bought, what you cut, how you managed expectations]
- **R:** [What was delivered and its impact]
- **L:** [What you would do the same/differently]

**Example (adapt to your own experience):**
"A customer pilot needed a working document Q&A prototype in three weeks, and I was the only engineer available. Instead of building a custom retrieval stack, I used a managed vector database, an off-the-shelf LLM API, and Streamlit for the UI. I agreed with the customer on the three document types and twenty test questions that mattered most, and explicitly deferred SSO and multi-tenancy. I built a small evaluation script over those twenty questions so I could iterate quickly. We demoed on time with 85% answer accuracy on their test set, the customer signed a paid pilot, and the scoped-down design was easy for the team to harden afterwards."

**Pitfalls:**
- Complaining about the constraints rather than showing how you worked within them.
- Delivering on time by silently cutting quality that later hurt the customer.

## Ambiguity & bias for action

### Tell me about a time you worked on a problem with no clear requirements.

**What they're looking for:** This is the core FDE skill. They want to see you create clarity: talk to users, form hypotheses, ship something small, and iterate — rather than waiting for a perfect spec.

**STAR template:**
- **S:** [The vague ask and who made it]
- **T:** [What you needed to figure out]
- **A:** [How you discovered requirements: interviews, data, prototypes; how you wrote them down and got agreement]
- **R:** [What shipped and how it was received]
- **L:** [Your playbook for ambiguity now]

**Example (adapt to your own experience):**
"A healthcare customer's VP asked us to 'use AI to reduce claim denials' — no spec, no data access yet. I scheduled interviews with three billing specialists and pulled six months of denial codes once we got access. That showed 40% of denials came from just two causes: missing prior authorisations and coding mismatches. I wrote a one-page problem statement with those two targets and a success metric, and got the VP to sign off. Then I built a pre-submission checker for those two cases in two weeks and ran it in shadow mode. It would have caught about 70% of those denials, so we turned it on. Denial rate dropped by 12 percentage points over the quarter. My lesson: turn the vague ask into a written, measurable problem as early as possible."

**Pitfalls:**
- Waiting for someone else to define requirements.
- Building a lot before validating — or never committing to any direction.

### Tell me about a time you had to make a decision with incomplete information.

**What they're looking for:** Judgment under uncertainty — how you gather enough information quickly, distinguish reversible from irreversible decisions, and set checkpoints to revisit.

**STAR template:**
- **S:** [The decision and what was unknown]
- **T:** [Deadline or cost of waiting]
- **A:** [What info you gathered quickly, the options, how you assessed risk/reversibility, how you hedged]
- **R:** [Outcome, including whether the decision was right]
- **L:** [Your framework]

**Example (adapt to your own experience):**
"Two weeks before a customer go-live, we had to choose between their on-prem Postgres and our managed cloud database for a deployment. We didn't know their data-residency requirements for certain — legal was still reviewing. Waiting would miss the go-live. I treated it as a reversible decision: I chose the cloud database because it unblocked us, but I kept the data layer behind a repository interface and wrote a migration script so we could move on-prem within a few days if needed. I documented the assumption and the trigger for revisiting. Legal came back three weeks later requiring EU residency, and because of the preparation we moved to an EU region in two days with no downtime."

**Pitfalls:**
- Presenting a lucky guess as good judgment — explain the reasoning and hedges.
- Paralysis: showing you waited for complete information.

### Tell me about a time you had to ship fast and cut corners. How did you decide what to cut?

**What they're looking for:** That you cut *deliberately*: you protect correctness, security and data integrity; you cut polish, scale and nice-to-haves; and you track the debt.

**STAR template:**
- **S:** [Deadline and why it mattered]
- **T:** [What needed to ship]
- **A:** [Your criteria for cutting; what you kept non-negotiable; how you recorded and communicated the debt]
- **R:** [Shipped outcome, and when/whether the debt was paid down]
- **L:** [Your rules for cutting]

**Example (adapt to your own experience):**
"A customer needed a data export feature live before their board meeting in five days. I sorted the work into three buckets. Non-negotiable: correct numbers, access control, and a test on the core aggregation. Cuttable: a UI (we shipped a CSV download link), async processing for large exports (we capped exports at 100k rows), and a configurable schema. I created tickets for each cut with the reason, and told the customer exactly what the limits were. We shipped in four days. Two weeks later, I came back and built async exports because two other customers hit the row limit."

**Pitfalls:**
- Cutting tests, security or data correctness and treating it as fine.
- Not tracking the debt or never coming back to it.

### Tell me about a time priorities changed suddenly.

**What they're looking for:** Adaptability without chaos — how you re-plan, communicate impact, and avoid losing work in progress.

**STAR template:**
- **S:** [What you were working on and what changed]
- **T:** [What you had to do now]
- **A:** [How you wound down/parked the old work, re-planned, communicated to stakeholders]
- **R:** [Outcome on both the new and old priorities]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"Mid-sprint, I was building a reporting feature when our largest customer reported that a schema change on their side had broken our nightly sync — their dashboards were stale. Leadership made it the top priority. I pushed my in-progress branch with a short note on its state, told the PM which deliverable would slip and by how much, and switched over. I worked with the customer's data engineer to understand the new schema, added a schema-validation step that fails loudly instead of silently, and restored the sync in a day and a half. The reporting feature slipped by three days, which I had already flagged, so nobody was surprised."

**Pitfalls:**
- Sounding resentful about the change.
- Forgetting to tell the owners of the deprioritised work.

### Tell me about a time you had to choose between speed and quality.

**What they're looking for:** That you see it as a context-dependent trade-off, not a false binary — and that you can explain the cost of each option.

**STAR template:**
- **S:** [The situation and the pressure]
- **T:** [The choice]
- **A:** [How you evaluated the cost of being late vs. the cost of a defect; the middle path, if any]
- **R:** [What happened]
- **L:** [Your rule of thumb]

**Example (adapt to your own experience):**
"A customer wanted an ML-based anomaly detector for their payments within two weeks. A proper model with backtesting would take six. I asked what happens if an anomaly is missed versus a false alarm: false alarms only cost an analyst a few minutes, but missing fraud was costly. So I shipped a simple statistical baseline — z-scores per merchant with conservative thresholds — in a week, clearly labelled as v0, and used the analysts' feedback on alerts as labelled data for the real model. The v0 caught two real fraud cases in its first month. The ML model shipped six weeks later with a measured 30% reduction in false alarms over the baseline. My rule: go fast where mistakes are cheap and reversible; slow down where they're expensive or irreversible."

**Pitfalls:**
- Always choosing one side ("I never compromise on quality").
- Not explaining how you reasoned about the cost of each option.

## Customer focus

### Tell me about a time you worked directly with a customer or end user.

**What they're looking for:** Comfort in front of customers, listening skills, and how you translate what you hear into technical work. This is the heart of the FDE job.

**STAR template:**
- **S:** [Customer, their context, your role]
- **T:** [What you needed to achieve with them]
- **A:** [How you built trust, learned their workflow, handled technical and non-technical people, fed back to your team]
- **R:** [Outcome for the customer and for your company]
- **L:** [What you learned about working with customers]

**Example (adapt to your own experience):**
"I was embedded for six weeks with an insurance customer rolling out our document-processing platform. I set up a weekly demo with their claims team and a daily 15-minute check-in with their IT lead. In the first week I sat beside adjusters and noticed they kept a separate spreadsheet of 'problem claims' — that became a requirement for an exceptions queue we hadn't planned. I also translated their requests into tickets for our product team with frequency and impact attached, so product could prioritise. By the end, the claims team was processing 60% of documents automatically, and two of my tickets became core product features used by other customers."

**Pitfalls:**
- Describing the customer as an obstacle.
- Only talking about the build, not the relationship.

### Tell me about a time you turned around an unhappy customer/stakeholder.

**What they're looking for:** Ownership without defensiveness, empathy, fast concrete action, and rebuilding trust through follow-through.

**STAR template:**
- **S:** [Why they were unhappy — be honest if your side was at fault]
- **T:** [What you needed to recover]
- **A:** [Listen → acknowledge → concrete plan with dates → over-communicate → deliver]
- **R:** [Relationship and business outcome]
- **L:** [What you changed to prevent a repeat]

**Example (adapt to your own experience):**
"I inherited a customer account where the previous integration had missed two deadlines and their data team was openly threatening to churn. In my first call I mostly listened, then summarised their three complaints back and acknowledged which ones were our fault. I proposed a two-week plan: fix the top data-quality bug in week one, and deliver a reconciliation report in week two so they could verify our numbers themselves. I sent a short written status every other day. We hit both dates. The reconciliation report mattered most — it gave them a way to trust us without taking our word for it. They renewed, and later became a reference customer."

**Pitfalls:**
- Blaming the customer or a colleague.
- Promising too much in the first meeting to make them happy.

### Tell me about a time you said no to a customer.

**What they're looking for:** That you can protect the product, your team, and the customer's real interests, while keeping the relationship. A good "no" comes with a reason and an alternative.

**STAR template:**
- **S:** [The request and who made it]
- **T:** [Why it was a bad idea: risk, scope, product direction, security]
- **A:** [How you understood the underlying need, explained the no, offered an alternative]
- **R:** [What the customer accepted and the outcome]
- **L:** [How you say no now]

**Example (adapt to your own experience):**
"A customer asked us to give their analysts direct write access to our production database so they could 'fix data themselves'. That would have broken our audit trail and risked corrupting other data. I asked what they were trying to fix and learned it was mostly wrong category labels from an upstream system. I explained the risk plainly, then offered an alternative: a correction workflow in the app with an approval step and full audit log, which I scoped to a week. They agreed. The workflow handled about 200 corrections in the first month, and their compliance team later said the audit log helped them in an internal audit."

**Pitfalls:**
- A flat no with no alternative.
- Saying no for your convenience rather than for a principled reason.

### Tell me about a time you discovered the customer's real need was different from what they asked for.

**What they're looking for:** The "ask why" skill — that you look for the underlying problem rather than building the literal request. This is one of the most important FDE questions.

**STAR template:**
- **S:** [The original request]
- **T:** [What you were asked to build]
- **A:** [How you dug deeper: questions, observing workflows, data; how you validated your hypothesis; how you brought the customer along]
- **R:** [What you built instead and why it worked better]
- **L:** [How you now probe requests]

**Example (adapt to your own experience):**
"A retail customer asked for a real-time dashboard of store inventory. Before designing it, I asked who would look at it and what they would do differently. It turned out regional managers only acted on one thing: items about to run out in high-traffic stores. They didn't want to watch a dashboard — they wanted to be told. I validated this by showing two managers a mock alert email next to a dashboard mock-up; both preferred the alert. We built a daily ranked stock-out risk alert instead, which was simpler to build and more useful. Stock-outs on top-selling items fell by about 20% in the pilot region."

**Pitfalls:**
- Sounding like you know better and overrode the customer.
- Not explaining how you validated the real need.

### Tell me about a time you had to manage customer expectations.

**What they're looking for:** Honesty, proactive communication, and the ability to reset expectations early rather than surprising people late.

**STAR template:**
- **S:** [What the customer expected and why it was unrealistic]
- **T:** [What you needed to reset]
- **A:** [How you communicated early, gave options, used data, documented agreements]
- **R:** [How the customer reacted and the result]
- **L:** [What you do proactively now]

**Example (adapt to your own experience):**
"After a sales demo, a customer expected our LLM extraction tool to be 99% accurate on their contracts from day one. On their real documents our first test was about 80%. Rather than hoping it would improve, I shared the evaluation results in week one, broke the errors down by type, and laid out a plan: we could reach about 92% in a month with better prompts, worked examples and their feedback, and route low-confidence extractions to human review to make the end-to-end process reliable. I put the targets and dates in writing. They were disappointed at first but appreciated the transparency, and we hit 93% with human review on the remaining cases. They signed the annual contract."

**Pitfalls:**
- Waiting until the deadline to share bad news.
- Blaming sales for overpromising in front of the interviewer.

### Tell me about a time you represented the user's voice in a technical decision.

**What they're looking for:** That you bring field knowledge back into engineering decisions, and can argue for users with evidence, not just opinion.

**STAR template:**
- **S:** [The technical decision being made]
- **T:** [The user impact others were missing]
- **A:** [How you gathered evidence — quotes, data, usage logs — and made the case]
- **R:** [The decision and its effect on users]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"Our team planned to move a batch import from synchronous to an async job queue, which was cleaner technically. From working with customers, I knew many users uploaded a file and then immediately built a report — with async, they'd hit empty data and think the product was broken. I pulled usage logs showing 65% of imports were followed by a report within two minutes, and shared three support tickets about confusing delays. We kept async processing but added a progress indicator, an email when done, and a report view that showed 'import in progress' instead of empty data. Support tickets about imports dropped after launch instead of rising."

**Pitfalls:**
- Arguing from anecdote only.
- Positioning engineering as the enemy of users.

## Conflict & collaboration

### Tell me about a time you disagreed with your manager.

**What they're looking for:** That you can disagree respectfully with data, escalate appropriately, and commit once a decision is made ("disagree and commit") — or change your mind.

**STAR template:**
- **S:** [The decision your manager made or proposed]
- **T:** [Why you disagreed]
- **A:** [How you raised it privately, with evidence and alternatives; how you listened to their reasoning]
- **R:** [What was decided and how it played out]
- **L:** [What you learned about disagreeing well]

**Example (adapt to your own experience):**
"My manager wanted to commit to a customer that we'd support their legacy SOAP API within the current quarter. I thought it would take far longer because their API had no sandbox and inconsistent docs. I asked for 20 minutes, showed a breakdown of the unknowns, and proposed a one-week spike before committing to a date. He was worried about losing the deal, which I hadn't fully appreciated. We agreed to tell the customer we'd confirm a date after the spike. The spike found two blocking issues, and we committed to a date six weeks later than originally planned — which we hit. The customer preferred a reliable date to a fast one."

**Pitfalls:**
- Making your manager look foolish.
- A story where you "won" and never considered their perspective, or where you complained but never raised it.

### Tell me about a time you disagreed with a teammate on a technical decision.

**What they're looking for:** Technical reasoning, openness to being wrong, and resolving conflict with data or experiments rather than ego.

**STAR template:**
- **S:** [The decision]
- **T:** [The two positions]
- **A:** [How you understood their view, made criteria explicit, used a prototype/benchmark or a third opinion]
- **R:** [Decision and outcome; relationship afterwards]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"A teammate wanted to use Kafka for a new event pipeline; I thought a managed queue like SQS was enough for our volume. Instead of debating, we wrote down criteria together: expected throughput, ordering needs, ops burden, and replay needs. Our volume was low, but he pointed out that the customer would need to replay events after a bug, which SQS doesn't handle well. That was a fair point I had missed. We compromised on a managed Kafka service so we got replay without running clusters ourselves. It worked well, and we used the same criteria doc for later decisions."

**Pitfalls:**
- Stories where the other person was simply wrong and you were right.
- Not showing what you learned from their perspective.

### Tell me about a time you had a conflict with a stakeholder.

**What they're looking for:** Navigating competing interests (sales, product, customer, security) professionally, finding shared goals, and escalating cleanly when needed.

**STAR template:**
- **S:** [The stakeholder and the conflict]
- **T:** [What was at stake]
- **A:** [How you understood their incentives, found common ground, proposed options, escalated if needed]
- **R:** [Resolution]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"A sales lead promised a prospect a custom SSO integration within two weeks to close a deal, without checking with engineering. I was frustrated, but I met with him to understand the deal: the prospect actually needed SSO before their security review, not before go-live. I proposed supporting their identity provider through our existing SAML setup, which would take three days, and deferring the custom attribute mapping to the next month. He took that to the prospect and they agreed. We closed the deal, and afterwards he and I set up a simple rule: any technical commitment in a deal gets a quick check from the deployment team first."

**Pitfalls:**
- Villainising the stakeholder.
- Solving the immediate conflict without fixing the process that caused it.

### Tell me about a time you had to work with a difficult person.

**What they're looking for:** Emotional maturity, curiosity about why someone behaves the way they do, and focus on the work rather than the personality.

**STAR template:**
- **S:** [The person (neutral description) and the behaviour]
- **T:** [What you needed from the working relationship]
- **A:** [How you sought to understand them, adjusted your approach, addressed it directly]
- **R:** [How the relationship and work improved]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"A customer's IT lead was blocking our deployment — he rejected every access request and was curt in meetings. I asked to meet him one-on-one and asked what worried him. It came out that a previous vendor had caused a data breach, and he would be blamed if it happened again. I changed my approach: I sent him a written security overview, asked for the minimum access we needed, and invited him to review our network diagram. He became one of our strongest supporters and later helped us get approval faster for a second project."

**Pitfalls:**
- Speaking negatively about the person.
- A story with no change in your own behaviour.

### Tell me about a time you influenced someone without authority.

**What they're looking for:** How you persuade: data, prototypes, aligning with their goals, and building coalitions — skills FDEs use constantly with customers and product teams.

**STAR template:**
- **S:** [Who you needed to influence and why you had no authority]
- **T:** [The change you wanted]
- **A:** [How you understood their goals, built evidence, made it easy to say yes]
- **R:** [The outcome]
- **L:** [Your influence playbook]

**Example (adapt to your own experience):**
"I wanted our core product team to add a bulk-import API that three of my customers needed; they had a full roadmap and no reason to listen to me. I gathered evidence: the hours deployment engineers spent on manual imports each month, and the deals that depended on it. I also wrote a short design proposal and a working prototype on a branch so the cost looked small. I presented it in their planning meeting framed around their goal of reducing custom work. They added it to the next quarter, and it now saves deployment engineers roughly a week per customer onboarding."

**Pitfalls:**
- Escalating to someone's boss as the first move.
- "I convinced them" without explaining how.

### Tell me about a time you worked with a cross-functional team (sales, product, design).

**What they're looking for:** That you understand what other functions care about, communicate in their language, and act as a bridge — central to the FDE role.

**STAR template:**
- **S:** [The project and the functions involved]
- **T:** [Your role in the team]
- **A:** [How you aligned goals, communicated, resolved trade-offs]
- **R:** [Outcome]
- **L:** [What you learned about each function]

**Example (adapt to your own experience):**
"For a strategic customer launch, I worked with an account executive, a PM, and a designer. Sales cared about the go-live date, product about not building one-off features, and design about a consistent experience. I set up a shared doc that mapped each customer requirement to either 'existing product', 'configuration', or 'new feature', and we reviewed it weekly. That made trade-offs explicit: two requests became generic product features the designer shaped, and one was handled with configuration. We launched on time, and the two features were reused by four other customers that year."

**Pitfalls:**
- Treating other functions as non-technical people who "don't get it."
- Describing meetings, not outcomes.

### Tell me about a time you received critical feedback. What did you do?

**What they're looking for:** Self-awareness, coachability, and evidence that you changed your behaviour.

**STAR template:**
- **S:** [Context and who gave the feedback]
- **T:** [The feedback itself — be specific]
- **A:** [Your reaction, how you sought to understand it, concrete steps you took]
- **R:** [Evidence of improvement]
- **L:** [How you seek feedback now]

**Example (adapt to your own experience):**
"After a customer workshop, my manager told me I had spent 20 minutes in technical detail that lost the business stakeholders, and that the customer's VP left without a clear next step. It stung, but she was right. I asked her to review my next deck before the meeting, and I started structuring customer presentations as 'decision needed, recommendation, supporting detail in the appendix'. I also began ending every meeting with written next steps. In my next review she pointed to two customer meetings that had gone noticeably better, and one VP asked for my deck to share internally."

**Pitfalls:**
- Choosing feedback that is secretly a strength ("I work too hard").
- Being defensive while telling the story.

### Tell me about a time you gave difficult feedback to someone.

**What they're looking for:** Courage, kindness, and clarity — feedback that is specific, timely, private, and focused on behaviour and impact.

**STAR template:**
- **S:** [Who and what behaviour]
- **T:** [Why it needed addressing]
- **A:** [How you prepared and delivered it (e.g. situation–behaviour–impact), listened, offered support]
- **R:** [How they responded and changed]
- **L:** [What you learned about giving feedback]

**Example (adapt to your own experience):**
"A peer on my deployment team was regularly merging changes to a customer's environment without review, and one caused a two-hour outage for that customer. I asked to talk privately and used a situation–behaviour–impact structure: 'In last week's deploy, the change went out without review, and the customer lost two hours of processing — and it's making them question our reliability.' I asked what was going on. He felt reviews were too slow because the customer kept requesting urgent changes. We agreed to propose a fast-track review rule for small changes, which the team adopted. He started following the process, and we didn't have another unreviewed incident."

**Pitfalls:**
- Feedback that was vague, public, or delayed for months.
- Not listening to the other person's side.

### Tell me about a time you helped a teammate succeed / mentored someone.

**What they're looking for:** Whether you make others better — teaching, unblocking, and giving credit.

**STAR template:**
- **S:** [Who and what they struggled with]
- **T:** [Your goal for them]
- **A:** [How you taught: pairing, gradually increasing ownership, feedback]
- **R:** [Their growth — specific]
- **L:** [What you learned about mentoring]

**Example (adapt to your own experience):**
"A new graduate on my team was strong at coding but nervous in customer calls. I had her shadow two calls, then lead the agenda of the third while I handled questions, then run a call alone with me as backup. Before each one we spent 15 minutes on what the customer cared about and likely questions; afterwards we debriefed. Within two months she was running her own customer's weekly check-ins, and the customer specifically praised her in a renewal call. I learned that giving people progressively bigger responsibility with a safety net works better than lots of advice."

**Pitfalls:**
- Taking credit for their success.
- Mentoring that was really just doing the work for them.

## Failure & learning

### Tell me about a time you failed.

**What they're looking for:** A real failure (not a disguised success), full ownership without excuses, and concrete learning applied afterwards.

**STAR template:**
- **S:** [Context]
- **T:** [What you were trying to achieve]
- **A:** [What you did — including the mistake, stated plainly]
- **R:** [The failure and its impact; what you did to recover]
- **L:** [What you changed afterwards, with evidence you applied it]

**Example (adapt to your own experience):**
"I led a three-month pilot to deploy a forecasting tool for a manufacturing customer, and it was not renewed. I had focused on model accuracy and spent little time with the planners who would use it. When we launched, they didn't trust the forecasts because they couldn't see why the numbers changed, so they kept using their spreadsheets. I tried to recover by adding explanations late, but the pilot ended first. What I changed: on every project since, I involve end users from week one, and I treat 'will they trust it and use it' as a requirement alongside accuracy. On my next deployment, I ran fortnightly sessions with users and adoption was above 80%."

**Pitfalls:**
- A fake failure ("I failed to say no to extra work because I care too much").
- Blaming others or circumstances.
- No evidence the lesson stuck.

### Tell me about your biggest mistake. What did you learn?

**What they're looking for:** Similar to failure, but focused on an error you made. They want honesty about severity and a clear, durable lesson.

**STAR template:**
- **S:** [Context]
- **T:** [Your responsibility]
- **A:** [The mistake, plainly]
- **R:** [Impact; how you owned it and fixed it]
- **L:** [Systemic change you made]

**Example (adapt to your own experience):**
"I ran a data migration for a customer and validated it by comparing row counts between old and new tables. The counts matched, so I signed off. A week later, the customer found that timestamps had shifted by several hours because of a timezone conversion I hadn't checked. I told them and my manager immediately, wrote a fix and re-ran the affected data within a day, and sent a written explanation of what happened. The lesson was that row counts are not validation. I now write migration checks that compare checksums and sample values for every column type, and I added a migration checklist to our team wiki that includes timezone and encoding checks."

**Pitfalls:**
- A trivial mistake that shows nothing.
- A catastrophic mistake told with no ownership or learning.

### Tell me about a project that didn't go as planned.

**What they're looking for:** How you react when things go sideways — re-planning, communicating, and salvaging value.

**STAR template:**
- **S:** [The plan]
- **T:** [Your role]
- **A:** [What went wrong, when you noticed, how you adapted and communicated]
- **R:** [What you salvaged]
- **L:** [What you would do differently in planning]

**Example (adapt to your own experience):**
"We planned a four-week integration with a customer's CRM, assuming their API was well documented. In week one we found that half the endpoints we needed were undocumented and rate-limited to 100 calls an hour. I flagged it to the customer and my manager that day with two options: wait for their vendor to raise limits, or use their nightly database export instead. We chose the export, which meant data was a day stale but unblocked us. We delivered in six weeks rather than four. Now I run a technical spike against real customer systems before committing to any timeline."

**Pitfalls:**
- Blaming the plan, the customer, or bad luck without your own learning.
- Hiding when you first noticed the problem.

### Tell me about a time you missed a deadline.

**What they're looking for:** Whether you flag risk early, own it, minimise impact, and fix your estimation or process.

**STAR template:**
- **S:** [The deadline and its importance]
- **T:** [Your commitment]
- **A:** [Why it slipped; when and how you communicated; mitigation]
- **R:** [Impact and recovery]
- **L:** [How you plan or communicate differently now]

**Example (adapt to your own experience):**
"I committed to delivering a reporting integration for a customer's quarterly review. I underestimated the work to handle their multi-currency data, and by the midpoint I knew I would be about a week late. I told the customer that day rather than at the deadline, and offered a partial delivery: reports in their main currency on time, with multi-currency a week later. They used the partial version for the review, and the full version landed six days later. Since then I add explicit buffers for unknowns in customer data and send a red/amber/green status at the midpoint of any commitment."

**Pitfalls:**
- Waiting until the deadline to tell anyone.
- Blaming the estimate on someone else.

### Tell me about a time you were wrong.

**What they're looking for:** Intellectual humility — that you update on evidence and say "I was wrong" clearly.

**STAR template:**
- **S:** [What you believed and argued for]
- **T:** [Why it mattered]
- **A:** [What evidence changed your mind; how you acknowledged it]
- **R:** [Outcome]
- **L:** [How you now hold opinions]

**Example (adapt to your own experience):**
"I argued strongly that we should build our own retrieval pipeline for a customer's search feature, because I believed off-the-shelf tools wouldn't handle their domain vocabulary. A colleague suggested we test a managed search service with hybrid keyword and vector search first. I agreed to a one-week bake-off on 100 real queries, and the managed service matched my prototype's quality at a fraction of the maintenance cost. I said in the team channel that I'd been wrong and we went with it. Now I try to state what evidence would change my mind before I argue a position."

**Pitfalls:**
- Being wrong about something trivial.
- Admitting it grudgingly or with qualifications.

### Tell me about a time you broke production.

**What they're looking for:** Calm incident handling, honesty, blameless analysis, and systemic fixes — not "I'll be more careful."

**STAR template:**
- **S:** [What you changed]
- **T:** [What broke and who was affected]
- **A:** [Detection, mitigation (rollback first), communication, root cause]
- **R:** [Duration and impact]
- **L:** [Systemic fixes: tests, alerts, deploy process]

**Example (adapt to your own experience):**
"I deployed a database index change for a customer's environment that locked a large table during peak hours, and their API latency spiked for about 25 minutes. Our alerts caught it within five minutes; I posted in the incident channel that it was my change, rolled back, and the customer's ops contact was updated within 15 minutes. In the blameless postmortem we found two gaps: no check for blocking migrations and no deploy window for customer environments. I added a migration linter that flags non-concurrent index creation, and we introduced off-peak deploy windows for schema changes. We had no repeat of that class of issue."

**Pitfalls:**
- Saying you've never broken production (not credible for experienced engineers).
- Hiding that it was your change, or a lesson of "be more careful."

### Tell me about a time you had to learn something new quickly.

**What they're looking for:** Learning strategy and speed — FDEs constantly land in new customer stacks and domains.

**STAR template:**
- **S:** [What you didn't know and why you needed it]
- **T:** [How fast and how deep]
- **A:** [Your learning approach: docs, experts, building small things, asking good questions]
- **R:** [What you delivered]
- **L:** [Your learning playbook]

**Example (adapt to your own experience):**
"I was assigned to a customer using SAP as their system of record, which I had never touched, and had to build an integration in three weeks. On day one I asked their SAP admin for a two-hour walkthrough of the specific tables and transactions we needed, rather than trying to learn SAP broadly. I read the relevant OData API docs, built a tiny script that pulled one record end to end, and kept a glossary of terms. By the end of week one I could hold a technical conversation with their team, and we delivered the integration on time. My approach is to learn only what the problem needs, get to a working end-to-end example fast, and lean on experts early."

**Pitfalls:**
- "I read the docs" with no strategy.
- Not showing the outcome.

### What's something you've changed your mind about recently?

**Framework:** Not a STAR story, but structure it as: *what I used to believe → what changed my mind (evidence/experience) → what I believe now → how it changed my behaviour.* Pick something professional and substantive, ideally relevant to the role. Avoid politics.

**Example (adapt to your own experience):**
"I used to think that evaluating LLM features was mostly a matter of eyeballing outputs, and formal evaluation was overkill for small projects. On a recent document-extraction project, a prompt change I was sure was an improvement made accuracy worse on a category of documents I hadn't looked at. Since then I've changed my mind: I now build a small labelled eval set — even 50 examples — before iterating on anything, and run it on every change. It's made me faster, not slower, because I stop arguing about whether a change helped."

**Pitfalls:**
- Something trivial ("I switched from tabs to spaces").
- Something that suggests you rarely update your views.

## Problem solving & technical judgment

### Tell me about the hardest technical problem you've solved.

**What they're looking for:** Real technical depth, how you break down hard problems, and whether you can explain them clearly. Expect follow-up questions — pick something you can go deep on.

**STAR template:**
- **S:** [System and context]
- **T:** [Why the problem was hard: scale, ambiguity, constraints]
- **A:** [Hypotheses, experiments, the key insight, the solution and alternatives rejected]
- **R:** [Measured result]
- **L:** [What you'd do differently]

**Example (adapt to your own experience):**
"A customer's entity-resolution job merged supplier records across 12 source systems — about 40 million records — and took 30 hours, so it couldn't run daily. Pairwise comparison was the bottleneck. I profiled it and found 95% of comparisons were between obviously different records. I introduced blocking: grouping candidates by normalised name prefixes and postcode, then running the expensive fuzzy matching only within blocks, in parallel on Spark. The hard part was recall — strict blocks missed real matches — so I used multiple blocking keys and measured recall against 2,000 hand-labelled pairs. Final runtime was 90 minutes with recall within 1% of the original. I'd now start with that labelled set on day one rather than building it halfway through."

**Pitfalls:**
- A problem that was hard only because of process or people.
- Too much jargon; failing to explain the key insight simply.

### Tell me about a difficult bug you tracked down.

**What they're looking for:** Systematic debugging — reproduce, isolate, form hypotheses, verify — and persistence. Also whether you prevented recurrence.

**STAR template:**
- **S:** [Symptom and impact]
- **T:** [Why it was hard: intermittent, only in prod, cross-system]
- **A:** [Steps: reproduction, logs/metrics, bisecting, hypotheses tested]
- **R:** [Root cause and fix]
- **L:** [Prevention: test, alert, tooling]

**Example (adapt to your own experience):**
"A customer reported that about 1 in 1,000 webhook events was processed twice, causing duplicate invoices. It never happened in staging. I added request IDs to the logs and found the duplicates always arrived around 30 seconds apart. That matched the load balancer's idle timeout: our handler sometimes took over 30 seconds on large payloads, the load balancer dropped the connection, and the sender retried while our first handler was still running and eventually succeeded. The fix was two parts: acknowledge the webhook immediately and process it async, and make processing idempotent with a unique key on the event ID. I added a test for duplicate delivery and an alert on processing time. Duplicates went to zero."

**Pitfalls:**
- "I looked at the code and found it" — no method.
- Fixing the symptom without addressing the root cause.

### Tell me about a time you made a significant architectural decision. What were the trade-offs?

**What they're looking for:** That you consider options, make trade-offs explicit, consider the team's ability to operate the system, and reflect honestly on the result.

**STAR template:**
- **S:** [System and requirements]
- **T:** [The decision]
- **A:** [Options considered, criteria (cost, latency, complexity, team skills, reversibility), why you chose one, how you documented it (e.g. ADR)]
- **R:** [How it played out]
- **L:** [What you'd change]

**Example (adapt to your own experience):**
"We had to decide how to deploy our platform for enterprise customers who wouldn't allow data to leave their cloud. Options were: multi-tenant SaaS with a data-plane agent in the customer's VPC, fully single-tenant deployments per customer, or a hybrid. Single-tenant was simplest to sell but would mean dozens of environments to upgrade. I wrote an ADR recommending the hybrid: control plane in our cloud, data processing in the customer's VPC via a Helm chart. Trade-offs: more complex networking and a harder debugging story, in exchange for one upgrade path and data residency. It let us onboard regulated customers within weeks. In hindsight, I'd have invested earlier in remote diagnostics, because debugging inside customer VPCs was our biggest pain."

**Pitfalls:**
- Presenting it as obviously correct with no downsides.
- Not knowing what happened after the decision.

### Tell me about a time you simplified a complex system.

**What they're looking for:** Taste — preferring simple, maintainable solutions — and the discipline to remove things safely.

**STAR template:**
- **S:** [The complex system and the cost of its complexity]
- **T:** [Your goal]
- **A:** [How you understood it fully before simplifying; what you removed or consolidated; how you migrated safely]
- **R:** [Measured improvement: incidents, latency, cost, onboarding time]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"Our customer data sync used four microservices, two queues and a cron job, built up over time. Debugging a failed sync meant checking six places, and on-call pages were frequent. I mapped the full data flow and found two services only reshaped data and one queue existed for a use case that had been removed. I proposed consolidating into a single worker with clear stages and structured logging, migrated one customer at a time behind a feature flag, and compared outputs between old and new paths for a week. We removed about 6,000 lines of code, sync-related pages dropped by roughly 70%, and new engineers could understand the flow in an afternoon."

**Pitfalls:**
- A rewrite that was really driven by personal preference.
- Not explaining how you migrated without breaking things.

### Tell me about a time you had to work in an unfamiliar codebase or technology.

**What they're looking for:** How fast you get productive in someone else's code — a daily FDE reality at customer sites.

**STAR template:**
- **S:** [The codebase/technology]
- **T:** [What you had to deliver and by when]
- **A:** [How you oriented: running it locally, tracing a request, reading tests, finding the owner, making a small change first]
- **R:** [What you shipped]
- **L:** [Your approach to new codebases]

**Example (adapt to your own experience):**
"I had to add an integration to a customer's internal Java monolith of about 500k lines, with my background mostly in Python. On day one I got it running locally and traced one request from the HTTP controller to the database with a debugger. I read the tests around the module I had to change, and asked their senior engineer for 30 minutes to explain the conventions. I made a tiny, low-risk change first to learn their build and review process. Then I built the integration over two weeks, following their existing patterns rather than mine. Their team approved the PR with minor comments, and they kept it as a reference for later integrations."

**Pitfalls:**
- Trying to understand everything before changing anything.
- Imposing your own style on someone else's codebase.

### Tell me about a time you turned a one-off solution into a reusable tool.

**What they're looking for:** Leverage — the FDE habit of noticing repeated custom work and productising it, and judgment about when to generalise.

**STAR template:**
- **S:** [The one-off solution]
- **T:** [The signal it should be reusable]
- **A:** [How you generalised it: abstraction, configuration, docs, getting others to adopt]
- **R:** [Reuse and time saved]
- **L:** [When to generalise vs. not]

**Example (adapt to your own experience):**
"I wrote a script for one customer that validated their CSV uploads against our data model and produced a readable error report. Three weeks later another deployment engineer asked me for it, and then a third. That was my signal. I turned it into a small CLI and library: rules defined in YAML per customer, an HTML report, and a test suite, and I documented it in our deployment playbook. It was used in every new onboarding after that, cutting the data-cleaning phase from about a week to two days. My rule is to generalise on the third time, not the first."

**Pitfalls:**
- Over-engineering a framework before there's proven repeat demand.
- Building a tool nobody adopted.

### Tell me about a time you used data to make a decision.

**What they're looking for:** Comfort with data, asking the right question, honest analysis (including limitations), and acting on the result.

**STAR template:**
- **S:** [The decision and the competing opinions]
- **T:** [The question you needed to answer]
- **A:** [What data you collected, how you analysed it, caveats]
- **R:** [Decision and outcome]
- **L:** [What you learned]

**Example (adapt to your own experience):**
"Our team debated whether to spend a quarter on performance or on new connectors, and opinions were split. I pulled support tickets, churn reasons from the CRM, and product analytics. Performance complaints came from 5% of accounts, but those were 40% of revenue. Connector requests were broad but mostly from small trials that rarely converted. I presented the analysis with its limitations — churn reasons were self-reported and noisy. We prioritised performance for the top accounts first. Two at-risk accounts renewed that quarter, and we picked up connectors the following quarter."

**Pitfalls:**
- Data used to justify a decision already made.
- Ignoring the limitations of the data.

## Prioritisation & pressure

### Tell me about a time you had to juggle multiple projects or customers.

**What they're looking for:** A system for prioritising, transparent communication with each stakeholder, and not dropping balls. FDEs often support several customers at once.

**STAR template:**
- **S:** [The projects/customers and their demands]
- **T:** [What made it hard]
- **A:** [How you prioritised (impact, urgency, commitments), time-boxed, communicated trade-offs, delegated]
- **R:** [Outcome for each]
- **L:** [Your system now]

**Example (adapt to your own experience):**
"For one quarter I supported three customers: one in go-live, one in pilot, and one in renewal negotiation. I ranked work by business impact and hard dates: the go-live got mornings, the pilot got two focused afternoons a week, and the renewal customer got a weekly check-in plus fast response to blockers. I kept a single board of all commitments, and every Friday sent each customer a short status with what was next. When the pilot customer asked for a large new feature, I was explicit that it would land after the go-live and offered a workaround. All three outcomes landed: on-time go-live, pilot converted, renewal signed."

**Pitfalls:**
- "I just worked longer hours."
- No mention of how you communicated trade-offs.

### Tell me about a time you worked under extreme pressure.

**What they're looking for:** Composure, clear thinking, and sustainable behaviour — not heroics that burn out the team.

**STAR template:**
- **S:** [Source of pressure: incident, deadline, executive visibility]
- **T:** [What was required]
- **A:** [How you stayed calm, focused on the most important thing, organised others, communicated]
- **R:** [Outcome]
- **L:** [What you do to handle pressure]

**Example (adapt to your own experience):**
"Two days before a customer's public product launch, which depended on our API, a load test showed our service failing at a third of their expected traffic. Their CTO was on every call. I split the problem: one teammate profiled the database, I worked on the API layer, and I gave the customer a short update every four hours so they weren't calling us constantly. We found an N+1 query and a missing connection-pool limit, fixed both, added caching for a hot read path, and re-ran the load test at twice the expected traffic. The launch went smoothly. What helped most was dividing work clearly and setting a communication rhythm so we could focus."

**Pitfalls:**
- Glorifying all-nighters as the method.
- Sounding panicked in the retelling.

### How do you prioritise when everything is urgent?

**Framework (not STAR, but back it with a short example):**
1. **List everything** in one place so you can see it.
2. **Clarify real urgency and impact**: what happens if this waits a day? Who is affected and how much money, risk or trust is at stake? Is there a hard external date?
3. **Order by impact × urgency**, with production issues affecting customers and security first; then committed deadlines; then everything else.
4. **Make trade-offs explicit**: tell stakeholders what you are not doing and when you will get to it. If two senior stakeholders conflict, escalate to whoever owns the priority rather than deciding silently.
5. **Protect focus**: finish the top item rather than making progress on five.

**Example (adapt to your own experience):**
"On one Monday I had a customer data bug, a demo for a prospect on Wednesday, and an internal feature due Friday. I asked each requester what happened if it waited. The data bug was corrupting new records, so it went first. The demo had a fixed date with revenue attached, so it came second. I told the feature's PM it would slip to the following Tuesday, and why. Everything that mattered landed, and nobody was surprised."

**Pitfalls:**
- "I just work harder" or trying to do everything at once.
- Never involving stakeholders in the trade-off.

### Tell me about a time you had to say no to work.

**What they're looking for:** That you protect focus and commitments, and say no with reasons and alternatives, not just to avoid work.

**STAR template:**
- **S:** [The request and your current load]
- **T:** [Why accepting would hurt something more important]
- **A:** [How you explained, offered alternatives (later date, smaller scope, someone else)]
- **R:** [Outcome]
- **L:** [How you decide what to decline]

**Example (adapt to your own experience):**
"While I was leading a customer go-live, a director asked me to build a custom demo for a large prospect the same week. I explained that taking it on would put the go-live date at risk, and that the go-live customer was worth more and had a contract commitment. I offered two alternatives: I could adapt an existing demo in half a day, or a colleague who'd built a similar demo could take it with my input. He chose the colleague. The go-live landed on time and the demo went well."

**Pitfalls:**
- Saying no without an alternative.
- Saying no to something that clearly mattered more than what you were doing.

### Tell me about a time you handled a production incident.

**What they're looking for:** Structured incident response: triage severity, mitigate first, communicate, find root cause, write a blameless postmortem with action items.

**STAR template:**
- **S:** [What broke and the impact]
- **T:** [Your role: incident commander, responder, customer liaison]
- **A:** [Detect → assess severity → mitigate (rollback, failover, feature flag) → communicate on a cadence → root cause → postmortem]
- **R:** [Time to mitigate, customer impact]
- **L:** [Systemic fixes]

**Example (adapt to your own experience):**
"On a Saturday, alerts showed a customer's document-processing queue backing up — 20,000 documents stuck, and their claims team needed them Monday morning. I was on call, so I declared an incident and took incident commander. Within 15 minutes we saw that a third-party OCR provider was returning errors. Mitigation first: I switched the pipeline to our fallback OCR provider via a config flag, accepting slightly lower quality, and the queue started draining. I posted customer updates every 30 minutes through their account manager. The backlog cleared by Sunday morning. In the postmortem we added automatic failover on provider error rate and an alert on queue age, not just queue size."

**Pitfalls:**
- Debugging the root cause while customers are still down instead of mitigating first.
- Leaving out communication.

## Communication

### Tell me about a time you explained something technical to a non-technical audience.

**What they're looking for:** Can you translate complexity into what matters to the audience — decisions, risks, costs — without condescension? FDEs do this weekly with customer executives.

**STAR template:**
- **S:** [Audience and what they needed to understand]
- **T:** [The decision or action that depended on it]
- **A:** [How you tailored it: analogy, visuals, focusing on impact, checking understanding]
- **R:** [What they decided/did]
- **L:** [Your technique]

**Example (adapt to your own experience):**
"A customer's COO had to decide whether to approve budget for moving their reporting from nightly batch to near-real-time streaming. Instead of explaining Kafka and CDC, I framed it around what he cared about: today, his warehouse managers act on yesterday's numbers; with streaming, they'd see issues within minutes, at about three times the infrastructure cost. I used a one-page diagram with two timelines showing when a stock-out would be noticed under each option, and gave a middle option — hourly updates — at much lower cost. He picked hourly, and said it was the first time someone had made the trade-off clear to him."

**Pitfalls:**
- Dumbing it down so much that it becomes inaccurate.
- Not checking whether the audience understood.

### Tell me about a time miscommunication caused a problem.

**What they're looking for:** Ownership of your part, how you resolved it, and the process change that prevents it recurring.

**STAR template:**
- **S:** [What was miscommunicated and between whom]
- **T:** [The impact]
- **A:** [How you discovered it, fixed the immediate issue, owned your part]
- **R:** [Outcome]
- **L:** [Process change: written specs, confirming understanding, examples]

**Example (adapt to your own experience):**
"A customer asked for a report of 'active users'. I built it using users who had logged in within 30 days. When they reviewed it, the numbers were far lower than expected — to them, 'active' meant anyone with an active licence. We'd lost a week. I fixed the definition in a day and, more importantly, changed how I take requirements: for any metric, I now write the exact definition and a worked example with real numbers in the spec, and get the customer to confirm it before building. That small step has caught several similar mismatches since."

**Pitfalls:**
- Making it entirely the other person's fault.
- No preventive change.

### Tell me about a presentation you gave that went well / badly.

**What they're looking for:** Self-awareness about communication, preparation habits, and learning from what didn't work. A story that went badly then better is often strongest.

**STAR template:**
- **S:** [Audience and purpose]
- **T:** [What you needed them to take away or decide]
- **A:** [How you prepared and delivered; what went well or wrong]
- **R:** [Outcome]
- **L:** [What you changed]

**Example (adapt to your own experience):**
"I presented a pilot readout to a customer's leadership team and it went badly: I spent the first 15 minutes on architecture, and the CFO interrupted to ask whether the pilot had saved money. I didn't have that number on a slide. For the follow-up two weeks later, I restructured it: the first slide was the answer — hours saved, cost per document, recommendation to expand — followed by risks and next steps, with architecture in the appendix. I rehearsed with our account executive playing the CFO. The second meeting ended with approval to expand. I now always start with the conclusion and the decision I need."

**Pitfalls:**
- Only telling a story where everything went perfectly.
- Not drawing a concrete lesson.

### Tell me about a time you wrote documentation that made a difference.

**What they're looking for:** That you value writing as leverage, and that your docs had measurable effects (fewer questions, faster onboarding, fewer incidents).

**STAR template:**
- **S:** [The knowledge gap and its cost]
- **T:** [What you wrote and for whom]
- **A:** [How you structured it, tested it with readers, kept it current]
- **R:** [Measured effect]
- **L:** [What makes docs useful]

**Example (adapt to your own experience):**
"Every customer deployment of our platform depended on one senior engineer, because the steps lived in his head and old Slack threads. I shadowed him on one deployment and wrote a deployment runbook: prerequisites checklist, step-by-step commands, common errors with fixes, and a verification checklist at the end. I then had a new engineer use it on a real deployment while I watched silently, and fixed every place she got stuck. Deployment time for new engineers dropped from about a week to two days, and the senior engineer was freed up for harder problems. Keeping the 'common errors' section updated after every deployment kept it alive."

**Pitfalls:**
- Docs nobody used.
- No evidence of impact.

## Values, ethics & culture

### Tell me about a time you faced an ethical dilemma at work.

**What they're looking for:** That you recognise ethical issues (privacy, misleading claims, safety, fairness), raise them through proper channels, and act with integrity even when it is costly.

**STAR template:**
- **S:** [The situation and the competing pressures]
- **T:** [The dilemma, stated clearly]
- **A:** [How you thought it through, who you consulted (manager, legal, security), what you did]
- **R:** [Outcome, including any cost]
- **L:** [Your principles]

**Example (adapt to your own experience):**
"During a deployment, a customer asked us to load production data containing customer personal information into our shared dev environment so we could debug faster. It would have saved days, and the customer contact was pushing hard. But it would have breached our data-processing agreement and put real people's data at risk. I declined, explained why, and escalated to my manager and our security team the same day so the decision wasn't mine alone. We agreed on an alternative: debugging in the customer's own environment with masked samples, which took two extra days. The customer's security team later thanked us for not taking the shortcut."

**Pitfalls:**
- A "dilemma" with an obvious answer and no cost.
- Acting unilaterally without involving the right people.

### Tell me about a time you pushed back on something you thought was wrong.

**What they're looking for:** Courage plus judgment — you speak up clearly, with evidence, through the right channels, and accept the outcome or escalate if it's serious.

**STAR template:**
- **S:** [What was being done or planned]
- **T:** [Why you thought it was wrong]
- **A:** [How you raised it: evidence, alternatives, appropriate escalation]
- **R:** [Outcome]
- **L:** [When and how you push back]

**Example (adapt to your own experience):**
"Our team was about to tell a customer that our model's accuracy was 95%, based on an internal benchmark. I had run it on the customer's own documents, and it was closer to 82%. I raised it with the account lead and product manager before the meeting, showing both results and explaining why the benchmark wasn't representative. There was pressure to keep the higher number because the deal was close. I suggested presenting both numbers with a plan to close the gap. We did, and the customer appreciated the honesty and signed a pilot with clear accuracy targets — which we then hit."

**Pitfalls:**
- Pushing back on something minor and calling it principle.
- Being self-righteous; not acknowledging the other side's pressures.

### What does integrity mean to you at work?

**Framework:** Give a short definition, two or three concrete behaviours, and a brief example.
- **Definition:** Doing what I say I'll do, telling the truth even when it's inconvenient, and behaving the same whether or not anyone is watching.
- **Behaviours:** (1) Accurate status reporting — no "green" dashboards that are really red. (2) Honest claims to customers about what the product can do. (3) Owning my mistakes quickly. (4) Protecting customer data and trust even when shortcuts are tempting.

**Example (adapt to your own experience):**
"For me integrity is mostly visible in small things. In a customer pilot, I noticed our dashboard was counting retried jobs as successes, which made our success rate look better than it was. Nobody else had noticed. I fixed the metric and told the customer that the real number was a few points lower, along with what we were doing about it. That kind of honesty is what lets a customer trust everything else you tell them."

**Pitfalls:**
- An abstract answer with no behaviours or example.
- Sounding preachy.

### How do you think about the societal impact of the technology you build? (Palantir, Anthropic, OpenAI)

**Framework:** Show that you've genuinely thought about it, can hold both benefits and risks, and translate concern into concrete practices — not just slogans. Tailor to the company's stated mission (read it beforehand).
1. **Benefits are real**: the reason to build is to help people do important work better.
2. **Risks are real**: misuse, privacy, bias, over-reliance, concentration of power, job displacement.
3. **Your role as an engineer**: you are close to how technology is actually used, so you can build in safeguards — access controls, audit logs, human review, evaluation for bias, clear limitations.
4. **Where you draw lines**: you'd raise concerns through proper channels and wouldn't build something you believed was clearly harmful.

**Example (adapt to your own experience):**
"I think the people deploying technology have a lot of influence over its impact, because that's where abstract capabilities meet real decisions. When I've built tools that affect people — for example, a system that prioritised loan reviews — I've pushed for audit logging, human override, and checking error rates across customer groups before launch. I'm excited about AI because it can make expert-level help much more accessible, and I want to work somewhere that takes the risks seriously enough to slow down when needed. That's part of why [company]'s approach to [specific mission/policy] appeals to me."

**Pitfalls:**
- "Technology is neutral; it's up to users."
- Doom with no constructive view, or being unaware of the company's specific stance.

### Would you be comfortable working with government/defense customers? (Palantir, Scale, Anduril)

**Framework:** Answer honestly — this is a values-fit question and a mismatch is bad for both sides. If yes, explain *why* in your own words and show you've thought about the responsibilities involved. If you have limits, state them clearly and calmly. Know the company's customer base before the interview.
- Why it can matter: governments provide services and security that millions rely on; good software can make them more effective, accountable and efficient.
- Responsibilities: lawful use, oversight, access controls, auditability, and willingness to raise concerns.
- Practicalities: you may need security clearance, citizenship requirements, travel, and strict data-handling rules.

**Example (adapt to your own views):**
"Yes. I think public institutions — including defence — are going to use software either way, and I'd rather they use well-built systems with strong access controls and auditability, built by people who care about how they're used. I'm comfortable with the constraints that come with it, like clearance processes and restricted environments. I'd also want to work somewhere with clear internal processes for raising concerns, and I'd use them if I saw something I believed was wrong. I've read [company]'s published principles on this and they align with how I think about it."

**Pitfalls:**
- Saying yes when you don't mean it — you'll be unhappy and they'll notice.
- A vague answer that suggests you haven't thought about it.

### What's your view on responsible AI deployment?

**Framework:** Show a practical, engineering-level view rather than abstract principles. Organise around the deployment lifecycle:
1. **Before:** define the use case and who is affected; evaluate on representative data, including failure modes and performance across groups; red-team for misuse.
2. **During:** human-in-the-loop for high-stakes decisions; confidence thresholds and fallbacks; clear communication of limitations to users; privacy and data minimisation; access controls.
3. **After:** monitoring for drift and errors; feedback channels; incident response; audit logs; willingness to roll back.
4. **Proportionality:** the level of safeguards should match the stakes — a writing assistant and a medical triage tool need different controls.

**Example (adapt to your own experience):**
"I think of responsible deployment as mostly concrete engineering work. On an LLM document-extraction project, we built an evaluation set from the customer's real documents, measured errors by document type, and routed low-confidence outputs to human review instead of auto-processing them. We logged every model decision so the customer could audit it, and we told users clearly which fields were AI-generated. For high-stakes uses I'd go further — staged rollouts and explicit sign-off from the people accountable for outcomes. The model is only one part; the system around it decides whether it's safe."

**Pitfalls:**
- Only buzzwords ("fairness, transparency, accountability") with no practices.
- Dismissing risks, or being so cautious you seem unwilling to ship anything.

### What kind of culture do you thrive in?

**Framework:** Be honest and specific, and connect it to what you know about the company (research it). Name 2–3 traits, each with a short reason or example. Avoid describing a culture that's the opposite of theirs.

**Example (adapt to your own experience):**
"I do best in three kinds of environment. First, high ownership — where I'm trusted with a problem, not just a ticket, and accountable for the outcome. Second, direct and kind feedback — I improve fastest when people tell me plainly what's not working. Third, close to the customer — I'm most motivated when I can see the people using what I build. From talking to [interviewer/team] and reading about how your FDEs work, it sounds like small teams own customer outcomes end to end, which is exactly what I'm looking for."

**Pitfalls:**
- Generic answers ("collaborative and innovative").
- Describing a culture that conflicts with how the company actually works (e.g. wanting stability at a fast-moving startup).

## Self-reflection

### What are your strengths?

**What they're looking for:** Self-awareness and fit — strengths relevant to the FDE role, each backed by evidence.

**Framework:** Pick 2–3 strengths that match the role (e.g. customer-facing problem solving, learning new stacks fast, shipping end to end, clear communication). For each: name it → one-line evidence → why it matters for this job.

**Example (adapt to your own experience):**
"Three things. First, turning vague problems into something shippable: on my last customer project, I went from 'use AI to cut claim denials' to a scoped tool in production within a month. Second, getting productive quickly in unfamiliar environments — I've shipped integrations into SAP, a Java monolith and a customer's Kubernetes cluster, each within a few weeks. Third, communicating with non-technical stakeholders: customers' leadership teams regularly ask me to present the technical side because I keep it focused on decisions. Those three are most of what an FDE does day to day."

**Pitfalls:**
- A list of adjectives with no evidence.
- Strengths irrelevant to the role.

### What are your weaknesses / areas for growth?

**What they're looking for:** Honest self-awareness and a real improvement plan. They are testing maturity, not looking for a flaw-free person.

**Framework:** Real weakness (not a disguised strength) → concrete example of its impact → what you are actively doing about it → evidence of progress. Pick something genuine but not disqualifying for the role (don't say "I dislike talking to customers").

**Example (adapt to your own experience):**
"I tend to take on too much myself rather than delegating or asking for help early, especially when a customer is under pressure. On one project, I held on to a data-pipeline task for too long and it became a bottleneck that a teammate could have unblocked in a day. What I'm doing about it: I now set a rule that if I'm stuck for more than half a day, I ask someone, and at the start of each project I explicitly split work with teammates. My last project lead noted in my review that I'd delegated well on a multi-customer rollout, but it's still something I watch."

**Pitfalls:**
- "I'm a perfectionist" / "I work too hard."
- A weakness that is a core requirement of the job, with no plan.

### How would your manager / peers describe you?

**What they're looking for:** Self-awareness, and consistency with what references will say. Reference checks (and informal back-channels) are common, especially at startups, so be accurate.

**Framework:** Use real feedback — quote performance reviews or specific comments where possible. Give 2–3 traits, and include one growth area to show honesty.

**Example (adapt to your own experience):**
"My last manager described me in my review as 'the person you send when a customer situation is messy' — I'm calm, I dig in, and I communicate clearly. Peers have said I'm generous with time and that my runbooks save them work. One piece of feedback I've had is that I can move ahead before everyone's aligned, so I've been more deliberate about writing a short plan and getting sign-off before big changes."

**Pitfalls:**
- Generic praise with no source.
- Something that contradicts what your references will say.

### What motivates you?

**What they're looking for:** Whether your motivations match the role: customer impact, hard problems, variety, ownership. FDE work involves ambiguity, travel and context-switching, so motivation needs to fit that.

**Framework:** 1–2 genuine motivators → a short story that shows it → link to this role.

**Example (adapt to your own experience):**
"I'm most motivated by seeing someone's work change because of something I built. The moment that stuck with me was a technician telling us our offline form app 'gave him his evenings back'. I'm also motivated by learning — I like landing in a new domain, whether insurance or logistics, and understanding it well enough to make a difference. Forward deployed engineering combines those two things: real users, hard problems, and new domains all the time."

**Pitfalls:**
- Only money or title.
- Motivations that clash with the role ("I like deep, uninterrupted focus on one codebase for years").

### What's something you're curious about outside of work?

**What they're looking for:** You as a person, and curiosity as a trait — ideally showing how you learn.

**Framework:** Pick something genuine → what specifically interests you → how you pursue it → (optional) a light link to how you think. Be ready for follow-up questions, so choose something you actually know about.

**Example (adapt to your own experience):**
"I've been getting into urban planning — specifically why some cities have good public transport and others don't. I read a lot about it, and I've started playing with open transit data to map how long it takes to get to work from different neighbourhoods in my city. What I like is that it's a mix of engineering, economics and politics, and the 'obvious' technical answer is rarely the one that gets built — which is a bit like customer work."

**Pitfalls:**
- Something you can't talk about for two minutes.
- Forcing a work connection that sounds rehearsed.

### What would you do in your first 90 days here?

**What they're looking for:** A structured, humble but action-oriented plan — learn fast, deliver an early win, then increase scope. Shows you understand how FDEs ramp up.

**Framework (30/60/90):**
- **Days 1–30 — Learn:** product deeply (build something with it), the deployment playbook, shadow senior FDEs on customer calls, meet sales/product/support, understand how success is measured. Ship a small fix or improvement to get through the toolchain.
- **Days 31–60 — Contribute:** take ownership of a defined workstream on a customer deployment; deliver a visible win for the customer; start a log of recurring friction points.
- **Days 61–90 — Own:** own a customer or significant deployment end to end; feed patterns back to product (e.g. one reusable tool or a written proposal); get feedback from my manager on what to adjust.

**Example (adapt to your own experience):**
"In the first month I'd focus on learning the product hands-on and shadowing deployments, and I'd aim to ship one small improvement so I learn the toolchain. In the second month I'd want to own a piece of a live customer deployment and deliver something the customer notices. By day 90 I'd like to be the primary engineer on a customer and to have brought back at least one concrete product insight or reusable tool from the field. I'd check in with my manager at 30 and 60 days to make sure I'm focused on what matters most to the team."

**Pitfalls:**
- Promising to change everything in the first month.
- A plan with no customer contact, or only "learn" with no delivery.

### Why should we hire you?

**What they're looking for:** A concise, confident pitch that maps your evidence to their needs. This is your closing argument.

**Framework:** Three parts, about 60 seconds:
1. **Their need:** what the role requires (from the job description and conversations).
2. **Your evidence:** 2–3 matching strengths, each with a one-line proof point.
3. **Motivation:** why this company and role specifically.

**Example (adapt to your own experience):**
"From our conversations, you need someone who can go into a customer with a vague problem, build something that works in their environment, and bring lessons back to the product. That's what I've done for the last few years: I took a logistics customer from a manual two-day reconciliation process to 30 minutes, shipped integrations into stacks I'd never seen before within weeks, and turned repeated customer work into tools the whole team uses. I'm also genuinely excited about [company]'s [product/mission], because [specific reason]. I'd be productive quickly and I'd make the team around me more effective."

**Pitfalls:**
- Generic claims ("I'm a hard worker and a fast learner").
- Not tailoring to this specific role and company.

### Is there anything else you'd like us to know?

**What they're looking for:** A last chance to fill gaps. Use it — don't just say "no."

**Framework:** Pick one of these:
- **Fill a gap:** a strength or story that hasn't come up yet but is relevant.
- **Clarify a weak answer:** briefly correct or strengthen something you answered poorly.
- **Restate fit and enthusiasm:** one or two sentences, then stop.

**Example (adapt to your own experience):**
"One thing we didn't cover is that I've done a fair amount of on-site customer work — I spent six weeks embedded with an insurance customer — so the travel and on-site parts of this role are something I enjoy rather than tolerate. And I want to say I'm really excited about this role; the conversations today made me more confident it's the right fit."

**Pitfalls:**
- "No, I think we covered everything."
- Starting a long new story or raising a new concern at the last minute.

## Hiring-manager round topics

### Deep dive on one project from your resume: architecture, your role, trade-offs, what you'd change.

**What they're looking for:** Real depth and real ownership. The hiring manager will probe until they find the edge of your knowledge, so choose a project you know inside out and can draw on a whiteboard.

**Framework (about 5–10 minutes, with room for questions):**
1. **Context (1 min):** customer/business problem, users, scale, constraints, team size.
2. **Your role:** exactly what you owned vs. what others did.
3. **Architecture:** draw the components and data flow; name technologies and why.
4. **Key decisions and trade-offs:** 2–3 decisions, alternatives considered, why you chose what you did.
5. **Hard parts:** the toughest problem and how you solved it.
6. **Results:** metrics.
7. **What you'd change:** an honest retrospective — this is the part many candidates skip.

**Example (adapt to your own experience):**
"I'll take the claims-document pipeline for an insurance customer. Problem: adjusters manually keyed data from about 5,000 documents a day. I was the lead engineer on a team of three and owned the extraction pipeline and the customer integration. Architecture: documents landed in S3, an SQS queue fed workers on ECS that ran OCR and then LLM-based extraction with a JSON schema; results went to Postgres, low-confidence fields went to a review UI, and approved data was pushed into their claims system via its REST API. Key trade-offs: we chose an LLM API over training a custom model because document types varied and we had little labelled data — accepting higher per-document cost for speed and flexibility; and we used confidence-based human review rather than full automation to keep error rates acceptable. The hardest part was evaluation: we built a 600-document labelled set and ran it on every prompt change. Results: 70% of documents processed with no human touch, and processing time per claim down from 20 minutes to 4. What I'd change: I'd add per-field cost and latency tracking from day one — we found out late that two rare document types caused most of our LLM spend — and I'd design the review UI with adjusters earlier."

**Pitfalls:**
- Not being able to go deeper when probed ("someone else handled that").
- Skipping the "what I'd change" part, or having no criticism of your own work.

### What kind of manager brings out your best work?

**Framework:** Be honest and specific; describe behaviours, not personality. Show you can work with a range of styles and that you take responsibility for the relationship (e.g. telling a manager what you need).

**Example (adapt to your own experience):**
"I do my best work with a manager who gives clear context on why something matters and what success looks like, then gives me room to figure out the how. I value direct feedback, given soon — I'd rather hear something hard on Tuesday than in a review six months later. I also appreciate a manager who will back me up in tough customer situations and help me see the bigger picture across accounts. When a manager's style is different, I adapt: for example, with a more hands-on manager I've offered more frequent written updates so they had visibility without needing to be in every detail."

**Pitfalls:**
- Describing a manager who never gives direction or feedback ("leaves me alone").
- Criticising past managers.

### How do you handle long travel stretches / on-site work?

**What they're looking for:** Realistic understanding of the role's demands, evidence you've done it (or similar), and that you have habits that make it sustainable. Be honest about constraints — it is better to clarify now.

**Framework:** (1) Honest stance on travel. (2) Relevant experience. (3) How you stay effective: routines, health, staying connected to your team. (4) Why you value on-site work. (5) Any real constraints, stated plainly.

**Example (adapt to your own experience):**
"I'm comfortable with it, and I actually find on-site time is where the most valuable work happens — you learn things in a day sitting with users that you'd never get from calls. On my last role I spent about six weeks on-site with a customer over two months. What keeps it sustainable for me is routine: I keep regular exercise and sleep habits on the road, block time for deep work in the evenings or early mornings, and do a short daily sync with my home team so I'm not isolated. I'd want to understand your typical travel pattern — how many weeks a quarter — so I can plan around family commitments."

**Pitfalls:**
- Saying "no problem at all" when it would be a problem.
- No evidence or habits — just enthusiasm.

### How do you keep code quality high when you're moving fast for a customer?

**What they're looking for:** Practical engineering judgment — what you never compromise on, what you defer, and the lightweight practices that keep speed sustainable.

**Framework:**
- **Non-negotiables:** correctness of data, security and access control, secrets handling, ability to roll back, basic observability (logs, errors).
- **Lightweight practices:** tests on critical paths and data transformations (not 100% coverage); typed interfaces (type hints, schemas like Pydantic) at boundaries; linting and formatting in CI; small PRs with quick peer review; feature flags; infrastructure as code.
- **Design for change:** isolate customer-specific code behind clear interfaces/config so it doesn't leak into the core product.
- **Track debt:** write down shortcuts as tickets with context; schedule paydown after the deadline.
- **Productise repeats:** when the same customization appears for the third customer, generalise it.

**Example (adapt to your own experience):**
"On a recent deployment we shipped weekly for a customer. I kept a short rule set: every data transformation had a test with real sample data, all inputs were validated with Pydantic schemas, and nothing deployed without CI passing and a second reviewer, even if that review was 10 minutes. Customer-specific logic went into a plugin module, not the core codebase. Where we cut corners — for example, a manual step in the deployment — I logged it as a ticket. After go-live we spent one week paying down the top items. We had no data-correctness incidents during the project."

**Pitfalls:**
- "I never cut corners" (unrealistic) or "speed always wins" (risky).
- Generic advice with no concrete practices.

### Business use-case discussion: how would you apply our product at company X?

**What they're looking for:** Commercial thinking, understanding of their product, and the FDE skill of mapping a capability to a customer's workflow and value. Treat it like a mini customer-discovery session — ask questions before proposing.

**Framework:**
1. **Clarify:** What does company X do? Who are the users? What are their biggest costs, risks or bottlenecks? What data and systems do they have?
2. **Pick a high-value workflow:** choose 1–2 use cases where the product's strengths meet a painful, measurable problem. Explain why this one first (value, feasibility, data availability, champion).
3. **Solution sketch:** how the product fits in — data sources, integrations, users, human-in-the-loop, security/compliance needs.
4. **Plan:** v0 pilot in 4–8 weeks with a narrow scope → v1 expansion.
5. **Risks:** data quality, integration access, change management/adoption, regulatory issues.
6. **Success metrics:** business metrics (cost, time, revenue, risk) plus adoption metrics.
7. **Expansion:** where it goes next if the pilot works.

**Example (adapt to the company and product):**
*Prompt: "How would you apply an LLM platform at a large regional hospital network?"*
"I'd first ask about their biggest operational pain points, but a likely high-value area is clinical documentation and prior authorisation — clinicians spend hours a day on paperwork, and authorisation delays hold up care and revenue. I'd start with prior authorisation because it's measurable and lower risk than anything diagnostic. The v0: pull the relevant notes and orders from the EHR via its FHIR API, use the model to draft the authorisation request against each payer's criteria, and have a staff member review and submit it — the human stays accountable. Key requirements: HIPAA compliance, a BAA with the vendor, data staying in their approved environment, and audit logs. Pilot with one department and two payers for six weeks. Metrics: staff time per request, turnaround time, approval rate on first submission, and staff adoption. Main risks: EHR integration access, variation in payer rules, and staff trust — so I'd involve the authorisation team from week one. If it works, expand to more payers, then to discharge summaries."

**Pitfalls:**
- Jumping to a solution without asking clarifying questions.
- Proposing something the product can't do, or ignoring compliance, data access and adoption.
- No success metrics.
