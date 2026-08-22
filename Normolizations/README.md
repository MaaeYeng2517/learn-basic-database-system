# บทเรียน: บรรทัดฐานของฐานข้อมูล (Database Normalization)

## 1. บทนำ

**บรรทัดฐานของฐานข้อมูล (Database Normalization)** คือกระบวนการออกแบบโครงสร้างฐานข้อมูล โดยการจัดระเบียบข้อมูลให้อยู่ในตารางที่เหมาะสม ลดการเก็บข้อมูลซ้ำซ้อน (Data Redundancy) และลดปัญหาที่อาจเกิดขึ้นเมื่อมีการเพิ่ม แก้ไข หรือลบข้อมูล

Normalization เป็นแนวคิดสำคัญของฐานข้อมูลเชิงสัมพันธ์ (Relational Database) และเกี่ยวข้องโดยตรงกับการออกแบบตาราง ความสัมพันธ์ระหว่างตาราง Primary Key และ Foreign Key

ตัวอย่างฐานข้อมูลที่ไม่ได้ออกแบบอย่างเหมาะสม:

| StudentID | StudentName | CourseID | CourseName  | Instructor |
| --------- | ----------- | -------- | ----------- | ---------- |
| S001      | สมชาย       | C001     | Database    | อาจารย์ A  |
| S001      | สมชาย       | C002     | Programming | อาจารย์ B  |
| S002      | สมหญิง      | C001     | Database    | อาจารย์ A  |

จะเห็นว่า `StudentName`, `CourseName` และ `Instructor` อาจถูกจัดเก็บซ้ำหลายครั้ง

Normalization จึงเข้ามาช่วยแยกข้อมูลให้เป็นโครงสร้างที่เหมาะสม

---

# 2. วัตถุประสงค์การเรียนรู้

เมื่อเรียนจบบทนี้ ผู้เรียนสามารถ

1. อธิบายความหมายของ Database Normalization ได้
2. อธิบายปัญหาของข้อมูลซ้ำซ้อนได้
3. อธิบาย Anomaly ที่เกิดจากการออกแบบฐานข้อมูลที่ไม่เหมาะสมได้
4. เข้าใจ Functional Dependency
5. อธิบาย 1NF, 2NF และ 3NF ได้
6. สามารถแปลงตารางที่ไม่เป็น Normal Form ให้เป็น 1NF, 2NF และ 3NF ได้
7. สามารถกำหนด Primary Key และ Foreign Key สำหรับตารางที่ผ่านการ Normalize ได้
8. สามารถนำหลัก Normalization ไปใช้ในการออกแบบฐานข้อมูลจริงได้

---

# 3. ผลลัพธ์การเรียนรู้

หลังจากเรียนบทนี้ ผู้เรียนควรสามารถ

> **วิเคราะห์ → แยกข้อมูล → กำหนด Key → สร้างความสัมพันธ์ → ลดข้อมูลซ้ำ → ได้ฐานข้อมูลที่มีโครงสร้างเหมาะสม**

ตัวอย่างเช่น

```text
ตารางเดิม
    ↓
วิเคราะห์ข้อมูลซ้ำ
    ↓
หา Functional Dependency
    ↓
1NF
    ↓
2NF
    ↓
3NF
    ↓
สร้าง Primary Key / Foreign Key
    ↓
ฐานข้อมูลที่มีโครงสร้างเหมาะสม
```

---

# 4. ปัญหาของฐานข้อมูลที่ไม่ได้ Normalize

หากออกแบบฐานข้อมูลโดยนำข้อมูลทุกอย่างมาเก็บไว้ในตารางเดียว อาจเกิดปัญหาสำคัญ 3 ประเภท

## 4.1 Update Anomaly

เกิดเมื่อข้อมูลเดียวกันถูกเก็บหลายแห่ง และจำเป็นต้องแก้ไขทุกแห่ง

ตัวอย่าง

| StudentID | StudentName | CourseID | CourseName | Instructor |
| --------- | ----------- | -------- | ---------- | ---------- |
| S001      | สมชาย       | C001     | Database   | อาจารย์ A  |
| S002      | สมหญิง      | C001     | Database   | อาจารย์ A  |

