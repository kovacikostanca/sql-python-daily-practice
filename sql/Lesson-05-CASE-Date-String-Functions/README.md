# SQL Masterclass – Lesson 05: CASE, Date & String Functions

## Overview

This lesson focuses on three important groups of SQL functions used in everyday Data Professional work:

- **CASE expressions** – classify data and create conditional logic.
- **Date functions** – extract, filter, calculate, and group dates.
- **String functions** – clean, format, combine, and extract text data.

These techniques are especially useful when preparing datasets, building reports, creating business categories, and analyzing sales trends.

**SQL dialect:** PostgreSQL

---

## 1. CASE Expressions

The `CASE` expression allows you to apply conditional logic in SQL. It works similarly to `IF / ELSE` statements in programming languages.

### Basic syntax

```sql
SELECT
    employee_id,
    first_name,
    salary,
    CASE
        WHEN salary > 60000 THEN 'High'
        WHEN salary > 45000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

### Important concepts

- SQL evaluates conditions from top to bottom.
- The first matching condition is returned.
- `ELSE` handles all remaining cases.
- `END` is required to close the expression.
- `CASE` can be used inside `SELECT`, `ORDER BY`, and aggregate functions.

### Conditional aggregation

You can combine `CASE` with aggregate functions to count or calculate values based on conditions.

```sql
SELECT
    COUNT(CASE WHEN salary > 50000 THEN 1 END) AS high_salary_count,
    COUNT(CASE WHEN salary <= 50000 THEN 1 END) AS lower_salary_count
FROM employees;
```

### Common use cases

- Employee salary classification
- Customer segmentation
- Order status categorization
- Conditional counting and summation
- Business reporting

---

## 2. Date Functions

Date functions allow you to extract information from dates, filter records by time periods, and calculate time differences.

### Extracting date components

PostgreSQL uses `EXTRACT()` to retrieve specific parts of a date.

```sql
SELECT
    order_id,
    order_date,
    EXTRACT(YEAR FROM order_date) AS order_year,
    EXTRACT(MONTH FROM order_date) AS order_month,
    EXTRACT(DAY FROM order_date) AS order_day
FROM orders;
```

### Filtering by date

A reliable way to filter all orders from February 2025:

```sql
SELECT *
FROM orders
WHERE order_date >= '2025-02-01'
  AND order_date < '2025-03-01';
```

This approach also works safely when `order_date` contains timestamps.

### Date arithmetic

PostgreSQL supports date calculations using intervals.

```sql
SELECT
    order_id,
    order_date,
    order_date + INTERVAL '7 days' AS date_after_7_days
FROM orders;
```

Calculate the number of days between an order date and today:

```sql
SELECT
    order_id,
    order_date,
    CURRENT_DATE - order_date AS days_since_order
FROM orders;
```

### Grouping by month

`DATE_TRUNC()` is useful for creating monthly reports.

```sql
SELECT
    DATE_TRUNC('month', order_date) AS order_month,
    SUM(order_amount) AS total_sales
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY order_month;
```

### Common use cases

- Monthly and yearly sales reports
- Quarterly performance analysis
- Customer purchase recency
- Date-based filtering
- Time-series analysis

---

## 3. String Functions

String functions help clean and transform text data.

They are especially important when working with customer names, email addresses, product descriptions, and other text fields.

| Function | Purpose | Example |
|---|---|---|
| `UPPER()` | Converts text to uppercase | `UPPER('alice')` → `ALICE` |
| `LOWER()` | Converts text to lowercase | `LOWER('BOB')` → `bob` |
| `TRIM()` | Removes leading and trailing spaces | `TRIM(' Alice ')` → `Alice` |
| `LTRIM()` | Removes leading spaces | `LTRIM(' Alice')` → `Alice` |
| `RTRIM()` | Removes trailing spaces | `RTRIM('Alice ')` → `Alice` |
| `CONCAT()` | Combines strings | `CONCAT('John', ' Smith')` |
| `LENGTH()` | Returns the number of characters | `LENGTH('Alice')` → `5` |
| `SUBSTRING()` | Extracts part of a string | `SUBSTRING('Analytics' FROM 1 FOR 4)` → `Anal` |
| `REPLACE()` | Replaces part of a string | `REPLACE('abc', 'a', 'x')` → `xbc` |
| `POSITION()` | Finds the position of a substring | `POSITION('@' IN 'a@gmail.com')` |
| `INITCAP()` | Capitalizes the first letter of each word | `INITCAP('john smith')` → `John Smith` |

### Combining strings

PostgreSQL supports both `CONCAT()` and the `||` operator.

```sql
SELECT
    first_name || ' ' || last_name AS full_name
