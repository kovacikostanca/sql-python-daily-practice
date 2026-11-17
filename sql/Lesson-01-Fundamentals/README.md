# SQL Masterclass – Lesson 1
## SQL Fundamentals: SELECT, WHERE, ORDER BY & NULL Values

Welcome to Lesson 1 of my SQL learning journey!

This lesson covers the fundamental SQL commands used to retrieve, filter, sort, and handle data in relational databases.

The goal is to understand not only how SQL syntax works, but also how to use it to answer real-world business questions as a Data Analyst.

---

## Table of Contents

1. [Introduction to SQL](#1-introduction-to-sql)
2. [Our Practice Database](#2-our-practice-database)
3. [SELECT – Retrieving Data](#3-select--retrieving-data)
4. [DISTINCT – Removing Duplicates](#4-distinct--removing-duplicates)
5. [WHERE – Filtering Data](#5-where--filtering-data)
6. [Comparison Operators](#6-comparison-operators)
7. [Logical Operators: AND & OR](#7-logical-operators-and--or)
8. [IN and NOT IN](#8-in-and-not-in)
9. [BETWEEN – Filtering a Range](#9-between--filtering-a-range)
10. [LIKE – Pattern Matching](#10-like--pattern-matching)
11. [ORDER BY – Sorting Data](#11-order-by--sorting-data)
12. [NULL Values – Understanding Missing Data](#12-null-values--understanding-missing-data)
13. [COALESCE – Replacing NULL in Results](#13-coalesce--replacing-null-in-results)
14. [Combining SQL Commands](#14-combining-sql-commands)
15. [SQL Logical Execution Order](#15-sql-logical-execution-order)
16. [LIMIT – Restricting Results](#16-limit--restricting-results)
17. [Common SQL Mistakes](#17-common-sql-mistakes)
18. [Practice Exercises](#18-practice-exercises)
19. [SQL Cheat Sheet](#19-sql-cheat-sheet)
20. [What I Learned](#20-what-i-learned)

---

## 1. Introduction to SQL

SQL stands for Structured Query Language.

It is used to communicate with relational databases and perform operations such as:

- Retrieving information.
- Filtering records.
- Sorting data.
- Analyzing datasets.
- Inserting, updating, and deleting records.
- Managing database structures.

SQL is an essential skill for Data Analysts, Data Scientists, Business Analysts, and Database Administrators.

In this lesson, we focus on retrieving and analyzing existing data.

## 2. Our Practice Database

Throughout this lesson, we use a fictional company database containing employee information.

Our table is called `employees`.

### Employees Table

| employee_id | first_name | department | salary | city | manager_id |
|---|---|---|---:|---|---:|
| 1 | John | IT | 55000 | Athens | 10 |
| 2 | Maria | HR | 42000 | Thessaloniki | 11 |
| 3 | George | IT | 65000 | Athens | 10 |
| 4 | Anna | Finance | 60000 | Patras | NULL |
| 5 | Peter | Marketing | 38000 | Athens | 12 |
| 6 | Elena | HR | 45000 | NULL | 11 |
| 7 | David | Finance | 72000 | Athens | NULL |
| 8 | Sofia | IT | 50000 | Heraklion | 10 |

### Understanding the Columns

| Column | Description |
|---|---|
| employee_id | Unique employee identifier |
| first_name | Employee's first name |
| department | Employee's department |
| salary | Employee's annual salary |
| city | Employee's city |
| manager_id | Employee's manager identifier |

Notice that some records contain `NULL` values. We will explore how SQL handles missing data later.

---

## 3. SELECT – Retrieving Data

`SELECT` is one of the most fundamental SQL commands.

It allows us to retrieve specific columns from a database table.

### Basic Syntax

```sql
SELECT column_name
FROM table_name;
```

### Example 1: Selecting One Column

Business question:

Show me the first names of all employees.

```sql
SELECT first_name
FROM employees;
```

Expected result:

| first_name |
|---|
| John |
| Maria |
| George |
| Anna |
| Peter |
| Elena |
| David |
| Sofia |

### Example 2: Selecting Multiple Columns

Business question:

Show me employee names, departments, and salaries.

```sql
SELECT first_name, department, salary
FROM employees;
```

We can select multiple columns by separating them with commas.

### Example 3: Selecting All Columns

The asterisk `*` selects every column from a table.

```sql
SELECT *
FROM employees;
```

**Professional tip:** Avoid using `SELECT *` unnecessarily in large datasets. Selecting only the columns you need can improve efficiency and make queries easier to maintain.

---

## 4. DISTINCT – Removing Duplicates

`DISTINCT` returns unique values and eliminates duplicate rows from the result.

Business question:

Which departments exist in our company?

Without DISTINCT:

```sql
SELECT department
FROM employees;
```

The result contains repeated departments.

With DISTINCT:

```sql
SELECT DISTINCT department
FROM employees;
```

Expected result:

| department |
|---|
| IT |
| HR |
| Finance |
| Marketing |

### DISTINCT with Multiple Columns

```sql
SELECT DISTINCT department, city
FROM employees;
```

This returns unique combinations of departments and cities, not just unique departments.

---

## 5. WHERE – Filtering Data

`WHERE` allows us to retrieve only records that satisfy a specific condition.

It is essential when working with large datasets.

### Basic Syntax

```sql
SELECT column_name
FROM table_name
WHERE condition;
```

### Example: Filtering by Department

Business question:

Find all employees who work in IT.

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

Expected result:

| employee_id | first_name | department | salary |
|---|---|---|---:|
| 1 | John | IT | 55000 |
| 3 | George | IT | 65000 |
| 8 | Sofia | IT | 50000 |

The WHERE clause filters records before returning the results.

---

## 6. Comparison Operators

SQL provides comparison operators to evaluate conditions.

| Operator | Meaning | Example |
|---|---|---|
| `=` | Equal to | `salary = 50000` |
| `<>` | Not equal to | `department <> 'IT'` |
| `!=` | Not equal to | `salary != 50000` |
| `>` | Greater than | `salary > 50000` |
| `<` | Less than | `salary < 50000` |
| `>=` | Greater than or equal to | `salary >= 50000` |
| `<=` | Less than or equal to | `salary <= 50000` |

### Example 1: Salary Greater Than 50,000

```sql
SELECT first_name, salary
FROM employees
WHERE salary > 50000;
```

### Example 2: Salary Less Than 50,000

```sql
SELECT first_name, salary
FROM employees
WHERE salary < 50000;
```

### Example 3: Employees Not Working in IT

```sql
SELECT first_name, department
FROM employees
WHERE department <> 'IT';
```

**Important:**

- Text values should generally be enclosed in single quotes.
- Numeric values do not require quotation marks.
- `<>` is the standard SQL not-equal operator.

---

## 7. Logical Operators: AND & OR

Logical operators allow us to combine multiple conditions.

### AND

`AND` requires all conditions to be TRUE.

Business question:

Find employees who work in IT and earn more than 50,000.

```sql
SELECT first_name, department, salary
FROM employees
WHERE department = 'IT'
AND salary > 50000;
```

Expected result:

| first_name | department | salary |
|---|---|---:|
| John | IT | 55000 |
| George | IT | 65000 |

### OR

`OR` requires at least one condition to be TRUE.

Business question:

Find employees who work in IT or HR.

```sql
SELECT first_name, department
FROM employees
WHERE department = 'IT'
OR department = 'HR';
```

### AND vs OR

| Operator | Requirement |
|---|---|
| AND | All conditions must be TRUE |
| OR | At least one condition must be TRUE |

### Combining AND and OR

SQL evaluates AND before OR.

Example:

```sql
SELECT *
FROM employees
WHERE department = 'IT'
OR department = 'HR'
AND salary > 50000;
```

SQL interprets this as:

```sql
WHERE department = 'IT'
OR (department = 'HR' AND salary > 50000);
```

To avoid confusion, use parentheses:

```sql
SELECT *
FROM employees
WHERE (department = 'IT' OR department = 'HR')
AND salary > 50000;
```

**Professional tip:** Use parentheses whenever you combine AND and OR to make your intentions clear.

---

## 8. IN and NOT IN

`IN` checks whether a value matches any value in a specified list.

It is a cleaner alternative to writing multiple OR conditions.

### IN Example

Business question:

Find employees who work in IT, HR, or Finance.

```sql
SELECT first_name, department
FROM employees
WHERE department IN ('IT', 'HR', 'Finance');
```

This is equivalent to:

```sql
WHERE department = 'IT'
OR department = 'HR'
OR department = 'Finance';
```

### NOT IN Example

Find employees who do not work in IT or HR.

```sql
SELECT first_name, department
FROM employees
WHERE department NOT IN ('IT', 'HR');
```

**Important NULL consideration:** If a column contains NULL, `NOT IN` does not automatically include those rows. NULL comparisons evaluate to UNKNOWN.

---

## 9. BETWEEN – Filtering a Range

`BETWEEN` filters values within a specified range.

Business question:

Find employees earning between 40,000 and 60,000.

```sql
SELECT first_name, salary
FROM employees
WHERE salary BETWEEN 40000 AND 60000;
```

This is equivalent to:

```sql
WHERE salary >= 40000
AND salary <= 60000;
```

### Important: BETWEEN Is Inclusive

Both boundaries are included.

| Salary | Included? |
|---:|---|
| 39999 | No |
| 40000 | Yes |
| 50000 | Yes |
| 60000 | Yes |
| 60001 | No |

---

## 10. LIKE – Pattern Matching

`LIKE` allows us to search for specific text patterns.

It is especially useful when we do not know the complete value.

### Wildcard Characters

| Wildcard | Meaning |
|---|---|
| `%` | Zero or more characters |
| `_` | Exactly one character |

### Examples

Find employees whose names start with M:

```sql
SELECT first_name
FROM employees
WHERE first_name LIKE 'M%';
```

Find employees whose names end with a:

```sql
SELECT first_name
FROM employees
WHERE first_name LIKE '%a';
```

Find employees whose names contain ar:

```sql
SELECT first_name
FROM employees
WHERE first_name LIKE '%ar%';
```

Find names where the second character is a:

```sql
SELECT first_name
FROM employees
WHERE first_name LIKE '_a%';
```

Find names beginning with A or D:

```sql
SELECT first_name
FROM employees
WHERE first_name LIKE 'A%'
OR first_name LIKE 'D%';
```

**Note:** LIKE case sensitivity depends on the database system and collation settings.

---

## 11. ORDER BY – Sorting Data

`ORDER BY` sorts query results.

It can sort numbers, text, and dates.

### Basic Syntax

```sql
SELECT column_name
FROM table_name
ORDER BY column_name;
```

### ASC – Ascending Order

`ASC` sorts from smallest to largest or alphabetically.

```sql
SELECT first_name, salary
FROM employees
ORDER BY salary ASC;
```

Expected salary order:

38,000 → 42,000 → 45,000 → 50,000 → 55,000 → 60,000 → 65,000 → 72,000.

ASC is the default sorting direction.

### DESC – Descending Order

`DESC` sorts from largest to smallest or reverse alphabetically.

```sql
SELECT first_name, salary
FROM employees
ORDER BY salary DESC;
```

### ORDER BY with Multiple Columns

Business question:

Sort employees by department alphabetically, then by salary from highest to lowest within each department.

```sql
SELECT first_name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
```

The first sorting column has priority. The second column determines the order within groups having the same first-column value.

### ORDER BY with Column Positions

```sql
SELECT first_name, department, salary
FROM employees
ORDER BY 3 DESC;
```

The number 3 refers to the third selected column, which is salary.

**Professional tip:** Using column names is generally clearer and easier to maintain than using column positions.

---

## 12. NULL Values – Understanding Missing Data

`NULL` represents a missing, unknown, or unavailable value.

It is one of the most important concepts in SQL.

### NULL Is Not the Same as Zero

| Value | Meaning |
|---|---|
| NULL | Missing or unknown |
| 0 | Numeric zero |
| `''` | Empty string |
| `' '` | A string containing a space |

### Incorrect: Comparing NULL Using =

This is incorrect:

```sql
SELECT *
FROM employees
WHERE manager_id = NULL;
```

Why?

Because NULL is not an ordinary value. SQL uses three-valued logic when evaluating NULL comparisons.

### Correct: IS NULL

Find employees without an assigned manager.

```sql
SELECT first_name, manager_id
FROM employees
WHERE manager_id IS NULL;
```

Expected result:

| first_name | manager_id |
|---|---|
| Anna | NULL |
| David | NULL |

### IS NOT NULL

Find employees who have a manager assigned.

```sql
SELECT first_name, manager_id
FROM employees
WHERE manager_id IS NOT NULL;
```

### Important NULL Rules

| Condition | Meaning |
|---|---|
| `IS NULL` | Value is missing |
| `IS NOT NULL` | Value exists |
| `= NULL` | Incorrect way to check NULL |
| `<> NULL` | Incorrect way to check NULL |

### SQL Three-Valued Logic

SQL uses three possible logical results:

- TRUE
- FALSE
- UNKNOWN

Examples:

| Expression | Result |
|---|---|
| `5 = 5` | TRUE |
| `5 = 10` | FALSE |
| `5 = NULL` | UNKNOWN |
| `NULL = NULL` | UNKNOWN |
| `NULL <> 5` | UNKNOWN |

Why?

Because SQL cannot determine whether an unknown value is equal to or different from another value.

### NULL with AND and OR

| Expression | Result |
|---|---|
| TRUE AND UNKNOWN | UNKNOWN |
| FALSE AND UNKNOWN | FALSE |
| TRUE OR UNKNOWN | TRUE |
| FALSE OR UNKNOWN | UNKNOWN |

An important rule:

**WHERE returns only records for which the condition evaluates to TRUE.**

FALSE and UNKNOWN are excluded.

### NULL and ORDER BY

Different database systems handle NULL sorting differently.

| Database | ASC | DESC |
|---|---|---|
| PostgreSQL | NULLs last | NULLs first |
| Oracle | NULLs last | NULLs first |
| MySQL | NULLs first | NULLs last |
| SQL Server | NULLs first | NULLs last |

In PostgreSQL and Oracle, we can explicitly control NULL placement:

```sql
SELECT first_name, city
FROM employees
ORDER BY city ASC NULLS LAST;
```

```sql
SELECT first_name, city
FROM employees
ORDER BY city ASC NULLS FIRST;
```

The exact syntax depends on the database.

---

## 13. COALESCE – Replacing NULL in Results

`COALESCE` returns the first non-NULL expression from a list.

It is useful when presenting reports with missing values.

Business question:

Show employee names and cities. If a city is missing, display Unknown.

```sql
SELECT
    first_name,
    COALESCE(city, 'Unknown') AS city
FROM employees;
```

Expected result for Elena:

| first_name | city |
|---|---|
| Elena | Unknown |

Important:

COALESCE changes the value displayed in the query result. It does not permanently update the original database record.

---

## 14. Combining SQL Commands

The real power of SQL comes from combining multiple commands to answer business questions.

### Business Challenge

Find employees who:

- Work in IT or Finance.
- Earn more than 50,000.
- Have an assigned manager.
- Sort results from highest to lowest salary.

### Complete Query

```sql
SELECT
    first_name,
    department,
    salary
FROM employees
WHERE department IN ('IT', 'Finance')
    AND salary > 50000
    AND manager_id IS NOT NULL
ORDER BY salary DESC;
```

Expected result:

| first_name | department | salary |
|---|---|---:|
| George | IT | 65000 |
| John | IT | 55000 |

This example demonstrates how SELECT, WHERE, IN, AND, IS NOT NULL, and ORDER BY work together.

---

## 15. SQL Logical Execution Order

One important concept is that SQL is written in one order but logically processed in another.

Consider:

```sql
SELECT first_name, salary
FROM employees
WHERE salary > 40000
ORDER BY salary DESC;
```

The logical execution order is:

1. FROM – Identify the source table.
2. WHERE – Filter rows.
3. SELECT – Determine the output columns.
4. ORDER BY – Sort the resulting rows.

This explains why certain column aliases created in SELECT cannot normally be referenced in WHERE.

Example:

This may fail:

```sql
SELECT salary * 12 AS annual_salary
FROM employees
WHERE annual_salary > 600000;
```

A correct alternative:

```sql
SELECT salary * 12 AS annual_salary
FROM employees
WHERE salary * 12 > 600000;
```

The full logical order will become more important as we study GROUP BY, HAVING, and aggregate functions.

---

## 16. LIMIT – Restricting Results

`LIMIT` restricts the maximum number of rows returned.

Business question:

Find the three highest-paid employees.

```sql
SELECT first_name, salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

Expected result:

| first_name | salary |
|---|---:|
| David | 72000 |
| George | 65000 |
| Anna | 60000 |

**Important:** LIMIT is supported in PostgreSQL and MySQL. Other database systems use different syntax, such as TOP in SQL Server.

---

## 17. Common SQL Mistakes

These are mistakes that beginners should learn to recognize.

### Mistake 1: Using = NULL

Incorrect:

```sql
WHERE manager_id = NULL;
```

Correct:

```sql
WHERE manager_id IS NULL;
```

### Mistake 2: Confusing IS NULL and IS NOT NULL

`IS NULL` includes missing values.

`IS NOT NULL` excludes missing values.

### Mistake 3: Confusing AND and OR

AND requires every condition to be TRUE.

OR requires at least one condition to be TRUE.

### Mistake 4: Forgetting Parentheses

Use parentheses when combining AND and OR to make filtering logic explicit.

### Mistake 5: Incorrect Column Names

SQL column names must match the actual database schema.

For example:

```sql
WHERE deparment = 'Finance';
```

This is incorrect if the actual column is called `department`.

### Mistake 6: Forgetting That BETWEEN Is Inclusive

```sql
WHERE salary BETWEEN 40000 AND 60000;
```

Includes both 40,000 and 60,000.

### Mistake 7: Thinking That A Query That Runs Is Always Correct

A query can execute successfully and still produce the wrong business result.

Always check whether the output makes sense.

---

## 18. Practice Exercises

Use the employees table to solve the following business questions.

### Beginner Level

**Exercise 1**

Display only the first names and salaries of all employees.

**Exercise 2**

Find employees who work in Finance.

Return their names and salaries.

**Exercise 3**

Find employees earning more than or equal to 50,000.

**Exercise 4**

Find employees who work in IT or HR and earn more than 45,000.

**Exercise 5**

Display all employees sorted by salary from lowest to highest.

### Intermediate Level

**Exercise 6**

Display employees sorted by department alphabetically, then salary from highest to lowest.

**Exercise 7**

Find employees who do not have an assigned manager.

**Exercise 8**

Find employees who either live in Athens or have no city recorded.

**Exercise 9**

Find employees who:

- Work in IT or Finance.
- Earn between 50,000 and 70,000.
- Have an assigned manager.
- Are sorted by salary from highest to lowest.

### Advanced Level

**Exercise 10**

Find employees who earn more than 40,000 but less than 65,000. Exclude employees whose city is NULL. Sort by salary descending.

**Exercise 11**

Find employees who work in IT or Finance and earn at least 55,000.

**Exercise 12**

Display employees who do not work in HR and whose salary is not NULL.

**Exercise 13**

Find employees whose first names start with A or D.

**Exercise 14**

Find employees who either earn more than 60,000 or have no manager assigned. Sort by salary ascending.

---

## 19. SQL Cheat Sheet

| Command | Purpose |
|---|---|
| SELECT | Retrieve columns |
| SELECT * | Retrieve all columns |
| DISTINCT | Return unique rows |
| FROM | Specify the source table |
| WHERE | Filter records |
| = | Equal to |
| <> | Not equal to |
| > | Greater than |
| < | Less than |
| >= | Greater than or equal to |
| <= | Less than or equal to |
| AND | All conditions must be TRUE |
| OR | At least one condition must be TRUE |
| IN | Match values in a list |
| NOT IN | Exclude listed values |
| BETWEEN | Filter a range |
| LIKE | Match a text pattern |
| IS NULL | Find missing values |
| IS NOT NULL | Find non-missing values |
| COALESCE | Return the first non-NULL value |
| ORDER BY | Sort results |
| ASC | Ascending order |
| DESC | Descending order |
| LIMIT | Restrict returned rows |

### Important SQL Syntax Template

```sql
SELECT column1, column2
FROM table_name
WHERE condition
ORDER BY column1 ASC;
```

---

## 20. What I Learned

In Lesson 1, I learned how to:

- Retrieve data using SELECT.
- Select individual and multiple columns.
- Retrieve all columns using SELECT *.
- Remove duplicate values using DISTINCT.
- Filter records using WHERE.
- Apply comparison operators.
- Combine conditions using AND and OR.
- Filter multiple values using IN and NOT IN.
- Filter ranges using BETWEEN.
- Search text patterns using LIKE.
- Sort results using ORDER BY, ASC, and DESC.
- Understand missing data using NULL.
- Filter NULL values using IS NULL and IS NOT NULL.
- Understand SQL's three-valued logic.
- Replace NULL values in query results using COALESCE.
- Combine multiple SQL commands to answer business questions.
- Understand the logical execution order of SQL.
- Restrict results using LIMIT.

### Key Takeaway

Learning SQL is not about memorizing commands. It is about understanding how to translate business questions into logical queries and retrieve accurate information from data.

The ability to write a query is important, but the ability to understand and validate its results is what makes a good Data Analyst.

---

## Next Lesson

**Lesson 2 – Aggregations, GROUP BY & HAVING**

Topics:

- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()
- GROUP BY
- HAVING
- WHERE vs HAVING
- Aggregate functions with NULL values
- Real-world business analysis exercises

Stay tuned for the next lesson!
