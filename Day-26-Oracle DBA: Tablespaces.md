# Day 26 – Oracle Tablespaces

## Objective

Learn about Oracle Tablespaces and practice creating and managing them.

---

# What is a Tablespace?

A **Tablespace** is a **logical storage unit** in Oracle Database that stores database objects such as:

- Tables
- Indexes
- Materialized Views
- LOBs

A tablespace does **not** directly store data. The actual data is stored in **Datafiles**.

```
Oracle Database
       │
       ▼
  Tablespace
       │
       ▼
   Datafile (.dbf)
       │
       ▼
   Physical Disk
```

---

# Why Do We Use Tablespaces?

- Organize database objects
- Separate application data
- Manage storage efficiently
- Improve administration
- Perform backups more easily
- Assign storage quotas to users

---

# Tablespace Types

## 1. SYSTEM Tablespace

- Created automatically during database creation.
- Stores Oracle Data Dictionary.
- Mandatory for database operation.
- Never delete or make it offline.

---

## 2. SYSAUX Tablespace

- Auxiliary tablespace for SYSTEM.
- Stores Oracle components such as:
  - AWR
  - Enterprise Manager
  - Statistics

---

## 3. USERS Tablespace

- Default tablespace for normal users.
- Stores user-created tables, indexes, procedures, etc.

---

## 4. TEMP Tablespace

Stores temporary data during operations like:

- ORDER BY
- GROUP BY
- Hash Join
- Sort

Temporary data is removed automatically.

---

## 5. UNDO Tablespace

Stores Undo information.

Used for:

- Rollback
- Read Consistency
- Flashback Operations

---

# Permanent vs Temporary Tablespace

| Permanent Tablespace | Temporary Tablespace |
|----------------------|----------------------|
| Stores permanent objects | Stores temporary data |
| Tables | Sorting |
| Indexes | Temporary Segments |
| Views | Hash Operations |

---

# Tablespace Architecture

```
Database
   │
   ▼
Tablespace
   │
   ▼
Datafile (.dbf)
   │
   ▼
Physical Storage
```

Example:

```
Employee Table
      │
      ▼
 USERS Tablespace
      │
      ▼
 USERS01.DBF
      │
      ▼
 Hard Disk
```

---

# View Existing Tablespaces

```sql
SELECT tablespace_name
FROM dba_tablespaces;
```

Example Output:

```
SYSTEM
SYSAUX
UNDOTBS1
TEMP
USERS
```

---

# View Datafiles

```sql
SELECT file_name,
       tablespace_name
FROM dba_data_files;
```

Example Output (Windows):

```
C:\ORACLE\21C\ORADATA\XE\SYSTEM01.DBF
C:\ORACLE\21C\ORADATA\XE\SYSAUX01.DBF
C:\ORACLE\21C\ORADATA\XE\USERS01.DBF
C:\ORACLE\21C\ORADATA\XE\UNDOTBS01.DBF
```

---

# View Temporary Files

```sql
SELECT *
FROM dba_temp_files;
```

---

# Create a Tablespace

**Linux Example**

```sql
CREATE TABLESPACE trainee_tbs
DATAFILE '/u01/app/oracle/oradata/XE/trainee_tbs01.dbf'
SIZE 100M;
```

---

## Windows Example (Oracle 21c XE)

```sql
CREATE TABLESPACE trainee_tbs
DATAFILE 'C:\ORACLE\21C\ORADATA\XE\TRAINEE_TBS01.DBF'
SIZE 100M
AUTOEXTEND ON
NEXT 10M
MAXSIZE 500M;
```

### Explanation

| Clause | Meaning |
|---------|---------|
| SIZE 100M | Initial file size |
| AUTOEXTEND ON | Automatically increase size |
| NEXT 10M | Grow by 10 MB each time |
| MAXSIZE 500M | Maximum size 500 MB |

---

# Verify Tablespace

```sql
SELECT tablespace_name
FROM dba_tablespaces
WHERE tablespace_name='TRAINEE_TBS';
```

---

# Check Tablespace Datafile

```sql
SELECT file_name,
       tablespace_name
FROM dba_data_files
WHERE tablespace_name='TRAINEE_TBS';
```

---

# Add Another Datafile

```sql
ALTER TABLESPACE trainee_tbs
ADD DATAFILE
'C:\ORACLE\21C\ORADATA\XE\TRAINEE_TBS02.DBF'
SIZE 100M;
```

---

# Resize Datafile

```sql
ALTER DATABASE DATAFILE
'C:\ORACLE\21C\ORADATA\XE\TRAINEE_TBS01.DBF'
RESIZE 300M;
```

