# Python Lesson 03 — Pandas: Filtering, GroupBy & Merge

## Overview

Welcome to Python Lesson 03!

In this lesson, we focus on three essential Pandas skills used every day by Data Professions:

1. **Filtering** — selecting rows that meet specific conditions.
2. **GroupBy and Aggregation** — summarizing data and calculating business metrics.
3. **Merge** — combining datasets using common columns, just like SQL JOINs.

These skills are fundamental for cleaning, exploring, analyzing, and reporting data.

**Level:** Beginner to Intermediate  
**Library:** Pandas  
**Language:** Python  
**Exercises:** 24  
**Mini Project:** Employee and Sales Analytics

---

## Learning Objectives

By the end of this lesson, you should be able to:

- Create and understand Pandas DataFrames.
- Inspect datasets and understand their structure.
- Filter rows using one or multiple conditions.
- Use `isin()`, `between()`, `isna()` and string methods.
- Group data using `groupby()`.
- Calculate sums, averages, counts, minimums and maximums.
- Use multiple aggregations with `agg()`.
- Combine datasets using `pd.merge()`.
- Understand INNER, LEFT, RIGHT and OUTER joins in Pandas.
- Validate merge relationships and identify unmatched records.
- Create useful reports for Data Analysis.

---

# 1. Introduction to Pandas

## What is Pandas?

Pandas is a Python library used for data manipulation and analysis.

It helps Data Analysts work with structured data, such as:

- CSV files
- Excel spreadsheets
- Database query results
- Tables containing customer, sales or employee information

Install Pandas:

```bash
pip install pandas
```

Import the library:

```python
import pandas as pd
```

The abbreviation `pd` is the standard alias used by most Python developers.

## What is a DataFrame?

A DataFrame is a two-dimensional table containing rows and columns.

It is similar to:

- A table in SQL
- A spreadsheet in Excel
- A database table

Example:

```python
import pandas as pd

data = {
    "name": ["John", "Maria", "George"],
    "age": [28, 32, 25],
    "salary": [55000, 42000, 65000]
}

df = pd.DataFrame(data)

print(df)
```

Expected output:

```text
     name  age  salary
0    John   28   55000
1   Maria   32   42000
2  George   25   65000
```

Each row has an index. By default, Pandas starts indexing from zero.

## What is a Series?

A Series is a one-dimensional labeled array.

Selecting one column from a DataFrame normally returns a Series:

```python
df["salary"]
```

Selecting multiple columns returns a DataFrame:

```python
df[["name", "salary"]]
```

---

# 2. Our Practice Datasets

We will use three datasets throughout the lesson.

## 2.1 Employees

```python
employees = pd.DataFrame({
    "employee_id": [1, 2, 3, 4, 5, 6, 7, 8],
    "name": [
        "John", "Maria", "George", "Anna",
        "Peter", "Elena", "David", "Sofia"
    ],
    "department_id": [10, 20, 10, 30, None, 20, 40, 10],
    "salary": [55000, 42000, 65000, 60000, 38000, 45000, 72000, 50000],
    "city": [
        "Athens", "Thessaloniki", "Athens", "Patras",
        "Athens", "Heraklion", "Athens", "Heraklion"
    ]
})

print(employees)
```

| employee_id | name | department_id | salary | city |
|---:|---|---:|---:|---|
| 1 | John | 10 | 55000 | Athens |
| 2 | Maria | 20 | 42000 | Thessaloniki |
| 3 | George | 10 | 65000 | Athens |
| 4 | Anna | 30 | 60000 | Patras |
| 5 | Peter | NaN | 38000 | Athens |
| 6 | Elena | 20 | 45000 | Heraklion |
| 7 | David | 40 | 72000 | Athens |
| 8 | Sofia | 10 | 50000 | Heraklion |

## 2.2 Departments

