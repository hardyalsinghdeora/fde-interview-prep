# 09 — Customer Scenarios & Role-Play: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers, not scripts to memorise. Adapt the examples to your own experience and practise saying the sample phrasing out loud, ideally with someone playing the customer. Each answer gives: what the interviewer is testing, a step-by-step approach, and sample phrasing (the actual words you would say).

---

## A. Scoping & requirements

### A customer comes to you with a vague request: "we want to use AI to improve operations." What do you do in the first meeting?

**What they're testing:** whether you resist jumping to a solution, can steer an open conversation toward a concrete, valuable, feasible problem, and leave with next steps.

**Approach:**
1. **Prepare:** read their annual report/website, know their industry's top cost drivers, find out who is in the room and why.
2. **Anchor on outcomes, not AI:** ask which metrics leadership cares about this year (cost per X, cycle time, error rate, revenue leakage).
3. **Walk the operation:** ask them to describe a day in the life of the team with the most pain. Look for high-volume, repetitive, text/document-heavy, rules-plus-judgement work.
4. **Build a candidate list** of 3-5 use cases live, and score each roughly on value, feasibility (is the data there?), and risk.
5. **Probe data and constraints:** where does the data live, who owns it, any regulatory limits, what has been tried before.
6. **Close with a concrete next step:** pick 1-2 use cases for a deeper discovery session, name owners, ask for sample data and access to an end user.
7. **Follow up in writing** within 24 hours with a summary and the shortlist.

**Sample phrasing:**
> "Before we talk about AI at all, help me understand where operations hurts most today. If you could fix one number on your dashboard by the end of the year, which would it be?"
>
> "Walk me through what happens from the moment an order comes in to when it's closed. Where do people wait, re-key data, or make judgement calls?"
>
> "From what I've heard, three areas stand out: invoice exceptions, support ticket triage, and supplier email handling. Invoice exceptions looks highest-value and you already have five years of data. Could we spend an hour next week with someone from AP and look at 50 real examples?"

**Pitfalls:** pitching a product demo in the first 10 minutes; promising results; leaving without an owner and a date.

### What questions do you ask in a discovery call?

**What they're testing:** structured curiosity, and whether you cover business, users, data, technical environment, and buying process, not just tech.

**Approach:** group questions so you don't miss a dimension, and listen more than you talk (aim for 70/30).

- **Problem and impact:** What problem are you trying to solve? How do you handle it today? What does it cost (time, money, risk)? Why now?
- **Success:** If this works perfectly, what is different in 6 months? How would you measure it? What is "good enough"?
- **Users and workflow:** Who does this work today? How many people, how often? What tools do they use? Where would the output need to show up?
- **Data:** What data exists, where, in what format, how much, how clean? Who owns it? Is there labelled history (past decisions/outcomes)?
- **Technical environment:** Cloud/on-prem? Identity provider (SSO)? Integration points (APIs, databases, ERP/CRM)? Security and compliance requirements (SOC 2, HIPAA, GDPR, data residency)?
- **Constraints and history:** Budget, deadlines, things tried before and why they failed.
- **People and process:** Who sponsors this? Who decides? Who needs to sign off (security, legal, procurement)? Who will use and maintain it?

**Sample phrasing:**
> "What's happening today that made this a priority now, rather than last year?"
>
> "If we fast-forward six months and this has been a big success, what would you be telling your boss?"
>
> "Can you show me, on screen, how someone does this today? I'd rather see it than hear it described."
>
> "Who else will need to say yes before this goes live: security, legal, procurement?"

**Pitfalls:** only asking technical questions; asking leading questions ("you'd want real-time, right?"); not asking about previous failed attempts.

### How do you turn a business problem into technical requirements?

**What they're testing:** translation skill: from fuzzy goal to testable, buildable spec with clear inputs, outputs, and constraints.

