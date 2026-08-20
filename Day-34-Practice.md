# Oracle DBA — Day 34
## Data Dictionary & Dynamic Performance Views

> **Goal:** Learn how an Oracle DBA investigates users, roles, privileges, tablespaces, database status, instance status, and sessions using Oracle's built-in views.

---

## 1. What is the Data Dictionary?

The **Oracle Data Dictionary** is a collection of Oracle-maintained tables and views that stores metadata about the database.

It contains information about:

- Users
- Roles
- Privileges
- Tables
- Views
- Sequences
- Tablespaces
- Datafiles
- Database objects
- Storage
- Configuration

### Simple Concept

```text
                 ORACLE DATABASE
                       |
             +---------+---------+
             |                   |
       Application Data      Metadata
             |                   |
        EMPLOYEE table      Data Dictionary
        CUSTOMER table            |
        ORDERS table              |
                            +-----+------+
                            |            |
                          Users        Objects
                          Roles        Privileges
                          Tablespaces  Datafiles
```

A DBA frequently queries the Data Dictionary to understand and troubleshoot the database.

---

# 2. Types of Data Dictionary Views

The three most important prefixes are:

```text
USER_*
ALL_*
DBA_*
```

Remember:

```text
USER_*  → Objects owned by the current user

ALL_*   → Objects accessible to the current user

DBA_*   → Database-wide information
```

---

# 3. USER_* Views

`USER_*` views show objects owned by the current user.

For example:

```sql
SELECT table_name
FROM user_tables;
```

If you are connected as `MUNNA`, Oracle shows tables owned by `MUNNA`.

### Check all objects owned by current user

```sql
SELECT object_name,
       object_type
FROM user_objects;
```

Example:

```text
OBJECT_NAME       OBJECT_TYPE
----------------  -----------
EMPLOYEE          TABLE
EMP_VIEW          VIEW
EMP_SEQ           SEQUENCE
```

---

# 4. ALL_* Views

`ALL_*` views show objects that the current user has access to.

Example:

```sql
SELECT owner,
       table_name
FROM all_tables;
```

The result may contain tables belonging to different schemas if the current user has access to them.

```text
USER_TABLES
    |
    +-- Tables owned by me

ALL_TABLES
    |
    +-- Tables I can access
```

---

# 5. DBA_* Views

`DBA_*` views provide database-wide information.

They are extremely important for Oracle DBAs.

Example:

```sql
SELECT username
FROM dba_users;
```

Another example:

```sql
SELECT tablespace_name
FROM dba_tablespaces;
```

---

# 6. USER vs ALL vs DBA

| View | Meaning |
|---|---|
| `USER_*` | Objects owned by current user |
| `ALL_*` | Objects accessible by current user |
| `DBA_*` | Database-wide information |

### Easy way to remember

```text
USER → ME

ALL → ACCESS

DBA → DATABASE
```

---

# 7. DBA_USERS

`DBA_USERS` contains information about database users.

### Basic query

```sql
SELECT username
FROM dba_users;
```

### Check account status

```sql
SELECT username,
       account_status
FROM dba_users;
```

Possible statuses include:

```text
OPEN
LOCKED
EXPIRED
EXPIRED & LOCKED
```

### Detailed user information

```sql
SELECT username,
       account_status,
       default_tablespace,
       temporary_tablespace,
       profile
FROM dba_users;
```

### Check MUNNA user

```sql
SELECT username,
       account_status,
       default_tablespace,
       temporary_tablespace,
       profile
FROM dba_users
WHERE username = 'MUNNA';
```

---

# 8. DBA_ROLES

`DBA_ROLES` contains information about roles.

### Show all roles

```sql
SELECT role
FROM dba_roles;
```

### Find a particular role

```sql
SELECT role
FROM dba_roles
WHERE role = 'RESOURCE';
```

---

# 9. DBA_ROLE_PRIVS

