---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[data-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Data Modeling, Quality & Governance

This reference covers schema design, data quality frameworks, and governance patterns—the foundations of trustworthy, usable analytics.

## Dimensional Modeling (Kimball Methodology)

The standard approach for designing analytical data warehouses.

### Core Concepts

**Fact Tables**: Measures (revenue, clicks, conversions). One row per event or transaction. Many rows, few columns.

```sql
CREATE TABLE fct_orders (
  order_id INT,
  date_id INT,        -- FK to dim_date
  customer_id INT,    -- FK to dim_customer
  product_id INT,     -- FK to dim_product
  amount DECIMAL,
  quantity INT
);
```

**Dimension Tables**: Attributes (customer name, product category, date). Few rows, many columns. Changes slowly.

```sql
CREATE TABLE dim_customer (
  customer_id INT PRIMARY KEY,
  name VARCHAR,
  segment VARCHAR,
  country VARCHAR,
  created_date DATE
);
```

**Grain**: The level of detail in a fact table. E.g., one row per order, or one row per order line item. Define clearly before modeling.

**Conformed Dimensions**: Same dimension used across multiple fact tables. E.g., dim_date shared by all time-based facts.

### Star Schema vs. Snowflake Schema

**Star Schema**
- Fact table at center; dimensions radiate out.
- Dimensions are denormalized (all attributes in one table).
- **Pros**: Simple joins; fast queries; analyst-friendly.
- **Cons**: more storage (repeated data); harder to maintain.

```
        dim_date
           |
dim_customer - fct_orders - dim_product
```

**Snowflake Schema**
- Dimensions normalized further (e.g., product_category is separate table).
- **Pros**: less storage; easier to maintain attributes.
- **Cons**: more joins; slightly slower; analysts must understand structure.

**Recommendation**: Start with star schema. Normalize only if storage is a hard constraint (rarely in modern warehouses).

### Slowly Changing Dimensions (SCD)

Dimension attributes change over time. How do you track history?

**SCD Type 0**: Don't track changes (ignore history).
**SCD Type 1**: Overwrite old value. Lose history. Rarely appropriate.

```sql
-- Before update: Acme Inc has segment "Fortune 500"
-- After update: Acme Inc now has segment "Enterprise"
UPDATE dim_customer SET segment = 'Enterprise' WHERE customer_id = 1;
```

**SCD Type 2**: Add new row with effective dates. Track full history.

```sql
INSERT INTO dim_customer VALUES (1, 'Acme', 'Fortune 500', '2020-01-01', '2024-03-31');
INSERT INTO dim_customer VALUES (1, 'Acme', 'Enterprise', '2024-04-01', '9999-12-31');
```

**SCD Type 3**: Add "previous value" column. Track one level of history.

```sql
ALTER TABLE dim_customer ADD COLUMN previous_segment VARCHAR;
UPDATE dim_customer SET previous_segment = 'Fortune 500', segment = 'Enterprise' WHERE customer_id = 1;
```

**Recommendation**: Use Type 2 for important attributes. Denormalize join keys (effective dates) into fact tables for easy filtering.

## Data Vault 2.0

Alternative to dimensional modeling; more flexible for evolving schemas.

### Structure

**Hubs**: Core business entities (customer, product, order). One row per unique entity.

```sql
CREATE TABLE hub_customer (
  customer_key INT PRIMARY KEY,
  customer_id INT,        -- Business key
  load_date TIMESTAMP,
  record_source VARCHAR
);
```

**Links**: Relationships between hubs.

```sql
CREATE TABLE link_order_customer (
  link_key INT PRIMARY KEY,
  order_key INT,          -- FK to hub_order
  customer_key INT,       -- FK to hub_customer
  load_date TIMESTAMP,
  record_source VARCHAR
);
```

**Satellites**: Attributes and history. One satellite per logical grouping.

```sql
CREATE TABLE sat_customer_details (
  customer_key INT,       -- FK to hub_customer
  load_date TIMESTAMP,
  end_date TIMESTAMP,     -- For historization
  name VARCHAR,
  email VARCHAR,
  segment VARCHAR
);
```

**When to use**: Highly variable schemas, complex CDC, multi-source integration. Steeper learning curve than dimensional modeling.

## dbt Project Structure & Best Practices

### Standard Structure

```
dbt_project.yml              -- Project config
models/
  ├── staging/              -- Raw data, minimal transformation
  │   ├── stg_customers.sql
  │   └── stg_orders.sql
  ├── intermediate/         -- Business logic, not end-user facing
  │   ├── int_orders_with_customers.sql
  │   └── int_revenue_by_day.sql
  ├── marts/                -- End-user tables
  │   ├── fct_orders.sql
  │   ├── dim_customers.sql
  │   └── dim_products.sql
  └── exposures.yml         -- Dashboards, BI tools consuming these models
tests/
  ├── generic/              -- Reusable tests
  └── singular/             -- Custom SQL tests
macros/                      -- Reusable SQL snippets
  ├── generate_surrogate_key.sql
  └── match_order_grain.sql
```

### Model Config

```yaml
{{
  config(
    materialized='table',      -- or view, incremental, dynamic_table
    schema='marts',
    alias='fct_orders_v2',
    unique_key='order_id',
    indexes=[{'columns': ['date_id']}],  -- Warehouse-specific
    post_hook='GRANT SELECT ON {{ this }} TO analyst_role'
  )
}}
```

**materialized**:
- **view**: For small transforms or rarely-queried data.
- **table**: For stable, frequently-queried models.
- **incremental**: For fact tables; only process new/changed rows.
- **dynamic_table**: Snowflake; auto-refreshing materialized views.

**incremental**:

```sql
{{
  config(
    materialized='incremental',
    unique_key='order_id',
    on_schema_change='fail'
  )
}}

SELECT * FROM {{ source('raw', 'orders') }}
{% if execute %}
  {% if var('is_incremental') %}
    WHERE created_at > (SELECT MAX(created_at) FROM {{ this }})
  {% endif %}
{% endif %}
```

### Naming Conventions

- **stg_***: Staging; minimal business logic; 1:1 with source.
- **int_***: Intermediate; complex logic; not exposed.
- **fct_***: Fact table; measures; historized grain.
- **dim_***: Dimension table; attributes; SCD Type 2.
- **rpt_***: Report-ready; aggregated; specific use case.

### Testing

```yaml
# models/fct_orders.yml
- name: fct_orders
  columns:
    - name: order_id
      tests:
        - unique
        - not_null
    - name: customer_id
      tests:
        - relationships:
            to: ref('dim_customers')
            field: customer_id
```

- **unique**: No duplicates.
- **not_null**: No nulls.
- **accepted_values**: Only specific values allowed.
- **relationships**: Foreign key constraint.

### Documentation & Lineage

```yaml
- name: fct_orders
  description: >
    One row per order. Grain: order level.
    Updated daily at 2 AM UTC.
  tests:
    - dbt_utils.recency:
        datepart: day
        field: created_date
        interval: 1
```

dbt generates lineage automatically. Expose via `exposures.yml` to link dashboards and BI tools.

## Data Quality Frameworks

### The Four Dimensions of Quality

1. **Accuracy**: Data matches reality. Order total = sum of line items.
2. **Completeness**: No missing values where not allowed. Every order has a customer_id.
3. **Timeliness**: Data is fresh. Daily refresh completes by 8 AM.
4. **Consistency**: Data is uniform across systems. Customer in CRM matches customer in warehouse.

### dbt Tests

Built-in, zero-config tests for structural quality:

```yaml
tests:
  - not_null
  - unique
  - accepted_values: { values: [active, inactive] }
  - relationships:
      to: ref('dim_customers')
      field: customer_id
```

Catches schema issues: nulls in PKs, duplicates, broken FKs.

### Great Expectations

Declarative data validation; profiles and custom expectations.

```python
validator = context.get_validator(batch_request=batch_request)
validator.expect_column_values_to_be_in_set(
    column='status',
    value_set=['active', 'inactive']
)
validator.expect_table_row_count_to_be_between(min_value=100000, max_value=2000000)
validator.save_expectation_suite()
```

- **Profile**: Auto-detect expected ranges, cardinality, types.
- **Validate on load**: Catch bad data before it reaches analysts.

### Elementary

Data observability; runs alongside dbt. Detects anomalies, tracks lineage.

```yaml
elementary:
  anomaly_detection:
    - metric: row_count
      where_clause: "created_date = cast(current_date as date)"
```

Alerts on:
- Unexpected row count spike / drop.
- Column freshness (no updates in 24h).
- Schema changes.

### Monte Carlo

SaaS data observability. Lineage, incident detection, root cause analysis.

Best for large enterprises; tracks data downtime across the stack.

### dbt Assertions

New in dbt 1.8+; threshold-based health checks:

```yaml
- name: fct_orders
  assertions:
    - assert_non_negative: amount
    - assert_row_count: [{ min: 100000, max: 2000000 }]
```

## Data Contracts & SLAs

### Contract Definition

A data contract is a formal agreement between data producer and consumer.

```yaml
contract:
  name: orders-events
  producer: analytics-team
  consumer: ml-platform, bi-dashboards
  columns:
    - name: order_id
      type: int
      required: true
    - name: amount
      type: decimal
      required: true
    - name: created_at
      type: timestamp
      required: true
  sla:
    freshness: daily by 2 AM UTC
    availability: 99.9%
    null_threshold: 0%
```

### SLA Definition

- **Freshness**: Data updated by X time daily / hourly.
- **Availability**: Pipeline success rate (99.9% = max 9 minutes downtime/week).
- **Completeness**: Row count within X% of expected.
- **Latency**: Max time from source event to warehouse.

## Data Governance Basics

### Ownership & Stewardship

- **Data owner**: Executive sponsor; owns data quality and access policies.
- **Steward**: Technical owner; maintains DDL, definitions, SLAs.
- **Consumer**: Uses data for analytics / applications.

Assign stewards to high-value datasets. Clear ownership prevents orphaned tables.

### Sensitive Data & PII Handling

- **PII**: Personally identifiable information (name, email, SSN, location).
- **Sensitive**: Payment cards, health data, government IDs.

**Pattern: Store encrypted, decrypt on read**
```sql
SELECT
  customer_id,
  decrypt(aes_decrypt_binary(email, key)) AS email
FROM customers
WHERE user_role = 'analyst';
```

**Pattern: Data masking**
```sql
SELECT
  customer_id,
  CASE WHEN current_user() = 'analyst_role'
    THEN CONCAT(LEFT(email, 2), '***')
    ELSE email
  END AS email
FROM customers;
```

### Data Catalog

Document what data exists, who owns it, where it comes from.

**Minimum fields**:
- Table name, description, grain.
- Owner, steward contact.
- Source system, update frequency.
- PII presence, sensitivity level.
- Downstream consumers.

**Tools**: Collibra (enterprise), Alation (SaaS), DataHub (open-source).

## Testing Strategies for Data Pipelines

### Unit Tests (Transformation Logic)

Test dbt model logic in isolation.

```yaml
- name: int_customer_orders
  tests:
    - dbt_expectations.expect_grouped_row_count_to_equal:
        group_by: [customer_id]
        expected_min_group_size: 1
        expected_max_group_size: 1000000
```

### Integration Tests

Test the full pipeline: source → warehouse → consumption.

```python
# Python test using pytest
def test_order_total_equals_sum_of_items():
    result = run_query("""
      SELECT COUNT(*) FROM fct_orders
      WHERE amount != (SELECT SUM(line_amount) FROM fct_order_items ...)
    """)
    assert result[0][0] == 0  # No mismatches
```

### Regression Tests

Ensure historical data isn't recomputed incorrectly.

```sql
-- Validate that gold tables match yesterday's snapshot
SELECT COUNT(*) mismatch_count
FROM gold.fct_orders gold
FULL OUTER JOIN snapshots.fct_orders_2024_04_02 snap
  USING (order_id)
WHERE gold.amount != snap.amount OR snap.order_id IS NULL;
```

### Observability in Pipelines

- Log key metrics: row counts, min/max values, processing time.
- Alert on threshold breaches.
- Track lineage so failures propagate clearly downstream.

