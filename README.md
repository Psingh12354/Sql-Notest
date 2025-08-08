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
## 🔘 IN Operator

The `IN` operator is used to match **a value against a list of possible values**.

### ✅ Syntax

```sql
SELECT column_name
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

### 🧪 Example

```sql
SELECT name
FROM employees
WHERE dept_id IN (101, 102);
```

Returns employees in department 101 or 102.

> ✅ Shortcut for multiple OR conditions:
>
> `WHERE dept_id = 101 OR dept_id = 102`

---

### 🔄 NOT IN

Negates the check — matches values **not in the list**.

```sql
SELECT name
FROM employees
WHERE dept_id NOT IN (101, 102);
```

---

## 📏 BETWEEN Operator

The `BETWEEN` operator is used to check if a value lies within a **range (inclusive)**.

### ✅ Syntax

```sql
SELECT column_name
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

> `value1` should be the **lower bound** and `value2` the **upper bound**.

---

### 🧪 Example (Numbers)

```sql
SELECT name, salary
FROM employees
WHERE salary BETWEEN 30000 AND 50000;
```

Returns employees earning between 30k and 50k (inclusive).

---

### 🧪 Example (Dates)

```sql
SELECT *
FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
```

> ✅ Useful for filtering date ranges

---

### 🔄 NOT BETWEEN

```sql
SELECT name
FROM employees
WHERE salary NOT BETWEEN 30000 AND 50000;
```

---

## 🔤 LIKE Operator

Used for **pattern matching** with **wildcards** in string values.

### ✅ Syntax

```sql
SELECT column_name
FROM table_name
WHERE column_name LIKE 'pattern';
```

---

### 🧪 Wildcards

| Wildcard | Meaning                       |
|----------|-------------------------------|
| `%`      | Zero or more characters       |
| `_`      | Exactly one character         |

---

### 🔍 Examples

```sql
-- Names starting with 'A'
SELECT name FROM employees
WHERE name LIKE 'A%';

-- Names ending with 'n'
SELECT name FROM employees
WHERE name LIKE '%n';

-- Names containing 'ar'
SELECT name FROM employees
WHERE name LIKE '%ar%';

-- Names with 5 characters
SELECT name FROM employees
WHERE name LIKE '_____';

-- Names starting with 'A' and 2nd letter is 'l'
SELECT name FROM employees
WHERE name LIKE 'Al%';
```

---

### 🔄 NOT LIKE

Negates the pattern:

```sql
SELECT name
FROM employees
WHERE name NOT LIKE 'A%';
```

---

## 🆚 Comparison Summary

| Operator  | Use Case                         | Example Syntax                          | Notes                                 |
|-----------|----------------------------------|------------------------------------------|----------------------------------------|
| `IN`      | Match one of multiple values     | `WHERE id IN (1, 2, 3)`                 | Clean replacement for multiple `OR`s  |
| `BETWEEN` | Match a value in a range         | `WHERE age BETWEEN 20 AND 30`           | Inclusive of both endpoints            |
| `LIKE`    | Match string patterns            | `WHERE name LIKE 'A%'`                  | Use `%` for wildcard matching          |

---

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

```sql
SELECT e.name AS employee, d.name AS department
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;
```

**Result:**

| employee | department |
|----------|------------|
| Alice    | Sales      |
| Bob      | Marketing  |

---

## ⬅️ LEFT JOIN (LEFT OUTER JOIN)

**Returns all rows from the left table, and matched rows from the right. NULLs if no match.**

```sql
SELECT e.name AS employee, d.name AS department
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
```

**Result:**

| employee | department |
|----------|------------|
| Alice    | Sales      |
| Bob      | Marketing  |
| Charlie  | NULL       |

---

## ➡️ RIGHT JOIN (RIGHT OUTER JOIN)

**Returns all rows from the right table, and matched rows from the left.**

```sql
SELECT e.name AS employee, d.name AS department
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;
```

**Result:**

| employee | department |
|----------|------------|
| Alice    | Sales      |
| Bob      | Marketing  |
| NULL     | Finance    |

---

## 🔄 FULL JOIN (FULL OUTER JOIN)

**Returns all records when there is a match in either table. NULLs for no matches.**

```sql
SELECT e.name AS employee, d.name AS department
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
```

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

```sql
SELECT e.name AS employee, d.name AS department
FROM employees e
CROSS JOIN departments d;
```

**Result:**  
If 3 employees × 3 departments → 9 rows total.

---

## 🔁 SELF JOIN

**A table joined to itself. Often used to find hierarchical relationships (e.g., employee-manager).**

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
JOIN employees m ON e.manager_id = m.id;
```

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

```sql
SELECT *
FROM employees
NATURAL JOIN departments;
```

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

```sql
SELECT *
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

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


---
### Common Table Expression (CTE) — Notes

**Definition:**
A Common Table Expression (CTE) is a temporary named result set defined within a single SQL statement. It acts like a temporary table that exists only during the execution of that query.

**Syntax:**

```sql
WITH cte_name AS (
  SELECT ...
)
SELECT * FROM cte_name;
```

**Purpose:**

- Break complex queries into simpler parts
- Reuse intermediate result sets inside a query
- Make queries easier to read and maintain
- Support recursive queries (hierarchical data)

**How it works:**

