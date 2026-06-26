# Day 20 – Oracle DBA Revision Notes

## SQL + Linux + Shell Scripting Revision

---

# Oracle DBA Roadmap

## Week 3 – Day 20 (Revision Day)

### Objectives

By the end of this revision, you should be able to:

* Write SQL queries confidently
* Work comfortably in Linux
* Create and execute basic shell scripts
* Answer common Oracle DBA interview questions

---

# Part 1: SQL Revision

## What is SQL?

SQL (Structured Query Language) is the standard language used to communicate with relational databases.

As an Oracle DBA, SQL is used for:

* Creating databases and tables
* Managing data
* Querying information
* Controlling user access
* Managing transactions

---

# SQL Command Categories

## 1. DDL (Data Definition Language)

Used to define or modify database structures.

| Command  | Purpose                    |
| -------- | -------------------------- |
| CREATE   | Create database objects    |
| ALTER    | Modify existing objects    |
| DROP     | Delete objects permanently |
| TRUNCATE | Remove all rows quickly    |
| RENAME   | Rename an object           |

Example:

```sql
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(100),
    salary NUMBER(10,2)
);
```

---

## 2. DML (Data Manipulation Language)

Used to manipulate data.

| Command | Purpose          |
| ------- | ---------------- |
| INSERT  | Add new records  |
| UPDATE  | Modify records   |
| DELETE  | Delete records   |
| MERGE   | Insert or update |

Example:

```sql
INSERT INTO employees
VALUES (101,'Munna',50000);
```

---

## 3. DQL (Data Query Language)

Retrieve data.

```sql
SELECT * FROM employees;
```

---

## 4. TCL (Transaction Control Language)

Controls transactions.

Commands

```sql
COMMIT
ROLLBACK
SAVEPOINT
```

Example

```sql
UPDATE employees
SET salary=60000
WHERE emp_id=101;

ROLLBACK;
```

---

## 5. DCL (Data Control Language)

Database security.

```sql
GRANT
REVOKE
```

Example

```sql
GRANT SELECT ON employees TO user1;
```

---

# Oracle Data Types

| Data Type | Description            |
| --------- | ---------------------- |
| NUMBER    | Integer/Decimal values |
| VARCHAR2  | Variable-length text   |
| CHAR      | Fixed-length text      |
| DATE      | Date values            |
| TIMESTAMP | Date & Time            |
| CLOB      | Large text             |
| BLOB      | Binary data            |

Example

```sql
salary NUMBER(10,2)
name VARCHAR2(100)
gender CHAR(1)
joining_date DATE
```

---

# Constraints

Used to maintain data integrity.

| Constraint  | Purpose                     |
| ----------- | --------------------------- |
| PRIMARY KEY | Unique identifier           |
| FOREIGN KEY | Relationship between tables |
| NOT NULL    | Cannot be empty             |
| UNIQUE      | Prevent duplicates          |
| CHECK       | Restrict values             |
| DEFAULT     | Assign default value        |

Example

```sql
CREATE TABLE employees(
emp_id NUMBER PRIMARY KEY,
name VARCHAR2(100) NOT NULL,
email VARCHAR2(100) UNIQUE,
salary NUMBER CHECK(salary>0)
);
```

---

# CRUD Operations

## Create

```sql
INSERT INTO employees VALUES (...);
```

## Read

```sql
SELECT * FROM employees;
```

## Update

```sql
UPDATE employees
SET salary=70000
WHERE emp_id=101;
```

## Delete

```sql
DELETE FROM employees
WHERE emp_id=101;
```

---

# SELECT Statement

Retrieve all records

```sql
SELECT * FROM employees;
```

Retrieve selected columns

```sql
SELECT emp_name,salary
FROM employees;
```

---

# WHERE Clause

```sql
SELECT *
FROM employees
WHERE salary>50000;
```

---

