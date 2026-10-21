# 07 — Data Engineering & SQL Questions

FDEs constantly connect to messy customer data. Databricks, Palantir and Snowflake-style loops often include a SQL screen.

---

## A. SQL coding prompts
- [ ] Find the second-highest (Nth-highest) salary.
- [ ] Find duplicate rows / emails.
- [ ] Top N per group (top 3 products per category) — window functions.
- [ ] Running total / cumulative sum.
- [ ] Moving 7-day average.
- [ ] Month-over-month growth rate.
- [ ] Day-1 / day-7 retention; cohort analysis.
- [ ] Users who did X but not Y (anti-join).
- [ ] Consecutive days logged in (gaps and islands).
- [ ] Sessionise events (new session after 30 minutes of inactivity).
- [ ] Funnel conversion between steps.
- [ ] Median without a MEDIAN function.
- [ ] Pivot / unpivot a table.
- [ ] Find customers whose spend increased every month.
- [ ] Employees earning more than their managers (self-join).
- [ ] Deduplicate keeping the latest record per key.
- [ ] Slowly changing dimension: point-in-time lookup.
- [ ] Recursive CTE: org chart / bill of materials hierarchy.
- [ ] Join three tables and explain the result row count.
- [ ] Find overlapping date ranges / bookings.

## B. SQL concepts
- [ ] INNER vs. LEFT vs. RIGHT vs. FULL OUTER vs. CROSS join.
- [ ] WHERE vs. HAVING.
- [ ] GROUP BY vs. window functions (PARTITION BY).
- [ ] ROW_NUMBER vs. RANK vs. DENSE_RANK.
- [ ] UNION vs. UNION ALL.
- [ ] How do NULLs behave in comparisons, joins and aggregates?
- [ ] CTEs vs. subqueries vs. temp tables.
- [ ] What are indexes? B-tree vs. hash vs. bitmap. When does an index hurt?
- [ ] How do you read an EXPLAIN plan? Optimise a slow query.
- [ ] Normalisation (1NF, 2NF, 3NF) and when to denormalise.
- [ ] Transactions, ACID, isolation levels (read committed, repeatable read, serializable).
- [ ] Deadlocks — what and how to avoid?
- [ ] Primary vs. foreign vs. composite vs. surrogate keys.
- [ ] Views vs. materialised views.

## C. Data modeling
- [ ] Design a schema for an e-commerce store / hospital / airline / ride-sharing app.
- [ ] Star schema vs. snowflake schema; facts vs. dimensions.
- [ ] Slowly changing dimensions (Type 1, 2, 3).
- [ ] OLTP vs. OLAP.
- [ ] Data lake vs. data warehouse vs. lakehouse.
- [ ] Ontology / semantic layer: what is it, why would a customer need one? (Palantir Foundry)
- [ ] How do you model time-series / sensor data?
- [ ] How do you model graph/relationship data? When use a graph DB?
- [ ] How do you design for multi-tenancy in a schema?

## D. Pipelines & data quality
- [ ] ETL vs. ELT.
- [ ] Batch vs. streaming. When would you need streaming?
- [ ] Design a pipeline to ingest data from 50 customer source systems.
- [ ] How do you handle schema drift from upstream sources?
- [ ] How do you make pipelines idempotent and re-runnable?
- [ ] How do you backfill data?
- [ ] How do you detect and handle bad data (validation, quarantine, alerts)?
- [ ] Data quality checks (Great Expectations, dbt tests) — what would you check?
- [ ] Late-arriving data and watermarks in streaming.
- [ ] Exactly-once vs. at-least-once semantics.
- [ ] Change data capture (CDC) — how does it work (Debezium, logical replication)?
- [ ] Data lineage — why does it matter to customers?
- [ ] Orchestration (Airflow, Dagster, Prefect) — DAGs, retries, SLAs.
- [ ] dbt — models, tests, incremental models.
- [ ] Entity resolution: merging the same customer across systems.
- [ ] How do you handle PII in pipelines (masking, tokenisation, access control)?

## E. Big data / Spark (Databricks, Palantir)
- [ ] How does Spark work (driver, executors, DAG, lazy evaluation)?
- [ ] Transformations vs. actions; narrow vs. wide transformations.
- [ ] What causes a shuffle? How to reduce it?
- [ ] Data skew — how to detect and fix (salting, broadcast joins)?
- [ ] Partitioning and bucketing strategies.
- [ ] Parquet vs. CSV vs. JSON vs. Avro vs. ORC.
- [ ] Delta Lake / Iceberg / Hudi — what problems do table formats solve (ACID, time travel)?
- [ ] Spark OOM errors — how do you debug?
- [ ] PySpark: write a job to aggregate X by Y.
- [ ] Kafka basics: topics, partitions, consumer groups, offsets.
- [ ] Spark Structured Streaming vs. Flink.
- [ ] MLOps on Databricks (MLflow) — tracking, registry, serving.

## F. Pandas / Python data wrangling
- [ ] Clean a messy CSV (missing values, bad types, duplicates).
- [ ] Merge/join DataFrames and explain duplicates after join.
- [ ] Group by and aggregate; pivot tables.
- [ ] Process a file too large for memory (chunking, generators, DuckDB, Polars).
- [ ] Parse nested JSON into a flat table.
