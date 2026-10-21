# My Story Bank

Twelve stories from my own work, written in STAR(+L) form. Almost every behavioral question can be answered with one of them. [MY-ANSWERS.md](MY-ANSWERS.md) maps each question to the best story.

How to use:
- Learn the **shape** of each story, not a script. Practise a 60-second version and a 2-minute version.
- `[I-xx]` marks a detail to fill in from [INPUTS-NEEDED.md](INPUTS-NEEDED.md). Don't use a story in an interview until its result has a real number or outcome.
- Say "I" for what you did and "we" for what the team did. Interviewers listen for this.

| # | Story | Grade | Best for |
|---|---|---|---|
| S1 | Bridging legacy and new platforms while both stayed live | [confirmed] | Proudest project, technical depth, ambiguity, architecture trade-offs |
| S2 | Turning discount chaos into a rules engine | [confirmed] | Working with business users, translating requirements, saying no |
| S3 | Billing work tied to cash collection | [confirmed] design, outcome [needs input] | Business impact, metrics, ROI |
| S4 | The reconciliation I should have built first | [confirmed] lesson, details [needs input] | Failure, mistake, what you'd do differently |
| S5 | Buy, don't build: speech-to-text | [partial] | Data-driven decisions, AI, cost trade-offs, compliance |
| S6 | Acting on hard feedback about my communication | [confirmed] | Weakness, critical feedback, growth |
| S7 | Moving every user off a dying stack | [confirmed] | Legacy modernisation, risk management, production change |
| S8 | Access control that matched how the business really works | [confirmed] | Understanding the user's real need, attention to detail |
| S9 | Building the team with written onboarding plans | [confirmed] | Mentoring, leadership, helping others succeed |
| S10 | Writing down what was holding us back | [needs input] | Influencing without authority, disagreeing, pushing back |
| S11 | Shipping a new language to schools with no internet | [needs input] | Constrained environments (FDE gold), end users, creativity |
| S12 | Phased rollout across two national tracks | [confirmed] | Prioritisation, dependencies, ambiguity, planning |

---

## S1 — Bridging legacy and new platforms while both stayed live

**Grade:** [confirmed] · **Use for:** proudest project, hardest technical problem, architecture decision, ambiguity, ownership, "design a migration" follow-ups.

- **Situation:** Our business was moving from a long-lived legacy MySQL estate to new Postgres microservices. The legacy side was split by product: each of six product lines had its own order tables, student breakdowns and pricing tables. Several legacy products still had to read users and subscriptions from the old database and couldn't be rewritten in time.
- **Task:** I owned the migration design. Old and new had to run side by side with no downtime for the products schools were using.
- **Action:**
  - I **separated two flows that people kept treating as one**. One was a one-time historical backfill (legacy → new, from a read-only snapshot, re-runnable during the migration window). The other was a continuous forward sync (new → legacy), so legacy products kept working unchanged.
  - I designed the **normalisation**: product-partitioned table families collapsing into one generic order model (orders, items, item details, configuration). This included a separate specification just for deriving one JSON configuration column correctly.
  - I built in **operational safety**. An idempotency guard stopped double-porting records that had already arrived through the CRM path. Strict ordering (organisations → orders → users and subscriptions). Retries, monitoring and a rollback plan.
  - I wrote **seven specifications** precise enough that other engineers could implement them without me in the room.
- **Result:** Legacy products kept serving throughout. [I-01: what was migrated, in shareable terms (e.g. "N years of orders across six products"); any incident or drift numbers; timeline.]
- **Learning:** Build reconciliation from day one (see S4). Write the guard conditions before the happy path.

**Likely follow-ups:** How did you validate correctness? *(Row counts and checksums per product, sample-based field comparison, reconciliation reports [I-02].)* What would you do differently? *(Reconciliation and drift alerts on day one, plus a dry-run mode for the backfill.)* Why not a big-bang cutover? *(Too many legacy readers to change at once; a reversible, gradual move was safer.)*

---

## S2 — Turning discount chaos into a rules engine

**Grade:** [confirmed] · **Use for:** working with non-technical stakeholders, translating business rules into systems, customer focus, pushing back.

