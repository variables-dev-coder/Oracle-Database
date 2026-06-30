# Day 28 – Revision & Interview Questions

**Week 4 – Oracle Architecture Revision**

---

# 🎯 Objectives

By the end of Day 28, you should be able to:

- Revise Oracle Architecture
- Explain Oracle Instance and Database
- Understand SGA and PGA
- Explain Oracle Background Processes
- Understand Physical Database Files
- Revise Tablespaces
- Understand UNDO and TEMP Tablespaces
- Answer common Oracle DBA interview questions

---

# Oracle Architecture Revision

Oracle Database consists of two major components:

```
                   Oracle Database System
                           │
          ┌────────────────┴────────────────┐
          │                                 │
     Oracle Instance                 Oracle Database
```

## Oracle Instance

An Oracle Instance is created whenever the Oracle database starts.

It contains:

- Memory Structures
- Background Processes

```
Oracle Instance
│
├── SGA (Shared Memory)
├── PGA (Private Memory)
│
├── PMON
├── SMON
├── DBWR
├── LGWR
├── CKPT
└── ARCn
```

---

## Oracle Database

The Oracle Database is the collection of physical files stored on disk.

```
Oracle Database
│
├── Datafiles
├── Control Files
└── Online Redo Log Files
```

---

# Memory Structures Revision

## SGA (System Global Area)

SGA is shared memory used by all users connected to the Oracle instance.

### Components

| Component | Purpose |
|------------|---------|
| Database Buffer Cache | Stores frequently accessed data blocks |
| Shared Pool | Stores SQL statements and execution plans |
| Redo Log Buffer | Stores redo entries before writing to redo logs |
| Large Pool | Used for RMAN and parallel execution |
| Java Pool | Used by Java stored procedures |

### Key Points

- Shared by all sessions
- Allocated when the instance starts
- Improves database performance

---

## PGA (Program Global Area)

PGA is private memory allocated to each server process.

### Stores

- Session information
- Sort area
- Hash area
- Cursor information

### Key Points

- Private memory
- One PGA per process
- Not shared

---

# SGA vs PGA

| SGA | PGA |
|------|------|
| Shared Memory | Private Memory |
| One per Instance | One per Process |
| Shared by all users | Used by one process |
| Allocated at startup | Allocated when process starts |

---

# Oracle Background Processes

## PMON (Process Monitor)

Responsibilities:

- Cleans failed sessions
- Releases locks
- Frees resources

---

## SMON (System Monitor)

Responsibilities:

- Performs instance recovery
- Cleans temporary segments
- Coalesces free space

---

## DBWR (Database Writer)

Responsibilities:

- Writes modified (dirty) blocks from Buffer Cache to Datafiles

---

## LGWR (Log Writer)

Responsibilities:

- Writes redo information from Redo Log Buffer to Online Redo Logs

---

## CKPT (Checkpoint Process)

Responsibilities:

- Updates Control Files
- Updates Datafile headers
- Signals DBWR during checkpoints

---

## ARCn (Archiver Process)

Responsibilities:

- Copies full Online Redo Logs to Archive Log Files
- Works only in ARCHIVELOG mode

---

# Background Process Flow

```
User Transaction
        │
        ▼
Database Buffer Cache
        │
        ▼
      DBWR
        │
        ▼
    Datafiles


User Transaction
        │
        ▼
Redo Log Buffer
        │
        ▼
      LGWR
        │
        ▼
Online Redo Log Files
```

---

# Physical Database Files

## Datafiles

Purpose:

- Store tables
- Store indexes
- Store actual user data

Examples

```
system01.dbf

users01.dbf

sysaux01.dbf
```

---

## Control Files

Contain metadata about the database.

Stores:

- Database name
- Datafile locations
- Redo log locations
- Checkpoint information
- Database creation timestamp

---

## Online Redo Log Files

Purpose:

- Store every database change
- Required for crash recovery
- Used during instance recovery

---

# Tablespaces Revision

A Tablespace is a logical storage unit made up of one or more Datafiles.

```
Tablespace
      │
      ▼
+--------------------+
| users01.dbf        |
| users02.dbf        |
+--------------------+
```

---

## SYSTEM Tablespace

Purpose

- Stores Data Dictionary
- Mandatory tablespace

---

## SYSAUX Tablespace

Purpose

- Auxiliary tablespace for SYSTEM
- Stores Oracle components

---

## USERS Tablespace

Purpose

- Stores user-created objects

---

## TEMP Tablespace

Purpose

- Sorting
- Hash joins
- Index creation
- Temporary operations

---

## UNDO Tablespace

Purpose

- Rollback
- Read consistency
- Transaction recovery

---

# TEMP vs UNDO

| TEMP | UNDO |
|------|------|
| Temporary work area | Stores old data |
| Sorting | Rollback |
| Joins | Read consistency |
| Index creation | Recovery |

---

# Useful SQL Commands

## List Tablespaces

```sql
SELECT tablespace_name
FROM dba_tablespaces;
```

---

## List Datafiles

```sql
SELECT file_name, tablespace_name
FROM dba_data_files;
```

---

## Check UNDO Tablespace

```sql
SHOW PARAMETER undo_tablespace;
```

or

```sql
SELECT tablespace_name
FROM dba_tablespaces
WHERE contents='UNDO';
```

---

## Check TEMP Tablespace

```sql
SELECT tablespace_name
FROM dba_tablespaces
WHERE contents='TEMPORARY';
```

---

# Linux Commands Revision

## Check Oracle Processes

