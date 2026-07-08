# Day 29 – Oracle User Creation & Management, Privileges & Roles

## Objective

Learn how to:

- Create Oracle users
- Change password
- Unlock users
- Drop users
- Switch between CDB and PDB
- Login using SQL*Plus
- Understand local users and common users
- Practice complete Oracle user management

---

# What is a User?

A **User** is an Oracle database account.

Every person or application connecting to Oracle must use a user account.

Examples:

- SYS
- SYSTEM
- HR
- SCOTT
- MUNNA

Every user has:

- Username
- Password
- Schema
- Privileges
- Tables
- Objects

---

# SYS User

- Highest privileged user
- Owns the Oracle Data Dictionary
- Connects using SYSDBA privilege
- Used for administration

Login

```sql
sqlplus / as sysdba
```

Check current user

```sql
SHOW USER;
```

Output

```text
USER is "SYS"
```

---

# CDB and PDB

Oracle 21c uses the Multitenant Architecture.

```
Container Database (CDB)
│
├── PDB$SEED
│
└── FREEPDB1
```

Normally we create application users inside a **PDB**, not inside **CDB$ROOT**.

---

# Check Current Container

```sql
SHOW CON_NAME;
```

Example

```text
CON_NAME
---------
CDB$ROOT
```

---

# View Available PDBs

```sql
SHOW PDBS;
```

Example

```text
CON_ID  CON_NAME    OPEN MODE
------  ----------  ----------
2       PDB$SEED    READ ONLY
3       FREEPDB1    READ WRITE
```

---

# Switch to PDB

```sql
ALTER SESSION SET CONTAINER=FREEPDB1;
```

Verify

```sql
SHOW CON_NAME;
```

Output

```text
FREEPDB1
```

---

# Create a User

Syntax

```sql
CREATE USER username
IDENTIFIED BY password;
```

Example

```sql
CREATE USER munna
IDENTIFIED BY munna123;
```

Output

```text
User created.
```

---

# Why Can't the User Login Yet?

Creating a user only creates the account.

The user still has **no login privilege**.

You must grant privileges before the user can connect.

Example

```sql
GRANT CONNECT TO munna;
```

---

# Change User Password

Syntax

```sql
ALTER USER username
IDENTIFIED BY new_password;
```

Example

```sql
ALTER USER munna
IDENTIFIED BY munna321;
```

Output

```text
User altered.
```

---

# Unlock a User

Sometimes Oracle locks a user after multiple failed login attempts.

Unlock

```sql
ALTER USER munna ACCOUNT UNLOCK;
```

Unlock and change password

```sql
ALTER USER munna
IDENTIFIED BY munna123
ACCOUNT UNLOCK;
```

---

# Lock a User

```sql
ALTER USER munna ACCOUNT LOCK;
```

Now the user cannot log in.

---

# Check User Information

```sql
SELECT USERNAME,
       ACCOUNT_STATUS
FROM DBA_USERS
WHERE USERNAME='MUNNA';
```

Example

```text
USERNAME   ACCOUNT_STATUS
--------   --------------
MUNNA      OPEN
```

---

# Delete a User

Syntax

```sql
DROP USER username;
```

Example

```sql
DROP USER munna;
```

If the user owns tables

```sql
DROP USER munna CASCADE;
```

`CASCADE` removes all objects owned by the user.

---

# Login as User

Exit SYS

```sql
EXIT
```

Login

```bash
sqlplus munna/munna123@FREEPDB1
```

Check current user

```sql
SHOW USER;
```

Output

```text
USER is "MUNNA"
```

---

# Local User vs Common User

## Local User

Exists only inside one PDB.

Example

```sql
CREATE USER munna IDENTIFIED BY munna123;
```

Location

```
FREEPDB1
    │
    └── MUNNA
```

---

## Common User

Exists in every PDB.

Must begin with

```
C##
```

Example

```sql
CREATE USER C##ADMIN
IDENTIFIED BY admin123;
```

Available in

```
CDB
│
├── PDB1
├── PDB2
└── PDB3
```

---

# Useful Commands

Current User

```sql
SHOW USER;
```

