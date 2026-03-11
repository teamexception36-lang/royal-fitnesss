# Royal Fitness - Implementation Checklist & Quick Start

## ✅ Phase 1: Database Migration (DONE)

- [x] Create trainers table migration
- [x] Create equipment table migration
- [x] Create payments table migration
- [x] Create expenses table migration
- [x] Create messages table migration
- [x] Create trainer_performance table migration
- [x] Create trainer_programs table migration
- [x] Update users table migration

## ✅ Phase 2: Models & Relationships (DONE)

- [x] User Model with relationships
- [x] Trainer Model with relationships
- [x] Equipment Model with scopes
- [x] Payment Model with scopes
- [x] Expense Model with scopes
- [x] Message Model with scopes
- [x] TrainerPerformance Model
- [x] TrainerProgram Model

## ✅ Phase 3: Controllers (DONE)

- [x] AuthController (login, register, profile)
- [x] TrainerController (CRUD)
- [x] EquipmentController (CRUD)
- [x] PaymentController (process, history, statistics)
- [x] ExpenseController (CRUD, summary)
- [x] MessageController (contact form, admin)
- [x] DashboardController (role-based views)

## ✅ Phase 4: Services & Repositories (DONE)

- [x] PaymentService
- [x] ReportService
- [x] PaymentRepository
- [x] TrainerRepository
- [x] EquipmentRepository
- [x] ExpenseRepository

## ✅ Phase 5: Routes & Middleware (DONE)

- [x] Create routes in web.php
- [x] Create IsAdmin middleware
- [x] Create IsTrainer middleware
- [x] Register middleware in bootstrap/app.php

---

## 🔧 Phase 6: Frontend Views (TODO)

### Views to Create:

#### Authentication Views
- [ ] `resources/views/auth/login.blade.php`
- [ ] `resources/views/auth/register.blade.php`
- [ ] `resources/views/auth/profile.blade.php`

#### Dashboard Views
- [ ] `resources/views/dashboard/admin.blade.php`
- [ ] `resources/views/dashboard/trainer.blade.php`
- [ ] `resources/views/dashboard/member.blade.php`

#### Trainer Management
- [ ] `resources/views/trainers/index.blade.php`
- [ ] `resources/views/trainers/create.blade.php`
- [ ] `resources/views/trainers/edit.blade.php`
- [ ] `resources/views/trainers/show.blade.php`

#### Equipment Management
- [ ] `resources/views/equipment/index.blade.php`
- [ ] `resources/views/equipment/create.blade.php`
- [ ] `resources/views/equipment/edit.blade.php`
- [ ] `resources/views/equipment/show.blade.php`

#### Payment Management
- [ ] `resources/views/payments/index.blade.php`
- [ ] `resources/views/payments/create.blade.php`
- [ ] `resources/views/payments/user-history.blade.php`
- [ ] `resources/views/payments/statistics.blade.php`

#### Expense Management
- [ ] `resources/views/expenses/index.blade.php`
- [ ] `resources/views/expenses/create.blade.php`
- [ ] `resources/views/expenses/edit.blade.php`
- [ ] `resources/views/expenses/summary.blade.php`

#### Messaging
- [ ] `resources/views/messages/index.blade.php`
- [ ] `resources/views/messages/create.blade.php`
- [ ] `resources/views/messages/show.blade.php`

#### Layouts
- [ ] `resources/views/layouts/app.blade.php` (main layout)
- [ ] `resources/views/layouts/dashboard.blade.php` (dashboard layout)

---

## 🎯 Quick Start Commands

### 1. Setup Database
```bash
# Copy .env.example to .env
cp .env.example .env

# Generate app key
php artisan key:generate

# Run migrations
php artisan migrate

# (Optional) Seed sample data
php artisan db:seed
```

### 2. Create Sample Seeders
```bash
php artisan make:seeder UserSeeder
php artisan make:seeder TrainerSeeder
php artisan make:seeder EquipmentSeeder
php artisan make:seeder PaymentSeeder
```

### 3. Start Development Server
```bash
php artisan serve
```

Visit: `http://localhost:8000`

---

## 📝 Database Schema Reference

### Users Table
```
id (PK)
name
username
email
phone
password
role: enum(admin, trainer, member, owner, manager)
active_plan
weight
height
bmi_score
profile_image
status: enum(active, inactive, suspended)
email_verified_at
remember_token
created_at
updated_at
deleted_at (soft delete)
```

### Trainers Table
```
id (PK)
name
email
phone
specialty
salary
bio
qualification
certification
joining_date
status: enum(active, inactive, on_leave)
created_at
updated_at
deleted_at (soft delete)
```

### Equipment Table
```
id (PK)
name
description
condition_status: enum(Good, Fair, Poor, Maintenance Required)
purchase_date
last_maintenance
cost
current_value
location
quantity
serial_number
created_at
updated_at
deleted_at (soft delete)
```

