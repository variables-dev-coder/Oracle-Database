# Oracle DBA Roadmap - Day 1

## Database & SQL Basics

---

# 1. What is DBMS?

DBMS (Database Management System) is software used to store, manage, retrieve, and manipulate data efficiently.

## Advantages

* Centralized data storage
* Fast data retrieval
* Security
* Backup and Recovery
* Reduced data redundancy

## Examples

* Oracle Database
* MySQL
* PostgreSQL
* SQL Server

---

# 2. What is RDBMS?

RDBMS (Relational Database Management System) stores data in tables and maintains relationships between tables.

## Features

* Data stored in rows and columns
* Supports Primary Keys
* Supports Foreign Keys
* Uses SQL language

## Example

### Student Table

| Student_ID | Student_Name |
| ---------- | ------------ |
| 1          | Munna        |
| 2          | Rahul        |

### Course Table

| Student_ID | Course |
| ---------- | ------ |
| 1          | Java   |
| 2          | Oracle |

Relationship is maintained using Student_ID.

---

# 3. Oracle Database Overview

Oracle Database is an Enterprise RDBMS used by:

* Banks
* Telecom Companies
* Government Organizations
* Large Enterprises

## Oracle Stores

* Tables
* Indexes
* Views
* Users
* Roles
* Data Files
* Redo Logs

---

# Oracle Architecture (Basic)

User
↓
SQL Query
↓
Oracle Instance
↓
Oracle Database

## Oracle Instance

Contains:

### Memory

* SGA (System Global Area)

### Background Processes

* DBWn
* LGWR
* SMON
* PMON

## Oracle Database

Contains:

* Data Files
* Control Files
* Redo Log Files

---

# Practical Commands

## Connect to Oracle

```sql
sqlplus / as sysdba
```

## Check Current User

```sql
SHOW USER;
```

---

# Create Student Table

```sql
CREATE TABLE student (
    student_id NUMBER(5),
    student_name VARCHAR2(50),
    city VARCHAR2(30)
);
```

## Check Structure

```sql
DESC student;
```

---

# Insert Records

```sql
INSERT INTO student
VALUES (1, 'Munna', 'Hyderabad');

INSERT INTO student
VALUES (2, 'Rahul', 'Mumbai');

COMMIT;
```

---

# View Records

```sql
SELECT * FROM student;
```

---

# Create Employee Table

```sql
CREATE TABLE employee (
    emp_id NUMBER(5),
    emp_name VARCHAR2(50),
    department VARCHAR2(30),
    salary NUMBER(8,2)
);
```

---

# Insert Records

```sql
INSERT INTO employee
VALUES (101, 'Munna', 'IT', 75000);

INSERT INTO employee
VALUES (102, 'Rahul', 'HR', 55000);

COMMIT;
```

---

# View Records

```sql
SELECT * FROM employee;
```

---

# SQL*Plus Formatting Commands

## Fix Column Display

```sql
COLUMN student_id FORMAT 99999
COLUMN student_name FORMAT A20
COLUMN city FORMAT A20
```

## Increase Line Size

```sql
SET LINESIZE 200
```

## Increase Page Size

```sql
SET PAGESIZE 100
```

---

# Commands Learned Today

```sql
CREATE TABLE
INSERT INTO
SELECT
DESC
COMMIT
SHOW USER
COLUMN
SET LINESIZE
SET PAGESIZE
```

---

# Interview Questions

## What is DBMS?

Software used to store and manage data.

## What is RDBMS?

A DBMS that stores data in related tables.

## Difference Between DBMS and RDBMS?

| DBMS             | RDBMS             |
| ---------------- | ----------------- |
| File/Table based | Table based       |
| No relation      | Supports relation |
| Less secure      | More secure       |

## What is Oracle Database?

An Enterprise Relational Database Management System.

## What is SQL?

Structured Query Language used to interact with databases.

---

# Day 1 Task

Create:

## Department Table

* department_id
* department_name
* location

## Product Table

* product_id
* product_name
* price

Insert at least 3 records into each table.

Verify:

```sql
SELECT * FROM department;

SELECT * FROM product;
```

---

# Day 1 Status

✅ DBMS

✅ RDBMS

✅ Oracle Overview

✅ SQLPlus Login

✅ Create Table

✅ Insert Data

✅ Select Data

✅ SQLPlus Formatting

Ready for Day 2: Data Types, Constraints & CRUD Operations.