**Approach:**
1. **State the business outcome as a metric:** "Reduce average claim processing time from 5 days to 2."
2. **Map the current workflow** step by step and find where time/cost goes. Pick the step(s) the solution will change.
3. **Define the system boundary:** inputs (data sources, formats, volume), outputs (what, to whom, where), and the decision or action it supports.
4. **Write functional requirements as user stories with acceptance criteria:** "As a claims handler, I see extracted fields pre-filled in the claim form; 95% of fields need no edit."
5. **Add non-functional requirements:** latency, throughput, availability, security (SSO, RBAC, encryption), data residency, audit logging, cost per transaction.
6. **List assumptions and open questions,** and validate them with the customer.
7. **Prioritise** (MoSCoW: must/should/could/won't) and get sign-off.

**Sample phrasing:**
> "Let me play this back. The business goal is cutting processing time to two days. The biggest delay is handlers manually reading PDFs to fill 30 fields. So the system needs to take a claim PDF, extract those fields with confidence scores, and pre-fill your claims system within a minute. Handlers review anything below a confidence threshold. Does that match how you see it?"
>
> "Is there a hard requirement that data stays in the EU, and does it need to work with Okta SSO?"

**Pitfalls:** writing requirements nobody can test ("should be fast"); skipping non-functionals that later block go-live (security, SSO).

### How do you define success criteria with a customer?

**What they're testing:** whether you make success measurable and agreed up front, so you can't be judged on shifting, unspoken expectations.

**Approach:**
1. **Tie to a business metric** the sponsor cares about (hours saved, cost per case, revenue, error rate, CSAT).
2. **Measure the baseline now.** Without a baseline, you cannot show improvement.
3. **Set layered criteria:**
   - Technical: e.g. extraction accuracy >= 92% on a 200-document held-out test set; p95 latency < 3 s.
   - Adoption: e.g. 80% of the pilot team using it weekly.
   - Business: e.g. handling time down 30%.
4. **Agree how and when it's measured** (who builds the test set, what data, what date).
5. **Define the decision it triggers:** "If we hit these, we move to production rollout."
6. **Write it down** in the SOW or a one-page success plan and get the sponsor to sign off.

**Sample phrasing:**
> "So we're all judging this the same way, can we agree on three numbers? Accuracy on a test set your team labels, how many of the pilot users use it weekly, and the change in average handling time. Where are those numbers today?"
>
> "If we hit those targets by the end of the pilot, is that enough for you to take a production proposal to your CFO?"

**Pitfalls:** vague criteria ("users like it"); letting the vendor alone define the test set; no link between success and a commercial decision.

### How do you scope a 2-week proof of concept (POC)?

**What they're testing:** ruthless scoping, de-risking the biggest unknown, and protecting a short timeline.

**Approach:**
1. **Pick one question the POC must answer:** usually "can this approach hit X quality on your real data?" Not "build the product."
2. **Choose one use case, one data source, one user group.**
3. **Agree success criteria and the test set on day 1.**
4. **Front-load dependencies:** data access, credentials, sandbox environment, named customer contact. Make these preconditions; the clock starts when they're met.
5. **Plan the two weeks:**
   - Days 1-2: data access, explore data, baseline.
   - Days 3-7: build the core pipeline end to end, ugly but working.
   - Days 8-9: evaluate on the test set, iterate on the worst failures.
   - Day 10: demo with real examples, results versus criteria, recommendation and next-phase plan.
6. **Explicitly list out-of-scope items:** SSO, production hardening, UI polish, scale, integrations.
7. **Daily or every-other-day check-ins** so there are no surprises.

**Sample phrasing:**
> "In two weeks we can prove one thing well: whether we can reliably extract the 12 key fields from your supplier contracts. We won't integrate with SAP or build user management; those are phase two. For this to work, I need read access to 300 sample contracts and two hours of a legal analyst's time to label 100 of them by day two. If access slips, the end date slips with it."

**Pitfalls:** trying to show everything; starting without data access; no written out-of-scope list.

### How do you decide what goes into the MVP?

**What they're testing:** prioritisation by value and risk, and the discipline to say no.

**Approach:**
1. **Start from the core job:** the smallest thing that delivers the main outcome to real users end to end.
2. **List all candidate features** and score each on value to the user/sponsor, effort, and risk reduction.
3. **Include must-haves for go-live** even if not glamorous: SSO, audit logs, basic monitoring, a way to correct mistakes. An MVP that security won't approve is not viable.
4. **Cut:** anything that serves edge cases (<5% volume), nice-to-have UI, configurability nobody asked for, automation of rare paths (handle manually).
5. **Prefer a thin vertical slice** (data in, model, UI, feedback) over a deep horizontal layer.
6. **Validate the list with the sponsor and an end user,** and keep a visible "next" list so cuts feel deferred, not denied.

**Sample phrasing:**
> "The MVP is: an analyst uploads a contract, gets the key terms with source highlights, and can correct them. That covers 80% of their weekly work. Bulk upload, custom templates and the Salesforce sync are on the 'next' list. We'll decide on them based on what we see in the first month of usage."

**Pitfalls:** "minimum" ends up meaning "everything the loudest stakeholder asked for"; leaving out non-functional must-haves.

### How do you find the real problem behind what the customer says they want?

**What they're testing:** the ability to separate a requested solution from the underlying need ("they ask for a faster horse").

**Approach:**
1. **Ask "why" a few times** (the 5 Whys), politely: what will that let you do?
2. **Ask for a recent concrete example:** "Tell me about the last time this hurt."
3. **Observe the work directly:** shadow a user, look at the actual spreadsheets and tickets.
4. **Quantify:** how often, how many people, what it costs. Real problems have numbers.
5. **Talk to multiple roles:** the executive, the manager, the end user. They often describe different problems.
6. **Play back the problem** (not the solution) and confirm.

**Sample phrasing:**
> Customer: "We need a dashboard of all our support tickets."
> You: "Happy to look at that. What decision would you make with the dashboard that you can't make today?"
> Customer: "We'd see which issues are growing."
> You: "And when you spot one growing today, what happens?"
> Customer: "Usually we find out a week late, from an angry enterprise customer."
> You: "So the real problem is detecting emerging issues early, before key accounts escalate. A dashboard might help, but an alert when a new issue cluster spikes might help more. Shall we design for that?"

**Pitfalls:** making the customer feel lectured; ignoring their idea entirely instead of building from it.

### The customer's stated requirements contradict each other. What do you do?

**What they're testing:** spotting conflicts early, making trade-offs explicit, and getting the right person to decide instead of silently picking one.

**Approach:**
1. **Make the conflict concrete and neutral:** write both requirements side by side with their source.
2. **Find the underlying goal of each:** often both can be partly satisfied (e.g. "real-time" really means "within 15 minutes").
3. **Offer options with trade-offs** (cost, time, risk), including compromises.
4. **Recommend one** with your reasoning.
5. **Escalate to the decision owner** if the stakeholders can't agree.
6. **Document the decision** and the reason.

**Sample phrasing:**
> "I want to flag a tension before we build. The ops team asked for every result to be reviewed by a human, and the business case assumes 70% fewer handling hours. We can't fully have both. Three options: review everything (safest, smaller savings), review only low-confidence results (about 60% savings, small risk), or sample 10% for audit (biggest savings, more risk). I'd recommend the second option for the pilot and revisit after we see the error data. Who should make that call?"

**Pitfalls:** quietly choosing one side; raising the conflict in a way that blames a stakeholder.

### Requirements keep changing mid-project. How do you handle it?

**What they're testing:** change control without being rigid, and understanding why requirements change.

**Approach:**
1. **Expect some change** and design for it: short iterations, frequent demos, a prioritised backlog.
2. **Understand the cause:** new information (good), a new stakeholder, unclear original goal, or politics. Fix the root cause, e.g. get the missing stakeholder involved.
3. **Use a lightweight change process:** each change gets a quick impact estimate (time, cost, risk) and goes to the sponsor to decide: swap something out, extend the timeline, or defer.
4. **Keep a visible change log and backlog,** so everyone sees what moved and why.
5. **Re-anchor on success criteria:** "does this change help us hit the agreed goal?"

**Sample phrasing:**
> "That's a good idea, and I can see why it matters after last week's demo. Adding PDF table extraction is about four extra days. We can swap it for the email integration, push the pilot date by a week, or put it first in phase two. Which works best for you?"
>
> "This is the third change to the output format in two weeks. I want to make sure we're building the right thing. Could we get the finance lead into Thursday's demo so we hear their needs directly?"

**Pitfalls:** saying yes to everything (and missing the deadline) or no to everything (and building the wrong thing).

### How do you estimate a project when you don't know the customer's environment?

**What they're testing:** honest handling of uncertainty: ranges, assumptions, and reducing unknowns fast.

**Approach:**
1. **Break the work into components** (data access, ingestion, core logic/model, integration, security review, UAT, rollout).
2. **Estimate each as a range** (best/likely/worst) based on comparable past projects.
3. **List assumptions explicitly:** "API access exists," "data is in one database," "security review takes under 3 weeks."
4. **Identify the biggest unknowns** (usually access, data quality, security approval, integrations) and propose a short paid discovery or technical assessment (1-2 weeks) to resolve them.
5. **Give a phased estimate:** firm for discovery, a range for the build, refined after discovery.
6. **Add contingency** for customer-side dependencies, which are the most common source of delay.

**Sample phrasing:**
> "Based on similar deployments, this is 8 to 14 weeks. The wide range is mostly about your environment: if the data is accessible through an API and your security review is standard, we'll be near 8; if we need on-prem deployment or a data cleanup, closer to 14. I'd suggest a two-week technical discovery first. At the end of it I'll give you a much tighter number."

**Pitfalls:** a single precise number with hidden assumptions; refusing to estimate at all.

### How do you identify the decision-maker vs. the end user vs. the blocker?

**What they're testing:** stakeholder mapping and political awareness, which matter as much as the code in enterprise deals.

**Approach:**
1. **Map the roles:**
   - **Economic buyer/decision-maker:** controls budget, signs off. Cares about ROI and risk.
   - **Champion:** wants this to succeed internally and sells it for you.
   - **End users:** do the daily work. Care about ease, time saved, not being replaced.
   - **Technical gatekeepers:** IT, security, legal, procurement. Can say no, rarely say yes.
   - **Blockers:** people threatened by the change (an internal team building a competing tool, a manager whose headcount depends on the process).
2. **Ask directly:** "Who else needs to be involved for this to go live?" "Who signed off the last similar project?"
3. **Watch behaviour:** who people look at before answering, who misses meetings, who asks about job impact.
4. **Work with your AE/CSM** who often already knows the org chart.
5. **Engage each one on their terms:** ROI for the buyer, workflow for users, controls and documentation for security, and take blockers' concerns seriously early.

**Sample phrasing:**
> "Walk me through how a decision like this got made last time. Who had to sign off, and who could have stopped it?"
>
> (To a likely blocker) "I'd really value your view on this. You know the process better than anyone. What would worry you about rolling this out?"

**Pitfalls:** assuming the most senior or loudest person decides; ignoring IT/security until the end.

## B. Difficult situations

### The customer wants a feature that our product doesn't support. What do you do?

**What they're testing:** honesty, creative problem-solving, and judgement about what to promise on behalf of the product team.

**Approach:**
1. **Understand the need behind the feature** (why they want it, how often, what happens without it).
2. **Check honestly** whether it is truly unsupported or achievable via configuration, APIs, or an existing integration.
3. **Offer options:** a workaround with existing features; a lightweight extension you build on top (script, connector) with clear ownership; a third-party tool; or logging it as a product request.
4. **Never promise a roadmap date** you don't control. Bring the request to product with business context (deal size, how many customers want it).
5. **Close the loop** with the customer on what product said.

**Sample phrasing:**
> "Today the product doesn't support approval workflows natively, and I don't want to pretend otherwise. Can you tell me what the approval needs to achieve? If it's mainly a manager sign-off before results are sent, we can do that with our webhook and your existing ServiceNow flow; I can sketch it this week. I'll also take the request to our product team with your use case, and I'll come back to you by Friday with what I hear. I won't give you a date until they do."

**Pitfalls:** saying "it's on the roadmap" to close the conversation; building an unmaintainable custom hack without telling anyone.

### The customer asks for something that's technically a bad idea. How do you push back?

**What they're testing:** whether you can disagree respectfully, with evidence, while recognising the customer owns the final decision (within safe limits).

**Approach:**
1. **Understand their reasoning first;** they may know a constraint you don't.
2. **Explain the risk in business terms** (cost, outage, security, maintenance), not "that's bad practice."
3. **Offer a better alternative** that meets their underlying goal.
4. **Use evidence:** a quick prototype, a benchmark, an example from another customer (anonymised).
5. **If they still insist and it's not unsafe or unethical,** document the risk in writing, get explicit acceptance, and proceed. Disagree and commit.

**Sample phrasing:**
> Customer wants to give the AI direct write access to the production database.
> "I understand the appeal; it removes a manual step. My concern is that if the model makes a mistake, it writes straight into your system of record with no undo, and your auditors would ask who approved each change. What if the model writes proposed changes to a staging table, and a person approves them in batch? You keep most of the speed, and every change is traceable. If after a month the error rate is low enough, we can auto-approve the high-confidence ones."
>
> If they insist: "Understood. I'll note in the design doc that we discussed the risk and you chose this approach. I'd suggest we at least keep a full change log so we can roll back."

**Pitfalls:** being condescending; caving silently; refusing without an alternative.

### The customer asks for something unethical or against policy. What do you do?

**What they're testing:** integrity and a clear line, delivered calmly without wrecking the relationship.

**Approach:**
1. **Be clear and early:** don't do it, don't hint you might.
2. **Explain why** briefly, referring to policy, law, or the contract, not personal judgement of the customer.
3. **Look for a legitimate version of their goal** and offer it.
4. **Escalate internally** (manager, legal, trust and safety) and document the request, especially if it's serious (e.g. surveillance of employees without consent, using data beyond its permitted purpose, misleading users, bypassing a model's safety controls, illegal discrimination).
5. **Stay professional;** often the customer hasn't thought it through.

**Sample phrasing:**
> "I can't help with using the call transcripts to profile individual employees' personal lives. That goes beyond what our acceptable-use policy allows and likely beyond what your employees consented to under GDPR. What I can do is help you analyse the calls in aggregate to find training gaps across the team, which I think gets at what you're after. I'll also loop in our legal team so we're aligned on what's in bounds."

**Pitfalls:** going along "just this once"; moralising; failing to escalate and document.

### A customer executive is angry because the deployment is late. Handle the conversation (role-play).

**What they're testing:** composure, empathy without grovelling, ownership, and turning emotion into a concrete plan. In a role-play, they're watching your first 30 seconds closely.

**Approach (LAER: Listen, Acknowledge, Explore, Respond):**
1. **Listen without interrupting.** Let them finish. Take notes.
2. **Acknowledge the impact** on them specifically, and own your part. Don't blame their team in the moment, even if they contributed.
3. **Explore:** ask what matters most now (a date, a partial launch, a board update).
4. **Respond with facts and a plan:** what happened (briefly), the new realistic date, what you're doing to protect it, early warning signals, and what you need from them.
5. **Commit to a follow-up** in writing and a cadence.

