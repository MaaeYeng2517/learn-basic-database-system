# คำสั่ง `SELECT` ในภาษา SQL

คำสั่ง `SELECT` ใช้สำหรับ **สืบค้นหรืออ่านข้อมูล** จากตารางหรือวิวในฐานข้อมูล โดยสามารถเลือกคอลัมน์ กรองข้อมูล เรียงลำดับ จัดกลุ่ม และจำกัดจำนวนแถวที่ต้องการได้

## 1. รูปแบบพื้นฐาน

```sql
SELECT ชื่อคอลัมน์
FROM ชื่อตาราง;
```

ตัวอย่าง

```sql
SELECT product_name, unit_price
FROM products;
```

คำสั่งนี้แสดงคอลัมน์ `product_name` และ `unit_price` จากตาราง `products`

## 2. การเลือกทุกคอลัมน์ด้วย `*`

```sql
SELECT *
FROM products;
```

เครื่องหมาย `*` หมายถึงเลือกทุกคอลัมน์ เหมาะสำหรับการสำรวจข้อมูลเบื้องต้น แต่ในโปรแกรมจริงควรระบุชื่อคอลัมน์ที่ต้องการอย่างชัดเจน

## 3. การตั้งชื่อคอลัมน์ด้วย `AS`

```sql
SELECT
    product_name AS product_name_th,
    unit_price AS price
FROM products;
```

สามารถใช้ `AS` กับนิพจน์คำนวณได้เช่นกัน

```sql
SELECT
    product_name,
    unit_price,
    unit_price * 1.07 AS price_including_tax
FROM products;
```

## 4. การกรองข้อมูลด้วย `WHERE`

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price > 500;
```

ตัวดำเนินการที่ใช้บ่อย ได้แก่

| ตัวดำเนินการ | ความหมาย |
|---|---|
| `=` | เท่ากับ |
| `<>` หรือ `!=` | ไม่เท่ากับ |
| `>` | มากกว่า |
| `<` | น้อยกว่า |
| `>=` | มากกว่าหรือเท่ากับ |
| `<=` | น้อยกว่าหรือเท่ากับ |
| `AND` | และ |
| `OR` | หรือ |
| `NOT` | ไม่ |
| `IN` | อยู่ในรายการที่กำหนด |
| `BETWEEN` | อยู่ระหว่างช่วงที่กำหนด |
| `LIKE` | ค้นหาตามรูปแบบข้อความ |

ตัวอย่างการใช้หลายเงื่อนไข

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price >= 300
  AND stock_qty > 0;
```

## 5. การใช้ `IN`

```sql
SELECT full_name, city
FROM customers
WHERE city IN ('กรุงเทพฯ', 'เชียงใหม่');
```

คำสั่งนี้แสดงลูกค้าที่อยู่ในกรุงเทพฯ หรือเชียงใหม่

## 6. การใช้ `BETWEEN`

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price BETWEEN 100 AND 700;
```

คำสั่งนี้แสดงสินค้าที่มีราคาอยู่ระหว่าง 100 ถึง 700 โดยทั่วไปจะรวมค่าขอบทั้งสองด้านด้วย

## 7. การค้นหาข้อความด้วย `LIKE`

```sql
SELECT product_name
FROM products
WHERE product_name LIKE '%SQL%';
```

สัญลักษณ์ที่ใช้ร่วมกับ `LIKE` มีดังนี้

| สัญลักษณ์ | ความหมาย |
|---|---|
| `%` | ตัวอักษรจำนวนศูนย์ตัวขึ้นไป |
| `_` | ตัวอักษรหนึ่งตัว |

ตัวอย่าง

```sql
-- ขึ้นต้นด้วยคำว่า 'หนังสือ'
SELECT product_name
FROM products
WHERE product_name LIKE 'หนังสือ%';

-- ลงท้ายด้วยคำว่า 'ไร้สาย'
SELECT product_name
FROM products
WHERE product_name LIKE '%ไร้สาย';
```

## 8. การจัดการค่า `NULL`

`NULL` หมายถึงไม่มีค่าหรือไม่ทราบค่า การตรวจสอบ `NULL` ต้องใช้ `IS NULL` หรือ `IS NOT NULL` ไม่ควรใช้ `= NULL`

```sql
SELECT full_name, city
FROM customers
WHERE city IS NULL;
```

```sql
SELECT full_name, city
FROM customers
WHERE city IS NOT NULL;
```

การแทนค่า `NULL` ด้วยข้อความอื่นสามารถใช้ `COALESCE`

```sql
SELECT
    full_name,
    COALESCE(city, 'ไม่ระบุจังหวัด') AS display_city
