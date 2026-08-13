# Day 33 — Oracle Schemas, Schema Objects & Object Privileges

## 📅 Day 33

## 🎯 Objective

Today we will learn and practice Oracle Schemas from starting to end.

Topics covered:

- What is a Schema
- User vs Schema
- Schema Objects
- Creating a User
- Connecting to a User
- Current User
- Current Schema
- Creating a Table
- Object Ownership
- USER_OBJECTS
- ALL_OBJECTS
- DBA_OBJECTS
- SQL*Plus formatting
- Fully Qualified Object Names
- Cross-Schema Access
- Creating a second User
- Object Privileges
- GRANT
- SELECT privilege
- INSERT privilege
- REVOKE
- Tablespace Quota
- DBA_TS_QUOTAS
- USER_TAB_PRIVS
- DBA_TAB_PRIVS
- ORA-00904
- ORA-00942
- ORA-01031
- ORA-01950
- Principle of Least Privilege
- Complete Hands-on Practical

---

# 1. What is a Schema?

A **Schema** is a logical collection of database objects owned by a database user.

Example:

~~~text
MUNNA Schema
│
├── EMPLOYEE
├── DEPARTMENT
├── EMP_VIEW
├── EMP_SEQ
├── PROCEDURES
└── FUNCTIONS
~~~

### Simple Definition

> A schema is a logical container that holds database objects owned by a user.

---

# 2. User vs Schema

| User | Schema |
|---|---|
| Database account | Logical collection of objects |
| Has username and password | Does not have a separate password |
| Can log in | Cannot log in |
| Used for authentication | Used for object ownership |
| Owns a schema | Contains objects owned by the user |

### Easy Memory Trick

~~~text
User   = Person
Schema = Person's Room
Objects = Things inside the Room
~~~

### Remember

> **User logs in. Schema owns objects.**

---

# 3. User and Schema Relationship

When we create:

~~~sql
CREATE USER MUNNA IDENTIFIED BY Munna123;
~~~

Oracle automatically creates a schema named:

~~~text
MUNNA
~~~

Relationship:

~~~text
Oracle Database
      |
      +---- MUNNA User
                |
                +---- MUNNA Schema
                         |
                         +---- Tables
                         +---- Views
                         +---- Indexes
                         +---- Sequences
                         +---- Procedures
~~~

We normally do not create the schema separately.

---

# 4. Oracle Environment Used

Our practical environment:

~~~text
Oracle Database 21c XE
        |
        +---- XEPDB1
~~~

We performed the practical inside:

~~~text
XEPDB1
~~~

Check the current container:

~~~sql
SHOW CON_NAME;
~~~

If connected to the root container:

~~~sql
ALTER SESSION SET CONTAINER = XEPDB1;
~~~

Verify:

~~~sql
SHOW CON_NAME;
~~~

Expected:

~~~text
XEPDB1
~~~

---

# 5. Creating MUNNA User

If the user already exists and the password is unknown, we can remove and recreate it in the practice database.

Connect as SYS:

~~~sql
sqlplus / as sysdba
~~~

Check the user:

~~~sql
SELECT username
FROM dba_users
WHERE username = 'MUNNA';
~~~

If MUNNA exists:

~~~sql
DROP USER MUNNA CASCADE;
~~~

> ⚠️ `DROP USER ... CASCADE` deletes the user and all objects owned by that user. Never use this casually in production.

Create the user:

~~~sql
CREATE USER MUNNA IDENTIFIED BY Munna123;
~~~

Grant login privileges:

~~~sql
GRANT CONNECT, RESOURCE TO MUNNA;
~~~

---

# 6. Connecting as MUNNA

Connect using EZCONNECT:

~~~text
sqlplus MUNNA/Munna123@localhost:1521/XEPDB1
~~~

Check the current user:

~~~sql
SHOW USER;
~~~

Expected:

~~~text
USER is "MUNNA"
~~~

---

# 7. Check Current Schema

Run:

~~~sql
SELECT SYS_CONTEXT('USERENV', 'CURRENT_SCHEMA')
FROM DUAL;
~~~

