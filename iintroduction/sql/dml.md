# กลุ่มคำสั่ง DML ในภาษา SQL

**DML** ย่อมาจาก **Data Manipulation Language** หรือ **ภาษาสำหรับจัดการข้อมูล** ใช้สำหรับเพิ่ม แก้ไข ลบ และผสานข้อมูลที่อยู่ภายในตาราง โดยคำสั่ง DML ที่ใช้บ่อย ได้แก่ `INSERT`, `UPDATE`, `DELETE` และ `MERGE`

> หมายเหตุ: ในบางตำรา `SELECT` อาจถูกจัดรวมอยู่ใน DML แต่ในการเรียนการสอนทั่วไปมักแยก `SELECT` เป็นกลุ่ม DQL หรือ Data Query Language เพื่อให้เห็นความแตกต่างระหว่างการอ่านข้อมูลกับการเปลี่ยนแปลงข้อมูล

## 1. คำสั่งในกลุ่ม DML

| คำสั่ง | หน้าที่ | ตัวอย่างการใช้งาน |
|---|---|---|
| `INSERT` | เพิ่มข้อมูลใหม่ | เพิ่มลูกค้า สินค้า หรือคำสั่งซื้อ |
| `UPDATE` | แก้ไขข้อมูลเดิม | เปลี่ยนราคาและจำนวนสินค้า |
| `DELETE` | ลบข้อมูล | ลบข้อมูลที่ไม่ต้องการ |
| `MERGE` | เพิ่มหรือแก้ไขตามเงื่อนไข | ทำข้อมูลต้นทางให้ตรงกับข้อมูลปลายทาง |

## 2. คำสั่ง `INSERT`

ใช้เพิ่มแถวใหม่ลงในตาราง

### 2.1 เพิ่มข้อมูลหนึ่งแถว

```sql
INSERT INTO customers (full_name, email, city)
VALUES ('กิตติพงษ์ ตั้งใจเรียน', 'kittipong@example.com', 'กรุงเทพฯ');
```

ควรระบุชื่อคอลัมน์ทุกครั้ง เพื่อให้เห็นชัดเจนว่าค่าแต่ละค่าจะถูกบันทึกลงในคอลัมน์ใด

### 2.2 เพิ่มข้อมูลหลายแถว

```sql
INSERT INTO categories (category_name)
VALUES
    ('หนังสือ'),
    ('อุปกรณ์ไอที'),
    ('เครื่องเขียน');
```

### 2.3 เพิ่มข้อมูลโดยใช้ค่าเริ่มต้น

```sql
INSERT INTO products (
    category_id,
    product_name,
    unit_price,
    stock_qty
)
VALUES (
    1,
    'หนังสือ SQL สำหรับผู้เริ่มต้น',
    350.00,
    DEFAULT
);
```

หากคอลัมน์มีค่า `DEFAULT` ระบบจะใช้ค่าที่กำหนดไว้ในโครงสร้างตาราง เช่น จำนวนสินค้าเริ่มต้นหรือวันที่สร้างข้อมูล

### 2.4 เพิ่มข้อมูลจากผลลัพธ์ของ `SELECT`

```sql
INSERT INTO archived_products (
    product_id,
    product_name,
    unit_price
)
SELECT
    product_id,
    product_name,
    unit_price
FROM products
WHERE is_active = FALSE;
```

รูปแบบนี้ใช้คัดลอกข้อมูลที่ตรงกับเงื่อนไขจากตารางหนึ่งไปยังอีกตารางหนึ่ง

### 2.5 ตรวจสอบข้อมูลที่เพิ่มด้วย `RETURNING`

ตัวอย่างนี้เป็นรูปแบบที่ PostgreSQL รองรับ

```sql
INSERT INTO categories (category_name)
VALUES ('อุปกรณ์สำนักงาน')
RETURNING category_id, category_name;
```

`RETURNING` ใช้ส่งค่าของแถวที่ถูกเพิ่มกลับมา เช่น รหัสที่ระบบสร้างให้อัตโนมัติ

## 3. คำสั่ง `UPDATE`

ใช้แก้ไขข้อมูลที่มีอยู่แล้วในตาราง

### 3.1 แก้ไขข้อมูลหนึ่งแถว

```sql
UPDATE products
SET unit_price = 450.00
WHERE product_id = 1;
```

### 3.2 แก้ไขหลายคอลัมน์พร้อมกัน

```sql
UPDATE customers
SET
    full_name = 'กิตติพงษ์ ตั้งใจเรียนมากขึ้น',
    city = 'เชียงใหม่'
WHERE customer_id = 1;
```

### 3.3 แก้ไขข้อมูลหลายแถว

```sql
UPDATE products
SET unit_price = unit_price * 1.10
WHERE category_id = 1;
```

คำสั่งนี้เพิ่มราคาสินค้าในหมวดหมู่ที่มี `category_id = 1` ขึ้น 10%

