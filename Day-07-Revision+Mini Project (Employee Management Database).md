# Day 7 - Revision + Mini Project (Employee Management Database)

## Objective

Revise all Oracle SQL concepts learned from Day 1 to Day 6 by building a mini Employee Management Database project.

---

# Topics Covered

- CREATE TABLE
- INSERT
- SELECT
- WHERE
- UPDATE
- DELETE
- ORDER BY
- DISTINCT
- LIKE
- BETWEEN
- IN
- COUNT
- SUM
- AVG
- MAX
- MIN
- GROUP BY
- HAVING

---

# Project: Employee Management Database

## Create Employee Table

```sql
CREATE TABLE EMPLOYEE (
    EMP_ID NUMBER PRIMARY KEY,
    EMP_NAME VARCHAR2(50),
    DEPARTMENT VARCHAR2(30),
    DESIGNATION VARCHAR2(30),
    SALARY NUMBER(10,2),
    CITY VARCHAR2(30),
    JOIN_DATE DATE
);
```

---

# Insert Records

```sql
INSERT INTO EMPLOYEE VALUES
(101,'Munna','IT','Developer',50000,'Hyderabad',
TO_DATE('10-01-2024','DD-MM-YYYY'));
```

Use multiple INSERT statements to populate the table.

---

# SELECT Statement

Retrieve all records:

```sql
SELECT * FROM EMPLOYEE;
```

Retrieve specific columns:

```sql
SELECT EMP_NAME, SALARY
FROM EMPLOYEE;
```

---

# WHERE Clause

Filter records based on conditions.

Example:

```sql
SELECT *
FROM EMPLOYEE
WHERE DEPARTMENT='IT';
```

---

# UPDATE Statement

Modify existing records.

Example:

```sql
UPDATE EMPLOYEE
SET SALARY = 55000
WHERE EMP_NAME='Munna';
```

Save changes:

```sql
COMMIT;
```

---

# DELETE Statement

Remove records from table.

Example:

```sql
DELETE FROM EMPLOYEE
WHERE EMP_ID=106;
```

```sql
COMMIT;
```

---

# ORDER BY

Sort records.

Ascending:

```sql
SELECT *
FROM EMPLOYEE
ORDER BY SALARY;
```

Descending:

```sql
SELECT *
FROM EMPLOYEE
ORDER BY SALARY DESC;
```

---

# DISTINCT

Remove duplicate values.

Example:

```sql
SELECT DISTINCT DEPARTMENT
FROM EMPLOYEE;
```

---

# LIKE Operator

Pattern matching.

Starts with A:

```sql
SELECT *
FROM EMPLOYEE
WHERE EMP_NAME LIKE 'A%';
```

Ends with A:

```sql
SELECT *
FROM EMPLOYEE
WHERE EMP_NAME LIKE '%A';
```

Contains A:

```sql
SELECT *
FROM EMPLOYEE
WHERE EMP_NAME LIKE '%A%';
```

---

# BETWEEN Operator

Filter within a range.

Example:

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY BETWEEN 50000 AND 70000;
```

---

# IN Operator

Check multiple values.

Example:

```sql
SELECT *
FROM EMPLOYEE
WHERE CITY IN ('Hyderabad','Mumbai');
```

---

# Aggregate Functions

## COUNT()

Counts rows.

```sql
SELECT COUNT(*)
FROM EMPLOYEE;
```

---

## SUM()

Calculates total.

```sql
SELECT SUM(SALARY)
FROM EMPLOYEE;
```

---

## AVG()

Calculates average.

```sql
SELECT AVG(SALARY)
FROM EMPLOYEE;
```

---

## MAX()

Find highest value.

```sql
SELECT MAX(SALARY)
FROM EMPLOYEE;
```

---

## MIN()

Find lowest value.

```sql
SELECT MIN(SALARY)
FROM EMPLOYEE;
```

---

# GROUP BY

Creates department-wise reports.

Example:

```sql
SELECT DEPARTMENT,
       COUNT(*) TOTAL_EMPLOYEES
FROM EMPLOYEE
GROUP BY DEPARTMENT;
```

Average salary by department:

```sql
SELECT DEPARTMENT,
       AVG(SALARY)
FROM EMPLOYEE
GROUP BY DEPARTMENT;
```

---

# HAVING Clause

Filters grouped data.

Example:

```sql
SELECT DEPARTMENT,
       AVG(SALARY)
FROM EMPLOYEE
GROUP BY DEPARTMENT
HAVING AVG(SALARY) > 55000;
```

---

# Mini Project Tasks

### Task 1

Display all employees.

```sql
SELECT * FROM EMPLOYEE;
```

### Task 2

Display employees from IT department.

```sql
SELECT * FROM EMPLOYEE
WHERE DEPARTMENT='IT';
```

### Task 3

Display employees earning more than 50000.

```sql
SELECT * FROM EMPLOYEE
WHERE SALARY > 50000;
```

### Task 4

Display employees from Hyderabad and Mumbai.

```sql
SELECT * FROM EMPLOYEE
WHERE CITY IN ('Hyderabad','Mumbai');
```

### Task 5

Display department-wise employee count.

```sql
SELECT DEPARTMENT,
       COUNT(*)
FROM EMPLOYEE
GROUP BY DEPARTMENT;
```

### Task 6

Display department-wise average salary.

```sql
SELECT DEPARTMENT,
       AVG(SALARY)
FROM EMPLOYEE
GROUP BY DEPARTMENT;
```

### Task 7

Display departments having average salary above 55000.

```sql
SELECT DEPARTMENT,
       AVG(SALARY)
FROM EMPLOYEE
GROUP BY DEPARTMENT
HAVING AVG(SALARY) > 55000;
```

---

# Interview Questions

### Q1: Difference between WHERE and HAVING?

WHERE filters rows before grouping.

HAVING filters groups after GROUP BY.

---

### Q2: Difference between DELETE and TRUNCATE?

DELETE removes selected rows.

TRUNCATE removes all rows and cannot be rolled back after commit.

---

### Q3: Difference between DISTINCT and GROUP BY?

DISTINCT removes duplicate values.

GROUP BY groups records for aggregate calculations.

---

### Q4: Which aggregate functions ignore NULL values?

- COUNT(column)
- SUM()
- AVG()
- MAX()
- MIN()

---

# Day 7 Summary

By completing Day 7, you can:

- Design tables
- Insert records
- Retrieve data
- Filter data
- Update data
- Delete data
- Sort records
- Generate reports
- Use aggregate functions
- Create department-wise summaries using GROUP BY and HAVING

This completes Week 1 Oracle SQL Fundamentals.