`DBA_ROLE_PRIVS` shows roles granted to users or other roles.

### Check MUNNA's roles

```sql
SELECT grantee,
       granted_role
FROM dba_role_privs
WHERE grantee = 'MUNNA';
```

Example:

```text
GRANTEE    GRANTED_ROLE
---------  ------------
MUNNA      CONNECT
MUNNA      RESOURCE
```

Concept:

```text
MUNNA
  |
  +---- CONNECT
  |
  +---- RESOURCE
```

---

# 10. DBA_SYS_PRIVS

`DBA_SYS_PRIVS` shows **system privileges**.

Examples:

```text
CREATE SESSION
CREATE TABLE
CREATE VIEW
CREATE USER
ALTER USER
DROP USER
```

### Check MUNNA's system privileges

```sql
SELECT grantee,
       privilege
FROM dba_sys_privs
WHERE grantee = 'MUNNA';
```

Example:

```text
GRANTEE    PRIVILEGE
---------  --------------
MUNNA      CREATE SESSION
MUNNA      CREATE TABLE
```

---

# 11. DBA_TAB_PRIVS

`DBA_TAB_PRIVS` shows **object privileges**.

Examples:

```text
SELECT
INSERT
UPDATE
DELETE
EXECUTE
```

### Query

```sql
SELECT grantee,
       owner,
       table_name,
       privilege
FROM dba_tab_privs;
```

### Check MUNNA

```sql
SELECT grantee,
       owner,
       table_name,
       privilege
FROM dba_tab_privs
WHERE grantee = 'MUNNA';
```

Example:

```text
GRANTEE  OWNER  TABLE_NAME  PRIVILEGE
-------  -----  ----------  ---------
MUNNA    HR     EMPLOYEE    SELECT
```

Meaning:

```text
MUNNA
  |
  +---- SELECT
          |
          +---- HR.EMPLOYEE
```

---

# 12. System Privilege vs Object Privilege

This is an important interview topic.

### System Privilege

Permission to perform a type of database operation.

Example:

```sql
GRANT CREATE TABLE TO MUNNA;
```

Meaning:

```text
MUNNA can create tables
```

### Object Privilege

Permission on a specific object.

Example:

```sql
GRANT SELECT ON HR.EMPLOYEE TO MUNNA;
```

Meaning:

```text
MUNNA can SELECT from HR.EMPLOYEE
```

### Difference

```text
SYSTEM PRIVILEGE
      |
      +---- CREATE TABLE
      +---- CREATE USER
      +---- CREATE VIEW

OBJECT PRIVILEGE
      |
      +---- SELECT on HR.EMPLOYEE
      +---- INSERT on HR.EMPLOYEE
      +---- UPDATE on HR.EMPLOYEE
```

---

# 13. DBA_TABLESPACES

`DBA_TABLESPACES` provides information about tablespaces.

### Show tablespaces

```sql
SELECT tablespace_name
FROM dba_tablespaces;
```

### Detailed information

```sql
SELECT tablespace_name,
       status,
       contents
FROM dba_tablespaces;
```

Example:

```text
TABLESPACE_NAME  STATUS   CONTENTS
---------------  -------  ---------
SYSTEM           ONLINE   PERMANENT
SYSAUX           ONLINE   PERMANENT
UNDOTBS1         ONLINE   UNDO
TEMP             ONLINE   TEMPORARY
USERS            ONLINE   PERMANENT
```

---

# 14. DBA_DATA_FILES

`DBA_DATA_FILES` provides information about datafiles.

A tablespace is a logical storage structure.

A datafile is a physical file associated with a tablespace.

```text
TABLESPACE
    |
    +---- DATAFILE
    |
    +---- DATAFILE
```

### Query datafiles

```sql
SELECT file_name,
       tablespace_name
FROM dba_data_files;
```

### Check size

```sql
SELECT file_name,
       tablespace_name,
       bytes / 1024 / 1024 AS size_mb
FROM dba_data_files;
```

