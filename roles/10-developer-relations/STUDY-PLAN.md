# Study Plan — Developer Relations

For engineers preparing for Developer Advocate, DevRel Engineer or DX Engineer roles; assumes 6 weeks at about 10–12 hours a week, or the 2-week crash plan if an interview is already booked. Unlike most roles, much of this plan is building a public portfolio, because the portfolio review is a round in itself.

Role overview, loop and model answers: [README.md](./README.md). Questions every role gets: [COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md).

---

## Before you start

Spend 30 minutes rating yourself. Skim what you rate 4–5; spend extra time on 1–2.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| Public portfolio | | You can send a link with two or three talks, posts or videos and two repos that you are proud of. |
| Technical talks | | You can give a 10-minute talk with a clear "one idea", a live demo and a recorded backup, on time. |
| Live coding and demos | | You can build something small with an unfamiliar API in 20 minutes while narrating clearly. |
| Docs and tutorials | | You know the tutorial / how-to / reference / explanation split and can write a tested getting-started guide. |
| API and DX critique | | You can review an API or a signup flow and produce three strengths and five ranked issues with fixes. |
| Community | | You can describe how you would grow a community from zero and respond to a public angry post. |
| Measuring impact | | You can propose metrics across awareness, activation, community and product impact, and say how each can mislead. |
| AI / LLM platform knowledge | | For AI companies: you can demo a model API with an evaluation step and explain RAG, tools and MCP simply. |

---

## 6-week plan

### Week 1 — Understand DevRel and audit yourself

**Goals:** understand what the team you are joining measures, and start your portfolio.

