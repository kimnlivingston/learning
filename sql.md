# Learning SQL

- [Learning SQL](#learning-sql)
  - [Oracle Datatypes](#oracle-datatypes)
    - [Objects](#objects)
    - [Column Names](#column-names)
    - [Data Types](#data-types)
  - [`SELECT` Statement Syntax](#select-statement-syntax)
  - [Expressions](#expressions)
  - [Aliases](#aliases)
  - [Subqueries](#subqueries)
  - [Literals](#literals)
    - [String Literals](#string-literals)
    - [Numeric Literals](#numeric-literals)
    - [Data Literals](#data-literals)
    - [Interval Literals](#interval-literals)
  - [String Manipulation](#string-manipulation)
    - [Concatenation: gluing strings and other things together](#concatenation-gluing-strings-and-other-things-together)
    - [Alternative String Quoting](#alternative-string-quoting)
  - [Filtering Rows](#filtering-rows)
    - [`WHERE` syntax (`WHERE` condition)](#where-syntax-where-condition)
    - [Subqueries in the `WHERE` Clause](#subqueries-in-the-where-clause)
    - [Joining Two Tables is a Type of `WHERE` clause](#joining-two-tables-is-a-type-of-where-clause)
  - [Sorting Rows](#sorting-rows)
    - [`ORDER BY` clause](#order-by-clause)
    - [Collation rules](#collation-rules)
  - [Single-Row Functions](#single-row-functions)
    - [Number](#number)
    - [Character](#character)
    - [Data/Time](#datatime)
    - [Conversion](#conversion)
    - [General](#general)
  - [Aggregation and Multiple-Row Functions](#aggregation-and-multiple-row-functions)
    - [Group Functions (`AVG`, `COUNT`, `MAX`, `MIN`, `STDDEV`, `VARIANCE`, `LISTAGG`)](#group-functions-avg-count-max-min-stddev-variance-listagg)
    - [`HAVING` clause](#having-clause)
    - [Using `DISTINCT`](#using-distinct)
  - [Joining Tables](#joining-tables)
    - [ANSI joins versus Oracle join syntax](#ansi-joins-versus-oracle-join-syntax)
    - [`(INNER) JOIN`](#inner-join)
    - [`LEFT (OUTER) JOIN`](#left-outer-join)
    - [`RIGHT (OUTER) JOIN`](#right-outer-join)
    - [`FULL (OUTER) JOIN`](#full-outer-join)
    - [`NATURAL JOIN`](#natural-join)
    - [`CROSS JOIN`](#cross-join)
  - [Using the Oracle Data Dictionary](#using-the-oracle-data-dictionary)
    - [Understanding the data dictionary](#understanding-the-data-dictionary)
    - [Querying the data dictionary](#querying-the-data-dictionary)
  - [Data Manipulation Language (DML)](#data-manipulation-language-dml)
    - [`INSERT`](#insert)
      - [Reasons for Failed `INSERT`](#reasons-for-failed-insert)
      - [Inserting single rows](#inserting-single-rows)
      - [Inserting multiple rows (three ways outside of PL/SQL)](#inserting-multiple-rows-three-ways-outside-of-plsql)
      - [Use CREATE TABLE (CTAS) with a subquery](#use-create-table-ctas-with-a-subquery)
    - [UPDATE table rows](#update-table-rows)
      - [Failures](#failures)
    - [DELETE rows with or without WHERE](#delete-rows-with-or-without-where)
      - [Failures](#failures-1)
    - [`MERGE`](#merge)
    - [`TRUNCATE` (more of a DDL command)](#truncate-more-of-a-ddl-command)
  - [Data Definition Language (DDL)](#data-definition-language-ddl)
    - [Creating tables](#creating-tables)
    - [Creating indexes](#creating-indexes)
    - [Dropping Objects](#dropping-objects)
      - [Dropping tables](#dropping-tables)
      - [Dropping indexes](#dropping-indexes)

## Oracle Datatypes

### Objects

- owned by a schema, aka a User, and can be manipulated and changed by SQL statements
- stored in table columns

| OBJECT | DEFINITION |
| ------ | ---------- |
| Table | contains rows and columns; where your data lives |
| Index | Provides fast access to rows in a table |
| View | An "on the fly" subset of one or more tables |
| Synonym | an alternate name for a table |
| Sequence | generates a series of numbers |
| Procedures and functions | SQL and PL/SQL code stored in the database |

### Column Names

- Starts with a letter (A-Z)
- 1-128 characters long
- Can only contain letters, digits, and special characters "_", "$", and "#"
- Cannot be an Oracle reserved word such as `WHERE`, `SELECT`, and `BY`
- Column names can be case sensitive but then you always have to enclose them in double quotes when referencing them

### Data Types

| TYPE | DEFINITION |
| ---- | ---------- |
| `NUMBER(p,s)` | numeric data with precision p and scale s |
| `VARCHAR2(length)` | variable length character data (maximum length of 13,767) |
| `DATE` | date + time values, precision to the nearest second |
| `TIMESTAMP`; `TIMESTAMP WITH TIME ZONE`; `TIMESTAMP WITH LOCAL TIME ZONE` | date + time values; precision to the nearest one millionth of a second |
| `CHAR(length)` | fixed-length character data |
| `CLOB` | large-capacity variable-length character data |
| `BLOB` | variable-length binary data |
| `ROWID` | internal pointers to table rows |
| `BFILE` | pointer to binary data stored in the OS |

## `SELECT` Statement Syntax

``` sql
SELECT column1, column2, column3 [, . . .]
FROM table1 [JOIN table2 ON col1 relational_operator col2]
  [. . . JOIN . . .]
WHERE condition_1 [AND | OR] condition_2 [. . .]
GROUP BY col1 [, col2 . . .]
HAVING groupby_expression relational_operator
  [constant | expression]
ORDER BY [column_number | column1 | groupby_expr1], . . .
```

Running a `SELECT` Statement from keyboard
- Press F9 or Ctrl+Enter

## Expressions

- Oracle Database 19c definition: "An arbitrarily complex combination of operands (variables, constants, literals, operators, function invocations, and placeholders) and operators. The simplest expression is a single variable."
- `||` (concat)
- `ORDER BY` (column position = number)

## Aliases

- Synonym for columns
- Enhance readability of query output
- Can be used in the `ORDER BY` clause
- Cannot be used in the `WHERE` clause
- Must be used when creating a new table with data from another table and you have expressions in the query

## Subqueries

- Syntactically follows the rules for a `SELECT` query
- Also known as a nested query
- Can be used almost anywhere a table name is referenced
  - Can replace a table name in the `FROM` clause
  - As a filter in a `WHERE` clause
- Subqueries can be nested up to 255 deep
- A subquery can be correlated or not

## Literals

- AKA constants - fixed data value
- Used by themselves or when constructing expressions
- Used in ORDER BY to reference columns in the `SELECT` clause

### String Literals

- Sequence of zero or more characters  surrounded by 'and'
- To include a single quote as part of a string literal, use two single quotes in a row (e.g. `'Bob''s Burgers'`)

### Numeric Literals

- Integer - no decimal point or exponents
  - Can use in `ORDER BY` for columns (4 would be the fourth column in a table) instead of column name
- Number - integer + decimal point
- Floating Point - decimal point and E to indicate scientific notation

### Data Literals

- `DATE`: date + time values, precision to the nearest second
- `TIMESTAMP`: date + time values; precision to the nearest one millionth of a second
  - `TIMESTAMP WITH TIME ZONE`
  - `TIMESTAMP WITH LOCAL TIME ZONE`

### Interval Literals

- An interval is a time period, not an absolute time
- Two interval types
  - `INTERVAL 'number' [YEAR | MONTH] TO [YEAR | MONTH]`
  - `INTERVAL 'number' [DAY | HOUR | MINUTE | SECOND] TO [DAY | HOUR | MINUTE | SECOND]`

## String Manipulation

### Concatenation: gluing strings and other things together

- Output is always a sting
- To use:
  - Two vertical bars || (ANSI)
  - Built-in function CONCAT(char1,char2)

### Alternative String Quoting

- Option 1: embedded quotes needs to be doubled
- Option 2: use a quote delimiter
  - Upper or lowercase Q plus a single quote character'
  - A quote delimiter, one of these characters: [, {, <, or (
  - The closing single quote character: '
  - Example: q'{ update employees where last_name != 'King'}';

## Filtering Rows

### `WHERE` syntax (`WHERE` condition)

- Comparison (>, <, !=, =, etc.)
- Logical (AND, OR, and NOT) (LIKE)
- `NULL`
- `BETWEEN`
- `EXISTS` Bullet point
- `IN`
- Compound
- Pattern matching

### Subqueries in the `WHERE` Clause

- `EXISTS`: Checks for the existence of one of more rows
- `IN`: Checks one or more columns against columns returned from the subquery
  - Use when you know you're comparing specific columns
- Both can be correlated or not: There are one or more columns in the main part of the `WHERE` compared to one of more columns in the subquery

### Joining Two Tables is a Type of `WHERE` clause

- Use the `DEPARTMENT_ID` column
- join `EMPLOYEES` and `DEPARTMENTS` using these methods
  - Only the FROM clause
    ``` sql
    select employee_id,last_name,department_id,department_name
    from employees
    join departments
      using (department_id);
    ```

  - Only the `WHERE` clause
    ``` sql
    select employee_id,last_name,d.department_id,department_name
    from employees e, departments d
    where e.department_id = de.department_id;
    ```

  - Even the `SELECT` statement
    ``` sql
    select employee_id,
          last_name,
          department_id,
          (select department_name from departments d
          where e.department_id=d.department_id) department_name
    from employees e
    where department_id is not null;
    ```

  - Outer joins: What if someone doesn't have a `DEPARTMENT_ID`?
    ``` sql
    select employee_id,last_name,department_id,department_name
    from employees
    left join departments
      using (department_id);
    ```
    ---OR---
    ``` sql
    select employee_id,last_name,department_id,department_name
    from departments
    right join employees
      using (department_id);
    ```

## Sorting Rows

### `ORDER BY` clause

- Should always go to the end
- Should always use one
- Should rarely need the other clauses
- Set operators (`UNION`, `UNION ALL`, `MINUS`, `INTERSECT`) force an internal `ORDER BY`, but it usually does not make sense
- The `ORDER BY` clause can be used with expressions, columns, or a special case where a numeric constant means something quite different

### Collation rules

- A collation is the way that strings are sorted with ORDER BY
- An algorithm defines what the "collation order" is
- Two parts of collations
  - Types
    - Binary: based on numeric values in the string
    - Linguistic: Ordering and compare based on the sequence of characters
  - Suffixes
    - _CI: Case insensitive, accent sensitive
    - _AI: Case and account insensitive
    - _CS: Both case and accent insensitive; this is the default
- Leave it at binary unless you know otherwise
- Use CI only at the table or column level
- Collation can be set at almost every database level
- Use functions like `COLLATION` to query the collation status of an expression or column

## Single-Row Functions

- Can be found in `SELECT`, `WHERE`, `START WITH/CONNECT BY`, `GROUP BY`, `HAVING` (after an explicit or implicit `GROUP BY`)
- Returns a single row or column value for every row in the query
- `NULL` values are handled in single-row numeric functions like they are in all functions; `NULL` arguments return a `NULL`

### Number

- Almost half of the single-row numeric functions are trigonometry related
- All of them return an integer, decimal number with decimal point, or a floating-point number as a result of the function

### Character

- Returns character values: Spilt into general functions, pattern matching, and language support functions
- Returns numeric values: return a number or `NULL` (only a small number)

### Data/Time

- Either return dates, time intervals, or non-date values either as a `NUMBER` or `VARCHAR2`

### Conversion

- Most of the single-row conversion functions convert the operand to a character string
- All of the convert one data type to another as a result

### General

- Most of the single-row general functions handle sets of data and unstructured data
- All of them transform or format one data type to another as a result of the function

## Aggregation and Multiple-Row Functions

### Group Functions (`AVG`, `COUNT`, `MAX`, `MIN`, `STDDEV`, `VARIANCE`, `LISTAGG`)

- `GROUP BY` rolls up individual rows into a single result set
- Ensure all columns in `GROUP BY` exist in the `SELECT`
- There will be as many result rows as there are combinations of column values in the `GROUP BY` clause
- You can put multiple multi-row functions in the same `SELECT`
- The column list in the `GROUP BY` is a subset or all of the non-aggregated values in the `SELECT` statement

### `HAVING` clause

- The `HAVING` clause is optional and sometimes not allowed
- Using a `HAVING` clause with `GROUP BY` is like having a `WHERE` clause with your `SELECT` statement
- The `HAVING` clause helps you manage the size of your `GROUP BY` results

### Using `DISTINCT`

- Can be used with single- and multi-row functions  by filtering rows before the function is applied
- Removes duplicates from a set of rows and columns
- Used in functions such as `APPROX_COUNT_DISTINCT`
- Restrictions on how or where you use:
  - Cannot use with LOBs
  - Not allowed with FOR `UPDATE` in a `SELECT`
  - The column list or expression in `SELECT . . . DISTINCT` must fit into one database block

## Joining Tables

### ANSI joins versus Oracle join syntax

- Both types of join syntax are supported
- You can mix and match both join syntaxes in the same statement
- With ANSI joins, all join login is in the `JOIN` clause; filtering logic is in the `WHERE` clause
- ANSI syntax requires fewer aliases and is easier to maintain
- ANSI join syntax: Auses the FROM clause for listing the tables and specifying the column or columns to join
  ``` sql
  select
  employee_id,last_name,department_id
  from employees
  join departments
    using (department_id)
  where
  employee_id = 101;
  ```

- Oracle join syntax: moves the join conditions to the `WHERE `clause
  ``` sql
  select
  employee_id,last_name,d.department_id
  from employees e, deparments d
  where
  employee_id = 101
  and
  e.department_id=d.department_id;
  ```

### `(INNER) JOIN`

- Only returns rows from the two tables if the common column is equal and may exclude rows from either table

### `LEFT (OUTER) JOIN`

- Returns all rows from the first table and when the common column is equal will populate from the second table
- Oracle syntax requires a (+) on the table where rows can be excluded

### `RIGHT (OUTER) JOIN`

- Return all rows from the second table and when the common column is equal will populate columns from the second table
- Oracle syntax requires a (+) on the table where rows can be excluded

### `FULL (OUTER) JOIN`

- Returns all rows from the first table and the second table and when the common column is equal will populate columns from both tables
- Oracle syntax requires a `LEFT JOIN` and a `RIGHT JOIN` with a `UNION ALL`

### `NATURAL JOIN`

- Returns columns from both tables when the common column is equal
- There is no Oracle-specific syntax when you don't want to specify the join columns and let the query engine join the tables
- Incorrect results may happen if a column is added to one table and it matches an unrelated column with the same name in another table

### `CROSS JOIN`

- Returns columns from both tables; the number of rows returned is the product of the number of rows in each table
- The Oracle-specific syntax achieves a `CROSS JOIN` by leaving out the column comparisons in the `WHERE` clause
- Rarely used, given the number of rows that may not get returned

## Using the Oracle Data Dictionary

### Understanding the data dictionary

- A repository of all object metadata
- Dynamic data in V$ views; permanent views retain and maintain data after system reboot
- Supplied scripts are run at database creation or when needed
- Direct or indirect privileges are required to access data dictionary views

### Querying the data dictionary

- The permanent views that have a public synonym and the V$ views are considered the data dictionary
- Views starting with `USER_` show owner objects, `ALL_ `show objects with granted permissions, and `DBA_` shows all objects
- Common V$ views are `V$INSTANCE` and `V$PDBS`
- The get metadata about table attributes, you can use SQL*Plus, SQLcl, SQL Developer, or write a query against `BDA_TAB_COLUMNS`

## Data Manipulation Language (DML)

- Use DML to make changes to a table
- Can use DML with one row, some rows, or all rows in one statement

### `INSERT`

- Specify column names and values in the same order but they don't have to be in the order of the columns in the table
- If you supply all column values, you don't have to specify the column list
- Use single quotes for string column; ideally TO_DATE for date values that are in single quotes
- You can leave off the column names and associated values for a column if the remaining columns are NULLable or they have a default defined in the table itself

#### Reasons for Failed `INSERT`

- Number too large for a column
- Character string too wide
- Providing a `NULL` value for a NOT `NULL` column
- Inserting a value that violates a foreign key constraint

#### Inserting single rows

``` sql
INSERT INTO [schema_name.]table_name [(col1[,col2 . . .])]
VALUES      (value1[,value3 . . .]);
```

- table_name is the place where your new rows will be inserted
- Provide a schema_name if you are inserting into a table you do not own
- Specify one or more column names—col1, col2, etc.—in the table
- Use the `VALUES` clause to specify the actual data for each column
- If you omit the column values, all columns in the table are assumed to be provided
- If you specify only certain columns, they must provide and the remaining columns must be NULLable

#### Inserting multiple rows (three ways outside of PL/SQL)
- Use `INSERT` with a SELECT subquery that can contain multiple rows
  ``` sql
  INSERT INTO [schema_name.]table_name [(col1,col2 . . .])]
  (SELECT (col1[,col2, . . .])]
    FROM table1[, join_condition table2 . . .]);
  ```

- Use `INSERT ALL` or `FIRST` to insert multiple rows into multiple tables
  - The `INSERT ALL` can inset into one, two, or many tables
  - If you specify `FIRST`, the `INSERT` stops for that row at that first table
  - `INSERT`s are only into a table, but it can be an IOT
  - Tables can't be over a DB link
  - `INSERT ALL` will take all rows in the specified subquery and insert them into each table specified in the INTO clauses
  - `INSERT FIRST` has a similar structure to `INSERT ALL` bust has conditional logic to insert into one or a subset of the tables in the `INTO` clauses

#### Use CREATE TABLE (CTAS) with a subquery

``` sql
CREATE TABLE [schema_name.]table_name AS
SELECT [col1,[col2 . . .])]
FROM table1[, join_condition table2 . . .];
```

- The fastest way to insert but uses DDL instead of DML

### UPDATE table rows

``` sql
UPDATE [schema_name.]table_name
SET col1=val1[,col2=val2 . . .]
WHERE condition;
```

- `UPDATE` with no `WHERE` updates all rows
- Use `SET` to change one or more column values
- The `WHERE` clause can have any expression including a correlated subquery

#### Failures

- Incorrect privileges on the table to be updated
- Updated value in one or more rows has  column protected by a primary key constraint
- Updated value in one or more rows has a column protected by a foreign key constraint in another table
- Trying to update a column that's defined as `NOT NULL` with a `NULL`

### DELETE rows with or without WHERE

#### Failures

- Incorrect privileges on the table to be updated
- Trying to delete a row that is part of a foreign key constraint validating column values in another table
- Attempting to delete a row that doesn't exist
- Running `DELETE` with no `WHERE` condition

``` sql
DELETE [FROM] [schema_name.]table_name
[WHERE condition];
```

### `MERGE`

combo of `INSERT`, `UPDATE`, and `DELETE`

### `TRUNCATE` (more of a DDL command)

- Deletes all rows in the table quickly
- Implicitly runs a `COMMIT` before truncation occurs
- DDL Statement; can't use ROLLBACK for recovery of rows
- Storage can stay allocated for future INSERTs
- The `CASCADE` option automatically disables all FK constraints using the table

``` sql
TRUNCATE TABLE [schema_name.]table_name
[{DROP | REUSE} STORAGE] [CASCADE];
```

## Data Definition Language (DDL)

### Creating tables

- Specify schema if you're creating the table in another user's schema
- Add the `DEFAULT` clause if you want to specify a value when one is not provided by the `INSERT` statement

``` sql
CREATE [schema.]table
  (column1 datatype [DEFAULT expr]
  [, column2 datatype [DEFAULT expr]
  [, . . .]);
```

### Creating indexes

- Specify the type of index: B-Tree (default), `UNIQUE`, or `BITMAP`
- Specify the index's schema if you're creating the index in another user's schema
- Specify the table's schema if you're creating the index on a table in another schema
- Provide a column list of the columns to be indexed in the table

``` sql
CREATE [indextype] INDEX [schema.]indexname
ON [schema.]tablename
  {column1[, column2][, . . .]);
```

### Dropping Objects

#### Dropping tables

- Specify the schema name if you're dropping the table in a different user's schema
- Use `CASCADE CONSTRAINTS` to drop all referential integrity constraints referring to this table
- Use `PURGE` if you want to free up space occupied by the table immediately

``` sql
DROP [schema.]table
[CASCADE CONSTRAINTS]
[PURGE];
```

#### Dropping indexes

- Specify the schema name if you're dropping the index in a different user's schema
- Use `ONLINE` if you want to allow DML to occur on the table while the index is being dropped

``` sql
DROP INDEX [schema.]index [ONLINE];
```
