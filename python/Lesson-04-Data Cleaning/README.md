# Lesson 04: Python - Data Cleaning with Pandas

## Overview

Welcome to Python Lesson 04!

Real-world datasets are rarely perfect. They may contain missing values, duplicate records, incorrect data types, inconsistent information and unusual observations.

Before performing analysis, a Data Analyst must investigate and address these data-quality issues.

In this lesson, we will learn how to clean datasets using Python and Pandas.

### Topics covered

1. **Missing values (NULLs)** — identify, investigate and handle missing data.
2. **Duplicates** — find and remove duplicate records safely.
3. **Data types** — convert columns into the correct formats.
4. **Outliers** — identify unusual values and decide how to handle them.
5. **Data validation** — verify that the cleaned dataset is reliable.

**Level:** Beginner to Intermediate  
**Language:** Python  
**Library:** Pandas  
**Exercises:** 20  
**Mini Project:** Employee Data Cleaning

---

# Learning Objectives

By the end of this lesson, you should be able to:

- Identify missing values in a DataFrame.
- Calculate missing-value counts and percentages.
- Remove or fill missing values using appropriate methods.
- Detect duplicate records.
- Remove duplicates based on selected columns.
- Understand the difference between exact duplicates and duplicate identifiers.
- Inspect and convert Pandas data types.
- Convert strings into numeric values.
- Convert text columns into datetime.
- Identify outliers using descriptive statistics.
- Apply the Interquartile Range (IQR) method.
- Understand the Z-score method.
- Build a basic data-cleaning workflow.
- Validate and export a cleaned dataset.

---

# 1. Introduction to Data Cleaning

## What is Data Cleaning?

Data cleaning is the process of identifying and correcting problems in a dataset so that it becomes suitable for analysis.

For example, imagine receiving an employee dataset containing:

| employee_id | name | age | salary |
|---:|---|---:|---:|
| 1 | John | 28 | 55000 |
| 2 | Maria | NULL | 42000 |
| 3 | George | 35 | 65000 |
| 3 | George | 35 | 65000 |
| 4 | Anna | 150 | 60000 |

There are several problems:

- Maria has a missing age.
- George appears twice.
- Anna's age is suspicious.
- We need to verify whether all columns have the correct data types.

If we analyze this dataset without checking its quality, our results may be misleading.

## Why is Data Cleaning important?

Poor-quality data can cause:

- Incorrect averages.
- Inflated revenue.
- Incorrect customer or employee counts.
- Incorrect business reports.
- Misleading visualizations.
- Wrong business decisions.

A Data Analyst should always inspect data before analyzing it.

**Important principle:** Never delete or modify data automatically without understanding the problem first.

---

# 2. Our Practice Dataset

We will use an intentionally messy employee dataset throughout the lesson.

## Create the dataset

```python
import pandas as pd
import numpy as np

employees = pd.DataFrame({
    "employee_id": [1, 2, 3, 4, 5, 5, 7, 8, 9, 10],
    "name": [
        "John", "Maria", "George", "Anna", "Peter",
        "Peter", "Elena", "David", "Sofia", "Alex"
    ],
    "age": [
        28, 32, np.nan, 45, 29,
        29, 150, 35, np.nan, 41
    ],
    "salary": [
        55000, 42000, 65000, 60000, 38000,
        38000, 45000, 72000, 50000, 999999
    ],
    "department": [
        "IT", "HR", "IT", "Finance", "Marketing",
        "Marketing", "HR", "Finance", "IT", "HR"
    ],
    "hire_date": [
        "2022-01-15", "2021-06-10", "2020-03-20",
        "2019-11-05", "2023-02-01", "2023-02-01",
        "2022-09-12", "2018-04-25", "2021-12-01",
        "2020-07-18"
    ],
    "performance_score": [
        "4.5", "4.0", "3.8", "4.2", "3.5",
        "3.5", "4.1", "4.8", "4.0", "5.0"
    ]
})

print(employees)
```

## Dataset structure