```bash
ps -ef | grep ora_
```

---

## Check PMON

```bash
ps -ef | grep pmon
```

---

## Useful Linux Commands

```bash
pwd
ls
cd
mkdir
touch
cp
mv
rm
cat
nano
chmod
chown
grep
```

---

# Top 30 Oracle DBA Interview Questions

## 1. What is Oracle Database?

Oracle Database is a Relational Database Management System (RDBMS) used to store, retrieve, and manage data.

---

## 2. What is Oracle Instance?

An Oracle Instance consists of Memory Structures (SGA, PGA) and Background Processes.

---

## 3. Difference between Database and Instance?

| Database | Instance |
|-----------|----------|
| Physical Files | Memory + Processes |
| Exists on Disk | Exists in Memory |
| Stores Data | Manages Data |

---

## 4. What is SGA?

Shared memory that stores frequently used data and SQL statements.

---

## 5. What is PGA?

Private memory allocated for each server process.

---

## 6. What does PMON do?

- Cleans failed sessions
- Releases locks
- Frees resources

---

## 7. What does SMON do?

- Performs instance recovery
- Cleans temporary segments
- Coalesces free space

---

## 8. What does DBWR do?

Writes dirty data blocks from Buffer Cache to Datafiles.

---

## 9. What does LGWR do?

Writes redo entries from Redo Log Buffer to Online Redo Log Files.

---

## 10. Difference between DBWR and LGWR?

| DBWR | LGWR |
|------|------|
| Writes Data Blocks | Writes Redo Entries |
| Writes to Datafiles | Writes to Redo Logs |

---

## 11. What are Datafiles?

Physical files that store tables, indexes, and user data.

---

## 12. What are Control Files?

Files containing database metadata.

---

## 13. What are Online Redo Log Files?

Files that record every database change.

---

## 14. What is a Tablespace?

A logical storage unit consisting of one or more Datafiles.

---

## 15. Difference between Tablespace and Datafile?

| Tablespace | Datafile |
|------------|----------|
| Logical | Physical |

---

## 16. What is SYSTEM Tablespace?

Stores Oracle Data Dictionary.

---

## 17. What is SYSAUX Tablespace?

Stores Oracle components and auxiliary data.

---

## 18. What is USERS Tablespace?

Stores user-created tables and indexes.

---

## 19. What is TEMP Tablespace?

Stores temporary data used for sorting and joins.

---

## 20. What is UNDO Tablespace?

Stores undo information for rollback and recovery.

---

## 21. Difference between TEMP and UNDO?

TEMP stores temporary work data, while UNDO stores old data for rollback and read consistency.

---

## 22. Which process writes Datafiles?

**DBWR**

---

## 23. Which process writes Redo Logs?

**LGWR**

---

## 24. Which process performs Instance Recovery?

**SMON**

---

## 25. Which process cleans failed sessions?

**PMON**

---

## 26. How do you list all Tablespaces?

```sql
SELECT tablespace_name
FROM dba_tablespaces;
```

---

## 27. How do you list all Datafiles?

```sql
SELECT file_name
FROM dba_data_files;
```

---

## 28. How do you check the TEMP Tablespace?

```sql
SELECT tablespace_name
FROM dba_tablespaces
WHERE contents='TEMPORARY';
```

---

## 29. How do you check the UNDO Tablespace?

```sql
SHOW PARAMETER undo_tablespace;
```

---

## 30. How do you check Oracle Background Processes in Linux?

```bash
ps -ef | grep ora_
```

---

# Practice Tasks

## Task 1

Draw the complete Oracle Architecture from memory.

---

## Task 2

Explain the following flow:

```
User
 │
 ▼
Oracle Instance
 │
 ▼
Oracle Database
```

---

## Task 3

Write the responsibilities of:

- PMON
- SMON
- DBWR
- LGWR
- CKPT
- ARCn

---

## Task 4

Practice SQL Commands

```sql
SELECT tablespace_name
FROM dba_tablespaces;

SELECT file_name, tablespace_name
FROM dba_data_files;

SHOW PARAMETER undo_tablespace;
```

---

## Task 5

Practice Linux Commands

```bash
ps -ef | grep ora_

ps -ef | grep pmon
```

---

# Quick Revision Summary

| Topic | Key Point |
|--------|-----------|
| Oracle Instance | Memory + Background Processes |
| Oracle Database | Physical Files |
| SGA | Shared Memory |
| PGA | Private Memory |
| PMON | Cleans Failed Sessions |
| SMON | Instance Recovery |
| DBWR | Writes Data Blocks |
| LGWR | Writes Redo Entries |
| Datafiles | Store Actual Data |
| Control Files | Store Database Metadata |
| Redo Log Files | Store Database Changes |
| Tablespace | Logical Storage Unit |
| TEMP | Sorting & Temporary Operations |
| UNDO | Rollback & Read Consistency |

---

# 🎯 Day 28 Checklist

- ✅ Revised Oracle Architecture
- ✅ Revised SGA & PGA
- ✅ Revised Background Processes
- ✅ Revised Physical Database Files
- ✅ Revised Tablespaces
- ✅ Revised TEMP & UNDO Tablespaces
- ✅ Practiced SQL Commands
- ✅ Practiced Linux Commands
- ✅ Completed 30 Oracle DBA Interview Questions

---

# 🚀 Next Topic (Day 29)

**Oracle Users, Schemas & Privileges**
- Create Users
- Alter Users
- Drop Users
- System Privileges
- Object Privileges
- Roles
- Profiles