หากเปลี่ยนชื่อวิชา `Database` เป็น `Database Systems`

ต้องแก้ไขหลายแถว

หากแก้ไม่ครบ อาจเกิดข้อมูลไม่สอดคล้องกัน

---

## 4.2 Insert Anomaly

เกิดเมื่อไม่สามารถเพิ่มข้อมูลบางประเภทได้ หากยังไม่มีข้อมูลอีกประเภทหนึ่ง

ตัวอย่าง

ต้องการเพิ่มรายวิชาใหม่

```text
C003 = Artificial Intelligence
```

แต่ยังไม่มีนักศึกษาลงทะเบียน

หากตารางออกแบบโดยรวม Student และ Course ไว้ด้วยกัน อาจไม่สามารถเพิ่มรายวิชาได้อย่างเหมาะสม

---

## 4.3 Delete Anomaly

เกิดเมื่อการลบข้อมูลหนึ่งรายการทำให้ข้อมูลอื่นที่ยังต้องการถูกลบไปด้วย

ตัวอย่าง

| StudentID | StudentName | CourseID | CourseName |
| --------- | ----------- | -------- | ---------- |
| S001      | สมชาย       | C001     | Database   |

ถ้าลบนักศึกษา S001 ออกจากตาราง

ข้อมูลเกี่ยวกับวิชา `Database` ก็อาจหายไปด้วย

---

# 5. แนวคิด Data Redundancy

**Data Redundancy** หมายถึง การจัดเก็บข้อมูลเดียวกันซ้ำหลายครั้งโดยไม่จำเป็น

ตัวอย่าง

```text
S001 สมชาย Database
S002 สมหญิง Database
S003 วิชัย   Database
```

ชื่อวิชา `Database` ถูกเก็บซ้ำ 3 ครั้ง

ถ้ามีนักศึกษา 10,000 คนที่เรียนวิชาเดียวกัน ก็อาจเกิดการเก็บชื่อวิชาซ้ำ 10,000 ครั้ง

Normalization ช่วยลดปัญหานี้ด้วยการแยกข้อมูลออกเป็นตารางที่เหมาะสม

---

# 6. Functional Dependency

Functional Dependency หรือ **การขึ้นต่อกันเชิงฟังก์ชัน** เป็นแนวคิดสำคัญในการทำ Normalization

เขียนในรูปแบบ

```text
A → B
```

อ่านว่า

> A กำหนดค่า B ได้

ตัวอย่าง

```text
StudentID → StudentName
```

หมายความว่า StudentID หนึ่งค่า สามารถระบุ StudentName ได้อย่างแน่นอน

เช่น

```text
S001 → สมชาย
S002 → สมหญิง
```

แต่

```text
StudentName → StudentID
```

อาจไม่เป็นจริงเสมอไป เพราะชื่อคนสามารถซ้ำกันได้

---

# 7. Key กับ Functional Dependency

สมมติว่ามีตาราง

```text
STUDENT
-----------------------------
StudentID
StudentName
Major
Email
```

กำหนดว่า

```text
StudentID → StudentName
StudentID → Major
StudentID → Email
```

ดังนั้น StudentID สามารถกำหนดข้อมูลของนักศึกษาได้

จึงเหมาะที่จะเป็น Primary Key

```text
StudentID = Primary Key
```

---

# 8. Normal Forms

Normal Form ที่พบในการออกแบบฐานข้อมูล ได้แก่

```text
1NF
 ↓
2NF
 ↓
3NF
 ↓
BCNF
 ↓
4NF
 ↓
5NF
```

ในการเรียนพื้นฐาน Database มักเน้น

* 1NF
* 2NF
* 3NF

เพราะเป็นพื้นฐานสำคัญในการออกแบบฐานข้อมูลเชิงสัมพันธ์

---

# 9. First Normal Form (1NF)

## 9.1 ความหมาย

**1NF (First Normal Form)** คือรูปแบบที่ตารางต้องมีคุณสมบัติพื้นฐาน เช่น