Expected:

~~~text
MUNNA
~~~

### User vs Current Schema

~~~text
SHOW USER
    ↓
Authentication User

CURRENT_SCHEMA
    ↓
Schema Oracle uses for unqualified object names
~~~

Usually they are the same when the user has not changed the current schema.

---

# 8. Schema Objects

Objects owned by a schema are called **Schema Objects**.

Common schema objects:

~~~text
Schema
│
├── Tables
├── Views
├── Indexes
├── Sequences
├── Synonyms
├── Procedures
├── Functions
├── Packages
├── Triggers
└── Materialized Views
~~~

---

# 9. Create EMPLOYEE Table

While logged in as MUNNA:

~~~sql
CREATE TABLE employee (
    id NUMBER,
    name VARCHAR2(50),
    salary NUMBER
);
~~~

Expected:

~~~text
Table created.
~~~

The table belongs to MUNNA.

Therefore:

~~~text
MUNNA Schema
     |
     +---- EMPLOYEE
~~~

Fully qualified name:

~~~text
MUNNA.EMPLOYEE
~~~

---

# 10. Check USER_OBJECTS

Run:

~~~sql
SELECT object_name, object_type
FROM user_objects;
~~~

Expected:

~~~text
OBJECT_NAME    OBJECT_TYPE
-------------  -----------
EMPLOYEE       TABLE
~~~

This tells us that the current user's schema contains the `EMPLOYEE` table.

---

# 11. Important USER_OBJECTS Lesson

We tried:

~~~sql
SELECT owner, object_name, object_type
FROM user_objects;
~~~

Oracle returned:

~~~text
ORA-00904: "OWNER": invalid identifier
~~~

### Why?

`USER_OBJECTS` does not have an `OWNER` column.

`USER_OBJECTS` already shows objects owned by the current user.

Since we were logged in as:

~~~text
MUNNA
~~~

the objects shown are already owned by MUNNA.

---

# 12. ALL_OBJECTS

To see the owner, use `ALL_OBJECTS`.

~~~sql
SELECT owner, object_name, object_type
FROM all_objects
WHERE object_name = 'EMPLOYEE';
~~~

Output:

~~~text
OWNER           OBJECT_NAME                    OBJECT_TYPE
--------------- ------------------------------ --------------------
MUNNA           EMPLOYEE                       TABLE
~~~

This tells us:

~~~text
OWNER       = MUNNA
OBJECT_NAME = EMPLOYEE
OBJECT_TYPE = TABLE
~~~

---

# 13. DBA_OBJECTS

`DBA_OBJECTS` provides information about database objects across schemas.

Example:

~~~sql
SELECT owner, object_name, object_type
FROM dba_objects
WHERE object_name = 'EMPLOYEE';
~~~

This is useful for DBAs when investigating objects across the database.

`DBA_OBJECTS` generally requires DBA-level privileges.

---

# 14. SQL*Plus Output Formatting

Initially, output can wrap when the line size is too small.

Use:

~~~sql
SET LINESIZE 100;
~~~

For more rows per page:

~~~sql
SET PAGESIZE 100;
~~~

Format columns:

~~~sql
COLUMN OWNER FORMAT A15
COLUMN OBJECT_NAME FORMAT A30
COLUMN OBJECT_TYPE FORMAT A20
~~~

Then:

~~~sql
SELECT owner, object_name, object_type
FROM all_objects
WHERE object_name = 'EMPLOYEE';
~~~

Clean output:

~~~text
OWNER           OBJECT_NAME                    OBJECT_TYPE
--------------- ------------------------------ --------------------
MUNNA           EMPLOYEE                       TABLE
~~~

---

# 15. Fully Qualified Object Name

Oracle uses:

~~~text
schema.object
~~~

Example:

~~~text
MUNNA.EMPLOYEE
~~~

Meaning:

~~~text
MUNNA     → Schema
EMPLOYEE  → Object
~~~

This is important when accessing another schema's object.

---

# 16. EMPLOYEE vs MUNNA.EMPLOYEE

When logged in as MUNNA:

~~~sql
SELECT * FROM employee;
~~~

