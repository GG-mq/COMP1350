# Mine 01 — SQL Workshop: Customer, Orders, and Products

This file contains all SQL table creation scripts, insert statements, and tasks for managing customers, products, orders, and order details.

---

## 🏗️ Table Creation

### **1. Customer Table**
```sql
CREATE TABLE Customer (
    CustomerID CHAR(3) PRIMARY KEY,
    CustomerName VARCHAR(40) NOT NULL,
    CustomerEmail VARCHAR(70) NOT NULL,
    CustomerAddress VARCHAR(150)
    -- Alternative way to declare primary key useful for multiple PKs
);
````

```sql
-- Show table structure
DESCRIBE Customer;
```

---

### **2. Product Table**

```sql
CREATE TABLE Product (
    ProductID CHAR(3) PRIMARY KEY,
    ProductName VARCHAR(20) NOT NULL,
    ProductPrice DECIMAL(6,2) NOT NULL
);
```

---

### **3. Customer Order Table**

```sql
CREATE TABLE CusOrder (
    OrderID CHAR(6) PRIMARY KEY,
    OrderDate DATE NOT NULL,
    OrderTotalCost DECIMAL(8,2) NOT NULL,
    CustomerID CHAR(3),
    FOREIGN KEY(CustomerID) REFERENCES Customer(CustomerID)
    -- Points to the Customer table's primary key
);
```

---

### **4. Order_Has_Product Table**

```sql
CREATE TABLE Order_Has_Product (
    OrderID CHAR(6),
    ProductID CHAR(3),
    Quantity INT NOT NULL,
    PRIMARY KEY(OrderID, ProductID),
    FOREIGN KEY(OrderID) REFERENCES CusOrder(OrderID),
    FOREIGN KEY(ProductID) REFERENCES Product(ProductID)
);
```

---

## 📥 Insert Statements

### **Customer Table Values**

```sql
INSERT INTO Customer VALUES 
('C01','Stanley Kubrick','stanley.kubrick@hotmail.com','19/99 ABC Road, DEF 8383'),
('C02','James Cameron','james.cameron@yahoo.com',NULL),
('C03','Steven Spielberg','steven.spielberg@hotmail.com',NULL),
('C04','Susanne Bier','susanne.bier@gmail.com',NULL),
('C05','Alfred Hitchcock','alfred.hitchcock@gmail.com','27 Tjsj Street, Ryde, 1980');
```

### **Product Table Values**

```sql
INSERT INTO Product VALUES 
('P01','Psycho',123.22),
('P02','Titanic',55.55),
('P03','Avatar',76.99),
('P04','Schindler''s List',150.00),
('P05','In a Better World',101.56);
```

### **CusOrder Table Values**

```sql
INSERT INTO CusOrder VALUES
('CO0001','2022-01-14',12345.56,'C01'),
('CO0002','2022-12-21',23493.43,'C02'),
('CO0003','2022-07-12',49038.22,'C03'),
('CO0004','2019-11-29',272829.00,'C01'),
('CO0005','2019-02-22',943746.00,'C04'),
('CO0006','2012-04-01',95738.22,'C05'),
('CO0007','2012-10-22',65758.09,'C01'),
('CO0008','2012-10-03',5738.99,'C02');
```

### **Order_Has_Product Table Values**

```sql
INSERT INTO Order_Has_Product VALUES
('CO0001','P01',2),
('CO0002','P02',50),
('CO0003','P02',22),
('CO0002','P01',20),
('CO0004','P03',10),
('CO0007','P05',12),
('CO0001','P04',5);
```

---

## 🧩 Tasks

### **Task 3: Add extra records**

```sql
-- Add new order-product record
INSERT INTO Order_Has_Product VALUES ('CO0005', 'P02', 10);

-- Attempt to add non-existent order record
INSERT INTO Order_Has_Product VALUES ('CO0010', 'P03', 11); 
-- Will fail because 'CO0010' does not exist in CusOrder
```

```sql
-- Add missing CusOrder record
INSERT INTO CusOrder VALUES ('CO0010','2022-09-23',1000.22,'C04');
```

---

### **Task 4: Add and modify column**

```sql
-- Add new column for customer date of birth
ALTER TABLE Customer ADD COLUMN CusDOB DATE;

-- Modify to set NOT NULL and default value
ALTER TABLE Customer MODIFY COLUMN CusDOB DATE NOT NULL DEFAULT '2020-01-01';

-- Check table structure
DESCRIBE Customer;
```

---

### **Task 5: Delete column**

```sql
ALTER TABLE Customer DROP COLUMN CusDOB;
DESCRIBE Customer;
```

---

### **Task 6: Truncate table**

```sql
TRUNCATE Order_Has_Product; -- Removes all records but keeps table structure
```

---

### **Task 7: Drop table**

```sql
DROP TABLE Order_Has_Product; -- Deletes table completely
```

**Difference between TRUNCATE and DROP**:

* `TRUNCATE` removes all rows but keeps the table structure.
* `DROP` removes the table entirely.

---

### **Task 8: Select data**

```sql
SELECT * FROM Customer;

-- Or with renamed columns
SELECT CustomerName AS CName, CustomerAddress AS Address 
FROM Customer;
```

* `*` → select all columns
* `AS` → renames column in the output

```
```
