# Royal Fitness - Laravel 12 Migration Guide

## Project Structure for Scalable Gym SaaS

```
royal-fitness-app/
├── app/
│   ├── Http/
│   │   ├── Controllers/           # API & Web Controllers
│   │   │   ├── Auth/
│   │   │   │   └── AuthController.php
│   │   │   ├── TrainerController.php
│   │   │   ├── EquipmentController.php
│   │   │   ├── PaymentController.php
│   │   │   ├── ExpenseController.php
│   │   │   ├── MessageController.php
│   │   │   └── DashboardController.php
│   │   ├── Middleware/
│   │   │   ├── IsAdmin.php
│   │   │   └── IsTrainer.php
│   │   └── Requests/             # Form validation (Optional)
│   │       ├── LoginRequest.php
│   │       ├── TrainerRequest.php
│   │       ├── EquipmentRequest.php
│   │       └── PaymentRequest.php
│   ├── Models/                    # Database Models
│   │   ├── User.php
│   │   ├── Trainer.php
│   │   ├── TrainerPerformance.php
│   │   ├── TrainerProgram.php
│   │   ├── Equipment.php
│   │   ├── Payment.php
│   │   ├── Expense.php
│   │   └── Message.php
│   ├── Services/                  # Business Logic Layer
│   │   ├── AuthService.php
│   │   ├── PaymentService.php
│   │   ├── ReportService.php
│   │   └── NotificationService.php
│   ├── Repositories/              # Data Access Layer
│   │   ├── TrainerRepository.php
│   │   ├── PaymentRepository.php
│   │   ├── EquipmentRepository.php
│   │   └── ExpenseRepository.php
│   ├── Traits/                    # Reusable trait code
│   │   ├── HasTimestamps.php
│   │   └── CanBeSearched.php
│   └── Providers/
│       └── AppServiceProvider.php
├── bootstrap/
│   ├── app.php                    # Application configuration
│   └── providers.php
├── config/
│   ├── app.php
│   ├── auth.php
│   ├── database.php
│   └── mail.php
├── database/
│   ├── migrations/                # Database migrations
│   │   ├── 2026_03_12_000001_create_trainers_table.php
│   │   ├── 2026_03_12_000002_create_equipment_table.php
│   │   ├── 2026_03_12_000003_create_payments_table.php
│   │   ├── 2026_03_12_000004_create_expenses_table.php
│   │   ├── 2026_03_12_000005_create_messages_table.php
│   │   ├── 2026_03_12_000006_create_trainer_performance_table.php
│   │   ├── 2026_03_12_000007_update_users_table.php
│   │   └── 2026_03_12_000008_create_trainer_programs_table.php
│   ├── factories/
│   │   ├── UserFactory.php
│   │   ├── TrainerFactory.php
│   │   ├── EquipmentFactory.php
│   │   └── PaymentFactory.php
│   └── seeders/
│       └── DatabaseSeeder.php
├── public/
│   ├── index.php                  # Entry point
│   ├── uploads/
│   │   ├── profiles/
│   │   ├── equipment/
│   │   └── documents/
│   └── assets/
│       ├── css/
│       ├── js/
│       └── images/
├── resources/
│   ├── css/
│   │   ├── app.css
│   │   └── bootstrap.css
│   ├── js/
│   │   ├── app.js
│   │   └── bootstrap.js
│   └── views/                     # Blade templates
│       ├── layouts/
│       │   ├── app.blade.php
│       │   └── dashboard.blade.php
│       ├── auth/
│       │   ├── login.blade.php
│       │   ├── register.blade.php
│       │   └── profile.blade.php
│       ├── dashboard/
│       │   ├── admin.blade.php
│       │   ├── trainer.blade.php
│       │   └── member.blade.php
│       ├── trainers/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── edit.blade.php
│       │   └── show.blade.php
│       ├── equipment/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── edit.blade.php
│       │   └── show.blade.php
│       ├── payments/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── user-history.blade.php
│       │   └── statistics.blade.php
│       ├── expenses/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── edit.blade.php
│       │   └── summary.blade.php
│       ├── messages/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   └── show.blade.php
│       └── welcome.blade.php
├── routes/
│   ├── web.php                    # Web routes
│   ├── api.php                    # API routes (optional)
│   └── console.php
├── storage/
│   ├── app/
│   ├── framework/
│   │   ├── cache/
│   │   ├── sessions/
│   │   └── views/
│   └── logs/
├── tests/
│   ├── Feature/
│   │   ├── AuthTest.php
│   │   ├── TrainerTest.php
│   │   ├── PaymentTest.php
│   │   └── EquipmentTest.php
│   └── Unit/
│       ├── UserTest.php
│       ├── TrainerTest.php
│       └── PaymentTest.php
├── .env                           # Environment variables
├── .env.example                   # Example env file
├── artisan                        # Laravel command runner
├── composer.json
├── composer.lock
├── package.json
├── phpunit.xml
└── README.md
```

---

## Directory Explanations

### 1. **app/Http/Controllers/**
- Handles all HTTP requests and responses
- Organized by feature (Auth, Trainers, Equipment, etc.)
- Each controller handles one resource's CRUD operations

### 2. **app/Models/**
- Eloquent Model classes matching database tables
- Contains relationships and query scopes
- Single responsibility principle

