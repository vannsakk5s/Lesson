-- ==========================================
-- Oracle Lab: Practice 11 Solutions
-- ==========================================

-- ------------------------------------------
-- PART 1: Creating, Using, and Removing Views
-- ------------------------------------------

-- 1. Create EMPLOYEES_VU view
CREATE OR REPLACE VIEW employees_vu AS
SELECT employee_id, last_name AS employee, department_id
FROM employees;

-- 2. Display the contents of EMPLOYEES_VU
SELECT * FROM employees_vu;

-- 3. Display employee names and department numbers from the view
SELECT employee, department_id 
FROM employees_vu;

-- 4. Create DEPT50 view with security constraint (WITH CHECK OPTION)
CREATE OR REPLACE VIEW dept50 (empno, employee, deptno) AS
SELECT employee_id, last_name, department_id
FROM employees
WHERE department_id = 50
WITH CHECK OPTION CONSTRAINT dept50_check;

-- 5. Display the structure and contents of DEPT50
DESCRIBE dept50;
SELECT * FROM dept50;

-- 6. Test the view (Attempt to reassign Matos to department 80)
-- NOTE: This will throw an error (ORA-01402) because of the WITH CHECK OPTION
-- UPDATE dept50
-- SET deptno = 80
-- WHERE employee = 'Matos';


-- ------------------------------------------
-- PART 2: Sequences, Indexes, and Synonyms
-- ------------------------------------------

-- 7. Create DEPT_ID_SEQ sequence
CREATE SEQUENCE dept_id_seq
START WITH 200
INCREMENT BY 10
MAXVALUE 1000;

-- 8. Insert two rows into the DEPT table using the sequence
INSERT INTO dept (id, name)
VALUES (dept_id_seq.NEXTVAL, 'Education');

INSERT INTO dept (id, name)
VALUES (dept_id_seq.NEXTVAL, 'Administration');

-- Confirm additions
SELECT * FROM dept;

-- 9. Create a nonunique index on the NAME column in the DEPT table
CREATE INDEX dept_name_idx ON dept (name);

-- 10. Create a synonym for the EMPLOYEES table
CREATE SYNONYM emp FOR employees;

-- ==========================================
-- End of Script
-- ==========================================