### 3.4 ใช้เงื่อนไขเพื่อป้องกันข้อมูลผิดพลาด

```sql
UPDATE products
SET stock_qty = stock_qty - 2
WHERE product_id = 3
  AND stock_qty >= 2;
```

เงื่อนไข `stock_qty >= 2` ช่วยป้องกันไม่ให้จำนวนสินค้าติดลบ หากไม่มีสินค้าเพียงพอ ระบบจะไม่แก้ไขแถวดังกล่าว

### 3.5 ตรวจสอบแถวที่ถูกแก้ไขด้วย `RETURNING`

```sql
UPDATE products
SET unit_price = unit_price * 1.05
WHERE product_id = 1
RETURNING product_id, product_name, unit_price;
```

## 4. คำสั่ง `DELETE`

ใช้ลบแถวออกจากตาราง

### 4.1 ลบข้อมูลหนึ่งแถว

```sql
DELETE FROM customers
WHERE customer_id = 4;
```

### 4.2 ลบข้อมูลตามเงื่อนไข

```sql
DELETE FROM products
WHERE is_active = FALSE
  AND stock_qty = 0;
```

คำสั่งนี้ลบสินค้าที่ปิดการขายและไม่มีสินค้าเหลืออยู่

### 4.3 ตรวจสอบแถวที่จะลบก่อนใช้ `DELETE`

ควรใช้ `SELECT` ตรวจสอบก่อนเสมอ

```sql
SELECT product_id, product_name
FROM products
WHERE is_active = FALSE
  AND stock_qty = 0;
```

เมื่อแน่ใจแล้วจึงใช้

```sql
DELETE FROM products
WHERE is_active = FALSE
  AND stock_qty = 0;
```

### 4.4 ลบและส่งข้อมูลที่ถูกลบกลับมา

```sql
DELETE FROM products
WHERE product_id = 10
RETURNING product_id, product_name;
```

## 5. คำสั่ง `MERGE`

`MERGE` ใช้เปรียบเทียบข้อมูลต้นทางกับข้อมูลปลายทาง แล้วเลือกดำเนินการ เช่น แก้ไขข้อมูลเมื่อพบแถวที่ตรงกัน หรือเพิ่มข้อมูลเมื่อไม่พบแถวที่ตรงกัน

ตัวอย่างต่อไปนี้ใช้รูปแบบที่ PostgreSQL รองรับ

```sql
MERGE INTO products AS target
USING new_products AS source
ON target.product_id = source.product_id
WHEN MATCHED THEN
    UPDATE SET
        product_name = source.product_name,
        unit_price = source.unit_price,
        stock_qty = source.stock_qty
WHEN NOT MATCHED THEN
    INSERT (product_id, category_id, product_name, unit_price, stock_qty)
    VALUES (
        source.product_id,
        source.category_id,
        source.product_name,
        source.unit_price,
        source.stock_qty
    );
```

หลักการทำงานคือ หากพบ `product_id` ตรงกัน จะทำการ `UPDATE` แต่หากไม่พบ จะทำการ `INSERT` ทั้งนี้ควรตรวจสอบไวยากรณ์และความสามารถของระบบฐานข้อมูลที่ใช้งาน เนื่องจากรายละเอียดของ `MERGE` อาจแตกต่างกันตามระบบ

## 6. การใช้ DML ร่วมกับ Transaction

คำสั่ง DML สามารถรวมอยู่ใน transaction เพื่อให้การเปลี่ยนแปลงหลายขั้นตอนสำเร็จทั้งหมดหรือยกเลิกทั้งหมด

```sql
BEGIN;

UPDATE products
SET stock_qty = stock_qty - 1
WHERE product_id = 3
  AND stock_qty >= 1;

INSERT INTO order_items (
    order_id,
    product_id,
    quantity,
    unit_price
)
SELECT
    5,
    product_id,
    1,
    unit_price
FROM products
WHERE product_id = 3;

COMMIT;
```

หากเกิดข้อผิดพลาด ให้ใช้ `ROLLBACK`

```sql
BEGIN;

UPDATE products
SET stock_qty = stock_qty - 1
WHERE product_id = 3;

-- หากตรวจพบข้อผิดพลาด
ROLLBACK;
```

| คำสั่ง | หน้าที่ |
|---|---|
| `BEGIN` | เริ่ม transaction |
| `COMMIT` | ยืนยันการเปลี่ยนแปลง |
| `ROLLBACK` | ยกเลิกการเปลี่ยนแปลง |
| `SAVEPOINT` | กำหนดจุดสำหรับย้อนกลับบางส่วน |

## 7. ข้อควรระวังในการใช้ DML

### 7.1 ระวังการลืม `WHERE`

คำสั่งต่อไปนี้จะแก้ไขข้อมูลทุกแถว

```sql
UPDATE products
SET unit_price = 0;
```

และคำสั่งนี้จะลบข้อมูลทุกแถวในตาราง

```sql
DELETE FROM products;
```

