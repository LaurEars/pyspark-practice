# Pyspark Cheat Sheet

## Basic AI-generated cheat sheet

```python
# PySpark Cheat Sheet

# 1. Importing PySpark:
   from pyspark.sql import SparkSession

# 2. Creating a SparkSession:
   spark = SparkSession.builder \
         .appName("myApp") \
         .getOrCreate()

# 3. Reading data:
   df = spark.read.format("csv") \
         .option("header", "true") \
         .option("inferSchema", "true") \
         .load("path_to/data.csv")

# 4. Displaying data, showing DataFrame schema:
   df.show()
   df.printSchema()

# 5. Selecting data:
   df.select("col1", "col2")
   df.filter(df.col1 == "value")
   df.filter("col2 > 10")
   # example of grouping by col1 and summing over col2
   df.groupBy("col1").agg({"col2": "sum"})

# 6. Joining data:
   joined_df = df1.join(df2, on="key")

# 7. Aggregating data:
   df.groupBy("col1").agg({"col2": "sum"})

# 8. Writing data:
   df.write.format("csv") \
         .option("header", "true") \
         .save("path_to/output")

# 9. Working with SQL:
   df.createOrReplaceTempView("myTable")
   sql_df = spark.sql("SELECT col1, col2 FROM myTable")

# 10. Machine Learning (Logistic Regression):
    from pyspark.ml.classification import LogisticRegression
    model = LogisticRegression().fit(train_df)
    predictions = model.transform(test_df)
    
# 11. Machine Learning (Linear Regression):
    from pyspark.ml.regression import LinearRegression
    lr = LinearRegression(featuresCol="features", labelCol="label")
    model = lr.fit(train_data)
    predictions = model.transform(test_data)

# 12. Showing data
    df.show(10)
    df.tail(10)  # required argument not lazily evaluated
    df.collect()  # return everything
```

## SQL functions

```python
from pyspark.sql.functions import *

# Aggregate functions
sum("column1")
avg("column1")
count("*")

# Date functions
year("date_column")
month("date_column")
dayofmonth("date_column")

# String functions
concat("string1", "string2")
substring("string_column", 2, 4)
lower("string_column")
upper("string_column")

# Array functions
array("column1", "column2")
array_contains("array_column", "value")
explode("array_column")
```

## Joins

```python
# Joins return a new data frame, based off the calculated join

# Inner join
joined_df = df1.join(df2, "common_column")

# Left outer join
joined_df = df1.join(df2, "common_column", "left_outer")

# Full outer join
joined_df = df1.join(df2, "common_column", "outer")
```

## SQL Window Functions

```python
from pyspark.sql.window import Window

# Define a window partition column and order column
window_spec = Window.partitionBy("partition_column").orderBy("order_column")

# Use a window function
rank().over(window_spec)
dense_rank().over(window_spec)
row_number().over(window_spec)
```

## Python UDFs

Python User-Defined Functions

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import *

# Define a UDF using python function
my_udf = udf(lambda x: x + 1, IntegerType())

# apply UDF
df.withColumn("new_column", my_udf("old_column"))
```

## Broadcast Joins

Store one data frame across all nodes. By storing smaller df in memory, provide considerable speed-up.

```python
from pyspark.sql.functions import broadcast

small_df = ...
large_df = ...

joined_df = large_df.join(broadcast(small_df), "join_column")
```

## Partitioning

Increase number of partitions
```python
df = df.repartition(num_partitions, "partition_column")
```

Reduce number of partitions
```python
df = df.coalesce(num_partitions)
```

## Caching

To cache a df that will be used in multiple places of the code, without having to recompute.
```python
df = df.cache()
```

Or store to disk (see storage types available).
```python
df = df.persist(StorageLevel.DISK_ONLY)
```

## Missing Data

Replace missing values
```python
df = df.fillna({"column1": "default_value", "column2": 0})
```

Drop missing values
```python
df = df.dropna()
```