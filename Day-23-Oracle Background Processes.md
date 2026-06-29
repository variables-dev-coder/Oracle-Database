# Day 23 – Oracle Background Processes

## Overview

Oracle Database runs several **background processes** automatically when the database instance starts. These processes perform essential tasks such as writing data to disk, recovering the database after a crash, cleaning up failed sessions, and maintaining database consistency.

### Why Background Processes are Important?

- Improve database performance
- Handle recovery after failures
- Write data safely to disk
- Manage memory automatically
- Clean up failed user sessions
- Ensure transaction consistency

---

# Oracle Background Process Architecture

```text
                  User
                    │
                    ▼
            SQL Statement
                    │
                    ▼
            Server Process
                    │
        ┌───────────┴────────────┐
        ▼                        ▼
 Database Buffer Cache      Redo Log Buffer
        │                        │
        ▼                        ▼
      DBWR                    LGWR
        │                        │
        ▼                        ▼
   Data Files             Redo Log Files
                │
                ▼
             Crash Recovery
                │
                ▼
              SMON

User Session Crash
        │
        ▼
      PMON
```

---

# 1. PMON (Process Monitor)

## Definition

PMON (Process Monitor) is responsible for cleaning up failed or abnormal user sessions.

Whenever a user session terminates unexpectedly due to a network failure, application crash, or system shutdown, PMON automatically cleans up the session.

---

## Responsibilities

### 1. Cleans Failed User Sessions

Removes inactive or crashed user sessions.

Example:

```text
User Connected
      │
Network Failure
      │
 Session Crashed
      │
     PMON
      │
Session Cleaned
```

---

### 2. Releases Locks

If a crashed session was holding database locks, PMON releases them automatically.

Example:

```sql
UPDATE employees
SET salary = 60000
WHERE employee_id = 101;
```

If the client crashes before `COMMIT`, PMON releases the row lock.

---

### 3. Frees Memory

PMON releases:

- Session memory
- PGA memory
- Other allocated resources

---

### 4. Registers Database with Listener

PMON dynamically registers the Oracle database with the Listener.

No manual registration is required in most modern Oracle versions.

---

## Interview Question

### What happens if a user session crashes?

**Answer:**

PMON detects the failed session, releases locks, frees memory, and cleans up all resources used by that session.

---

# 2. SMON (System Monitor)

## Definition

SMON (System Monitor) performs recovery and housekeeping tasks for the database.

It is mainly responsible for recovering the database after an abnormal shutdown.

---

## Responsibilities

### 1. Instance Recovery

If the database crashes due to power failure or system failure, SMON automatically performs instance recovery.

Recovery uses:

- Online Redo Logs
- Undo Information

Example:

```text
Power Failure
      │
Database Crash
      │
Database Startup
      │
    SMON
      │
Instance Recovery
```

---

### 2. Cleans Temporary Segments

SMON removes unused temporary segments created during operations like:

- ORDER BY
- GROUP BY
- CREATE INDEX

---

### 3. Coalesces Free Space

SMON combines adjacent free extents to reduce fragmentation.

Example:

Before

```text
■■ □ □ ■■ □ □
```

After

```text
■■ □□□□ ■■
```

---

### 4. Performs Housekeeping Tasks

SMON performs various internal maintenance operations to keep the database healthy.

---

## Interview Question

### Which process performs Instance Recovery?

**Answer:**

SMON (System Monitor)

---

# 3. DBWR (Database Writer)

## Definition

DBWR writes modified data blocks (Dirty Blocks) from the Database Buffer Cache to the Data Files.

Oracle may have multiple Database Writer processes:

- DBW0
- DBW1
- DBW2
- DBW3

---

## Dirty Block

A Dirty Block is a block that has been modified in memory but has not yet been written to disk.

Example:

```text
UPDATE Employee
        │
Buffer Cache
        │
 Dirty Block
```

---

## Working

```text
User Updates Data
        │
Database Buffer Cache
        │
Dirty Block
        │
      DBWR
        │
Data File (.dbf)
```

---

## When DBWR Writes?

### Buffer Cache becomes full

Oracle needs free space.

---

### During Checkpoint

Checkpoint forces Dirty Blocks to disk.

---

### Database Shutdown

Before shutting down the database.

---

### Periodically

Oracle periodically writes Dirty Blocks.

---

## Example

```sql
UPDATE employees
SET salary = 75000
WHERE employee_id = 101;
```

The data is first updated in memory.

DBWR later writes the updated block to the Data File.

---

## Interview Question

### What is a Dirty Block?

**Answer:**

A Dirty Block is a modified data block in the Database Buffer Cache that has not yet been written to the Data Files.

---

# 4. LGWR (Log Writer)

## Definition

LGWR writes Redo Entries from the Redo Log Buffer to the Online Redo Log Files.

Every database change first generates Redo information.

---

## Working

