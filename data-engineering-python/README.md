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
