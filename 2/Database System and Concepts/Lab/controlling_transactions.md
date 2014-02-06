# Controlling Transactions

ในการจัดการ Transaction นั้นจะใช้ Data Manipulation Language (DML) เช่นคำสั่งจำพวก `INSERT`, `UPDATE`, `DELETE` หรือ Data Definition Language (DDL) เช่น `CREATE`, `ALTER`, `DROP`, `RENAME`, `TRUNCATE` และ Data Control Language (DCL) เช่น `GRANT`, `REMOVE`

การจัดการ Transaction สามารถกระทำได้จากโดยผู้ใช้งานเอง และจาก DBMS ซึ่งใน DBMS นั้นจะมีการใช้ Transaction Log ในการติดตามการเปลี่ยนแปลงของแต่ละ Transaction ที่จะทำให้เกิดการเปลี่ยนแปลงต่อฐานข้อมูล หากระบบเกิดมีปัญหา DBMS ก็จะทำการตรวจสอบจาก Transaction Log เพื่อหาจุดของการเปลี่ยนแปลงที่ยังทำไม่เสร็จสิ้นหรือยังไม่ถูก commit และทำการกู้คืนข้อมูลไปยังจุดนั้น

การทำงานของ Transaction จะเริ่มขึ้นเมื่อคำสั่ง DML ใดๆ ถูกเอ็กซีคิวต์ให้ทำงาน และจบลงเมื่อมีการใช้คำสั่ง `COMMIT`, `ROLLBACK`, มีการใช้คำสั่งในส่วนของ DDL, DCL ซึ่งจะเป็นการ commit โดยอัตโนมัติ, ผู้ใช้ออกจากโปรแกรม iSQL Plus หรือระบบนั้นเกิดปัญหา

## COMMIT

จบการทำงานของ Transaction นั้นโดยการยืนยันและบันทึกการเปลี่ยนแปลงนั้นแบบถาวรด้วย โดยการ COMMIT จะเป็นล้างจุด SAVEPOINT ด้วย โดยมีตัวอย่างการทำงานดังนี้

ทำการเปลี่ยนแปลงข้อมูล

```
DELETE FROM sales_reps
WHERE reps_id IN (160,170);
2 rows deleted.
UPDATE sales_reps
SET salary=8000
WHERE reps_id = 157;
1 rows update
```

ทำการ commit การเปลี่ยนแปลงข้อมูล

```
COMMIT;
Commit Complete.
```

## SAVEPOINT name

ใช้ในการสร้าง SAVEPOINT ใน Transaction ปัจจุบันเพื่อบันทึกการเปลี่ยนแปลง โดยจะต้องมีการระบุชื่อของ SAVEPOINT นั้นๆ ด้วย

## ROLLBACK

ทำการยกเลิกการเปลี่ยนแปลงของ Transaction และยกเลิกการเปลี่ยนแปลงข้อมูล

```
DELETE FROM sales_reps;
31 rows deleted.
ROLLBACK;
Rollback Complete.
```

## ROLLBACK TO SAVEPOINT name

ทำการยกเลิกการเปลี่ยนแปลงของ Transaction โดยย้อนกลับไปที่จุด SAVEPOINT ซึ่งจะทำให้ข้อมูลต่างๆ นั้นกลับไปอยู่ในลักษณะเดียวกันกับตอนที่ SAVEPOINT

```
DELETE FROM sales_reps
WHERE reps_id = 204;
SAVEPOINT delete_done;
1 row deleted.
Savepoint complete.
UPDATE sales_reps
SET salary = 6500
WHERE reps_id = 202;
ROLLBACK TO delete_done;
1 row updated.
Rollback complete.
```

จากตัวอย่างเป็นการใช้คำสั่ง `SAVEPOINT` สร้างจุดที่มีชื่อว่า *delete_one* และมีการใช้คำสั่ง `ROLLBACK` กลับไปยังจุด *delete_one*

## AUTOMATIC TRANSACTION PROCESSING

การเกิดขึ้นของ Automatic Transaction Processing นั้นมีอยู่สองรูปแบบคือการเกิด Automatic Commit และ Automatic Rollback โดยรูปแบบของ Automatic Commit นั้นจะเกิดขึ้นจากเหตุการณ์ในรูปแบบต่อไปนี้

- มีการใช้คำสั่งในรูปแบบของ DDL
- มีการใช้คำสั่งในรูปแบบของ DCL
- ผู้ใช้งานออกจากโปรแกรมโดยไม่มีการใช้คำสั่ง `COMMIT` หรือ `ROLLBACK`

และ Automatic Rollback จะเกิดขึ้นจากเหตุการณ์ต่อไปนี้

- มีการปิดตัวลงของโปรแกรม iSQL Plus หรือ SQL *Plus อย่างผิดปกติ
- ระบบทำงานผิดพลาด

ในโปรแกรม iSQL Plus การออกจากระบบอย่างสมบูรณ์นั้นจะเกิดขึ้นเมื่อผู้ใช้งานมีการ Logout ออกจาก session ที่เปิดอยู่ และในโปรแกรม SQL *Plus นั้นจะเกิดขึ้นเมื่อมีการใช้คำสั่ง `EXIT` โดยการปิดหน้าต่างของโปรแกรมนั้นจะถือเป็นการปิดตัวลงของโปรแกรมอย่างผิดปกติ

*State ของข้อมูล_ก่อน_ที่จะมีการใช้คำสั่ง `COMMIT หรือ `ROLLBACK`*

- ทุกๆ การเปลี่ยนแปลงข้อมูลระหว่างการทำ Transaction จะถือว่าเป็นการเปลี่ยนแปลงข้อมูลแบบชั่วคราวจนกว่า Transaction นั้นๆ จะถูก commit
- การจัดการข้อมูลนั้นเป็นการกระทำที่ส่งผลต่อหน่วยความจำของฐานข้อมูล ซึ่งหมายความว่าสามารถย้อนคืน State ของข้อมูลนั้นๆ ได้
- ผู้ใช้งานปัจจุบันสามารถทำการดูผลลัพธ์ของการจัดการกับข้อมูลได้ผ่านทางการ query ในขณะที่ผู้ใช้งานคนอื่นๆ จะไม่สามารถดูผลลัพธ์ได้
- การเปลี่ยนแปลงต่อ rows นั้นๆ จะถูกล็อคไม่ให้ผู้ใช้งานอื่นมีการเปลี่ยนแปลงในขณะที่ยังมีการเปลี่ยนแปลงโดยผู้ใช้งานปัจจุบันอยู่

*State ของข้อมูลหลังจากการใช้คำสั่ง `COMMIT` หรือ `ROLLBACK`*

- การเปลี่ยนแปลงของข้อมูลถูกเขียนไปยังฐานข้อมูลอย่างถาวร
- State ของข้อมูลก่อนหน้านี้จะไม่สามารถนำมาแสดงผลหรือใช้งานได้
- ผู้ใช้งานทุกคนสามารถดูผลลัพธ์ของ Transaction ได้
- ผู้ใช้งานทุกคนสามารถทำการแก้ไขข้อมูลได้
- จุด SAVEPOINT ทุกจุดจะถูกลบออกจากระบบ
