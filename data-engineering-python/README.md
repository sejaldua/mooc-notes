# data-engineering-with-python

Data Engineering with Python | DataCamp

## Chapter 1 - What is data engineering?

### Data Engineers vs Data Scientists

| Data Engineers | Data Scientists |
| --- | --- |
| ingest and store the data so it's easily accessible and ready to be analyzed | prepare the data according to their analysis needs, explore it, visualize it, run experiments, and built predictive models |

***Data engineers lay the groundwork that makes data science activity possible!***

Music Streaming Company Example

| Data Engineers | Data Scientists |
| --- | --- |
| ensure that databases are optimized for analysis (correct table structure, information easy to retrieve) | access the database and exploit the data it contains |
| build data pipelines | use the pipelines' outputs
| software experts | analytics experts |
| Python, Java, SQL to create, update, and transform databases | Python, R, SQL to query databases |
| use Java to build a pipeline collecting album covers and storing them | find out in which countries certain artists are popular to given them insights on where to tour |
| ensure that people who use the databases can't erase music videos by mistake | identify which customers are likely to end their Spotflix subscriptions, so marketing can target them and encourage them to renew |
| provide listening sessions data so it can be analyzed with minimal preparation work | use Python to run an analysis on whether users prefer having the search bar on the top left or the top right of the Spotflix desktop app |

> In 2012, IBM declared that 90% of the data in the world had been created in the past 2 years. That same year, the amount of digital data in the world first exceeded 1 zetabyte (1 billion terabytes). In 2020, we're expected to reach 44 zetabytes.

> "Data is the new oil" - The Economist, 2017-05-06, by David Parkins

### The Data Pipeline

***Data pipelines ensure an efficient flow of the data***

Automate

- Extracting
- Transforming
- Combining
- Validating
- Loading

Reduce

- Human intervention
- Errors
- Time it takes data to flow

#### ETL and Data Pipelines

ETL

- popular framework for designing data pipelines
- 1) **Extract** data
- 2) **Transform** extracted data
- 3) **Load** transformed data to another database

Data Pipelines

- move data from one system to another
- may follow ETL
- data may not be transformed

## Chapter 2 - Storing data
