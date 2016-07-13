# Introduction to SQL Exercises

Exercises completed at http://sqlbolt.com/ between July 6th and 8th 2016.

### Lesson 1: SELECT queries 101

**ANSWERS:**
```
SELECT title 
FROM movies;


SELECT director
FROM movies;


SELECT title, director
FROM movies;


SELECT title, year
FROM movies;


SELECT *
FROM movies;
```

### Lesson 2: Queries with constraints (Pt.1)

**ANSWERS:**
```
SELECT * 
FROM movies
WHERE id = 6;


SELECT *
FROM movies 
WHERE year BETWEEN 2000 and 2010;


SELECT *
FROM movies
WHERE year NOT BETWEEN 2000 and 2010;


SELECT *
FROM movies
WHERE id BETWEEN 1 AND 5;
```

There is also the `IN` and `NOT IN` operators which operate on a list like `(2,4,6)`.

### Lesson 3: Queries with constraints (Pt.2)

Here are some string comparison and pattern matching operators.

|Operator | Condition |Example|
| :---- | :----: | :----|
|`=` | Case sensitive exact string comparison (notice the single equals) | `col_name = "abc"` |
|`!= or <>` | Case sensitive exact string inequality comparison | `col_name != "abcd"`|
|`LIKE` | Case insensitive exact string comparison | `col_name LIKE "ABC"`|
|`NOT LIKE` | Case insensitive exact string inequality comparison | `col_name NOT LIKE "ABCD"`|
|`%` | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | `col_name LIKE "%AT%"` (matches "AT", "ATTIC", "CAT" or even "BATS") |
|`_` | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | `col_name LIKE "AN_"` (matches "AND", but not "AN")|
|`IN (…)` | String exists in a list | `col_name IN ("A", "B", "C")`|
|`NOT IN (…)` | String does not exist in a list | `col_name NOT IN ("D", "E", "F")`|

**ANSWERS:**
```
SELECT *
FROM movies
WHERE title LIKE "%Toy Story%";


SELECT *
FROM movies
WHERE director = "John Lasseter";


SELECT *
FROM movies
WHERE director != "John Lasseter";


SELECT *
FROM movies
WHERE title LIKE "WALL-%";
```

### Lesson 4: Filtering and sorting query results

Even though the data in a database may be unique, the results of any particular query may not be – take our Movies table for example, many different movies can be released the same year. In such cases, SQL provides a convenient way to discard rows that have any duplicate column value by using the DISTINCT keyword.

```
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);
```

SQL provides a way to sort your results by a given column in ascending or descending order using the ORDER BY clause.

```
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
```

The LIMIT will reduce the number of rows to return, and the optional OFFSET will specify where to begin counting the number rows from.

```
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

**ANSWERS**
```
SELECT DISTINCT director 
FROM movies
ORDER BY director ASC;


SELECT *
FROM movies
ORDER BY year DESC
LIMIT 4;


SELECT *
FROM movies
ORDER BY title ASC
LIMIT 5;


SELECT *
FROM movies
ORDER BY title ASC
LIMIT 5
OFFSET 5;
```

### LESSON 5: SQL Review: Simple SELECT queries

```
SELECT country, population 
FROM north_american_cities
WHERE country = "Canada";


SELECT city
FROM north_american_cities
WHERE country = "United States"
ORDER BY latitude DESC;


SELECT city
FROM north_american_cities
WHERE longitude < (
	SELECT longitude
	FROM north_american_cities
	WHERE city = "Chicago"
)
ORDER BY longitude ASC;


SELECT city
FROM north_american_cities
WHERE country = "Mexico" 
ORDER BY population DESC
LIMIT 2;


SELECT city, population
FROM north_american_cities
WHERE country = "United States"
ORDER BY population DESC
LIMIT 2
OFFSET 2;
```

### LESSON 6: Multi-table queries with JOINs

Entity data in the real world is often broken down into pieces and stored across multiple orthogonal tables using a process known as normalization. Database normalization is useful because it minimizes duplicate data in any single table, and allows for data in the database to grow independently of each other (ie. Types of car engines can grow independent of each type of car).

The INNER JOIN is a process that matches rows from the first table and the second table which have the same key (as defined by the ON constraint) to create a result row with the combined columns from both tables. After the tables are joined, the other clauses we learned previously are then applied.

```
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

