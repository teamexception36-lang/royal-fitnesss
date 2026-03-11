# Royal Fitness - Laravel Quick Reference Guide

## 🚀 Quick Start

```bash
# 1. Setup environment
cp .env.example .env
php artisan key:generate

# 2. Configure database in .env
DB_DATABASE=login_system
DB_USERNAME=root
DB_PASSWORD=

# 3. Run migrations
php artisan migrate

# 4. Start server
php artisan serve
```

---

## 📝 Common Eloquent Operations

### Get All Records
```php
$trainers = Trainer::all();
$trainers = Trainer::get();
```

### Get With Pagination
```php
$trainers = Trainer::paginate(15);
```

### Find by ID
```php
$trainer = Trainer::find(1);
$trainer = Trainer::findOrFail(1); // Throws 404 if not found
```

### Where Clauses
```php
$active = Trainer::where('status', 'active')->get();
$multiple = Trainer::where('status', 'active')
    ->where('salary', '>', 5000)
    ->get();
$in = Trainer::whereIn('id', [1, 2, 3])->get();
```

### Order By
```php
$sorted = Trainer::orderBy('name', 'asc')->get();
$desc = Trainer::orderBy('salary', 'desc')->get();
$latest = Payment::latest('payment_date')->get();
```

### Count & Sum
```php
$count = Trainer::count();
$total = Payment::sum('amount');
$avg = Payment::avg('amount');
```

### Create New Record
```php
$trainer = Trainer::create([
    'name' => 'John',
    'email' => 'john@example.com',
    'phone' => '9876543210',
    'specialty' => 'Cardio',
    'salary' => 5000,
]);
```

### Update Record
```php
$trainer->update(['salary' => 6000]);
// Or
Trainer::find(1)->update(['salary' => 6000]);
// Or
Trainer::where('id', 1)->update(['salary' => 6000]);
```

### Delete Record
```php
$trainer->delete();  // Soft delete
$trainer->forceDelete();  // Permanent delete
$trainer->restore();  // Restore soft deleted
Trainer::where('status', 'inactive')->delete();
```

---

## 🔗 Working with Relationships

### One-to-Many: User has Many Payments
```php
$user = User::with('payments')->find(1);
$payments = $user->payments;

foreach ($payments as $payment) {
    echo $payment->amount;
}
```

### One-to-One: Trainer has One Performance
```php
$trainer = Trainer::with('performance')->find(1);
$performance = $trainer->performance;
echo $performance->rating;
```

### Create Related Record
```php
$user = User::find(1);
$user->payments()->create([
    'plan_name' => 'Monthly',
    'amount' => 1100,
    'transaction_id' => 'TXN123',
]);
```

### Query Through Relationship
```php
// Get trainers who have programs
$trainers = Trainer::has('trainerPrograms')->get();

// Get trainers without programs
$trainers = Trainer::doesntHave('trainerPrograms')->get();
```

---

## 🎯 Query Scopes

### Define Scope in Model
```php
// In Trainer model
public function scopeActive($query) {
    return $query->where('status', 'active');
}
```

### Use Scope
```php
$active = Trainer::active()->get();
$active = Trainer::active()->orderBy('name')->paginate(10);
```

### Multiple Scopes
```php
// Define in model
public function scopeActive($query) { ... }
public function scopeHighSalary($query) {
    return $query->where('salary', '>', 5000);
}

// Use together
$trainers = Trainer::active()->highSalary()->get();
```

---

## 📝 Form Handling

### In Blade Template
```blade
<form method="POST" action="{{ route('trainers.store') }}">
    @csrf
    
    <input type="text" name="name" value="{{ old('name') }}">
    @error('name')
        <span class="error">{{ $message }}</span>
    @enderror
</form>
```

### In Controller
```php
public function store(Request $request) {
    $validated = $request->validate([
        'name' => 'required|string|max:255',
        'email' => 'required|email|unique:trainers',
        'phone' => 'required|digits:10',
    ]);
    
    Trainer::create($validated);
    return redirect()->route('trainers.index')->with('success', 'Trainer added!');
}
```

---

## 🔐 Authentication & Authorization

### Check if Logged In
```php
if (auth()->check()) {
    echo "User is logged in";
}

// Or in blade
@auth
    <p>Welcome {{ auth()->user()->name }}</p>
@endauth
```

### Get Current User
```php
$user = auth()->user();
echo $user->email;

// Shorthand
echo auth()->user()->email;
```

### Check User Role
```php
if (auth()->user()->role === 'admin') {
    // Admin logic
}

// In blade
@if(auth()->user()->role === 'admin')
    <p>Admin panel</p>
@endif
```

### Protect Routes
```php
// In routes/web.php
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
});

Route::middleware(['auth', 'admin'])->group(function () {
    Route::get('/admin', [AdminController::class, 'index']);
});
```

---

## 📊 Common Queries

### Get Revenue This Month
```php
$revenue = Payment::verified()
    ->whereMonth('payment_date', now()->month)
    ->whereYear('payment_date', now()->year)
    ->sum('amount');
```

### Get Expense by Category
```php
$rentExpense = Expense::where('category', 'Rent')
    ->sum('amount');
```

### Get Equipment Needing Maintenance
```php
$toMaintain = Equipment::where('condition_status', 'Maintenance Required')->get();
```

