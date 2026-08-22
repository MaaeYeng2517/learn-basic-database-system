# บทเรียนภาษา SQL สำหรับนักศึกษาระดับปริญญาตรี

**ชื่อรายวิชา:** การจัดการฐานข้อมูลและภาษา SQL  
**ระดับ:** ปริญญาตรี  
**รูปแบบ:** บทเรียนภาคทฤษฎีผสมปฏิบัติ  
**ระบบฐานข้อมูลอ้างอิงในตัวอย่าง:** PostgreSQL โดยคำสั่งส่วนใหญ่สามารถประยุกต์ใช้กับ MySQL, MariaDB, SQL Server หรือ SQLite ได้ แต่ชนิดข้อมูล ฟังก์ชันวันที่ และคำสั่งเฉพาะระบบอาจแตกต่างกัน

> **SQL** หรือ Structured Query Language คือภาษาสำหรับนิยามโครงสร้าง จัดการ และสืบค้นข้อมูลในฐานข้อมูลเชิงสัมพันธ์ ผู้เรียนควรเข้าใจทั้ง “การเขียนคำสั่งให้ได้ผลลัพธ์” และ “การออกแบบข้อมูลให้ถูกต้อง ปลอดภัย และดูแลได้ในระยะยาว”

## 1. ภาพรวมรายวิชา

บทเรียนนี้แบ่งเป็นสามระดับต่อเนื่องกัน ระดับเริ่มต้นเน้นการอ่านตารางและเขียนคำสั่งพื้นฐาน ระดับกลางเน้นการรวมข้อมูลจากหลายตาราง การสรุปผล และการออกแบบโครงสร้างข้อมูล ส่วนระดับสูงเน้นการวิเคราะห์ข้อมูล ประสิทธิภาพ การทำงานพร้อมกัน ความปลอดภัย และการประยุกต์ใช้ในระบบจริง

การเรียนควรใช้ฐานข้อมูลตัวอย่างเดียวตลอดหลักสูตร เพื่อให้ผู้เรียนเห็นพัฒนาการของคำสั่งจากการค้นหาข้อมูลง่าย ๆ ไปสู่การสร้างรายงานและการแก้ปัญหาทางธุรกิจอย่างเป็นระบบ โครงสร้างตัวอย่างในเอกสารนี้เป็นระบบร้านค้าออนไลน์ขนาดเล็ก ประกอบด้วยลูกค้า สินค้า คำสั่งซื้อ รายการสินค้าในคำสั่งซื้อ และหมวดหมู่สินค้า

### 1.1 ผลลัพธ์การเรียนรู้

เมื่อเรียนจบ ผู้เรียนควรสามารถดำเนินการต่อไปนี้ได้

| ด้านความสามารถ | ผลลัพธ์การเรียนรู้ที่คาดหวัง |
|---|---|
| ความรู้พื้นฐาน | อธิบายตาราง แถว คอลัมน์ คีย์หลัก คีย์นอก ความสัมพันธ์ และค่า `NULL` ได้ |
| SQL ระดับเริ่มต้น | เขียน `CREATE TABLE`, `INSERT`, `SELECT`, `UPDATE`, `DELETE` และใช้เงื่อนไขพื้นฐานได้ |
| SQL ระดับกลาง | ใช้ `JOIN`, aggregate functions, `GROUP BY`, `HAVING`, subquery, CTE และ view ได้ |
| การออกแบบ | แปลงความต้องการเป็นตารางและความสัมพันธ์ พร้อมกำหนด constraints ที่เหมาะสมได้ |
| SQL ระดับสูง | ใช้ window functions, recursive CTE, transaction, index, `EXPLAIN` และแนวคิด concurrency ได้ |
| ความปลอดภัย | อธิบาย SQL injection และเลือกใช้ parameterized query หรือ prepared statement ได้ |
| การสื่อสาร | สร้างรายงานข้อมูลและอธิบายเหตุผลของ query ให้ผู้อื่นตรวจสอบได้ |

### 1.2 แผนการเรียนที่แนะนำ

| หน่วย | ระดับ | หัวข้อ | เวลาแนะนำ |
|---|---|---|---:|
| 1 | เริ่มต้น | แนวคิดฐานข้อมูลและการอ่านข้อมูล | 3 ชั่วโมง |
| 2 | เริ่มต้น | การสร้างตารางและ constraints | 4 ชั่วโมง |
| 3 | เริ่มต้น | การเพิ่ม แก้ไข ลบ และสืบค้นข้อมูล | 5 ชั่วโมง |
| 4 | เริ่มต้น | การคำนวณและการสรุปผล | 3 ชั่วโมง |
| 5 | กลาง | การเชื่อมตารางและ subquery | 6 ชั่วโมง |
| 6 | กลาง | CTE, view, set operations และฟังก์ชัน | 5 ชั่วโมง |
| 7 | กลาง | การออกแบบเชิงสัมพันธ์และดัชนี | 5 ชั่วโมง |
| 8 | กลาง | Transaction และความถูกต้องของข้อมูล | 3 ชั่วโมง |
| 9 | สูง | Window functions และ recursive query | 5 ชั่วโมง |
| 10 | สูง | Query plan และการเพิ่มประสิทธิภาพ | 4 ชั่วโมง |
| 11 | สูง | Concurrency, security, routines และ triggers | 5 ชั่วโมง |
| 12 | ทุกระดับ | โครงงานปลายภาค | 8–12 ชั่วโมง |

## 2. ชุดข้อมูลตัวอย่างสำหรับห้องปฏิบัติการ

ตัวอย่างทั้งหมดใช้ชื่อคอลัมน์ภาษาอังกฤษเพื่อให้สอดคล้องกับเครื่องมือฐานข้อมูลทั่วไป ส่วนคำอธิบายใช้ภาษาไทย ผู้สอนสามารถเปลี่ยนชื่อเป็นภาษาไทยได้หากระบบรองรับและกำหนดมาตรฐานการตั้งชื่ออย่างชัดเจน

### 2.1 การสร้างตาราง

```sql
CREATE TABLE categories (
    category_id  INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    category_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE customers (
    customer_id  INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    full_name    VARCHAR(150) NOT NULL,
    email        VARCHAR(255) NOT NULL UNIQUE,
    city         VARCHAR(100),
    created_at   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE products (
    product_id   INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    category_id  INTEGER NOT NULL REFERENCES categories(category_id),
    product_name VARCHAR(150) NOT NULL,
    unit_price   NUMERIC(10, 2) NOT NULL CHECK (unit_price > 0),
    stock_qty    INTEGER NOT NULL DEFAULT 0 CHECK (stock_qty >= 0),
    is_active    BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE orders (
    order_id     INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    customer_id  INTEGER NOT NULL REFERENCES customers(customer_id),
    order_date   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status       VARCHAR(20) NOT NULL DEFAULT 'NEW'
                 CHECK (status IN ('NEW', 'PAID', 'SHIPPED', 'CANCELLED'))
);

CREATE TABLE order_items (
    order_id    INTEGER NOT NULL REFERENCES orders(order_id) ON DELETE CASCADE,
    product_id  INTEGER NOT NULL REFERENCES products(product_id),
    quantity    INTEGER NOT NULL CHECK (quantity > 0),
    unit_price  NUMERIC(10, 2) NOT NULL CHECK (unit_price > 0),
    PRIMARY KEY (order_id, product_id)
);
```