| Column | Description | Expected data type |
|---|---|---|
| employee_id | Unique employee identifier | Integer |
| name | Employee name | String |
| age | Employee age | Numeric |
| salary | Annual salary | Numeric |
| department | Employee department | String or category |
| hire_date | Employee hire date | Datetime |
| performance_score | Employee performance score | Numeric |

## Problems intentionally included

| Problem | Example |
|---|---|
| Missing values | George and Sofia have missing ages |
| Duplicate records | Peter appears twice |
| Duplicate identifier | Employee ID 5 appears twice |
| Invalid age | Elena is 150 years old |
| Extreme salary | Alex earns 999,999 |
| Incorrect data type | Performance score is stored as text |
| Date stored as text | Hire date is an object/string |

---

# 3. Missing Values (NULLs)

## 3.1 What are missing values?

A missing value means that information is unavailable or was not recorded.

Examples:

- A customer has no recorded email.
- An employee has no recorded age.
- A transaction has no payment date.

Pandas commonly represents missing values using:

- `NaN`
- `None`
- `NaT` for missing datetime values

## 3.2 Identify missing values

Use `isna()`:

```python
employees.isna()
```

This returns a Boolean DataFrame:

- `True` means the value is missing.
- `False` means the value exists.

Count missing values per column:

```python
employees.isna().sum()
```

Expected output:

```text
employee_id          0
name                 0
age                  2
salary               0
department           0
hire_date            0
performance_score    0
```

## 3.3 Calculate missing-value percentages

```python
missing_percentage = (
    employees.isna().mean() * 100
)

print(missing_percentage)
```

Why does this work?

- `isna()` identifies missing values.
- Pandas treats True as 1 and False as 0 when calculating the mean.
- Multiplying by 100 gives the percentage.

## 3.4 Find rows containing missing values

Find all rows with at least one missing value:

```python
employees[employees.isna().any(axis=1)]
```

Find rows where age is missing:

```python
employees[employees["age"].isna()]
```

Find rows where age is available:

```python
employees[employees["age"].notna()]
```

## 3.5 Remove rows with missing values

Use `dropna()`:

```python
cleaned_employees = employees.dropna()
```

This removes every row containing at least one missing value.

Remove rows only when a specific column is missing:

```python
cleaned_employees = employees.dropna(
    subset=["age"]
)
```

Remove rows only when all selected columns are missing:

```python
cleaned_employees = employees.dropna(
    subset=["age", "salary"],
    how="all"
)
```

**Warning:** Removing rows can result in valuable information being lost. Always investigate before using `dropna()`.

## 3.6 Fill missing values

Use `fillna()` when replacing missing values is justified.

### Fill with the mean

```python
average_age = employees["age"].mean()

employees["age"] = employees["age"].fillna(
    average_age
)
```

### Fill with the median

```python
median_age = employees["age"].median()

employees["age"] = employees["age"].fillna(
    median_age
)
```

The median is often more robust than the mean when a distribution contains extreme values.

Example:

```text
30000, 35000, 40000, 45000, 999999
```

The extreme value pulls the mean upwards, while the median is less affected.

### Fill categorical values

```python
employees["department"] = employees["department"].fillna(
    "Unknown"
)
```

This preserves the record without pretending to know its actual department.

## 3.7 Forward fill and backward fill

Forward fill copies the previous available value:

```python
employees["department"] = employees["department"].ffill()
```

Backward fill copies the next available value:

```python
employees["department"] = employees["department"].bfill()
```

These methods can be useful for time-series data, but they are not automatically appropriate for employee records.

### Data Analyst rule

Never fill missing values with zero just because it is convenient.

Zero can represent a real measurement and change your analysis.

---

# 4. Duplicate Records

## 4.1 What are duplicates?

A duplicate is a record that appears more than once when it should only appear once.

Duplicates can cause:

- Incorrect customer counts.
- Inflated revenue.
- Incorrect averages.
- Double-counted transactions.

Our dataset contains Peter twice.

## 4.2 Find duplicates

Use `duplicated()`:

```python
employees.duplicated()
```

Count duplicate rows:

```python
employees.duplicated().sum()
```

Expected result:

```text
1
```

