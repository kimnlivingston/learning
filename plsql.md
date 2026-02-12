# Learning PL/SQL

- [Learning PL/SQL](#learning-plsql)
  - [Overview](#overview)
  - [Procedures and Functions](#procedures-and-functions)
    - [Creating functions](#creating-functions)
    - [Creating procedures](#creating-procedures)
    - [NULL-Aware Functions](#null-aware-functions)
  - [Declaration Section](#declaration-section)
    - [Declaring variables](#declaring-variables)
    - [Declaring cursors](#declaring-cursors)
  - [Security Model](#security-model)
    - [Invoker rights versus definer rights](#invoker-rights-versus-definer-rights)
    - [How blocks define scope](#how-blocks-define-scope)
  - [Conditional Statements](#conditional-statements)
    - [`LOOP`](#loop)
    - [`CONTINUE` and `EXIT`](#continue-and-exit)
    - [`FOR` loop](#for-loop)
    - [`WHILE` loop](#while-loop)
    - [`IF` Statement](#if-statement)
  - [Using SQL `SELECT` Statements](#using-sql-select-statements)
    - [`SELECT INTO` and `%ROWCOUNT`](#select-into-and-rowcount)
    - [Running SQL statements: Static](#running-sql-statements-static)
    - [Running SQL statements: Dynamic](#running-sql-statements-dynamic)
  - [Error Handling](#error-handling)
    - [With `EXCEPTION`](#with-exception)
    - [User-defined exception types](#user-defined-exception-types)
  - [Leveraging User-Defined Types](#leveraging-user-defined-types)
    - [Declaring and using user-defined types](#declaring-and-using-user-defined-types)
    - [Using type with `TABLE` functions](#using-type-with-table-functions)

## Overview

- A full 3GL programming language
- Integrates tightly with Oracle SQL statements
  - All data types are interchangeable
  - Transparently uses declared data types with `%TYPE` and `%ROWTYPE`
- Process rows one at a time if desired
- Integrate PL/SQL functions in `WITH` clause
- Support static and dynamic SQL statements

## Procedures and Functions

- Procedure: process zero or more arguments in a stored block of code
- Function: process zero or more variable and return a variable of any data type
- Built-in PL/SQL procedures and functions
  - Refer to the Basic SQL section "Single Row Functions"
  - Use built-in functions in `SELECT` statements
  - Built-in functions are categorized into what they return: strings, numbers, or converted values
  - Built-in PL/SQL procedures are generally delivered in a package

### Creating functions

``` sql
create or replace function
  new_salary(current_salary number, department_id number)
  return number is
  working_salary number;
begin
  working_salary := current_salary * 1.2;
  if deparment_id = 101 then
    working_salary := working_salary * 1.05;
  end if;
  return working_salary
end;
/

update employee
set salary = new_salary(salary,department_id);
```

- Name, data types, and names of parameters, data type of return value
- Declaration section for intermediate variables
- Function body with a `RETURN` at the end

### Creating procedures

``` sql
[declare
-- declaration #1
-- declaration #2
-- declaration #n]
begin
  -- statement #1
  -- statement #n
  return;
  [exception
    -- statements to run when errors occur]
end;
/
```

- Name, data types, and names of parameters and I/O type
- Declaration section for intermediate variables
- Procedure body with a `RETURN` at the end
- Use `EXEC` to call a procedure in a single line from SQL Developer

### NULL-Aware Functions

- `NVL(expr1, expr2)`: if `expr1` is `NULL`, convert to `expr2`
- `NVL2((expr1, expr2, expr3)`: return `expr2` if `expr1` is not `NULL`, else `expr3`
- `NULLIF(expr1, expr2)`: return `NULL` if `expr1` and `expr2` are equal, else `expr1`
- `COALESCE(expr1, expr2,…,expr_n)`: returns first non-NULL expr
  - If you want NULLs to return first, use NULLs first

## Declaration Section

###  Declaring variables

- A procedure of any type can have an optional `DECLARE` section
- You only use `DECLARE` in an anonymous block; otherwise use IS
- Variable can be Uninitialized, initialized, or locked into a CONSTANT
- Use `%TYPE` to match the data type of a table's column
  ``` sql
  declare
    nameval employees.last_name%type;
    cursor  empcur is select * from employees;
  begin
    for empval in empcur
    loop
      nameval := empval.last_name;
      dbms_output.put_line('Employee last name: ' || nameval);
    end loop;
  end;
  /
  ```
- Use `%ROWTYPE` to match the column definitions of a table
  ``` sql
  declare
    emprec employees%rowtype;
  begin
    emprec.employee_id := 901;
    emprec.last_name := 'Smith';
        dbms_output.putline(emprec.employee_id || ' ' || emprec.last_name);
  end;
  /
  ```

### Declaring cursors

- Cursor: a pointer to a private SQL area with metadata for running a `SELECT` or other DML statement
- Explicit cursor: defined with `CURSOR` in the DECLARE section
  ``` sql
  declare
    nameval employees.last_name%type;
    cursor  empcur is select * from employees;
  begin
    for empval in empcur
    loop
      nameval := empval.last_name;
      dbms_output.put_line('Employee last name: ' || nameval);
    end loop;
  end;
  /
  ```
- Implicit cursor: used in the code section
  ``` sql
  declare
    nameval employees.last_name%type;
  begin
    for empval in (select * from employees)
    loop
      nameval := empval.last_name;
      dbms_output.put_line('Employee last name: ' || nameval);
    end loop;
  end;
  /
  ```
- Example of how to use built-in SQL% variables to see how many rows were changed from an update (implicit variable):
  ``` sql
  declare
    salupd number;
  begin
    update jobs set max_salary = 40000 where job_title like 'Sales%';
    salupd := sql%rowcount;
    dbms_output.put_line('JOBS salary updates: ' || salupd);
  end;
  /
  ```

## Security Model

### Invoker rights versus definer rights

- Definer rights (the default_) run with the privileges of the procedure owner
- Invoker rights run with the privileges of the user running the procedure
- User running the procedure needs `EXECUTE` privileges on the procedure regardless of whether it was created with definer or invoker rights
- Database roles granted to a user, even when they are a default role, are not enabled in a procedure created with definer rights

###  How blocks define scope

- A block is an anonymous or named section of code between `BEGIN` and `END` with an optional `DECLARE` section
- Document code blocks with comments and labels for ease of future code maintenance
- Blocks can be nested to facilitate local error handling
- Labeled nested blocks can qualify variable names
  ``` sql
  <<blockname>>
  blockname.variable;
  ```

## Conditional Statements

### `LOOP`

- The code inside a `LOOP` will always execute at least once
- Loop is terminated with `EXIT`, `EXIT WHEN`, and `GOTO`
- Execution order of code inside the loop can be altered with `GOTO` and `CONTINUE`

### `CONTINUE` and `EXIT`

- Use `CONTINUE` to interrupt processing and go back to the top
- Use `EXIT` to leave the loop and continue running after the end of the loop (break)
- Simple LOOP statements require an `EXIT`
- `GOTO` is another way to `EXIT` outside of the loop
- RETUNR exits the loops and the enclosing procedure block

### `FOR` loop

- First form uses a begin and end value for the loop
- Iterate backward through a loop with the `REVERSE` keyword
- Interrupt `FOR` loop processing with `CONTINUE`, `EXIT`, `RETURN`, and `GOTO`
- Second form of `FOR` loops iterates through a cursor instead of an integer range

### `WHILE` loop

- The code inside a `WHILE` may execute zero or more times
- A `WHILE` will have diner conditional control than `FOR`
- An `EXIT` statement with an `IF` can exit the WHILE loops early

### `IF` Statement

- `IF` statements always end with `END IF`
- Whether something inside of EF / `END` is executed or not, the next statement executed is after `END IF`
- If there are no alternate actions, just use `END IF`
- If there is an alternate action if the boolean expression is false, use `ELSE` and the commands
- If you need to test for multiple boolean conditions each with different outcomes, use `ELSIF` and the next boolean evaluation

## Using SQL `SELECT` Statements

### `SELECT INTO` and `%ROWCOUNT`

- PL/SQL supports every `SELECT` and DML statement in Oracle SQL
- Use `SELECT INTO` to retrieve columns from exactly a single row into local variables
- For multiple row retrieval, you use `BULK COLLECT` syntax
- For DML, `SQL%ROWCOUNT` contains the number of affected rows, regardless of whether an `INSERT`, `DELETE`, or `UPDATE` is run

### Running SQL statements: Static

- `SELECT` requires and `INTO` clause
- DML statements: `INSERT`, `UPDATE`, `DELETE`, `MERGE`
- EXCEPTION blocks handle errors during DML or SELECT execution
- Use built-in variables like `SQL%ROWCOUNT` to check how many rows were affected by the DML statement
- Sequence variables `CURRVAL` and `NEXTVAL` can be assigned to variables outside of their use in DML statements

### Running SQL statements: Dynamic

- Dynamic SQL runs SQL statements that can
- T be run natively in PL/SQL, such as `CREATE TABLE` or `GRANT`
- Some `SELECT` statements can run with native SQL or dynamic SQL
- Dynamic SQL must have an `INTO` clause if some kind of return value is expected
- An optional `USING` clause specifies values that are substituted for bind variables in `EXECUTE IMMEDIATE`

## Error Handling

### With `EXCEPTION`

- Error codes are internally defined, predefined, or user defined
- Use `PRAGMA` to assign labels to existing error codes
- Multiple `WHEN` statements in an `EXCEPTION` handler check the error conditions regardless of type

### User-defined exception types

- Include an `EXCEPTION` handler in the top-level block, at a minimum
- Use PRAGMA to assign an user-defined exception name to an internal error code
- Declare other `EXCEPTION` types to handle errors in business logic or processes
- An explicit `RAISE` will trigger the `EXCEPTION` block
- Use built-in `SQLCODE` and `SQLERRM` for all other errors

## Leveraging User-Defined Types

### Declaring and using user-defined types

- All data types for table columns exist in PL/SQL
- CLOB columns extend `VARCHAR2` columns and need DBMS_LOB to populate them in PL/SQL
- Data types, `BOOLEAN`, and `PLS_INTEGER` are supposed only in PL/SQL
- Subtypes facilitate restrictions on the base data type

### Using type with `TABLE` functions