**Sample role-play:**
> Exec: "This was supposed to be live two weeks ago. I've told my board it's done. This is embarrassing."
>
> You: "You're right to be frustrated. We committed to the 1st and we missed it, and I understand it's put you in a difficult position with your board. I'm sorry for that. Can I take two minutes to tell you exactly where we are, and then hear what matters most to you?"
>
> "The core system is built and passed testing. What's holding go-live is the SSO integration: we found your Entra ID (Azure AD) setup needs a configuration change that only your identity team can make, and it took longer than we planned for. I should have raised that risk with you earlier, and that's on me."
>
> "Here's the plan. We go live with the 20-person pilot team on the 15th using temporary accounts, so you have real usage to show. Full rollout follows on the 29th, once SSO is done. I need one thing from you: a named contact in your identity team this week. I'll send you a written status every Tuesday until we're live."
>
> "Is there something you'd find useful for the board, like a one-page update or early results from the pilot?"

**Pitfalls:** defensiveness ("your IT team was slow"); over-apologising with no plan; promising a new date you aren't confident in.

### The demo breaks in front of the customer. What do you do?

**What they're testing:** grace under pressure, honesty, and preparation habits.

**Approach:**
1. **Stay calm and name it:** don't pretend it's fine or ramble.
2. **Give it one quick, bounded attempt** (30-60 seconds: refresh, re-run, check connection). Don't debug live for ten minutes.
3. **Switch to your backup:** recorded video, screenshots, a second environment, or a pre-run notebook with outputs.
4. **Keep the meeting's purpose on track:** the conversation about their problem matters more than the click-through.
5. **Follow up:** explain the root cause and, if useful, offer a re-run.
6. **Prevention (mention this):** rehearse, use a stable demo environment, cached responses for external APIs, a backup recording, check Wi-Fi/VPN, avoid deploying just before the demo.

**Sample phrasing:**
> "It looks like the connection to the model endpoint has dropped. Let me try once more... Still not responding, so rather than spend your time debugging, I'll show you the recording of this exact flow from this morning, on your sample data. I'll find out what happened and send you a note today, and happy to run it live again on our next call."
>
> Optional light touch: "Well, you've now seen how we handle errors."

**Pitfalls:** blaming the customer's network; panicking; hiding the cause afterward.

### The customer's IT team won't give you access to the data you need.

**What they're testing:** empathy for IT's position, creativity in reducing the risk they are guarding against, and escalation through the right channel.

**Approach:**
1. **Understand their concern:** security, privacy, workload, or a policy. IT usually says no because the risk is theirs and the benefit is someone else's.
2. **Reduce the ask:** least-privilege, read-only, a subset of fields, masked/anonymised or synthetic data, a time-limited account, access only from their environment.
3. **Offer controls and documentation:** your SOC 2 report, data flow diagram, encryption details, retention/deletion policy, DPA.
4. **Offer alternative architectures:** deploy in their VPC, run the processing in their environment, or have them run an export script you provide.
5. **Make it easy:** write the exact access request (tables, permissions, duration).
6. **Escalate via your sponsor** if it stalls, framed as a timeline/business impact issue, not a complaint about IT.
7. **Make the delay visible** in status updates.

**Sample phrasing:**
> To IT: "I understand you don't want an outside vendor in your production database, and I wouldn't either. What would make you comfortable? We could start with a read-only replica, just these six columns, with customer names masked, through an account that expires in 30 days. I've written up the exact grants and our data handling diagram here."
>
> To the sponsor: "We're blocked on data access, which pushes the POC by a day for each day of delay. IT has fair concerns and I've proposed a reduced, masked access option. Could you help us get a decision by Thursday?"

**Pitfalls:** going around IT; using personal laptops or emailed spreadsheets of sensitive data to "move fast".

### The customer's data is much worse than expected and the deadline is fixed.

**What they're testing:** triage, scope flexibility, and early, honest communication.

**Approach:**
1. **Quantify the problem quickly:** missing fields %, duplicates, inconsistent formats, label quality. Show examples.
2. **Tell the sponsor early** with facts, not just "the data is bad."
3. **Since time is fixed, flex scope or quality:**
   - Narrow scope to the cleanest segment (one region, one product line, recent data).
   - Clean the highest-impact issues only (80/20).
   - Use techniques robust to messy data: LLM-based extraction/normalisation, fuzzy matching, human-in-the-loop review for low-confidence cases.
   - Ask the customer for help: a domain expert to label or fix a sample.
4. **Reset success criteria** if needed and document it.
5. **Make data quality a deliverable:** a data quality report with recommendations is valuable in itself and sets up phase two.

**Sample phrasing:**
> "I want to flag something early. About 40% of the records are missing the product code, and dates come in five formats. With the deadline fixed, I'd suggest we focus the pilot on the EMEA data from the last 12 months, which is about 85% complete. We'll still prove the approach on real data, and I'll deliver a data quality report showing what it would take to bring the rest up to standard. Does that work, or is full coverage essential for this deadline?"

**Pitfalls:** quietly working nights to clean everything and still missing; hiding the issue until the demo.

### The champion who sponsored the project leaves the customer's company.

**What they're testing:** account risk awareness and multi-threading relationships.

**Approach:**
1. **Act immediately:** this is a top risk to renewal or expansion.
2. **Ask the departing champion** (if possible) for a warm handoff: who should own it, who else cares.
3. **Find the new owner** with your AE/CSM, and meet them quickly.
4. **Re-sell the value:** a short, evidence-based summary of results so far (metrics, user quotes) tailored to the new person's priorities, which may differ.
5. **Strengthen other relationships:** end users, their managers, IT. Prevention is multi-threading from the start.
6. **Re-confirm the plan and success criteria** with the new owner; adjust if their goals differ.

**Sample phrasing:**
> To the new owner: "Thanks for making time. I know you've inherited this, so I want to be useful rather than just give you a sales update. In three months the pilot cut triage time by 35% for 40 agents. Before I tell you our plan, what are your priorities for this year? I want to make sure this project supports them, or hear honestly if it doesn't."

**Pitfalls:** waiting for the new person to reach out; assuming they share the old champion's goals.

### Two stakeholders at the customer want opposite things.

**What they're testing:** neutrality, facilitation, and knowing it's not your job to decide their internal politics, but it is your job to surface it.

**Approach:**
1. **Talk to each separately** to understand their goal and what they're worried about.
2. **Find shared objectives** (both want the project to succeed, usually for different reasons).
3. **Lay out options with trade-offs** neutrally, using data where possible.
4. **Bring them together** (with the sponsor if needed) to decide. Facilitate, don't take sides publicly.
5. **Look for designs that satisfy both:** configuration, phasing, pilots of both approaches.
6. **Document the decision and owner.**

**Sample phrasing:**
> "I've heard two strong views. Sales wants the model to auto-reply to customers to cut response time. Compliance wants every reply reviewed. Both make sense: one is about speed, the other about regulatory risk. Here's an option: auto-reply for the five lowest-risk question types, which is about half the volume, and human review for everything else. We measure for a month and revisit. Could we get both of you and Maria together on Thursday to agree?"

**Pitfalls:** siding with whoever you talk to last; letting the conflict stall the project silently.

### The customer's end users refuse to adopt the tool.

**What they're testing:** that you treat adoption as part of the job, and diagnose before you push.

**Approach:**
1. **Find out why,** directly from users: shadow them, look at usage data, ask. Common causes: it's slower than the old way, outputs aren't trusted, it's not in their workflow, no training, fear of job loss, the manager doesn't reinforce it.
2. **Fix the real issue:**
   - Workflow: embed it where they already work (CRM, email, Slack) instead of another tab.
   - Trust: show sources/citations and confidence, fix the top errors, let them correct outputs.
   - Speed: remove friction, cut clicks.
   - Fear: position it as removing the boring work; involve management messaging.
3. **Recruit power users/champions** among the users and let them shape it.
4. **Involve managers:** make it part of the process and metrics.
5. **Measure adoption** (weekly active users, % of cases through the tool) and iterate.

**Sample phrasing:**
> To users: "I'm not here to push the tool on you. I want to understand what's not working. Could I watch you handle a couple of cases this morning, with and without it?"
>
> To the sponsor: "Usage is at 15%. The main reasons are that the summaries live in a separate app and agents don't trust them without seeing the source. Next sprint we'll put the summary into the Zendesk sidebar with links to source messages. I'd also like two senior agents to help us test it. Could you help us get them some time?"

**Pitfalls:** blaming users; "more training" as the only answer.

### The customer wants a guarantee that the AI will never be wrong.

**What they're testing:** honesty about probabilistic systems, and turning an impossible guarantee into a managed risk.

**Approach:**
1. **Be honest:** no system, human or AI, is 100% accurate. Promising it destroys trust later.
2. **Reframe to measured performance versus today's baseline,** including human error rates.
3. **Design for error:** human review for high-stakes or low-confidence cases, guardrails, grounding with citations, restricted scope, abstaining when unsure.
4. **Offer measurement and monitoring:** an agreed evaluation set, ongoing sampling and audit, alerting.
5. **Offer contractual commitments you can keep:** process commitments (SLAs on uptime, response to issues), not accuracy guarantees, unless carefully defined.

**Sample phrasing:**
> "I can't promise it will never be wrong, and I'd be wary of anyone who does. What I can do is three things. First, measure it: we'll test on 500 of your real cases so you know the error rate before you rely on it. Your team's current error rate on these is about 4%, so that's our bar. Second, design around mistakes: for anything above a $10,000 value, or where the model isn't confident, a person reviews it, and every answer shows the source document. Third, monitor it in production, with a weekly sample reviewed by your team. Would that give you the confidence you need?"

**Pitfalls:** promising 100%; dismissing the concern instead of addressing the risk underneath it.

### You discover a bug in production that's affecting the customer's data. What do you do first?

**What they're testing:** incident instincts: contain first, communicate fast, be transparent, fix properly, learn.

