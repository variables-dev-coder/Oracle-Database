# Day 8 — SQL Advanced (JOINS) + Linux Basics

## Learning Goals

### SQL JOINS

* INNER JOIN
* LEFT JOIN
* RIGHT JOIN
* FULL OUTER JOIN

### Linux Basics

* pwd
* ls
* cd
* mkdir
* touch
* rm
* cat

---

# SQL JOINS

A JOIN combines rows from two or more tables using a related column.

## Sample Tables

### Department

```sql
CREATE TABLE Department (
    dept_id INT,
    dept_name VARCHAR(50)
);
```

### Employee

```sql
CREATE TABLE Employee (
    emp_id INT,
    name VARCHAR(50),
    dept_id INT
);
```

### Insert Data (Oracle)

```sql
INSERT INTO Department VALUES (101, 'HR');
INSERT INTO Department VALUES (102, 'IT');
INSERT INTO Department VALUES (104, 'Finance');

INSERT INTO Employee VALUES (1, 'Alice', 101);
INSERT INTO Employee VALUES (2, 'Bob', 102);
INSERT INTO Employee VALUES (3, 'Charlie', 103);
INSERT INTO Employee VALUES (4, 'David', NULL);
```

---

## Employee Table

| emp_id | name    | dept_id |
| ------ | ------- | ------- |
| 1      | Alice   | 101     |
| 2      | Bob     | 102     |
| 3      | Charlie | 103     |
| 4      | David   | NULL    |

## Department Table

| dept_id | dept_name |
| ------- | --------- |
| 101     | HR        |
| 102     | IT        |
| 104     | Finance   |

---

# 1. INNER JOIN

Returns only matching rows from both tables.

```sql
SELECT e.name, d.dept_name
FROM Employee e
INNER JOIN Department d
ON e.dept_id = d.dept_id;
```

### Output

| name  | dept_name |
| ----- | --------- |
| Alice | HR        |
| Bob   | IT        |

### Memory Trick

**INNER = Intersection**

Only common records are returned.

---

# 2. LEFT JOIN

Returns all rows from the left table and matching rows from the right table.

```sql
SELECT e.name, d.dept_name
FROM Employee e
LEFT JOIN Department d
ON e.dept_id = d.dept_id;
```

### Output

| name    | dept_name |
| ------- | --------- |
| Alice   | HR        |
| Bob     | IT        |
| Charlie | NULL      |
| David   | NULL      |

### Memory Trick

**LEFT JOIN = Keep everything on the LEFT**

---

# 3. RIGHT JOIN

Returns all rows from the right table and matching rows from the left table.

```sql
SELECT e.name, d.dept_name
FROM Employee e
RIGHT JOIN Department d
ON e.dept_id = d.dept_id;
```

### Output

| name  | dept_name |
| ----- | --------- |
| Alice | HR        |
| Bob   | IT        |
| NULL  | Finance   |

### Memory Trick

**RIGHT JOIN = Keep everything on the RIGHT**

---

# 4. FULL OUTER JOIN

Returns all matching and non-matching rows from both tables.

```sql
SELECT e.name, d.dept_name
FROM Employee e
FULL OUTER JOIN Department d
ON e.dept_id = d.dept_id;
```

### Output

| name    | dept_name |
| ------- | --------- |
| Alice   | HR        |
| Bob     | IT        |
| Charlie | NULL      |
| David   | NULL      |
| NULL    | Finance   |

### Memory Trick

**FULL JOIN = LEFT JOIN + RIGHT JOIN**

---

# Interview Questions

## Find Employees Without Departments

```sql
SELECT e.*
FROM Employee e
LEFT JOIN Department d
ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;
```

### Result

* Charlie
* David

---

## Find Departments Without Employees

```sql
SELECT d.*
FROM Department d
LEFT JOIN Employee e
ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
```

### Result

* Finance

---

## Replace NULL with Custom Text

```sql
SELECT
    e.name,
    COALESCE(d.dept_name, 'No Department') AS department
FROM Employee e
LEFT JOIN Department d
ON e.dept_id = d.dept_id;
```

---

# JOIN Summary Table

| JOIN Type  | Returns                   |
| ---------- | ------------------------- |
| INNER JOIN | Matching rows only        |
| LEFT JOIN  | All rows from left table  |
| RIGHT JOIN | All rows from right table |
| FULL JOIN  | All rows from both tables |

---

# Linux Basics

## Current Directory

```bash
pwd
```

Shows the current working directory.

---

## List Files

```bash
ls
```

Lists files and folders.

---

## Change Directory

```bash
cd folder_name
```

Move into a folder.

---

## Go Back One Folder

```bash
cd ..
```

Move to the parent directory.

---

## Create Directory

```bash
mkdir projects
```

Creates a folder named projects.

---

## Create File

```bash
touch notes.txt
```

Creates an empty file.

---

## View File Contents

```bash
cat notes.txt
```

Displays file content.

---

## Delete File

```bash
rm notes.txt
```

Removes a file.

---

## Delete Directory

```bash
rm -r projects
```

Removes a directory and its contents.

---

# Practice Tasks

### SQL

1. Show employee names with department names.
2. Show all employees even if they have no department.
3. Show all departments even if they have no employees.
4. Find employees without departments.
5. Find departments without employees.

### Linux

1. Create a folder named sql_practice.
2. Enter the folder.
3. Create a file named notes.txt.
4. View the file.
5. Delete the file.
6. Delete the folder.

---

# Day 8 Quick Revision

* INNER JOIN → Matching rows only
* LEFT JOIN → Keep all left rows
* RIGHT JOIN → Keep all right rows
* FULL JOIN → Keep everything
* pwd → Current directory
* ls → List files
* cd → Change directory
* mkdir → Create folder
* touch → Create file
* cat → View file
* rm → Delete file/folder
