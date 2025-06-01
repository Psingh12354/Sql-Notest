# 📘 SQL Basics - Complete Guide

This document provides an overview of essential SQL (Structured Query Language) concepts with explanations and examples. Ideal for beginners and intermediate learners.

---

## 📚 Table of Contents

- [Introduction to SQL](#introduction-to-sql)
- [Types of SQL Commands](#types-of-sql-commands)
- [Database Objects](#database-objects)
- [Data Types](#data-types)
- [SQL Clauses](#sql-clauses)
- [Operators](#operators)
- [Functions](#functions)
- [Joins](#joins)
- [Subqueries](#subqueries)
- [Indexes](#indexes)
- [Views](#views)
- [Stored Procedures](#stored-procedures)
- [Normalization](#normalization)

---

## 🧾 Introduction to SQL

SQL (Structured Query Language) is the standard language for interacting with relational databases. You can use SQL to insert, query, update, and delete data.

```sql
-- Select all data from the 'employees' table
SELECT * FROM employees;
```

---

## 🛠️ Types of SQL Commands

SQL commands are categorized based on their functionality:

| Type  | Description                           | Commands                        |
|-------|---------------------------------------|---------------------------------|
| DDL   | Data Definition Language              | `CREATE`, `ALTER`, `DROP`       |
| DML   | Data Manipulation Language            | `INSERT`, `UPDATE`, `DELETE`    |
| DQL   | Data Query Language                   | `SELECT`                        |
| DCL   | Data Control Language                 | `GRANT`, `REVOKE`               |
| TCL   | Transaction Control Language          | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

---

## 🧱 Database Objects

These are the components of a database.

- **Table**: Holds data in rows and columns.
- **View**: A virtual table created from a SQL query.
- **Index**: Improves data retrieval speed.
- **Stored Procedure**: A saved collection of SQL commands.

---

## 🔢 Data Types (MySQL / SQL Server)

SQL supports various data types for different kinds of data.

| Data Type   | Example Values       |
|-------------|----------------------|
| INT         | 1, 42                |
| VARCHAR(n)  | 'John', 'Hello'      |
| DATE        | '2025-01-01'         |
| FLOAT       | 3.14, 9.81           |
| BOOLEAN     | TRUE, FALSE          |

---

## 📄 SQL Clauses

SQL clauses help refine the data selection.

### `WHERE` - Filters rows based on condition

```sql
SELECT * FROM employees WHERE department = 'Sales';
```

### `ORDER BY` - Sorts the result set

```sql
SELECT * FROM employees ORDER BY salary DESC;
```

### `GROUP BY` & `HAVING` - Group data and filter groups

```sql
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department 
HAVING COUNT(*) > 5;
```

### `LIMIT` / `TOP` - Restrict the number of rows returned

```sql
-- MySQL
SELECT * FROM employees LIMIT 5;

-- SQL Server
SELECT TOP 5 * FROM employees;
```

---

## ⚙️ Operators

Used to form conditions in SQL statements.

- **Comparison**: =, !=, <, >, <=, >=  
- **Logical**: AND, OR, NOT  
- **IN** / **BETWEEN** / **LIKE**: Used for matching patterns and ranges

```sql
SELECT * FROM employees 
WHERE salary BETWEEN 50000 AND 100000;
```

---

## 🧮 Functions

Functions are built-in operations to process data.

### Aggregate Functions

```sql
SELECT COUNT(*), MAX(salary), MIN(salary), AVG(salary) FROM employees;
```

### String Functions

```sql
SELECT UPPER(name), LENGTH(name) FROM employees;
```

### Date Functions

```sql
SELECT CURRENT_DATE, NOW();
```

---

## 🔗 Joins

Used to combine rows from two or more tables.

### `INNER JOIN` - Returns matching rows

```sql
SELECT e.name, d.name 
FROM employees e 
INNER JOIN departments d ON e.dept_id = d.id;
```

### `LEFT JOIN` - All from left table + matched from right

```sql
SELECT e.name, d.name 
FROM employees e 
LEFT JOIN departments d ON e.dept_id = d.id;
```

### `RIGHT JOIN` / `FULL JOIN` - Similar to LEFT JOIN or all matched

```sql
SELECT e.name, d.name 
FROM employees e 
FULL OUTER JOIN departments d ON e.dept_id = d.id;
```

---

## 🔍 Subqueries

A query inside another query.

```sql
SELECT name 
FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);
```

---

## ⚡ Indexes

Used to speed up searches/queries.

```sql
CREATE INDEX idx_name ON employees(name);
```

---

## 👁️ Views

Virtual tables created from SELECT statements.

```sql
CREATE VIEW view_sales AS
SELECT * FROM employees WHERE department = 'Sales';

-- Using the view
SELECT * FROM view_sales;
```

---

## 📦 Stored Procedures

Reusable blocks of SQL code.

```sql
-- SQL Server
CREATE PROCEDURE GetEmployeeByID @ID INT
AS
BEGIN
  SELECT * FROM employees WHERE id = @ID;
END;

-- Execute it
EXEC GetEmployeeByID 1;
```

---

## 🧠 Normalization

Process of organizing data to reduce redundancy and improve integrity.

| Normal Form | Description                                 |
|-------------|---------------------------------------------|
| 1NF         | Eliminate repeating groups                  |
| 2NF         | Remove partial dependencies                 |
| 3NF         | Remove transitive dependencies              |
| BCNF        | Every determinant is a candidate key        |

---

## ✅ Sample CRUD Operations

```sql
-- Create
INSERT INTO employees (name, salary) VALUES ('Alice', 50000);

-- Read
SELECT * FROM employees;

-- Update
UPDATE employees SET salary = 55000 WHERE name = 'Alice';

-- Delete
DELETE FROM employees WHERE name = 'Alice';
```

# 🔗 SQL JOINs - Complete Guide with Comparisons

JOINs are used to combine rows from two or more tables based on related columns. This guide covers all SQL JOIN types with explanations, syntax, examples, diagrams, and comparisons.

---

## 📚 Table of Contents

- [What is a JOIN?](#what-is-a-join)
- [INNER JOIN](#inner-join)
- [LEFT JOIN (LEFT OUTER JOIN)](#left-join-left-outer-join)
- [RIGHT JOIN (RIGHT OUTER JOIN)](#right-join-right-outer-join)
- [FULL JOIN (FULL OUTER JOIN)](#full-join-full-outer-join)
- [CROSS JOIN](#cross-join)
- [SELF JOIN](#self-join)
- [NATURAL JOIN](#natural-join)
- [JOIN Comparison Table](#join-comparison-table)
- [Visual Diagrams](#visual-diagrams)
- [Quick Recap](#quick-recap)

---

## 🔍 What is a JOIN?

A **JOIN** clause is used to retrieve data from multiple tables based on a related column between them (e.g., a foreign key).

---

## 📄 Sample Tables

**employees**

| id | name     | dept_id |
|----|----------|---------|
| 1  | Alice    | 101     |
| 2  | Bob      | 102     |
| 3  | Charlie  | NULL    |

**departments**

| id  | name       |
|-----|------------|
| 101 | Sales      |
| 102 | Marketing  |
| 103 | Finance    |

---

## 🔗 INNER JOIN

**Returns only the rows with matching values in both tables.**

***sql
SELECT e.name AS employee, d.name AS department
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;
***

**Result:**

| employee | department |
|----------|------------|
| Alice    | Sales      |
| Bob      | Marketing  |

---

## ⬅️ LEFT JOIN (LEFT OUTER JOIN)

**Returns all rows from the left table, and matched rows from the right. NULLs if no match.**

***sql
SELECT e.name AS employee, d.name AS department
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
***

**Result:**

| employee | department |
|----------|------------|
| Alice    | Sales      |
| Bob      | Marketing  |
| Charlie  | NULL       |

---

## ➡️ RIGHT JOIN (RIGHT OUTER JOIN)

**Returns all rows from the right table, and matched rows from the left.**

***sql
SELECT e.name AS employee, d.name AS department
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;
***

**Result:**

| employee | department |
|----------|------------|
| Alice    | Sales      |
| Bob      | Marketing  |
| NULL     | Finance    |

---

## 🔄 FULL JOIN (FULL OUTER JOIN)

**Returns all records when there is a match in either table. NULLs for no matches.**

***sql
SELECT e.name AS employee, d.name AS department
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
***

**Result:**

| employee | department |
|----------|------------|
| Alice    | Sales      |
| Bob      | Marketing  |
| Charlie  | NULL       |
| NULL     | Finance    |

> ⚠️ Note: MySQL does not support FULL JOIN natively. Use `UNION` of LEFT and RIGHT JOINs.

---

## ❌ CROSS JOIN

**Returns the Cartesian product (all possible combinations) of both tables.**

***sql
SELECT e.name AS employee, d.name AS department
FROM employees e
CROSS JOIN departments d;
***

**Result:**  
If 3 employees × 3 departments → 9 rows total.

---

## 🔁 SELF JOIN

**A table joined to itself. Often used to find hierarchical relationships (e.g., employee-manager).**

***sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
JOIN employees m ON e.manager_id = m.id;
***

---

## 🌿 NATURAL JOIN

**Automatically joins tables on all columns with the same name and compatible data type.**

> ⚠️ Implicit join logic. Can cause unintended results if tables share more than one common column name.

Assuming these structures:

**employees**

| id | name     | dept_id |
|----|----------|---------|

**departments**

| dept_id | name |
|---------|------|

Then:

***sql
SELECT *
FROM employees
NATURAL JOIN departments;
***

**Result:**

| id | name     | dept_id | name       |
|----|----------|---------|------------|
| 1  | Alice    | 101     | Sales      |
| 2  | Bob      | 102     | Marketing  |

> 🚫 Not supported in SQL Server  
> ✅ Supported in MySQL, PostgreSQL, Oracle

### ✅ Pros

- Short, clean syntax when you have standardized schemas.

### ⚠️ Cons

- Hidden logic (harder to debug).
- Risky if unintended columns share the same name.

### 🔁 Equivalent INNER JOIN:

***sql
SELECT *
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
***

---

## 📊 JOIN Comparison Table

| Join Type     | Description                              | Matching Rows | Unmatched Left | Unmatched Right |
|---------------|------------------------------------------|----------------|----------------|-----------------|
| INNER JOIN    | Only matching rows                       | ✅             | ❌             | ❌              |
| LEFT JOIN     | All left + matching right                | ✅             | ✅             | ❌              |
| RIGHT JOIN    | All right + matching left                | ✅             | ❌             | ✅              |
| FULL JOIN     | All matching + unmatched both sides      | ✅             | ✅             | ✅              |
| CROSS JOIN    | All combinations (Cartesian product)     | ✅ (all pairs) | ✅             | ✅              |
| NATURAL JOIN  | Matches all same-named columns           | ✅             | ❌ (like INNER) | ❌ (like INNER) |

---

## 🎨 Visual Diagrams (Venn-style)

**INNER JOIN**
```
   A ∩ B
```

**LEFT JOIN**
```
   A ⟵ A ∩ B
```

**RIGHT JOIN**
```
   B ⟶ A ∩ B
```

**FULL JOIN**
```
   A ∪ B
```

**CROSS JOIN**
```
   A × B (all pairs)
```

**NATURAL JOIN**
```
   Like INNER JOIN based on matching column names automatically
```

---

## ✅ Quick Recap

| Type           | Use When You Want...                                         |
|----------------|---------------------------------------------------------------|
| INNER JOIN     | Only records with matches in both tables                      |
| LEFT JOIN      | All from the left, matched from the right                     |
| RIGHT JOIN     | All from the right, matched from the left                     |
| FULL JOIN      | All from both tables, matched or not                          |
| CROSS JOIN     | Every possible combination of rows                            |
| NATURAL JOIN   | Shortcut join based on column names (be careful with overlap) |
| SELF JOIN      | Compare rows within the same table (e.g., employee-manager)   |

---

## 🧪 Practice

- [SQL Fiddle](https://sqlfiddle.com/)
- [DB Fiddle](https://www.db-fiddle.com/)
- [LeetCode SQL Problems](https://leetcode.com/problemset/database/)

---

Let me know if you'd like:
- This exported as a `.md` file  
- JOINs with ER diagrams or JOIN animations  
- Practice problems for JOINs