จึงควรตรวจสอบเงื่อนไขด้วย `SELECT` ก่อนใช้ `UPDATE` หรือ `DELETE`

### 7.2 ตรวจสอบจำนวนแถวที่ได้รับผลกระทบ

โปรแกรมควรตรวจสอบว่าคำสั่ง DML แก้ไขหรือลบข้อมูลกี่แถว หากคาดว่าจะเปลี่ยนแปลงหนึ่งแถวแต่ได้รับผลกระทบเป็นจำนวนมาก อาจแสดงว่าเงื่อนไขผิดพลาด

### 7.3 ระวังข้อจำกัดของตาราง

การเพิ่มหรือแก้ไขข้อมูลอาจไม่สำเร็จหากละเมิด constraint เช่น

- ใส่ค่า `NULL` ให้คอลัมน์ที่กำหนด `NOT NULL`
- ใส่ค่าซ้ำในคอลัมน์ที่กำหนด `UNIQUE`
- ใส่ค่า foreign key ที่ไม่มีอยู่ในตารางต้นทาง
- ใส่ค่าที่ไม่ผ่านเงื่อนไข `CHECK`

### 7.4 ระวังความสัมพันธ์ระหว่างตาราง

การลบข้อมูลจากตารางหลักอาจทำไม่ได้หากมีตารางอื่นอ้างอิงอยู่ หรืออาจทำให้แถวในตารางลูกถูกลบอัตโนมัติหากกำหนด `ON DELETE CASCADE` ดังนั้นต้องวิเคราะห์ผลกระทบก่อนเสมอ

## 8. การเปรียบเทียบ DML กับกลุ่มคำสั่งอื่น

| กลุ่มคำสั่ง | ชื่อเต็ม | หน้าที่ | ตัวอย่าง |
|---|---|---|---|
| DDL | Data Definition Language | จัดการโครงสร้างฐานข้อมูล | `CREATE`, `ALTER`, `DROP` |
| DML | Data Manipulation Language | จัดการข้อมูลในตาราง | `INSERT`, `UPDATE`, `DELETE`, `MERGE` |
| DQL | Data Query Language | สืบค้นข้อมูล | `SELECT` |
| TCL | Transaction Control Language | ควบคุม transaction | `COMMIT`, `ROLLBACK` |
| DCL | Data Control Language | จัดการสิทธิ์ | `GRANT`, `REVOKE` |

## 9. แบบฝึกหัด

1. เพิ่มข้อมูลลูกค้าใหม่สามคนด้วย `INSERT`
2. เพิ่มสินค้าใหม่โดยใช้ค่า `DEFAULT` สำหรับจำนวนสินค้า
3. เพิ่มราคาสินค้าในหมวดหมู่หนึ่งขึ้น 5%
4. ลดจำนวนสินค้าคงเหลือโดยไม่อนุญาตให้ติดลบ
5. ลบสินค้าที่ปิดการขายและไม่มีสินค้าในคลัง
6. เขียน transaction สำหรับการสร้างคำสั่งซื้อและลดสต็อกสินค้า
7. อธิบายผลกระทบของการใช้ `DELETE` โดยไม่มี `WHERE`
8. อธิบายความแตกต่างระหว่าง `INSERT`, `UPDATE`, `DELETE` และ `MERGE`
9. เขียนคำสั่งที่ตรวจสอบข้อมูลก่อนแก้ไข แล้วใช้ `UPDATE` เปลี่ยนข้อมูลเฉพาะแถวที่ต้องการ
10. ทดลองใช้ `ROLLBACK` เพื่อยกเลิกคำสั่ง `INSERT` และ `UPDATE`

## สรุป

กลุ่มคำสั่ง **DML** ใช้จัดการข้อมูลภายในตาราง โดย `INSERT` ใช้เพิ่มข้อมูล `UPDATE` ใช้แก้ไขข้อมูล `DELETE` ใช้ลบข้อมูล และ `MERGE` ใช้เพิ่มหรือแก้ไขข้อมูลตามเงื่อนไข การใช้ DML อย่างปลอดภัยควรตรวจสอบข้อมูลก่อนดำเนินการ ใช้ `WHERE` อย่างรอบคอบ ตรวจสอบ constraint และใช้ transaction กับการเปลี่ยนแปลงที่สำคัญ

## เอกสารอ้างอิง

[1]: https://www.postgresql.org/docs/current/dml.html "PostgreSQL Documentation: Data Manipulation"

[2]: https://www.postgresql.org/docs/current/sql-insert.html "PostgreSQL Documentation: INSERT"

[3]: https://www.postgresql.org/docs/current/sql-update.html "PostgreSQL Documentation: UPDATE"

[4]: https://www.postgresql.org/docs/current/sql-delete.html "PostgreSQL Documentation: DELETE"

[5]: https://www.postgresql.org/docs/current/sql-merge.html "PostgreSQL Documentation: MERGE"
