# Day 25 – Oracle DBA
# Practice: Check Oracle Processes in Linux

## Objective

Today you will learn how to check whether an Oracle Database is running and verify its background processes from Linux. This is one of the first health checks performed by an Oracle DBA.

---

# Why Check Oracle Processes?

Checking Oracle processes helps you:

- Verify that the database instance is running.
- Ensure important background processes are active.
- Check whether the Oracle Listener is running.
- Troubleshoot database startup issues.
- Monitor database health.

---

# Oracle Background Processes

| Process | Description |
|----------|-------------|
| **PMON** | Process Monitor – Cleans up failed user processes and releases resources. |
| **SMON** | System Monitor – Performs instance recovery and cleanup. |
| **DBWR (DBW0)** | Database Writer – Writes modified data from memory to datafiles. |
| **LGWR** | Log Writer – Writes redo entries from memory to redo log files. |
| **CKPT** | Checkpoint Process – Updates datafile headers and control files during checkpoints. |
| **ARCn** | Archiver Process – Copies filled redo logs to archive logs (Archive Log Mode). |
| **MMON** | Manageability Monitor – Collects performance statistics. |
| **MMNL** | Memory Monitor Light – Writes AWR-related statistics. |

---

# Important Linux Commands

## 1. View All Oracle Processes

```bash
ps -ef | grep ora_
```

Example Output

```text
oracle 1023 1 0 ? 00:00:00 ora_pmon_ORCL
oracle 1025 1 0 ? 00:00:00 ora_smon_ORCL
oracle 1027 1 0 ? 00:00:01 ora_dbw0_ORCL
oracle 1030 1 0 ? 00:00:02 ora_lgwr_ORCL
oracle 1032 1 0 ? 00:00:00 ora_ckpt_ORCL
oracle 1034 1 0 ? 00:00:01 ora_arc0_ORCL
```

**Purpose**

Displays all Oracle background processes currently running.

---

## 2. Check PMON

```bash
ps -ef | grep pmon
```

Example

```text
oracle 1023 1 0 ? 00:00:00 ora_pmon_ORCL
```

**Purpose**

PMON confirms that the Oracle instance is running.

---

## 3. Check SMON

```bash
ps -ef | grep smon
```

---

## 4. Check DBWR

```bash
ps -ef | grep dbw
```

---

## 5. Check LGWR

```bash
ps -ef | grep lgwr
```

---

# Check Oracle Listener

## Find Listener Process

```bash
ps -ef | grep tnslsnr
```

Example

```text
oracle 980 1 0 ? 00:00:01 tnslsnr LISTENER
```

---

## Check Listener Status

```bash
lsnrctl status
```

Example

```text
Service "ORCL" has 1 instance(s)

Instance "ORCL", status READY
```

**Meaning**

- Listener is running.
- Database service is registered.
- Client connections are accepted.

---

# Check Oracle SID

```bash
echo $ORACLE_SID
```

Example

```text
ORCL
```

**Purpose**

Displays the Oracle System Identifier (SID).

---

# Verify Database Using SQL*Plus

Connect as SYSDBA

```bash
sqlplus / as sysdba
```

---

## Check Instance Status

```sql
SELECT INSTANCE_NAME, STATUS
FROM V$INSTANCE;
```

Example

```text
INSTANCE_NAME   STATUS
--------------  ------
ORCL            OPEN
```

---

## Check Database Status

```sql
SELECT NAME, OPEN_MODE
FROM V$DATABASE;
```

Example

```text
NAME   OPEN_MODE
----   ----------
ORCL   READ WRITE
```

---

# Check Active Background Processes

```sql
SELECT NAME
FROM V$BGPROCESS
WHERE PADDR <> '00';
```

Example

```text
PMON
SMON
DBW0
LGWR
CKPT
ARC0
MMON
MMNL
```

---

# Count Oracle Processes

```bash
ps -ef | grep ora_ | wc -l
```

