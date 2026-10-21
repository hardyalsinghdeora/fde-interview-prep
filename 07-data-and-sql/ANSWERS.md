# 07 — Data Engineering & SQL: Model Answers

Questions: [README.md](README.md)

**How to use these answers:** these are reference answers, not scripts. Adapt them to your own experience and the customer data you have actually worked with. Practise saying them out loud, and for SQL prompts, write the query by hand first, then compare. All SQL is PostgreSQL dialect unless noted.

---

## A. SQL coding prompts

### Find the second-highest (Nth-highest) salary.

**Schema:** `employees(id, name, salary, department_id)`

```sql
-- Nth highest distinct salary (N = 2). Returns no rows if there are fewer than N distinct salaries.
SELECT salary
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM employees
) t
WHERE rnk = 2
LIMIT 1;

-- Alternative without window functions
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
OFFSET 1 LIMIT 1;          -- OFFSET N-1

-- Classic "return NULL if none" version
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

- Use `DENSE_RANK` so ties at the top don't push the real second value out (with `ROW_NUMBER`, two people on 100k would make 100k the "second highest").
- Clarify: distinct salary or second person? Per department? (add `PARTITION BY department_id`). NULL salaries: `ORDER BY ... DESC` puts NULLs first in Postgres, so filter `WHERE salary IS NOT NULL` or use `DESC NULLS LAST`.

### Find duplicate rows / emails.

**Schema:** `users(id, email, created_at)`

```sql
-- Which emails are duplicated, and how many times
SELECT LOWER(TRIM(email)) AS email_norm, COUNT(*) AS cnt
FROM users
GROUP BY LOWER(TRIM(email))
HAVING COUNT(*) > 1
ORDER BY cnt DESC;

