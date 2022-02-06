---
id: bMOyoNtbjZ2cn4RRAmp0W
title: Work with user-defined Functions
desc: ''
updated: 1644120616119
created: 1644118096680
---

Part of the [[azure.certs.datascientist]] study path

Related to [[azure.databricks]]

> [Notes from MS Learn Course](https://docs.microsoft.com/en-us/learn/modules/work-with-user-defined-functions/)

User-defined functions are abbreviated as **UDF** and are conceptually similar to a [[libs.pandas]] `apply` function.

## Defining UDFs
```py
def firstInitialFunction(name):
  return name[0]

firstInitialUDF = udf(firstInitialFunction, "string") # Return type optional
```

UDFs can be applied to all columns fairly easily:
```py
from pyspark.sql.functions import col

# Run each host_name through the UDF and use the result. Essentially map a column
display(df.select(firstInitialUDF(col("host_name")))) result
```

### SQL Support

Supported in [[techs.spark]] [[langs.sql]] too:

```py
# Register the dataframe for SQL Querying in the FROM clause
df.createOrReplaceTempView("myDF")

# Register the UDF for SQL querying
spark.udf.register("sql_udf", firstInitialFunction)
```

And then queried:

```sql
SELECT 
    sql_udf(host_name) AS firstInitial 
FROM 
    myDF
```

The registration step of the UDF can be skipped by using decorators in [[langs.python]]:
```py
@udf("string")
def myFunction(value):
    return value[0]
```

### Performance Considerations

**WARNING:** These are significantly slower than built-in functions due to serialization and the Python interpreter.

Vectorized UDFs in [[techs.spark]] 2.3+ can help though:

```py
@pandas_udf("string")
def vectorizedUDF(name):
  return name.str[0]
```

Moral of the story: `@pandas_udf` > `@udf`
