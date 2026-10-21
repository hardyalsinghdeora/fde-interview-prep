# 01 — Recruiter Screen & Motivation: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** These are reference answers to adapt to your own background — replace every [bracketed placeholder] with real facts. Never claim experience you don't have. Say each answer out loud; most should take 30–90 seconds in a recruiter screen.

## Motivation & role fit

### Tell me about yourself / walk me through your resume.

**What they want:** a 60–90 second narrative that ends with "...which is why this FDE role is my next step." Not a chronological list.

**Structure (present → past → future):**
1. **Now:** current role, one line on what you build, one headline result.
2. **Thread through the past:** 2–3 highlights that show the FDE mix — building, working with users, ambiguity, learning domains fast.
3. **Next:** why this role and company is the logical next step.

**Example (adapt to your own experience):**
"I'm a software engineer at [company], where I build [backend services / data pipelines / LLM features] for [users]. Most recently I led [project], which [result, e.g. cut report generation from two days to 20 minutes for 40 analysts]. Before that I was at [company], where I [worked closely with customers to integrate X] — that's where I realised I enjoy the part where engineering meets a real user's problem. Over the last year I've also been building with LLMs: [project]. I'm looking for a role where I write production code but sit right next to the customer and own the outcome, which is exactly what the FDE role at [company] is."

**Pitfalls:** reading your resume line by line; going over 2 minutes; no clear link to the role.

### Why do you want to be a Forward Deployed Engineer?

**Hit four points:** love of building, love of seeing real users use it, comfort with ambiguity, learning many domains.

**Example (adapt):**
"Three reasons. First, I do my best work when I can see the user. In my [project], I sat with [users] and the thing I built changed because of what I saw — that feedback loop is what I want every day. Second, I like owning outcomes, not tickets. An FDE is accountable for whether the deployment actually works for the customer, which means doing whatever it takes — code, data cleanup, training, debugging their environment. Third, I learn fast and I like variety: every customer is a new domain, new data, new constraints. And on the AI side, the gap between what models can do and what companies actually get out of them is mostly a deployment problem — FDEs close that gap."

**Pitfalls:** "I want to travel" or "I don't like pure coding" as the main reason. It must be a pull toward the role, not a push away from engineering.

### Why FDE instead of a core software engineering role?

**Key message:** you still want to be an engineer — you want the engineering to be anchored to a real customer outcome.

**Example (adapt):**
"I like core engineering and I'll still be writing production code. The difference is the feedback loop. As a core engineer I'd often ship a feature and never learn whether it solved anyone's problem. As an FDE, I'm the one who sees the customer's messy data, their constraints and their reaction, and I adjust in days. I also think FDEs have outsized product influence: you see patterns across customers before anyone else, and the best product features often start as something an FDE hacked together. I want that combination of building and direct impact."

**Pitfalls:** implying you're weaker technically or bored by code. FDE bars for coding are often the same as SWE.

### Why FDE instead of solutions engineering, sales engineering, or consulting?

**Draw the distinctions clearly:**
- **Sales/solutions engineering:** mostly pre-sale — demos, POCs, technical questions to close a deal. FDEs are mostly post-sale and own a production deployment.
- **Consulting:** often advice, slides and recommendations; frequently someone else builds. FDEs build and ship working software, usually on a product platform.
- **FDE:** writes production code, owns outcomes after the contract is signed, and feeds patterns back into the core product.

**Example (adapt):**
"I want to be the person who builds the thing and stays until it works in production. Sales engineering is valuable, but it centres on winning the deal; I'm more motivated by what happens after. Consulting gets you close to the business problem, but often stops at the recommendation. An FDE gets both: deep customer context and the responsibility to ship real code — plus a direct line into the product team."

### What do you think a Forward Deployed Engineer does day to day?

Show a realistic picture, not a glamorous one.

- **Customer work:** discovery calls, workshops, scoping, weekly syncs with customer engineers and business owners, demos.
- **Building:** integrations with the customer's systems (APIs, SSO, data warehouses), data pipelines and cleaning, custom apps or workflows on the platform, LLM prompts/RAG/agents and evals.
- **Debugging in their environment:** network/firewall issues, permissions, bad data, performance.
- **Operations:** monitoring the deployment, handling escalations, writing runbooks.
- **Internal:** writing product feedback and feature requests, contributing fixes upstream, sharing reusable code with other FDEs, sometimes supporting sales on technical scoping.
- **Communication:** status updates, documentation, training users.