---

# 15. Tablespace vs Datafile

```text
Logical Layer
     |
     +---- USERS tablespace
                |
                +---- users01.dbf
                +---- users02.dbf

Physical Layer
     |
     +---- Datafiles on disk
```

Remember:

```text
Tablespace = Logical storage

Datafile   = Physical storage file
```

---

# 16. Dynamic Performance Views

Oracle also provides **Dynamic Performance Views**.

They are commonly called:

```text
V$ Views
```

Examples:

```text
V$DATABASE
V$INSTANCE
V$SESSION
V$PARAMETER
V$PROCESS
V$LOG
V$DATAFILE
```

These views provide information about the current state of the Oracle database and instance.

---

# 17. DBA_* vs V$*

A simple way to understand the difference:

```text
DBA_*
   |
   +---- Database metadata/information

V$*
   |
   +---- Current database/instance state
```

For example:

```text
DBA_USERS
    |
    +---- Information about users

V$SESSION
    |
    +---- Current database sessions
```

---

# 18. V$DATABASE

`V$DATABASE` provides information about the database.

### Query

```sql
SELECT name,
       open_mode,
       database_role
FROM v$database;
```

Example:

```text
NAME  OPEN_MODE   DATABASE_ROLE
----  ----------  -------------
XE    READ WRITE  PRIMARY
```

Useful information includes:

```text
NAME
OPEN_MODE
DATABASE_ROLE
```

---

# 19. V$INSTANCE

`V$INSTANCE` provides information about the Oracle instance.

### Query

```sql
SELECT instance_name,
       status,
       startup_time
FROM v$instance;
```

Example:

```text
INSTANCE_NAME  STATUS
-------------  ------
XE             OPEN
```

It helps determine:

- Instance status
- Instance name
- Startup time

---

# 20. V$SESSION

`V$SESSION` is one of the most important performance/troubleshooting views.

It contains information about sessions connected to Oracle.

### Show sessions

```sql
SELECT sid,
       serial#,
       username,
       status
FROM v$session
WHERE username IS NOT NULL;
```

Example:

```text
SID  SERIAL#  USERNAME  STATUS
---  -------  --------  --------
21   3456     MUNNA     INACTIVE
25   7812     SYSTEM    ACTIVE
```

---

# 21. What is a Session?

A session represents a user's connection to Oracle.

```text
User
  |
  | Connect
  v
Oracle Database
  |
  +---- Session
```

One user can have multiple sessions.

```text
MUNNA
  |
  +---- Session 1
  |
  +---- Session 2
```

---

# 22. ACTIVE vs INACTIVE Session

### ACTIVE

The session is currently performing work.

```text
MUNNA
  |
  +---- ACTIVE
```

### INACTIVE

The session is connected but currently not executing SQL.

```text
MUNNA
  |
  +---- INACTIVE
```

---

# 23. SID and SERIAL#

Two important columns are:

```text
SID
SERIAL#
```

Together they identify a session at a point in time.

Example:

```text
SID     = 21
SERIAL# = 3456
```

DBAs commonly need these values when troubleshooting or managing sessions.

---

# 24. V$PARAMETER

`V$PARAMETER` shows Oracle initialization parameters.

### Show parameters

```sql
SELECT name,
       value
FROM v$parameter;
```

### Search for one parameter

```sql
SELECT name,
       value
FROM v$parameter
WHERE name = 'processes';
```

This is useful for checking Oracle configuration.

---

# 25. Important Views to Remember

```text
+-----------------------+
| Data Dictionary Views |
+-----------------------+
| DBA_USERS             |
| DBA_ROLES             |
| DBA_SYS_PRIVS         |
| DBA_TAB_PRIVS         |
| DBA_ROLE_PRIVS        |
| DBA_TABLESPACES       |
| DBA_DATA_FILES        |
+-----------------------+

+-----------------------+
| Dynamic Views         |
+-----------------------+
| V$DATABASE            |
| V$INSTANCE            |
| V$SESSION             |
| V$PARAMETER           |
+-----------------------+
```