คำสั่งข้างต้นแสดงแนวคิดสำคัญหลายประการ ได้แก่ `PRIMARY KEY` สำหรับระบุแถวอย่างไม่ซ้ำ `FOREIGN KEY` สำหรับรักษาความสัมพันธ์ระหว่างตาราง `NOT NULL` สำหรับข้อมูลที่จำเป็น `UNIQUE` สำหรับข้อมูลที่ไม่ควรซ้ำ และ `CHECK` สำหรับกฎทางธุรกิจเบื้องต้น Constraints เป็นกลไกที่ช่วยป้องกันข้อมูลผิดตั้งแต่ชั้นฐานข้อมูล ไม่ควรพึ่งพาการตรวจสอบจากหน้าจอหรือโปรแกรมเพียงอย่างเดียว [2]

ตาราง `order_items` ใช้ primary key แบบหลายคอลัมน์ เพราะสินค้าหนึ่งรายการควรปรากฏได้ไม่เกินหนึ่งแถวภายในคำสั่งซื้อเดียวกัน ตารางนี้ทำหน้าที่เป็นตารางกลางของความสัมพันธ์แบบหลายต่อหลายระหว่าง `orders` และ `products`

### 2.2 ข้อมูลเริ่มต้น

```sql
INSERT INTO categories (category_name) VALUES
    ('หนังสือ'),
    ('อุปกรณ์ไอที'),
    ('เครื่องเขียน');

INSERT INTO customers (full_name, email, city) VALUES
    ('กมลชนก ใจดี', 'kamonchan@example.com', 'กรุงเทพฯ'),
    ('ธนกร รักเรียน', 'thanakorn@example.com', 'เชียงใหม่'),
    ('ปวีณา สร้างสรรค์', 'paweena@example.com', 'ขอนแก่น'),
    ('ณัฐวุฒิ มีสุข', 'nattawut@example.com', NULL);

INSERT INTO products (category_id, product_name, unit_price, stock_qty) VALUES
    (1, 'หนังสือ SQL ฉบับปฏิบัติ', 420.00, 25),
    (1, 'หนังสือการออกแบบฐานข้อมูล', 380.00, 12),
    (2, 'คีย์บอร์ดไร้สาย', 890.00, 18),
    (2, 'เมาส์เพื่อสุขภาพ', 650.00, 30),
    (3, 'สมุดบันทึกโครงงาน', 85.00, 100),
    (3, 'ปากกาหมึกเจล', 35.00, 200);

INSERT INTO orders (customer_id, order_date, status) VALUES
    (1, '2026-01-10 09:30:00', 'PAID'),
    (2, '2026-01-11 14:00:00', 'SHIPPED'),
    (1, '2026-02-03 11:15:00', 'NEW'),
    (3, '2026-02-05 16:40:00', 'CANCELLED');

INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES
    (1, 1, 1, 420.00),
    (1, 5, 3, 85.00),
    (2, 3, 1, 890.00),
    (2, 6, 5, 35.00),
    (3, 2, 1, 380.00),
    (3, 4, 2, 650.00),
    (4, 1, 1, 420.00);
```

> **หลักการทดลอง:** ก่อนรันคำสั่งแก้ไขหรือลบข้อมูล ให้เริ่มจาก `SELECT` เพื่อตรวจสอบแถวเป้าหมายเสมอ และในงานจริงควรทดลองบนฐานข้อมูลทดสอบหรือใช้ transaction เพื่อย้อนกลับได้

# ส่วนที่หนึ่ง: ระดับเริ่มต้น

## 3. หน่วยที่ 1 — ทำความเข้าใจฐานข้อมูลและ SQL

ฐานข้อมูลเชิงสัมพันธ์จัดเก็บข้อมูลเป็นตาราง ตารางหนึ่งประกอบด้วยแถวซึ่งแทนระเบียน และคอลัมน์ซึ่งแทนคุณลักษณะของระเบียน แต่ละตารางควรมีความหมายชัดเจนและมีคีย์ที่ใช้ระบุแถวอย่างเหมาะสม การแยกข้อมูลเป็นหลายตารางช่วยลดการเก็บข้อมูลซ้ำและทำให้รักษาความถูกต้องได้ง่ายขึ้น

SQL แบ่งคำสั่งตามหน้าที่ได้หลายกลุ่ม ดังตารางต่อไปนี้

| กลุ่ม | ความหมาย | ตัวอย่างคำสั่ง |
|---|---|---|
| DDL | นิยามหรือเปลี่ยนโครงสร้าง | `CREATE`, `ALTER`, `DROP` |
| DML | เพิ่ม แก้ไข หรือลบข้อมูล | `INSERT`, `UPDATE`, `DELETE` |
| DQL | สืบค้นข้อมูล | `SELECT` |
| TCL | ควบคุม transaction | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT` |
| DCL | ควบคุมสิทธิ์ | `GRANT`, `REVOKE` |

SQL ไม่ได้ทำงานเหมือนภาษาโปรแกรมทั่วไปในแง่ที่ผู้เขียนมักบอก “ต้องวนลูปทีละแถว” แต่จะบอกผลลัพธ์ที่ต้องการในลักษณะ declarative จากนั้นระบบฐานข้อมูลจะเลือกแผนการประมวลผลที่เหมาะสมให้ ผู้เรียนจึงควรเน้นการระบุข้อมูลที่ต้องการ เงื่อนไข ความสัมพันธ์ และการจัดกลุ่มอย่างชัดเจน

## 4. หน่วยที่ 2 — การสืบค้นข้อมูลด้วย SELECT

คำสั่งพื้นฐานที่สุดคือ `SELECT` ใช้ระบุคอลัมน์ที่ต้องการอ่านและ `FROM` ใช้ระบุตารางต้นทาง

```sql
SELECT product_id, product_name, unit_price
FROM products;
```

การใช้ `SELECT *` เหมาะสำหรับการสำรวจตารางระหว่างเรียน แต่ในโปรแกรมจริงควรระบุคอลัมน์ที่ต้องการ เพราะทำให้ความหมายชัด ลดข้อมูลที่ส่งผ่านเครือข่าย และลดผลกระทบเมื่อโครงสร้างตารางเปลี่ยน

### 4.1 การตั้งชื่อผลลัพธ์ด้วย alias

```sql
SELECT
    product_name AS name,
    unit_price AS price,
    unit_price * 1.07 AS price_including_tax
FROM products;
```

`AS` ใช้ตั้งชื่อชั่วคราวให้คอลัมน์ผลลัพธ์ ตารางหรือคอลัมน์ที่มีชื่อยาวควรใช้ alias เพื่อให้อ่านง่าย โดยเฉพาะเมื่อมีการเชื่อมหลายตาราง

### 4.2 การกรองด้วย WHERE

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price >= 400;
```

ตัวดำเนินการที่ใช้บ่อย ได้แก่ `=`, `<>`, `>`, `<`, `>=`, `<=`, `AND`, `OR`, `NOT`, `IN`, `BETWEEN` และ `LIKE`

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price BETWEEN 100 AND 700
  AND is_active = TRUE;

SELECT product_name
FROM products
WHERE product_name LIKE '%ฐานข้อมูล%';

