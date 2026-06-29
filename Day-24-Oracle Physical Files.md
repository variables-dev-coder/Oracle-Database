# Day 24 – Oracle Physical Files

## Objectives

Today you will learn:

- Datafiles
- Control Files
- Redo Log Files

---

# Oracle Physical Database Structure

Oracle stores all database information in physical files on the operating system.

```
Oracle Database
│
├── Datafiles (.dbf)
├── Control Files (.ctl)
└── Redo Log Files (.log)
```

### Easy Memory Trick

| File | Purpose |
|------|----------|
| Datafile | Stores actual database data |
| Control File | Stores database metadata |
| Redo Log | Stores every database change |

---

# 1. Datafiles

## Definition

A **Datafile** is a physical operating system file that stores the actual Oracle database data.

Everything created inside Oracle is ultimately stored in datafiles.

Examples:

- Tables
- Indexes
- Views (metadata)
- Procedures
- Packages
- Users
- Data

---

## Example

Create a table:

```sql
CREATE TABLE employee (
    id NUMBER,
    name VARCHAR2(50)
);
```

Insert data:

```sql
INSERT INTO employee VALUES (1, 'Aziz');
COMMIT;
```

The table structure and inserted data are stored inside a **Datafile**.

---

## File Extension

```
.dbf
```

Example:

```
system01.dbf
users01.dbf
undotbs01.dbf
```

---

## Common Datafiles

```
system01.dbf
sysaux01.dbf
users01.dbf
undotbs01.dbf
example01.dbf
```

---

## What is Stored in Datafiles?

- Tables
- Indexes
- Materialized Views
- LOB Data
- Undo Data
- Segments
- Extents
- Blocks

---

## Datafile Hierarchy

```
Datafile
   │
   ├── Blocks
   ├── Extents
   └── Segments
```

Remember:

```
Block
   ↓
Extent
   ↓
Segment
   ↓
Table
```

---

## Multiple Datafiles

A tablespace can have multiple datafiles.

Example:

```
USERS Tablespace

├── users01.dbf
├── users02.dbf
└── users03.dbf
```

Oracle automatically uses available space among them.

---

## View Datafiles

```sql
SELECT file_name
FROM dba_data_files;
```

---

## View Datafile Size

```sql
SELECT
    file_name,
    bytes/1024/1024 AS MB
FROM dba_data_files;
```

---

## Add New Datafile

```sql
ALTER TABLESPACE users
ADD DATAFILE
'/u01/oradata/users02.dbf'
SIZE 100M;
```

---

# Interview Question

### What is a Datafile?

**Answer:**

A Datafile is a physical operating system file that stores actual Oracle database data such as tables, indexes, and other database objects.

---

# 2. Control Files

## Definition

A **Control File** is a small but critical file that stores metadata about the Oracle database.

Without a valid control file, Oracle cannot mount or open the database.

---

## File Extension

```
.ctl
```

Examples:

```
control01.ctl
control02.ctl
control03.ctl
```

---

## What Information Does It Store?

- Database Name
- Database ID (DBID)
- SCN (System Change Number)
- Checkpoint Information
- Datafile Names
- Redo Log Names
- Tablespace Information
- Backup Information

---

## Easy Example

Imagine a library.

```
Books
      ↓
Datafiles

Library Catalog
      ↓
Control File
```

Without the catalog, you don't know where the books are.

Similarly, Oracle cannot locate database files without the control file.

---

## Multiplexing Control Files

Oracle recommends storing multiple copies.

```
Disk 1

control01.ctl

Disk 2

control02.ctl

Disk 3

control03.ctl
```

If one control file is damaged, Oracle can use another copy.

---

## View Control Files

```sql
SELECT name
FROM v$controlfile;
```

---

## Show Control File Parameter

```sql
SHOW PARAMETER control_files;
```

Example Output:

```
control01.ctl
control02.ctl
control03.ctl
```

---

# Interview Question

### Can Oracle run without a Control File?

**Answer:**

No.

Oracle cannot mount or open the database without a valid control file.

---

# 3. Redo Log Files

## Definition

Redo Log Files store all database changes.

Oracle uses them to recover committed transactions after a crash.

---

## File Extension

```
.log
```

Examples:

```
redo01.log
redo02.log
redo03.log
```

---

## Which Operations Generate Redo?

- INSERT
- UPDATE
- DELETE
- CREATE
- ALTER
- DROP

Nearly every change generates redo information.

---

## Example

Current Salary

```
10000
```

Run:

```sql
UPDATE employee
SET salary = 20000;
```

Oracle first records the change in the redo log before writing the updated data to the datafile.

---

# Redo Log Groups

Oracle organizes redo logs into groups.

Example:

```
Group 1

redo01.log
redo02.log

↓

Group 2

redo03.log
redo04.log

↓

Group 3

redo05.log
redo06.log
```

Each group may contain multiple members for redundancy.

---

# Circular Writing

Oracle writes redo logs in a circular manner.

