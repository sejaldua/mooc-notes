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

```python
query = "SELECT origin, dest, COUNT(*) as N FROM flights GROUP BY origin, dest"

# Run the query
flight_counts = spark.sql(query)

# Convert the results to a pandas DataFrame
pd_counts = flight_counts.toPandas()

# Print the head of pd_counts
print(pd_counts.head())
```

---

## Create Spark Tables

```python
# Create pd_temp
pd_temp = pd.DataFrame(np.random.random(10))

# Create spark_temp from pd_temp
spark_temp = spark.createDataFrame(pd_temp)

# Examine the tables in the catalog
print(spark.catalog.listTables())

# Add spark_temp to the catalog
spark_temp.createOrReplaceTempView('temp')

# Examine the tables in the catalog again
print(spark.catalog.listTables())
```

---

### Load Data into Spark

```python
file_path = "/usr/local/share/datasets/airports.csv"

# Read in the airports data
airports = spark.read.csv(file_path, header=True)

# Show the data
airports.show()
```

---

### Creating Columns

```python
# Create the DataFrame flights
flights = spark.table("flights")

# Show the head
flights.show()

# Add duration_hrs
flights = flights.withColumn("duration_hrs", flights.air_time / 60)
```
---

### SQL in a nutshell

```sql
SELECT * FROM my_table
```

```sql
SELECT origin, dest, air_time / 60 FROM flights;
```

```sql
SELECT * FROM students
WHERE grade = 'A';
```

---

### SQL in a nutshell (*continued*)

```sql
SELECT COUNT(*) FROM flights
GROUP BY origin;
```

```sql
SELECT origin, dest, COUNT(*) FROM flights
GROUP BY origin, dest;
```

---

### Filtering Data

Filter by passing a string:
```python
long_flights1 = flights.filter("distance > 1000")
```

Filter by passing a column of boolean values:
```python
long_flights2 = flights.filter(flights.distance > 1000)
```

---

### Selecting Data

Selecting using column string syntax:
```python
selected1 = flights.select("tailnum", "origin", "dest")
```

Selecting using `df.colName` syntax:
```python
temp = flights.select(flights.origin, flights.dest, flights.carrier)
```

```python
# Filter the data, first by filterA
selected2 = temp.filter(filterA).filter(filterB)
```

---

### Selecting Data (*advanced*)

Using the `.select()` method to perform column-wise operations:
```python
flights.select(flights.air_time/60)
```

Using the `.alias()` method to rename a column you're selecting:
```python
flights.select((flights.air_time/60).alias("duration_hrs"))
```

The `.selectExpr()` method takes SQL expressions as a string:
```python
flights.selectExpr("air_time/60 as duration_hrs")
```

---

### Aggregating

Common aggregation methods: `.min()`, `.max()`, `.count()`

Can create these `GroupedData` methods by calling `.groupBy()`

Find the minimum value of a column, `col`, in a DataFrame, `df`:

```python
df.groupBy().min("col").show()
```

---

### Aggregation Examples

```python
# Find the shortest flight from PDX in terms of distance
flights.filter(flights.origin == "PDX").groupBy().min("distance").show()

# Find the longest flight from SEA in terms of air time
flights.filter(flights.origin == "SEA").groupBy().max("air_time").show()
```

```python
# Average duration of Delta flights
flights.filter(flights.carrier == "DL").filter(flights.origin == "SEA").groupBy().avg("air_time").show()

# Total hours in the air
flights.withColumn("duration_hrs", flights.air_time/60).groupBy().sum("duration_hrs").show()
```