SELECT full_name, city
FROM customers
WHERE city IN ('กรุงเทพฯ', 'เชียงใหม่');
```

`BETWEEN` รวมค่าขอบทั้งสองด้านในระบบส่วนใหญ่ ส่วน `LIKE '%คำ%'` หมายถึงมีคำดังกล่าวอยู่ตำแหน่งใดก็ได้ เครื่องหมาย `%` แทนตัวอักษรจำนวนศูนย์ตัวขึ้นไป และ `_` แทนตัวอักษรหนึ่งตัว

### 4.3 ค่า NULL

`NULL` ไม่ได้หมายถึงศูนย์ สตริงว่าง หรือคำว่า “ไม่ทราบว่าเป็นอะไร” แบบเดียวกันเสมอไป แต่หมายถึงไม่มีค่าที่ใช้งานได้ในแถวนั้น การเปรียบเทียบกับ `NULL` ต้องใช้ `IS NULL` หรือ `IS NOT NULL`

```sql
SELECT full_name
FROM customers
WHERE city IS NULL;

SELECT full_name, COALESCE(city, 'ไม่ระบุจังหวัด') AS display_city
FROM customers;
```

เนื่องจาก SQL ใช้ตรรกะแบบสามค่า คือ true, false และ unknown เงื่อนไขที่เกี่ยวข้องกับ `NULL` อาจไม่ให้ผลอย่างที่ผู้เริ่มต้นคาด การใช้ `COALESCE` ช่วยเลือกค่าทดแทนเมื่อค่านั้นเป็น `NULL`

### 4.4 การเรียงลำดับและจำกัดจำนวนแถว

```sql
SELECT product_name, unit_price
FROM products
ORDER BY unit_price DESC, product_name ASC
LIMIT 3;
```

ถ้าไม่ระบุ `ORDER BY` ไม่ควรสมมติว่าผลลัพธ์จะเรียงตามลำดับใดลำดับหนึ่ง ระบบอาจส่งแถวตามแผนการประมวลผลที่เร็วที่สุด การอธิบายลำดับผลลัพธ์ที่แน่นอนจึงต้องระบุ `ORDER BY` [1]

## 5. หน่วยที่ 3 — การเพิ่ม แก้ไข และลบข้อมูล

### 5.1 INSERT

```sql
INSERT INTO categories (category_name)
VALUES ('อุปกรณ์สำนักงาน');
```

ควรระบุชื่อคอลัมน์ทุกครั้งที่ทำได้ เพื่อป้องกันข้อผิดพลาดเมื่อมีการเพิ่มคอลัมน์ใหม่และเพื่อทำให้ผู้อ่านเห็นว่าแต่ละค่าถูกบันทึกลงที่ใด

การเพิ่มหลายแถวใช้รูปแบบเดียวกันได้

```sql
INSERT INTO products (category_id, product_name, unit_price, stock_qty)
VALUES
    (4, 'แฟ้มเอกสาร', 75.00, 40),
    (4, 'เทปใส', 28.00, 60);
```

### 5.2 UPDATE

```sql
UPDATE products
SET unit_price = unit_price * 1.05
WHERE category_id = 1;
```

หากละ `WHERE` จะปรับปรุงทุกแถว จึงควรตรวจสอบด้วยคำสั่ง `SELECT` ก่อนเสมอ

```sql
SELECT product_id, product_name, unit_price
FROM products
WHERE product_id = 3;

UPDATE products
SET stock_qty = stock_qty - 2
WHERE product_id = 3
  AND stock_qty >= 2;
```

เงื่อนไข `stock_qty >= 2` ช่วยป้องกันการลดสต็อกจนติดลบ และยังควรตรวจสอบจำนวนแถวที่ได้รับผลกระทบจากโปรแกรมด้วย

### 5.3 DELETE

```sql
DELETE FROM customers
WHERE customer_id = 4;
```

คำสั่งนี้อาจล้มเหลวหากลูกค้ามีคำสั่งซื้อที่อ้างอิงอยู่ ทั้งนี้ขึ้นกับ foreign key และนโยบายการลบที่กำหนดไว้ การลบแบบ `ON DELETE CASCADE` ควรใช้เมื่อข้อมูลลูกเป็นส่วนประกอบที่ไม่ควรอยู่โดยลำพังเท่านั้น ไม่ควรใช้เพื่อความสะดวกโดยไม่วิเคราะห์ผลกระทบ

## 6. หน่วยที่ 4 — ฟังก์ชัน การคำนวณ และการสรุปผล

### 6.1 ฟังก์ชันแถวและการจัดรูปแบบ

```sql
SELECT
    UPPER(product_name) AS product_name_upper,
    ROUND(unit_price * 1.07, 2) AS price_with_tax
FROM products;
```

ฟังก์ชันอาจแตกต่างกันตามระบบฐานข้อมูล ตัวอย่างเช่น ฟังก์ชันจัดการวันที่และรูปแบบสตริงของ PostgreSQL กับ MySQL มีชื่อหรือไวยากรณ์ไม่เหมือนกัน ผู้เรียนควรตรวจคู่มือของระบบที่ใช้จริง

### 6.2 Aggregate functions

ฟังก์ชันกลุ่มจะสรุปข้อมูลหลายแถวเป็นค่าหนึ่งค่า เช่น `COUNT`, `SUM`, `AVG`, `MIN` และ `MAX`

```sql
SELECT
    COUNT(*) AS product_count,
    AVG(unit_price) AS average_price,
    MIN(unit_price) AS lowest_price,
    MAX(unit_price) AS highest_price
FROM products
WHERE is_active = TRUE;
```

`COUNT(*)` นับแถว ส่วน `COUNT(column_name)` โดยทั่วไปไม่นับแถวที่คอลัมน์นั้นเป็น `NULL` ความแตกต่างนี้เป็นประเด็นสำคัญเมื่อคอลัมน์อนุญาตให้ว่าง

### 6.3 GROUP BY และ HAVING

```sql
SELECT
    category_id,
    COUNT(*) AS product_count,
    AVG(unit_price) AS average_price
FROM products
GROUP BY category_id
HAVING COUNT(*) >= 2
ORDER BY average_price DESC;
```

`WHERE` กรองแถวก่อนการจัดกลุ่ม ส่วน `HAVING` กรองกลุ่มหลังจากคำนวณ aggregate แล้ว ลำดับเชิงตรรกะโดยย่อของ query คือ `FROM`/`JOIN` ตามด้วย `WHERE`, `GROUP BY`, `HAVING`, `SELECT`, `ORDER BY` และการจำกัดผลลัพธ์ โดยรายละเอียดจริงอาจถูก optimizer ปรับเปลี่ยนได้ [1]

### แบบฝึกหัดระดับเริ่มต้น

1. แสดงชื่อและราคาสินค้าที่มีราคามากกว่า 500 บาท โดยเรียงจากแพงไปถูก
2. แสดงลูกค้าที่ไม่ได้ระบุจังหวัด โดยให้แสดงข้อความ “ไม่ระบุ” แทน `NULL`
3. หาจำนวนสินค้า ราคาต่ำสุด ราคาสูงสุด และราคาเฉลี่ยของสินค้าทั้งหมด
4. ปรับราคาสินค้าหมวด “เครื่องเขียน” เพิ่ม 10% โดยต้องแสดงรายการที่จะถูกปรับก่อน
5. หาหมวดหมู่ที่มีสินค้าอย่างน้อยสองรายการ

# ส่วนที่สอง: ระดับกลาง

## 7. หน่วยที่ 5 — การเชื่อมตารางด้วย JOIN

ฐานข้อมูลเชิงสัมพันธ์มักแบ่งข้อมูลออกเป็นหลายตาราง จึงต้องใช้ `JOIN` เพื่อประกอบข้อมูลกลับมาเป็นรายงาน ความเข้าใจเรื่อง cardinality และคีย์ที่ใช้เชื่อมมีความสำคัญกว่าการจำรูปแบบคำสั่งเพียงอย่างเดียว

### 7.1 INNER JOIN

```sql
SELECT
    p.product_name,
    c.category_name,
    p.unit_price
