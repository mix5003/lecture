# Database System Concepts Survival Guide

## SQL Statements

```sql
SELECT      Data Retrieval

INSERT
UPDATE      Data Manipulation Language (DML)
DELETE
MERGE

CREATE
ALTER
DROP        Data Deginition Language (DDL)
RENAME
TRUNCATE

COMMIT
ROLLBACK    Transaction Control
SAVEPOINT

GRANT
REVOKE      Data Control Language (DCL)
```

## WRITING SQL STATEMENTS

- SQL statements are **not case sensitive**.
- SQL statements can be **on one or more lines**.
- Keywords **cannot be abbreviated** or split across lines.
- **Keywords typically are entered in **uppercase**.
- Clauses are usually placed on separate lines.
- Indents are used to enhance readability.

### Writing basic SQL SELECT statements

- Selecting all columns:

```sql
SELECT *
FROM departments;
```

- Selecting specific columns:

```sql
SELECT department_id, localtion_id
FROM departments;
```

- Arithmetic expressions and parentheses can using with select:
    - Operator precedence: * / + -

```sql
SELECT last_name, salary, 12*salary+100
FROM employees;
```

- Defining a column alias:

```sql
SELECT last_name AS name, comission_pct comm
FROM employees;
```

```sql
SELECT last_name "Name", salary*12 "Annual Salary"
FROM employees;
```

- Concatenation Operator:

```sql
SELECT last_name || job_id AS "Employees"
FROM employees;
```

- Literal Character Strings:

```sql
SELECT last_name || 'is a' || job_id AS "Employee Details"
FROM employees;
```

- Eliminating Duplicate Rows:

```sql
SELECT DISTINCT department_id
FROM employees;
```

## Restricitng and sorting data

- Limited the rows selected using WHERE clause:
    - *Character strings* and *date values* must be enclosed in **single quotation marks **```(' ')```

    ```sql
    SELECT employeee_id, last_name, job_id, department_id
    FROM employees
    WHERE department_id = 90;
    ```

    ```sql
    SELECT last_name, job_id, department_id
    FROM employees
    WHERE last_name = 'Whalen';
    ```

- WHERE clause can using with comparison conditions and operator:
    - `BETWEEN ... AND ...` : Use to display rows based on a range of values:

    ```sql
    SELECT last_name, salary
    FROM employees
    WHERE salary BETWEEN 2500 AND 3500;
    ```

    - `IN (set)` : Use to match and test for values in a set:

    ```sql
    SELECT employee_id, last_name, salary, manager_id
    FROM employees
    WHERE manager_id IN (100, 101, 201);
    ```

    - `LIKE` : Match a character pattern
        - `%` denotes zero or many characters:

        ```sql
        SELECT first_name
        FROM employees
        WHERE first_name LIKE 'S%';
        ```

        - `_` denotes one character:

        ```sql
        SELECT first_name
        FROM employees
        WHERE last_name LIKE '_o%';
        ```

        - `ESCAPE` to search for the actual `%` and `_` symbols:

        ```sql
        SELECT employee_id, last_name, job_id
        FROM employees
        WHERE job_id LIKE %SA\_% ESCAPE '\';
        ```

    - `IS NULL` : Is a null value

    ```sql
    SELECT last_name, manager_id
    FROM employees
    WHERE manager_id IS NULL;
    ```

- WHERE clause can using with logical conditions
    - `AND` : Returns **TRUE** if *both* component conditions are true:

    ```sql
    -- Only employees who have a job title that contains the string MAN and earn $10,000 or more are selected
    SELECT employee_id, last_name, job_id, salary
    FROM employees
    WHERE salary >= 10000
    AND job_id LIKE '%MAN%';
    ```

    - `OR` : Returns **TRUE** if *either* component condition is true:

    ```sql
    -- Any employee who has a job ID containing MAN or earn $10,000 or more are selected
    SELECT employee_id, last_name, job_id, salary
    FROM employees
    WHERE salary >= 10000
    OR job_id LIKE '%MAN%';
    ```

    - `NOT` : Returns **TRUE** if the following condition is false:

    ```sql
    -- Displays the last names and job ID of all employees whose job ID is not IT_PROG, ST_CLERK, or SA_REP
    SELECT last_name, job_id
    FROM employees
    WHERE job_id
    NOT IN ('IT_PROG', 'ST_CLERK', 'SA_REP');
    ```

- Rules of Precedence
    - Can use parentheses to force priority

Order Evaluated | Operator
--- | ---
1 | Arithmetic operators
2 | Concatenation operator
3 | Comparison Conditions
4 | `IS [NOT] NULL`, `LIKE`, `[NOT] IN`
5 | `[NOT] BETWEEN`
6 | `NOT` logical condition
7 | `AND` logical condition
8 | `OR` logical condition

```sql
-- Select the row if an employee is a president and earns more than $15,000, or if the employee is a sales representative
SELECT last_name, job_id, salary
FROM employees
WHERE job_id = 'SA_REP'
OR job_id = 'AD_PRES'
AND salary > 15000;
```

- ORDER BY clause to sort rows
    - `ASC` : ascending order, **default**
    - `DESC` : descending order
    - The `ORDER BY` clause comes last in the `SELECT` statement

```sql
SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date;
```
