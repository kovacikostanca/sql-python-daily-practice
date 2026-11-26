# Lesson 02: Python (Lists, Dictionaries, Functions & Loops)

## 1. Lesson Overview

Welcome to Python Lesson 02!

In this lesson, we move beyond basic Python syntax and learn four fundamental programming concepts:

1. Lists
2. Dictionaries
3. Functions
4. Loops

These concepts are essential for anyone who wants to become a Data Professional.

As a Data Analyst, you will frequently need to:
- Store multiple records.
- Organize structured information.
- Extract specific values from datasets.
- Calculate totals and averages.
- Automate repetitive operations.
- Create reusable calculations.
- Process records one by one.
- Transform raw data into meaningful information.

Python provides powerful tools for performing these tasks. Before moving to Pandas and more advanced data-analysis libraries, it is important to understand how these fundamental structures and operations work.

### Lesson Information

| Category | Details |
|---|---|
| Lesson | Python Lesson 02 |
| Level | Beginner |
| Language | Python 3 |
| Main topics | Lists, Dictionaries, Functions, Loops |
| Focus | Data Analysis |
| Dataset | Employee dataset |
| Exercises | 21 |
| Mini project | Employee Analytics Report |

**Repository location:**

`python/Lesson-01-Python-Basics/README.md`

---

## 2. Learning Objectives

By the end of this lesson, you should be able to:

### Lists
- Understand what lists are.
- Create lists containing different data types.
- Access elements using indexes.
- Use negative indexing.
- Extract elements using slicing.
- Add, update, and remove elements.
- Sort lists.
- Calculate minimum, maximum, sum, and length.
- Check whether an element exists in a list.

### Dictionaries
- Understand key-value pairs.
- Create dictionaries.
- Access values using keys.
- Add and update dictionary elements.
- Remove dictionary elements.
- Use dictionary methods.
- Iterate through dictionaries.
- Work with lists of dictionaries.

### Functions
- Understand why functions are useful.
- Define and call functions.
- Use parameters and arguments.
- Return calculated results.
- Understand the difference between `print()` and `return`.
- Use multiple parameters.
- Define default parameters.
- Create reusable analytical functions.

### Loops
- Understand why loops are important.
- Use `for` loops.
- Use `while` loops.
- Iterate through lists and dictionaries.
- Use conditions inside loops.
- Calculate totals and counts.
- Use `break` and `continue`.
- Use `range()` and `enumerate()`.

### Data Analyst Skills
- Process employee records.
- Calculate total and average salaries.
- Count employees by department.
- Calculate department-level statistics.
- Classify salaries.
- Generate a simple analytical report.

---

# PART 1: LISTS

## 3. What Is a List?

A list is a Python data structure that allows us to store multiple values inside one variable.

Instead of creating separate variables for every employee, salary, or department, we can store all values in a single list.

Example:

```python
employee_1 = "John"
employee_2 = "Maria"
employee_3 = "George"
```

This approach becomes inconvenient when working with hundreds or thousands of records.

Instead, we can create:

```python
employees = ["John", "Maria", "George"]
```

Now all three employees are stored in one variable.

### 3.1 Characteristics of Lists

Python lists have four important characteristics:

| Characteristic | Explanation |
|---|---|
| Ordered | Elements maintain their position |
| Mutable | Elements can be changed |
| Allow duplicates | The same value can appear multiple times |
| Flexible | Different data types can be stored together |

Example:

```python
example_list = [
    "John",
    55000,
    True,
    None,
    25.5
]

print(example_list)
```

A list can contain strings, integers, floats, Boolean values, and `None`.

### 3.2 Creating Lists

Syntax:

```python
list_name = [value1, value2, value3]
```

Examples:

```python
employees = ["John", "Maria", "George", "Anna"]

salaries = [55000, 42000, 65000, 60000]

departments = ["IT", "HR", "Finance", "Marketing"]

sales = [250.50, 400.00, 150.75, 600.00]
```

You can also create an empty list:

```python
employees = []
```

This is useful when you want to populate a list later using a loop.

---

## 4. Accessing List Elements

Python uses zero-based indexing.

This means the first element has index `0`, not index `1`.

Example:

```python
employees = ["John", "Maria", "George", "Anna"]
```

| Index | Employee |
|---:|---|
| 0 | John |
| 1 | Maria |
| 2 | George |
| 3 | Anna |

Accessing elements:

```python
print(employees[0])
print(employees[1])
print(employees[2])
```

Output:

```text
John
Maria
George
```

### 4.1 Negative Indexing

Python also supports negative indexes.

Negative indexing starts from the end of the list.

```python
employees = ["John", "Maria", "George", "Anna"]

print(employees[-1])
print(employees[-2])
```

Output:

```text
Anna
George
```

| Index | Element |
|---:|---|
| -1 | Anna |
| -2 | George |
| -3 | Maria |
| -4 | John |

This is particularly useful when you need the last record without knowing how many elements the list contains.

### 4.2 Common Indexing Error

Consider:

```python
employees = ["John", "Maria", "George"]
```

This will produce an error:

```python
print(employees[3])
```

Why?

There are only three elements, with indexes `0`, `1`, and `2`.

The error is:

```text
IndexError: list index out of range
```

Always remember that the last valid positive index is:

`len(list_name) - 1`

---

## 5. List Slicing

Slicing allows us to extract a section of a list.

Syntax:

```python
list_name[start:end]
```

Important:
- `start` is included.
- `end` is excluded.

Example:

