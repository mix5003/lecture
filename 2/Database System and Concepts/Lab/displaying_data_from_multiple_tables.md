# Displaying Data From Multiple Tables

## Cartesian Products

Cartesian Products เป็นลักษณะของการ join ระหว่างทุกแถวของเทเบิลแรกกับทุกแถวของเทเบิลที่สอง _ซึ่งจะเกิดขึ้นในกรณีที่เงื่อนไขในการ join นั้นผิดพลาดหรือไม่ครบถ้วน_ โดย Cartesian Products นั้นจะมีจำนวนแถวของผลลัพธ์เท่ากับ **จำนวนแถวของตารางแรก x จำนวนแถวของตารางที่สอง** และอาจเกิดขึ้นจากคำสั่ง SQL เช่น

```sql
SELECT *
FROM employees, departments;
```

และการ join แบบ Cross joins ด้วย

## Type of Joins

การ Join อ้างอิงจากมาตราฐานของภาษา SQL ปี 1999 นั้นสามารถจำแนกเป็นประเภทได้ดังนี้

- Cross joins
- Old-style join
- Equijoin
- Full (or two-sided) outer joins
- Arbitrary join conditions for outer joins
- Natural joins
- USING clause

## CROSS JOINS

`CROSS JOIN` เป็นรูปแบบการ JOIN ที่ทำให้เกิด [Cartesian Product](#cartesian-products) โดยมีรูปแบบคำสั่งคือ

```sql
SELECT *
FROM employees
CROSS JOIN departments;
```

### INNER VS OUTER JOINS

จากมาตราฐานของ SQL ปี 1999 **การ join ของสองเทเบิลที่คืนค่ามาเฉพาะแถวที่เหมือนกันจะเรียกว่า inner join** ซึ่งประกอบไปด้วยการ join ดังต่อไปนี้

- Old-style join
- Natural join
- Join with Using clause
- Join with On clause

และ**การ join ของสองเทเบิลที่คืนค่าทั้งแถวที่เหมือนกันและไม่เหมือนกัน โดยแถวที่ไม่เหมือนกันจะคืนค่าเป็น NULL จะเรียกว่า outer join** ซึ่งประกอบไปด้วยการ join ดังต่อไปนี้

- Left outer join
- Right outer join
- Full outer join

```sql
SELECT table1.column, table2.column
FROM table1
  [CROSS JOIN table2] |
  [JOIN table2 USING (column_name)] |
  [JOIN table2
     ON (table1.column_name = table2.column_name)] |
  [LEFT | RIGHT | FULL OUTER JOIN table2
     ON (table1.column_name = table2.column_name)] |
[NATURAL JOIN table2] ;
```

 ## OLD-STYLE

```sql
SELECT table1.column, table2.column
FROM table1, table2
WHERE table1.column_name = table2.column_name;
```

# ง่วงแล้ว ปวดตาด้วย เดี๋ยวมาเขียนต่อ ฮี้กับๆ
