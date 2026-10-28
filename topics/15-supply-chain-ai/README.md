# 15 — Supply Chain & AI for Supply Chain

Supply chain is one of the most common customer domains for FDE and solutions roles. Palantir (Foundry/AIP), Databricks, the cloud providers, consulting firms and supply-chain software vendors (o9, Kinaxis, Blue Yonder, SAP IBP, Coupa, project44, Flexport) all deploy into it, and it shows up in decomposition, system design, data and case rounds even when the job ad doesn't mention it.

Interviewers don't expect you to be a planner. They expect you to **speak the vocabulary, know where the data lives, reason about trade-offs (service level vs. inventory cost), and know where AI helps and where it doesn't.**

> The differentiator question: **"Your forecast is more accurate. Why didn't inventory go down?"** Accuracy is not value until a decision changes.

Related questions elsewhere in the repo (not repeated here): [supply-chain visibility platform](../03-system-design/ANSWERS.md#design-a-supply-chain-visibility-platform-for-a-manufacturer-with-200-suppliers) · [inventory forecasting system](../03-system-design/ANSWERS.md#design-an-inventory-forecasting-system-for-a-retail-chain) · [hospital supply inventory](../04-decomposition/ANSWERS.md#help-a-hospital-manage-its-medical-supply-inventory) · [slow warehouse](../04-decomposition/ANSWERS.md#a-logistics-companys-warehouse-is-slow-help-them) · [supply-chain primer](../05-learning-round/ANSWERS.md#a-domain-concept-supply-chain-clinical-trials-double-entry-bookkeeping-options-pricing) · [Palantir supply-chain ontology](../../roles/01-forward-deployed-engineer/README.md#palantir-how-would-you-model-a-customers-supply-chain-as-a-foundry-ontology-and-what-would-you-build-on-top-of-it-with-aip).

---

## A. Supply chain fundamentals
- [ ] Walk me through an end-to-end supply chain and the systems behind each step (ERP, MRP, APS, WMS, TMS, procurement).
- [ ] Which supply chain KPIs matter, and what do OTIF, fill rate, inventory turns, days of supply and cash-to-cash cycle mean?
- [ ] How do you calculate safety stock and the reorder point? What drives them?
- [ ] What is the EOQ (economic order quantity) and when does the formula break down?
- [ ] What is the bullwhip effect and how do you reduce it?
- [ ] Push vs. pull, make-to-stock vs. make-to-order, just-in-time vs. just-in-case.
- [ ] What is S&OP / integrated business planning, and where does software fit in it?
- [ ] What is a bill of materials and how does MRP explode demand into purchase and production orders?
- [ ] What are the main EDI documents (850, 855, 856, 810) and why do integrations still use EDI?
- [ ] What are Incoterms and why does an engineer need to know them?
- [ ] What is multi-echelon inventory, and why is optimising each site separately wrong?
- [ ] What makes supply chain data hard to work with?

## B. Data & SQL for supply chain
- [ ] Design a data model for inventory, orders and shipments.
- [ ] SQL: compute OTIF by supplier per month.
- [ ] SQL: compute on-hand inventory as of any date from a transaction ledger, and flag days with a stockout.
- [ ] SQL: ABC classification of SKUs by cumulative share of revenue.
- [ ] SQL: days of supply per SKU and location, using the last 28 days of demand.
- [ ] Unit-of-measure mismatches, duplicate part numbers and wrong lead times: how do you clean supply chain master data?
- [ ] How do you build a "single source of truth" across several ERPs after an acquisition?

## C. Forecasting & ML
- [ ] How would you approach a demand forecasting project for a customer, from first meeting to production?
- [ ] Which forecast accuracy metrics do you use (MAPE, WAPE, bias, forecast value add), and why is MAPE dangerous?
- [ ] How do you backtest a time-series model correctly?
- [ ] Classical statistics (ETS, ARIMA) vs. gradient boosting vs. deep learning vs. time-series foundation models: how do you choose?
- [ ] How do you forecast intermittent demand for slow-moving spare parts?
- [ ] Stockouts hide true demand. How do you handle censored sales data?
- [ ] How do you forecast a brand-new product with no history?
- [ ] How do you model promotions, price changes, holidays and external signals (weather, macro)?
- [ ] What is hierarchical forecast reconciliation, and why do planners care?
- [ ] Why use probabilistic (quantile) forecasts, and how do they feed safety stock?
- [ ] How would you predict shipment ETAs and late deliveries?
- [ ] Your forecast is more accurate. Why didn't inventory go down?
- [ ] How do you communicate forecast uncertainty to planners who want one number?

## D. Optimisation & operations research
- [ ] When is a problem an optimisation problem rather than a prediction problem?
- [ ] Explain linear programming and mixed-integer programming with a supply chain example.
- [ ] Formulate a production planning problem: decision variables, objective, constraints.
- [ ] How would you approach a vehicle routing problem for a delivery fleet?
- [ ] How do you allocate scarce inventory across customers or channels when demand exceeds supply?
- [ ] The solver says the model is infeasible, or it takes hours. What do you do?
- [ ] Predict-then-optimise: how do you combine a forecast with an optimiser, and what goes wrong?
- [ ] How do you get planners to accept an optimiser's recommendation?

## E. Generative AI & agents in supply chain
- [ ] Where do LLMs genuinely help in supply chain, and where should you not use them?
- [ ] Design an LLM pipeline that extracts structured data from supplier emails, PDFs and order confirmations.
- [ ] Design a "control tower copilot" that answers "why is this order late and what should I do?"
- [ ] Text-to-SQL over the ERP and planning data: how do you make it reliable?
- [ ] An agent wants to expedite shipments or change purchase orders in the ERP. What guardrails do you put in place?
- [ ] How would you use LLMs for supplier risk monitoring (news, sanctions, financial distress, weather)?
- [ ] How would you use AI in procurement: contract analysis, spend classification, negotiation support?
- [ ] How do you evaluate an AI agent for a supply chain workflow?
- [ ] How do you combine an LLM with forecasting and optimisation models instead of replacing them?
- [ ] How do you measure the ROI of AI in supply chain?

## F. System design
- [ ] Design a supply chain control tower with exception management and an AI assistant.
- [ ] Design a supplier risk monitoring and early-warning system.
- [ ] Design an automated replenishment system for 2,000 stores that planners can override.
- [ ] Design a disruption "what-if" scenario planner (a port closes or a supplier fails).
- [ ] Design a cold-chain monitoring system for temperature-sensitive goods using IoT sensors.

## G. Decomposition & case prompts
- [ ] A retailer wants to cut stockouts by 30% without increasing inventory. Where do you start?
- [ ] New tariffs make a key supplier country 25% more expensive. Help the customer decide what to do.
- [ ] A distributor wants to cut logistics cost by 10%. Decompose the problem.
- [ ] A port strike starts tomorrow. Build a response plan and the tool to support it.
- [ ] A food company throws away 8% of its perishable stock. Help them.

## H. Customer scenarios
- [ ] The planners don't trust the forecast and override 60% of it. What do you do?
- [ ] The customer's ERP data is too dirty to forecast from. How do you proceed?
- [ ] The COO says "we need AI for our supply chain." What do you ask?
- [ ] IT won't give you direct access to SAP. How do you get the data you need?
- [ ] Your pilot improved one warehouse. How do you scale it to 40 without it falling apart?

## I. Coding exercises (reported style)
- [ ] Simulate a reorder-point inventory policy over a demand series and report stockout days and average inventory.
- [ ] Explode a multi-level bill of materials into total component requirements.
- [ ] Allocate limited stock to orders by priority, then by order date, supporting partial fills.
- [ ] Given shipment events, compute the current status and flag shipments that missed a milestone.