```python
employees = ["John", "Maria", "George", "Anna", "Peter"]

print(employees[0:2])
```

Output:

```text
['John', 'Maria']
```

The elements at indexes 0 and 1 are returned, but index 2 is excluded.

### 5.1 More Slicing Examples

```python
employees = ["John", "Maria", "George", "Anna", "Peter"]

print(employees[1:4])
print(employees[:3])
print(employees[2:])
print(employees[-2:])
```

Output:

```text
['Maria', 'George', 'Anna']
['John', 'Maria', 'George']
['George', 'Anna', 'Peter']
['Anna', 'Peter']
```

### 5.2 Slicing with Steps

Syntax:

```python
list_name[start:end:step]
```

Example:

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]

print(numbers[::2])
```

Output:

```text
[1, 3, 5, 7, 9]
```

The step value of 2 means Python takes every second element.

Reverse a list using slicing:

```python
print(numbers[::-1])
```

Output:

```text
[9, 8, 7, 6, 5, 4, 3, 2, 1]
```

---

## 6. Modifying Lists

Lists are mutable, meaning we can change their contents.

### 6.1 Updating an Element

```python
employees = ["John", "Maria", "George"]

employees[1] = "Elena"

print(employees)
```

Output:

```text
['John', 'Elena', 'George']
```

### 6.2 Adding Elements with append()

The `append()` method adds one element to the end of a list.

```python
employees = ["John", "Maria"]

employees.append("George")

print(employees)
```

Output:

```text
['John', 'Maria', 'George']
```

### 6.3 Adding Elements with insert()

The `insert()` method adds an element at a specific position.

Syntax:

```python
list_name.insert(index, value)
```

Example:

```python
employees = ["John", "Maria", "George"]

employees.insert(1, "Anna")

print(employees)
```

Output:

```text
['John', 'Anna', 'Maria', 'George']
```

### 6.4 Adding Multiple Elements with extend()

The `extend()` method adds multiple elements from another iterable.

```python
employees = ["John", "Maria"]

employees.extend(["George", "Anna"])

print(employees)
```

Output:

```text
['John', 'Maria', 'George', 'Anna']
```

### 6.5 append() vs extend()

This distinction is important.

Using `append()`:

```python
numbers = [1, 2, 3]

numbers.append([4, 5])

print(numbers)
```

Output:

```text
[1, 2, 3, [4, 5]]
```

The entire second list is added as one element.

Using `extend()`:

```python
numbers = [1, 2, 3]

numbers.extend([4, 5])

print(numbers)
```

Output:

```text
[1, 2, 3, 4, 5]
```

The elements are added individually.

---

## 7. Removing List Elements

Python provides several ways to remove elements.

### 7.1 remove()

Removes an element by value.

```python
employees = ["John", "Maria", "George", "Anna"]

employees.remove("Maria")

print(employees)
```

Output:

```text
['John', 'George', 'Anna']
```

If the value does not exist, Python raises a `ValueError`.

### 7.2 pop()

Removes an element by index and returns the removed value.

```python
employees = ["John", "Maria", "George", "Anna"]

removed_employee = employees.pop()

print(removed_employee)
print(employees)
```

Output:

```text
Anna
['John', 'Maria', 'George']
```

You can also specify an index:

```python
employees.pop(0)
```

This removes the first employee.

### 7.3 clear()

Removes all elements.

```python
employees = ["John", "Maria", "George"]

employees.clear()

print(employees)
```

Output:

```text
[]
```

### 7.4 del

Deletes an element or a section of a list.

```python
employees = ["John", "Maria", "George", "Anna"]

del employees[1]

print(employees)
```

Output:

```text
['John', 'George', 'Anna']
```

---

## 8. Useful List Functions and Methods

Python provides built-in functions that make working with lists easier.

Example dataset:

```python
salaries = [55000, 42000, 65000, 60000, 38000]
```

### len()

Returns the number of elements.

```python
print(len(salaries))
```

Output:

```text
5
```

### sum()

Calculates the total.

```python
print(sum(salaries))
```

Output:

```text
260000
```

### min()

Returns the smallest value.

```python
print(min(salaries))
```

Output:

```text
38000
```

### max()

Returns the largest value.

```python
print(max(salaries))
```

Output:

```text
65000
```

### sorted()

Returns a new sorted list.

```python
sorted_salaries = sorted(salaries)

print(sorted_salaries)
```

Output:

```text
[38000, 42000, 55000, 60000, 65000]
```

For descending order:

```python
sorted_salaries = sorted(salaries, reverse=True)
```

### sort()

Changes the original list.

```python
salaries.sort()

print(salaries)
```

Descending:

```python
salaries.sort(reverse=True)
```

### Important Difference: sorted() vs sort()

| Method | Changes original list? | Returns |
|---|---|---|
| `sorted()` | No | New sorted list |
| `.sort()` | Yes | `None` |

### 8.1 Checking Whether an Element Exists

Use the `in` operator.

```python
departments = ["IT", "HR", "Finance", "Marketing"]

