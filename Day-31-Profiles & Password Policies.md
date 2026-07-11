# Day 31 – Profiles & Password Policies

## Objective

Learn how Oracle uses **Profiles** to enforce:

- Password security
- Resource limits
- Session restrictions
- Account locking policies

Profiles allow DBAs to apply the same security rules to multiple users instead of configuring each user individually.

---

# What is a Profile?

A **Profile** is a collection of password and resource management rules that can be assigned to one or more database users.

Instead of configuring security settings for every user separately, Oracle stores these rules inside a profile.

Example:

```
Developer Profile

Password expires after 60 days

Maximum failed login attempts = 3

Idle session = 20 minutes

Maximum connection time = 120 minutes
```

Assign this profile to:

- MUNNA
- RAHUL
- PRIYA
- AMIT

All users automatically inherit these settings.

---

# Why Profiles?

Without Profiles

```
User A → Password expires after 30 days

User B → Password expires after 90 days

User C → Password expires after 45 days
```

Managing each user individually becomes difficult.

With Profiles

```
Developer Profile

↓

Assigned to all developers

↓

Same password policy

Same session policy

Easy administration
```

Benefits

- Centralized administration
- Better security
- Standard policies
- Easier maintenance
- Suitable for enterprise environments

---

# Types of Profile Limits

Oracle profiles contain two categories of limits.

## 1. Password Limits

These control password-related security.

Examples

- Password lifetime
- Password expiration
- Failed login attempts
- Password reuse
- Password complexity
- Grace period
- Account lock duration

---

## 2. Resource Limits

These control database resource usage.

Examples

- Idle time
- Connect time
- CPU usage
- Sessions per user
- Logical reads

---

# Check Existing Profiles

```sql
SELECT profile
FROM dba_profiles
GROUP BY profile;
```

Example Output

```
PROFILE
------------------------
DEFAULT
ORA_STIG_PROFILE
APP_PROFILE
```

---

# View Complete Profile Details

```sql
SELECT profile,
       resource_name,
       limit
FROM dba_profiles
ORDER BY profile;
```

Example

```
PROFILE      RESOURCE_NAME            LIMIT

DEFAULT      PASSWORD_LIFE_TIME       180

DEFAULT      FAILED_LOGIN_ATTEMPTS    10

DEFAULT      IDLE_TIME                UNLIMITED
```

---

# Default Profile

Every newly created user is automatically assigned the **DEFAULT** profile unless another profile is specified.

Check assigned profiles

```sql
SELECT username,
       profile
FROM dba_users;
```

Example

```
USERNAME      PROFILE

SYS           DEFAULT

SYSTEM        DEFAULT

MUNNA         DEFAULT
```

---

# Password Parameters

## PASSWORD_LIFE_TIME

Defines how long a password remains valid.

Example

```text
PASSWORD_LIFE_TIME 60
```

Meaning

```
Password expires after 60 days.
```

---

## PASSWORD_GRACE_TIME

Number of days users can still log in after password expiration.

Example

```text
PASSWORD_LIFE_TIME 60

PASSWORD_GRACE_TIME 7
```

Timeline

```
Day 1

↓

Password Created

↓

Day 60

Password Expired

↓

Day 61–67

Login Allowed

↓

After Day 67

Login Rejected
```

---

## FAILED_LOGIN_ATTEMPTS

Maximum incorrect password attempts before account locking.

Example

```text
FAILED_LOGIN_ATTEMPTS 3
```

Result

```
Wrong Password

↓

Attempt 1

↓

Attempt 2

↓

Attempt 3

↓

Account Locked
```

---

## PASSWORD_LOCK_TIME

Specifies how long the account remains locked.

Example

```text
PASSWORD_LOCK_TIME 1
```

Meaning

```
Account remains locked for 1 day.
```

---

## PASSWORD_REUSE_TIME

Number of days before an old password can be reused.

Example

```
365
```

Meaning

Old passwords cannot be reused within one year.

---

## PASSWORD_REUSE_MAX

Specifies how many previous passwords Oracle remembers.

Example

```
5
```

Meaning

The last five passwords cannot be reused.

---

## PASSWORD_VERIFY_FUNCTION

Ensures password complexity.

Weak Password

```
abc123
```

Rejected

Strong Password

```
Munna@123
```

Accepted

---

# Resource Parameters

## IDLE_TIME

Maximum inactive session time.

Example

```
20
```

Meaning

Oracle disconnects users after 20 minutes of inactivity.

---

## CONNECT_TIME

Maximum duration of a database session.

Example

```
120
```

Meaning

Users can remain connected for only 120 minutes.

---

## SESSIONS_PER_USER

Maximum simultaneous logins.

Example

```
2
```

Allowed

- Laptop
- Desktop

Third login

```
Rejected
```

---

## CPU_PER_SESSION

Limits CPU usage for a session.

Useful for preventing excessive CPU consumption.

---

# Create a Profile

```sql
CREATE PROFILE developer_profile
LIMIT
    FAILED_LOGIN_ATTEMPTS 3
    PASSWORD_LIFE_TIME 60
    PASSWORD_GRACE_TIME 7
    PASSWORD_LOCK_TIME 1
    IDLE_TIME 20
    CONNECT_TIME 120;
```

Oracle creates a profile with these limits.

---

# Assign Profile to User

```sql
ALTER USER munna
PROFILE developer_profile;
```

Verify

