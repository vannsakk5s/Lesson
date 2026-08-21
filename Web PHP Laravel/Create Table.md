# 🗄️ Laravel Migrations: Create Tables (Students & Products)

## 📌 Step 1 — Student Migration

បើអ្នកបាន run មុននេះ៖

```bash
php artisan make:model Student -m
```

ចូលទៅ៖ `database/migrations/xxxx_xx_xx_xxxxxx_create_students_table.php`

កែជា៖

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('students', function (Blueprint $table) {

            // Primary Key Auto Number
            $table->id('studentid');

            // Max 255 characters
            $table->string('studentname', 255);

            // Male or Female
            $table->enum('gender', ['Male', 'Female']);

            // Integer
            $table->integer('age');

            // Timestamp
            $table->timestamp('dob');

            // Max 255 characters
            $table->string('subject', 255);

            // Decimal (20,2)
            $table->decimal('payment', 20, 2);

            // Allow NULL
            $table->text('description')->nullable();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('students');
    }
};
```

### 📋 Structure នឹងបាន៖

```text
students
├── studentid      BIGINT PRIMARY KEY AUTO_INCREMENT
├── studentname    VARCHAR(255)
├── gender         ENUM('Male','Female')
├── age            INT
├── dob            TIMESTAMP
├── subject        VARCHAR(255)
├── payment        DECIMAL(20,2)
└── description    TEXT NULL
```

---

## 📌 Step 2 — Product Migration

ចូលទៅ៖ `database/migrations/xxxx_xx_xx_xxxxxx_create_products_table.php`

ដាក់៖

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('products', function (Blueprint $table) {

            // Primary Key Auto Number
            $table->id('productid');

            // Product Name + Unique
            $table->string('productname', 255)->unique();

            // Decimal (20,2)
            $table->decimal('price', 20, 2);

            // Integer
            $table->integer('quantity');

            // amount = price * quantity
            $table->decimal('amount', 20, 2)
                  ->storedAs('price * quantity');

            // Allow NULL
            $table->text('description')->nullable();

            // created_at + updated_at
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('products');
    }
};
```

### 💡 ចំណុចសំខាន់៖

> [!NOTE]
> **amount = price × quantity**
> 
> ដូច្នេះយើងប្រើ៖
> ```php
> $table->decimal('amount', 20, 2)
>       ->storedAs('price * quantity');
> ```
> **ឧទាហរណ៍៖**
> - `price = 100`
> - `quantity = 3`
> - `amount = 100 × 3 = 300`
>
> ➡️ **MySQL គណនាឲ្យដោយស្វ័យប្រវត្តិ**។

---

## 📌 Step 3 — Run Migration

បើ migration ទាំងនេះ មិនទាន់បាន run៖

```bash
php artisan migrate
```

បើ success៖

```text
INFO  Running migrations.

create_students_table ........ DONE
create_products_table ........ DONE
```

បន្ទាប់ទៅ **dbForge** → **Refresh database** → **Tables**។

អ្នកនឹងឃើញ៖
- `students`
- `products`

> [!WARNING]
> **បើអ្នកបាន migrate table ទាំងនេះរួចមុនកែ Fields**
> 
> បើ project នេះគ្រាន់តែជា demo ហើយគ្មាន data សំខាន់ អាចប្រើ៖
> ```bash
> php artisan migrate:fresh
> ```
> ⚠️ **Command នេះលុប tables ទាំងអស់ក្នុង database ហើយ create ឡើងវិញ។**

---

## 📌 Step 4 — Insert 2 Rows into students

នៅ **dbForge** → **New Query**។

Run៖

```sql
INSERT INTO students
(
    studentname,
    gender,
    age,
    dob,
    subject,
    payment,
    description
)
VALUES
(
    'Dara',
    'Male',
    20,
    '2006-01-15 00:00:00',
    'Web Project',
    250.00,
    'Student year 1'
),
(
    'Sokha',
    'Female',
    19,
    '2007-05-20 00:00:00',
    'Web Project',
    300.00,
    'Student year 1'
);
```

> [!TIP]
> យើង **មិនដាក់** `studentid` ព្រោះវា **Auto Increment**។

### Check Table:

```sql
SELECT * FROM students;
```

**លទ្ធផលប្រហែល៖**

| studentid | studentname | gender | age | dob | subject | payment | description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Dara | Male | 20 | 2006-01-15 00:00:00 | Web Project | 250.00 | Student year 1 |
| 2 | Sokha | Female | 19 | 2007-05-20 00:00:00 | Web Project | 300.00 | Student year 1 |

---

## 📌 Step 5 — Insert 2 Rows into products

Run៖

```sql
INSERT INTO products
(
    productname,
    price,
    quantity,
    description,
    created_at,
    updated_at
)
VALUES
(
    'iPhone 16 Pro',
    999.00,
    2,
    'Apple Smartphone',
    NOW(),
    NOW()
),
(
    'MacBook Air M4',
    1199.00,
    3,
    'Apple Laptop',
    NOW(),
    NOW()
);
```

> [!TIP]
> - យើង **មិនដាក់** `productid` ព្រោះវា **Auto Increment**។
> - យើងក៏ **មិនដាក់** `amount` ព្រោះ MySQL គណនាស្វ័យប្រវត្តិ៖ `amount = price × quantity`

### Check Table:

```sql
SELECT * FROM products;
```

**នឹងបានប្រហែល៖**

| productid | productname | price | quantity | amount | description | created_at | updated_at |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | iPhone 16 Pro | 999.00 | 2 | 1998.00 | Apple Smartphone | *Timestamp* | *Timestamp* |
| 2 | MacBook Air M4 | 1199.00 | 3 | 3597.00 | Apple Laptop | *Timestamp* | *Timestamp* |

**គណនា៖**
- **iPhone:** `999 × 2 = 1998`
- **MacBook:** `1199 × 3 = 3597`

---

## 📌 Step 6 — Check in dbForge

Run:

```sql
SELECT * FROM students;
```

និង៖

```sql
SELECT * FROM products;
```

---

## 🎯 Summary / Assignment Checklist (Part VI)

```text
VI.
├── 1. Migration students ✅
│   ├── studentid
│   ├── studentname
│   ├── gender
│   ├── age
│   ├── dob
│   ├── subject
│   ├── payment
│   └── description
│
├── 2. Migration products ✅
│   ├── productid
│   ├── productname unique
│   ├── price
│   ├── quantity
│   ├── amount = price × quantity
│   ├── description
│   └── timestamps
│
└── 3. INSERT 2 rows ✅
    ├── students
    └── products
```