Oracle searches the current schema.

Therefore:

~~~text
EMPLOYEE
   ↓
MUNNA.EMPLOYEE
~~~

We can also explicitly specify:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

Both refer to the same table when the current schema is MUNNA.

---

# 17. Empty Table vs Missing Table

If we run:

~~~sql
SELECT * FROM employee;
~~~

and Oracle returns:

~~~text
no rows selected
~~~

that does **not** mean the table is missing.

It means:

~~~text
Table exists
     +
Table contains 0 rows
~~~

If the table does not exist or cannot be accessed, Oracle may return:

~~~text
ORA-00942: table or view does not exist
~~~

---

# 18. Insert Data into EMPLOYEE

As MUNNA:

~~~sql
INSERT INTO employee
(id, name, salary)
VALUES (1, 'Munna', 25000);
~~~

Commit:

~~~sql
COMMIT;
~~~

Check:

~~~sql
SELECT * FROM employee;
~~~

The row belongs to:

~~~text
MUNNA.EMPLOYEE
~~~

---

# 19. Multiple Schemas Can Have the Same Table Name

Suppose:

~~~text
MUNNA Schema
    |
    +---- EMPLOYEE

RAHUL Schema
    |
    +---- EMPLOYEE
~~~

This is allowed.

Oracle identifies them as:

~~~text
MUNNA.EMPLOYEE
RAHUL.EMPLOYEE
~~~

They are different objects:

~~~text
MUNNA.EMPLOYEE != RAHUL.EMPLOYEE
~~~

---

# 20. Create RAHUL User

Now we create another user to practice cross-schema access.

Connect as SYS:

~~~sql
sqlplus / as sysdba
~~~

Make sure we are in XEPDB1:

~~~sql
ALTER SESSION SET CONTAINER = XEPDB1;
~~~

Create RAHUL:

~~~sql
CREATE USER RAHUL IDENTIFIED BY Rahul123;
~~~

Grant login:

~~~sql
GRANT CONNECT TO RAHUL;
~~~

---

# 21. RAHUL Attempts to Access MUNNA Table

Connect as RAHUL:

~~~text
sqlplus RAHUL/Rahul123@localhost:1521/XEPDB1
~~~

Check:

~~~sql
SHOW USER;
~~~

Expected:

~~~text
USER is "RAHUL"
~~~

Try:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

Initially we received:

~~~text
ORA-00942: table or view does not exist
~~~

### Why?

RAHUL does not automatically have permission to access MUNNA's table.

~~~text
RAHUL
   |
   X
   |
MUNNA.EMPLOYEE
~~~

---

# 22. Cross-Schema Access

When one user accesses an object owned by another schema, this is called **Cross-Schema Access**.

Example:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

Here:

~~~text
Current User = RAHUL
Object Owner = MUNNA
Object        = EMPLOYEE
~~~

---

# 23. Grant SELECT Privilege

As SYS or the object owner MUNNA:

~~~sql
GRANT SELECT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

Expected:

~~~text
Grant succeeded.
~~~

Now RAHUL can:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

---

# 24. Check RAHUL's Privilege

As RAHUL:

~~~sql
SELECT owner, table_name, privilege
FROM user_tab_privs
WHERE owner = 'MUNNA';
~~~

Expected:

~~~text
OWNER    TABLE_NAME    PRIVILEGE
-------  ------------  ---------
MUNNA    EMPLOYEE      SELECT
~~~

This means:

~~~text
RAHUL
  |
  +---- SELECT
          |
          v
     MUNNA.EMPLOYEE
~~~

---

# 25. Object Privileges

Object privileges control what a user can do with a specific object.

Common object privileges:

~~~text
SELECT
INSERT
UPDATE
DELETE
ALTER
INDEX
REFERENCES
EXECUTE
~~~

---

# 26. SELECT Privilege

Grant:

~~~sql
GRANT SELECT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

RAHUL can:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

But SELECT does not automatically give:

~~~text
INSERT
UPDATE
DELETE
~~~

---

# 27. Test INSERT Without INSERT Privilege