---

# 26. Day 34 Hands-on Practice

Connect as SYSDBA:

```sql
sqlplus / as sysdba
```

Switch to your PDB:

```sql
ALTER SESSION SET CONTAINER = XEPDB1;
```

Check:

```sql
SHOW CON_NAME;
```

Expected:

```text
XEPDB1
```

---

## Practice 1 — List Users

```sql
SELECT username,
       account_status
FROM dba_users;
```

---

## Practice 2 — Check MUNNA

```sql
SELECT username,
       account_status,
       default_tablespace,
       temporary_tablespace,
       profile
FROM dba_users
WHERE username = 'MUNNA';
```

---

## Practice 3 — List Roles

```sql
SELECT role
FROM dba_roles;
```

---

## Practice 4 — Check MUNNA Roles

```sql
SELECT grantee,
       granted_role
FROM dba_role_privs
WHERE grantee = 'MUNNA';
```

---

## Practice 5 — Check System Privileges

```sql
SELECT grantee,
       privilege
FROM dba_sys_privs
WHERE grantee = 'MUNNA';
```

---

## Practice 6 — Check Object Privileges

```sql
SELECT grantee,
       owner,
       table_name,
       privilege
FROM dba_tab_privs
WHERE grantee = 'MUNNA';
```

---

## Practice 7 — List Tablespaces

```sql
SELECT tablespace_name,
       status,
       contents
FROM dba_tablespaces;
```

---

## Practice 8 — Check Datafiles

```sql
SELECT file_name,
       tablespace_name,
       bytes / 1024 / 1024 AS size_mb
FROM dba_data_files;
```

---

## Practice 9 — Check Database

```sql
SELECT name,
       open_mode,
       database_role
FROM v$database;
```

---

## Practice 10 — Check Instance

```sql
SELECT instance_name,
       status,
       startup_time
FROM v$instance;
```

---

## Practice 11 — Check Sessions

```sql
SELECT sid,
       serial#,
       username,
       status
FROM v$session
WHERE username IS NOT NULL;
```

---

## Practice 12 — Check Parameter

```sql
SELECT name,
       value
FROM v$parameter
WHERE name = 'processes';
```

---

# 27. Real DBA Troubleshooting Examples

## Scenario 1 — User cannot login

Check:

```sql
SELECT username,
       account_status,
       profile
FROM dba_users
WHERE username = 'MUNNA';
```

Possible problems:

```text
LOCKED
EXPIRED
EXPIRED & LOCKED
```

---

## Scenario 2 — User doesn't have required role

Check:

```sql
SELECT grantee,
       granted_role
FROM dba_role_privs
WHERE grantee = 'MUNNA';
```

---

## Scenario 3 — User doesn't have required system privilege

Check:

```sql
SELECT grantee,
       privilege
FROM dba_sys_privs
WHERE grantee = 'MUNNA';
```

---

## Scenario 4 — User cannot access another table

Check:

```sql
SELECT grantee,
       owner,
       table_name,
       privilege
FROM dba_tab_privs
WHERE grantee = 'MUNNA';
```

---

## Scenario 5 — Database status check

```sql
SELECT name,
       open_mode,
       database_role
FROM v$database;
```

---

## Scenario 6 — Find connected users

```sql
SELECT sid,
       serial#,
       username,
       status
FROM v$session
WHERE username IS NOT NULL;
```

---

# 28. Important DBA Query Cheat Sheet

