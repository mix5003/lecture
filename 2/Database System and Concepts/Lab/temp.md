# Transaction Management

ในการจัดการ Transaction นั้นจะใช้ Data Manipulation Language (DML) เช่นคำสั่งจำพวก `INSERT`, `UPDATE`, `DELETE` หรือ Data Definition Language (DDL) เช่น `CREATE`, `ALTER`, `DROP`, `RENAME`, `TRUNCATE` และ Data Control Language (DCL) เช่น `GRANT`, `REMOVE` 

## COMMIT

จบการทำงานของ Transaction นั้นโดยการยืนยันและบันทึกการเปลี่ยนแปลงนั้นแบบถาวรด้วย โดยการ COMMIT จะเป็นล้างจุด SAVEPOINT ด้วย

## SAVEPOINT name

ใช้ในการสร้าง SAVEPOINT ใน Transaction ปัจจุบันเพื่อบันทึกการเปลี่ยนแปลง โดยจะต้องมีการระบุชื่อของ SAVEPOINT นั้นๆ ด้วย

## ROLLBACK

ทำการยกเลิกการเปลี่ยนแปลงของ Transaction และยกเลิกการเปลี่ยนแปลงข้อมูล

## ROLLBACK TO SAVEPOINT name

ทำการยกเลิกการเปลี่ยนแปลงของ Transaction โดยย้อนกลับไปที่จุด SAVEPOINT ซึ่งจะทำให้ข้อมูลต่างๆ นั้นกลับไปอยู่ในลักษณะเดียวกันกับตอนที่ SAVEPOINT