RAHUL tried:

~~~sql
INSERT INTO MUNNA.EMPLOYEE
(id, name, salary)
VALUES (2, 'Rahul', 30000);
~~~

Oracle returned:

~~~text
ORA-01031: insufficient privileges
~~~

### Why?

RAHUL had:

~~~text
SELECT = YES
INSERT = NO
~~~

Therefore:

~~~text
SELECT  ✅
INSERT  ❌
UPDATE  ❌
DELETE  ❌
~~~

---

# 28. Grant INSERT Privilege

As SYS or MUNNA:

~~~sql
GRANT INSERT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

Now:

~~~text
RAHUL
  |
  +---- MUNNA.EMPLOYEE
           |
           +---- SELECT  ✅
           +---- INSERT  ✅
           +---- UPDATE  ❌
           +---- DELETE  ❌
~~~

---

# 29. Test INSERT Again

As RAHUL:

~~~sql
INSERT INTO MUNNA.EMPLOYEE
(id, name, salary)
VALUES (2, 'Rahul', 30000);
~~~

At this point, we encountered:

~~~text
ORA-01950: no privileges on tablespace 'USERS'
~~~

This is a different problem from `ORA-01031`.

---

# 30. Understanding ORA-01031 vs ORA-01950

## ORA-01031

~~~text
ORA-01031: insufficient privileges
~~~

Meaning:

> The user does not have the required database/object privilege.

In our case:

~~~text
RAHUL had SELECT
RAHUL did not have INSERT
~~~

Solution:

~~~sql
GRANT INSERT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

---

## ORA-01950

~~~text
ORA-01950: no privileges on tablespace 'USERS'
~~~

Meaning:

> The required user does not have sufficient quota on the tablespace for storage allocation.

This is related to **storage**, not the INSERT object privilege.

---

# 31. Tablespace Quota

A quota controls how much space a user can consume in a particular tablespace.

Example:

~~~sql
ALTER USER MUNNA
QUOTA 50M ON USERS;
~~~

Meaning:

~~~text
MUNNA
  |
  +---- USERS Tablespace
          |
          +---- Maximum quota = 50 MB
~~~

---

# 32. Why MUNNA Needed Quota

The table is:

~~~text
MUNNA.EMPLOYEE
~~~

Therefore the table owner is:

~~~text
MUNNA
~~~

When Oracle needs to allocate storage for the table, the table owner's quota matters.

Our flow was:

~~~text
RAHUL
  |
  | INSERT
  v
MUNNA.EMPLOYEE
  |
  | Storage required
  v
USERS Tablespace
  |
  | Check MUNNA quota
  v
No sufficient quota
  |
  v
ORA-01950
~~~

---

# 33. Give MUNNA Tablespace Quota

As SYS:

~~~sql
ALTER USER MUNNA
QUOTA 50M ON USERS;
~~~

Expected:

~~~text
User altered.
~~~

---

# 34. Check MUNNA Quota

As SYS:

~~~sql
SELECT username, tablespace_name, max_bytes
FROM dba_ts_quotas
WHERE username = 'MUNNA';
~~~

Example:

~~~text
USERNAME   TABLESPACE_NAME   MAX_BYTES
---------  ----------------  ----------
MUNNA      USERS             52428800
~~~

`52428800` bytes = 50 MB.

---

# 35. Important Practical Lesson About RAHUL Quota

We also tested:

~~~sql
ALTER USER RAHUL QUOTA 50M ON USERS;
~~~

Verification:

~~~sql
SELECT username, tablespace_name, max_bytes
FROM dba_ts_quotas
WHERE username = 'RAHUL';
~~~

Output:

~~~text
USERNAME   TABLESPACE_NAME   MAX_BYTES
---------  ----------------  ----------
RAHUL      USERS             52428800
~~~

However, RAHUL's quota did not fix the INSERT into:

~~~text
MUNNA.EMPLOYEE
~~~

because the table is owned by MUNNA.

Therefore we needed:

~~~sql
ALTER USER MUNNA
QUOTA 50M ON USERS;
~~~

### Important DBA Concept

