# SQL Masterclass – Lesson 2: Aggregations, GROUP BY & HAVING

## Table of Contents

1. [Introduction](#1-introduction)
2. [Practice Dataset](#2-practice-dataset)
3. [Aggregate Functions](#3-aggregate-functions)
   - [COUNT()](#31-count)
   - [SUM()](#32-sum)
   - [AVG()](#33-avg)
   - [MIN() and MAX()](#34-min-and-max)
   - [Combining Aggregate Functions](#35-combining-aggregate-functions)
4. [GROUP BY](#4-group-by)
   - [Basic GROUP BY](#41-basic-group-by)
   - [GROUP BY with COUNT()](#42-group-by-with-count)
   - [GROUP BY with SUM()](#43-group-by-with-sum)
   - [Multiple Aggregate Functions](#44-multiple-aggregate-functions)
5. [GROUP BY with Multiple Columns](#5-group-by-with-multiple-columns)
6. [WHERE vs HAVING](#6-where-vs-having)
   - [WHERE: Filtering Rows](#61-where-filtering-rows)
   - [HAVING: Filtering Groups](#62-having-filtering-groups)
   - [Using WHERE and HAVING Together](#63-using-where-and-having-together)
7. [ORDER BY with GROUP BY](#7-order-by-with-group-by)
8. [SQL Logical Processing Order](#8-sql-logical-processing-order)
9. [Common Mistakes](#9-common-mistakes)
10. [NULL Handling in Aggregate Functions](#10-null-handling-in-aggregate-functions)
11. [Practical Data Analyst Examples](#11-practical-data-analyst-examples)
12. [Practice Assignment](#12-practice-assignment)
13. [SQL Cheat Sheet](#13-sql-cheat-sheet)
14. [Lesson Summary](#14-lesson-summary)
15. [Next Lesson](#15-next-lesson)

---

## 1. Introduction

Welcome to **SQL Masterclass – Lesson 2**!

In Lesson 1, we learned how to retrieve, filter, and sort individual records using:

- `SELECT`
- `WHERE`
- `DISTINCT`
- `ORDER BY`
- Comparison operators
- Logical operators
- `LIKE`
- `IN`
- `BETWEEN`
- `NULL` handling

In this lesson, we move from retrieving individual records to analyzing groups of data.

As a Data Professional, you will frequently need to answer questions such as:

- How many employees work in each department?
- What is the average salary per department?
- Which department has the highest total salary cost?
- Which departments have an average salary above €50,000?
- How many employees work in each city?

To answer these questions, we need to understand three essential SQL concepts:

| Concept | Purpose |
|---|---|
| Aggregate Functions | Perform calculations across multiple rows |
| GROUP BY | Organize rows into groups |
| HAVING | Filter groups based on aggregate conditions |

### Learning Objectives

By the end of this lesson, you should be able to:

- Use `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`.
- Calculate totals, averages, minimums, and maximums.
- Group records using one or multiple columns.
- Combine multiple aggregate functions in one query.
- Understand the difference between `WHERE` and `HAVING`.
- Filter grouped results using aggregate conditions.
- Sort aggregated results using `ORDER BY`.
- Understand how aggregate functions handle `NULL` values.
- Translate business questions into SQL queries.

---

## 2. Practice Dataset

We will continue using the same `employees` table from Lesson 1.

### Employees Table

| employee_id | first_name | department | salary | city | manager_id |
|---:|---|---|---:|---|---:|
| 1 | John | IT | 55000 | Athens | 10 |
| 2 | Maria | HR | 42000 | Thessaloniki | 11 |
| 3 | George | IT | 65000 | Athens | 10 |
| 4 | Anna | Finance | 60000 | Patras | NULL |
| 5 | Peter | Marketing | 38000 | Athens | 12 |
| 6 | Elena | HR | 45000 | NULL | 11 |
| 7 | David | Finance | 72000 | Athens | NULL |
| 8 | Sofia | IT | 50000 | Heraklion | 10 |

### Dataset Overview

- Total employees: 8
- Departments: 4
- Cities represented: Athens, Thessaloniki, Patras, Heraklion, and NULL
- Total salary cost: 427,000
- Average salary: 53,375
- Highest salary: 72,000
- Lowest salary: 38,000

This dataset will be used throughout the lesson to demonstrate aggregate functions, grouping, and filtering.

---

## 3. Aggregate Functions

Aggregate functions perform calculations across multiple rows and return a single result.

SQL provides five commonly used aggregate functions:

| Function | Description | Example |
|---|---|---|
| `COUNT()` | Counts rows or non-NULL values | Number of employees |
| `SUM()` | Calculates the total | Total salary cost |
| `AVG()` | Calculates the average | Average salary |
| `MIN()` | Finds the minimum value | Lowest salary |
| `MAX()` | Finds the maximum value | Highest salary |

### 3.1 COUNT()

`COUNT()` is used to count rows or non-NULL values in a column.

**Example 1: Count all employees**

```sql
SELECT COUNT(*)
FROM employees;
```

Expected result:

| count |
|---:|
| 8 |

`COUNT(*)` counts every row, including rows containing NULL values.

**Example 2: Count employees with a manager**

```sql
SELECT COUNT(manager_id)
FROM employees;
```

Expected result:

| count |
|---:|
| 6 |

Why 6 instead of 8?

Because two employees have a NULL value in the `manager_id` column.

**Example 3: Count unique departments**

```sql
SELECT COUNT(DISTINCT department)
FROM employees;
```

Expected result:

| count |
|---:|
| 4 |

Important distinctions:

- `COUNT(*)`: Counts all rows.
- `COUNT(column)`: Counts only non-NULL values in that column.
- `COUNT(DISTINCT column)`: Counts unique, non-NULL values.

### 3.2 SUM()

`SUM()` calculates the total of a numeric column.

**Example: Calculate total salary cost**

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

Expected result:

| total_salary |
|---:|
| 427000 |

The `AS` keyword creates a column alias, making the output easier to understand.

### 3.3 AVG()

`AVG()` calculates the average of numeric values.

**Example: Calculate average salary**

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

Expected result:

| average_salary |
|---:|
| 53375 |

Calculation:

```text
(55000 + 42000 + 65000 + 60000 +
 38000 + 45000 + 72000 + 50000) / 8

= 427000 / 8

= 53375
```

Important: `AVG()` ignores NULL values. It does not treat them as zero.

### 3.4 MIN() and MAX()

`MIN()` finds the smallest value.

```sql
SELECT MIN(salary) AS lowest_salary
FROM employees;
```

Expected result: `38000`

`MAX()` finds the largest value.

```sql
SELECT MAX(salary) AS highest_salary
FROM employees;
```

Expected result: `72000`

### 3.5 Combining Aggregate Functions

You can use multiple aggregate functions in a single query.

```sql
SELECT
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MIN(salary) AS lowest_salary,
    MAX(salary) AS highest_salary
FROM employees;
```

Expected result:

| total_employees | total_salary | average_salary | lowest_salary | highest_salary |
|---:|---:|---:|---:|---:|
| 8 | 427000 | 53375 | 38000 | 72000 |

This query provides a complete salary overview of the company.

**Data Analyst Tip:** Combining aggregate functions is useful when building reports and creating business performance summaries.

---

## 4. GROUP BY

`GROUP BY` organizes rows that have the same values in specified columns.

It allows us to perform calculations separately for each group.

For example, instead of calculating the average salary of the entire company, we can calculate the average salary for each department.

### 4.1 Basic GROUP BY

**Business question:**

What is the average salary in each department?

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

Expected result:

| department | average_salary |
|---|---:|
| Finance | 66000 |
| HR | 43500 |
| IT | 56666.67 |
| Marketing | 38000 |

What happens?

1. SQL identifies the department values.
2. Employees are separated into department groups.
3. The average salary is calculated inside each group.
4. One result row is returned for each department.

### 4.2 GROUP BY with COUNT()

**Business question:**

How many employees work in each department?

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

Expected result:

| department | employee_count |
|---|---:|
| Finance | 2 |
| HR | 2 |
| IT | 3 |
| Marketing | 1 |

### 4.3 GROUP BY with SUM()

**Business question:**

What is the total salary cost for each department?

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

Expected result:

| department | total_salary |
|---|---:|
| Finance | 132000 |
| HR | 87000 |
| IT | 170000 |
| Marketing | 38000 |

### 4.4 Multiple Aggregate Functions

You can combine multiple aggregate functions with `GROUP BY`.

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MIN(salary) AS lowest_salary,
    MAX(salary) AS highest_salary
FROM employees
GROUP BY department;
```

Expected result:

| department | employee_count | total_salary | average_salary | lowest_salary | highest_salary |
|---|---:|---:|---:|---:|---:|
| Finance | 2 | 132000 | 66000 | 60000 | 72000 |
| HR | 2 | 87000 | 43500 | 42000 | 45000 |
| IT | 3 | 170000 | 56666.67 | 50000 | 65000 |
| Marketing | 1 | 38000 | 38000 | 38000 | 38000 |

This query is particularly useful for department-level reporting.

---

## 5. GROUP BY with Multiple Columns

SQL allows grouping by more than one column.

**Business question:**

How many employees work in each department in each city?

```sql
SELECT
    department,
    city,
    COUNT(*) AS employee_count
FROM employees
GROUP BY
    department,
    city;
```

Expected result:

| department | city | employee_count |
|---|---|---:|
| Finance | Athens | 1 |
| Finance | Patras | 1 |
| HR | Thessaloniki | 1 |
| HR | NULL | 1 |
| IT | Athens | 2 |
| IT | Heraklion | 1 |
| Marketing | Athens | 1 |

Each unique combination of department and city becomes a separate group.

For example:

- IT + Athens = one group.
- IT + Heraklion = another group.
- Finance + Athens = another group.
- Finance + Patras = another group.

**Important:** SQL groups NULL values together. That is why Elena appears in the HR group with a NULL city.

The order of columns in `GROUP BY` defines the grouping hierarchy, although it does not guarantee the order of the returned rows.

Use `ORDER BY` when you need sorted results.

---

## 6. WHERE vs HAVING

This is one of the most important concepts in this lesson.

Both `WHERE` and `HAVING` filter data, but they operate at different stages.

| WHERE | HAVING |
|---|---|
| Filters individual rows | Filters groups |
| Applied before GROUP BY | Applied after GROUP BY |
| Filters source records | Filters aggregated results |
| Cannot directly use aggregate functions at the same query level | Can use aggregate functions |

### 6.1 WHERE: Filtering Rows

`WHERE` filters individual records before grouping.

**Business question:**

Calculate the average salary per department, considering only employees earning more than 45,000.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
WHERE salary > 45000
GROUP BY department;
```

The query works in this order:

1. `WHERE` removes employees earning 45,000 or less.
2. `GROUP BY` groups the remaining employees.
3. `AVG()` calculates the average salary inside each group.

The average is therefore calculated only from employees who passed the WHERE condition.

### 6.2 HAVING: Filtering Groups

`HAVING` filters groups after aggregation.

**Business question:**

Show only departments with an average salary greater than 50,000.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

Expected result:

| department | average_salary |
|---|---:|
| Finance | 66000 |
| IT | 56666.67 |

HR and Marketing are excluded because their average salaries are below 50,000.

### 6.3 Using WHERE and HAVING Together

You can combine both clauses in the same query.

**Business question:**

Show departments where employees earning more than 40,000 have an average salary greater than 50,000.

```sql
SELECT
    department,
    AVG(salary) AS average_salary,
    COUNT(*) AS employee_count
FROM employees
WHERE salary > 40000
GROUP BY department
HAVING AVG(salary) > 50000;
```

The logic:

1. `WHERE salary > 40000`: Filters individual employees.
2. `GROUP BY department`: Creates department groups.
3. `AVG(salary)`: Calculates each department's average.
4. `HAVING AVG(salary) > 50000`: Keeps only qualifying groups.

Expected result:

| department | average_salary | employee_count |
|---|---:|---:|
| Finance | 66000 | 2 |
| IT | 56666.67 | 3 |

### The Golden Rule

```text
WHERE → GROUP BY → HAVING
```

Remember:

- `WHERE`: Which rows should participate?
- `GROUP BY`: How should the rows be grouped?
- `HAVING`: Which groups should remain?

---

## 7. ORDER BY with GROUP BY

You can sort aggregated results using `ORDER BY`.

**Business question:**

Show departments ordered by average salary, from highest to lowest.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
ORDER BY average_salary DESC;
```

Expected result:

| department | average_salary |
|---|---:|
| Finance | 66000 |
| IT | 56666.67 |
| HR | 43500 |
| Marketing | 38000 |

You can sort using an aggregate expression directly:

```sql
ORDER BY AVG(salary) DESC;
```

Using an alias such as `average_salary` is usually easier to read.

**Remember:**

- `ASC` = Ascending order.
- `DESC` = Descending order.

---

## 8. SQL Logical Processing Order

SQL is written in one order but logically processes query clauses in another order.

For the clauses covered in this lesson, the logical processing order is:

| Order | SQL Clause | Purpose |
|---:|---|---|
| 1 | FROM | Identifies the source table |
| 2 | WHERE | Filters individual rows |
| 3 | GROUP BY | Creates groups |
| 4 | HAVING | Filters groups |
| 5 | SELECT | Determines output columns and calculations |
| 6 | ORDER BY | Sorts the results |
| 7 | LIMIT | Restricts the number of returned rows, where supported |

This explains why aggregate functions cannot normally be used directly inside `WHERE`.

**Incorrect query:**

```sql
SELECT
    department,
    AVG(salary)
FROM employees
WHERE AVG(salary) > 50000
GROUP BY department;
```

The problem is that `WHERE` is processed before the groups and their averages are calculated.

**Correct query:**

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

---

## 9. Common Mistakes

### Mistake 1: Selecting a column without grouping or aggregating it

Incorrect:

```sql
SELECT
    department,
    first_name,
    AVG(salary)
FROM employees
GROUP BY department;
```

Why is this incorrect?

Each department can contain multiple employees with different first names. SQL cannot choose one arbitrary first name to represent the entire group.

Correct:

```sql
SELECT
    department,
    AVG(salary)
FROM employees
GROUP BY department;
```

### Mistake 2: Using WHERE with an aggregate function

Incorrect:

```sql
SELECT department, COUNT(*)
FROM employees
WHERE COUNT(*) > 2
GROUP BY department;
```

Correct:

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

### Mistake 3: Confusing COUNT(*) and COUNT(column)

```sql
COUNT(*)          -- Counts all rows
COUNT(manager_id) -- Counts only non-NULL manager IDs
```

### Mistake 4: Forgetting that AVG() ignores NULL

`AVG(salary)` calculates the average of non-NULL salary values.

It does not treat NULL as zero.

### Mistake 5: Forgetting non-aggregated columns in GROUP BY

If you select both `department` and `city` alongside an aggregate function, both columns generally need to appear in `GROUP BY`.

Correct:

```sql
SELECT
    department,
    city,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department, city;
```

### Mistake 6: Assuming GROUP BY sorts results

`GROUP BY` creates groups but does not guarantee their display order.

Use `ORDER BY` when sorting is required.

---

## 10. NULL Handling in Aggregate Functions

NULL values are especially important when working with aggregate functions.

Consider the following examples:

| Function | How it handles NULL |
|---|---|
| `COUNT(*)` | Counts rows even if they contain NULL |
| `COUNT(column)` | Ignores NULL values |
| `SUM(column)` | Ignores NULL values |
| `AVG(column)` | Ignores NULL values |
| `MIN(column)` | Ignores NULL values |
| `MAX(column)` | Ignores NULL values |

For example:

```sql
SELECT COUNT(manager_id)
FROM employees;
```

Returns `6`, because two manager IDs are NULL.

However:

```sql
SELECT COUNT(*)
FROM employees;
```

Returns `8`.

### Important distinction

Aggregate functions generally ignore NULL values, but `COUNT(*)` counts every row.

If all values in a column are NULL:

- `COUNT(column)` returns 0.
- `SUM(column)`, `AVG(column)`, `MIN(column)`, and `MAX(column)` return NULL.

This is important when interpreting business reports and calculating averages.

---

## 11. Practical Data Analyst Examples

Aggregate functions and grouping are used in almost every area of data analysis.

| Business Question | SQL Concept |
|---|---|
| How many customers do we have? | COUNT() |
| What is the total revenue? | SUM() |
| What is the average order value? | AVG() |
| Which product has the highest price? | MAX() |
| Which product has the lowest price? | MIN() |
| How many orders does each customer have? | GROUP BY + COUNT() |
| Which categories generate the most revenue? | GROUP BY + SUM() |
| Which customers have more than 10 orders? | GROUP BY + HAVING |
| Which departments have more than 5 employees? | GROUP BY + HAVING |
| Which regions have average sales above a target? | GROUP BY + AVG() + HAVING |

### Example: Business Reporting

Imagine an e-commerce company with an `orders` table.

A manager asks:

"Show the total revenue generated by each product category, from highest to lowest."

A possible query would be:

```sql
SELECT
    category,
    SUM(order_amount) AS total_revenue
FROM orders
GROUP BY category
ORDER BY total_revenue DESC;
```

This is a typical Data Analyst task.

The same logic can be applied to sales, customers, products, transactions, employees, and many other business datasets.

---

## 12. Practice Assignment

Use the same `employees` table.

### Employees Table

| employee_id | first_name | department | salary | city | manager_id |
|---:|---|---|---:|---|---:|
| 1 | John | IT | 55000 | Athens | 10 |
| 2 | Maria | HR | 42000 | Thessaloniki | 11 |
| 3 | George | IT | 65000 | Athens | 10 |
| 4 | Anna | Finance | 60000 | Patras | NULL |
| 5 | Peter | Marketing | 38000 | Athens | 12 |
| 6 | Elena | HR | 45000 | NULL | 11 |
| 7 | David | Finance | 72000 | Athens | NULL |
| 8 | Sofia | IT | 50000 | Heraklion | 10 |

Write SQL queries for the following exercises.

Try to solve each exercise independently before checking your notes.

### Level 1 – Fundamentals

**Exercise 1:** Find the total number of employees.

**Exercise 2:** Find the average salary of all employees. Name the result `average_salary`.

**Exercise 3:** Find the total salary cost for the company. Name the result `total_salary`.

**Exercise 4:** Find the highest and lowest salary in the company in the same query.

### Level 2 – GROUP BY

**Exercise 5:** Count how many employees work in each department.

**Exercise 6:** Calculate the average salary for each department. Sort the results from highest to lowest average salary.

**Exercise 7:** Find the total salary cost for each department. Display only the department and total salary.

**Exercise 8:** Count how many employees work in each city. Make sure you understand how NULL cities are handled.

### Level 3 – WHERE and HAVING

**Exercise 9:** Show departments with more than 1 employee.

**Exercise 10:** Show departments whose average salary is greater than 50,000.

**Exercise 11:** Calculate the average salary per department, considering only employees with salaries greater than 45,000.

**Exercise 12:** Show departments with at least 2 employees and a total salary greater than 100,000.

### Level 4 – Data Analyst Challenge

**Exercise 13:** Find departments where the highest salary is greater than 60,000. Display the department, highest salary, and average salary.

**Exercise 14:** Find departments where the average salary is greater than 45,000, but calculate that average using only employees whose salary is greater than 40,000. Sort by average salary descending.

### Practice Guidelines

For every exercise:

1. Understand the business question.
2. Identify which columns you need.
3. Decide whether you need an aggregate function.
4. Decide whether you need `GROUP BY`.
5. Decide whether the filter belongs in `WHERE` or `HAVING`.
6. Add `ORDER BY` if sorting is required.
7. Check whether your result makes logical sense.

---

## 13. SQL Cheat Sheet

### Aggregate Functions

```sql
-- Count all rows
SELECT COUNT(*) FROM employees;

-- Count non-NULL values
SELECT COUNT(manager_id) FROM employees;

-- Count unique departments
SELECT COUNT(DISTINCT department) FROM employees;

-- Calculate total salary
SELECT SUM(salary) FROM employees;

-- Calculate average salary
SELECT AVG(salary) FROM employees;

-- Find minimum salary
SELECT MIN(salary) FROM employees;

-- Find maximum salary
SELECT MAX(salary) FROM employees;
```

### GROUP BY

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

### GROUP BY with Multiple Columns

```sql
SELECT
    department,
    city,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department, city;
```

### WHERE with GROUP BY

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
WHERE salary > 45000
GROUP BY department;
```

### HAVING

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

### WHERE + GROUP BY + HAVING

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS average_salary
FROM employees
WHERE salary > 40000
GROUP BY department
HAVING AVG(salary) > 50000;
```

### GROUP BY with ORDER BY

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC;
```

---

## 14. Lesson Summary

In Lesson 2, we learned how to move beyond individual records and start analyzing data.

### Concepts Covered

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`
- `GROUP BY`
- GROUP BY with multiple columns
- `WHERE`
- `HAVING`
- `ORDER BY` with grouped results
- SQL logical processing order
- NULL handling in aggregate functions

### Key Takeaways

1. Aggregate functions perform calculations across multiple rows.
2. `GROUP BY` organizes records into groups.
3. `COUNT(*)` counts all rows, while `COUNT(column)` ignores NULL values.
4. Aggregate functions such as `AVG()` and `SUM()` ignore NULL values.
5. `WHERE` filters individual rows before grouping.
6. `HAVING` filters groups after aggregation.
7. `ORDER BY` sorts the final results.
8. Every non-aggregated column selected in a grouped query generally needs to appear in `GROUP BY`.

### Most Important Concept

Remember this sequence:

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
  ↓
LIMIT
```

Understanding this logical order will make more advanced SQL queries much easier to understand.

---

## 15. Next Lesson

### Lesson 3 – SQL JOINs

In the next lesson, we will learn how to combine data from multiple tables.

Topics will include:

- `INNER JOIN`
- `LEFT JOIN`
- `RIGHT JOIN`
- `FULL OUTER JOIN`
- `CROSS JOIN`
- Joining tables using primary and foreign keys
- Understanding relationships between tables
- Avoiding duplicate rows and incorrect joins
- Practical Data Analyst exercises

**Goal:** Learn how to retrieve meaningful information from multiple related database tables.

---

## Progress Tracker

| Lesson | Topic | Status |
|---|---|---|
| Lesson 01 | SELECT, WHERE, ORDER BY & NULL Values | Completed |
| Lesson 02 | Aggregations, GROUP BY & HAVING | Completed |
| Lesson 03 | SQL JOINs | Upcoming |
| Lesson 04 | Subqueries and CTES | Upcoming |
| Lesson 05 | CASE, date and string functions | Upcoming |
| Lesson 06 | String and Date Functions | Upcoming |

---

**End of Lesson 2 – Keep Practicing!**

The goal is not just to memorize SQL syntax. It is to understand how to translate real business questions into accurate SQL queries.

Practice consistently, check your results, and focus on understanding the logic behind every query.