### Payments Table
```
id (PK)
user_id (FK)
plan_name
amount
transaction_id
status: enum(Pending, Verified, Failed, Refunded)
payment_date
payment_method
notes
created_at
updated_at
deleted_at (soft delete)
```

### Expenses Table
```
id (PK)
category: enum(Rent, Electricity, Salary, Maintenance, Supplies, Other)
amount
description
expense_date
recorded_by (FK to users)
status: enum(Pending, Approved, Rejected)
created_at
updated_at
deleted_at (soft delete)
```

### Messages Table
```
id (PK)
name
email
phone
message
status: enum(New, Read, Replied)
created_at
updated_at
deleted_at (soft delete)
```

### TrainerPerformance Table
```
id (PK)
trainer_id (FK)
sessions_conducted
clients_served
rating (out of 5.00)
reviews_count
avg_client_satisfaction (out of 100)
achievements
certifications
performance_date
created_at
updated_at
deleted_at (soft delete)
```

### TrainerPrograms Table
```
id (PK)
user_id (FK)
trainer_id (FK)
program_name
workout_details
diet_plan
assigned_date
status: enum(active, completed, paused)
created_at
updated_at
deleted_at (soft delete)
```

---

## 🔐 User Roles & Permissions

### Admin / Owner / Manager
- ✅ View all users
- ✅ Manage trainers (CRUD)
- ✅ Manage equipment (CRUD)
- ✅ View all payments & revenue
- ✅ Add/approve expenses
- ✅ View reports & analytics
- ✅ Manage messages

### Trainer
- ✅ View assigned clients
- ✅ Create/manage programs
- ✅ View own performance
- ❌ View financial data
- ❌ Manage other trainers

### Member
- ✅ Update own profile
- ✅ View payment history
- ✅ View assigned programs
- ✅ Send messages
- ❌ View other users
- ❌ Access admin panels

---

## 📚 Example: Using Services in Controllers

### Before (Without Service)
```php
public function processPayment(Request $request) {
    $payment = Payment::create([
        'user_id' => auth()->id(),
        'plan_name' => $request->plan,
        'amount' => $request->amount,
        // ... more fields
    ]);
    
    auth()->user()->update(['active_plan' => $request->plan]);
    return redirect()->back()->with('success', 'Payment successful!');
}
```

### After (With Service)
```php
public function __construct(private PaymentService $paymentService) {}

public function processPayment(Request $request) {
    $this->paymentService->processPayment(
        auth()->id(),
        $request->plan,
        $request->amount,
        $request->txn_id,
        $request->method
    );
    
    return redirect()->back()->with('success', 'Payment successful!');
}
```

Benefits:
- Reusable logic
- Easier testing
- Cleaner controllers
- Single responsibility

---

## 🚀 Deploy to Production Checklist

Before deploying to production:

- [ ] Set `APP_ENV=production`
- [ ] Set `APP_DEBUG=false`
- [ ] Generate new `APP_KEY`
- [ ] Set up proper database credentials
- [ ] Enable HTTPS/SSL
- [ ] Configure email service
- [ ] Set up file upload directories
- [ ] Run `composer install --no-dev`
- [ ] Run migrations on production database
- [ ] Create `.htaccess` for mod_rewrite (if using Apache)
- [ ] Set proper file permissions (755 for dirs, 644 for files)
- [ ] Configure PHP memory limit (`memory_limit = 256M`)
- [ ] Enable OPcache for performance
- [ ] Set up log rotation
- [ ] Configure automated backups

---

## 🐛 Troubleshooting

### "Class not found" errors
Solution: Run `composer dump-autoload`

### Migration errors
Solution: Check for syntax errors, run `php artisan migrate:rollback`

### 403 Forbidden errors
Solution: Check middleware, verify user role with `auth()->user()->role`

### CSRF token mismatch
Solution: Add `@csrf` in all POST forms

### Eloquent not finding relationships
Solution: Use eager loading: `User::with('payments')->find(1)`

---

## 📞 Support Resources

- Laravel docs: https://laravel.com/docs
- Eloquent Guide: https://laravel.com/docs/eloquent
- Blade Templating: https://laravel.com/docs/blade
- Tutorial videos: Search "Laravel 12 tutorial" on YouTube

---

## 🎓 Learning Path for Student Developers

1. **Understand MVC Pattern**
   - Models: Database representation
   - Views: User interface
   - Controllers: Business logic

2. **Master Eloquent ORM**
   - Learn Model relationships
   - Query scopes
   - Mass assignment

3. **Practice Routing**
   - RESTful routing
   - Route parameters
   - Named routes

4. **Implement Validation**
   - Form validation
   - Custom rules
   - Error handling

5. **Learn Middleware**
   - Authentication
   - Authorization
   - Custom middleware

6. **Advanced Topics**
   - API development
   - Testing
   - Caching
   - Job queues

---

Generated: March 12, 2026
Version: 1.0