FROM products AS p
INNER JOIN categories AS c
    ON c.category_id = p.category_id;
```

`INNER JOIN` แสดงเฉพาะแถวที่มีคู่ตรงกันทั้งสองตาราง หากสินค้าทุกแถวต้องมีหมวดหมู่ตาม constraint ผลลัพธ์จะสอดคล้องกับสินค้าที่มี category เสมอ

### 7.2 LEFT JOIN

```sql
SELECT
    c.customer_id,
    c.full_name,
    o.order_id,
    o.status
FROM customers AS c
LEFT JOIN orders AS o
    ON o.customer_id = c.customer_id
ORDER BY c.customer_id, o.order_id;
```

`LEFT JOIN` เก็บทุกแถวจากตารางด้านซ้าย แม้ไม่มีแถวตรงกันด้านขวา ในกรณีนี้จึงใช้ค้นหาลูกค้าที่ไม่เคยสั่งซื้อได้

```sql
SELECT c.customer_id, c.full_name
FROM customers AS c
LEFT JOIN orders AS o
    ON o.customer_id = c.customer_id
WHERE o.order_id IS NULL;
```

ข้อควรระวังคือเงื่อนไขที่วางใน `WHERE` อาจทำให้ `LEFT JOIN` มีพฤติกรรมคล้าย `INNER JOIN` โดยไม่ตั้งใจ ตัวอย่างต่อไปนี้จะเก็บเฉพาะลูกค้าที่มีคำสั่งซื้อสถานะ `PAID`

```sql
SELECT c.full_name, o.order_id
FROM customers AS c
LEFT JOIN orders AS o
    ON o.customer_id = c.customer_id
   AND o.status = 'PAID';
```

การวางเงื่อนไขสถานะไว้ใน `ON` ทำให้ยังคงเก็บลูกค้าที่ไม่มีคำสั่งซื้อแบบ `PAID` และให้คอลัมน์ฝั่งคำสั่งซื้อเป็น `NULL`

### 7.3 รายงานยอดคำสั่งซื้อ

```sql
SELECT
    o.order_id,
    c.full_name,
    o.order_date,
    SUM(oi.quantity * oi.unit_price) AS order_total
FROM orders AS o
JOIN customers AS c
    ON c.customer_id = o.customer_id
JOIN order_items AS oi
    ON oi.order_id = o.order_id
GROUP BY o.order_id, c.full_name, o.order_date
ORDER BY o.order_date;
```

เมื่อเลือกคอลัมน์ที่ไม่ใช่ aggregate พร้อมกับ `GROUP BY` คอลัมน์ดังกล่าวมักต้องอยู่ในกลุ่มด้วย รายงานนี้รวมสามตารางเพื่อให้ได้รหัสคำสั่งซื้อ ชื่อลูกค้า วันที่ และยอดรวม

## 8. หน่วยที่ 6 — Subquery, EXISTS และ CTE

### 8.1 Scalar subquery

Subquery คือ query ที่อยู่ภายใน query อื่น ใช้เมื่อผลลัพธ์ของ query หนึ่งเป็นเงื่อนไขหรือแหล่งข้อมูลของอีก query หนึ่ง

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price > (
    SELECT AVG(unit_price)
    FROM products
);
```

Subquery ในตัวอย่างคืนค่าหนึ่งค่า จึงนำไปเปรียบเทียบกับราคาของแต่ละสินค้าได้ หาก subquery คืนหลายแถว ต้องใช้ `IN`, `ANY`, `ALL` หรือปรับรูปแบบให้เหมาะสม

### 8.2 EXISTS

```sql
SELECT c.customer_id, c.full_name
FROM customers AS c
WHERE EXISTS (
    SELECT 1
    FROM orders AS o
    WHERE o.customer_id = c.customer_id
      AND o.status = 'PAID'
);
```

`EXISTS` ใช้ตรวจว่ามีแถวที่ตรงเงื่อนไขอย่างน้อยหนึ่งแถวหรือไม่ มักเหมาะกับคำถามลักษณะ “ลูกค้าคนใดมีคำสั่งซื้อที่ชำระแล้ว” และไม่จำเป็นต้องดึงข้อมูลจาก subquery ออกมาเป็นผลลัพธ์

### 8.3 Common Table Expression หรือ CTE

CTE ใช้ `WITH` ตั้งชื่อผลลัพธ์ชั่วคราวภายใน query ทำให้ query ซับซ้อนอ่านง่ายขึ้นและแบ่งเป็นขั้นตอนเชิงตรรกะได้

```sql
WITH order_totals AS (
    SELECT
        order_id,
        SUM(quantity * unit_price) AS total_amount
    FROM order_items
    GROUP BY order_id
)
SELECT
    o.order_id,
    c.full_name,
    ot.total_amount
FROM orders AS o
JOIN customers AS c ON c.customer_id = o.customer_id
JOIN order_totals AS ot ON ot.order_id = o.order_id
WHERE ot.total_amount >= 500;
```

CTE ไม่ได้แปลว่าจะเร็วกว่า subquery หรือทำให้ข้อมูลถูก materialize เสมอไป การเลือกใช้ควรพิจารณาความชัดเจนและแผนการประมวลผลร่วมกัน PostgreSQL ระบุว่า `WITH` สามารถทำหน้าที่คล้ายตารางชั่วคราวของ query และรองรับ recursive query ด้วย [1]

## 9. หน่วยที่ 7 — Set operations, View และฟังก์ชัน CASE

### 9.1 UNION, INTERSECT และ EXCEPT

```sql
SELECT email FROM customers WHERE city = 'กรุงเทพฯ'
UNION
SELECT email FROM customers WHERE city = 'เชียงใหม่';
```

`UNION` รวมผลลัพธ์และตัดข้อมูลซ้ำ ส่วน `UNION ALL` รวมผลลัพธ์โดยไม่ตัดซ้ำ `INTERSECT` คืนค่าที่อยู่ในทั้งสองผลลัพธ์ และ `EXCEPT` คืนค่าที่อยู่ในผลลัพธ์แรกแต่ไม่อยู่ในผลลัพธ์ที่สอง จำนวนคอลัมน์และชนิดข้อมูลที่เข้าคู่กันต้องสอดคล้องกัน

### 9.2 CASE

```sql
SELECT
    product_name,
    stock_qty,
    CASE
        WHEN stock_qty = 0 THEN 'หมดสต็อก'
        WHEN stock_qty < 10 THEN 'เหลือน้อย'
        ELSE 'มีสินค้า'
    END AS stock_status
FROM products;
```

`CASE` เป็นเครื่องมือสำคัญสำหรับเปลี่ยนค่าดิบให้เป็นหมวดหมู่ที่ใช้ในรายงาน และควรตั้งชื่อผลลัพธ์ให้สื่อความหมาย

### 9.3 View

View คือ query ที่ตั้งชื่อไว้เพื่อเรียกใช้เสมือนตาราง เหมาะกับรายงานที่ใช้ซ้ำหรือการซ่อนความซับซ้อนบางส่วน

