# Day 5 - SQL Aggregate Functions

## Objective

Learn how to generate summary reports using Aggregate Functions.

Aggregate Functions perform calculations on multiple rows and return a single result.

---

# Aggregate Functions

## 1. COUNT()

### Purpose

Counts the number of rows.

### Syntax

```sql
SELECT COUNT(*) FROM employees;
```

### Example

```sql
SELECT COUNT(*) FROM employees;
```

### Output

```text
COUNT(*)
--------
10
```

### Real-Time Use Cases

* Total employees
* Total customers
* Total orders
* Total products

---

## 2. SUM()

### Purpose

Calculates the total of a numeric column.

### Syntax

```sql
SELECT SUM(salary) FROM employees;
```

### Example

```sql
SELECT SUM(salary) FROM employees;
```

### Output

```text
SUM(SALARY)
-----------
450000
```

### Real-Time Use Cases

* Total salary expense
* Total sales amount
* Total revenue
* Total profit

---

## 3. AVG()

### Purpose

Calculates the average value.

### Syntax

```sql
SELECT AVG(salary) FROM employees;
```

### Example

```sql
SELECT AVG(salary) FROM employees;
```

### Output

```text
AVG(SALARY)
-----------
45000
```

### Real-Time Use Cases

* Average salary
* Average marks
* Average order value
* Average product price

---

## 4. MAX()

### Purpose

Finds the highest value.

### Syntax

```sql
SELECT MAX(salary) FROM employees;
```

### Example

```sql
SELECT MAX(salary) FROM employees;
```

### Output

```text
MAX(SALARY)
-----------
90000
```

### Real-Time Use Cases

* Highest salary
* Highest marks
* Maximum sales amount
* Most expensive product

---

## 5. MIN()

### Purpose

Finds the lowest value.

### Syntax

```sql
SELECT MIN(salary) FROM employees;
```

### Example

```sql
SELECT MIN(salary) FROM employees;
```

### Output

```text
MIN(SALARY)
-----------
25000
```

### Real-Time Use Cases

* Lowest salary
* Lowest marks
* Minimum sales amount
* Cheapest product

---

# Using Multiple Aggregate Functions

```sql
SELECT
COUNT(*) AS TOTAL_EMPLOYEES,
SUM(salary) AS TOTAL_SALARY,
AVG(salary) AS AVERAGE_SALARY,
MAX(salary) AS HIGHEST_SALARY,
MIN(salary) AS LOWEST_SALARY
FROM employees;
```

### Sample Output

```text
TOTAL_EMPLOYEES  TOTAL_SALARY  AVERAGE_SALARY  HIGHEST_SALARY  LOWEST_SALARY
---------------  ------------  --------------  --------------  -------------
10               450000        45000           90000           25000
```

---

# Practice Queries

## Count Total Employees

```sql
SELECT COUNT(*) FROM employees;
```

## Find Total Salary

```sql
SELECT SUM(salary) FROM employees;
```

## Find Average Salary

```sql
SELECT AVG(salary) FROM employees;
```

## Find Highest Salary

```sql
SELECT MAX(salary) FROM employees;
```

## Find Lowest Salary

```sql
SELECT MIN(salary) FROM employees;
```

## Generate Complete Salary Report

```sql
SELECT
COUNT(*) AS TOTAL_EMPLOYEES,
SUM(salary) AS TOTAL_SALARY,
AVG(salary) AS AVERAGE_SALARY,
MAX(salary) AS HIGHEST_SALARY,
MIN(salary) AS LOWEST_SALARY
FROM employees;
```

---

# Interview Questions

## Q1. What are Aggregate Functions?

Aggregate Functions perform calculations on multiple rows and return a single value.

Examples:

* COUNT()
* SUM()
* AVG()
* MAX()
* MIN()

---

## Q2. Difference between COUNT(*) and COUNT(column_name)?

### COUNT(*)

Counts all rows.

```sql
SELECT COUNT(*) FROM employees;
```

### COUNT(column_name)

Counts only non-NULL values.

```sql
SELECT COUNT(salary) FROM employees;
```

---

## Q3. Which function returns the highest value?

```sql
MAX()
```

---

## Q4. Which function returns the lowest value?

```sql
MIN()
```

---

## Q5. Can multiple aggregate functions be used together?

Yes.

```sql
SELECT COUNT(*), SUM(salary), AVG(salary)
FROM employees;
```

---

# Day 5 Summary

Today you learned:

* COUNT()
* SUM()
* AVG()
* MAX()
* MIN()
* Salary Report Queries

These functions are heavily used in reporting, analytics, DBA tasks, and real-world business applications.
