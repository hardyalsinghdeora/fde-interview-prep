# My Answers

Personalised answers built from [STORY-BANK.md](STORY-BANK.md) and [PROFILE.md](PROFILE.md). Part 1 has full answers to the questions asked in almost every loop. Part 2 maps every behavioral question in the repo to the story to use.

`[I-xx]` = fill in from [INPUTS-NEEDED.md](INPUTS-NEEDED.md) before using.

---

## Part 1 — Full answers to the most-asked questions

### Tell me about yourself (90 seconds)

> I'm a principal software engineer with about fifteen years in the industry [I-14: one line on where you started]. For the last few years I've been at a K–12 education company, where my work has been about one problem: making new systems work inside an old, live business.
>
> Three things I'm proudest of. First, I designed the bridge between a long-lived legacy database estate and our new service platform, across six product lines, while both sides stayed live. That meant separating a one-time historical backfill from a continuous reverse sync so older products never noticed. Second, I designed the commercial platform that sales and finance run on: orders, subscriptions, a discount approval engine, and invoicing integrated with our accounting system and tied to how fast cash comes in. Third, I lead a squad of four engineers and own delivery end to end: release plans, monthly programme reports, onboarding plans.
>
> What I've realised is that the part of the job I enjoy most is sitting with the people who use the system (sales, finance, academic teams), understanding what they actually need, and then shipping it. That's why I'm looking at forward deployed and solutions roles: I want to do that work directly with customers, especially on AI deployments, where the hard part is integrating with the real business.

*Shorter version (30 seconds):* "Principal engineer, about 15 years. I specialise in integrations and live migrations: I designed the bridge between a legacy estate and a new platform while both stayed live, and the order and billing platform our sales and finance teams use. I lead a small squad. I want to do this directly with customers, as an FDE."

### Why FDE (or solutions) and not a core SWE role?

> Looking back, my best work has always been at the boundary between engineering and the business. The discount engine wasn't hard because of the code. It was hard because sales and finance had to agree on one set of rules, and I was the person who got them there. The migration worked because I understood which legacy products the business couldn't afford to break. An FDE role makes that boundary the whole job: I'd own the outcome for a customer, not just a ticket. I also like breadth. I've worked across commerce, CRM, identity, content and field operations, and I learn new domains quickly. And at principal level, the most valuable thing I can do is turn messy real-world requirements into systems that work, which is exactly what forward deployed work is.

### Why are you leaving your current role?

> [I-15: your honest reason, framed positively.] Suggested framing: "I've done the work I set out to do: the commercial platform and the migration design are in place. I want to apply the same skills directly with external customers and in AI, where the demand for engineers who can integrate with messy real businesses is growing fastest. That's hard to do from an internal platform team."

Never criticise your employer, colleagues or manager.

### What are your strengths?

> Three. **One, making old and new systems work together**: integrations, live migrations, idempotency, reconciliation (S1). **Two, turning business rules into systems**: sitting with sales and finance and coming out with an agreed, auditable rule set (S2). **Three, writing things down so others can execute**: specifications other engineers can build from without me, release plans, and onboarding plans that get new hires productive fast (S9).

### What's your biggest weakness?

Use S6. This is a real weakness with a real fix, which interviewers trust far more than "I work too hard".

> Communication used to be a real gap for me. In a review, my manager marked my communication goal as not yet met: my updates were too long and I didn't always double-check them. I treated it like an engineering problem. I set a 2–3 sentence limit for status emails, used a fixed format, blocked 30 minutes a week to review what I'd sent, and made sure every message named the person and the action. [I-07: what changed.] Later I wrote the monthly programme report for seven straight months. I still have to watch my tendency to over-explain, especially to executives, so I lead with the conclusion now.

### Tell me about the project you're most proud of