"Roughly I'd expect half building, a quarter customer-facing, and the rest debugging, internal feedback and travel — but it shifts by phase of the deployment."

### Why this company? Why now?

**Formula:** product/mission + specific evidence you did homework + why the timing matters + why you specifically fit.

**Example template (adapt):**
"Why [company]: [specific thing about product — e.g. it's one of the few platforms whose customers are running it in production for critical workflows, like X]. I read [blog/case study/eng post] about [deployment], and the [specific detail] is exactly the type of problem I want to work on. Why now: [market timing — e.g. enterprises are moving from LLM pilots to production, and the bottleneck is deployment and integration, not models], and [company stage — e.g. the FDE team is growing, so I can help shape how it works]. And for me personally, my experience in [relevant skill/domain] maps directly to [their customers/use cases]."

**Pitfalls:** generic praise ("great culture, smart people"); anything you could say about any company.

### What do you know about our product/platform?

**Prepare:** what the product does in one sentence, who buys it, 2–3 core components, how customers deploy it, recent launches, notable customers.

**Answer structure:**
1. One-sentence summary in your own words.
2. Main components / how it works.
3. Who uses it and for what (named customer stories if public).
4. Recent news or launches.
5. A thoughtful question or hypothesis: "I'd guess the hard part of deployments is [data integration / evals / security reviews] — is that right?"

**Example (generic for an AI platform, adapt):** "You provide [models via API + an enterprise app + agent tooling]. Customers use it for [customer support automation, coding assistants, document processing]. Recently you launched [X]. From the outside, I'd guess the FDE team's job is turning those capabilities into reliable workflows inside customers' systems — integrating with their data, building evals, and getting through security review."

### Which of our customers or use cases excites you most?

Pick **one specific** public case study and explain *why* with an engineering lens.

**Example template (adapt):** "The [customer] deployment for [use case]. What excites me is [technical challenge — e.g. combining messy unstructured documents with structured data, the latency constraint, the high stakes for accuracy]. It's also a use case where success is measurable — [metric, e.g. handle time, claims processed] — and I like problems where you can prove impact. I've done something adjacent in [your experience]."

**Pitfalls:** picking the biggest logo without a reason; not having read the case study.

### What parts of the job are you excited about, and what do you think you'll struggle with?

**Excited:** be specific — first weeks with a new customer, finding the real problem, shipping a v0 they use, seeing patterns across customers.

**Struggle:** pick a real but manageable challenge and show your plan. Good options:
- Context switching between customers — "I'll rely on strict notes per customer and timeboxing."
- Saying no / scope creep — "My instinct is to please; I'm learning to tie every request to the agreed outcome."
- Domain ramp-up in [industry] — "I'll read their docs and shadow users early."
- Travel/energy management.

**Pitfalls:** "I don't think I'll struggle"; or naming a core requirement as a weakness ("I don't like talking to customers").

### Where do you see yourself in 3–5 years? (FDE → PM? → founder? → eng lead?)

**Key:** show ambition that the role feeds, and commitment to doing the FDE job well first.

**Example (adapt):**
"In the next couple of years I want to be a really strong FDE — someone the team trusts with the hardest, most ambiguous deployments. Beyond that I'm open, but I'm drawn to [leading a pod of FDEs / moving toward product, turning field patterns into platform features]. What I'm sure about is that I want to stay close to customers and to building — and the FDE path is the best training for any of those directions."

Mentioning "founder" is fine at many startups if framed as long-term and honest, but don't make it sound like you'll leave in a year.

### What kind of problems energise you?

**Example (adapt):**
"Problems where there's a real person stuck and the path isn't defined. For example, [story: a team doing a manual process for hours a week, where I had to figure out the data, build the tool, and get them to adopt it]. I like the combination of a messy real-world constraint — bad data, legacy systems, unclear requirements — and a clear finish line where someone's work gets better. I'm less energised by optimising something nobody uses."

Tie it to FDE: messy integrations, unclear requirements, measurable user outcomes.

### Do you prefer breadth or depth? Why?

**Best answer for FDE:** "T-shaped" — breadth by default, with the ability to go deep fast when the problem demands it.

**Example (adapt):**
"I'd describe myself as T-shaped. My default is breadth — I like working across the stack, data, infra and the customer conversation — because most real problems cross those boundaries. But I can go deep when needed: in [example], I had to dig into [Postgres query plans / tokenizer behaviour / network config] to fix [issue]. For an FDE I think that's the right shape: broad enough to own a whole deployment, deep enough not to get stuck."

