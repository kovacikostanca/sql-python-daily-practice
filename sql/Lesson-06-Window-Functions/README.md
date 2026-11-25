# SQL Masterclass – Lesson 06 – SQL Window Functions

## 1. Overview

Welcome to Lesson 06 of the SQL Masterclass!

In this lesson, we explore **SQL Window Functions**, an essential skill for Data Analysts. Window functions allow us to perform calculations across related rows while keeping the individual rows visible in the result.

Unlike `GROUP BY`, which combines multiple rows into summary rows, window functions preserve the original level of detail.

We will focus on:

- `ROW_NUMBER()`
- `RANK()`
- `DENSE_RANK()`
- `OVER()`
- `PARTITION BY`
- Running totals using `SUM() OVER()`
- Window frames
- Top N records per group
- Practical analytical reporting

**SQL dialect:** PostgreSQL.

---

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Explain what a window function is.
- Understand the difference between window functions and `GROUP BY`.
- Rank records using `ROW_NUMBER()`, `RANK()` and `DENSE_RANK()`.
- Restart rankings within groups using `PARTITION BY`.
- Identify the highest-paid employee in each department.
- Retrieve the top N employees per department.
- Calculate running totals.
- Understand how window frames work.
- Combine CTEs and window functions in analytical queries.
- Apply window functions to real Data Analyst tasks.

---

## 3. Practice Dataset

We will reuse the `employees` and `orders` tables from previous lessons.

### 3.1 Employees Table

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

### 3.2 Orders Table

| order_id | customer_name | order_date | order_amount |
|---:|---|---|---:|
| 101 | Alice | 2025-01-15 | 250 |
| 102 | Bob | 2025-02-10 | 400 |
| 103 | Alice | 2025-02-20 | 150 |
| 104 | Charlie | 2025-03-05 | 600 |
| 105 | Diana | 2025-03-15 | 300 |
| 106 | Bob | 2025-04-01 | 500 |

### Important Dataset Notes

- There are 8 employees.
- Department 10 has 3 employees.
- Department 20 has 2 employees.
- Department 30 has 1 employee.
- Department 40 has 1 employee.
- Peter has no assigned department.
- All employee salaries are different in this dataset.
- The orders table contains 6 orders.

---

# PART A – SQL WINDOW FUNCTIONS

## 4. What Are Window Functions?

A window function performs a calculation across a set of rows related to the current row.

The key difference:

**GROUP BY:**
- Combines rows into summary rows.
- Reduces the number of rows returned.

**Window functions:**
- Calculate across related rows.
- Preserve individual rows.
- Allow us to compare each row with group-level or overall values.

### Example: GROUP BY

```sql
SELECT
    department_id,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department_id;
```

This returns one row per department.

### Example: Window Function

```sql
SELECT
    employee_id,
    first_name,
    department_id,
    salary,
    AVG(salary) OVER (
        PARTITION BY department_id
    ) AS department_avg_salary
FROM employees;
```

This returns every employee, together with their department's average salary.

**Expected result:**

| first_name | department_id | salary | department_avg_salary |
|---|---:|---:|---:|
| John | 10 | 55000 | 56666.67 |
| George | 10 | 65000 | 56666.67 |
| Sofia | 10 | 50000 | 56666.67 |
| Maria | 20 | 42000 | 43500 |
| Elena | 20 | 45000 | 43500 |
| Anna | 30 | 60000 | 60000 |
| David | 40 | 72000 | 72000 |
| Peter | NULL | 38000 | 38000 |

The exact row order is not guaranteed without a final `ORDER BY`.

---

## 5. Understanding OVER()

The `OVER()` clause tells SQL that we are defining a window for the calculation.

Basic syntax:

```sql
FUNCTION_NAME() OVER (
    PARTITION BY column_name
    ORDER BY column_name
)
```

The main components are:

