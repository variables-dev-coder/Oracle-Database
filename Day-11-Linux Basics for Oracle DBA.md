# Day 11 - Linux Basics for Oracle DBA

## Objective

Learn basic Linux commands required for Oracle DBA daily activities.

---

# Why Linux for Oracle DBA?

Most Oracle databases run on Linux servers.

As an Oracle DBA, you will:

* Navigate directories
* Create folders
* Manage files
* Check Oracle installation paths
* View logs
* Handle backups

---

# 1. pwd (Print Working Directory)

Displays the current directory path.

## Syntax

```bash
pwd
```

## Example

```bash
$ pwd

/home/oracle
```

## Output

```text
/home/oracle
```

## DBA Use Case

Check Oracle Home location.

```bash
pwd

/u01/app/oracle/product/19c/dbhome_1
```

---

# 2. ls (List Files and Directories)

Displays files and folders in the current directory.

## Syntax

```bash
ls
```

## Example

```bash
ls
```

Output:

```text
Documents
Downloads
backup
scripts
```

---

## ls -l

Displays detailed information.

```bash
ls -l
```

Example:

```text
drwxr-xr-x backup
-rw-r--r-- test.sql
```

---

## ls -a

Displays hidden files.

```bash
ls -a
```

Example:

```text
.
..
.bash_profile
.bashrc
```

---

## ls -lh

Displays file sizes in human-readable format.

```bash
ls -lh
```

Example:

```text
10K file1.txt
2M backup.dmp
1G database_backup.tar
```

---

# 3. cd (Change Directory)

Used to move between directories.

## Syntax

```bash
cd directory_name
```

## Example

```bash
cd Documents
```

---

## Move One Directory Back

```bash
cd ..
```

---

## Move to Home Directory

```bash
cd
```

or

```bash
cd ~
```

---

## Absolute Path

```bash
cd /u01/app/oracle
```

---

## Relative Path

```bash
cd backup
```

---

## DBA Use Case

```bash
cd /u01/app/oracle/product/19c/dbhome_1
```

---

# 4. mkdir (Make Directory)

Creates a new folder.

## Syntax

```bash
mkdir folder_name
```

## Example

```bash
mkdir backup
```

---

## Create Multiple Directories

```bash
mkdir scripts backups logs
```

---

## Create Nested Directories

```bash
mkdir -p project/day11/practice
```

Creates:

```text
project
└── day11
    └── practice
```

---

## DBA Use Case

```bash
mkdir /backup/rman
```

---

# 5. touch

Creates an empty file.

## Syntax

```bash
touch filename
```

## Example

```bash
touch test.txt
```

---

## Create Multiple Files

```bash
touch file1.txt file2.txt file3.txt
```

---

## DBA Use Case

```bash
touch create_user.sql
touch backup_script.sh
touch monitoring.sql
```

---

# 6. rm (Remove)

Deletes files and directories.

⚠️ Deleted files cannot be recovered easily.

---

## Delete File

```bash
rm test.txt
```

---

## Delete Multiple Files

```bash
rm file1.txt file2.txt
```

---

## Delete Empty Directory

```bash
rmdir folder_name
```

Example:

```bash
rmdir test
```

---

## Delete Directory with Contents

```bash
rm -r folder_name
```

Example:

```bash
rm -r backup
```

---

## Force Delete

```bash
rm -rf backup
```

⚠️ Very dangerous command.

Never execute:

```bash
rm -rf /
```

This can remove the entire Linux system.

---

# Important Linux Directories for Oracle DBA

```text
/

├── bin
├── boot
├── dev
├── etc
├── home
├── tmp
├── usr
├── var
└── u01
```

---

## /home

User home directories.

Example:

```text
/home/oracle
```

---

## /etc

Configuration files.

Example:

```text
/etc/passwd
```

---

## /tmp

Temporary files.

Example:

```text
/tmp
```

---

## /var

System log files.

Example:

```text
/var/log
```

---

## /u01

Oracle software installation path.

Example:

```text
/u01/app/oracle/product/19c/dbhome_1
```

---

# Practice Lab

## Step 1

Check current directory.

```bash
pwd
```

---

## Step 2

Create Day11 folder.

```bash
mkdir Day11
```

---

## Step 3

Enter Day11 folder.

```bash
cd Day11
```

---

## Step 4

Create practice directories.

```bash
mkdir scripts backups logs
```

---

## Step 5

Verify directories.

```bash
ls
```

Expected Output:

```text
scripts
backups
logs
```

---

## Step 6

Create files.

```bash
touch scripts/create_user.sql
touch scripts/backup.sql
touch logs/day11.log
```

---

## Step 7

Verify structure.

```bash
ls -R
```

Expected Output:

```text
scripts
backups
logs

scripts:
create_user.sql
backup.sql

logs:
day11.log
```

---

## Step 8

Delete log file.

```bash
rm logs/day11.log
```

---

## Step 9

Verify deletion.

```bash
ls -R
```

---

# Interview Questions

## Q1. What does pwd do?

Displays the current working directory.

---

## Q2. Difference between Absolute Path and Relative Path?

### Absolute Path

Starts from root directory (/).

Example:

```bash
/u01/app/oracle
```

### Relative Path

Starts from the current directory.

Example:

```bash
backup
```

---

## Q3. How to create multiple directories?

```bash
mkdir dir1 dir2 dir3
```

---

## Q4. How to create an empty file?

```bash
touch file.txt
```

---

## Q5. How to delete a directory with files?

```bash
rm -r directory_name
```

---

# Quick Revision

| Command | Purpose                |
| ------- | ---------------------- |
| pwd     | Show current directory |
| ls      | List files and folders |
| ls -l   | Detailed file list     |
| ls -a   | Show hidden files      |
| cd      | Change directory       |
| mkdir   | Create directory       |
| touch   | Create file            |
| rm      | Delete file            |
| rm -r   | Delete directory       |
| rm -rf  | Force delete directory |

---

# Day 11 Outcome

After completing Day 11, you should be able to:

✅ Navigate Linux directories

✅ View files and folders

✅ Create directories

✅ Create files

✅ Delete files and folders

✅ Understand Linux directory structure

✅ Perform basic Linux operations required for Oracle DBA