print("IT" in departments)
print("Sales" in departments)
```

Output:

```text
True
False
```

This is useful for checking whether a category or value exists before processing it.

---

# PART 2: DICTIONARIES

## 9. What Is a Dictionary?

A dictionary is a Python data structure that stores information using key-value pairs.

Think of a dictionary as a structured record.

Example:

```python
employee = {
    "employee_id": 1,
    "name": "John",
    "department": "IT",
    "salary": 55000
}
```

Here:

| Key | Value |
|---|---|
| employee_id | 1 |
| name | John |
| department | IT |
| salary | 55000 |

Unlike lists, dictionaries are accessed using keys rather than numeric positions.

### 9.1 Characteristics of Dictionaries

| Characteristic | Explanation |
|---|---|
| Mutable | Values can be changed |
| Key-value structure | Every value is associated with a key |
| Unique keys | A key can appear only once |
| Flexible values | Values can have different data types |
| Insertion ordered | Maintains insertion order in modern Python |

Dictionary keys must be hashable, commonly strings, numbers, or tuples of hashable values.

---

## 10. Creating and Accessing Dictionaries

### 10.1 Creating a Dictionary

```python
employee = {
    "employee_id": 1,
    "name": "John",
    "department": "IT",
    "salary": 55000
}
```

An empty dictionary:

```python
employee = {}
```

### 10.2 Accessing Values

Use the key:

```python
print(employee["name"])
print(employee["salary"])
```

Output:

```text
John
55000
```

### 10.3 Using get()

The `.get()` method is safer when a key might not exist.

```python
employee = {
    "name": "John",
    "department": "IT",
    "salary": 55000
}

print(employee.get("name"))
print(employee.get("city"))
print(employee.get("city", "Unknown"))
```

Output:

```text
John
None
Unknown
```

Difference:

```python
employee["city"]
```

Raises `KeyError` if the key does not exist.

```python
employee.get("city")
```

Returns `None` if the key does not exist.

This is useful when processing external datasets where some fields may be missing.

---

## 11. Adding and Updating Dictionary Values

Dictionaries are mutable.

### 11.1 Updating an Existing Value

```python
employee = {
    "name": "John",
    "department": "IT",
    "salary": 55000
}

employee["salary"] = 58000

print(employee)
```

### 11.2 Adding a New Key

```python
employee["city"] = "Athens"

print(employee)
```

The dictionary now contains the city field.

### 11.3 Updating Multiple Values

Use `update()`:

```python
employee.update({
    "salary": 62000,
    "city": "Thessaloniki"
})
```

This updates existing keys and adds new ones.

---

## 12. Removing Dictionary Elements

### del

```python
del employee["department"]
```

### pop()

```python
removed_salary = employee.pop("salary")
```

Returns the removed value.

### popitem()

Removes and returns the last inserted key-value pair.

```python
employee.popitem()
```

### clear()

Removes all elements.

```python
employee.clear()
```

---

## 13. Useful Dictionary Methods

Example:

```python
employee = {
    "name": "Maria",
    "department": "HR",
    "salary": 42000
}
```

### keys()

Returns all keys.

```python
print(employee.keys())
```

### values()

Returns all values.

```python
print(employee.values())
```

### items()

Returns key-value pairs.

```python
print(employee.items())
```

### update()

Adds or changes multiple values.

```python
employee.update({"salary": 45000})
```

### Summary

| Method | Description |
|---|---|
| `.keys()` | Returns all keys |
| `.values()` | Returns all values |
| `.items()` | Returns key-value pairs |
| `.get()` | Safely retrieves a value |
| `.update()` | Adds or updates values |
| `.pop()` | Removes a key and returns its value |
| `.clear()` | Removes all entries |

---

## 14. Looping Through Dictionaries

We can use loops to process dictionary information.

### 14.1 Loop Through Keys

```python
employee = {
    "name": "Maria",
    "department": "HR",
    "salary": 42000
}

for key in employee:
    print(key)
```

### 14.2 Loop Through Values

```python
for value in employee.values():
    print(value)
```

### 14.3 Loop Through Keys and Values

```python
for key, value in employee.items():
    print(key, ":", value)
```

Output:

```text
name : Maria
department : HR
salary : 42000
```

---

## 15. Lists of Dictionaries

This is one of the most important structures in this lesson.

A list of dictionaries allows us to store multiple records with the same fields.

Example:

```python
employees = [
    {
        "name": "John",
        "department": "IT",
        "salary": 55000
    },
    {
        "name": "Maria",
        "department": "HR",
        "salary": 42000
    },
    {
        "name": "George",
        "department": "IT",
        "salary": 65000
    },
    {
        "name": "Anna",
        "department": "Finance",
        "salary": 60000
    }
]
```

This structure resembles a database table.

| name | department | salary |
|---|---|---:|
| John | IT | 55000 |
| Maria | HR | 42000 |
| George | IT | 65000 |
| Anna | Finance | 60000 |

### 15.1 Accessing Records

```python
print(employees[0])
```

Returns the first employee's complete dictionary.

```python
print(employees[0]["name"])
```

Returns:

```text
John
```

```python
print(employees[2]["salary"])
```

Returns:

```text
65000
```

### 15.2 Why Is This Important for Data Analysts?

Lists of dictionaries are commonly encountered when:

- Reading JSON files.
- Working with APIs.
- Processing records from web services.
- Handling structured information before converting it into a DataFrame.
- Building small reports and data-processing scripts.

Understanding this structure will make learning Pandas easier.

---

# PART 3: FUNCTIONS

## 16. What Is a Function?

A function is a reusable block of code that performs a specific task.

Instead of writing the same calculation many times, we define it once and call it whenever necessary.

For example, imagine calculating the average salary for five different departments.

Without a function, we might repeat the same calculation five times.

With a function, we write the logic once.

### 16.1 Why Are Functions Important?

Functions help us:

- Avoid repeated code.
- Organize programs.
- Make code easier to read.
- Reuse calculations.
- Test individual operations.
- Build more maintainable projects.

---

## 17. Defining and Calling Functions

Syntax:

```python
def function_name():
    # Instructions
