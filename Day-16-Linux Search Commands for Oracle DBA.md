# Day 16 – Linux Search Commands (`grep` & `find`) | Oracle DBA Notes

## Objective

Learn how to search text inside files and locate files/directories in Linux using:

- `grep` → Search text inside files
- `find` → Search files and directories

These are essential commands for Oracle DBAs to analyze logs, locate configuration files, and troubleshoot issues.

---

# 1. grep Command

## What is grep?

`grep` searches for a specific word, text, or pattern inside one or more files.

### Syntax

```bash
grep "pattern" filename
```

### Example

Create a log file:

```bash
nano app.log
```

Contents:

```text
Database started successfully
User connected
ORA-12514 error occurred
Backup completed
```

Search for Oracle errors:

```bash
grep "ORA" app.log
```

Output:

```text
ORA-12514 error occurred
```

---

# Common grep Options

## Ignore Case (-i)

Matches text regardless of uppercase or lowercase.

```bash
grep -i "oracle" file.txt
```

Matches:

```text
oracle
Oracle
ORACLE
```

---

## Show Line Numbers (-n)

```bash
grep -n "ORA-" alert.log
```

Example Output:

```text
25:ORA-12514 error occurred
78:ORA-01017 Invalid username/password
```

---

## Count Matches (-c)

```bash
grep -c "ORA-" alert.log
```

Example Output:

```text
15
```

---

## Invert Match (-v)

Displays lines that do NOT contain the given text.

```bash
grep -v "INFO" app.log
```

---

## Recursive Search (-r)

Search inside all files within a directory.

```bash
grep -r "ORA-" /oracle/logs
```

---

# Oracle DBA Examples

Search Oracle errors:

```bash
grep "ORA-" alert.log
```

Search Listener errors:

```bash
grep "TNS" listener.log
```

Search failed login attempts:

```bash
grep "invalid" alert.log
```

---

# 2. find Command

## What is find?

`find` searches for files and directories based on conditions like:

- Name
- Type
- Size
- Date
- Permissions

### Syntax

```bash
find <location> <condition>
```

---

# Find File by Name

```bash
find . -name "test.txt"
```

Output:

```text
./notes/test.txt
```

`.` = Current directory

---

# Find Oracle Alert Log

```bash
find /u01 -name "alert*.log"
```

Example Output:

```text
/u01/app/oracle/diag/rdbms/orcl/trace/alert_orcl.log
```

---

# Find All Log Files

```bash
find . -name "*.log"
```

---

# Find Only Directories

```bash
find . -type d
```

---

# Find Only Files

```bash
find . -type f
```

---

# Find Large Files

Find files larger than 100 MB:

```bash
find . -size +100M
```

---

# Find Empty Files

```bash
find . -empty
```

---

# Find Recently Modified Files

Modified within the last 1 day:

```bash
find . -mtime -1
```

Modified within the last 7 days:

```bash
find . -mtime -7
```

---

# Using grep and find Together

Find all log files and search for Oracle errors:

```bash
find . -name "*.log" | xargs grep "ORA-"
```

---

# Real Oracle DBA Commands

## Find Alert Log

```bash
find /u01 -name "alert*.log"
```

---

## Search Oracle Errors

```bash
grep "ORA-" alert_orcl.log
```

---

## Search Listener Errors

```bash
grep "TNS" listener.log
```

---

## Find Backup Files

```bash
find /backup -name "*.bkp"
```

---

## Find Files Larger Than 1 GB

```bash
find /backup -size +1G
```

---

# Practice Exercises

## Task 1

Create a practice directory and files:

```bash
mkdir day16
cd day16

touch app.log
touch alert.log
touch backup.log
```

---

## Task 2

Edit `alert.log`

```bash
nano alert.log
```

Add:

```text
ORA-01017 Invalid username/password
ORA-12514 Listener error
Database opened
```

---

## Task 3

Search Oracle errors:

```bash
grep "ORA-" alert.log
```

---

## Task 4

Display line numbers:

```bash
grep -n "ORA-" alert.log
```

---

## Task 5

Find all log files:

```bash
find . -name "*.log"
```

---

## Task 6

Find files modified today:

```bash
find . -mtime -1
```

---

# grep vs find

| Feature | grep | find |
|----------|------|------|
| Purpose | Search text inside files | Search files/directories |
| Works On | File contents | File names and metadata |
| Output | Matching lines | Matching file paths |
| Common DBA Use | Search ORA/TNS errors | Locate logs, backups, config files |

---

# Interview Questions

## Q1. What is the difference between `grep` and `find`?

- `grep` searches **inside files** for text.
- `find` searches **files and directories** based on conditions.

---

## Q2. Search Oracle errors in an alert log.

```bash
grep "ORA-" alert.log
```

---

## Q3. Find all `.log` files.

```bash
find . -name "*.log"
```

---

## Q4. Search text ignoring case.

```bash
grep -i "oracle" file.txt
```

---

## Q5. Find files larger than 500 MB.

```bash
find . -size +500M
```

---

# Quick Revision

## grep

| Command | Description |
|----------|-------------|
| `grep "text" file` | Search text |
| `grep -i` | Ignore case |
| `grep -n` | Show line numbers |
| `grep -c` | Count matches |
| `grep -v` | Exclude matching lines |
| `grep -r` | Search recursively |

---

## find

| Command | Description |
|----------|-------------|
| `find . -name "file"` | Find by name |
| `find . -type f` | Files only |
| `find . -type d` | Directories only |
| `find . -size +100M` | Large files |
| `find . -mtime -1` | Modified within 1 day |
| `find . -empty` | Empty files |

---

# Oracle DBA Takeaways

- Use `grep` to quickly locate Oracle errors (`ORA-`), Listener errors (`TNS`), and login failures in log files.
- Use `find` to locate alert logs, listener logs, backup files, and configuration files.
- Combine `find` and `grep` to search all log files for specific errors efficiently.

---

# Day 16 Summary

✅ Learned the `grep` command to search text inside files.

✅ Learned the `find` command to locate files and directories.

✅ Practiced searching Oracle log files and locating backup files.

✅ Learned to combine `find` and `grep` for powerful Linux searches used in Oracle DBA administration.
