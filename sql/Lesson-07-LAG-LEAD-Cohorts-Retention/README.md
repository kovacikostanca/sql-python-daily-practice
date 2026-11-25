# Lesson 07 – SQL: LAG(), LEAD(), Cohort Analysis & Retention

## 1. Overview

Welcome to Lesson 07 of the SQL Masterclass!

In this lesson, we move into more advanced SQL analysis, focusing on customer behaviour, purchasing patterns, cohort analysis and customer retention.

These concepts are particularly important for Data Analysts working in:

- E-commerce.
- Marketing Analytics.
- Customer Analytics.
- Product Analytics.
- Business Intelligence.
- Revenue and Sales Analytics.

We will learn how to compare records over time, identify customers' first and repeat purchases, group customers into cohorts and calculate customer retention rates.

### Topics Covered

- `LAG()` and `LEAD()`
- Comparing current and previous periods.
- Month-over-month changes.
- Percentage change calculations.
- First and repeat purchases.
- Customer purchase history.
- Cohort analysis.
- Cohort assignment.
- Cohort activity.
- Customer retention.
- Retention percentages.
- Retention matrices.
- Practical customer analytics.

**SQL dialect:** PostgreSQL.

---

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Explain the difference between `LAG()` and `LEAD()`.
- Retrieve previous and next row values.
- Use `PARTITION BY` with window functions.
- Calculate month-over-month changes.
- Calculate percentage changes.
- Identify customers' first purchases.
- Identify repeat customers.
- Calculate the time between purchases.
- Explain what cohort analysis is.
- Assign customers to cohorts.
- Calculate cohort sizes.
- Calculate monthly customer activity.
- Calculate retention counts and percentages.
- Build a customer retention matrix.
- Interpret customer retention results from a business perspective.

---

## 3. Practice Datasets

We will use three datasets throughout this lesson:

1. `monthly_sales`
2. `customers`
3. `orders`

These datasets represent a fictional e-commerce company.

### 3.1 Monthly Sales Dataset

| month | month_number | total_sales |
|---|---:|---:|
| January | 1 | 10000 |
| February | 2 | 12000 |
| March | 3 | 9000 |
| April | 4 | 15000 |
| May | 5 | 18000 |
| June | 6 | 16000 |

This dataset will be used for:

- Previous month comparisons.
- Next month comparisons.
- Sales differences.
- Month-over-month percentage changes.

### 3.2 Customers Dataset

| customer_id | customer_name | signup_date |
|---:|---|---|
| 1 | Alice | 2025-01-05 |
| 2 | Bob | 2025-01-12 |
| 3 | Charlie | 2025-02-03 |
| 4 | Diana | 2025-02-15 |
| 5 | Elena | 2025-03-02 |
| 6 | Frank | 2025-03-20 |
| 7 | George | 2025-04-04 |
| 8 | Helen | 2025-04-18 |

This dataset represents customers who registered with the company.

### 3.3 Orders Dataset

| order_id | customer_id | order_date | amount |
|---:|---:|---|---:|
| 101 | 1 | 2025-01-10 | 100 |
| 102 | 1 | 2025-02-15 | 150 |
| 103 | 1 | 2025-03-20 | 200 |
| 104 | 2 | 2025-01-20 | 80 |
| 105 | 2 | 2025-03-10 | 120 |
| 106 | 3 | 2025-02-10 | 90 |
| 107 | 3 | 2025-03-15 | 110 |
| 108 | 4 | 2025-02-20 | 200 |
| 109 | 4 | 2025-04-05 | 100 |
| 110 | 5 | 2025-03-05 | 130 |
| 111 | 5 | 2025-04-10 | 170 |
| 112 | 6 | 2025-03-25 | 70 |
| 113 | 7 | 2025-04-10 | 160 |
| 114 | 7 | 2025-05-15 | 190 |
| 115 | 8 | 2025-04-20 | 110 |

### Dataset Relationships

The `customers` and `orders` tables are related through `customer_id`.

- One customer can have multiple orders.
- Every order belongs to one customer.
- A customer may have no orders.
- The first order date can be different from the customer's signup date.

**Important:** In this lesson, we define customer cohorts based on the first purchase date, not the signup date.

---

# PART A – LAG() AND LEAD()

## 4. Understanding LAG() and LEAD()

`LAG()` and `LEAD()` are SQL window functions.

They allow us to retrieve values from other rows without joining a table to itself.