| Component | Purpose |
|---|---|
| `OVER()` | Defines the window |
| `PARTITION BY` | Divides rows into groups |
| `ORDER BY` | Defines the order within each window |
| Window frame | Defines which rows are included in a calculation |

Not every window function needs all these components.

---

## 6. ROW_NUMBER()

`ROW_NUMBER()` assigns a unique sequential number to every row.

### Syntax

```sql
ROW_NUMBER() OVER (
    ORDER BY salary DESC
)
```

### Example: Rank All Employees by Salary

```sql
SELECT
    employee_id,
    first_name,
    salary,
    ROW_NUMBER() OVER (
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

**Expected result:**

| first_name | salary | row_num |
|---|---:|---:|
| David | 72000 | 1 |
| George | 65000 | 2 |
| Anna | 60000 | 3 |
| John | 55000 | 4 |
| Sofia | 50000 | 5 |
| Elena | 45000 | 6 |
| Maria | 42000 | 7 |
| Peter | 38000 | 8 |

### Important

`ROW_NUMBER()` always assigns a different number to each row, even if two employees have the same salary.

If ties exist and you want deterministic results, add a tie-breaker:

```sql
ROW_NUMBER() OVER (
    ORDER BY salary DESC, employee_id
)
```

### Analyst Use Case

You can use `ROW_NUMBER()` to:

- Select the latest transaction for each customer.
- Identify the first purchase.
- Remove duplicate records while retaining one row.
- Retrieve the top employee in each department.

---

## 7. PARTITION BY

`PARTITION BY` divides rows into separate groups before applying the window function.

It is similar to grouping conceptually, but it does not collapse the rows.

### Example: Number Employees Within Each Department

```sql
SELECT
    employee_id,
    first_name,
    department_id,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department_id
        ORDER BY salary DESC
    ) AS department_row_num
FROM employees;
```

**Expected result:**

| first_name | department_id | salary | department_row_num |
|---|---:|---:|---:|
| George | 10 | 65000 | 1 |
| John | 10 | 55000 | 2 |
| Sofia | 10 | 50000 | 3 |
| Elena | 20 | 45000 | 1 |
| Maria | 20 | 42000 | 2 |
| Anna | 30 | 60000 | 1 |
| David | 40 | 72000 | 1 |
| Peter | NULL | 38000 | 1 |

Notice that the numbering restarts for every department.

**Important:** Rows with `NULL` in `department_id` form their own partition.

---

## 8. RANK() and DENSE_RANK()

Both functions rank rows based on the specified ordering.

The difference appears when multiple rows have the same value.

### 8.1 RANK()

`RANK()` assigns the same rank to tied values, then skips subsequent rank numbers.

Example:

| Employee | Salary | RANK |
|---|---:|---:|
| David | 72000 | 1 |
| George | 65000 | 2 |
| John | 65000 | 2 |
| Anna | 60000 | 4 |

Rank 3 is skipped because two employees share rank 2.

### 8.2 DENSE_RANK()

`DENSE_RANK()` also assigns the same rank to tied values, but it does not skip numbers.

| Employee | Salary | DENSE_RANK |
|---|---:|---:|
| David | 72000 | 1 |
| George | 65000 | 2 |
| John | 65000 | 2 |
| Anna | 60000 | 3 |

### 8.3 SQL Example

```sql
SELECT
    employee_id,
    first_name,
    salary,
    RANK() OVER (
        ORDER BY salary DESC
    ) AS salary_rank,
    DENSE_RANK() OVER (
        ORDER BY salary DESC
    ) AS salary_dense_rank