```sql
-- Users
SELECT username, account_status
FROM dba_users;

-- User details
SELECT username, default_tablespace, temporary_tablespace, profile
FROM dba_users
WHERE username = 'MUNNA';

-- Roles
SELECT role
FROM dba_roles;

-- Roles granted to user
SELECT grantee, granted_role
FROM dba_role_privs
WHERE grantee = 'MUNNA';

-- System privileges
SELECT grantee, privilege
FROM dba_sys_privs
WHERE grantee = 'MUNNA';

-- Object privileges
SELECT grantee, owner, table_name, privilege
FROM dba_tab_privs
WHERE grantee = 'MUNNA';

-- Tablespaces
SELECT tablespace_name, status, contents
FROM dba_tablespaces;

-- Datafiles
SELECT file_name, tablespace_name,
       bytes / 1024 / 1024 AS size_mb
FROM dba_data_files;

-- Database
SELECT name, open_mode, database_role
FROM v$database;

-- Instance
SELECT instance_name, status, startup_time
FROM v$instance;

-- Sessions
SELECT sid, serial#, username, status
FROM v$session
WHERE username IS NOT NULL;

-- Parameter
SELECT name, value
FROM v$parameter
WHERE name = 'processes';
```

---

# 29. Interview Questions

### Q1. What is the Oracle Data Dictionary?

The Data Dictionary is a collection of Oracle-maintained tables and views containing metadata about the database.

### Q2. What is the difference between USER_, ALL_, and DBA_ views?

```text
USER_* → Objects owned by current user

ALL_*  → Objects accessible to current user

DBA_*  → Database-wide information
```

### Q3. How do you check Oracle users?

```sql
SELECT username, account_status
FROM dba_users;
```

### Q4. How do you check roles assigned to a user?

```sql
SELECT grantee, granted_role
FROM dba_role_privs
WHERE grantee = 'MUNNA';
```

### Q5. How do you check system privileges?

```sql
SELECT grantee, privilege
FROM dba_sys_privs
WHERE grantee = 'MUNNA';
```

### Q6. How do you check object privileges?

```sql
SELECT grantee, owner, table_name, privilege
FROM dba_tab_privs
WHERE grantee = 'MUNNA';
```

### Q7. What are V$ views?

V$ views are Oracle Dynamic Performance Views that provide information about the current state of the Oracle instance and database.

### Q8. How do you check database status?

```sql
SELECT name, open_mode
FROM v$database;
```

### Q9. How do you check instance status?

```sql
SELECT instance_name, status
FROM v$instance;
```

### Q10. How do you check connected sessions?

```sql
SELECT sid, serial#, username, status
FROM v$session
WHERE username IS NOT NULL;
```

### Q11. What is SID?

SID is the Session Identifier used to identify a database session.

### Q12. What is SERIAL#?

`SERIAL#` helps identify a particular session together with SID, especially when managing sessions.

---

# 30. Day 34 Summary

```text
                  DAY 34
                     |
        +------------+-------------+
        |                          |
 Data Dictionary            Dynamic Views
        |                          |
      DBA_*                       V$*
        |                          |
   +----+----+              +------+------+
   |    |    |              |             |
 Users Roles Privileges   Database     Instance
   |    |    |              |             |
   |    |    |           V$DATABASE   V$INSTANCE
   |    |    |
   |    |    +-- DBA_SYS_PRIVS
   |    +------- DBA_ROLE_PRIVS
   +------------ DBA_USERS

 Other important:
 DBA_TAB_PRIVS
 DBA_TABLESPACES
 DBA_DATA_FILES
 V$SESSION
 V$PARAMETER
```

## Day 34 Target

By the end of Day 34, you should be able to answer:

```text
✓ What users exist?
✓ Is a user locked or expired?
✓ What roles does a user have?
✓ What system privileges does a user have?
✓ What object privileges does a user have?
✓ What tablespaces exist?
✓ What datafiles exist?
✓ Is the database OPEN?
✓ Is the instance running?
✓ Which users are connected?
✓ What is the current value of an Oracle parameter?
```

> **Day 34 Core Skill:** Don't just know how to create Oracle objects — learn how to inspect and troubleshoot them.