- [ ] Read [The role](./README.md#the-role) and [Typical interview loop](./README.md#typical-interview-loop); find out whether the role sits in engineering, product or marketing.
- [ ] Work through [Measuring DevRel impact](./README.md#measuring-devrel-impact-1): [how to measure impact](./README.md#how-do-you-measure-the-impact-of-developer-relations), [proving ROI to a VP](./README.md#your-vp-asks-you-to-prove-devrels-return-on-investment-what-do-you-show) and [choosing between talk, post, sample app and docs fix](./README.md#with-limited-time-how-do-you-choose-between-a-conference-talk-a-blog-post-a-sample-app-and-a-docs-fix).
- [ ] Read [why DevRel rather than software engineering or FDE](./README.md#why-developer-relations-rather-than-software-engineering-or-a-forward-deployed-role) and [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md).
- [ ] Study the target company's DevRel output: docs, blog, YouTube, forum or Discord, GitHub samples, changelog. Note three gaps.

**Deliverables:**
1. A simple portfolio page (a GitHub profile README is fine) listing what you already have, even if small.
2. A one-page measurement plan for the role: two or three metrics each for awareness, activation, community and product impact.

**Mock:** 30-minute recruiter plus hiring-manager mock: "what is your DevRel philosophy?" and "how would you measure your own impact?".

**End of week check:** you can explain how DevRel creates value for the company in two minutes, with metrics.

### Week 2 — Developer experience critique and API design

**Goals:** pass the DX critique round with a structured, evidence-based review.

- [ ] Work through [Developer experience and API critique](./README.md#developer-experience-and-api-critique-1): [sign up and critique](./README.md#sign-up-for-our-product-build-something-small-and-critique-the-developer-experience), [time to first hello world](./README.md#what-is-time-to-first-hello-world-and-how-do-you-reduce-it), [reviewing an API design](./README.md#review-this-api-design-what-would-you-change), [error messages](./README.md#how-do-you-design-error-messages-that-developers-can-act-on) and [what an SDK should provide](./README.md#what-should-an-sdk-provide-beyond-a-thin-wrapper-over-the-api).
- [ ] Read [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) sections [A (APIs)](../../topics/08-cloud-devops-integration/README.md#a-apis--integration) and [B (auth)](../../topics/08-cloud-devops-integration/README.md#b-authentication-authorization--security), with [answers](../../topics/08-cloud-devops-integration/ANSWERS.md): naming, status codes, pagination, idempotency, versioning, error format.

**Deliverables:**
1. A timestamped friction log from signing up to the target company's product and building something small.
2. A two-page DX critique from that log: three strengths, five ranked issues, a proposed fix and an expected effect for each.

**Mock:** 45-minute DX critique mock: share your screen, go through the signup and quickstart live, and narrate what a new developer would feel.

**End of week check:** you can critique any API or onboarding flow with ranked, actionable findings rather than opinions.

### Week 3 — Docs and tutorials (the build-and-write take-home)

**Goals:** produce a tutorial good enough to ship; this is the most common take-home.

- [ ] Work through [Docs and tutorials](./README.md#docs-and-tutorials-1): [getting-started tutorial](./README.md#write-a-getting-started-tutorial-for-our-product-what-goes-in-it), [tutorial vs how-to vs reference vs explanation](./README.md#what-is-the-difference-between-a-tutorial-a-how-to-guide-reference-docs-and-an-explanation), [keeping samples working](./README.md#how-do-you-keep-code-samples-in-docs-and-tutorials-working-over-time), [reviewing a docs page](./README.md#review-this-documentation-page-and-tell-us-what-you-would-change) and [measuring docs quality](./README.md#how-do-you-measure-whether-documentation-is-good).
- [ ] Read [09-customer-scenarios section C (communication)](../../topics/09-customer-scenarios/README.md#c-communication) for explaining technical ideas to different audiences.

**Deliverables:**
1. A getting-started tutorial for the target company's product (or a public API), tested from a clean machine or container, with the sample code in a repo that runs in CI.
2. A short review of one of the company's existing docs pages with suggested edits.

**Mock:** a peer follows your tutorial without help while you watch silently; note every place they get stuck, then fix it.

**End of week check:** a stranger can complete your tutorial unaided in under 15 minutes.

### Week 4 — Talks and live demos

**Goals:** pass the presentation round and the live demo round.

- [ ] Work through [Content and talks](./README.md#content-and-talks-1): [walk us through your content](./README.md#walk-us-through-a-talk-post-or-video-you-made-why-that-topic-and-format-and-how-did-it-perform), [the 10-minute talk](./README.md#give-a-10-minute-technical-talk-on-a-topic-of-your-choice), [choosing content](./README.md#how-do-you-decide-what-content-to-create), [writing a CFP](./README.md#how-do-you-write-a-conference-talk-proposal-cfp-that-gets-accepted) and [beginners vs experts](./README.md#how-do-you-adapt-the-same-topic-for-beginners-and-for-experienced-developers).
- [ ] Work through [Live demo](./README.md#live-demo-1): [build live in 20 minutes](./README.md#build-something-with-our-api-live-in-20-minutes-while-explaining-what-you-are-doing), [the demo breaks](./README.md#your-live-demo-breaks-on-stage-what-do-you-do) and [preparing a reliable demo](./README.md#how-do-you-prepare-a-demo-so-that-it-works-reliably).
- [ ] Read [05-learning-round](../../topics/05-learning-round/README.md) and its [practice ideas](../../topics/05-learning-round/README.md#practice-ideas): learning an unfamiliar API fast and explaining it back is the core skill.

**Deliverables:**
1. A 10-minute talk with slides, a live demo and a recorded backup video, rehearsed three times with a timer.
2. One CFP-style abstract (title, 150-word abstract, takeaways) for the same talk.

**Mock:** 30-minute presentation round: give the talk to two peers, then 10 minutes of hostile Q&A. Separately, a 20-minute live build with an API you have never used, chosen by the peer.

**End of week check:** you finish the talk within 10 minutes, recover calmly if the demo breaks, and can build live with a new API while narrating.

### Week 5 — Community, feedback to product and AI developer relations

**Goals:** answer strategy questions about community and product influence; prepare for AI platform roles.

- [ ] Work through [Community](./README.md#community-1): [community from zero](./README.md#how-would-you-build-a-developer-community-for-our-product-from-zero), [angry public complaint](./README.md#a-developer-posts-an-angry-public-complaint-about-the-product-how-do-you-respond), [hackathons and workshops](./README.md#how-do-you-run-a-hackathon-or-workshop-that-leads-to-real-adoption) and [champions programs](./README.md#how-would-you-design-a-community-champions-or-ambassador-program).
- [ ] Work through [Feedback to product](./README.md#feedback-to-product-1), especially [balancing developers and the company](./README.md#how-do-you-balance-advocating-for-developers-with-advocating-for-the-company).
- [ ] Work through [DevRel for AI and AI-assisted developers](./README.md#devrel-for-ai-and-ai-assisted-developers-1).
- [ ] For AI companies, read [06-ai-llm](../../topics/06-ai-llm/README.md) sections [A (fundamentals)](../../topics/06-ai-llm/README.md#a-llm-fundamentals), [C (RAG)](../../topics/06-ai-llm/README.md#c-rag-retrieval-augmented-generation), [D (agents and tools)](../../topics/06-ai-llm/README.md#d-agents-and-tool-use) and [E (evaluation)](../../topics/06-ai-llm/README.md#e-evaluation-most-important), with [answers](../../topics/06-ai-llm/ANSWERS.md) and [part 2](../../topics/06-ai-llm/ANSWERS-part2.md).

**Deliverables:**
1. A sample app using the target company's API (for AI companies: a small model-API demo with an evaluation step), published with a README and a short blog post about building it.
2. A one-page 90-day community plan for the company: channels, first three programs, metrics.

**Mock:** 30-minute strategy mock: "how would you build our developer community from zero?" followed by "a developer just posted an angry thread about us; respond now" (written, 10 minutes).

**End of week check:** you have a public sample app and post, and a concrete community plan you can defend.

### Week 6 — Portfolio polish, behavioral and full loop

**Goals:** tie the portfolio together and rehearse every round.

- [ ] Work through [Role-specific behavioral](./README.md#role-specific-behavioral-1), including [content that did not work](./README.md#tell-me-about-a-piece-of-content-that-did-not-work-what-did-you-learn) and [sustaining output without burning out](./README.md#how-do-you-sustain-content-output-and-travel-without-burning-out).
- [ ] Read [10-behavioral](../../topics/10-behavioral/README.md) sections [Conflict and collaboration](../../topics/10-behavioral/README.md#conflict--collaboration) and [Communication](../../topics/10-behavioral/README.md#communication); write four stories into [STORY-BANK.md](../../my-prep/STORY-BANK.md).
- [ ] Practise one practical coding problem from [02-coding section A](../../topics/02-coding/README.md#a-reported-fde-coding-problems) in case of a technical screen.
- [ ] Tick off the [Prep checklist](./README.md#prep-checklist); add DevRel-specific questions to those in [14-questions-to-ask](../../topics/14-questions-to-ask/README.md#about-the-team--product).

**Deliverables:**
1. The final portfolio page: your talk recording, tutorial, sample app, blog post and DX critique, sent to the recruiter before the loop.
2. Four STAR stories: content that worked, content that failed, feedback that changed a product, handling public criticism.

**Mock:** full mock loop over two days: portfolio walkthrough (30 min), talk plus Q&A (30 min), live demo (30 min), DX critique (30 min), behavioral (45 min).

**End of week check:** you can send a portfolio link today and you have rehearsed every round type at least twice.

---

## 2-week crash plan

About 2 hours a day. Focus on one strong talk, one tutorial and one DX critique.

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and loop | Read role and loop; write "why DevRel" | [README](./README.md#the-role), [Why DevRel](./README.md#why-developer-relations-rather-than-software-engineering-or-a-forward-deployed-role) |
| 2 | Company audit | Study their docs, blog, community, samples; note gaps | [Prep checklist](./README.md#prep-checklist) |
| 3 | Friction log | Sign up, build something small, timestamped log | [DX critique](./README.md#sign-up-for-our-product-build-something-small-and-critique-the-developer-experience) |
| 4 | DX critique | Turn the log into three strengths and five ranked issues | [API design review](./README.md#review-this-api-design-what-would-you-change), [08 A](../../topics/08-cloud-devops-integration/README.md#a-apis--integration) |
| 5 | Tutorial | Draft getting-started tutorial; test on a clean machine | [Docs and tutorials](./README.md#docs-and-tutorials-1) |
| 6 | Talk build | Outline and slides for a 10-minute talk with a demo | [10-minute talk](./README.md#give-a-10-minute-technical-talk-on-a-topic-of-your-choice) |
| 7 | Rehearse | Three timed rehearsals; record a backup demo video | [Reliable demos](./README.md#how-do-you-prepare-a-demo-so-that-it-works-reliably) |
| 8 | Mock 1 | Talk plus Q&A in front of a peer | — |
| 9 | Live build | 20-minute live build with an unfamiliar API, twice | [05 practice ideas](../../topics/05-learning-round/README.md#practice-ideas) |
| 10 | Community and impact | Community from zero, angry post, metrics, ROI | [Community](./README.md#community-1), [Measuring impact](./README.md#measuring-devrel-impact-1) |
| 11 | AI (if relevant) | LLM fundamentals, RAG, evaluation; small demo | [06-ai-llm](../../topics/06-ai-llm/README.md#a-llm-fundamentals) |
| 12 | Mock 2 | Live demo plus DX critique mock | — |
| 13 | Stories and portfolio | Four STAR stories; publish portfolio page | [Behavioral](./README.md#role-specific-behavioral-1), [STORY-BANK.md](../../my-prep/STORY-BANK.md) |
| 14 | Light review | One last talk run-through; rest | — |

---

## Daily routine

| Minutes | Activity |
|---|---|
| 0–15 | Read and answer aloud one model answer from this role's README |
| 15–60 | Build or write: work on the week's portfolio piece (critique, tutorial, talk, sample app) |
| 60–80 | Speak: rehearse part of your talk or narrate a 10-minute live build with an unfamiliar API |
| 80–90 | Publish or log: push code, post a draft, or write down what to fix tomorrow |

DevRel prep rewards output. Aim to publish something small every week.

---

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Hiring-manager philosophy and metrics (30 min) | Peer |
| 2 | Live DX critique of the company's product (45 min) | Peer developer who has not used the product |
| 3 | Silent tutorial test (30 min) | Any developer friend |
| 4 | Talk plus Q&A (30 min) and live build (20 min) | Two peers; a local meetup lightning talk is even better |
| 5 | Community strategy plus written response (40 min) | Peer, ideally from a developer community |
| 6 | Full loop (five rounds over two days) | Mix of peers; optionally a coached mock on Exponent or interviewing.io |

Options: give a lightning talk at a local meetup or an online community event; post your talk recording and ask for feedback in DevRel communities. Pramp and interviewing.io can cover the practical coding screen.

---

## Fast track for the repo owner's profile

- **Skim:** API design, REST conventions, versioning, auth and rate limiting. Contract-first integrations, Swagger and an API gateway with JWT auth and rate limiting are strong background for the DX critique and API review rounds.
- **Skim:** cross-functional and influence questions. Leading a squad, mentoring and working with sales, finance and academic teams give you collaboration stories.
- **Biggest gap 1 — public portfolio:** there is no public code portfolio yet, and the portfolio review is its own round. Weeks 3 to 6 deliverables (tutorial, sample app, post, talk) are not optional for you.
- **Biggest gap 2 — talks and live demos:** live demo and presentation practice with external audiences is a known gap. Give at least one talk to strangers (a meetup or online event) before the loop.
- **Biggest gap 3 — AI developer platforms:** production LLM, RAG and evaluation experience is a gap; for AI companies, do the Week 5 model-API demo with an evaluation step.
- **Biggest gap 4 — language for samples:** your evidenced stack is PHP, SQL and REST. Confirm which language you will write samples in and match it to the company's main SDKs.
- **Round mapping — teaching and docs:** onboarding plans and mentoring map to "how do you adapt content for beginners and experts" and to the tutorial take-home.
- **Round mapping — DX critique and API design:** the API gateway and aggregation (BFF) layer across about 13 services, plus contract-first design, give credible "what makes a good API" answers.
- **Round mapping — feedback to product:** your contribution to the speech-to-text provider evaluation (buy-vs-build cost model, data-protection constraints) shows evidence-based recommendations. Keep these stories in [STORY-BANK.md](../../my-prep/STORY-BANK.md).

---

## Resources

- *The Business Value of Developer Relations* by Mary Thengvall — how DevRel teams are measured and justified.
- *Developer Relations: How to Build and Grow a Successful Developer Program* by Caroline Lewko and James Parton.
- *Docs for Developers: An Engineer's Field Guide to Technical Writing* by Jared Bhatti and others.
- Diátaxis documentation framework (https://diataxis.fr) — tutorials, how-to guides, reference and explanation.
- Google developer documentation style guide (https://developers.google.com/style) — a widely used style reference.
- *Resonate* by Nancy Duarte — structuring talks that land.
- DevRel Collective — an active community of DevRel practitioners.
