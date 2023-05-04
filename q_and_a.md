# Pyspark Interview Questions

## Questions

1. What is PySpark?
   - PySpark is the Python API for Apache Spark, a distributed computing framework for big data processing. PySpark allows users to write Spark code in Python, which is then translated into the underlying Spark language, enabling high-performance distributed computing.
1. How does PySpark handle big data?
   - PySpark handles big data by distributing data and processing across a cluster of nodes, which allows it to process large datasets in parallel. PySpark also uses a memory-based caching system to optimize query performance.
1. What is the difference between a DataFrame and a RDD?
   - A DataFrame is a distributed collection of data organized into named columns, while a RDD (Resilient Distributed Dataset) is a distributed collection of data without named columns. DataFrames have a more structured API for querying data, while RDDs provide a lower-level API for more flexible processing.
1. What is the role of SparkSession in PySpark?
   - SparkSession is the entry point for PySpark, which allows you to create DataFrames and RDDs, perform SQL queries, and interact with Spark. SparkSession also provides configuration options and other parameters for Spark jobs.
1. How do you read a CSV file in PySpark?
   - You can read a CSV file in PySpark using the read.csv() method.
1. How do you handle missing data in PySpark?
   - PySpark provides several functions for handling missing data, such as fillna() and dropna(). You can use these functions to replace missing values with a default value or to remove rows with missing values. For example:
1. What is a broadcast join in PySpark?
   - A broadcast join is a type of join operation where one DataFrame is small enough to fit in memory and is broadcast to all worker nodes. This can greatly improve performance by reducing the amount of data transferred over the network.
1. What is partitioning in PySpark?
   - Partitioning is the process of splitting a large dataset into smaller, more manageable pieces. When you partition a DataFrame in PySpark, Spark will store each partition separately and execute operations on each partition in parallel. This can greatly improve performance, especially for large datasets.
1. What is a UDF in PySpark?
   - A UDF (User-Defined Function) in PySpark is a function that you can define yourself and then apply to a DataFrame using the withColumn() method. UDFs are useful for performing custom transformations or aggregations on data.
1. What is MLlib in PySpark?
   - MLlib is the machine learning library in PySpark, which provides a range of algorithms for tasks such as classification, regression, clustering, and collaborative filtering. MLlib also includes tools for feature extraction, model evaluation, and data preprocessing

## Transformations and Actions

### Transformations

Transformations are **lazily** operated, i.e. they are not called immediately from the code.

- map(): applies a function to each element of an RDD and returns a new RDD with the transformed values.
- filter(): applies a boolean function to each element of an RDD and returns a new RDD with only the elements that satisfy the condition.
- groupByKey(): groups the elements of an RDD by key and returns a new RDD of key-value pairs.
- reduceByKey(): aggregates the values of each key in an RDD using a provided function and returns a new RDD of key-value pairs with the aggregated values.

### Actions

Actions trigger computation and are **immediately** executed.

- count(): returns the number of elements in an RDD or DataFrame.
- collect(): returns all the elements of an RDD or DataFrame to the driver program as a list.
- take(n): returns the first n elements of an RDD or DataFrame to the driver program as a list.
- reduce(): aggregates the elements of an RDD or DataFrame using a provided function and returns the result.

## Partitioning

PySpark supports two types of partitioning:
1. Hash partitioning: This type of partitioning divides the data into partitions based on a hash function applied to the partition key. The hash function should be designed such that it evenly distributes the data across partitions. Hash partitioning is suitable when the data has a natural partition key, such as a timestamp or a user ID.
1. Range partitioning: This type of partitioning divides the data into partitions based on a range of values. Range partitioning is suitable when the data has an inherent order, such as time-series data or alphabetical data.

To use partitioning in PySpark, you can use the `repartition()` or `coalesce()` methods.

- `repartition(n)` shuffles the data and creates n partitions.
- `coalesce(n)` combines existing partitions into n partitions.