Current Container

```sql
SHOW CON_NAME;
```

Available PDBs

```sql
SHOW PDBS;
```

Change Container

```sql
ALTER SESSION SET CONTAINER=FREEPDB1;
```

---

# Complete Practical (Step-by-Step)

## Step 1

Login as SYS

```bash
sqlplus / as sysdba
```

---

## Step 2

Check current user

```sql
SHOW USER;
```

---

## Step 3

Check current container

```sql
SHOW CON_NAME;
```

---

## Step 4

Display all PDBs

```sql
SHOW PDBS;
```

---

## Step 5

Switch to FREEPDB1

```sql
ALTER SESSION SET CONTAINER=FREEPDB1;
```

Verify

```sql
SHOW CON_NAME;
```

---

## Step 6

Create user

```sql
CREATE USER munna
IDENTIFIED BY munna123;
```

---

## Step 7

Verify user exists

```sql
SELECT USERNAME
FROM DBA_USERS
WHERE USERNAME='MUNNA';
```

---

## Step 8

Grant login privilege

```sql
GRANT CONNECT TO munna;
```

---

## Step 9

Exit SYS

```sql
EXIT
```

---

## Step 10

Login as Munna

```bash
sqlplus munna/munna123@FREEPDB1
```

---

## Step 11

Check current user

```sql
SHOW USER;
```

---

## Step 12

Exit

```sql
EXIT
```

---

## Step 13

Login again as SYS

```bash
sqlplus / as sysdba
```

---

## Step 14

Switch to FREEPDB1

```sql
ALTER SESSION SET CONTAINER=FREEPDB1;
```

---

## Step 15

Change password

```sql
ALTER USER munna
IDENTIFIED BY munna321;
```

---

## Step 16

Unlock account

```sql
ALTER USER munna ACCOUNT UNLOCK;
```

---

## Step 17

Check account status

```sql
SELECT USERNAME,
       ACCOUNT_STATUS
FROM DBA_USERS
WHERE USERNAME='MUNNA';
```

---

## Step 18

(Optional)

Delete user

```sql
DROP USER munna CASCADE;
```

---

# Complete Flow Diagram

```
Open CMD
    │
    ▼
sqlplus / as sysdba
    │
    ▼
SHOW USER
    │
    ▼
SHOW CON_NAME
    │
    ▼
SHOW PDBS
    │
    ▼
ALTER SESSION SET CONTAINER=FREEPDB1
    │
    ▼
SHOW CON_NAME
    │
    ▼
CREATE USER munna IDENTIFIED BY munna123
    │
    ▼
GRANT CONNECT TO munna
    │
    ▼
EXIT
    │
    ▼
sqlplus munna/munna123@FREEPDB1
    │
    ▼
SHOW USER
    │
    ▼
EXIT
    │
    ▼
sqlplus / as sysdba
    │
    ▼
ALTER SESSION SET CONTAINER=FREEPDB1
    │
    ▼
ALTER USER munna IDENTIFIED BY munna321
    │
    ▼
ALTER USER munna ACCOUNT UNLOCK
    │
    ▼
SELECT USERNAME, ACCOUNT_STATUS
FROM DBA_USERS
    │
    ▼
DROP USER munna CASCADE (Optional)
```

---

# Interview Questions

### 1. What is a database user?

A database user is an Oracle account used to connect to the database and own database objects.

---

### 2. What is the difference between SYS and SYSTEM?

| SYS | SYSTEM |
|------|---------|
| Owns the data dictionary | Administrative user |
| Connects as SYSDBA | Normal administrative account |
| Highest privilege | Less privileged than SYS |

---

### 3. Why do we switch to a PDB before creating users?

Because application users should be created inside a Pluggable Database (PDB). Users created in a PDB are local to that database.

---

### 4. What is the difference between a Local User and a Common User?

- **Local User:** Exists only in one PDB.
- **Common User:** Exists across all PDBs and starts with `C##`.

---

### 5. Why can't a newly created user log in?

Because the user has no login privilege until `CREATE SESSION` (or the `CONNECT` role) is granted.

---

