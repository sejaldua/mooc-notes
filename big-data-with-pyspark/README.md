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

---

### Grouping and Aggregating

```python
# Group by tailnum
by_plane = flights.groupBy("tailnum")

# Number of flights each plane made
by_plane.count().show()

# Group by origin
by_origin = flights.groupBy("origin")

# Average duration of flights from PDX and SEA
by_origin.avg("air_time").show()
```

---

### Grouping and Aggregating (*continued*)

In addition to `GroupedData` methods that we've already seen, there is also the `.agg()` method can be used with any of the functions from the `pyspark.sql.functions` submodule

```python
# Import pyspark.sql.functions as F
import pyspark.sql.functions as F

# Group by month and dest
by_month_dest = flights.groupBy("month", "dest")

# Standard deviation of departure delay
by_month_dest.agg(F.stddev("dep_delay")).show()
```

---

### Joining

A join will combine two different tables along a column that they share. The column is called the *key*. 

In PySpark, joins are performed using the DataFrame method `.join()`, which takes 3 arguments: 
  - the dataframe to join
  - `on` (the name of the key column(s) as a string)
  - `how` (the kind of join to perform)
    - default is `how="leftouter"`

---

### Join (example)

```python
# Examine the data
print(airports.show())

# Rename the faa column
airports = airports.withColumnRenamed("faa", "dest")

# Join the DataFrames
flights_with_airports = flights.join(airports, on='dest', how='leftouter')

# Examine the new DataFrame
print(flights_with_airports.show())
```

---

### Machine Learning Pipelines

`pyspark.ml` module contains `Transformer` and `Estimator` classes
  - `Transformer` classes have a `.transform()` method that takes a DataFrame and returns a new DataFrame
      examples: `Bucketizer` to create discrete bins from a continuous feature and `PCA` to reduce the dimensionalitity of dataset using principal component analysis
  - `Estimator` classes all implement a `.fit()` method that takes a DataFrame and returns a model object
    - `StringIndexerModel` for including categorical data saved as strings and `RandomForestModel` 

---

### Data types

- Spark models *only* handle numeric data (all columns must be either integers or decimals (a.k.a 'doubles'))
- Spark sometimes represents numeric columns as strings containing numbers
- To remedy this, use `.cast()` in combination with `.withColumn()`
  - `.cast()` works on columns while `.withColumn()` works on dataframes
    - takes an argument indicating what kind of value you want to create (e.g. `"integer"` or `"double"`)
  
---

### Cast (example)

```python
# Cast the columns to integers
model_data = model_data.withColumn("arr_delay", model_data.arr_delay.cast("integer"))
model_data = model_data.withColumn("air_time", model_data.air_time.cast("integer"))
model_data = model_data.withColumn("month", model_data.month.cast("integer"))
model_data = model_data.withColumn("plane_year", model_data.plane_year.cast("integer"))
```

---

### Making a Boolean

```python
# Create is_late
model_data = model_data.withColumn("is_late", model_data.arr_delay > 0)

# Convert to an integer
model_data = model_data.withColumn("label", model_data.is_late.cast("integer"))

# Remove missing values
model_data = model_data.filter(
    "arr_delay is not NULL \
    and dep_delay is not NULL \
    and air_time is not NULL \
    and plane_year is not NULL"
  )
```

---

### Strings and factors

- Use `pyspark.ml.features` submodule to create "one-hot vectors" to represent string variables as numeric data
  - **one hot vector** = a way of representing a categorical feature where every observation has a vector in which all elements are 0 except for at most one element, which as a value of 1

---

### How to create a one-hot vector

  - Step 1: create a `StringIndexer`, which takes a dataframe with a column of strings and maps each unique string to a number, then the `Estimator` returns a `Transformer` that takes a DataFrame, attaches the mapping as its metadata, and returns a new dataframe with a numeric column corresponding to the string column
  - Step 2: encode numeric column as a hone-hot vector using a `OneHotEncoder`
    - also creates an `Estimator`, then a `Transformer`

---

### One Hot Encoding (example)

```python
# Create a StringIndexer
carr_indexer = StringIndexer(inputCol="carrier", outputCol="carrier_index")

# Create a OneHotEncoder
carr_encoder = OneHotEncoder(inputCol="carrier_index", outputCol="carrier_fact")
```
