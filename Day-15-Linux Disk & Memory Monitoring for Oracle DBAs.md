# Day 15 - Linux Disk & Memory Monitoring for Oracle DBAs

## Objective

Learn how to monitor:

* Disk Space
* Directory Size
* Memory Usage

Commands:

```bash
df -h
du -sh
free -m
```

---

# Why DBAs Monitor Disk & Memory

Oracle databases continuously generate:

* Data Files
* Redo Logs
* Archive Logs
* Backup Files
* Temporary Files

If disk space becomes full:

* Database may stop
* Transactions may fail
* Archive logs may not generate
* Backups may fail

If memory becomes low:

* Queries become slow
* Swap usage increases
* Database performance degrades

---

# 1. df -h

## Purpose

Displays filesystem disk usage.

## Syntax

```bash
df -h
```

## Example Output

```bash
Filesystem      Size Used Avail Use% Mounted on
/dev/sda1        50G  20G   28G  42% /
tmpfs           2.0G    0   2.0G   0% /dev/shm
```

## Column Meaning

| Column     | Description     |
| ---------- | --------------- |
| Filesystem | Disk Partition  |
| Size       | Total Space     |
| Used       | Used Space      |
| Avail      | Available Space |
| Use%       | Percentage Used |
| Mounted on | Mount Location  |

## DBA Usage

Check if storage is running out:

```bash
df -h
```

If usage reaches 90%+:

```text
Immediate investigation required.
```

---

# 2. du -sh

## Purpose

Displays size of a directory.

## Syntax

```bash
du -sh directory_name
```

## Example

```bash
du -sh backups
```

Output:

```bash
5.2G backups
```

Meaning:

```text
The backups directory uses 5.2 GB.
```

## Options

### -s

Summary only

```bash
du -s backups
```

### -h

Human-readable format

```bash
du -sh backups
```

Displays:

```text
KB, MB, GB, TB
```

instead of bytes.

## Find Large Directories

```bash
du -sh *
```

Example:

```bash
2G backups
500M scripts
8G archive_logs
```

Useful for locating storage-consuming folders.

---

# Difference Between df and du

## df -h

Shows filesystem usage.

```bash
df -h
```

Example:

```text
Disk Size = 100GB
Used = 70GB
Free = 30GB
```

## du -sh

Shows directory usage.

```bash
du -sh backups
```

Example:

```text
backups = 10GB
```

## Quick Rule

```text
df = How much disk is used?
du = Which folder is using it?
```

---

# 3. free -m

## Purpose

Displays memory usage.

## Syntax

```bash
free -m
```

## Example Output

```bash
              total   used   free
Mem:           8000   3000   5000
Swap:          2000      0   2000
```

## Column Meaning

| Column | Description    |
| ------ | -------------- |
| total  | Total RAM      |
| used   | Used RAM       |
| free   | Available RAM  |
| swap   | Virtual Memory |

---

# What is Swap?

When RAM becomes full, Linux uses disk space as temporary memory.

This is called:

```text
Swap Memory
```

Example:

```bash
Swap: 2000 500 1500
```

Meaning:

```text
500 MB swap is currently being used.
```

Heavy swap usage may indicate memory pressure.

---

# Why Oracle DBAs Check Memory

Oracle uses memory for:

* SGA (System Global Area)
* PGA (Program Global Area)
* Buffer Cache
* Shared Pool

Low memory can cause:

* Slow queries
* High response times
* Increased swap usage

Check regularly:

```bash
free -m
```

---

# Real DBA Troubleshooting Scenario

## User Complaint

```text
Database is slow.
```

### Step 1

Check memory:

```bash
free -m
```

Result:

```text
95% RAM utilized
```

### Step 2

Check disk:

```bash
df -h
```

Result:

```text
Disk 98% full
```

### Step 3

Find large directories:

```bash
du -sh *
```

Result:

```text
archive_logs = 40GB
```

### Step 4

Clean old archive logs or perform backup.

Problem resolved.

---

# Practice Exercises

## Check Disk Space

```bash
df -h
```

## Create Test Directories

```bash
mkdir backups
mkdir archive_logs
```

## Check Directory Size

```bash
du -sh backups
du -sh archive_logs
```

## Check Memory Usage

```bash
free -m
```

## Run All Commands Together

```bash
df -h
du -sh *
free -m
```

---

# Interview Questions

### Q1: Difference between df and du?

**Answer:**

```text
df shows filesystem usage.
du shows directory/file usage.
```

---

### Q2: Which command checks memory usage?

```bash
free -m
```

---

### Q3: What does -h mean in df -h?

**Answer:**

```text
Human-readable format (KB, MB, GB).
```

---

### Q4: What is Swap?

**Answer:**

```text
Disk space used as virtual memory when RAM becomes full.
```

---

### Q5: Which command identifies large directories?

```bash
du -sh *
```

---

# Day 15 Summary

```text
df -h     → Check filesystem disk usage
du -sh    → Check directory size
free -m   → Check memory usage

DBA Checklist:
✓ Monitor disk usage daily
✓ Check archive log growth
✓ Monitor RAM usage
✓ Watch swap usage
✓ Prevent disk from reaching 100%
```
