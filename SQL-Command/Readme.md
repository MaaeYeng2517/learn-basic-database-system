# บทเรียนพื้นฐานภาษา SQL และฐานข้อมูล ตั้งแต่เริ่มต้น

## 1. บทนำ

**SQL (Structured Query Language)** คือภาษาที่ใช้สำหรับจัดการข้อมูลใน **ฐานข้อมูล (Database)** โดยสามารถใช้ SQL เพื่อ

* สร้างฐานข้อมูล
* สร้างตาราง
* เพิ่มข้อมูล
* ค้นหาข้อมูล
* แก้ไขข้อมูล
* ลบข้อมูล
* เชื่อมโยงข้อมูลระหว่างตาราง
* จัดกลุ่มและวิเคราะห์ข้อมูล
* กำหนดสิทธิ์การเข้าถึงข้อมูล

ระบบจัดการฐานข้อมูลที่รองรับ SQL ที่ได้รับความนิยม ได้แก่

* MySQL
* PostgreSQL
* Microsoft SQL Server
* Oracle Database
* MariaDB
* SQLite

---

# 2. วัตถุประสงค์การเรียนรู้

เมื่อเรียนบทนี้จบ ผู้เรียนสามารถ

1. อธิบายความหมายของ Database และ SQL ได้
2. อธิบายความแตกต่างระหว่าง Database, Table, Row และ Column ได้
3. สร้าง Database และ Table ได้
4. กำหนดชนิดข้อมูลให้เหมาะสมกับข้อมูลได้
5. เพิ่มข้อมูลด้วย `INSERT`
6. ค้นหาข้อมูลด้วย `SELECT`
7. กรองข้อมูลด้วย `WHERE`
8. เรียงลำดับข้อมูลด้วย `ORDER BY`
9. แก้ไขข้อมูลด้วย `UPDATE`
10. ลบข้อมูลด้วย `DELETE`
11. เข้าใจ Primary Key และ Foreign Key
12. เชื่อมข้อมูลหลายตารางด้วย `JOIN`
13. ใช้ฟังก์ชันพื้นฐาน เช่น `COUNT()`, `SUM()`, `AVG()`, `MAX()` และ `MIN()`
14. ใช้ `GROUP BY` และ `HAVING` ได้
15. สามารถสร้างฐานข้อมูลขนาดเล็กสำหรับใช้งานจริงได้

---

# 3. Database คืออะไร?

**Database (ฐานข้อมูล)** คือระบบสำหรับจัดเก็บข้อมูลอย่างเป็นระบบ เพื่อให้สามารถ

* จัดเก็บข้อมูล
* ค้นหาข้อมูล
* เพิ่มข้อมูล
* แก้ไขข้อมูล
* ลบข้อมูล
* วิเคราะห์ข้อมูล

ได้อย่างมีประสิทธิภาพ

ตัวอย่างระบบมหาวิทยาลัยอาจมีข้อมูล

* นักศึกษา
* อาจารย์
* รายวิชา
* ห้องเรียน
* การลงทะเบียน
* คะแนน
* คณะ
* สาขาวิชา

โครงสร้างโดยรวมอาจเป็น

```text
University Database
│
├── Students
├── Teachers
├── Courses
├── Departments
├── Classrooms
└── Enrollments
```

---

# 4. Database กับ DBMS ต่างกันอย่างไร?

## Database

คือข้อมูลที่ถูกจัดเก็บอย่างเป็นระบบ

## DBMS

**DBMS (Database Management System)** คือซอฟต์แวร์ที่ใช้จัดการฐานข้อมูล

ตัวอย่าง

```text
Database
    ↓
DBMS
    ↓
Application
```

ตัวอย่าง DBMS ได้แก่

| DBMS       | ลักษณะ                  |
| ---------- | ----------------------- |
| MySQL      | นิยมใน Web Application  |
| PostgreSQL | รองรับความสามารถขั้นสูง |
| SQL Server | นิยมในองค์กร            |
| Oracle     | ระบบองค์กรขนาดใหญ่      |
| SQLite     | ฐานข้อมูลแบบไฟล์        |
| MariaDB    | พัฒนาต่อยอดจาก MySQL    |

---

# 5. SQL คืออะไร?

SQL ย่อมาจาก

> **Structured Query Language**

เป็นภาษามาตรฐานสำหรับสื่อสารกับระบบฐานข้อมูลเชิงสัมพันธ์ (**Relational Database**)

