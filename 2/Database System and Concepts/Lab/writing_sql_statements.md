# WRITING SQL STATEMENTS

- คำสั่งในภาษา SQL นั้นอยู่ในรูปแบบ **not case sensitive** ซึ่งหมายความว่าการพิมพ์ `SELECT` จะมีเท่ากับการพิมพ์ `select`
- แต่คำสั่งในภาษา SQL มักจะถูกเขียนอยู่ในรูปตัวพิมพ์ใหญ่ เพื่อให้สามารถแยกแยะระหว่างตัวคำสั่งและข้อมูลได้
- คำสั่งในภาษา SQL นั้นสามารถมีได้มากกว่าหนึ่งบรรทัด โดยจะจบคำสั่งด้วยการใช้เครื่อง `;` (semi-colon)
- คีย์เวิร์ดหรือคำสั่งไม่มีรูปแบบตัวย่อ และไม่มีสามารถแบ่งคำสั่งให้อยู่ระหว่างบรรทัดได้
- การใช้ Clauses มักใช้เพื่อแยกบรรทัดกัน
- การ Indent มักใช้เพื่อให้สามารถอ่านคำสั่งได้ง่าย

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
**CAUTION** Alias will be treated as case-insensitive *(BUT displayed as uppercase)* if it isn't enclosed in double-quotes.

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