Display duplicate rows:

```python
employees[employees.duplicated()]
```

## 4.3 Find duplicates based on specific columns

Sometimes two records are considered duplicates based on an identifier rather than every column.

For example, if `employee_id` should be unique:

```python
employees[
    employees.duplicated(
        subset=["employee_id"],
        keep=False
    )
]
```

The `keep=False` parameter marks every occurrence of a duplicated value as True.

### The keep parameter

| Value | Behavior |
|---|---|
| `"first"` | Marks later duplicates |
| `"last"` | Marks earlier duplicates |
| `False` | Marks all duplicated occurrences |

## 4.4 Remove duplicates

Remove completely identical rows:

```python
employees = employees.drop_duplicates()
```

Remove duplicates based on employee ID:

```python
employees = employees.drop_duplicates(
    subset=["employee_id"],
    keep="first"
)
```

Keep the last occurrence:

```python
employees = employees.drop_duplicates(
    subset=["employee_id"],
    keep="last"
)
```

**Important:** `keep="first"` is not always correct. If records contain different information, you need a business rule to decide which record to keep.

For example, if you have multiple customer records and want the latest update, sort by update date first:

```python
customers = customers.sort_values(
    "updated_at"
)

customers = customers.drop_duplicates(
    subset=["customer_id"],
    keep="last"
)
```

## 4.5 Exact duplicates vs duplicate identifiers

These are two different situations:

- Exact duplicates: all column values are identical.
- Duplicate identifiers: the same ID appears more than once, but other information may differ.

For example:

| employee_id | name | salary |
|---:|---|---:|
| 5 | Peter | 38000 |
| 5 | Peter | 40000 |

These are not exact duplicates, but they share the same employee ID.

You must investigate which record is correct.

---

# 5. Data Types

## 5.1 Why data types matter

A column can look numeric but actually be stored as text.

For example:

```python
salary = ["55000", "42000", "65000"]
```

These values are strings, not numbers.

This can cause problems when calculating:

- Average salary.
- Total revenue.
- Minimum and maximum values.
- Date differences.

## 5.2 Inspect data types

```python
employees.dtypes
```

Or:

```python
employees.info()
```

Our `performance_score` column is stored as an object because its values are strings.

The `hire_date` column is also stored as an object.

## 5.3 Convert text to numeric

Use `pd.to_numeric()`:

```python
employees["performance_score"] = pd.to_numeric(
    employees["performance_score"]
)
```

If the column contains invalid values:

```python
employees["performance_score"] = pd.to_numeric(
    employees["performance_score"],
    errors="coerce"
)
```

### What does errors="coerce" do?

It converts invalid values into NaN instead of raising an error.

Example:

```python
scores = pd.Series([
    "4.5", "3.8", "unknown", "5.0"
])

scores = pd.to_numeric(
    scores,
    errors="coerce"
)

print(scores)
```

Expected output:

```text
0    4.5
1    3.8
2    NaN
3    5.0
dtype: float64
```

## 5.4 Convert to integer

```python
employees["age"] = employees["age"].astype("Int64")
```

Notice the capital `I` in `"Int64"`.

Pandas nullable `Int64` supports missing values. Ordinary NumPy integer types generally cannot represent NaN.

## 5.5 Convert to datetime

Our `hire_date` is currently stored as text.

Convert it:

```python
employees["hire_date"] = pd.to_datetime(
    employees["hire_date"]
)
```

Extract the year:

```python
employees["hire_year"] = employees["hire_date"].dt.year
```

Extract the month:

```python
employees["hire_month"] = employees["hire_date"].dt.month
```

Extract the day:

```python
employees["hire_day"] = employees["hire_date"].dt.day
```

Calculate how long employees have worked for the company:

```python
today = pd.Timestamp.today().normalize()

employees["days_employed"] = (
    today - employees["hire_date"]
).dt.days
```

For invalid dates:

```python
employees["hire_date"] = pd.to_datetime(
    employees["hire_date"],
    errors="coerce"
)
```

Invalid date strings become `NaT`.

## 5.6 Convert categorical columns