ตัวอย่าง SQL

```sql
SELECT *
FROM students;
```

ความหมายคือ

> เลือกข้อมูลทั้งหมดจากตาราง `students`

---

# 6. โครงสร้างพื้นฐานของฐานข้อมูลเชิงสัมพันธ์

ฐานข้อมูลเชิงสัมพันธ์ประกอบด้วยตารางหลายตารางที่สามารถเชื่อมโยงกันได้

```text
Database
│
├── Table
│   ├── Column
│   ├── Column
│   └── Column
│
├── Table
│   ├── Column
│   └── Column
│
└── Table
    ├── Column
    └── Column
```

---

# 7. Table คืออะไร?

**Table (ตาราง)** คือโครงสร้างสำหรับจัดเก็บข้อมูลประเภทเดียวกัน

ตัวอย่างตาราง `students`

| student_id | name    | age | major                  |
| ---------: | ------- | --: | ---------------------- |
|       1001 | Somchai |  20 | Computer Science       |
|       1002 | Suda    |  21 | Information Technology |
|       1003 | Anan    |  19 | Computer Science       |

---

# 8. Column คืออะไร?

**Column** คือช่องข้อมูลที่อธิบายคุณลักษณะของข้อมูล

ตัวอย่าง

```text
student_id
name
age
major
```

จากตาราง

| student_id | name    | age | major            |
| ---------: | ------- | --: | ---------------- |
|       1001 | Somchai |  20 | Computer Science |

มีทั้งหมด 4 Columns ได้แก่

* `student_id`
* `name`
* `age`
* `major`

---

# 9. Row คืออะไร?

**Row** คือข้อมูลหนึ่งรายการ

ตัวอย่าง

| student_id | name    | age | major            |
| ---------: | ------- | --: | ---------------- |
|       1001 | Somchai |  20 | Computer Science |

ข้อมูลทั้งแถวถือเป็นหนึ่ง Record

```text
Record
│
├── student_id = 1001
├── name = Somchai
├── age = 20
└── major = Computer Science
```

---

# 10. Tuple คืออะไร?

ในทฤษฎีฐานข้อมูลเชิงสัมพันธ์ คำว่า **Tuple** หมายถึงข้อมูลหนึ่งแถวใน Relation หรือ Table

ดังนั้นสามารถจำง่าย ๆ ว่า

```text
Row ≈ Record ≈ Tuple
```

ในระดับพื้นฐาน ผู้เรียนสามารถใช้คำว่า **Row** หรือ **Record** ได้

---

# 11. Attribute คืออะไร?

**Attribute** หมายถึงคุณลักษณะของข้อมูล

ในฐานข้อมูลเชิงสัมพันธ์ Attribute มักสอดคล้องกับ **Column**

ตัวอย่าง

```text
Student
│
├── student_id
├── name
├── age
└── major
```

ดังนั้น

```text
Attribute ≈ Column
```

---

# 12. Data Type

**Data Type** คือชนิดข้อมูลที่กำหนดว่าคอลัมน์สามารถเก็บข้อมูลประเภทใด

ตัวอย่างชนิดข้อมูลพื้นฐาน

## 12.1 Integer

ใช้เก็บจำนวนเต็ม

```sql
age INT
```

ตัวอย่าง

```text
18
20
25
100
```

---

## 12.2 Decimal

ใช้เก็บตัวเลขที่มีทศนิยม

```sql
price DECIMAL(10,2)
```

ตัวอย่าง

```text
1250.50
999.99
```

เหมาะสำหรับข้อมูลทางการเงิน

---

## 12.3 VARCHAR

ใช้เก็บข้อความที่มีความยาวเปลี่ยนแปลงได้

```sql
name VARCHAR(100)
```

ตัวอย่าง

```text
Somchai
Suda
Computer Science
```

---

## 12.4 TEXT

ใช้เก็บข้อความขนาดใหญ่

```sql
description TEXT
```

เหมาะสำหรับ

* รายละเอียดสินค้า
* บทความ
* คำอธิบาย
* หมายเหตุ

---

## 12.5 DATE

ใช้เก็บวันที่

```sql
birth_date DATE
```

ตัวอย่าง

```text
2005-08-22
```

---

## 12.6 DATETIME

ใช้เก็บวันที่และเวลา

```sql
created_at DATETIME
```

ตัวอย่าง

```text
2026-08-22 10:30:00
```

---

# 13. ประเภทคำสั่ง SQL

