# Day 13 - Linux Permissions (Oracle DBA)

## Objective

Learn Linux file permissions and ownership management.

Topics Covered:

* chmod
* chown
* User Permissions
* Permission Testing
* Creating Users

---

# 1. Linux Permission Basics

Every file and directory in Linux has:

* Owner (User)
* Group
* Others

Check permissions:

```bash
ls -l
```

Example:

```bash
-rw-r--r-- 1 oracle dba 1000 Jun 20 data.txt
```

Permission breakdown:

```text
rw- r-- r--
│   │   │
│   │   └── Others
│   └────── Group
└────────── Owner
```

---

# 2. Permission Types

| Permission | Symbol | Value |
| ---------- | ------ | ----- |
| Read       | r      | 4     |
| Write      | w      | 2     |
| Execute    | x      | 1     |

---

# 3. Numeric Permissions

| Number | Permission |
| ------ | ---------- |
| 7      | rwx        |
| 6      | rw-        |
| 5      | r-x        |
| 4      | r--        |
| 0      | ---        |

Examples:

### 777

```text
rwx rwx rwx
```

Everyone has full access.

### 755

```text
rwx r-x r-x
```

Owner has full access.

Group and others have read and execute.

### 644

```text
rw- r-- r--
```

Owner can read/write.

Others can only read.

### 600

```text
rw-------
```

Only owner can access.

---

# 4. chmod Command

Used to change file permissions.

Syntax:

```bash
chmod permission file_name
```

Example:

```bash
chmod 777 test.txt
```

Give full access to everyone.

---

### Common Examples

```bash
chmod 755 backup.sh
chmod 644 file.txt
chmod 600 secret.txt
```

---

# 5. Symbolic chmod

### Add Execute Permission

```bash
chmod +x script.sh
```

### Remove Write Permission

```bash
chmod -w file.txt
```

### Add Execute Permission to User

```bash
chmod u+x file.txt
```

### Add Write Permission to Group

```bash
chmod g+w file.txt
```

### Remove All Permissions from Others

```bash
chmod o-rwx file.txt
```

---

# 6. chown Command

Used to change ownership.

Syntax:

```bash
chown user file
```

Example:

```bash
sudo chown oracle test.txt
```

---

## Change User and Group

```bash
sudo chown oracle:dba test.txt
```

Verify:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 oracle dba test.txt
```

---

## Recursive Ownership Change

```bash
sudo chown -R oracle:dba /u01/app/oracle
```

Changes ownership of all files and directories.

---

# 7. Creating Users

Create User:

```bash
sudo useradd user1
```

Set Password:

```bash
sudo passwd user1
```

Create Another User:

```bash
sudo useradd user2
sudo passwd user2
```

Verify Users:

```bash
cat /etc/passwd
```

---

# 8. Permission Testing Lab

## Step 1: Create Users

```bash
sudo useradd user1
sudo useradd user2
```

---

## Step 2: Create File

```bash
touch secret.txt
echo "Oracle DBA Notes" > secret.txt
```

---

## Step 3: Change Ownership

```bash
sudo chown user1:user1 secret.txt
```

Check:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 user1 user1 secret.txt
```

---

## Step 4: Login as User2

```bash
su - user2
```

Try editing:

```bash
nano secret.txt
```

Result:

```text
Permission Denied
```

---

## Step 5: Allow Everyone to Write

```bash
chmod 666 secret.txt
```

Output:

```text
-rw-rw-rw-
```

Now user2 can edit.

---

## Step 6: Make File Private

```bash
chmod 600 secret.txt
```

Output:

```text
-rw-------
```

Only owner can access.

---

# 9. Oracle DBA Usage

Common Oracle ownership:

```text
oracle:dba
```

Check Oracle directory:

```bash
ls -ld /u01/app/oracle
```

Example:

```text
drwxr-x--- oracle dba
```

---

# 10. Frequently Used DBA Commands

```bash
ls -l

chmod 755 backup.sh

chmod 600 listener.ora

chmod 644 tnsnames.ora

chown oracle:dba backup.sh

chown -R oracle:dba /u01/app/oracle
```

---

# Interview Questions

### What does chmod 755 mean?

```text
rwx r-x r-x
```

Owner: Full Access

Group/Others: Read + Execute

---

### Difference between chmod and chown?

| Command | Purpose            |
| ------- | ------------------ |
| chmod   | Change Permissions |
| chown   | Change Ownership   |

---

### How to make a script executable?

```bash
chmod +x script.sh
```

---

### How to check permissions?

```bash
ls -l
```

---

# Day 13 Summary

✅ Understand Linux Permissions

✅ Read Permission Strings

✅ Use chmod Command

✅ Use chown Command

✅ Create Linux Users

✅ Perform Permission Testing

✅ Oracle DBA File Security Basics

---

# Next Topic (Day 14)

Linux Process Management

* ps
* top
* grep
* kill

Used daily by Oracle DBAs to manage database processes.
