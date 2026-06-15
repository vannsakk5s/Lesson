# Practice 7: Overview

In this practice, you write complex queries using nested SELECT statements.

For practice questions, you may want to create the inner query first. Make sure that it runs and produces the data that you anticipate before you code the outer query.

### 1. Employees in the same department
**Question:** The HR department needs a query that prompts the user for an employee last name. The query then displays the last name and hire date of any employee in the same department as the employee whose name they supply (excluding that employee). For example, if the user enters Zlotkey, find all employees who work with Zlotkey (excluding Zlotkey).

```sql
SELECT last_name, hire_date
FROM employees
WHERE department_id = (SELECT department_id
                       FROM employees
                       WHERE last_name = '&enter_last_name')
  AND last_name <> '&enter_last_name';
```

### 2. Employees earning more than average
**Question:** Create a report that displays the employee number, last name, and salary of all employees who earn more than the average salary. Sort the results in order of ascending salary.

```sql
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) 
                FROM employees)
ORDER BY salary ASC;
```

### 3. Departments with an employee whose name contains 'u'
**Question:** Write a query that displays the employee number and last name of all employees who work in a department with any employee whose last name contains the letter "u." Save your SQL statement as `lab_07_03.sql`. Run your query.

```sql
-- Save this to lab_07_03.sql
SELECT employee_id, last_name
FROM employees
WHERE department_id IN (SELECT DISTINCT department_id
                        FROM employees
                        WHERE LOWER(last_name) LIKE '%u%');
```

### 4. Employees by location ID
**Question:** The HR department needs a report that displays the last name, department number, and job ID of all employees whose department location ID is 1700.

Modify the query so that the user is prompted for a location ID. Save this to a file named `lab_07_04.sql`.

```sql
-- Save this to lab_07_04.sql
SELECT last_name, department_id, job_id
FROM employees
WHERE department_id IN (SELECT department_id
                        FROM departments
                        WHERE location_id = &enter_location_id);
```

### 5. Employees reporting to King
**Question:** Create a report for HR that displays the last name and salary of every employee who reports to King.

```sql
SELECT last_name, salary
FROM employees
WHERE manager_id IN (SELECT employee_id
                     FROM employees
                     WHERE last_name = 'King');
```

### 6. Employees in Executive department
**Question:** Create a report for HR that displays the department number, last name, and job ID for every employee in the Executive department.

```sql
SELECT department_id, last_name, job_id
FROM employees
WHERE department_id = (SELECT department_id
                       FROM departments
                       WHERE department_name = 'Executive');
```

### 7. Multiple Subqueries
**Question:** Modify the query in `lab_07_03.sql` to display the employee number, last name, and salary of all employees who earn more than the average salary, and who work in a department with any employee whose last name contains a "u." Resave `lab_07_03.sql` as `lab_07_07.sql`. Run the statement in `lab_07_07.sql`.

```sql
-- Save this to lab_07_07.sql
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) 
                FROM employees)
  AND department_id IN (SELECT DISTINCT department_id
                        FROM employees
                        WHERE LOWER(last_name) LIKE '%u%');
```