สามารถแบ่งคำสั่ง SQL เป็นกลุ่มสำคัญได้ดังนี้

```text
SQL
│
├── DDL
│   ├── CREATE
│   ├── ALTER
│   ├── DROP
│   └── TRUNCATE
│
├── DML
│   ├── INSERT
│   ├── UPDATE
│   └── DELETE
│
├── DQL
│   └── SELECT
│
├── DCL
│   ├── GRANT
│   └── REVOKE
│
└── TCL
    ├── COMMIT
    ├── ROLLBACK
    └── SAVEPOINT
```

สำหรับผู้เริ่มต้น ควรเน้น

```text
CREATE
INSERT
SELECT
UPDATE
DELETE
```

---

# 14. DDL

**DDL (Data Definition Language)** ใช้สำหรับจัดการโครงสร้างฐานข้อมูล

คำสั่งสำคัญ ได้แก่

```sql
CREATE
ALTER
DROP
TRUNCATE
```

---

# 15. DML

**DML (Data Manipulation Language)** ใช้จัดการข้อมูลภายในตาราง

```sql
INSERT
UPDATE
DELETE
```

---

# 16. DQL

**DQL (Data Query Language)** ใช้สำหรับค้นหาข้อมูล

คำสั่งหลักคือ

```sql
SELECT
```

---

# 17. การสร้าง Database

ใช้คำสั่ง

```sql
CREATE DATABASE university_db;
```

ตรวจสอบฐานข้อมูล

```sql
SHOW DATABASES;
```

เลือกฐานข้อมูล

```sql
USE university_db;
```

---

# 18. การสร้าง Table