If a column contains a limited number of repeated categories, you can use the category data type:

```python
employees["department"] = employees["department"].astype(
    "category"
)
```

This can reduce memory usage and make the data structure more meaningful.

---

# 6. Outliers

## 6.1 What is an outlier?

An outlier is a value that is unusually far from the rest of the observations.

Examples:

- An employee aged 150.
- A salary of 999,999 when most salaries are between 38,000 and 72,000.
- A transaction worth 10 million when typical transactions are below 1,000.

Outliers can be caused by:

1. Data entry mistakes.
2. Measurement errors.
3. Fraud or unusual activity.
4. Genuine extreme observations.

**Important:** An outlier is not automatically an error. Your job is to investigate it.

## 6.2 Method 1 — Descriptive statistics

Start by understanding the distribution:

```python
employees["salary"].describe()
```

You can also sort values:

```python
employees["salary"].sort_values(
    ascending=False
)
```

This helps identify unusually high or low observations.

## 6.3 Method 2 — IQR (Interquartile Range)

The IQR method is commonly used to detect outliers.

Formula:

\[
IQR = Q3 - Q1
\]

Where:

- Q1 = 25th percentile.
- Q3 = 75th percentile.

Calculate the boundaries:

\[
Lower = Q1 - 1.5 \times IQR
\]

\[
Upper = Q3 + 1.5 \times IQR
\]

Values outside these boundaries are flagged as potential outliers.

### Step 1 — Calculate Q1 and Q3

```python
Q1 = employees["salary"].quantile(0.25)

Q3 = employees["salary"].quantile(0.75)

IQR = Q3 - Q1
```

### Step 2 — Calculate boundaries

```python
lower_bound = Q1 - 1.5 * IQR

upper_bound = Q3 + 1.5 * IQR
```

### Step 3 — Identify outliers

```python
outliers = employees[
    (employees["salary"] < lower_bound) |
    (employees["salary"] > upper_bound)
]

print(outliers)
```

With our dataset, the salary of 999,999 should be flagged.

### Step 4 — Select non-outlier observations

```python
employees_without_outliers = employees[
    (employees["salary"] >= lower_bound) &
    (employees["salary"] <= upper_bound)
]
```

This creates a new DataFrame without those flagged observations.

Do not automatically delete them. First investigate whether the values are errors or genuine observations.

## 6.4 Method 3 — Z-score

Another method uses standard deviations.

Formula:

\[
Z = \frac{x-\mu}{\sigma}
\]

Where:

- \(x\) = individual value.
- \(\mu\) = mean.
- \(\sigma\) = standard deviation.

A common rule is to flag values with an absolute Z-score greater than 3.

```python
mean_salary = employees["salary"].mean()

std_salary = employees["salary"].std()

employees["salary_zscore"] = (
    employees["salary"] - mean_salary
) / std_salary
```

Identify potential outliers:

```python
outliers = employees[
    employees["salary_zscore"].abs() > 3
]
```

## 6.5 IQR vs Z-score

| IQR | Z-score |
|---|---|
| Based on quartiles | Based on mean and standard deviation |
| More robust to extreme values | Sensitive to extreme values |
| Useful for skewed distributions | Most useful for approximately normal distributions |
| Uses 1.5 × IQR as a common rule | Uses 3 standard deviations as a common rule |

Neither method proves that a value is wrong.

---

# 7. Complete Data Cleaning Workflow

Now let's put the concepts together.

We will create a cleaned copy and keep the original dataset unchanged.

```python
cleaned_employees = employees.copy()
```

## Step 1 — Inspect the dataset

```python
print(cleaned_employees.shape)

print(cleaned_employees.info())

print(cleaned_employees.isna().sum())

print(cleaned_employees.duplicated().sum())
```

## Step 2 — Remove exact duplicate rows

```python
cleaned_employees = cleaned_employees.drop_duplicates()
```

## Step 3 — Convert data types

```python
cleaned_employees["performance_score"] = pd.to_numeric(
    cleaned_employees["performance_score"],
    errors="coerce"
)

cleaned_employees["hire_date"] = pd.to_datetime(
    cleaned_employees["hire_date"],
    errors="coerce"
)
```

