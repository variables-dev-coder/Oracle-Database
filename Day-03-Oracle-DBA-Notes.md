# Oracle SQL Day 3 Notes

## Topics Covered

* UPDATE
* DELETE
* WHERE Clause
* Company-Style Employee Modifications

---

# 1. UPDATE Statement

The UPDATE statement is used to modify existing records in a table.

## Syntax

```sql
UPDATE table_name
SET column_name = value
WHERE condition;
```

## Example

```sql
UPDATE EMPLOYEE
SET SALARY = 35000
WHERE EMP_ID = 101;
```

### Before Update

| EMP_ID | NAME  | SALARY |
| ------ | ----- | ------ |
| 101    | Munna | 30000  |
| 102    | Rahul | 40000  |

### After Update

| EMP_ID | NAME  | SALARY |
| ------ | ----- | ------ |
| 101    | Munna | 35000  |
| 102    | Rahul | 40000  |

---

# 2. DELETE Statement

The DELETE statement is used to remove records from a table.

## Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

## Example

```sql
DELETE FROM EMPLOYEE
WHERE EMP_ID = 102;
```

### Result

Employee with EMP_ID 102 is removed from the table.

---

# 3. WHERE Clause

The WHERE clause is used to filter specific rows.

## Syntax

```sql
SELECT *
FROM table_name
WHERE condition;
```

## Example

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY > 30000;
```

This query displays employees whose salary is greater than 30000.

---

# Why WHERE is Important

## Dangerous UPDATE

```sql
UPDATE EMPLOYEE
SET SALARY = 50000;
```

### Result

All employee salaries become 50000.

---

## Dangerous DELETE

```sql
DELETE FROM EMPLOYEE;
```

### Result

All employee records are deleted.

---

Always use WHERE when modifying specific rows.

Example:

```sql
WHERE EMP_ID = 101
```

or

```sql
WHERE NAME = 'Munna'
```

---

# Comparison Operators

## Equal To (=)

```sql
SELECT *
FROM EMPLOYEE
WHERE EMP_ID = 101;
```

---

## Not Equal To (<>)

```sql
SELECT *
FROM EMPLOYEE
WHERE EMP_ID <> 101;
```

---

## Greater Than (>)

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY > 30000;
```

---

## Less Than (<)

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY < 30000;
```

---

## Greater Than or Equal To (>=)

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY >= 30000;
```

---

## Less Than or Equal To (<=)

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY <= 30000;
```

---

# Company-Style Examples

## Update Employee Salary

```sql
UPDATE EMPLOYEE
SET SALARY = 45000
WHERE EMP_ID = 105;
```

---

## Update Employee City

```sql
UPDATE EMPLOYEE
SET CITY = 'Hyderabad'
WHERE EMP_ID = 105;
```

---

## Update Multiple Columns

```sql
UPDATE EMPLOYEE
SET SALARY = 50000,
    CITY = 'Mumbai'
WHERE EMP_ID = 105;
```

---

## Delete Resigned Employee

```sql
DELETE FROM EMPLOYEE
WHERE EMP_ID = 110;
```

---

## Find Employees from Mumbai

```sql
SELECT *
FROM EMPLOYEE
WHERE CITY = 'Mumbai';
```

---

## Find Employees with High Salary

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY > 50000;
```

---

# Day 3 Practice Tasks

## Task 1

Update salary of employee 1.

```sql
UPDATE EMPLOYEE
SET SALARY = 35000
WHERE EMP_ID = 1;
```

---

## Task 2

Update city of employee 2.

```sql
UPDATE EMPLOYEE
SET CITY = 'Pune'
WHERE EMP_ID = 2;
```

---

## Task 3

Delete employee 5.

```sql
DELETE FROM EMPLOYEE
WHERE EMP_ID = 5;
```

---

## Task 4

Show employees from Hyderabad.

```sql
SELECT *
FROM EMPLOYEE
WHERE CITY = 'Hyderabad';
```

---

## Task 5

Show employees whose salary is greater than 40000.

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY > 40000;
```

---

# Interview Questions

## Q1. Difference Between DELETE and DROP?

| DELETE                  | DROP                       |
| ----------------------- | -------------------------- |
| Removes rows            | Removes entire table       |
| WHERE can be used       | WHERE cannot be used       |
| Table structure remains | Table structure is removed |

---

## Q2. What Happens If WHERE Is Omitted in UPDATE?

All rows are updated.

Example:

```sql
UPDATE EMPLOYEE
SET SALARY = 50000;
```

---

## Q3. What Happens If WHERE Is Omitted in DELETE?

All rows are deleted.

Example:

```sql
DELETE FROM EMPLOYEE;
```

---

## Q4. Can UPDATE Modify Multiple Columns?

Yes.

```sql
UPDATE EMPLOYEE
SET SALARY = 50000,
    CITY = 'Mumbai'
WHERE EMP_ID = 1;
```

---

# Day 3 Summary

✅ UPDATE Statement

✅ DELETE Statement

✅ WHERE Clause

✅ Comparison Operators

✅ Employee Data Modification

✅ Employee Data Deletion

✅ Company-Style SQL Operations

✅ Interview Questions

---

# Real Project Usage

UPDATE is used for:

* Salary revisions
* Address changes
* Status updates
* Employee profile modifications

DELETE is used for:

* Removing duplicate records
* Removing resigned employees
* Cleaning unnecessary data

WHERE is used in almost every SQL query to filter required records.