- `LAG()` retrieves a value from a previous row.
- `LEAD()` retrieves a value from a following row.

They are useful for analysing changes over time and comparing records.

### Basic Syntax

**LAG():**

```sql
LAG(column_name, offset, default_value)
OVER (
    PARTITION BY grouping_column
    ORDER BY ordering_column
)
```

**LEAD():**

```sql
LEAD(column_name, offset, default_value)
OVER (
    PARTITION BY grouping_column
    ORDER BY ordering_column
)
```

| Parameter | Meaning |
|---|---|
| `column_name` | The value to retrieve |
| `offset` | Number of rows backwards or forwards |
| `default_value` | Value returned if the requested row does not exist |
| `PARTITION BY` | Optional grouping |
| `ORDER BY` | Defines the sequence of rows |

The default offset is 1.

---

## 5. LAG() – Retrieve Previous Row Values

Imagine that we are analysing a company's monthly sales.

We want to compare every month's sales with the previous month.

### Example

```sql
SELECT
    month,
    total_sales,
    LAG(total_sales) OVER (
        ORDER BY month_number
    ) AS previous_month_sales
FROM monthly_sales
ORDER BY month_number;
```

### Expected Result

| Month | Total Sales | Previous Month Sales |
|---|---:|---:|
| January | 10000 | NULL |
| February | 12000 | 10000 |
| March | 9000 | 12000 |
| April | 15000 | 9000 |
| May | 18000 | 15000 |
| June | 16000 | 18000 |

January returns `NULL` because there is no previous month in our dataset.

### How Does It Work?

```sql
LAG(total_sales) OVER (
    ORDER BY month_number
)
```

SQL:

1. Orders the rows by month.
2. Looks at the previous row.
3. Returns the previous row's `total_sales` value.

### LAG() with an Offset

We can also retrieve a value from two rows earlier.

```sql
SELECT
    month,
    total_sales,
    LAG(total_sales, 2) OVER (
        ORDER BY month_number
    ) AS sales_two_months_ago
FROM monthly_sales
ORDER BY month_number;
```

The first two rows return `NULL` because there are no two previous rows.

### LAG() with a Default Value

We can specify a default value instead of `NULL`.

```sql
SELECT
    month,
    total_sales,
    LAG(total_sales, 1, 0) OVER (
        ORDER BY month_number
    ) AS previous_month_sales
FROM monthly_sales
ORDER BY month_number;
```

January will return `0` instead of `NULL`.

Use a default value only when it makes sense for the business calculation. A missing previous period is not necessarily the same as zero sales.

---

## 6. Month-over-Month Sales Difference

Now we can calculate the difference between current and previous month sales.

### Formula

```text
Sales Difference = Current Month Sales - Previous Month Sales
```

### SQL Query

```sql
SELECT
    month,
    total_sales,

    LAG(total_sales) OVER (
        ORDER BY month_number
    ) AS previous_month_sales,

    total_sales - LAG(total_sales) OVER (
        ORDER BY month_number
    ) AS sales_difference

FROM monthly_sales
ORDER BY month_number;
```

### Expected Result

| Month | Total Sales | Previous Sales | Difference |
|---|---:|---:|---:|
| January | 10000 | NULL | NULL |
| February | 12000 | 10000 | 2000 |
| March | 9000 | 12000 | -3000 |
| April | 15000 | 9000 | 6000 |
| May | 18000 | 15000 | 3000 |
| June | 16000 | 18000 | -2000 |

Positive values indicate an increase.

Negative values indicate a decrease.

---

## 7. Month-over-Month Percentage Change

Data Analysts frequently calculate percentage changes to measure business performance.

### Formula

\[
\text{Percentage Change} =
\frac{\text{Current Value} - \text{Previous Value}}
{\text{Previous Value}} \times 100
\]

### SQL Query

```sql
WITH sales_comparison AS (
    SELECT
        month,
        month_number,
        total_sales,

        LAG(total_sales) OVER (
            ORDER BY month_number
        ) AS previous_sales

    FROM monthly_sales
)

SELECT
    month,
    total_sales,
    previous_sales,

    ROUND(
        100.0 * (total_sales - previous_sales)
        / NULLIF(previous_sales, 0),
        2
    ) AS percentage_change

FROM sales_comparison
ORDER BY month_number;
```

### Expected Result