1. แต่ละช่องเก็บข้อมูลเพียงค่าเดียว
2. ไม่มีข้อมูลหลายค่าอยู่ในช่องเดียว
3. แต่ละแถวสามารถระบุได้อย่างชัดเจน
4. มี Primary Key ที่เหมาะสม

---

## 9.2 ตัวอย่างที่ไม่เป็น 1NF

| StudentID | StudentName | Phone                  |
| --------- | ----------- | ---------------------- |
| S001      | สมชาย       | 0811111111, 0822222222 |

ช่อง Phone มีข้อมูล 2 ค่า

```text
0811111111
0822222222
```

จึงไม่เป็น Atomic Value

---

## 9.3 การปรับให้เป็น 1NF

สามารถแยกเป็นหลายแถว

| StudentID | StudentName | Phone      |
| --------- | ----------- | ---------- |
| S001      | สมชาย       | 0811111111 |
| S001      | สมชาย       | 0822222222 |

หรือออกแบบเป็นตารางใหม่

### STUDENT

| StudentID | StudentName |
| --------- | ----------- |
| S001      | สมชาย       |

### STUDENT_PHONE

| PhoneID | StudentID | Phone      |
| ------- | --------- | ---------- |
| P001    | S001      | 0811111111 |
| P002    | S001      | 0822222222 |

รูปแบบนี้เหมาะกับการออกแบบฐานข้อมูลมากกว่า

---

# 10. Second Normal Form (2NF)

## 10.1 ความหมาย

ตารางจะเป็น **2NF** เมื่อ

1. เป็น 1NF
2. ไม่มี Partial Dependency

กล่าวง่าย ๆ คือ

> Attribute ที่ไม่ใช่ Key ต้องขึ้นอยู่กับ Primary Key ทั้งหมด ไม่ใช่ขึ้นอยู่กับเพียงบางส่วนของ Composite Key

---

# 11. Composite Key

Composite Key คือ Key ที่ประกอบด้วยหลาย Attribute

ตัวอย่าง

```text
(StudentID, CourseID)
```

ใช้ร่วมกันเพื่อระบุการลงทะเบียน

ตัวอย่าง

| StudentID | CourseID | Grade |
| --------- | -------- | ----- |
| S001      | C001     | A     |
| S001      | C002     | B+    |
| S002      | C001     | A     |

Primary Key คือ

```text
(StudentID, CourseID)
```

---

# 12. Partial Dependency

สมมติว่ามีตาราง

```text
ENROLLMENT
------------------------------------------------
StudentID
CourseID
StudentName
CourseName
Grade
```

Primary Key:

```text
(StudentID, CourseID)
```

Functional Dependency:

```text
StudentID → StudentName

CourseID → CourseName

(StudentID, CourseID) → Grade
```

ปัญหาคือ

```text
StudentName
```

ขึ้นอยู่กับ StudentID เพียงอย่างเดียว

และ

```text
CourseName
```

ขึ้นอยู่กับ CourseID เพียงอย่างเดียว

จึงเกิด Partial Dependency

ดังนั้นตารางนี้ยังไม่เป็น 2NF

---

# 13. การปรับเป็น 2NF

แยกออกเป็น 3 ตาราง

## STUDENT

| StudentID | StudentName |
| --------- | ----------- |
| S001      | สมชาย       |
| S002      | สมหญิง      |

## COURSE

| CourseID | CourseName  |
| -------- | ----------- |
| C001     | Database    |
| C002     | Programming |

## ENROLLMENT

| StudentID | CourseID | Grade |
| --------- | -------- | ----- |
| S001      | C001     | A     |
| S001      | C002     | B+    |
| S002      | C001     | A     |

ความสัมพันธ์

```text
STUDENT
   |
   | 1:N
   ↓
ENROLLMENT
   ↑
   | N:1
COURSE
```

ตอนนี้ข้อมูลของ Student และ Course ไม่จำเป็นต้องเก็บซ้ำใน Enrollment

---

# 14. Third Normal Form (3NF)

## 14.1 ความหมาย

ตารางจะเป็น **3NF** เมื่อ

1. เป็น 2NF
2. ไม่มี Transitive Dependency

