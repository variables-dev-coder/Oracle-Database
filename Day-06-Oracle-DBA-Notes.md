# Day 6 – GROUP BY & HAVING

## Objective

Learn how to generate department-wise reports using aggregate functions and filter grouped data using the HAVING clause.

---

# GROUP BY

## Definition

`GROUP BY` is used to group rows that have the same values in one or more columns.

### Syntax

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

---

## Example Table

| EMP_ID | NAME  | DEPARTMENT | SALARY |
| ------ | ----- | ---------- | ------ |
| 101    | Rahul | IT         | 50000  |
| 102    | Amit  | HR         | 40000  |
| 103    | Priya | IT         | 60000  |
| 104    | Neha  | HR         | 45000  |
| 105    | Raj   | SALES      | 55000  |

---

# GROUP BY with COUNT

Count employees in each department.

```sql
SELECT DEPARTMENT,
       COUNT(*)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

# GROUP BY with SUM

Calculate total salary department-wise.

```sql
SELECT DEPARTMENT,
       SUM(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

# GROUP BY with AVG

Calculate average salary department-wise.

```sql
SELECT DEPARTMENT,
       AVG(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

# GROUP BY with MAX

Find highest salary in each department.

```sql
SELECT DEPARTMENT,
       MAX(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

# GROUP BY with MIN

Find lowest salary in each department.

```sql
SELECT DEPARTMENT,
       MIN(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

# Important GROUP BY Rule

Every selected column must either:

* Be included in GROUP BY
* Or be used inside an aggregate function

### Wrong

```sql
SELECT NAME, DEPARTMENT, SUM(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

Error:

```text
ORA-00979: not a GROUP BY expression
```

### Correct

```sql
SELECT DEPARTMENT,
       SUM(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

# HAVING Clause

## Definition

`HAVING` filters grouped data after the GROUP BY operation.

### Syntax

```sql
SELECT column_name,
       aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

---

# Example 1

Departments whose total salary exceeds 100000.

```sql
SELECT DEPARTMENT,
       SUM(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT
HAVING SUM(SALARY) > 100000;
```

---

# Example 2

Departments having more than 2 employees.

```sql
SELECT DEPARTMENT,
       COUNT(*)
FROM EMPLOYEES
GROUP BY DEPARTMENT
HAVING COUNT(*) > 2;
```

---

# WHERE vs HAVING

| WHERE                          | HAVING                      |
| ------------------------------ | --------------------------- |
| Filters rows                   | Filters groups              |
| Executes before GROUP BY       | Executes after GROUP BY     |
| Cannot use aggregate functions | Can use aggregate functions |

---

## WHERE + GROUP BY + HAVING

```sql
SELECT DEPARTMENT,
       SUM(SALARY)
FROM EMPLOYEES
WHERE SALARY > 40000
GROUP BY DEPARTMENT
HAVING SUM(SALARY) > 100000;
```

---

# SQL Execution Order

```text
1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY
```

---

# Practice Queries

## Count Employees Department-wise

```sql
SELECT DEPARTMENT,
       COUNT(*)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

## Total Salary Department-wise

```sql
SELECT DEPARTMENT,
       SUM(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

## Average Salary Department-wise

```sql
SELECT DEPARTMENT,
       AVG(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

## Highest Salary Department-wise

```sql
SELECT DEPARTMENT,
       MAX(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

## Lowest Salary Department-wise

```sql
SELECT DEPARTMENT,
       MIN(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

---

## Departments with Total Salary Greater Than 100000

```sql
SELECT DEPARTMENT,
       SUM(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT
HAVING SUM(SALARY) > 100000;
```

---

# Interview Questions

### 1. Why do we use GROUP BY?

To group rows having the same values and perform aggregate calculations.

### 2. Can GROUP BY be used without aggregate functions?

Yes.

```sql
SELECT DEPARTMENT
FROM EMPLOYEES
GROUP BY DEPARTMENT;
```

### 3. What is the difference between WHERE and HAVING?

* WHERE filters rows.
* HAVING filters groups.

### 4. Which executes first: WHERE or HAVING?

WHERE executes first.

### 5. Can aggregate functions be used in WHERE?

No.

Wrong:

```sql
SELECT *
FROM EMPLOYEES
WHERE SUM(SALARY) > 100000;
```

Correct:

```sql
SELECT DEPARTMENT,
       SUM(SALARY)
FROM EMPLOYEES
GROUP BY DEPARTMENT
HAVING SUM(SALARY) > 100000;
```

---

# Day 6 Summary

* GROUP BY
* COUNT()
* SUM()
* AVG()
* MAX()
* MIN()
* HAVING
* WHERE vs HAVING
* Department-wise Reports
* SQL Execution Order
* Interview Questions