> Object privileges control access to the object. Tablespace quotas control storage allocation for objects owned by the user.

---

# 36. Test INSERT After Quota

Return to RAHUL.

Run:

~~~sql
INSERT INTO MUNNA.EMPLOYEE
(id, name, salary)
VALUES (2, 'Rahul', 30000);
~~~

Then:

~~~sql
COMMIT;
~~~

Now the INSERT succeeds.

---

# 37. Verify Data

As RAHUL:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

Example:

~~~text
ID    NAME     SALARY
----  -------  ------
1     Munna    25000
2     Rahul    30000
~~~

Notice:

~~~text
RAHUL
  |
  +---- Can access
          |
          v
     MUNNA.EMPLOYEE
~~~

But ownership remains:

~~~text
OWNER = MUNNA
~~~

---

# 38. Object Ownership vs Object Access

This is extremely important.

~~~text
MUNNA
  |
  +---- OWNS ----> EMPLOYEE
                       ^
                       |
                    ACCESS
                       |
                     RAHUL
~~~

RAHUL can access the table.

RAHUL does not become the owner.

The owner remains:

~~~text
MUNNA
~~~

---

# 39. Checking Object Privileges as RAHUL

~~~sql
SELECT owner, table_name, privilege
FROM user_tab_privs
WHERE owner = 'MUNNA';
~~~

Expected:

~~~text
OWNER    TABLE_NAME    PRIVILEGE
-------  ------------  ---------
MUNNA    EMPLOYEE      SELECT
MUNNA    EMPLOYEE      INSERT
~~~

---

# 40. DBA_TAB_PRIVS

As SYS:

~~~sql
SELECT grantee,
       owner,
       table_name,
       privilege
FROM dba_tab_privs
WHERE owner = 'MUNNA'
AND table_name = 'EMPLOYEE';
~~~

Example:

~~~text
GRANTEE   OWNER   TABLE_NAME   PRIVILEGE
--------  ------  -----------  ---------
RAHUL     MUNNA   EMPLOYEE     SELECT
RAHUL     MUNNA   EMPLOYEE     INSERT
~~~

This tells the DBA exactly who has privileges on the table.

---

# 41. USER_TAB_PRIVS vs DBA_TAB_PRIVS

| View | Purpose |
|---|---|
| `USER_TAB_PRIVS` | Object privileges available to current user |
| `DBA_TAB_PRIVS` | Object privileges across the database |
| `ALL_TAB_PRIVS` | Object privileges accessible to current user |

For DBA work, `DBA_TAB_PRIVS` is especially useful.

---

# 42. Checking Tablespace Quotas

As SYS:

~~~sql
SELECT username,
       tablespace_name,
       max_bytes
FROM dba_ts_quotas;
~~~

For MUNNA:

~~~sql
SELECT username,
       tablespace_name,
       max_bytes
FROM dba_ts_quotas
WHERE username = 'MUNNA';
~~~

For both users:

~~~sql
SELECT username,
       tablespace_name,
       max_bytes
FROM dba_ts_quotas
WHERE username IN ('MUNNA', 'RAHUL');
~~~

---

# 43. GRANT Syntax

General syntax:

~~~sql
GRANT privilege
ON schema.object
TO user;
~~~

Example:

~~~sql
GRANT SELECT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

Multiple privileges:

~~~sql
GRANT SELECT, INSERT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

---

# 44. REVOKE Syntax

General syntax:

~~~sql
REVOKE privilege
ON schema.object
FROM user;
~~~

Example:

~~~sql
REVOKE SELECT
ON MUNNA.EMPLOYEE
FROM RAHUL;
~~~

---

# 45. Testing REVOKE

After:

~~~sql
REVOKE SELECT
ON MUNNA.EMPLOYEE
FROM RAHUL;
~~~

RAHUL will no longer have the SELECT privilege.

Check:

~~~sql
SELECT owner, table_name, privilege
FROM user_tab_privs
WHERE owner = 'MUNNA';
~~~

If SELECT was revoked, only remaining privileges will be displayed.

---

# 46. Common Object Privileges

