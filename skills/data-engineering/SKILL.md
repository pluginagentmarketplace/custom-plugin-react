---
name: data-engineering
description: SQL, Python, Spark, data pipelines, warehouses, ETL/ELT, data quality, governance.
---

# Data Engineering Skills

## SQL Optimization

```sql
-- Efficient query with proper indexing
CREATE INDEX idx_orders_date_user ON orders(order_date DESC, user_id);

-- Window functions for analytics
SELECT 
    user_id,
    order_date,
    amount,
    SUM(amount) OVER (PARTITION BY user_id ORDER BY order_date) as cumulative_amount,
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY order_date DESC) as recency_rank
FROM orders;

-- CTE for readability
WITH monthly_revenue AS (
    SELECT 
        DATE_TRUNC('month', order_date) as month,
        SUM(amount) as revenue
    FROM orders
    GROUP BY DATE_TRUNC('month', order_date)
)
SELECT month, revenue, LAG(revenue) OVER (ORDER BY month) as prev_month_revenue
FROM monthly_revenue;
```

## Apache Spark ETL

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import *

spark = SparkSession.builder.appName("ETL").getOrCreate()

# Read data
df_raw = spark.read.parquet("s3://bucket/raw-data/")

# Transform
df_clean = (df_raw
    .filter(col("status") == "active")
    .withColumn("revenue", col("price") * col("quantity"))
    .withColumn("processed_date", current_timestamp())
)

# Aggregate
df_summary = (df_clean
    .groupBy("product_id", "region")
    .agg(
        sum("revenue").alias("total_revenue"),
        count("*").alias("transaction_count"),
        avg("quantity").alias("avg_quantity")
    )
    .filter(col("total_revenue") > 1000)
)

# Write
df_summary.write.mode("overwrite").parquet("s3://bucket/summary/")
```

## dbt for Data Transformation

```sql
-- models/staging/stg_users.sql
{{ config(materialized='table') }}

SELECT
    user_id,
    email,
    name,
    LOWER(TRIM(email)) as normalized_email,
    created_at,
    CURRENT_TIMESTAMP as dbt_loaded_at
FROM {{ source('raw', 'users') }}
WHERE created_at IS NOT NULL

-- models/marts/fct_orders.sql
{{ config(materialized='table') }}

SELECT
    o.order_id,
    o.user_id,
    u.email,
    o.order_date,
    o.amount,
    CASE 
        WHEN o.amount > 1000 THEN 'high'
        WHEN o.amount > 100 THEN 'medium'
        ELSE 'low'
    END as amount_category
FROM {{ ref('stg_orders') }} o
LEFT JOIN {{ ref('stg_users') }} u ON o.user_id = u.user_id
```