```sql
CREATE VIEW order_summary AS
SELECT
    o.order_id,
    o.customer_id,
    o.order_date,
    o.status,
    SUM(oi.quantity * oi.unit_price) AS total_amount
FROM orders AS o
JOIN order_items AS oi ON oi.order_id = o.order_id
GROUP BY o.order_id, o.customer_id, o.order_date, o.status;

SELECT *
FROM order_summary
WHERE status <> 'CANCELLED';
```

View ปกติไม่ได้เก็บผลลัพธ์แยกเป็นข้อมูลชุดใหม่ แต่จะประเมิน query เมื่อถูกเรียกใช้ ส่วน materialized view จะเก็บผลลัพธ์ไว้และต้องมีกลยุทธ์ refresh ซึ่งเป็นแนวคิดระดับสูงกว่า

## 10. หน่วยที่ 8 — การออกแบบฐานข้อมูลเชิงสัมพันธ์

การออกแบบเริ่มจากการเก็บความต้องการ เช่น “ลูกค้าหนึ่งคนมีหลายคำสั่งซื้อ” และ “คำสั่งซื้อหนึ่งรายการมีสินค้าหลายชนิด” จากนั้นระบุ entity, attribute, key และ relationship ก่อนแปลงเป็นตาราง

| ความสัมพันธ์ | วิธีออกแบบทั่วไป | ตัวอย่าง |
|---|---|---|
| หนึ่งต่อหนึ่ง | ใช้ foreign key ที่มี `UNIQUE` | บุคคล–ข้อมูลหนังสือเดินทาง |
| หนึ่งต่อหลาย | เก็บ foreign key ฝั่งหลาย | ลูกค้า–คำสั่งซื้อ |
| หลายต่อหลาย | สร้างตารางกลาง | คำสั่งซื้อ–สินค้า ผ่าน `order_items` |
| ความสัมพันธ์กับตนเอง | foreign key ชี้ตารางเดียวกัน | พนักงาน–หัวหน้างาน |

การทำ normalization ระดับเบื้องต้นมีเป้าหมายลดความซ้ำและปัญหา anomaly โดย 1NF ให้ข้อมูลในแต่ละช่องเป็นค่าเดียว ไม่เก็บรายการที่คั่นด้วย comma; 2NF ลดการพึ่งพาบางส่วนของคีย์ผสม; และ 3NF ลดการพึ่งพาแบบส่งต่อจากคอลัมน์ที่ไม่ใช่คีย์หนึ่งไปยังอีกคอลัมน์หนึ่ง ในระบบวิเคราะห์ข้อมูล อาจมีการ denormalize อย่างมีเหตุผลเพื่อประสิทธิภาพ แต่ควรทำหลังจากเข้าใจ trade-off แล้ว

ตัวอย่างการออกแบบที่ไม่ดีคือการเก็บ `product_1`, `product_2`, `product_3` ไว้ในตารางคำสั่งซื้อ วิธีนี้จำกัดจำนวนสินค้า ทำให้ค้นหาและรวมยอดยาก และทำให้โครงสร้างเปลี่ยนเมื่อความต้องการเพิ่มขึ้น การสร้าง `order_items` แยกเป็นหลายแถวจึงยืดหยุ่นกว่า

## 11. หน่วยที่ 9 — ดัชนีและพื้นฐานประสิทธิภาพ

ดัชนีช่วยให้ระบบค้นหาแถวบางประเภทได้เร็วขึ้น แต่ต้องแลกกับพื้นที่จัดเก็บและต้นทุนเมื่อเพิ่มหรือแก้ไขข้อมูล ดัชนีจึงไม่ใช่สิ่งที่ควรสร้างทุกคอลัมน์ ควรสร้างจากรูปแบบ query จริงและตรวจสอบด้วย query plan

```sql
CREATE INDEX idx_orders_customer_date
ON orders (customer_id, order_date);

CREATE INDEX idx_products_active_price
ON products (unit_price)
WHERE is_active = TRUE;
```

ลำดับคอลัมน์ใน composite index สำคัญ โดยทั่วไป query ที่กรองหรือเรียงตามคอลัมน์นำหน้าจะใช้ประโยชน์ได้ดีกว่า ก่อนเพิ่มดัชนีควรวิเคราะห์ selectivity ปริมาณข้อมูล ความถี่ของ query และภาระการเขียน

การใช้ฟังก์ชันกับคอลัมน์ในเงื่อนไขอาจทำให้ดัชนีธรรมดาใช้ไม่ได้ เช่น `WHERE LOWER(email) = ...` จึงอาจต้องปรับรูปแบบข้อมูล สร้าง expression index หรือเลือกแนวทางที่ระบบฐานข้อมูลรองรับ

## 12. หน่วยที่ 10 — Transaction และความถูกต้องของการแก้ไขข้อมูล

Transaction คือกลุ่มคำสั่งที่ต้องสำเร็จทั้งหมดหรือไม่เกิดผลบางส่วน คุณสมบัตินี้สำคัญกับงาน เช่น ตัดเงินและลดสต็อก หากขั้นตอนหนึ่งล้มเหลว ไม่ควรเกิดสถานะที่เงินถูกตัดแล้วแต่สต็อกไม่ลด ระบบ transaction จึงช่วยรักษาความเป็นหน่วยเดียวของการดำเนินการ [3]

```sql
BEGIN;

UPDATE products
SET stock_qty = stock_qty - 2
WHERE product_id = 4
  AND stock_qty >= 2;

-- โปรแกรมควรตรวจสอบจำนวนแถวที่ถูกแก้ไข

INSERT INTO order_items (order_id, product_id, quantity, unit_price)
SELECT 3, product_id, 2, unit_price
FROM products
WHERE product_id = 4
  AND stock_qty >= 0;

COMMIT;
```

หากตรวจพบข้อผิดพลาด ให้ใช้ `ROLLBACK` แทน `COMMIT` การปรับตัวอย่างให้สมบูรณ์ในระบบจริงต้องตรวจสอบจำนวนแถวที่ถูก update และล็อกหรือใช้เงื่อนไขที่ป้องกันการแข่งขันระหว่าง transaction ด้วย

```sql
BEGIN;
UPDATE products SET stock_qty = stock_qty - 2
WHERE product_id = 4 AND stock_qty >= 2;

SAVEPOINT after_stock_update;
-- หากขั้นตอนถัดไปมีปัญหา
ROLLBACK TO after_stock_update;

COMMIT;
```

PostgreSQL อธิบายว่า `BEGIN` และ `COMMIT` ใช้ล้อมกลุ่มคำสั่ง และ `ROLLBACK` ยกเลิกการเปลี่ยนแปลงใน transaction ส่วน `SAVEPOINT` ช่วยย้อนกลับเฉพาะบางช่วงได้ [3]

### แบบฝึกหัดระดับกลาง

1. แสดงรายการคำสั่งซื้อพร้อมชื่อลูกค้าและยอดรวม โดยไม่รวมคำสั่งซื้อที่ถูกยกเลิก
2. หาลูกค้าที่เคยมีคำสั่งซื้อสถานะ `PAID` อย่างน้อยหนึ่งรายการ โดยใช้ `EXISTS`
3. แสดงหมวดหมู่ที่มีราคาสินค้าเฉลี่ยสูงกว่าราคาเฉลี่ยของสินค้าทั้งหมด
4. ออกแบบตารางสำหรับระบบลงทะเบียนเรียนที่นักศึกษาหนึ่งคนลงทะเบียนได้หลายวิชา และวิชาหนึ่งมีนักศึกษาได้หลายคน
5. เขียน transaction สำหรับการโอนสินค้าออกจากคลัง โดยต้องไม่อนุญาตให้สต็อกติดลบ
6. เสนอ index อย่างน้อยสองรายการสำหรับ query ที่ค้นหาคำสั่งซื้อของลูกค้าตามช่วงวันที่ พร้อมอธิบายเหตุผล

