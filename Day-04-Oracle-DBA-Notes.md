# Day 4 - SQL Data Filtering & Sorting

## Topics Covered

- ORDER BY
- DISTINCT
- LIKE
- BETWEEN
- IN

---

# 1. ORDER BY

## Purpose

Used to sort records in ascending or descending order.

## Syntax

```sql
SELECT * FROM employee
ORDER BY salary;
```

Default sorting is Ascending (ASC).

## Ascending Order

```sql
SELECT * FROM employee
ORDER BY salary ASC;
```

## Descending Order

```sql
SELECT * FROM employee
ORDER BY salary DESC;
```

## Example

Show highest-paid employees first:

```sql
SELECT * FROM employee
ORDER BY salary DESC;
```

---

# 2. DISTINCT

## Purpose

Removes duplicate values from the result set.

## Syntax

```sql
SELECT DISTINCT department
FROM employee;
```

## Example

Employee Table:

| EMP_ID | EMP_NAME | DEPARTMENT |
|---------|-----------|------------|
| 1 | Munna | IT |
| 2 | Rahul | IT |
| 3 | Priya | HR |

Query:

```sql
SELECT DISTINCT department
FROM employee;
```

Output:

```text
IT
HR
```

## Real-Time Use

Find all unique departments in a company.

---

# 3. LIKE

## Purpose

Search records using patterns.

## Wildcards

### % (Zero or More Characters)

Find names starting with M:

```sql
SELECT *
FROM employee
WHERE emp_name LIKE 'M%';
```

Examples:

```text
Munna
Manoj
Mahesh
```

---

Find names ending with a:

```sql
SELECT *
FROM employee
WHERE emp_name LIKE '%a';
```

---

Find names containing "un":

```sql
SELECT *
FROM employee
WHERE emp_name LIKE '%un%';
```

---

### _ (Single Character)

Find names with exactly 5 letters starting with R:

```sql
SELECT *
FROM employee
WHERE emp_name LIKE 'R____';
```

---

## Real-Time Use

Search employees by name.

---

# 4. BETWEEN

## Purpose

Filters records within a range.

## Syntax

```sql
SELECT *
FROM employee
WHERE salary BETWEEN 30000 AND 60000;
```

Equivalent to:

```sql
salary >= 30000
AND
salary <= 60000
```

## Example

```sql
SELECT *
FROM employee
WHERE emp_id BETWEEN 1 AND 10;
```

## Real-Time Use

Find employees within a salary range.

---

# 5. IN

## Purpose

Filters multiple values using a single condition.

## Syntax

```sql
SELECT *
FROM employee
WHERE department IN ('IT','HR');
```

Equivalent to:

```sql
department = 'IT'
OR
department = 'HR'
```

## Example

```sql
SELECT *
FROM employee
WHERE city IN ('Hyderabad','Mumbai');
```

## Real-Time Use

Filter employees from selected departments or cities.

---

# Combined Example

```sql
SELECT *
FROM employee
WHERE department IN ('IT','HR')
AND salary BETWEEN 30000 AND 80000
ORDER BY salary DESC;
```

## Meaning

1. Select employees from IT or HR.
2. Salary between 30,000 and 80,000.
3. Display highest salary first.

---

# Practice Tasks

## Task 1

Show all employees sorted by salary (low to high).

```sql
SELECT *
FROM employee
ORDER BY salary ASC;
```

---

## Task 2

Show all employees sorted by salary (high to low).

```sql
SELECT *
FROM employee
ORDER BY salary DESC;
```

---

## Task 3

Display unique departments.

```sql
SELECT DISTINCT department
FROM employee;
```

---

## Task 4

Find employees whose names start with 'A'.

```sql
SELECT *
FROM employee
WHERE emp_name LIKE 'A%';
```

---

## Task 5

Find employees whose names end with 'n'.

```sql
SELECT *
FROM employee
WHERE emp_name LIKE '%n';
```

---

## Task 6

Find employees whose names contain 'ra'.

```sql
SELECT *
FROM employee
WHERE emp_name LIKE '%ra%';
```

---

## Task 7

Find employees with salary between 25000 and 50000.

```sql
SELECT *
FROM employee
WHERE salary BETWEEN 25000 AND 50000;
```

---

## Task 8

Find employees with IDs between 5 and 15.

```sql
SELECT *
FROM employee
WHERE emp_id BETWEEN 5 AND 15;
```

---

## Task 9

Find employees from IT, HR, and Finance departments.

```sql
SELECT *
FROM employee
WHERE department IN ('IT','HR','Finance');
```

---

## Task 10

Find employees from Hyderabad and Mumbai.

```sql
SELECT *
FROM employee
WHERE city IN ('Hyderabad','Mumbai');
```

---

# Interview Questions

## Q1. Difference Between WHERE and ORDER BY?

### WHERE

Filters rows.

```sql
SELECT *
FROM employee
WHERE salary > 50000;
```

### ORDER BY

Sorts rows.

```sql
SELECT *
FROM employee
ORDER BY salary DESC;
```

---

## Q2. Difference Between DISTINCT and GROUP BY?

### DISTINCT

Removes duplicate values.

```sql
SELECT DISTINCT department
FROM employee;
```

### GROUP BY

Groups records for aggregate functions.

```sql
SELECT department, COUNT(*)
FROM employee
GROUP BY department;
```

---

## Q3. Difference Between BETWEEN and IN?

### BETWEEN

Works with ranges.

```sql
SELECT *
FROM employee
WHERE salary BETWEEN 10000 AND 50000;
```

### IN

Works with specific values.

```sql
SELECT *
FROM employee
WHERE department IN ('IT','HR');
```

---

# Day 4 Outcome

After completing Day 4, you can:

- Sort data using ORDER BY
- Remove duplicates using DISTINCT
- Search patterns using LIKE
- Filter ranges using BETWEEN
- Filter multiple values using IN
- Write real-world employee search queries

These commands are heavily used by SQL Developers, Oracle DBAs, and Application Developers for reporting and data analysis.
