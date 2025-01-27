---
title: "SQL"
description: "Part 1 of 1: No Sequel"
date: "2025-01-27"
# image: "deep_learning_model.png"
categories: [InterviewPrep, Software, DataEngineering]
---

# 1. SQL Overview

## 1.1. SQL and RDBMS
Structured Query Language (SQL) is used to manage and query data stored in a **Relational Database Management System (RDMBS)**.
Essentially, put data in tables and get it out again.

An RDMBS organises data into tables with defined schemas (column names and data types).

SQL is a *declarative* language. You specify what you want to happen, not how to achieve it.
The SQL query engine optimises how the query run internally, e.g. what order to execute commands, what indexes to use.


## 1.2. SQL Flavours
There are different "flavours" of SQL. For example, MySQL, PostegreSQL, SQLite, SQL Server.
These are often, incorrectly, used interchangeably with SQL.
SQL is a general, high-level language for querying RDMBSes.

The "flavours" are each a specific RDBMS which you query with the corresponding language.
Postgres is an RDBMS which you can query by writing PostgreSQL.
MySQL is and RDBMS which you can query by writing MySQL.
Etc.
In practice, the difference is pretty minimal. If you can use one, you'll learn the others pretty quickly.


## 1.3. PostgreSQL
We use PostgreSQL and its associated Postgres RDBMS.

Some of the advantages of PostgreSQL:

- **Popularity**: One of the most popular behind MySQL
- **Open Source**: BSD-style license is not too restrictive
- **Extensible**: Postgres has extensions like PostGIS for geospatial data, etc
- **ANSI Compliant**: American National Standards Institute (ANSI) define standards and PostgreSQL mostly conforms to these. One of the least quirky flavours of SQL.


# 2. Basic SQL Commands
## 2.1. SELECT
Use `SELECT` to read specific columns (or all with `*`) from a given table.

```postgresql
SELECT column1, column2
FROM table_name;
```

We can optional use `LIMIT` to return a set number of rows. This can be helpful if we're querying a massive table that might be a huge query.
```postgresql
SELECT *
FROM table_name
LIMIT 10
```

We can use the `AS` keyword to alias a column name.

```postgresql
SELECT column1, column2 AS skibidi  -- using a stupid alias
FROM table_name;
```

We can also add comments with `--` as above.


## 2.2. WHERE
Use `WHERE` to filter the result on a specific condition.
Conditions can be: `=`, `!=`, `<`, `>`, `<=`, `>=`

```postgresql
SELECT column1, column2
FROM table_name
WHERE condition;
```


## 2.3. Combining Conditions
Use logical operators `AND`, `OR`, `NOT` to chain multiple conditions.

```postgresql
SELECT *
FROM table
WHERE condition1
  AND condition2
  AND NOT condition3;
```


## 2.4. BETWEEN
The `BETWEEN` operator can also be used as a condition, and is equivalent to a combination of >= AND <=.
Note that both sides are inclusive.

For example, the following `BETWEEN` condition:
```postgresql
SELECT column1, column2
FROM table_name
WHERE column1 BETWEEN 0 AND 100;
```

is equivalent to

```postgresql
SELECT column1, column2
FROM table_name
WHERE column1 >= 0 AND column2 <= 100;
```


## 2.5. IN
The `IN` operator is another implicit combined condition.
It saves us the hassle of writing out multiple `OR` conditions.

For example, the following `IN` condition:
```postgresql
SELECT column1, column2
FROM table_name
WHERE column1 (1, 2);
```

is equivalent to
```postgresql
SELECT column1, column2
FROM table_name
WHERE column1 = 1 OR column1 = 2;
```


## 2.6. LIKE
The `LIKE` operator is another implicit condition.
Similarly to `IN`, it save us the hassle of writing out multiple `OR` conditions.

It allows us to match patterns using the wildcards `_` (to represent a single character) or
`%` (to represent arbitrary number of characters).

```postgresql
SELECT product_id,
       manufacturer,
       drug
FROM pharmacy_sales
WHERE drug LIKE '%Relief%';
```

We can use multiple underscores to match a specific number of unknown characters.
For example, this will match "a" followed by any 3 characters.
```postgresql
WHERE word LIKE 'a___'
```

The wildcards can be at multiple points in the pattern, e.g.
```postgresql
WHERE word LIKE 'f_c_'
```


## 2.7. ORDER BY

The order of rows saved in the database is not guaranteed.
Executing the same SELECT twice in a row can give a different ordering.

If we want a specific order, we can specify the `ORDER BY` column(s).

```postgresql
SELECT column1, column2
FROM table_name
ORDER BY column1;
```

By default, this is in ascending order (`ASC`).
We can pass `DESC` to instead return items in descending order.
This can be column-specific.

```postgresql
SELECT column1, column2
FROM table_name
ORDER BY column1 ASC, column2 DESC;
```

We can also pass the column numbers rather than names.

```postgresql
SELECT policy_holder_id, call_category, call_received
FROM callers
ORDER BY 1,3 DESC;
```

We can use `ORDER BY` in conjunction with `LIMIT` where we need the top N highest/lowest results.
We can also use `OFFSET` to skip a number of results.