```python
departments = pd.DataFrame({
    "department_id": [10, 20, 30, 40, 50],
    "department_name": [
        "IT", "HR", "Finance", "Marketing", "Sales"
    ],
    "location": [
        "Athens", "Thessaloniki", "Patras",
        "Athens", "Heraklion"
    ]
})

print(departments)
```

| department_id | department_name | location |
|---:|---|---|
| 10 | IT | Athens |
| 20 | HR | Thessaloniki |
| 30 | Finance | Patras |
| 40 | Marketing | Athens |
| 50 | Sales | Heraklion |

## 2.3 Orders

```python
orders = pd.DataFrame({
    "order_id": [101, 102, 103, 104, 105, 106, 107, 108],
    "customer": [
        "Alice", "Bob", "Alice", "Charlie",
        "Bob", "Diana", "Alice", "Charlie"
    ],
    "region": [
        "North", "South", "North", "East",
        "South", "West", "North", "East"
    ],
    "order_amount": [250, 400, 150, 600, 500, 300, 350, 200]
})

print(orders)
```

| order_id | customer | region | order_amount |
|---:|---|---|---:|
| 101 | Alice | North | 250 |
| 102 | Bob | South | 400 |
| 103 | Alice | North | 150 |
| 104 | Charlie | East | 600 |
| 105 | Bob | South | 500 |
| 106 | Diana | West | 300 |
| 107 | Alice | North | 350 |
| 108 | Charlie | East | 200 |

---

# 3. Inspecting DataFrames

Before analyzing a dataset, you should understand its structure.

This is one of the first things a Data Analyst does when receiving a new dataset.

## View the first rows

```python
employees.head()
```

By default, it returns the first five rows.

```python
employees.head(3)
```

Returns the first three rows.

## View the last rows

```python
employees.tail()
```

## Check the dimensions

```python
employees.shape
```

Expected output:

```text
(8, 5)
```

This means:

- 8 rows
- 5 columns

## View column names

```python
employees.columns
```

## Check data types

```python
employees.dtypes
```

## Get information about the dataset

```python
employees.info()
```

This displays:

- Column names
- Non-null values
- Data types
- Memory usage

## Statistical summary

```python
employees.describe()
```

For numerical columns, this provides:

- Count
- Mean
- Standard deviation
- Minimum
- 25th percentile
- Median
- 75th percentile
- Maximum

**Analyst tip:** Start with `head()`, `shape`, `info()` and `describe()` before writing complicated analysis.

---

# 4. Filtering DataFrames

Filtering means selecting only the rows that satisfy a condition.

In SQL, you use `WHERE`.

In Pandas, you use Boolean conditions inside square brackets.

## 4.1 Basic filtering

Find employees with a salary greater than 50,000:

```python
employees[employees["salary"] > 50000]
```

Expected employees:

| name | salary |
|---|---:|
| John | 55000 |
| George | 65000 |
| Anna | 60000 |
| David | 72000 |

Find employees with a salary below 45,000:

```python
employees[employees["salary"] < 45000]
```

Expected employees:

- Maria
- Peter

## 4.2 Comparison operators

| Operator | Meaning |
|---|---|
| `==` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

Example:

```python
employees[employees["department_id"] == 10]
```

Returns employees in department 10.

## 4.3 Multiple conditions — AND

In Pandas, use `&` instead of SQL's `AND`.

Find employees in department 10 with a salary above 50,000:

```python
employees[
    (employees["department_id"] == 10) &
    (employees["salary"] > 50000)
]
```

Expected result:

- John
- George

**Important:** Put parentheses around each condition.

Incorrect:

```python
employees[
    employees["department_id"] == 10 &
    employees["salary"] > 50000
]
```

Correct:

```python
employees[
    (employees["department_id"] == 10) &
    (employees["salary"] > 50000)
]
```

## 4.4 Multiple conditions — OR

Use `|` instead of SQL's `OR`.

Find employees who work in department 10 or department 20:

```python
employees[
    (employees["department_id"] == 10) |
    (employees["department_id"] == 20)
]
```

## 4.5 NOT condition