FROM customers;
```

### Cleaning customer data

```sql
SELECT
    INITCAP(TRIM(first_name)) AS clean_first_name,
    INITCAP(TRIM(last_name)) AS clean_last_name,
    LOWER(TRIM(email)) AS clean_email
FROM customers;
```

**Important:** Functions such as `TRIM()` and `LOWER()` change the query output, not the original stored data. To permanently update records, you need an `UPDATE` statement.

### Common use cases

- Cleaning inconsistent customer names
- Standardizing email addresses
- Combining first and last names
- Extracting email domains
- Preparing text fields for analysis

---

## 4. Practical Data Analyst Examples

### Example 1: Salary classification

```sql
SELECT
    first_name,
    salary,
    CASE
        WHEN salary > 60000 THEN 'High'
        WHEN salary > 45000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

### Example 2: Monthly sales report

```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    COUNT(*) AS total_orders,
    SUM(order_amount) AS total_sales,
    AVG(order_amount) AS average_order_value
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month;
```

### Example 3: High-value sales

```sql
SELECT
    COUNT(CASE WHEN order_amount > 300 THEN 1 END) AS high_value_orders,
    SUM(CASE WHEN order_amount > 300 THEN order_amount ELSE 0 END) AS high_value_sales
FROM orders;
```

### Example 4: Extracting email domains

```sql
SELECT
    email,
    SUBSTRING(email FROM POSITION('@' IN email) + 1) AS email_domain
FROM customers;
```

---

## 5. SQL Dialect Differences

Date functions can vary between database systems.

| Task | PostgreSQL | MySQL | SQL Server |
|---|---|---|---|
| Extract year | `EXTRACT(YEAR FROM date)` | `YEAR(date)` | `YEAR(date)` |
| Extract month | `EXTRACT(MONTH FROM date)` | `MONTH(date)` | `MONTH(date)` |
| Current date | `CURRENT_DATE` | `CURRENT_DATE()` | `CAST(GETDATE() AS DATE)` |
| Add days | `date + INTERVAL '7 days'` | `DATE_ADD(date, INTERVAL 7 DAY)` | `DATEADD(DAY, 7, date)` |
| Truncate to month | `DATE_TRUNC('month', date)` | `DATE_FORMAT(date, '%Y-%m-01')` | `DATEFROMPARTS(YEAR(date), MONTH(date), 1)` |

**Note:** This lesson uses PostgreSQL syntax. Always check the documentation for the database system you are using.

---

## 6. Practice Exercises

Use the `employees`, `departments`, `orders`, and `customers` tables from the lesson datasets.

### Beginner – CASE

1. Create a `salary_category` column:
   - High: salary above 60,000
   - Medium: salary above 45,000 and up to 60,000
   - Low: salary up to 45,000

2. Count how many employees belong to each salary category.

3. Create an `employee_type` column based on department IDs:
   - 10 → IT
   - 20 → HR
   - 30 → Finance
   - All other values → Other

4. Count employees earning above 50,000 and employees earning 50,000 or less using conditional aggregation.

### Intermediate – Date Functions

5. Extract the year and month from every order date.

6. Retrieve all orders placed in February 2025.

7. Calculate the number of days between each order date and today.

8. Calculate total sales per month.

9. Retrieve all orders placed during the first quarter of 2025.

### Intermediate – String Functions

10. Convert customer first and last names to uppercase.

11. Remove unnecessary spaces from customer names.

12. Create a `full_name` column by combining first and last names.

13. Extract the domain from each email address.

14. Calculate the number of characters in each email address.

### Challenge – Data Analyst Tasks

15. Classify employees by salary category and calculate the average salary for each category.

16. Create a monthly sales report containing:
    - Month
    - Total orders
    - Total sales
    - Average order value

17. Clean customer names and emails using `TRIM()`, `INITCAP()`, and `LOWER()`.

18. Calculate the percentage of orders with an amount above 300 compared with all orders.

---

## 7. Key Takeaways

After completing this lesson, you should be able to:

- Use `CASE WHEN` to create conditional categories.
- Apply conditional aggregation with `COUNT()` and `SUM()`.
- Extract year, month, and day from dates.
- Filter records using date ranges.
- Perform date arithmetic with intervals.
- Group sales data by month using `DATE_TRUNC()`.
- Clean text using string functions.
- Combine, extract, and standardize string values.
- Apply these techniques to practical Data Analyst reporting tasks.

---

## 8. Next Lesson

**Lesson 06 – Window Functions**

Topics to explore:

- `OVER()`
- `PARTITION BY`
- `ORDER BY` inside window functions
- `ROW_NUMBER()`
- `RANK()` and `DENSE_RANK()`
- `LAG()` and `LEAD()`
- Running totals and moving averages

Window functions are essential for more advanced SQL analysis and are frequently used in Data Analyst interviews.

---

