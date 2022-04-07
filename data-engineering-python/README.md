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


# Data Engineering with Python 
### DataCamp

---

# Data Engineering for Everyone

---

## Structured vs Unstructured Data

| Structured | Semi-Structured | Unstructured |
| --- | --- | --- |
| Is easy to search and organize | Is moderately easy to search and organize | Is difficult to search and organize|
| Corresponds to data in a tabular format | Follows a model while allowing more flexibility than structured data | Stores images, pictures, videos, and text |
| Is created and queried using SQL | Is stored in XML or JSON format, or in NoSQL databases | Is usually stored in data lakes |

---

## SQL Databases

- SQL = structured query language
- industry standard for Relational Database Management System (RDBMS)
- allows you to access many records at once, and group, filter, or aggregate them
- close to written English-- easy to write and understand
- data engineers use SQL to create and maintain databases
- data scientists use SQL to query databases

---

## SQL for Data Engineers

```sql
CREATE TABLE employees (
  employee_id INT,
  first_name VARCHAR(255),
  role VARCHAR(255),
  team VARCHAR(255),
  full_time BOOLEAN,
  office VARCHAR(255)
)
```

---

## Database Schema

- databases are made of tables
- database schema governs how tables are related

---

## Data Warehouses and Data Lakes

| Data Lake | Data Warehouse |
| --- | --- |
| Stores all the raw data | Specific data for specific use |
| Can be petabytes (1 million GBs) | Relatively small |
| Stores all data structures | Stores mainly structured data |
| Cost-effective | More costly to update |
| Difficult to analyze | Optimized for data analysis |
| Requires an up-to-date data catalog | |
| Used by data scientists | Also used by data analysts and business analysts |
| Big data, real-time analytics | Ad-hoc, read-only queries |

---

## Data Catalog for Data

- what is the source of this data?
- where is this data used?
- who is the owner of the data?
- how often is the data updated?
- good practice in terms of data governance
- ensures reproducibility
- good practice for any data storage solution
  - good for reliability, autonomy, scabilibility, speed

---

## Database vs Data Warehouse

- database
  - general term
  - definition: *organized data stored and accessed on a computer*
- data warehouse
  - type of database

---

## Processing Data

- **data processing** = converting raw data into meaningful information
- remove unwanted data
- optimize memory, process and network costs
- convert data from one type to another
- organize data
- fit into a schema / structure
- increase productivity

---

## How data engineers process data

- data manipulation, cleaning, and tidying tasks
  - can be automated
  - always need to be done
- store data in a sanely structured database
- create views on top of the database tables
- optimizing the performance of the database
  
---

## Scheduling data

- can apply to any task listed in data processing
- scheduling is the glue of your system
- holds each piece and organize how they work together
- runs tasks in a specific order and resolves all dependencies

---

## Manual, time, and sensor scheduling

- manually
- automatically run at a specific time
- automatically run if a specific condition is met (sensor scheduling)
  - requires having sensors always listening to see if something has been added

---

## Batches and Streams