ใช้คำสั่ง `CREATE TABLE`

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    major VARCHAR(100)
);
```

โครงสร้างคือ

```text
students
│
├── student_id
├── name
├── age
└── major
```

---

# 19. Primary Key

**Primary Key** คือคอลัมน์ที่ใช้ระบุข้อมูลแต่ละ Record ให้แตกต่างกัน

ตัวอย่าง

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

`student_id` สามารถเป็น Primary Key ได้ เพราะนักศึกษาแต่ละคนควรมีรหัสไม่ซ้ำกัน

คุณสมบัติสำคัญ

* ต้องไม่ซ้ำ
* ไม่ควรเป็น `NULL`
* ใช้ระบุ Record แต่ละรายการ

---

# 20. AUTO_INCREMENT

ใน MySQL สามารถสร้างเลข ID อัตโนมัติได้

```sql
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);
```

เพิ่มข้อมูล

```sql
INSERT INTO students (name, age)
VALUES ('Somchai', 20);
```

ระบบสามารถสร้าง ID ให้โดยอัตโนมัติ

```text
1
2
3
4
...
```

---

# 21. INSERT

`INSERT` ใช้เพิ่มข้อมูลลงใน Table

```sql
INSERT INTO students (
    student_id,
    name,
    age,
    major
)
VALUES (
    1001,
    'Somchai',
    20,
    'Computer Science'
);
```

เพิ่มหลายรายการพร้อมกัน

```sql
INSERT INTO students (
    student_id,
    name,
    age,
    major
)
VALUES
(1001, 'Somchai', 20, 'Computer Science'),
(1002, 'Suda', 21, 'Information Technology'),
(1003, 'Anan', 19, 'Computer Science');
```

---

# 22. SELECT

`SELECT` ใช้ค้นหาข้อมูล

แสดงข้อมูลทั้งหมด

```sql
SELECT *
FROM students;
```

หรือเขียนแบบบรรทัดเดียว

```sql
SELECT * FROM students;
```

---

# 23. SELECT เฉพาะ Column

ตัวอย่าง

```sql
SELECT name, major
FROM students;
```

ผลลัพธ์

| name    | major                  |
| ------- | ---------------------- |
| Somchai | Computer Science       |
| Suda    | Information Technology |
| Anan    | Computer Science       |

---

# 24. WHERE

`WHERE` ใช้กำหนดเงื่อนไข

ตัวอย่าง

```sql
SELECT *
FROM students
WHERE age = 20;
```

ค้นหานักศึกษาที่อายุมากกว่า 20

```sql
SELECT *
FROM students
WHERE age > 20;
```

---

# 25. Operator

ตัวดำเนินการเปรียบเทียบที่สำคัญ

| Operator | ความหมาย            |
| -------- | ------------------- |
| `=`      | เท่ากับ             |
| `<>`     | ไม่เท่ากับ          |
| `>`      | มากกว่า             |
| `<`      | น้อยกว่า            |
| `>=`     | มากกว่าหรือเท่ากับ  |
| `<=`     | น้อยกว่าหรือเท่ากับ |

ตัวอย่าง

```sql
SELECT *
FROM students
WHERE age >= 20;
```

---

# 26. AND

ใช้เมื่อทุกเงื่อนไขต้องเป็นจริง

```sql
SELECT *
FROM students
WHERE age >= 20
AND major = 'Computer Science';
```

ความหมาย

> ค้นหานักศึกษาที่อายุอย่างน้อย 20 ปี และเรียน Computer Science

---

# 27. OR

ใช้เมื่ออย่างน้อยหนึ่งเงื่อนไขเป็นจริง

```sql
SELECT *
FROM students
WHERE major = 'Computer Science'
OR major = 'Information Technology';
```

---

# 28. LIKE

ใช้ค้นหาข้อความบางส่วน

```sql
SELECT *
FROM students
WHERE name LIKE 'S%';
```

หมายถึงชื่อที่ขึ้นต้นด้วย `S`

ตัวอย่าง

```text
Somchai
Suda
```

สัญลักษณ์สำคัญ

```text
%  = ตัวอักษรจำนวนกี่ตัวก็ได้
_  = ตัวอักษรหนึ่งตัว
```

ตัวอย่าง

```sql
SELECT *
FROM students
WHERE name LIKE '%chai%';
```

ค้นหาชื่อที่มีคำว่า `chai`

---

# 29. ORDER BY

ใช้เรียงลำดับข้อมูล

เรียงจากน้อยไปมาก

```sql
SELECT *
FROM students
ORDER BY age ASC;
```

เรียงจากมากไปน้อย

```sql
SELECT *
FROM students
ORDER BY age DESC;
```

ตัวอย่างเรียงตามชื่อ

```sql
SELECT *
FROM students
ORDER BY name ASC;
```

---

# 30. LIMIT

ใช้จำกัดจำนวนข้อมูล

```sql
SELECT *
FROM students
LIMIT 5;
```

หมายถึงแสดงข้อมูลสูงสุด 5 รายการ

---

# 31. UPDATE

ใช้แก้ไขข้อมูล

ตัวอย่าง

```sql
UPDATE students
SET age = 21
WHERE student_id = 1001;
```

โครงสร้าง

```sql
UPDATE table_name
SET column_name = value
WHERE condition;
```

> **ข้อควรระวัง:** ควรตรวจสอบ `WHERE` ก่อนใช้ `UPDATE`

ตัวอย่างที่อันตราย

```sql
UPDATE students
SET age = 21;
```

คำสั่งนี้อาจเปลี่ยนอายุของนักศึกษาทุกคน

---

# 32. DELETE

ใช้ลบข้อมูล

```sql
DELETE FROM students
WHERE student_id = 1003;
```

คำสั่งนี้ลบเฉพาะ Record ที่มี `student_id = 1003`

ข้อควรระวัง

```sql
DELETE FROM students;
```

คำสั่งนี้จะลบข้อมูลทุก Record ในตาราง

---

# 33. DELETE, TRUNCATE และ DROP

## DELETE

```sql
DELETE FROM students;
```

ลบข้อมูล แต่ Table ยังคงอยู่

## TRUNCATE

```sql
TRUNCATE TABLE students;
```

ลบข้อมูลทั้งหมดใน Table

## DROP

```sql
DROP TABLE students;
```

ลบทั้ง Table และข้อมูล

จำง่าย ๆ

```text
DELETE
→ ลบข้อมูล

TRUNCATE
→ ล้างข้อมูลใน Table

DROP
→ ลบ Table
```

---

# 34. NULL

`NULL` หมายถึงไม่มีค่าหรือยังไม่มีข้อมูล

ตัวอย่าง

| id | name    | email |
| -: | ------- | ----- |
|  1 | Somchai | NULL  |

การตรวจสอบ `NULL` ต้องใช้

```sql
IS NULL
```

เช่น

```sql
SELECT *
FROM students
WHERE email IS NULL;
```

ตรวจสอบข้อมูลที่ไม่เป็น NULL

```sql
SELECT *
FROM students
WHERE email IS NOT NULL;
```

ไม่ควรใช้

```sql
WHERE email = NULL;
```

---

# 35. Constraint

**Constraint** คือข้อกำหนดที่ใช้ควบคุมความถูกต้องของข้อมูล

ตัวอย่าง

```text
PRIMARY KEY
FOREIGN KEY
NOT NULL
UNIQUE
DEFAULT
CHECK
```

---

# 36. NOT NULL

กำหนดว่าคอลัมน์ต้องมีข้อมูล

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

หมายความว่า `name` ห้ามเป็น `NULL`

---

# 37. UNIQUE

กำหนดว่าข้อมูลต้องไม่ซ้ำ

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);
```