**ANSWERS:**
```
SELECT title, domestic_sales, international_sales
FROM movies
INNER JOIN boxoffice
	ON movies.id = boxoffice.movie_id;


SELECT *
FROM movies
INNER JOIN boxoffice
	ON movies.id = boxoffice.movie_id
WHERE international_sales > domestic_sales;


SELECT title, rating
FROM movies
INNER JOIN boxoffice
	ON movies.id = boxoffice.movie_id
ORDER BY rating DESC;
```

### LESSON 7: OUTER JOINs

If the two tables have asymmetric data, which can easily happen when data is entered in different stages, then we would have to use a LEFT JOIN, RIGHT JOIN or FULL JOIN instead to ensure that the data you need is not left out of the results.

```
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

Note that the OUTER keyword is optional (LEFT OUTER JOIN === LEFT JOIN).


**ANSWERS:**
```
SELECT DISTINCT building_name
FROM buildings
LEFT JOIN employees
	ON buildings.building_name = employees.building
WHERE building IS NOT NULL;


SELECT *
FROM buildings;


SELECT DISTINCT building_name, role
FROM buildings
LEFT JOIN employees
	ON buildings.building_name = employees.building;
```

### LESSON 8: A short note on NULLs

**ANSWERS:**
```
SELECT *
FROM employees
LEFT JOIN buildings
	ON employees.building = buildings.building_name
WHERE building_name IS NULL;


SELECT *
FROM buildings
LEFT JOIN employees
	ON buildings.building_name = employees.building
WHERE role IS NULL;
```
### LESSON 9: Queries with expressions

An example of a query with expressions (using basic mathematical/string functions to transform values when the query is executed).

```
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```

or

```
SELECT column AS better_column_name, …
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales
  ON mywidgets.id = widget_sales.widget_id;
```

**ANSWERS:**
```
SELECT
	title,
	(domestic_sales + international_sales)/1000000 AS sales
FROM boxoffice AS b
INNER JOIN movies AS m
	ON m.id = b.movie_id;


SELECT
	title,
	rating*10 AS ratings_percent
FROM boxoffice AS b
INNER JOIN movies AS m
	ON m.id = b.movie_id;


SELECT title, year
FROM boxoffice AS b
INNER JOIN movies AS m
	ON m.id = b.movie_id
WHERE year % 2 = 0
ORDER BY year ASC;
```

### LESSON 10: Queries with aggregates (Pt.1)

```
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column;
```


|Function | Description |
| :---- | :----: |
|`COUNT(*)` <br/> `COUNT(column)` | A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column.|
|`MIN(column)` | Finds the smallest numerical value in the specified column for all rows in the group.|
|`MAX(column)` | Finds the largest numerical value in the specified column for all rows in the group.|
|`AVG(column)` | Finds the average numerical value in the specified column for all rows in the group.|
|`SUM(column)` | Finds the sum of all numerical values in the specified column for the rows in the group.|


**ANSWERS:**
```
SELECT MAX(years_employed) as longest_years
FROM employees;


SELECT
    role,
    AVG(years_employed)AS average_years_employed
FROM employees
GROUP BY role;


SELECT
    building,
    SUM(years_employed)AS sum_of_years_employed
FROM employees
GROUP BY building;
```

### LESSON 11: Queries with aggregates (Pt.2)
```
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```
**ANSWERS:**
```
SELECT COUNT(role)
FROM employees
WHERE role = 'Artist';

SELECT
    role,
    COUNT(name) AS number_of_employees
FROM employees
GROUP BY role;

SELECT
	role,
	SUM(years_employed)
FROM employees
GROUP BY role
HAVING role = "Engineer";
```

### LESSON 12: Order of execution of a query

The order of execution of different parts of a query is:
1. `FROM` and `JOIN`s
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `DISTINCT`
7. `ORDER BY`
8. `LIMIT` and `OFFSET`

**ANSWERS:**
```
SELECT
	director,
	COUNT(*)
FROM movies
GROUP BY director;


SELECT
    director,
    SUM(domestic_sales + international_sales) AS total_sales 
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id
GROUP BY director;
```

### LESSON 13: Inserting rows

### LESSON 14: Updating rows

### LESSON 15: Deleting rows

### LESSON 16: Creating tables

```
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```

### LESSON 17: Altering tables

### LESSON 18: Dropping tables














