| Privilege | Purpose |
|---|---|
| `SELECT` | Read data |
| `INSERT` | Add rows |
| `UPDATE` | Modify rows |
| `DELETE` | Delete rows |
| `ALTER` | Alter an object |
| `INDEX` | Create an index on a table |
| `REFERENCES` | Create foreign key references |
| `EXECUTE` | Execute procedures/functions/packages |

---

# 47. Data Dictionary Views

## USER_OBJECTS

Objects owned by the current user:

~~~sql
SELECT object_name, object_type
FROM user_objects;
~~~

## ALL_OBJECTS

Objects accessible to the current user:

~~~sql
SELECT owner, object_name, object_type
FROM all_objects;
~~~

## DBA_OBJECTS

Database-wide object information:

~~~sql
SELECT owner, object_name, object_type
FROM dba_objects;
~~~

## USER_TAB_PRIVS

Object privileges for the current user:

~~~sql
SELECT owner, table_name, privilege
FROM user_tab_privs;
~~~

## DBA_TAB_PRIVS

Database-wide object privileges:

~~~sql
SELECT grantee, owner, table_name, privilege
FROM dba_tab_privs;
~~~

## DBA_TS_QUOTAS

Tablespace quotas:

~~~sql
SELECT username, tablespace_name, max_bytes
FROM dba_ts_quotas;
~~~

---

# 48. SQL*Plus Commands Used

## Current User

~~~sql
SHOW USER;
~~~

## Current Container

~~~sql
SHOW CON_NAME;
~~~

## Change Container

~~~sql
ALTER SESSION SET CONTAINER = XEPDB1;
~~~

## Linesize

~~~sql
SET LINESIZE 100;
~~~

## Pagesize

~~~sql
SET PAGESIZE 100;
~~~

## Column Formatting

~~~sql
COLUMN OWNER FORMAT A15;
COLUMN OBJECT_NAME FORMAT A30;
COLUMN OBJECT_TYPE FORMAT A20;
~~~

---

# 49. Practical Architecture

Our final practical environment:

~~~text
                         ORACLE DATABASE
                                |
                              XEPDB1
                                |
              +-----------------+----------------+
              |                                  |
            MUNNA                              RAHUL
             User                               User
              |                                  |
         MUNNA Schema                       RAHUL Schema
              |                                  |
              |                                  |
         EMPLOYEE Table                      Objects
              |
              |
       +------+------+
       |             |
    Munna           Rahul
     Row             Row
~~~

Access:

~~~text
RAHUL
  |
  +---- SELECT ----> MUNNA.EMPLOYEE
  |
  +---- INSERT ----> MUNNA.EMPLOYEE
~~~

Ownership:

~~~text
MUNNA
  |
  +---- OWNER ----> EMPLOYEE
~~~

Storage:

~~~text
MUNNA
  |
  +---- 50M Quota
           |
           v
        USERS
       Tablespace
~~~

---

# 50. Complete Practical Flow

~~~text
START
  |
  v
Connect as SYS
  |
  v
Check XEPDB1
  |
  v
Create/Recreate MUNNA
  |
  v
CREATE USER MUNNA
  |
  v
GRANT CONNECT, RESOURCE
  |
  v
Connect as MUNNA
  |
  v
SHOW USER
  |
  v
Check CURRENT_SCHEMA
  |
  v
Create EMPLOYEE table
  |
  v
Check USER_OBJECTS
  |
  v
Check ALL_OBJECTS
  |
  v
Insert data
  |
  v
Connect as SYS
  |
  v
Create RAHUL
  |
  v
GRANT CONNECT
  |
  v
Connect as RAHUL
  |
  v
Try SELECT MUNNA.EMPLOYEE
  |
  v
ORA-00942
  |
  v
GRANT SELECT
  |
  v
SELECT succeeds
  |
  v
RAHUL tries INSERT
  |
  v
ORA-01031
  |
  v
GRANT INSERT
  |
  v
Try INSERT again
  |
  v
ORA-01950
  |
  v
Give MUNNA quota on USERS
  |
  v
ALTER USER MUNNA QUOTA 50M ON USERS
  |
  v
