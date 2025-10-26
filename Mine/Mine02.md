
# 🎬 SQL Practice: Aggregates, Joins & Subqueries

## 1️⃣ Count the number of movies released in the last 2 months of the year

### Step 1: Check the criteria

```sql
-- Alternative 1
SELECT *
FROM Movie
WHERE MONTH(MovieRelDate) = 11 OR MONTH(MovieRelDate) = 12;

-- Alternative 2
SELECT *
FROM Movie
WHERE MONTH(MovieRelDate) IN (11, 12);

-- Alternative 3
SELECT *
FROM Movie
WHERE MONTH(MovieRelDate) > 10;  -- Works since months only go up to 12
```

### Step 2: Apply the aggregate function

```sql
SELECT COUNT(MovieID) AS "MoviesReleasedNovDec"
FROM Movie
WHERE MONTH(MovieRelDate) IN (11, 12);
```

---

## 2️⃣ Display actor names, their highest salary, and total salary with a 20% pay rise

### Alternative 1 – Using Equijoin

```sql
SELECT ActorName,
       CONCAT('$', MAX(Salary)) AS "MostEarnt",
       CONCAT('$', ROUND((SUM(Salary) * 1.2), 2)) AS "TotalSalaryWithPayRise"
FROM Actor a, Acts ac
WHERE a.ActorID = ac.ActorID
GROUP BY a.ActorID;
```

### Alternative 2 – Using JOIN ON

```sql
SELECT ActorName,
       CONCAT('$', MAX(Salary)) AS "MostEarnt",
       CONCAT('$', ROUND((SUM(Salary) * 1.2), 2)) AS "TotalSalaryWithPayRise"
FROM Actor a
JOIN Acts ac ON a.ActorID = ac.ActorID
GROUP BY a.ActorID;
```

### Alternative 3 – Using JOIN USING

```sql
SELECT ActorName,
       CONCAT('$', MAX(Salary)) AS "MostEarnt",
       CONCAT('$', ROUND((SUM(Salary) * 1.2), 2)) AS "TotalSalaryWithPayRise"
FROM Actor
JOIN Acts USING (ActorID)
GROUP BY ActorID;
```

---

## 3️⃣ Display actors who earned more than $600,000 in a movie

```sql
SELECT ActorName,
       CONCAT('$', MAX(Salary)) AS "MostEarnt"
FROM Actor a
JOIN Acts ac ON a.ActorID = ac.ActorID
GROUP BY ActorID
HAVING MAX(Salary) > 600;
```

---

## 4️⃣ Display director details and total salary paid (excluding those below $1,000,000)

### Alternative 1 – With JOIN

```sql
SELECT DirectorName, DirectorIsActive, DirectorNumMovies,
       CONCAT('$', SUM(Salary)) AS "TotalSalaryPaid"
FROM Director
JOIN Movie USING (DirectorID)
JOIN Acts USING (MovieID)
GROUP BY DirectorID
HAVING SUM(Salary) > 1000;
```

### Alternative 2 – Using Equijoin

```sql
SELECT DirectorName, DirectorIsActive, DirectorNumMovies,
       CONCAT('$', SUM(Salary)) AS "TotalSalaryPaid"
FROM Director d, Movie m, Acts ac
WHERE d.DirectorID = m.DirectorID
  AND m.MovieID = ac.MovieID
GROUP BY d.DirectorID
HAVING SUM(Salary) >= 1000;
```

### Alternative 3 – Group by non-aggregate columns

```sql
SELECT DirectorName, DirectorIsActive, DirectorNumMovies,
       CONCAT('$', SUM(Salary)) AS "TotalSalaryPaid"
FROM Director d, Movie m, Acts ac
WHERE d.DirectorID = m.DirectorID
  AND m.MovieID = ac.MovieID
GROUP BY DirectorName, DirectorIsActive, DirectorNumMovies
HAVING SUM(Salary) >= 1000;
```

---

## 5️⃣ Display actors with more than 1 movie and never paid less than $400,000

```sql
SELECT ActorName,
       COUNT(MovieID) AS "NumMovies"
FROM Actor a, Acts ac
WHERE a.ActorID = ac.ActorID
GROUP BY a.ActorID
HAVING MIN(Salary) >= 400
   AND COUNT(MovieID) > 1;
```

---

## 6️⃣ Directors of movies released in May

### (a) Using Join

```sql
SELECT DirectorName
FROM Director d, Movie m
WHERE d.DirectorID = m.DirectorID
  AND MONTH(MovieRelDate) = 5;
```

### (b) Using Subquery

```sql
SELECT DirectorName
FROM Director
WHERE DirectorID IN (
    SELECT DirectorID
    FROM Movie
    WHERE MONTH(MovieRelDate) = 5
);
```

---

## 7️⃣ Movies where director’s name starts with 'A' and year ends with '8'

### (a) Join version 1 – 8 as the 4th character

```sql
SELECT MovieName
FROM Director d
JOIN Movie m ON d.DirectorID = m.DirectorID
WHERE DirectorName LIKE 'a%'
  AND MovieRelDate LIKE '___8%';
```

### (a) Join version 2 – 8 is last digit in year

```sql
SELECT MovieName
FROM Director d
JOIN Movie m ON d.DirectorID = m.DirectorID
WHERE DirectorName LIKE 'a%'
  AND YEAR(MovieRelDate) LIKE '%8';
```

### (b) Subquery version

```sql
SELECT MovieName
FROM Movie
WHERE YEAR(MovieRelDate) LIKE '%8'
  AND DirectorID IN (
      SELECT DirectorID
      FROM Director
      WHERE DirectorName LIKE 'a%'
  );
```

💡 **Difference:**
A *join* combines tables first, whereas a *subquery* runs one query inside another — often more readable but sometimes slower.

---

## 8️⃣ Display actors earning more than the average salary

```sql
SELECT DISTINCT ActorName
FROM Actor a, Acts ac
WHERE a.ActorID = ac.ActorID
  AND Salary > (
      SELECT AVG(Salary)
      FROM Acts
  );
```

---

## 9️⃣ Display actors and whether they ever earned above average

```sql
SELECT DISTINCT ActorName,
       IF(MAX(Salary) > (SELECT AVG(Salary) FROM Acts),
          "True", "False") AS "EarnedAboveAverage?"
FROM Actor a, Acts ac
WHERE a.ActorID = ac.ActorID
GROUP BY a.ActorID;
```

---

## 🔟 Salary comparison with average salary (difference shown)

### Alternative 1 – Subquery in FROM clause

```sql
SELECT ActorName, RoleName,
       CONCAT('$', Salary) AS "Salary",
       CONCAT('$', ROUND((Salary - temp.AverageSalary), 2)) AS "AboveAvgBy"
FROM Actor a, Acts ac,
     (SELECT AVG(Salary) AS "AverageSalary" FROM Acts) AS temp
WHERE a.ActorID = ac.ActorID;
```

### Alternative 2 – Subquery in SELECT clause

```sql
SELECT ActorName, RoleName,
       CONCAT('$', Salary) AS "Salary",
       CONCAT('$', ROUND((Salary - (SELECT AVG(Salary) FROM Acts)), 2)) AS "AboveAvgBy"
FROM Actor a, Acts ac
WHERE a.ActorID = ac.ActorID;
```

### Alternative 3 – Correlated Subquery

```sql
SELECT ActorName, RoleName,
       Salary - (
           SELECT AVG(Salary)
           FROM Acts a1
           WHERE a1.ActorID = a2.ActorID
           GROUP BY ActorID
       ) AS "DifferenceFromAverage"
FROM Actor a2
JOIN Acts USING (ActorID);
```

---