**Approach:**
1. **Contain:** stop the damage. Disable the feature flag, pause the job, roll back the deploy. Don't start by debugging root cause while it's still writing bad data.
2. **Assess scope:** which data, which customers, since when, how many records. Preserve logs and evidence.
3. **Escalate internally** following the incident process (on-call, manager, security if data exposure is possible).
4. **Tell the customer early,** even before you have every answer: what happened, impact, what you've done, next update time. Contracts and regulations may impose deadlines if personal data is exposed (e.g. under GDPR the controller must notify the regulator within 72 hours of becoming aware of a breach, and a processor must inform the controller without undue delay).
5. **Remediate:** fix the bug, repair or restore affected data (backups, replay from source), verify with the customer.
6. **Post-incident review:** blameless, root cause, prevention (tests, monitoring, validation), shared with the customer.

**Sample phrasing:**
> "I'm calling to let you know about an issue we found an hour ago. A bug in yesterday's update caused about 1,200 invoice records to be tagged with the wrong cost centre since 2pm yesterday. We've paused the sync, so nothing new is affected. No data left your environment. We're preparing a corrected file and expect to have the records fixed by 5pm today; I'll update you at 2pm either way. I'm sorry for the disruption. Once it's resolved, we'll send you a written root-cause report and what we're changing to prevent it."

**Pitfalls:** fixing quietly and hoping nobody notices; debugging before containing; speculating about cause to the customer before you know.

### The customer asks for a custom build that would only help them. Do you build it?

**What they're testing:** commercial and product judgement: not "always yes" or "always no", but a reasoned decision.

**Approach:** decide based on:
- **Value of the account and the deal:** does it unblock a large contract or renewal?
- **Cost to build and, more importantly, to maintain:** who owns it in a year?
- **Generalisability:** is it truly unique, or the first instance of a pattern others will want?
- **Alternatives:** configuration, existing APIs, a partner, or the customer building it on our platform.
- **Strategic fit:** does it pull the product in a direction we don't want?

Options: decline politely; build it as a clearly scoped, paid professional-services item on top of public APIs, with handover to the customer; or generalise it into a product feature if others need it. Decide with your manager/product, not alone.

**Sample phrasing:**
> "I want to make sure we solve this well for you. The custom export to your legacy mainframe format is specific to your setup, so it won't get maintained as part of the core product. What I'd propose is that we build it as a small adapter on our public API, scoped as a services item, and hand it over to your team with documentation and tests. That way you own it and it won't break when we upgrade. Does that work for you?"

**Pitfalls:** quietly building one-off code inside the core product; refusing without exploring what the customer needs.

### The customer asks you to commit to a timeline you think is unrealistic.

**What they're testing:** the courage to say no to a date, while being constructive about what's possible.

**Approach:**
1. **Ask why that date:** board meeting, regulatory deadline, budget cycle? The date often matters for one specific outcome.
2. **Show your reasoning:** the main pieces of work and dependencies, not just "it's too fast".
3. **Offer options:** reduce scope to meet the date, add resources (if it actually helps), phase the delivery, or move the date.
4. **Recommend one** and commit firmly only to what you believe.
5. **Put it in writing,** including assumptions and dependencies on their side.

**Sample phrasing:**
> "I want to give you a date I can actually hit, rather than one that sounds good today. Full rollout to all 500 users by March 1 isn't realistic. SSO, the security review and the SAP integration alone are about eight weeks. What's happening on March 1?"
> "OK, so what you need is something real to show the board. Here's what I can commit to: by March 1, 50 users in the finance team using it daily, with real results. Full rollout by mid-April. If your security review finishes in under three weeks, we may beat that, and I'll tell you as soon as I know."

**Pitfalls:** agreeing to please and missing later (much worse); a flat "no" without alternatives.

### The sales team promised something the product can't do. What do you do?

**What they're testing:** internal collaboration (not blaming sales), customer honesty, and practical recovery.

**Approach:**
1. **Clarify internally first:** talk to the AE privately. Understand exactly what was said and in what context. Don't contradict sales in front of the customer.
2. **Assess the gap:** can it be met with a workaround, configuration, roadmap item, or partial delivery?
3. **Agree a joint message** with the AE (and manager if needed).
4. **Tell the customer early and honestly,** with what you can do instead.
5. **Fix the process:** offer to join sales calls on technical topics, create a "what we do/don't do" sheet, or a technical review before commitments.

**Sample phrasing:**
> To the AE: "I want to get ahead of something. The customer believes we support on-prem deployment with no internet access. We don't today. Can we work out together how to handle it before Thursday's call?"
>
> To the customer (with the AE): "I want to clarify something about the deployment model so there's no confusion later. Today we run in a private, single-tenant cloud environment in your region, not fully air-gapped on-prem. I know that's different from what you understood, and I'm sorry for the confusion. Let's look at your actual requirements: if it's about data not leaving your control, a private VPC deployment with customer-managed keys may meet them. If true air-gap is a regulatory must, I'd rather tell you that now."

**Pitfalls:** throwing sales under the bus; going along with the promise and failing later.

### The customer compares you unfavourably to a competitor. How do you respond?

**What they're testing:** composure, curiosity, and focus on the customer's needs rather than trashing competitors.

**Approach:**
1. **Don't get defensive** and never bad-mouth the competitor.
2. **Ask what specifically** they like about the competitor. It's free product feedback and reveals their real criteria.
3. **Acknowledge where the competitor is genuinely stronger,** if true. Credibility matters.
4. **Reframe on their priorities:** where you're stronger for their use case, ideally with evidence (a test on their data, a reference customer).
5. **Offer a fair comparison:** a bake-off on their real data and agreed criteria.
6. **Feed it back** to product.

**Sample phrasing:**
> "That's useful to hear. What specifically stood out about their tool? ... Yes, their dashboard builder is more flexible than ours today, that's fair. From what you've told me, though, your biggest priority is accuracy on scanned contracts and keeping data in your region. That's where I think we're strongest. Rather than take my word for it, would you be open to running both on the same 100 contracts and comparing results?"

**Pitfalls:** criticising the competitor; claiming you're better at everything.

### The customer wants to see source code or model weights.

**What they're testing:** understanding of IP and policy limits, and finding out the real concern (usually security, trust, or lock-in).

**Approach:**
1. **Ask what they want to achieve:** security assurance, auditability, regulatory explainability, continuity if the vendor fails, or fine-tuning/self-hosting.
2. **Know the policy:** proprietary code and model weights are typically not shared. Don't improvise; check with legal/product.
3. **Offer alternatives that meet the need:**
   - Security: SOC 2 Type II / ISO 27001 reports, pen test summaries, security questionnaire, architecture diagrams.
   - Auditability: model cards, system cards, eval results, logs of inputs/outputs, explanations and citations.
   - Continuity: source code escrow agreements, data export guarantees.
   - Customisation: fine-tuning APIs, private deployment options.
   - Code you wrote for them: integration code built for the customer is often theirs under the contract; share it.
4. **Escalate** for formal requests (escrow, audit rights) to legal.

**Sample phrasing:**
> "We don't share the model weights or the core platform source, so let me understand what's behind the request. Is it about a security review, explaining decisions to your regulator, or making sure you're not stuck if something happens to us? ... For the regulator, we can give you the model documentation, our evaluation results on your data, and full logs of every input and output with citations. For continuity, our legal team can discuss source code escrow. And all the integration code we wrote for you is in your repo already."

**Pitfalls:** sharing what you're not authorised to; flat refusal without addressing the concern.

### You realise mid-project that the approach you proposed won't work.

**What they're testing:** intellectual honesty, speed of course correction, and ownership.

