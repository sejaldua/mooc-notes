# Dataquest: Intermediate SQL

## Joining Data in SQL

- Topics Covered:
  - employ the four common join types
  - employ joins with subqueries to write intermediate queries
  - employ joins to answer business questions

### Introducing Joins

> Write a query that returns all columns from the `facts` and `cities` tables
>
> - Use an `INNER JOIN` to join the `cities` table to the `facts` table
> - Join the tables on the values where `facts.id` and `cities.facts_id` are equal
> - Limit the query to the first 10 rows

```SQL
SELECT * FROM facts
INNER JOIN cities ON cities.facts_id = facts.id
LIMIT 10;
```

### Understanding Inner Joins

> Write a query that
>
> - Joins `cities` to `facts` using an `INNER JOIN`
> - Uses aliases for table names
> - Includes, in order:
>   - All columns from `cities`
>   - The `name` column from the `facts` aliased to `country_name`
> - Includes only the first 5 rows
