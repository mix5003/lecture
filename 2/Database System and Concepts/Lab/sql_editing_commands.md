# SQL Editing Commands

ในการใช้งาน SQL *Plus นั้นจะมีคำสั่งที่สามารถใช้เพื่อควบคุม จัดการ และแก้ไขคำสั่ง SQL ได้ เนื่องจากการทำงานแบบ Command-line Interface (CLI) นั้นทำเกิดความยากในการใช้งาน ดังนั้นการเรียนรู้รูปแบบของคำสั่งต่อไปนี้จะช่วยให้สามารถแก้ไขหรือจัดการคำสั่ง SQL ได้อย่างง่ายดายขึ้น

ปกตินั้นใน SQL *Plus เราจะสามารถเขียน SQL Statement หรือคำสั่ง SQL ได้หลายบรรทัดจนกว่าจะมีกว่าปิดคำสั่งด้วยเครื่องหมาย `;` (semi-colon) เช่น

```sql
SQL > SELECT employee_id, first_name
  2   FROM employees;

<query result>
```

## LIST

ใช้ในการแสดงคำสั่งที่มีอยู่ใน SQL Buffer

```sql
SQL> list
  1  SELECT employee_id, first_name
  2* FROM employees
SQL> 
```

## CHANGE

ใช้ในการแก้ไขคำสั่งโดยมีรูปแบบคือการใช้ `CHANGE /คำเก่า/คำใหม่`

```sql
SQL> change /firstname/first_name
  1* SELECT employee_id, first_name
SQL>
```

## RUN

ใช้ในการรันคำสั่งที่มีอยู่ใน SQL Buffer

```sql
SQL> run
  1  SELECT employee_id, first_name
  2* FROM employees

<query result>

SQL>
```

## <n>

`<n>` ใช้ในการใส่ตัวเลขเพื่อระบุบรรทัดที่จะมีการแก้ไข สังเกตได้จากการใช้คำสั่ง `list` จะมีเครื่องหมาย * อยู่ข้างหน้า

```sql
SQL> list
  1  SELECT employee_id, first_name
  2* FROM employees
SQL> 1
  1* SELECT employee_id, first_name
SQL>
```

## <n> <text>

`<n> <text>` ใข้ในการแทนบรรทัดที่ `n` ด้วยข้อความ `<text>` ทั้งบรรทัด

```sql
SQL> list
  1  SELECT employee_id, first_name
  2* FROM employees
SQL> 1 SELECT employee_id, first_name, job_id
SQL>
```

## APPEND

ใช้ในการเพิ่มตัวคำสั่งต่อท้ายบรรทัดที่ระบุปัจจุบัน

```sql
SQL> list
  1  SELECT employee_id, first_name
  2* FROM employees
SQL> APPEND , last_name
  1* SELECT employee_id, first_name, last_name
SQL>
```

## INPUT

ใช้ในการเพิ่มบรรทัดต่อท้าย

```sql
SQL> list
  1  SELECT employee_id, first_name
  2* FROM employees
SQL> input
  3  WHERE employee_id > 201;

<query result>

SQL>
```

## INPUT <text>

ใช้ในการเพิ่มบรรทัดพร้อมใส่ข้อความ

```sql
SQL> list
  1  SELECT employee_id, first_name
  2* FROM employees
SQL> input ORDER BY last_name;
SQL> run
  1  SELECT employee_id, first_name, last_name, job_id
  2  FROM employees
  3  WHERE employee_id > 201
  4* ORDER BY last_name;

<query result>

SQL>
```

## SAVE

ใช้ในการบันทึกคำสั่ง SQL ที่มีอยู่ใน SQL Buffer ปัจจุบันลงในไฟล์

```sql
SQL> SAVE test.sql
Created file test.sql
SQL>
```

## START

ใช้ในการเรียกไฟล์ SQL ที่ถูกบันทึก

```sql
SQL> start test

<query result>

SQL>
```

## @

ใช้ในการเรียกไฟล์ SQL ที่ถูกบันทึก

```sql
SQL> @test

<query result>

SQL>
```