Use `~` to reverse a Boolean condition.

Find employees who are not in department 10:

```python
employees[~(employees["department_id"] == 10)]
```

Note: rows with missing department IDs are also included because their comparison is not equal to `True`.

If you want to exclude missing department IDs explicitly:

```python
employees[
    employees["department_id"].notna() &
    (employees["department_id"] != 10)
]
```

## 4.6 Using isin()

`isin()` checks whether values belong to a list.

Find employees in departments 10, 20 or 30:

```python
employees[
    employees["department_id"].isin([10, 20, 30])
]
```

This is similar to SQL:

```sql
WHERE department_id IN (10, 20, 30)
```

To exclude those departments:

```python
employees[
    ~employees["department_id"].isin([10, 20, 30])
]
```

## 4.7 Using between()

Find employees whose salary is between 40,000 and 60,000:

```python
employees[
    employees["salary"].between(40000, 60000)
]
```

By default, both boundaries are included.

Expected employees:

- John
- Maria
- Anna
- Elena
- Sofia

You can exclude the boundaries:

```python
employees[
    employees["salary"].between(
        40000, 60000, inclusive="neither"
    )
]
```

## 4.8 Filtering missing values

Find employees without a department:

```python
employees[employees["department_id"].isna()]
```

Expected result: Peter.

Find employees with a department:

```python
employees[employees["department_id"].notna()]
```

Important:

Do not use:

```python
employees[employees["department_id"] == None]
```

Use `.isna()` or `.notna()` instead.

## 4.9 Filtering text columns

Find employees whose names start with `A`:

```python
employees[
    employees["name"].str.startswith("A")
]
```

Find employees whose names contain `a`:

```python
employees[
    employees["name"].str.contains("a", case=False)
]
```

Find employees whose names end with `a`:

```python
employees[
    employees["name"].str.endswith("a")
]
```

These methods are useful for searching and cleaning text data.

## 4.10 Selecting columns with loc and iloc

Use `.loc` for label-based selection.

```python
employees.loc[
    employees["salary"] > 50000,
    ["name", "salary"]
]
```

This filters rows and selects only the requested columns.

Use `.iloc` for integer-position-based selection.

```python
employees.iloc[0:3, 0:2]
```

This selects:

- Rows at positions 0, 1 and 2
- Columns at positions 0 and 1

Remember:

- `.loc` uses labels.
- `.iloc` uses integer positions.

---

# 5. GroupBy and Aggregation

## What is GroupBy?

GroupBy allows us to divide data into groups and calculate statistics for each group.

It is similar to SQL:

```sql
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id;
```

In Pandas:

```python
employees.groupby("department_id")["salary"].mean()
```

## 5.1 Group by one column

Calculate average salary per department:

```python
employees.groupby("department_id")["salary"].mean()
```

Expected values:

| department_id | average salary |
|---:|---:|
| 10 | 56666.67 |
| 20 | 43500.00 |
| 30 | 60000.00 |
| 40 | 72000.00 |

By default, Pandas excludes missing group keys. To include the missing department group:

```python
employees.groupby(
    "department_id",
    dropna=False
)["salary"].mean()
```

The missing department group has an average salary of 38,000.

## 5.2 Common aggregation functions

| Function | Purpose |
|---|---|
| `.sum()` | Calculates the sum |
| `.mean()` | Calculates the average |
| `.count()` | Counts non-missing values |
| `.size()` | Counts rows in each group |
| `.min()` | Finds the minimum |
| `.max()` | Finds the maximum |
| `.median()` | Calculates the median |
| `.nunique()` | Counts distinct values |

Examples:

```python
employees.groupby("department_id")["salary"].sum()
```

```python
employees.groupby("department_id")["salary"].max()
```

```python
employees.groupby("department_id")["employee_id"].count()
```

### count() vs size()

This distinction is important:

- `.count()` counts non-null values in the selected column.
- `.size()` counts rows in each group, including rows where other columns contain missing values.

## 5.3 Group by multiple columns