- **Situation:** Sales gave discounts on school orders in inconsistent ways. Finance had no reliable way to audit who approved what, and some deals were even sold above list price.
- **Task:** Design how discounts and approvals would work in the new commercial platform.
- **Action:**
  - I worked with sales leadership and finance to turn informal habits into explicit rules.
  - I designed a **four-tier approval ladder**: small discounts self-approved, larger ones escalating through a regional sales manager and up to C-suite sign-off for the largest. There was also automatic approval for qualifying large orders.
  - I modelled the commercial details explicitly: list price versus selling price (including above-list sales), contract term from start and end dates, minimum guarantees. I raised an open question on currency conversion for international orders and set out the options (convert in place, or add a column).
- **Result:** Every discount became auditable with a clear approver. [I-03: adoption, approval turnaround, reduction in disputes or margin leakage — any shareable outcome.]
- **Learning:** The hardest part wasn't the code. It was getting sales and finance to agree on one version of the rules. I now start these projects with a rules workshop and a decision table everyone signs off.

---

## S3 — Billing work tied to cash collection

**Grade:** [confirmed] design · [needs input] outcome · **Use for:** business impact, metrics, "how did you measure success", ROI.

- **Situation:** Slow invoice reconciliation was delaying cash collection. Payments were hard to match to invoices.
- **Task:** As tech lead for invoicing, improve the invoice-to-payment flow.
- **Action:** I designed structured proof-of-payment capture and oldest-invoice-first payment mapping. I integrated invoice creation directly with the accounting system from our platform instead of entering invoices by hand.
- **Result:** The goal was to reduce days-sales-outstanding (how long cash takes to come in) to an organisation-wide target. [I-04: did it move? Give a shareable before/after or a percentage.]
- **Learning:** Tie technical work to a metric the CFO already tracks. It makes prioritisation and buy-in much easier.

---

## S4 — The reconciliation I should have built first

**Grade:** [confirmed] lesson · [needs input] specifics · **Use for:** failure, mistake, "what would you do differently", production issue.

- **Situation:** During the migration (S1), we ran the forward sync without automated reconciliation between old and new.
- **Task:** Keep both systems consistent while products stayed live.
- **Action:** When the first data drift showed up [I-05: what drifted, how it was noticed, impact], I traced the cause, fixed the affected records, and then built reconciliation checks and drift reporting.
- **Result:** We caught later drift early, before users noticed. [I-05: frequency or scale, in shareable terms.]
- **Learning:** *"Instrument reconciliation earlier. We built it in response to the first drift we found."* Now I treat reconciliation as part of the migration design, not an add-on.

---

## S5 — Buy, don't build: speech-to-text

**Grade:** [partial]. A colleague authored the specification and I contributed, so say **"I contributed to"** or **"we"**. [I-06: confirm exactly what your part was.] · **Use for:** data-driven decisions, AI experience, cost trade-offs, compliance, "two-way door" decisions.

- **Situation:** We needed speech-to-text for field visit notes, including Indian languages and mixed Hindi-English speech.
- **Task:** Choose how to deliver it.
- **Action (team, with my contribution):** We compared four delivery models: managed APIs, self-hosted open-source models on cloud VMs, a custom GPU service, and in-browser on-device. We corrected an inconsistent usage estimate before modelling cost. We built a break-even analysis that included engineering time. We treated India's data-protection law as a hard constraint and benchmarked an India-based provider for that reason. We stated non-goals honestly, such as perfect accuracy on mixed-language speech.
- **Result:** The recommendation was to **buy, behind a thin internal adapter**: a reversible decision, because self-hosting only paid off at several times our volume, and only if engineering time were free.
- **Learning:** Fix the input numbers before modelling. Make vendor decisions reversible with an adapter layer.

---

## S6 — Acting on hard feedback about my communication

**Grade:** [confirmed] · **Use for:** weakness, critical feedback received, self-awareness, growth.

- **Situation:** In a 1:1 review, my communication goal was marked as not yet met. My emails and updates were too long and sometimes not double-checked.
- **Task:** Fix it measurably, not just "try harder".
- **Action:** I set concrete rules: a 2–3 sentence limit for status emails, a format template, double-checking before sending, a 30-minute weekly block to review my sent emails, and naming the person and the action they need to take.
- **Result:** [I-07: what changed — feedback from your manager, fewer follow-up questions, reports others relied on.] Over the following year I wrote the monthly programme report for seven consecutive months.
- **Learning:** Treat a soft skill like an engineering problem: set rules, measure, review.

---

## S7 — Moving every user off a dying stack

**Grade:** [confirmed] · **Use for:** legacy modernisation, managing risk in production, learning an unfamiliar codebase.