# BETWEEN

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 30000 AND 60000;
```

---

# IN Operator

```sql
SELECT *
FROM employees
WHERE department_id IN (10,20,30);
```

---

# LIKE Operator

| Symbol | Meaning             |
| ------ | ------------------- |
| %      | Multiple characters |
| _      | Single character    |

Example

```sql
name LIKE 'A%'
name LIKE '_a%'
```

---

# ORDER BY

Ascending

```sql
ORDER BY salary;
```

Descending

```sql
ORDER BY salary DESC;
```

---

# DISTINCT

```sql
SELECT DISTINCT department_id
FROM employees;
```

---

# Aggregate Functions

```sql
COUNT()

SUM()

AVG()

MAX()

MIN()
```

Example

```sql
SELECT AVG(salary)
FROM employees;
```

---

# GROUP BY

```sql
SELECT department_id,
AVG(salary)
FROM employees
GROUP BY department_id;
```

---

# HAVING

```sql
SELECT department_id,
AVG(salary)
FROM employees
GROUP BY department_id
HAVING AVG(salary)>50000;
```

---

# Joins

## INNER JOIN

Returns matching records.

```sql
SELECT *
FROM employees e
INNER JOIN departments d
ON e.department_id=d.department_id;
```

---

## LEFT JOIN

Returns all rows from left table.

```sql
LEFT JOIN
```

---

## RIGHT JOIN

Returns all rows from right table.

```sql
RIGHT JOIN
```

---

## FULL OUTER JOIN

Returns all rows from both tables.

```sql
FULL OUTER JOIN
```

---

## SELF JOIN

Join a table with itself.

Example

Employee → Manager

---

# Subqueries

Highest salary employee

```sql
SELECT *
FROM employees
WHERE salary=
(
SELECT MAX(salary)
FROM employees
);
```

---

# Views

Virtual table.

Create

```sql
CREATE VIEW employee_view AS
SELECT emp_id,name,salary
FROM employees;
```

Delete

```sql
DROP VIEW employee_view;
```

---

# Sequences

Auto-increment values.

```sql
CREATE SEQUENCE emp_seq
START WITH 1
INCREMENT BY 1;
```

Use

```sql
emp_seq.NEXTVAL
```

---

# SQL Interview Questions

* DDL vs DML
* DELETE vs TRUNCATE vs DROP
* CHAR vs VARCHAR2
* WHERE vs HAVING
* UNION vs UNION ALL
* PRIMARY KEY vs UNIQUE
* VIEW vs TABLE
* Sequence vs Identity
* GROUP BY purpose
* Types of JOINs

---

# Part 2: Linux Revision

## Navigation Commands

```bash
pwd
ls
ls -l
ls -a
cd
mkdir
rmdir
```

---

# File Commands

```bash
touch
cp
mv
rm
cat
nano
less
head
tail
```

---

# Search Commands

```bash
find
grep
locate
```

Examples

```bash
find . -name "*.log"

grep ERROR app.log
```

---

# Linux Permissions

Display permissions

```bash
ls -l
```

Example

```text
-rwxr-xr-x
```

Meaning

```
Owner
Group
Others
```

Permission Values

| Permission | Value |
| ---------- | ----- |
| Read       | 4     |
| Write      | 2     |
| Execute    | 1     |

Examples

```bash
chmod 755 file.sh

chmod 644 notes.txt

chmod +x script.sh
```

Owner

```bash
chown oracle:dba file.sh
```

---

# User Commands

```bash
whoami
id
sudo
su
```

---

# Process Management

```bash
ps

ps -ef

top

kill PID

kill -9 PID
```

---

# Memory Monitoring

```bash
free -m
```

---

# Disk Monitoring

```bash
df -h

du -sh folder_name
```

Difference

* df → Filesystem usage
* du → Folder/File size

---

# Networking

```bash
ping google.com

ip addr

ss -tuln
```

---

# Log Monitoring

```bash
tail -f app.log
```

Used to monitor log files in real time.

---

# Linux Interview Questions

* cp vs mv
* rm vs rmdir
* grep vs find
* df vs du
* head vs tail
* chmod vs chown
* kill vs kill -9

---

# Part 3: Shell Scripting Revision

## What is Shell?

A Shell is a command-line interpreter that receives user commands and passes them to the Linux Kernel.

Common Shells

* bash
* sh
* ksh
* zsh

---

# Create Your First Script

```bash
nano hello.sh
```

```bash
#!/bin/bash

