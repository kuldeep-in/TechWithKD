# Databricks Tables

Databricks supports different types of tables that are optimized for different data processing workloads. Choosing the right table type can significantly improve the performance and cost-effectiveness of your data pipelines. In this post, we will explore the main types of tables in Databricks and when to use each one.

### Managed Tables
Managed tables are the default table type in Databricks. They provide a simple way to create tables that are managed completely by Databricks.

**Use cases:**
- Creating tables from DataFrames for ad-hoc data exploration and visualization
- Caching tables frequently accessed by notebooks or jobs to improve performance
- Storing temporary result sets from transformations

``` python
# Create a managed table from a DataFrame
df = spark.read.parquet("/data/profiles") 
df.write.saveAsTable("profiles_table")
```

### External Tables
External tables point to data located in external storage like S3, ADLS, GCS, etc. The data is not managed or controlled by Databricks.

**Use cases:**
- Creating tables pointing to raw data in cloud storage
- Querying data in place instead of loading into Databricks first
- Allowing other applications to access the same data through the table

```python
# Create external table based on data in S3
spark.sql("CREATE TABLE customers_ext (id INT, name STRING) USING parquet LOCATION 's3://bucket/customerdata'")
```

### Cached Tables
Cached tables store data in the Databricks cache for faster queries. Data is lazily loaded from the source.

**Use cases:**
- Caching a table that needs low-latency, interactive queries
- Accelerating ETL jobs by caching transformed outputs
- Caching tables instead of views for performance gains

```python
# Cache a table 
spark.sql("CACHE TABLE customers_cached AS SELECT * FROM customers")
```

### Temporary Tables
Temporary tables only exist for the duration of the session or notebook that created them. Useful for intermediate data.

**Use cases:**
- Storing temporary results within a notebook
- Passing data between different steps in a notebook
- Quick experiments without creating permanent tables

``` python
# Create a temporary table from a DataFrame
cleaned_df = clean_data(df)
cleaned_df.createOrReplaceTempView("cleaned_data")
```

### Live Tables
Live tables point to external data sources that are automatically updated by Databricks when new data arrives. This allows querying of real-time data.

**Use cases:**
- Creating real-time dashboards and reports on streaming data
- Building applications that need to access latest data with low latency
- Joining streaming data with batch data

```python
# Create live table from Kafka stream
spark.sql("CREATE LIVE TABLE logs (level STRING, msg STRING) TBLPROPERTIES (kafka.bootstrap.servers='host:port', subscribe='topic')")
```

### Streaming Tables
Streaming tables are designed for streaming writes to append-only tables from structured streaming queries.

**Use cases:**
- Writing the results of streaming aggregations to tables
- Appending streams into tables for auditing or logging
- Streaming ETL from sources like Kafka into queryable tables

``` python
# Write streaming query results to table
stream_df = spark.readStream("<stream_source>")
stream_df.writeStream.format("parquet").option("checkpointLocation", "...").start("stream_table")
```