- Define a named query with `WITH` and a subquery (`SELECT`) inside parentheses
- Use the CTE name in the outer query as if it were a real table
- The CTE is temporary and limited to the single query execution


### Examples

1. **Basic CTE to calculate average salary by department:**
```sql
WITH DeptAvg AS (
  SELECT department_id, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department_id
)
SELECT *
FROM DeptAvg;
```

2. **Use CTE in main query to find employees earning above their department average:**
```sql
WITH DeptAvg AS (
  SELECT department_id, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department_id
)
SELECT e.*
FROM employees e
JOIN DeptAvg d ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

3. **Multiple CTEs in one query with a join:**
```sql
WITH Sales_CTE AS (
  SELECT SalesPersonID, SUM(TotalDue) AS TotalSales
  FROM SalesOrderHeader
  GROUP BY SalesPersonID
),
Quota_CTE AS (
  SELECT SalesPersonID, SUM(SalesQuota) AS TotalQuota
  FROM SalesPersonQuota
  GROUP BY SalesPersonID
)
SELECT s.SalesPersonID, s.TotalSales, q.TotalQuota
FROM Sales_CTE s
JOIN Quota_CTE q ON s.SalesPersonID = q.SalesPersonID;
```


### Summary

- CTE = named temporary result set (like a temporary table) inside query
- Write `WITH cte_name AS (SELECT ...)` before main query
- Can simplify, modularize, and improve readability of SQL
- Useful for recursive and hierarchical queries


---

### LEFT

- **Purpose:** Extracts a specified number of characters from the beginning (left side) of a string.
- **Syntax:**

```sql
LEFT(column_name, number_of_characters)
```

- **Example:**
`LEFT('Delhi', 1)` returns `'D'`
- **Use case:** To get the first character(s) of a string (e.g., to check the first letter in city names).

***

### RIGHT

- **Purpose:** Extracts a specified number of characters from the end (right side) of a string.
- **Syntax:**

```sql
RIGHT(column_name, number_of_characters)
```

- **Example:**
`RIGHT('Delhi', 1)` returns `'i'`
- **Use case:** To get the last character(s) of a string (e.g., to check the last letter in city names).

***

### SUBSTR (or SUBSTRING)

- **Purpose:** Extracts a substring starting from a specific position for a given length.
- **Syntax:**

```sql
SUBSTR(string, start_position, length)
```

or

```sql
SUBSTRING(string, start_position, length)
```

- **Details:**
    - `start_position` is 1-based (starts counting at 1).
    - `length` is how many characters to extract from `start_position`.
- **Examples:**
    - `SUBSTR('Delhi', 1, 1)` returns `'D'` (first character)
    - `SUBSTR('Delhi', 5, 1)` returns `'i'` (last character)
    - `SUBSTR('Delhi', 2, 3)` returns `'elh'` (characters starting from position 2, length 3)
- **Use case:** More flexible than LEFT/RIGHT, allowing extraction from any position within the string.

***

### Summary

- Use **LEFT** to get characters from the start.
- Use **RIGHT** to get characters from the end.
- Use **SUBSTR**/ **SUBSTRING** for extracting substrings from any position with flexible length.
### FLOOR

- **Purpose:**
Returns the largest integer **less than or equal to** a given number. It always rounds **down**, truncating the decimal part.
- **Syntax:**

```sql
FLOOR(number)
```

- **Example:**

```sql
FLOOR(4.9)  -- returns 4
FLOOR(4.1)  -- returns 4
FLOOR(-4.1) -- returns -5 (because -5 < -4.1)
```

- **Use case:**
When you want to round down to the nearest integer, ensuring the result is never greater than the original number. Good for conservative calculations, like rounding down average populations so you don’t overstate counts.

***

### CEIL (or CEILING)

- **Purpose:**
Returns the smallest integer **greater than or equal to** the given number. It always rounds **up**, moving to the next integer if there is any decimal part.
- **Syntax:**

```sql
CEIL(number)
-- or sometimes CEILING(number)
```

- **Example:**

```sql
CEIL(4.1)   -- returns 5
CEIL(4.9)   -- returns 5
CEIL(-4.1)  -- returns -4 (because -4 is the smallest integer >= -4.1)
```

- **Use case:**
When you want to round up to ensure the result is never less than the original number. Useful in scenarios where you must cover the full amount, like allocating resources.

***

### ROUND

- **Purpose:**
Rounds the number to the nearest integer or to a specified number of decimal places.
- **Syntax:**

```sql
ROUND(number [, decimal_places])
```

- **Behavior:**
    - If the decimal part is 0.5 or greater, rounds **up**.
    - Otherwise, rounds **down**.
- **Example:**

```sql
ROUND(4.5)   -- returns 5
ROUND(4.4)   -- returns 4
ROUND(-4.5)  -- returns -4 (depending on SQL dialect rounding rules)
ROUND(4.567, 2) -- returns 4.57 (round to 2 decimal places)
```

- **Use case:**
When you want the closest approximation rather than always rounding up or down. Common for general statistics or financial calculations.

***

### Summary Comparison

| Function | Rounding Behavior | Example Input | Example Output |
| :-- | :-- | :-- | :-- |
| FLOOR | Always rounds **down** | 4.9 | 4 |
| CEIL | Always rounds **up** | 4.1 | 5 |
| ROUND | Rounds to **nearest integer/decimal** | 4.5 | 5 |
--- 
