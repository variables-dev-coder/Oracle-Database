# Day 32 – Practice
# Create Multiple Company Users

## 🎯 Objective

In this practice, you will simulate a real company's Oracle database by creating multiple users for different departments and assigning appropriate privileges.

By the end of this practice, you will be able to:

- Create multiple Oracle users
- Assign default and temporary tablespaces
- Set tablespace quotas
- Grant system privileges
- Test user login
- Verify users and privileges
- Lock and unlock user accounts
- Change user passwords
- Expire passwords
- Drop users safely

---

# Company Scenario

Suppose your company is **ABC Technologies**.

```
ABC Technologies
│
├── HR Department
│      ├── hr_admin
│      └── hr_emp
│
├── Finance Department
│      ├── fin_admin
│      └── fin_user
│
├── Sales Department
│      ├── sales_admin
│      └── sales_user
│
└── Developer Department
       ├── dev_admin
       └── developer1
```

Total Users = **8**

---

# Step 1 – Login as SYS

```sql
sqlplus / as sysdba
```

Switch to your Pluggable Database.

```sql
ALTER SESSION SET CONTAINER = XEPDB1;
```

Verify the current container.

```sql
SHOW CON_NAME;
```

Output

```
XEPDB1
```

---

# Step 2 – Check Existing Users

```sql
SELECT username
FROM dba_users
ORDER BY username;
```

Purpose:

- Displays all users in the database.
- Helps verify whether a user already exists.

---

# Step 3 – Create HR Admin

```sql
CREATE USER hr_admin
IDENTIFIED BY Hr@123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 100M ON USERS;
```

Grant privileges

```sql
GRANT CREATE SESSION TO hr_admin;

GRANT CREATE TABLE TO hr_admin;

GRANT CREATE VIEW TO hr_admin;

GRANT CREATE SEQUENCE TO hr_admin;
```

---

# Step 4 – Create HR Employee

```sql
CREATE USER hr_emp
IDENTIFIED BY HrEmp123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 20M ON USERS;
```

Grant login permission.

```sql
GRANT CREATE SESSION TO hr_emp;
```

Notice:

- HR Employee can only log in.
- Cannot create tables.

---

# Step 5 – Create Finance Admin

```sql
CREATE USER fin_admin
IDENTIFIED BY Fin123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 100M ON USERS;
```

Grant privileges.

```sql
GRANT CREATE SESSION,
      CREATE TABLE,
      CREATE VIEW,
      CREATE PROCEDURE
TO fin_admin;
```

---

# Step 6 – Create Finance User

```sql
CREATE USER fin_user
IDENTIFIED BY FinUser123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 20M ON USERS;
```

Grant login permission.

```sql
GRANT CREATE SESSION TO fin_user;
```

---

# Step 7 – Create Sales Admin

```sql
CREATE USER sales_admin
IDENTIFIED BY Sales123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 100M ON USERS;
```

Grant privileges.

```sql
GRANT CREATE SESSION,
      CREATE TABLE,
      CREATE VIEW
TO sales_admin;
```

---

# Step 8 – Create Sales User

```sql
CREATE USER sales_user
IDENTIFIED BY SalesUser123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 20M ON USERS;
```

Grant login permission.

```sql
GRANT CREATE SESSION TO sales_user;
```

---

# Step 9 – Create Developer Admin

```sql
CREATE USER dev_admin
IDENTIFIED BY Dev123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 200M ON USERS;
```

Grant privileges.

```sql
GRANT CREATE SESSION,
      CREATE TABLE,
      CREATE VIEW,
      CREATE PROCEDURE,
      CREATE SEQUENCE
TO dev_admin;
```

---

# Step 10 – Create Developer User

```sql
CREATE USER developer1
IDENTIFIED BY DevUser123
DEFAULT TABLESPACE USERS
TEMPORARY TABLESPACE TEMP
QUOTA 50M ON USERS;
```

Grant login permission.

```sql
GRANT CREATE SESSION TO developer1;
```

---

# Step 11 – Verify Users

```sql
SELECT username,
       account_status,
       default_tablespace
FROM dba_users
WHERE username IN
(
'HR_ADMIN',
'HR_EMP',
'FIN_ADMIN',
'FIN_USER',
'SALES_ADMIN',
'SALES_USER',
'DEV_ADMIN',
'DEVELOPER1'
);
```

Expected Output

```
USERNAME      ACCOUNT_STATUS    DEFAULT_TABLESPACE
--------------------------------------------------
HR_ADMIN      OPEN              USERS
HR_EMP        OPEN              USERS
FIN_ADMIN     OPEN              USERS
FIN_USER      OPEN              USERS
SALES_ADMIN   OPEN              USERS
SALES_USER    OPEN              USERS
DEV_ADMIN     OPEN              USERS
DEVELOPER1    OPEN              USERS
```

---

# Step 12 – Check User Privileges

Example

```sql
SELECT *
FROM dba_sys_privs
WHERE grantee='HR_ADMIN';
```

