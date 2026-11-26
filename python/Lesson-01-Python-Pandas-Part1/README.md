# Lesson 01: Python (Pandas - Part 1)

## 1. Overview

Welcome to the Python part of Lesson 01!

In this lesson, we introduce **Python and Pandas**, two essential tools used by Data Analysts to work with, clean, transform and analyse data.

Python is a general-purpose programming language. Pandas is a Python library that makes working with structured data much easier.

We will focus on:

- Python's role in Data Analysis.
- Pandas Series and DataFrames.
- Importing and exporting datasets.
- Inspecting data.
- Selecting columns and rows.
- Filtering records.
- Sorting data.
- Creating new columns.
- Handling missing values.
- Identifying duplicates.
- Grouping and aggregating data.
- Cleaning text data.
- Working with dates.
- Calculating running totals.
- Exporting analytical results.

**Tools:** Python and Pandas.

---

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Understand the role of Python in Data Analysis.
- Import and use the Pandas library.
- Create a DataFrame from a Python dictionary.
- Read CSV and Excel files.
- Inspect datasets using Pandas.
- Select specific columns and rows.
- Filter records using conditions.
- Sort data.
- Create calculated columns.
- Identify and handle missing values.
- Detect and remove duplicates.
- Group and aggregate data.
- Clean text columns.
- Convert columns into datetime format.
- Extract date components.
- Calculate monthly sales and running totals.
- Export cleaned data to CSV and Excel.

---

## 3. What Is Python?

Python is a programming language used in many areas, including:

- Data Analysis.
- Data Science.
- Automation.
- Machine Learning.
- Web Development.
- Data Engineering.

For Data Analysts, Python is particularly useful when working with datasets that require cleaning, transformation, exploration or automation.

### Why Should a Data Analyst Learn Python?

SQL is excellent for retrieving and analysing data stored in databases.

Python allows us to go further by:

- Automating repetitive tasks.
- Cleaning complex datasets.
- Combining data from different sources.
- Performing exploratory data analysis.
- Creating visualisations.
- Building repeatable analysis workflows.

SQL and Python are complementary tools.

---

## 4. What Is Pandas?

Pandas is a Python library designed for working with structured data.

It provides tools for:

- Reading datasets.
- Cleaning data.
- Filtering rows.
- Selecting columns.
- Aggregating information.
- Manipulating dates.
- Exporting results.

### Installing Pandas

Open your terminal and run:

```bash
pip install pandas
```

If you are using Jupyter Notebook, you can also run:

```python
%pip install pandas
```

### Importing Pandas

```python
import pandas as pd
```

We use `pd` as the conventional alias for Pandas.

---

## 5. Understanding Series and DataFrames

Pandas has two important data structures.

### 5.1 Series

A Series is a one-dimensional structure.

It is similar to a single column in a table.

```python
import pandas as pd

salaries = pd.Series([55000, 42000, 65000, 60000])

print(salaries)
```

### 5.2 DataFrame

A DataFrame is a two-dimensional structure containing rows and columns.

It is similar to a SQL table or an Excel worksheet.

```python
data = {
    "first_name": ["John", "Maria", "George"],
    "salary": [55000, 42000, 65000]
}

df = pd.DataFrame(data)

print(df)
```

**Expected output:**

| index | first_name | salary |
|---:|---|---:|
| 0 | John | 55000 |
| 1 | Maria | 42000 |
| 2 | George | 65000 |

The index identifies the rows in the DataFrame.

---

# PART A – WORKING WITH DATA

## 6. Creating Our Employees DataFrame

We will reuse the employee dataset from our SQL lessons.

```python
import pandas as pd

employees = {
    "employee_id": [1, 2, 3, 4, 5, 6, 7, 8],
    "first_name": [
        "John", "Maria", "George", "Anna",
        "Peter", "Elena", "David", "Sofia"
    ],
    "department_id": [10, 20, 10, 30, None, 20, 40, 10],
    "salary": [55000, 42000, 65000, 60000, 38000, 45000, 72000, 50000]
}

df = pd.DataFrame(employees)

print(df)
```

### Expected Dataset

