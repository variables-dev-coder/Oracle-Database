# Day 14 - Linux Process Management

## Objective

Learn how to monitor and manage running processes in Linux.

Commands Covered:

* `ps`
* `top`
* `kill`

Practice:

* Monitor running processes
* Identify Process IDs (PID)
* Stop unwanted processes

---

# What is a Process?

A **process** is a program currently running in memory.

Examples:

* Oracle Database
* SQL*Plus
* Java Application
* Chrome Browser
* RMAN Backup

Each process has a unique identifier called:

**PID (Process ID)**

Example:

```text
PID   COMMAND
1234  oracle
5678  java
9012  chrome
```

---

# ps Command

Displays currently running processes.

## Syntax

```bash
ps
```

Example:

```bash
ps
```

Output:

```text
PID TTY          TIME CMD
1023 pts/0    00:00:00 bash
2050 pts/0    00:00:00 ps
```

---

## Detailed Process List

```bash
ps -ef
```

Example:

```text
UID       PID   PPID   CMD
root        1      0   /sbin/init
oracle   2100      1   ora_pmon_ORCL
oracle   2101      1   ora_smon_ORCL
```

### Important Columns

| Column | Description         |
| ------ | ------------------- |
| UID    | User owning process |
| PID    | Process ID          |
| PPID   | Parent Process ID   |
| CMD    | Command Name        |

---

## Search Specific Process

Find Oracle processes:

```bash
ps -ef | grep oracle
```

Find PMON process:

```bash
ps -ef | grep pmon
```

Find Listener:

```bash
ps -ef | grep tnslsnr
```

Find Java process:

```bash
ps -ef | grep java
```

---

# top Command

Displays real-time system information.

## Syntax

```bash
top
```

Shows:

* CPU Usage
* Memory Usage
* Running Processes
* Process Statistics

Example:

```text
PID USER %CPU %MEM COMMAND

2450 oracle 30.0 5.1 oracle
3345 root    5.2 1.0 java
```

---

## Useful Keys in top

### Sort by CPU Usage

Press:

```text
P
```

### Sort by Memory Usage

Press:

```text
M
```

### Quit top

Press:

```text
q
```

---

# kill Command

Used to terminate a running process.

## Syntax

```bash
kill PID
```

Example:

```bash
kill 2450
```

---

## Force Kill

If a process does not stop normally:

```bash
kill -9 PID
```

Example:

```bash
kill -9 2450
```

### Difference

| Command     | Meaning              |
| ----------- | -------------------- |
| kill PID    | Graceful termination |
| kill -9 PID | Force termination    |

Always try:

```bash
kill PID
```

before using:

```bash
kill -9 PID
```

---

# Useful Process Commands

## Show User Processes

```bash
ps -u $USER
```

---

## Display Process Tree

```bash
pstree
```

Example:

```text
systemd
 ├── sshd
 ├── oracle
 └── java
```

---

## Find PID of a Program

```bash
pidof java
```

Example:

```text
2450
```

---

## Check Memory Usage

```bash
free -m
```

Example:

```text
total    used    free
```

Shows RAM utilization in MB.

---

# Oracle DBA Commands

## Check Oracle Database Process

```bash
ps -ef | grep pmon
```

---

## Check All Oracle Processes

```bash
ps -ef | grep ora_
```

---

## Check Oracle Listener

```bash
ps -ef | grep tnslsnr
```

---

## Monitor CPU Usage

```bash
top
```

Press:

```text
P
```

to sort by CPU.

---

## Kill Stuck RMAN Job

Find Process:

```bash
ps -ef | grep rman
```

Kill Process:

```bash
kill PID
```

---

# Practice Lab

## Step 1

Start a process:

```bash
sleep 300
```

---

## Step 2

Find the process:

```bash
ps -ef | grep sleep
```

---

## Step 3

Monitor processes:

```bash
top
```

Exit using:

```text
q
```

---

## Step 4

Kill the process:

```bash
kill PID
```

---

## Step 5

Verify process termination:

```bash
ps -ef | grep sleep
```

---

# Interview Questions

### Q1. What is PID?

PID stands for Process ID, a unique number assigned to each running process.

### Q2. How do you check if Oracle Database is running?

```bash
ps -ef | grep pmon
```

### Q3. Difference between kill and kill -9?

| kill           | kill -9    |
| -------------- | ---------- |
| Graceful stop  | Force stop |
| Allows cleanup | No cleanup |
| Safer          | Riskier    |

### Q4. Which command shows real-time CPU and memory usage?

```bash
top
```

---

# Quick Revision

```bash
ps                 # Show processes

ps -ef             # Detailed process list

top                # Real-time monitoring

kill PID           # Stop process

kill -9 PID        # Force stop process

pstree             # Process hierarchy

pidof process      # Find PID

free -m            # Memory usage
```

---

# Day 14 Summary

* Learned Linux Processes
* Used `ps` to view processes
* Used `top` for real-time monitoring
* Used `kill` to terminate processes
* Learned Oracle DBA process monitoring commands
* Practiced identifying and killing running processes

**Next Day (Day 15): Linux Disk Management**