```text
User Updates Data
        │
Redo Generated
        │
Redo Log Buffer
        │
     LGWR
        │
Redo Log Files
```

---

## Why is LGWR Important?

Redo Logs are used to recover the database after a crash.

Without Redo Logs, committed transactions could be lost.

---

## When LGWR Writes?

### On COMMIT

Immediately writes Redo information.

```sql
COMMIT;
```

---

### Every 3 Seconds

Automatically flushes the Redo Buffer.

---

### Redo Buffer becomes One-Third Full

Automatically writes Redo entries.

---

### Before DBWR Writes Dirty Blocks

Oracle follows the **Write-Ahead Logging (WAL)** principle.

Redo information must be written before the corresponding data blocks.

---

## Example

```sql
UPDATE employees
SET salary = 80000
WHERE employee_id = 101;

COMMIT;
```

Flow:

```text
UPDATE
    │
Redo Generated
    │
Redo Buffer
    │
 LGWR
    │
Redo Log File
    │
COMMIT Successful
```

---

## Interview Question

### Which process writes Online Redo Log Files?

**Answer:**

LGWR (Log Writer)

---

# DBWR vs LGWR

| Feature | DBWR | LGWR |
|----------|------|------|
| Full Form | Database Writer | Log Writer |
| Writes | Data Blocks | Redo Entries |
| Source | Buffer Cache | Redo Log Buffer |
| Destination | Data Files | Online Redo Log Files |
| Main Purpose | Save modified data | Ensure recovery |
| Trigger | Checkpoint, Buffer Full, Shutdown | COMMIT, Every 3 Seconds, Redo Buffer Full |

---

# PMON vs SMON

| PMON | SMON |
|------|------|
| Process Monitor | System Monitor |
| Cleans failed sessions | Performs instance recovery |
| Releases locks | Cleans temporary segments |
| Frees session memory | Coalesces free space |
| Registers with Listener | Performs housekeeping |

---

# Complete Flow

```text
             User
               │
               ▼
      SQL Statement
               │
               ▼
      Server Process
               │
      ┌────────┴────────┐
      ▼                 ▼
Buffer Cache      Redo Log Buffer
      │                 │
      ▼                 ▼
    DBWR              LGWR
      │                 │
      ▼                 ▼
 Data Files      Redo Log Files
               │
               ▼
      Crash Recovery
               │
               ▼
             SMON

Session Crash
      │
      ▼
    PMON
```

---

# Real-Life Analogy

Imagine you are writing a document on your computer.

- **Buffer Cache** → RAM where you edit the document.
- **Redo Log Buffer** → Notebook recording every change you make.
- **DBWR** → Person who periodically saves the document to the hard disk.
- **LGWR** → Person who immediately records every change in a secure journal.
- **SMON** → Recovery expert who restores your work after a power failure.
- **PMON** → IT technician who closes crashed programs and frees system resources.

---

# Summary Table

| Process | Full Form | Main Responsibility |
|----------|-----------|---------------------|
| PMON | Process Monitor | Cleans failed sessions, releases locks, frees memory |
| SMON | System Monitor | Performs instance recovery and housekeeping |
| DBWR | Database Writer | Writes Dirty Blocks from Buffer Cache to Data Files |
| LGWR | Log Writer | Writes Redo Entries from Redo Buffer to Redo Log Files |

---

# Frequently Asked Interview Questions

### 1. What is PMON?

PMON cleans failed user sessions, releases locks, frees memory, and registers the database with the Listener.

---

### 2. What is SMON?

SMON performs instance recovery after database crashes and handles housekeeping tasks.

---

### 3. What is DBWR?

DBWR writes modified data blocks (Dirty Blocks) from the Database Buffer Cache to the Data Files.

---

### 4. What is LGWR?

LGWR writes Redo Entries from the Redo Log Buffer to the Online Redo Log Files.

---

### 5. What is a Dirty Block?

A Dirty Block is a modified block in the Buffer Cache that has not yet been written to the Data Files.

---

### 6. Which process performs Instance Recovery?

**SMON**

---

### 7. Which process writes Data Files?

**DBWR**

---

### 8. Which process writes Online Redo Log Files?

**LGWR**

---

### 9. Why does LGWR write before DBWR?

Because Oracle follows the **Write-Ahead Logging (WAL)** principle, ensuring redo information is safely stored before modified data blocks are written to disk.

---

### 10. What happens when a user session crashes?

PMON automatically detects the failure, cleans the session, releases locks, frees memory, and removes allocated resources.

---

# Key Points to Remember

- **PMON** → Cleans failed user sessions.
- **SMON** → Performs database recovery.
- **DBWR** → Writes Dirty Blocks to Data Files.
- **LGWR** → Writes Redo Entries to Redo Log Files.
- **Dirty Block** = Modified block in Buffer Cache.
- **Redo Logs** are essential for database recovery.
- Oracle uses **Write-Ahead Logging (WAL)** to guarantee data consistency.
