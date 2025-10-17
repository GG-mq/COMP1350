# Week 8 — SQL Workshop

```sql
/*
ONLY USE THE FOLLOWING 2 COMMANDS IF YOU ARE USING A LOCAL CONNECTION
*/

CREATE SCHEMA workshop7;
USE workshop7;
````

---

## 🧩 Table Creation

### **1. Season Table**

```sql
CREATE TABLE Season (
    SeasonID CHAR(2),
    SeasonName VARCHAR(20),
    PRIMARY KEY (SeasonID)
);
```

### **2. Episode Table**

```sql
CREATE TABLE Episode (
    SeasonID CHAR(2),
    EpisodeNo VARCHAR(3),
    AirDate DATE,
    PRIMARY KEY (SeasonID, EpisodeNo),
    FOREIGN KEY (SeasonID) REFERENCES Season(SeasonID)
);
```

### **3. Actor Table**

```sql
CREATE TABLE Actor (
    ActorID CHAR(3),
    ActorName VARCHAR(30),
    PRIMARY KEY (ActorID)
);
```

### **4. Contract Table**

```sql
CREATE TABLE Contract (
    ContractID CHAR(3),
    SeasonID CHAR(2),
    EpisodeNo VARCHAR(3),
    ActorID CHAR(3),
    AirTime DECIMAL(4,2),
    PRIMARY KEY (ContractID),
    FOREIGN KEY (SeasonID, EpisodeNo) REFERENCES Episode(SeasonID, EpisodeNo),
    FOREIGN KEY (ActorID) REFERENCES Actor(ActorID)
);
```

---

## 📥 Insert Statements

### **Season Data**

```sql
INSERT INTO Season VALUES 
('S1', 'Season 1'),
('S2', 'Season 2'),
('S3', 'Season 3'),
('S4', 'Season 4');
```

### **Episode Data**

```sql
INSERT INTO Episode VALUES 
('S1', 'E1', '2011-04-17'),
('S1', 'E2', '2011-04-24'),
('S1', 'E3', '2011-05-01'),
('S2', 'E1', '2012-04-01'),
('S2', 'E2', '2012-04-08'),
('S2', 'E3', '2012-04-15'),
('S3', 'E2', '2013-04-07'),
('S4', 'E1', '2014-04-08'),
('S4', 'E10', '2014-06-15');
```

### **Actor Data**

```sql
INSERT INTO Actor VALUES 
('A01', 'KIT HARRINGTON'),
('A02', 'PETER DINKLAGE'),
('A03', 'LENA HEADEY'),
('A04', 'MAISIE WILLIAMS'),
('A05', 'EMILIA CLARKE'),
('A06', 'SOPHIE TURNER'),
('A07', 'IWAN RHEON'),
('A08', 'SEAN BEAN');
```

### **Contract Data**

```sql
INSERT INTO Contract VALUES
('C01', 'S1', 'E1', 'A01', 14.34),
('C02', 'S2', 'E2', 'A01', 34.13),
('C03', 'S4', 'E10', 'A03', 43.09),
('C04', 'S3', 'E2', 'A02', 35.34),
('C05', 'S2', 'E3', 'A04', 47.23),
('C06', 'S2', 'E2', 'A01', 24.26),
('C07', 'S3', 'E2', 'A05', 8.54),
('C08', 'S4', 'E1', 'A04', 41.08),
('C09', 'S1', 'E1', 'A08', 24.54);
```

---

## 🧮 Queries

### **Task 2: Order Contracts by AirTime**

```sql
SELECT *
FROM Contract
ORDER BY AirTime DESC;
```

### **Task 3: Display All Actor Names**

```sql
SELECT ActorName
FROM Actor;
```

### **Task 4: Display SeasonID and Year of AirDate**

```sql
SELECT SeasonID, YEAR(AirDate)
FROM Episode;
```

#### **Task 4 Extension: Distinct Season and Year**

```sql
SELECT DISTINCT SeasonID, YEAR(AirDate) AS 'Season Year'
FROM Episode;
```

---

## 🎯 Conditional Queries

### **Task 5.1: Contracts for Season 1**

```sql
SELECT ContractID, ActorID
FROM Contract
WHERE SeasonID = 'S1';
```

### **Task 5.2: Contracts Not from Season 1**

```sql
SELECT ContractID, ActorID
FROM Contract
WHERE SeasonID <> 'S1';
```

### **Task 5.3: AirTime Greater Than 35**

```sql
SELECT ContractID, ActorID
FROM Contract
WHERE AirTime > 35;
```

### **Task 5.4: AirTime Less Than or Equal to 20**

```sql
SELECT ContractID, ActorID
FROM Contract
WHERE AirTime <= 20;
```

### **Task 5.5: AirTime Between 20 and 40**

```sql
SELECT ContractID, ActorID
FROM Contract
WHERE AirTime BETWEEN 20 AND 40;
```

---

## 📆 Date-Based Queries

### **Task 6.1: Episodes Aired in April**

```sql
SELECT *
FROM Episode
WHERE MONTH(AirDate) = 4;
```

### **Task 6.2: Episodes from Season 2 and 3**

```sql
SELECT *
FROM Episode
WHERE SeasonID IN ('S2', 'S3');
```

### **Task 6.3: Episodes Not from Season 1 or 2**

```sql
SELECT *
FROM Episode
WHERE SeasonID NOT IN ('S1', 'S2');
```

### **Task 6.4: Episodes in April Excluding Season 2**

```sql
SELECT *
FROM Episode
WHERE MONTH(AirDate) = 4 AND SeasonID <> 'S2';
```

```

---

Would you like me to **merge this with your previous Markdown writings** into one `.md` file (like a “COMP Database Workshop Notes.md”) or keep each week separate (e.g., `Week-8.md`, `Week-7.md`, etc.)?
```