FROM employees;
```

In our current dataset, salaries are unique, so both ranking functions return the same sequence.

### 8.4 Comparison

| Function | Unique number for every row? | Handles ties | Skips ranks after ties? |
|---|---|---|---|
| ROW_NUMBER() | Yes | Gives separate numbers | Not applicable |
| RANK() | No | Same rank | Yes |
| DENSE_RANK() | No | Same rank | No |

### When Should You Use Each?

- `ROW_NUMBER()`: When you need exactly one unique sequence number per row.
- `RANK()`: When tied values should share a rank and gaps are meaningful.
- `DENSE_RANK()`: When tied values should share a rank without gaps.

---

## 9. Top N Records per Group

One of the most important Data Analyst applications of window functions is retrieving the top N records within each group.

### Business Question

Who are the two highest-paid employees in each department?

We can use `ROW_NUMBER()` with `PARTITION BY`.

### Step 1: Create the Ranking

```sql
SELECT
    employee_id,
    first_name,
    department_id,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department_id
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

### Step 2: Filter the Ranking

We cannot directly filter a window function in the same query's `WHERE` clause in PostgreSQL.

We can use a CTE:

```sql
WITH employee_ranking AS (
    SELECT
        employee_id,
        first_name,
        department_id,
        salary,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS row_num
    FROM employees
)

SELECT *
FROM employee_ranking
WHERE row_num <= 2;
```

### Expected Result

| first_name | department_id | salary | row_num |
|---|---:|---:|---:|
| George | 10 | 65000 | 1 |
| John | 10 | 55000 | 2 |
| Elena | 20 | 45000 | 1 |
| Maria | 20 | 42000 | 2 |
| Anna | 30 | 60000 | 1 |
| David | 40 | 72000 | 1 |
| Peter | NULL | 38000 | 1 |

Departments with fewer than two employees return fewer than two rows.

### Important Business Decision

If you need exactly two employees per department, use `ROW_NUMBER()`.

If you want to include everyone tied for the second position, consider `RANK()` or `DENSE_RANK()` instead.

---

## 10. Running Totals

A running total calculates the cumulative sum of a value as we move through the rows.

This is very common in financial and sales analysis.

### Example: Total Sales Over Time

```sql
SELECT
    order_id,
    customer_name,
    order_date,
    order_amount,
    SUM(order_amount) OVER (
        ORDER BY order_date, order_id
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS running_total
FROM orders;
```

### Expected Result

| order_id | order_date | order_amount | running_total |
|---:|---|---:|---:|
| 101 | 2025-01-15 | 250 | 250 |
| 102 | 2025-02-10 | 400 | 650 |
| 103 | 2025-02-20 | 150 | 800 |
| 104 | 2025-03-05 | 600 | 1400 |
| 105 | 2025-03-15 | 300 | 1700 |
| 106 | 2025-04-01 | 500 | 2200 |

The running total increases after every order.

### Explanation

```sql
SUM(order_amount) OVER (...)
```

Calculates a sum over a window.

```sql
ORDER BY order_date, order_id
```

Defines the sequence in which orders are accumulated.

```sql
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
```

Includes all rows from the beginning of the window through the current row.

### Why Use a Tie-Breaker?

If multiple orders have the same date, ordering only by `order_date` may not give a deterministic row-by-row sequence.

Adding `order_id` creates a stable order.

---

## 11. Running Totals per Customer

We can calculate a separate running total for every customer using `PARTITION BY`.

```sql
SELECT
    order_id,
    customer_name,
    order_date,
    order_amount,
    SUM(order_amount) OVER (
        PARTITION BY customer_name
        ORDER BY order_date, order_id
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS customer_running_total
FROM orders;
```

### Expected Result

| customer_name | order_date | order_amount | customer_running_total |
|---|---|---:|---:|
| Alice | 2025-01-15 | 250 | 250 |
| Alice | 2025-02-20 | 150 | 400 |
| Bob | 2025-02-10 | 400 | 400 |
| Bob | 2025-04-01 | 500 | 900 |
| Charlie | 2025-03-05 | 600 | 600 |
| Diana | 2025-03-15 | 300 | 300 |

Each customer's running total starts again from zero.

---

## 12. Understanding Window Frames

