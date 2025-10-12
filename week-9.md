
# 📘 Week - 9 SQL Practical Tasks

This file contains all SQL writing exactly as provided, formatted and beautified for readability and Markdown clarity.  
Each task includes the original SQL statements, comments, and notes for understanding.

---

## 🧩 Task 6

### 📝 Description  
Write an SQL statement that prints the `SeasonID` and the year the season aired. Use **`s`** as the Season table alias.

---

### ✅ Query 1
```sql
SELECT DISTINCT SeasonID, YEAR(AirDate) AS 'Season Year'
FROM Episode;
````

---

### ✅ Query 2

```sql
SELECT DISTINCT Episode.SeasonID, YEAR(e.AirDate) AS 'Season Year'
FROM Episode e, Season s
WHERE e.SeasonID = s.SeasonID; -- join Statement
```

---

### 💡 Observation

If the result has duplicate rows, add the **`DISTINCT`** keyword to remove identical values.
Now, change `s.SeasonID` to `SeasonID` — you’ll get an **ambiguous column name** error.
This happens because both tables contain a column with the same name, and SQL doesn’t know which one to use.

---

## 🧩 Task 7

### 📝 Description

Write an SQL statement that prints the `ContractIDs` and `Names` of actors.
Try different **INNER JOIN** methods to join these tables.

---

### 🔹 Equi Join

```sql
SELECT ContractID, ActorName
FROM Contract c, Actor a
WHERE c.ActorID = a.ActorID;
```

---

### 🔹 Join ON Clause

```sql
SELECT c.ContractID, a.ActorName
FROM contracts c
JOIN actors a ON c.ActorID = a.ActorID;
```

---

### 🔹 Join USING Clause

```sql
SELECT c.ContractID, a.ActorName
FROM contracts c
JOIN actors a USING (ActorID); -- both columns must be named the same
```

---

### ⚠️ Natural Join (Do not use unless specified)

```sql
SELECT ContractID, ActorName
FROM contracts NATURAL JOIN actors;
```

---

## 🧩 Task 8

### 📝 Description

Using a **JOIN** clause, write an SQL statement that prints all contract details for episodes that aired in **2013**.

---

### ✅ Query

```sql
SELECT c.*
FROM Contract c 
JOIN Episode e
ON c.SeasonID = e.SeasonID
AND c.EpisodeNo = e.EpisodeNo
-- on (c.SeasonID, c.EpisodeNo) = (e.SeasonID, e.EpisodeNo)
WHERE YEAR(AirDate) = 2013;
```

---

## 🧩 Task 9

### 📝 Description

Using a **subquery**, write an SQL statement that prints all contract details for episodes that aired in **2013**.

---

### ✅ Query

```sql
SELECT *
FROM Contract
WHERE (SeasonID, EpisodNo) IN 
    (SELECT SeasonID, EpisodeNo
     FROM Episode 
     WHERE YEAR(AirDate) = 2013);
```

---

## 🧩 Task 10

### 📝 Description

Write an SQL statement that prints the **Contract ID**, the **Actor's name**, the **AirDate**, and the **Season's name** for all contracts issued,
where the episode aired after **2013**, for actors with the string `'li'` in their names.

---

### ⚠️ Provided Query

```sql
SELECT ContractID, ActorName, AirDate, SeasonName
FROM Season s 
JOIN Episode e
ON s.SeasonID - e,SeasonID
JOIN Contract c
ON (c.SeasonID, c.EpisodeNo) = (e.EpisodeNo, e.SeasonID)
JOIN Actor a
ON c.ActorID = a.ActorID
WHERE YEAR(AirDate) > 2013
AND ActorName LIKE %li%;
```

---

## 🧩 Task 11

### 📝 Description

Write a query to print the **Episode details** (`SeasonID`, `EpisodeNo`) and the **number of days since the show has aired**.

---

### ✅ Query

```sql
SELECT SeasonID, EpisodeNo, DATEDIFF(CURDATE(), AirDate) AS NumOfDays
FROM Episode;
```

---

## 🧩 Task 12

### 📝 Description

Write a query to print the **names of actors** who have **never acted in any episodes**.

---

### ⚠️ Provided Query

```sql
SELECT ActorName
FROM Actor a 
JOIN Contract c
ON a.ActorID = c.ActorID
WHERE ContractID IS NULL;
```

---

## 🏁 End of Week 9 SQL Tasks

```
```