เหมาะสำหรับ

* Email
* Username
* Student Code

---

# 38. DEFAULT

กำหนดค่าเริ่มต้น

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    status VARCHAR(20) DEFAULT 'active'
);
```

ถ้าไม่กำหนด `status` ระบบจะใช้

```text
active
```

---

# 39. Foreign Key

**Foreign Key** ใช้สร้างความสัมพันธ์ระหว่างตาราง

ตัวอย่าง

```text
Students
   │
   │ student_id
   ↓
Enrollments
   │
   │ course_id
   ↓
Courses
```

สร้างตาราง

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

สร้างตาราง Courses

```sql
CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);
```

สร้างตาราง Enrollments

```sql
CREATE TABLE enrollments (
    enrollment_id INT PRIMARY KEY,
    student_id INT,
    course_id INT,

    FOREIGN KEY (student_id)
        REFERENCES students(student_id),

    FOREIGN KEY (course_id)
        REFERENCES courses(course_id)
);
```

---

# 40. ความสัมพันธ์ของตาราง

ความสัมพันธ์พื้นฐานมี 3 รูปแบบ

## One-to-One

```text
A ───── 1 : 1 ───── B
```

ตัวอย่าง

```text
Person
  │
  └── Passport
```

---

## One-to-Many

```text
A ───── 1 : N ───── B
```

ตัวอย่าง

```text
Department
    │
    ├── Student
    ├── Student
    └── Student
```

---

## Many-to-Many

```text
A ───── N : M ───── B
```

ตัวอย่าง

```text
Students
    │
    └── Enrollments
             │
             └── Courses
```

นักศึกษาหนึ่งคนเรียนได้หลายวิชา และหนึ่งวิชามีนักศึกษาหลายคน

---

# 41. JOIN

`JOIN` ใช้เชื่อมข้อมูลจากหลายตาราง

ตัวอย่าง

```sql
SELECT
    students.name,
    enrollments.course_id
FROM students
JOIN enrollments
ON students.student_id = enrollments.student_id;
```

ผลลัพธ์

| name    | course_id |
| ------- | --------: |
| Somchai |       101 |
| Suda    |       102 |

---

# 42. INNER JOIN

แสดงเฉพาะข้อมูลที่ตรงกันทั้งสองตาราง

```sql
SELECT
    students.name,
    courses.course_name
FROM students
INNER JOIN enrollments
    ON students.student_id = enrollments.student_id
INNER JOIN courses
    ON enrollments.course_id = courses.course_id;
```

---

# 43. LEFT JOIN

แสดงข้อมูลทั้งหมดจากตารางด้านซ้าย และข้อมูลที่ตรงกันจากตารางด้านขวา

```sql
SELECT
    students.name,
    enrollments.course_id
FROM students
LEFT JOIN enrollments
    ON students.student_id = enrollments.student_id;
```

เหมาะเมื่อเราต้องการเห็นนักศึกษาทุกคน แม้บางคนยังไม่ได้ลงทะเบียน

---

# 44. Aggregate Functions

SQL มีฟังก์ชันสำหรับคำนวณข้อมูล

| Function  | ความหมาย  |
| --------- | --------- |
| `COUNT()` | นับจำนวน  |
| `SUM()`   | ผลรวม     |
| `AVG()`   | ค่าเฉลี่ย |
| `MAX()`   | ค่าสูงสุด |
| `MIN()`   | ค่าต่ำสุด |

---

# 45. COUNT()

นับจำนวนนักศึกษา

```sql
SELECT COUNT(*)
FROM students;
```

กำหนดชื่อผลลัพธ์

```sql
SELECT COUNT(*) AS total_students
FROM students;
```

---

# 46. AVG()

หาค่าเฉลี่ย

```sql
SELECT AVG(age) AS average_age
FROM students;
```

---

# 47. MAX()

หาค่าสูงสุด

```sql
SELECT MAX(age) AS max_age
FROM students;
```

---

# 48. MIN()

หาค่าต่ำสุด

```sql
SELECT MIN(age) AS min_age
FROM students;
```

---

# 49. SUM()

หาผลรวม

```sql
SELECT SUM(score) AS total_score
FROM scores;
```

---

# 50. GROUP BY

ใช้จัดกลุ่มข้อมูล

ตัวอย่างนับจำนวนผู้เรียนในแต่ละสาขา

```sql
SELECT
    major,
    COUNT(*) AS total_students