A window frame defines which rows are included in a window calculation for the current row.

For running totals, we commonly use:

```sql
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
```

This means:

- `UNBOUNDED PRECEDING`: Start from the first row in the partition.
- `CURRENT ROW`: Stop at the current row.
- `ROWS`: Work with physical rows in the ordered sequence.

### Why Does This Matter?

With an ordered aggregate window, PostgreSQL's default frame includes peer rows with the same ordering value.

If you need a strictly row-by-row running total, explicitly define a `ROWS` frame and use a deterministic ordering.

### Full Example

```sql
SELECT
    order_id,
    order_date,
    order_amount,
    SUM(order_amount) OVER (
        ORDER BY order_date, order_id
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS running_total
FROM orders;
```

---

## 13. Combining Window Functions with CTEs

CTEs make complex analytical queries easier to read.

### Example: Department Salary Report

```sql
WITH department_report AS (
    SELECT
        employee_id,
        first_name,
        department_id,
        salary,
        AVG(salary) OVER (
            PARTITION BY department_id
        ) AS department_avg_salary,
        RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS salary_rank
    FROM employees
)

SELECT *
FROM department_report
ORDER BY department_id, salary_rank;
```

This report provides:

- Employee information.
- Department average salary.
- Employee salary ranking within the department.

### Example: Monthly Sales and Cumulative Sales

First, aggregate the orders by month. Then calculate the cumulative total across the monthly results.

```sql
WITH monthly_sales AS (
    SELECT
        DATE_TRUNC('month', order_date)::date AS sales_month,
        COUNT(*) AS total_orders,
        SUM(order_amount) AS total_sales
    FROM orders
    GROUP BY DATE_TRUNC('month', order_date)
)

SELECT
    sales_month,
    total_orders,
    total_sales,
    SUM(total_sales) OVER (
        ORDER BY sales_month
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS cumulative_sales
FROM monthly_sales
ORDER BY sales_month;
```

### Expected Result

| sales_month | total_orders | total_sales | cumulative_sales |
|---|---:|---:|---:|
| 2025-01-01 | 1 | 250 | 250 |
| 2025-02-01 | 2 | 550 | 800 |
| 2025-03-01 | 2 | 900 | 1700 |
| 2025-04-01 | 1 | 500 | 2200 |

This is a common monthly performance reporting pattern.

---

## 14. Common Mistakes

| Mistake | Why It Is Wrong | Correct Approach |
|---|---|---|
| Using `GROUP BY` when individual rows are needed | It collapses rows | Use a window function |
| Forgetting `OVER()` | Window functions require an `OVER` clause | Add `OVER()` |
| Forgetting `PARTITION BY` | Ranking continues across the entire dataset | Partition by the relevant group |
| Filtering a window function directly in `WHERE` | Window calculations happen after `WHERE` | Use a CTE or subquery |
| Assuming `RANK()` and `DENSE_RANK()` are identical | They handle gaps differently | Choose based on the business requirement |
| Using `ROW_NUMBER()` when ties must be included | It forces unique row numbers | Consider `RANK()` |
| Forgetting a tie-breaker | Row order may be nondeterministic | Add a unique ordering column |
| Ignoring window frames | Running totals may behave unexpectedly with tied order values | Define an explicit frame when needed |

---

## 15. Real-World Data Analyst Use Cases

Window functions are frequently used for:

1. **Top performers:** Find the highest-performing employees, products or sales representatives.
2. **Customer behavior:** Identify a customer's first or most recent purchase.
3. **Financial reporting:** Calculate cumulative revenue and balances.
4. **Sales analysis:** Compare each month's performance with previous months.
5. **Ranking:** Rank products by revenue within each category.
6. **Data cleaning:** Identify duplicate records using `ROW_NUMBER()`.
7. **Business monitoring:** Compare individual results against group averages.

---

# PART B – PRACTICE ASSIGNMENT

## 16. SQL Exercises

