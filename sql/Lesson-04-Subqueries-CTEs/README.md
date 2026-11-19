# SQL Masterclass – Lesson 4: Subqueries and CTEs

## Table of Contents

1. [Introduction](#1-introduction)
2. [What Is a Subquery?](#2-what-is-a-subquery)
3. [Types of Subqueries](#3-types-of-subqueries)
   - [Scalar Subqueries](#31-scalar-subqueries)
   - [Multi-Row Subqueries with IN](#32-multi-row-subqueries-with-in)
   - [EXISTS and NOT EXISTS](#33-exists-and-not-exists)
   - [Correlated Subqueries](#34-correlated-subqueries)
4. [What Is a CTE?](#4-what-is-a-cte)
5. [CTE Structure](#5-cte-structure)
6. [Multiple CTEs](#6-multiple-ctes)
7. [Subqueries vs CTEs](#7-subqueries-vs-ctes)
8. [Common Mistakes](#8-common-mistakes)
9. [Practical Data Analyst Examples](#9-practical-data-analyst-examples)
10. [Practice Assignment](#10-practice-assignment)
11. [SQL Cheat Sheet](#11-sql-cheat-sheet)
12. [Lesson Summary](#12-lesson-summary)
13. [Next Lesson](#13-next-lesson)

---

## 1. Introduction

Welcome to Lesson 4 of the SQL Masterclass!

In this lesson, we explore two important SQL techniques:

- **Subqueries:** Queries inside other queries.
- **CTEs (Common Table Expressions):** Named temporary query results that help organize complex SQL statements.

These techniques are especially useful for Data Analysts because real-world business questions often require multiple calculation and filtering steps.

### Learning Objectives

By the end of this lesson, you should be able to:

- Understand and write subqueries.
- Use scalar and multi-row subqueries.
- Work with `IN`, `EXISTS`, and `NOT EXISTS`.
- Understand correlated subqueries.
- Create CTEs using `WITH`.
- Work with multiple CTEs.
- Decide when to use a subquery or a CTE.
- Write more readable and organized SQL queries.

### Dataset – Tables Used in This Lesson

For this lesson, we will continue working with the `employees` and `departments` tables introduced in Lesson 03.

These tables will help us practice:
- Scalar subqueries
- Multi-row subqueries
- `IN`, `EXISTS` and `NOT EXISTS`
- Correlated subqueries
- Common Table Expressions (CTEs)

### 1. Employees Table

The `employees` table contains information about employees, their departments and their salaries.

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

**Column descriptions:**

| Column | Description |
|---|---|
| employee_id | Unique identifier for each employee (Primary Key) |
| first_name | Employee's first name |
| department_id | Department assigned to the employee (Foreign Key) |
| salary | Employee's salary |

### 2. Departments Table

The `departments` table contains information about the company's departments.

| department_id | department_name |
|---:|---|
| 10 | IT |
| 20 | HR |
| 30 | Finance |
| 40 | Marketing |
| 50 | Sales |

**Column descriptions:**

| Column | Description |
|---|---|
| department_id | Unique identifier for each department (Primary Key) |
| department_name | Name of the department |

### 3. Table Relationship

The two tables are connected through the `department_id` column.

- `departments.department_id` is the Primary Key.
- `employees.department_id` is a Foreign Key referencing the departments table.
- An employee can belong to one department.
- A department can have multiple employees.
- An employee may have a NULL department ID or an ID that does not match an existing department.

---

## 2. What Is a Subquery?

A **subquery** is a SQL query inside another SQL query.

It allows us to use the result of one query in another query.

Think of it as two steps:

1. The inner query calculates or finds something.
2. The outer query uses that result to produce the final answer.

### Example: Find employees earning more than the company average

First, calculate the average salary:

```sql
SELECT AVG(salary)
FROM employees;
```

Expected result:

| avg |
|---:|
| 53375 |

Now use that calculation inside another query:

```sql
SELECT
    first_name,
    salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Expected result:

| first_name | salary |
|---|---:|
| George | 65000 |
| Anna | 60000 |
| David | 72000 |

### Explanation

- Inner query calculates the average salary.
- Outer query selects employees earning more than that average.

The inner query is executed as part of the outer query.

**Key idea:** A subquery helps us use the result of one query inside another.

---

## 3. Types of Subqueries

### 3.1 Scalar Subqueries

A scalar subquery returns exactly one value: one row and one column.

It is commonly used with comparison operators such as:

- `=`
- `>`
- `<`
- `>=`
- `<=`

Example: Find employees earning more than the average salary.

```sql
SELECT
    first_name,
    salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

The inner query returns one number: `53375`.

The outer query compares every employee's salary with that number.

Other possible use cases:

- Find products priced above the average product price.
- Find orders greater than the average order value.
- Find employees earning less than the maximum salary.

### 3.2 Multi-Row Subqueries with IN

Sometimes a subquery returns multiple rows instead of one value.

In this case, we can use `IN`.

Example: Find employees working in departments with more than one employee.

```sql
SELECT
    first_name,
    department_id
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM employees
    GROUP BY department_id
    HAVING COUNT(*) > 1
);
```

The inner query identifies department IDs with more than one employee.

The outer query returns employees whose department ID belongs to that list.

**IN means:** Check whether a value exists inside a list of values.

### 3.3 EXISTS and NOT EXISTS

`EXISTS` and `NOT EXISTS` are used to check whether matching records exist in another table.

#### EXISTS

Example: Find departments that have at least one employee.

```sql
SELECT
    d.department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

The query returns departments with at least one matching employee.

#### NOT EXISTS

Example: Find departments that have no employees.

```sql
SELECT
    d.department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

Expected result:

| department_name |
|---|
| Sales |

The Sales department has no employees assigned to it.

**Important:**

- `EXISTS` checks whether at least one matching row exists.
- `NOT EXISTS` checks whether no matching row exists.
- `SELECT 1` is a conventional way to write the existence check. Its actual value does not matter.

### 3.4 Correlated Subqueries

A correlated subquery is a subquery that references a column from the outer query.

Unlike a simple subquery, it depends on the current row being processed by the outer query.

Example: Find employees earning more than the average salary of their own department.

```sql
SELECT
    e.first_name,
    e.department_id,
    e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

### Explanation

The important condition is:

```sql
WHERE e2.department_id = e.department_id
```

It compares the department of the employee in the inner query with the department of the employee in the outer query.

For example:

- John is compared with the average salary in IT.
- Maria is compared with the average salary in HR.
- Anna is compared with the average salary in Finance.

Each employee is compared with their own department's average salary.

**Key idea:** A correlated subquery uses information from the outer query.

---

## 4. What Is a CTE?

CTE stands for **Common Table Expression**.

A CTE is a named temporary result that we define using the `WITH` keyword.

It exists for the duration of the SQL statement and can be referenced by the main query.

Think of it as creating a temporary named result to make a complex query easier to understand.

### Example: Find employees earning more than the average salary

Using a subquery:

```sql
SELECT
    first_name,
    salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Using a CTE:

```sql
WITH average_salary AS (
    SELECT AVG(salary) AS avg_salary
    FROM employees
)
SELECT
    e.first_name,
    e.salary
FROM employees e
CROSS JOIN average_salary a
WHERE e.salary > a.avg_salary;
```

Both queries produce the same result.

The difference is that the CTE gives the intermediate calculation a name: `average_salary`.

### Why Use CTEs?

CTEs are useful because they:

- Make complex queries easier to read.
- Separate calculations into logical steps.
- Allow a named result to be referenced multiple times within the same statement.
- Make SQL code easier to maintain.

---

## 5. CTE Structure

The general syntax is:

```sql
WITH cte_name AS (
    SELECT ...
    FROM ...
)
SELECT ...
FROM cte_name;
```

### Understanding the Structure

1. `WITH` starts the CTE.
2. `cte_name` gives the temporary result a name.
3. `AS` introduces the query that defines the CTE.
4. The query inside parentheses produces the result.
5. The main query uses the CTE.

### Example

```sql
WITH department_stats AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary,
        COUNT(*) AS employee_count
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_stats;
```

Expected result:

| department_id | avg_salary | employee_count |
|---:|---:|---:|
| NULL | 38000 | 1 |
| 10 | 56666.67 | 3 |
| 20 | 43500 | 2 |
| 30 | 60000 | 1 |
| 40 | 72000 | 1 |

The CTE calculates the average salary and employee count for every department ID.

The main query retrieves the result.

---

## 6. Multiple CTEs

We can create multiple CTEs in the same SQL statement.

This is especially useful when a business problem requires several calculation steps.

### Example: Find departments with an average salary above 50,000

```sql
WITH department_stats AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary,
        COUNT(*) AS employee_count
    FROM employees
    WHERE department_id IS NOT NULL
    GROUP BY department_id
),

high_salary_departments AS (
    SELECT *
    FROM department_stats
    WHERE avg_salary > 50000
)

SELECT *
FROM high_salary_departments;
```

### Explanation

**First CTE – department_stats**

Calculates:

- Average salary per department.
- Employee count per department.

**Second CTE – high_salary_departments**

Keeps only departments with an average salary above 50,000.

**Final query**

Returns the filtered result.

Expected result:

| department_id | avg_salary | employee_count |
|---:|---:|---:|
| 10 | 56666.67 | 3 |
| 30 | 60000 | 1 |
| 40 | 72000 | 1 |

**Key idea:** Multiple CTEs allow us to divide a complex SQL problem into smaller, understandable steps.

---

## 7. Subqueries vs CTEs

Both techniques can help solve similar problems, but they have different structures.

| Feature | Subquery | CTE |
|---|---|---|
| Syntax | Query inside another query | Uses `WITH` |
| Readability | Can become difficult when nested | Often easier for multi-step logic |
| Reuse | May need to repeat the query | Can reference the CTE multiple times in the same statement |
| Best use | Simple nested calculations | Complex, multi-step queries |

### When Should You Use Each?

**Use subqueries when:**

- You need a simple calculation inside a query.
- You need to filter rows using another query.
- You want to check whether matching records exist.

**Use CTEs when:**

- Your query has multiple logical steps.
- You want to separate calculations.
- You need to make complex SQL easier to understand.
- You want to reference an intermediate result more than once.

**Important:** CTEs are not automatically faster than subqueries. Query performance depends on the database engine, query structure, and optimizer.

---

## 8. Common Mistakes

### Mistake 1: Using = when a subquery returns multiple rows

Incorrect:

```sql
SELECT *
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM employees
    WHERE salary > 50000
);
```

The inner query can return multiple department IDs.

Using `=` with multiple returned rows can cause an error.

Correct:

```sql
SELECT *
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM employees
    WHERE salary > 50000
);
```

### Mistake 2: Forgetting the CTE name

Incorrect:

```sql
WITH AS (
    SELECT ...
)
```

Correct:

```sql
WITH department_stats AS (
    SELECT ...
)
```

### Mistake 3: Forgetting the main query after a CTE

A CTE must be followed by a main SQL statement that uses it.

Incorrect:

```sql
WITH department_stats AS (
    SELECT department_id, AVG(salary)
    FROM employees
    GROUP BY department_id
);
```

Correct:

```sql
WITH department_stats AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_stats;
```

### Mistake 4: Using NOT IN with NULL values

Be careful when using `NOT IN` with a subquery that may return NULL.

A NULL in the returned list can cause unexpected filtering because of SQL's NULL comparison rules.

For anti-join problems, `NOT EXISTS` is often a safer alternative.

### Mistake 5: Forgetting the correlation condition

A correlated subquery must correctly connect the inner query to the outer query.

For example:

```sql
WHERE e2.department_id = e.department_id
```

Without the correct condition, you might calculate an overall average instead of a department-specific average.

---

## 9. Practical Data Analyst Examples

Subqueries and CTEs are useful in real business analysis.

### Example 1: Employee salary analysis

Business question:

Which employees earn more than the company average?

Technique: Scalar subquery.

```sql
SELECT
    first_name,
    salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### Example 2: Department performance

Business question:

Which departments have an average salary above 50,000?

Technique: CTE.

```sql
WITH department_stats AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_stats
WHERE avg_salary > 50000;
```

### Example 3: Employees without a valid department

Business question:

Which employees are not assigned to an existing department?

Technique: `NOT EXISTS`.

```sql
SELECT
    e.employee_id,
    e.first_name,
    e.department_id
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM departments d
    WHERE d.department_id = e.department_id
);
```

This also includes employees whose `department_id` is NULL.

### Example 4: Department salary comparison

Business question:

Which employees earn more than their department's average salary?

Technique: Correlated subquery.

```sql
SELECT
    e.first_name,
    e.department_id,
    e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

---

## 10. Practice Assignment

Use the same `employees` and `departments` tables from Lesson 3.

### Beginner – Subqueries

**Exercise 1:** Find employees earning more than the company's average salary.

**Exercise 2:** Find employees earning less than the company's average salary.

**Exercise 3:** Find employees working in departments with more than one employee.

**Exercise 4:** Find departments that have at least one employee using `EXISTS`.

**Exercise 5:** Find departments that have no employees using `NOT EXISTS`.

### Intermediate – Correlated Subqueries

**Exercise 6:** Find employees earning more than the average salary of their own department.

**Exercise 7:** Find the highest-paid employee in each department using a correlated subquery.

**Exercise 8:** Find employees earning the maximum salary in the entire company.

### CTEs

**Exercise 9:** Create a CTE that calculates the average salary per department.

**Exercise 10:** Using a CTE, find departments with average salaries above 50,000.

**Exercise 11:** Create a CTE that calculates the total salary per department.

**Exercise 12:** Using two CTEs, find departments with at least two employees and an average salary above 50,000.

### Challenge – Data Analyst Level

**Exercise 13:** Find employees whose salary is above the average salary of the entire company, using a CTE.

**Exercise 14:** Using a CTE, calculate the average salary per department and return only employees earning more than their department's average.

---

## 11. SQL Cheat Sheet

| What you want | SQL technique |
|---|---|
| Compare against one calculated value | Scalar subquery |
| Compare against a list of values | `IN` |
| Check whether matching records exist | `EXISTS` |
| Find records without matches | `NOT EXISTS` |
| Compare each row with its group | Correlated subquery |
| Break a query into named steps | CTE (`WITH`) |
| Create multiple named query steps | Multiple CTEs |

### Important Syntax

**Scalar subquery:**

```sql
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

**Multi-row subquery:**

```sql
WHERE department_id IN (
    SELECT department_id
    FROM employees
);
```

**EXISTS:**

```sql
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

**NOT EXISTS:**

```sql
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

**CTE:**

```sql
WITH cte_name AS (
    SELECT ...
)
SELECT *
FROM cte_name;
```

**Multiple CTEs:**

```sql
WITH first_cte AS (
    SELECT ...
),
second_cte AS (
    SELECT ...
    FROM first_cte
)
SELECT *
FROM second_cte;
```

---

## 12. Lesson Summary

In this lesson, we learned:

- A subquery is a query inside another query.
- Scalar subqueries return one value.
- Multi-row subqueries can be used with `IN`.
- `EXISTS` checks whether matching records exist.
- `NOT EXISTS` finds records without matching records.
- Correlated subqueries reference the outer query.
- CTEs use the `WITH` keyword to create named temporary results.
- Multiple CTEs help organize complex SQL statements.
- Subqueries and CTEs are both important tools for Data Analysts.

### Progress Tracker

| Lesson | Topic | Status |
|---|---|---|
| Lesson 01 | SELECT, WHERE, ORDER BY & NULL Values | Completed |
| Lesson 02 | Aggregations, GROUP BY & HAVING | Completed |
| Lesson 03 | JOINs | Completed |
| Lesson 04 | Subqueries and CTEs | Completed |
| Lesson 05 | CASE, String and Date Functions | Upcoming |

---

## 13. Next Lesson

### Lesson 5 – CASE WHEN, Date & String Functions

In the next lesson, we will learn how to create conditional logic inside SQL queries.

Topics will include:

- `CASE WHEN`
- Multiple conditions
- `ELSE`
- Creating categories from numerical values
- Conditional aggregations
- Date & String Functions
- Practical Data Analyst examples

**Keep practicing. The goal is not just to understand SQL syntax, but to learn how to solve real business problems with SQL.**