```

Example:

```python
def greet():
    print("Hello, Data Analyst!")

greet()
```

Output:

```text
Hello, Data Analyst!
```

The keyword `def` defines the function.

The function name is `greet`.

The parentheses contain parameters, if needed.

The indented code is the function body.

Calling the function executes it.

---

## 18. Functions with Parameters

Parameters allow functions to receive information.

```python
def greet_employee(name):
    print("Hello,", name)

greet_employee("John")
greet_employee("Maria")
```

Output:

```text
Hello, John
Hello, Maria
```

Here:
- `name` is a parameter.
- `"John"` and `"Maria"` are arguments.

### 18.1 Multiple Parameters

```python
def calculate_total(price, quantity):
    total = price * quantity
    print(total)

calculate_total(100, 3)
```

Output:

```text
300
```

### 18.2 Keyword Arguments

You can also specify arguments by parameter name.

```python
def employee_info(name, department, salary):
    print(name, department, salary)

employee_info(
    name="John",
    department="IT",
    salary=55000
)
```

This makes calls easier to read, particularly when functions have multiple parameters.

---

## 19. Return Values

This is one of the most important concepts in Python.

There is a difference between `print()` and `return`.

### print()

Displays information.

### return

Sends a value back to the place where the function was called.

Example:

```python
def calculate_total(price, quantity):
    total = price * quantity
    return total

result = calculate_total(100, 3)

print(result)
```

Output:

```text
300
```

Now the result can be reused:

```python
total_1 = calculate_total(100, 3)
total_2 = calculate_total(50, 4)

grand_total = total_1 + total_2

print(grand_total)
```

Output:

```text
500
```

### 19.1 What Happens If We Don't Use return?

```python
def calculate_total(price, quantity):
    print(price * quantity)

result = calculate_total(100, 3)

print(result)
```

Output:

```text
300
None
```

Why?

The function displays `300`, but it does not return that value.

Python functions return `None` by default if no explicit return statement is executed.

This distinction is essential when writing analytical functions.

---

## 20. Functions with Conditions

Functions can contain `if`, `elif`, and `else`.

Example: salary classification.

```python
def salary_category(salary):

    if salary > 60000:
        return "High"

    elif salary > 45000:
        return "Medium"

    else:
        return "Low"


print(salary_category(72000))
print(salary_category(55000))
print(salary_category(42000))
```

Output:

```text
High
Medium
Low
```

Conditions are evaluated from top to bottom.

The first matching condition is executed.

### Important Example: Condition Order

```python
def salary_category(salary):

    if salary > 45000:
        return "Medium"

    elif salary > 60000:
        return "High"

    else:
        return "Low"
```

A salary of `65000` returns `"Medium"` because the first condition is already true.

Always place more specific or higher-threshold conditions before broader conditions when the logic requires it.

---

## 21. Default Parameters

Functions can have default parameter values.

```python
def greet_employee(name, department="IT"):
    return f"{name} works in {department}"
```

Calling the function:

```python
print(greet_employee("John"))
print(greet_employee("Maria", "HR"))
```

Output:

```text
John works in IT
Maria works in HR
```

If no department is provided, Python uses `"IT"`.

---

## 22. Practical Data Analyst Functions

### 22.1 Calculate Average

```python
def calculate_average(numbers):

    if len(numbers) == 0:
        return 0

    return sum(numbers) / len(numbers)


salaries = [55000, 42000, 65000, 60000, 38000]

print(calculate_average(salaries))
```

Output:

```text
52000.0
```

The empty-list check prevents division by zero.

### 22.2 Calculate Bonus

```python
def calculate_bonus(salary, bonus_rate=0.10):

    return salary * bonus_rate


print(calculate_bonus(55000))
```

Output:

```text
5500.0
```

### 22.3 Find Highest Salary

```python
def highest_salary(salaries):

    if len(salaries) == 0:
        return None

    return max(salaries)


print(highest_salary([55000, 42000, 65000, 60000]))
```

Output:

```text
65000
```

### 22.4 Count Employees by Department

```python
def count_department(employees, department):

    count = 0

    for employee in employees:

        if employee["department"] == department:
            count += 1

    return count
```

This function can be reused for IT, HR, Finance, or any other department.

---

# PART 4: LOOPS

## 23. What Are Loops?

A loop allows us to execute a block of code repeatedly.

Imagine having 10,000 employee records.

Printing or calculating information manually for each employee would be impractical.

Loops automate this process.

Python has two main types:

1. `for` loops
2. `while` loops

---

## 24. For Loops

A `for` loop iterates over the elements of a sequence or other iterable.

Syntax:

```python
for item in sequence:
    # Instructions
```

Example:

```python
employees = ["John", "Maria", "George", "Anna"]

for employee in employees:
    print(employee)
```

Output:

```text
John
Maria
George
Anna
```

The loop processes one employee at a time.

### 24.1 Looping Through Numbers with range()

The `range()` function generates a sequence of integers.

```python
for number in range(5):
    print(number)
```

Output:

```text
0
1
2
3
4
```

The ending value is excluded.

You can specify a start and an end:

```python
for number in range(1, 6):
    print(number)
```

Output:

```text
1
2
3
4
5
```

You can also specify a step:

```python
for number in range(1, 10, 2):
    print(number)
```

Output:

```text
1
3
5
7
9
```

---

## 25. Looping Through Lists

Example:

```python
salaries = [55000, 42000, 65000, 60000, 38000]