## Step 4 — Handle missing values

For this example, we will fill missing ages with the median:

```python
median_age = cleaned_employees["age"].median()

cleaned_employees["age"] = cleaned_employees["age"].fillna(
    median_age
)
```

This is only an example policy. In a real project, you would first investigate why age is missing.

## Step 5 — Identify invalid ages

Suppose our business rule is that employee ages must be between 18 and 100.

```python
invalid_ages = cleaned_employees[
    (cleaned_employees["age"] < 18) |
    (cleaned_employees["age"] > 100)
]
```

Investigate the flagged records before deciding what to do.

## Step 6 — Identify salary outliers

```python
Q1 = cleaned_employees["salary"].quantile(0.25)
Q3 = cleaned_employees["salary"].quantile(0.75)

IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

salary_outliers = cleaned_employees[
    (cleaned_employees["salary"] < lower_bound) |
    (cleaned_employees["salary"] > upper_bound)
]
```

## Step 7 — Validate the cleaned dataset

```python
print(cleaned_employees.info())

print(cleaned_employees.isna().sum())

print(cleaned_employees.duplicated().sum())

print(cleaned_employees.describe())
```

The key principle:

**Cleaning is not complete until you validate the result.**

---

# 8. Common Mistakes

## Mistake 1 — Removing all missing rows automatically

Incorrect approach:

```python
employees = employees.dropna()
```

Why is this dangerous?

You may remove many useful records just because one column is missing.

Better approach:

Investigate missing values per column and decide how to handle each one.

## Mistake 2 — Filling every missing value with zero

Incorrect approach:

```python
employees = employees.fillna(0)
```

Why is this dangerous?

A missing salary does not necessarily mean that the employee earns zero.

Better approach:

Use a business rule, a justified statistical replacement, or preserve missing values.

## Mistake 3 — Removing duplicates without checking identifiers

Incorrect approach:

```python
employees = employees.drop_duplicates(
    subset=["employee_id"]
)
```

Why is this dangerous?

Two records with the same ID may contain different information. You need to understand which record is correct.

## Mistake 4 — Converting invalid values without investigating them

Example:

```python
pd.to_numeric(
    employees["salary"],
    errors="coerce"
)
```

This converts invalid values into NaN.

You should inspect the newly created missing values rather than silently accepting them.

## Mistake 5 — Automatically deleting outliers

An unusually high salary could be genuine.

Investigate the value, source and business context before changing it.

## Mistake 6 — Modifying the original dataset

Prefer:

```python
cleaned_employees = employees.copy()
```

This preserves the original data for comparison and auditing.

---

# 9. Practice Exercises

Complete these exercises without looking at the solutions first.

Use the employee dataset from Section 2.

## Part A — Missing Values

### Exercise 1

Count missing values in every column of `employees`.

### Exercise 2

Calculate the percentage of missing values in every column.

### Exercise 3

Display all rows where age is missing.

### Exercise 4

Create a copy of the dataset and fill missing ages with the median age.

### Exercise 5

Fill missing department values with `"Unknown"`.

## Part B — Duplicates

### Exercise 6

Count the exact duplicate rows.

### Exercise 7

Display all rows that have a duplicated `employee_id`, including the first occurrence.

### Exercise 8

Remove exact duplicate rows.

### Exercise 9

Remove duplicate employee IDs, keeping the last occurrence.

## Part C — Data Types

### Exercise 10

Display the data types of all columns.

### Exercise 11

Convert `performance_score` into a numeric column.

### Exercise 12

Convert `hire_date` into datetime.

### Exercise 13

Create a new column called `hire_year`.

### Exercise 14

Create a numeric Series containing:

```python
["100", "200", "unknown", "300"]
```

Convert it using `errors="coerce"`.

## Part D — Outliers

### Exercise 15

Calculate the mean, median, minimum and maximum salary.

### Exercise 16

Calculate Q1, Q3 and IQR for salary.

### Exercise 17

Calculate the lower and upper IQR boundaries.