กล่าวง่าย ๆ คือ

> Attribute ที่ไม่ใช่ Key ต้องขึ้นอยู่กับ Primary Key โดยตรง ไม่ใช่ขึ้นอยู่กับ Attribute ที่ไม่ใช่ Key ตัวอื่น

---

# 15. Transitive Dependency

ตัวอย่าง

```text
STUDENT
--------------------------------
StudentID
StudentName
DepartmentID
DepartmentName
```

Functional Dependency:

```text
StudentID → StudentName

StudentID → DepartmentID

DepartmentID → DepartmentName
```

ดังนั้น

```text
StudentID → DepartmentID → DepartmentName
```

DepartmentName ไม่ได้ขึ้นกับ StudentID โดยตรง แต่ขึ้นกับ DepartmentID

จึงเกิด Transitive Dependency

---

# 16. การปรับเป็น 3NF

แยกเป็น 2 ตาราง

## STUDENT

| StudentID | StudentName | DepartmentID |
| --------- | ----------- | ------------ |
| S001      | สมชาย       | D01          |
| S002      | สมหญิง      | D02          |

## DEPARTMENT

| DepartmentID | DepartmentName         |
| ------------ | ---------------------- |
| D01          | Computer Science       |
| D02          | Information Technology |

ความสัมพันธ์

```text
DEPARTMENT
    |
    | 1:N
    ↓
STUDENT
```

ตอนนี้

```text
StudentID → DepartmentID
DepartmentID → DepartmentName
```

แต่ DepartmentName ถูกเก็บไว้ในตาราง Department

จึงลด Transitive Dependency

---

# 17. เปรียบเทียบ 1NF, 2NF และ 3NF

| Normal Form | เงื่อนไขสำคัญ                           | ปัญหาที่แก้                            |
| ----------- | --------------------------------------- | -------------------------------------- |
| 1NF         | ค่าในแต่ละช่องต้องเป็น Atomic           | Repeating Group                        |
| 2NF         | เป็น 1NF และไม่มี Partial Dependency    | ข้อมูลขึ้นกับบางส่วนของ Composite Key  |
| 3NF         | เป็น 2NF และไม่มี Transitive Dependency | ข้อมูลขึ้นต่อกันผ่าน Non-Key Attribute |

จำง่าย ๆ:

```text
1NF
↓
"หนึ่งช่อง = หนึ่งค่า"

2NF
↓
"ต้องขึ้นกับ Key ทั้งหมด"

3NF
↓
"ต้องขึ้นกับ Key โดยตรง"
```

---

# 18. ตัวอย่างการ Normalize แบบครบกระบวนการ

กำหนดตารางเริ่มต้น

```text
ORDER
------------------------------------------------------
OrderID
CustomerID
CustomerName
ProductID
ProductName
Price
Quantity
```

ตัวอย่างข้อมูล

| OrderID | CustomerID | CustomerName | ProductID | ProductName | Price | Quantity |
| ------- | ---------- | ------------ | --------- | ----------- | ----: | -------: |
| O001    | C001       | สมชาย        | P001      | Keyboard    |   800 |        2 |
| O001    | C001       | สมชาย        | P002      | Mouse       |   500 |        1 |
| O002    | C002       | สมหญิง       | P001      | Keyboard    |   800 |        1 |

---

## ขั้นที่ 1: วิเคราะห์ Key

สมมติว่า Order หนึ่งรายการสามารถมีหลาย Product

ดังนั้นรายละเอียดสินค้าใช้

```text
(OrderID, ProductID)
```

เป็น Composite Key

---

## ขั้นที่ 2: วิเคราะห์ Dependency

```text
CustomerID → CustomerName

ProductID → ProductName, Price

(OrderID, ProductID) → Quantity
```

พบว่า CustomerName ขึ้นกับ CustomerID

ProductName และ Price ขึ้นกับ ProductID

แต่ Quantity ขึ้นกับ OrderID + ProductID

---

## ขั้นที่ 3: สร้างตาราง 2NF

### CUSTOMER

```text
CustomerID
CustomerName
```

### PRODUCT