Use the `employees` and `orders` datasets.

Try to solve the exercises independently before checking the solutions.

### Section A – ROW_NUMBER, RANK and DENSE_RANK

**Exercise 1 – Employee Row Numbers**

Return all employees with a row number based on salary from highest to lowest.

Expected columns:
- `employee_id`
- `first_name`
- `salary`
- `row_num`

**Exercise 2 – Department Row Numbers**

Assign a row number to each employee within their department, ordered by salary descending.

Expected columns:
- `first_name`
- `department_id`
- `salary`
- `department_row_num`

**Exercise 3 – Company Salary Ranking**

Rank all employees by salary using `RANK()`.

**Exercise 4 – Dense Salary Ranking**

Repeat Exercise 3 using `DENSE_RANK()`.

Explain the difference between the two ranking functions when ties exist.

### Section B – Top N per Group

**Exercise 5 – Highest-Paid Employee per Department**

Use a CTE and `ROW_NUMBER()` to return the highest-paid employee in each department.

**Exercise 6 – Top Two Employees per Department**

Return the two highest-paid employees in each department.

### Section C – Analytical Comparisons

**Exercise 7 – Department Average Salary**

Return each employee together with the average salary of their department.

**Exercise 8 – Salary Difference**

Return:
- Employee name.
- Salary.
- Department average salary.
- Difference between individual salary and department average.

Use a window function.

### Section D – Running Totals

**Exercise 9 – Company Running Total**

Calculate the cumulative order amount over time.

**Exercise 10 – Customer Running Total**

Calculate a separate running total for each customer.

**Exercise 11 – Monthly Cumulative Sales**

First calculate monthly sales using a CTE. Then calculate cumulative sales across the months.

**Exercise 12 – Complete Sales Report**

Create a report containing:

- `order_id`
- `customer_name`
- `order_date`
- `order_amount`
- Row number ordered by date.
- Running total of sales.

Use a window function for both calculations.

---

## 17. Mini Project – Employee and Sales Analytics

Create two analytical reports.

### Project A – Employee Performance Report

Your report should contain:

- Employee name.
- Department ID.
- Salary.
- Salary category using `CASE`.
- Salary rank within the department.
- Department average salary.
- Difference between employee salary and department average.

Required concepts:

- `CASE`
- `RANK()` or `DENSE_RANK()`
- `PARTITION BY`
- `AVG() OVER()`

### Project B – Monthly Sales Performance

Your report should contain:

- Month.
- Total orders.
- Total sales.
- Average order value.
- Cumulative sales.

Required concepts:

- `DATE_TRUNC()`
- `COUNT()`
- `SUM()`
- `AVG()`
- CTEs.
- Running totals using window functions.

---

## 18. SQL Cheat Sheet

| Requirement | SQL |
|---|---|
| Unique row numbering | `ROW_NUMBER() OVER (ORDER BY salary DESC)` |
| Ranking with gaps | `RANK() OVER (ORDER BY salary DESC)` |
| Ranking without gaps | `DENSE_RANK() OVER (ORDER BY salary DESC)` |
| Ranking per group | `ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC)` |
| Average per group without collapsing rows | `AVG(salary) OVER (PARTITION BY department_id)` |
| Company-wide average | `AVG(salary) OVER ()` |
| Running total | `SUM(amount) OVER (ORDER BY date)` |
| Running total per customer | `SUM(amount) OVER (PARTITION BY customer ORDER BY date)` |
| Explicit row-by-row frame | `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` |

---


## What Comes Next?

In the next SQL lesson, we will expand our window-function knowledge with:

- `LAG()` and `LEAD()`
- `FIRST_VALUE()` and `LAST_VALUE()`
- Month-over-month comparisons.
- Percentage change calculations.
- Advanced analytical reporting.
- SQL interview exercises.

**Keep practising. Window functions are one of the most valuable SQL skills for a Data Analyst.**
