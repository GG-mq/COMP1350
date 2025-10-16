# 🎬 Week 6 SQL Practice – Actor & Acts Tables

This Markdown document organizes and beautifies your SQL script for schema creation, table management, data insertion, and query practice.

---

## 🏗️ 1. Schema Setup

```sql
-- Create and use a new schema
CREATE SCHEMA week6_practice;
USE week6_practice;
````

---

## 🧹 2. Dropping or Renaming Tables

### Drop Tables

```sql
DROP TABLE IF EXISTS Actor;
DROP TABLE IF EXISTS Acts;
```

### Rename Tables

```sql
ALTER TABLE Actor RENAME TO Actor_old;
ALTER TABLE Acts RENAME TO Acts_old;
```

---

## 🧱 3. Table Creation

### Actor Table

```sql
CREATE TABLE Actor (
    ActorID CHAR(3),
    ActorName VARCHAR(30),
    MentorID CHAR(3),
    PRIMARY KEY (ActorID)
);
```

### Acts Table

```sql
CREATE TABLE Acts (
    ActorID CHAR(3),
    MovieID CHAR(3),
    RoleName VARCHAR(30),
    Salary DECIMAL(5,2),
    PRIMARY KEY (ActorID, MovieID)
);
```

---

## 🎭 4. Inserting Values

### Actor Records

```sql
INSERT INTO Actor VALUES ('A01', 'Leonardo DiCaprio', NULL);
INSERT INTO Actor VALUES ('A02', 'Margot Robbie', 'A01');
INSERT INTO Actor VALUES ('A03', 'Brad Pitt', 'A01');
INSERT INTO Actor VALUES ('A04', 'Emma Stone', 'A03');
INSERT INTO Actor VALUES ('A05', 'Ryan Gosling', 'A03');
INSERT INTO Actor VALUES ('A06', 'Tom Hardy', 'A01');
INSERT INTO Actor VALUES ('A07', 'Saoirse Ronan', 'A04');
INSERT INTO Actor VALUES ('A08', 'Robert De Niro', NULL);
INSERT INTO Actor VALUES ('A09', 'James Stewart', NULL);
INSERT INTO Actor VALUES ('A10', 'David Oyelowo', 'A08');
INSERT INTO Actor VALUES ('A11', 'Rupert Grint', NULL);
```

### Acts Records

```sql
INSERT INTO Acts (ActorID, MovieID, RoleName, Salary) VALUES 
    ('A01', 'M01', 'Dom Cobb', 534.50),
    ('A06', 'M02', 'Farrier', 790.75),
    ('A07', 'M03', 'Christine McPherson', 431.00),
    ('A07', 'M04', 'Jo March', 986.50),
    ('A01', 'M05', 'Jordan Belfort', 125.25),
    ('A08', 'M06', 'Frank Sheeran', 679.00),
    ('A09', 'M07', 'John Ferguson', 245.75),
    ('A09', 'M08', 'Sam Loomis', 542.25),
    ('A10', 'M09', 'Martin Luther King Jr.', 765.50),
    ('A04', 'M10', 'Mrs. Whatsit', 346.75),
    ('A11', 'M11', 'Alan A. Allen', 568.25),
    ('A02', 'M05', 'Naomi Lapaglia', 255.75),
    ('A07', 'M10', 'Jo March', 750.00);
```

---

## ⚙️ 5. DDL (Data Definition Language)

### Common MySQL Data Types

* `DECIMAL`
* `VARCHAR`
* `CHAR`
* `FLOAT`

### Suitable Data Types

* **Actor Table:** `CHAR`, `VARCHAR`
* **Acts Table:** `CHAR`, `VARCHAR`, `DECIMAL`

### Create Table with a Single-Column Primary Key

```sql
ActorID CHAR(8) PRIMARY KEY
```

### Create Table with a Composite Primary Key

```sql
PRIMARY KEY (ActorID, MovieID)
```

### Add a New Column

```sql
ALTER TABLE Actor ADD DOB DATE;
```

### Remove a Column

```sql
ALTER TABLE Actor DROP COLUMN MentorID;
```

### Remove the Table Completely

```sql
DROP TABLE Actor;
```

---

## ✍️ 6. DML (Data Manipulation Language)

### Insert a Record

```sql
INSERT INTO table_name (col1, col2, ...) VALUES (val1, val2, ...);
```

### Insert into Acts

```sql
INSERT INTO Acts VALUES (val1, val2, ...);
```

### Update a Record

```sql
UPDATE table_name
SET col1 = val1, ...
WHERE condition;
```

### Delete a Record

```sql
DELETE FROM table_name
WHERE condition;
```

---

## 🔍 7. DQL (Data Query Language)

### 1. Display all actor details

```sql
SELECT * FROM Actor;
```

### 2. Display only actor names

```sql
SELECT ActorName FROM Actor;
```

### 3. Display roles and salaries, sorted by salary

```sql
SELECT RoleName, Salary AS 'Payment for the role'
FROM Acts
ORDER BY Salary ASC;
```

### 4. Display roles with salary > 500

```sql
SELECT RoleName
FROM Acts
WHERE Salary > 500;
```

### 5. Display unique ActorIDs with salary ≤ 300

```sql
SELECT DISTINCT ActorID
FROM Acts
WHERE Salary <= 300;
```

### 6. Display roles with salary between 300 and 600

```sql
SELECT RoleName
FROM Acts
WHERE Salary BETWEEN 300 AND 600;
```

### 7. Display roles not in movies M02 and M05

```sql
SELECT RoleName
FROM Acts
WHERE MovieID NOT IN ('M02', 'M05');
```

### 8. Display actors whose names begin with 'L'

```sql
SELECT ActorName
FROM Actor
WHERE ActorName LIKE 'L%';
```

### 9. Display actors whose names end with 't', sorted by MentorID

```sql
SELECT ActorName
FROM Actor
WHERE ActorName LIKE '%t'
ORDER BY MentorID ASC;
```

### 10. Display actors with 't' as the third letter in their name

```sql
SELECT ActorName
FROM Actor
WHERE ActorName LIKE '__t%';
```

### 11. Display actors without a mentor

```sql
SELECT *
FROM Actor
WHERE MentorID IS NULL;
```

### 12. Display roles in movie M05 with salary > 200, formatted

```sql
SELECT RoleName,
       CONCAT(Salary, ' thousand USD') AS 'Payment'
FROM Acts
WHERE MovieID = 'M05' AND Salary > 200;
```