# ส่วนที่สาม: ระดับสูง

## 13. หน่วยที่ 11 — Window Functions

Window function คำนวณข้ามกลุ่มแถวที่เกี่ยวข้องกับแถวปัจจุบัน แต่ยังคงแสดงแต่ละแถวแยกกัน ต่างจาก aggregate ปกติที่รวมหลายแถวให้เหลือหนึ่งแถว [4]

### 13.1 จัดอันดับสินค้าในแต่ละหมวดหมู่

```sql
SELECT
    c.category_name,
    p.product_name,
    p.unit_price,
    ROW_NUMBER() OVER (
        PARTITION BY p.category_id
        ORDER BY p.unit_price DESC, p.product_id
    ) AS price_rank
FROM products AS p
JOIN categories AS c ON c.category_id = p.category_id;
```

`PARTITION BY` แบ่งข้อมูลเป็นกลุ่มย่อย ส่วน `ORDER BY` ภายใน `OVER` กำหนดลำดับสำหรับการคำนวณ ไม่จำเป็นต้องเหมือน `ORDER BY` ของผลลัพธ์สุดท้าย

### 13.2 เปรียบเทียบยอดคำสั่งซื้อกับค่าเฉลี่ยของลูกค้า

```sql
WITH order_totals AS (
    SELECT order_id, SUM(quantity * unit_price) AS total_amount
    FROM order_items
    GROUP BY order_id
)
SELECT
    o.customer_id,
    o.order_id,
    ot.total_amount,
    AVG(ot.total_amount) OVER (
        PARTITION BY o.customer_id
    ) AS customer_average,
    ot.total_amount
      - AVG(ot.total_amount) OVER (PARTITION BY o.customer_id)
      AS difference_from_average
FROM orders AS o
JOIN order_totals AS ot ON ot.order_id = o.order_id;
```

### 13.3 Running total และการเปลี่ยนแปลงจากแถวก่อนหน้า

```sql
WITH daily_sales AS (
    SELECT
        CAST(o.order_date AS DATE) AS sales_date,
        SUM(oi.quantity * oi.unit_price) AS daily_total
    FROM orders AS o
    JOIN order_items AS oi ON oi.order_id = o.order_id
    WHERE o.status IN ('PAID', 'SHIPPED')
    GROUP BY CAST(o.order_date AS DATE)
)
SELECT
    sales_date,
    daily_total,
    SUM(daily_total) OVER (ORDER BY sales_date) AS running_total,
    daily_total - LAG(daily_total) OVER (ORDER BY sales_date) AS change_from_previous_day
FROM daily_sales
ORDER BY sales_date;
```

Window function มักอยู่ใน `SELECT` หรือ `ORDER BY` และหากต้องกรองผลลัพธ์หลังคำนวณ เช่น เอาเฉพาะสามอันดับแรกต่อหมวดหมู่ ต้องห่อ query ไว้ใน subquery หรือ CTE ก่อน แล้วจึงกรองใน query ชั้นนอก [4]

## 14. หน่วยที่ 12 — Recursive CTE

Recursive CTE เหมาะกับข้อมูลแบบลำดับชั้น เช่น โครงสร้างหน่วยงาน หมวดหมู่ย่อย หรือกราฟที่มีความสัมพันธ์ต่อเนื่อง ตัวอย่างต่อไปนี้สร้างตารางพนักงานที่มีหัวหน้างาน

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    full_name   VARCHAR(150) NOT NULL,
    manager_id  INTEGER REFERENCES employees(employee_id),
    job_title   VARCHAR(100) NOT NULL
);
```

การดึงสายบังคับบัญชาจากพนักงานคนหนึ่งสามารถเขียนได้ดังนี้

```sql
WITH RECURSIVE management_chain AS (
    SELECT
        employee_id,
        full_name,
        manager_id,
        0 AS level
    FROM employees
    WHERE employee_id = 10

    UNION ALL

    SELECT
        e.employee_id,
        e.full_name,
        e.manager_id,
        mc.level + 1
    FROM employees AS e
    JOIN management_chain AS mc
      ON e.employee_id = mc.manager_id
)
SELECT *
FROM management_chain
ORDER BY level;
```

Recursive query ต้องมีส่วนตั้งต้นและส่วน recursive ที่อ้างถึงผลลัพธ์เดิม ผู้เรียนต้องระวังวงจรของข้อมูลและควรกำหนดเงื่อนไขหยุดหรือใช้กลไกตรวจจับ cycle ตามความสามารถของระบบ

## 15. หน่วยที่ 13 — การอ่าน Query Plan ด้วย EXPLAIN

`EXPLAIN` แสดงแผนที่ optimizer เลือกเพื่อประมวลผล query ส่วน `EXPLAIN ANALYZE` รัน query จริงพร้อมรายงานเวลาที่เกิดขึ้นจริงและจำนวนแถวที่ได้ จึงต้องระวังเมื่อใช้กับ `INSERT`, `UPDATE` หรือ `DELETE` เพราะคำสั่งจะเกิดผลจริงหากไม่ใช้ transaction ครอบไว้

```sql
EXPLAIN
SELECT o.order_id, o.order_date
FROM orders AS o
WHERE o.customer_id = 1
  AND o.order_date >= DATE '2026-01-01';
```

รูปแบบที่ปลอดภัยสำหรับทดลองคำสั่งแก้ไขคือ

```sql
BEGIN;
EXPLAIN ANALYZE
UPDATE products
SET unit_price = unit_price * 1.02
WHERE category_id = 1;
ROLLBACK;
```

สิ่งที่ควรสังเกตคือ estimated rows กับ actual rows, ชนิดของ scan, จำนวน loop, เวลาในแต่ละ node และความแตกต่างระหว่างค่าประมาณกับค่าจริง หากแตกต่างมาก อาจเกิดจากสถิติไม่ทันสมัย เงื่อนไขมี selectivity ต่ำ หรือการกระจายข้อมูลไม่เป็นไปตามสมมติฐาน

แนวทางเพิ่มประสิทธิภาพที่ควรพิจารณาตามลำดับคือปรับ query ให้ตรงความต้องการจริง ลดคอลัมน์ที่ไม่จำเป็น ตรวจสอบเงื่อนไขและชนิดข้อมูล สร้าง index ที่สอดคล้องกับ workload ปรับปรุงสถิติ และค่อยพิจารณาการปรับโครงสร้างหรือการแบ่งพาร์ทิชัน การมี index มากเกินไปอาจทำให้คำสั่งเขียนช้าลงและเพิ่มภาระดูแล

## 16. หน่วยที่ 14 — Concurrency และ Isolation

เมื่อหลาย transaction ทำงานพร้อมกัน ระบบต้องกำหนดว่าการอ่านและการเขียนจะมองเห็นกันอย่างไร ปัญหาที่ควรรู้จัก ได้แก่ dirty read, non-repeatable read, phantom read และ lost update ระดับ isolation ที่เข้มขึ้นมักลดความผิดพลาดบางประเภท แต่แลกกับการรอ lock หรือ throughput ที่ลดลง

ตัวอย่างการป้องกันการซื้อสินค้าที่อาจเหลือไม่พอคือให้ตรวจและแก้ไขใน transaction เดียว โดยใช้เงื่อนไขหรือการล็อกตามที่ระบบรองรับ

```sql
BEGIN;