### 6. What is `DROP USER ... CASCADE`?

It deletes the user along with all database objects owned by that user.

---

# Commands Summary

```sql
SHOW USER;

SHOW CON_NAME;

SHOW PDBS;

ALTER SESSION SET CONTAINER=FREEPDB1;

CREATE USER munna IDENTIFIED BY munna123;

GRANT CONNECT TO munna;

ALTER USER munna IDENTIFIED BY munna321;

ALTER USER munna ACCOUNT UNLOCK;

SELECT * FROM DBA_USERS;

DROP USER munna CASCADE;

EXIT;
```

---



# Day 29 Summary

✔ Oracle Users

✔ SYS User

✔ CDB vs PDB

✔ Switch Container

✔ Create User

✔ Grant Login Privilege

✔ Change Password

✔ Lock & Unlock User

✔ Check User Status

✔ Drop User

✔ SQL*Plus Login

✔ Local User vs Common User

✔ Complete Hands-on Practice

✔ Interview Questions

---

# Day 30 – Oracle Privileges & Roles

## Objective

Today you will learn:

- What is a Privilege?
- Types of Privileges
- System Privileges
- Object Privileges
- GRANT Statement
- REVOKE Statement
- What is a Role?
- Built-in Roles
- CONNECT Role
- RESOURCE Role
- DBA Role
- WITH ADMIN OPTION
- WITH GRANT OPTION
- Viewing User Privileges
- Viewing User Roles
- Complete Hands-on Practical
- Interview Questions

---

# What is a Privilege?

A **Privilege** is a permission that allows a user to perform a specific action in the Oracle database.

Without the required privilege, Oracle will not allow the operation.

Examples:

- Login to the database
- Create a table
- Create a view
- Create a procedure
- Select data
- Insert data
- Delete data

---

# Types of Privileges

Oracle provides two types of privileges.

```
Privileges
│
├── System Privileges
│
└── Object Privileges
```

---

# 1. System Privileges

A **System Privilege** allows a user to perform database-level operations.

Examples

```text
CREATE SESSION
CREATE TABLE
CREATE VIEW
CREATE USER
ALTER USER
DROP USER
CREATE PROCEDURE
CREATE SEQUENCE
```

Example

```sql
GRANT CREATE SESSION TO munna;
```

Now the user can log in to the database.

---

# 2. Object Privileges

Object privileges allow a user to perform operations on a specific database object.

Objects include:

- Tables
- Views
- Sequences
- Procedures

Examples

```text
SELECT
INSERT
UPDATE
DELETE
ALTER
INDEX
REFERENCES
```

Example

```sql
GRANT SELECT
ON employees
TO munna;
```

Now `munna` can query the `employees` table.

---

# GRANT Statement

The `GRANT` statement gives privileges or roles to a user.

Syntax

```sql
GRANT privilege TO username;
```

Example

```sql
GRANT CREATE SESSION TO munna;
```

Grant multiple privileges

```sql
GRANT CREATE SESSION,
      CREATE TABLE
TO munna;
```

---

# REVOKE Statement

The `REVOKE` statement removes privileges or roles from a user.

Syntax

```sql
REVOKE privilege FROM username;
```

Example

```sql
REVOKE CREATE TABLE
FROM munna;
```

---

# What is a Role?

A **Role** is a collection of privileges.

Instead of granting many privileges one by one, Oracle groups them into a role.

```
Privileges
      │
      ▼
     Role
      │
      ▼
     User
```

Example

Instead of

```sql
GRANT CREATE SESSION TO munna;

GRANT CREATE TABLE TO munna;

GRANT CREATE VIEW TO munna;
```

Simply grant one role.

---

# Built-in Roles

Oracle provides several predefined roles.

Common roles:

- CONNECT
- RESOURCE
- DBA

---

# CONNECT Role

The **CONNECT** role allows a user to connect to the database.

Modern Oracle versions mainly include:

```text
CREATE SESSION
```

Grant CONNECT

```sql
GRANT CONNECT TO munna;
```

---

# RESOURCE Role

The **RESOURCE** role is intended for developers.

It typically includes privileges to create database objects.