Example

```text
12
```

---

# Find Process ID (PID)

```bash
ps -ef | grep pmon
```

Example

```text
oracle 1023 1 0 ? 00:00:00 ora_pmon_ORCL
```

Here,

- **1023** → Process ID (PID)
- **ora_pmon_ORCL** → Oracle PMON Process

---

# Monitor Oracle Processes Continuously

```bash
watch -n 2 "ps -ef | grep ora_"
```

Refreshes every **2 seconds**.

Stop monitoring:

```text
Ctrl + C
```

---

# Useful Monitoring Commands

## top

```bash
top
```

Displays CPU and memory usage of running Oracle processes.

Exit

```text
q
```

---

## htop (if installed)

```bash
htop
```

Search

```text
F3
```

Type

```text
oracle
```

Exit

```text
F10
```

---

# Practice Commands

```bash
ps -ef | grep ora_

ps -ef | grep pmon

ps -ef | grep smon

ps -ef | grep dbw

ps -ef | grep lgwr

ps -ef | grep tnslsnr

lsnrctl status

echo $ORACLE_SID

sqlplus / as sysdba

ps -ef | grep ora_ | wc -l
```

---

# Important SQL Queries

```sql
SELECT INSTANCE_NAME, STATUS
FROM V$INSTANCE;
```

```sql
SELECT NAME, OPEN_MODE
FROM V$DATABASE;
```

```sql
SELECT NAME
FROM V$BGPROCESS
WHERE PADDR <> '00';
```

---

# Interview Questions

### 1. Which Linux command displays Oracle background processes?

```bash
ps -ef | grep ora_
```

---

### 2. Which process confirms that an Oracle instance is running?

**Answer:** PMON (`ora_pmon_<SID>`)

---

### 3. Which command checks Oracle Listener status?

```bash
lsnrctl status
```

---

### 4. How do you check the Oracle SID?

```bash
echo $ORACLE_SID
```

---

### 5. Which SQL view displays Oracle background processes?

**Answer**

```sql
V$BGPROCESS
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `ps -ef \| grep ora_` | Display all Oracle background processes |
| `ps -ef \| grep pmon` | Verify Oracle instance is running |
| `ps -ef \| grep smon` | Check SMON process |
| `ps -ef \| grep dbw` | Check DBWR process |
| `ps -ef \| grep lgwr` | Check LGWR process |
| `ps -ef \| grep tnslsnr` | Check Listener process |
| `lsnrctl status` | Display Listener status |
| `echo $ORACLE_SID` | Show Oracle SID |
| `sqlplus / as sysdba` | Connect as SYSDBA |
| `SELECT INSTANCE_NAME, STATUS FROM V$INSTANCE;` | Check instance status |
| `SELECT NAME, OPEN_MODE FROM V$DATABASE;` | Check database open mode |
| `SELECT NAME FROM V$BGPROCESS WHERE PADDR <> '00';` | List active background processes |
| `ps -ef \| grep ora_ \| wc -l` | Count Oracle processes |

---

# Key Points

- Oracle processes run under the **oracle** user.
- **PMON** is the easiest process to verify a running Oracle instance.
- Use **ps -ef | grep ora_** to view all Oracle background processes.
- **Listener (tnslsnr)** must be running for remote client connections.
- **lsnrctl status** confirms the Listener and registered database services.
- Use **V$INSTANCE** to check instance status.
- Use **V$DATABASE** to verify the database is open.
- Use **V$BGPROCESS** to view active Oracle background processes.

---

# Day 25 Summary

Today you learned how to:

- Check Oracle background processes in Linux.
- Verify PMON, SMON, DBWR, LGWR, CKPT, and ARCn processes.
- Check whether the Oracle Listener is running.
- Verify Oracle SID.
- Connect to SQL*Plus as SYSDBA.
- Check instance and database status.
- Display active background processes.
- Monitor Oracle processes using Linux utilities.
