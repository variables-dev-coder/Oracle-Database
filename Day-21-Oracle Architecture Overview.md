# Day 21 Notes – Oracle Architecture Overview (Oracle DBA Roadmap)

## Week 4 – Oracle Architecture

---

# Learning Objectives

By the end of this lesson, you will understand:

* What Oracle Architecture is
* Oracle Instance
* Oracle Database
* Difference between Instance and Database
* Oracle Startup Process
* Oracle Shutdown Process
* Oracle Architecture Diagram
* Interview Questions
* Practice Commands

---

# What is Oracle Architecture?

Oracle Architecture describes the internal structure of an Oracle Database system and how all components work together to process user requests.

Oracle Architecture consists of **two main components**:

```
                 Oracle Database System
                        │
        ┌───────────────┴───────────────┐
        │                               │
  Oracle Instance                 Oracle Database
 (Memory + Processes)             (Physical Files)
```

**Formula to Remember**

```
Oracle = Instance + Database
```

---

# Oracle Instance

## Definition

An **Oracle Instance** is the running part of Oracle Database.

It is created whenever the Oracle database starts.

An Oracle Instance consists of:

* Memory (SGA)
* Background Processes

```
Oracle Instance
│
├── Memory (SGA)
│
└── Background Processes
```

### Simple Definition

> Instance = Memory + Background Processes

---

# Components of Oracle Instance

## 1. Memory (SGA)

The System Global Area (SGA) stores frequently used data and SQL statements in memory to improve performance.

Examples:

* Shared Pool
* Database Buffer Cache
* Redo Log Buffer

---

## 2. Background Processes

Oracle automatically starts several processes to manage the database.

Important background processes:

* DBWR (Database Writer)
* LGWR (Log Writer)
* SMON (System Monitor)
* PMON (Process Monitor)

More processes will be covered in later lessons.

---

# Oracle Database

## Definition

An **Oracle Database** is the collection of physical files stored on disk.

These files permanently store all database information.

```
Oracle Database
│
├── Data Files
├── Control Files
├── Redo Log Files
├── Temp Files
├── Undo Files
├── Archived Logs
├── Parameter Files
└── Password File
```

---

# Physical Files

## 1. Data Files

Stores:

* Tables
* Indexes
* User Data

Example:

```
EMPLOYEES

ID   NAME
-------------
1    John
2    Alex
3    Sara
```

---

## 2. Control Files

Stores database information such as:

* Database name
* Data file locations
* Redo log information
* Checkpoint information

---

## 3. Redo Log Files

Store every change made to the database.

Used during crash recovery.

---

## 4. Temp Files

Used for temporary operations like:

* Sorting
* Joins
* Large queries

---

## 5. Undo Files

Store old versions of data.

Used for:

* Rollback
* Read Consistency

---

## 6. Archived Logs

Copies of redo logs used for:

* Backup
* Recovery

---

# Oracle Instance vs Oracle Database

| Oracle Instance           | Oracle Database        |
| ------------------------- | ---------------------- |
| Running component         | Physical storage       |
| Memory + Processes        | Physical Files         |
| Temporary                 | Permanent              |
| Created at startup        | Exists on disk         |
| Executes SQL              | Stores data            |
| Disappears after shutdown | Remains after shutdown |

---

# Real-Life Example

Imagine a Laptop.

### Hard Disk (SSD)

Stores:

* Photos
* Videos
* Documents

This is like the **Oracle Database**.

---

### RAM + CPU

When you turn on the laptop:

* RAM becomes active
* CPU starts working
* Operating System loads

This is like the **Oracle Instance**.

When the laptop is turned off:

* RAM is cleared
* CPU stops
* Files remain safe

Exactly how Oracle works.

---

# Oracle Startup Process

When Oracle starts:

```
STARTUP
    │
    ▼
Allocate Memory (SGA)
    │
    ▼
Start Background Processes
    │
    ▼
Read Control Files
    │
    ▼
Open Data Files
    │
    ▼
Database Ready
```

### Steps

1. Oracle allocates memory.
2. Oracle starts background processes.
3. Control files are opened.
4. Data files are opened.
5. Users can connect.

---

# Oracle Shutdown Process

```
Users Disconnect
      │
      ▼
Database Closes
      │
      ▼
Instance Stops
      │
      ▼
Memory Released
```

Database files remain safely stored on disk.

---

# Oracle Architecture Diagram

