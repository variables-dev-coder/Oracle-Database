# Day 10 — Views & Sequences (Oracle DBA)

## Objective

Learn:

* Views
* Sequences

Practice:

* Auto-generated Employee IDs

---

# 1. Views

## What is a View?

A View is a **virtual table** created from a SQL query.

* Does not store data physically.
* Displays data from one or more tables.
* Used for security and simplification.

---

## Create View

```sql
CREATE VIEW employee_public_view AS
SELECT emp_id,
       emp_name,
       department,
       designation
FROM employee;
```

---

## Query View

```sql
SELECT * FROM employee_public_view;
```

---

## Filter Data from View

```sql
SELECT *
FROM employee_public_view
WHERE department = 'IT';
```

---

## Replace Existing View

```sql
CREATE OR REPLACE VIEW employee_public_view AS
SELECT emp_id,
       emp_name,
       department,
       designation
FROM employee;
```

---

## Drop View

```sql
DROP VIEW employee_public_view;
```

---

## Advantages of Views

### Security

Hide sensitive columns like:

* Salary
* Password
* Bank Details

### Simplicity

Complex queries can be saved as a view.

### Data Abstraction

Users do not need to know table structure.

---

# 2. Sequences

## What is a Sequence?

A Sequence is a database object used to generate unique numbers automatically.

Common Uses:

* Employee IDs
* Customer IDs
* Order IDs
* Invoice Numbers

---

## Create Sequence

```sql
CREATE SEQUENCE emp_auto_seq
START WITH 1001
INCREMENT BY 1;
```

---

## Generate Next Value

```sql
SELECT emp_auto_seq.NEXTVAL
FROM dual;
```

Example Output:

```
1001
1002
1003
```

---

## Current Value

```sql
SELECT emp_auto_seq.CURRVAL
FROM dual;
```

---

## NEXTVAL vs CURRVAL

| NEXTVAL               | CURRVAL              |
| --------------------- | -------------------- |
| Generates next number | Shows current number |
| Increases sequence    | Does not increase    |
| Used before insert    | Shows latest value   |

---

# Using Sequence in INSERT

## Create Table

```sql
CREATE TABLE employee_auto (
    emp_id NUMBER(5),
    emp_name VARCHAR2(50),
    department VARCHAR2(30)
);
```

---

## Insert Records

```sql
INSERT INTO employee_auto
VALUES (emp_auto_seq.NEXTVAL, 'Munna', 'IT');

INSERT INTO employee_auto
VALUES (emp_auto_seq.NEXTVAL, 'Rahul', 'HR');

INSERT INTO employee_auto
VALUES (emp_auto_seq.NEXTVAL, 'Amit', 'Finance');
```

---

## Commit

```sql
COMMIT;
```

---

## Verify Data

```sql
SELECT * FROM employee_auto;
```

Example Output:

| EMP_ID | EMP_NAME | DEPARTMENT |
| ------ | -------- | ---------- |
| 1002   | Munna    | IT         |
| 1003   | Rahul    | HR         |
| 1004   | Amit     | Finance    |

---

# Common Errors

## ORA-00903: Invalid Table Name

Wrong:

```sql
SELECT * FROM;
```

Correct:

```sql
SELECT * FROM employee;
```

---

## ORA-00904: Invalid Identifier

Wrong:

```sql
SELECT city
FROM employee;
```

Reason:

`CITY` column does not exist.

Check table structure:

```sql
DESC employee;
```

---

# Practice Task

## Create Student Table

```sql
CREATE TABLE student (
    student_id NUMBER(5),
    student_name VARCHAR2(50),
    course VARCHAR2(30),
    fees NUMBER(8,2)
);
```

---

## Create Sequence

```sql
CREATE SEQUENCE student_seq
START WITH 501
INCREMENT BY 1;
```

---

## Insert Students

```sql
INSERT INTO student
VALUES (student_seq.NEXTVAL, 'Munna', 'Java', 15000);

INSERT INTO student
VALUES (student_seq.NEXTVAL, 'Rahul', 'Python', 12000);

INSERT INTO student
VALUES (student_seq.NEXTVAL, 'Amit', 'Oracle DBA', 18000);

INSERT INTO student
VALUES (student_seq.NEXTVAL, 'Priya', 'Testing', 10000);

INSERT INTO student
VALUES (student_seq.NEXTVAL, 'Neha', 'DevOps', 20000);
```

---

## Create View

```sql
CREATE VIEW student_public_view AS
SELECT student_id,
       student_name,
       course
FROM student;
```

---

## Verify View

```sql
SELECT * FROM student_public_view;
```

---

# Interview Questions

### What is a View?

A View is a virtual table based on a SQL query.

### What is a Sequence?

A Sequence generates unique numbers automatically.

### Difference Between Table and View?

| Table           | View            |
| --------------- | --------------- |
| Stores data     | Virtual table   |
| Physical object | Logical object  |
| Uses storage    | Minimal storage |

### Why Use Sequences?

To generate unique IDs automatically and avoid duplicate values.

---

# Day 10 Summary

✅ View = Virtual Table

✅ Create, Replace, Query, Drop View

✅ Sequence = Auto Number Generator

✅ NEXTVAL generates next number

✅ CURRVAL shows current number

✅ Sequences are commonly used for Primary Keys and Employee IDs