Calculate average salary by department and city:

```python
employees.groupby(
    ["department_id", "city"]
)["salary"].mean()
```

This creates groups for each unique combination of department and city.

## 5.4 Multiple aggregations with agg()

Suppose we want:

- Average salary
- Total salary
- Minimum salary
- Maximum salary
- Number of employees

We can calculate everything in one operation:

```python
department_summary = employees.groupby(
    "department_id"
).agg(
    average_salary=("salary", "mean"),
    total_salary=("salary", "sum"),
    minimum_salary=("salary", "min"),
    maximum_salary=("salary", "max"),
    employee_count=("employee_id", "count")
)

print(department_summary)
```

This is called **named aggregation**.

It makes reports easier to read because you choose the output column names.

To include employees without a department:

```python
department_summary = employees.groupby(
    "department_id",
    dropna=False
).agg(
    average_salary=("salary", "mean"),
    total_salary=("salary", "sum"),
    employee_count=("employee_id", "count")
)
```

## 5.5 Reset the index

After GroupBy, the grouped column often becomes the index.

Use:

```python
department_summary = department_summary.reset_index()
```

Now `department_id` becomes a regular column.

## 5.6 Sorting aggregated results

Find departments ordered by average salary, highest first:

```python
department_summary.sort_values(
    by="average_salary",
    ascending=False
)
```

## 5.7 Filtering aggregated results

Find departments with an average salary above 50,000:

```python
department_summary[
    department_summary["average_salary"] > 50000
]
```

This is similar to SQL `HAVING`.

Example in SQL:

```sql
SELECT
    department_id,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

---

# 6. Merge — Combining DataFrames

## What is Merge?

A merge combines two DataFrames using one or more common columns.

This is similar to SQL JOIN.

For example, our employees dataset contains `department_id`, but it does not contain the department name.

The departments dataset contains both `department_id` and `department_name`.

We can combine them.

## 6.1 Basic merge — INNER JOIN

```python
employee_departments = pd.merge(
    employees,
    departments,
    on="department_id",
    how="inner"
)

print(employee_departments)
```

This returns only rows where a matching department exists in both DataFrames.

Equivalent SQL:

```sql
SELECT *
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id;
```

Expected behavior:

- Employees with valid matching departments are included.
- Peter is excluded because his department ID is missing.
- Sales is not included because no employee belongs to department 50.

## 6.2 LEFT JOIN

A LEFT JOIN keeps every row from the left DataFrame.

```python
employee_departments = pd.merge(
    employees,
    departments,
    on="department_id",
    how="left"
)
```

This keeps all employees.

Peter remains in the result, but his department information is missing.

Equivalent SQL:

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id;
```

## 6.3 RIGHT JOIN

A RIGHT JOIN keeps every row from the right DataFrame.

```python
department_employees = pd.merge(
    employees,
    departments,
    on="department_id",
    how="right"
)
```

This keeps all departments, including Sales, even though Sales has no employees.

Equivalent SQL:

```sql
SELECT *
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.department_id;
```

## 6.4 OUTER JOIN

An OUTER JOIN keeps all rows from both DataFrames.

```python
all_records = pd.merge(
    employees,
    departments,
    on="department_id",
    how="outer"
)
```

This includes:

- Matching employee and department records
- Employees without a matching department
- Departments without employees

Equivalent SQL:

```sql
SELECT *
FROM employees e
FULL OUTER JOIN departments d
    ON e.department_id = d.department_id;
```

## 6.5 Merge using different column names

Sometimes the key columns have different names.

Example:

```python
pd.merge(
    employees,
    departments,
    left_on="department_id",
    right_on="department_id",
    how="left"
)
```

If the names were different, you could use:

```python
pd.merge(
    employees,
    departments,
    left_on="department_code",
    right_on="department_id",
    how="left"
)
```

## 6.6 Merge using multiple columns

You can merge using more than one key:

```python
pd.merge(
    df1,
    df2,
    on=["customer_id", "order_date"],
    how="inner"
)
```

Both columns must match for a row to match.

## 6.7 Handling duplicate column names

If both DataFrames contain columns with the same name, Pandas adds suffixes.

Example:

```python
pd.merge(
    df1,
    df2,
    on="customer_id",
    how="inner",
    suffixes=("_left", "_right")
)
```

This helps distinguish columns from each DataFrame.

## 6.8 Validate merge relationships

A very useful feature is `validate`.

For example:

```python
pd.merge(
    employees,
    departments,
    on="department_id",
    how="left",
    validate="many_to_one"
)
```

This checks that:

- Many employees may belong to one department.
- Each department ID appears at most once in the departments DataFrame.

Other validation options include:

| Option | Meaning |
|---|---|
| `one_to_one` | Each key appears once in both DataFrames |
| `one_to_many` | Each left key appears once; right keys may repeat |
| `many_to_one` | Left keys may repeat; each right key appears once |
| `many_to_many` | Repeated keys are allowed on both sides |

Validation can help detect unexpected duplicate keys and prevent incorrect reporting.

## 6.9 Identify unmatched records with indicator

Use `indicator=True` to understand where each merged row came from.

```python
merge_check = pd.merge(
    employees,
    departments,
    on="department_id",
    how="outer",
    indicator=True
)

print(merge_check)
```

Pandas adds a column called `_merge`.

Possible values:

| Value | Meaning |
|---|---|
| `left_only` | Row exists only in the left DataFrame |
| `right_only` | Row exists only in the right DataFrame |
| `both` | Matching row exists in both DataFrames |

Find departments without employees:

```python
merge_check[
    merge_check["_merge"] == "right_only"
]
```

Find employees without a matching department:

```python
merge_check[
    merge_check["_merge"] == "left_only"
]
```

**Important:** Pandas merge behavior around missing keys can differ from SQL NULL join behavior. Inspect missing keys before merging, and do not assume that missing values behave exactly like SQL NULLs.

---

# 7. Practical Data Analyst Workflow

Now let's combine filtering, GroupBy and Merge into one realistic analysis.

## Step 1: Merge employees with departments

```python
employee_report = pd.merge(
    employees,
    departments,
    on="department_id",
    how="left"
)
```

## Step 2: Calculate average salary per department

```python
department_averages = employees.groupby(
    "department_id"
).agg(
    department_average_salary=("salary", "mean")
).reset_index()
```

## Step 3: Merge department averages back into employee data

```python
employee_report = pd.merge(
    employee_report,
    department_averages,
    on="department_id",
    how="left"
)
```

## Step 4: Compare each employee's salary with the department average

```python
employee_report["above_department_average"] = (
    employee_report["salary"] >
    employee_report["department_average_salary"]
)
```

## Step 5: Filter employees earning above their department average

```python
above_average_employees = employee_report[
    employee_report["above_department_average"]
]

print(above_average_employees)
```

Expected employees:

- George — IT
- Maria — HR

Peter has no department, so his department average is missing and the comparison does not evaluate to `True`.

For a production report, it is often better to make missing department information explicit:

```python
employee_report["salary_comparison"] = "No department average"

employee_report.loc[
    employee_report["salary"] >
    employee_report["department_average_salary"],
    "salary_comparison"
] = "Above average"

employee_report.loc[
    employee_report["salary"] <
    employee_report["department_average_salary"],
    "salary_comparison"
] = "Below average"

employee_report.loc[
    employee_report["salary"] ==
    employee_report["department_average_salary"],
    "salary_comparison"
] = "Equal to average"
```

This avoids treating missing information as an ordinary below-average result.

---

# 8. Common Mistakes

## Mistake 1: Using `and` instead of `&`

Incorrect:

```python
employees[
    (employees["salary"] > 50000) and
    (employees["department_id"] == 10)
]
```

Correct:

```python
employees[
    (employees["salary"] > 50000) &
    (employees["department_id"] == 10)
]
```

