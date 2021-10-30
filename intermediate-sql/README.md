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

### Generating Columns with the `CASE` Statement

```SQL
CASE
    WHEN [comparison_1] THEN [value_1]
    WHEN [comparison_2] THEN [value_2]
    ELSE [value_3]
    END
    AS [new_column_name]
```

Example: let's look at how we can use `CASE` to add a new column to `protected`, which indicates whether each media type is protected

```SQL
SELECT
    media_type_id,
    name,
    CASE
        WHEN name LIKE '%Protected%' THEN 1
        ELSE 0
        END
        AS protected
FROM media_type;
```

> Write a query that summarizes the purchases of each customer. Assume we do not have any two customers with the same name. Your query should include the following columns, in order:
>
> - `customer_name` - containing the `first_name` and `last_name` columns separated by a space (e.g. Luke Skywalker)
> - `number_of_purchases` - counting the number of purchases made by each customer
> - `total_spent` - the total sum of money spent by each customer
> - `customer_category` - a column that categorizes the customer based on their total purchases
>   - `small spender` - if the customer's total purchases are less than $40
>   - `big spender` - if the customer's total purchases are greater than $100
>   - `regular` - if the customer's total purchases are between $40 and $100 (inclusive)
> - Order your results by the `customer_name` column

```SQL
SELECT 
    c.first_name || " " || c.last_name customer_name,
    COUNT(i.invoice_id) number_of_purchases,
    SUM(i.total) total_spent,
    CASE
        WHEN SUM(i.total) < 40 THEN 'small spender'
        WHEN SUM(i.total) > 100 THEN 'big spender'
        ELSE 'regular'
        END
        AS customer_category
FROM invoice i
INNER JOIN customer c
    ON i.customer_id = c.customer_id
GROUP BY i.customer_id
ORDER BY customer_name
```

## Building and Organizing Complex Queries

> "Even if you don't intend anybody else to read your code, there's still a very good chance that somebody will have to stare at your code and figure out what it does: That person is probably going to be you, twelve months from now."
>
> --Raymond Chen

### Formatting Tips

- if a select statement has more than one column, put each on a new line, indented from the select statement
- always capitalize SQL functoin names and keywords
- put each clause of your query on a new line
- Use indenting to make subqueries appear logically separate

### The With Clause

```SQL
WITH [alias_name] AS ([subquery])

SELECT [main_query]
```

Example: **no** `WITH` clause

```SQL
SELECT * FROM
    (
     SELECT
         t.name,
         ar.name artist,
         al.title album_name,
         mt.name media_type,
         g.name genre,
         t.milliseconds length_milliseconds
     FROM track t
     INNER JOIN media_type mt ON mt.media_type_id = t.media_type_id
     INNER JOIN genre g ON g.genre_id = t.genre_id
     INNER JOIN album al ON al.album_id = t.album_id
     INNER JOIN artist ar ON ar.artist_id = al.artist_id
    )
WHERE album_name = "Jagged Little Pill";
```

Example: `WITH` clause

```SQL
WITH track_info AS
    (                
     SELECT
         t.name,
         ar.name artist,
         al.title album_name,
         mt.name media_type,
         g.name genre,
         t.milliseconds length_milliseconds
     FROM track t
     INNER JOIN media_type mt ON mt.media_type_id = t.media_type_id
     INNER JOIN genre g ON g.genre_id = t.genre_id
     INNER JOIN album al ON al.album_id = t.album_id
     INNER JOIN artist ar ON ar.artist_id = al.artist_id
    )

SELECT * FROM track_info
WHERE album_name = "Jagged Little Pill";
```

> Create a query that shows summary data for every playlist in the Chinook database
>
> - Use a `WITH` clause to create a named subquery with the following info:
>   - The unique ID for the playlist
>   - The name of the playlist
>   - The name of each track from the playlist
>   - The length of each track in seconds
> - Your final table should have the following columns, in order:
>   - `playlist_id` - the unique ID for the playlist
>   - `playlist_name` - the name of the playlist
>   - `number_of_tracks` - a count of the number of tracks in the playlist
>   - `length_seconds` - the sum of the length of the playlist in seconds
> - The results should be sorted by `playlist_id` in ascending order

```SQL
WITH track_info AS
    (                
     SELECT
         t.name,
         ar.name artist,
         al.title album_name,
         mt.name media_type,
         g.name genre,
         t.milliseconds length_milliseconds
     FROM track t
     INNER JOIN media_type mt ON mt.media_type_id = t.media_type_id
     INNER JOIN genre g ON g.genre_id = t.genre_id
     INNER JOIN album al ON al.album_id = t.album_id
     INNER JOIN artist ar ON ar.artist_id = al.artist_id
    )

SELECT * FROM track_info
WHERE album_name = "Jagged Little Pill";
```

### Creating Views

View = permanently defining a subquery that we can use again and again

syntax for creating a view:

```SQL
CREATE VIEW database.view_name AS
    SELECT * FROM database.table;
```

```SQL
CREATE VIEW chinook.customer_2 AS
    SELECT * FROM chinook.customer;
```

to redefine a view, first we have to delete / drop it:

```SQL
DROP VIEW chinook.customer_2;
```

> Create a view called `customer_gt_90_dollars`
>
> - The view should contain the columns from `customer`, in their original order
> - The view should contain only customers who have purchased more than $90 in tracks from the store
> After the SQL query creates the view, write a second query to display our newly created view

```SQL
DROP VIEW IF EXISTS chinook.customer_gt_90_dollars;
CREATE VIEW chinook.customer_gt_90_dollars AS 
     SELECT c.*
     FROM chinook.invoice i
     INNER JOIN chinook.customer c ON i.customer_id = c.customer_id
     GROUP BY 1
     HAVING SUM(i.total) > 90;
SELECT * FROM chinook.customer_gt_90_dollars;
```

### Combining Rows with Union

> Use `UNION` to produce a table of customers in the USA *or* have spent more than $90, using the `customer_usa` and `customer_gt_90_dollars` views

```SQL
SELECT * FROM chinook.customer_usa
UNION
SELECT * FROM chinook.customer_gt_90_dollars;
```

### Combining Rows Using Intersect and Except

| Operator | What it does | Python Equivalent |
| --- | --- | --- |
| `UNION` | Select rows that occur in *either* statement | `or` |
| `INTERSECT` | Select rows that occur in *both* statements | `and` |
| `EXCEPT` | Select rows that occur in the first statement, but don't occur in the second statement | `and not` |

Customers who are in the USA **and** have spent more than $90

```SQL
SELECT * from customer_usa
INTERSECT
SELECT * from customer_gt_90_dollars;
```

Customers who are in the USA **and** have **not** spent more than $90

```
SELECT * from customer_usa
EXCEPT
SELECT * from customer_gt_90_dollars;
```