```
Group 1
   ↓
Group 2
   ↓
Group 3
   ↓
Group 1
   ↓
Group 2
```

---

## View Redo Log Groups

```sql
SELECT
    group#,
    status
FROM v$log;
```

---

## View Redo Log Members

```sql
SELECT
    group#,
    member
FROM v$logfile;
```

---

# Interview Question

### Why are Redo Log Files Important?

**Answer:**

Redo Log Files record every database change and are used for crash recovery to ensure committed transactions are not lost.

---

# Oracle Write Process

```
User
   │
   ▼
SQL Statement
   │
   ▼
Redo Log Buffer (SGA)
   │
   ▼
LGWR
   │
   ▼
Redo Log Files
   │
   ▼
DBWR
   │
   ▼
Datafiles
```

### Explanation

- User executes SQL.
- Changes are first stored in the **Redo Log Buffer**.
- **LGWR** writes redo information to Redo Log Files.
- **DBWR** later writes modified data blocks to Datafiles.

---

# Difference Between Physical Files

| Feature | Datafiles | Control Files | Redo Log Files |
|----------|-----------|---------------|----------------|
| Stores actual data | ✅ | ❌ | ❌ |
| Stores metadata | ❌ | ✅ | ❌ |
| Stores database changes | ❌ | ❌ | ✅ |
| Used during recovery | ✅ | ✅ | ✅ |
| Required to open database | ✅ | ✅ | ✅ |
| Extension | .dbf | .ctl | .log |

---

# Real-Life Analogy

Imagine a bank.

### Datafiles

```
Customer Lockers

Store Money & Documents
```

### Control Files

```
Bank Register

Contains locker numbers and locations.
```

### Redo Log Files

```
Security Camera / Transaction Log

Records every activity inside the bank.
```

---

# Important SQL Commands

## View Datafiles

```sql
SELECT file_name
FROM dba_data_files;
```

---

## View Datafile Sizes

```sql
SELECT
    file_name,
    bytes/1024/1024 AS MB
FROM dba_data_files;
```

---

## View Control Files

```sql
SELECT name
FROM v$controlfile;
```

---

## Show Control File Parameter

```sql
SHOW PARAMETER control_files;
```

---

## View Redo Log Groups

```sql
SELECT
    group#,
    status
FROM v$log;
```

---

## View Redo Log Members

```sql
SELECT
    group#,
    member
FROM v$logfile;
```

---

# Interview Questions

## 1. What are Oracle physical files?

- Datafiles
- Control Files
- Redo Log Files

---

## 2. Which file stores actual database data?

**Answer:** Datafiles

---

## 3. Which file stores database metadata?

**Answer:** Control Files

---

## 4. Which file stores all database changes?

**Answer:** Redo Log Files

---

## 5. Can Oracle start without a Control File?

**Answer:** No.

---

## 6. What is the extension of a Datafile?

**Answer:** `.dbf`

---

## 7. What is the extension of a Control File?

**Answer:** `.ctl`

---

## 8. What is the extension of a Redo Log File?

**Answer:** `.log`

---

## 9. Why are multiple Control Files maintained?

**Answer:**

To provide redundancy (multiplexing) so the database can still start if one control file is lost or corrupted.

---

## 10. Why are Redo Log Files important?

**Answer:**

They help Oracle recover committed transactions after an unexpected shutdown or system crash.

---

# Practice

## Task 1

View all datafiles.

```sql
SELECT file_name
FROM dba_data_files;
```

---

## Task 2

View datafile sizes.

```sql
SELECT
    file_name,
    bytes/1024/1024 AS MB
FROM dba_data_files;
```

---

## Task 3

View control files.

```sql
SELECT name
FROM v$controlfile;
```

---

## Task 4

View configured control files.

```sql
SHOW PARAMETER control_files;
```

---

## Task 5

View redo log groups.

```sql
SELECT
    group#,
    status
FROM v$log;
```

---

## Task 6

View redo log members.

```sql
SELECT
    group#,
    member
FROM v$logfile;
```

---

# Revision Notes (1-Minute Recall)

```
Oracle Physical Files

Datafiles (.dbf)
│
├── Store actual database data
├── Tables
├── Indexes
├── Users
└── Data

Control Files (.ctl)
│
├── Database metadata
├── DB Name
├── DBID
├── SCN
├── Datafile locations
└── Redo Log locations

Redo Log Files (.log)
│
├── Record every database change
├── Used for crash recovery
├── Written by LGWR
└── Read during recovery
```

---

# Day 24 Summary

- Datafiles (`.dbf`) store all actual database objects and user data.
- Control Files (`.ctl`) store metadata required to mount and open the database.
- Redo Log Files (`.log`) record every database change and are essential for crash recovery.
- Oracle uses **LGWR** to write redo information and **DBWR** to write modified data blocks to datafiles.
- Multiplexing Control Files and Redo Log members improves fault tolerance and availability.

---
**End of Day 24 Notes**