echo "Hello Oracle DBA"
```

Save

```
Ctrl + O
Enter
Ctrl + X
```

Permission

```bash
chmod +x hello.sh
```

Run

```bash
./hello.sh
```

or

```bash
bash hello.sh
```

---

# Variables

```bash
name="Munna"

echo $name
```

---

# User Input

```bash
read name

echo "Welcome $name"
```

---

# Command Substitution

```bash
today=$(date)

echo $today
```

---

# If Else

```bash
if [ $age -ge 18 ]
then
    echo "Adult"
else
    echo "Minor"
fi
```

---

# For Loop

```bash
for i in 1 2 3 4 5
do
    echo $i
done
```

---

# While Loop

```bash
count=1

while [ $count -le 5 ]
do
    echo $count
    count=$((count+1))
done
```

---

# Backup Script

```bash
#!/bin/bash

SOURCE=~/Documents
DEST=~/Backup

mkdir -p "$DEST"

cp -r "$SOURCE" "$DEST"

echo "Backup Completed Successfully"
```

---

# Exit Status

```bash
echo $?
```

| Value    | Meaning |
| -------- | ------- |
| 0        | Success |
| Non-zero | Error   |

---

# Special Variables

| Variable | Meaning             |
| -------- | ------------------- |
| $0       | Script name         |
| $1       | First argument      |
| $2       | Second argument     |
| $#       | Number of arguments |
| $@       | All arguments       |
| $$       | Current Process ID  |

---

# File Test Operators

```bash
-f file.txt

-d folder

-r file.txt

-w file.txt

-x script.sh
```

---

# Practice Tasks

## SQL

* Create employees table
* Insert 10 records
* Update employee salary
* Delete one employee
* Use GROUP BY
* Use HAVING
* Create VIEW
* Create SEQUENCE
* Practice JOINS
* Solve subquery examples

---

## Linux

* Create directory
* Create files
* Copy files
* Move files
* Delete files
* Search files
* Search text
* Change permissions
* Check memory
* Check disk
* Monitor logs

---

## Shell Scripting

* Hello World script
* User input script
* Even/Odd checker
* For loop
* While loop
* Backup script

---

# Oracle DBA Interview Quick Revision

### SQL

✓ What is SQL?

✓ Types of SQL Commands

✓ Constraints

✓ Aggregate Functions

✓ Joins

✓ Views

✓ Sequences

✓ Subqueries

✓ GROUP BY

✓ HAVING

---

### Linux

✓ File Commands

✓ Search Commands

✓ Process Commands

✓ Permission Commands

✓ Networking Commands

✓ Disk Monitoring

✓ Memory Monitoring

✓ Log Monitoring

---

### Shell Scripting

✓ Variables

✓ User Input

✓ Conditions

✓ Loops

✓ Backup Script

✓ Exit Status

✓ Special Variables

✓ File Test Operators

---

# Day 20 Revision Checklist

* ✅ SQL Basics
* ✅ DDL, DML, DCL, TCL, DQL
* ✅ Constraints
* ✅ CRUD Operations
* ✅ SELECT Queries
* ✅ Aggregate Functions
* ✅ GROUP BY & HAVING
* ✅ JOINS
* ✅ Views
* ✅ Sequences
* ✅ Subqueries
* ✅ Linux Commands
* ✅ File Management
* ✅ Permissions
* ✅ Users & Processes
* ✅ Memory & Disk Monitoring
* ✅ Networking
* ✅ Log Monitoring
* ✅ Shell Scripting Basics
* ✅ Variables
* ✅ User Input
* ✅ Loops
* ✅ Conditions
* ✅ Backup Script

---

## 🎯 Revision Complete

You have now revised everything covered from **Day 1 through Day 19**. This serves as a solid foundation in **SQL**, **Linux**, and **Shell Scripting**—the core skills every Oracle DBA should master before moving on to advanced database administration topics.