| Month | Total Sales | Previous Sales | Percentage Change |
|---|---:|---:|---:|
| January | 10000 | NULL | NULL |
| February | 12000 | 10000 | 20.00 |
| March | 9000 | 12000 | -25.00 |
| April | 15000 | 9000 | 66.67 |
| May | 18000 | 15000 | 20.00 |
| June | 16000 | 18000 | -11.11 |

### Important: NULLIF()

```sql
NULLIF(previous_sales, 0)
```

This prevents division by zero.

If `previous_sales` is zero, `NULLIF()` returns `NULL`.

This avoids a division-by-zero error.

---

## 8. LEAD() – Retrieve Next Row Values

`LEAD()` works in the opposite direction of `LAG()`.

It retrieves a value from a following row.

### Example

```sql
SELECT
    month,
    total_sales,

    LEAD(total_sales) OVER (
        ORDER BY month_number
    ) AS next_month_sales

FROM monthly_sales
ORDER BY month_number;
```

### Expected Result

| Month | Total Sales | Next Month Sales |
|---|---:|---:|
| January | 10000 | 12000 |
| February | 12000 | 9000 |
| March | 9000 | 15000 |
| April | 15000 | 18000 |
| May | 18000 | 16000 |
| June | 16000 | NULL |

June returns `NULL` because there is no following month.

### LAG() vs LEAD()

| Function | Direction | Typical Use |
|---|---|---|
| `LAG()` | Previous row | Compare current month with previous month |
| `LEAD()` | Next row | Compare current purchase with the next purchase |

---

## 9. LAG() and LEAD() with PARTITION BY

Let's imagine we have monthly sales for different products.

| Product | Month Number | Sales |
|---|---:|---:|
| Laptop | 1 | 10000 |
| Laptop | 2 | 12000 |
| Laptop | 3 | 15000 |
| Phone | 1 | 8000 |
| Phone | 2 | 9000 |
| Phone | 3 | 7500 |

We want to compare each product's sales with its own previous month.

```sql
SELECT
    product,
    month_number,
    sales,

    LAG(sales) OVER (
        PARTITION BY product
        ORDER BY month_number
    ) AS previous_month_sales

FROM product_sales
ORDER BY product, month_number;
```

### Expected Result

| Product | Month | Sales | Previous Sales |
|---|---:|---:|---:|
| Laptop | 1 | 10000 | NULL |
| Laptop | 2 | 12000 | 10000 |
| Laptop | 3 | 15000 | 12000 |
| Phone | 1 | 8000 | NULL |
| Phone | 2 | 9000 | 8000 |
| Phone | 3 | 7500 | 9000 |

The previous value resets for every product.

This is the purpose of `PARTITION BY`.

---

# PART B – CUSTOMER PURCHASE BEHAVIOUR

## 10. Identifying First Purchases

A customer's first purchase is an important starting point for behavioural analysis.

We can use `ROW_NUMBER()` to number each customer's orders.

```sql
SELECT
    order_id,
    customer_id,
    order_date,
    amount,

    ROW_NUMBER() OVER (
        PARTITION BY customer_id
        ORDER BY order_date, order_id
    ) AS purchase_number

FROM orders
ORDER BY customer_id, order_date, order_id;
```

### Expected Interpretation

| Customer | Order Date | Purchase Number |
|---:|---|---:|
| 1 | 2025-01-10 | 1 |
| 1 | 2025-02-15 | 2 |
| 1 | 2025-03-20 | 3 |
| 2 | 2025-01-20 | 1 |
| 2 | 2025-03-10 | 2 |
| 3 | 2025-02-10 | 1 |
| 3 | 2025-03-15 | 2 |

The first purchase for every customer has `purchase_number = 1`.

### Retrieve Only First Purchases

```sql
WITH ranked_orders AS (
    SELECT
        order_id,
        customer_id,
        order_date,
        amount,

        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY order_date, order_id
        ) AS purchase_number

    FROM orders
)

SELECT *
FROM ranked_orders
WHERE purchase_number = 1;
```

This is a common analytical pattern for identifying first purchases.

---

## 11. Previous and Next Customer Purchases

Now let's analyse the time between customer purchases.

```sql
SELECT
    customer_id,
    order_id,
    order_date,
    amount,

    LAG(order_date) OVER (
        PARTITION BY customer_id
        ORDER BY order_date, order_id
    ) AS previous_purchase_date,

    LEAD(order_date) OVER (
        PARTITION BY customer_id
        ORDER BY order_date, order_id
    ) AS next_purchase_date

FROM orders
ORDER BY customer_id, order_date, order_id;
```

