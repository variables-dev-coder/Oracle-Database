# Day 29 – Oracle User Creation (Oracle 21c XE)

## Objective

Learn how to:

- Create Oracle users
- Understand CDB and PDB
- Grant user privileges
- Connect as a new user
- Create tables
- Insert data
- Fix common Oracle errors

---

# What is an Oracle User?

An Oracle User is a database account.

Every user can own:

- Tables
- Views
- Indexes
- Procedures
- Functions
- Packages

Example:

```
SYS
SYSTEM
HR
SCOTT
MUNNA
```

Each user has its own schema.

Example:

```
MUNNA.STUDENT
MUNNA.EMPLOYEE
```

---

# Oracle 21c XE Architecture

Oracle XE uses Multitenant Architecture.

```
Oracle Database
│
├── CDB$ROOT
│
└── XEPDB1
```

### CDB$ROOT

- Root Container
- Stores Oracle metadata
- Stores common users

### XEPDB1

- Pluggable Database (PDB)
- Stores application users
- Local users are created here

---

# Step 1 – Login as SYSDBA

Open Command Prompt.

```bash
sqlplus / as sysdba
```

Check current user

```sql
SHOW USER;
```

Output

```
USER is "SYS"
```

---

# Step 2 – Check Current Container

```sql
SHOW CON_NAME;
```

Output

```
CDB$ROOT
```

---

# Step 3 – Switch to PDB

```sql
ALTER SESSION SET CONTAINER = XEPDB1;
```

Verify

```sql
SHOW CON_NAME;
```

Output

```
XEPDB1
```

---

# Step 4 – Check Existing User

```sql
SELECT username
FROM dba_users
WHERE username='MUNNA';
```

---

# Step 5 – Delete Existing User (Optional)

```sql
DROP USER munna CASCADE;
```

Output

```
User dropped.
```

### CASCADE

Deletes

- User
- Tables
- Views
- Indexes
- Procedures
- All owned objects

---

# Step 6 – Create User

```sql
CREATE USER munna
IDENTIFIED BY test;
```

Output

```
User created.
```

---

# Step 7 – Grant Login Permission

```sql
GRANT CREATE SESSION TO munna;
```

Output

```
Grant succeeded.
```

### CREATE SESSION

Allows a user to connect to Oracle.

Without it

```
ORA-01045
User lacks CREATE SESSION privilege
```

---

# Step 8 – Grant Table Creation Permission

```sql
GRANT CREATE TABLE TO munna;
```

Output

```
Grant succeeded.
```

Allows user to create tables.

---

# Step 9 – Grant Tablespace Quota

Oracle XE requires quota on USERS tablespace.

```sql
ALTER USER munna
QUOTA UNLIMITED ON users;
```

Output

```
User altered.
```

Without this

```
ORA-01950:
no privileges on tablespace USERS
```

---

# Step 10 – Exit SYS

```sql
EXIT;
```

---

# Step 11 – Connect as User

Open SQL*Plus

```bash
sqlplus
```

When prompted

```
Enter user-name:
```

Type

```
munna/test@localhost:1521/xepdb1
```

Connected

```
Connected to:
Oracle Database 21c Express Edition
```

---

# Step 12 – Verify Current User

```sql
SHOW USER;
```

Output

```
USER is "MUNNA"
```

---

# Step 13 – Create Table

```sql
CREATE TABLE student
(
    id NUMBER,
    name VARCHAR2(50)
);
```

Output

```
Table created.
```

---

# Step 14 – Check Table

```sql
SELECT table_name
FROM user_tables;
```

Output

```
STUDENT
```

---

# Step 15 – Insert Data

```sql
INSERT INTO student
VALUES
(
1,
'Munna'
);
```

---

# Step 16 – Commit

```sql
COMMIT;
```

---

# Step 17 – View Data

```sql
SELECT *
FROM student;
```

Output

```
ID   NAME
---------------
1    Munna
```

---

