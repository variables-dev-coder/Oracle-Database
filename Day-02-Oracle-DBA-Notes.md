# Day 2 - SQL Commands (CREATE, INSERT, SELECT)

## Objective

Learn how to:

- Create a table
- Insert records into a table
- Retrieve records from a table

---

# 1. CREATE Command

## Definition

The CREATE command is used to create database objects such as:

- Tables
- Views
- Indexes
- Users

For now, we will create a table.

## Syntax

```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype
);
```

## Example

```sql
CREATE TABLE employee (
    emp_id NUMBER(5),
    emp_name VARCHAR2(50),
    department VARCHAR2(30),
    salary NUMBER(8,2)
);
```

## Explanation

| Column | Data Type | Description |
|----------|----------|----------|
| emp_id | NUMBER(5) | Employee ID |
| emp_name | VARCHAR2(50) | Employee Name |
| department | VARCHAR2(30) | Department Name |
| salary | NUMBER(8,2) | Employee Salary |

---

# 2. INSERT Command

## Definition

The INSERT command is used to add records (rows) into a table.

## Syntax

```sql
INSERT INTO table_name
VALUES (value1, value2, value3);
```

## Example

```sql
INSERT INTO employee
VALUES (101, 'Munna', 'IT', 7500);
```

## Result

One row is inserted into the employee table.

---

# Multiple Inserts

```sql
INSERT INTO employee VALUES (102,'Rahul','HR',5000);

INSERT INTO employee VALUES (103,'Amit','Finance',6000);

INSERT INTO employee VALUES (104,'Neha','IT',8000);
```

---

# 3. SELECT Command

## Definition

The SELECT command is used to retrieve data from a table.

## Syntax

```sql
SELECT * FROM table_name;
```

## Example

```sql
SELECT * FROM employee;
```

## Explanation

- SELECT = Retrieve data
- * = All columns
- FROM = Source table

---

# Selecting Specific Columns

## Example

```sql
SELECT emp_name, salary
FROM employee;
```

## Output

Displays only:

- Employee Name
- Salary

---

# SELECT with WHERE Condition

## Example

```sql
SELECT *
FROM employee
WHERE department = 'IT';
```

## Result

Displays only employees from the IT department.

---

# Practical Exercise

## Create Table

```sql
CREATE TABLE employee (
    emp_id NUMBER(5),
    emp_name VARCHAR2(50),
    department VARCHAR2(30),
    salary NUMBER(8,2)
);
```

---

## Insert 20 Records

```sql
INSERT INTO employee VALUES (101,'Munna','IT',7500);
INSERT INTO employee VALUES (102,'Rahul','HR',5000);
INSERT INTO employee VALUES (103,'Amit','Finance',6000);
INSERT INTO employee VALUES (104,'Neha','IT',8000);
INSERT INTO employee VALUES (105,'Priya','HR',5500);
INSERT INTO employee VALUES (106,'Rohit','IT',7200);
INSERT INTO employee VALUES (107,'Ankit','Finance',6800);
INSERT INTO employee VALUES (108,'Sneha','IT',8200);
INSERT INTO employee VALUES (109,'Vikas','HR',5100);
INSERT INTO employee VALUES (110,'Pooja','Finance',6300);
INSERT INTO employee VALUES (111,'Karan','IT',7700);
INSERT INTO employee VALUES (112,'Nisha','HR',5300);
INSERT INTO employee VALUES (113,'Arjun','IT',8100);
INSERT INTO employee VALUES (114,'Deepak','Finance',6900);
INSERT INTO employee VALUES (115,'Riya','HR',5400);
INSERT INTO employee VALUES (116,'Manish','IT',7900);
INSERT INTO employee VALUES (117,'Sanjay','Finance',6500);
INSERT INTO employee VALUES (118,'Asha','HR',5200);
INSERT INTO employee VALUES (119,'Vivek','IT',8300);
INSERT INTO employee VALUES (120,'Kavita','Finance',7000);
```

---

## Verify Data

```sql
SELECT * FROM employee;
```

---

# Command Categories

## DDL (Data Definition Language)

Commands used to define database structures.

Examples:

```sql
CREATE
ALTER
DROP
TRUNCATE
```

---

## DML (Data Manipulation Language)

Commands used to manipulate data.

Examples:

```sql
INSERT
UPDATE
DELETE
MERGE
```

---

## DQL (Data Query Language)

Commands used to retrieve data.

Examples:

```sql
SELECT
```

---

# Interview Questions

## Q1. What is CREATE?

CREATE is a DDL command used to create database objects.

---

## Q2. What is INSERT?

INSERT is a DML command used to add records into a table.

---

## Q3. What is SELECT?

SELECT is a DQL command used to retrieve data from a table.

---

## Q4. What does * mean in SELECT *?

The * symbol means all columns.

Example:

```sql
SELECT * FROM employee;
```

---

## Q5. Can INSERT be used before CREATE?

No.

First create the table.

Then insert data.

---

# Day 2 Summary

- CREATE creates tables.
- INSERT adds rows.
- SELECT retrieves rows.
- DDL defines structures.
- DML manipulates data.
- DQL retrieves data.

---

# Day 2 Completion Checklist

- [ ] Create Employee Table
- [ ] Insert 20 Records
- [ ] Execute SELECT *
- [ ] Execute SELECT Specific Columns
- [ ] Execute SELECT with WHERE
- [ ] Understand DDL, DML, DQL
- [ ] Practice Commands 5 Times