| employee_id | first_name | department_id | salary |
|---:|---|---:|---:|
| 1 | John | 10 | 55000 |
| 2 | Maria | 20 | 42000 |
| 3 | George | 10 | 65000 |
| 4 | Anna | 30 | 60000 |
| 5 | Peter | NaN | 38000 |
| 6 | Elena | 20 | 45000 |
| 7 | David | 40 | 72000 |
| 8 | Sofia | 10 | 50000 |

**Important:** Python uses `None` to represent the missing department ID in our original dictionary. Pandas normally converts this missing numeric value to `NaN`.

---

## 7. Importing and Exporting Data

In real Data Analyst jobs, we usually work with files or database extracts rather than manually created DataFrames.

### Read a CSV File

```python
df = pd.read_csv("employees.csv")
```

### Read an Excel File

```python
df = pd.read_excel("employees.xlsx")
```

### Export to CSV

```python
df.to_csv("employees_cleaned.csv", index=False)
```

### Export to Excel

```python
df.to_excel("employees_cleaned.xlsx", index=False)
```

### What Does index=False Mean?

Pandas normally includes the DataFrame index when exporting.

`index=False` prevents the index from being written as an extra column in the exported file.

---

## 8. Inspecting Data

Before analysing a dataset, we need to understand its structure and quality.

This is one of the first steps in Exploratory Data Analysis (EDA).

### 8.1 head()

Returns the first five rows by default.

```python
df.head()
```

To return the first three rows:

```python
df.head(3)
```

### 8.2 tail()

Returns the last five rows by default.

```python
df.tail()
```

### 8.3 shape

Returns the number of rows and columns.

```python
df.shape
```

Expected output:

```python
(8, 4)
```

This means:

- 8 rows.
- 4 columns.

### 8.4 columns

Returns the column names.

```python
df.columns
```

### 8.5 dtypes

Shows the data type of each column.

```python
df.dtypes
```

### 8.6 info()

Provides an overview of the dataset.

```python
df.info()
```

It includes:

- Column names.
- Non-null counts.
- Data types.
- Memory usage.

### 8.7 describe()

Provides descriptive statistics for numeric columns.

```python
df.describe()
```

Expected salary statistics:

| Statistic | Salary |
|---|---:|
| Count | 8 |
| Mean | 53375 |
| Standard deviation | Approximately 11980.34 |
| Minimum | 38000 |
| 25% | 43500 |
| 50% | 52500 |
| 75% | 61250 |
| Maximum | 72000 |

### 8.8 sample()

Returns random rows.

```python
df.sample(5)
```

The selected rows may change each time you run the code.

---

## 9. Selecting Columns

### Select One Column

```python
df["first_name"]
```

This returns a Series.

### Select Multiple Columns

```python
df[["first_name", "salary"]]
```

This returns a DataFrame.

### Important Difference

One pair of brackets:

```python
df["salary"]
```

Two pairs of brackets:

```python
df[["salary"]]
```

The first returns a Series; the second returns a DataFrame containing one column.

---

## 10. Selecting Rows with loc and iloc

Pandas provides two common ways to select rows.

### 10.1 loc

`.loc` selects rows and columns using labels.

```python
df.loc[0]
```

Select a specific row and column:

```python
df.loc[0, "first_name"]
```

Select multiple columns:

```python
df.loc[:, ["first_name", "salary"]]
```

### 10.2 iloc

`.iloc` selects rows and columns using integer positions.

```python
df.iloc[0]
```

Select the first three rows:

```python
df.iloc[0:3]
```

Select the first row and second column:

```python
df.iloc[0, 1]
```

### Difference Between loc and iloc

| Method | Selection |
|---|---|
| `.loc` | Labels |
| `.iloc` | Integer positions |

---

## 11. Filtering Data

Filtering allows us to return only the records that meet specific conditions.

### Example 1 – Salary Above 50,000

```python
df[df["salary"] > 50000]
```

Expected employees:

- John? No, his salary is 55,000, so yes.
- George – 65,000.
- Anna – 60,000.
- David – 72,000.

### Example 2 – Salary Above 60,000

```python
df[df["salary"] > 60000]
```

Expected employees:

- George.
- David.

### Example 3 – Multiple Conditions with AND