### Calculate Days Between Purchases

PostgreSQL allows us to subtract two dates.

```sql
WITH purchase_history AS (
    SELECT
        customer_id,
        order_id,
        order_date,

        LAG(order_date) OVER (
            PARTITION BY customer_id
            ORDER BY order_date, order_id
        ) AS previous_purchase_date

    FROM orders
)

SELECT
    customer_id,
    order_id,
    order_date,
    previous_purchase_date,

    order_date - previous_purchase_date
        AS days_between_purchases

FROM purchase_history
ORDER BY customer_id, order_date, order_id;
```

### Example: Alice

- First purchase: 10 January.
- Second purchase: 15 February.
- Third purchase: 20 March.

The query calculates the number of days between each purchase.

**Analyst Use Case:** Identify customers who are purchasing less frequently or whose time between purchases is increasing.

---

# PART C – COHORT ANALYSIS

## 12. What Is Cohort Analysis?

A cohort is a group of customers who share a common characteristic or starting event.

In customer retention analysis, we commonly define cohorts using the month of a customer's first purchase.

### Example

| Customer | First Purchase | Cohort |
|---|---|---|
| Alice | January 2025 | January 2025 |
| Bob | January 2025 | January 2025 |
| Charlie | February 2025 | February 2025 |
| Diana | February 2025 | February 2025 |
| Elena | March 2025 | March 2025 |
| Frank | March 2025 | March 2025 |
| George | April 2025 | April 2025 |
| Helen | April 2025 | April 2025 |

We have four cohorts:

- January 2025: Alice and Bob.
- February 2025: Charlie and Diana.
- March 2025: Elena and Frank.
- April 2025: George and Helen.

### Why Is Cohort Analysis Important?

It helps businesses understand:

- Whether newer customers are more engaged.
- Whether customer retention is improving.
- How long customers continue purchasing.
- Which acquisition periods produced more valuable customers.
- Whether a product or marketing change affected customer behaviour.

Instead of analysing all customers together, we compare groups that started at different times.

---

## 13. Step 1 – Identify Each Customer's First Purchase

We need to calculate the first purchase date for every customer.

```sql
WITH first_purchase AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_purchase_date
    FROM orders
    GROUP BY customer_id
)

SELECT *
FROM first_purchase
ORDER BY customer_id;
```

### Expected Result

| Customer ID | First Purchase Date |
|---:|---|
| 1 | 2025-01-10 |
| 2 | 2025-01-20 |
| 3 | 2025-02-10 |
| 4 | 2025-02-20 |
| 5 | 2025-03-05 |
| 6 | 2025-03-25 |
| 7 | 2025-04-10 |
| 8 | 2025-04-20 |

Why use `MIN()`?

Because we want the earliest purchase date for every customer.

---

## 14. Step 2 – Assign Each Customer to a Cohort

We can use `DATE_TRUNC()` to extract the month of the first purchase.

```sql
WITH first_purchase AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_purchase_date
    FROM orders
    GROUP BY customer_id
)

SELECT
    customer_id,
    first_purchase_date,

    DATE_TRUNC(
        'month',
        first_purchase_date
    )::date AS cohort_month

FROM first_purchase
ORDER BY customer_id;
```

### Expected Result

| Customer ID | First Purchase | Cohort Month |
|---:|---|---|
| 1 | 2025-01-10 | 2025-01-01 |
| 2 | 2025-01-20 | 2025-01-01 |
| 3 | 2025-02-10 | 2025-02-01 |
| 4 | 2025-02-20 | 2025-02-01 |
| 5 | 2025-03-05 | 2025-03-01 |
| 6 | 2025-03-25 | 2025-03-01 |
| 7 | 2025-04-10 | 2025-04-01 |
| 8 | 2025-04-20 | 2025-04-01 |

The `cohort_month` becomes the customer's starting month for our analysis.

---

## 15. Step 3 – Calculate Customer Activity by Month

Now we need to know which months each customer made purchases.

We will connect each order to the customer's cohort.

```sql
WITH first_purchase AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_purchase_date
    FROM orders
    GROUP BY customer_id
),

customer_cohorts AS (
    SELECT
        customer_id,

        DATE_TRUNC(
            'month',
            first_purchase_date
        )::date AS cohort_month

    FROM first_purchase
),

customer_activity AS (
    SELECT DISTINCT
        customer_id,

        DATE_TRUNC(
            'month',
            order_date
        )::date AS activity_month

    FROM orders
)

SELECT
    c.customer_id,
    c.cohort_month,
    a.activity_month

FROM customer_cohorts c

JOIN customer_activity a
    ON c.customer_id = a.customer_id

ORDER BY
    c.cohort_month,
    c.customer_id,
    a.activity_month;
```