FROM customers;
```

## 9. การกำจัดข้อมูลซ้ำด้วย `DISTINCT`

```sql
SELECT DISTINCT city
FROM customers;
```

คำสั่งนี้แสดงรายชื่อจังหวัดโดยไม่แสดงค่าซ้ำ

## 10. การเรียงลำดับด้วย `ORDER BY`

```sql
SELECT product_name, unit_price
FROM products
ORDER BY unit_price ASC;
```

- `ASC` เรียงจากน้อยไปมาก และเป็นค่าเริ่มต้น
- `DESC` เรียงจากมากไปน้อย

สามารถเรียงด้วยหลายคอลัมน์ได้

```sql
SELECT product_name, unit_price
FROM products
ORDER BY unit_price DESC, product_name ASC;
```

## 11. การจำกัดจำนวนแถวด้วย `LIMIT`

```sql
SELECT product_name, unit_price
FROM products
ORDER BY unit_price DESC
LIMIT 3;
```

คำสั่งนี้แสดงสินค้าที่มีราคาสูงสุด 3 รายการ การใช้ `LIMIT` ควรใช้ร่วมกับ `ORDER BY` หากต้องการผลลัพธ์ที่แน่นอน

## 12. การข้ามแถวด้วย `OFFSET`

```sql
SELECT product_id, product_name, unit_price
FROM products
ORDER BY product_id
LIMIT 10 OFFSET 20;
```

คำสั่งนี้ข้าม 20 แถวแรก แล้วแสดงข้อมูล 10 แถวถัดไป เหมาะสำหรับการแบ่งหน้าในระบบเว็บ

## 13. การใช้ฟังก์ชันรวมข้อมูล

ฟังก์ชันรวมที่ใช้บ่อย ได้แก่ `COUNT`, `SUM`, `AVG`, `MIN` และ `MAX`

```sql
SELECT
    COUNT(*) AS product_count,
    SUM(stock_qty) AS total_stock,
    AVG(unit_price) AS average_price,
    MIN(unit_price) AS lowest_price,
    MAX(unit_price) AS highest_price
FROM products;
```

## 14. การจัดกลุ่มด้วย `GROUP BY`

```sql
SELECT
    category_id,
    COUNT(*) AS product_count,
    AVG(unit_price) AS average_price
FROM products
GROUP BY category_id;
```

คำสั่งนี้จัดกลุ่มสินค้าตาม `category_id` แล้วนับจำนวนและคำนวณราคาเฉลี่ยของแต่ละกลุ่ม

## 15. การกรองกลุ่มด้วย `HAVING`

```sql
SELECT
    category_id,
    COUNT(*) AS product_count
FROM products
GROUP BY category_id
HAVING COUNT(*) >= 2;
```

ความแตกต่างระหว่าง `WHERE` และ `HAVING` คือ

| คำสั่ง | ใช้สำหรับ |
|---|---|
| `WHERE` | กรองข้อมูลแต่ละแถวก่อนจัดกลุ่ม |
| `HAVING` | กรองข้อมูลหลังจากจัดกลุ่มแล้ว |

## 16. การใช้ `JOIN` ร่วมกับ `SELECT`

```sql
SELECT
    p.product_name,
    c.category_name,
    p.unit_price
FROM products AS p
JOIN categories AS c
    ON c.category_id = p.category_id;
```

คำสั่งนี้เชื่อมตาราง `products` กับ `categories` เพื่อแสดงชื่อสินค้า ชื่อหมวดหมู่ และราคา

## 17. การใช้ Subquery ร่วมกับ `SELECT`

```sql
SELECT product_name, unit_price
FROM products
WHERE unit_price > (
    SELECT AVG(unit_price)
    FROM products
);
```

คำสั่งนี้แสดงสินค้าที่มีราคาสูงกว่าราคาเฉลี่ยของสินค้าทั้งหมด

## 18. รูปแบบคำสั่ง `SELECT` แบบครบถ้วน

```sql
SELECT คอลัมน์
FROM ตาราง
JOIN ตารางอื่น ON เงื่อนไขการเชื่อม
WHERE เงื่อนไขการกรองแถว
GROUP BY คอลัมน์ที่จัดกลุ่ม
HAVING เงื่อนไขการกรองกลุ่ม
ORDER BY คอลัมน์ที่เรียงลำดับ
LIMIT จำนวนแถว;
```

ตัวอย่าง

```sql
SELECT
    c.category_name,
    COUNT(p.product_id) AS product_count,
    AVG(p.unit_price) AS average_price