Try INSERT again
  |
  v
INSERT succeeds
  |
  v
COMMIT
  |
  v
Verify data
  |
  v
DAY 33 COMPLETE
~~~

---

# 51. Errors We Encountered

## ORA-00904

~~~text
ORA-00904: "OWNER": invalid identifier
~~~

### Cause

We tried to select `OWNER` from `USER_OBJECTS`.

### Wrong

~~~sql
SELECT owner, object_name, object_type
FROM user_objects;
~~~

### Correct

~~~sql
SELECT object_name, object_type
FROM user_objects;
~~~

Or:

~~~sql
SELECT owner, object_name, object_type
FROM all_objects;
~~~

---

## ORA-00942

~~~text
ORA-00942: table or view does not exist
~~~

### Cause

RAHUL tried to access:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

without the required object privilege.

### Solution

~~~sql
GRANT SELECT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

---

## ORA-01031

~~~text
ORA-01031: insufficient privileges
~~~

### Cause

RAHUL had SELECT but did not have INSERT.

### Solution

~~~sql
GRANT INSERT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

---

## ORA-01950

~~~text
ORA-01950: no privileges on tablespace 'USERS'
~~~

### Cause

The table owner MUNNA did not have sufficient quota on the USERS tablespace.

### Solution

~~~sql
ALTER USER MUNNA
QUOTA 50M ON USERS;
~~~

---

# 52. Security — Principle of Least Privilege

Users should receive only the privileges they actually need.

Bad approach:

~~~sql
GRANT ALL
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

Better:

~~~sql
GRANT SELECT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

If RAHUL also needs to insert:

~~~sql
GRANT INSERT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

### Principle

> Give users only the minimum privileges required for their job.

This is called the **Principle of Least Privilege**.

---

# 53. Real-World DBA Example

A production database may contain:

~~~text
Oracle Database
│
├── HR Schema
│   ├── EMPLOYEES
│   ├── DEPARTMENTS
│   └── JOBS
│
├── FINANCE Schema
│   ├── SALARY
│   ├── PAYROLL
│   └── BONUS
│
├── SALES Schema
│   ├── CUSTOMERS
│   ├── ORDERS
│   └── PRODUCTS
│
└── REPORT Schema
    ├── SALES_REPORT
    └── FINANCE_REPORT
~~~

A DBA might grant:

~~~sql
GRANT SELECT
ON SALES.ORDERS
TO REPORT;
~~~

Now the REPORT user can read SALES data without owning the SALES tables.

This is a common enterprise database design.

---

# 54. Interview Questions

## Q1. What is a schema?

A schema is a logical collection of database objects owned by a database user.

## Q2. Is a user the same as a schema?

No.

A user is a database account, while a schema contains the objects owned by that user.

## Q3. What happens when a user is created?

Oracle automatically creates a schema with the same name.

## Q4. Can two schemas have tables with the same name?

Yes.

Example:

~~~text
MUNNA.EMPLOYEE
RAHUL.EMPLOYEE
~~~

They are different objects.

## Q5. What is a fully qualified object name?

It is:

~~~text
schema.object
~~~

Example:

~~~text
MUNNA.EMPLOYEE
~~~

## Q6. What is cross-schema access?

When one user accesses an object owned by another schema.

Example:

~~~sql
SELECT * FROM MUNNA.EMPLOYEE;
~~~

## Q7. How do you grant SELECT?

~~~sql
GRANT SELECT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

## Q8. How do you grant INSERT?

~~~sql
GRANT INSERT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

## Q9. How do you revoke SELECT?

~~~sql
REVOKE SELECT
ON MUNNA.EMPLOYEE
FROM RAHUL;
~~~

## Q10. What is a tablespace quota?

A quota controls how much storage a user can consume in a particular tablespace.

## Q11. How do you give quota?

~~~sql
ALTER USER MUNNA
QUOTA 50M ON USERS;
~~~

## Q12. What is the difference between object privilege and quota?

Object privilege controls what a user can do with an object.

Quota controls how much storage a user can consume in a tablespace.

