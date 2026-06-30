# Day 27 – Undo & Temporary Tablespace

## Objective
Learn about:
- Undo Tablespace
- Temporary (TEMP) Tablespace
- Their purpose, architecture, and common DBA commands

---

# Undo Tablespace

## What is Undo Tablespace?

An **Undo Tablespace** stores the **before image (old data)** of rows that are modified by transactions.

Oracle automatically creates undo records before changing any data.

These undo records are used for:
- Rollback
- Read Consistency
- Instance Recovery
- Flashback Operations

---

## How Undo Works

Example:

```sql
UPDATE employees
SET salary = 60000
WHERE emp_id = 101;
```

Suppose the original salary is:

```
50000
```

Before Oracle updates the row, it stores:

```
50000
```

inside the Undo Tablespace.

After update:

```
Table Data
-----------
60000

Undo Data
-----------
50000
```

If the transaction is rolled back:

```sql
ROLLBACK;
```

Oracle restores:

```
Salary = 50000
```

---

# Why Undo Tablespace is Important

## 1. Rollback Transactions

Example:

```sql
UPDATE employees
SET salary = salary + 5000;

ROLLBACK;
```

Oracle restores all original values using Undo data.

---

## 2. Read Consistency

Example:

User A:

```sql
UPDATE employees
SET salary = 70000
WHERE emp_id = 101;
```

(User A has NOT committed.)

User B:

```sql
SELECT salary
FROM employees
WHERE emp_id = 101;
```

User B still sees the old salary because Oracle reads it from the Undo Tablespace.

---

## 3. Transaction Recovery

If the database crashes before a transaction commits, Oracle uses Undo data to reverse incomplete changes during recovery.

---

## 4. Flashback Features

Undo data allows Oracle to view or restore previous versions of data.

Example:

```sql
FLASHBACK TABLE employees
TO TIMESTAMP (SYSTIMESTAMP - INTERVAL '5' MINUTE);
```

---

# Check Current Undo Tablespace

```sql
SHOW PARAMETER undo_tablespace;
```

or

```sql
SELECT value
FROM v$parameter
WHERE name = 'undo_tablespace';
```

---

# List Undo Tablespaces

```sql
SELECT tablespace_name,
       contents,
       status
FROM dba_tablespaces
WHERE contents = 'UNDO';
```

---

# Check Undo Datafiles

```sql
SELECT tablespace_name,
       file_name
FROM dba_data_files
WHERE tablespace_name = 'UNDOTBS1';
```

---

# Create Undo Tablespace

```sql
CREATE UNDO TABLESPACE undotbs2
DATAFILE '/u01/app/oracle/oradata/XEPDB1/undotbs2.dbf'
SIZE 200M;
```

---

# Switch to New Undo Tablespace

```sql
ALTER SYSTEM
SET undo_tablespace = undotbs2;
```

---

# Temporary (TEMP) Tablespace

## What is Temporary Tablespace?

A Temporary Tablespace stores temporary data created while SQL statements are executing.

Oracle automatically removes this data after the operation finishes.

---

# When TEMP Tablespace is Used

Oracle uses TEMP during:

- ORDER BY
- GROUP BY
- DISTINCT
- UNION
- Hash Join
- CREATE INDEX
- Large Sort Operations

---

## Example

```sql
SELECT *
FROM employees
ORDER BY salary;
```

If Oracle cannot complete the sorting in memory (PGA), it uses the TEMP Tablespace.

---

# Check Default Temporary Tablespace

```sql
SELECT property_value
FROM database_properties
WHERE property_name = 'DEFAULT_TEMP_TABLESPACE';
```

---

# List Temporary Tablespaces

```sql
SELECT tablespace_name,
       contents
FROM dba_tablespaces
WHERE contents = 'TEMPORARY';
```

---

# Check Temp Files

```sql
SELECT tablespace_name,
       file_name
FROM dba_temp_files;
```

---

# Create Temporary Tablespace

```sql
CREATE TEMPORARY TABLESPACE temp2
TEMPFILE '/u01/app/oracle/oradata/XEPDB1/temp2.dbf'
SIZE 100M;
```

---

# Change Default Temporary Tablespace

```sql
ALTER DATABASE
DEFAULT TEMPORARY TABLESPACE temp2;
```

---

# Undo vs Temporary Tablespace

| Feature | Undo Tablespace | Temporary Tablespace |
|----------|-----------------|----------------------|
| Purpose | Stores old versions of modified data | Stores temporary work data |
| Used For | Rollback, Recovery, Read Consistency, Flashback | Sorting, Grouping, Joins, Index Creation |
| File Type | Datafile | Tempfile |
| Data Retention | Until no longer needed | Automatically removed after operation |

---

# Important Data Dictionary Views

| View | Description |
|------|-------------|
| `DBA_TABLESPACES` | Displays all tablespaces |
| `DBA_DATA_FILES` | Displays permanent datafiles |
| `DBA_TEMP_FILES` | Displays temporary files |
| `V$PARAMETER` | Shows database parameter values |
| `DATABASE_PROPERTIES` | Displays database properties |

---

# Real-Life Example

### Undo Tablespace

Imagine writing a document in Microsoft Word.

Before changing a paragraph, Word keeps the previous version.

If you press **Ctrl + Z**, it restores the old text.

Undo Tablespace works exactly like this.

---

### Temporary Tablespace

Imagine solving a math problem on rough paper.

After completing the calculation, you throw away the rough sheet.

TEMP Tablespace works in the same way.

---

# Interview Questions

### 1. What is an Undo Tablespace?

Undo Tablespace stores the previous version of modified data so Oracle can perform rollback, provide read consistency, recover incomplete transactions, and support flashback features.

---

### 2. What is a Temporary Tablespace?

A Temporary Tablespace stores temporary data generated while executing SQL operations such as sorting, grouping, joins, and index creation.

---

### 3. What is the difference between Undo and TEMP Tablespace?

Undo stores old versions of modified data for transaction management, whereas TEMP stores temporary working data required during SQL execution.

---

### 4. Which SQL operations commonly use TEMP Tablespace?

- ORDER BY
- GROUP BY
- DISTINCT
- UNION
- HASH JOIN
- CREATE INDEX

---

### 5. How do you check the current Undo Tablespace?

```sql
SHOW PARAMETER undo_tablespace;
```

---

### 6. Which view displays Temp files?

```sql
SELECT *
FROM dba_temp_files;
```

---

### 7. Which view displays permanent datafiles?

```sql
SELECT *
FROM dba_data_files;
```

---

# Practice Tasks

## Task 1
Check the current Undo Tablespace.

---

## Task 2
List all Undo Tablespaces.

---

## Task 3
Display Undo Datafiles.

---

## Task 4
Check the default Temporary Tablespace.

---

## Task 5
List all Temporary Tablespaces.

---

## Task 6
Display all Temp files.

---

## Task 7
Create a new Temporary Tablespace.

---

## Task 8
Create a new Undo Tablespace.

---

# Day 27 Summary

- Learned the purpose of **Undo Tablespace**.
- Understood **Rollback**, **Read Consistency**, **Recovery**, and **Flashback**.
- Learned the purpose of **Temporary Tablespace**.
- Practiced commands to view Undo and TEMP information.
- Learned how to create and switch Undo and Temporary Tablespaces.
- Compared **Undo** and **TEMP** Tablespaces with real-world examples.

---
**End of Day 27 Notes**
