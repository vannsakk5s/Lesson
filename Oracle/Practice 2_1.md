# Practice 1: Overview (Security & Privileges)

To complete question 6 and the subsequent questions, you need to connect to the database using iSQL*Plus or SQL Developer. To do this, launch one of them and use DBA (sys or system) or ORA1 accounts with user name and password provided by your instructor to log on to the database.

### Practice 1:

**1. What privilege should a user be given to log on to the Oracle server? Is this a system or an object privilege?**
- **Answer:** A user should be given the `CREATE SESSION` privilege. This is a **system privilege**.

**2. What privilege should a user be given to create tables?**
- **Answer:** A user should be given the `CREATE TABLE` system privilege.

**3. If you create a table, who can pass along privileges to other users on your table?**
- **Answer:** You (the owner of the table) and the DBA can pass along privileges. Also, anyone who has been granted privileges on the table with the `WITH GRANT OPTION`.

**4. You are the DBA. You are creating many users who require the same system privileges. What should you use to make your job easier?**
- **Answer:** You should use a **Role**. You can create a role, grant the necessary system privileges to the role, and then grant the role to the users.

**5. What command do you use to change your password?**
- **Answer:** The `ALTER USER` command (or the `PASSWORD` command in SQL*Plus).
```sql
ALTER USER username IDENTIFIED BY new_password;
```

**6. Create a user, grant SELECT and INSERT permission on the Employees table to the user. Then, revoke INSERT permission on the Employees table from the user.**
```sql
-- Create user
CREATE USER user1 IDENTIFIED BY password123;
GRANT CREATE SESSION TO user1;

-- Grant permissions
GRANT SELECT, INSERT ON Employees TO user1;

-- Revoke INSERT permission
REVOKE INSERT ON Employees FROM user1;
```

**7. Create a new table, a new role, grant SELECT permission on the table to the role and grant the role to the user you have created in step 6.**
```sql
-- Create table
CREATE TABLE test_table (
  id NUMBER PRIMARY KEY,
  name VARCHAR2(50)
);

-- Create role
CREATE ROLE test_role;

-- Grant SELECT on table to role
GRANT SELECT ON test_table TO test_role;

-- Grant role to user
GRANT test_role TO user1;
```

**8. Create a new user, a database role, grant SELECT permission on the Departments table to the role and grant the role to the user.**
```sql
-- Create user
CREATE USER user2 IDENTIFIED BY password123;
GRANT CREATE SESSION TO user2;

-- Create role
CREATE ROLE dept_role;

-- Grant SELECT on Departments to role
GRANT SELECT ON Departments TO dept_role;

-- Grant role to user
GRANT dept_role TO user2;
```

**9. Create a view to restrict access to sensitive columns in the Employees table and grant SELECT permission on the view to the user you have created in step 8.**
```sql
-- Create view (excluding sensitive columns like SALARY or COMMISSION_PCT)
CREATE VIEW emp_public_view AS
SELECT employee_id, first_name, last_name, email, department_id
FROM Employees;

-- Grant SELECT on view to user2
GRANT SELECT ON emp_public_view TO user2;
```

**10. Create a user, grant and deny DELETE and UPDATE permission on the Ora1.Departments table to/from the user.**
```sql
-- Create user
CREATE USER user3 IDENTIFIED BY password123;
GRANT CREATE SESSION TO user3;

-- Grant DELETE and UPDATE permission
GRANT DELETE, UPDATE ON Ora1.Departments TO user3;

-- Revoke (deny) DELETE and UPDATE permission
REVOKE DELETE, UPDATE ON Ora1.Departments FROM user3;
```