### Expected Interpretation

Alice belongs to the January cohort and has activity in:

- January.
- February.
- March.

Bob belongs to the January cohort and has activity in:

- January.
- March.

We use `DISTINCT` because retention normally counts whether a customer was active in a month, not how many orders they made during that month.

---

## 16. Step 4 – Calculate Cohort Month Number

We need to know how many months have passed since each customer's cohort month.

We call this `cohort_index` or `months_since_cohort`.

### Example

| Cohort Month | Activity Month | Cohort Index |
|---|---|---:|
| January | January | 0 |
| January | February | 1 |
| January | March | 2 |
| February | February | 0 |
| February | March | 1 |
| March | March | 0 |

We can calculate this using PostgreSQL's `AGE()` and `EXTRACT()` functions.

```sql
WITH first_purchase AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_purchase_date
    FROM orders
    GROUP BY customer_id
),

customer_cohorts AS (
    SELECT
        customer_id,

        DATE_TRUNC(
            'month',
            first_purchase_date
        )::date AS cohort_month

    FROM first_purchase
),

customer_activity AS (
    SELECT DISTINCT
        customer_id,

        DATE_TRUNC(
            'month',
            order_date
        )::date AS activity_month

    FROM orders
),

cohort_activity AS (
    SELECT
        c.customer_id,
        c.cohort_month,
        a.activity_month,

        (
            EXTRACT(
                YEAR FROM AGE(
                    a.activity_month,
                    c.cohort_month
                )
            ) * 12

            + EXTRACT(
                MONTH FROM AGE(
                    a.activity_month,
                    c.cohort_month
                )
            )
        )::int AS cohort_index

    FROM customer_cohorts c

    JOIN customer_activity a
        ON c.customer_id = a.customer_id
)

SELECT *
FROM cohort_activity
ORDER BY cohort_month, cohort_index, customer_id;
```

### Interpretation

- `cohort_index = 0`: Customer's first purchase month.
- `cohort_index = 1`: One month after the first purchase month.
- `cohort_index = 2`: Two months after the first purchase month.

---

# PART D – RETENTION ANALYSIS

## 17. What Is Customer Retention?

Customer retention measures how many customers continue using or purchasing from a business after an initial period.

For this lesson, we define a retained customer as someone who makes at least one purchase in a later calendar month.

### Example

If 100 customers make their first purchase in January and 40 of them purchase again in February:

\[
\text{February Retention} =
\frac{40}{100} \times 100 = 40\%
\]

Retention is an important business KPI because it helps measure customer engagement and loyalty.

### Retention vs Repeat Purchase

These concepts are related but not identical:

- **Repeat purchase:** A customer makes more than one purchase.
- **Monthly retention:** A customer is active in a later calendar month.
- **Customer retention rate:** The percentage of the original cohort active in a later period, according to the chosen definition.

A customer can make multiple purchases within their first month without being retained in a later month.

---

## 18. Step 5 – Calculate Retained Customers

We combine the previous steps to count active customers for every cohort and activity month.

```sql
WITH first_purchase AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_purchase_date
    FROM orders
    GROUP BY customer_id
),

customer_cohorts AS (
    SELECT
        customer_id,

        DATE_TRUNC(
            'month',
            first_purchase_date
        )::date AS cohort_month

    FROM first_purchase
),

customer_activity AS (
    SELECT DISTINCT
        customer_id,

        DATE_TRUNC(
            'month',
            order_date
        )::date AS activity_month

    FROM orders
),

cohort_activity AS (
    SELECT
        c.customer_id,
        c.cohort_month,
        a.activity_month,

        (
            EXTRACT(
                YEAR FROM AGE(
                    a.activity_month,
                    c.cohort_month
                )
            ) * 12

            + EXTRACT(
                MONTH FROM AGE(
                    a.activity_month,
                    c.cohort_month
                )
            )
        )::int AS cohort_index

    FROM customer_cohorts c

    JOIN customer_activity a
        ON c.customer_id = a.customer_id
)

SELECT
    cohort_month,
    cohort_index,

    COUNT(DISTINCT customer_id) AS active_customers

FROM cohort_activity

GROUP BY
    cohort_month,
    cohort_index

ORDER BY
    cohort_month,
    cohort_index;
```

