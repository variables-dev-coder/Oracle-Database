# Day 19 – Shell Scripting Basics (Oracle DBA)

## 📚 Learning Objectives

By the end of this lesson, you will be able to:

* Understand what Shell Scripting is.
* Understand why Oracle DBAs use Shell Scripts.
* Create and execute Shell Scripts.
* Use variables in Shell Scripts.
* Accept user input using `read`.
* Display system information.
* Create a simple backup script.
* Create a timestamp-based backup script.
* Write basic automation scripts for Oracle DBA tasks.

---

# What is Shell Scripting?

A **Shell Script** is a text file containing Linux commands that are executed sequentially by the shell.

Instead of typing the same commands repeatedly, you write them once in a script and execute the script whenever needed.

A shell script is simply a file with a `.sh` extension containing Bash commands.

Example:

```bash
#!/bin/bash

echo "Hello Oracle DBA"
date
pwd
```

---

# Why Oracle DBAs Use Shell Scripts?

Oracle DBAs perform many repetitive administrative tasks every day.

Instead of manually executing commands, DBAs automate them using Shell Scripts.

### Common Oracle DBA Automation Tasks

* Database Backup
* RMAN Backup
* Archive Log Cleanup
* Alert Log Monitoring
* Listener Status Check
* Disk Usage Monitoring
* Memory Monitoring
* Database Health Check
* Compress Backup Files
* Delete Old Backups

Automation saves time, reduces manual errors, and ensures consistency.

---

# What is Bash?

Bash stands for:

> **Bourne Again Shell**

It is the default shell in most Linux distributions, including Oracle Linux.

Check the current shell:

```bash
echo $SHELL
```

Example Output

```text
/bin/bash
```

---

# Shebang (`#!/bin/bash`)

Every Shell Script should start with:

```bash
#!/bin/bash
```

This is called the **Shebang**.

It tells Linux which interpreter should execute the script.

Without the shebang, Linux may use another shell, causing unexpected behavior.

Always keep it as the first line of the script.

---

# Creating Your First Shell Script

Create a script:

```bash
nano hello.sh
```

Write:

```bash
#!/bin/bash

echo "Hello Oracle DBA"
echo "Welcome to Shell Scripting"
```

Save the file.

---

# Make Script Executable

New scripts don't have execute permission.

Grant execute permission:

```bash
chmod +x hello.sh
```

Check permission:

```bash
ls -l hello.sh
```

Example:

```text
-rwxr-xr-x
```

Here:

* r = Read
* w = Write
* x = Execute

---

# Running a Shell Script

Method 1 (Recommended)

```bash
./hello.sh
```

Method 2

```bash
bash hello.sh
```

Method 3

```bash
sh hello.sh
```

Output

```text
Hello Oracle DBA
Welcome to Shell Scripting
```

---

# echo Command

The `echo` command prints text or variable values on the terminal.

Example:

```bash
echo "Hello World"
```

Output

```text
Hello World
```

Example with variables:

```bash
name="Munna"

echo $name
```

Output

```text
Munna
```

---

# Variables in Shell Script

Variables store values that can be reused throughout the script.

Syntax:

```bash
variable_name=value
```

Example:

```bash
company="Oracle"

echo $company
```

Output

```text
Oracle
```

**Important Rules**

Correct:

```bash
name="Oracle"
```

Wrong:

```bash
name = "Oracle"
```

No spaces are allowed around `=`.

---

# User Input (`read`)

The `read` command accepts input from the user.

Example

```bash
#!/bin/bash

echo "Enter your name"

read name

echo "Welcome $name"
```

Output

```text
Enter your name

Munna

Welcome Munna
```

Alternative syntax

```bash
read -p "Enter Name: " name
```

---

# Comments

Comments are ignored during execution.

Used for documentation.

Single-line comment:

```bash
# This is a comment
```

Example

```bash
#!/bin/bash

# Display today's date

date
```

---

# Display Current Date

```bash
date
```

Script Example

```bash
#!/bin/bash

echo "Today's Date"

date
```

---

# Display Current Directory

```bash
pwd
```

Script Example

```bash
#!/bin/bash

echo "Current Directory"

pwd
```

---

# Display Hostname

```bash
hostname
```

Example

```bash
#!/bin/bash

echo "Hostname"

hostname
```

---

# Display Current User

```bash
whoami
```

Example

```bash
#!/bin/bash

echo "Current User"

whoami
```

---

# Disk Usage

Display available disk space.

Command

```bash
df -h
```

Explanation

| Option | Meaning         |
| ------ | --------------- |
| df     | Disk Filesystem |
| -h     | Human Readable  |

Example Output

```text
Filesystem      Size Used Avail Use%
```

Oracle DBAs frequently use this command before backups.

---

# Memory Usage

Display memory usage.

Command

```bash
free -m
```

Explanation

| Option | Meaning        |
| ------ | -------------- |
| free   | Display memory |
| -m     | Display in MB  |

Example Output

```text
Mem:
Swap:
```

Useful for checking server memory before database operations.

---

# Combining Multiple Commands

Example System Information Script

```bash
#!/bin/bash

echo "===== SYSTEM INFORMATION ====="

echo

echo "Date:"
date

echo

echo "Current Directory:"
pwd

echo

echo "Hostname:"
hostname

echo

echo "Current User:"
whoami

echo

echo "Disk Usage:"
df -h

echo

echo "Memory Usage:"
free -m
```

This script is commonly used by DBAs to collect server information.

---

# Exit Status (`echo $?`)

Every Linux command returns an exit status.

Display exit status:

```bash
echo $?
```

Example

```bash
ls

echo $?
```

Output

```text
0
```

Meaning:

Success.

Failed command

```bash
abcd

echo $?
```

Output

```text
127
```

### Common Exit Codes

| Exit Code | Meaning                 |
| --------- | ----------------------- |
| 0         | Success                 |
| 1         | General Error           |
| 2         | Incorrect Command Usage |
| 126       | Permission Denied       |
| 127       | Command Not Found       |

Checking exit status helps DBAs verify whether automation scripts completed successfully.

---
