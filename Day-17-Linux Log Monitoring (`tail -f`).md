# Day 17 – Linux Log Monitoring (`tail -f`)

## Objective

Learn how to monitor log files in real time using the `tail` command, especially `tail -f`, an essential command for Oracle DBAs and Linux administrators.

---

# Why Log Monitoring is Important?

Logs help administrators monitor:

- Database startup and shutdown
- Application activity
- User logins
- Errors and warnings
- Backup status
- Security events

As an Oracle DBA, checking logs is often the first step in troubleshooting production issues.

---

# The `tail` Command

The `tail` command displays the last lines of a file.

### Syntax

```bash
tail filename
```

### Example

```bash
tail app.log
```

### Output

```text
Server Started
Database Connected
Backup Completed
User Login
```

**Default Behavior:**

- Displays the **last 10 lines** of a file.

---

# Display the Last N Lines

### Syntax

```bash
tail -n <number> filename
```

### Example

```bash
tail -n 5 app.log
```

Displays only the **last 5 lines**.

---

# Live Log Monitoring with `tail -f`

The `-f` option means **follow**.

It continuously monitors a file and displays new lines as soon as they are added.

### Syntax

```bash
tail -f filename
```

### Example

```bash
tail -f app.log
```

If another process writes:

```text
User Login
Payment Success
User Logout
```

These lines immediately appear in your terminal.

---

# Real-Time Monitoring Example

### Terminal 1

```bash
tail -f app.log
```

### Terminal 2

```bash
echo "Server Started" >> app.log
echo "Database Connected" >> app.log
echo "User Login" >> app.log
```

### Terminal 1 Output

```text
Server Started
Database Connected
User Login
```

No need to refresh the screen.

---

# Oracle DBA Example

Monitor the Oracle Alert Log:

```bash
tail -f alert_orcl.log
```

Possible Output:

```text
Database Mounted
Database Open
Checkpoint Complete
Archive Log Generated
```

If an error occurs:

```text
ORA-00600 Internal Error
```

You can detect issues immediately.

---

# Monitor Authentication Logs

```bash
tail -f auth.log
```

Example Output:

```text
Accepted password
Session Opened
User Login
```

Useful for security monitoring.

---

# Monitor Web Server Logs

```bash
tail -f access.log
```

Example Output:

```text
GET /
POST /login
GET /products
```

Useful for monitoring website traffic.

---

# Stop Live Monitoring

Press:

```text
Ctrl + C
```

to stop `tail -f`.

---

# Difference Between `cat` and `tail`

| Command | Description |
|----------|-------------|
| `cat file` | Displays the entire file |
| `tail file` | Displays the last 10 lines |
| `tail -n 20 file` | Displays the last 20 lines |
| `tail -f file` | Continuously monitors new log entries |

---

# Practice Lab

## Step 1: Create a Directory

```bash
mkdir day17
cd day17
```

---

## Step 2: Create a Log File

```bash
touch app.log
```

---

## Step 3: Start Monitoring

Open Terminal 1:

```bash
tail -f app.log
```

---

## Step 4: Write Logs

Open Terminal 2:

```bash
echo "Server Started" >> app.log
echo "Database Connected" >> app.log
echo "User Login" >> app.log
echo "Payment Success" >> app.log
echo "User Logout" >> app.log
```

Observe the output in Terminal 1 updating automatically.

---

# Common Commands

Display last 10 lines:

```bash
tail app.log
```

Display last 20 lines:

```bash
tail -n 20 app.log
```

Monitor file continuously:

```bash
tail -f app.log
```

Append a new log:

```bash
echo "Backup Completed" >> app.log
```

Stop monitoring:

```text
Ctrl + C
```

---

# Interview Questions

### 1. What does the `tail` command do?

**Answer:**
Displays the last 10 lines of a file by default.

---

### 2. What does `tail -f` do?

**Answer:**
Continuously monitors a file and displays new lines as they are appended.

---

### 3. How do you display the last 25 lines of a file?

```bash
tail -n 25 filename
```

---

### 4. How do you stop `tail -f`?

**Answer:**

Press:

```text
Ctrl + C
```

---

### 5. Why do Oracle DBAs use `tail -f`?

**Answer:**

To monitor:

- Oracle Alert Logs
- Listener Logs
- Application Logs
- Security Logs

in real time for troubleshooting and system monitoring.

---

# Key Points

- `tail` displays the last **10 lines** of a file.
- `tail -n N` displays the last **N lines**.
- `tail -f` follows a file and shows new log entries in real time.
- Press **Ctrl + C** to stop monitoring.
- `tail -f` is one of the most frequently used Linux commands by Oracle DBAs in production environments.

---

# Day 17 Summary

✅ Learned the `tail` command

✅ Viewed the last lines of a log file

✅ Monitored log files in real time using `tail -f`

✅ Practiced live log monitoring

✅ Understood real-world Oracle DBA log monitoring scenarios
