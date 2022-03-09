---
marp: true
theme: gaia
size: 16:9
_class: 
- lead
- invert
paginate: true
math: katex
---

# Big Data with PySpark

---

# Introduction to PySpark

---

## What is Spark?

- Spark is a platform for cluster computing
- lets you spread data and computations over clusters with multiple nodes (think of each node as a separate computer)
- as each node works on its own subset of the total data, it also carries out a part of the total calculations required, so that both data processing and computation are performed in parallel over the nodes in the cluster

---

## Using Spark in Python?

- cluster is hosted on remote machine (**master**) which is connected to all other nodes (*workers*)
- to create connection, create an instance of the `SparkContext` class, named `sc`

```python
# verify SparkContext
print(sc)

# print Spark version
print(sc.version)
```

---

## Using DataFrames

- Spark's core data structure = Resilient Distributed Dataset (RDD)
  - enables Spark to do its magic by splitting data across multiple nodes in the cluster
- was designed to behave a lot like SQL table
- many ways to arrive at same result, but query can be optimized when working with RDDs
- need to create `SparkSession` object (called `spark`) from `SparkContext`

---

## SparkSession Example

```python
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession

# Create my_spark
my_spark = SparkSession.builder.getOrCreate()

# Print my_spark
print(my_spark)
```

### Viewing Tables

```python
# Print the tables in the catalog
print(spark.catalog.listTables())
```
---

## Query Table

```python
query = "FROM flights SELECT * LIMIT 10"

# Get the first 10 rows of flights
flights10 = spark.sql(query)

# Show the results
flights10.show()
```

---

## Pandafy a Spark DataFrame