Return employees in department 10 with a salary above 50,000.

```python
df[
    (df["department_id"] == 10) &
    (df["salary"] > 50000)
]
```

### Example 4 – Multiple Conditions with OR

Return employees with a salary below 45,000 or above 65,000.

```python
df[
    (df["salary"] < 45000) |
    (df["salary"] > 65000)
]
```

### Important Operators

| Operator | Meaning |
|---|---|
| `&` | AND |
| `|` | OR |
| `~` | NOT |
| `==` | Equal |
| `!=` | Not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

**Important:** Always put parentheses around individual conditions when combining them with `&` or `|`.

---

## 12. Sorting Data

Use `sort_values()` to sort a DataFrame.

### Ascending Order

```python
df.sort_values(by="salary")
```

### Descending Order

```python
df.sort_values(
    by="salary",
    ascending=False
)
```

### Sorting by Multiple Columns

```python
df.sort_values(
    by=["department_id", "salary"],
    ascending=[True, False]
)
```

This sorts:

1. Departments in ascending order.
2. Salaries within each department in descending order.

By default, `sort_values()` returns a new DataFrame rather than modifying the original.

---

# PART B – DATA TRANSFORMATION AND CLEANING

## 13. Creating New Columns

A Data Analyst often needs to create new columns based on existing information.

### Example: Salary Category

```python
df.loc[df["salary"] > 60000, "salary_category"] = "High"

df.loc[
    (df["salary"] > 45000) &
    (df["salary"] <= 60000),
    "salary_category"
] = "Medium"

df.loc[df["salary"] <= 45000, "salary_category"] = "Low"
```

### Expected Categories

| first_name | salary | salary_category |
|---|---:|---|
| John | 55000 | Medium |
| Maria | 42000 | Low |
| George | 65000 | High |
| Anna | 60000 | Medium |
| Peter | 38000 | Low |
| Elena | 45000 | Low |
| David | 72000 | High |
| Sofia | 50000 | Medium |

### Alternative: numpy.select()

For more complex conditions, you can use `numpy.select()`.

```python
import numpy as np

conditions = [
    df["salary"] > 60000,
    df["salary"] > 45000
]

choices = ["High", "Medium"]

df["salary_category"] = np.select(
    conditions,
    choices,
    default="Low"
)
```

The conditions are evaluated in order. Therefore, the highest salary threshold should be checked first.

---

## 14. Handling Missing Values

Missing values are common in real-world datasets.

Pandas provides several methods to identify and handle them.

### 14.1 isnull()

Identify missing values.

```python
df.isnull()
```

### 14.2 Count Missing Values

```python
df.isnull().sum()
```

Expected result:

| Column | Missing values |
|---|---:|
| employee_id | 0 |
| first_name | 0 |
| department_id | 1 |
| salary | 0 |

### 14.3 dropna()

Remove rows containing missing values.

```python
df.dropna()
```

By default, this removes any row with at least one missing value.

**Important:** This would remove Peter from our dataset because his department ID is missing.

### 14.4 fillna()

Replace missing values.

```python
df["department_id"] = df["department_id"].fillna(0)
```

This replaces missing department IDs with zero.

However, do not replace missing values without understanding what they represent.

Possible strategies include:

- Keeping the missing value.
- Replacing it with a meaningful category.
- Using a suitable statistical value.
- Removing the record if justified.
- Investigating the original data source.

The correct choice depends on the business context.

---

## 15. Identifying and Removing Duplicates

Duplicate records can cause incorrect analysis.

### 15.1 Identify Duplicates

```python
df.duplicated()
```

### 15.2 Count Duplicates

```python
df.duplicated().sum()
```

### 15.3 Remove Duplicates

```python
df.drop_duplicates()
```

### 15.4 Check Duplicates Based on Specific Columns

```python
df.duplicated(subset=["employee_id"])
```

### 15.5 Keep the Latest Record

If a dataset contains multiple records per customer and includes a date:

```python
df = df.sort_values("order_date")

df = df.drop_duplicates(
    subset=["customer_id"],
    keep="last"
)
```

This keeps the last record for each customer after sorting by date.