### How do you feel about context switching between several customers?

**Show:** you're comfortable with it *and* have a system.

**Example (adapt):**
"I'm comfortable with it — in my current role I [support N teams / handle on-call plus feature work]. What makes it work for me: one running notes doc per customer with current status, open questions and next steps, so I can reload context in five minutes; blocking deep-work time for building and batching meetings; and being explicit with customers about response times. The risk is that everything gets shallow attention, so I try to keep a clear priority order and tell stakeholders early if something has to move."

### How do you feel about work that's part engineering, part consulting?

**Example (adapt):**
"I like it — I think the consulting part is what makes the engineering effective. Figuring out what the customer actually needs, defining success and getting buy-in decides whether the code matters at all. In [example] I spent the first week mostly talking to users, and that changed what I built. The thing I'd watch is staying technically sharp, so I'd want to keep writing real production code every week, which this role does."

**Pitfalls:** sounding like you tolerate the consulting part; or sounding like you want to stop coding.

### Have you worked directly with customers/end users before?

**If yes:** give a concrete example — who, what you did, outcome. Mention frequency (weekly calls, on-site).

**If limited:** reframe honestly with adjacent experience: internal users (ops, analysts, support), open-source users filing issues, freelance clients, hackathon users, supporting sales calls, user interviews for a side project.

**Example (adapt):**
"Yes — at [company] I was the engineering point of contact for [customer/internal team]. I ran weekly calls with [their roles], gathered requirements, demoed progress, and handled escalations. One example: they asked for [X], but after watching their workflow I realised they needed [Y], which we built in [time] and which [result]."

### What's the most impactful thing you've built?

Use your **"Biggest technical achievement"** story (see [00-overview/ANSWERS.md](../00-overview/ANSWERS.md)). In the recruiter screen keep it to 60 seconds:

- **Problem** and who had it (one sentence).
- **What you built** and the one key technical decision.
- **Impact** in numbers (time, money, users, reliability).
- **Your role** — say "I".

**Example (adapt):** "I built [system] that [did X] for [users]. Before, [painful baseline]. I designed [key part] using [tech], and the result was [metric]. It's now used by [N] people/teams."

Prefer impact on real users over technical complexity alone.

### Why are you leaving your current role?

**Rule:** positive, forward-looking, never criticise your employer. Frame it as moving *toward* something.

**Example (adapt):**
"I've learned a lot at [company] — especially [skill]. But my role has become [more focused on internal platform work / further from users], and I've realised the work I enjoy most is when I'm directly solving a customer's problem end to end. There isn't really a path to that where I am, and FDE is exactly that role."

If laid off, say so plainly: "My team was affected by a reduction in [month]; it was a business decision. It gave me the chance to be deliberate about my next step, which is why I'm focused on FDE roles."

### What are you looking for in your next role that you don't have now?

