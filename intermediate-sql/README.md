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

```SQL
SELECT c.*, f.name country_name FROM facts f
INNER JOIN cities c ON c.facts_id = f.id
LIMIT 5;
```

### Practicing Inner Joins

> Write a query that uses an `INNER JOIN` to join the two tables in your query and returns, in order:
>
> - A column of country names, called `country`
> - A column of each country's capital city, called `capital_city`

```SQL
SELECT f.name country, c.name capital_city FROM cities c
INNER JOIN facts f ON f.id = c.facts_id
WHERE c.capital = 1
```

### Left Joins

> Write a query that returns the countries that don't exist in `cities`
>
> - Your query should return two columns:
>   - The country names, with the alias `country`
>   - The country population
> - Use a `LEFT JOIN` to join `cities` to `facts`
> - Include only the countries from `facts` that don't have a corresponding value in `cities`

```SQL
SELECT f.name country, f.population
FROM facts f
LEFT JOIN cities c ON c.facts_id = f.id
WHERE c.name IS NULL;
```

### Finding the Most Populous Capital Cities

> Write a query that returns the 10 capital cities with the highest population ranked from biggest to smallest population
>
> - You should include the following columns, in order:
>   - `capital_city`, the name of the city
>   - `country`, the name of the country the city is from
>   - `population`, the population of the city

```SQL
SELECT c.name capital_city, f.name country, c.population
FROM facts f
INNER JOIN cities c ON c.facts_id = f.id
WHERE c.capital = 1
ORDER BY c.population DESC
LIMIT 10;
```

### Combining Joins with Subqueries

> Using a join and a subquery, write a query that returns capital cities with populations of over 10 million ordered from largest to smallest. Include the following columns:
>
> - `capital_city` - the name of the city
> - `country` - the name of the country the city is the capital of
> - `population` - the population of the city

```SQL
SELECT c.name capital_city, f.name country, c.population population
FROM facts f
INNER JOIN (
            SELECT * FROM cities
            WHERE capital = 1
            AND population > 10000000
           ) c ON c.facts_id = f.id
ORDER BY population DESC;
```

### Challenge: Complex Query with Joins and Subqueries

> Write a query that generates output as shown above. The query should include:
>
> - The following columns, in order:
>   - `country`, the name of the country
>   - `urban_pop`, the sum of the population in major urban areas belonging to that country
>   - `total_pop`, the total population of the country
>   - `urban_pct`, the percentage of the population within urban areas, calculated by dividing `urban_pop` by `total_pop`
> - Only countries that have an `urban_pct` greater than 0.5
> Rows should be sorted by `urban_pct` in ascending order

```SQL
SELECT
    f.name country,
    c.urban_pop,
    f.population total_pop,
    (c.urban_pop / CAST(f.population AS FLOAT)) urban_pct
FROM facts f
INNER JOIN (
            SELECT
                facts_id,
                SUM(population) urban_pop
            FROM cities
            GROUP BY c.facts_id
           ) c ON c.facts_id = f.id
WHERE urban_pct > .5
ORDER BY urban_pct ASC;
```

Expected Output:

| **`country`** | **`urban_pop`** | **`total_pop`** | **`urban_pct`** |
| --- | --- | --- | --- |
| Uruguay | 1672000 | 3341893 | 0.500315 |
| Congo, Republic of the | 2445000 | 4755097 | 0.514185 |
| Brunei | 241000 | 429646 | 0.560927 |
| New Caledonia | 157000 | 271615 | 0.578024 |
| Virgin Islands | 60000 | 103574 | 0.579296 |
|Falkland Islands (Islas Malvinas) | 2000 | 3361 | 0.595061 |
|Djibouti | 496000 | 828324 | 0.598800 |
|Australia | 13789000 | 22751014 | 0.606083 |
|Iceland | 206000 | 331918 | 0.620635 |
|Israel | 5226000 | 8049314 | 0.649248 |
|United Arab Emirates | 3903000 | 5779760 | 0.675288 |
|Puerto Rico | 2475000 | 3598357 | 0.687814 |
|Bahamas, The | 254000 | 324597 | 0.782509 |
|Kuwait | 2406000 | 2788534 | 0.862819 |
|Saint Pierre and Miquelon | 5000 | 5657 | 0.883861 |
|Guam | 169000 | 161785 | 1.044596 |
|Northern Mariana Islands | 56000 | 52344 | 1.069846 |
|American Samoa | 64000 | 54343 | 1.177705 |

## Intermediate Joins in SQL

### Joining Three Tables

```sql
SELECT [column_names] FROM [table_name_one]
[join_type] JOIN [table_name_two] ON [join_constraint]
[join_type] JOIN [table_name_three] ON [join_constraint];
```

> Write a query that gathers data about the invoice with an `invoice_id` of 4. Include the following columns in order:
>
> - The id of the track, `track_id`
> - The name of the track, `track_name`
> - The name of media type of the track, `track_type`
> - The price that the customer paid for the track, `unit_price`
> - The quantity of the track that was purchased, `quantity`