# Step 18 – Exit

```sql
EXIT;
```

---

# Complete Practical Script

## Login as SYS

```bash
sqlplus / as sysdba
```

```sql
ALTER SESSION SET CONTAINER=XEPDB1;

DROP USER munna CASCADE;

CREATE USER munna IDENTIFIED BY test;

GRANT CREATE SESSION TO munna;

GRANT CREATE TABLE TO munna;

ALTER USER munna
QUOTA UNLIMITED ON users;

EXIT;
```

Login as Munna

```bash
sqlplus
```

```
munna/test@localhost:1521/xepdb1
```

```sql
SHOW USER;

CREATE TABLE student
(
id NUMBER,
name VARCHAR2(50)
);

INSERT INTO student
VALUES
(
1,
'Munna'
);

COMMIT;

SELECT * FROM student;

SELECT table_name
FROM user_tables;

EXIT;
```

---

# Data Dictionary Views

## All Users

```sql
SELECT *
FROM dba_users;
```

---

## Current User Tables

```sql
SELECT *
FROM user_tables;
```

---

## Current User

```sql
SHOW USER;
```

---

## Current Container

```sql
SHOW CON_NAME;
```

---

# Common Errors

## ORA-65096

```
invalid common user or role name
```

Reason

Creating a local user inside CDB$ROOT.

Solution

```sql
ALTER SESSION SET CONTAINER=XEPDB1;
```

---

## ORA-01920

```
User name conflicts with another user
```

Reason

User already exists.

Solution

```sql
DROP USER username CASCADE;
```

---

## ORA-01017

```
Invalid username/password
```

Reason

Wrong password or wrong connection string.

Correct Connection

```
munna/test@localhost:1521/xepdb1
```

---

## ORA-01950

```
No privileges on tablespace USERS
```

Reason

User has no quota.

Solution

```sql
ALTER USER munna
QUOTA UNLIMITED ON users;
```

---

## ORA-12154

```
TNS could not resolve connect identifier
```

Reason

Wrong service name.

Correct Connection

```
localhost:1521/xepdb1
```

---

# Commands Learned

```sql
SHOW USER;

SHOW CON_NAME;

ALTER SESSION SET CONTAINER=XEPDB1;

CREATE USER;

DROP USER;

GRANT CREATE SESSION;

GRANT CREATE TABLE;

ALTER USER QUOTA UNLIMITED ON users;

CREATE TABLE;

INSERT;

COMMIT;

SELECT;

EXIT;
```

---

# Interview Questions

## 1. What is an Oracle User?

An account that can connect to Oracle and own database objects.

---

## 2. Why do we switch to XEPDB1?

Because local users must be created inside the Pluggable Database.

---

## 3. What is CREATE SESSION?

System privilege that allows a user to log in.

---

## 4. What is CREATE TABLE privilege?

Allows a user to create tables.

---

## 5. What is QUOTA?

Quota specifies how much space a user can use in a tablespace.

---

## 6. Why did ORA-01950 occur?

Because the user had no quota on the USERS tablespace.

---

## 7. Difference between CDB and PDB?

| CDB | PDB |
|------|-----|
| Root Database | Pluggable Database |
| Stores Oracle metadata | Stores user data |
| Common Users | Local Users |

---

## 8. Which view shows all users?

```sql
DBA_USERS
```

---

## 9. Which view shows current user's tables?

```sql
USER_TABLES
```

---

## 10. How do you delete a user completely?

```sql
DROP USER username CASCADE;
```

---

# Key Takeaways

- Oracle 21c XE uses **Multitenant Architecture (CDB + PDB)**.
- Always switch to **XEPDB1** before creating local users.
- A user needs **CREATE SESSION** to log in.
- A user needs **CREATE TABLE** to create tables.
- A user needs **tablespace quota** to store data.
- Connect to the PDB using:

```text
munna/test@localhost:1521/xepdb1
```

This is the recommended connection format for Oracle 21c XE.