FROM students
GROUP BY major;
```

ผลลัพธ์

| major                  | total_students |
| ---------------------- | -------------: |
| Computer Science       |              2 |
| Information Technology |              1 |

---

# 51. HAVING

ใช้กรองผลลัพธ์หลัง `GROUP BY`

```sql
SELECT
    major,
    COUNT(*) AS total_students
FROM students
GROUP BY major
HAVING COUNT(*) > 5;
```

ความแตกต่าง

```text
WHERE
→ กรอง Row ก่อน GROUP BY

HAVING
→ กรองกลุ่มหลัง GROUP BY
```

---

# 52. ALTER TABLE

ใช้แก้ไขโครงสร้าง Table

เพิ่ม Column

```sql
ALTER TABLE students
ADD email VARCHAR(100);
```

ลบ Column

```sql
ALTER TABLE students
DROP COLUMN email;
```

เปลี่ยนชนิดข้อมูลใน MySQL

```sql
ALTER TABLE students
MODIFY COLUMN name VARCHAR(200);
```

---

# 53. Alias

Alias ใช้ตั้งชื่อชั่วคราวให้ Column หรือ Table

ตัวอย่าง

```sql
SELECT
    name AS student_name,
    age AS student_age
FROM students;
```

ผลลัพธ์

| student_name | student_age |
| ------------ | ----------: |
| Somchai      |          20 |

---

# 54. DISTINCT

ใช้ตัดค่าที่ซ้ำกัน

ตัวอย่าง

```sql
SELECT DISTINCT major
FROM students;
```

ถ้ามีข้อมูล

```text
Computer Science
Computer Science
Information Technology
Computer Science
```

ผลลัพธ์จะเหลือ

```text
Computer Science
Information Technology
```

---

# 55. IN

ใช้ตรวจสอบว่าค่าอยู่ในรายการหรือไม่

แทนที่จะเขียน

```sql
SELECT *
FROM students
WHERE major = 'Computer Science'
   OR major = 'Information Technology';
```

สามารถเขียน

```sql
SELECT *
FROM students
WHERE major IN (
    'Computer Science',
    'Information Technology'
);
```

---

# 56. BETWEEN

ใช้ค้นหาค่าที่อยู่ในช่วง

```sql
SELECT *
FROM students
WHERE age BETWEEN 18 AND 25;
```

หมายถึงอายุระหว่าง 18 ถึง 25

---

# 57. SQL Query พื้นฐานที่ควรจำ

```sql
SELECT column1, column2
FROM table_name
WHERE condition
ORDER BY column_name ASC
LIMIT 10;
```

ตัวอย่าง

```sql
SELECT name, age, major
FROM students
WHERE age >= 20
ORDER BY age DESC
LIMIT 10;
```

---

# 58. ลำดับการทำงานของ SELECT

เมื่อเขียน Query แบบซับซ้อน ควรเข้าใจลำดับเชิงตรรกะ

```text
FROM
  ↓
JOIN
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
  ↓
LIMIT
```

การเข้าใจลำดับนี้จะช่วยให้เขียน SQL ที่ซับซ้อนได้ง่ายขึ้น

---

# 59. Mini Project: ระบบจัดการนักศึกษา

เราจะสร้างระบบฐานข้อมูลขนาดเล็ก

```text
school_db
│
├── students
├── courses
└── enrollments
```

## สร้าง Database

```sql
CREATE DATABASE school_db;

