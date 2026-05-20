# ផ្នែកទី ១: សំណួរទ្រឹស្តី (Questions 1 - 5)

១. តើត្រូវផ្តល់សិទ្ធិ (privilege) អ្វីខ្លះទៅកាន់ user ដើម្បីអាច log on ចូលទៅកាន់ Oracle server បាន? ហើយវាជាប្រភេទ system privilege ឬ object privilege?

**ចម្លើយ:** ត្រូវផ្តល់សិទ្ធិ `CREATE SESSION`។ វាជាប្រភេទ System privilege (ព្រោះវាជាសិទ្ធិទូទៅក្នុងការភ្ជាប់ទៅកាន់ database មិនមែនលើ object ជាក់លាក់ណាមួយឡើយ)។

២. តើត្រូវផ្តល់សិទ្ធិអ្វីខ្លះទៅកាន់ user ដើម្បីអាចបង្កើត tables បាន?

**ចម្លើយ:** ត្រូវផ្តល់សិទ្ធិ `CREATE TABLE` (និងជាទូទៅត្រូវការផ្តល់ទំហំផ្ទុកទិន្នន័យ `UNLIMITED TABLESPACE` ឬ quota លើ tablespace ផងដែរ ដើម្បីអាចបង្កើតបានជោគជ័យ)។

៣. ប្រសិនបើបងបង្កើត table មួយ តើនរណាខ្លះដែលអាចបោះសិទ្ធិ (pass along privileges) នៅលើ table របស់បងទៅឱ្យ user ផ្សេងទៀតបាន?

**ចម្លើយ:** មានតែ រូបបងផ្ទាល់ (ម្ចាស់ table / Owner) និង DBA (Database Administrator) ប៉ុណ្ណោះ។ (លើកលែងតែបងបានផ្តល់សិទ្ធិទៅឱ្យនរណាម្នាក់ផ្សេងទៀតដោយភ្ជាប់ជាមួយឃ្លា `WITH GRANT OPTION`)។

៤. ក្នុងនាមជា DBA ប្រសិនបើបងចង់បង្កើត user ច្រើននាក់ដែលមានសិទ្ធិ (system privileges) ដូចៗគ្នា តើបងគួរប្រើប្រាស់អ្វីដើម្បីជួយឱ្យការងារនេះកាន់តែងាយស្រួល?

**ចម្លើយ:** គួរប្រើប្រាស់ Role (បង្កើត Role មួយ រួចផ្តល់សិទ្ធិទៅឱ្យ Role នោះ បន្ទាប់មកយក Role នោះទៅផ្តល់ឱ្យ user ទាំងឡាយជាការស្រេច)។

៥. តើត្រូវប្រើប្រាស់ Command អ្វីដើម្បីផ្លាស់ប្តូរលេខសម្ងាត់ (password) ផ្ទាល់ខ្លួន?

**ចម្លើយ:** ប្រើ command: `ALTER USER user_name IDENTIFIED BY new_password;` (ឬប្រើ command ខ្លី `PASSWORD` នៅក្នុង SQL*Plus)។

# ផ្នែកទី ២: កូដ SQL សម្រាប់អនុវត្ត (Questions 6 - 10)

*ចំណាំ: ដើម្បីដំណើរការកូដខាងក្រោមនេះបាន បងត្រូវ Log in ចូលប្រើប្រាស់ account ជា DBA (sys ឬ system) ឬ account ORA1 ដូចដែលរូបភាពទី២ របស់បងបានបង្ហាញ។*

### 6. Create a user, grant SELECT and INSERT, then revoke INSERT

```sql
-- កូដបង្កើត user ថ្មី (ឧទាហរណ៍ឈ្មោះ user1)
CREATE USER user1 IDENTIFIED BY password123;
GRANT CREATE SESSION TO user1; -- ផ្តល់សិទ្ធិឱ្យចូលប្រព័ន្ធបាន

-- ផ្តល់សិទ្ធិ SELECT និង INSERT លើ table Employees
GRANT SELECT, INSERT ON Employees TO user1;

-- ដកសិទ្ធិ INSERT ចេញវិញ
REVOKE INSERT ON Employees FROM user1;
```

### 7. Create a new table, a new role, grant SELECT to role, and grant role to user1

```sql
-- បង្កើត table ថ្មីមួយ
CREATE TABLE new_test_table (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(50)
);

-- បង្កើត Role ថ្មីមួយ (ឧទាហរណ៍ឈ្មោះ test_role)
CREATE ROLE test_role;

-- ផ្តល់សិទ្ធិ SELECT លើ table ថ្មីទៅឱ្យ Role
GRANT SELECT ON new_test_table TO test_role;

-- ផ្តល់ Role នោះទៅឱ្យ user1 (ដែលបានបង្កើតក្នុងជំហានទី ៦)
GRANT test_role TO user1;
```

### 8. Create a new user, a database role, grant SELECT on Departments to role, and grant role to user

```sql
-- បង្កើត user ថ្មីមួយទៀត (ឧទាហរណ៍ឈ្មោះ user2)
CREATE USER user2 IDENTIFIED BY password123;
GRANT CREATE SESSION TO user2;

-- បង្កើត Role ថ្មីមួយទៀត (ឧទាហរណ៍ឈ្មោះ dept_role)
CREATE ROLE dept_role;

-- ផ្តល់សិទ្ធិ SELECT លើ table Departments ទៅឱ្យ Role
GRANT SELECT ON Departments TO dept_role;

-- ផ្តល់ Role នោះទៅឱ្យ user2
GRANT dept_role TO user2;
```

### 9. Create a view to restrict access to sensitive columns and grant SELECT to user2
(ឧបមាថា table Employees មាន column សំខាន់ៗដូចជា id, name, និង salary។ យើងបង្កើត View បង្ហាញតែ id និង name ដើម្បីលាក់ salary)

```sql
-- បង្កើត View ឈ្មោះ emp_public_view
CREATE VIEW emp_public_view AS 
SELECT employee_id, first_name, last_name 
FROM Employees;

-- ផ្តល់សិទ្ធិ SELECT លើ View នេះទៅឱ្យ user2 (ដែលបង្កើតក្នុងជំហានទី ៨)
GRANT SELECT ON emp_public_view TO user2;
```

### 10. Create a user, grant and deny DELETE and UPDATE permission on Ora1.Departments
(ចំណាំ៖ នៅក្នុង Oracle មិនមាន command "DENY" ផ្ទាល់ដូច SQL Server ទេ ការបដិសេធសិទ្ធិគឺប្រើប្រាស់ការមិនផ្តល់ឱ្យ ឬប្រើប្រាស់ REVOKE ប្រសិនបើធ្លាប់ផ្តល់ឱ្យ)

```sql
-- បង្កើត user ថ្មី (ឧទាហរណ៍ឈ្មោះ user3)
CREATE USER user3 IDENTIFIED BY password123;
GRANT CREATE SESSION TO user3;

-- ផ្តល់សិទ្ធិ DELETE និង UPDATE លើ table Ora1.Departments
GRANT DELETE, UPDATE ON Ora1.Departments TO user3;

-- ដកសិទ្ធិ (Deny/Revoke) ទាំងពីរនោះចេញវិញ
REVOKE DELETE, UPDATE ON Ora1.Departments FROM user3;
```