```sql
SELECT username,
       profile
FROM dba_users
WHERE username='MUNNA';
```

Output

```
USERNAME      PROFILE

MUNNA         DEVELOPER_PROFILE
```

---

# Modify a Profile

```sql
ALTER PROFILE developer_profile
LIMIT
PASSWORD_LIFE_TIME 90;
```

Now passwords expire after 90 days.

---

# Drop a Profile

```sql
DROP PROFILE developer_profile;
```

If users are assigned

```
ORA-02382
```

Use

```sql
DROP PROFILE developer_profile CASCADE;
```

Users automatically return to the DEFAULT profile.

---

# Unlock a Locked Account

```sql
ALTER USER munna ACCOUNT UNLOCK;
```

Unlock and reset password

```sql
ALTER USER munna
IDENTIFIED BY NewPass@123
ACCOUNT UNLOCK;
```

---

# View Profile Details

```sql
SELECT profile,
       resource_name,
       limit
FROM dba_profiles
WHERE profile='DEVELOPER_PROFILE';
```

---

# Practical Exercise

## Step 1

Login as SYS

```sql
sqlplus / as sysdba
```

---

## Step 2

Create a user

```sql
CREATE USER test_profile
IDENTIFIED BY "Test@123";
```

---

## Step 3

Grant login permission

```sql
GRANT CREATE SESSION TO test_profile;
```

---

## Step 4

Create profile

```sql
CREATE PROFILE test_profile_policy
LIMIT
FAILED_LOGIN_ATTEMPTS 3
PASSWORD_LIFE_TIME 30
PASSWORD_GRACE_TIME 5
PASSWORD_LOCK_TIME 1
IDLE_TIME 15
CONNECT_TIME 60;
```

---

## Step 5

Assign profile

```sql
ALTER USER test_profile
PROFILE test_profile_policy;
```

---

## Step 6

Verify

```sql
SELECT username,
       profile
FROM dba_users
WHERE username='TEST_PROFILE';
```

---

## Step 7

View profile

```sql
SELECT profile,
       resource_name,
       limit
FROM dba_profiles
WHERE profile='TEST_PROFILE_POLICY'
ORDER BY resource_name;
```

---

## Step 8

Modify profile

```sql
ALTER PROFILE test_profile_policy
LIMIT
IDLE_TIME 30;
```

---

## Step 9

Verify modification

```sql
SELECT resource_name,
       limit
FROM dba_profiles
WHERE profile='TEST_PROFILE_POLICY'
AND resource_name='IDLE_TIME';
```

---

## Step 10 (Optional Cleanup)

```sql
DROP PROFILE test_profile_policy CASCADE;

DROP USER test_profile;
```

---

# Real-Time Company Examples

## Developer Profile

- Password expires every 60 days
- 5 failed login attempts
- Idle timeout 30 minutes

---

## HR Profile

- Password expires every 30 days
- Only one concurrent session
- Strong password complexity

---

## DBA Profile

- Long session limits
- Strong password policy
- More resource privileges

Different departments often use different profiles based on business requirements.

---

# Data Dictionary Views

## DBA_PROFILES

Stores profile definitions.

```sql
SELECT *
FROM dba_profiles;
```

---

## DBA_USERS

Shows assigned profile for every user.

```sql
SELECT username,
       profile
FROM dba_users;
```

---

# Important SQL Commands

Create Profile

```sql
CREATE PROFILE profile_name
LIMIT ...;
```

Assign Profile

```sql
ALTER USER username
PROFILE profile_name;
```

Modify Profile

```sql
ALTER PROFILE profile_name
LIMIT ...;
```

Drop Profile

```sql
DROP PROFILE profile_name CASCADE;
```

Unlock User

```sql
ALTER USER username ACCOUNT UNLOCK;
```

View Profiles

```sql
SELECT *
FROM dba_profiles;
```

---

# Interview Questions

## 1. What is a Profile?

A Profile is a named collection of password and resource management rules assigned to database users.

---

## 2. Why are Profiles used?

To centrally manage password policies and resource limits for multiple users.

---

## 3. Which view stores profile information?

```
DBA_PROFILES
```

---

## 4. Which view shows the profile assigned to users?

```
DBA_USERS
```

---

## 5. What is the default profile name?

```
DEFAULT
```

---

## 6. What does FAILED_LOGIN_ATTEMPTS do?

Locks the account after the specified number of incorrect password attempts.

---

## 7. Difference between PASSWORD_LIFE_TIME and PASSWORD_GRACE_TIME?

PASSWORD_LIFE_TIME

- Password validity period.

PASSWORD_GRACE_TIME

- Additional login period after password expiration.

---

## 8. Can one profile be assigned to multiple users?

Yes.

---

## 9. How do you assign a profile?

```sql
ALTER USER username
PROFILE profile_name;
```

---

## 10. How do you modify a profile?

```sql
ALTER PROFILE profile_name
LIMIT parameter value;
```

---

# Key Points to Remember

- Profile = Collection of security rules.
- One profile can be assigned to many users.
- DEFAULT is automatically assigned if no profile is specified.
- Password parameters control security.
- Resource parameters control database usage.
- Profile information is stored in DBA_PROFILES.
- User profile assignments are stored in DBA_USERS.
- Accounts can be automatically locked after failed login attempts.
- Passwords can expire automatically.
- Profiles simplify user administration in enterprise environments.