- **Situation:** One product still ran partly on an old PHP 5.3 stack that was becoming impossible to maintain and secure.
- **Task:** Move all users to the PHP 7.3 version and decommission the old servers.
- **Action:** [I-08: how you planned it (batches? feature parity checks? rollback?), what broke, how you communicated with schools and support.] In the same period I automated content porting between products, with a documented process, a data-flow diagram and database triggers for ID mapping.
- **Result:** Old servers retired. [I-08: users moved, downtime, issues.]
- **Learning:** This was my first live migration, and the instincts carried straight into the much larger migration in S1.

---

## S8 — Access control that matched how the business really works

**Grade:** [confirmed] · **Use for:** understanding the user's real need, attention to detail, "a time you found the real requirement".

- **Situation:** Sales and academic staff needed to see school onboardings in a new portfolio view. The obvious design was to scope each user by one role.
- **Task:** Design access rules that worked for real users.
- **Action:** I talked through how teams actually work. A school has both an assigned salesperson and an assigned academic consultant, and some people hold both roles. I designed scoping as a **union**: leadership sees everything, everyone else sees records where they're the salesperson *or* the consultant. I enforced it everywhere: list views, the KPI counts on the dashboard (which most implementations get wrong), and the detail and write paths (forbidden on unassigned records).
- **Result:** One consistent view of "my schools" with no count mismatches between cards and lists. [I-09: any adoption or support-ticket outcome.]
- **Learning:** Access rules should follow how the organisation works, not the tidy org chart.

---

## S9 — Building the team with written onboarding plans

**Grade:** [confirmed] · **Use for:** mentoring, helping a teammate succeed, leadership, scaling a team.

- **Situation:** The team grew from a few full-time and contract engineers. New graduates were joining a complex legacy and commercial codebase.
- **Task:** Get people productive fast and growing.
- **Action:** I wrote 30-day and 90-day onboarding plans for the school-site and order systems. I placed new graduates deliberately in areas that matched their strengths. I coached one engineer on prioritisation and managing their work. I set a quality bar: daily grooming, pull-request reviews, and a QA walkthrough before every build handover.
- **Result:** [I-10: how quickly people became productive, who grew into ownership, team size now.] I now lead a squad of four engineers and am tech lead for three business-critical modules.
- **Learning:** Onboarding is a product. Writing it down once saves repeating it for every hire.

---

## S10 — Writing down what was holding us back

**Grade:** [needs input] · **Use for:** influencing without authority, disagreeing with leadership, pushing for change.

- **What I have:** I wrote a long internal document naming the organisational "choking points" slowing the team down. It was an organisational critique, not a bug list.
- **What I need from you [I-11]:** What were the 2–3 main points? Who did you send it to? What changed as a result? Was there pushback, and how did you handle it?
- **Draft shape:** *Situation:* repeated delays traced to process, not code. *Action:* gathered evidence, wrote it up constructively with proposed fixes, discussed with [leadership]. *Result:* [changes adopted]. *Learning:* criticism lands when it comes with data and a proposed fix.

---

## S11 — Shipping a new language to schools with no internet

**Grade:** [needs input] · **Use for:** constrained or air-gapped environments (very relevant for FDE), end-user empathy, creative delivery.

- **What I have:** I led delivery of a product's tenth language, including distribution to schools without reliable internet via offline SD cards. I also handled localisation for another country (decimal and thousand separators).
- **What I need from you [I-12]:** How did offline distribution work (content packaging, updates, syncing results back)? What went wrong? How many schools or students, in shareable terms?
- **Why it matters:** FDE interviewers love "deployed where the network didn't exist" stories. It maps directly to air-gapped and on-prem questions.

---

## S12 — Phased rollout across two national tracks

**Grade:** [confirmed] · **Use for:** prioritisation, dependency management, ambiguity, planning large programmes.

- **Situation:** The new commercial platform had to roll out to two different national business tracks with different needs, while legacy systems were still live.
- **Task:** I owned product clarity end to end: what ships when, in what order, and what has to migrate first.
- **Action:** I mapped dependencies between modules and the migration (organisations before orders before users), split the rollout into phases, wrote build and release plans and landing plans, and reported progress monthly to leadership.
- **Result:** [I-13: phases delivered, on time or not, what slipped and why.]
- **Learning:** Dependency maps and written landing plans are the cheapest way to stop three teams from blocking each other.