SELECT product_id, stock_qty
FROM products
WHERE product_id = 4
FOR UPDATE;

UPDATE products
SET stock_qty = stock_qty - 1
WHERE product_id = 4
  AND stock_qty >= 1;

-- ตรวจสอบว่า UPDATE ได้รับผลกระทบหนึ่งแถว
COMMIT;
```

คำสั่ง `FOR UPDATE` เป็นตัวอย่างการล็อกแถวที่เลือกเพื่อประสานงานกับ transaction อื่น แต่รายละเอียด isolation และพฤติกรรมของ lock แตกต่างกันตามระบบฐานข้อมูล จึงต้องอ่านคู่มือของระบบที่ใช้จริงและทดสอบสถานการณ์พร้อมกันก่อนนำไปผลิต

## 17. หน่วยที่ 15 — Stored Function, Trigger และการกำกับกฎ

Stored function ช่วยรวมตรรกะที่ควรทำใกล้ข้อมูล เช่น การคำนวณหรือการตรวจสอบที่ใช้ซ้ำ ส่วน trigger ทำงานอัตโนมัติเมื่อเกิด `INSERT`, `UPDATE` หรือ `DELETE` ตัวอย่างเช่น บันทึกประวัติการเปลี่ยนสถานะคำสั่งซื้อ

```sql
CREATE TABLE order_status_history (
    history_id  INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    order_id    INTEGER NOT NULL REFERENCES orders(order_id),
    old_status  VARCHAR(20),
    new_status  VARCHAR(20) NOT NULL,
    changed_at  TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

ในบทเรียนเบื้องต้นควรให้ผู้เรียนเข้าใจแนวคิดก่อนเรียนไวยากรณ์เฉพาะระบบ เพราะภาษา procedural ของ PostgreSQL, MySQL และ SQL Server แตกต่างกัน Trigger ควรมีขอบเขตเล็ก ตรวจสอบได้ และมีเอกสารชัดเจน เนื่องจากตรรกะที่ซ่อนอยู่ใน trigger อาจทำให้ผู้พัฒนาไม่เห็นผลข้างเคียงจากคำสั่งหลัก

## 18. หน่วยที่ 16 — ความปลอดภัยและ SQL Injection

SQL injection เกิดเมื่อโปรแกรมนำข้อความจากผู้ใช้ไปต่อเป็นคำสั่ง SQL โดยไม่แยกข้อมูลออกจากโค้ด ตัวอย่างที่ไม่ปลอดภัยคือการต่อ string เพื่อสร้างเงื่อนไข login หรือค้นหา วิธีป้องกันหลักคือใช้ parameterized query หรือ prepared statement ให้ไลบรารีส่งคำสั่งและค่าพารามิเตอร์แยกกัน

```text
ไม่ปลอดภัย:
SELECT * FROM users WHERE email = '" + input_email + "'

แนวคิดที่ปลอดภัย:
SELECT * FROM users WHERE email = :email
ส่งค่า input_email ผ่านพารามิเตอร์ของไลบรารีฐานข้อมูล
```

ไม่ควรใช้การ escape string แบบเขียนเองแทน parameterization และควรใช้หลัก least privilege ให้บัญชีของแอปพลิเคชันมีสิทธิ์เฉพาะที่จำเป็น เช่น ระบบรายงานอาจมีสิทธิ์อ่าน แต่ไม่ควรมีสิทธิ์ลบตาราง นอกจากนี้ควรหลีกเลี่ยงการนำรหัสผ่านจริงมาใช้ในห้องเรียนและไม่ควรบันทึกข้อมูลลับในไฟล์ SQL ที่แชร์ต่อสาธารณะ

### แบบฝึกหัดระดับสูง

1. จัดอันดับสินค้าตามราคาในแต่ละหมวดหมู่ และแสดงเฉพาะสามอันดับแรกต่อหมวดหมู่
2. คำนวณยอดขายสะสมรายวันและส่วนต่างจากวันก่อนหน้าโดยใช้ `LAG`
3. สร้าง recursive CTE เพื่อแสดงสายบังคับบัญชาของพนักงานหรือเส้นทางในกราฟ
4. เปรียบเทียบ query plan ก่อนและหลังสร้าง index สำหรับการค้นหาคำสั่งซื้อของลูกค้า
5. จำลอง transaction สองชุดที่แก้ไขสต็อกสินค้าเดียวกัน แล้วอธิบายผลจากการใช้ lock
6. ออกแบบสิทธิ์สำหรับผู้ดูแลระบบ พนักงานขาย และผู้จัดทำรายงาน โดยใช้หลัก least privilege
7. ปรับโค้ดค้นหาสมาชิกที่ต่อ string จาก input ให้เป็น parameterized query ในภาษาที่ผู้เรียนถนัด

# 19. วิธีแก้ปัญหา SQL อย่างเป็นระบบ

การเขียน query ที่ถูกต้องควรเริ่มจากแปลงคำถามภาษาไทยเป็นข้อกำหนดที่ตรวจสอบได้ เช่น “ยอดขายของลูกค้าแต่ละคนในไตรมาสแรก” ต้องระบุว่ารวมเฉพาะสถานะใด ใช้วันที่ใดเป็นเกณฑ์ และต้องแสดงลูกค้าที่ไม่เคยซื้อหรือไม่

จากนั้นกำหนด grain ของผลลัพธ์ก่อนเขียนคำสั่ง เช่น หนึ่งแถวต่อคำสั่งซื้อ หนึ่งแถวต่อลูกค้า หรือหนึ่งแถวต่อวัน หากไม่กำหนด grain อาจเกิดยอดซ้ำจากการ join ตารางที่มีหลายแถวต่อหนึ่ง entity

ตารางตรวจสอบ query ที่แนะนำมีดังนี้

| ขั้นตอน | คำถามที่ควรถาม |
|---|---|
| นิยามผลลัพธ์ | ผลลัพธ์หนึ่งแถวแทนอะไร |
| แหล่งข้อมูล | ต้องใช้ตารางใดและ join ด้วยคอลัมน์ใด |
| ขอบเขต | กรองวันที่ สถานะ หรือประเภทใด |
| การรวม | ต้องใช้ `SUM`, `COUNT`, `AVG` หรือไม่ |
| ค่าไม่มีข้อมูล | ควรแสดงแถวที่ไม่มีคู่ด้วย `LEFT JOIN` หรือไม่ |
| ความถูกต้อง | มียอดซ้ำจาก join หรือค่า `NULL` หรือไม่ |
| ประสิทธิภาพ | query อ่านข้อมูลมากเกินไปหรือใช้ index ได้หรือไม่ |
| ความปลอดภัย | มี input จากผู้ใช้ที่ต้อง bind parameter หรือไม่ |

ตัวอย่างคำถาม “แสดงลูกค้าที่มียอดสั่งซื้อรวมมากกว่า 1,000 บาทในปี 2026” สามารถแตกเป็นขั้นตอน คือคำนวณยอดต่อคำสั่งซื้อ เลือกสถานะที่นับเป็นยอดขาย กรองปี รวมตามลูกค้า และกรองด้วย `HAVING`

```sql
SELECT
    c.customer_id,
    c.full_name,
    SUM(oi.quantity * oi.unit_price) AS total_spent
FROM customers AS c
JOIN orders AS o ON o.customer_id = c.customer_id
JOIN order_items AS oi ON oi.order_id = o.order_id
WHERE o.order_date >= TIMESTAMP '2026-01-01 00:00:00'
  AND o.order_date <  TIMESTAMP '2027-01-01 00:00:00'
  AND o.status IN ('PAID', 'SHIPPED')
GROUP BY c.customer_id, c.full_name
HAVING SUM(oi.quantity * oi.unit_price) > 1000
ORDER BY total_spent DESC;
```

# 20. โครงงานปลายภาค

ให้ผู้เรียนเลือกโดเมนหนึ่ง เช่น ระบบห้องสมุด ระบบคลินิก ระบบลงทะเบียนเรียน ระบบจองห้อง หรือระบบคลังสินค้า แล้วพัฒนา database mini-project ตั้งแต่การวิเคราะห์ความต้องการจนถึงรายงานเชิงวิเคราะห์

### ข้อกำหนดขั้นต่ำ

| องค์ประกอบ | เกณฑ์ขั้นต่ำ |
|---|---|
| การออกแบบ | มีอย่างน้อย 5 ตาราง มี primary key และ foreign key ครบถ้วน |
| ข้อมูลทดสอบ | มีข้อมูลพอให้เห็นกรณีปกติ กรณีไม่มีข้อมูล และกรณีผิดเงื่อนไข |
| SQL พื้นฐาน | มี DDL, `INSERT`, `UPDATE`, `DELETE` และ query พื้นฐาน |
| SQL ระดับกลาง | มี join หลายตาราง aggregate, subquery หรือ CTE และ view อย่างน้อยหนึ่งรายการ |
| SQL ระดับสูง | มี window function หรือ recursive query อย่างน้อยหนึ่งรายการ และมีการวิเคราะห์ plan |
| ความถูกต้อง | ใช้ constraints และ transaction กับกระบวนการสำคัญ |
| ความปลอดภัย | อธิบาย parameterization และสิทธิ์ของผู้ใช้ |
| การนำเสนอ | อธิบายปัญหา schema, query สำคัญ ผลลัพธ์ และข้อจำกัด |

### ลำดับการทำโครงงาน

เริ่มจากเขียน user stories และรายการข้อมูลที่ต้องจัดเก็บ ต่อด้วย ER diagram และ data dictionary จากนั้นแปลงเป็น DDL สร้างข้อมูลทดสอบ และเขียน query ตามกรณีใช้งานจริง ผู้เรียนควรตรวจสอบ query ด้วยตัวอย่างข้อมูลขนาดเล็กก่อน แล้วจึงสร้างข้อมูลมากขึ้นเพื่อทดลองประสิทธิภาพ

รายงานควรอธิบายเหตุผลของการแยกตาราง การเลือก primary key และ foreign key กฎที่ใช้ใน constraints ตลอดจน trade-off ระหว่างความถูกต้อง ความยืดหยุ่น และประสิทธิภาพ ไม่ควรส่งเฉพาะไฟล์ SQL ที่ไม่มีคำอธิบาย

## 21. เกณฑ์ประเมินผลที่แนะนำ

| รายการ | สัดส่วน | แนวพิจารณา |
|---|---:|---|
| แบบฝึกหัดรายสัปดาห์ | 20% | ความถูกต้อง ความสม่ำเสมอ และคำอธิบายเหตุผล |
| สอบภาคทฤษฎี | 15% | แนวคิด relational model, NULL, keys, normalization และ transactions |
| สอบปฏิบัติระดับเริ่มต้น–กลาง | 20% | เขียน query ได้ถูกต้องและอ่านผลลัพธ์ได้ |
| สอบปฏิบัติระดับสูง | 15% | window, CTE, plan, concurrency และ security |
| โครงงานปลายภาค | 25% | schema, constraints, query, performance, security และการนำเสนอ |
| การมีส่วนร่วมในห้องปฏิบัติการ | 5% | การทดสอบ การตั้งคำถาม และการตรวจงานเพื่อน |

สำหรับการให้คะแนน query ไม่ควรดูเพียงผลลัพธ์สุดท้าย เพราะ query ที่ได้คำตอบถูกอาจเกิดจากการ hard-code หรือมีความเสี่ยงด้านความถูกต้อง ควรพิจารณาความชัดเจน การเลือก join การจัดการ `NULL` การป้องกันข้อมูลซ้ำ และความปลอดภัยด้วย

## 22. สรุปแนวคิดสำคัญ

ผู้เรียนระดับเริ่มต้นต้องจำให้ได้ว่า `SELECT` ใช้อ่านข้อมูล `WHERE` ใช้กรองแถว `ORDER BY` ใช้เรียงผลลัพธ์ `GROUP BY` ใช้จัดกลุ่ม และ `HAVING` ใช้กรองกลุ่ม การแก้ไขข้อมูลต้องระวัง `WHERE` และควรทดลองใน transaction เมื่อผลกระทบมีความสำคัญ

ผู้เรียนระดับกลางต้องมองเห็นความสัมพันธ์ระหว่างตารางและกำหนด grain ของผลลัพธ์ก่อนใช้ `JOIN` ต้องเข้าใจความแตกต่างของ `INNER JOIN` และ `LEFT JOIN` รวมถึงการจัดการข้อมูลที่ไม่มีคู่ ขณะเดียวกันควรออกแบบ schema โดยใช้ keys, constraints และ normalization เพื่อให้ข้อมูลสอดคล้อง

ผู้เรียนระดับสูงต้องสามารถอธิบายไม่เพียงว่า query ให้คำตอบอะไร แต่ต้องอธิบายว่าคำนวณอย่างไร ปลอดภัยหรือไม่ ทำงานร่วมกับ transaction อื่นอย่างไร และมีประสิทธิภาพเพียงใด Window functions เหมาะกับการวิเคราะห์แถวที่ยังต้องคงรายละเอียดรายแถว ส่วน `EXPLAIN` ช่วยเชื่อมโยงการเขียน SQL กับการทำงานจริงของระบบฐานข้อมูล

> **หลักคิดสุดท้าย:** SQL ที่ดีไม่ใช่ SQL ที่สั้นที่สุดเสมอไป แต่คือ SQL ที่ให้ผลถูกต้อง อธิบายได้ ปลอดภัย ตรวจสอบได้ และเหมาะสมกับปริมาณข้อมูลกับรูปแบบการใช้งานจริง

## References

[1]: https://www.postgresql.org/docs/current/sql-select.html "PostgreSQL 18 Documentation: SELECT"

[2]: https://www.postgresql.org/docs/current/ddl-constraints.html "PostgreSQL 18 Documentation: Constraints"

[3]: https://www.postgresql.org/docs/current/tutorial-transactions.html "PostgreSQL 18 Documentation: Transactions"

[4]: https://www.postgresql.org/docs/current/tutorial-window.html "PostgreSQL 18 Documentation: Window Functions"

[5]: https://www.postgresql.org/docs/current/using-explain.html "PostgreSQL 18 Documentation: Using EXPLAIN"

[6]: https://www.postgresql.org/docs/current/indexes.html "PostgreSQL 18 Documentation: Indexes"