Repeat for each user.

---

# Step 13 – Test Login

Exit SYS.

```sql
EXIT
```

Login as HR Admin.

```sql
sqlplus hr_admin/Hr@123@XEPDB1
```

Create a table.

```sql
CREATE TABLE employee
(
    id NUMBER,
    name VARCHAR2(50)
);
```

Expected Result

```
Table created.
```

---

Login as HR Employee.

```sql
sqlplus hr_emp/HrEmp123@XEPDB1
```

Try creating a table.

```sql
CREATE TABLE test
(
    id NUMBER
);
```

Expected Result

```
ORA-01031: insufficient privileges
```

Reason:

`hr_emp` only has **CREATE SESSION** privilege.

---

# Step 14 – Lock User Account

```sql
ALTER USER hr_emp ACCOUNT LOCK;
```

Verify.

```sql
SELECT username,
       account_status
FROM dba_users
WHERE username='HR_EMP';
```

Expected Output

```
LOCKED
```

Unlock.

```sql
ALTER USER hr_emp ACCOUNT UNLOCK;
```

---

# Step 15 – Change Password

```sql
ALTER USER hr_emp
IDENTIFIED BY HrEmp456;
```

---

# Step 16 – Expire Password

```sql
ALTER USER hr_emp PASSWORD EXPIRE;
```

Next login will require a new password.

---

# Step 17 – Drop User

```sql
DROP USER sales_user CASCADE;
```

**CASCADE** removes all objects owned by the user before deleting the account.

---

# Useful DBA Queries

## List all users

```sql
SELECT username
FROM dba_users;
```

---

## Check account status

```sql
SELECT username,
       account_status
FROM dba_users;
```

---

## Check tablespace quotas

```sql
SELECT username,
       tablespace_name,
       bytes
FROM dba_ts_quotas;
```

---

## Check system privileges

```sql
SELECT grantee,
       privilege
FROM dba_sys_privs
ORDER BY grantee;
```

---

## Check granted roles

```sql
SELECT grantee,
       granted_role
FROM dba_role_privs
ORDER BY grantee;
```

---

# Real Company User Mapping

| User | Department | Role | Privileges |
|------|------------|------|------------|
| hr_admin | HR | HR Administrator | CREATE SESSION, TABLE, VIEW, SEQUENCE |
| hr_emp | HR | HR Employee | CREATE SESSION |
| fin_admin | Finance | Finance Administrator | CREATE SESSION, TABLE, VIEW, PROCEDURE |
| fin_user | Finance | Finance Employee | CREATE SESSION |
| sales_admin | Sales | Sales Administrator | CREATE SESSION, TABLE, VIEW |
| sales_user | Sales | Sales Employee | CREATE SESSION |
| dev_admin | Development | Lead Developer | CREATE SESSION, TABLE, VIEW, PROCEDURE, SEQUENCE |
| developer1 | Development | Developer | CREATE SESSION |

---

# Interview Questions

### Q1. Why do we create different users?

**Answer**

To isolate departments, improve security, and control access.

---

### Q2. What is CREATE SESSION?

**Answer**

Allows a user to connect (log in) to the Oracle database.

---

### Q3. Why assign a quota?

**Answer**

A quota limits how much space a user can consume in a tablespace.

---

### Q4. Why use DEFAULT TABLESPACE?

**Answer**

It specifies where the user's objects are stored by default.

---

### Q5. Why use TEMPORARY TABLESPACE?

**Answer**

It stores temporary data used during sorting, joins, and indexes.

---

### Q6. What does ACCOUNT LOCK do?

**Answer**

Prevents the user from logging in without deleting the account.

---

### Q7. What is PASSWORD EXPIRE?

**Answer**

Forces the user to change their password at the next login.

---

### Q8. What is the difference between DROP USER and DROP USER CASCADE?

| DROP USER | DROP USER CASCADE |
|-----------|-------------------|
| Deletes only an empty user | Deletes the user and all owned objects |

---

# Practical Assignment

Complete these tasks without referring to the notes.

1. Create all eight company users.
2. Assign appropriate quotas.
3. Grant only the required privileges.
4. Verify all users using `DBA_USERS`.
5. Log in as `hr_admin` and create a table.
6. Log in as `hr_emp` and verify table creation fails.
7. Lock and unlock `fin_user`.
8. Change the password of `developer1`.
9. Expire the password of `developer1`.
10. Drop `sales_user` using `CASCADE`.
11. Verify all privileges using `DBA_SYS_PRIVS`.
12. Verify all roles using `DBA_ROLE_PRIVS`.

---

# Day 32 Summary

After completing this practice, you should be comfortable with:

- Creating Oracle users
- Managing multiple users
- Assigning tablespaces
- Setting storage quotas
- Granting system privileges
- Testing user access
- Verifying users
- Locking and unlocking accounts
- Changing and expiring passwords
- Dropping users safely
- Using common DBA verification queries

These are essential day-to-day Oracle DBA administration tasks used in real enterprise environments.
