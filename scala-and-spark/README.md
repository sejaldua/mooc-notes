# scala-and-spark

Course Notes from Scala and Spark for Big Data and Machine Learning Udemy Course

## Big Data

- a local process will use the computation resources of a single machine
- a distributed process has access to the computational resources across a number of machines connected through a network
- after a certain point, it is easier to scale out to many lower CPU machines than to try to scale up to a single machine with high CPU
- advantages of distributed machines
  - easy to scale: add more machines
  - fault tolerance: if one machine fails, the whole network can still go on
- use Hadoop Distributed File System (HDFS) to distribute large data sets
- use MapReduce to distribute a computational task to a distributed data set

## Spark

- one of the latest technologies being used to quickly and easily handle Big Data
- open source project on Apache
- first released in February 2013
- exploded due to ease of use and speed
- created at AMPLab of UC Berkeley

### Spark vs MapReduce

- MapReduce requires files to be stored in HDFS, Spark does not!
- Spark can perform operations up to 100x faster than MapReduce
  - MapReduce writes most data to disk after each map and reduce operation
  - Spark keeps most of the date in memory after each transformation; can spill over to disk if memory is filled

### Spark RDDs

- Resilient Distributed Dataset (RDD) has 4 main features:
  - distributed collection of data
  - fault-tolerant
  - parallel operation - partitioned
  - ability to use many data sources
- immutable, lazily evaluated, and cacheable
- 2 types of RDD operations
  - transformations: recipes to follow
  - actions: actually perform what the recipe says to do and returns something back
- Spark 2.0 is moving toward dataframe-based syntax

## Scala Basics

General format when creating an object:

- `val <name>: <type> = <literal>`
- `var <name>: <type> = <literal>`

```scala
var myvar: Int = 10
```

- can reassign variables, but cannot reassign values
- scala can infer data types

```scala
val c = 12
val my_string = "Hello"
```

- names cannot start with special character of number
- cannot use periods instead of underscores

Hack:

```scala
val `my.string` = "hello"
```
