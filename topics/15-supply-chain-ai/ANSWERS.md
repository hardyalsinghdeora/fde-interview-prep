# 15 — Supply Chain & AI for Supply Chain: Model Answers (Part 1)

Questions: [README.md](README.md) · Sections E–I (generative AI and agents, system design, decomposition, customer scenarios, coding): [ANSWERS-part2.md](ANSWERS-part2.md)

**How to use these answers:** these are reference answers covering sections A–D. You are not expected to be a supply chain planner. Use the vocabulary correctly, tie every technique to **the decision it changes** (what to order, where to put stock, which order to ship first), and always ask how the customer measures service level and inventory cost. Formulas marked as approximations are what you'd say in an interview, not what a planning system implements.

---

## A. Supply chain fundamentals

### Walk me through an end-to-end supply chain and the systems behind each step (ERP, MRP, APS, WMS, TMS, procurement).

**The flow:** plan → source → make → deliver → return (the SCOR model's steps).

| Step | What happens | Typical systems |
|---|---|---|
| **Plan** | Forecast demand, balance supply and demand, set inventory targets | Demand planning and APS (advanced planning and scheduling): SAP IBP, o9, Kinaxis, Blue Yonder; often Excel |
| **Source** | Choose suppliers, negotiate contracts, place purchase orders | ERP purchasing, procurement suites (Coupa, SAP Ariba), supplier portals, EDI |
| **Make** | Turn materials into products: production orders, scheduling, quality | ERP + MRP, MES (manufacturing execution system), QMS |
| **Deliver** | Store, pick, pack, ship, track | WMS (warehouse management), TMS (transport management), carrier and visibility APIs (project44, FourKites) |
| **Return** | Returns, repairs, recycling | ERP/RMA modules, WMS |

**The ERP** (SAP S/4HANA or ECC, Oracle, Microsoft Dynamics, NetSuite) is the system of record for master data (materials, suppliers, customers, BOMs), orders and financial postings. **MRP** inside the ERP turns demand into planned orders. Planning tools usually read from and write back to the ERP.

**Interview point:** the data you need is spread across these systems, keyed differently, refreshed at different rates, and owned by different teams. Most FDE work starts by joining them.

### Which supply chain KPIs matter, and what do OTIF, fill rate, inventory turns, days of supply and cash-to-cash cycle mean?

Supply chain is a trade-off between **service** and **cost/working capital**, so you always track both.

**Service**
- **OTIF (on time, in full):** % of orders (or lines) delivered complete by the promised date. Strict: one missing unit fails the order. Agree the definition (order vs line, requested vs promised date, tolerance window) before measuring anything.
- **Fill rate:** % of demand met from stock immediately. Unit fill rate is units shipped / units ordered; line fill rate counts lines.
- **Cycle service level:** probability of not stocking out in a replenishment cycle. This is the "95%" behind a safety stock formula, and it is not the same as fill rate.
- **Perfect order rate:** on time, complete, undamaged, correct paperwork.

**Inventory and cost**
- **Inventory turns** = cost of goods sold / average inventory value. Higher means less capital tied up.
- **Days of supply (DOS) / days inventory outstanding (DIO):** how many days current stock covers at the expected demand rate.
- **Cash-to-cash cycle** = DIO + DSO (days sales outstanding) − DPO (days payables outstanding). How long cash is tied up between paying suppliers and getting paid.
- **Logistics cost** as % of revenue, cost per unit shipped, **obsolete/expired stock** written off.

**Forecast:** WAPE, bias, forecast value add (see section C).

**What to say:** "I'd agree one service KPI and one cost KPI with the customer, baseline both before building, and report them together, because improving one by wrecking the other isn't a win."

### How do you calculate safety stock and the reorder point? What drives them?

**Reorder point (ROP)** = expected demand during lead time + safety stock = d̄ × L + SS, where d̄ is average daily demand and L the lead time in days. When inventory position (on hand + on order − backorders) falls to the ROP, you order.

**Safety stock** buffers uncertainty:
- Demand uncertainty only: **SS = z × σ_d × √L**
- Demand and lead-time uncertainty: **SS = z × √(L × σ_d² + d̄² × σ_L²)**

z comes from the target cycle service level (95% → 1.65, 98% → 2.05, 99% → 2.33). σ_d is the standard deviation of daily demand, σ_L that of lead time.

**Worked example:** d̄ = 100/day, σ_d = 30, L = 9 days, 95% service. SS = 1.65 × 30 × 3 = ~149. ROP = 900 + 149 = ~1,049.

**What drives it, and what to tell a customer:**
- Service level is non-linear: 95% → 99% raises safety stock by ~40% (1.65 → 2.33), and 99.9% is very expensive. Not every SKU deserves 99%; segment by value and criticality (ABC/XYZ).
- **Lead-time variability is often the bigger driver** than demand variability. Fixing an unreliable supplier can free more cash than a better forecast.
- These formulas assume roughly normal, independent demand. For intermittent or skewed demand, use simulation or the forecast's quantiles directly (section C).

### What is the EOQ (economic order quantity) and when does the formula break down?

EOQ balances **ordering cost** (fixed cost per order: admin, setup, freight minimum) against **holding cost** (capital, storage, obsolescence, typically 15–30% of item value per year).

**EOQ = √(2DS / H)**, where D = annual demand, S = cost per order, H = annual holding cost per unit.

Example: D = 12,000 units, S = $100, H = $4 → EOQ = √(2 × 12,000 × 100 / 4) = √600,000 ≈ 775 units, about 15 orders a year.

**When it breaks down:** demand isn't constant (seasonality, promotions); quantity discounts or pallet, case and container multiples; capacity or shelf-life limits; shared orders across many SKUs from one supplier (joint replenishment); lead-time variability. In practice EOQ is a sanity check and a starting point. Real systems round to pack sizes and apply supplier minimums. The total cost curve is flat near the optimum, so being 20% off EOQ costs only a few percent.

### What is the bullwhip effect and how do you reduce it?

Small changes in consumer demand become **larger and larger swings** in orders further up the chain: retailer → distributor → manufacturer → raw material supplier.

**Causes:** each tier forecasts from its customer's *orders*, not real end demand; batching orders (weekly or monthly, full trucks); price promotions and forward buying; shortage gaming (ordering more than needed when supply is rationed); long lead times.

**Fixes:**
- **Share real demand data** (point-of-sale, sell-through, inventory) upstream; vendor-managed inventory (VMI) and CPFR (collaborative planning, forecasting and replenishment).
- Smaller, more frequent orders; shorter lead times.
- Everyday low pricing instead of deep promotions.
- Allocate scarce supply by past sales, not by what was ordered, to stop gaming.

**FDE angle:** the bullwhip is a data-sharing problem. The fix is often an integration (sharing sell-through data with suppliers), not a model.

### Push vs. pull, make-to-stock vs. make-to-order, just-in-time vs. just-in-case.

- **Push:** produce to a forecast and push stock towards the market. **Pull:** produce or move only when real demand signals it (kanban, replenish what was consumed).
- **Make-to-stock (MTS):** build ahead of demand (consumer goods). Fast delivery, forecast risk. **Make-to-order (MTO):** build after the order (custom machinery). No finished-goods risk, longer lead time. **Assemble-to-order (ATO):** stock components, assemble on order (PCs). **Engineer-to-order (ETO):** design per order.
- The **decoupling point** is where push turns into pull. Stock is held there.
- **Just-in-time (JIT):** minimal inventory, frequent deliveries. Efficient, but fragile when supply is disrupted. **Just-in-case:** hold buffers against disruption. COVID and the 2021 chip shortage pushed many companies from JIT towards more buffer stock, dual sourcing and nearshoring.

**What to say:** "Most real chains are hybrids. The question is where to hold buffers and how big, which is exactly what inventory optimisation answers."

### What is S&OP / integrated business planning, and where does software fit in it?

**S&OP (sales and operations planning)** is a **monthly cross-functional process** that agrees one plan for the next 3–18 months, balancing demand and supply. **IBP (integrated business planning)** extends it with financial plans and strategy.

**Typical monthly cycle:**
1. **Data gathering:** close last month, update actuals.
2. **Demand review:** sales, marketing and demand planning agree an unconstrained demand forecast.
3. **Supply review:** operations checks capacity, materials and suppliers; produces a constrained supply plan and highlights gaps.
4. **Pre-S&OP / reconciliation:** resolve gaps, prepare scenarios with financial impact.
5. **Executive S&OP:** leadership decides trade-offs (accept lower service, add a shift, build inventory ahead of a peak) and signs off one plan.

**Where software fits:** a single data model across demand, supply and finance; scenario planning ("what if demand is +15%?"); constraint-aware supply planning; dashboards of gaps and decisions. Most of the value is in the **process and agreed numbers**. A tool on top of a broken process just makes the arguments faster.

### What is a bill of materials and how does MRP explode demand into purchase and production orders?

A **bill of materials (BOM)** is a tree (more precisely a directed acyclic graph, because components are shared) listing which components and quantities make one unit of a parent item. Example: 1 bicycle = 1 frame + 2 wheels; 1 wheel = 1 rim + 32 spokes + 1 tyre.

**MRP (material requirements planning)** works level by level:
1. **Gross requirements** for the top item come from the master production schedule (forecast + orders).
2. **Net requirements** = gross − on hand − scheduled receipts + safety stock.
3. **Lot sizing:** round to batch size, minimum order or EOQ.
4. **Lead-time offset:** if a wheel takes 3 days, its planned order starts 3 days before the bike needs it.
5. **Explode** planned orders into requirements for the next BOM level. Repeat to the bottom (purchased parts become purchase requisitions).

**Gotchas interviewers like:** shared components must be aggregated across parents (process in **low-level code** order, a topological sort); scrap/yield factors; phantom assemblies; BOM effectivity dates and versions; MRP assumes infinite capacity, so APS tools add capacity constraints. See the coding exercise in [part 2](ANSWERS-part2.md#explode-a-multi-level-bill-of-materials-into-total-component-requirements).

### What are the main EDI documents (850, 855, 856, 810) and why do integrations still use EDI?

**EDI (electronic data interchange)** is the standard for B2B documents between trading partners. In North America the standard is ANSI **X12**; elsewhere it is mostly **EDIFACT**.

| X12 | EDIFACT | Document | Direction |
|---|---|---|---|
| 850 | ORDERS | Purchase order | Buyer → supplier |
| 855 | ORDRSP | PO acknowledgement (accept, change, reject lines) | Supplier → buyer |
| 856 | DESADV | Advance ship notice (ASN): what's in which carton or pallet, on which truck | Supplier → buyer |
| 810 | INVOIC | Invoice | Supplier → buyer |
| 846 | INVRPT | Inventory advice | Either |
| 214 | IFTSTA | Shipment status (from carriers) | Carrier → shipper |
| 940 / 945 | — | Warehouse shipping order / shipping advice (with a 3PL) | Shipper ↔ warehouse |
| 997 | CONTRL | Functional acknowledgement ("I received and parsed it") | Either |

**Why still EDI:** decades of investment, every large retailer and manufacturer mandates it, it's reliable and auditable, and trading-partner agreements are built on it. It's typically transported over **AS2** or SFTP, or via a VAN (value-added network), and translated by an EDI platform into ERP formats (for SAP, IDocs).

**Integration lessons:** the 997 only means "parsed", not "accepted"; the 855 is the business acceptance. Every partner has its own **implementation guide** (a variant of the standard). Match ASN → PO → invoice (**three-way match**) for receiving and payment. Small suppliers often can't do EDI, so you need a portal or spreadsheet path too.

### What are Incoterms and why does an engineer need to know them?

**Incoterms** (published by the International Chamber of Commerce, current version **Incoterms 2020**) define **who pays for and bears the risk of** each leg of an international shipment, and **where ownership of risk transfers**.

Common ones: **EXW** (ex works: buyer does everything from the seller's door), **FCA** (free carrier: seller hands over to the buyer's carrier), **FOB** (free on board: risk passes when goods are on the vessel; sea only), **CIF** (seller pays cost, insurance and freight to the destination port, but risk passes at loading), **DAP** (delivered at place, unloading and import duty excluded), **DDP** (delivered duty paid: seller does everything, including import duty).

**Why an engineer cares:**
- **Landed cost** calculations (product + freight + insurance + duty) depend on the Incoterm, so do supplier cost comparisons and tariff impact models.
- **Who tracks the shipment:** under FOB the buyer books the freight and owns the tracking data; under DDP the seller does. This decides which party's data you have to integrate.
- **When inventory and liability transfer** affects in-transit inventory accounting and which events count as "delivered" in OTIF.

### What is multi-echelon inventory, and why is optimising each site separately wrong?

An **echelon** is a tier in the network: plant → central DC → regional DCs → stores. **Multi-echelon inventory optimisation (MEIO)** sets stock targets for all tiers together.

**Why separately is wrong:** if each site holds safety stock for its own uncertainty, you **double-buffer**: the regional DC buffers against store demand and the stores buffer again. Upstream stock serves many downstream sites, so it benefits from **risk pooling**: variability of the sum grows like √n, not n. Holding more stock centrally and less at the edge often gives the same service with less total inventory. The trade-off is longer replenishment to the edge.

**Key ideas to mention:** the **guaranteed service model** (each stage promises a service time to its customer, and you choose where to place buffers); placing stock at the decoupling point; service level at the end customer is what counts, not at each internal node. Typical results are a 10–30% inventory reduction at the same service level (vendor claims; verify for the customer).

### What makes supply chain data hard to work with?

- **Many systems, different keys:** the same part has different numbers in the ERP, the WMS and the supplier's system; multiple ERPs after acquisitions.
- **Master data quality:** wrong lead times (often never updated since setup), wrong pack sizes and units of measure, duplicate materials, missing BOM versions, obsolete items still active.
- **Units and time:** each vs case vs pallet; time zones and calendar differences (fiscal weeks, factory calendars, holidays).
- **Sales ≠ demand:** stockouts, substitutions and backorders hide true demand.
- **Events are late, missing or out of order:** an ASN arrives after the truck; carrier status updates are patchy.
- **Snapshots vs transactions:** some systems give daily inventory snapshots, others movement ledgers, and they don't reconcile.
- **Manual overrides and Excel:** the real plan often lives in planners' spreadsheets.
- **Partner data:** suppliers and 3PLs send data in their own formats with their own quality.

**What to say:** "I'd profile before modelling: completeness, freshness and a reconciliation of inventory from the ledger against the snapshot. Then I'd fix the five data issues that matter for the decision, not all of them."

---

## B. Data & SQL for supply chain

### Design a data model for inventory, orders and shipments.

Separate **master data**, **transactions (events)** and **derived state**.

```
-- Master data
item(item_id, sku, description, base_uom, abc_class, shelf_life_days)
location(location_id, type /* plant|dc|store|supplier */, region, timezone)
supplier(supplier_id, name, country)
item_supplier(item_id, supplier_id, lead_time_days, moq, pack_size, unit_cost, valid_from, valid_to)
uom_conversion(item_id, from_uom, to_uom, factor)

-- Transactions (append-only)
inventory_txn(txn_id, item_id, location_id, txn_ts, qty_base_uom, txn_type /* receipt|issue|adjust|transfer_in|transfer_out */, ref_doc)
sales_order(so_id, customer_id, order_ts, requested_date, promised_date)
sales_order_line(so_id, line_no, item_id, qty, ship_from_location_id)
purchase_order(po_id, supplier_id, order_ts) / po_line(po_id, line_no, item_id, qty, need_date, confirmed_date)
shipment(shipment_id, mode, carrier, origin_id, dest_id, planned_departure, planned_arrival)
shipment_line(shipment_id, ref_doc_type, ref_doc_id, line_no, item_id, qty)
shipment_event(shipment_id, event_ts, event_type /* picked_up|departed|arrived|delivered|exception */, location, source)

-- Derived (rebuildable)
inventory_daily(item_id, location_id, date, on_hand, on_order, allocated, available)
```

**Design points to say out loud:** store quantities in a **base unit of measure** with a conversion table; keep **inventory as an append-only ledger** and derive snapshots (so you can rebuild any date and reconcile against the ERP's snapshot); keep supplier terms **effective-dated** (lead times change); keep event `source` and both event time and received time, because carrier data arrives late; model shipments to PO or SO lines many-to-many (one order splits into several shipments, one truck carries several orders).

### SQL: compute OTIF by supplier per month.

Assume `po_line(po_id, line_no, supplier_id, item_id, qty_ordered, promised_date)` and `receipt(po_id, line_no, receipt_date, qty_received)`. A line is OTIF if the full quantity arrived on or before the promised date.

```sql
WITH received AS (
  SELECT p.supplier_id, p.po_id, p.line_no, p.qty_ordered, p.promised_date,
         COALESCE(SUM(r.qty_received) FILTER (WHERE r.receipt_date <= p.promised_date), 0) AS qty_on_time,
         COALESCE(SUM(r.qty_received), 0) AS qty_total,
         MAX(r.receipt_date) AS last_receipt
  FROM po_line p
  LEFT JOIN receipt r ON r.po_id = p.po_id AND r.line_no = p.line_no
  WHERE p.promised_date < CURRENT_DATE          -- only lines already due
  GROUP BY p.supplier_id, p.po_id, p.line_no, p.qty_ordered, p.promised_date
)
SELECT supplier_id,
       date_trunc('month', promised_date) AS month,
       COUNT(*) AS lines_due,
       AVG(CASE WHEN qty_on_time >= qty_ordered THEN 1.0 ELSE 0 END) AS otif,
       AVG(CASE WHEN last_receipt <= promised_date THEN 1.0 ELSE 0 END) AS on_time_any_qty,
       SUM(LEAST(qty_total, qty_ordered))::numeric / SUM(qty_ordered) AS unit_fill
FROM received
GROUP BY 1, 2
ORDER BY 1, 2;
```

**Talk through:** the `LEFT JOIN` keeps lines with no receipt (they count as failures, which an inner join would hide); only count lines already due; "in full" with a tolerance (e.g. ≥ 98%) if the business agrees; report on-time and in-full separately so the supplier knows which to fix. `FILTER` is Postgres syntax; elsewhere use `SUM(CASE WHEN ... THEN qty_received END)`.

### SQL: compute on-hand inventory as of any date from a transaction ledger, and flag days with a stockout.

Using `inventory_txn(item_id, location_id, txn_ts, qty_base_uom)` with signed quantities (receipts positive, issues negative) and a `calendar(date)` table:

```sql
WITH daily_net AS (
  SELECT item_id, location_id, CAST(txn_ts AS date) AS d, SUM(qty_base_uom) AS net
  FROM inventory_txn
  GROUP BY 1, 2, 3
),
grid AS (                                   -- every item-location for every day
  SELECT il.item_id, il.location_id, c.date AS d
  FROM (SELECT DISTINCT item_id, location_id FROM inventory_txn) il
  CROSS JOIN calendar c
  WHERE c.date BETWEEN DATE '2026-01-01' AND CURRENT_DATE
)
SELECT g.item_id, g.location_id, g.d,
       SUM(COALESCE(n.net, 0)) OVER (
         PARTITION BY g.item_id, g.location_id ORDER BY g.d
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) + COALESCE(o.opening_qty, 0) AS on_hand_eod,
       (SUM(COALESCE(n.net, 0)) OVER (
         PARTITION BY g.item_id, g.location_id ORDER BY g.d
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) + COALESCE(o.opening_qty, 0)) <= 0 AS stockout_eod
FROM grid g
LEFT JOIN daily_net n USING (item_id, location_id, d)
LEFT JOIN opening_balance o USING (item_id, location_id);   -- balance as of 2026-01-01
```

**Talk through:** the calendar grid makes days with no movement appear (a plain running sum over transactions would skip them); the running sum is the classic window-function answer; you need an **opening balance** or the full history. End-of-day zero misses intraday stockouts; if that matters, compute the running balance at transaction level and find periods where it hit zero. Finally, **reconcile** the derived balance against the ERP's snapshot; differences reveal missing transaction types.

### SQL: ABC classification of SKUs by cumulative share of revenue.

A = top ~80% of revenue, B = next ~15%, C = the last ~5% (the thresholds are a business choice).

```sql
WITH rev AS (
  SELECT item_id, SUM(qty * unit_price) AS revenue
  FROM sales_order_line
  WHERE order_date >= CURRENT_DATE - INTERVAL '12 months'
  GROUP BY item_id
),
ranked AS (
  SELECT item_id, revenue,
         SUM(revenue) OVER (ORDER BY revenue DESC, item_id
                            ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
         / SUM(revenue) OVER () AS cum_share
  FROM rev
)
SELECT item_id, revenue, cum_share,
       CASE WHEN cum_share <= 0.80 THEN 'A'
            WHEN cum_share <= 0.95 THEN 'B'
            ELSE 'C' END AS abc_class
FROM ranked
ORDER BY revenue DESC;
```

**Talk through:** the tie-breaker (`item_id`) makes the running sum deterministic; use `ROWS` not the default `RANGE` so ties don't share a cumulative value. Mention **XYZ** classification (by demand variability, e.g. coefficient of variation) combined with ABC: an "AX" item (high value, stable) gets a different policy from a "CZ" item (low value, erratic). Items with zero sales in the period don't appear, which is exactly the dead-stock list you should also show.

### SQL: days of supply per SKU and location, using the last 28 days of demand.

```sql
WITH demand AS (
  SELECT item_id, location_id, SUM(qty) / 28.0 AS avg_daily_demand
  FROM demand_history                     -- shipments or, better, unconstrained demand
  WHERE demand_date >= CURRENT_DATE - 28 AND demand_date < CURRENT_DATE
  GROUP BY 1, 2
)
SELECT i.item_id, i.location_id, i.on_hand, d.avg_daily_demand,
       CASE WHEN COALESCE(d.avg_daily_demand, 0) = 0 THEN NULL
            ELSE i.on_hand / d.avg_daily_demand END AS days_of_supply
FROM inventory_daily i
LEFT JOIN demand d USING (item_id, location_id)
WHERE i.date = CURRENT_DATE - 1;
```

**Talk through:** dividing the sum by 28 (not `AVG` over rows) counts days with no demand as zero; handle zero demand explicitly (infinite days of supply means excess or dead stock, which is itself an insight); backward-looking demand is wrong for seasonal items, so use the **forward forecast** when you have one; decide whether "on hand" includes stock on order, in transit or already allocated.

### Unit-of-measure mismatches, duplicate part numbers and wrong lead times: how do you clean supply chain master data?

1. **Profile first:** per field, completeness, distinct values, outliers (a lead time of 0 or 999 days, a pack size of 1 for a pallet item).
2. **Fix units of measure:** one base UoM per item, an explicit conversion table, and validation that conversions are consistent (case → each → case returns the same number). Check against reality: transactions where an order quantity is an exact multiple of 12 or 24 usually means case vs each confusion.
3. **Deduplicate parts:** standardise descriptions (normalise units, abbreviations), block on manufacturer part number or supplier code, fuzzy match, and send grey-zone matches to a **data steward** review queue. Keep a crosswalk; don't delete source IDs.
4. **Lead times from actuals:** the ERP's planned lead time is often stale. Compute **actual lead time** from PO date to receipt date per supplier and item, compare with the master value, and propose updates with evidence (median and spread). This alone often improves planning more than any model.
5. **Make it stick:** validation rules on entry, ownership per field (who is the steward), a data quality dashboard, and fixes made in the **source system**, not just in your pipeline. Otherwise the problem comes back.

**What to say:** "I'd prioritise by impact on the decision: for replenishment that's lead time, pack size, MOQ and UoM on A items first."

### How do you build a "single source of truth" across several ERPs after an acquisition?

Don't promise a big-bang ERP consolidation; that's a multi-year programme. Build a **harmonised data layer** on top:

1. **Agree the questions it must answer** (global inventory, supplier spend, OTIF across all businesses) and pick those entities first.
2. **Land raw data** from each ERP unchanged (SAP tables like MARA/MARC, EKKO/EKPO, MSEG; Oracle and the others) into a lakehouse or warehouse, with source system as part of every key.
3. **Map to a canonical model** (item, location, supplier, order, inventory) with explicit transformation logic per source, version-controlled.
4. **Master data matching:** cross-reference items and suppliers across ERPs (entity resolution, see the [master data design](../03-system-design/ANSWERS.md#design-a-system-to-unify-customer-records-from-10-systems-entity-resolution--master-data)). Global IDs plus crosswalks back to local IDs.
5. **Harmonise semantics, not just fields:** "open order", "on hand", "lead time" and fiscal calendars mean different things in each system. Document definitions in a glossary that the business signs off.
6. **Reconcile** against each ERP's own reports so finance trusts it.
7. **Ship one dashboard or decision on it early**, then extend.

This is the pattern behind ontology-style platforms (Palantir Foundry) and lakehouse deployments (Databricks): integrate once into business objects, then build many applications on top.

---

## C. Forecasting & ML

### How would you approach a demand forecasting project for a customer, from first meeting to production?

1. **Decision first:** what will the forecast drive (store replenishment, production plan, S&OP), at what **granularity** (SKU × store × day, or family × region × month), **horizon** (lead time plus review period, at minimum) and **frequency**? Who uses it?
2. **Baseline:** what do they do today (planner judgement, last year + x%, a vendor tool)? Measure its accuracy. You must beat it, and a seasonal-naive model, on the same backtest.
3. **Data:** sales/shipments history (2+ years for seasonality), stockouts and inventory, price and promotions, calendar and events, product hierarchy, new and discontinued items. Clean demand (remove stockout censoring, returns, one-off bulk orders).
4. **Model in steps:** baseline → classical per-series models (ETS) → a global gradient-boosting model with features → anything fancier only if the backtest justifies it.
5. **Backtest properly** (rolling origin, the right horizon, business metrics like WAPE and bias by segment).
6. **Integrate into the decision:** forecasts feed replenishment or planning tools, with **probabilistic outputs** for safety stock, explanations, and planner overrides.
7. **Operate:** retraining schedule, monitoring of accuracy and drift, tracking **forecast value add** of overrides, and a fallback when the pipeline fails.
8. **Measure value:** stockouts, inventory, waste, planner time. Not just accuracy.

### Which forecast accuracy metrics do you use (MAPE, WAPE, bias, forecast value add), and why is MAPE dangerous?

- **MAPE** = mean of |actual − forecast| / actual. **Dangerous** because it's undefined when actual = 0 and explodes for small actuals: a slow mover with actual 1 and forecast 3 scores 200% and dominates the average. It also penalises over-forecasting more than under-forecasting.
- **WAPE** (weighted APE) = Σ|actual − forecast| / Σ actual. Volume-weighted, robust to zeros, easy to explain ("we're off by 18% of volume"). The usual default for business reporting.
- **Bias** = Σ(forecast − actual) / Σ actual. Systematic over- or under-forecasting. Often **more important than accuracy**: a consistently biased forecast drives systematic excess or shortage. Track it separately.
- **MASE** (mean absolute scaled error): error relative to a naive forecast's error. Good for comparing across series of different scale.
- **Pinball (quantile) loss** for probabilistic forecasts; **coverage** of prediction intervals (does the 90% interval contain actuals ~90% of the time?).
- **Forecast value add (FVA):** compare each step (naive → statistical model → planner override) on the same metric. If overrides make the forecast worse, that's evidence for changing the process.

**Always:** measure at the level and horizon of the decision (forecast made at lead time, not the day before), and segment by ABC class and new vs mature items.

### How do you backtest a time-series model correctly?

- **Never shuffle.** Use **rolling-origin (walk-forward) evaluation**: train on data up to time t, forecast t+1…t+h, move t forward, repeat. Several origins cover different seasons.
- **Evaluate at the decision horizon.** If orders are placed 4 weeks ahead, score the forecast made 4 weeks before each target week.
- **Prevent leakage:** features must only use information available at forecast time. Classic leaks: rolling means computed over the whole series, actual promotion lift (rather than the planned promotion), price changes known only later, inventory data that includes the outcome, and target encoding computed on the full set.
- **Compare against baselines** (seasonal naive, the customer's current method) on identical windows.
- **Segment results** (by category, ABC class, new vs mature, promo vs non-promo). An overall win can hide a loss on A items.
- **Include special periods** (peak season, last year's disruption) or explicitly exclude them, and say which.

### Classical statistics (ETS, ARIMA) vs. gradient boosting vs. deep learning vs. time-series foundation models: how do you choose?

| Approach | Good for | Weak spots |
|---|---|---|
| **Naive / seasonal naive** | The baseline you must beat | No drivers |
| **ETS / ARIMA / Theta** (per series) | Few series, stable patterns, explainability, small data | Don't share information across series; weak with promotions and many drivers; poor for cold start |
| **Global gradient boosting** (LightGBM, XGBoost) on lag, calendar, price and promo features | Thousands to millions of related series with drivers; the workhorse in industry and the top of the M5 competition | Feature engineering effort; needs care for multi-step horizons |
| **Deep learning** (DeepAR, N-BEATS, Temporal Fusion Transformer) | Very large datasets, complex covariates, native probabilistic outputs | More data, tuning and compute; harder to explain; often only marginally better than GBMs |
| **Time-series foundation models** (e.g. Amazon Chronos, Google TimesFM, Salesforce Moirai, Nixtla TimeGPT; *verify current versions*) | Zero-shot forecasts with little history; quick baselines; cold start | Limited use of business covariates in many versions; less controllable; must still be backtested against your baselines |

**How I choose:** start with seasonal naive and ETS as baselines, then a global LightGBM with sensible features, and only go further if the backtest shows a gain that matters for the decision. Ensembles of two or three different models are often the most robust. I'd test a foundation model as a cheap extra baseline, not assume it wins.

### How do you forecast intermittent demand for slow-moving spare parts?

Intermittent demand = many zero periods with occasional, variable-sized demand (spare parts, B2B). Standard models forecast a smooth small number that is never right.

- **Classify first** with the average demand interval (ADI) and squared coefficient of variation (CV²) of non-zero demand: smooth, erratic, intermittent, lumpy (Syntetos–Boylan).
- **Croston's method** (and the **SBA** bias-corrected version, and **TSB**, which handles obsolescence): forecast demand size and interval separately; the forecast is size / interval, a demand *rate*.
- Use **aggregation** (forecast monthly instead of daily, or at a higher level) to reduce noise.
- **What matters is the stocking decision, not the point forecast.** Use the **distribution** (Poisson, negative binomial, or bootstrapping of history) to set the stock level that gives the target service. For very expensive, rarely used parts, the decision may be "stock 1 or 0", and **criticality** (what does a stockout cost, e.g. a line stoppage) drives it more than the forecast.
- Use **install base and maintenance schedules** as drivers where available: demand for a part is tied to the machines in service.

### Stockouts hide true demand. How do you handle censored sales data?

When the shelf is empty, sales = 0 but demand wasn't zero. Training on raw sales teaches the model that demand is low, which leads to ordering less, causing more stockouts (a vicious circle).

- **Detect stockout periods** from inventory data (on hand = 0, or very low at store level), lost-sales logs, or e-commerce "out of stock" page views.
- **Option 1, exclude or mask** those periods from training and evaluation.
- **Option 2, impute** true demand from similar periods, similar stores, or the rate of sales before the stockout within the day.
- **Option 3, model it explicitly** (censored-demand models, e.g. Tobit-style likelihoods).
- Account for **substitution**: some lost demand moved to a similar product, inflating its sales.
- Tell the customer: "Sales history understates demand on the items that matter most, the ones that run out. Fixing this is often worth more than switching model."

### How do you forecast a brand-new product with no history?

- **Analogues ("like" items):** pick similar past launches by attributes (category, price point, brand, size, season) and use their launch curves, scaled. Let planners choose or confirm the analogues.
- **Attribute-based global models:** a gradient boosting model trained on many launches, with product attributes (and sometimes text or image embeddings of the product description) as features, predicting launch-period demand.
- **Judgement and pre-orders:** buyer estimates, pre-orders, early sell-through.
- **Update fast:** after a few days or weeks of sales, blend the prior (analogue) with actuals, Bayesian-style, and re-plan.
- **Hedge the decision:** smaller first buy, quick replenishment, postponement (keep stock undifferentiated as long as possible), since launch forecasts are very uncertain. Show a wide range.

### How do you model promotions, price changes, holidays and external signals (weather, macro)?

- **Features in a global model:** planned promotion flags, discount depth, promo type and display, price relative to the regular price, competitor price if available; holiday and event calendars with lead and lag windows (demand rises before Christmas and dips after); day-of-week and paydays.
- **Promotions cause pull-forward and cannibalisation:** a promoted item steals sales from substitutes and from the weeks after the promotion. Model the halo and cannibalisation effects, or at least measure them, so the promo doesn't look better than it is.
- **Only use what's known at forecast time:** the *planned* promotion calendar, the weather *forecast* (not observed weather) for short horizons. Weather typically helps at short horizons for weather-sensitive categories (drinks, ice cream, garden) and adds little beyond a couple of weeks.
- **Macro signals** (consumer confidence, industrial production) help in aggregate, longer-horizon planning more than in SKU-level replenishment.
- **Test the value of each signal in the backtest.** External data has cost and maintenance; keep it only if it moves accuracy where decisions are made.

### What is hierarchical forecast reconciliation, and why do planners care?

Forecasts exist at many levels: SKU × store, SKU × region, category × country, total. Forecasting each level independently gives numbers that **don't add up**, and planners, finance and S&OP each look at a different level.

**Reconciliation** makes them coherent:
- **Bottom-up:** sum SKU forecasts. Coherent, but noisy at the bottom and misses aggregate patterns.
- **Top-down:** forecast the total and split by historical proportions. Stable, but loses local detail.
- **Middle-out:** forecast at a middle level, then go both ways.
- **Optimal reconciliation (MinT, minimum trace):** forecast all levels, then adjust them with a statistical combination that minimises total error. Often best, and available in libraries (e.g. Nixtla's `hierarchicalforecast`).

**Why planners care:** the S&OP volume at category level must match the sum of store replenishment, or supply plans and store orders diverge. Also, a planner adjusting the category total ("+10% for a marketing campaign") needs that change spread consistently to SKUs.

### Why use probabilistic (quantile) forecasts, and how do they feed safety stock?

A point forecast says "100 units". The decision needs to know **how wrong that could be**: "90% chance demand is below 140".

- **Quantile forecasts** (p10, p50, p90, …) come from quantile loss in gradient boosting, from DeepAR-style models, or from conformal prediction around any model.
- **Direct use in inventory:** to hit a 95% cycle service level, stock up to the **p95 of demand over the lead time plus review period**. That replaces the normal-distribution safety stock formula, and it handles skewed and intermittent demand properly.
- **Newsvendor logic** for one-shot decisions (perishables, fashion): order up to the quantile **cu / (cu + co)**, where cu = cost of being under (lost margin) and co = cost of being over (waste or markdown). If margin is $6 and waste costs $2, order the 75th percentile.
- **Check calibration:** the p90 should be exceeded about 10% of the time in the backtest. Miscalibrated intervals give the wrong stock.

### How would you predict shipment ETAs and late deliveries?

- **Frame it:** predict arrival time (regression), or P(late versus the promised date) (classification), at each point in the journey. The prediction is updated as new events arrive.
- **Data:** planned schedule, mode, carrier, lane (origin–destination), historical transit times on the lane, current status events and GPS, port congestion and dwell times, weather, day-of-week and holidays, customs status.
- **Model:** start from the **historical transit-time distribution per lane and carrier** (a strong baseline), then gradient boosting on lane features plus "time since last event" and "current milestone". Output a distribution (p50 and p90 ETA), not a point.
- **Handle missing events:** missing scans are common; "no update for 48 hours" is itself a signal.
- **Use it for exceptions:** alert when P(late) crosses a threshold **and** the shipment feeds a critical order or production line, so planners act on the 5% that matter.
- **Measure:** MAE of ETA by days-before-arrival, calibration, and how early true delays are flagged (lead time of the warning).

### Your forecast is more accurate. Why didn't inventory go down?

This is the most important question in the section. Possible reasons, and what to check:

- **The forecast isn't used in the decision:** planners override it, or the replenishment system still uses its own forecast. Check override rates and the data feed.
- **The inventory policy didn't change:** safety stock is still set by an old rule ("4 weeks of cover") rather than from forecast error. Better accuracy only reduces stock if safety stock is recalculated from the new error.
- **Constraints dominate:** MOQs, pack sizes, full-truck ordering and long supplier lead times set inventory more than the forecast does.
- **Improved the wrong items or horizon:** accuracy rose on C items or at a one-day horizon, while A items at lead-time horizon didn't change.
- **Lead-time variability:** unreliable suppliers force buffers regardless of forecast quality.
- **Measurement:** inventory went up for other reasons (growth, new stores, strategic stock build), so compare against a control group.

**What to say:** "Accuracy is a means, not the outcome. I'd map the decision chain from forecast to order quantity, find where the improvement gets lost, and fix that link, often the safety stock policy or overrides."

### How do you communicate forecast uncertainty to planners who want one number?

- **Give one number and a range:** "We expect 1,200, likely between 1,000 and 1,450." Then translate it into the decision: "Ordering 1,350 covers 90% of outcomes."
- **Make the trade-off explicit:** a chart of service level versus inventory cost so they choose the point, rather than the model hiding it.
- **Show track record:** past forecasts vs actuals and how often the range held, which builds calibrated trust.
- **Explain drivers** for the big changes ("promotion in week 3, last year's peak"), not the model internals.
- **Make overrides easy, and measure them** (forecast value add), so planners keep control and see where their judgement helps.

See also [communicating forecast uncertainty to executives](../../roles/11-forward-deployed-data-ml/README.md#how-do-you-communicate-forecast-uncertainty-to-executives).

---

## D. Optimisation & operations research

### When is a problem an optimisation problem rather than a prediction problem?

- **Prediction** answers "what will happen?": demand next week, ETA, probability of a supplier failing.
- **Optimisation** answers "what should we do, given constraints and an objective?": how much to make on each line, which warehouse ships which order, which routes trucks take.

Signs it's optimisation: there are **decisions with many interacting choices**, **hard constraints** (capacity, budget, truck size, time windows), and a **clear objective** (minimise cost, maximise service or margin). The number of combinations is too large to enumerate or do by hand.

Most real supply chain problems are **both**: predict demand, then optimise inventory, production or routes given that prediction. Many teams have a forecasting model and a spreadsheet for the decision; the spreadsheet is where an optimiser adds value.

### Explain linear programming and mixed-integer programming with a supply chain example.

- **Linear programming (LP):** choose continuous decision variables to minimise or maximise a linear objective subject to linear constraints. Solved quickly and to proven optimality even with millions of variables (simplex, interior point).
- **Mixed-integer programming (MIP/MILP):** some variables must be integers or binary (open this warehouse or not, number of trucks, a minimum batch if we produce at all). Much harder (NP-hard in general), solved by branch-and-bound with cuts. Solvers report a **gap** between the best solution found and the best possible bound.

**Example: transportation problem.** Plants i with capacity Cᵢ, customers j with demand Dⱼ, cost cᵢⱼ per unit shipped.
- Variables: xᵢⱼ ≥ 0, units from plant i to customer j.
- Minimise Σ cᵢⱼ xᵢⱼ
- Subject to: Σⱼ xᵢⱼ ≤ Cᵢ (capacity), Σᵢ xᵢⱼ ≥ Dⱼ (demand).

That's an LP. Add "each warehouse has a fixed cost Fᵢ if opened" (binary yᵢ, xᵢⱼ ≤ Cᵢ yᵢ, add Σ Fᵢ yᵢ to the objective) and it becomes a MIP: the classic **facility location** problem.

**Tools:** open-source (Google OR-Tools, HiGHS, CBC, SCIP), commercial (Gurobi, CPLEX, FICO Xpress), modelling layers (Pyomo, PuLP, CVXPY). Gurobi and CPLEX are much faster on hard MIPs, which matters at scale.

### Formulate a production planning problem: decision variables, objective, constraints.

**Setting:** products p, weeks t, one factory with limited hours.

- **Data:** demand d_{p,t}, production hours per unit h_p, capacity H_t, production cost c_p, holding cost k_p per unit per week, setup cost s_p, a large number M, initial inventory I_{p,0}.
- **Decision variables:** x_{p,t} ≥ 0 units produced; I_{p,t} ≥ 0 inventory at the end of week t; y_{p,t} ∈ {0,1}, 1 if product p is set up in week t; optionally b_{p,t} ≥ 0 backorders.
- **Objective:** minimise Σ (c_p x_{p,t} + k_p I_{p,t} + s_p y_{p,t} + penalty × b_{p,t}).
- **Constraints:**
  - Inventory balance: I_{p,t} − b_{p,t} = I_{p,t−1} − b_{p,t−1} + x_{p,t} − d_{p,t}
  - Capacity: Σ_p h_p x_{p,t} ≤ H_t
  - Setup linking: x_{p,t} ≤ M · y_{p,t} (can only produce if set up)
  - Optional: minimum batch size, shelf life, overtime variables with extra cost.

**Talk through:** the setup binaries make it a MIP (a lot-sizing problem); choose M as small as possible (e.g. the maximum you could produce in the week) for faster solving; a backorder penalty keeps the model feasible when demand exceeds capacity, and shows *where* capacity is short, which is often the most useful output for S&OP.

### How would you approach a vehicle routing problem for a delivery fleet?

**Clarify:** number of stops per day and vehicles; vehicle capacity (weight, volume, pallets); **time windows** at customers; driver shift and break rules; one depot or several; pickups and deliveries; static plan the night before, or dynamic re-routing during the day; objective (distance, cost, number of vehicles, on-time rate).

**Approach:**
- The VRP is NP-hard; nobody solves large instances to proven optimality. Use **heuristics and metaheuristics**: construction (savings algorithm, nearest neighbour), then improvement (2-opt, relocate, swap) inside guided local search, tabu search or large neighbourhood search. **Google OR-Tools' routing solver** handles capacities and time windows out of the box; commercial routing engines also exist.
- **Distance and time matrix** from a routing engine (OSRM, Valhalla or a maps API) with real road travel times, ideally by time of day.
- Solve in a **time budget** (e.g. 60 seconds) and keep the best solution found.

**Deployment realities:** drivers know things the model doesn't (loading docks, parking, customers who are never in), so allow manual changes and capture the reasons. Measure planned vs actual time and feed corrections back. Start with a comparison: plan yesterday's real orders and compare kilometres and on-time with what actually happened.

### How do you allocate scarce inventory across customers or channels when demand exceeds supply?

1. **Agree the business rules first:** this is a policy decision, not a technical one. Options: priority by customer tier or contract (SLA penalties), by margin, **fair share** (pro-rata to orders or to historical sales, which also prevents shortage gaming), protecting strategic accounts or channels, and ring-fencing stock for critical uses (e.g. hospitals).
2. **Implement it as a rules engine or an optimisation:** a simple ranked allocation (priority, then order date, with partial fills; see the [coding exercise](ANSWERS-part2.md#allocate-limited-stock-to-orders-by-priority-then-by-order-date-supporting-partial-fills)) or, when there are many products, locations and substitution options, an LP that maximises weighted fulfilment subject to available stock and minimum fair-share constraints.
3. **Allocate across time too:** reserve part of incoming supply for orders expected later in the week (allocation by **available-to-promise**, ATP), instead of first come, first served.
4. **Make it transparent:** customers and sales teams accept shortages better when the rules are known. Log every allocation decision with the rule that drove it.
5. **Review regularly** with sales and finance; rules that made sense in a crisis shouldn't stay forever.

### The solver says the model is infeasible, or it takes hours. What do you do?

**Infeasible:**
- Ask the solver for an **IIS** (irreducible infeasible subsystem, available in Gurobi and CPLEX), the minimal set of constraints that conflict.
- Check data first: units (hours vs minutes), a demand with no possible source, capacity of zero for a holiday week.
- Make hard constraints **soft** with penalty variables (allow unmet demand or overtime at a high cost). The model always solves, and the slack shows where and how big the problem is. That's usually the business answer: "we're 400 units short in week 12 at plant B."

**Too slow:**
- Accept a **MIP gap** (e.g. 1%) and a time limit; the last 1% often takes most of the time and isn't worth it.
- Tighten the formulation: small big-M values, valid inequalities, symmetry breaking (identical trucks or lines).
- **Decompose:** solve by region or time window; rolling horizon (plan week 1 in detail, later weeks aggregated).
- **Warm start** from yesterday's plan or a heuristic solution.
- Aggregate (product families instead of SKUs) for the strategic layer, with detail only where needed.
- Benchmark solvers: commercial solvers can be an order of magnitude faster on hard MIPs.

### Predict-then-optimise: how do you combine a forecast with an optimiser, and what goes wrong?

**Pattern:** a forecast (ideally a distribution or scenarios) feeds the optimiser's parameters (demand, lead times, prices), and the optimiser makes the decision.

**What goes wrong:**
- **Plugging in a point forecast** ignores uncertainty: the optimiser treats demand as certain and produces brittle plans with no buffer. Use quantiles, **scenario-based (stochastic) optimisation** or robust constraints instead.
- **Optimising on errors:** the optimiser exploits forecast mistakes, e.g. sending everything to the location where demand was over-forecast ("optimiser's curse").
- **Accuracy metric ≠ decision quality:** a forecast that is better on WAPE can produce worse decisions if its errors fall where costs are asymmetric. Evaluate the pipeline on **decision cost in a backtest** (simulate the plan against what actually happened), not only on forecast accuracy.
- **Feedback loops:** the decisions change future data (allocations change sales, routes change delivery times), which biases the next training set.
- **Disconnected teams:** data science owns the forecast and operations owns the optimiser, and nobody owns the end result. Assign one owner for the decision outcome.

### How do you get planners to accept an optimiser's recommendation?

- **Involve them in building it:** capture their rules and constraints in workshops. Most "bad" optimiser outputs come from constraints nobody wrote down ("that customer never takes deliveries on Fridays").
- **Explain the recommendation** in their terms: what changed, the main driver, and the cost of the alternative ("shipping from DC 2 saves $4,000 but adds one day").
- **Start as decision support, not automation:** recommendations with accept or edit; capture override reasons and use them to improve the model.
- **Prove it with a backtest and a pilot:** replay last month's decisions versus the optimiser's on actual outcomes, then run a side-by-side pilot in one region.
- **Automate gradually:** auto-apply the routine, low-risk decisions once trust is earned; keep humans on exceptions and high-value decisions.
- **Respect their incentives:** if planners are measured only on stockouts, they'll never accept a plan that lowers inventory. Align KPIs with the people involved.
