---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[data-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Pipeline Architecture & Data Movement Patterns

This reference covers the fundamental patterns, trade-offs, and best practices for designing data pipelines—how data moves from source systems into your warehouse or lake, and how it transforms for analytics.

## Pipeline Architecture Patterns

### Batch vs. Streaming vs. Micro-Batch

**Batch Processing** (daily, hourly, or on-demand jobs)
- Simplest to reason about. One transaction, clear start/end.
- Latency: hours to days depending on schedule.
- Cost-efficient: compute spins up, processes, spins down.
- Use case: Standard reporting, historical aggregations, non-urgent analytics.
- Tools: Airflow DAGs, dbt jobs, Spark batch jobs.

**Streaming** (continuous event flow)
- Sub-second to minute-level latency.
- More complex: must handle out-of-order events, state management, windowing.
- Higher operational overhead and cost.
- Use case: Real-time dashboards, fraud detection, live personalization.
- Tools: Kafka, Kinesis, Pub/Sub feeding stream processors (Flink, Spark Structured Streaming).

**Micro-Batch** (small batch jobs at frequent intervals—every 5-15 min)
- Sweet spot for many use cases: lower latency than hourly batch, simpler than continuous streaming.
- Reduces operational complexity while hitting near-real-time requirements.
- Use case: Frequent analytics updates without extreme latency sensitivity.
- Tools: Airflow with frequent schedules, dbt Cloud jobs, Databricks Jobs.

### ETL vs. ELT

**ETL** (Extract → Transform → Load)
- Transform in middleware (e.g., Python scripts, Talend, Informatica).
- Advantages: reduces warehouse compute; can clean/validate before load.
- Disadvantages: black box transformations; hard to debug; tight coupling to source systems.
- When to use: limited warehouse compute; source systems unstable or high-latency.

**ELT** (Extract → Load → Transform)
- Load raw data first, transform in the warehouse using SQL/dbt.
- Advantages: raw data stays immutable; transformations are versionable and testable; warehouse is the source of truth.
- Disadvantages: more warehouse compute; raw data must be stored.
- When to use: default; warehouse has sufficient compute; you value auditability and agility.
- Modern best practice: almost always ELT.

## Orchestration Patterns

### Airflow DAGs (Apache Airflow)

Directed acyclic graph of tasks with explicit dependencies.

```
BashOperator    →  PythonOperator  →  SQLExecuteQueryOperator  →  Sensor
 (extract)         (validate)           (transform)                (wait_for_bq_table)
```

- **Pros**: Mature, scalable, clear task dependencies, rich operators.
- **Cons**: YAML-based DAG definition can be verbose; task outputs via XComs are awkward.
- **Best for**: Complex multi-step pipelines with conditional logic and error handling.

### Dagster Assets

Asset-oriented DAGs: define what data assets exist and their dependencies, not tasks.

```python
@asset
def raw_orders(database: DatabaseResource) -> duckdb.Relation:
    return database.query("SELECT * FROM orders")

@asset
def order_summary(raw_orders: duckdb.Relation) -> duckdb.Relation:
    return raw_orders.aggregate(...)
```

- **Pros**: Cleaner mental model; automatic lineage; excellent for analytics workflows.
- **Cons**: Newer; smaller ecosystem than Airflow.
- **Best for**: Dbt-first teams; modern data stacks valuing lineage.

### Prefect Flows

Flow-based orchestration with @task and @flow decorators.

```python
@task
def extract_data():
    return get_data_from_api()

@task
def transform(data):
    return clean(data)

@flow
def data_pipeline():
    raw = extract_data()
    result = transform(raw)
```

- **Pros**: Python-native, dynamic flows, excellent debugging.
- **Cons**: Smaller community than Airflow.
- **Best for**: Python-heavy pipelines; dynamic task generation.

### dbt Cloud / dbt Scheduler

dbt models are jobs too—schedule them in dbt Cloud with built-in testing and lineage.

- **Best for**: SQL-first analytics; tightly integrated with modeling.

## Idempotency & Exactly-Once Processing

### Idempotency

A job is idempotent if running it multiple times yields the same result as running it once.

**Why it matters**: Pipelines fail. Retries happen. Without idempotency, retrying a failed job can duplicate data.

**How to achieve it**:
- **Merge/upsert logic**: INSERT OR REPLACE with primary key.
- **Partition-based reloads**: Reprocess a day or hour of data, replacing what was there.
- **Deduplication**: Apply dedup logic downstream (e.g., in transformation layer).
- **Unique constraints**: Database constraints prevent accidental duplicates.

Example (SQL):
```sql
MERGE INTO orders o
USING new_orders n
ON o.order_id = n.order_id
WHEN MATCHED THEN UPDATE SET o.amount = n.amount
WHEN NOT MATCHED THEN INSERT VALUES (n.order_id, n.amount);
```

### Exactly-Once Semantics

In streaming systems, define what "exactly once" means:
- **Exactly-once delivery**: message reaches destination once (hard; requires deduplication).
- **Exactly-once processing**: message is processed once, state updates are durable (achievable with checkpoints).

**In Kafka → data warehouse**: Use partition keys, idempotent writes, and upsert logic to guarantee exactly-once results.

## Schema Evolution

Data sources change: new columns, renamed fields, type changes. Pipelines must adapt.

### Handling Schema Changes

1. **Additive Changes** (new fields)
   - Safe: backward compatible.
   - Warehouse: add new columns automatically or explicitly.

2. **Breaking Changes** (deleted/renamed fields)
   - Risky: can break downstream consumption.
   - Best practice: deprecate old fields, signal consumers, wait before removal.

3. **Type Changes**
   - Avoid if possible: can cause casting errors.
   - If necessary: create new column, migrate, deprecate old column.

### Tools for Schema Management

- **Avro/Protobuf**: Schemas as code; version schemas explicitly.
- **dbt**: Versioning via git; breaking changes are explicit in code.
- **Data contracts**: Formal schema + SLA agreements between producers and consumers.

## Change Data Capture (CDC)

### Why CDC?

Instead of full table scans, capture only what changed: inserts, updates, deletes.

**Benefits**: Reduced data transfer, faster refreshes, enables real-time replication.

### CDC Patterns

**1. Debezium** (open-source)
- Reads database transaction logs (PostgreSQL WAL, MySQL binlog, Oracle redo logs).
- Publishes changes to Kafka.
- Consumes in downstream systems.

**2. Native DB Logs**
- Snowflake: STREAM objects (change tracking).
- BigQuery: BigQuery Change Data Capture.
- Redshift: no native CDC; use Debezium.

**3. Timestamp-based Incremental**
```sql
SELECT * FROM users WHERE updated_at > last_run_timestamp
ORDER BY updated_at;
```
- Simple but misses deletes; requires updated_at column.

**4. API Polling**
- Pull changes from source API at intervals.
- High latency; easy to implement.

**Trade-off**: Debezium is complex but most reliable for high-volume transactional systems.

## Data Lake vs. Lakehouse Architecture

### Data Lake

Raw data in object storage (S3, GCS, ADLS) organized by date/source.

```
s3://data-lake/
  ├── salesforce/2024/04/03/
  ├── segment/events/2024/04/03/
  └── internal-db/2024/04/03/
```

**Pros**: Cheap storage; flexible schema; good for exploratory analysis.
**Cons**: No transactions; hard to enforce quality; query engines required.

### Lakehouse

Data lake + transaction layer (Delta, Iceberg, Hudi). SQL-queryable, ACID-compliant.

```sql
SELECT * FROM delta.`s3://data-lake/sales/` WHERE date = '2024-04-03';
```

**Pros**: ACID guarantees; schema enforcement; versioning.
**Cons**: More operational overhead than data lake; more expensive than warehouse.

**When to choose lakehouse**: You have petabyte-scale data and need both flexibility and reliability.

## Medallion Architecture (Bronze → Silver → Gold)

Industry-standard three-layer data lake/lakehouse design:

### Bronze (Raw)

- All data, as-is from sources.
- Minimal transformation: just load.
- Not for end users; for debugging and auditing.
- Retention: 30-90 days (cost savings).

### Silver (Conformed)

- Cleaned, deduplicated, integrated across sources.
- Joins across source systems; applies business logic.
- Standard schema; quality checks.
- Consumer: analysts, most BI tools.

### Gold (Business)

- Aggregated, mart-like tables (revenue by region, daily active users).
- Optimized for specific use cases.
- Often replicated to OLTP systems (application databases).
- Consumer: dashboards, applications.

**Dbt mapping**:
- Bronze: `sources` (raw load).
- Silver: `stg_*` (staging) + `int_*` (intermediate).
- Gold: `fct_*` (facts) + `dim_*` (dimensions) + `rpt_*` (reports).

## File Format Selection

### Parquet

- Columnar format; excellent compression; schema stored.
- Best for: analytical queries, cost-efficient storage.
- **Hive partitioning**: Partition by date/region, each partition a separate file.

### Delta Lake

- Parquet + transaction log; ACID, time travel, schema enforcement.
- Best for: production lakes, version control, time travel queries.

### Iceberg

- Columnar + hidden partitioning + schema versioning.
- Best for: large-scale tables, schema evolution, partition pruning.

### Hudi

- Incremental processing; record-level updates.
- Best for: streaming CDC, record-level mutations.

### JSON / Newline Delimited JSON (NDJSON)

- Human-readable; flexible schema; larger file size.
- Best for: APIs, streaming events.

**Rule of thumb**: Use Parquet for production batch lakes; Delta/Iceberg for medallion architecture.

## Cost Optimization for Data Platforms

**Storage**: Partitioning by date reduces scan cost. Delete old data. Use compression.

**Compute**: Incremental loads beat full refreshes. Query pushdown (filters early). Clustering on join keys.

**Architecture**: Batch beats streaming for cost (but trades latency). Lakehouse beats warehouse for scale (but trades operational complexity).

**Monitoring**: Track cost per pipeline; find outliers. Automate cleanup of temporary tables.

