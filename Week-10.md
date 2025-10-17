# 📘 Week 9 SQL Practice – Season, Episode, Actor, Contract Tables

This Markdown file contains all SQL code for **Week 9 tasks** — from schema setup and data insertion to analytical queries using `GROUP BY`, `HAVING`, and joins.

---

## 🧹 1. Drop Existing Tables

```sql
DROP TABLE IF EXISTS Contract, Episode, Season, Actor;
````

---

## 🧱 2. Table Creation

### Season Table

```sql
CREATE TABLE Season (
    SeasonID CHAR(2),
    SeasonName VARCHAR(20),
    PRIMARY KEY (SeasonID)
);
```

### Episode Table

```sql
CREATE TABLE Episode (
    SeasonID CHAR(2),
    EpisodeNo VARCHAR(3),
    AirDate DATE,
    PRIMARY KEY (SeasonID, EpisodeNo),
    FOREIGN KEY (SeasonID) REFERENCES Season(SeasonID)
);
```

### Actor Table

```sql
CREATE TABLE Actor (
    ActorID CHAR(3),
    ActorName VARCHAR(30),
    PRIMARY KEY (ActorID)
);
```

### Contract Table

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

## 🎬 3. Insert Statements

### Season Data

```sql
INSERT INTO Season VALUES ('S1', 'Season 1');
INSERT INTO Season VALUES ('S2', 'Season 2');
INSERT INTO Season VALUES ('S3', 'Season 3');
INSERT INTO Season VALUES ('S4', 'Season 4');
```

### Episode Data

```sql
INSERT INTO Episode VALUES ('S1', 'E1', '2011-04-17');
INSERT INTO Episode VALUES ('S1', 'E2', '2011-04-24');
INSERT INTO Episode VALUES ('S1', 'E3', '2011-05-01');
INSERT INTO Episode VALUES ('S2', 'E1', '2012-04-01');
INSERT INTO Episode VALUES ('S2', 'E2', '2012-04-08');
INSERT INTO Episode VALUES ('S2', 'E3', '2012-04-15');
INSERT INTO Episode VALUES ('S3', 'E2', '2013-04-07');
INSERT INTO Episode VALUES ('S4', 'E1', '2014-04-08');
INSERT INTO Episode VALUES ('S4', 'E10', '2014-06-15');
INSERT INTO Episode VALUES ('S4', 'E2', '2014-04-15');
```

### Actor Data

```sql
INSERT INTO Actor VALUES ('A01', 'KIT HARRINGTON');
INSERT INTO Actor VALUES ('A02', 'PETER DINKLAGE');
INSERT INTO Actor VALUES ('A03', 'LENA HEADEY');
INSERT INTO Actor VALUES ('A04', 'MAISIE WILLIAMS');
INSERT INTO Actor VALUES ('A05', 'EMILIA CLARKE');
INSERT INTO Actor VALUES ('A06', 'SOPHIE TURNER');
INSERT INTO Actor VALUES ('A07', 'IWAN RHEON');
INSERT INTO Actor VALUES ('A08', 'SEAN BEAN');
```

### Contract Data

```sql
INSERT INTO Contract VALUES ('C01', 'S1', 'E1', 'A01', 14.34);
INSERT INTO Contract VALUES ('C02', 'S2', 'E2', 'A01', 34.13);
INSERT INTO Contract VALUES ('C03', 'S4', 'E10', 'A03', 43.09);
INSERT INTO Contract VALUES ('C04', 'S3', 'E2', 'A02', 35.34);
INSERT INTO Contract VALUES ('C05', 'S2', 'E3', 'A04', 47.23);
INSERT INTO Contract VALUES ('C06', 'S2', 'E2', 'A01', 24.26);
INSERT INTO Contract VALUES ('C07', 'S3', 'E2', 'A05', 8.54);
INSERT INTO Contract VALUES ('C08', 'S4', 'E1', 'A04', 41.08);
INSERT INTO Contract VALUES ('C09', 'S1', 'E1', 'A08', 24.54);
INSERT INTO Contract VALUES ('C10', 'S4', 'E2', 'A05', 15.14);
```

---

## 🧮 4. Task 1 – Counting Records

### Count the Number of Seasons

```sql
SELECT COUNT(SeasonID) AS 'Season Count'
FROM Season;
```

### Count the Number of Actors

```sql
SELECT COUNT(ActorID) AS 'Number of Actors'
FROM Actor;
```

### Count Actors Whose Names Contain 'S'

```sql
SELECT COUNT(ActorID) AS 'Number of Actors'
FROM Actor
WHERE ActorName LIKE '%S%';
```

---

## 📊 5. Task 2 – Counting and Grouping Contracts

### Count the Number of Contracts

```sql
SELECT COUNT(*) FROM Contract;
```

**`COUNT(*)`** counts **all rows**, including those with `NULL` values.
Replacing it with `COUNT(ContractID)` gives the same result if all contracts have IDs.

---

### Count Contracts per Actor (with alias)

```sql
SELECT ActorID, COUNT(ContractID) AS ContractCount
FROM Contract
GROUP BY ActorID;
```

> **Explanation:**
> The `GROUP BY` clause is needed because the query contains both an aggregated (`COUNT()`) and a non-aggregated (`ActorID`) column.

---

### Sort Results by Contract Count (Ascending)

```sql
SELECT ActorID, COUNT(ContractID) AS ContractCount
FROM Contract
GROUP BY ActorID
ORDER BY ContractCount ASC;
```

---

### Display Actor ID, Actor Name, and Contract Count

```sql
SELECT ActorID, ActorName, COUNT(ContractID) AS ContractCount
FROM Actor
JOIN Contract USING (ActorID)
GROUP BY ActorID
ORDER BY ContractCount ASC;
```

---

## 🎞️ 6. Task 3 – Episode Counting and Filtering

### Count the Number of Episodes in Each Season

```sql
SELECT SeasonID, COUNT(EpisodeNo) AS EpisodeCount
FROM Episode
GROUP BY SeasonID
ORDER BY SeasonID;
```

---

### Count Episodes for Seasons 1, 2, and 3

```sql
SELECT SeasonID, COUNT(EpisodeNo) AS EpisodeCount
FROM Episode
WHERE SeasonID IN ('S1', 'S2', 'S3')
GROUP BY SeasonID
ORDER BY SeasonID;
```

---

### Sort Results by Episode Count (Descending)

```sql
SELECT SeasonID, COUNT(EpisodeNo) AS EpisodeCount
FROM Episode
WHERE SeasonID IN ('S1', 'S2', 'S3')
GROUP BY SeasonID
ORDER BY EpisodeCount DESC;
```

---

### Include Only Seasons with More Than 2 Episodes

```sql
SELECT SeasonID, COUNT(EpisodeNo) AS EpisodeCount
FROM Episode
WHERE SeasonID IN ('S1', 'S2', 'S3')
GROUP BY SeasonID
HAVING EpisodeCount > 2
ORDER BY EpisodeCount DESC;
```

> 💡 **Note:**
> Use `WHERE` for non-aggregated filters, and `HAVING` for filters on aggregated results.

---

## 📺 7. Task 4 – Counting with Joins

### Insert New Season (Season 5)

```sql
INSERT INTO Season VALUES ('S5', 'Season 5');
```

### Count Episodes per Season (Including Seasons Without Episodes)

```sql
SELECT s.SeasonID, COUNT(EpisodeNo) AS EpisodeCount
FROM Season s
LEFT JOIN Episode e USING (SeasonID)
GROUP BY s.SeasonID;
```

> ✅ **Explanation:**
> The `LEFT JOIN` ensures that **all seasons** are displayed, even if they **don’t have episodes yet**.

---

## 🧩 Summary of SQL Concepts Used

| Concept          | Description                                                |
| ---------------- | ---------------------------------------------------------- |
| **COUNT()**      | Counts rows or specific column values                      |
| **GROUP BY**     | Aggregates data by categories                              |
| **HAVING**       | Filters aggregated results                                 |
| **LEFT JOIN**    | Returns all rows from the left table (even unmatched ones) |
| **LIKE**         | Pattern matching using `%` and `_`                         |
| **Foreign Keys** | Enforce relational integrity between tables                |