```
                     User

                      │
                      ▼

                 SQL Query

                      │

             Oracle Instance

        +------------------------+
        |        Memory          |
        |------------------------|
        | Shared Pool            |
        | Buffer Cache           |
        | Redo Log Buffer        |
        +------------------------+

        +------------------------+
        | Background Processes   |
        |------------------------|
        | DBWR                   |
        | LGWR                   |
        | SMON                   |
        | PMON                   |
        +------------------------+

                      │
                      ▼

             Oracle Database

        +------------------------+
        | Data Files             |
        | Control Files          |
        | Redo Log Files         |
        | Temp Files             |
        | Undo Files             |
        +------------------------+
```

---

# SQL Execution Flow

Example Query:

```sql
SELECT * FROM employees;
```

Execution Flow:

```
User

↓

Oracle Instance receives SQL

↓

Check Memory (Cache)

↓

Data Found?
   │
Yes ──► Return Data

No

↓

Read Data Files

↓

Store in Memory

↓

Return Result
```

---

# Why Oracle Separates Instance and Database

Advantages:

* Faster performance using memory caching
* Better multi-user support
* Easier crash recovery
* Improved scalability
* Better resource management

---

# Important Terms

| Term     | Meaning                              |
| -------- | ------------------------------------ |
| Instance | Running Oracle environment           |
| Database | Collection of physical files         |
| SGA      | Shared memory used by Oracle         |
| DBWR     | Writes modified data to data files   |
| LGWR     | Writes redo information to redo logs |
| Startup  | Starts instance and opens database   |
| Shutdown | Closes database and releases memory  |

---

# Interview Questions

## Q1. What is Oracle Architecture?

**Answer:**

Oracle Architecture consists of two major components:

* Oracle Instance
* Oracle Database

The instance manages processing, while the database stores data permanently.

---

## Q2. What is an Oracle Instance?

**Answer:**

An Oracle Instance is the running part of Oracle Database consisting of:

* SGA (Memory)
* Background Processes

---

## Q3. What is an Oracle Database?

**Answer:**

An Oracle Database is a collection of physical files such as data files, control files, redo log files, temp files, and undo files that permanently store database information.

---

## Q4. What is the difference between Instance and Database?

**Answer:**

| Instance           | Database       |
| ------------------ | -------------- |
| Memory + Processes | Physical Files |
| Temporary          | Permanent      |
| Executes SQL       | Stores Data    |

---

## Q5. What happens during Oracle Startup?

**Answer:**

* Memory is allocated.
* Background processes start.
* Control files are opened.
* Data files are opened.
* Database becomes available.

---

## Q6. Does shutting down Oracle delete the database?

**Answer:**

No.

Only the instance stops.

The database files remain safely stored on disk.

---

# Oracle Dynamic Performance Views

Check Instance Information:

```sql
SELECT INSTANCE_NAME, STATUS
FROM V$INSTANCE;
```

---

Check Database Information:

```sql
SELECT NAME, OPEN_MODE
FROM V$DATABASE;
```

---

Check Oracle Version:

```sql
SELECT * FROM V$VERSION;
```

---

# Practice Tasks

## Theory

* Explain Oracle Architecture.
* Explain Oracle Instance.
* Explain Oracle Database.
* Differentiate Instance and Database.
* Explain Oracle Startup Process.
* Explain Oracle Shutdown Process.

---

## Practical

Run the following SQL commands:

```sql
SELECT INSTANCE_NAME, STATUS
FROM V$INSTANCE;
```

```sql
SELECT NAME, OPEN_MODE
FROM V$DATABASE;
```

```sql
SELECT * FROM V$VERSION;
```

Observe:

* Instance Name
* Instance Status
* Database Name
* Database Open Mode
* Oracle Version

---

# Quick Revision

```
Oracle
│
├── Instance
│     ├── SGA (Memory)
│     └── Background Processes
│
└── Database
      ├── Data Files
      ├── Control Files
      ├── Redo Log Files
      ├── Temp Files
      ├── Undo Files
      └── Archived Logs
```

---

# Memory Tricks

### Trick 1

```
Oracle = Instance + Database
```

### Trick 2

```
Instance = Memory + Processes
```

### Trick 3

```
Database = Physical Files
```

### Trick 4

```
Instance Works
Database Stores
```

---

# Key Takeaways

* Oracle Architecture has two main components: **Instance** and **Database**.
* An **Instance** is the running environment consisting of **SGA** and **background processes**.
* A **Database** is a collection of **physical files** that permanently store data.
* During **startup**, Oracle creates the instance first and then opens the database.
* During **shutdown**, the instance stops, but the database files remain safely stored on disk.
* Understanding the difference between an **Instance** and a **Database** is one of the most frequently asked Oracle DBA interview topics.