**Important:** `drop_duplicates()` returns a new DataFrame by default. Assign the result back to a variable if you want to retain the change.

---

## 16. Grouping and Aggregating Data

Pandas `groupby()` is similar to SQL `GROUP BY`.

It allows us to calculate statistics for different groups.

### Example: Average Salary per Department

```python
df.groupby("department_id")["salary"].mean()
```

### Example: Multiple Aggregations

```python
department_summary = df.groupby("department_id").agg(
    average_salary=("salary", "mean"),
    total_salary=("salary", "sum"),
    employee_count=("employee_id", "count")
)

print(department_summary)
```

### Expected Result

| department_id | average_salary | total_salary | employee_count |
|---:|---:|---:|---:|
| 10 | 56666.67 | 170000 | 3 |
| 20 | 43500 | 87000 | 2 |
| 30 | 60000 | 60000 | 1 |
| 40 | 72000 | 72000 | 1 |

### Important: Missing Department IDs

By default, `groupby()` excludes missing group keys.

To include the missing department group:

```python
df.groupby(
    "department_id",
    dropna=False
).agg(
    average_salary=("salary", "mean"),
    employee_count=("employee_id", "count")
)
```

This includes Peter's missing department group.

### Common Aggregation Functions

| Function | Purpose |
|---|---|
| `count` | Count non-null values |
| `sum` | Calculate total |
| `mean` | Calculate average |
| `min` | Find minimum |
| `max` | Find maximum |
| `median` | Calculate median |
| `nunique` | Count distinct values |

---

# PART C – TEXT AND DATE ANALYSIS

## 17. Working with Text Data

Pandas provides the `.str` accessor for string operations.

### Convert Text to Uppercase

```python
df["first_name"].str.upper()
```

### Convert Text to Lowercase

```python
df["first_name"].str.lower()
```

### Remove Unnecessary Spaces

```python
df["first_name"].str.strip()
```

### Combine First and Last Names

```python
customers["full_name"] = (
    customers["first_name"] + " " +
    customers["last_name"]
)
```

### Extract Email Domains

```python
customers["email_domain"] = (
    customers["email"].str.split("@").str[1]
)
```

### Example

| email | email_domain |
|---|---|
| alice@gmail.com | gmail.com |
| bob@yahoo.com | yahoo.com |
| charlie@gmail.com | gmail.com |
| diana@hotmail.com | hotmail.com |

---

## 18. Working with Dates

Date analysis is an important part of business reporting.

Pandas provides `pd.to_datetime()` to convert columns into datetime format.

### 18.1 Create the Orders DataFrame

```python
orders = {
    "order_id": [101, 102, 103, 104, 105, 106],
    "customer_name": [
        "Alice", "Bob", "Alice",
        "Charlie", "Diana", "Bob"
    ],
    "order_date": [
        "2025-01-15",
        "2025-02-10",
        "2025-02-20",
        "2025-03-05",
        "2025-03-15",
        "2025-04-01"
    ],
    "order_amount": [250, 400, 150, 600, 300, 500]
}

orders_df = pd.DataFrame(orders)
```

### 18.2 Convert to Datetime

```python
orders_df["order_date"] = pd.to_datetime(
    orders_df["order_date"]
)
```

### 18.3 Extract Year

```python
orders_df["year"] = orders_df["order_date"].dt.year
```

### 18.4 Extract Month

```python
orders_df["month"] = orders_df["order_date"].dt.month
```

### 18.5 Extract Quarter

```python
orders_df["quarter"] = orders_df["order_date"].dt.quarter
```

### Expected Date Components

| order_id | order_date | year | month | quarter |
|---:|---|---:|---:|---:|
| 101 | 2025-01-15 | 2025 | 1 | 1 |
| 102 | 2025-02-10 | 2025 | 2 | 1 |
| 103 | 2025-02-20 | 2025 | 2 | 1 |
| 104 | 2025-03-05 | 2025 | 3 | 1 |
| 105 | 2025-03-15 | 2025 | 3 | 1 |
| 106 | 2025-04-01 | 2025 | 4 | 2 |

### 18.6 Filter February 2025 Orders

```python
february_orders = orders_df[
    (orders_df["order_date"] >= "2025-02-01") &
    (orders_df["order_date"] < "2025-03-01")
]
```