### Exercise 18

Identify salary outliers using the IQR method.

### Exercise 19

Create a new DataFrame excluding the flagged salary outliers. Keep the original unchanged.

### Exercise 20

Identify employees whose ages are below 18 or above 100.

---

# 10. Mini Project — Clean an Employee Dataset

Your manager has sent you a messy employee dataset and wants a clean version ready for analysis.

Complete the following:

1. Create a copy of the original dataset.
2. Inspect its dimensions and data types.
3. Calculate missing values and percentages.
4. Identify and remove exact duplicates.
5. Check whether employee IDs are unique.
6. Convert numeric columns into the correct data types.
7. Convert `hire_date` to datetime.
8. Investigate missing ages and decide how to handle them.
9. Identify invalid ages.
10. Detect salary outliers using IQR.
11. Do not automatically delete suspicious records without investigating them.
12. Validate the final dataset.
13. Export the cleaned data to CSV.

Example export:

```python
cleaned_employees.to_csv(
    "cleaned_employees.csv",
    index=False
)
```

## What you should deliver

- A cleaned DataFrame.
- A list of missing values per column.
- A list of duplicate records.
- A list of invalid ages.
- A list of potential salary outliers.
- A short explanation of the cleaning decisions you made.

---

# 11. Data Cleaning Cheat Sheet

| Task | Pandas method |
|---|---|
| Check missing values | `df.isna()` |
| Count missing values | `df.isna().sum()` |
| Missing percentage | `df.isna().mean() * 100` |
| Find non-missing values | `df.notna()` |
| Remove missing rows | `df.dropna()` |
| Fill missing values | `df.fillna()` |
| Forward fill | `df.ffill()` |
| Backward fill | `df.bfill()` |
| Find duplicates | `df.duplicated()` |
| Count duplicates | `df.duplicated().sum()` |
| Remove duplicates | `df.drop_duplicates()` |
| Inspect data types | `df.dtypes` |
| Dataset information | `df.info()` |
| Convert to numeric | `pd.to_numeric()` |
| Convert to datetime | `pd.to_datetime()` |
| Convert data type | `df.astype()` |
| Calculate mean | `df.mean()` |
| Calculate median | `df.median()` |
| Calculate quartiles | `df.quantile()` |
| Calculate standard deviation | `df.std()` |
| Copy DataFrame | `df.copy()` |
| Export CSV | `df.to_csv()` |

---

# 12. Recommended Data Cleaning Workflow

When working with a new dataset, follow this sequence:

1. Load the original data.
2. Create a copy for cleaning.
3. Inspect the first rows and dataset dimensions.
4. Check column names and data types.
5. Identify missing values.
6. Investigate duplicate records.
7. Convert columns to appropriate data types.
8. Identify invalid values using business rules.
9. Investigate potential outliers.
10. Apply justified cleaning decisions.
11. Validate the cleaned dataset.
12. Export the final result.

This workflow helps make your analysis reproducible and reliable.

---

# Completion Checklist

Before moving to the next lesson, make sure you can:

- [ ] Identify missing values using `isna()`.
- [ ] Calculate missing-value counts and percentages.
- [ ] Remove rows with missing values when appropriate.
- [ ] Fill missing values using `fillna()`.
- [ ] Identify exact duplicate records.
- [ ] Find duplicates based on selected columns.
- [ ] Remove duplicates using `drop_duplicates()`.
- [ ] Inspect data types using `dtypes` and `info()`.
- [ ] Convert text into numeric values.
- [ ] Convert text into datetime.
- [ ] Identify potential outliers using descriptive statistics.
- [ ] Calculate IQR boundaries.
- [ ] Understand the Z-score method.
- [ ] Validate the cleaned dataset.
- [ ] Export cleaned data to CSV.

---

# What's Next?

After this lesson, recommended topics include:

- Pandas string operations and text cleaning.
- Date and time manipulation.
- Exploratory Data Analysis (EDA).
- Data visualization with Matplotlib.
- Working with real-world datasets.
- SQL data-quality checks.
- Data Analyst interview exercises.
