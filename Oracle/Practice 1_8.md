# Practice 8: Overview

In this practice, you write queries using the set operators.

### 1. Departments without ST_CLERK
**Question:** The HR department needs a list of department IDs for departments that do not contain the job ID `ST_CLERK`. Use the set operators to create this report.

```sql
SELECT department_id
FROM departments
MINUS
SELECT department_id
FROM employees
WHERE job_id = 'ST_CLERK';
```

### 2. Countries with no departments
**Question:** The HR department needs a list of countries that have no departments located in them. Display the country ID and the name of the countries. Use the set operators to create this report.

```sql
SELECT country_id, country_name
FROM countries
MINUS
SELECT country_id, country_name
FROM countries
JOIN locations USING (country_id)
JOIN departments USING (location_id);
```

### 3. Jobs for specific departments in order
**Question:** Produce a list of jobs for departments 10, 50, and 20, in that order. Display the job ID and department ID by using the set operators.

```sql
SELECT job_id, department_id, 1 AS dummy_order
FROM employees
WHERE department_id = 10
UNION
SELECT job_id, department_id, 2 AS dummy_order
FROM employees
WHERE department_id = 50
UNION
SELECT job_id, department_id, 3 AS dummy_order
FROM employees
WHERE department_id = 20
ORDER BY dummy_order;
```
*(Note: An extra column `dummy_order` is added to enforce the exact sorting order as requested, since SET operators don't natively preserve order.)*

### 4. Employees returning to their original jobs
**Question:** Create a report that lists the employee IDs and job IDs of those employees who currently have a job title that is the same as their job title when they were initially hired by the company (that is, they changed jobs but have now gone back to doing their original job).

```sql
SELECT employee_id, job_id
FROM employees
INTERSECT
SELECT employee_id, job_id
FROM job_history;
```

### 5. Compound query for Employees and Departments
**Question:** The HR department needs a report with the following specifications:
- Last name and department ID of all employees from the EMPLOYEES table, regardless of whether or not they belong to a department
- Department ID and department name of all departments from the DEPARTMENTS table, regardless of whether or not they have employees working in them

Write a compound query to accomplish this.

```sql
SELECT last_name, department_id, TO_CHAR(NULL) AS department_name
FROM employees
UNION
SELECT TO_CHAR(NULL), department_id, department_name
FROM departments;
```