Use:

- `&` for AND
- `|` for OR
- `~` for NOT

## Mistake 2: Forgetting parentheses

Incorrect:

```python
employees[
    employees["salary"] > 50000 &
    employees["department_id"] == 10
]
```

Correct:

```python
employees[
    (employees["salary"] > 50000) &
    (employees["department_id"] == 10)
]
```

## Mistake 3: Using `== None` for missing values

Incorrect:

```python
employees[employees["department_id"] == None]
```

Correct:

```python
employees[employees["department_id"].isna()]
```

## Mistake 4: Forgetting reset_index()

After GroupBy, a column may become the index.

Use:

```python
summary = employees.groupby(
    "department_id"
)["salary"].mean().reset_index()
```

## Mistake 5: Using the wrong merge type

A LEFT JOIN and INNER JOIN do not return the same rows.

Before merging, ask yourself:

- Do I want to keep all rows from the left DataFrame?
- Do I only want matching records?
- Do I need to keep unmatched records from both datasets?

## Mistake 6: Unexpected duplicate rows after merging

If a key appears multiple times in both DataFrames, the merge can create multiple combinations.

For example, if one customer has three rows in the first DataFrame and four rows in the second, a many-to-many merge can create twelve combinations for that key.

Always inspect key uniqueness:

```python
employees["employee_id"].is_unique
```

```python
departments["department_id"].is_unique
```

---

# 9. SQL vs Pandas Cheat Sheet

| SQL | Pandas |
|---|---|
| `SELECT *` | `df` |
| `SELECT name` | `df["name"]` |
| `WHERE salary > 50000` | `df[df["salary"] > 50000]` |
| `AND` | `&` |
| `OR` | `\|` |
| `NOT` | `~` |
| `IN (...)` | `.isin([...])` |
| `BETWEEN` | `.between()` |
| `IS NULL` | `.isna()` |
| `IS NOT NULL` | `.notna()` |
| `GROUP BY` | `.groupby()` |
| `SUM()` | `.sum()` |
| `AVG()` | `.mean()` |
| `COUNT()` | `.count()` |
| `HAVING` | Filter the grouped result |
| `ORDER BY` | `.sort_values()` |
| `INNER JOIN` | `pd.merge(how="inner")` |
| `LEFT JOIN` | `pd.merge(how="left")` |
| `RIGHT JOIN` | `pd.merge(how="right")` |
| `FULL OUTER JOIN` | `pd.merge(how="outer")` |

---

# 10. Practice Exercises

Complete these exercises without looking at the solutions first.

Use the three datasets from this README.

## Part A — Filtering

### Exercise 1
Select all employees with a salary greater than 50,000.

### Exercise 2
Select employees who work in department 10.

### Exercise 3
Find employees who work in department 10 and earn more than 50,000.

### Exercise 4
Find employees who work in department 10 or department 20.

### Exercise 5
Find employees whose salary is between 40,000 and 60,000, including both boundaries.

### Exercise 6
Find employees who do not have a department assigned.

### Exercise 7
Find employees whose names start with the letter `A`.

### Exercise 8
Select only the `name` and `salary` columns for employees earning more than 45,000.

## Part B — GroupBy and Aggregation

### Exercise 9
Calculate the average salary per department.

### Exercise 10
Calculate the total salary per department.

### Exercise 11
Count the employees in each department.

### Exercise 12
Find the maximum salary in each department.

### Exercise 13
Calculate the following for every department:

- Average salary
- Total salary
- Minimum salary
- Maximum salary
- Employee count

Use `.agg()`.

### Exercise 14
Find departments with an average salary greater than 50,000.

### Exercise 15
Calculate total sales per customer using the orders dataset.

### Exercise 16
Calculate total sales per region and sort the results from highest to lowest.

## Part C — Merge

### Exercise 17
Perform an INNER JOIN between employees and departments.

### Exercise 18
Perform a LEFT JOIN between employees and departments. Keep every employee.