---

# Create User with Default Tablespace

```sql
CREATE USER app_user
IDENTIFIED BY password
DEFAULT TABLESPACE trainee_tbs;
```

---

# Give Tablespace Quota

```sql
ALTER USER app_user
QUOTA 200M ON trainee_tbs;
```

Unlimited quota:

```sql
ALTER USER app_user
QUOTA UNLIMITED ON trainee_tbs;
```

---

# Check User Default Tablespace

```sql
SELECT username,
       default_tablespace
FROM dba_users;
```

---

# Check Free Space

```sql
SELECT tablespace_name,
       bytes/1024/1024 AS MB
FROM dba_free_space;
```

---

# Drop Tablespace

```sql
DROP TABLESPACE trainee_tbs
INCLUDING CONTENTS
AND DATAFILES;
```

---

# Practice (Completed)

## Step 1

View existing tablespaces.

```sql
SELECT tablespace_name
FROM dba_tablespaces;
```

---

## Step 2

View existing datafiles.

```sql
SELECT file_name,
       tablespace_name
FROM dba_data_files;
```

---

## Step 3

Create a new tablespace.

```sql
CREATE TABLESPACE trainee_tbs
DATAFILE 'C:\ORACLE\21C\ORADATA\XE\TRAINEE_TBS01.DBF'
SIZE 100M
AUTOEXTEND ON
NEXT 10M
MAXSIZE 500M;
```

---

## Step 4

Verify the tablespace.

```sql
SELECT tablespace_name
FROM dba_tablespaces
WHERE tablespace_name='TRAINEE_TBS';
```

---

## Step 5

Check the datafile.

```sql
SELECT file_name,
       tablespace_name
FROM dba_data_files
WHERE tablespace_name='TRAINEE_TBS';
```

---

## Step 6

Add another datafile.

```sql
ALTER TABLESPACE trainee_tbs
ADD DATAFILE
'C:\ORACLE\21C\ORADATA\XE\TRAINEE_TBS02.DBF'
SIZE 100M;
```

---

## Step 7

Drop the practice tablespace.

```sql
DROP TABLESPACE trainee_tbs
INCLUDING CONTENTS
AND DATAFILES;
```

---

# Common Errors

## ORA-01119

```
Error in creating database file
```

**Reason:**
Invalid or non-existent datafile path.

---

## ORA-27040

```
File create error
```

**Reason:**
Oracle cannot create the datafile because the directory does not exist or lacks permission.

---

## OS Error 3

```
The system cannot find the path specified.
```

**Solution:**

Use the correct Windows Oracle datafile directory.

Example:

```
C:\ORACLE\21C\ORADATA\XE\
```

instead of Linux path:

```
/u01/app/oracle/oradata/XE/
```

---

# Interview Questions

## 1. What is a Tablespace?

A Tablespace is a logical storage unit that groups one or more datafiles and stores database objects.

---

## 2. What is the difference between a Tablespace and a Datafile?

| Tablespace | Datafile |
|------------|----------|
| Logical storage | Physical storage |
| Contains database objects logically | Stores actual data on disk |

---

## 3. What are the main types of Tablespaces?

- SYSTEM
- SYSAUX
- USERS
- TEMP
- UNDO

---

## 4. What is AUTOEXTEND?

AUTOEXTEND automatically increases the size of a datafile when it becomes full.

---

## 5. Why is the TEMP Tablespace used?

It stores temporary data generated during sorting, joins, and other SQL operations.

---

## 6. Which Tablespace stores Oracle Data Dictionary?

SYSTEM Tablespace.

---

## 7. Which Tablespace stores Undo Information?

UNDOTBS1 (UNDO Tablespace).

---

# Key Takeaways

- A Tablespace is a logical storage unit.
- Data is physically stored in **.DBF** datafiles.
- SYSTEM and SYSAUX are mandatory system tablespaces.
- USERS is the default tablespace for user objects.
- TEMP stores temporary data.
- UNDO stores rollback information.
- AUTOEXTEND helps datafiles grow automatically.
- Always use the correct operating system path when creating datafiles.

---

# Day 26 Summary

✅ Learned Oracle Tablespaces

✅ Understood Tablespace architecture

✅ Learned SYSTEM, SYSAUX, USERS, TEMP, and UNDO Tablespaces

✅ Viewed existing Tablespaces and Datafiles

✅ Created a new Tablespace

✅ Verified the Tablespace

✅ Added a Datafile

✅ Learned common errors and solutions

✅ Practiced real Oracle DBA administration tasks