-- Return the full duplicate rows
SELECT *
FROM (
  SELECT u.*, COUNT(*) OVER (PARTITION BY LOWER(TRIM(email))) AS cnt
  FROM users u
) t
WHERE cnt > 1
ORDER BY email, created_at;
```

- Fully duplicate rows: `GROUP BY` every column with `HAVING COUNT(*) > 1`.
- Normalise (case, whitespace) before grouping — real customer data has `Bob@x.com` and `bob@x.com `. Mention that and ask what counts as "the same".

### Top N per group (top 3 products per category) — window functions.

**Schema:** `products(product_id, category_id, name)`, `order_items(order_id, product_id, quantity, unit_price)`

```sql
WITH product_revenue AS (
  SELECT p.category_id, p.product_id, p.name,
         SUM(oi.quantity * oi.unit_price) AS revenue
  FROM products p
  JOIN order_items oi ON oi.product_id = p.product_id
  GROUP BY p.category_id, p.product_id, p.name
),
ranked AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY category_id ORDER BY revenue DESC, product_id) AS rn
  FROM product_revenue
)
SELECT category_id, product_id, name, revenue
FROM ranked
WHERE rn <= 3
ORDER BY category_id, rn;
```

- `ROW_NUMBER` gives exactly 3 per group (tie broken by `product_id` for determinism); use `RANK`/`DENSE_RANK` if ties should all be included.
- You cannot filter on a window function in `WHERE` at the same level, hence the CTE. (Some warehouses — Snowflake, Databricks, BigQuery — support `QUALIFY rn <= 3`; Postgres does not.)
- Postgres alternative: `LATERAL` join with `ORDER BY ... LIMIT 3` per category, efficient with an index on `(category_id, revenue)`.

### Running total / cumulative sum.

**Schema:** `orders(order_id, customer_id, order_date, amount)`

```sql
SELECT customer_id, order_date, amount,
       SUM(amount) OVER (
         PARTITION BY customer_id
         ORDER BY order_date, order_id
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total
FROM orders
ORDER BY customer_id, order_date, order_id;
```

- Specify `ROWS` explicitly. The default frame with `ORDER BY` is `RANGE ... CURRENT ROW`, which lumps together rows with the same `order_date` (peers), so ties get the same running total — a classic subtle bug.
- Add a unique tiebreaker (`order_id`) for deterministic output.

### Moving 7-day average.

**Schema:** `daily_sales(sale_date DATE, revenue NUMERIC)` (or aggregate from `orders` first)

```sql
WITH days AS (                     -- fill gaps so missing days count as 0
  SELECT d::date AS sale_date
  FROM generate_series((SELECT MIN(sale_date) FROM daily_sales),
                       (SELECT MAX(sale_date) FROM daily_sales),
                       interval '1 day') d
),
filled AS (
  SELECT d.sale_date, COALESCE(s.revenue, 0) AS revenue
  FROM days d LEFT JOIN daily_sales s USING (sale_date)
)
SELECT sale_date, revenue,
       AVG(revenue) OVER (ORDER BY sale_date
                          ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS ma_7d
FROM filled
ORDER BY sale_date;
```

- `ROWS 6 PRECEDING` means "last 7 rows", which equals 7 days only if there is one row per day — hence the date spine. Postgres 11+ also supports `RANGE BETWEEN INTERVAL '6 days' PRECEDING AND CURRENT ROW`, which handles gaps without a spine (but then averages only days that exist).
- The first 6 days average fewer than 7 points; null them out with `CASE WHEN COUNT(*) OVER (...) = 7` if needed.

### Month-over-month growth rate.

**Schema:** `orders(order_id, order_date, amount)`

```sql
WITH monthly AS (
  SELECT date_trunc('month', order_date)::date AS month,
         SUM(amount) AS revenue
  FROM orders
  GROUP BY 1
)
SELECT month, revenue,
       LAG(revenue) OVER (ORDER BY month) AS prev_revenue,
       ROUND(100.0 * (revenue - LAG(revenue) OVER (ORDER BY month))
             / NULLIF(LAG(revenue) OVER (ORDER BY month), 0), 2) AS mom_growth_pct
FROM monthly
ORDER BY month;
```

- `NULLIF(..., 0)` avoids divide-by-zero; `100.0` forces numeric division (integer division is a common bug).
- If a month has no sales, `LAG` compares to the previous *existing* month — join to a month spine (`generate_series`) if gaps matter.

### Day-1 / day-7 retention; cohort analysis.

**Schema:** `users(user_id, signup_date DATE)`, `events(user_id, event_ts TIMESTAMP)`

```sql
-- Day-N retention by signup cohort (daily cohorts)
WITH activity AS (
  SELECT DISTINCT e.user_id, e.event_ts::date AS active_date
  FROM events e
)
SELECT u.signup_date AS cohort,
       COUNT(DISTINCT u.user_id) AS cohort_size,
       ROUND(100.0 * COUNT(DISTINCT a1.user_id) / COUNT(DISTINCT u.user_id), 1) AS d1_retention_pct,
       ROUND(100.0 * COUNT(DISTINCT a7.user_id) / COUNT(DISTINCT u.user_id), 1) AS d7_retention_pct
FROM users u
LEFT JOIN activity a1 ON a1.user_id = u.user_id AND a1.active_date = u.signup_date + 1
LEFT JOIN activity a7 ON a7.user_id = u.user_id AND a7.active_date = u.signup_date + 7
GROUP BY u.signup_date
ORDER BY cohort;

-- Monthly cohort matrix: % of cohort active N months after signup
WITH cohort AS (
  SELECT user_id, date_trunc('month', signup_date) AS cohort_month FROM users
),
act AS (
  SELECT DISTINCT user_id, date_trunc('month', event_ts) AS active_month FROM events
),
sizes AS (SELECT cohort_month, COUNT(*) AS n FROM cohort GROUP BY 1)
SELECT c.cohort_month,
       (EXTRACT(YEAR FROM age(a.active_month, c.cohort_month)) * 12
        + EXTRACT(MONTH FROM age(a.active_month, c.cohort_month)))::int AS month_number,
       ROUND(100.0 * COUNT(DISTINCT a.user_id) / s.n, 1) AS retention_pct
FROM cohort c
JOIN act a ON a.user_id = c.user_id AND a.active_month >= c.cohort_month
JOIN sizes s ON s.cohort_month = c.cohort_month
GROUP BY c.cohort_month, month_number, s.n
ORDER BY 1, 2;
```

- Clarify the definition: "active exactly on day 7" vs "active any time in days 1–7" (rolling/unbounded retention). Exclude cohorts too young to have a day-7 yet (`signup_date <= CURRENT_DATE - 7`), or they drag the number down.
- Watch timezones: casting to date in UTC vs the user's local day changes results.

### Users who did X but not Y (anti-join).

**Schema:** `events(user_id, event_name, event_ts)` — users who added to cart but never purchased.

```sql
SELECT DISTINCT e.user_id
FROM events e
WHERE e.event_name = 'add_to_cart'
  AND NOT EXISTS (
    SELECT 1 FROM events p
    WHERE p.user_id = e.user_id AND p.event_name = 'purchase'
  );

-- Equivalent LEFT JOIN anti-join
SELECT DISTINCT a.user_id
FROM events a
LEFT JOIN events p ON p.user_id = a.user_id AND p.event_name = 'purchase'
WHERE a.event_name = 'add_to_cart' AND p.user_id IS NULL;

-- Aggregate version (one scan)
SELECT user_id
FROM events
GROUP BY user_id
HAVING BOOL_OR(event_name = 'add_to_cart') AND NOT BOOL_OR(event_name = 'purchase');
```

- Avoid `NOT IN (subquery)`: if the subquery returns any NULL, `NOT IN` returns no rows. `NOT EXISTS` is NULL-safe and the planner turns it into an anti-join.
- Ask about ordering: "added to cart and did not purchase *afterwards*" needs `p.event_ts > e.event_ts` in the condition.

### Consecutive days logged in (gaps and islands).

**Schema:** `logins(user_id, login_ts TIMESTAMP)` — find streaks and users with 3+ consecutive days.

```sql
WITH d AS (
  SELECT DISTINCT user_id, login_ts::date AS day FROM logins
),
grp AS (
  SELECT user_id, day,
         day - (ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY day))::int AS island
  FROM d
)
SELECT user_id, MIN(day) AS streak_start, MAX(day) AS streak_end, COUNT(*) AS streak_len
FROM grp
GROUP BY user_id, island
HAVING COUNT(*) >= 3
ORDER BY user_id, streak_start;
```

- Trick: for consecutive dates, `date - row_number` is constant, so it labels each island. `DISTINCT` first, because multiple logins per day would break the arithmetic.
- For the longest streak per user, wrap and take `MAX(streak_len)` per user.

### Sessionise events (new session after 30 minutes of inactivity).

**Schema:** `events(user_id, event_ts TIMESTAMP, event_name)`

```sql
WITH flagged AS (
  SELECT *,
         CASE WHEN LAG(event_ts) OVER w IS NULL
                OR event_ts - LAG(event_ts) OVER w > INTERVAL '30 minutes'
              THEN 1 ELSE 0 END AS is_new_session
  FROM events
  WINDOW w AS (PARTITION BY user_id ORDER BY event_ts)
),
numbered AS (
  SELECT *,
         SUM(is_new_session) OVER (PARTITION BY user_id ORDER BY event_ts
                                   ROWS UNBOUNDED PRECEDING) AS session_seq
  FROM flagged
)
SELECT user_id, session_seq,
       user_id || '-' || session_seq AS session_id,
       MIN(event_ts) AS session_start, MAX(event_ts) AS session_end,
       COUNT(*) AS events
FROM numbered
GROUP BY user_id, session_seq
ORDER BY user_id, session_seq;
```

- Pattern: flag boundaries with `LAG`, then a running `SUM` of the flags gives a session number. Same pattern solves most "group consecutive rows" problems.
- At scale (billions of events) this is a Spark/streaming job with session windows; mention that in incremental pipelines sessions can span batch boundaries.

### Funnel conversion between steps.

**Schema:** `events(user_id, event_name, event_ts)`; funnel: `view_product -> add_to_cart -> checkout -> purchase`

```sql
WITH firsts AS (
  SELECT user_id,
         MIN(event_ts) FILTER (WHERE event_name = 'view_product') AS t_view,
         MIN(event_ts) FILTER (WHERE event_name = 'add_to_cart')  AS t_cart,
         MIN(event_ts) FILTER (WHERE event_name = 'checkout')     AS t_checkout,
         MIN(event_ts) FILTER (WHERE event_name = 'purchase')     AS t_purchase
  FROM events
  GROUP BY user_id
),
steps AS (                      -- enforce order: each step after the previous one
  SELECT user_id,
         t_view IS NOT NULL                                      AS s1,
         t_view IS NOT NULL AND t_cart > t_view                  AS s2,
         t_view IS NOT NULL AND t_cart > t_view AND t_checkout > t_cart AS s3,
         t_view IS NOT NULL AND t_cart > t_view AND t_checkout > t_cart
           AND t_purchase > t_checkout                           AS s4
  FROM firsts
)
SELECT COUNT(*) FILTER (WHERE s1) AS viewed,
       COUNT(*) FILTER (WHERE s2) AS carted,
       COUNT(*) FILTER (WHERE s3) AS checked_out,
       COUNT(*) FILTER (WHERE s4) AS purchased,
       ROUND(100.0 * COUNT(*) FILTER (WHERE s2) / NULLIF(COUNT(*) FILTER (WHERE s1), 0), 1) AS view_to_cart_pct,
       ROUND(100.0 * COUNT(*) FILTER (WHERE s3) / NULLIF(COUNT(*) FILTER (WHERE s2), 0), 1) AS cart_to_checkout_pct,
       ROUND(100.0 * COUNT(*) FILTER (WHERE s4) / NULLIF(COUNT(*) FILTER (WHERE s3), 0), 1) AS checkout_to_purchase_pct
FROM steps;
```

- Comparisons with NULL yield NULL, which `FILTER` treats as false, so users who never reached a step drop out correctly.
- Simplification: uses first occurrence of each event. Clarify whether steps must be in order, within a time window (e.g. same session or 7 days), and whether the unit is user or session.

### Median without a MEDIAN function.

**Schema:** `employees(id, salary)`

```sql
-- Postgres has an ordered-set aggregate:
SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median FROM employees;

-- Without it: average the middle one or two rows
WITH ordered AS (
  SELECT salary,
         ROW_NUMBER() OVER (ORDER BY salary) AS rn,
         COUNT(*)     OVER ()                AS n
  FROM employees
  WHERE salary IS NOT NULL
)
SELECT AVG(salary) AS median
FROM ordered
WHERE rn IN ((n + 1) / 2, (n + 2) / 2);   -- integer division
```

- Odd n=5: rows 3 and 3 → row 3. Even n=6: rows 3 and 4 → their average.
- Per group: add `PARTITION BY dept` to both windows and `GROUP BY dept`.

### Pivot / unpivot a table.

**Schema:** `sales(region, quarter, revenue)` with quarter in `('Q1'..'Q4')`

```sql
-- Pivot: one row per region, one column per quarter (conditional aggregation — portable)
SELECT region,
       SUM(revenue) FILTER (WHERE quarter = 'Q1') AS q1,
       SUM(revenue) FILTER (WHERE quarter = 'Q2') AS q2,
       SUM(revenue) FILTER (WHERE quarter = 'Q3') AS q3,
       SUM(revenue) FILTER (WHERE quarter = 'Q4') AS q4
FROM sales
GROUP BY region;

-- Unpivot: wide table sales_wide(region, q1, q2, q3, q4) back to long
SELECT s.region, v.quarter, v.revenue
FROM sales_wide s
CROSS JOIN LATERAL (VALUES ('Q1', s.q1), ('Q2', s.q2), ('Q3', s.q3), ('Q4', s.q4))
     AS v(quarter, revenue);
```

- `SUM(CASE WHEN quarter='Q1' THEN revenue END)` is the fully portable form. Postgres also has `crosstab()` (tablefunc extension); Snowflake/Databricks/SQL Server have `PIVOT`/`UNPIVOT` keywords.
- SQL needs the output columns known up front; dynamic pivots require generating SQL or pivoting in the BI tool / pandas.

### Find customers whose spend increased every month.

**Schema:** `orders(order_id, customer_id, order_date, amount)`

```sql
WITH monthly AS (
  SELECT customer_id, date_trunc('month', order_date) AS month, SUM(amount) AS spend
  FROM orders
  GROUP BY 1, 2
),
diffs AS (
  SELECT customer_id, month, spend,
         LAG(spend) OVER (PARTITION BY customer_id ORDER BY month) AS prev_spend,
         LAG(month) OVER (PARTITION BY customer_id ORDER BY month) AS prev_month
  FROM monthly
)
SELECT customer_id
FROM diffs
GROUP BY customer_id
HAVING COUNT(*) >= 2                                            -- need at least 2 months to "increase"
   AND BOOL_AND(prev_spend IS NULL OR spend > prev_spend)       -- strictly increasing
   AND BOOL_AND(prev_month IS NULL OR month = prev_month + INTERVAL '1 month');  -- no gap months
```

- The last condition treats a month with zero spend as a break in the streak. Clarify: must they be active every month in the range, and is "increased" strict?
- Portable alternative to `BOOL_AND`: `SUM(CASE WHEN spend <= prev_spend THEN 1 ELSE 0 END) = 0`.

### Employees earning more than their managers (self-join).

**Schema:** `employees(id, name, salary, manager_id REFERENCES employees(id))`

```sql
SELECT e.name AS employee, e.salary, m.name AS manager, m.salary AS manager_salary
FROM employees e
JOIN employees m ON m.id = e.manager_id
WHERE e.salary > m.salary;
```

- Inner join excludes the CEO (`manager_id IS NULL`) — correct here, since they have no manager to compare with.
- Same table, two roles; aliases make it readable.

### Deduplicate keeping the latest record per key.

**Schema:** `customer_updates(customer_id, email, address, updated_at, ingested_at)`

```sql
-- Read the latest per key
SELECT *
FROM (
  SELECT c.*, ROW_NUMBER() OVER (PARTITION BY customer_id
                                 ORDER BY updated_at DESC, ingested_at DESC) AS rn
  FROM customer_updates c
) t
WHERE rn = 1;

-- Postgres shorthand
SELECT DISTINCT ON (customer_id) *
FROM customer_updates
ORDER BY customer_id, updated_at DESC, ingested_at DESC;

-- Physically delete older versions (needs a unique row id or use ctid)
DELETE FROM customer_updates c
USING customer_updates newer
WHERE newer.customer_id = c.customer_id
  AND (newer.updated_at, newer.ingested_at) > (c.updated_at, c.ingested_at);
```

- Always include a tiebreaker in `ORDER BY`; otherwise two rows with the same `updated_at` give nondeterministic results between runs.
- In a warehouse this is typically a `MERGE` into a target table or a dbt incremental model with `unique_key`.

### Slowly changing dimension: point-in-time lookup.

**Schema:** SCD Type 2 `dim_customer(customer_sk, customer_id, segment, valid_from TIMESTAMP, valid_to TIMESTAMP NULL, is_current BOOL)`; fact `orders(order_id, customer_id, order_ts, amount)`

```sql
-- What segment was the customer in at the time of each order?
SELECT o.order_id, o.customer_id, o.order_ts, o.amount, d.segment
FROM orders o
LEFT JOIN dim_customer d
  ON d.customer_id = o.customer_id
 AND o.order_ts >= d.valid_from
 AND o.order_ts <  COALESCE(d.valid_to, 'infinity'::timestamp);

-- Snapshot of all customers as of a date
SELECT * FROM dim_customer
WHERE valid_from <= TIMESTAMP '2026-01-01'
  AND (valid_to IS NULL OR valid_to > TIMESTAMP '2026-01-01');
```

- Use half-open intervals `[valid_from, valid_to)` so a timestamp matches exactly one version. Overlapping versions in the dimension would duplicate fact rows — test for that.
- Better practice: resolve the surrogate key at load time and store `customer_sk` on the fact, so queries are a simple equi-join.
- `LEFT JOIN` keeps orders that predate the first dimension version (data quality signal).

### Recursive CTE: org chart / bill of materials hierarchy.

**Schema:** `employees(id, name, manager_id)`; BOM `components(parent_part, child_part, qty)`

```sql
-- All reports (direct and indirect) under manager 42, with depth and path
WITH RECURSIVE org AS (
  SELECT id, name, manager_id, 0 AS depth, ARRAY[id] AS path
  FROM employees WHERE id = 42                       -- anchor
  UNION ALL
  SELECT e.id, e.name, e.manager_id, o.depth + 1, o.path || e.id
  FROM employees e
  JOIN org o ON e.manager_id = o.id                  -- recursive step
  WHERE NOT e.id = ANY(o.path)                       -- cycle guard
)
SELECT * FROM org ORDER BY depth, name;

-- Bill of materials: total quantity of each leaf part needed for one 'BIKE'
WITH RECURSIVE bom AS (
  SELECT child_part, qty::numeric AS total_qty
  FROM components WHERE parent_part = 'BIKE'
  UNION ALL
  SELECT c.child_part, b.total_qty * c.qty
  FROM components c JOIN bom b ON c.parent_part = b.child_part
)
SELECT child_part, SUM(total_qty) AS total_qty
FROM bom
WHERE child_part NOT IN (SELECT parent_part FROM components)   -- leaves only (parent_part is NOT NULL)
GROUP BY child_part;
```

- Anchor + `UNION ALL` + recursive member; recursion stops when the step returns no rows.
- Guard against cycles (path array, or Postgres 14+ `CYCLE` clause) — dirty customer HR data often has loops. Quantities multiply down the tree in BOMs.

### Join three tables and explain the result row count.

**Schema:** `customers(customer_id, name)`, `orders(order_id, customer_id, order_date)`, `order_items(order_item_id, order_id, product_id, qty)`

```sql
SELECT c.customer_id, c.name, o.order_id, oi.product_id, oi.qty
FROM customers c
LEFT JOIN orders o       ON o.customer_id = c.customer_id
LEFT JOIN order_items oi ON oi.order_id   = o.order_id;
```

**Row count reasoning:**
- Joins multiply along one-to-many relationships. The output grain becomes the finest table: one row per order item, plus one row for each order with no items, plus one row for each customer with no orders (because of `LEFT JOIN`, with NULLs).
- With `INNER JOIN`s, customers without orders and orders without items disappear: count = number of order items whose order and customer exist.
- If the join key is not unique on the "one" side (e.g. duplicate `customer_id` in `customers`), rows multiply unexpectedly — fan-out. Joining two independent one-to-many children (orders and support_tickets) to the same customer produces orders x tickets per customer, and `SUM(amount)` becomes inflated.
- How I check: `COUNT(*)` before and after each join, and `SELECT key, COUNT(*) ... HAVING COUNT(*) > 1` on the side I expect to be unique. Fix fan-out by aggregating each child to the parent grain before joining.

### Find overlapping date ranges / bookings.

**Schema:** `bookings(booking_id, room_id, start_ts TIMESTAMPTZ, end_ts TIMESTAMPTZ)` (end exclusive)

```sql
-- All pairs of overlapping bookings for the same room
SELECT a.booking_id AS booking_a, b.booking_id AS booking_b, a.room_id
FROM bookings a
JOIN bookings b
  ON a.room_id = b.room_id
 AND a.booking_id < b.booking_id          -- each pair once, no self-match
 AND a.start_ts < b.end_ts
 AND b.start_ts < a.end_ts;

-- Postgres range types
SELECT a.booking_id, b.booking_id
FROM bookings a JOIN bookings b
  ON a.room_id = b.room_id AND a.booking_id < b.booking_id
 AND tstzrange(a.start_ts, a.end_ts) && tstzrange(b.start_ts, b.end_ts);

-- Prevent overlaps at write time
CREATE EXTENSION IF NOT EXISTS btree_gist;
ALTER TABLE bookings ADD CONSTRAINT no_overlap
  EXCLUDE USING gist (room_id WITH =, tstzrange(start_ts, end_ts) WITH &&);
```

- Two intervals overlap iff `start1 < end2 AND start2 < end1`. Use `<=` if touching endpoints count as overlap.
- The self-join is O(n^2) per room; for large data, sort by start and compare with `LAG(MAX(end_ts))` (a running max of end), which is O(n log n).

## B. SQL concepts

### INNER vs. LEFT vs. RIGHT vs. FULL OUTER vs. CROSS join.

- **INNER:** only rows with a match on both sides.
- **LEFT (OUTER):** all rows from the left, matched right rows or NULLs. Most common in analytics ("all customers, with orders if any").
- **RIGHT:** mirror of LEFT; rarely used — rewrite as LEFT by swapping tables for readability.
- **FULL OUTER:** all rows from both sides, NULLs where no match. Great for reconciliation: `WHERE a.id IS NULL OR b.id IS NULL` shows records missing in either system.
- **CROSS:** Cartesian product, n x m rows. Useful deliberately (date spine x products to fill zeros), dangerous accidentally.

Gotchas I mention:
- A filter on the right table in `WHERE` turns a LEFT JOIN into an INNER JOIN (`WHERE o.status = 'paid'` drops NULL rows). Put it in the `ON` clause instead.
- Any join can multiply rows if the key isn't unique on one side.
- NULL keys never match (`NULL = NULL` is unknown); use `IS NOT DISTINCT FROM` if you need them to.

### WHERE vs. HAVING.

- `WHERE` filters **rows before** grouping/aggregation; it cannot reference aggregates.
- `HAVING` filters **groups after** `GROUP BY`; it can reference aggregates.
- Logical order: `FROM/JOIN -> WHERE -> GROUP BY -> HAVING -> SELECT (window functions) -> DISTINCT -> ORDER BY -> LIMIT`.

```sql
SELECT customer_id, SUM(amount) AS total
FROM orders
WHERE order_date >= DATE '2026-01-01'   -- row filter, can use an index
GROUP BY customer_id
HAVING SUM(amount) > 1000;              -- group filter
```

- Performance: push non-aggregate filters into `WHERE` so fewer rows get grouped. Window functions are evaluated after `HAVING`, so filtering on them needs a subquery/CTE (or `QUALIFY` in Snowflake/Databricks).

### GROUP BY vs. window functions (PARTITION BY).

- `GROUP BY` **collapses** rows: one output row per group; you lose row-level detail.
- Window functions **keep every row** and add a computed column over a related set of rows (the window): `PARTITION BY` defines the group, `ORDER BY` and the frame define which rows within it.

```sql
-- Each order with its share of the customer's total
SELECT order_id, customer_id, amount,
       amount / SUM(amount) OVER (PARTITION BY customer_id) AS share_of_customer
FROM orders;
```

- Use window functions for ranking, running totals, LAG/LEAD comparisons, percent-of-total, dedup. Use GROUP BY when you want a summary table.
- They combine: window functions can run over the result of a GROUP BY (`SUM(SUM(amount)) OVER ()` for grand total share).

### ROW_NUMBER vs. RANK vs. DENSE_RANK.

For salaries 100, 90, 90, 80:

| salary | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|
| 100 | 1 | 1 | 1 |
| 90 | 2 | 2 | 2 |
| 90 | 3 | 2 | 2 |
| 80 | 4 | 4 | 3 |

- **ROW_NUMBER:** unique sequential numbers; ties broken arbitrarily (add a tiebreaker for determinism). Use for dedup and "exactly N per group".
- **RANK:** ties share a rank, then gaps (Olympic medals). Use for "top N including ties".
- **DENSE_RANK:** ties share a rank, no gaps. Use for "Nth highest distinct value".

### UNION vs. UNION ALL.

- `UNION` concatenates and **removes duplicates** — requires a sort or hash over the whole result, so it is slower.
- `UNION ALL` concatenates and **keeps everything** — cheap, streaming.
- Default to `UNION ALL`; use `UNION` only when you actually need deduplication, and say so. Accidental `UNION` can silently drop legitimate identical rows (two identical $10 transactions).
- Both require the same number of columns with compatible types; column names come from the first query.

### How do NULLs behave in comparisons, joins and aggregates?

- **Comparisons:** NULL means unknown. `NULL = NULL`, `NULL <> 1`, `NULL > 0` are all `NULL` (unknown), and `WHERE` keeps only TRUE. Use `IS NULL` / `IS NOT NULL` / `IS DISTINCT FROM`.
- **Three-valued logic:** `TRUE OR NULL = TRUE`, `FALSE AND NULL = FALSE`, everything else with NULL is NULL. `NOT IN (1, NULL)` is never true — the classic bug.
- **Joins:** NULL keys never match each other, so rows with NULL join keys drop out of inner joins.
- **Aggregates:** `COUNT(*)` counts rows; `COUNT(col)`, `SUM`, `AVG`, `MIN`, `MAX` ignore NULLs. `AVG(col)` divides by non-null count — different from treating NULL as 0. `SUM` of all NULLs is NULL, not 0 (wrap with `COALESCE`).
- **GROUP BY / DISTINCT:** NULLs are grouped together as one group.
- **Sorting:** Postgres puts NULLs last for ASC and first for DESC by default; control with `NULLS FIRST/LAST`.
- **Arithmetic/concat:** `1 + NULL` is NULL; `'a' || NULL` is NULL in Postgres (use `CONCAT`, which ignores NULLs).

### CTEs vs. subqueries vs. temp tables.

- **Subquery:** inline, scoped to one place. Fine for small things; nested subqueries get unreadable.
- **CTE (`WITH`):** named, readable, can be referenced multiple times, and supports recursion. In Postgres 12+ non-recursive CTEs referenced once are inlined (optimised like subqueries); use `MATERIALIZED` / `NOT MATERIALIZED` to control. Before 12 they were an optimisation fence.
- **Temp table:** physically stored for the session; can be indexed and `ANALYZE`d so the planner has real statistics. Good for multi-step scripts, reusing an expensive intermediate result many times, or breaking a huge query that the planner mis-estimates.
- Rule of thumb: CTEs for readability by default; temp tables when the intermediate is expensive and reused or needs an index; in warehouses, a dbt model (table/view) when it is reused across queries.

### What are indexes? B-tree vs. hash vs. bitmap. When does an index hurt?

An index is a separate data structure that maps key values to row locations so the database can find rows without scanning the whole table.

- **B-tree (default):** balanced sorted tree, O(log n). Supports `=`, ranges, `ORDER BY`, prefix `LIKE 'abc%'`. Composite indexes follow the leftmost-prefix rule: `(a, b)` helps `WHERE a = ?` and `WHERE a = ? AND b > ?` but not `WHERE b = ?` alone.
- **Hash:** equality only, no ranges or sorting. Occasionally smaller/faster for pure `=` lookups; B-tree is usually fine.
- **Bitmap:** one bitmap per distinct value; very compact for low-cardinality columns (status, gender) and cheap to AND/OR together. Common in data warehouses (Oracle bitmap indexes). Postgres has no persistent bitmap index but builds **bitmap scans** at query time to combine indexes.
- Others worth naming: GIN (JSONB, arrays, full-text), GiST (ranges, geo), BRIN (huge append-only tables ordered by time — tiny index of block min/max).

**When an index hurts:**
- Writes: every INSERT/UPDATE/DELETE must update each index — slows ingestion, adds locking and WAL.
- Storage and memory; unused indexes waste cache.
- Low selectivity: if a query returns a large fraction of the table, a sequential scan is faster than random index lookups.
- Indexes not used when you wrap the column in a function (`WHERE LOWER(email) = ...` needs an expression index), on implicit type casts, or leading-wildcard `LIKE '%x'`.
- Analytical column stores (Snowflake, BigQuery, Delta) rely on partitioning, clustering and min/max pruning instead of B-trees.

### How do you read an EXPLAIN plan? Optimise a slow query.

**Reading it (Postgres):**
- `EXPLAIN` shows the estimated plan; `EXPLAIN (ANALYZE, BUFFERS)` actually runs it and shows real time, rows and I/O.
- Read the tree bottom-up / innermost first: scans (Seq Scan, Index Scan, Index Only Scan, Bitmap Heap Scan) feed joins (Nested Loop, Hash Join, Merge Join), then Sort, Aggregate, Limit.
- Look for: the node with the largest actual time; **estimated vs actual rows** far apart (stale statistics, correlated columns -> bad join choice); Seq Scans on large tables with selective filters; Sorts/hashes spilling to disk ("external merge", "Batches: >1"); nested loops over many rows; "Rows Removed by Filter" huge.

**Optimisation checklist:**
1. Reproduce and measure with `EXPLAIN ANALYZE` on realistic data.
2. `ANALYZE` the tables if estimates are off; consider extended statistics.
3. Add or fix indexes matching the filter/join/sort (composite, covering with `INCLUDE`, partial, expression).
4. Make predicates sargable: no functions on indexed columns, matching types, no `OR` across columns (use `UNION ALL`).
5. Reduce data early: filter before joins, select only needed columns, pre-aggregate before joining to avoid fan-out.
6. Replace correlated subqueries with joins; `NOT IN` with `NOT EXISTS`.
7. Tune `work_mem` for sorts/hashes; partition very large tables by date for pruning.
8. If it is still too slow, change the design: materialised view, summary table, or move the workload to a warehouse.

### Normalisation (1NF, 2NF, 3NF) and when to denormalise.

- **1NF:** atomic values, no repeating groups (no `phone1, phone2, phone3` or comma-separated lists); each row unique.
- **2NF:** 1NF plus no partial dependency — every non-key column depends on the **whole** composite key. In `order_items(order_id, product_id, product_name)`, `product_name` depends only on `product_id`, so move it to `products`.
- **3NF:** 2NF plus no transitive dependency — non-key columns depend only on the key. In `employees(id, dept_id, dept_name)`, `dept_name` depends on `dept_id`; move it to `departments`. ("The key, the whole key, and nothing but the key.")
- **Why:** removes update/insert/delete anomalies, keeps one source of truth — right for OLTP.

**When to denormalise:**
- Analytics/OLAP: star schemas, wide tables for BI tools, fewer joins over billions of rows.
- Read-heavy hot paths where joins are the bottleneck (cache a computed `order_total`, copy `customer_name` onto a search document).
- Document/NoSQL stores modelled around access patterns.
- Cost: duplicated data must be kept in sync (triggers, pipelines, eventual consistency). Denormalise deliberately and from a normalised source of truth.

### Transactions, ACID, isolation levels (read committed, repeatable read, serializable).

**ACID:**
- **Atomicity:** all or nothing (rollback on failure).
- **Consistency:** a transaction moves the DB from one valid state to another (constraints hold).
- **Isolation:** concurrent transactions don't see each other's partial work (to the degree set by the isolation level).
- **Durability:** once committed, survives crashes (write-ahead log).

**Anomalies and isolation levels:**

| Level | Dirty read | Non-repeatable read | Phantom read | Notes |
|---|---|---|---|---|
| Read uncommitted | possible (not in Postgres) | possible | possible | Postgres treats it as read committed |
| Read committed | no | possible | possible | Postgres default; each statement sees data committed before it started |
| Repeatable read | no | no | possible per SQL standard; prevented in Postgres (snapshot isolation) | Whole transaction sees one snapshot; write skew still possible |
| Serializable | no | no | no | Result equals some serial order; Postgres SSI aborts conflicting transactions — app must retry |

- Example of why it matters: two transactions both check "at least one doctor on call" and both go off call (write skew) — only serializable (or explicit `SELECT ... FOR UPDATE`) prevents it.
- Trade-off: stronger isolation = more aborts/locking, lower throughput. Use read committed by default, row locks or serializable for invariants spanning rows, and always build retry logic for serialization failures.

### Deadlocks — what and how to avoid?

A deadlock is a cycle of transactions each waiting for a lock the other holds: T1 locks row A then wants B; T2 locks B then wants A. Neither can proceed. Postgres detects the cycle (after `deadlock_timeout`) and aborts one transaction with an error.

**Avoid:**
- Acquire locks in a **consistent order** (e.g. always update accounts in ascending id order).
- Keep transactions **short**; no user interaction or network calls inside a transaction.
- Lock what you need up front (`SELECT ... FOR UPDATE` in a defined order) rather than escalating later.
- Use appropriate indexes so updates lock fewer rows; batch large updates into smaller chunks.
- Use lower isolation or optimistic concurrency (version column) where possible; `NOWAIT` / `SKIP LOCKED` for queue workers.
- **Handle them anyway:** catch deadlock/serialization errors and retry with backoff; monitor `pg_locks` and logs (`log_lock_waits`).

### Primary vs. foreign vs. composite vs. surrogate keys.

- **Primary key:** uniquely identifies each row; unique and NOT NULL; one per table; usually backed by an index.
- **Foreign key:** column(s) referencing another table's primary/unique key; enforces referential integrity (no orders for non-existent customers) with `ON DELETE` actions (RESTRICT, CASCADE, SET NULL).
- **Composite key:** a key made of multiple columns, e.g. `order_items(order_id, line_no)` or a junction table `(student_id, course_id)`.
- **Natural key:** meaningful business value (email, SSN, ISBN). **Surrogate key:** system-generated, meaningless (identity/bigserial, UUID).
- Trade-offs: natural keys can change, be reused, or be non-unique in practice (emails change; customer data is messy), so I typically use a surrogate primary key plus a `UNIQUE` constraint on the natural key. In warehouses, surrogate keys are required for SCD Type 2 (multiple versions of the same natural key). UUIDs are good for distributed generation but larger and, if random (v4), fragment B-tree indexes — prefer UUIDv7 or bigint where order matters.

### Views vs. materialised views.

- **View:** a stored query; no data stored. Always current, costs the query time on each read. Used for abstraction, security (expose a subset of columns/rows), and a stable interface over changing tables.
- **Materialised view:** stores the query result physically. Fast reads, can be indexed, but data is stale until refreshed. In Postgres, `REFRESH MATERIALIZED VIEW` recomputes fully; `CONCURRENTLY` (needs a unique index) avoids blocking readers. Snowflake/Databricks/BigQuery offer automatic incremental refresh for some query shapes.
- Choose a view when the query is cheap or freshness is critical; a materialised view (or a scheduled summary table / dbt table) for expensive aggregations read often, where some staleness is acceptable.

## C. Data modeling

### Design a schema for an e-commerce store / hospital / airline / ride-sharing app.

Approach (say it out loud): clarify use cases and scale -> identify entities and relationships -> keys and constraints -> access patterns and indexes -> what goes to analytics. Worked example: **e-commerce (OLTP, Postgres)**.

```sql
CREATE TABLE customers (
  customer_id  BIGSERIAL PRIMARY KEY,
  email        TEXT NOT NULL UNIQUE,
  name         TEXT NOT NULL,
  created_at   TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE TABLE addresses (
  address_id   BIGSERIAL PRIMARY KEY,
  customer_id  BIGINT NOT NULL REFERENCES customers,
  line1 TEXT, city TEXT, postcode TEXT, country CHAR(2)
);
CREATE TABLE categories (
  category_id  BIGSERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  parent_id    BIGINT REFERENCES categories          -- category tree
);
CREATE TABLE products (
  product_id   BIGSERIAL PRIMARY KEY,
  sku          TEXT NOT NULL UNIQUE,
  name         TEXT NOT NULL,
  category_id  BIGINT REFERENCES categories,
  price_cents  INTEGER NOT NULL CHECK (price_cents >= 0),
  attributes   JSONB                                  -- variable per category
);
CREATE TABLE inventory (
  product_id   BIGINT REFERENCES products,
  warehouse_id BIGINT,
  quantity     INTEGER NOT NULL CHECK (quantity >= 0),
  PRIMARY KEY (product_id, warehouse_id)
);
CREATE TABLE orders (
  order_id     BIGSERIAL PRIMARY KEY,
  customer_id  BIGINT NOT NULL REFERENCES customers,
  shipping_address_id BIGINT REFERENCES addresses,
  status       TEXT NOT NULL CHECK (status IN ('pending','paid','shipped','delivered','cancelled','refunded')),
  total_cents  INTEGER NOT NULL,
  created_at   TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE TABLE order_items (
  order_id     BIGINT REFERENCES orders,
  line_no      INTEGER,
  product_id   BIGINT NOT NULL REFERENCES products,
  quantity     INTEGER NOT NULL CHECK (quantity > 0),
  unit_price_cents INTEGER NOT NULL,                 -- price at time of purchase
  PRIMARY KEY (order_id, line_no)
);
CREATE TABLE payments (
  payment_id   BIGSERIAL PRIMARY KEY,
  order_id     BIGINT NOT NULL REFERENCES orders,
  provider_ref TEXT UNIQUE,                           -- idempotency with the PSP
  amount_cents INTEGER NOT NULL,
  status       TEXT NOT NULL,
  created_at   TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE INDEX ON orders (customer_id, created_at DESC);
CREATE INDEX ON order_items (product_id);
```

Design points to call out:
- Store **price at purchase** on `order_items` (products change price); money as integer cents or `NUMERIC`, never float.
- Order status history in an `order_events` table for audit; payments separate because one order can have several attempts/refunds.
- `JSONB` for category-specific attributes instead of hundreds of nullable columns.
- Inventory decrement with `SELECT ... FOR UPDATE` or a conditional `UPDATE ... WHERE quantity >= n` to prevent overselling.
- Analytics: replicate via CDC into a warehouse star schema (`fact_order_items`, `dim_customer`, `dim_product`, `dim_date`).

Other domains — core entities to name quickly:
- **Hospital:** patients, providers, departments, encounters/visits, appointments, diagnoses (ICD-10 codes), procedures, prescriptions/medications, lab orders/results, beds/admissions, insurance/claims. Emphasise PHI access control, audit logs, and that clinical records are append-only/versioned, not overwritten.
- **Airline:** airports, aircraft, routes, flights (scheduled) vs flight_instances (date-specific), fares/fare classes, bookings (PNR), passengers, tickets/segments, seats and seat assignments, crew. Key constraint: one passenger per seat per flight instance (unique constraint); overbooking rules.
- **Ride-sharing:** riders, drivers, vehicles, trips (request, match, pickup, dropoff timestamps; status), driver location pings (high-volume time series in a separate store), pricing/surge, payments, ratings. Hot path is geo-matching (geo index / H3 cells, in-memory store), not the relational DB.

### Star schema vs. snowflake schema; facts vs. dimensions.

- **Fact table:** measurable events at a declared **grain** (one row per order line, per ad impression, per daily balance). Holds foreign keys to dimensions plus numeric measures (quantity, revenue). Long and narrow, grows fast. Types: transactional, periodic snapshot, accumulating snapshot.
- **Dimension table:** descriptive context for slicing/filtering (customer, product, date, store). Wide, relatively small, denormalised attributes, surrogate keys.
- **Star schema:** fact in the middle, each dimension denormalised into one table directly joined to the fact. Simple, few joins, BI-friendly, fast. The default choice (Kimball).
- **Snowflake schema:** dimensions normalised into sub-dimensions (`product -> category -> department`). Less redundancy, easier maintenance of hierarchies, but more joins and harder for analysts. Storage savings rarely matter on modern columnar warehouses.
- In interviews: always state the grain first, use conformed dimensions shared across fact tables (same `dim_customer` for orders and support tickets), and include a `dim_date`.

### Slowly changing dimensions (Type 1, 2, 3).

How to handle dimension attributes that change over time (a customer moves city):
- **Type 0:** never change (original value, e.g. signup date).
- **Type 1 — overwrite:** update in place; no history. Use for corrections (typo fixes) or when history doesn't matter. Historical reports silently change.
- **Type 2 — add a new row:** new version with a new surrogate key, `valid_from`, `valid_to`, `is_current`. Full history; facts point to the version that was true at event time. Most common for analytics where "as was" reporting matters (sales by the region the customer was in at the time). Cost: table grows, joins need the surrogate key or a point-in-time join.
- **Type 3 — add a column:** `current_city`, `previous_city`. Limited history (one previous value); useful for a known reorganisation ("old territory vs new territory").
- Hybrids (Type 4 history table, Type 6 = 1+2+3) exist.
- Implementation: dbt snapshots (`strategy: timestamp` or `check`), or `MERGE` in Delta/Snowflake. Detect changes via hash of tracked columns.

### OLTP vs. OLAP.

| | OLTP | OLAP |
|---|---|---|
| Purpose | Run the business: transactions | Analyse the business: reporting, BI, ML |
| Queries | Many small reads/writes by key | Few large scans and aggregations |
| Schema | Normalised (3NF) | Denormalised (star), wide tables |
| Storage | Row-oriented | Column-oriented, compressed |
| Concurrency | Thousands of users, low latency (ms) | Fewer users, seconds-minutes |
| Data | Current state | History, many sources |
| Examples | Postgres, MySQL, Oracle, DynamoDB | Snowflake, BigQuery, Redshift, Databricks SQL, ClickHouse |

- Don't run heavy analytics on the production OLTP DB — replicate via CDC/ELT into a warehouse. HTAP systems (SingleStore, TiDB, AlloyDB) blur the line.

### Data lake vs. data warehouse vs. lakehouse.

- **Data warehouse:** structured, schema-on-write, curated, SQL, strong governance and performance; proprietary storage. Snowflake, BigQuery, Redshift. Great for BI; costlier for raw/unstructured data and ML on files.
- **Data lake:** cheap object storage (S3/ADLS/GCS) holding any format (raw JSON, CSV, images, Parquet), schema-on-read. Flexible and cheap, but historically no ACID, no updates/deletes, weak governance — risk of a "data swamp".
- **Lakehouse:** open table formats (Delta Lake, Apache Iceberg, Hudi) on top of lake storage, adding ACID transactions, schema enforcement/evolution, time travel, MERGE/updates, and indexing/statistics, so one copy of data serves BI (SQL engines) and ML (Spark, Python). Databricks, Snowflake with Iceberg tables, AWS Athena/Glue with Iceberg.
- Typical architecture: medallion layers — bronze (raw), silver (cleaned, conformed), gold (business aggregates).
- What I'd tell a customer: choose based on workloads (BI only vs BI + ML + unstructured), existing skills, data volume, and lock-in tolerance.

### Ontology / semantic layer: what is it, why would a customer need one? (Palantir Foundry)

- **What:** a layer that maps raw tables to **business concepts** — objects (Customer, Aircraft, Work Order), their properties, the links between them (Aircraft *has* Maintenance Events), and in Foundry also **actions** (writeback operations like "reassign work order") and functions. A semantic layer in the BI world (dbt Semantic Layer, LookML, Cube) similarly defines metrics and entities once ("active customer", "net revenue").
- **Why customers need it:**
  - Raw data lives in dozens of systems with cryptic names (`T_MTRL_MSTR.MATNR`). Users think in business terms; the ontology is a shared vocabulary.
  - **One definition** of each metric/entity, so dashboards and apps stop disagreeing.
  - Apps, analytics and AI/LLM agents build on objects, not on pipelines — swap the source system without breaking the app.
  - Enables operational workflows: users act on objects, and actions write back with permissions and audit.
  - Central place for security (object/property-level permissions) and lineage.
- **FDE angle:** a big part of deployment work is modelling the ontology with the customer's domain experts: which objects matter for the decision we're supporting, what the primary keys are (often requires entity resolution), and which actions users need.

### How do you model time-series / sensor data?

- **Narrow (long) format:** `readings(device_id, metric, ts, value)` — flexible, handles new sensors without schema change; standard for heterogeneous IoT. **Wide format:** one column per metric — efficient when the sensor set is fixed and queried together.
- Separate **metadata** (devices, sites, sensor types, units, calibration) into dimension tables; keep the readings table lean.
- **Partition by time** (day/month) and cluster/sort by `device_id, ts` so queries for one device over a window prune well. Use columnar formats with compression (delta encoding makes timestamps tiny).
- **Retention and rollups:** keep raw data for N days, downsample to 1-minute/1-hour aggregates (min/max/avg/count) for long-term; continuous aggregates in TimescaleDB, materialised views elsewhere.
- Handle: late and out-of-order data, duplicate readings (dedupe on `device_id, ts`), clock skew (store both device time and ingestion time), timezones (store UTC), gaps (interpolate or forward-fill at query time, not in storage), units normalisation.
- Tech: TimescaleDB (Postgres extension), InfluxDB, ClickHouse, Delta/Iceberg tables in a lakehouse, Kafka for ingestion; AWS Timestream / Azure Data Explorer for managed options.

### How do you model graph/relationship data? When use a graph DB?

- **In relational:** nodes as entity tables, edges as an edge table `edges(src_id, dst_id, type, props, valid_from)`, with indexes on `(src_id)` and `(dst_id)`. Traverse with recursive CTEs. Works well for shallow, known-depth traversals (1–3 hops) and hierarchies.
- **Graph DB** (Neo4j, Amazon Neptune, TigerGraph; Cypher/Gremlin) stores adjacency directly ("index-free adjacency"), so multi-hop traversal cost depends on the neighbourhood, not table size.
- **Use a graph DB when:** queries are variable-depth or many hops (fraud rings, "who is connected to this sanctioned entity within 4 hops"), path finding, network/supply-chain dependency analysis, recommendations, knowledge graphs, identity graphs; and the relationships are the main thing you query.
- **Don't** when the workload is mostly aggregations over large tables, or traversals are shallow — the extra system adds ops cost. Middle ground: graph analytics in Spark (GraphFrames) or Postgres + Apache AGE, or model links in an ontology layer.

### How do you design for multi-tenancy in a schema?

Three main patterns:
1. **Shared schema, `tenant_id` column on every table** — cheapest, easiest to operate at many tenants. Risk: data leakage if a query forgets the filter. Mitigate with Postgres **Row-Level Security** (`CREATE POLICY ... USING (tenant_id = current_setting('app.tenant_id')::bigint)`), composite keys / indexes leading with `tenant_id`, and tenant-aware ORM scopes. Noisy neighbours possible.
2. **Schema per tenant** — same DB, separate schemas. Better isolation, per-tenant customisation and easy per-tenant restore; migrations must run N times; catalog bloat beyond thousands of tenants.
3. **Database (or cluster/account) per tenant** — strongest isolation, per-tenant encryption keys, data residency, independent scaling; highest cost and ops overhead.

- Often hybrid: shared for small tenants, dedicated for large/regulated ones. Shard by `tenant_id` (e.g. Citus) as you grow.
- Also design: tenant-scoped encryption keys, per-tenant quotas/rate limits, tenant id in every log line and metric, and tested data export/deletion per tenant (GDPR).
- FDE angle: enterprise customers often demand dedicated deployments or their own cloud account — be clear on the trade-off.

## D. Pipelines & data quality

### ETL vs. ELT.

- **ETL (Extract, Transform, Load):** transform in a separate engine (Informatica, SSIS, custom Python/Spark) before loading into the target. Made sense when warehouse compute was expensive and limited. Pros: only clean data lands; can strip PII before load. Cons: raw data is lost, every change to logic means re-extracting, the transform tier is a bottleneck.
- **ELT (Extract, Load, Transform):** load raw data into the warehouse/lakehouse first (Fivetran, Airbyte, CDC), then transform in SQL inside it (dbt). Pros: raw history kept so you can re-transform/backfill, scales with warehouse compute, analysts can own transforms in SQL, faster to first data. Cons: raw (possibly sensitive) data sits in the warehouse — needs governance; warehouse cost.
- Modern default is ELT with a medallion layout (raw -> staged -> marts). ETL still fits when you must cleanse/mask before data crosses a boundary (PII, on-prem to cloud) or for heavy non-SQL processing (images, parsing binaries).

### Batch vs. streaming. When would you need streaming?

- **Batch:** process bounded chunks on a schedule (hourly/daily). Simpler, cheaper, easy to re-run and backfill, easier to reason about correctness. Covers most reporting and ML training.
- **Streaming:** process unbounded events continuously with seconds-or-less latency (Kafka + Flink/Spark Structured Streaming/Kafka Streams). Harder: state management, late/out-of-order data, exactly-once, schema changes on a live system, 24/7 operations.
- **Need streaming when the business value decays quickly:** fraud detection at transaction time, real-time alerting on sensor/equipment anomalies, live operational dashboards (logistics, ride dispatch), personalisation/recommendations in session, inventory/pricing updates, CDC replication with low lag.
- First question to a customer: "What decision is made on this data, and how fast does it need to be made?" If the answer is "a daily report", batch it. Micro-batch (every 1–5 minutes) is a common middle ground. Lambda (batch + stream) vs Kappa (stream only, replay from log) architectures exist; today unified engines (Spark, Flink) reduce the need for two code paths.

### Design a pipeline to ingest data from 50 customer source systems.

**Clarify:** which systems (ERP like SAP, CRM like Salesforce, Postgres/Oracle/SQL Server databases, SFTP files, APIs, Kafka)? Volumes and change rates? Freshness needed? Network (on-prem, VPN, private link)? PII/compliance? Who owns each source?

**Design:**
1. **Inventory and prioritise** — catalog the 50 sources (owner, type, volume, SLA, sensitivity). Start with the 3–5 sources that unlock the first use case; don't boil the ocean.
2. **Ingestion by pattern, not by source** — build a small number of reusable, config-driven connector types:
   - Databases: CDC (Debezium -> Kafka, or Fivetran/Airbyte/DMS) or incremental pulls on `updated_at` watermarks.
   - SaaS APIs: managed connectors (Fivetran/Airbyte) or a generic paginated-API ingester with rate limiting and retries.
   - Files: landing bucket (S3/ADLS), event-triggered ingestion (Auto Loader), checksum + manifest.
   - Each source = a YAML/JSON config (connection, tables, primary keys, incremental column, schedule, owner) — onboarding source 51 is a config PR, not new code.
3. **Land raw immutably (bronze)** in object storage / Delta/Iceberg with metadata columns: `_source`, `_ingested_at`, `_batch_id`, file name. Never mutate raw — enables replay.
4. **Standardise (silver):** type casting, dedup, schema mapping to a canonical model, timezones to UTC, entity resolution across systems, SCD2 history.
5. **Serve (gold):** business marts / ontology objects for the use case.
6. **Orchestration:** Airflow/Dagster with one parameterised DAG/asset factory per connector type; retries, SLAs, backfill.
7. **Data quality:** contracts and checks at each layer (row counts vs source, nulls on keys, freshness), quarantine bad records, alerts to the right owner.
8. **Observability and governance:** per-source dashboard (last success, lag, row counts, failures), lineage, a catalog (Unity Catalog / DataHub), secrets in a vault, PII tagging and masking, least-privilege service accounts.

**Risks:** source access and credentials take longer than code (start early); undocumented schemas and schema drift; source systems that can't take load (read from replicas, off-peak); hard deletes not visible to incremental pulls (CDC or periodic full reconciliation).
**Metrics:** sources onboarded, freshness/SLA hit rate, failed runs, DQ failures, time to onboard a new source.

### How do you handle schema drift from upstream sources?

- **Detect:** compare incoming schema with the registered/expected schema on every load (column added/removed/renamed, type changed, nullability). Schema registry (Confluent, with compatibility rules) for Kafka; Auto Loader / Delta schema tracking for files.
- **Classify and policy:**
  - Additive new nullable column — usually safe: auto-evolve (Delta `mergeSchema`, Auto Loader `addNewColumns`) in bronze, alert so modelling can decide to use it.
  - Type widening (int -> bigint) — often auto-allow; narrowing or incompatible change (string -> int) — fail or quarantine.
  - Removed/renamed column — break loudly before downstream silently fills NULLs; check with the source owner.
- **Design for resilience:** land raw as permissive types (strings/JSON/variant) in bronze and apply explicit casts in silver; capture unexpected fields in a `_rescued_data` column rather than dropping them; select columns explicitly downstream (no `SELECT *`).
- **Prevent:** data contracts with source owners (agreed schema, versioning, notice period), contract tests in CI, version endpoints/topics for breaking changes.
- **Operate:** alert on drift with a clear diff, keep schema history, and make backfill easy after a fix.

### How do you make pipelines idempotent and re-runnable?

Idempotent = running the same job for the same input/period twice yields the same result as once (no duplicates, no double-counting).

- **Partition-scoped overwrites:** each run is parameterised by a logical date / batch id and **replaces** its partition (`INSERT OVERWRITE ... PARTITION (dt='2026-10-20')`, Delta `replaceWhere`), instead of appending.
- **Upserts with keys:** `MERGE` on a natural/business key (+ version/timestamp) so replays update rather than insert duplicates. Dedup within the batch first.
- **Deterministic inputs:** use the run's logical date, not `now()`; read a fixed snapshot/time range; deterministic IDs (hash of key fields) instead of random UUIDs.
- **Atomic writes:** write to staging then swap/commit (table formats give atomic commits); no partially written outputs visible on failure.
- **Track progress:** checkpoints/offsets (streaming), processed-file manifests (Auto Loader), high-watermarks stored only after a successful commit.
- **Side effects:** external calls (emails, API posts) use idempotency keys or an outbox table.
- **Test it:** run the same partition twice in CI and assert identical output and counts.

### How do you backfill data?

- **Why:** new pipeline needs history, logic bug fixed, new column added, late source data.
- **How:**
  1. Make sure the pipeline is idempotent and parameterised by date/partition — then a backfill is just "run for a range" (Airflow `airflow dags backfill` / catchup, Dagster partition backfills, dbt `--full-refresh` or vars for a date range).
  2. Scope it: which partitions and which downstream models are affected (use lineage).
  3. Write to a **shadow/staging table** first, validate against the current table (row counts, sums, sample diffs), then swap atomically or overwrite partitions.
  4. Chunk it (per day/month) to control cost and allow resume; limit concurrency so you don't overload source systems or starve production jobs.
  5. Re-run downstream dependents in order; notify consumers that historical numbers will change.
- **Watch for:** reading source data "as of now" when you need "as of then" (use SCD2/snapshots), source retention limits (raw layer saves you), API rate limits, and cost of reprocessing years of data.

### How do you detect and handle bad data (validation, quarantine, alerts)?

- **Validate at boundaries:** on ingest (schema, types, required fields, parseable dates), after transforms (business rules), before publishing (aggregate checks).
- **Types of checks:** schema, nulls on required fields, uniqueness of keys, referential integrity, allowed values/ranges, freshness, volume anomalies (row count vs trailing average), distribution drift, reconciliation totals vs source.
- **Handle by severity:**
  - **Row-level, recoverable:** route bad rows to a **quarantine/dead-letter table** with the error reason, source, and batch id; let good rows flow. Reprocess after fixing.
  - **Batch-level, critical** (0 rows, primary key duplicates, totals off by 30%): **fail the pipeline / circuit-break** and don't publish — stale-but-correct beats fresh-but-wrong. Use write-audit-publish (write to staging, test, then swap).
  - **Warnings:** publish but flag, and track trend.
- **Alert** the owner who can fix it (source team vs pipeline team) with context and a sample, via Slack/PagerDuty; avoid alert fatigue by tuning thresholds.
- **Close the loop:** dashboard of DQ metrics, root-cause with the source owner, add a test for every incident.

### Data quality checks (Great Expectations, dbt tests) — what would you check?

- **Schema:** expected columns and types exist.
- **Completeness:** `not_null` on keys and required fields; % null within tolerance for optional ones.
- **Uniqueness:** primary/business keys unique (`unique`, or `dbt_utils.unique_combination_of_columns`).
- **Validity:** `accepted_values` for enums (status), ranges (`amount >= 0`, ages 0–120), regex formats (email, postcode), dates not in the future.
- **Referential integrity:** `relationships` test — every `order.customer_id` exists in customers.
- **Consistency:** business rules across columns (`end_date >= start_date`, line totals sum to order total).
- **Freshness:** `dbt source freshness` — latest `loaded_at` within SLA.
- **Volume:** row count within expected band vs previous runs; no partition missing.
- **Reconciliation:** counts/sums match the source system.
- **Distribution/anomaly:** mean/percentiles shift, new categories appearing (Great Expectations, Monte Carlo, Soda, Elementary).

```yaml
# dbt schema.yml
models:
  - name: orders
    columns:
      - name: order_id
        tests: [unique, not_null]
      - name: status
        tests:
          - accepted_values: {values: ['pending','paid','shipped','cancelled']}
      - name: customer_id
        tests:
          - relationships: {to: ref('customers'), field: customer_id}
```

- Set severity (`warn` vs `error`) per test; prioritise checks on columns that feed decisions.

### Late-arriving data and watermarks in streaming.

- **Event time vs processing time:** events carry when they happened; they arrive later and out of order (mobile offline, network delays). Aggregations should use event time.
- **Watermark:** the engine's estimate of "event time up to which I've probably seen everything", typically `max_event_time_seen - allowed_lateness`. When the watermark passes the end of a window, the window can be finalised and its state dropped.
- **Trade-off:** larger lateness allowance = more complete results but more state/memory and higher latency; smaller = faster, more dropped data.
- **Handling data later than the watermark:** drop it (and count it), send to a side output / dead-letter for batch correction, or allow updates to already-emitted results (Flink allowed lateness, update output mode).
- Spark Structured Streaming:

```python
from pyspark.sql import functions as F
agg = (events
       .withWatermark("event_time", "10 minutes")
       .groupBy(F.window("event_time", "5 minutes"), "device_id")
       .agg(F.avg("temp").alias("avg_temp")))
```

- Batch equivalent: late-arriving facts — reprocess a rolling lookback window (e.g. last 3 days) on every run; late-arriving dimensions — insert placeholder "unknown" dimension rows and update later.

### Exactly-once vs. at-least-once semantics.

- **At-most-once:** may lose messages, never duplicates (commit offset before processing).
- **At-least-once:** never loses, may duplicate (process then commit offset; a crash between the two causes reprocessing). The common default.
- **Exactly-once:** each message affects the result exactly once. In practice it is "effectively once" — achieved by combining at-least-once delivery with **idempotent or transactional** processing:
  - Kafka idempotent producers + transactions (read-process-write with offsets committed in the same transaction) for Kafka-to-Kafka.
  - Flink / Spark Structured Streaming: checkpointed state + replayable source + idempotent or transactional sink (Delta Lake commits with batch ids, two-phase commit sinks).
  - Idempotent consumers: dedupe on a message id / upsert by key, so duplicates are harmless.
- End-to-end exactly-once only holds if the sink participates; sending emails or calling external APIs needs idempotency keys.
- Trade-off: exactly-once adds latency and complexity; often at-least-once plus idempotent writes is the simplest correct design.

### Change data capture (CDC) — how does it work (Debezium, logical replication)?

- **Idea:** capture row-level inserts, updates and deletes from a database and stream them downstream, instead of repeatedly querying whole tables.
- **Log-based CDC (preferred):** read the database's transaction log — Postgres WAL via **logical replication** (logical decoding with a replication slot and an output plugin like `pgoutput`), MySQL binlog, SQL Server CDC tables, Oracle LogMiner/GoldenGate. Low overhead on the source, captures deletes and every intermediate change, in commit order.
- **Debezium:** Kafka Connect source connectors that read these logs and publish a change event per row to a Kafka topic per table: `before`, `after`, `op` (c/u/d/r for snapshot), source metadata (LSN, txn id, timestamp). It does an initial **snapshot** of existing data then streams changes, storing its log position in offsets so it can resume.
- **Downstream:** apply changes with `MERGE` into the lake/warehouse (latest state) and/or keep the event history (SCD2, audit).
- **Alternatives:** query-based CDC (`WHERE updated_at > last_watermark`) — simple but misses hard deletes and needs reliable `updated_at`; trigger-based — captures everything but adds write load. Managed: AWS DMS, Fivetran, Databricks Lakeflow Connect.
- **Gotchas:** replication slots retain WAL if the consumer stops — disk can fill on the source (monitor slot lag); schema changes; need `REPLICA IDENTITY FULL` for full `before` images on updates/deletes; ordering is per key/partition; toasted columns may be missing in updates.

### Data lineage — why does it matter to customers?

Lineage = the map of where data came from, what transformations it went through, and what depends on it (column-level ideally).

- **Trust and explainability:** "Where does this number on the exec dashboard come from?" Being able to trace a metric to source tables builds trust, especially when numbers disagree.
- **Impact analysis:** before changing or deprecating a table/column, see every downstream model, dashboard and ML model affected.
- **Root-cause analysis:** when a report is wrong, walk upstream to find the broken step quickly.
- **Compliance:** GDPR/CCPA (where does personal data flow, what to delete), HIPAA, SOX/BCBS 239 in banking (regulators require demonstrable data provenance), AI governance (which data trained this model).
- **Cost and cleanup:** find unused tables and pipelines.
- **Tools:** dbt docs/DAG, Unity Catalog, OpenLineage/Marquez, DataHub, Collibra, Foundry's built-in lineage graph.

### Orchestration (Airflow, Dagster, Prefect) — DAGs, retries, SLAs.

- **Orchestrator:** schedules and coordinates tasks with dependencies, retries, alerting, backfills, and gives visibility into runs. It should orchestrate work, not do heavy compute itself (push compute to Spark/warehouse).
- **DAG:** directed acyclic graph of tasks; edges are dependencies. Runs are parameterised by a logical date/data interval, which makes them idempotent and backfillable.
- **Retries:** per-task `retries` with exponential backoff for transient failures (network, rate limits); tasks must be idempotent for retries to be safe. Don't retry deterministic failures (bad data) forever.
- **SLAs / timeouts:** `execution_timeout` per task; SLA or freshness checks that alert when data isn't ready by a deadline; sensors to wait for upstream data (with timeouts and `reschedule` mode to avoid holding worker slots).
- **Tool differences:**
  - **Airflow:** most mature and widespread, task-centric, huge operator ecosystem; managed as MWAA, Cloud Composer, Astronomer.
  - **Dagster:** asset-centric (define the tables you produce), strong typing, testing, built-in lineage and partitioned backfills; good fit with dbt.
  - **Prefect:** Pythonic, dynamic flows, easy local dev and hybrid execution.
  - Also: Databricks Workflows, dbt Cloud jobs, Argo for Kubernetes-native.
- Good practices: DAGs as code in Git with CI, small tasks, no data passed through XCom beyond metadata, alerts to owners, pools/concurrency limits to protect sources.

### dbt — models, tests, incremental models.

- **What:** dbt = transformations-as-code in SQL (+ Jinja) running inside the warehouse; brings software engineering (Git, CI, tests, docs, modularity) to the T in ELT.
- **Models:** each `.sql` file is a `SELECT`; dbt materialises it as `view`, `table`, `incremental`, or `ephemeral`. `ref('model')` builds the dependency DAG; `source()` declares raw inputs. Typical layering: staging (1:1 clean source) -> intermediate -> marts (facts/dims).
- **Tests:** generic (`unique`, `not_null`, `accepted_values`, `relationships`, plus packages like dbt_utils / dbt_expectations), singular tests (a SQL query that returns failing rows), source freshness, and unit tests (dbt 1.8+). `dbt build` runs models and tests in DAG order, stopping downstream on failures.
- **Incremental models:** only process new/changed rows on each run:

```sql
{{ config(materialized='incremental', unique_key='order_id',
          incremental_strategy='merge', on_schema_change='append_new_columns') }}

SELECT order_id, customer_id, status, amount, updated_at
FROM {{ source('shop', 'orders') }}
{% if is_incremental() %}
  -- lookback window to catch late updates
  WHERE updated_at > (SELECT MAX(updated_at) - INTERVAL '3 days' FROM {{ this }})
{% endif %}
```

- Strategies: `append`, `merge` (upsert on `unique_key`), `delete+insert`, `insert_overwrite` (partition replace). Use `--full-refresh` to rebuild. Pitfalls: late data outside the lookback, deletes not propagated, schema changes.
- Also: snapshots (SCD2), seeds (small CSVs), macros, exposures, docs site with lineage.

### Entity resolution: merging the same customer across systems.

**Problem:** "ACME Corp" in Salesforce, "Acme Corporation Ltd" in SAP, and "acme corp." in billing are the same entity; no shared key.

**Approach:**
1. **Standardise:** lowercase, trim, strip punctuation and legal suffixes (Inc, Ltd), normalise addresses (libpostal), phone numbers (E.164), emails; transliteration.
2. **Deterministic matching first:** exact matches on strong identifiers (tax ID, DUNS, email, normalised phone + name). High precision.
3. **Blocking:** avoid O(n^2) comparisons by only comparing candidates sharing a block key (same postcode, first 3 letters of name, phonetic code like Soundex/Metaphone).
4. **Probabilistic / fuzzy matching within blocks:** string similarity (Jaro-Winkler, Levenshtein, token-set ratio), embeddings for names/addresses, weighted into a score — Fellegi-Sunter model (Splink is a good open-source implementation on Spark/DuckDB), or a trained classifier with labelled pairs.
5. **Thresholds:** auto-merge above high threshold, human review queue in the grey zone, non-match below.
6. **Clustering:** matched pairs form a graph; connected components give clusters (watch transitive chains merging unrelated entities).
7. **Golden record:** assign a stable master ID; survivorship rules per attribute (most recent, most trusted source, most complete); keep a crosswalk table `source_system, source_id -> master_id`.
- **Measure:** precision/recall on a labelled sample; false merges are usually worse than missed matches (e.g. merging two patients). Support un-merge and keep history. Tools: Splink, Zingg, Senzing, Tamr, Foundry/MDM tools.

### How do you handle PII in pipelines (masking, tokenisation, access control)?

- **Minimise first:** only ingest PII fields the use case needs; set retention limits.
- **Classify and tag:** detect and tag PII columns (automated scanners + manual review) in the catalog so policies apply by tag.
- **Protect:**
  - **Masking:** dynamic masking at query time based on role (show `***-**-1234`) — Snowflake masking policies, Unity Catalog column masks, Postgres views.
  - **Tokenisation:** replace values with tokens; mapping held in a secure vault; reversible only by authorised services. Keeps format and joinability.
  - **Hashing/pseudonymisation:** keyed hash (HMAC with a secret, not plain SHA of an email, which is brute-forceable) to join across datasets without exposing raw values.
  - **Encryption:** at rest (KMS, customer-managed keys) and in transit (TLS); column-level encryption for highly sensitive fields.
  - **Anonymisation/aggregation** for analytics and sharing (k-anonymity thresholds, differential privacy for published stats).
- **Access control:** least privilege, RBAC/ABAC, row-level and column-level security, separate raw (restricted) zone from curated (masked) zone, break-glass access with approval.
- **Audit and compliance:** audit logs of who accessed what, lineage to support GDPR right-to-erasure (deletes must propagate — table formats with deletes and vacuum), data residency, DPAs. Keep PII out of logs, error messages, test fixtures and LLM prompts.

## E. Big data / Spark (Databricks, Palantir)

### How does Spark work (driver, executors, DAG, lazy evaluation)?

- **Driver:** runs your program (`SparkSession`), builds the logical plan, optimises it (Catalyst), splits it into jobs, stages and tasks, schedules tasks, and collects results.
- **Cluster manager:** allocates resources (YARN, Kubernetes, standalone, Databricks).
- **Executors:** JVM processes on worker nodes that run tasks (one task per partition per stage), cache data in memory, and write shuffle files. Each has N cores = N concurrent tasks.
- **Lazy evaluation:** transformations (`filter`, `select`, `join`) only build a plan; nothing runs until an **action** (`count`, `collect`, `write`). This lets Catalyst optimise the whole query: predicate pushdown, column pruning, constant folding, join reordering and strategy selection.
- **DAG and stages:** the plan becomes a DAG of stages separated by **shuffle boundaries** (wide transformations). Within a stage, narrow transformations are pipelined. Tungsten does whole-stage code generation and off-heap binary memory.
- **AQE (Adaptive Query Execution, on by default in Spark 3.x):** re-optimises at runtime using shuffle statistics — coalesces small partitions, switches to broadcast joins, splits skewed partitions.
- **Fault tolerance:** lineage of transformations lets Spark recompute lost partitions.

### Transformations vs. actions; narrow vs. wide transformations.

- **Transformations** return a new DataFrame/RDD lazily: `select`, `filter`, `withColumn`, `join`, `groupBy().agg()`.
- **Actions** trigger execution and return a result or write output: `count`, `collect`, `show`, `take`, `write`, `foreach`.
- **Narrow:** each output partition depends on one input partition — no data movement: `map`, `filter`, `select`, `withColumn`, `union`, `coalesce` (reducing partitions).
- **Wide:** output partitions depend on many input partitions — requires a **shuffle** across the network: `groupBy`/aggregations, `join` (non-broadcast), `distinct`, `orderBy`, `repartition`, window functions with `partitionBy`.
- Why it matters: wide transformations create stage boundaries and are where most cost, skew and OOM problems happen. Avoid `collect()` on large data — it pulls everything to the driver.

### What causes a shuffle? How to reduce it?

**Causes:** any operation that must co-locate rows with the same key: `groupBy`/`agg`, `join` (sort-merge or shuffle-hash), `distinct`/`dropDuplicates`, `orderBy`/`sort`, `repartition`, window functions with `partitionBy`, set operations like `intersect`.

**Reduce it:**
- **Filter and project early** so less data is shuffled (Catalyst helps, but avoid UDFs that block pushdown).
- **Broadcast joins** for small tables (`F.broadcast(dim)`; auto under `spark.sql.autoBroadcastJoinThreshold`, default 10 MB) — no shuffle of the large side.
- **Pre-aggregate** before joins; `reduceByKey`-style map-side combine happens automatically for DataFrame aggregations.
- **Bucketing** both tables on the join key (same bucket count) so repeated joins skip the shuffle; or store data already partitioned/clustered.
- **Avoid unnecessary `repartition`/`orderBy`;** use `coalesce` to reduce partitions without a full shuffle; use `sortWithinPartitions` if global order isn't needed.
- **Tune `spark.sql.shuffle.partitions`** (default 200) or rely on AQE to coalesce; aim for roughly 100–200 MB per partition.
- Reuse: cache a DataFrame that is shuffled once and used several times.
- Check with `df.explain()` — look for `Exchange` nodes.

### Data skew — how to detect and fix (salting, broadcast joins)?

**Detect:**
- Spark UI: in a stage, one or a few tasks take far longer and read far more shuffle data than the median (look at min/median/max task metrics); job stuck at 199/200 tasks; one executor OOMs or spills.
- Check key distribution: `df.groupBy("key").count().orderBy(F.desc("count")).show()` — typical culprits are NULL keys, default values ("unknown", 0), or one huge customer.

**Fix:**
- **AQE skew join:** `spark.sql.adaptive.skewJoin.enabled=true` (default in Spark 3.x) automatically splits skewed partitions in sort-merge joins.
- **Broadcast join** if the other side is small — skew on the large side no longer matters.
- **Handle NULL/default keys separately:** filter them out and process/union them back, or give them random keys.
- **Salting:** add a random salt to the skewed key on the big side and replicate the small side across all salt values:

```python
from pyspark.sql import functions as F
N = 16
big_s = big.withColumn("salt", (F.rand() * N).cast("int"))
small_s = small.crossJoin(spark.range(N).withColumnRenamed("id", "salt"))
joined = big_s.join(small_s, on=["key", "salt"]).drop("salt")
```

- For aggregations: **two-stage aggregation** — aggregate by `(key, salt)` first, then by `key`.
- Split processing: handle the top few hot keys in a separate path (broadcast), the rest normally.

### Partitioning and bucketing strategies.

- **Partitioning (on disk):** `df.write.partitionBy("event_date")` creates a directory per value; queries filtering on that column skip other directories (partition pruning).
  - Choose low-to-medium cardinality columns commonly used in filters — date is the classic. Avoid high cardinality (user_id) -> millions of tiny files.
  - Target files of ~128 MB–1 GB; partitions should hold at least ~1 GB. Over-partitioning (small files problem) hurts more than under-partitioning.
- **Bucketing:** `bucketBy(64, "customer_id").sortBy(...)` hashes rows into a fixed number of files per partition; tables bucketed on the same key with the same count can join without shuffle. Works only with tables saved to the metastore (`saveAsTable`); less used with Delta.
- **Clustering / Z-ordering / liquid clustering (Delta), Iceberg sort orders:** co-locate related values in files so min/max statistics enable data skipping on high-cardinality columns (user_id, device_id) without partitioning on them. Databricks recommends liquid clustering over partitioning for most new tables.
- **In-memory partitions:** `repartition(n, "key")` (full shuffle, even distribution by key) vs `coalesce(n)` (merge without shuffle, can be uneven). Set partition count so each is ~100–200 MB and there are at least 2–3x as many tasks as cores.
- Also: `OPTIMIZE` to compact small files; Iceberg hidden partitioning (`days(ts)`) avoids users needing to know partition columns.

### Parquet vs. CSV vs. JSON vs. Avro vs. ORC.

| Format | Layout | Schema | Strengths | Weaknesses / use |
|---|---|---|---|---|
| CSV | Row, text | None (header only) | Universal, human-readable | No types, quoting/encoding/delimiter bugs, no compression benefits, slow; use for exchange with humans/legacy |
| JSON | Row, text | Self-describing, nested | Flexible, nested data, APIs | Verbose, slow to parse, types ambiguous; use for raw landing/APIs |
| Avro | Row, binary | Embedded schema, strong schema evolution | Fast writes, compact, great for streaming/Kafka with schema registry | Poor for analytical scans of a few columns |
| Parquet | Columnar, binary | Embedded, nested supported | Column pruning, predicate pushdown via min/max stats, excellent compression (dictionary, RLE); de facto analytics standard; base of Delta/Iceberg | Slow for row-by-row writes/updates; not human-readable |
| ORC | Columnar, binary | Embedded | Similar to Parquet, strong in Hive ecosystem, built-in indexes, ACID in Hive | Less widely supported outside Hadoop |

- Rule: Avro (or Protobuf) for row-oriented messaging/ingestion; Parquet (inside Delta/Iceberg) for analytics storage; CSV/JSON only at the edges.

### Delta Lake / Iceberg / Hudi — what problems do table formats solve (ACID, time travel)?

Plain Parquet files in a lake have problems: no atomic writes (readers see half-written jobs), no updates/deletes/MERGE without rewriting directories, no concurrency control, schema drift, slow file listing, small-file mess.

Open table formats add a **metadata/transaction layer** over Parquet files:
- **ACID transactions:** atomic commits via a transaction log (Delta `_delta_log` JSON + checkpoints; Iceberg metadata files, manifest lists and manifests; Hudi timeline). Optimistic concurrency for concurrent writers.
- **Updates, deletes, MERGE/upserts:** CDC application, GDPR deletes, SCD2 — copy-on-write or merge-on-read (deletion vectors).
- **Time travel:** query previous versions (`SELECT ... VERSION AS OF 42` / `TIMESTAMP AS OF`), reproduce ML training data, audit, roll back bad writes (`RESTORE`).
- **Schema enforcement and evolution:** reject mismatched writes; add/rename columns safely (Iceberg tracks columns by ID).
- **Performance:** file-level statistics for data skipping, no expensive directory listing, compaction (`OPTIMIZE`), clustering, hidden partitioning and partition evolution (Iceberg).
- **Streaming + batch on the same table**, change data feed.
- **Differences:** Delta — Databricks-native, very mature with Spark; Iceberg — broadest engine support (Trino, Snowflake, Flink, Athena, Spark), strong for multi-engine; Hudi — strong for streaming upserts/incremental pulls. Convergence via UniForm / XTable and REST catalogs.
- Maintenance: `VACUUM`/expire snapshots to remove old files (limits time travel).

### Spark OOM errors — how do you debug?

1. **Driver or executor?** Read the error and logs. Driver OOM: `collect()`, `toPandas()`, large broadcast, huge number of tasks/files tracked. Executor OOM: "Container killed by YARN for exceeding memory limits", `java.lang.OutOfMemoryError`, executor lost.
2. **Spark UI:** find the failing stage; check task metrics — skew (one task with huge shuffle read), spill to memory/disk, GC time, input size per task.
3. **Common causes and fixes:**
   - Collecting to driver — write output instead, use `take`/`limit`, or aggregate first; raise `spark.driver.memory` only if needed.
   - Broadcasting a table that is too big — lower/disable `autoBroadcastJoinThreshold`, remove explicit `broadcast()` hint.
   - Too few / too large partitions — increase `spark.sql.shuffle.partitions` or `repartition`, aim for ~100–200 MB per partition; enable AQE.
   - Skew — salting, AQE skew join, handle NULL keys.
   - Exploding data — `explode`, cartesian or many-to-many joins multiplying rows; check join keys.
   - Python UDFs / pandas UDFs — Python worker memory is outside JVM heap; increase `spark.executor.memoryOverhead` or `spark.executor.pyspark.memory`; prefer built-in functions; smaller Arrow batches.
   - Caching too much — unpersist, use `MEMORY_AND_DISK`.
   - Large windows (`partitionBy` on a skewed key with no ordering bound), `collect_list` of huge groups.
4. **Resource tuning last:** executor memory/overhead, fewer cores per executor (less concurrent memory pressure), bigger instance types. Fix the data problem before throwing memory at it.

### PySpark: write a job to aggregate X by Y.

Example: daily revenue, order count and unique customers per country from orders and customers, written as a partitioned Delta table.

```python
from pyspark.sql import SparkSession, functions as F

spark = SparkSession.builder.appName("daily_revenue_by_country").getOrCreate()

orders = (spark.read.parquet("s3://lake/silver/orders/")
          .filter(F.col("status") == "completed")
          .select("order_id", "customer_id", "amount", "order_ts"))
customers = spark.read.parquet("s3://lake/silver/customers/").select("customer_id", "country")

daily = (orders
         .join(F.broadcast(customers), "customer_id", "left")   # customers is small
         .withColumn("order_date", F.to_date("order_ts"))
         .groupBy("order_date", F.coalesce("country", F.lit("UNKNOWN")).alias("country"))
         .agg(F.sum("amount").alias("revenue"),
              F.countDistinct("order_id").alias("orders"),
              F.countDistinct("customer_id").alias("unique_customers"),
              F.avg("amount").alias("avg_order_value")))

(daily.write.format("delta")
      .mode("overwrite")
      .partitionBy("order_date")
      .option("partitionOverwriteMode", "dynamic")   # idempotent: replace only touched dates
      .save("s3://lake/gold/daily_revenue_by_country/"))
```

- Points to mention: filter/select early, broadcast the small dimension, `countDistinct` is expensive (use `approx_count_distinct` at scale), dynamic partition overwrite makes re-runs idempotent, NULL country handled explicitly. Equivalent Spark SQL is fine too.

### Kafka basics: topics, partitions, consumer groups, offsets.

- **Topic:** named, append-only log of messages (e.g. `orders`). Retained for a configured time/size (or compacted to keep the latest value per key), independent of consumption.
- **Partitions:** a topic is split into partitions for parallelism and scale; each is an ordered, immutable log replicated across brokers (leader + followers, `replication.factor`, `min.insync.replicas`). **Ordering is guaranteed only within a partition.** The producer picks the partition by hashing the message key, so all events for a key (e.g. `order_id`) stay ordered.
- **Consumer group:** consumers sharing a `group.id` split the partitions between them — each partition is consumed by exactly one consumer in the group, so max useful parallelism = number of partitions. Different groups each get the full stream independently (fan-out). Rebalances happen when consumers join/leave.
- **Offsets:** position of a message in a partition. Consumers commit offsets (stored in the `__consumer_offsets` topic) to record progress; commit after processing = at-least-once. Can reset to earliest/latest or a timestamp to replay.
- Related: producer `acks=all` + idempotent producer for durability; consumer lag as the key health metric; Schema Registry with Avro/Protobuf; Kafka Connect for sources/sinks; managed options Confluent Cloud, MSK, Event Hubs (Kafka API).

### Spark Structured Streaming vs. Flink.

| | Spark Structured Streaming | Apache Flink |
|---|---|---|
| Model | Micro-batch by default (continuous mode is limited); streams as an unbounded table | True event-at-a-time streaming |
| Latency | ~100 ms to seconds | Milliseconds |
| State | Good (RocksDB state store), checkpoints | Very strong: large keyed state, savepoints, rich timers |
| Event time | Watermarks, windows | Watermarks, windows, allowed lateness, side outputs, CEP library |
| APIs | Same DataFrame/SQL API as batch — one codebase | DataStream API, Table/SQL API |
| Ecosystem | Databricks, Delta Lake, ML; easy for teams already on Spark | Confluent, AWS Managed Flink, Ververica; common at large streaming shops |

- **Choose Spark** when the team already uses Spark/Databricks, latency of seconds is fine, and you want unified batch + streaming code and Delta integration (most analytics/ETL streaming).
- **Choose Flink** for low-latency, complex stateful event processing (fraud, alerting, CEP), very large state, or fine control over time and timers.
- Mention Kafka Streams for simple JVM microservice-embedded processing.

### MLOps on Databricks (MLflow) — tracking, registry, serving.

- **Tracking:** `mlflow.start_run()` logs parameters, metrics, artifacts (plots, model files), code version and environment for each training run; autologging for sklearn/XGBoost/PyTorch etc. Compare runs in the experiment UI to choose the best model; reproducibility via logged data versions (Delta time travel version) and environment.
- **Model Registry (Unity Catalog):** register a model as `catalog.schema.model` with versions; promote using **aliases** (e.g. `@champion`, `@challenger`) rather than the older stage transitions; governance with UC permissions, lineage from model to training tables, approvals and descriptions.
- **Serving:** Databricks Model Serving gives a REST endpoint with autoscaling (including scale-to-zero), GPU, A/B traffic splitting between versions, and inference tables that log requests/responses. Batch inference via `mlflow.pyfunc.spark_udf` on Delta tables. Feature Store / feature engineering in UC provides consistent features for training and online lookup.
- **Around it:** Databricks Workflows or Asset Bundles for CI/CD (train -> evaluate -> register -> deploy with tests), Lakehouse Monitoring for data/prediction drift, retraining triggers. For LLMs: MLflow tracing and evaluation, foundation model endpoints, AI Gateway.

```python
import mlflow
mlflow.set_registry_uri("databricks-uc")
with mlflow.start_run():
    mlflow.log_param("max_depth", 6)
    model.fit(X_train, y_train)
    mlflow.log_metric("auc", auc)
    mlflow.sklearn.log_model(model, "model", registered_model_name="ml.churn.churn_model",
                             input_example=X_train[:5])
```

## F. Pandas / Python data wrangling

### Clean a messy CSV (missing values, bad types, duplicates).

```python
import pandas as pd

df = pd.read_csv("customers.csv", dtype=str, keep_default_na=True,
                 na_values=["", "N/A", "n/a", "NULL", "-", "?"])   # read as strings first

# Normalise column names
df.columns = df.columns.str.strip().str.lower().str.replace(r"\W+", "_", regex=True)

# Trim whitespace in string columns
str_cols = df.select_dtypes("object").columns
df[str_cols] = df[str_cols].apply(lambda s: s.str.strip())

# Fix types; invalid values become NaN/NaT instead of crashing
df["amount"] = pd.to_numeric(df["amount"].str.replace(r"[$,]", "", regex=True), errors="coerce")
df["signup_date"] = pd.to_datetime(df["signup_date"], errors="coerce", format="mixed")  # pandas 2.x
df["email"] = df["email"].str.lower()

# Handle missing values deliberately
bad = df[df["customer_id"].isna() | df["amount"].isna()]          # keep for review
df = df.dropna(subset=["customer_id"])
df["country"] = df["country"].fillna("UNKNOWN")

# Deduplicate: keep most recent per key
df = (df.sort_values("signup_date")
        .drop_duplicates(subset=["customer_id"], keep="last"))

df = df.astype({"customer_id": "string", "country": "category"})
bad.to_csv("rejected_rows.csv", index=False)
```

- Explain choices: read as strings to avoid silent coercion (leading zeros in IDs/zip codes); `errors="coerce"` then count how many failed; don't blindly fill numeric NaN with 0 — impute only if the business agrees; quarantine rejected rows and report counts before vs after each step.

### Merge/join DataFrames and explain duplicates after join.

```python
merged = orders.merge(customers, on="customer_id", how="left",
                      validate="many_to_one",   # raises MergeError if customers has duplicate keys
                      indicator=True)            # _merge column: both / left_only / right_only
print(merged["_merge"].value_counts())
```

- `how`: `inner`, `left`, `right`, `outer`, `cross`; `left_on`/`right_on` for different names; `suffixes=("_x", "_y")` for overlapping columns.
- **Why duplicates appear:** if a key appears m times on the left and n times on the right, the join outputs m x n rows for that key. Usually it means the "lookup" side isn't unique (duplicate customer records, multiple SCD versions, or keys that collide after you normalise case and whitespace).
- **Diagnose:** `customers["customer_id"].duplicated().sum()`, compare `len()` before and after, use `validate=`.
- **Fix:** dedupe the lookup side (`drop_duplicates(subset="customer_id", keep="last")`), aggregate to the right grain before merging, or add the missing key column (e.g. date for point-in-time). Also watch for dtype mismatch (`int` vs `str` keys) causing zero matches, and NaN keys (pandas matches NaN to NaN in merges, unlike SQL).

### Group by and aggregate; pivot tables.

```python
# Named aggregation
summary = (df.groupby(["region", "product"], as_index=False)
             .agg(revenue=("amount", "sum"),
                  orders=("order_id", "nunique"),
                  avg_order=("amount", "mean")))

# Per-group transform (keeps row count) - like a SQL window function
df["pct_of_region"] = df["amount"] / df.groupby("region")["amount"].transform("sum")

# Top 3 per group
top3 = df.sort_values("amount", ascending=False).groupby("region").head(3)

# Pivot table: rows = region, columns = month, values = revenue
pt = pd.pivot_table(df, index="region", columns="month", values="amount",
                    aggfunc="sum", fill_value=0, margins=True)

# Unpivot (wide -> long)
long = pt.reset_index().melt(id_vars="region", var_name="month", value_name="revenue")
```

- `pivot` requires unique index/column pairs (errors on duplicates); `pivot_table` aggregates them. `groupby` drops NaN keys by default — use `dropna=False` to keep them. `agg` vs `transform` vs `apply`: aggregate reduces, transform keeps shape, apply is flexible but slow.

### Process a file too large for memory (chunking, generators, DuckDB, Polars).

```python
# 1. pandas chunking: stream and aggregate incrementally
import pandas as pd
totals = None
for chunk in pd.read_csv("events.csv", chunksize=1_000_000,
                         usecols=["country", "amount"], dtype={"country": "category"}):
    part = chunk.groupby("country", observed=True)["amount"].sum()
    totals = part if totals is None else totals.add(part, fill_value=0)

# 2. DuckDB: SQL directly on files, out-of-core, multi-threaded
import duckdb
result = duckdb.sql("""
    SELECT country, SUM(amount) AS revenue
    FROM read_csv_auto('events.csv')
    GROUP BY country
""").df()

# 3. Polars lazy + streaming
import polars as pl
result = (pl.scan_csv("events.csv")
            .group_by("country").agg(pl.col("amount").sum())
            .collect(streaming=True))   # newer Polars: .collect(engine="streaming")

# 4. Plain generator for line-by-line processing
def read_records(path):
    with open(path) as f:
        for line in f:
            yield line.rstrip("\n").split(",")
```

- Other levers: read only needed columns (`usecols`), smaller dtypes (`category`, `int32`, `float32`), convert once to Parquet (columnar, compressed, readable column-by-column) and query that, filter early. Beyond one machine: Spark/Dask. My default for a single large file today is DuckDB or Polars — fast, low memory, and easy to explain to a customer.
- Watch out: aggregates like mean or distinct count can't be naively combined across chunks — keep sums and counts (or sets/HyperLogLog) and combine at the end.

### Parse nested JSON into a flat table.

```python
import json
import pandas as pd

data = json.loads("""[
  {"order_id": 1, "customer": {"id": 10, "name": "Ana", "address": {"city": "Lisbon"}},
   "items": [{"sku": "A", "qty": 2}, {"sku": "B", "qty": 1}]},
  {"order_id": 2, "customer": {"id": 11, "name": "Ben"}, "items": []}
]""")

# Nested objects -> dotted columns (one row per order)
orders = pd.json_normalize(data, sep="_")
# columns: order_id, customer_id, customer_name, customer_address_city, items

# Arrays -> one row per element, with parent fields carried along
items = pd.json_normalize(data, record_path="items",
                          meta=["order_id", ["customer", "id"]], sep="_")
#   sku  qty  order_id  customer_id
#   A    2    1         10
#   B    1    1         10
```

- Missing keys become NaN (customer 11 has no address); empty arrays produce no rows in the exploded table, so join back to `orders` with a left join if you need them.
- Alternative: `df.explode("items")` then `pd.json_normalize(df["items"])`. For big or deeply nested data: DuckDB (`read_json_auto` + `unnest`), Spark (`from_json`, `explode`, `col("a.b")`), or Postgres `jsonb_to_recordset` / `jsonb_array_elements`.
- Clarify the target grain (one row per order or per item) before flattening; flattening two independent arrays in the same record creates a cross product.