```text
ProductID
ProductName
Price
```

### ORDER_DETAIL

```text
OrderID
ProductID
Quantity
```

---

# 19. การออกแบบฐานข้อมูลหลัง Normalize

สามารถออกแบบเป็น

```text
CUSTOMER
----------------------
CustomerID PK
CustomerName


ORDER
----------------------
OrderID PK
CustomerID FK


PRODUCT
----------------------
ProductID PK
ProductName
Price


ORDER_DETAIL
----------------------
OrderID PK, FK
ProductID PK, FK
Quantity
```

ความสัมพันธ์

```text
CUSTOMER
    |
    | 1:N
    ↓
  ORDER
    |
    | 1:N
    ↓
ORDER_DETAIL
    ↑
    | N:1
PRODUCT
```

นี่เป็นรูปแบบที่พบได้บ่อยในระบบขายสินค้า

---

# 20. ตัวอย่าง SQL

## สร้างตาราง Customer

```sql
CREATE TABLE Customer (
    CustomerID VARCHAR(10) PRIMARY KEY,
    CustomerName VARCHAR(100) NOT NULL
);
```

## สร้างตาราง Product

```sql
CREATE TABLE Product (
    ProductID VARCHAR(10) PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL
);
```

## สร้างตาราง Order

```sql
CREATE TABLE Orders (
    OrderID VARCHAR(10) PRIMARY KEY,
    CustomerID VARCHAR(10) NOT NULL,

    FOREIGN KEY (CustomerID)
        REFERENCES Customer(CustomerID)
);
```

## สร้างตาราง Order Detail

```sql
CREATE TABLE OrderDetail (
    OrderID VARCHAR(10),
    ProductID VARCHAR(10),
    Quantity INT NOT NULL,

    PRIMARY KEY (OrderID, ProductID),

    FOREIGN KEY (OrderID)
        REFERENCES Orders(OrderID),

    FOREIGN KEY (ProductID)
        REFERENCES Product(ProductID)
);
```

---

# 21. ข้อดีของ Normalization

## 21.1 ลดข้อมูลซ้ำ

ข้อมูล Customer และ Product ไม่ต้องเก็บซ้ำทุก Order

## 21.2 ลดความผิดพลาด

ลดปัญหาการแก้ไขข้อมูลไม่ครบทุกแถว

## 21.3 เพิ่มความถูกต้องของข้อมูล

ข้อมูลแต่ละประเภทมีแหล่งเก็บที่ชัดเจน

## 21.4 ทำให้โครงสร้างฐานข้อมูลชัดเจน

แต่ละตารางมีหน้าที่เฉพาะ

```text
Customer → ลูกค้า
Product → สินค้า
Order → ใบสั่งซื้อ
OrderDetail → รายละเอียดสินค้า
```

## 21.5 ดูแลระบบง่ายขึ้น

เมื่อแก้ไขข้อมูลประเภทหนึ่ง จะไม่ต้องค้นหาข้อมูลซ้ำหลายแห่ง

---

# 22. ข้อเสียของ Normalization

Normalization ไม่ได้มีแต่ข้อดี

เมื่อแยกข้อมูลออกเป็นหลายตาราง การ Query อาจต้องใช้ JOIN เพิ่มขึ้น

ตัวอย่าง

```sql
SELECT
    c.CustomerName,
    p.ProductName,
    od.Quantity
FROM Customer c
JOIN Orders o
    ON c.CustomerID = o.CustomerID
JOIN OrderDetail od
    ON o.OrderID = od.OrderID
JOIN Product p
    ON od.ProductID = p.ProductID;
```

ดังนั้นการออกแบบฐานข้อมูลจริงต้องพิจารณาทั้ง

```text
Data Integrity
+
Performance
+
Storage
+
Maintainability
```

---

# 23. Normalization กับ Denormalization

## Normalization

เน้น

```text
ลดข้อมูลซ้ำ
ลด Anomaly
เพิ่ม Data Integrity
```

เหมาะกับระบบ Transaction เช่น

* ระบบธนาคาร
* ระบบขายสินค้า
* ระบบทะเบียนนักศึกษา
* ระบบโรงพยาบาล
* ระบบ ERP