Expected orders:

- Order 102 – 400.
- Order 103 – 150.

### 18.7 Calculate Days Since an Order

```python
today = pd.Timestamp.today().normalize()

orders_df["days_since_order"] = (
    today - orders_df["order_date"]
).dt.days
```

The exact result changes depending on the date when the code is executed.

---

## 19. Monthly Sales Analysis

Monthly reporting is a common Data Analyst task.

We can group orders by month and calculate total sales and order counts.

```python
orders_df["sales_month"] = (
    orders_df["order_date"].dt.to_period("M")
)

monthly_sales = orders_df.groupby("sales_month").agg(
    total_orders=("order_id", "count"),
    total_sales=("order_amount", "sum"),
    average_order_value=("order_amount", "mean")
)

print(monthly_sales)
```

### Expected Result

| sales_month | total_orders | total_sales | average_order_value |
|---|---:|---:|---:|
| 2025-01 | 1 | 250 | 250 |
| 2025-02 | 2 | 550 | 275 |
| 2025-03 | 2 | 900 | 450 |
| 2025-04 | 1 | 500 | 500 |

---

## 20. Calculating Running Totals in Pandas

We can use `cumsum()` to calculate cumulative sums.

### Company Running Total

```python
orders_df = orders_df.sort_values(
    by=["order_date", "order_id"]
)

orders_df["running_total"] = (
    orders_df["order_amount"].cumsum()
)
```

### Expected Result

| order_id | order_amount | running_total |
|---:|---:|---:|
| 101 | 250 | 250 |
| 102 | 400 | 650 |
| 103 | 150 | 800 |
| 104 | 600 | 1400 |
| 105 | 300 | 1700 |
| 106 | 500 | 2200 |

### Running Total per Customer

```python
orders_df = orders_df.sort_values(
    by=["customer_name", "order_date", "order_id"]
)

orders_df["customer_running_total"] = (
    orders_df.groupby("customer_name")["order_amount"]
    .cumsum()
)
```

This calculates a separate cumulative total for every customer.

---

## 21. Cleaning Customer Data – Practical Example

Let's create a small customer dataset containing inconsistent text values.

```python
customers = {
    "customer_id": [1, 2, 3, 4],
    "first_name": ["alice", "BOB", " charlie", "Diana"],
    "last_name": ["smith", "johnson", "brown ", "Wilson"],
    "email": [
        "alice@gmail.com",
        "bob@yahoo.com",
        "charlie@gmail.com",
        "diana@hotmail.com"
    ]
}

customers_df = pd.DataFrame(customers)
```

### Step 1 – Remove Extra Spaces

```python
customers_df["first_name"] = (
    customers_df["first_name"].str.strip()
)

customers_df["last_name"] = (
    customers_df["last_name"].str.strip()
)
```

### Step 2 – Standardise Name Capitalisation

```python
customers_df["first_name"] = (
    customers_df["first_name"].str.title()
)

customers_df["last_name"] = (
    customers_df["last_name"].str.title()
)
```

### Step 3 – Standardise Email Addresses

```python
customers_df["email"] = (
    customers_df["email"].str.strip().str.lower()
)
```

### Step 4 – Create Full Name

```python
customers_df["full_name"] = (
    customers_df["first_name"] + " " +
    customers_df["last_name"]
)
```

### Step 5 – Extract Email Domain

```python
customers_df["email_domain"] = (
    customers_df["email"].str.split("@").str[1]
)
```

### Expected Cleaned Data

| customer_id | first_name | last_name | full_name | email | email_domain |
|---:|---|---|---|---|---|
| 1 | Alice | Smith | Alice Smith | alice@gmail.com | gmail.com |
| 2 | Bob | Johnson | Bob Johnson | bob@yahoo.com | yahoo.com |
| 3 | Charlie | Brown | Charlie Brown | charlie@gmail.com | gmail.com |
| 4 | Diana | Wilson | Diana Wilson | diana@hotmail.com | hotmail.com |

---

## 22. SQL vs Pandas – Quick Comparison