Examples

```text
CREATE TABLE

CREATE PROCEDURE

CREATE TRIGGER

CREATE TYPE

CREATE SEQUENCE
```

Grant RESOURCE

```sql
GRANT RESOURCE TO munna;
```

---

# DBA Role

The **DBA** role is the highest predefined administrative role.

A user with the DBA role can:

- Create Users
- Drop Users
- Create Tablespaces
- Shutdown Database
- Startup Database
- Grant Privileges
- Perform Backup and Recovery
- Manage the Database

Grant DBA

```sql
GRANT DBA TO munna;
```

> **Warning:** Never grant the DBA role to normal application users in a production environment.

---

# WITH ADMIN OPTION

Allows the user to grant a **role or system privilege** to another user.

Example

```sql
GRANT CONNECT
TO munna
WITH ADMIN OPTION;
```

Now `munna` can grant the CONNECT role to another user.

---

# WITH GRANT OPTION

Used only with **Object Privileges**.

Example

```sql
GRANT SELECT
ON employees
TO munna
WITH GRANT OPTION;
```

Now `munna` can grant SELECT on the `employees` table to other users.

---

# View Granted Roles

```sql
SELECT *
FROM DBA_ROLE_PRIVS
WHERE GRANTEE='MUNNA';
```

---

# View System Privileges

```sql
SELECT *
FROM DBA_SYS_PRIVS
WHERE GRANTEE='MUNNA';
```

---

# View Object Privileges

```sql
SELECT *
FROM DBA_TAB_PRIVS
WHERE GRANTEE='MUNNA';
```

---

# View Current User Roles

Login as the user

```sql
SELECT *
FROM USER_ROLE_PRIVS;
```

---

# View Current Session Privileges

```sql
SELECT *
FROM SESSION_PRIVS;
```

---

# Complete Practical

## Step 1

Login as SYS

```bash
sqlplus / as sysdba
```

---

## Step 2

Switch to PDB

```sql
ALTER SESSION SET CONTAINER=FREEPDB1;
```

---

## Step 3

Check Current User

```sql
SHOW USER;
```

Output

```text
USER is "SYS"
```

---

## Step 4

Grant CONNECT Role

```sql
GRANT CONNECT TO munna;
```

---

## Step 5

Grant RESOURCE Role

```sql
GRANT RESOURCE TO munna;
```

---

## Step 6

Verify Granted Roles

```sql
SELECT GRANTEE,
       GRANTED_ROLE
FROM DBA_ROLE_PRIVS
WHERE GRANTEE='MUNNA';
```

Expected Output

```text
GRANTEE   GRANTED_ROLE
--------  ------------
MUNNA     CONNECT
MUNNA     RESOURCE
```

---

## Step 7

Exit SYS

```sql
EXIT;
```

---

## Step 8

Login as Munna

```bash
sqlplus munna/munna321@FREEPDB1
```

(Use the password you set on Day 29.)

---

## Step 9

Check Current User

```sql
SHOW USER;
```

Expected Output

```text
USER is "MUNNA"
```

---

## Step 10

Create a Table

```sql
CREATE TABLE student
(
    id NUMBER,
    name VARCHAR2(50)
);
```

---

## Step 11

Insert Data

```sql
INSERT INTO student
VALUES (1,'Aziz');
```

---

## Step 12

Commit

```sql
COMMIT;
```

---

## Step 13

View Data

```sql
SELECT *
FROM student;
```

---

## Step 14

Check User Roles

```sql
SELECT *
FROM USER_ROLE_PRIVS;
```

---

## Step 15

Check Session Privileges

```sql
SELECT *
FROM SESSION_PRIVS;
```

---

## Step 16

Exit

```sql
EXIT;
```

---

## Step 17

Login as SYS

```bash
sqlplus / as sysdba
```

---

## Step 18

Switch to FREEPDB1

```sql
ALTER SESSION SET CONTAINER=FREEPDB1;
```

---

## Step 19

Revoke RESOURCE

```sql
REVOKE RESOURCE
FROM munna;
```

---

## Step 20

Verify Roles