### Exercise 19
Perform a RIGHT JOIN between employees and departments. Keep every department.

### Exercise 20
Perform an OUTER JOIN between employees and departments.

### Exercise 21
Use `indicator=True` to identify departments without employees.

### Exercise 22
Use `validate="many_to_one"` when merging employees and departments.

### Exercise 23
Merge employees with departments and display only:

- Employee name
- Salary
- Department name
- Department location

### Exercise 24
Create a department report containing:

- Department name
- Number of employees
- Average salary
- Total salary

Include departments with zero employees.

---

# 11. Mini Project — Employee and Sales Analytics

Now apply everything you have learned.

You are working as a Junior Data Analyst. Your manager gives you employee, department and order datasets.

Your task is to create two reports.

## Project A — Employee Analytics

Using `employees` and `departments`:

1. Merge employees with departments.
2. Keep all employees.
3. Calculate average salary per department.
4. Add the department average to each employee.
5. Identify employees earning above their department average.
6. Create a department summary containing:
   - Department name
   - Employee count
   - Average salary
   - Total salary
7. Sort departments by average salary, highest first.

## Project B — Sales Analytics

Using the `orders` DataFrame:

1. Calculate total sales.
2. Calculate average order value.
3. Calculate total sales per customer.
4. Calculate total sales per region.
5. Count orders per customer.
6. Find the customer with the highest total sales.
7. Find regions with total sales greater than 500.
8. Create a final report sorted by total sales.

### Expected results for checking

| Metric | Expected result |
|---|---:|
| Number of orders | 8 |
| Total sales | 2750 |
| Average order value | 343.75 |

Customer sales:

| Customer | Total sales |
|---|---:|
| Alice | 750 |
| Bob | 900 |
| Charlie | 800 |
| Diana | 300 |

Regional sales:

| Region | Total sales |
|---|---:|
| North | 750 |
| South | 900 |
| East | 800 |
| West | 300 |

The customer with the highest total sales is Bob.

---

# 12. Recommended Workflow for Data Analysts

When working with a new dataset, follow this order:

1. Load the data.
2. Inspect the first rows.
3. Check dimensions and column names.
4. Check data types and missing values.
5. Understand which columns identify records.
6. Filter the data to answer specific questions.
7. Group and aggregate to calculate metrics.
8. Merge datasets when additional information is needed.
9. Validate the results.
10. Export the final report.

Example export:

```python
department_summary.to_csv(
    "department_summary.csv",
    index=False
)
```

You can also export to Excel:

```python
department_summary.to_excel(
    "department_summary.xlsx",
    index=False
)
```

---

# Completion Checklist

Before moving to the next lesson, make sure you can:

- [ ] Create a Pandas DataFrame.
- [ ] Inspect a dataset using `head()`, `shape`, `info()` and `describe()`.
- [ ] Filter rows using comparison operators.
- [ ] Combine conditions using `&`, `|` and `~`.
- [ ] Use `isin()` and `between()`.
- [ ] Identify missing values with `isna()` and `notna()`.
- [ ] Group data using `groupby()`.
- [ ] Calculate multiple metrics using `agg()`.
- [ ] Sort aggregated results.
- [ ] Merge two DataFrames using INNER and LEFT joins.
- [ ] Use RIGHT and OUTER joins.
- [ ] Identify unmatched records with `indicator=True`.
- [ ] Validate merge relationships.
- [ ] Complete the Employee and Sales Analytics mini project.

---

# What's Next?

After this lesson, you should be comfortable with the core Pandas operations used in everyday Data Analysis.

Recommended next topics:

- Handling missing data and duplicates in depth
- Pandas string and datetime operations
- Python functions, loops and reusable analysis workflows
- Exploratory Data Analysis (EDA)
- Data visualization with Matplotlib
- Working with real-world datasets
- SQL and Pandas interview exercises

**The goal is not just to learn Pandas syntax. The goal is to use Pandas to answer real business questions and produce reliable reports.**
