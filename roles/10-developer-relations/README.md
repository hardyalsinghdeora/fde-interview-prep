# Developer Relations

Engineers who help developers succeed with a platform through content, docs, demos, samples and community, and who carry developer feedback back to product. Shares the "technical + customer-facing" mix of FDE, but one-to-many instead of one-to-one.

## The role

**Day to day**

- Build sample apps, starter templates, SDK examples and integrations.
- Write tutorials, blog posts, quickstarts and docs; record videos and livestreams.
- Give talks and workshops at conferences, meetups and webinars; run hackathons.
- Answer developers on forums, Discord, GitHub issues and Stack Overflow.
- Test new features before launch ("friction logging") and report developer experience (DX) problems.
- Collect feedback from the community and bring it to product and engineering with evidence.
- Support launches with content, demos and announcements, together with product marketing.

**Common titles and aliases:** Developer Advocate, Developer Relations Engineer (Google's title), Developer Evangelist, Cloud Advocate (Microsoft), Developer Experience (DX) Engineer, Community Engineer, Technical Content Engineer, Developer Educator, Head of DevRel. Some companies put DX Engineers in engineering (building SDKs, CLIs, docs tooling) and Advocates in marketing; check where the role reports.

**Companies that hire for it (examples):** Google, Microsoft, AWS, GitHub, Twilio, Stripe, Cloudflare, Vercel, Supabase, MongoDB, Postman, Hugging Face, and most AI model and developer-tool companies. Team size and focus vary a lot; at a startup one advocate may own docs, community and events alone.

**How it differs from FDE**

| | Developer Relations | Forward Deployed Engineer |
|---|---|---|
| Audience | Many developers, mostly unknown individuals | A few named customer accounts |
| Main output | Content, samples, docs, talks, community, feedback | Production systems inside a customer |
| Code | Demo-quality to production-quality samples; SDKs in DX roles | Production code under customer constraints |
| Success measure | Adoption, activation, community health, DX improvements | Customer outcomes, deployment success, revenue |
| Time horizon | Campaigns and launches; long-term community | Deployment milestones |
| Travel | Conferences and events | Customer sites |

Both roles translate between users and the product team and need strong communication plus real coding ability. FDE goes deep with one customer; DevRel goes wide.

**Who it suits:** engineers who enjoy teaching, writing and speaking, like building small things quickly, have empathy for beginners, and are comfortable being public (talks, social media, livestreams).

**Skills mix**

| Skill | Weight |
|---|---|
| Coding (samples, demos, SDKs) | Medium to High (High for DX Engineer) |
| Customer / developer communication (writing, speaking) | High |
| Sales | Low (indirect: drives adoption, not deals) |
| Product (feedback, DX critique, launches) | High |
| Marketing / content strategy | Medium |
| Community management | Medium to High |

**Career paths:** senior/staff/principal advocate, DevRel manager or Head of DevRel, developer experience or platform engineering, product manager (especially for APIs and developer tools), solutions architect / FDE, technical product marketing, founder of a developer-tools startup.

## Typical interview loop

Typical/reported structure; it varies by company and by whether the team sits in engineering, product or marketing.

| Round | What it tests | How to prepare |
|---|---|---|
| Recruiter screen | Motivation, public presence, travel | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/README.md) |
| Hiring manager | DevRel philosophy, how you measure impact, portfolio overview | Questions below |
| Portfolio / content review | Quality of past talks, posts, videos, repos | Questions below; [Prep checklist](#prep-checklist) |
| Technical screen | Coding ability, often practical (build or extend a small app) | [02-coding](../../topics/02-coding/README.md) (practical problems) |
| Take-home: build and write | Build something with the company's API, write a tutorial or blog post about it | Questions below |
| Presentation / talk round | 10–30 minute talk to a panel, with Q&A | Questions below |
| Live demo | Build or show something live while explaining | Questions below; [09 demo-breaks scenario](../../topics/09-customer-scenarios/README.md) |
| DX critique | Review the company's API, SDK or docs and suggest improvements | Questions below; [08 APIs & integration](../../topics/08-cloud-devops-integration/README.md) |
| Cross-functional | Working with PM, engineering, marketing | [10-behavioral](../../topics/10-behavioral/README.md) |

## Common rounds — use the shared sections

- [05-learning-round](../../topics/05-learning-round/README.md) — learning an unfamiliar API fast and explaining it back is the core DevRel skill; practise these prompts.
- [08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md) — section A (APIs and integration) and B (auth) for API design and DX critique.
- [06-ai-llm](../../topics/06-ai-llm/README.md) — essential if the company is an AI/LLM platform: fundamentals, prompting, RAG, agents, MCP.
- [09-customer-scenarios](../../topics/09-customer-scenarios/README.md) — section C (communication) and "the demo breaks".
- [02-coding](../../topics/02-coding/README.md) — practical problems; DevRel coding screens are usually practical rather than heavy DS&A.
- [10-behavioral](../../topics/10-behavioral/README.md) — influence without authority, cross-functional work, customer focus.
- [14-questions-to-ask](../../topics/14-questions-to-ask/README.md) — add DevRel-specific ones: where the team reports, how success is measured, content vs. community vs. DX split.

## Role-specific questions

### Content and talks

- [ ] Walk us through a talk, post or video you made. Why that topic and format, and how did it perform?
- [ ] Give a 10-minute technical talk on a topic of your choice.
- [ ] How do you decide what content to create?
- [ ] How do you write a conference talk proposal (CFP) that gets accepted?
- [ ] How do you adapt the same topic for beginners and for experienced developers?

### Live demo

- [ ] Build something with our API live in 20 minutes while explaining what you are doing.
- [ ] Your live demo breaks on stage. What do you do?
- [ ] How do you prepare a demo so that it works reliably?

### Docs and tutorials

- [ ] Write a getting-started tutorial for our product. What goes in it?
- [ ] What is the difference between a tutorial, a how-to guide, reference docs and an explanation?
- [ ] How do you keep code samples in docs and tutorials working over time?
- [ ] Review this documentation page and tell us what you would change.
- [ ] How do you measure whether documentation is good?

### Developer experience and API critique

- [ ] Sign up for our product, build something small, and critique the developer experience.
- [ ] What makes a great developer experience?
- [ ] What is "time to first hello world", and how do you reduce it?
- [ ] Review this API design. What would you change?
- [ ] How do you design error messages that developers can act on?
- [ ] What should an SDK provide beyond a thin wrapper over the API?

### Community

- [ ] How would you build a developer community for our product from zero?
- [ ] A developer posts an angry, public complaint about the product. How do you respond?
- [ ] How do you run a hackathon or workshop that leads to real adoption?
- [ ] How would you design a community champions or ambassador program?

### Measuring DevRel impact

- [ ] How do you measure the impact of developer relations?
- [ ] Your VP asks you to prove DevRel's return on investment. What do you show?
- [ ] With limited time, how do you choose between a conference talk, a blog post, a sample app and a docs fix?

### Feedback to product

- [ ] How do you collect developer feedback and get the product team to act on it?
- [ ] Developers strongly want a feature that product has decided not to build. How do you handle it with the community?
- [ ] How do you balance advocating for developers with advocating for the company?

### DevRel for AI and AI-assisted developers

- [ ] How is DevRel changing now that many developers learn through AI coding assistants?
- [ ] How would you run developer relations for an LLM API product?

### Role-specific behavioral

- [ ] Tell me about a piece of content that did not work. What did you learn?
- [ ] Tell me about a time developer feedback you collected changed the product.
- [ ] Why developer relations rather than software engineering or a forward deployed role?
- [ ] How do you sustain content output and travel without burning out?

## Model answers

### Content and talks

#### Walk us through a talk, post or video you made. Why that topic and format, and how did it perform?

Pick your best one or two pieces in advance and send links before the interview. Structure the walkthrough:

1. **Audience and goal:** who it was for and what they should be able to do afterwards ("backend developers new to webhooks; ship a verified webhook handler").
2. **Why the topic:** evidence of demand, such as repeated forum questions, search terms, support tickets, or a launch.
3. **Why the format:** a tutorial post for step-by-step tasks, a video for visual workflows, a talk for ideas and stories, a sample repo for copy-paste starting points.
4. **How you made it:** outline, code, testing, review by an engineer or editor.
5. **Results:** views and read time, repo stars or clones, sign-ups or activations attributed to it, comments and questions, follow-up content it led to.
6. **What you would change:** shows judgment.

Example framing (adapt to your own experience): "I wrote a tutorial on [topic] because [N] forum questions a month asked about it. I chose a step-by-step post with a companion repo. It became the [Nth] most-read page in the docs that quarter, and related forum questions dropped. In hindsight I'd add a short video for the setup step, where most comments asked for help."

#### Give a 10-minute technical talk on a topic of your choice.

Choose a topic you know deeply, that fits the company's audience, and that has a live or visual element. Structure for 10 minutes:

- **0:00–1:00 Hook:** a concrete problem the audience has ("your LLM app returns broken JSON 5% of the time").
- **1:00–2:00 Promise:** what they will be able to do by the end.
- **2:00–8:00 Body:** three points maximum, each with an example or short demo. Show code large enough to read.
- **8:00–9:00 Recap:** the three points in one sentence each.
- **9:00–10:00 Call to action:** a link to a repo or docs, one next step.

Delivery: rehearse out loud at least three times with a timer; finish slightly early; one idea per slide; avoid reading slides. For Q&A, repeat each question, answer briefly, and say "I don't know, I'll find out" when true.

Panels usually score clarity, technical accuracy, audience awareness, timing and how you handle questions, more than polish.

#### How do you decide what content to create?

Start from developer needs and company goals, not from what is fun to make.

**Signals of demand:**
- Repeated questions in forums, Discord, GitHub issues, support tickets.
- Docs search queries with no results; high exit pages.
- Funnel drop-off points (for example, many sign-ups never make a first API call).
- Upcoming launches and strategic integrations.
- External search trends and what competitors' users ask.

**Prioritise** by: number of developers affected × importance to activation or retention × effort. Prefer content that removes a blocker in the adoption journey over content that only gets views.

**Choose the format** for the job: reference and quickstarts in docs; tutorials and sample apps for common use cases; videos for visual workflows; talks and posts for ideas and awareness.

Keep a content backlog with the evidence for each item, and review it with product and support monthly.

#### How do you write a conference talk proposal (CFP) that gets accepted?

- **Know the conference:** audience level, past accepted talks, theme. Reviewers reject topics that do not fit.
- **Title:** specific and outcome-based. "Cutting LLM costs 60% with prompt caching: lessons from production" beats "Intro to LLM optimisation".
- **Abstract (shown to attendees):** the problem, what you will show, what attendees will take away. Two short paragraphs.
- **Details for reviewers:** outline with timings, why you are qualified, any real data or experience.
- **Avoid a product pitch.** Most community conferences reject vendor pitches. Teach a transferable idea; your product can appear as one example.
- **Bring something new:** real numbers, a failure story, a production lesson, an open-source tool.
- **Submit early,** to several suitable conferences, and reuse feedback from rejections.

Example abstract opening: "Most RAG demos work on ten documents and break on ten thousand. In this talk I'll show three retrieval failures we hit in production and the evaluation setup that caught them."

#### How do you adapt the same topic for beginners and for experienced developers?

| | Beginners | Experienced developers |
|---|---|---|
| Goal | First success, confidence | Depth, trade-offs, edge cases |
| Prerequisites | State them; link setup guides | Assume them |
| Pace | Every step shown, including install and auth | Skip setup; focus on the hard part |
| Code | Complete, copy-paste, minimal abstraction | Excerpts, patterns, production concerns |
| Explanation | What and how | Why, when not to, performance, failure modes |
| Format | Tutorial, quickstart, short video | Deep-dive post, talk, reference architecture |

Example on webhooks: beginners get "receive your first webhook locally with a tunnel in 10 minutes". Experienced developers get "idempotency, retries, ordering and signature verification at scale".

Tell readers at the start who the piece is for. A beginner piece that wanders into advanced material loses both audiences.

### Live demo

#### Build something with our API live in 20 minutes while explaining what you are doing.

What is scored: you can code, you narrate clearly, you handle errors calmly, and you show the product's value quickly.

Approach:

1. **Before the interview,** sign up, read the quickstart, and do a practice build. Have API keys set as environment variables and a skeleton project ready if allowed.
2. **Start with the outcome (1 minute):** "We'll build a small app that does X. By the end you'll see Y."
3. **Build in visible steps:** auth → first call → useful result → one extra feature. Run the code after each step.
4. **Narrate the why,** not just the keystrokes: "I'm using the SDK rather than raw HTTP because it handles retries."
5. **Point out DX as you go:** "This error message told me exactly what to fix, which is great." This shows your critique skill.
6. **Keep a fallback:** a finished version in a branch in case of trouble.
7. **End with a recap and next steps** (what you would add for production).

Keep the scope small. A simple app that works beats an ambitious one that does not.

#### Your live demo breaks on stage. What do you do?

1. **Stay calm and say what happened.** "That's a 401, so my key isn't being picked up." Audiences forgive failures; they remember panic.
2. **Time-box the fix** to about 30–60 seconds. Debugging in front of an audience is actually instructive if it is quick.
3. **Switch to the fallback:** pre-recorded video, screenshots, or a finished branch. "Let me show you the finished version I ran this morning."
4. **Keep the message intact:** the point of the demo is the idea, not the live keystrokes.
5. **Follow up:** post the working code and a short explanation of what broke.

Prevention matters more (see the next question). For the shared customer version of this scenario, see [09-customer-scenarios](../../topics/09-customer-scenarios/README.md).

#### How do you prepare a demo so that it works reliably?

- **Rehearse** end to end several times, including on the actual machine and network you will use.
- **Remove network risk:** use a phone hotspot as backup; mock or cache slow external calls; avoid depending on conference Wi-Fi.
- **Pin versions** of SDKs and dependencies; do not upgrade the day before.
- **Prepare state:** pre-created accounts, seeded data, valid API keys with enough quota, fresh tokens.
- **Use checkpoints:** git branches or tags for each step, so you can jump ahead.
- **Record a backup video** of the full demo.
- **Make it readable:** large font, high-contrast theme, notifications off, clean desktop, close other apps.
- **For LLM demos:** outputs are non-deterministic. Use low temperature, test the prompts many times, and choose examples that work reliably. Be honest if an output differs.
- **Have a checklist** you run 30 minutes before.

### Docs and tutorials

#### Write a getting-started tutorial for our product. What goes in it?

Goal: a new developer reaches a working, meaningful result as fast as possible.

1. **Title and outcome:** "Send your first message in 5 minutes."
2. **Prerequisites:** language version, account, anything to install. Link, do not explain at length.
3. **Get credentials:** exact clicks, where the API key is, how to store it safely (environment variable, not in code).
4. **Install:** one command (`pip install example-sdk`).
5. **First call:** a complete, copy-paste code block that runs as-is, with the expected output shown.
6. **Make it real:** one small step that shows value beyond hello world.
7. **Troubleshooting:** the three most common errors and fixes (wrong key, wrong region, missing permission).
8. **Next steps:** two or three links to the most common next tasks.

Rules: test it on a clean machine; one path only (no "alternatively..." branches); explain only what is needed to proceed; every code block runnable. Measure success by completion rate and time to first successful call.

#### What is the difference between a tutorial, a how-to guide, reference docs and an explanation?

This is the Diátaxis framework, widely used in documentation teams:

| Type | Purpose | Reader's state | Example |
|---|---|---|---|
| Tutorial | Learning by doing; a guided lesson | New, wants to learn | "Build your first chatbot" |
| How-to guide | Solve a specific task | Knows basics, has a goal | "How to rotate API keys" |
| Reference | Accurate technical description | Working, needs facts | API endpoint docs, SDK method list |
| Explanation | Understanding concepts and the "why" | Studying, wants context | "How rate limiting works" |

Why it matters: mixing them makes docs worse. A tutorial full of reference tables loses beginners; a reference page that tells a story is hard to scan. When reviewing docs, first ask which type each page is trying to be.

#### How do you keep code samples in docs and tutorials working over time?

- **Store samples as real code in a repository,** not only as text in the docs. Include them in docs at build time.
- **Test them in CI:** run every sample against a test or sandbox environment on each SDK release and on a schedule (for example nightly), so API changes that break samples are caught.
- **Pin and update versions** deliberately; use automated dependency update tools and review the results.
- **Use doc-testing tools** where they fit (for example language-native doctests, or tools that extract and run code blocks from Markdown).
- **Generate reference docs** from the API spec (OpenAPI) so they cannot drift from the API.
- **Own each sample:** a named owner and a "last verified" date.
- **Watch signals:** user comments, GitHub issues and support tickets mentioning a sample.
- **Delete** samples that are no longer worth maintaining instead of leaving broken ones.

Broken samples damage trust more than missing samples.

#### Review this documentation page and tell us what you would change.

Use a repeatable checklist and give prioritised feedback, not a list of nitpicks.

1. **Purpose and audience:** is it clear who the page is for and what they will achieve? Which Diátaxis type is it?
2. **Accuracy:** run every code sample. Do they work? Are versions current?
3. **Completeness:** prerequisites, auth, error handling, expected output.
4. **Structure:** scannable headings, steps numbered, important information first, no long paragraphs before the first code.
5. **Clarity:** plain language, consistent terms, jargon defined once.
6. **Code quality:** copy-paste ready, secrets not hard-coded, realistic names, multiple languages if the audience needs them.
7. **Navigation:** links to next steps and related reference.
8. **Accessibility:** alt text, readable contrast, not relying on screenshots for essential text.

Then present the **top three changes** with reasons and the expected impact ("the code sample fails on the current SDK; this is the top fix because it blocks everyone"). Be respectful: the author may be on the panel.

#### How do you measure whether documentation is good?

No single metric works; combine several:

- **Task success:** completion of the quickstart; time to first successful API call; activation rate of new sign-ups who visit docs.
- **Support deflection:** fewer tickets and forum questions on topics with good docs; ticket links to docs pages.
- **Search:** docs searches with no results; searches followed by a ticket.
- **Page feedback:** "was this helpful" ratings and comments (low volume, but specific).
- **Behaviour:** exit pages in the onboarding flow; copy-button clicks on code samples; time on page (ambiguous: long can mean confusion).
- **Qualitative:** user tests where you watch a developer follow the docs; friction logs.
- **Quality checks:** broken links, failing samples, stale "last reviewed" dates.

Page views alone are a weak measure: a page may get many views because the product is confusing.
### Developer experience and API critique

#### Sign up for our product, build something small, and critique the developer experience.

This is often a take-home or the core of an onsite. Keep a **friction log** while you build: a timestamped list of every moment of confusion, delay or delight.

Walk the developer journey and note what happened at each stage:

1. **Discovery:** is it clear from the homepage what the product does and for whom?
2. **Sign-up:** steps, email verification, credit card required?
3. **Credentials:** how long to find the API key; is there a test/sandbox mode?
4. **Quickstart:** did it run first time? Time to first successful call?
5. **Building:** SDK quality, types, autocompletion, docs search, examples for your use case.
6. **Errors:** were error messages actionable?
7. **Going further:** pagination, rate limits, webhooks, pricing clarity.

Present it as: summary (overall impression, time to first success), three things that are excellent (be specific and sincere), top three to five issues ranked by impact on new developers, each with a concrete suggested fix, and small polish items in an appendix. Avoid sounding like you are attacking the team that built it; frame issues as opportunities with evidence.

#### What makes a great developer experience?

- **Fast time to value:** a working result in minutes, ideally without a sales call or credit card.
- **Predictable, consistent API:** naming, pagination, errors and auth work the same everywhere.
- **Excellent docs:** accurate, searchable, runnable examples in the languages developers use.
- **Helpful errors:** say what went wrong, why, and how to fix it, with a request ID and a docs link.
- **Good tooling:** idiomatic SDKs with types, a CLI, a sandbox or test mode, local development support, logs and request inspection in a dashboard.
- **Safe to experiment:** test keys, idempotency, easy cleanup.
- **Stability:** versioning, clear deprecation policy, changelog, migration guides.
- **Transparent operations:** status page, rate limits documented, pricing understandable.
- **Responsive community and support.**

Companies often cited as strong examples are Stripe (docs and API consistency) and Twilio (onboarding). Use one or two examples you have personally used and explain exactly what they do well.

#### What is "time to first hello world", and how do you reduce it?

Time to first hello world (TTFHW), sometimes called time to first call, is the time from a developer landing on your site to getting a first successful result from the product. It is a leading indicator of activation: developers who struggle early often never come back.

How to measure: instrument sign-up, key creation, first API call and first successful call, and look at the median time and drop-off between steps. Also do timed user tests with fresh developers.

How to reduce it:

- Remove steps: no credit card, no sales contact, auto-created API key on sign-up.
- Put a working, copy-paste example on the first docs page, with the user's key pre-filled when they are logged in.
- Offer a browser playground or API explorer so nothing needs installing.
- Provide one-command setup (`npx create-...`, templates, Codespaces/devcontainers).
- Show the most common first errors and fixes inline.
- Make the first result meaningful, not just "200 OK".

#### Review this API design. What would you change?

Example snippet an interviewer might show:

```
POST /createUser            -> 200 {"status":"error","msg":"bad email"}
GET  /getUsers?page=2       -> [ ... ]  (no total, no next link)
POST /users/delete?id=42
GET  /user_orders/42
```

Critique, then propose:

- **Resource-oriented URLs and HTTP verbs:** `POST /users`, `GET /users`, `DELETE /users/42`, `GET /users/42/orders`.
- **Status codes that mean something:** a validation error should be `400` or `422`, not `200` with an error body.
- **Structured errors:** `{"error": {"type": "invalid_request", "code": "email_invalid", "param": "email", "message": "...", "doc_url": "..."}}`.
- **Consistent naming:** pick one case style (`snake_case` or `camelCase`) everywhere.
- **Pagination:** cursor-based with `next_cursor` / `has_more`, since page numbers break when data changes.
- **Idempotency** for creates (`Idempotency-Key` header) so retries are safe.
- **Versioning** strategy and a deprecation policy.
- **Wrap list responses in an object** so fields can be added later without breaking clients.

Say which changes are breaking and how you would migrate existing users.

#### How do you design error messages that developers can act on?

A good error answers: what happened, why, and what to do next.

- **Correct HTTP status** (400 vs. 401 vs. 403 vs. 404 vs. 409 vs. 422 vs. 429 vs. 5xx).
- **Stable machine-readable code** (`rate_limit_exceeded`) that clients can branch on and that never changes.
- **Human message that names the cause and the fix:** "API key is for test mode; use a live key (sk_live_...) for this endpoint."
- **The parameter at fault** (`"param": "email"`).
- **Request ID** to quote to support.
- **Docs link** for that error code.
- **Retry information:** `Retry-After` header for 429/503; say whether retry is safe.
- **Do not leak secrets or internal details** such as stack traces or internal hostnames.

Bad: `{"error": "Invalid request"}`. Good: `{"error": {"code": "missing_param", "param": "amount", "message": "amount is required when currency is set", "request_id": "req_123", "doc_url": "..."}}`.

Support ticket data is a great source for which errors to improve first.

#### What should an SDK provide beyond a thin wrapper over the API?

- **Idiomatic design** for each language: naming, async support, iterators, context managers.
- **Types:** full type hints / TypeScript types for requests and responses, for autocompletion and early errors.
- **Authentication handling:** reading keys from environment variables, token refresh for OAuth.
- **Retries with backoff** for transient errors and 429s, respecting `Retry-After`, with idempotency keys added automatically where safe.
- **Pagination helpers:** auto-paginating iterators.
- **Typed exceptions** mapped from API error codes.
- **Streaming support** (server-sent events) where the API streams, for example LLM token streams.
- **Webhook signature verification helpers.**
- **Timeouts and configuration** (base URL, proxy, logging/debug mode).
- **Testing support:** mocks or a test mode.
- **Versioning:** semantic versioning, changelog, generated from an API spec where possible so it stays in sync, with hand-written ergonomics on top.

Many teams generate SDKs from an OpenAPI spec and then add a hand-crafted layer; mention the trade-off between consistency and idiomatic quality.

### Community

#### How would you build a developer community for our product from zero?

1. **Find where developers already are** before creating new spaces: GitHub, Stack Overflow, Reddit, Discord servers, existing meetups. Answer questions there first.
2. **Pick one home** (Discord, a forum such as Discourse, or GitHub Discussions) and staff it so every question gets an answer quickly. A community with unanswered questions dies fast.
3. **Seed it** with your early users and internal engineers; invite people who already build with the product.
4. **Give people reasons to come back:** office hours, showcase of community projects, early access to features, changelog announcements.
5. **Make contributors visible:** highlight their projects, invite them to speak, credit them in docs.
6. **Set rules early:** code of conduct, moderation process.
7. **Close the loop:** show when community feedback led to changes.
8. **Measure health:** active members, percentage of questions answered and by whom (community vs. staff), time to first answer, returning contributors.

Grow slowly and keep quality high; a small active community is worth more than a large silent one.

#### A developer posts an angry, public complaint about the product. How do you respond?

1. **Respond quickly and publicly,** in a calm, human tone. Others are watching how you handle it.
2. **Acknowledge the specific problem** and thank them for raising it, even if the tone is harsh.
3. **Check the facts** before defending anything. If they are right, say so.
4. **Move details to a private channel** if account data is involved ("I've sent you a DM to look at your account"), but post the outcome publicly afterwards.
5. **Involve the right people:** support for account issues, engineering for bugs, and your communications/PR team if it is getting large attention.
6. **Fix or explain:** a workaround, a bug filed, or a clear explanation of the design decision.
7. **Follow up publicly** when it is fixed.
8. **Do not** argue, delete legitimate criticism, or make promises on behalf of product.

Often the complaint reveals a real DX issue; log it as feedback with the link.

#### How do you run a hackathon or workshop that leads to real adoption?

**Before:**
- Define the goal: activation (participants build something real), feedback, or recruiting champions.
- Prepare starter templates, sandbox credentials with generous limits, and a tested setup path. Setup problems waste most of a workshop otherwise.
- Pick themes linked to real use cases.

**During:**
- Short kickoff demo (under 15 minutes), then mentors walking the room.
- For workshops: checkpoints and finished code at each step so nobody falls behind.
- Watch where people get stuck; this is first-class product feedback.

**After:**
- Follow up within a week: share projects, recordings and next steps.
- Offer credits or support to teams who want to continue.
- Measure: projects submitted, accounts still active 30 and 90 days later, feedback items filed, content created by participants.
- Write up friction points for the product team.

Judge prizes by real use of the product and quality, not only flashy demos.

#### How would you design a community champions or ambassador program?

- **Purpose:** scale your reach through trusted community members who teach, answer questions and give feedback.
- **Selection:** people already contributing (answering questions, writing content, building integrations), not just people with big follower counts. Aim for diversity of region, language and use case.
- **What they get:** early access and direct line to product, recognition (profile page, badges), speaking opportunities, swag, conference tickets, credits; possibly paid stipends for content.
- **What you ask:** a light, realistic commitment (for example one contribution per quarter), honest feedback, code of conduct.
- **Operations:** a private channel, regular calls with product teams, clear guidelines on disclosure (they should disclose the relationship when promoting the product).
- **Measure:** contributions, questions answered, content reach, feedback items that led to changes, retention of champions.
- **Review annually;** retire inactive members gracefully.

Treat champions as partners, not unpaid marketing.

### Measuring DevRel impact

#### How do you measure the impact of developer relations?

Use a funnel tied to business outcomes, plus quality signals:

| Stage | Example metrics |
|---|---|
| Awareness | Content reach, talk attendance, referral traffic, search ranking for key terms |
| Acquisition | Sign-ups attributed to DevRel content (UTM links, referral codes, "how did you hear about us") |
| Activation | Time to first call, quickstart completion, % of sign-ups making a successful call |
| Retention / growth | Active developers or apps, API usage from accounts that engaged with DevRel |
| Community | Active members, % questions answered, community-answered share, contributors |
| Product impact | Feedback items shipped, DX issues fixed, docs improvements and ticket deflection |

Attribution is imperfect: developers read content without clicking tracked links. Combine data with qualitative evidence (quotes, case studies) and pick a few metrics agreed with leadership in advance. Avoid vanity metrics alone (followers, raw views).

#### Your VP asks you to prove DevRel's return on investment. What do you show?

Agree on goals first, then show contribution to them with data plus stories:

1. **Activation lift:** compare activation or time to first call for developers who used DevRel content (quickstart, templates, workshops) vs. those who did not. Note it is correlation unless you ran an experiment.
2. **Pipeline influence:** accounts or deals whose developers engaged with DevRel content or events before converting, in partnership with sales operations.
3. **Support deflection:** tickets avoided after docs and KB improvements, times average cost per ticket.
4. **Product improvements:** DX issues you identified that were fixed, with before/after metrics (for example error-related tickets down).
5. **Community leverage:** questions answered by community members instead of paid staff.
6. **Launch impact:** adoption of new features in the weeks after DevRel launch content.
7. **Qualitative:** customer quotes, notable apps built, enterprise developers who cite the content.

Be honest about attribution limits; credibility matters more than a large number.

#### With limited time, how do you choose between a conference talk, a blog post, a sample app and a docs fix?

Compare expected impact per hour, against current goals:

- **Docs fix:** reaches every future developer on that path, lasts, compounds. If a docs problem blocks activation, it usually wins.
- **Sample app:** high value when developers ask "how do I build X?" repeatedly; reusable in docs, talks and workshops.
- **Blog post:** good for launches, search traffic and explaining ideas; moderate lifespan.
- **Conference talk:** high effort (writing, rehearsing, travel), reaches one room plus the recording; best for awareness in a target audience, partnerships and credibility.

A practical rule: fix blockers first (docs, errors), then build reusable assets (samples), then turn them into derived content (post, talk, video). One good sample app can become a tutorial, a talk demo and a workshop. Say you would confirm priorities with your manager against the team's goals for the quarter.
### Feedback to product

#### How do you collect developer feedback and get the product team to act on it?

**Collect:**
- Friction logs from your own building and from new-hire or user testing.
- Forum, Discord, GitHub issues, social media, workshop observations.
- Support ticket categories (partner with the support team).
- Direct conversations at events and with key developers.

**Process:**
- Log every item in one shared place (a tracker or product feedback tool), with source links and the developer's words.
- Deduplicate and count: how many developers, which segments, which accounts.
- Tie to impact: does it block activation, cause churn, create tickets?

**Influence:**
- Bring a short, ranked list to regular product reviews, not a stream of one-off complaints.
- Include evidence (counts, quotes, a recorded user session) and a suggested solution.
- Respect product's priorities; ask what evidence would change their mind.
- Close the loop: tell the community when something ships, crediting them.

Product teams act on DevRel feedback when it is reliable, quantified and aligned with their goals.

#### Developers strongly want a feature that product has decided not to build. How do you handle it with the community?

- **Understand the decision** first: cost, strategy, security, or "not now". Ask product what you can share publicly.
- **Be honest with developers.** Do not imply it might come if it will not. "We're not planning to build this in the near future, because [reason you are allowed to share]."
- **Offer alternatives:** a workaround, a sample showing how to build it on top of the API, a partner integration, or a community-built extension.
- **Keep listening:** maintain the request with counts; if demand keeps growing or the context changes, bring it back with new evidence.
- **Do not criticise product publicly** or side with the community against your colleagues. Disagreement goes through internal channels.
- **Consider building it yourself as an open-source sample** if it is small and useful; this often satisfies the need.

Trust in DevRel depends on developers believing what you tell them.

#### How do you balance advocating for developers with advocating for the company?

They usually align: the company succeeds when developers succeed with the product. The tension appears around pricing, deprecations, breaking changes, rate limits, and features that would help developers but hurt the business.

How to handle it:

- **Internally, represent developers firmly** with evidence: "this deprecation timeline breaks N integrations; developers need 12 months and a migration tool."
- **Externally, represent the company honestly:** explain decisions and their reasons; never mislead developers.
- **Do not overpromise** or leak roadmap.
- **Make trade-offs explicit** to both sides so decisions are informed.
- **Hold a personal line:** if you would not recommend something to a friend, do not promote it; say so internally.

A useful phrase: "I work for the company, on behalf of developers." Credibility with developers is the main asset of the role, and the company benefits from protecting it.

### DevRel for AI and AI-assisted developers

#### How is DevRel changing now that many developers learn through AI coding assistants?

Many developers now ask an AI assistant instead of reading docs directly, so docs and samples must work for both humans and models.

- **Docs as model input:** clean, well-structured Markdown; complete runnable examples; consistent terminology. Some companies publish a plain-text or Markdown version of docs for models, such as an `llms.txt` file (a proposed convention listing docs for LLM consumption).
- **Current information:** models trained on old docs suggest deprecated APIs. Clear version markers, migration guides and "deprecated" notices help both humans and models.
- **Tool integrations:** providing an MCP (Model Context Protocol) server or similar so assistants can call your API or search your docs directly.
- **Measuring:** traffic to docs may fall even as usage grows; track activation and usage, not only page views.
- **Testing:** check what popular assistants generate for common tasks with your product, and fix docs or samples that lead to wrong code.
- **Content mix shifts** toward architecture, best practices, trade-offs and real-world examples, which assistants handle less well.

#### How would you run developer relations for an LLM API product?

Specifics compared with a typical API:

- **Teach evaluation and reliability,** not only "hello world": prompt design, structured output, tool use, evals, cost and latency trade-offs. Developers struggle most with getting from a demo to production quality.
- **Cookbooks and reference apps** for common patterns: RAG, agents, classification, extraction, with evaluation included.
- **Fast change:** models and features change quickly; keep samples updated with model versions, publish migration notes, and deprecate old content.
- **Non-determinism in demos and docs:** show expected output as examples, not exact matches; test prompts many times.
- **Cost transparency:** token counting, caching, batch options, pricing examples.
- **Safety and responsible use:** show guardrails and policies in samples.
- **Feedback channels:** model behaviour issues (refusals, format errors) need reproducible examples to be useful to research and product teams.
- **Community:** showcases of production apps, evaluation datasets, open-source integrations with popular frameworks.

Link your answers to the technical material in [06-ai-llm](../../topics/06-ai-llm/README.md).

### Role-specific behavioral

#### Tell me about a piece of content that did not work. What did you learn?

**What they are looking for:** you measure your work honestly, can analyse why something failed, and change your approach.

**STAR outline:**
- **Situation:** [the content, audience, goal].
- **Task:** [what success was supposed to look like].
- **Action:** [how you noticed it underperformed: metrics, comments; what you investigated: topic, format, distribution, timing, audience mismatch].
- **Result:** [what you changed and the result of the next attempt].

**Example (adapt to your own experience):** I produced a 40-minute video deep dive on a new feature. Views were low and most viewers left within the first five minutes. Comments asked for the code. I realised the audience wanted to implement, not watch. I turned it into a written tutorial with a repo and a 5-minute overview video; the tutorial got far more completions, and I now decide format based on what the reader needs to do next.

#### Tell me about a time developer feedback you collected changed the product.

**What they are looking for:** you close the loop between community and product, use evidence, and influence without authority.

**STAR outline:**
- **Situation:** [the feedback, where it came from, how often].
- **Task:** [why you decided to push for change].
- **Action:** [how you gathered evidence (counts, quotes, friction log, recordings), who you took it to, what you proposed, any prototype or workaround you built].
- **Result:** [what shipped, metrics afterwards, how you told the community].

**Example (adapt to your own experience):** Several developers in our forum and at a workshop struggled to debug failed webhooks because the dashboard showed no delivery history. I collected around 20 examples, recorded two workshop participants getting stuck, and showed them to the product manager with a one-page proposal for a delivery log. It was added to the roadmap and shipped the next quarter. I wrote the launch tutorial and replied in each original forum thread to tell those developers.

#### Why developer relations rather than software engineering or a forward deployed role?

Show you understand all three and chose DevRel deliberately:

- **Software engineering:** builds the product; depth on one system. You may enjoy it, but you want more direct contact with users and more teaching.
- **FDE:** deep work with a few customers' production systems. You prefer one-to-many impact: one good tutorial or sample can help thousands of developers.
- **DevRel:** combines building, teaching and product influence. Point to evidence you already do it unpaid or part-time: talks, posts, open-source, answering questions, mentoring.

Example framing (adapt to your own experience): "The most satisfying moments in my engineering work were explaining a system to new teammates and writing internal docs that people actually used. I've given [N] meetup talks and maintain [project]. DevRel lets me keep building while multiplying my impact through teaching, and I like being the developer's voice inside the product team."

#### How do you sustain content output and travel without burning out?

Interviewers ask this because burnout is common in DevRel. Give a concrete system:

- **Reuse content:** one core asset (sample app or deep dive) becomes a post, talk, workshop and short video.
- **Plan quarterly** with clear priorities agreed with your manager; say no to low-impact requests with data.
- **Limit travel:** choose events by audience fit and expected impact; use virtual talks and recordings; set a personal monthly travel limit.
- **Batch work:** writing days, recording days; protect focus time.
- **Share load:** community champions, other teams' engineers as guest authors, rotation for community coverage.
- **Rest after events:** schedule recovery days after long trips.
- **Measure impact,** not volume, so you are not pushed into producing more for its own sake.

Mention how you handled a heavy period before, briefly and honestly.

## Prep checklist

- [ ] Build a portfolio page with your best two or three talks, posts or videos and two repos; send it before the interview.
- [ ] Sign up for the company's product, follow the quickstart, build a small app, and keep a timestamped friction log. Turn it into a short DX critique (three strengths, five ranked issues with fixes).
- [ ] Prepare a 10-minute talk with a live demo and a recorded backup; rehearse it three times with a timer and once in front of someone.
- [ ] Write a getting-started tutorial for the company's product, tested on a clean machine.
- [ ] Practise building something live with an unfamiliar API in 20 minutes while narrating (see [05-learning-round](../../topics/05-learning-round/README.md)).
- [ ] Review API design principles: REST resource naming, status codes, pagination, idempotency, versioning, error format ([08-cloud-devops-integration](../../topics/08-cloud-devops-integration/README.md)).
- [ ] Study the company's current DevRel output: docs, blog, YouTube, Discord/forum, GitHub samples, changelog. Note gaps you could fill.
- [ ] Prepare a measurement plan for the role: a few metrics across awareness, activation, community and product impact.
- [ ] Prepare four STAR stories: content that worked, content that failed, feedback that changed a product, handling public criticism.
- [ ] For AI companies, prepare a demo using their model API with an evaluation step, and review [06-ai-llm](../../topics/06-ai-llm/README.md).