### Expected Result

| Cohort Month | Cohort Index | Active Customers |
|---|---:|---:|
| January | 0 | 2 |
| January | 1 | 1 |
| January | 2 | 2 |
| February | 0 | 2 |
| February | 1 | 1 |
| February | 2 | 1 |
| March | 0 | 2 |
| March | 1 | 1 |
| April | 0 | 2 |
| April | 1 | 1 |

### Understanding January

- January: Alice and Bob = 2 active customers.
- February: Alice = 1 active customer.
- March: Alice and Bob = 2 active customers.

Bob did not purchase in February, but returned in March.

This is why period retention does not necessarily decline continuously.

---

## 19. Step 6 – Calculate Retention Percentage

Now we need the original cohort size.

### Formula

\[
\text{Retention Rate} =
\frac{\text{Active Customers in Period}}
{\text{Original Cohort Size}} \times 100
\]

For January:

- Original cohort size = 2.
- February active customers = 1.
- February retention = 50%.
- March active customers = 2.
- March retention = 100%.

### Complete Retention Query

```sql
WITH first_purchase AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_purchase_date
    FROM orders
    GROUP BY customer_id
),

customer_cohorts AS (
    SELECT
        customer_id,

        DATE_TRUNC(
            'month',
            first_purchase_date
        )::date AS cohort_month

    FROM first_purchase
),

cohort_sizes AS (
    SELECT
        cohort_month,
        COUNT(*) AS cohort_size
    FROM customer_cohorts
    GROUP BY cohort_month
),

customer_activity AS (
    SELECT DISTINCT
        customer_id,

        DATE_TRUNC(
            'month',
            order_date
        )::date AS activity_month

    FROM orders
),

cohort_activity AS (
    SELECT
        c.customer_id,
        c.cohort_month,
        a.activity_month,

        (
            EXTRACT(
                YEAR FROM AGE(
                    a.activity_month,
                    c.cohort_month
                )
            ) * 12

            + EXTRACT(
                MONTH FROM AGE(
                    a.activity_month,
                    c.cohort_month
                )
            )
        )::int AS cohort_index

    FROM customer_cohorts c

    JOIN customer_activity a
        ON c.customer_id = a.customer_id
),

retention_counts AS (
    SELECT
        cohort_month,
        cohort_index,

        COUNT(DISTINCT customer_id) AS active_customers

    FROM cohort_activity

    GROUP BY
        cohort_month,
        cohort_index
)

SELECT
    r.cohort_month,
    r.cohort_index,
    s.cohort_size,
    r.active_customers,

    ROUND(
        100.0 * r.active_customers / s.cohort_size,
        2
    ) AS retention_rate

FROM retention_counts r

JOIN cohort_sizes s
    ON r.cohort_month = s.cohort_month

ORDER BY
    r.cohort_month,
    r.cohort_index;
```

### Expected Result

| Cohort Month | Index | Cohort Size | Active Customers | Retention |
|---|---:|---:|---:|---:|
| January | 0 | 2 | 2 | 100% |
| January | 1 | 2 | 1 | 50% |
| January | 2 | 2 | 2 | 100% |
| February | 0 | 2 | 2 | 100% |
| February | 1 | 2 | 1 | 50% |
| February | 2 | 2 | 1 | 50% |
| March | 0 | 2 | 2 | 100% |
| March | 1 | 2 | 1 | 50% |
| April | 0 | 2 | 2 | 100% |
| April | 1 | 2 | 1 | 50% |

**Important:** Month 0 is always 100% in this purchase-based cohort query because each customer is assigned to their cohort based on their first purchase.

---

## 20. Step 7 – Build a Retention Matrix

A retention matrix is a common way to present cohort results to business stakeholders.

Instead of showing every period on a separate row, we can use conditional aggregation to create columns for Month 0, Month 1, Month 2 and so on.

Use the CTEs from the previous retention query, then replace its final `SELECT` with:

```sql
SELECT
    r.cohort_month,
    MAX(s.cohort_size) AS cohort_size,

    ROUND(
        100.0 * SUM(
            CASE
                WHEN r.cohort_index = 0
                THEN r.active_customers
                ELSE 0
            END
        ) / MAX(s.cohort_size),
        2
    ) AS month_0,

    ROUND(
        100.0 * SUM(
            CASE
                WHEN r.cohort_index = 1
                THEN r.active_customers
                ELSE 0
            END
        ) / MAX(s.cohort_size),
        2
    ) AS month_1,

    ROUND(
        100.0 * SUM(
            CASE
                WHEN r.cohort_index = 2
                THEN r.active_customers
                ELSE 0
            END
        ) / MAX(s.cohort_size),
        2
    ) AS month_2

FROM retention_counts r

JOIN cohort_sizes s
    ON r.cohort_month = s.cohort_month

GROUP BY r.cohort_month

ORDER BY r.cohort_month;
```

### Expected Retention Matrix

| Cohort | Size | Month 0 | Month 1 | Month 2 |
|---|---:|---:|---:|---:|
| January | 2 | 100% | 50% | 100% |
| February | 2 | 100% | 50% | 50% |
| March | 2 | 100% | 50% | NULL |
| April | 2 | 100% | 50% | NULL |

Why are March and April Month 2 values `NULL`?

Because our dataset ends before those cohorts have had enough time to reach Month 2.

A missing future period is not the same as zero retention.

We should not automatically interpret an unobserved period as 0%.

---

# PART E – COMMON MISTAKES AND ANALYST CONSIDERATIONS

## Common SQL Mistakes

| Mistake | Explanation | Correct Approach |
|---|---|---|
| Forgetting `ORDER BY` in LAG/LEAD | The previous or next row is not properly defined | Specify a meaningful ordering |
| Forgetting `PARTITION BY` | Values can be compared across different customers or products | Partition by the relevant entity |
| Counting orders instead of customers | Customers with multiple orders may be counted more than once | Use `COUNT(DISTINCT customer_id)` |
| Using signup date when measuring purchase cohorts | Signup and first purchase are different events | Define the cohort event explicitly |
| Treating missing future months as zero | The observation period may not be complete | Distinguish missing periods from zero activity |
| Forgetting to remove duplicate customer-month activity | Multiple orders inflate retention counts | Use `SELECT DISTINCT` |
| Dividing by the wrong cohort size | Retention percentages become incorrect | Divide by the original cohort population |
| Ignoring the reporting period | Recent cohorts have less time to mature | Compare only periods with sufficient observation time |

## Important Business Definitions

Before writing a retention query, always establish the business definition.

### Questions to Ask

1. Is a cohort based on signup date or first purchase date?
2. Does customer activity mean a purchase, login or any interaction?
3. Is retention measured daily, weekly or monthly?
4. Does the first month count as Month 0?
5. Should a customer who returns after skipping a month count as retained?
6. Are we measuring all customers or only customers who made a purchase?

Different definitions can produce different retention results.

### Two Common Retention Approaches

**Classic period retention**

Counts customers active in each individual period. Customers can return after an inactive period.

**Consecutive retention**

Counts customers who remain active in every consecutive period since the cohort start.

The SQL query in this lesson calculates classic period retention, not consecutive retention.

---

# PART F – PRACTICE ASSIGNMENT

## SQL Exercises

Use the `monthly_sales`, `customers` and `orders` datasets from this lesson.

Try to solve the exercises independently before checking any solutions.

### Section A – LAG() and LEAD()

**Exercise 1 – Previous Month Sales**

Use `LAG()` to retrieve the previous month's sales.

Return:

- Month.
- Current sales.
- Previous month's sales.

**Exercise 2 – Sales Difference**

Calculate the difference between current month sales and previous month sales.

**Exercise 3 – Percentage Change**

Calculate the month-over-month percentage change in sales.

**Exercise 4 – Next Month Sales**

Use `LEAD()` to retrieve the next month's sales.

**Exercise 5 – Product Comparison**

Using the `product_sales` example, retrieve the previous month's sales for each product.

**Exercise 6 – Customer Purchase History**

For every order, return:

- Customer ID.
- Order date.
- Previous purchase date.
- Next purchase date.

Use both `LAG()` and `LEAD()`.

**Exercise 7 – Days Between Purchases**

Calculate the number of days between each customer's current and previous purchase.

### Section B – First Purchases and Repeat Purchases

**Exercise 8 – First Purchase**

Find the first purchase date for every customer.

**Exercise 9 – Purchase Number**

Assign a sequential purchase number to every order for each customer using `ROW_NUMBER()`.

**Exercise 10 – Repeat Customers**

Identify customers who made more than one purchase.

Return:

- Customer ID.
- Number of orders.
- Total spending.