```SQL
SELECT 
  il.track_id, 
  t.name track_name, 
  mt.name track_type, 
  t.unit_price, 
  il.quantity 
FROM invoice_line il
INNER JOIN track t ON t.track_id = il.track_id
INNER JOIN media_type mt ON mt.media_type_id = t.media_type_id
WHERE invoice_id = 4;
```

> Add a column containing the artists name to the previous query.
>
> - The column should be called `artist_name`
> - The column should be placed between `track_name` and `track_type`

```SQL
SELECT
    il.track_id,
    t.name track_name,
    ar.name artist_name,
    mt.name track_type,
    il.unit_price,
    il.quantity
FROM invoice_line il
INNER JOIN track t ON t.track_id = il.track_id
INNER JOIN album al ON t.album_id = al.album_id
INNER JOIN artist ar ON al.artist_id = ar.artist_id
INNER JOIN media_type mt ON mt.media_type_id = t.media_type_id
WHERE il.invoice_id = 4;
```

## Combining Multiple Joins with Subqueries

Goal: write a query that lists the top 10 artists, calculated by the number of times a track by that artist has been purchased

Process:

1. Write a subquery that produces a table with `track.track_id` and `artist.name`
2. Join that subquery to the `invoice_line` table
3. Use a `GROUP BY` statement to calculate the number of times each artist has had a track purchased, and find the top 10

1. Subquery

```SQL
SELECT
    t.track_id,
    ar.name artist_name
FROM track t
INNER JOIN album al ON al.album_id = t.album_id
INNER JOIN artist ar ON ar.artist_id = al.artist_id
ORDER BY 1 LIMIT 5;
```

2. Join subquery to `invoice_line` table

```SQL
SELECT
    il.invoice_line_id,
    il.track_id,
    ta.artist_name
FROM invoice_line il
INNER JOIN (
            SELECT
                t.track_id,
                ar.name artist_name
            FROM track t
            INNER JOIN album al ON al.album_id = t.album_id
            INNER JOIN artist ar ON ar.artist_id = al.artist_id
           ) ta
           ON ta.track_id = il.track_id
ORDER BY 1 LIMIT 5;
```

3. `GROUP BY` statement

```SQL
SELECT
    ta.artist_name artist,
    COUNT(*) tracks_purchased
FROM invoice_line il
INNER JOIN (
            SELECT
                t.track_id,
                ar.name artist_name
            FROM track t
            INNER JOIN album al ON al.album_id = t.album_id
            INNER JOIN artist ar ON ar.artist_id = al.artist_id
           ) ta
           ON ta.track_id = il.track_id
GROUP BY 1
ORDER BY 2 DESC LIMIT 10;
```

> Write a query that returns the top 5 albums, as calculated by the number of times a track from that album has been purchased. Your query should be sorted from most tracks purchased to least tracks purchased and return the following columns, in order:
>
> - `album`, the title of the album
> - `artist`, the artist who produced the album
> - `tracks_purchased`, the total number of tracks purchased from that album

```SQL
SELECT
    ta.album_title album,
    ta.artist_name artist,
    COUNT(*) tracks_purchased
FROM invoice_line il
INNER JOIN (
            SELECT
                t.track_id,
                al.title album_title,
                ar.name artist_name
            FROM track t
            INNER JOIN album al ON al.album_id = t.album_id
            INNER JOIN artist ar ON ar.artist_id = al.artist_id
           ) ta
           ON ta.track_id = il.track_id
GROUP BY 1
ORDER BY 3 DESC
LIMIT 5;
```

### Recursive Joins

Example:

```SQL
SELECT
    e1.employee_id,
    e2.employee_id supervisor_id
FROM employee e1
INNER JOIN employee e2 on e1.reports_to = e2.employee_id
LIMIT 4;
```

> Write a query that returns information about each employee and their supervisor
>
> - The report should include employees even if they do not report to another employee
> - The report should be sorted alphabetically by the `employee_name` column
> - Your query should return the following columns, in order:
>   - `employee_name` - containing the `first_name` and `last_name` columns separated by a space (e.g. Luke Skywalker)
>   - `employee_title` - the title of that employee
>   - `supervisor_name` - the first and last name of the person that employee reports to, in the same format as `employee_name`
>   - `supervisor_title` - the title of the person the employee reports to

```SQL
SELECT
    e1.first_name || " " || e1.last_name employee_name,
    e1.title employee_title,
    e2.first_name || " " || e2.last_name supervisor_name,
    e2.title supervisor_title
FROM employee e1
LEFT JOIN employee e2 on e1.reports_to = e2.employee_id
ORDER BY employee_name;
```

### Pattern Matching Using Like

Example:

```SQL
SELECT
    first_name,
    last_name,
    phone
FROM customer
WHERE first_name LIKE "%Jen%";
```

> Write a query that finds the contact details of a customer with a `first_name` containing `Belle` from the database. Your query should include the following columns, in order:
>
> - `first_name`
> - `last_name`
> - `phone`

```SQL
SELECT c.first_name, c.last_name, c.phone
FROM customer c
WHERE first_name LIKE '%Belle%';
```