**Approach:**
1. **Confirm it with evidence quickly:** a test or experiment that shows why (e.g. accuracy plateaued at 70% against a 90% target).
2. **Develop alternatives before you raise it** (but don't sit on it long): new approach, rough effort, risk.
3. **Tell your internal team, then the customer, early,** with evidence and a recommendation.
4. **Own it** without excessive apology.
5. **Agree the new plan,** including any changes to timeline or scope, in writing.
6. **Learn:** what earlier test would have caught this? (Usually: prototype the riskiest part first.)

**Sample phrasing:**
> "I need to share something important. We've tested the rules-based extraction on 300 of your documents and it's reaching 68% accuracy. We need 90%, and the trend tells me rules won't get us there because your supplier formats vary too much. I should have tested format variety earlier, and I've learned from that. I recommend switching to an LLM-based extraction with human review for low-confidence fields. In a quick test on 50 documents it reached 91%. It adds about a week and changes the running cost to roughly 2 cents per document. Can we walk through it together tomorrow?"

**Pitfalls:** hoping it'll work out; sunk-cost thinking; presenting the problem without a proposal.

### The customer keeps adding scope without extending the timeline.

**What they're testing:** scope management with a positive tone; making trade-offs visible.

**Approach:**
1. **Welcome the idea** (it shows engagement), then **quantify the cost** of each addition.
2. **Make the trade-off explicit:** fixed time means something comes out. Use a visible, prioritised backlog.
3. **Let the customer choose** what to swap, defer, or pay for.
4. **Reference the agreed SOW/scope** neutrally.
5. **If it's a pattern,** have a reset conversation with the sponsor about the plan and possibly a change order.

**Sample phrasing:**
> "I like the idea of adding the Slack notifications. It's about three days. We have a fixed date of the 30th, so to fit it in, something else needs to move. The options are: drop the CSV export, which only two users asked for, or add Slack to the first post-launch release. Which would you prefer?"
>
> Reset: "Over the past month we've added six items, about three weeks of work, while the date stayed the same. I want to protect the launch. Can we go through the backlog together and agree what's in for the 30th, and put the rest in a phase-two change order?"

**Pitfalls:** absorbing everything with overtime; saying no without offering a path.

### A customer's security review is blocking the project for weeks.

**What they're testing:** proactive navigation of enterprise security processes, which are often the longest pole.

**Approach:**
1. **Start the security review on day one,** not at the end. Ask early about the process and the questionnaire.
2. **Find out exactly what's outstanding** and who owns each item. Get a named reviewer.
3. **Make it easy:** pre-fill questionnaires (SIG, CAIQ), provide SOC 2/ISO reports, pen test summary, data flow diagrams, DPA, sub-processor list, AI-specific answers (training on customer data? retention? model hosting location?).
4. **Offer a working session** between their security team and your security engineer instead of a long email chain.
5. **Reduce risk to speed approval:** start with non-sensitive or synthetic data, deploy in their VPC, limit scope of the first phase.
6. **Escalate via sponsor** with the business cost of delay.
7. **Keep building** what you can in parallel (with synthetic data).

**Sample phrasing:**
> To security: "I know you've got a long queue. What's left open from your side? If it helps, I can bring our security lead to a 30-minute call this week to go through the remaining 14 questions live rather than by email."
>
> To the sponsor: "We're in week four of the security review. Each week of delay pushes the savings we projected, about $40k a month, back by a week. We've answered everything they've asked within 24 hours. Could you help get it prioritised, or would it help to start the pilot with anonymised data while the review completes?"

**Pitfalls:** treating security as an obstacle; leaving it to the end.

### The customer is not responsive / not providing feedback.

**What they're testing:** whether you diagnose disengagement and protect the project, rather than just waiting.

**Approach:**
1. **Diagnose:** are they busy, deprioritising the project, unhappy, or unclear about what you need? Check with your AE/CSM.
2. **Make responding easy:** specific, small, closed questions with a default ("If I don't hear by Friday, I'll proceed with option A").
3. **Change the channel:** a 15-minute call instead of an email; join their standup; message on Slack/Teams.
4. **Go wider:** other stakeholders, end users.
5. **Make the impact visible:** state what's blocked and the effect on the timeline.
6. **Escalate** to the sponsor if it continues, and ask directly whether priorities have changed.

**Sample phrasing:**
> "Hi Sarah, I need one decision to keep us on track: should low-confidence results go to the AP team's queue or to the original requester? I'd suggest the AP queue. If I don't hear by Thursday, I'll go with that and we can change it later."
>
> To the sponsor: "I've noticed we haven't had feedback from the team in three weeks. Has something changed in your priorities? I'd rather know now so we can adjust the plan than build something that isn't needed."

**Pitfalls:** passively waiting and then blaming them for a missed date.

### You're on-site and the customer asks you to fix something unrelated to your project.

**What they're testing:** relationship building versus scope discipline, and judgement about liability.

**Approach:**
1. **Consider size and risk:** a 10-minute favour that builds goodwill (e.g. help with a query, explain an API error) is often worth doing.
2. **Avoid things that create liability or ownership:** changing their production systems you're not responsible for, security-sensitive work, or anything that will become "yours" forever.
3. **If it's bigger,** acknowledge the need, don't do it on the spot, and route it properly: their IT, our support, or a scoped add-on with the AE (it may be an expansion opportunity).
4. **Protect your project's commitments** and tell your manager if requests become frequent.

**Sample phrasing:**
> Small: "Sure, let me take a quick look. ... It's a timezone issue in your query; here's the fix."
>
> Bigger: "I'm happy to help where I can, but I'd rather not change your HR system's config without the team who owns it, in case it affects something I can't see. Let me write down what I think is going on, and I'll connect you with the right person. If it's something we could help with more formally, I'll mention it to Tom, our account lead."

**Pitfalls:** always saying yes and missing your own deliverables; always saying no and seeming unhelpful.

## C. Communication

### Explain [RAG / embeddings / an API / a database index / Kubernetes / LLM hallucination] to a non-technical executive.

**What they're testing:** whether you can drop jargon, use a familiar analogy, and connect the concept to something the executive cares about (cost, risk, speed), in under a minute.

**Approach:** one-sentence analogy, how it works in plain words, why it matters to their business, then check understanding. Avoid acronyms. Stop before you lose them.

**RAG (retrieval-augmented generation):**
> "An AI model on its own is like a very bright new hire who has read a lot of the internet, but has never seen your company's documents. RAG is like giving that new hire your company handbook and telling them: before you answer any question, look it up first and quote the page. When someone asks a question, the system first searches your documents for the most relevant passages, then hands those to the AI to write an answer, with references. For you, that means answers based on your own up-to-date policies, which people can check, without the cost of retraining a model every time a document changes."

**Embeddings:**
> "Embeddings are a way of turning text into a kind of map coordinate based on meaning. Sentences that mean similar things land close together on the map, even if they use different words. So 'How do I get my money back?' and 'refund policy' end up near each other. That's what lets a search find the right document when a customer doesn't use your exact wording. It's the engine behind smarter search and recommendations."

**An API:**
> "An API is like a waiter in a restaurant. You don't go into the kitchen; you give the waiter your order in a standard format, and they bring back what you asked for. Software systems use APIs to request data or actions from each other in an agreed way. That's why we can connect our system to your CRM without anyone touching the CRM's internals, and why the connection keeps working when either side changes behind the scenes."

**A database index:**
> "It's like the index at the back of a book. Without it, to find every mention of 'pricing', you'd read every page. With it, you go straight to the right pages. A database index does the same for data, so finding one customer among 50 million records takes milliseconds instead of minutes. The trade-off is that, like a book index, it takes some space and has to be updated whenever the content changes, so we add indexes for the questions you ask most often, not for everything."

**Kubernetes:**
> "Think of Kubernetes as an automated operations manager for a fleet of delivery vans. You tell it, 'I always need ten vans on the road.' If one breaks down, it sends a replacement automatically. If demand spikes, it adds more vans, and removes them when it's quiet. For software, the 'vans' are copies of your application. The business benefit is that the system stays up when something fails, scales with demand, and you only pay for the capacity you need."

**LLM hallucination:**
> "An AI language model works by predicting what a good answer looks like, a bit like a very confident colleague who's excellent at sounding right. Most of the time it is right, but when it doesn't actually know, it can still give a fluent, confident answer that's made up: a policy that doesn't exist, or a wrong figure. That's called a hallucination. We manage it in three ways: we make the AI answer from your own documents and show its sources, we tell it to say 'I don't know' when the documents don't cover the question, and we test it on real questions and keep people in the loop where mistakes would be costly. It's a risk we reduce and measure, not one we can promise is zero."

**Pitfalls:** analogies that need their own explanation; going into technical detail unprompted; not linking to business value.

### Explain a technical trade-off to a business stakeholder.

**What they're testing:** framing technical choices as business decisions (cost, speed, risk, quality) and letting the stakeholder decide with a recommendation.

**Approach:**
1. **State the decision** in one line.
2. **Give 2-3 options,** each with impact on things they care about: cost, time to deliver, quality, risk, flexibility later.
3. **Use numbers** where you can.
4. **Recommend one** and say why.
5. **Say what's reversible** and what isn't.

**Example (choosing a model size):**
> "We need to choose which AI model runs your customer email replies. Option A is the largest model: best quality, about 94% of replies need no edits, but it costs around $9,000 a month at your volume and takes about 4 seconds per reply. Option B is a smaller model: 89% need no edits, about $1,200 a month and under a second. The 5% difference means roughly 250 more emails a month an agent has to touch up, about 20 hours of work. My recommendation is B for routine emails, and automatically using A for complaints and high-value accounts. That gets you most of the quality for about a quarter of the cost, and we can change the mix any time."

**Pitfalls:** presenting options without a recommendation; hiding uncertainty; using jargon like "latency" or "parameters" without translation.

### How do you give status updates to a customer? How often, in what format?

**What they're testing:** proactive, predictable, honest communication that matches audience and project phase.

**Approach:**
- **Cadence:**
  - Working team: daily/async in a shared Slack/Teams channel during intense phases; weekly sync.
  - Sponsor: a short weekly written update.
  - Executives: monthly or at milestones (steering committee), plus immediately for major risks.
- **Format (weekly written update, one screen):**
  - Overall status: green/amber/red, with one-line reason.
  - Progress since last update (outcomes, not activities).
  - Next steps with dates.
  - Risks and issues with owner and mitigation.
  - Decisions or help needed from the customer, with deadlines.
  - Key metrics (accuracy, adoption, usage) once live.
- **Principles:** consistent day and template; bad news early, never surprise anyone in a meeting; be honest about amber/red; keep a running decision log; link to a shared project tracker.

**Sample phrasing (opening of a written update):**
> "Status: Amber. Build is on track, but go-live on the 15th is at risk because the SAP test environment isn't available yet.
> Done this week: extraction accuracy reached 93% on the 300-invoice test set (target 90%).
> Next week: integration testing, if SAP access arrives by Tuesday.
> Need from you: SAP test credentials by Tuesday (owner: IT, Raj). If they arrive later, go-live moves day for day."

**Pitfalls:** "everything's green" until it suddenly isn't; long activity lists nobody reads.

### How do you deliver bad news to a customer?

**What they're testing:** speed, directness, ownership, and coming with a plan.

**Approach:**
1. **Deliver it early,** as soon as you're reasonably confident. Bad news ages badly.
2. **Choose the right channel:** a live conversation (call or in person) for anything significant, followed up in writing. Not first by email.
3. **Lead with the headline** in the first sentence. Don't bury it.
4. **Explain briefly** what happened and why, factually, owning your part.
5. **State the impact** on them.
6. **Present the plan and options,** with a recommendation.
7. **Say what you need** from them and when you'll update next.
8. **Follow up in writing.**

**Sample phrasing:**
> "I have some disappointing news, and I wanted to tell you directly. We won't hit the pilot launch on the 10th. Our load test last night showed the system slows down badly above 200 concurrent users, and you need 500. The cause is how we're querying your document store, and that's on us. We have a fix designed and expect to launch on the 17th. Alternatively, we can launch on the 10th for your first 150 users and add the rest on the 17th. I'd recommend the phased launch so your team gets started. I'll send this in writing today and update you Wednesday on the fix."

**Pitfalls:** delaying, softening it until it's unclear, blaming others, or giving bad news without a plan.

### How do you write a project proposal / statement of work?

**What they're testing:** clarity on scope, deliverables, responsibilities, and assumptions, which prevents disputes later.

**Approach - typical structure:**
1. **Background and objectives:** the business problem and the outcome, in the customer's words.
2. **Scope:** in-scope items, and an explicit **out-of-scope** list.
3. **Approach and phases:** e.g. discovery, build, pilot, rollout, with milestones.
4. **Deliverables:** concrete and verifiable (deployed service, integration, documentation, training sessions, evaluation report).
5. **Success criteria / acceptance criteria:** measurable, and how acceptance is signed off.
6. **Timeline:** milestones with dates or relative weeks.
7. **Roles and responsibilities:** ours and theirs (data access, SMEs, environments, approvals). A RACI table is useful.
8. **Assumptions and dependencies:** e.g. "access to data within 5 business days of kickoff."
9. **Change management:** how scope changes are requested, estimated and approved.
10. **Commercials:** fees, payment milestones (often tied to deliverables), expenses.
11. **Risks** and mitigations; governance (status meetings, steering committee).

**Tips:** write in plain language; make customer dependencies explicit because they cause most delays; review it with the AE and legal; walk the customer through it live rather than just emailing it.

**Sample phrasing (walking the customer through it):**
> "Before you sign, I'd like to walk through page 3, the assumptions. These are the things we need from your side. If any of them don't look realistic, now's the time to say so, because the timeline depends on them."

**Pitfalls:** vague deliverables ("AI solution"); no out-of-scope section; no acceptance process.

### How do you run a customer workshop?

**What they're testing:** facilitation: preparation, structure, getting input from everyone, and leaving with decisions.

**Approach:**
1. **Before:**
   - Agree the goal and the outputs with the sponsor (e.g. "a prioritised list of 3 use cases with owners").
   - Invite the right mix: decision-maker, end users, IT/data, and keep it to about 6-12 people.
   - Send an agenda and any pre-reading; collect sample data or examples in advance.
2. **During (e.g. a half-day):**
   - Intro: goals, agenda, ground rules (10 min).
   - Context: current state, pain points; let users speak (45 min).
   - Short "art of the possible" demo grounded in their data, not a sales pitch (20 min).
   - Ideation: sticky notes, individually first, then grouped (45 min).
   - Prioritisation: value versus feasibility matrix, dot voting (30 min).
   - Deep dive on top 1-2: data, users, success metrics, risks (45 min).
   - Next steps: owners and dates (15 min).
   - Facilitate: draw out quiet people, park off-topic items in a "parking lot", watch time.
3. **After:** send a summary within 24-48 hours: decisions, prioritised list, owners, next steps, parking lot.

**Sample phrasing:**
> "Let's take five minutes in silence and each write down the tasks that waste the most time in your week, one per sticky note. Then we'll group them. I want to hear from everyone, not just the people who talk the most, me included."
>
> "That's a great point, but it's beyond today's goal. I'll put it in the parking lot and we'll make sure it gets an owner at the end."

**Pitfalls:** turning it into a two-hour product demo; letting the most senior person dominate; no written follow-up.

### Present your take-home solution as if to the customer's CTO.

**What they're testing:** executive-level technical communication: business framing, architecture clarity, trade-offs, risks, and handling tough questions, in limited time.

**Approach (aim for 10-15 minutes, leaving time for questions):**
1. **Problem and outcome (1 min):** what problem, for whom, and the result in one sentence. Lead with the result.
2. **Demo (3-4 min):** the main flow on realistic data. Have a recording as backup.
3. **Architecture (3 min):** one clear diagram: data sources, processing, model/logic, storage, interfaces. Explain key technology choices and why.
4. **Trade-offs and decisions (2 min):** what you chose and what you deliberately didn't (e.g. "batch rather than streaming, because the business needs daily results; streaming would add cost and complexity").
5. **Quality and evaluation (2 min):** how you know it works: tests, metrics, eval set, limitations.
6. **Production readiness (2 min):** security (auth, secrets, data handling), scaling, monitoring, cost estimate, failure modes.
7. **Roadmap and next steps (1 min):** what you'd do with 2 more weeks and what you need from them.
8. **Q&A:** answer directly; say "I don't know, here's how I'd find out" when needed.

**Sample phrasing (opening):**
> "You asked for a way to route incoming support tickets automatically. The short version: this prototype classifies tickets into your 12 queues with 91% accuracy on a held-out set of 1,000 historical tickets, in under a second each, for about $0.002 per ticket. Let me show it, then walk through how it's built, what I'd change before production, and the risks I see."
>
> On trade-offs: "I chose a hosted LLM with few-shot examples rather than training a custom classifier because you don't have consistent labels yet. Once we've collected 10,000 corrected labels from agents, a fine-tuned small model would likely be cheaper and faster, so I've designed the interface to allow swapping it."

**Pitfalls:** starting with code; overclaiming; hiding limitations (a CTO will find them, and naming them first builds credibility).

### How do you document a solution so the customer can maintain it after you leave?

**What they're testing:** ownership beyond go-live, and a practical, audience-specific handover.

**Approach:**
1. **Write for specific readers:** the engineers who will maintain it, the operators who run it, and the business owner.
2. **Core documents (kept in their repo/wiki):**
   - **README:** what it does, how to set it up and run locally, how to deploy.
   - **Architecture overview:** diagram, components, data flows, external dependencies, why key decisions were made (short ADRs).
   - **Runbook:** monitoring dashboards, alerts and what to do for each, common failures and fixes, how to roll back, escalation contacts.
   - **Configuration and secrets:** what's configurable, where secrets live (never in docs), how to rotate keys.
   - **Model/AI specifics:** prompts and where they live, evaluation set and how to re-run evals, how to update knowledge sources, known limitations, cost drivers.
   - **User guide** for end users and an admin guide.
3. **Make the code self-explanatory:** tests, CI, infrastructure as code (Terraform), clear naming.
4. **Hand over actively:** walkthrough sessions, have their engineer do a deploy and fix a planted issue while you watch, recorded videos, and a period of shadow support.
5. **Agree the ownership and support model** after you leave.

**Sample phrasing:**
> "Before I roll off, I'd like Priya to do the next two deployments and handle an alert with me watching rather than doing it. If she gets stuck anywhere, that's a gap in the docs and I'll fix it."

**Pitfalls:** a 60-page document nobody reads; docs written at the last minute; knowledge only in your head.

### How do you handle a technical question in front of a customer that you don't know the answer to?

**What they're testing:** honesty and credibility. Bluffing is the worst answer.

**Approach:**
1. **Say you don't know,** plainly. It builds trust.
2. **Share what you do know** and your reasoning, clearly labelled as such.
3. **Clarify the question** so you get the right answer.
4. **Commit to a specific follow-up:** who will answer and by when.
5. **Follow up on time,** in writing, and if relevant bring the expert to the next call.

**Sample phrasing:**
> "That's a good question and I don't want to guess. Let me make sure I have it right: you're asking whether the embeddings we store could be reversed to reconstruct the original text from patient records? I know we encrypt them at rest and they stay in your region, but I'm not certain about the research on reversibility for our specific model. I'll check with our security team and get you a written answer by Thursday, and if useful I'll bring our security lead to the next call."

**Pitfalls:** making something up; over-apologising; forgetting to follow up.

## D. Product feedback & scale

### How do you decide whether a customer request should become a core product feature?

**What they're testing:** product thinking: separating one customer's preference from a market need, and respecting the product team's roadmap.

**Approach - evaluate against:**
- **Breadth:** how many customers (current and target) have the same underlying need? Check with other FDEs, CS, sales, support tickets.
- **Depth/value:** revenue tied to it (deals blocked, churn risk, expansion), and how painful the workaround is.
- **Strategic fit:** does it fit the product vision and target segment, or pull toward a niche?
- **Generalisability:** can it be built as a general capability (e.g. "custom fields" rather than "field X for customer Y")?
- **Cost:** build, and ongoing maintenance and complexity for every user.
- **Evidence:** a field-built version already in use with good outcomes is strong evidence.

**Process:** write it up as a problem statement (not a solution), with customer examples, frequency, revenue impact and your proposed generalisation; bring it to product; accept their decision while giving the customer an honest answer.

**Sample phrasing (to the PM):**
> "Three enterprise customers have now asked for the same thing in different words: they need to restrict which documents each team's assistant can search. Together that's about $1.2M ARR, and one renewal depends on it. Today I'm solving it with separate indexes per team, which won't scale. I think the general feature is document-level permissions synced from their identity provider. Here are the three use cases and the workaround. Can we review it in the next planning cycle?"

**Pitfalls:** pushing every request as a must-have; describing a customer-specific solution instead of the general problem.

### How do you feed learnings from the field back to the product/engineering team?

**What they're testing:** that you see yourself as the product team's eyes in the field, and do it in a structured, actionable way.

**Approach:**
1. **Capture continuously:** log friction, bugs, workarounds, feature requests and wins as they happen, tagged by customer, area, severity and revenue impact.
2. **Use the shared system:** the product feedback tool or tracker (Jira, Linear, Productboard), not just Slack messages.
3. **Write good reports:** problem, evidence (examples, logs, quotes), frequency, impact, current workaround, suggestion.
4. **Aggregate:** a monthly field insights summary of top themes across customers is far more persuasive than single anecdotes.
5. **Build relationships:** regular syncs with PMs, join planning reviews, invite engineers on customer calls or site visits.
6. **Bring artefacts:** prototypes or field-built code that shows what's needed.
7. **Close the loop** with customers when things ship.

**Sample phrasing:**
> "Top three themes from the field this month: first, ingestion of scanned PDFs fails silently in about 8% of documents across four customers. That's our most common support escalation, examples linked. Second, admins can't see usage by team, which every expansion conversation asks for. Third, a smaller one: SSO setup takes about two days per customer because the docs are out of date. I've drafted updated docs for that one."

**Pitfalls:** anecdotes without data; complaining without proposals; feedback only through informal channels where it gets lost.

### You've built the same integration for 3 customers. What now?

**What they're testing:** the instinct to turn repeated work into leverage (the "rule of three"), and doing it collaboratively.

**Approach:**
1. **Compare the three:** what's identical, what varies (auth, field mappings, edge cases). The variable parts become configuration.
2. **Build a reusable version:** a connector/module with configuration, tests, documentation, and a deployment template.
3. **Decide where it lives:** an internal FDE toolkit/library first; propose to product as a supported, productised connector if demand continues.
4. **Migrate the existing three** onto it when practical, so you maintain one thing, not three.
5. **Share it:** demo to the FDE team, write a playbook entry.
6. **Measure the gain:** e.g. integration time from two weeks to two days. This also makes the case for productisation.

**Sample phrasing (to your manager/PM):**
> "I've now built the Salesforce case integration three times, each taking about eight days. About 80% of the code is the same; the differences are field mappings and auth. I'd like to spend a week turning it into a configurable connector with tests and docs. That should bring the next one down to about a day. Two more customers in the pipeline need it this quarter. Longer term, I think it belongs in the product, and I'll write up the case for that."

**Pitfalls:** building a "framework" too early (after one customer); forking code three more times out of inertia.

### Hack it now vs. build it properly — how do you decide?

**What they're testing:** pragmatism with judgement, and awareness of what makes "temporary" become permanent.

**Approach - hack it when:**
- You're validating whether something is worth building at all (POC, demo, one-off analysis).
- It's easily reversible, low-risk, and isolated.
- A real deadline matters more than long-term cost (a deal, a critical outage workaround).

**Build properly when:**
- It touches security, customer data integrity, money, or compliance.
- Others will depend on it or it will run in production for a long time.
- It will be repeated (you'll do it again for more customers).
- Failures would be hard to detect.

**Rules for hacks:** time-box them, label them clearly (TODOs, tickets), keep them isolated behind an interface, tell stakeholders it's temporary, and schedule the follow-up. Never compromise on security or data correctness, even in a hack.

**Sample phrasing:**
> "For Thursday's demo, I'll hard-code the field mappings; that's fine for a demo and saves two days. If they go ahead, I've logged a ticket to build proper configurable mappings with validation before any production data flows through it. The one thing I won't shortcut is authentication; that's done properly from day one."

**Pitfalls:** hacks that become production without anyone knowing; over-engineering a POC that may be thrown away.

### How do you balance customer-specific work with reusable work?

**What they're testing:** thinking about leverage and the long-term health of the platform while still delivering for the customer in front of you.

**Approach:**
1. **Default architecture:** keep customer-specific logic in a thin layer (configuration, adapters, prompts) on top of shared, reusable components.
2. **Ask at each task:** "Is this the first time I've seen this, or the second or third?" If repeated, make it reusable.
3. **Build reuse into the plan:** e.g. 70-80% customer delivery, 20-30% reusable tooling, agreed with your manager.
4. **Prioritise reusable work by multiplier:** time saved across upcoming deployments.
5. **Customer still comes first** when a commitment is at stake. Reuse can be done right after if it's tracked.
6. **Share learnings:** playbooks, templates, internal libraries.

**Sample phrasing:**
> "For this customer I'm writing the document parser as a shared module with their specifics in a config file. It took maybe a day longer than a one-off, but the next two healthcare deployments can use it directly."

**Pitfalls:** all one-offs (the team never gets faster); premature generalisation that delays the customer.

### How do you handle a product bug the core team won't prioritise but your customer needs fixed?

**What they're testing:** influencing without authority, working within the system, and creative mitigation.

**Approach:**
1. **Understand why** it was deprioritised: low reported impact, unclear repro, team capacity, or risk of the fix.
2. **Strengthen the case:** clear reproduction steps, impact in business terms (revenue at risk, other affected customers, support load), and how often it occurs.
3. **Mitigate for the customer meanwhile:** a workaround, a guard in the integration layer, a data correction script.
4. **Offer to help:** write the fix yourself as a PR to the core repo following their standards and review, if the team accepts that. FDEs often have the context to do this.
5. **Escalate through proper channels** if the business impact justifies it (your manager to their manager), with data, not emotion.
6. **Be honest with the customer** about the status and workaround, without blaming the product team.

**Sample phrasing (to the core team):**
> "I understand it's low on the list. Some new information: this bug affects every customer using non-English documents, which is three accounts worth about $800k ARR, and one is threatening to pause rollout. I have a reliable repro and I think the fix is in the tokenizer config. Would you be open to me opening a PR, with one of your team reviewing it?"
>
> To the customer: "The underlying fix is with our engineering team. In the meantime, I've added a check in your pipeline that catches these documents and routes them for manual review, so nothing incorrect reaches your users."

**Pitfalls:** going around the team (hot-patching core code in production); blaming the product team to the customer.

## E. Business acumen

### How do you measure the ROI of a deployment?

**What they're testing:** whether you can connect technical work to money, using a credible, conservative method the CFO would accept.

**Approach:**
1. **Baseline before you start:** volume, time per task, cost per task, error rates, revenue metrics.
2. **Identify value drivers:**
   - Cost savings: hours saved x fully loaded hourly cost (only count it if the time is redeployed or hiring is avoided).
   - Revenue: higher conversion, faster sales cycle, reduced churn.
   - Quality/risk: fewer errors, fines avoided, fewer rework cycles.
   - Speed: cycle time reduction (e.g. faster cash collection improves working capital).
3. **Count total costs:** licences/usage (including model API costs), implementation, internal staff time, infrastructure, ongoing maintenance and change management.
4. **Measure after,** ideally with a comparison group (pilot team vs. non-pilot team, or before/after controlling for volume).
5. **Calculate:** ROI = (benefit - cost) / cost; payback period; and present both conservative and expected cases.
6. **Combine hard numbers with qualitative evidence:** user quotes, employee satisfaction.

**Worked example:**
> 40 agents each spend 2 hours/day on ticket summaries. The tool saves 50% of that: 40 x 1 h x 220 days = 8,800 hours/year. At $45/hour fully loaded, about $396k/year. Costs: $120k licence + $30k usage + $50k implementation in year one = $200k. Year-one ROI = (396 - 200) / 200 = 98%; payback about 6 months.

**Sample phrasing:**
> "I'd suggest we only count savings your finance team agrees with. Do they accept hours saved, or do they need to see reduced overtime or avoided hires?"

**Pitfalls:** inflated "time saved" numbers that don't turn into real savings; ignoring internal costs; no baseline.

### How do you identify expansion opportunities at an existing customer?

**What they're testing:** commercial awareness that grows from delivering value, not from pushy selling.

**Approach:**
1. **Deliver and measure success first.** Expansion follows proven value.
2. **Look for signals:**
   - Usage growing or hitting limits; other teams asking for access or building workarounds.
   - Adjacent workflows in the same team (same data, different task).
   - Similar teams in other regions or business units.
   - Feature requests that map to other products or tiers.
   - Recurring pain points you hear about during the work.
3. **Ask:** "Who else in the company deals with a similar problem?"
4. **Use your champion** to introduce you to other teams; offer to present results internally.
5. **Hand qualified opportunities to the AE/CSM** with context: team, problem, rough size, who to talk to. Don't negotiate commercials yourself.

**Sample phrasing:**
> To the champion: "The claims team's results have been great. You mentioned the underwriting team reads the same kinds of documents. Would it be useful if I showed their lead what we've done here? No pressure, just in case it's relevant."
>
> To the AE: "Possible expansion at Acme: underwriting, about 60 users, same document types. Their lead is Sam. Champion is happy to introduce us. I'd estimate a 3-week deployment since we can reuse most of the pipeline."

**Pitfalls:** pitching before the current project delivers; acting as a salesperson instead of a trusted advisor.

### How would you prioritise among 5 customers all asking for your time?

**What they're testing:** structured prioritisation, transparency, and using the team instead of heroics.

**Approach:**
1. **List the asks** with urgency (deadline, production issue?) and impact (revenue, renewal, strategic value, number of users affected).
2. **Triage:**
   - Production incidents and data issues first.
   - Contractual commitments and imminent deadlines (go-lives, renewals).
   - High-value or strategic accounts and expansions.
   - Requests that can be handled by others or async (docs, support, CSM).
3. **Align with your manager and AEs** on priorities. It's a business decision, not just yours.
4. **Communicate clearly** with each customer: when you'll get to it, and what they'll get meanwhile. Set realistic expectations rather than going silent.
5. **Reduce demand:** reusable docs, office hours, self-serve tools, delegating to support/CS.
6. **Flag the capacity issue** if it's ongoing; it may need more headcount.

**Sample phrasing:**
> To your manager: "I have five asks this week and capacity for about three. Here's my proposed order: the Globex production issue, the Initech go-live Friday, then the Acme renewal demo. For Umbrella and Hooli, I'd suggest support takes the API questions and I schedule a call next Tuesday. Do you agree?"
>
> To a deprioritised customer: "I want to give this proper attention rather than rush it. I can do a working session Tuesday at 10. Meanwhile, here's a doc that covers the first two questions."

**Pitfalls:** first-come-first-served; whoever shouts loudest wins; saying yes to everyone and dropping balls.

### What makes a deployment successful beyond the technical solution?

**What they're testing:** whether you understand that most enterprise projects fail on people, process and politics rather than technology.

**Key factors:**
- **Clear business outcome and agreed metrics,** measured and communicated.
- **Executive sponsorship** that stays engaged, plus multiple champions.
- **Adoption:** the tool fits into daily workflow, users are trained, managers reinforce it, users trust the outputs.
- **Change management:** communication about why, addressing job-impact fears, updated processes and SOPs.
- **Integration with existing systems** (SSO, CRM, ticketing) so it's not "yet another tool."
- **Operational ownership:** someone on the customer side owns it, with runbooks, support model, and monitoring.
- **Security, compliance, and legal approval** done properly.
- **Feedback loop:** user feedback improves the system over time.
- **Relationship and trust:** reliable communication, keeping promises.
- **Commercial success:** renewal, expansion, a reference customer.

**Sample phrasing:**
> "I'd say a deployment is successful when, six months later, people are using it every day without us in the room, the customer can show the business result to their leadership, and they'd recommend us to a peer. The code is maybe a third of what it takes to get there."

**Pitfalls:** defining success as "deployed to production."

### How do you work with account executives / customer success / deployment strategists?

**What they're testing:** teamwork in a customer-facing org and clarity on roles.

**Roles (typical):**
- **Account executive (AE):** owns the commercial relationship, deal, pricing, contract. Revenue goal.
- **Customer success manager (CSM):** owns long-term health, adoption, renewal, ongoing relationship.
- **Deployment strategist / engagement manager:** owns business problem definition, use-case strategy, stakeholder management, project plan, change management (title varies by company, e.g. Palantir's "deployment strategist").
- **FDE:** owns the technical solution: architecture, building, integration, technical credibility, and field feedback to product.

**How to work well:**
1. **Agree roles and a single account plan** up front: goals, stakeholders, risks, who talks to whom.
2. **Share information constantly:** a shared channel, account notes in the CRM, short internal syncs before and after customer meetings.
3. **Present a united front:** disagree internally, not in front of the customer.
4. **Support each other:** join sales calls for technical credibility; give the AE honest feasibility input before they commit; tell the CSM about adoption risks early; hand expansion signals to the AE.
5. **Respect boundaries:** don't negotiate price or make contractual promises; the AE shouldn't promise features without checking with you.

**Sample phrasing:**
> To the AE before a call: "Quick prep: I'll cover the architecture and security questions. If they ask about on-prem pricing, that's yours. One heads-up: their IT lead is sceptical about data residency, so I'll bring the region diagram."

**Pitfalls:** treating sales as the enemy; working in a silo so the CSM learns about problems from the customer.

### What's the difference between a POC, a pilot, and a production deployment?

**What they're testing:** precise understanding of stages, purposes and exit criteria, and that you design each stage to lead to the next.

| | POC (proof of concept) | Pilot | Production deployment |
|---|---|---|---|
| **Question answered** | Can it work technically on our data? | Does it deliver value for real users in a real workflow? | Run it at scale as part of the business |
| **Users** | Mostly the build team + a few SMEs | Small real user group (e.g. 10-50) | All intended users |
| **Data** | Sample or historical, sometimes anonymised | Real, live data, limited scope | Full live data |
| **Duration** | 2-6 weeks | 1-3 months | Ongoing |
| **Engineering** | Prototype, minimal hardening | Integrated, secured, monitored, but limited | Full: SSO, SLAs, scaling, monitoring, support, DR |
| **Security review** | Light | Usually required | Full |
| **Success measure** | Technical metrics (accuracy, latency) | Adoption + business metrics vs. baseline | Ongoing ROI, reliability, adoption |
| **Commercial** | Often free or low-cost | Paid pilot, ideally with pre-agreed production terms | Contract/subscription |

**Sample phrasing:**
> "The POC tells us whether the technology works on your data. The pilot tells us whether your people use it and the business numbers move. Production is when it becomes part of how you operate. I'd like us to agree now what result in each stage triggers the next, so we don't end up in a pilot that never ends."

**Pitfalls:** "pilot purgatory": pilots with no exit criteria or production plan.

### How do you turn a pilot into a production contract?

**What they're testing:** designing a pilot to convert, and partnering with the AE to close.

**Approach:**
1. **Design for conversion from day one:**
   - Agree success criteria and the decision process ("if we hit X, you move to production") before the pilot starts.
   - Ideally, pre-agree production pricing and terms in the pilot contract.
   - Use real users and real data so results are credible.
   - Start security, legal and procurement reviews during the pilot, not after.
2. **During the pilot:** measure against baseline continuously, fix adoption issues fast, collect user quotes and stories, keep the sponsor and economic buyer informed.
3. **Build the business case** with the champion: results, projected ROI at full scale, rollout plan, risks addressed. Make it easy for the champion to sell internally (a one-page summary and a deck they can present).
4. **Present results** to the decision-makers in a formal readout, with the AE leading the commercial conversation.
5. **Provide a production plan:** timeline, integrations, support model, so the decision feels low-risk.
6. **Address objections** early: cost, security, change management.

**Sample phrasing (at the pilot readout):**
> "We agreed at the start that success meant 85% accuracy, 70% weekly adoption, and a 25% cut in handling time. We hit 91%, 78%, and 31%. At full scale across 400 agents, that's about $2.1M a year in capacity. Here's the rollout plan: three waves over ten weeks, starting with the teams who are already asking for it. The security review is already approved for production. What else would you need to see to make a decision this quarter?"

**Pitfalls:** no agreed criteria; starting procurement only after the pilot; relying on the champion alone without access to the economic buyer.

### How do you handle a customer who wants to churn?

**What they're testing:** calm, curious response; separating fixable problems from real fit issues; acting with the CSM/AE.

**Approach:**
1. **Coordinate internally first** with the CSM and AE; agree who leads.
2. **Listen and understand the real reason:** value not proven, poor adoption, budget cuts, champion left, competitor, product gaps, bad experience (outages, support).
3. **Don't get defensive.** Acknowledge what went wrong.
4. **Show the data:** usage, outcomes achieved. Sometimes the value is there but invisible to the new decision-maker.
5. **Propose a concrete recovery plan** for fixable issues: e.g. a 60-day plan with specific goals, extra support, fixing key gaps, re-training users, right-sizing the contract.
6. **If it's a genuine poor fit,** be gracious and help them exit cleanly (data export, transition). A good exit protects reputation and future business.
7. **Capture lessons** for product and the account team.

**Sample phrasing:**
> "Thank you for telling us directly. Before anything else, I want to understand what's not working. What would have needed to be different for this to be worth it?"
>
> "You're right that the first three months were rough, especially the outage in June. What I'm hearing is that the team stopped using it after that. Here's what I'd propose: give us 60 days. We'll fix the two workflow issues your team raised, run refresher sessions, and agree a target: if weekly usage isn't back above 60% and handling time isn't down 20% by the end of that period, we'll help you wind down with no friction. Would that be fair?"

**Pitfalls:** discounting immediately without fixing the cause; arguing; trying to retain a customer who's a genuine poor fit.

### Tell me how you'd approach your first 30/60/90 days on a new customer account.

**What they're testing:** a structured plan that balances learning, relationship-building and early delivery.

**Days 1-30: Learn and build trust**
- Read everything internally: contract/SOW, account plan, previous notes, support tickets, sales promises.
- Meet the account team (AE, CSM) and agree roles.
- Meet customer stakeholders: sponsor, champion, users, IT/security. Map who decides and who could block.
- Discovery: their goals, current workflows, data, systems. Shadow end users.
- Get access set up (environments, data, accounts) and start the security review.
- Agree success criteria and a project plan.
- Deliver one **quick win** (e.g. a prototype on their data, or fixing an existing pain point) to build credibility.

**Days 31-60: Deliver the core**
- Build the first end-to-end version and get it in front of real users.
- Weekly status updates and demos; tight feedback loop.
- Measure against the baseline.
- Address adoption and integration issues; build relationships with champions among users.
- Start feeding product feedback to the core team.

**Days 61-90: Prove value and expand**
- Move from pilot toward production (hardening, monitoring, documentation, support model).
- Present measured results to the sponsor and leadership.
- Identify the next use cases and expansion opportunities with the AE.
- Document and make the deployment maintainable; capture reusable components and playbooks.
- Agree the plan for the next quarter.

**Sample phrasing (to the sponsor in week 1):**
> "In my first month I mostly want to listen: meet your team, watch how the work happens today, and agree with you what success looks like by the end of the quarter. I'd also like to give you something useful early, so if there's a quick problem we can solve on real data in the first few weeks, I'd love to hear it."

**Pitfalls:** starting to build before understanding; no early win; not meeting IT/security until they become a blocker.

---

## Role-play tips

These are guidance rather than questions; a quick checklist for live role-plays:

- **Listen first and paraphrase:** "So what I'm hearing is... is that right?"
- **Acknowledge feelings without over-promising:** "I understand why that's frustrating" is not the same as "We'll fix it by tomorrow."
- **Ask what matters most:** "Of everything we've discussed, what outcome matters most to you?"
- **Give options with trade-offs, recommend one:** "We could do A, B, or C. I'd recommend B because..."
- **Agree concrete next steps and follow up in writing:** "I'll send a summary today with owners and dates."
- **Bonus:** stay calm, keep sentences short, never blame your colleagues or the customer's team, and never promise what you don't control.