## Q13. What is USER_OBJECTS?

It displays objects owned by the current user.

## Q14. What is ALL_OBJECTS?

It displays objects accessible to the current user.

## Q15. What is DBA_OBJECTS?

It provides database-wide information about objects and is commonly used by DBAs.

## Q16. What is USER_TAB_PRIVS?

It shows object privileges available to the current user.

## Q17. What is DBA_TAB_PRIVS?

It shows object privileges across the database.

## Q18. What is DBA_TS_QUOTAS?

It shows tablespace quotas assigned to users.

## Q19. Why did RAHUL receive ORA-01031?

Because RAHUL had SELECT privilege but did not have INSERT privilege.

## Q20. Why did we receive ORA-01950?

Because MUNNA, the owner of the EMPLOYEE table, did not have sufficient quota on the USERS tablespace.

## Q21. Does granting INSERT to RAHUL make RAHUL the owner?

No.

The owner remains MUNNA.

## Q22. Can RAHUL access MUNNA's table?

Yes, if MUNNA or a DBA grants the required object privileges.

---

# 55. Final Day 33 Summary

~~~text
USER
  |
  | owns
  v
SCHEMA
  |
  | owns
  v
SCHEMA OBJECTS
  |
  +---- TABLE
  +---- VIEW
  +---- INDEX
  +---- SEQUENCE
  +---- PROCEDURE
  +---- FUNCTION
~~~

Cross-schema access:

~~~text
RAHUL
  |
  | SELECT / INSERT
  v
MUNNA.EMPLOYEE
  |
  +---- OWNER = MUNNA
~~~

Storage:

~~~text
MUNNA
  |
  | QUOTA 50M
  v
USERS Tablespace
~~~

---

# 56. Most Important Concepts to Remember

### 1. User

~~~text
User = Database account
~~~

### 2. Schema

~~~text
Schema = Collection of objects owned by a user
~~~

### 3. Object

~~~text
Object = Table / View / Index / Sequence / Procedure / Function / etc.
~~~

### 4. Object Privilege

~~~text
Privilege = What another user can do with an object
~~~

Example:

~~~sql
GRANT SELECT
ON MUNNA.EMPLOYEE
TO RAHUL;
~~~

### 5. Tablespace Quota

~~~text
Quota = How much storage a user can consume
~~~

Example:

~~~sql
ALTER USER MUNNA
QUOTA 50M ON USERS;
~~~

---

# 57. One-Line DBA Mental Model

~~~text
USER
  ↓
SCHEMA
  ↓
OBJECT
  ↓
OWNER
  ↓
GRANT / REVOKE
  ↓
ACCESS CONTROL
  ↓
TABLESPACE QUOTA
  ↓
STORAGE CONTROL
~~~

### ⭐ Remember

> **User logs in → Schema owns objects → Privileges control access → Quota controls storage.**

---

# 🏆 Day 33 Checklist

- [x] User vs Schema
- [x] Understand Schema
- [x] Schema Objects
- [x] Create/Recreate MUNNA
- [x] Connect as MUNNA
- [x] Check Current User
- [x] Check Current Schema
- [x] Create EMPLOYEE table
- [x] Understand Table Ownership
- [x] USER_OBJECTS
- [x] ALL_OBJECTS
- [x] DBA_OBJECTS
- [x] SQL*Plus Output Formatting
- [x] Fully Qualified Object Names
- [x] Create RAHUL
- [x] Connect as RAHUL
- [x] Cross-Schema Access
- [x] Grant SELECT
- [x] Grant INSERT
- [x] Test SELECT
- [x] Test INSERT
- [x] REVOKE
- [x] Tablespace Quota
- [x] DBA_TS_QUOTAS
- [x] USER_TAB_PRIVS
- [x] DBA_TAB_PRIVS
- [x] ORA-00904
- [x] ORA-00942
- [x] ORA-01031
- [x] ORA-01950
- [x] Principle of Least Privilege
- [x] Complete Cross-Schema Practical

---

# 🎉 DAY 33 COMPLETE

## Next Target

**Day 34 — Oracle Views & Synonyms**
