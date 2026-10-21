# 14 — Questions to Ask Your Interviewers: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** For each question: why it's a good question, what a good vs red-flag answer from the interviewer sounds like, and when to ask it. Adapt the wording to your own voice, pick 2–3 per interview, and always follow up on what you hear ("Can you give me an example?"). Practise asking them out loud so they sound natural, not read.

**General rules**
- Match the question to the interviewer: role logistics to the recruiter, team strategy to the hiring manager, day-to-day reality to engineers/FDEs.
- Don't ask what's on the careers page. Do reference something you learned earlier in the loop.
- Listen for specifics. Vague answers ("it depends", "we're figuring it out") on core topics are a signal — not always bad at a startup, but probe.
- Leave 5 minutes for questions; have backups in case earlier interviewers already answered your first choice.

## About the role

### What does a typical week look like for an FDE here?

**Why it's good:** "FDE" means very different things across companies (pre-sales demos vs production engineering vs project management). This exposes the real job.

**Good answer:** concrete activities with rough proportions — "Monday customer sync, two or three days building integrations and evals, a day on internal feedback and code review, some weeks on-site."

**Red flags:** mostly demos and RFP responses (it's sales engineering under another name); mostly ticket-taking with no customer ownership; "every week is a fire drill"; can't describe it at all.

**When to ask:** early in the process (recruiter or first FDE interviewer). Follow up with "and what did last week look like for you?"

### How is time split between customer work, coding, and travel?

**Why it's good:** tests whether the role stays technical and whether travel matches your constraints. Shows you're thinking realistically.

**Good answer:** specific numbers and how they change by deployment phase — "around 50–60% building, 25% customer-facing, travel about 20%, heavier at kickoff."

**Red flags:** coding is under ~20% when the role was sold as engineering; travel much higher than the job posting said; no one knows.

**When to ask:** recruiter screen or hiring manager — before you invest in a long loop if travel is a hard constraint.

### How many customers does an FDE work with at once?

**Why it's good:** reveals depth vs breadth, workload and context switching. One embedded customer vs six light-touch accounts is a completely different job.

**Good answer:** a clear number and rationale — "One major deployment plus one or two smaller ones; we cap it so people can go deep."

**Red flags:** "As many as come in" or a high number (e.g. 8–10) with no support — signals burnout and shallow, support-style work.

**When to ask:** hiring manager or a current FDE (compare their answers).

### How long does an FDE usually stay on one deployment?

**Why it's good:** tells you whether you'll see outcomes through to production and how deep you'll go in a domain. Also shows how handover works.

**Good answer:** a typical range plus a defined handover — "Three to six months to production, then we transition to a lighter-touch support model with runbooks."

**Red flags:** "Until the customer stops asking" (no exit plan, risk of becoming permanent staff augmentation); or "two weeks" when you want to build real systems.

**When to ask:** with FDE interviewers or the hiring manager.

### What does success look like in the first 30/60/90 days?

**Why it's good:** shows you think about ramp-up and outcomes; gives you a concrete onboarding picture and the manager's real expectations.

**Good answer:** structured milestones — "30: onboarding, shadow a deployment, ship a small fix. 60: own a workstream on a live customer. 90: lead a scoped deployment or major feature with the customer."

**Red flags:** "Hit the ground running" with no onboarding; expectations that are vague or unrealistic (owning three customers solo in month one).

**When to ask:** hiring manager. You can later use their answer in your thank-you note.

### How is FDE performance measured?

**Why it's good:** incentives drive behaviour. Tells you whether you're measured on customer outcomes, revenue, utilisation hours, or product contributions.

**Good answer:** a balance of customer outcomes (deployment live, adoption, measurable value), customer satisfaction/renewals, engineering quality, and contributions back to the product.

**Red flags:** only billable hours or utilisation (consulting-shop dynamics); only revenue (pressure to oversell); "we don't really have a process".

**When to ask:** hiring manager. Polite with a recruiter too.

### What's the hardest part of this role that candidates underestimate?

**Why it's good:** honest, signals maturity, and gets a real answer most people won't volunteer. Gives you material to show self-awareness later.

**Good answer:** a candid, specific challenge plus how the team supports it — "Context switching and being the face of the company when the product breaks; we have a strong escalation path and pair new FDEs with a senior."

**Red flags:** "Nothing really"; or a challenge with no support ("you're on your own when things break at 2am").

**When to ask:** any FDE or hiring manager — works well as a closing question.

## About the team & product

### How does FDE feedback make it back into the core product?

**Why it's good:** the best FDE orgs are a product discovery engine. Shows you care about leverage, not just one-off delivery.

**Good answer:** a defined mechanism — "We file structured feedback, there's a weekly FDE–product sync, PMs triage it, and FDEs can contribute directly to the core repo."

**Red flags:** "We send it to product and hope"; FDEs can't touch core code; product sees FDEs as a support function.

**When to ask:** hiring manager, engineers, or a PM if you meet one.

### Can you give an example of a product feature that started as FDE work?

**Why it's good:** a concrete test of the previous question — anyone can describe a process; real examples prove it works.

**Good answer:** a named feature or tool with a story — "Our connector framework started as a script an FDE wrote for three customers; it's now a product with its own team."

**Red flags:** can't name one; or the example is very old and nothing since.

**When to ask:** right after the previous question, or alone with a senior FDE or manager.

### How do FDEs work with deployment strategists / account execs / product managers?

**Why it's good:** role boundaries determine your day. Clarifies who owns the customer relationship, scope and commercial decisions.

**Good answer:** clear ownership — "The AE owns the commercial relationship, the strategist/PM owns the use case and success metrics, the FDE owns the technical delivery; we scope jointly."

**Red flags:** FDEs routinely get handed scope sales promised without technical input; blurred ownership and lots of blame when things slip.

**When to ask:** hiring manager or an FDE. At Palantir, where engineers typically work alongside Deployment Strategists, ask specifically how that pairing works.

### What does the FDE team's on-call or escalation process look like?

**Why it's good:** production deployments break. Reveals operational maturity and work–life balance.

**Good answer:** a rotation, runbooks, clear severity levels, escalation into core engineering, and customer-specific support SLAs that are realistic.

**Red flags:** "Whoever owns the customer is always on call"; no escalation path to core engineering; frequent weekend fires described as normal.

**When to ask:** engineers or FDEs (they'll be candid); hiring manager for the policy view.

### What tools and stack do FDEs use day to day?

**Why it's good:** practical, helps you prepare, and shows how much is in-house platform vs customer-specific tech.

**Good answer:** specific — "Python and TypeScript, our SDK and platform, Postgres/Snowflake on the customer side, Terraform for infra, an internal eval harness, plus whatever the customer runs."

**Red flags:** heavy manual work with no internal tooling; FDEs barred from writing real code (only configuration).

**When to ask:** engineer interviewer. Good warm-up question.

### How large is the FDE team and how is it growing?

**Why it's good:** tells you about the company's commitment to the function, your chance to shape it, and how much mentorship exists.

**Good answer:** numbers and a plan — "We're 15 FDEs across two regions, hiring to 30 this year, adding team leads and specialisations by industry."

**Red flags:** very rapid growth with no senior people (you'll be onboarding yourself); or a shrinking team (customers moving to self-serve, or the function being cut).

**When to ask:** recruiter or hiring manager.

## About customers

### What kind of customers will I work with first?

**Why it's good:** sets expectations for industry, size, technical maturity, and whether you'll need a clearance or travel.

**Good answer:** specific segment and the reason — "Probably mid-size financial services customers, because that's where we're expanding and your data background fits."

**Red flags:** "We'll see" for the whole first year; or only the most chaotic accounts get given to new hires.

**When to ask:** hiring manager, near the end of the loop.

### What's a recent deployment you're proud of?

**Why it's good:** people light up describing good work; you learn what "great" looks like here and what the team values.

**Good answer:** a story with the problem, what the team built, and a measurable outcome — and credit to the customer and team.

**Red flags:** can't think of one; pride is only about the size of the contract, not customer value.

**When to ask:** any FDE or engineer. Great question to build rapport.

### What's a deployment that went badly, and what did the team learn?

**Why it's good:** shows you're realistic and tests the culture — blameless learning or finger-pointing.

**Good answer:** candid story, clear root cause (scope, data access, misaligned expectations), and a concrete change they made (e.g. a scoping checklist, earlier security review).

**Red flags:** "We haven't had one"; blaming the customer or another team without reflection.

**When to ask:** with someone you've built rapport with — hiring manager or senior FDE. Phrase it lightly: "...if you're comfortable sharing."

### What's the most common reason deployments stall?

**Why it's good:** shows you think about the real bottlenecks (security reviews, data access, unclear ownership, change management), and you learn what you'll fight against.

**Good answer:** specific and with a mitigation — "Data access and security review. We now start those in week one and have a pre-approved architecture pack."

**Red flags:** "The product isn't ready" (you'll be patching core gaps constantly); or no awareness at all.

**When to ask:** FDEs or hiring manager. Pairs well with talking about your own experience of getting a blocker unstuck.

## About growth

### What career paths do FDEs take here (PM, core eng, leadership, founders)?

**Why it's good:** shows long-term thinking and checks the role isn't a dead end.

**Good answer:** real examples of people who moved into each path, and support for doing it — "Two FDEs became PMs last year, several lead FDE pods, a few left to found companies."

**Red flags:** nobody has moved anywhere; or high turnover with people leaving the company rather than growing.

**When to ask:** hiring manager or recruiter. Avoid sounding like you want to leave the role quickly.

### How is learning and mentorship structured?

**Why it's good:** FDEs face new domains constantly; structured support speeds ramp-up and signals a healthy team.

**Good answer:** onboarding program, shadowing on a live deployment, a named mentor/buddy, internal knowledge base of past deployments, regular knowledge-sharing.

**Red flags:** "You learn by doing" with nothing else; no documentation from past deployments.

**When to ask:** any interviewer; especially recent hires.

### What would make someone exceptional in this role after a year?

**Why it's good:** tells you the real bar and what's rewarded beyond basic competence. Better than "what makes someone successful".

**Good answer:** specific behaviours — "They own a major customer end to end, the customer trusts them as an advisor, and they've shipped something reusable that other FDEs use."

**Red flags:** only "works really hard / long hours"; purely sales metrics.

**When to ask:** hiring manager. You can reference it in your thank-you note.

## To the hiring manager

### What's the biggest challenge your team is facing right now?

**Why it's good:** shows you want to help solve real problems; reveals priorities and whether you'd be walking into a mess.

**Good answer:** honest and specific — "Scaling: demand is ahead of headcount, so we need repeatable deployment playbooks." Bonus: you can then link your experience to it.

**Red flags:** evasive answers; or serious issues described casually (leadership churn, product not working for customers, attrition).

**When to ask:** hiring manager. Follow up: "How would this hire help with that?"

### What would you want me to accomplish in my first quarter?

**Why it's good:** frames you as already in the role; gives concrete expectations; useful for your 30/60/90 thinking.

**Good answer:** concrete and realistic — "Get fully onboarded, own one workstream on [customer], and ship [specific thing]."

**Red flags:** no idea; or extremely unrealistic expectations for a new hire.

**When to ask:** hiring manager, later in the conversation.

### Is there anything in my background that gives you pause? (Then address it.)

**Why it's good:** surfaces concerns while you can still respond, instead of them being decided in the debrief. Shows confidence and openness to feedback.

**Good answer:** a direct, specific concern — "You haven't worked much with external customers." Then **you address it** briefly with evidence: "Fair — my customers have been internal, but [example of working with users, handling a tough stakeholder]. I'd also..."

**Red flags:** none for you if they say "no concerns" — just thank them. A defensive reaction from you is the risk; stay calm and factual.

**When to ask:** at the end of a hiring manager conversation where rapport is good. Skip it if time is very short or the interview went awkwardly.

## To an engineer interviewer

### What's something you shipped recently that you're excited about?

**Why it's good:** builds rapport; people like talking about their work. Shows what real projects look like and how much ownership engineers have.

**Good answer:** a specific project they owned, with customer impact and clear enthusiasm.

**Red flags:** struggles to name anything; all work is maintenance, firefighting or blocked.

**When to ask:** with any engineer, especially at the end of a technical round.

### What surprised you most when you joined?

**Why it's good:** gets an honest, unscripted view of culture versus the pitch.

**Good answer:** a genuine surprise, ideally positive with some nuance — "How much autonomy FDEs have; also how much of the job is navigating customer IT."

**Red flags:** surprises that are all negative (process chaos, lack of support), or a clearly rehearsed answer.

**When to ask:** with recent joiners (under ~1 year).

### What do you wish you'd known before starting as an FDE?

**Why it's good:** practical advice you can use, and shows you're already thinking about doing the job well.

**Good answer:** concrete tips — "Get security review and data access started on day one", "Write everything down; customers forget what they agreed", "Ship a v0 early."

**Red flags:** "That it's basically support"; regret about the role itself.

**When to ask:** with FDE interviewers, ideally as your final question. Note their advice and mention it in your thank-you note.
