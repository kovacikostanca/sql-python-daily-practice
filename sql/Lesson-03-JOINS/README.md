# SQL Masterclass – Lesson 3: JOINs (INNER, LEFT, SELF & ANTI JOIN)

## Table of Contents

1. [Introduction](#1-introduction)
2. [Why Do We Need JOINs?](#2-why-do-we-need-joins)
3. [Understanding Table Relationships](#3-understanding-table-relationships)
4. [INNER JOIN](#4-inner-join)
   - [Basic INNER JOIN](#41-basic-inner-join)
   - [Understanding the ON Condition](#42-understanding-the-on-condition)
   - [Using Table Aliases](#43-using-table-aliases)
5. [LEFT JOIN](#5-left-join)
   - [Basic LEFT JOIN](#51-basic-left-join)
   - [INNER JOIN vs LEFT JOIN](#52-inner-join-vs-left-join)
6. [ANTI JOIN](#6-anti-join)
   - [LEFT JOIN with IS NULL](#61-left-join-with-is-null)
   - [NOT EXISTS](#62-not-exists)
   - [Finding Departments Without Employees](#63-finding-departments-without-employees)
7. [SELF JOIN](#7-self-join)
   - [Basic SELF JOIN](#71-basic-self-join)
   - [Understanding Aliases in SELF JOIN](#72-understanding-aliases-in-self-join)
   - [Practical Applications](#73-practical-applications)
8. [Comparing JOIN Types](#8-comparing-join-types)
9. [Common JOIN Mistakes](#9-common-join-mistakes)
10. [Practical Data Analyst Examples](#10-practical-data-analyst-examples)
11. [Practice Assignment](#11-practice-assignment)
12. [SQL JOIN Cheat Sheet](#12-sql-join-cheat-sheet)
13. [Lesson Summary](#13-lesson-summary)
14. [Next Lesson](#14-next-lesson)

---

## 1. Introduction

Welcome to **SQL Masterclass – Lesson 3**!

In Lesson 1, we learned how to retrieve, filter, and sort data using:

- `SELECT`
- `WHERE`
- `DISTINCT`
- `ORDER BY`
- Comparison and logical operators
- `LIKE`
- `IN`
- `BETWEEN`
- NULL handling

In Lesson 2, we learned how to summarize and analyze data using:

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`
- `GROUP BY`
- `HAVING`

In this lesson, we move to one of the most important SQL skills for a Data Analyst: **combining data from multiple tables**.

Real-world databases rarely store all information in one table.

For example, a company might have separate tables for:

- Employees
- Departments
- Customers
- Orders
- Products
- Payments
- Managers

To retrieve meaningful information from these tables, we need SQL JOINs.

### Learning Objectives

By the end of this lesson, you should be able to:

- Understand why JOINs are necessary.
- Understand Primary Keys and Foreign Keys.
- Combine tables using `INNER JOIN`.
- Combine tables using `LEFT JOIN`.
- Identify unmatched records using ANTI JOIN logic.
- Use `NOT EXISTS` to find records without matches.
- Join a table with itself using `SELF JOIN`.
- Use table aliases to write cleaner queries.
- Understand how NULL values affect JOIN results.
- Recognize one-to-many relationships.
- Translate real business questions into JOIN queries.

---

## 2. Why Do We Need JOINs?

Imagine that a company stores employee information in one table and department information in another.

### Table 1: employees

| employee_id | first_name | department_id | salary |
|---:|---|---:|---:|
| 1 | John | 10 | 55000 |
| 2 | Maria | 20 | 42000 |
| 3 | George | 10 | 65000 |
| 4 | Anna | 30 | 60000 |
| 5 | Peter | NULL | 38000 |
| 6 | Elena | 20 | 45000 |
| 7 | David | 40 | 72000 |
| 8 | Sofia | 10 | 50000 |

### Table 2: departments

| department_id | department_name |
|---:|---|
| 10 | IT |
| 20 | HR |
| 30 | Finance |
| 40 | Marketing |
| 50 | Sales |

Notice that both tables have a common column: `department_id`.

This allows us to connect the two tables and retrieve information from both.

For example, we can combine an employee's name and salary with their department name.

Without JOINs, we would have to retrieve the information separately and combine it manually.

JOINs allow SQL to perform this operation directly.

---

## 3. Understanding Table Relationships

Before learning JOIN syntax, we need to understand two important database concepts.

### Primary Key (PK)

A Primary Key uniquely identifies each record in a table.

Example:

In the `departments` table, `department_id` is the Primary Key.

| department_id (PK) | department_name |
|---:|---|
| 10 | IT |
| 20 | HR |
| 30 | Finance |
| 40 | Marketing |
| 50 | Sales |

Every department has a unique ID.

### Foreign Key (FK)

A Foreign Key is a column that references a key in another table.

In our example, `employees.department_id` references `departments.department_id`.

| employees | departments |
|---|---|
| department_id (FK) | department_id (PK) |

This creates a relationship between the two tables.

### Visual Representation

```text
employees                         departments
-------------------               -------------------
employee_id (PK)                  department_id (PK)
first_name                        department_name
department_id (FK)  ----------->  department_id
salary
```

### One-to-Many Relationship

In our example:

- One department can have multiple employees.
- Each employee can belong to one department.

This is called a **one-to-many relationship**.

Understanding relationships is essential because they determine how tables should be joined.

---

## 4. INNER JOIN

`INNER JOIN` returns only records that have matching values in both tables.

It is one of the most commonly used JOIN types in SQL.

### 4.1 Basic INNER JOIN

**Business question:**

Show each employee's name and department name.

```sql
SELECT
    employees.employee_id,
    employees.first_name,
    departments.department_name
FROM employees
INNER JOIN departments
    ON employees.department_id = departments.department_id;
```

Expected result:

| employee_id | first_name | department_name |
|---:|---|---|
| 1 | John | IT |
| 2 | Maria | HR |
| 3 | George | IT |
| 4 | Anna | Finance |
| 6 | Elena | HR |
| 7 | David | Marketing |
| 8 | Sofia | IT |

What happened?

- John matched department 10 → IT.
- Maria matched department 20 → HR.
- Anna matched department 30 → Finance.
- Peter has a NULL department ID, so he has no match.
- Sales has no employee assigned to department 50, so it does not appear.

**Important:** INNER JOIN excludes unmatched records from both tables.

### 4.2 Understanding the ON Condition

The most important part of a JOIN is the `ON` condition.

```sql
ON employees.department_id = departments.department_id
```

This tells SQL how the two tables should be connected.

The condition compares the department IDs in both tables.

When the values match, SQL combines the corresponding records.

Without the correct join condition, you may get incorrect results or a Cartesian product.

### 4.3 Using Table Aliases

In professional SQL, you will frequently see table aliases.

Instead of writing:

```sql
employees.employee_id
departments.department_name
```

We can use shorter names:

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM employees AS e
INNER JOIN departments AS d
    ON e.department_id = d.department_id;
```

Here:

- `e` represents the `employees` table.
- `d` represents the `departments` table.

Aliases make queries shorter, cleaner, and easier to understand.

**Professional Tip:** Get comfortable using aliases. They become especially important when joining three or more tables.

---

## 5. LEFT JOIN

`LEFT JOIN` returns:

1. Every record from the left table.
2. Matching records from the right table.
3. NULL values for right-table columns when no match exists.

### 5.1 Basic LEFT JOIN

**Business question:**

Show all employees, including employees who are not assigned to a department.

```sql
SELECT
    e.employee_id,
    e.first_name,
    d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id;
```

Expected result:

| employee_id | first_name | department_name |
|---:|---|---|
| 1 | John | IT |
| 2 | Maria | HR |
| 3 | George | IT |
| 4 | Anna | Finance |
| 5 | Peter | NULL |
| 6 | Elena | HR |
| 7 | David | Marketing |
| 8 | Sofia | IT |

Notice Peter.

He has no matching department, but he still appears because `employees` is the left table.

The unmatched department information is represented by NULL.

### 5.2 INNER JOIN vs LEFT JOIN

| INNER JOIN | LEFT JOIN |
|---|---|
| Returns matching records only | Returns all records from the left table |
| Excludes unmatched left records | Keeps unmatched left records |
| Excludes unmatched right records | Excludes unmatched right records |
| Useful when only matches are needed | Useful when all left-side records must be preserved |

### Key Difference

Think about the following questions:

- "Show employees who belong to a department." → `INNER JOIN`
- "Show all employees, even those without a department." → `LEFT JOIN`

The wording of the business question helps you choose the correct JOIN.

---

## 6. ANTI JOIN

An ANTI JOIN returns records from one table that **do not have a matching record** in another table.

There is no universal `ANTI JOIN` keyword in standard SQL.

A common way to perform an anti-join is with:

- `LEFT JOIN` + `IS NULL`
- `NOT EXISTS`

ANTI JOIN logic is extremely useful in Data Analytics.

Typical business questions include:

- Which employees do not have a valid department?
- Which customers have never placed an order?
- Which products have never been sold?
- Which departments have no employees?
- Which orders do not have a matching payment?

### 6.1 LEFT JOIN with IS NULL

**Business question:**

Find employees who do not have a matching department.

```sql
SELECT
    e.employee_id,
    e.first_name,
    e.department_id
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

Expected result:

| employee_id | first_name | department_id |
|---:|---|---:|
| 5 | Peter | NULL |

How does this work?

1. `LEFT JOIN` keeps all employees.
2. Matching department records are attached.
3. Unmatched department records contain NULL values.
4. `WHERE d.department_id IS NULL` keeps only unmatched employees.

This is a standard anti-join pattern.

### 6.2 NOT EXISTS

Another way to perform an anti-join is with `NOT EXISTS`.

```sql
SELECT
    e.employee_id,
    e.first_name,
    e.department_id
FROM employees AS e
WHERE NOT EXISTS (
    SELECT 1
    FROM departments AS d
    WHERE d.department_id = e.department_id
);
```

This query checks whether a matching department exists for each employee.

- If a match exists, the employee is excluded.
- If no match exists, the employee is returned.

`NOT EXISTS` is often a clean and reliable way to express anti-join logic.

### 6.3 Finding Departments Without Employees

ANTI JOIN can also work in the opposite direction.

**Business question:**

Which departments have no employees?

```sql
SELECT
    d.department_id,
    d.department_name
FROM departments AS d
LEFT JOIN employees AS e
    ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

Expected result:

| department_id | department_name |
|---:|---|
| 50 | Sales |

Why?

Sales exists in the `departments` table, but no employee references department 50.

**Important:** The left table determines which unmatched records you are looking for.

- Employees LEFT JOIN Departments → Find employees without departments.
- Departments LEFT JOIN Employees → Find departments without employees.

---

## 7. SELF JOIN

A SELF JOIN occurs when a table is joined with itself.

Why would we do this?

Imagine that every employee has a `manager_id`, which references another employee's `employee_id`.

This means the same table stores both employees and their managers.

### Example Dataset

| employee_id | first_name | manager_id |
|---:|---|---:|
| 1 | John | 3 |
| 2 | Maria | 3 |
| 3 | George | NULL |
| 4 | Anna | 1 |
| 5 | Peter | 1 |
| 6 | Elena | 2 |

We want to answer:

"Who is the manager of each employee?"

Both employees and managers are stored in the same table.

We need to join `employees` with itself.

### 7.1 Basic SELF JOIN

```sql
SELECT
    e.employee_id,
    e.first_name AS employee_name,
    m.first_name AS manager_name
FROM employees AS e
LEFT JOIN employees AS m
    ON e.manager_id = m.employee_id;
```

Expected result:

| employee_id | employee_name | manager_name |
|---:|---|---|
| 1 | John | George |
| 2 | Maria | George |
| 3 | George | NULL |
| 4 | Anna | John |
| 5 | Peter | John |
| 6 | Elena | Maria |

Let's understand the join condition:

```sql
ON e.manager_id = m.employee_id
```

This means:

Find the employee whose manager ID matches another employee's ID.

George has no manager, so his manager name is NULL.

We use `LEFT JOIN` to keep employees without managers.

### 7.2 Understanding Aliases in SELF JOIN

Why do we need aliases?

Because SQL needs to distinguish between the two references to the same table.

We use:

| Alias | Role |
|---|---|
| e | Employee |
| m | Manager |

Although both aliases reference `employees`, they represent different roles.

Without aliases, it would be unclear which `employee_id` or `first_name` we mean.

### 7.3 Practical Applications

SELF JOINs are useful for:

- Employee-manager relationships.
- Organizational hierarchies.
- Finding colleagues who share the same manager.
- Comparing records within the same table.
- Finding relationships between parent and child records.

---

## 8. Comparing JOIN Types

Let's summarize the four JOIN patterns covered in this lesson.

| JOIN Type | What It Returns | Typical Use |
|---|---|---|
| INNER JOIN | Only matching records | Employees with valid departments |
| LEFT JOIN | All left records and matching right records | All employees, including those without departments |
| ANTI JOIN | Left records without a match | Employees without a valid department |
| SELF JOIN | A table joined to itself | Employees and their managers |

### Quick Reference

**INNER JOIN**

```text
Only matching records from both tables.
```

**LEFT JOIN**

```text
All records from the left table,
plus matching records from the right table.
```

**ANTI JOIN**

```text
Records from the left table
that have no matching record on the right.
```

**SELF JOIN**

```text
One table connected to itself
using different aliases.
```

---

## 9. Common JOIN Mistakes

Understanding JOINs is not just about writing the syntax. You must also understand the data relationships.

### Mistake 1: Forgetting the ON Condition

Incorrect:

```sql
SELECT *
FROM employees
INNER JOIN departments;
```

Without a proper join condition, some SQL systems will reject the query. If you explicitly use a cross join or otherwise omit the intended relationship, you may produce a Cartesian product.

Correct:

```sql
SELECT *
FROM employees AS e
INNER JOIN departments AS d
    ON e.department_id = d.department_id;
```

### Mistake 2: Joining the Wrong Columns

Incorrect:

```sql
ON e.employee_id = d.department_id
```

Correct:

```sql
ON e.department_id = d.department_id
```

Always understand which columns represent the relationship.

### Mistake 3: Using WHERE Incorrectly After LEFT JOIN

Consider this query:

```sql
SELECT
    e.first_name,
    d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```

This returns only employees belonging to IT.

Although we used LEFT JOIN, the `WHERE` condition removes unmatched rows because their department name is NULL.

For this condition, the result behaves like an INNER JOIN.

If your goal is to preserve every employee but attach department information only when it is IT, put the condition in `ON`:

```sql
SELECT
    e.first_name,
    d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id
    AND d.department_name = 'IT';
```

Now every employee remains in the result. Employees outside IT have NULL for `department_name`.

### Mistake 4: Using the Wrong NULL Column for an Anti-Join

Correct anti-join pattern:

```sql
SELECT e.first_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

Check a column from the right table that should be non-NULL for every real matching record, ideally its Primary Key.

### Mistake 5: Unexpected Duplicate Rows

Suppose one customer has five orders.

If you join customers to orders, that customer will appear five times: once for each matching order.

This is not necessarily an error.

It is a consequence of a one-to-many relationship.

Always understand the relationship between your tables before interpreting the number of rows returned.

---

## 10. Practical Data Analyst Examples

Here are examples of real business questions that can be solved using JOINs.

| Business Question | SQL Concept |
|---|---|
| Which employees work in each department? | INNER JOIN |
| Show all employees, including those without departments | LEFT JOIN |
| Which customers have never placed an order? | ANTI JOIN |
| Which products have never been sold? | ANTI JOIN |
| Who manages each employee? | SELF JOIN |
| Which departments have no employees? | ANTI JOIN |
| Show all customers and their orders, if available | LEFT JOIN |
| Find employees who share the same manager | SELF JOIN |

### Example: Customers Without Orders

Imagine we have two tables.

**customers**

| customer_id | customer_name |
|---:|---|
| 1 | Alice |
| 2 | Bob |
| 3 | Charlie |
| 4 | Diana |

**orders**

| order_id | customer_id | order_amount |
|---:|---:|---:|
| 101 | 1 | 250 |
| 102 | 2 | 400 |
| 103 | 1 | 150 |

**Business question:**

Which customers have never placed an order?

```sql
SELECT
    c.customer_id,
    c.customer_name
FROM customers AS c
LEFT JOIN orders AS o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

Expected result:

| customer_id | customer_name |
|---:|---|
| 3 | Charlie |
| 4 | Diana |

This is a typical Data Analyst query for identifying inactive customers.

---

## 11. Practice Assignment

Use the `employees` and `departments` tables from this lesson.


- For the SELF JOIN exercises, use the employee-manager dataset from Section 7.

- For Exercise 6, use the `customers` and `orders` example tables from Section 10.


Try to solve each exercise independently before checking your notes.

### Level 1 – INNER JOIN and LEFT JOIN

**Exercise 1:** Show each employee's first name and department name. Only include employees who have a matching department.

**Exercise 2:** Show all employees and their department names, including employees without an assigned department.

**Exercise 3:** Show the employee ID, first name, department name, and salary for every employee who belongs to the IT department.

### Level 2 – ANTI JOIN

**Exercise 4:** Find employees who do not have a matching department.

**Exercise 5:** Find departments that have no employees.

**Exercise 6:** Find customers who have never placed an order using the `customers` and `orders` example tables.

### Level 3 – SELF JOIN

Use this employee-manager dataset:

| employee_id | first_name | manager_id |
|---:|---|---:|
| 1 | John | 3 |
| 2 | Maria | 3 |
| 3 | George | NULL |
| 4 | Anna | 1 |
| 5 | Peter | 1 |
| 6 | Elena | 2 |

**Exercise 7:** Display every employee and their manager's name. Include employees without managers.

**Exercise 8:** Find employees who report directly to John.

**Exercise 9:** Display employees who have the same manager as Maria, excluding Maria herself.

### Level 4 – Data Analyst Challenge

**Exercise 10:** Show every department and the number of employees assigned to it. Include departments with zero employees.

**Exercise 11:** Find departments with no employees using `LEFT JOIN` and `IS NULL`.

**Exercise 12:** Find employees who have a valid department assignment but earn more than 50,000.

**Exercise 13:** Show all employees and their manager names. Only return employees whose manager exists.

**Exercise 14:** Find employees who do not have a valid department assignment using `NOT EXISTS`.

### Practice Guidelines

For every exercise:

1. Understand the business question.
2. Identify the tables you need.
3. Identify the relationship between the tables.
4. Decide which JOIN type is appropriate.
5. Write the `ON` condition carefully.
6. Add `WHERE` conditions if necessary.
7. Check whether the number of returned rows makes sense.
8. Verify whether unmatched records should be included or excluded.

---

## 12. SQL JOIN Cheat Sheet

### INNER JOIN

```sql
SELECT
    e.first_name,
    d.department_name
FROM employees AS e
INNER JOIN departments AS d
    ON e.department_id = d.department_id;
```

### LEFT JOIN

```sql
SELECT
    e.first_name,
    d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id;
```

### ANTI JOIN Using LEFT JOIN

```sql
SELECT
    e.first_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

### ANTI JOIN Using NOT EXISTS

```sql
SELECT
    e.first_name
FROM employees AS e
WHERE NOT EXISTS (
    SELECT 1
    FROM departments AS d
    WHERE d.department_id = e.department_id
);
```

### SELF JOIN

```sql
SELECT
    e.first_name AS employee_name,
    m.first_name AS manager_name
FROM employees AS e
LEFT JOIN employees AS m
    ON e.manager_id = m.employee_id;
```

---

## 13. Lesson Summary

In Lesson 3, we learned how to combine information stored in multiple database tables.

### Concepts Covered

- Primary Keys and Foreign Keys.
- One-to-many relationships.
- `INNER JOIN`.
- `LEFT JOIN`.
- ANTI JOIN using `LEFT JOIN` and `IS NULL`.
- ANTI JOIN using `NOT EXISTS`.
- `SELF JOIN`.
- Table aliases.
- JOIN conditions using `ON`.
- NULL values in JOIN results.
- Common JOIN mistakes.

### Key Takeaways

1. JOINs allow us to combine information stored in different tables.
2. `INNER JOIN` returns only matching records.
3. `LEFT JOIN` preserves every record from the left table.
4. An ANTI JOIN finds records without a match.
5. `NOT EXISTS` is another way to express anti-join logic.
6. `SELF JOIN` connects a table to itself.
7. Aliases make queries easier to read.
8. A correct `ON` condition is essential.
9. One-to-many relationships can produce multiple rows for the same entity.
10. The placement of a condition in `ON` versus `WHERE` can change the results of a LEFT JOIN.

### Most Important Concept

Before writing a JOIN query, ask yourself:

- Do I need only matching records?
- Do I need to preserve every record from the left table?
- Do I need to find records without a match?
- Am I joining two different tables or the same table?

The answers will help you choose the correct JOIN type.

---

## 14. Next Lesson

### Lesson 4 – Subqueries and CTEs

In the next lesson, we will learn how to use queries inside other queries.

Topics will include:

- What subqueries are.
- Single-row subqueries.
- Multiple-row subqueries.
- Scalar subqueries.
- `IN` and `NOT IN` with subqueries.
- `EXISTS` and `NOT EXISTS`.
- Correlated subqueries.
- Practical Data Analyst exercises.

**Goal:** Learn how to solve more complex business questions by using one query as an input to another.

---

## Progress Tracker

| Lesson | Topic | Status |
|---|---|---|
| Lesson 01 | SELECT, WHERE, ORDER BY & NULL Values | Completed |
| Lesson 02 | Aggregations, GROUP BY & HAVING | Completed |
| Lesson 03 | JOINs | In Progress |
| Lesson 04 | Subqueries and CTEs | Upcoming |
| Lesson 05 | CASE, String and Date Functions | Upcoming |


---

**End of Lesson 3 – Keep Practicing!**

The objective is not just to memorize JOIN syntax. It is to understand how tables relate to each other and how to retrieve accurate information from a relational database.

Practice every JOIN type until you can recognize which one a business question requires.