FROM categories AS c
JOIN products AS p
    ON p.category_id = c.category_id
WHERE p.is_active = TRUE
GROUP BY c.category_id, c.category_name
HAVING AVG(p.unit_price) > 300
ORDER BY average_price DESC
LIMIT 5;
```

## 19. ลำดับการประมวลผลเชิงตรรกะ

แม้จะเขียนคำสั่งโดยเริ่มจาก `SELECT` แต่ระบบจะพิจารณาส่วนต่าง ๆ ตามลำดับเชิงตรรกะโดยทั่วไปดังนี้

1. `FROM` และ `JOIN` — ระบุแหล่งข้อมูล
2. `WHERE` — กรองแถว
3. `GROUP BY` — จัดกลุ่มแถว
4. `HAVING` — กรองกลุ่ม
5. `SELECT` — สร้างคอลัมน์ผลลัพธ์
6. `DISTINCT` — ตัดข้อมูลซ้ำ
7. `ORDER BY` — เรียงลำดับ
8. `LIMIT` และ `OFFSET` — จำกัดหรือข้ามแถว

## 20. ข้อควรระวัง

1. หากไม่ใช้ `ORDER BY` ไม่ควรสมมติว่าผลลัพธ์จะเรียงตามลำดับใดลำดับหนึ่ง
2. ก่อนใช้ `UPDATE` หรือ `DELETE` ควรทดลองเงื่อนไขด้วย `SELECT` ก่อน
3. การเปรียบเทียบกับ `NULL` ต้องใช้ `IS NULL` หรือ `IS NOT NULL`
4. ควรระบุชื่อคอลัมน์แทนการใช้ `SELECT *` ในโปรแกรมจริง
5. เมื่อใช้ `JOIN` หลายตาราง ควรตรวจสอบว่ามีข้อมูลซ้ำจากความสัมพันธ์แบบหนึ่งต่อหลายหรือไม่
6. ควรใช้ parameterized query เมื่อค่าที่นำมาใช้ในเงื่อนไขมาจากผู้ใช้

## 21. แบบฝึกหัด

1. แสดงชื่อสินค้าและราคาสินค้าที่มีราคาต่ำกว่า 300 บาท
2. แสดงลูกค้าที่อยู่ในกรุงเทพฯ หรือเชียงใหม่
3. แสดงสินค้าที่มีจำนวนคงเหลือน้อยกว่า 10 ชิ้น
4. แสดงชื่อหมวดหมู่โดยไม่ให้มีข้อมูลซ้ำ
5. แสดงสินค้าที่มีราคาสูงสุด 5 รายการ
6. แสดงจำนวนสินค้าในแต่ละหมวดหมู่
7. แสดงหมวดหมู่ที่มีสินค้ามากกว่าหรือเท่ากับ 2 รายการ
8. แสดงชื่อลูกค้าและคำสั่งซื้อทั้งหมดโดยใช้ `JOIN`
9. แสดงสินค้าที่มีราคาสูงกว่าราคาเฉลี่ยของสินค้าทั้งหมด
10. แสดงสินค้าที่มีสถานะพร้อมขาย โดยมี `is_active = TRUE` และ `stock_qty > 0`

## สรุป

คำสั่ง `SELECT` เป็นพื้นฐานสำคัญของ SQL ผู้เรียนควรฝึกใช้ `FROM`, `WHERE`, `ORDER BY`, `GROUP BY`, `HAVING`, `JOIN`, `LIMIT` และฟังก์ชันรวมข้อมูลให้คล่อง ก่อนพัฒนาไปสู่ subquery, CTE, window functions และการเพิ่มประสิทธิภาพ query

## เอกสารอ้างอิง

[1]: https://www.postgresql.org/docs/current/sql-select.html "PostgreSQL Documentation: SELECT"

[2]: https://www.postgresql.org/docs/current/tutorial-select.html "PostgreSQL Documentation: Querying a Table"