```sql
SELECT GRANTEE,
       GRANTED_ROLE
FROM DBA_ROLE_PRIVS
WHERE GRANTEE='MUNNA';
```

Expected Output

```text
GRANTEE   GRANTED_ROLE
--------  ------------
MUNNA     CONNECT
```

---

## Step 21

Exit SYS

```sql
EXIT;
```

---

## Step 22

Login as Munna

```bash
sqlplus munna/munna321@FREEPDB1
```

---

## Step 23

Try Creating Another Table

```sql
CREATE TABLE employee
(
    id NUMBER
);
```

Depending on your Oracle version and remaining privileges, Oracle may return:

```text
ORA-01031: insufficient privileges
```

---

# Complete Flow Diagram

```
Open CMD
    │
    ▼
sqlplus / as sysdba
    │
    ▼
ALTER SESSION SET CONTAINER=FREEPDB1
    │
    ▼
GRANT CONNECT TO munna
    │
    ▼
GRANT RESOURCE TO munna
    │
    ▼
Verify Roles
    │
    ▼
EXIT
    │
    ▼
sqlplus munna/password@FREEPDB1
    │
    ▼
SHOW USER
    │
    ▼
CREATE TABLE student
    │
    ▼
INSERT DATA
    │
    ▼
COMMIT
    │
    ▼
SELECT *
    │
    ▼
USER_ROLE_PRIVS
    │
    ▼
SESSION_PRIVS
    │
    ▼
EXIT
    │
    ▼
sqlplus / as sysdba
    │
    ▼
ALTER SESSION SET CONTAINER=FREEPDB1
    │
    ▼
REVOKE RESOURCE FROM munna
    │
    ▼
Verify Roles
    │
    ▼
EXIT
    │
    ▼
sqlplus munna/password@FREEPDB1
    │
    ▼
CREATE TABLE employee
    │
    ▼
ORA-01031 (if required privilege is no longer available)
```

---

# Interview Questions

### 1. What is a privilege?

A privilege is a permission that allows a user to perform a specific action in the Oracle database.

---

### 2. What are the two types of privileges?

- System Privileges
- Object Privileges

---

### 3. What is the difference between a System Privilege and an Object Privilege?

System privileges allow database-level operations, while object privileges allow operations on a specific object such as a table or view.

---

### 4. What is a role?

A role is a collection of privileges that can be granted to users.

---

### 5. What is the CONNECT role?

The CONNECT role allows a user to connect to the Oracle database (primarily through the `CREATE SESSION` privilege in modern Oracle versions).

---

### 6. What is the RESOURCE role?

The RESOURCE role provides privileges to create common database objects such as tables, procedures, and sequences.

---

### 7. What is the DBA role?

The DBA role is the highest predefined administrative role and provides extensive database administration privileges.

---

### 8. What is the difference between GRANT and REVOKE?

- `GRANT` gives privileges or roles.
- `REVOKE` removes privileges or roles.

---

### 9. What is `WITH ADMIN OPTION`?

It allows the grantee to grant a role or system privilege to other users.

---

### 10. What is `WITH GRANT OPTION`?

It allows the grantee to grant an object privilege to other users.

---

# Commands Summary

```sql
GRANT CONNECT TO munna;

GRANT RESOURCE TO munna;

GRANT CREATE SESSION TO munna;

GRANT SELECT
ON employees
TO munna;

REVOKE RESOURCE FROM munna;

SELECT * FROM DBA_ROLE_PRIVS;

SELECT * FROM DBA_SYS_PRIVS;

SELECT * FROM DBA_TAB_PRIVS;

SELECT * FROM USER_ROLE_PRIVS;

SELECT * FROM SESSION_PRIVS;
```

---

# Day 30 Summary

- Privileges
- System Privileges
- Object Privileges
- GRANT
- REVOKE
- Roles
- CONNECT Role
- RESOURCE Role
- DBA Role
- WITH ADMIN OPTION
- WITH GRANT OPTION
- USER_ROLE_PRIVS
- DBA_ROLE_PRIVS
- SESSION_PRIVS
- Complete Practical
- Flow Diagram
- Interview Questions
