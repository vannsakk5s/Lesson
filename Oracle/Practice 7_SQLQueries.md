# Practice 7 SQL Queries

### ១. ស្វែងរកបុគ្គលិកដែលធ្វើការក្នុង Department ដូចគ្នា (Prompt last name)
**គោលបំណង:** បង្កើត Query ដែលទាមទារឱ្យ User បញ្ចូល Last name រួចបង្ហាញ Last name និង Hire date របស់បុគ្គលិកផ្សេងទៀតដែលស្ថិតក្នុង Department ជាមួយគ្នា (ដោយមិនបង្ហាញបុគ្គលិកដែលបញ្ចូលឈ្មោះនោះទេ) ។  
- **Inner Query:** ស្វែងរក `department_id` របស់បុគ្គលិកដែលមាន Last name ដូចដែល User បានបញ្ចូល ។  
- **Outer Query:** បង្ហាញបុគ្គលិកក្នុង `department_id` នោះ ដោយប្រើលក្ខខណ្ឌ `<>` (មិនស្មើនឹង) ដើម្បី Exclude ឈ្មោះខ្លួនឯងចេញ ។  

```sql
SELECT last_name, hire_date
FROM employees
WHERE department_id = (SELECT department_id
                       FROM employees
                       WHERE last_name = '&enter_last_name')
  AND last_name <> '&enter_last_name';
```
*ចំណាំ (Note): និមិត្តសញ្ញា `&` ប្រើសម្រាប់បង្កើត Substitution Variable (ការទាមទារឱ្យ User បញ្ចូលទិន្នន័យនៅពេល Run)។*

### ២. បុគ្គលិកដែលមានប្រាក់ខែលើសពីមធ្យមភាគ (Average Salary)
**គោលបំណង:** បង្កើត Report បង្ហាញ Employee number, Last name, និង Salary របស់បុគ្គលិកទាំងឡាយណាដែលមានប្រាក់ខែខ្ពស់ជាង Average salary ដោយតម្រៀបតាម Salary ពីតូចទៅធំ (Ascending) ។  
- **Inner Query:** គណនាប្រាក់ខែជាមធ្យម (`AVG(salary)`) របស់បុគ្គលិកទាំងអស់ ។  
- **Outer Query:** ច្រោះយកតែបុគ្គលិកណាដែលមាន salary ធំជាងលទ្ធផលមធ្យមភាគនោះ ។  

```sql
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) 
                FROM employees)
ORDER BY salary ASC;
```

### ៣. ស្វែងរកបុគ្គលិកដែលធ្វើការក្នុង Department ដែលមានសមាជិកឈ្មោះមានអក្សរ "u"
**គោលបំណង:** បង្ហាញ Employee number និង Last name របស់បុគ្គលិកទាំងអស់ ដែលធ្វើការក្នុង Department ណាដែលមានបុគ្គលិកយ៉ាងហោចណាស់ម្នាក់មាន Last name ផ្ទុកអក្សរ "u" ។ រក្សាទុកជាឯកសារឈ្មោះ `lab_07_03.sql` ។  
- **Inner Query:** ស្វែងរកលេខ `department_id` ទាំងឡាយណាដែលមានបុគ្គលិកឈ្មោះមានអក្សរ "u" (ប្រើ `LIKE '%u%'`) ។  
- **Outer Query:** ប្រើ Operator `IN` ពីព្រោះ Inner query អាចនឹងផ្ដល់លទ្ធផល `department_id` ច្រើនជាងមួយ (Multiple rows) ។  

```sql
-- Save ឯកសារនេះជា lab_07_03.sql
SELECT employee_id, last_name
FROM employees
WHERE department_id IN (SELECT DISTINCT department_id
                        FROM employees
                        WHERE LOWER(last_name) LIKE '%u%');
```