USE school_db;
```

---

## สร้างตาราง Students

```sql
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    student_code VARCHAR(20) NOT NULL UNIQUE,
    name VARCHAR(100) NOT NULL,
    major VARCHAR(100),
    age INT
);
```

---

## สร้างตาราง Courses

```sql
CREATE TABLE courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    course_code VARCHAR(20) NOT NULL UNIQUE,
    course_name VARCHAR(100) NOT NULL,
    credits INT
);
```

---

## สร้างตาราง Enrollments

```sql
CREATE TABLE enrollments (
    enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,

    FOREIGN KEY (student_id)
        REFERENCES students(student_id),

    FOREIGN KEY (course_id)
        REFERENCES courses(course_id)
);
```

---

# 60. เพิ่มข้อมูลนักศึกษา

```sql
INSERT INTO students (
    student_code,
    name,
    major,
    age
)
VALUES
('STD001', 'Somchai', 'Computer Science', 20),
('STD002', 'Suda', 'Information Technology', 21),
('STD003', 'Anan', 'Computer Science', 19),
('STD004', 'Mali', 'Data Science', 22);
```

---

# 61. เพิ่มข้อมูลรายวิชา

```sql
INSERT INTO courses (
    course_code,
    course_name,
    credits
)
VALUES
('CS101', 'Introduction to Programming', 3),
('CS102', 'Database Systems', 3),
('CS103', 'Data Structures', 3);
```

---

# 62. ค้นหานักศึกษาทั้งหมด

```sql
SELECT *
FROM students;
```

---

# 63. ค้นหานักศึกษา Computer Science

```sql
SELECT *
FROM students
WHERE major = 'Computer Science';
```

---

# 64. เรียงนักศึกษาตามอายุ

```sql
SELECT *
FROM students
ORDER BY age DESC;
```

---

# 65. นับจำนวนนักศึกษา

```sql
SELECT COUNT(*) AS total_students
FROM students;
```

---

# 66. นับนักศึกษาแต่ละสาขา

```sql
SELECT
    major,
    COUNT(*) AS total
FROM students
GROUP BY major;
```

---

# 67. แก้ไขข้อมูล

เปลี่ยนอายุของนักศึกษา `STD001`

```sql
UPDATE students
SET age = 21
WHERE student_code = 'STD001';
```

---

# 68. ลบข้อมูล

```sql
DELETE FROM students
WHERE student_code = 'STD004';
```

---

# 69. การฝึกปฏิบัติ

## แบบฝึกหัดที่ 1

สร้าง Database ชื่อ

```text
shop_db
```

---

## แบบฝึกหัดที่ 2

สร้าง Table ชื่อ

```text
products
```

โดยมี Column

```text
product_id
product_name
price
quantity
```

---

## แบบฝึกหัดที่ 3

เพิ่มสินค้าอย่างน้อย 5 รายการ

---

## แบบฝึกหัดที่ 4

แสดงสินค้าทั้งหมด

```sql
SELECT *
FROM products;
```

---

## แบบฝึกหัดที่ 5

ค้นหาสินค้าที่มีราคามากกว่า 1,000

```sql
SELECT *
FROM products
WHERE price > 1000;
```

---

## แบบฝึกหัดที่ 6

เรียงสินค้าจากราคาแพงที่สุด

```sql
SELECT *
FROM products
ORDER BY price DESC;
```

---

## แบบฝึกหัดที่ 7

หาสินค้าที่แพงที่สุด

```sql
SELECT MAX(price)
FROM products;
```

---

## แบบฝึกหัดที่ 8

หาค่าเฉลี่ยราคาสินค้า

```sql
SELECT AVG(price)
FROM products;
```

---

# 70. แบบทดสอบหลังเรียน

### ข้อ 1

คำสั่งใดใช้สร้าง Database?

A. `SELECT`
B. `CREATE DATABASE`
C. `INSERT`
D. `UPDATE`

**คำตอบ: B**

---

### ข้อ 2

คำสั่งใดใช้ค้นหาข้อมูล?

A. `SELECT`
B. `DELETE`
C. `DROP`
D. `ALTER`

**คำตอบ: A**

---

### ข้อ 3

คำสั่งใดใช้เพิ่มข้อมูล?

A. `INSERT`
B. `SELECT`
C. `UPDATE`
D. `DROP`

**คำตอบ: A**

---

### ข้อ 4

คำสั่งใดใช้แก้ไขข้อมูล?

A. `CREATE`
B. `INSERT`
C. `UPDATE`
D. `SELECT`

**คำตอบ: C**

---

### ข้อ 5

คำสั่งใดใช้ลบข้อมูล?

A. `DELETE`
B. `SELECT`
C. `ALTER`
D. `CREATE`

**คำตอบ: A**

---

### ข้อ 6

Primary Key มีหน้าที่อะไร?

A. จัดเรียงข้อมูล
B. ระบุ Record แต่ละรายการ
C. ลบข้อมูล
D. สร้าง Database

**คำตอบ: B**

---

### ข้อ 7

คำสั่งใดใช้เชื่อมตาราง?

A. `GROUP BY`
B. `JOIN`
C. `ORDER BY`
D. `LIMIT`

**คำตอบ: B**

---

### ข้อ 8

คำสั่งใดใช้กรองข้อมูล?

A. `WHERE`
B. `CREATE`
C. `DROP`
D. `INSERT`

**คำตอบ: A**

---

# 71. Roadmap การเรียน SQL

แนะนำให้เรียนตามลำดับต่อไปนี้

```text
ระดับที่ 1: Database Fundamentals
│
├── Database
├── DBMS
├── Table
├── Row
├── Column
├── Attribute
└── Data Type
        ↓
