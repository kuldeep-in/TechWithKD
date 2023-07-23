# Databricks Delta Tables

Databricks Delta tables are a storage layer that brings reliability to data lakes. Delta tables allow you to unify batch and streaming data processing on top of blob storage like S3 or ADLS. In this post, we'll look at how Delta tables work and their key features.

Delta Architecture
Delta tables are based on the Delta architecture which logically separates batch data and streaming data. Batch views perform historical analytics while speed views analyze recent data. Delta tables merge these datasets providing a unified view.

### Delta Architecture

<>

### Key Features
**ACID Transactions:** Delta tables support atomic commits and isolation between concurrent operations. Changes are tracked at the row level.

**Scalable Metadata Handling:** Delta tables store metadata about table changes like commits, deletions, etc. This metadata is indexed for fast lookups and scale to large datasets.

**Unified Batch & Streaming:** Streaming data can be incrementally added to Delta tables enabling unified batch and streaming analytics.

**Time Travel:** Delta tables track history allowing you to access previous versions or roll back changes when needed.

**Schema Enforcement:** Schema changes can be enforced when appending data to Delta tables. This guarantees data quality.

**Data Skipping:** Delta tables track which data files have been processed already. This allows skipping over already processed data for more efficient reads.

**Z-Ordering:** Data in Delta tables is Z-ordered by default to improve compression and performance when querying ranges of data.

### Sample Usage
Here is some sample PySpark code to give an idea of Delta table usage:

```python
# Read from Delta table 
df = spark.read.format("delta").load("/data/delta-table") 

# Append new rows to Delta table
new_rows_df = spark.createDataFrame([...]) 
new_rows_df.write.format("delta").mode("append").save("/data/delta-table")

# Upsert (merge) rows based on key
upsert_df = spark.createDataFrame([...])
upsert_df.write.format("delta").mode("overwrite").option("mergeSchema", "true").save("/data/delta-table")
```

Delta tables enable building scalable, reliable data lakes and streaming applications. By providing ACID transactions, unified batch/streaming processing, and time travel, Delta tables unlock new possibilities for data pipelines on blob storage.