| Task | SQL | Pandas |
|---|---|---|
| Select columns | `SELECT first_name` | `df["first_name"]` |
| Filter rows | `WHERE salary > 50000` | `df[df["salary"] > 50000]` |
| Sort | `ORDER BY salary DESC` | `df.sort_values("salary", ascending=False)` |
| Group | `GROUP BY department_id` | `df.groupby("department_id")` |
| Average | `AVG(salary)` | `.mean()` |
| Total | `SUM(salary)` | `.sum()` |
| Count | `COUNT(*)` | `.count()` or `.size()` |
| Missing values | `IS NULL` | `.isnull()` |
| Remove duplicates | `DISTINCT` | `.drop_duplicates()` |
| Running total | `SUM() OVER()` | `.cumsum()` |

---

# PART D – PRACTICE ASSIGNMENT

## 23. Python and Pandas Exercises

Use the `employees`, `orders_df` and `customers_df` datasets created in this lesson.

### Section A – Creating and Inspecting DataFrames

**Exercise 13 – Create Employees DataFrame**

Create the employees DataFrame using the dataset provided in this lesson.

**Exercise 14 – Inspect the Dataset**

Return:

- First five rows.
- Number of rows and columns.
- Column names.
- Data types.
- Descriptive statistics.

**Exercise 15 – Select Columns**

Return only:

- `first_name`
- `salary`

### Section B – Filtering and Sorting

**Exercise 16 – Salary Filter**

Return employees with a salary above 50,000.

**Exercise 17 – Multiple Conditions**

Return employees who:

- Belong to department 10.
- Have a salary above 50,000.

**Exercise 18 – Sorting**

Sort employees by salary from highest to lowest.

### Section C – Data Transformation and Cleaning

**Exercise 19 – Salary Categories**

Create a new column called `salary_category`:

- Above 60,000 → High.
- Above 45,000 and up to 60,000 → Medium.
- 45,000 or below → Low.

**Exercise 20 – Missing Values**

Identify the missing values in each column and calculate the number of missing values.

**Exercise 21 – Department Summary**

Calculate:

- Average salary per department.
- Employee count per department.

Also investigate how Pandas handles the missing department ID.

**Exercise 22 – Duplicates**

Check whether the employees DataFrame contains duplicate rows.

Then check duplicates based on `employee_id`.

### Section D – Orders and Dates

**Exercise 23 – Create Orders DataFrame**

Create the orders DataFrame using the dataset provided in this lesson.

**Exercise 24 – Date Components**

Convert `order_date` to datetime and create:

- Year.
- Month.
- Quarter.

**Exercise 25 – February Orders**

Return all orders placed during February 2025.

**Exercise 26 – Monthly Sales Report**

Calculate the following for each month:

- Total orders.
- Total sales.
- Average order value.

### Section E – Running Totals and Customer Cleaning

**Exercise 27 – Running Total**

Calculate the cumulative sales amount ordered by date.

Then calculate a separate running total for each customer.

**Exercise 28 – Customer Data Cleaning**

Clean the customer dataset by:

- Removing unnecessary spaces.
- Standardising first and last names.
- Converting emails to lowercase.
- Creating a full name.
- Extracting email domains.

---

## 24. Mini Project – Sales and Customer Analysis

Now combine the concepts from the lesson into a small Data Analyst project.

### Project A – Employee Analysis

Create a report that includes:

- Employee name.
- Department ID.
- Salary.
- Salary category.
- Department average salary.
- Difference between individual salary and department average.

Use Pandas filtering, calculated columns and aggregation.

### Project B – Sales Analysis

Create a monthly sales report with:

- Month.
- Total orders.
- Total sales.
- Average order value.
- Cumulative sales.

Use Pandas datetime operations, `groupby()` and `cumsum()`.

### Project C – Customer Data Cleaning

Clean the customer dataset and create:

- Standardised first name.
- Standardised last name.
- Full name.
- Standardised email.
- Email domain.

### Final Step – Export Your Reports

Export the cleaned customer dataset:

```python
customers_df.to_csv(
    "cleaned_customers.csv",
    index=False
)
```

Export the monthly sales report:

```python
monthly_sales.to_csv(
    "monthly_sales_report.csv"
)
```

You can also export the results to Excel:

```python
with pd.ExcelWriter("data_analysis_reports.xlsx") as writer:
    monthly_sales.to_excel(
        writer,
        sheet_name="Monthly Sales"
    )

    customers_df.to_excel(
        writer,
        sheet_name="Cleaned Customers",
        index=False
    )
```

---

## 25. Pandas Cheat Sheet

| Requirement | Pandas |
|---|---|
| Create DataFrame | `pd.DataFrame(data)` |
| Read CSV | `pd.read_csv("file.csv")` |
| Read Excel | `pd.read_excel("file.xlsx")` |
| First rows | `df.head()` |
| Last rows | `df.tail()` |
| Dimensions | `df.shape` |
| Column names | `df.columns` |
| Data types | `df.dtypes` |
| Dataset information | `df.info()` |
| Descriptive statistics | `df.describe()` |
| Select column | `df["salary"]` |
| Select multiple columns | `df[["first_name", "salary"]]` |
| Filter rows | `df[df["salary"] > 50000]` |
| Sort descending | `df.sort_values("salary", ascending=False)` |
| Missing values | `df.isnull().sum()` |
| Remove missing rows | `df.dropna()` |
| Fill missing values | `df.fillna(value)` |
| Remove duplicates | `df.drop_duplicates()` |
| Group and aggregate | `df.groupby("department_id").agg(...)` |
| Uppercase | `df["name"].str.upper()` |
| Lowercase | `df["name"].str.lower()` |
| Remove spaces | `df["name"].str.strip()` |
| Convert to datetime | `pd.to_datetime(df["date"])` |
| Extract year | `df["date"].dt.year` |
| Extract month | `df["date"].dt.month` |
| Running total | `df["amount"].cumsum()` |
| Export CSV | `df.to_csv("file.csv", index=False)` |
| Export Excel | `df.to_excel("file.xlsx", index=False)` |

---

## 26. Common Mistakes to Avoid

| Mistake | Explanation | Correct Approach |
|---|---|---|
| Using `=` instead of `==` in a filter | `=` is assignment, not comparison | Use `==` |
| Forgetting parentheses in combined filters | Can cause operator precedence errors | Parenthesise each condition |
| Using `and` instead of `&` | Pandas Series require element-wise operators | Use `&` or `|` |
| Selecting multiple columns with one string | Returns a single column rather than a multi-column DataFrame | Use a list of column names |
| Forgetting to convert dates | String dates do not provide full datetime functionality | Use `pd.to_datetime()` |
| Removing all missing rows without checking | Can discard valuable records | Investigate missing values first |
| Forgetting to assign a cleaning operation | Many Pandas methods return a new DataFrame | Reassign the result |
| Ignoring missing group keys | `groupby()` excludes them by default | Use `dropna=False` when appropriate |
| Sorting after calculating a running total | The cumulative result may not follow the intended order | Sort before applying `cumsum()` |

---

## 27. Lesson Completion Checklist

Before moving to the next lesson, make sure you can:

- [ ] Explain what Python and Pandas are used for.
- [ ] Create a Pandas DataFrame.
- [ ] Import CSV and Excel files.
- [ ] Inspect a dataset.
- [ ] Select rows and columns.
- [ ] Filter data using multiple conditions.
- [ ] Sort data.
- [ ] Create calculated columns.
- [ ] Identify and handle missing values.
- [ ] Find and remove duplicates.
- [ ] Group and aggregate data.
- [ ] Clean text columns.
- [ ] Convert and analyse dates.
- [ ] Calculate monthly sales.
- [ ] Calculate running totals.
- [ ] Export reports to CSV and Excel.

---

## 28. What Comes Next?

In the next Python lessons, we will develop more advanced skills:

- Python variables and data types in more depth.
- Conditional statements (`if`, `elif`, `else`).
- Loops (`for`, `while`).
- Functions.
- Pandas `merge()` and `concat()`.
- Exploratory Data Analysis (EDA).
- Data visualisation using Matplotlib.
- Working with real-world datasets.
- Practical Data Analyst projects.

**Remember:** The goal is not simply to learn Python syntax. The goal is to use Python and Pandas to answer business questions, clean data and communicate useful insights.
