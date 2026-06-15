# Practice 10: Overview (DDL)

Create new tables by using the `CREATE TABLE` statement. Confirm that the new table was added to the database. You also learn to set the status of a table as READ ONLY and then revert to READ/WRITE.

### Practice 10

**1. Create the DEPT table based on the following table instance chart. Save the statement in a script called `lab_10_01.sql`, and then execute the statement in the script to create the table. Confirm that the table is created.**
```sql
-- Save this to lab_10_01.sql
CREATE TABLE dept (
  id NUMBER(7) NOT NULL,
  name VARCHAR2(25)
);

-- Confirm creation
DESCRIBE dept;
```

**2. Populate the DEPT table with data from the DEPARTMENTS table. Include only columns that you need.**
```sql
INSERT INTO dept (id, name)
SELECT department_id, department_name
FROM departments;

COMMIT;
```

**3. Create the EMP table based on the following table instance chart. Save the statement in a script called `lab_10_03.sql`, and then execute the statement in the script to create the table. Confirm that the table is created.**
```sql
-- Save this to lab_10_03.sql
CREATE TABLE emp (
  id NUMBER(7),
  last_name VARCHAR2(25),
  first_name VARCHAR2(25),
  dept_id NUMBER(7)
);

-- Confirm creation
DESCRIBE emp;
```

**4. Create the EMPLOYEES2 table based on the structure of the EMPLOYEES table. Include only the EMPLOYEE_ID, FIRST_NAME, LAST_NAME, SALARY, and DEPARTMENT_ID columns. Name the columns in your new table ID, FIRST_NAME, LAST_NAME, SALARY, and DEPT_ID, respectively.**
```sql
CREATE TABLE employees2 AS
SELECT employee_id AS id, 
       first_name, 
       last_name, 
       salary, 
       department_id AS dept_id
FROM employees;
```

**5. Alter the EMPLOYEES2 table status to read-only.**
```sql
ALTER TABLE employees2 READ ONLY;
```

**6. Try to insert the following row in the EMPLOYEES2 table:**
*(ID: 34, FIRST_NAME: Grant, LAST_NAME: Marcie, SALARY: 5678, DEPT_ID: 10)*
```sql
INSERT INTO employees2 (id, first_name, last_name, salary, dept_id)
VALUES (34, 'Grant', 'Marcie', 5678, 10);
-- You will get an error message (ORA-12081) because the table is in READ ONLY mode.
```

**7. Revert the EMPLOYEES2 table to the read/write status. Now, try to insert the same row again. You should get the following messages:**
```sql
ALTER TABLE employees2 READ WRITE;

INSERT INTO employees2 (id, first_name, last_name, salary, dept_id)
VALUES (34, 'Grant', 'Marcie', 5678, 10);
-- You should get a message saying "1 row inserted."
```

**8. Drop the EMPLOYEES2 table.**
```sql
DROP TABLE employees2;
```

**9. Add a table-level PRIMARY KEY constraint to the EMP table on the ID column. The constraint should be named at creation. Name the constraint `my_emp_id_pk`.**
```sql
ALTER TABLE emp
ADD CONSTRAINT my_emp_id_pk PRIMARY KEY (id);
```

**10. Add a foreign key reference on the EMP table on the DEPT_ID column that ensures the employee is not assigned to a nonexistent department. Name the constraint `my_emp_dept_id_fk`.**
```sql
-- Assuming you are referencing the original DEPARTMENTS table:
ALTER TABLE emp
ADD CONSTRAINT my_emp_dept_id_fk FOREIGN KEY (dept_id) REFERENCES departments(department_id);

-- Note: If you want to reference the newly created 'dept' table instead, 
-- 'dept' must have a PRIMARY KEY added to it first:
-- ALTER TABLE dept ADD CONSTRAINT my_dept_id_pk PRIMARY KEY (id);
-- ALTER TABLE emp ADD CONSTRAINT my_emp_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept(id);
```