for salary in salaries:
    print(salary)
```

### 25.1 Calculating a Total

```python
salaries = [55000, 42000, 65000, 60000, 38000]

total_salary = 0

for salary in salaries:
    total_salary += salary

print(total_salary)
```

Output:

```text
260000
```

The `+=` operator adds a value to the existing variable.

It is equivalent to:

```python
total_salary = total_salary + salary
```

### 25.2 Counting Elements

```python
employees = ["John", "Maria", "George", "Anna"]

count = 0

for employee in employees:
    count += 1

print(count)
```

Output:

```text
4
```

---

## 26. Conditions Inside Loops

Loops become much more powerful when combined with conditions.

Example: Find employees earning more than 50000.

```python
employees = [
    {"name": "John", "salary": 55000},
    {"name": "Maria", "salary": 42000},
    {"name": "George", "salary": 65000},
    {"name": "Anna", "salary": 60000}
]

for employee in employees:

    if employee["salary"] > 50000:
        print(employee["name"])
```

Output:

```text
John
George
Anna
```

This resembles filtering rows in SQL.

For example:

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

The Python loop checks each record and only processes those meeting the condition.

---

## 27. Looping Through Dictionaries

```python
employee = {
    "name": "John",
    "department": "IT",
    "salary": 55000
}

for key, value in employee.items():
    print(key, value)
```

This allows us to process all fields of a record.

---

## 28. Looping Through Lists of Dictionaries

This is especially useful for Data Analysts.

```python
employees = [
    {"name": "John", "department": "IT", "salary": 55000},
    {"name": "Maria", "department": "HR", "salary": 42000},
    {"name": "George", "department": "IT", "salary": 65000},
    {"name": "Anna", "department": "Finance", "salary": 60000}
]

for employee in employees:

    print(
        employee["name"],
        employee["department"],
        employee["salary"]
    )
```

Each iteration processes one employee record.

### 28.1 Calculating Total Salary

```python
total_salary = 0

for employee in employees:
    total_salary += employee["salary"]

print(total_salary)
```

### 28.2 Filtering Employees

```python
for employee in employees:

    if employee["department"] == "IT":
        print(employee["name"])
```

### 28.3 Creating a New List

We can use loops to build a new list containing only selected records.

```python
high_salary_employees = []

for employee in employees:

    if employee["salary"] > 50000:
        high_salary_employees.append(employee)

print(high_salary_employees)
```

This is a common data transformation pattern.

---

## 29. While Loops

A `while` loop continues executing while its condition is true.

Syntax:

```python
while condition:
    # Instructions
```

Example:

```python
number = 1

while number <= 5:

    print(number)

    number += 1
```

Output:

```text
1
2
3
4
5
```

### 29.1 How Does It Work?

1. Python checks whether `number <= 5`.
2. If true, it executes the code.
3. The number increases by one.
4. Python checks the condition again.
5. The loop stops when the condition becomes false.

### 29.2 Important: Infinite Loops

This code is dangerous:

```python
number = 1

while number <= 5:
    print(number)
```

The number never changes.

The condition remains true forever.

Always make sure that a `while` loop has a way to reach its stopping condition.

---

## 30. Break and Continue

### 30.1 break

The `break` statement immediately stops a loop.

```python
for number in range(1, 10):

    if number == 5:
        break

    print(number)
```

Output:

```text
1
2
3
4
```

### 30.2 continue

The `continue` statement skips the current iteration and continues with the next one.

```python
for number in range(1, 6):

    if number == 3:
        continue

    print(number)
```

Output:

```text
1
2
4
5
```

### Difference

| Statement | Behaviour |
|---|---|
| `break` | Stops the entire loop |
| `continue` | Skips only the current iteration |

---

## 31. Enumerate()

The `enumerate()` function allows us to access both the index and the value while looping.

Example:

```python
employees = ["John", "Maria", "George"]

for index, employee in enumerate(employees):
    print(index, employee)
```

Output:

```text
0 John
1 Maria
2 George
```

You can start counting from 1:

```python
for index, employee in enumerate(employees, start=1):
    print(index, employee)
```

Output:

```text
1 John
2 Maria
3 George
```

This is useful when generating numbered reports.

---

# PART 5: PRACTICAL DATA ANALYST EXAMPLE

## 32. Employee Dataset

We will use one dataset throughout this section.

```python
employees = [
    {"name": "John", "department": "IT", "salary": 55000},
    {"name": "Maria", "department": "HR", "salary": 42000},
    {"name": "George", "department": "IT", "salary": 65000},
    {"name": "Anna", "department": "Finance", "salary": 60000},
    {"name": "Peter", "department": "Marketing", "salary": 38000},
    {"name": "Elena", "department": "HR", "salary": 45000},
    {"name": "David", "department": "Finance", "salary": 72000},
    {"name": "Sofia", "department": "IT", "salary": 50000}
]
```

### Dataset Description

| Column | Description | Data type |
|---|---|---|
| name | Employee name | String |
| department | Employee department | String |
| salary | Employee salary | Integer |

Number of employees: 8

Departments:
- IT
- HR
- Finance
- Marketing

### 32.1 Calculate Total Salary

```python
total_salary = 0

for employee in employees:
    total_salary += employee["salary"]

print(total_salary)
```

Expected result:

```text
427000
```

### 32.2 Calculate Average Salary

```python
total_salary = 0

for employee in employees:
    total_salary += employee["salary"]

average_salary = total_salary / len(employees)

print(average_salary)
```

Expected result:

```text
53375.0
```

### 32.3 Find the Highest-Paid Employee

```python
highest_paid = employees[0]

