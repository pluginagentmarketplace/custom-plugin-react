---
name: data-engineering
description: Design and build data pipelines, data warehouses, ETL processes, and big data systems. Work with SQL, Spark, and modern data tools. Use when building data infrastructure or optimizing data workflows.
---

# Data Engineering

## Quick Start

### SQL data pipeline:

```sql
-- Create staging table
CREATE TABLE users_staging (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255),
    name VARCHAR(255),
    created_at TIMESTAMP
);

-- Transform and load
INSERT INTO users_prod
SELECT
    id,
    LOWER(TRIM(email)) as email,
    UPPER(name) as name,
    created_at,
    CURRENT_TIMESTAMP as loaded_at
FROM users_staging
WHERE email IS NOT NULL
    AND created_at > CURRENT_DATE - INTERVAL '1 day'
ON CONFLICT (email) DO UPDATE SET
    updated_at = CURRENT_TIMESTAMP;

-- Clean up
TRUNCATE TABLE users_staging;

-- Add index for performance
CREATE INDEX IF NOT EXISTS idx_users_email ON users_prod(email);
```

### Apache Spark with Python:

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, sum, count, when

spark = SparkSession.builder.appName("DataPipeline").getOrCreate()

# Read data
df = spark.read.csv("sales_data.csv", header=True, inferSchema=True)

# Transform
sales_summary = df.filter(col("status") == "completed") \
    .groupBy("region", "product") \
    .agg(
        sum("amount").alias("total_sales"),
        count("*").alias("transaction_count"),
        sum(when(col("profit") > 0, 1).otherwise(0)).alias("profitable_transactions")
    ) \
    .filter(col("total_sales") > 1000) \
    .orderBy(col("total_sales").desc())

# Write results
sales_summary.write.mode("overwrite").parquet("output/sales_summary")
```

### dbt data transformation:

```sql
-- models/staging/stg_users.sql
{{ config(materialized='table') }}

SELECT
    id,
    email,
    name,
    created_at,
    CURRENT_TIMESTAMP as dbt_loaded_at
FROM {{ source('raw', 'users') }}
WHERE created_at IS NOT NULL
```

### Data validation with Great Expectations:

```python
import great_expectations as ge

# Load data
df = ge.read_csv("data.csv")

# Define expectations
df.expect_column_to_exist("email")
df.expect_column_values_to_not_be_null("email")
df.expect_column_values_to_match_regex("email", r".*@.*\.com")
df.expect_column_values_to_be_between("age", 0, 150)

# Validate
results = df.validate()
print(results)
```

## Core Concepts

**Data Pipeline Architecture**
- Source → Staging → Warehouse → Analytics
- Batch processing vs stream processing
- Data lineage and dependencies
- Error handling and retries
- Scheduling and orchestration

**Database Optimization**
- Indexing strategies and cardinality
- Query optimization and execution plans
- Partitioning and sharding
- Denormalization for performance
- Data type selection

**ETL/ELT Patterns**
- Extract: APIs, databases, logs, files
- Transform: Cleaning, validation, aggregation
- Load: Batch or incremental loading
- Idempotency and deduplication
- SCD (Slowly Changing Dimensions)

**Big Data Processing**
- Apache Spark architecture (Driver, Executors)
- Distributed processing and partitioning
- RDDs, DataFrames, Datasets
- Wide vs narrow transformations
- Caching and optimization

**Data Quality**
- Validation rules and constraints
- Anomaly detection
- Data profiling
- Monitoring and alerting
- SLAs for data

## Best Practices

1. **Pipeline Design**
   - Keep pipelines idempotent
   - Separate transformation logic
   - Use version control for all code
   - Document assumptions
   - Plan for scalability

2. **Performance**
   - Index appropriately
   - Monitor query execution
   - Use columnar formats (Parquet)
   - Partition data strategically
   - Cache intermediate results

3. **Reliability**
   - Implement data validation
   - Handle failures gracefully
   - Monitor pipeline execution
   - Set up alerting
   - Test recovery procedures

4. **Security**
   - Encrypt sensitive data
   - Control access with IAM
   - Audit data access
   - Secure credentials
   - Regular backups

## Common Patterns

**Incremental load pattern:**
```sql
-- Track last load timestamp
CREATE TABLE IF NOT EXISTS pipeline_metadata (
    table_name VARCHAR(255),
    last_load_timestamp TIMESTAMP
);

-- Load new data since last run
INSERT INTO fact_table
SELECT * FROM source_table
WHERE updated_at > (
    SELECT COALESCE(last_load_timestamp, '1970-01-01')
    FROM pipeline_metadata
    WHERE table_name = 'fact_table'
);

-- Update metadata
UPDATE pipeline_metadata
SET last_load_timestamp = CURRENT_TIMESTAMP
WHERE table_name = 'fact_table';
```

**Data quality checks:**
```python
def validate_data(df):
    checks = {
        'null_count': df.isnull().sum(),
        'unique_count': df.nunique(),
        'value_range': {
            'age': (df['age'].min(), df['age'].max()),
        }
    }
    return checks

# Assertions
assert checks['null_count']['email'] == 0, "Email has nulls"
assert checks['unique_count']['id'] == len(df), "Duplicate IDs"
```

## Resources

- **SQL**: sqlzoo.net, mode.com/sql-tutorial
- **dbt**: docs.getdbt.com
- **Apache Spark**: spark.apache.org/docs
- **Airflow**: airflow.apache.org
- **Great Expectations**: greatexpectations.io
- **Data warehouses**: snowflake.com, bigquery.cloud.google.com
