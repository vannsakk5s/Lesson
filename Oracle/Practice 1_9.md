# Practice 9: Data Manipulation Language (DML) and Transaction Control

The HR department wants you to create SQL statements to insert, update, and delete employee data. As a prototype, you use the `MY_EMPLOYEE` table before giving the statements to the HR department.

### Insert data into the MY_EMPLOYEE table.

**1. Run the statement to build the MY_EMPLOYEE table used in this practice.**
```sql
CREATE TABLE my_employee (
  id NUMBER(4) NOT NULL,
  last_name VARCHAR2(25),
  first_name VARCHAR2(25),
  userid VARCHAR2(8),
  salary NUMBER(9, 2)
);
```

**2. Describe the structure of the MY_EMPLOYEE table to identify the column names.**
```sql
DESCRIBE my_employee;
```

**3. Create an INSERT statement to add the first row of data to the MY_EMPLOYEE table from the following sample data. Do not list the columns in the INSERT clause.**
*(Row 1: 1, Patel, Ralph, rpatel, 895)*
```sql
INSERT INTO my_employee 
VALUES (1, 'Patel', 'Ralph', 'rpatel', 895);
```

**4. Populate the MY_EMPLOYEE table with the second row of the sample data. This time, list the columns explicitly in the INSERT clause.**
*(Row 2: 2, Dancs, Betty, bdancs, 860)*
```sql
INSERT INTO my_employee (id, last_name, first_name, userid, salary)
VALUES (2, 'Dancs', 'Betty', 'bdancs', 860);
```

**5. Confirm your addition to the table.**
```sql
SELECT * FROM my_employee;
```

**6. Write an INSERT statement in a dynamic reusable script file to load the remaining rows into the MY_EMPLOYEE table. The script should prompt for all the columns (ID, LAST_NAME, FIRST_NAME, USERID, and SALARY). Save this script to a `lab_09_06.sql` file.**
```sql
-- Save this to lab_09_06.sql
INSERT INTO my_employee (id, last_name, first_name, userid, salary)
VALUES (&enter_id, '&enter_last_name', '&enter_first_name', '&enter_userid', &enter_salary);
```

**7. Populate the table with the next two rows of the sample data listed in step 3 by running the INSERT statement in the script that you created.**
*(Row 3: 3, Popp, Louis, lpopp, 980)*  
*(Row 4: 4, Raphealy, Den, drapheal, 850)*
```sql
-- Run lab_09_06.sql and enter values when prompted:
-- ID: 3, Last Name: Popp, First Name: Louis, Userid: lpopp, Salary: 980

-- Run lab_09_06.sql again and enter values when prompted:
-- ID: 4, Last Name: Raphealy, First Name: Den, Userid: drapheal, Salary: 850
```

**8. Confirm your additions to the table.**
```sql
SELECT * FROM my_employee;
```

**9. Make the data additions permanent.**
```sql
COMMIT;
```

### Update and delete data in the MY_EMPLOYEE table.

**10. Change the last name of employee 3 to Drexler.**
```sql
UPDATE my_employee
SET last_name = 'Drexler'
WHERE id = 3;
```

**11. Change the salary to $1,000 for all employees who have a salary less than $900.**
```sql
UPDATE my_employee
SET salary = 1000
WHERE salary < 900;
```

**12. Verify your changes to the table.**
```sql
SELECT * FROM my_employee;
```

**13. Delete Betty Dancs from the MY_EMPLOYEE table.**
```sql
DELETE FROM my_employee
WHERE first_name = 'Betty' AND last_name = 'Dancs';
```

**14. Confirm your changes to the table.**
```sql
SELECT * FROM my_employee;
```

**15. Commit all pending changes.**
```sql
COMMIT;
```

### Control data transaction to the MY_EMPLOYEE table.

**16. Populate the table with the last row of the sample data listed in step 3 by using the statements in the script that you created in step 6. Run the statements in the script.**
*(Row 6: 6, Mourgos, Kevin, kmourgos, 995)*
```sql
-- Run lab_09_06.sql and enter the following values:
-- ID: 6, Last Name: Mourgos, First Name: Kevin, Userid: kmourgos, Salary: 995
```

**17. Confirm your addition to the table.**
```sql
SELECT * FROM my_employee;
```

**18. Mark an intermediate point in the processing of the transaction.**
```sql
SAVEPOINT step_18;
```

**19. Delete all the rows from the MY_EMPLOYEE table.**
```sql
DELETE FROM my_employee;
```

**20. Confirm that the table is empty.**
```sql
SELECT * FROM my_employee;
```

**21. Discard the most recent DELETE operation without discarding the earlier INSERT operation.**
```sql
ROLLBACK TO step_18;
```

**22. Confirm that the new row is still intact.**
```sql
SELECT * FROM my_employee;
```

**23. Make the data addition permanent.**
```sql
COMMIT;
```

**24. Modify the `lab_09_06.sql` script such that the USERID is generated automatically by concatenating the first letter of the first name and the first seven characters of the last name. The generated USERID must be in lowercase. Hence, the script should not prompt for the USERID. Save this script to a file named `lab_09_24.sql`.**
```sql
-- Save this to lab_09_24.sql
INSERT INTO my_employee (id, last_name, first_name, userid, salary)
VALUES (&enter_id, '&enter_last_name', '&enter_first_name', 
        LOWER(SUBSTR('&enter_first_name', 1, 1) || SUBSTR('&enter_last_name', 1, 7)), 
        &enter_salary);
```

**25. Run the script `lab_09_24.sql` to insert the following record:**
*(Row 5: 5, Lorentz, Diana, dlorentz, 920)*
```sql
-- Run lab_09_24.sql and enter the values when prompted:
-- ID: 5, Last Name: Lorentz, First Name: Diana, Salary: 920
```

**26. Confirm that the new row was added with correct USERID.**
```sql
SELECT * FROM my_employee
WHERE id = 5;
```