for employee in employees:

    if employee["salary"] > highest_paid["salary"]:
        highest_paid = employee

print(highest_paid)
```

Expected result:

```text
{'name': 'David', 'department': 'Finance', 'salary': 72000}
```

### 32.4 Find the Lowest-Paid Employee

```python
lowest_paid = employees[0]

for employee in employees:

    if employee["salary"] < lowest_paid["salary"]:
        lowest_paid = employee

print(lowest_paid)
```

Expected result:

```text
{'name': 'Peter', 'department': 'Marketing', 'salary': 38000}
```

### 32.5 Count Employees by Department

```python
department_counts = {}

for employee in employees:

    department = employee["department"]

    if department not in department_counts:
        department_counts[department] = 0

    department_counts[department] += 1

print(department_counts)
```

Expected result:

```python
{
    "IT": 3,
    "HR": 2,
    "Finance": 2,
    "Marketing": 1
}
```

### Explanation

1. We create an empty dictionary.
2. We loop through every employee.
3. We extract the department.
4. If the department does not exist, we initialize its count to zero.
5. We increase the count by one.

This is a common frequency-counting pattern.

### 32.6 Calculate Average Salary by Department

First, collect salaries by department.

```python
department_salaries = {}

for employee in employees:

    department = employee["department"]
    salary = employee["salary"]

    if department not in department_salaries:
        department_salaries[department] = []

    department_salaries[department].append(salary)
```

Then calculate averages:

```python
for department, salaries in department_salaries.items():

    average = sum(salaries) / len(salaries)

    print(department, round(average, 2))
```

Expected results:

| Department | Average salary |
|---|---:|
| IT | 56666.67 |
| HR | 43500 |
| Finance | 66000 |
| Marketing | 38000 |

### 32.7 Create a Salary Classification Function

```python
def classify_salary(salary):

    if salary > 60000:
        return "High"

    elif salary > 45000:
        return "Medium"

    else:
        return "Low"
```

### 32.8 Add a New Field to Each Employee

```python
for employee in employees:

    employee["salary_category"] = classify_salary(
        employee["salary"]
    )
```

Now each employee contains an additional field.

Example:

```python
{
    "name": "John",
    "department": "IT",
    "salary": 55000,
    "salary_category": "Medium"
}
```

### 32.9 Generate an Employee Report

```python
for employee in employees:

    print(
        employee["name"],
        employee["department"],
        employee["salary"],
        employee["salary_category"]
    )
```

This combines:
- Lists
- Dictionaries
- Functions
- Loops
- Conditions

These are the foundations of data-processing scripts.

---

# PART 6: COMMON MISTAKES

## 33. Common Python Mistakes

### Mistake 1: Incorrect List Index

Incorrect:

```python
employees = ["John", "Maria", "George"]

print(employees[3])
```

Why?

There are only three elements, with indexes 0, 1, and 2.

### Mistake 2: Confusing append() and extend()

Incorrect when you want separate elements:

```python
numbers = [1, 2, 3]

numbers.append([4, 5])
```

Result:

```python
[1, 2, 3, [4, 5]]
```

Use `extend()` if you want:

```python
[1, 2, 3, 4, 5]
```

### Mistake 3: Accessing a Missing Dictionary Key

Potentially unsafe:

```python
print(employee["email"])
```

Safer:

```python
print(employee.get("email"))
```

### Mistake 4: Forgetting return

Incorrect:

```python
def calculate_bonus(salary):
    bonus = salary * 0.10

result = calculate_bonus(50000)
```

The function returns `None`.

Correct:

```python
def calculate_bonus(salary):
    bonus = salary * 0.10
    return bonus
```

### Mistake 5: Confusing print() and return

`print()` displays information.

`return` sends a result back from the function.

Use `return` when the result needs to be used in another calculation.

### Mistake 6: Incorrect Condition Order

Incorrect:

```python
if salary > 45000:
    category = "Medium"

elif salary > 60000:
    category = "High"
```

A salary of 70000 is classified as Medium.

Correct:

```python
if salary > 60000:
    category = "High"

elif salary > 45000:
    category = "Medium"

else:
    category = "Low"
```

### Mistake 7: Infinite While Loop

Incorrect:

```python
number = 1

while number <= 5:
    print(number)
```

The variable never changes.

Correct:

```python
number = 1

while number <= 5:
    print(number)
    number += 1
```

### Mistake 8: Modifying a List While Iterating

Removing elements from a list while looping directly over that same list can cause elements to be skipped.

A safer approach is to build a new list:

```python
numbers = [1, 2, 3, 4, 5]

even_numbers = []

for number in numbers:

    if number % 2 == 0:
        even_numbers.append(number)