### ៤. ស្វែងរកបុគ្គលិកតាមរយៈ Location ID (Prompt location ID)
**គោលបំណង:** បង្ហាញ Last name, Department number, និង Job ID របស់បុគ្គលិកទាំងអស់ដែលបម្រើការក្នុង Location ID 1700 រួចកែប្រែវាទៅជាការផ្ដល់ Prompt ឱ្យ User បញ្ចូលលេខ ID វិញ រក្សាទុកជា `lab_07_04.sql` ។  
- **Inner Query:** ស្វែងរក `department_id` ទាំងអស់ដែលស្ថិតនៅក្នុង `location_id` ដែល User បានបញ្ចូល (ទាញចេញពី Table departments) ។  
- **Outer Query:** ទាញទិន្នន័យបុគ្គលិកដែលស្ថិតក្នុង Departments ទាំងនោះ ។  

```sql
-- Save ឯកសារនេះជា lab_07_04.sql
SELECT last_name, department_id, job_id
FROM employees
WHERE department_id IN (SELECT department_id
                        FROM departments
                        WHERE location_id = &enter_location_id);
```

### ៥. បុគ្គលិកដែលរាយការណ៍ជូន King (Reports to King)
**គោលបំណង:** បង្ហាញ Last name និង Salary របស់បុគ្គលិកគ្រប់រូបដែលធ្វើការក្រោមការគ្រប់គ្រងរបស់ Manager ឈ្មោះ "King" ។  
- **Inner Query:** ស្វែងរក `employee_id` របស់ Manager ដែលមានឈ្មោះថា 'King' ។  
- **Outer Query:** យក `manager_id` របស់បុគ្គលិកទៅផ្ទៀងផ្ទាត់ជាមួយ ID ដែលរកឃើញ ។  

```sql
SELECT last_name, salary
FROM employees
WHERE manager_id IN (SELECT employee_id
                     FROM employees
                     WHERE last_name = 'King');
```
*ហេតុអ្វីប្រើ `IN`? ពីព្រោះនៅក្នុងក្រុមហ៊ុនអាចមានអ្នកមានត្រកូល "King" ច្រើនជាងម្នាក់ (ឧទាហរណ៍៖ Steven King និង Jan King) ដូច្នេះការប្រើ `IN` ការពារមិនឱ្យមាន Error។*

### ៦. បុគ្គលិកនៅក្នុង Executive Department
**គោលបំណង:** បង្ហាញ Department number, Last name, និង Job ID សម្រាប់បុគ្គលិកទាំងអស់ដែលស្ថិតក្នុង "Executive" department ។  
- **Inner Query:** ស្វែងរក `department_id` របស់ Department ដែលមានឈ្មោះថា 'Executive' ពី Table departments ។  
- **Outer Query:** ទាញទិន្នន័យបុគ្គលិកដែលមាន `department_id` ត្រូវគ្នានឹងលទ្ធផលខាងលើ ។  

```sql
SELECT department_id, last_name, job_id
FROM employees
WHERE department_id = (SELECT department_id
                       FROM departments
                       WHERE department_name = 'Executive');
```

### ៧. ការរួមបញ្ចូលលក្ខខណ្ឌ (Multiple Subqueries)
**គោលបំណង:** កែប្រែ Query ក្នុង `lab_07_03.sql` ដើម្បីបង្ហាញបុគ្គលិកណាដែលទាំងមានប្រាក់ខែលើសពី Average salary ផង និងធ្វើការក្នុង Department ដែលមានបុគ្គលិកឈ្មោះមានអក្សរ "u" ផង ។ រក្សាទុកជា `lab_07_07.sql` ។  
Query នេះប្រើប្រាស់ Subqueries ចំនួនពីរនៅក្នុង WHERE clause ដោយភ្ជាប់គ្នាដោយ Operator `AND` ។  

```sql
-- Save ឯកសារនេះជា lab_07_07.sql
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) 
                FROM employees)
  AND department_id IN (SELECT DISTINCT department_id
                        FROM employees
                        WHERE LOWER(last_name) LIKE '%u%');
```