### Get User's Active Payments
```php
$payments = User::find($userId)
    ->payments()
    ->where('status', 'Verified')
    ->orderBy('payment_date', 'desc')
    ->get();
```

### Get Top Trainers by Rating
```php
$topTrainers = Trainer::with('performance')
    ->orderBy('performance.rating', 'desc')
    ->limit(10)
    ->get();
```

---

## 🎨 Blade Template Syntax

### Variables
```blade
<p>{{ $variable }}</p>
<p>{{ $user->name }}</p>
<p>{{ strtoupper($name) }}</p>
```

### Loops
```blade
@foreach($trainers as $trainer)
    <p>{{ $trainer->name }}</p>
@endforeach

@forelse($payments as $payment)
    <p>{{ $payment->amount }}</p>
@empty
    <p>No payments</p>
@endforelse
```

### Conditionals
```blade
@if($user->role === 'admin')
    <p>Admin panel</p>
@elseif($user->role === 'trainer')
    <p>Trainer panel</p>
@else
    <p>Member panel</p>
@endif
```

### Authentication
```blade
@auth
    <p>Logged in: {{ auth()->user()->name }}</p>
@endauth

@guest
    <p>Please login</p>
@endguest
```

### Forms
```blade
<form method="POST" action="{{ route('action') }}">
    @csrf
    <input type="text" name="field" value="{{ old('field') }}">
    @error('field')
        <span>{{ $message }}</span>
    @enderror
</form>
```

---

## 🛠️ Useful Commands

```bash
# Artisan commands
php artisan tinker                 # Interactive shell
php artisan migrate                # Run migrations
php artisan migrate:rollback       # Undo migrations
php artisan make:controller Name   # Create controller
php artisan make:model Name -m     # Create model with migration
php artisan db:seed               # Run seeders
php artisan cache:clear           # Clear cache
php artisan config:clear          # Clear config cache
php artisan route:list            # List all routes
php artisan serve                 # Start dev server
```

---

## 🔧 Controller Template

```php
<?php

namespace App\Http\Controllers;

use App\Models\Trainer;
use Illuminate\Http\Request;

class TrainerController extends Controller
{
    // Show all
    public function index() {
        $trainers = Trainer::paginate(10);
        return view('trainers.index', compact('trainers'));
    }

    // Show single
    public function show($id) {
        $trainer = Trainer::findOrFail($id);
        return view('trainers.show', compact('trainer'));
    }

    // Show create form
    public function create() {
        return view('trainers.create');
    }

    // Store new record
    public function store(Request $request) {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:trainers',
        ]);
        
        Trainer::create($validated);
        return redirect()->route('trainers.index')->with('success', 'Created!');
    }

    // Show edit form
    public function edit($id) {
        $trainer = Trainer::findOrFail($id);
        return view('trainers.edit', compact('trainer'));
    }

    // Update record
    public function update(Request $request, $id) {
        $trainer = Trainer::findOrFail($id);
        
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:trainers,email,'.$id,
        ]);
        
        $trainer->update($validated);
        return redirect()->route('trainers.index')->with('success', 'Updated!');
    }

    // Delete record
    public function destroy($id) {
        Trainer::findOrFail($id)->delete();
        return redirect()->route('trainers.index')->with('success', 'Deleted!');
    }
}
```

---

## 🚨 Debugging Tips

### Log Messages
```php
\Log::info('Message', ['context' => $data]);
\Log::error('Error message', ['exception' => $exception]);
```

### Dump and Die
```php
dd($variable);  // Dump and die
dump($variable); // Dump (continue)
```

### Query Debugging
```php
$trainers = Trainer::where('status', 'active');
dd($trainers->toSql());  // See the SQL
dd($trainers->getBindings()); // See the values
```

### Check Relationships
```php
// In tinker
$trainer = Trainer::with('performance')->find(1);
$trainer->performance; // Check if loaded
```

---

## 📚 File Locations

- **Models:** `app/Models/`
- **Controllers:** `app/Http/Controllers/`
- **Views:** `resources/views/`
- **Migrations:** `database/migrations/`
- **Routes:** `routes/web.php`
- **Config:** `config/`
- **ServiceProvider:** `app/Providers/`

---

## 💡 Pro Tips

1. **Use `with()`** to eagerly load relationships
   ```php
   $trainers = Trainer::with('performance')->get();
   ```

2. **Use named routes** instead of hardcoded URLs
   ```php
   route('trainers.edit', $trainer->id)  // Good
   '/trainers/' . $trainer->id . '/edit' // Bad
   ```

3. **Use `findOrFail()`** to avoid null checks
   ```php
   $trainer = Trainer::findOrFail($id); // Throws 404 automatically
   ```

4. **Use scopes** for repeated queries
   ```php
   Trainer::active()->highSalary()->get();
   ```

5. **Use validation** to prevent bad data
   ```php
   $validated = $request->validate([...]);
   ```

---

## 🔗 Quick Links

- **Models:** [Models List](docs/models.md)
- **Routes:** [Routes Configuration](routes/web.php)
- **Full Migration Guide:** [MIGRATION_GUIDE.md](MIGRATION_GUIDE.md)
- **Implementation Checklist:** [IMPLEMENTATION_CHECKLIST.md](IMPLEMENTATION_CHECKLIST.md)

---

**Keep this handy!** Bookmark this file for quick reference while coding.

Last Updated: March 12, 2026
