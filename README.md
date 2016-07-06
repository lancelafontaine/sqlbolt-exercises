# Introduction to SQL Exercises

Exercises completed at http://sqlbolt.com/ between July 6th and 8th 2016.

### Lesson 1: SELECT queries 101
```
SELECT title FROM movies;
SELECT director FROM movies;
SELECT title, director FROM movies;
SELECT title, year FROM movies;
SELECT * FROM movies;
```

### Lesson 2: Queries with constraints (Pt.1)

```
SELECT * FROM movies WHERE id = 6;
SELECT * FROM movies WHERE year BETWEEN 2000 and 2010;
SELECT * FROM movies WHERE year NOT BETWEEN 2000 and 2010;
SELECT * FROM movies WHERE id BETWEEN 1 AND 5;
```

There is also the `IN` and `NOT IN` operators which operate on a list like `(2,4,6)`.

### Lesson 3: Queries with constraints (Pt.2)

Here are some string comparison and pattern matching operators.

|Operator | Condition |Example
| :---- | :----: | :----|
|`=` | Case sensitive exact string comparison (notice the single equals) | `col_name = "abc"` |
|`!= or <>` | Case sensitive exact string inequality comparison | `col_name != "abcd"`|
|`LIKE` | Case insensitive exact string comparison | `col_name LIKE "ABC"`|
|`NOT LIKE` | Case insensitive exact string inequality comparison | `col_name NOT LIKE "ABCD"`|
|`%` | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | `col_name LIKE "%AT%"` (matches "AT", "ATTIC", "CAT" or even "BATS") |
|`_` | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | `col_name LIKE "AN_"` (matches "AND", but not "AN")|
|`IN (…)` | String exists in a list | `col_name IN ("A", "B", "C")`|
|`NOT IN (…)` | String does not exist in a list | `col_name NOT IN ("D", "E", "F")`|





