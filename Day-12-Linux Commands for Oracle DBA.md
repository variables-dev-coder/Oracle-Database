# Day 12 - Linux Commands for Oracle DBA

## Objective
Learn essential Linux file management commands used by Oracle DBAs.

Commands Covered:
- cp
- mv
- cat
- nano

---

# 1. cp (Copy Files and Directories)

Used to create a copy of files or directories.

## Syntax

```bash
cp source_file destination_file
```

## Example

```bash
touch employee.txt

cp employee.txt employee_backup.txt
```

Verify:

```bash
ls
```

Output:

```text
employee.txt
employee_backup.txt
```

## Copy File to Another Directory

```bash
cp employee.txt /home/oracle/backup/
```

## Copy Entire Directory

```bash
cp -r notes notes_backup
```

Example:

```bash
mkdir notes
touch notes/day1.txt

cp -r notes notes_backup
```

---

# 2. mv (Move or Rename Files)

Used to move files or rename them.

## Rename a File

```bash
mv employee.txt emp.txt
```

Before:

```text
employee.txt
```

After:

```text
emp.txt
```

## Move a File

```bash
mv emp.txt backup/
```

## Move Multiple Files

```bash
mv file1.txt file2.txt backup/
```

---

# 3. cat (Display File Contents)

Used to view file contents.

## Syntax

```bash
cat filename
```

## Example

```bash
cat emp.txt
```

Output:

```text
Munna
Oracle DBA
```

## Create File Using cat

```bash
cat > notes.txt
```

Type:

```text
Oracle DBA Learning
Linux Commands Practice
```

Press:

```text
CTRL + D
```

View File:

```bash
cat notes.txt
```

## Merge Multiple Files

```bash
cat file1.txt file2.txt > merged.txt
```

---

# 4. nano (Text Editor)

Used to create and edit text files.

## Open/Create File

```bash
nano dba_notes.txt
```

Add:

```text
Oracle DBA Day 12

Linux Commands:
cp
mv
cat
nano
```

## Nano Shortcuts

| Shortcut | Action |
|-----------|----------|
| CTRL + O | Save File |
| ENTER | Confirm Save |
| CTRL + X | Exit |
| CTRL + K | Cut Line |
| CTRL + U | Paste Line |
| CTRL + W | Search Text |

---

# Oracle DBA Real-Time Usage

## View Listener Configuration

```bash
cat $ORACLE_HOME/network/admin/listener.ora
```

## View TNS Configuration

```bash
cat $ORACLE_HOME/network/admin/tnsnames.ora
```

## Backup Configuration File

```bash
cp listener.ora listener.ora.bkp
```

## Rename Backup File

```bash
mv listener.ora.bkp listener_backup.ora
```

## Edit Configuration File

```bash
nano listener.ora
```

---

# Practice Lab

## Step 1: Create Working Directory

```bash
mkdir oracle_dba
cd oracle_dba
```

## Step 2: Create Notes File

```bash
nano dba_notes.txt
```

Add:

```text
Oracle DBA Learning

Day 12 Linux Commands

cp   - Copy
mv   - Move
cat  - View Content
nano - Text Editor
```

Save:

```text
CTRL + O
ENTER
CTRL + X
```

## Step 3: View File

```bash
cat dba_notes.txt
```

## Step 4: Create Backup

```bash
cp dba_notes.txt dba_notes_backup.txt
```

## Step 5: Rename Backup

```bash
mv dba_notes_backup.txt backup_notes.txt
```

## Step 6: Verify Files

```bash
ls
```

Output:

```text
backup_notes.txt
dba_notes.txt
```

---

# Interview Questions

## Q1. Difference Between cp and mv?

### cp
- Creates a copy.
- Original file remains unchanged.

### mv
- Moves or renames a file.
- Original file is removed from the old location.

---

## Q2. How to Copy an Entire Directory?

```bash
cp -r source_directory destination_directory
```

---

## Q3. How to Display File Contents?

```bash
cat filename
```

---

## Q4. Which Editors Are Commonly Used in Linux?

- nano
- vi
- vim

---

# Day 12 Summary

✅ cp - Copy files and directories

✅ mv - Move and rename files

✅ cat - Display file contents

✅ nano - Create and edit text files

✅ Practiced file backup and management

✅ Learned Oracle DBA real-world Linux usage

---

# Next Topic (Day 13)

Linux Permissions:

- chmod
- chown
- whoami
- groups

These commands are very important for Oracle Database Administration.