For example, we can skip the first 10 rows and then return the next 5, so the following query returns the 11th-15th ordered results.

```postgresql
SELECT *
FROM callers
ORDER BY call_received DESC
OFFSET 10
LIMIT 5;
```


# 3. Intermediate SQL Commands

## 3.1. Aggregate Functions

We can aggregate data with `SUM`, `MIN`, `MAX`, `AVG`, `COUNT`.

```postgresql
SELECT COUNT(*)
FROM table_name;
```


## 3.2. GROUP BY

The aggregate functions can be run on the entire table as above.
But they come into their own when grouping by particular fields.

We can `GROUP BY` one or more columns.

```postgresql
SELECT
category,
    SUM(spend) AS total_spend
FROM product_spend
GROUP BY category;
```


## 3.3. HAVING

Suppose we want to filter the data on the aggregated value.
For example, in the previous example, we want to only return categories with `total_spend > 10`.

Naively, we might try to use `WHERE`. But `WHERE` filters individual rows.
Trying this will give some variation of the following error message

> aggregate functions are not allowed in WHERE

The `HAVING` clause is essentially the analog of `WHERE`, but operates on grouped data rather than individual rows.

```postgresql
SELECT ticker, AVG(open)
FROM stock_prices
GROUP BY ticker
HAVING AVG(open) > 200;
```

## 3.4. DISTINCT

The `DISTINCT` keyword can specify that only rows where the column(s) are distinct.
If we pass multiple columns, we will get all of the distinct pairs (or tuples in the general case)
of those columns.

```postgresql
SELECT DISTINCT col1, col2
FROM table_name;
```

`DISTINCT` can be combined with aggregate functions, typically `COUNT`.

```postgresql
SELECT COUNT(DISTINCT user_id)
FROM trades;
```


## 3.5. Arithmetic

We can use standard mathematical operations `+`, `-`, `/`, `*`, `^`, `%`

```postgresql
SELECT salary + bonus AS total_compensation
FROM employees;
```

We have the modulus operator `%` which returns the remainder of a division.
This is often helpful in problems where we need to find odd or even values.

```postgresql
SELECT *
FROM measurements
WHERE measurement_num % 2 = 1
```

These operations follow the usual BODMAS rule (or PEMDAS if you're an asshole).



## 3.6. Mathematical Functions

The following built-in maths functions are useful:

- `ABS()` - absolute value
- `CEIL()` - round up
- `FLOOR()` - round down
- `ROUND(column_name, N)` - round to N decimal places
- `POWER(column_name, exponent)` - equivalent to `column_name ^ exponent`
- `MOD(column_name, divisor) - equivalent to `column_name % divisor`


## 3.7. Division

Division is SQL can be deceptively tricky.
Naively, we might think we just do `col1 / col2`, job done.

But in practice, we can get weird results depending on the data types of the numerator or denominator.

| Input           | SQL Output         | Expected     |
|-----------------|--------------------|--------------|
| `SELECT 10/4`   | 2                  | 2.5          |
| `SELECT 10/2`   | 5                  | 5            |
| `SELECT 10/6`   | 1                  | 1.6666666667 |
| `SELECT 10.0/4` | 2.5000000000000000 | 2.5          |
| `SELECT 10/3.0` | 3.3333333333333333 | 3.333333333  |

We can coerce values to floats by:

- Using the `CAST(column_name AS FLOAT)` function
- Multiplying `* 1.0`
- Explicitly using types with `::`

```postgresql
SELECT 
  CAST(10 AS DECIMAL)/4,
  CAST(10 AS FLOAT)/4,
  10 * 1.0 / 4
  10::DECIMAL / 4
```


## 3.8. Nulls

A `NULL` value indicates the **absence of a value**. 
Missing data is different to data which is populated but empty, like a 0 or an empty string.

We can identify null and non-null values with `IS NULL` and `IS NOT NULL`.

```postgresql
SELECT *
FROM goodreads
WHERE book_title IS NULL;
```

The `COALESCE` keyword allows us to pass multiple inputs and return the first non-null value.
We can pass multiple columns, or a mix of columns and a hard-coded default value.
This makes it useful to fill nulls. Think of it like the pandas `fillna` method.

```
SELECT COALESCE(book_rating, 0)  -- fill NULL values with 0
FROM goodreads;
```

We can also use the `IFNULL` keyword to fill null values.


```postgresql
SELECT 
  book_title, 
  IFNULL(book_rating, 0) AS rated_books  - fill NULL values with 0
FROM goodreads;
```

In the above examples, `IFNULL` and `COALESCE` are interchangeable.
In general, use `COALESCE` when checking multiple columns, e.g. `COALESCE(col1, col2, col3)`.
If only checking one column, `IFNULL` is more concise.





<!-- # N+1. Miscellaneous Points

## N+1.1. Order of Execution

The clauses in a SQL query must be written according to the following
[SQL Order of Execution](https://www.datacamp.com/tutorial/sql-order-of-execution). -->


# References

- https://datalemur.com/sql-tutorial
- [SQL Order of Execution](https://www.datacamp.com/tutorial/sql-order-of-execution)
