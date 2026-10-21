# 04 — Decomposition Questions

The decomposition round (Palantir's signature round; also used by Databricks, Scale AI, OpenAI and many AI startups) gives you a vague, real-world problem. There's no model answer. You're scored on:

- **Clarifying**: Who's the user? What decision do they make? What's success?
- **Structured breakdown**: MECE sub-problems
- **Data reasoning**: What data exists, its quality, how to join it
- **Scoping judgment**: What to build first
- **End-user empathy**
- **Iteration**: How you respond when the interviewer pushes back or adds constraints

**Most common failure:** jumping straight to a solution.

See the framework in [00-overview](../00-overview/README.md#decomposition-palantir-style).

---

## A. Public sector / government / defense
- [ ] A city wants to reduce 911 response times. You have call data, traffic data, and ambulance GPS. What do you do?
- [ ] Help a police department decide where to deploy officers.
- [ ] Help a government agency detect benefits fraud.
- [ ] Help the military track and maintain its vehicle/aircraft fleet.
- [ ] Help coordinate disaster response after a hurricane/earthquake (shelters, supplies, rescue).
- [ ] Help a city reduce homelessness using data from shelters, hospitals and police.
- [ ] Help a country distribute vaccines during a pandemic.
- [ ] Help a border agency process immigration cases faster.
- [ ] Help a tax authority find under-reported income.
- [ ] Help a city plan public transport routes.
- [ ] Help track illegal fishing / sanctions evasion using ship AIS data.
- [ ] Help an intelligence analyst find connections between people across many datasets.
- [ ] Help a school district reduce dropout rates.
- [ ] Help a city manage its water supply during a drought.
- [ ] Help a fire department predict which buildings are at highest fire risk.

## B. Healthcare
- [ ] A hospital wants to reduce patient readmissions. Build a solution.
- [ ] A hospital's ER is overcrowded. How would you help?
- [ ] Help a hospital network manage bed capacity across sites.
- [ ] Help a pharma company speed up clinical trial recruitment.
- [ ] Help a health insurer detect fraudulent claims.
- [ ] Help a hospital reduce nurse burnout / optimise staffing schedules.
- [ ] Help a hospital manage its medical supply inventory.
- [ ] Help doctors spend less time on documentation.

## C. Commercial / enterprise
- [ ] An airline has frequent flight delays. How do you help?
- [ ] A manufacturer has too many defects on its production line. Help them.
- [ ] A retailer keeps running out of stock on popular items. Help them.
- [ ] A bank wants to detect money laundering.
- [ ] A shipping company wants to optimise routes and reduce fuel costs.
- [ ] An energy company wants to predict equipment failures on the grid.
- [ ] An oil & gas company wants to improve drilling efficiency.
- [ ] A car manufacturer wants to reduce warranty claims.
- [ ] A telecom wants to reduce customer churn.
- [ ] An insurance company wants to process claims faster.
- [ ] A restaurant chain wants to reduce food waste.
- [ ] A logistics company's warehouse is slow. Help them.
- [ ] A company has data spread across 50 systems and nobody trusts the numbers. Help them.
- [ ] A CEO says "we want to use AI." Where do you start?
- [ ] A sales team wants to know which leads to prioritise.
- [ ] A company wants to reduce employee attrition.
- [ ] A construction company keeps going over budget on projects.
- [ ] A farm wants to increase crop yield.
- [ ] A bank wants to reduce loan defaults.
- [ ] An e-commerce company has too many returns.

## D. AI-company decomposition prompts
- [ ] A customer wants to automate 50% of its customer support with an LLM. Plan it.
- [ ] A law firm wants to use LLMs to review contracts. How do you scope it?
- [ ] A bank wants an internal AI assistant for its 10,000 employees. What do you build first?
- [ ] A customer's RAG pilot works in demos but fails in production. How do you approach it?
- [ ] A customer wants an agent that automates procurement end to end. Break it down.
- [ ] A hospital wants AI to summarise patient records for doctors. What are the risks and steps?
- [ ] A customer says the model is "hallucinating too much." What do you do?
- [ ] A customer wants to replace a team of 20 data-entry workers with AI. How do you approach it?
- [ ] An enterprise has 5,000 internal documents and wants a Q&A bot by Friday. Plan it.
- [ ] A customer wants to measure ROI of their AI deployment. How?

## E. Abstract / product decomposition
- [ ] How would you build a product to help people find a parking spot?
- [ ] How would you help a library decide which books to buy?
- [ ] How would you design a system to match volunteers to non-profits?
- [ ] How would you help a university schedule its classes and rooms?
- [ ] How would you help a sports team scout players?
- [ ] How would you build a tool to track the spread of misinformation?
- [ ] Estimate how many X are in Y (Fermi questions), then build a tool to track it.

## F. Typical interviewer follow-ups / push-backs
- [ ] Who is the actual user, and what does their day look like?
- [ ] What's the first thing you'd build? Why that?
- [ ] What if the data you assumed doesn't exist?
- [ ] What if the data is 80% wrong/missing?
- [ ] The customer only has 2 weeks. What do you deliver?
- [ ] How would you know if your solution is working? What metric?
- [ ] What are the privacy / ethical concerns?
- [ ] What happens when users don't trust your model's output?
- [ ] The stakeholder disagrees with your priority. How do you handle it?
- [ ] How would this scale to 100 hospitals/cities/plants?
- [ ] What would make this project fail?
- [ ] Which parts are generalisable into a product?
- [ ] Now add a constraint: no internet / no cloud / legacy mainframe. What changes?
- [ ] How would you get adoption from users who resist change?