Name 2–3 things that this role clearly offers:
- Direct customer contact and owning outcomes.
- Variety of domains and problems.
- Working on [AI / the company's area] at the frontier.
- A team with a high bar where you'll learn fast.

**Example (adapt):** "Mainly closeness to the customer. Right now I'm a couple of steps removed from the people using what I build. I also want more variety — different domains, different stacks — and to be working on AI deployments where the field is moving fast. This role gives me all three."

## Logistics

### Are you comfortable with travel? How much (e.g., 25–50%, weeks on-site)?

**Be honest — this is a real filter.** Know your actual limit before the call.

- Ask first if useful: "What does travel typically look like for this team — and is it front-loaded at the start of a deployment?"
- State a clear number: "Yes, I'm comfortable with up to [X]%, including [week-long on-site stints]. Being on-site early in a deployment is valuable — you learn far more watching users than on video calls."
- If you have constraints, say so with what you *can* do: "I can do up to 25% with notice; longer stretches are harder because of [family commitment], but I'm flexible for key moments like kickoffs and launches."

**Pitfall:** saying "yes, anything" and then declining at offer stage.

### Are you open to relocation / working on-site with customers?

- Know your answer and which cities/regions are acceptable.
- **Example:** "I'm based in [city] and would prefer to stay, but I'm open to relocating to [city/cities] for the right role. Working on-site at customer offices is fine — I see it as part of the job." Or: "I'm not able to relocate, but I'm happy to be on-site with customers regularly."
- Ask: "Is the role tied to a hub office, or to where the customers are?"

### Are you eligible for (or do you hold) a security clearance? (government/defense FDE roles)

Answer factually; never overstate.

- **If you hold one:** level (e.g. Secret, Top Secret, TS/SCI, UK SC/DV), whether it's active, and when it was granted/last reinvestigated.
- **If not, but eligible:** "I don't currently hold one, but I'm a [citizen of X] and [have no issues that would prevent me getting one]. I'm willing to go through the process."
- **If not eligible:** be upfront, and ask whether there are non-cleared customer teams.

Clearance eligibility typically depends on citizenship, background, finances and foreign contacts. Many US government roles require US citizenship.

### Work authorisation / visa sponsorship?

State the facts in one or two sentences:
- "I'm authorised to work in [country] without sponsorship."
- or "I'm currently on [visa type], valid until [date], and I would need [transfer/sponsorship]."
- Ask early whether they sponsor, so neither side wastes time. If you'll need sponsorship in the future (e.g. an OPT/STEM-OPT end date), mention it now.

### What's your notice period / earliest start date?

- Give a specific, honest answer: "My notice period is [X weeks]. I could start around [date]."
- Add flexibility if true: "I may be able to shorten it by agreement" or "I'd like [one week] between roles."
- Don't promise a date you can't keep.

### What are your compensation expectations?

**Goal:** don't anchor low; get the band first; stay collaborative.

**Approach:**
1. **Deflect politely first:** "I'd like to understand the role and level better first. Could you share the band for this position?" In many US states and some countries, the range must be disclosed in the posting.
2. **If pressed, give a researched range** (levels.fyi, posted ranges, peers) based on total compensation, with your target near the bottom: "Based on my research for FDE roles at this level in [location], I'm looking at total compensation in the range of [X–Y], depending on the equity and level."
3. **Keep it open:** "I'm flexible on the mix of base and equity — the role matters most."

**Pitfalls:** stating your current salary (not required, and illegal to ask in some places); a single number; apologising.

### Are you interviewing elsewhere? Where are you in those processes?

- Be honest and brief; it helps them manage timing.
- **Example:** "Yes, I'm in process with a couple of other companies for [FDE / applied AI] roles — one at the final round stage. [Company] is a top choice for me because [reason], so I'd love to keep things moving. Please let me know if timelines get tight on either side."
- Don't name companies unless you're comfortable; don't invent offers. Do tell them promptly if you get an offer with a deadline.

### Which languages and stacks are you strongest in?

Be specific, ranked, and honest; connect to FDE needs.

**Example (adapt):**
"Strongest in **Python** — [N years], used for backend services with [FastAPI/Django], data pipelines with [pandas/Spark/Airflow], and LLM apps with [SDKs, RAG with pgvector]. Solid with **SQL** and **Postgres**, and **TypeScript/React** for internal tools. On infrastructure, [AWS — Lambda, ECS, S3, IAM], Docker and Terraform, with CI on [GitHub Actions]. I'm comfortable picking up whatever a customer runs — I recently [learned X in Y days]."

Frame it as: core strength + data/SQL + cloud + ability to adapt. FDEs need breadth across APIs, data and deployment.

### Are you comfortable in a fast-paced, ambiguous environment with changing priorities?

Say yes **with evidence** and **a method**, using your ambiguity story.

**Example (adapt):**
"Yes — some of my best work came from it. At [company], [stakeholder] asked for [vague goal] with [short deadline]. I wrote down assumptions and a success metric, got quick sign-off, shipped a v0 in [time] and iterated from feedback. When priorities change, I make the trade-off explicit — what we're dropping and why — so nobody is surprised. Ambiguity doesn't bother me as long as I can talk to the people who have the problem."

## AI-company specific motivation

### Why do you want to work on AI deployment specifically?

**Key message:** models are capable; the value gap is in deployment — integration, data, evals, workflow change, trust.

**Example (adapt):**
"Because that's where the bottleneck is. The models can already do a lot more than most organisations get out of them. The gap is in connecting them to the right data and systems, defining what 'good' looks like with evals, handling failures and safety, and redesigning the workflow so people actually use it. That's exactly FDE work. I've seen it myself: [example — e.g. an internal LLM prototype that demoed well but needed retrieval, evals and guardrails before anyone trusted it]. Doing that across many customers is the fastest way to learn what really works with AI in the real world."

### What's a recent AI product or model release that impressed you, and why?

**Pick something you've actually used**, explain what impressed you technically *and* what it unlocks for customers.

**Structure:**
1. What it is (one sentence).
2. What you did with it (hands-on).
3. Why it matters (technical reason).
4. What it unlocks for deployments, plus a caveat (shows judgment).

**Example structure (adapt; update with current releases before the interview):**
"[Model/product, e.g. a recent agentic coding tool or a model with stronger tool use and long context]. I used it to [task — e.g. refactor a service / build an agent that queries our docs and Jira]. What impressed me was [specific — e.g. reliable multi-step tool use, staying on track over long tasks, handling a large codebase in context]. For enterprises, that moves agents from demos to real workflows like [use case]. The caveat is you still need evals, permissions and human review for anything high-stakes."

**Pitfall:** naming something you only read a headline about.

### How have you used LLMs in your own work or side projects?

Give **concrete, hands-on** examples at three levels if you can:

1. **Daily productivity:** coding assistants for refactors, tests, unfamiliar codebases — and how you verify output.
2. **Built with the API:** e.g. "a RAG app over [docs] using [embedding model + pgvector], with chunking by section, citations, and an eval set of [N] questions scored for faithfulness."
3. **Agents / tools / production:** e.g. "an agent that calls [internal APIs] via tool use to [task], with retries, structured outputs (JSON schema), and logging of every step."

Mention what you learned: evals matter more than prompt tweaks; retrieval quality is usually the bottleneck; structured outputs and guardrails make it reliable; cost/latency trade-offs (smaller model for routing, larger for reasoning; prompt caching).

**Example (adapt):** "I built [project] that [does X]. My first version hallucinated on [case], so I added [retrieval/citations/eval set], which took accuracy on my test set from [X] to [Y]."

### What do you think about AI safety / responsible deployment? (Anthropic, OpenAI)

**Show genuine, practical thinking**, both the long-term and the deployment-level view.

**Example (adapt):**
"I take it seriously at two levels. At the frontier level, models are becoming more capable quickly, and I think it's important that labs invest in alignment, interpretability and careful evaluation of dangerous capabilities before release — [for Anthropic: things like the Responsible Scaling Policy and Constitutional AI are concrete examples of putting that into practice].

At the deployment level — where an FDE lives — responsible deployment means:
- Matching autonomy to stakes: human review for high-impact decisions; least-privilege permissions for agents.
- Evals before launch and monitoring after, including for bias and failure modes, not just accuracy.
- Guarding against prompt injection and data leakage, especially when agents read untrusted content.
- Being honest with customers about limitations instead of overselling.

I don't see safety as a brake on adoption. Customers deploy faster when they trust the system."

**Pitfalls:** dismissiveness, or only abstract philosophy with no practical view.

### What's one thing customers get wrong when adopting LLMs?

Pick one and go deep; mention one or two others briefly.

**Strong pick: no evals / no definition of success.**
"The most common mistake is starting without a way to measure quality. Teams build a demo, it looks great on ten hand-picked examples, then they change a prompt or a model and have no idea whether things got better or worse. The fix is cheap: collect 50–200 real examples with expected outputs, define metrics (accuracy, faithfulness, format compliance, escalation rate), automate the run, and gate changes on it. That turns prompt tweaking into engineering."

**Other good ones:**
- Treating it as a chatbot problem instead of redesigning the workflow around it.
- Expecting the model to know their private data without retrieval or tool access.
- Trying to automate 100% instead of starting with human-in-the-loop for the hard cases.
- Underestimating data quality and permissions (RAG over messy, stale or access-controlled docs).
- Picking the most expensive model for everything rather than right-sizing for latency/cost.

### Which do you think matters more for enterprise AI adoption: model quality or integration? Why?

**Take a position, with nuance.**

**Example (adapt):**
"For most enterprise use cases today, integration matters more — with the caveat that model quality sets the ceiling.

- Frontier models are already good enough for a large range of tasks: summarisation, extraction, drafting, classification, many coding and agentic workflows. The bottleneck is rarely raw intelligence.
- What usually decides success is integration: access to the right data with the right permissions, connections into systems of record (CRM, ticketing, ERP), evals, guardrails, latency and cost, and fitting into how people actually work.
- But model quality still matters a lot at the frontier of the use case — long multi-step agentic tasks, complex reasoning, reliable tool use. Better models also simplify integration: less prompt scaffolding, fewer workarounds.

So I'd say: model quality decides *what's possible*; integration decides *what gets adopted*. That's exactly why FDE roles exist."