Use **S1**, the migration. Structure: the problem (two live systems, six product lines, legacy readers you couldn't change) → the key insight (separate one-time backfill from continuous sync) → the safety design (idempotency guard, ordering, retries, rollback) → the result [I-01] → the lesson (reconciliation first, S4).

### Tell me about a time you failed

Use **S4**: "I didn't build reconciliation until the first drift appeared." Keep the impact honest and show the system you built afterwards. If you have a bigger failure from earlier in your career, add it in [I-16]. A pre-2022 story also shows breadth.

### Tell me about a time you worked directly with a customer

Use **S2** (sales and finance as customers) or **S8** (sales and academic staff). Start with: "My customers were internal business teams, so let me describe how I worked with them." Then tell it exactly like a customer story: discovery, finding the real need, trade-offs, rollout. If you have any external vendor, partner or school-facing work, prefer that [I-17].

### Tell me about a time you disagreed with your manager / pushed back

Use **S10** once you fill in [I-11]. Alternative: the currency-conversion question in S2, where you laid out options instead of silently picking one. Or [I-18], a real disagreement story.

### Tell me about a time you had to learn something new quickly

Use **S5** (speech-to-text providers, Indian data-protection law, GPU self-hosting costs: all new to the team) with the partial-credit wording. Or **S7** (an unfamiliar legacy stack). Add [I-19] if you've recently learned LLM tooling for your portfolio projects. That's the best answer for AI-company loops.

### Tell me about a time you had to make a decision with incomplete information

Use **S5**. "The usage estimates conflicted, so we first fixed the input numbers, then made a reversible decision behind an adapter, so being wrong would be cheap."

### How do you prioritise when everything is urgent?

> I sort by business impact and by what unblocks others. On the migration rollout, the dependency order (organisations, then orders, then users) decided a lot of the priority for us. Anything that blocked another team went first. Then I make the trade-off visible: I write down what we're not doing this sprint and confirm it with the stakeholder, so nobody is surprised. Monthly programme reports kept leadership aligned on that.

### Why should we hire you?

> You need someone who can walk into a customer's messy real environment (legacy databases, a CRM, an accounting system, business rules nobody wrote down) and ship something that works, safely, without breaking what's already running. That's what I've done for the last several years, at principal level, including leading a team and owning delivery. I'm also honest about where I'm still building: I'm actively building hands-on LLM projects [I-19], and I learn fast when the stakes are real.

### What would you do in your first 90 days?

> First 30 days: learn the platform hands-on, shadow two or three live deployments, and understand the customers and why they bought. Days 30–60: own a workstream on one customer deployment end to end and write down every rough edge I hit. Days 60–90: own a deployment, and turn one recurring pain point into a reusable tool or doc for the team. I've done this before: I write 30- and 90-day onboarding plans for my own team, so I'd write my own and share it with my manager in week one.

### How do you keep code quality high when moving fast?

> I keep a short, non-negotiable list. Pull-request reviews and a QA walkthrough before handover were standard on my team. Then I make speed safe: idempotent, re-runnable jobs; guard conditions written before the happy path; feature flags or a thin adapter so decisions can be reversed. Anything I deliberately cut, I write down as known debt with an owner.

### Would you be comfortable with travel and on-site work?

> [I-20: your real answer, covering percentage, relocation, and visa or work authorisation.]

### Deep dive on a project from your resume

Use **S1** and prepare a whiteboard version: legacy product-partitioned tables → normalised order model; backfill path versus continuous sync path; the idempotency guard; the ordering constraints; monitoring and reconciliation. Expect "what would you change?" (reconciliation first, a dry-run mode, a CDC tool instead of custom sync if starting today [I-21: was CDC considered?]).

---

## Part 2 — Which story for every behavioral question

Questions from [topics/10-behavioral](../topics/10-behavioral/README.md). **Bold** = strongest choice.

### Ownership & impact
| Question | Story |
|---|---|
| Tell me about the project you're most proud of. | **S1** |
| Tell me about a time you owned something end to end. | **S1**, S12 |
| Tell me about a time you went beyond your job description. | **S10**, S9 |
| Tell me about the most impactful thing you've built. How did you measure impact? | **S3** (once it has a number), S1 |
| Tell me about a time you took initiative without being asked. | **S10**, S4 (built reconciliation) |
| Tell me about a time you shipped something that users loved. | **S8**, [I-09] |
| Tell me about a time you had to deliver results with limited resources. | **S9** (small team), S5 (buy, don't build) |

### Ambiguity & bias for action
| Question | Story |
|---|---|
| Tell me about a time you worked on a problem with no clear requirements. | **S2**, S12 |
| Tell me about a time you had to make a decision with incomplete information. | **S5** |
| Tell me about a time you had to ship fast and cut corners. How did you decide what to cut? | **S4** (what we deferred and paid for), [I-22] |
| Tell me about a time priorities changed suddenly. | [I-22] |
| Tell me about a time you had to choose between speed and quality. | **S4**, S7 |

### Customer focus
| Question | Story |
|---|---|
| Tell me about a time you worked directly with a customer or end user. | **S2**, S8, [I-17] |
| Tell me about a time you turned around an unhappy customer/stakeholder. | [I-23] |
| Tell me about a time you said no to a customer. | **S5** (said no to self-hosting), S2 (not every exception becomes a rule) |
| Tell me about a time you discovered the customer's real need was different from what they asked for. | **S8** |
| Tell me about a time you had to manage customer expectations. | **S5** (non-goal: mixed-language accuracy), S12 |
| Tell me about a time you represented the user's voice in a technical decision. | **S8**, S11 |

### Conflict & collaboration
| Question | Story |
|---|---|
| Tell me about a time you disagreed with your manager. | **S10** [I-11], [I-18] |
| Tell me about a time you disagreed with a teammate on a technical decision. | [I-24] |
| Tell me about a time you had a conflict with a stakeholder. | **S2** (sales vs finance) |
| Tell me about a time you had to work with a difficult person. | [I-24] |
| Tell me about a time you influenced someone without authority. | **S10**, S2 |
| Tell me about a time you worked with a cross-functional team (sales, product, design). | **S2**, S3 |
| Tell me about a time you received critical feedback. What did you do? | **S6** |
| Tell me about a time you gave difficult feedback to someone. | **S9** (prioritisation coaching) [I-10] |
| Tell me about a time you helped a teammate succeed / mentored someone. | **S9** |

### Failure & learning
| Question | Story |
|---|---|
| Tell me about a time you failed. | **S4**, [I-16] |
| Tell me about your biggest mistake. What did you learn? | **S4**, [I-16] |
| Tell me about a project that didn't go as planned. | **S12** [I-13], S4 |
| Tell me about a time you missed a deadline. | [I-25] (sprint overspill records may help you remember one) |
| Tell me about a time you were wrong. | **S5** (the usage numbers we started with were wrong), S4 |
| Tell me about a time you broke production. | [I-26] |
| Tell me about a time you had to learn something new quickly. | **S5**, S7, [I-19] |
| What's something you've changed your mind about recently? | **S5** (buy over build), or your view on AI tools [I-19] |

### Problem solving & technical judgment
| Question | Story |
|---|---|
| Tell me about the hardest technical problem you've solved. | **S1** |
| Tell me about a difficult bug you tracked down. | **S4** (drift root cause), [I-26] |
| Tell me about a time you made a significant architectural decision. What were the trade-offs? | **S1** (two flows), S5 (adapter) |
| Tell me about a time you simplified a complex system. | **S1** (six table families into one model) |
| Tell me about a time you had to work in an unfamiliar codebase or technology. | **S7** |
| Tell me about a time you turned a one-off solution into a reusable tool. | **S9** (onboarding plans), S7 (content-porting automation) |
| Tell me about a time you used data to make a decision. | **S5** |

### Prioritisation & pressure
| Question | Story |
|---|---|
| Tell me about a time you had to juggle multiple projects or customers. | **S12** (tech lead for three modules) |
| Tell me about a time you worked under extreme pressure. | [I-26] or [I-22] |
| How do you prioritise when everything is urgent? | Part 1 answer above |
| Tell me about a time you had to say no to work. | **S5**, [I-22] |
| Tell me about a time you handled a production incident. | [I-26] |

### Communication
| Question | Story |
|---|---|
| Tell me about a time you explained something technical to a non-technical audience. | **S2** (rules to finance), S3 |
| Tell me about a time miscommunication caused a problem. | **S6**, S5 (conflicting usage numbers) |
| Tell me about a presentation you gave that went well / badly. | [I-27] |
| Tell me about a time you wrote documentation that made a difference. | **S1** (seven specs others executed), S9 |

### Values, ethics & culture
| Question | Story |
|---|---|
| Tell me about a time you faced an ethical dilemma at work. | [I-28] |
| Tell me about a time you pushed back on something you thought was wrong. | **S10** |
| What does integrity mean to you at work? | Use S5's "honest non-goals": say what the system *won't* do |
| How do you think about the societal impact of the technology you build? | Education for children: data protection (S5), access control (S8) |
| Would you be comfortable working with government/defense customers? | [I-29] |
| What's your view on responsible AI deployment? | S5: data protection as a hard constraint, honest limits, reversible decisions |
| What kind of culture do you thrive in? | Written, ownership-driven culture (your release plans and reports) |

### Self-reflection
| Question | Story |
|---|---|
| What are your strengths? | Part 1 answer |
| What are your weaknesses / areas for growth? | **S6**, Part 1 answer |
| How would your manager / peers describe you? | [I-30] (quote real feedback if you have it) |
| What motivates you? | Seeing users depend on what you built (S2, S8) |
| What's something you're curious about outside of work? | [I-31] |
| What would you do in your first 90 days here? | Part 1 answer |
| Why should we hire you? | Part 1 answer |
| Is there anything else you'd like us to know? | Your one-line positioning statement from [PROFILE.md](PROFILE.md) |

### Hiring-manager round topics
| Question | Story |
|---|---|
| Deep dive on one project from your resume: architecture, your role, trade-offs, what you'd change. | **S1** |
| What kind of manager brings out your best work? | [I-32] |
| How do you handle long travel stretches / on-site work? | [I-20] |
| How do you keep code quality high when you're moving fast for a customer? | Part 1 answer |
| Business use-case discussion: how would you apply our product at company X? | Prepare per company. Use your K–12 / commerce domain as "company X" |
