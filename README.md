# Introduction to SQL Exercises

Exercises completed at http://sqlbolt.com/ between July 6th and 8th 2016.

### Lesson 1: SELECT queries 101

**ANSWERS:**
```
SELECT title FROM movies;
SELECT director FROM movies;
SELECT title, director FROM movies;
SELECT title, year FROM movies;
SELECT * FROM movies;
```

### Lesson 2: Queries with constraints (Pt.1)

**ANSWERS:**
```
SELECT * FROM movies WHERE id = 6;
SELECT * FROM movies WHERE year BETWEEN 2000 and 2010;
SELECT * FROM movies WHERE year NOT BETWEEN 2000 and 2010;
SELECT * FROM movies WHERE id BETWEEN 1 AND 5;
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
SELECT * FROM movies WHERE title LIKE "%Toy Story%";
SELECT * FROM movies WHERE director = "John Lasseter";
SELECT * FROM movies WHERE director != "John Lasseter";
SELECT * FROM movies WHERE title LIKE "WALL-%";
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
SELECT DISTINCT director FROM movies ORDER BY director ASC;
SELECT * FROM movies ORDER BY year DESC LIMIT 4;
SELECT * FROM movies ORDER BY title ASC LIMIT 5;
SELECT * FROM movies ORDER BY title ASC LIMIT 5 OFFSET 5;
```

### LESSON 5: SQL Review: Simple SELECT queries

```
SELECT country, population FROM north_american_cities WHERE country = "Canada" ;
SELECT city FROM north_american_cities WHERE country = "United States" ORDER BY latitude DESC;
SELECT city FROM north_american_cities WHERE longitude < (SELECT longitude FROM north_american_cities WHERE city = "Chicago") ORDER BY longitude ASC;
SELECT city FROM north_american_cities WHERE country = "Mexico" ORDER BY population DESC LIMIT 2;
SELECT city, population FROM north_american_cities WHERE country = "United States" ORDER BY population DESC LIMIT 2 OFFSET 2;
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
SELECT title, domestic_sales, international_sales FROM movies INNER JOIN boxoffice ON movies.id = boxoffice.movie_id;
SELECT * FROM movies INNER JOIN boxoffice ON movies.id = boxoffice.movie_id WHERE international_sales > domestic_sales;
SELECT title, rating FROM movies INNER JOIN boxoffice ON movies.id = boxoffice.movie_id ORDER BY rating DESC;
```












