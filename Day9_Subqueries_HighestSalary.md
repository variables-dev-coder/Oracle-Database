# Day 9 - SQL Subqueries & Highest Salary Employee Problems

## What is a Subquery?

A subquery is a query written inside another query.

```sql
SELECT *
FROM employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM employees
);
```

### Execution Flow

1. Inner query executes first:

```sql
SELECT AVG(salary)
FROM employees;
```

Suppose result = `50000`

2. Outer query becomes:

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

---

# Types of Subqueries

## 1. Scalar Subquery

Returns exactly one value.

```sql
SELECT name
FROM employees
WHERE salary =
(
    SELECT MAX(salary)
    FROM employees
);
```

---

## 2. Multiple Row Subquery

Returns multiple rows.

```sql
SELECT *
FROM employees
WHERE dept_id IN
(
    SELECT dept_id
    FROM departments
    WHERE location = 'Mumbai'
);
```

---

## 3. Correlated Subquery

Inner query depends on the outer query.

```sql
SELECT e1.*
FROM employees e1
WHERE salary >
(
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.dept_id = e1.dept_id
);
```

Meaning:

> Find employees earning above the average salary of their department.

---

# Operators Used with Subqueries

## IN

```sql
SELECT *
FROM employees
WHERE dept_id IN
(
    SELECT dept_id
    FROM departments
);
```

---

## EXISTS

```sql
SELECT *
FROM departments d
WHERE EXISTS
(
    SELECT 1
    FROM employees e
    WHERE e.dept_id = d.dept_id
);
```

---

## ANY

```sql
SELECT *
FROM employees
WHERE salary > ANY
(
    SELECT salary
    FROM employees
    WHERE dept_id = 10
);
```

---

## ALL

```sql
SELECT *
FROM employees
WHERE salary > ALL
(
    SELECT salary
    FROM employees
    WHERE dept_id = 10
);
```

---

# Sample Employee Table

| emp_id | name  | dept | salary |
|---------|--------|------|--------|
| 1 | John  | IT | 50000 |
| 2 | Alice | IT | 70000 |
| 3 | Bob   | HR | 60000 |
| 4 | Sarah | HR | 80000 |
| 5 | Tom   | IT | 70000 |

---

# Highest Salary Employee Problems

## Problem 1: Employee(s) with Highest Salary

```sql
SELECT *
FROM employees
WHERE salary =
(
    SELECT MAX(salary)
    FROM employees
);
```

---

## Problem 2: Second Highest Salary

```sql
SELECT MAX(salary)
FROM employees
WHERE salary <
(
    SELECT MAX(salary)
    FROM employees
);
```

---

## Problem 3: Employee(s) with Second Highest Salary

```sql
SELECT *
FROM employees
WHERE salary =
(
    SELECT MAX(salary)
    FROM employees
    WHERE salary <
    (
        SELECT MAX(salary)
        FROM employees
    )
);
```

---

## Problem 4: Third Highest Salary

```sql
SELECT MAX(salary)
FROM employees
WHERE salary <
(
    SELECT MAX(salary)
    FROM employees
    WHERE salary <
    (
        SELECT MAX(salary)
        FROM employees
    )
);
```

---

## Problem 5: Nth Highest Salary Using LIMIT

### Second Highest

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

### Third Highest

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 2;
```

### Nth Highest

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET N-1;
```

---

## Problem 6: Highest Salary in Each Department

### Correlated Subquery

```sql
SELECT *
FROM employees e1
WHERE salary =
(
    SELECT MAX(e2.salary)
    FROM employees e2
    WHERE e1.dept = e2.dept
);
```

---

## Problem 7: Second Highest Salary in Each Department

```sql
SELECT *
FROM employees e1
WHERE salary =
(
    SELECT MAX(e2.salary)
    FROM employees e2
    WHERE e2.dept = e1.dept
      AND e2.salary <
      (
          SELECT MAX(e3.salary)
          FROM employees e3
          WHERE e3.dept = e1.dept
      )
);
```

---

## Problem 8: Employees Earning More Than Company Average

```sql
SELECT *
FROM employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM employees
);
```

---

## Problem 9: Employees Earning More Than Department Average

```sql
SELECT *
FROM employees e1
WHERE salary >
(
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.dept = e1.dept
);
```

---

## Problem 10: Department with Highest Average Salary

```sql
SELECT dept
FROM employees
GROUP BY dept
HAVING AVG(salary) =
(
    SELECT MAX(avg_sal)
    FROM
    (
        SELECT AVG(salary) avg_sal
        FROM employees
        GROUP BY dept
    ) t
);
```

---

# Modern Solution Using Window Functions

## Second Highest Salary

```sql
SELECT *
FROM
(
    SELECT *,
           DENSE_RANK() OVER(ORDER BY salary DESC) rnk
    FROM employees
) t
WHERE rnk = 2;
```

### Why DENSE_RANK?

Salaries:

```
80000
70000
70000
60000
50000
```

Ranks:

```
1
2
2
3
4
```

---

## Highest Salary in Each Department

```sql
SELECT *
FROM
(
    SELECT *,
           DENSE_RANK() OVER(
               PARTITION BY dept
               ORDER BY salary DESC
           ) rnk
    FROM employees
) t
WHERE rnk = 1;
```

---

## Second Highest Salary in Each Department

```sql
SELECT *
FROM
(
    SELECT *,
           DENSE_RANK() OVER(
               PARTITION BY dept
               ORDER BY salary DESC
           ) rnk
    FROM employees
) t
WHERE rnk = 2;
```

---

# Interview Cheatsheet

| Question | Recommended Approach |
|-----------|---------------------|
| Highest Salary | MAX() |
| Highest Salary Employee | MAX() + Subquery |
| Second Highest Salary | MAX() + Subquery |
| Nth Highest Salary | DENSE_RANK() |
| Highest Salary per Department | DENSE_RANK() |
| Second Highest per Department | DENSE_RANK() |
| Above Company Average | AVG() Subquery |
| Above Department Average | Correlated Subquery |

---

# Most Asked Interview Questions

1. Employee with highest salary.
2. Second highest salary.
3. Nth highest salary.
4. Highest salary in each department.
5. Second highest salary in each department.
6. Employees earning above company average.
7. Employees earning above department average.
8. Departments with highest average salary.

---

# Day 9 Practice Tasks

Solve without looking at notes:

- [ ] Highest salary employee
- [ ] Second highest salary
- [ ] Third highest salary
- [ ] Nth highest salary
- [ ] Highest salary in each department
- [ ] Second highest salary in each department
- [ ] Employees above company average
- [ ] Employees above department average
- [ ] Department with highest average salary