---

## Denormalization

คือการนำข้อมูลบางส่วนกลับมาเก็บซ้ำโดยตั้งใจ เพื่อเพิ่มประสิทธิภาพในการอ่านข้อมูล

เหมาะกับบางระบบ เช่น

* Data Warehouse
* Analytics
* Reporting
* Big Data
* ระบบที่อ่านข้อมูลจำนวนมาก

แนวคิดสำคัญคือ

> Normalize เพื่อให้ข้อมูลถูกต้องและมีโครงสร้าง
> Denormalize เมื่อมีเหตุผลด้าน Performance

---

# 24. แบบฝึกหัดที่ 1: วิเคราะห์ 1NF

กำหนดตาราง

| StudentID | StudentName | Courses               |
| --------- | ----------- | --------------------- |
| S001      | สมชาย       | Database, Programming |
| S002      | สมหญิง      | Database              |

### คำถาม

1. ตารางนี้เป็น 1NF หรือไม่
2. เพราะเหตุใด
3. ออกแบบตารางใหม่ให้เป็น 1NF

---

# 25. แบบฝึกหัดที่ 2: วิเคราะห์ 2NF

กำหนดตาราง

```text
ENROLLMENT
----------------------------
StudentID
CourseID
StudentName
CourseName
Grade
```

Primary Key:

```text
(StudentID, CourseID)
```

### คำถาม

1. ตารางนี้เป็น 2NF หรือไม่
2. Attribute ใดเกิด Partial Dependency
3. ควรแยกเป็นตารางใดบ้าง
4. กำหนด Primary Key ของแต่ละตาราง

---

# 26. แบบฝึกหัดที่ 3: วิเคราะห์ 3NF

กำหนดตาราง

```text
EMPLOYEE
--------------------------------
EmployeeID
EmployeeName
DepartmentID
DepartmentName
ManagerName
```

กำหนด Dependency

```text
EmployeeID → EmployeeName
EmployeeID → DepartmentID
DepartmentID → DepartmentName
DepartmentID → ManagerName
```

### คำถาม

1. ตารางนี้เป็น 3NF หรือไม่
2. มี Transitive Dependency หรือไม่
3. ควรแยกตารางอย่างไร
4. กำหนด Primary Key และ Foreign Key

---

# 27. แบบฝึกปฏิบัติการ

## การออกแบบฐานข้อมูลระบบร้านค้า

ให้ผู้เรียนออกแบบฐานข้อมูลสำหรับระบบร้านค้า โดยมีข้อมูล

```text
ลูกค้า
สินค้า
คำสั่งซื้อ
รายละเอียดคำสั่งซื้อ
พนักงาน
ประเภทสินค้า
```

### ขั้นตอนการปฏิบัติ

1. วิเคราะห์ Entity
2. ระบุ Attribute
3. กำหนด Primary Key
4. กำหนด Foreign Key
5. วิเคราะห์ Functional Dependency
6. ตรวจสอบ 1NF
7. ตรวจสอบ 2NF
8. ตรวจสอบ 3NF
9. สร้าง ER Diagram
10. เขียน SQL CREATE TABLE
11. เพิ่มข้อมูลด้วย INSERT
12. ทดลอง SELECT และ JOIN

---

# 28. แบบทดสอบก่อนเรียน

### ข้อ 1

Normalization มีวัตถุประสงค์หลักคืออะไร?

A. เพิ่มขนาดฐานข้อมูล
B. ลดข้อมูลซ้ำและปรับปรุงโครงสร้างข้อมูล
C. เพิ่มจำนวนตารางให้มากที่สุด
D. ลบ Primary Key

**เฉลย:** B

### ข้อ 2

1NF เน้นเรื่องใด?

A. Foreign Key
B. Atomic Value
C. Index
D. Transaction

**เฉลย:** B

### ข้อ 3

Partial Dependency เกี่ยวข้องกับ Normal Form ใด?

A. 1NF
B. 2NF
C. 3NF
D. 5NF

**เฉลย:** B

### ข้อ 4