print(even_numbers)
```

Output:

```text
[2, 4]
```

---

# PART 7: PRACTICE EXERCISES

## 34. Exercise Instructions

Try to solve every exercise independently.

Use the employee dataset from Section 32 unless an exercise specifies otherwise.

Recommended approach:

1. Read the exercise carefully.
2. Identify the required data structure.
3. Decide whether a loop or function is needed.
4. Write the code.
5. Execute it.
6. Compare the output with the expected result where provided.
7. Explain what your code does.

Do not worry about solving everything perfectly on the first attempt.

Understanding the logic is more important than memorizing syntax.

---

## Part A: Lists

### Exercise 1 — Create a Salary List

Create a list containing the salaries of all eight employees.

Expected structure:

```python
[55000, 42000, 65000, 60000, 38000, 45000, 72000, 50000]
```

### Exercise 2 — Access List Elements

Print:
- The first salary.
- The last salary.
- The first three salaries.

### Exercise 3 — Add a Salary

Add `48000` to the salary list.

Print the updated list.

### Exercise 4 — Sort Salaries

Sort all salaries from highest to lowest.

### Exercise 5 — Salary Statistics

Calculate:
- Total salary.
- Average salary.
- Minimum salary.
- Maximum salary.
- Number of salaries.

Use built-in functions.

---

## Part B: Dictionaries

### Exercise 6 — Create an Employee Dictionary

Create a dictionary for the following employee:

| Key | Value |
|---|---|
| employee_id | 10 |
| name | Alex |
| department | IT |
| salary | 58000 |

### Exercise 7 — Access Dictionary Values

Print:
- Alex's name.
- Alex's salary.

### Exercise 8 — Update Dictionary

Change Alex's salary to `62000`.

Add a new key:

```python
"city": "Athens"
```

Print the updated dictionary.

### Exercise 9 — Safe Dictionary Access

Use `.get()` to retrieve the employee's email.

If the key does not exist, return:

```text
Not provided
```

### Exercise 10 — Department Counts

Using the main employee dataset, create a dictionary containing the number of employees in each department.

Expected result:

```python
{
    "IT": 3,
    "HR": 2,
    "Finance": 2,
    "Marketing": 1
}
```

---

## Part C: Functions

### Exercise 11 — Calculate Bonus

Create a function:

```python
calculate_bonus(salary)
```

The function should return 10% of the salary.

Example:

```python
calculate_bonus(50000)
```

Expected result:

```text
5000
```

### Exercise 12 — Salary Classification

Create a function:

```python
salary_category(salary)
```

Rules:

| Condition | Category |
|---|---|
| Salary > 60000 | High |
| Salary > 45000 and <= 60000 | Medium |
| Salary <= 45000 | Low |

Test the function with salaries of:

- 72000
- 55000
- 42000

### Exercise 13 — Calculate Average

Create a function:

```python
calculate_average(numbers)
```

The function should:
- Receive a list of numbers.
- Calculate the average.
- Return the result.
- Return `0` if the list is empty.

### Exercise 14 — Count Employees by Department

Create a function:

```python
count_department(employees, department)
```

The function should:
- Receive the employee dataset.
- Receive a department name.
- Count the employees in that department.
- Return the count.

Test with:

- IT
- HR
- Finance
- Marketing

---

## Part D: Loops

### Exercise 15 — Print Employee Names

Use a `for` loop to print the names of all employees.

### Exercise 16 — Filter High Salaries

Use a loop and an `if` statement to print employees earning more than 50000.

Print their names and salaries.

### Exercise 17 — Calculate Total Without sum()

Use a loop to calculate the total salary.

Do not use the built-in `sum()` function.

### Exercise 18 — Count IT Employees

Use a loop to count how many employees belong to IT.

### Exercise 19 — While Loop

Use a `while` loop to print numbers from 1 to 10.

### Exercise 20 — Enumerate Employees

Use `enumerate()` to print each employee's position, starting at 1, and their name.

Expected structure:

```text
1 John
2 Maria
3 George
...
```

---

## Part E: Mini Project

# Exercise 21 — Employee Analytics Report

### Project Objective

Build a small employee analytics program using everything covered in this lesson.

You are working as a Junior Data Analyst.

Your manager gives you the employee dataset and asks you to create a simple report.

### Dataset

```python
employees = [
    {"name": "John", "department": "IT", "salary": 55000},
    {"name": "Maria", "department": "HR", "salary": 42000},
    {"name": "George", "department": "IT", "salary": 65000},
    {"name": "Anna", "department": "Finance", "salary": 60000},
    {"name": "Peter", "department": "Marketing", "salary": 38000},
    {"name": "Elena", "department": "HR", "salary": 45000},
    {"name": "David", "department": "Finance", "salary": 72000},
    {"name": "Sofia", "department": "IT", "salary": 50000}
]
```

### Requirements

Your program must:

1. Calculate total salary.
2. Calculate average salary.
3. Find the highest-paid employee.
4. Find the lowest-paid employee.
5. Count employees by department.
6. Calculate average salary by department.
7. Create a function for salary classification.
8. Add a salary category to each employee.
9. Print a clean employee report.

### Expected Analytical Results

| Metric | Expected result |
|---|---:|
| Total employees | 8 |
| Total salary | 427000 |
| Average salary | 53375 |
| Highest salary | 72000 |
| Lowest salary | 38000 |

Department counts:

| Department | Employees |
|---|---:|
| IT | 3 |
| HR | 2 |
| Finance | 2 |
| Marketing | 1 |

Average salaries:

| Department | Average salary |
|---|---:|
| IT | 56666.67 |
| HR | 43500 |
| Finance | 66000 |
| Marketing | 38000 |

### Expected Skills

Your project should demonstrate:

- Lists for storing records.
- Dictionaries for organizing employee information.
- Functions for reusable calculations.
- Loops for processing employees.
- Conditions for filtering and classification.

**Important:** Try to write the program yourself. You can use built-in functions such as `sum()`, `len()`, `min()`, and `max()` where appropriate.

---

# PART 8: CHEAT SHEET

## 35. Python Lists Cheat Sheet

| Operation | Syntax |
|---|---|
| Create list | `items = [1, 2, 3]` |
| Access first element | `items[0]` |
| Access last element | `items[-1]` |
| Slice | `items[0:2]` |
| Add element | `items.append(4)` |
| Insert element | `items.insert(1, 4)` |
| Add multiple elements | `items.extend([4, 5])` |
| Remove by value | `items.remove(2)` |
| Remove last element | `items.pop()` |
| Clear list | `items.clear()` |
| Length | `len(items)` |
| Total | `sum(items)` |
| Minimum | `min(items)` |
| Maximum | `max(items)` |
| Sort | `items.sort()` |
| Create sorted copy | `sorted(items)` |
| Reverse | `items[::-1]` |
| Check membership | `2 in items` |

## 36. Python Dictionaries Cheat Sheet

| Operation | Syntax |
|---|---|
| Create dictionary | `person = {"name": "John"}` |
| Access value | `person["name"]` |
| Safe access | `person.get("name")` |
| Add value | `person["city"] = "Athens"` |
| Update value | `person["salary"] = 60000` |
| Update multiple values | `person.update({...})` |
| Remove key | `del person["name"]` |
| Remove and return | `person.pop("salary")` |
| Get keys | `person.keys()` |
| Get values | `person.values()` |
| Get pairs | `person.items()` |
| Clear dictionary | `person.clear()` |

## 37. Python Functions Cheat Sheet

| Concept | Syntax |
|---|---|
| Define function | `def my_function():` |
| Function parameter | `def greet(name):` |
| Call function | `greet("John")` |
| Return result | `return result` |
| Multiple parameters | `def total(price, quantity):` |
| Default parameter | `def greet(name, city="Athens"):` |
| Keyword argument | `greet(name="John")` |

## 38. Python Loops Cheat Sheet

| Concept | Syntax |
|---|---|
| For loop | `for item in items:` |
| While loop | `while condition:` |
| Range | `range(5)` |
| Start and end | `range(1, 6)` |
| Step | `range(1, 10, 2)` |
| Stop loop | `break` |
| Skip iteration | `continue` |
| Index and value | `enumerate(items)` |
| Loop through dictionary | `for key, value in data.items():` |

---

# PART 9: CONNECTION TO DATA ANALYSIS

## 39. How These Concepts Relate to SQL and Pandas

These Python fundamentals are closely connected to the SQL and Pandas concepts you will learn.

| Python concept | SQL equivalent / related idea | Pandas equivalent |
|---|---|---|
| List | Collection of values | Series |
| Dictionary | A record with named fields | Row / dictionary representation |
| List of dictionaries | Table-like records | DataFrame input |
| `if` statement | CASE WHEN | `where()` / conditional selection |
| `for` loop | Processing records conceptually | Often replaced by vectorized operations |
| Function | Reusable calculation | Custom function / `apply()` |
| Dictionary counting | GROUP BY COUNT conceptually | `groupby().size()` |

### Important Note

Python loops are useful for learning and for tasks that require record-by-record processing.

However, when working with large datasets in Pandas, vectorized operations and methods such as `groupby()` are often more efficient and readable than manually looping through every row.

The goal is not to replace SQL with Python loops. It is to understand the logic behind data processing so that you can choose the right tool for each task.

---

# PART 10: COMPLETION CHECKLIST

## 40. Lesson Completion

Before moving to the next lesson, make sure you can:

### Lists
- [ ] Create lists.
- [ ] Access elements using positive and negative indexes.
- [ ] Slice lists.
- [ ] Add, update, and remove elements.
- [ ] Sort lists.
- [ ] Calculate list statistics.

### Dictionaries
- [ ] Create dictionaries.
- [ ] Access values using keys.
- [ ] Use `.get()` safely.
- [ ] Add, update, and remove values.
- [ ] Loop through dictionary elements.
- [ ] Work with lists of dictionaries.

### Functions
- [ ] Define functions.
- [ ] Use parameters and arguments.
- [ ] Return values.
- [ ] Understand `print()` versus `return`.
- [ ] Use default parameters.
- [ ] Create analytical functions.

### Loops
- [ ] Use `for` loops.
- [ ] Use `while` loops.
- [ ] Use `range()`.
- [ ] Use `break` and `continue`.
- [ ] Use `enumerate()`.
- [ ] Process dictionaries inside loops.
- [ ] Calculate totals and counts using loops.

### Practical Work
- [ ] Complete Exercises 1–20.
- [ ] Complete the Employee Analytics Report.
- [ ] Understand every part of your solution.
- [ ] Be able to explain your code.

---

# 11. What Comes Next?

After mastering lists, dictionaries, functions, and loops, the next step is to become more comfortable working with real-world data.

Recommended Python learning path:

| Next topic | What you will learn |
|---|---|
| List comprehensions | Write shorter and cleaner list transformations |
| Lambda functions | Create small anonymous functions |
| CSV files | Read and write structured data |
| JSON | Work with API responses and nested data |
| Exception handling | Manage errors with `try` and `except` |
| NumPy | Perform numerical calculations efficiently |
| Pandas | Work with DataFrames and real datasets |
| Data cleaning | Handle missing values, duplicates, and inconsistent data |
| Data visualization | Create charts and explore patterns |

The next major milestone is learning how to work with external datasets and transform them into analysis-ready data.

---

## Final Note

The objective of this lesson is not simply to memorize Python syntax.

You should understand how to:
- Store information.
- Access and manipulate records.
- Automate repetitive operations.
- Build reusable calculations.
- Transform raw information into useful reports.

These are fundamental skills for becoming a Data Analyst.

**Practice consistently, write your own code, and focus on understanding the logic behind every operation.**