ระดับที่ 2: SQL Fundamentals
│
├── CREATE DATABASE
├── CREATE TABLE
├── INSERT
├── SELECT
├── UPDATE
└── DELETE
        ↓
ระดับที่ 3: Query
│
├── WHERE
├── AND / OR
├── LIKE
├── IN
├── BETWEEN
├── ORDER BY
├── LIMIT
└── DISTINCT
        ↓
ระดับที่ 4: Database Design
│
├── Primary Key
├── Foreign Key
├── Constraint
├── Relationship
└── Normalization
        ↓
ระดับที่ 5: Advanced Query
│
├── JOIN
├── GROUP BY
├── HAVING
├── Aggregate Functions
├── Subquery
└── CASE
        ↓
ระดับที่ 6: Database Engineering
│
├── Index
├── View
├── Transaction
├── Stored Procedure
├── Trigger
├── Security
└── Backup & Recovery
```

---

# 72. สรุปคำสั่ง SQL ที่ควรรู้

| SQL               | หน้าที่              |
| ----------------- | -------------------- |
| `CREATE DATABASE` | สร้าง Database       |
| `USE`             | เลือก Database       |
| `CREATE TABLE`    | สร้าง Table          |
| `ALTER TABLE`     | แก้ไขโครงสร้าง Table |
| `DROP TABLE`      | ลบ Table             |
| `INSERT`          | เพิ่มข้อมูล          |
| `SELECT`          | อ่านข้อมูล           |
| `WHERE`           | กรองข้อมูล           |
| `ORDER BY`        | เรียงข้อมูล          |
| `LIMIT`           | จำกัดจำนวน           |
| `DISTINCT`        | ตัดข้อมูลซ้ำ         |
| `UPDATE`          | แก้ไขข้อมูล          |
| `DELETE`          | ลบข้อมูล             |
| `JOIN`            | เชื่อมตาราง          |
| `GROUP BY`        | จัดกลุ่ม             |
| `HAVING`          | กรองกลุ่ม            |
| `COUNT()`         | นับจำนวน             |
| `SUM()`           | หาผลรวม              |
| `AVG()`           | หาค่าเฉลี่ย          |
| `MAX()`           | หาค่าสูงสุด          |
| `MIN()`           | หาค่าต่ำสุด          |

---

# 73. สรุปภาพรวม

สิ่งที่ผู้เริ่มต้นควรเข้าใจเป็นลำดับคือ

```text
Database
    ↓
Table
    ↓
Column + Row
    ↓
Data Type
    ↓
CREATE
    ↓
INSERT
    ↓
SELECT
    ↓
WHERE
    ↓
ORDER BY
    ↓
UPDATE
    ↓
DELETE
    ↓
Primary Key
    ↓
Foreign Key
    ↓
Relationship
    ↓
JOIN
    ↓
GROUP BY
    ↓
HAVING
    ↓
Subquery
    ↓
Index
    ↓
Transaction
    ↓
Database Design
```

หัวใจสำคัญของ SQL สำหรับผู้เริ่มต้นคือการเข้าใจว่า

> **Database ใช้จัดเก็บข้อมูล → Table ใช้แบ่งประเภทข้อมูล → Row คือข้อมูลแต่ละรายการ → Column คือคุณลักษณะของข้อมูล → SQL ใช้สั่งให้ Database จัดการข้อมูล**

เมื่อพื้นฐานเหล่านี้แน่นแล้ว จึงค่อยต่อยอดไปสู่ **Database Design → ER Diagram → Normalization → Advanced SQL → Index → Transaction → API → Backend → Data Engineering** ได้อย่างเป็นระบบ
