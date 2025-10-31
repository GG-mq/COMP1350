# 🧠 Skill Exercise Test — SQL Practice

## 🧩 Part 1

### 🪪 1. Consent
No SQL query for this part.

---

### 🧮 2. Members with Points Above 100

```sql
SELECT JoinDate, Name
FROM Member
WHERE CurrentPoints > 100;
````

---

### 🔠 3. Members Whose Name Contains “g”

```sql
SELECT Name, CurrentPoints
FROM Member
WHERE Name LIKE '%g%';
```

---

### 🐈 4. Cats by Weight and Adoption Age

```sql
SELECT FurPattern, AdoptionDate
FROM Cat
WHERE Weight <= 4 AND AdoptionAge >= 2;
```

---

### ☕ 5. Drinks Served with Price > 4.5

```sql
SELECT l.Address, d.DrinkName
FROM Location l, Drink d
WHERE l.LocationID = d.ServedLocationID 
  AND d.Price > 4.5
ORDER BY d.Price ASC;
```

---

### 👩‍💼 6. Shifts ≤ 5 Hours (with Role and Employee Info)

```sql
SELECT sh.StartTime, r.HourlyPay, e.Name
FROM Shift sh, Role r, Employee e
WHERE sh.RoleName = r.RoleName 
  AND sh.EmployeeID = e.EmployeeID 
  AND sh.DurationHrs <= 5
ORDER BY e.ContactNumber ASC;
```

---

## 🧩 Part 2

### 📅 7. Members with Low Points and Bookings

```sql
SELECT m.Name, b.Status, s.StartTime
FROM Member m, Booking b, Session s
WHERE m.MemberID = b.MemberID 
  AND b.SessionID = s.SessionID 
  AND m.CurrentPoints <= 35
ORDER BY s.StartTime DESC;
```

---

### 👥 8. Average Guest Count per Session

```sql
SELECT AVG(s.GuestCount) AS 'Average Session Guests'
FROM Session s
JOIN Shift sh ON s.FacilitatedByShift = sh.ShiftID
WHERE s.StartTime < '2024-01-06 15:18:00' 
   OR sh.DurationHrs > 5;
```

---

### 🍪 9. Average Treat Purchase Quantity

```sql
SELECT t.Name, AVG(tp.Quantity) AS 'Average purchase qty'
FROM Treat t, TreatPurchase tp
WHERE t.TreatID = tp.TreatID 
  AND t.PointsReward > 3
GROUP BY t.TreatID
HAVING AVG(tp.Quantity) > 1
ORDER BY t.Price ASC;
```