Transitive Dependency เกี่ยวข้องกับ Normal Form ใด?

A. 1NF
B. 2NF
C. 3NF
D. 4NF

**เฉลย:** C

### ข้อ 5

ข้อใดเป็น Composite Key?

A. StudentID
B. CourseID
C. StudentID + CourseID
D. StudentName

**เฉลย:** C

---

# 29. แบบทดสอบหลังเรียน

### ข้อ 1

ข้อใดอธิบาย 1NF ได้ถูกต้องที่สุด?

A. ทุกตารางต้องมี Foreign Key
B. ทุกช่องควรเก็บค่าที่เป็น Atomic
C. ทุกตารางต้องมี 3 Primary Key
D. ทุกตารางต้องมี Index

**เฉลย:** B

### ข้อ 2

ตารางที่เป็น 1NF แต่มี Partial Dependency ควรปรับเป็นอะไร?

A. 2NF
B. 3NF
C. BCNF
D. 5NF

**เฉลย:** A

### ข้อ 3

ถ้า

```text
StudentID → DepartmentID
DepartmentID → DepartmentName
```

Dependency นี้เป็นลักษณะใด?

A. Partial Dependency
B. Transitive Dependency
C. Multivalued Dependency
D. Functional Index

**เฉลย:** B

### ข้อ 4

ข้อใดเป็นประโยชน์ของ Normalization?

A. เพิ่มข้อมูลซ้ำ
B. ลด Data Redundancy
C. ทำให้ทุก Query ไม่ต้องใช้ JOIN
D. ทำให้ทุกตารางมีข้อมูลมากขึ้น

**เฉลย:** B

### ข้อ 5

ข้อใดเหมาะสมที่สุดสำหรับระบบลงทะเบียนเรียน?

```text
STUDENT
COURSE
ENROLLMENT
```

A. รวมทุกอย่างไว้ตารางเดียว
B. แยกตารางตาม Entity และใช้ Relationship เชื่อมกัน
C. ลบ Primary Key
D. เก็บชื่อวิชาซ้ำในทุกแถว

**เฉลย:** B

---

# 30. สรุปบทเรียน

Normalization คือกระบวนการจัดโครงสร้างฐานข้อมูลเพื่อให้ข้อมูลมีความเป็นระเบียบ ลดข้อมูลซ้ำ และลดปัญหาในการเพิ่ม แก้ไข และลบข้อมูล

แนวคิดสำคัญที่ควรจำ:

```text
Database Normalization
        │
        ├── Data Redundancy
        │
        ├── Functional Dependency
        │
        ├── Primary Key
        │
        ├── Foreign Key
        │
        ├── 1NF
        │     └── Atomic Value
        │
        ├── 2NF
        │     └── No Partial Dependency
        │
        └── 3NF
              └── No Transitive Dependency
```

### สูตรจำง่าย

```text
1NF = หนึ่งช่อง หนึ่งค่า

2NF = ขึ้นกับ Key ทั้งหมด

3NF = ขึ้นกับ Key โดยตรง
```

หรือจำเป็นประโยคว่า

> **“หนึ่งช่องต้องเป็นค่าเดียว → ทุกข้อมูลต้องขึ้นกับ Key ทั้งหมด → และต้องขึ้นกับ Key โดยตรง”**

---

# 31. แนวคิดต่อยอด

หลังจากเข้าใจ 1NF–3NF แล้ว ควรศึกษาเรื่องต่อไปนี้

```text
Normalization
      ↓
Functional Dependency
      ↓
Candidate Key
      ↓
Primary Key / Foreign Key
      ↓
ER Model
      ↓
ER Diagram
      ↓
Relational Schema
      ↓
SQL
      ↓
JOIN
      ↓
Index
      ↓
Transaction
      ↓
Database Design
```

ความเข้าใจ Normalization จะเป็นพื้นฐานสำคัญสำหรับการออกแบบฐานข้อมูลในระบบจริง เช่น **MySQL, PostgreSQL, SQL Server และ Oracle** รวมถึงการออกแบบ Database Schema สำหรับ Web Application และระบบ Enterprise