### 3. **app/Services/**
- Business logic layer
- Reusable logic used across controllers
- Payment processing, notifications, reports
- Example: `PaymentService::processPayment()`

### 4. **app/Repositories/**
- Data access abstraction layer
- Isolated database queries
- Example: `PaymentRepository::getMonthlyRevenue()`

### 5. **database/migrations/**
- Define database schema changes
- Version controlled
- Reversible (up/down methods)

### 6. **resources/views/**
- Blade template files
- Organized by feature
- Reusable components and layouts

---

## Eloquent ORM Conversion Examples

### Before (Raw MySQL):
```php
$trainers = mysqli_query($conn, "SELECT * FROM trainers WHERE status = 'active'");
while($t = mysqli_fetch_assoc($trainers)) {
    echo $t['name'];
}
```

### After (Eloquent):
```php
$trainers = Trainer::where('status', 'active')->get();
foreach ($trainers as $trainer) {
    echo $trainer->name;
}
```

---

## Model Relationships

### User → Payments (One-to-Many)
```php
$user = User::with('payments')->find(1);
$allPayments = $user->payments;
```

### Trainer → TrainerPerformance (One-to-One)
```php
$trainer = Trainer::with('performance')->find(1);
$performance = $trainer->performance;
```

### Trainer → TrainerPrograms (One-to-Many)
```php
$trainer = Trainer::with('trainerPrograms')->find(1);
$programs = $trainer->trainerPrograms;
```

---

## Authentication & Authorization

### Middleware Protection:
```php
// In routes/web.php
Route::middleware(['auth'])->group(function () {
    // Protected routes
});

Route::middleware(['auth', 'admin'])->group(function () {
    // Admin-only routes
});
```

### Check User Role in Controller:
```php
if (auth()->user()->role === 'admin') {
    // Admin logic
}
```

---

## Database Migrations - Run Commands

```bash
# Create all tables
php artisan migrate

# Rollback all migrations
php artisan migrate:rollback

# Reset database
php artisan migrate:reset

# Refresh database (drop and recreate)
php artisan migrate:refresh --seed

# Create new migration
php artisan make:migration create_table_name
```

---

## Useful Artisan Commands for Development

```bash
# Generate model with migration and factory
php artisan make:model Trainer -mf

# Generate controller
php artisan make:controller TrainerController

# Generate middleware
php artisan make:middleware IsAdmin

# Run tinker (interactive shell)
php artisan tinker

# Cache clear
php artisan cache:clear
php artisan config:clear
```

---

## Best Practices for Student Developers

### 1. **Use Eloquent Over Raw SQL**
- Prevents SQL injection
- More readable
- Easier to maintain

### 2. **Apply Validation**
```php
$validated = $request->validate([
    'email' => 'required|email|unique:users',
    'password' => 'required|min:8|confirmed',
]);
```

### 3. **Use Middleware for Authorization**
- Check permissions before processing
- Consistent security across app

### 4. **Soft Deletes for Important Data**
```php
use SoftDeletes;
// Data marked as deleted but not removed from DB
```

### 5. **Use Query Scopes**
```php
// Define in Model
public function scopeActive($query) {
    return $query->where('status', 'active');
}

// Use in Controller
$active = Trainer::active()->get();
```

### 6. **Load Relationships Efficiently**
```php
// Eager load to avoid N+1 queries
$trainers = Trainer::with('performance', 'trainerPrograms')->get();

// Instead of lazy loading
$trainers = Trainer::all(); // Then accessing $trainer->performance for each
```

### 7. **Use Form Requests for Validation**
```php
// Create: php artisan make:request TrainerRequest
// Use in controller: public function store(TrainerRequest $request)
```

---

## Environment Configuration (.env)

```env
APP_NAME="Royal Fitness"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=login_system
DB_USERNAME=root
DB_PASSWORD=

MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
```

---

## Testing Your Application

### Run migrations:
```bash
php artisan migrate
```

### Seed test data:
```bash
php artisan db:seed
```

### Run tests:
```bash
php artisan test
```

---

## Performance Tips for SaaS

1. **Use pagination instead of loading all data**
   ```php
   $trainers = Trainer::paginate(15);
   ```

2. **Cache queries for better performance**
   ```php
   $trainers = Cache::remember('active_trainers', 3600, fn() => 
       Trainer::active()->get()
   );
   ```

3. **Use indexes on frequently searched columns**
   - email, username, payment_date, expense_date

4. **Implement rate limiting**
   ```php
   Route::middleware('throttle:60,1')->group(function () { ... });
   ```

---

## Next Steps

1. **Create Blade templates** for all views
2. **Implement validation** using Form Requests
3. **Add tests** for critical features
4. **Set up API routes** for mobile app (optional)
5. **Implement analytics** dashboard for reports
6. **Add notification system** (email/SMS)
7. **Implement search** functionality
8. **Add role-based permissions** (Advanced)

---

## Resources

- [Laravel Documentation](https://laravel.com/docs)
- [Eloquent ORM Guide](https://laravel.com/docs/eloquent)
- [Blade Templating](https://laravel.com/docs/blade)
- [API Resources](https://laravel.com/docs/eloquent-resources)