**Exercise 11 – Days to Second Purchase**

Calculate how many days passed between each customer's first and second purchases.

Hint: Use `ROW_NUMBER()` and `LAG()` or conditional aggregation.

### Section C – Cohort Analysis

**Exercise 12 – Customer Cohorts**

Assign each customer to a cohort based on their first purchase month.

**Exercise 13 – Cohort Sizes**

Calculate the number of customers in each cohort.

**Exercise 14 – Monthly Customer Activity**

Calculate the number of unique active customers for every calendar month.

**Exercise 15 – Cohort Activity**

Return:

- Cohort month.
- Activity month.
- Cohort index.
- Number of active customers.

### Section D – Retention Analysis

**Exercise 16 – Retention Counts**

Calculate the number of active customers in every cohort period.

**Exercise 17 – Retention Percentage**

Calculate the retention percentage for each cohort period.

**Exercise 18 – Retention Matrix**

Create a retention matrix containing:

- Cohort month.
- Cohort size.
- Month 0 retention.
- Month 1 retention.
- Month 2 retention.

**Exercise 19 – Best Retention Cohort**

Identify which cohort has the highest Month 1 retention percentage.

**Exercise 20 – Returning Customers**

Find customers who made a purchase in at least two different calendar months.

Return:

- Customer ID.
- Number of active months.
- First purchase date.
- Latest purchase date.

---

## Mini Project – E-commerce Customer Retention Analysis

Imagine you are working as a Data Analyst for an e-commerce company.

The management team wants to understand how customers behave after their first purchase.

Your task is to build a complete SQL customer retention report.

### Project A – Customer Acquisition

Calculate:

- Total customers.
- First purchase date per customer.
- First purchase month.
- Customers acquired per month.

### Project B – Purchase Behaviour

Calculate:

- Total orders per customer.
- Total spending per customer.
- Average order value per customer.
- Days between purchases.
- Number of repeat customers.

### Project C – Cohort Analysis

Calculate:

- Cohort month.
- Cohort size.
- Monthly active customers.
- Cohort index.

### Project D – Retention Analysis

Calculate:

- Month 0 retention.
- Month 1 retention.
- Month 2 retention.
- Month 3 retention.
- Retention percentage.

### Project E – Business Insights

Write at least five observations based on your results.

For example:

- Which cohort has the strongest Month 1 retention?
- Are customers returning after their first purchase?
- Which cohort has the highest repeat-purchase activity?
- Does retention improve or decline across cohorts?
- What additional data would help explain the results?

### Expected Final Deliverable

Your final report should contain:

| Metric | Description |
|---|---|
| Cohort month | Month of first purchase |
| Cohort size | Number of customers in the cohort |
| Month 0 | Customers active in their first month |
| Month 1 | Customers active one month later |
| Month 2 | Customers active two months later |
| Month 3 | Customers active three months later |
| Retention rate | Active customers divided by cohort size |

---

# PART G – SQL CHEAT SHEET

## Quick Reference

| Requirement | SQL |
|---|---|
| Previous row value | `LAG(column) OVER (ORDER BY date)` |
| Next row value | `LEAD(column) OVER (ORDER BY date)` |
| Previous value per customer | `LAG(column) OVER (PARTITION BY customer_id ORDER BY date)` |
| First purchase date | `MIN(order_date)` |
| Number of purchases per customer | `COUNT(*)` with `GROUP BY customer_id` |
| Purchase sequence | `ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date)` |
| Month of first purchase | `DATE_TRUNC('month', first_purchase_date)` |
| Unique active customers | `COUNT(DISTINCT customer_id)` |
| Cohort month difference | `AGE(activity_month, cohort_month)` with `EXTRACT()` |
| Retention rate | `active_customers * 100.0 / cohort_size` |
| Conditional aggregation | `COUNT(CASE WHEN condition THEN 1 END)` |
| Prevent division by zero | `NULLIF(value, 0)` |

---


## What Comes Next?

After completing this lesson, you will have a stronger foundation in analytical SQL.

In upcoming lessons, we can cover:

- Advanced SQL interview challenges.
- Query optimisation and indexing.
- Customer Lifetime Value (CLV).
- Customer segmentation.
- Funnel analysis.
- Advanced business KPIs.
- Practical SQL case studies using real-world datasets.

**Final advice:** Cohort and retention analysis are highly valuable Data Analyst skills. Focus not only on writing the SQL correctly, but also on understanding the business definitions behind every metric.
