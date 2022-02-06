---
id: TnGXJc2G0Ts5fGAQ3M1qj
title: Delta Lake
desc: ''
updated: 1644123652857
created: 1644120546645
---

[Web Site](https://delta.io/)

A high performance and higher reliability version of a [[azure.datalake]]

Open source

Integrates with [[techs.spark]] and pairs with [[techs.databricks]]

Supported on a wide variety of platforms

> Delta Lake is a transactional storage layer designed specifically to work with Apache Spark and Databricks File System (DBFS).
> 
> -- [MS Learn](https://docs.microsoft.com/en-us/learn/modules/build-query-delta-lake/2-describe-open-source)

An optimized storage layer for [[Data Lakes|techs.datalake]]

Benefits:

- ACID support
- Scalable metadata
- Data Versioning via Time Travel
- Parquet Format
- Batch and Streaming Support
- Schema enforcement
- Schema evolution
- Spark compatibility

Used in [[langs.python]] by specifying `df.write.format("delta")`

### Loading and Writing

```py
df
  .write
  .format("delta")
  .partitionBy("SomeColumn")
  .mode("append")
  .save(DataPath)
```


### Creating a Table

```py
spark.sql("""
  DROP TABLE IF EXISTS my_table
""")
spark.sql("""
  CREATE TABLE my_table
  USING DELTA
  LOCATION '{}'
""".format(DataPath))
```

### Upsert Data

```sql
MERGE INTO my_table
USING my_df -- registered earlier via df.createOrReplaceTempView("my_df)
ON my_table.SomeColumn = my_df.SomeColumn
  AND my_table.AnotherColumn = my_df.AnotherColumn
WHEN MATCHED THEN
  UPDATE SET *
WHEN NOT MATCHED
  THEN INSERT *
```

### Features of [[techs.deltalake]]

Adding [[techs.deltalake]] to [[azure.databricks]] we get additional features in optimization, data skipping, Z-Order support, and caching

**Optimization** solves the small file problem of heavy I/O for very very small files slowing things down by automatically combining them into a larger "compacted" file.

The `OPTIMIZE` command handles this by combining the files up into a single file up to 1GB per combined file. This is a slow operation and should be scheduled at a frequency according to budget preferences.

**Data skipping** ignores files that would never meet a partition based on the `WHERE` clause

**ZOrdering** manipulates partitions to put related data together.

The `VACUUM` command is used to clean up files after `OPTIMIZE` commands