- batches
  - group records at intervals
  - often cheaper (can schedule when resources aren't being used, e.g. over night)
- Streams
  - send individual records right away

---

## Parallel Computing / Processing

- basis of modern data processing tools
- how it works
  - split tasks up into several smaller subtasks
  - subtasks are distributed over several computers
- benefits: extra processing power, reduced memory footprint
- disadvantages: moving data incurs a cost, requires communication time between processes

---

## Cloud Computing

| Servers on premises ("on prem") | Servers on the cloud |
| --- | --- |
| Bought | Rented |
| Need space | Don't need space |
| Electrical and maintenance cost | Use just the resources we need |
| Enough power for peak moments | When we need them |
| Processing power unused at quieter times | The closer to the user, the better |

---

## Cloud computing for data storage

- database reliability: data replication
- risk with sensitive data
- 3 big players: AWS (32.4%), Microsoft Azure (17.6%), Google Cloud (6%)

| Company | File Storage | Computation | Databases |
| --- | --- | --- | --- |
| AWS | S3 | EC2 | RDS |
| Microsoft Azure | Blob Storage | Virtual Machines | SQL Database |
| Google Cloud | Cloud Storage | Compute Engine | Cloud SQL |

---

## Multicloud

- advantages
  - reduce reliance on a single vendor
  - cost-efficiencies
  - local laws requiring certain data to be physically present
- disadvantages
  - cloud providers try to lock in consumers by integrating services
  - incompatibility
  - security and governance

---

# Data Engineering with Python

---

## What is data engineering?

- **data engineer** = an engineer that develops, constructs, tests, and maintains architectures such as databases and large-scale processing systems
- tasks
  - processing large amounts of data
  - use of clusters of machines

---

## Data Engineer vs Data Scientist

| Data Engineer | Data Scientist |
| --- | --- |
| develop scalable data architecture | mining data for patterns |
| streamline data acquisition | statistical modeling |
| set up processes to bring together data | predicting models using machine learning |
| clean corrupt data | monitor business processes |
| well versed in cloud technology | clean outliers in data |

---

## Data Engineering Tools

- databases: MySQL, PostgreSQL
- processing: Spark, Hive
- scheduling: Apache Airflow, Oozie, Cron (Linux)


---

## Data storage in the cloud

- *clusters of machines required*
- problem: self-hosted data center
  - cover electrical and maintenance costs
  - peaks vs. quiet moments: hard to optimize
- solution: use the cloud (however many resources you need, when you need them)

---

## Data storage in the cloud

- *reliability is required*
- problem: self-hosted data center
  - disaster will strike
  - need different geographical locations

---

## 3 big players: AWS, Azure, and Google

- AWS: 32% market share in 2018
- Azure: 17% market share in 2018
- Google Cloud: 10% of market share in 2018

---

## Services

- **storage**: *upload files (e.g. storing product images)*
- **computation**: *perform calculations (e.g. hosting a web server)*
- **databases**: *hold structured information*

---

## Databases

- **database** = usually large collection of data organized especially for rapid search and retrieval
  - holds data
  - organizes data
  - retrieve data through DBMS

---

## Structured and Unstructured Data

- structured: database schema
  - relational database
- semi-structure:
  - JSON
- unstructured: schema-less, more like files
  - videos, photos

---

## SQL and NoSQL

- SQL (e.g. MySQL, PostgreSQL)
  - tables
  - database schema
  - relational databases
- NoSQL (e.g. Redis, MongoDB)
  - non-relational databases
  - structured or unstructured
  - key-value stores (e.g. caching)
  - document DB (e.g. JSON objects)

---

## SQL: the database schema

- star schema: consists of one or more fact tables referencing any number of dimension tables
  - facts: things that happened (e.g. product orders)
  - dimensions: information on the world (e.g. customer information)

---

## SQL: the database schema (*continued*)

```python
# Complete the SELECT statement
data = pd.read_sql("""
SELECT first_name, last_name FROM "Customer"
ORDER BY last_name, first_name
""", db_engine)

# Show the first 3 rows of the DataFrame
print(data.head(3))

# Show the info of the DataFrame
print(data.info())
```

---

## Joining on relations

```python
# Complete the SELECT statement
data = pd.read_sql("""
SELECT * FROM "Customer"
INNER JOIN "Order"
ON "Order"."customer_id"="Customer"."id"
""", db_engine)

# Show the id column of data
print(data.id)
```

---

## Idea behind parallel computing

*basis of modern data processing tools*

- optimize **memory** and **processing power**
  - split tasks into subtasks
  - distribute subtasks over several computers

---

## Risks of parallel computing

*overhead due to communication*... speed does not increase linearly due to *parallel slow-down*

- task needs to be large
- need several processing units


---

## Parallel Computing Example

```python
from multiprocessing import Pool

def take_mean_age(year_and_group):
    year, group = year_and_group
    return pd.DataFrame({"Age": group["Age"].mean()}, index=[year])

with Pool(4) as p:
    results = p.map(take_mean_age, athlete_events.groupby("Year"))

result_df = pd.concat(results)
```

---

## From tasks to subtasks

The `multiprocessor.Pool` API allows you to distribute your workload over several processes. `parallel_apply()` takes as input the function being applied, the grouping used, and the number of cores needed for the analysis.

```python
# Function to apply a function over multiple cores
@print_timing
def parallel_apply(apply_func, groups, nb_cores):
    with Pool(nb_cores) as p:
        results = p.map(apply_func, groups)
    return pd.concat(results)

# Parallel apply using 4 cores
parallel_apply(take_mean_age, athlete_events.groupby('Year'), 4)
```

---

## Parallel computing using a dataframe

A more convenient way to parallelize an apply over several groups is using the `dask` framework and its abstraction of the `pandas` DataFrame

```python
import dask.dataframe as dd

# Set the number of partitions
athlete_events_dask = dd.from_pandas(athlete_events, npartitions=4)

# Calculate the mean Age per Year
print(athlete_events_dask.groupby('Year').Age.mean().compute())
```

---

## Parallel computation frameworks

- **Apache Hadoop**
  - HDFS = distributed file system where files reside on multiple different computers

---

## Parallel computation frameworks (continued)

- **Apache Hadoop**
  - Map Reduce = split tasks into subtasks between several processing units
    - Hive = layered on top of Hadoop ecosystem using SQL to make it easier to write map-reduce jobs *(originally developed by Facebook)*
    - Spark = didstributes data processing tasks between clusters of computers while avoiding disk writes and keeping as much processing as possible in memory *(originally developed by UC Berkeley)*

---

## Resilient distributed datasets (RDDs)

- Spark relies on them
- don't have named columns
- list of tuples
- 2 types of operations:
  - transformations: `.map()` or `.filter()`
  - actions: `.count()` or `.first()`

---

## PySpark GroupBy

```python
# Print the schema of athlete_events_spark
print(athlete_events_spark.printSchema())

# Group by the Year, and find the mean Age
print(athlete_events_spark.groupBy('Year').mean('Age').show())
```

---

## Workflow scheduling frameworks

- `cron` = scheduling tool
- DAGs = Directed Acycling Graph
  - set of nodes
  - connected by directed edges
  - no cycles
- tools: Linux cron, Spotify Luigi, Apache Airflow

---

## Airflow DAGs

```python
# Create the DAG object
dag = DAG(dag_id="car_factory_simulation",
          default_args={"owner": "airflow","start_date": airflow.utils.dates.days_ago(2)},
          schedule_interval="0 * * * *")

# Task definitions
assemble_frame = BashOperator(task_id="assemble_frame", bash_command='echo "Assembling frame"', dag=dag)
place_tires = BashOperator(task_id="place_tires", bash_command='echo "Placing tires"', dag=dag)
assemble_body = BashOperator(task_id="assemble_body", bash_command='echo "Assembling body"', dag=dag)
apply_paint = BashOperator(task_id="apply_paint", bash_command='echo "Applying paint"', dag=dag)

# Complete the downstream flow
assemble_frame.set_downstream(place_tires)
assemble_frame.set_downstream(assemble_body)
assemble_body.set_downstream(apply_paint)
```

---

# Extract, Transform, and Load (ETL)

---

## Extract

extracting data from persistent storage (e.g. Amazon S3 or a SQL database or an API) into memory

- unstructured (plain text)
- flat files
  - row = record
  - column = attribute
  - e.g. `.tsv` or `.csv`

---
## Extract (*continued*)
- JSON (JavaScript Object Notation)
  - semi-structured
  - atomic: `number`, `string`, `boolean`, `null`
  - composite: `array`, `object`

---

## Data in databases

- Applications databases
  - transactions
  - inserts or changes
  - OLTP (online transaction processing)
  - row-oriented

---

## Data in databases

- Analytical databases
  - OLAP (online analytical processing)
  - column-oriented

---

## Extraction from databases

- Connecting string / URI
  - `postgresql://[user[:password]@][host][:port]`
- Use in Python

  ```python
  import sqlalchemy
  connection_uri = "postgresql://repl:password@localhost:5432/pagila"

  import pandas as pd
  pd.read_sql("SELECT * FROM customer", db_engine)
  ```

---

## Transform

Example: split (pandas)

```python
customer_df # Pandas DataFrame with customer data

# Split email column into 2 columns on the '@' symbol
split_email = customer_df.email.str.split("@", expand=True)

# Create 2 new columns using the resulting DataFrame
customer_df = customer_df.assign(
  username=split_email[0],
  domain=split_email[1]
)
```

---

## Transforming in PySpark

```python
import pyspark.sql
spark = pyspark.sql.SparkSession.builder.getOrCreate()
spark.read.jdbc("jdbc:postgresql://localhost:5432/pagila",
  "customer",
  properties={"user":"repl", "password":"password"})
```

---

## Transform (*continued*)

Example: join

```python
customer_df # PySpark DataFrame with customer data
ratings_df # PySpark DataFrame with ratings data

# Groupby ratings
ratings_per_customer = ratings_df.groupBy("customer_id").mean("rating")

# Join on customer ID
customer_df.join(
  ratings_per_customer,
  customer_df.customer_id==ratings_per_customer.customer_id
)
```

---

## Loading

- Analytics
  - aggregate queries
  - OLAP
  - column-oriented
  - quries about subset of columns
  - parallelization

---

## Loading (*continued*)

- Applications
  - lots of transactions
  - OLTP
  - row-oriented
  - stored per record
  - added per transaction
  - e.g. adding customer is fast

---

## MPP Databases

*Massively Parallel Processing Databases*

- Amazon Redshift
- Azure SQL Data Warehouse
- Google BigQuery

---

## Redshift Example

Load from file to columnar storage format

```python
df.to_parquet("./s3://path/to/bucket/customer.parquet")
df.write.parquet("./s3://path/to/bucket/customer.parquet")
```

```sql
COPY customer
FROM 's3://path/to/bucket/customer.parquet'
FORMAT as parquet
```

---

## Load to PostgreSQL

```python
# transformation on data
recommendations = transform_find_recommendations(ratings_df)

# load into postgreSQL database
recommendations.to_sql("recommendations", db_engine, schema="store", if_exists="replace")
```

---

## ETL Function

```python
def extract_table_to_df(tablename, db_engine):
  return pd.read_sql("SELECT * FROM {}".format(tablename), db_engine)

def split_columns_transform(df, column, pat, suffixes):
  # Converts column into str and splits it on pat...

def load_df_into_dwh(film_df, tablename, schema, db_engine):
  return pd.to_sql(tablename, db_engine, schema=schema, if_exists="replace")
```

---

## All together

```python
db_engines = { ... } # Needs to be configured
def etl():
  # Extract
  film_df = extract_table_to_df("film", db_engines["store"])
  # Transform
  film_df = split_columns_transform(film_df, "rental_rate", ".", ["_dollar", "_cents"])
  # Load
  load_df_into_dwh(film_df, "film", "store", db_engines["dwh"])
```

---

## DAG Definition FIle

```python
from airflow.models import DAG
from airflow.operators.python_operator import PythonOperator

dag = DAG(dag_id="etl_pipeline", schedule_interval="0 0 * * *")
etl_task = PythonOperator(task_id="etl_task", python_callable=etl, dag=dag)
```

