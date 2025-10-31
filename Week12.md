Here’s your explanation beautifully formatted in Markdown (`.md`) style — clear, structured, and ready to include in your notes or documentation:

---

```md
# 🧩 Database Normalisation (Bottom-Up Approach)

**Normalisation** is the process of decomposing a large table into smaller, logically structured tables to reduce redundancy and improve data integrity.

---

## ⚙️ Overview
Normalisation is performed **bottom-up**, meaning we start with one large unnormalised table and gradually refine it through multiple stages (normal forms).

---

## 🧱 Types of Normal Forms

### 🔹 **UNF – Unnormalised Form**
- Data may contain **Multi-Valued Attributes (MVAs)**.  
- To move to **1NF**, we **dissolve MVAs** — meaning, we ensure each field contains only atomic (single) values.

---

### 🔹 **1NF – First Normal Form**
- Each column holds **atomic values** only.  
- A **composite primary key** (PK made of multiple columns) may exist.  
- May still contain **partial dependencies** — where a non-key attribute depends only on part of a composite PK.  
- ✅ **To move to 2NF:** remove partial dependencies by creating new tables.

---

### 🔹 **2NF – Second Normal Form**
- Table has a **single-column primary key**.  
- May still contain **transitive dependencies** — where a non-key attribute depends on another non-key attribute.  
- ✅ **To move to 3NF:** remove transitive dependencies.

---

### 🎯 **3NF – Third Normal Form (Goal)**
- Every non-key attribute depends **only on the key**, and **nothing but the key**.  
- Removes redundancy and ensures all dependencies are logical.

---

## 🧾 Summary Table

| Normal Form | Issue Present | Solution | Result |
|--------------|----------------|-----------|---------|
| UNF | Multi-valued attributes (MVA) | Dissolve MVAs | → 1NF |
| 1NF | Partial dependencies | Remove partial dependencies | → 2NF |
| 2NF | Transitive dependencies | Remove transitive dependencies | → 3NF |
| 3NF | None (fully normalised) | ✅ | Final goal |

---

📘 **Example:** See the attached Excel file [`Normalization_sample.xlsx`](Normalization_sample.xlsx) for a worked example.
```

---

Would you like me to **add a visual Mermaid diagram** showing the transition from UNF → 1NF → 2NF → 3NF?
