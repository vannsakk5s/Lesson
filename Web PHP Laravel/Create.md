# 🚀 របៀបបង្កើត Laravel 13 Project (Laravel 13 Setup Guide)

---

## 📌 Step 1 — Check PHP & Composer Environment

បើក **Terminal** ក្នុង VS Code ហើយត្រួតពិនិត្យកំណែប្រែ (Version) នៃ **PHP** និង **Composer**:

### 1.1 Check PHP Version
```bash
php -v
```
> **ចំណាំ:** Laravel 13 ទាមទារ Support ជាមួយ **PHP 8.3–8.5**។

**លទ្ធផល (Actual Output):**
```text
PHP 8.3.31 (cli) (built: May  5 2026 17:23:04) (NTS Visual C++ 2019 x64)
Copyright (c) The PHP Group
Zend Engine v4.3.31, Copyright (c) Zend Technologies
```

---

### 1.2 Check Composer Version
```bash
composer -V
```

**លទ្ធផល (Actual Output):**
```text
Composer version 2.10.2 2026-07-01 11:24:45
```

---

## 📌 Step 2 — Create Laravel 13 Project

**Assignment Project Name:** `SOVANARASV7`

### 2.1 បង្កើត Project ថ្មី (Create Project)
រត់ Command ខាងក្រោមដើម្បីបង្កើត Laravel 13 Project:

```bash
composer create-project laravel/laravel:^13.0 SOVANARASV7
```

---

### 2.2 ចូលទៅកាន់ Project Folder
```bash
cd SOVANARASV7
```

---

### 2.3 ត្រួតពិនិត្យ Laravel Version (Check Laravel Version)
```bash
php artisan --version
```

**លទ្ធផលដែលគួរបង្ហាញ (Expected Output):**
```text
Laravel Framework 13.x.x
```
