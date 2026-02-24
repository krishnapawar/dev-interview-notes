# 🔥 Core PHP

# 🔥 1️⃣ Difference Between `==` and `===`

- `==` → Compares value only (type conversion allowed)  
- `===` → Compares value + type (strict comparison)  

### Example:

```php
var_dump(5 == "5");   // true
var_dump(5 === "5");  // false
```

---

# 🔥 2️⃣ What Are Sessions in PHP?

Sessions store user data on the **server side**.

Used for:

- Login authentication  
- Shopping cart data  
- User preferences  

PHP stores the session ID in a cookie called `PHPSESSID`.

---

## 🔥 3️⃣ Difference Between `include` and `require`

- `include` → Warning if file not found, script continues  
- `require` → Fatal error if file not found, script stops  

---

# 🔥 4️⃣ Difference Between `include` and `include_once`

## ✅ `include`

- Includes a file  
- If file not found → **Warning** 
- Redeclaration errors (functions/classes)   
- Script continues execution  
- Can include the same file multiple times  

---

## ✅ `include_once`

- Includes a file only one time  
- If the same file is included again → ignored  
- Prevents redeclaration errors (functions/classes)  

📌 Use `include_once` when the file contains functions or classes.

---

# 🔥 5️⃣ Difference Between `require` and `require_once`

## ✅ `require`

- Includes a file  
- If file not found → **Fatal Error**  
- Script stops execution  
- Can include the same file multiple times  

---

## ✅ `require_once`

- Includes file only once  
- Stops script if file is missing  
- Prevents duplicate class/function declaration  

📌 In real projects, we usually use `require_once` for config files and autoload files.

---

# 🔥 6️⃣ Difference Between `echo` and `print_r()`

## ✅ `echo`

- Used to print string output  
- Faster  
- Can print multiple strings  
- No return value  

### Example:

```php
echo "Hello";
```

---

## ✅ `print_r()`

- Used to print arrays and objects (mainly for debugging)  
- Shows structured, human-readable output  
- Mostly used during development  

### Example:

```php
print_r($array);
```

---

📌 For debugging arrays → use `print_r()`  
📌 For normal output → use `echo`

---

# 🔥 7️⃣ Difference Between Session and Cookies

| Session | Cookies |
|----------|----------|
| Stored on server | Stored in browser |
| More secure | Less secure |
| Accessed via `$_SESSION` | Accessed via `$_COOKIE` |
| Expires when session ends (by default) | Can have custom expiry time |
| Used for login/authentication | Used for small data like "remember me" |

---

## 🎯 Strong Interview Line

> Session stores data on the server, and only the session ID is stored in the cookie.

---

# 🔥7️⃣ What Is Output Buffering?

Output buffering stores output in memory before sending it to the browser.

### Used For:

- Modifying headers after output  
- Improving performance  

### Function:

```php
ob_start();
echo "Hello";
$content = ob_get_clean();
```

---

# 🔥8️⃣ How to Prevent SQL Injection?

- Use prepared statements  
- Use parameter binding (PDO / MySQLi)  
- Never concatenate user input directly in queries  

---

# 🔥9️⃣ What Are PHP Superglobals?

Predefined global arrays:

- `$_GET`  
- `$_POST`  
- `$_SESSION`  
- `$_COOKIE`  
- `$_SERVER`  
- `$_FILES`  
- `$_REQUEST`  

---

# 🔥1️⃣0️⃣ Difference Between GET and POST

## ✅ GET

- Data sent in URL  
- Limited length  
- Less secure  
- Used for fetching data  

---

## ✅ POST

- Data sent in request body  
- No practical size limit  
- More secure than GET  
- Used for form submission and sensitive data  

---

# 🔥1️⃣1️⃣ What Is Rate Limiting?

## ✅ Definition

Rate limiting restricts the number of requests a user/client can make to a server within a specific time period.

### Example

- Max 100 requests per minute per user  
- 101st request → server blocks or returns **429 Too Many Requests**

---

## ✅ Why Rate Limiting Is Important

- Prevent API abuse  
- Stop brute-force attacks  
- Protect server from overload  
- Control resource usage  
- Ensure fair usage for all users  

---

## ✅ Real-Life Examples

- ATM machine → limited withdrawals per day  
- OTP requests → max 3 per 10 minutes  

---

## ✅ Example in Laravel

```php
Route::middleware(['throttle:60,1'])->group(function () {
    Route::get('/api/data', function () {
        return response()->json(['data' => 'Limited']);
    });
});
```

- 60 requests per 1 minute  

---

## ✅ Example in Node.js (Express)

```javascript
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP
});

app.use(limiter);
```

---

# 🔥1️⃣2️⃣ What Is Throttling?

## ✅ Definition

Throttling controls the **speed of requests**, slowing them down instead of completely blocking.

- Rate limiting → **block requests**  
- Throttling → **slow down requests**

### Example

- First 50 requests → fast  
- After that → 1-second delay per request  

---

## ✅ Where It’s Used

- API gateways  
- Network bandwidth control  
- File uploads  
- Search APIs  

---

# 🔥 Difference Between Rate Limiting and Throttling

| Rate Limiting | Throttling |
|---------------|------------|
| Blocks requests after limit | Slows down requests |
| Returns 429 error | Adds delay |
| Strict control | Soft control |
| Used for security | Used for performance |

---

## 🎯 Interview Smart Answer

> Rate limiting restricts the number of API requests a client can make, usually returning a 429 error when exceeded.  
> Throttling controls the speed of requests, slowing them down instead of blocking.  
> Rate limiting → security/abuse prevention  
> Throttling → performance management  

---

# 🔥1️⃣3️⃣ `fopen()` in PHP

## ✅ Definition

`fopen()` opens a file or URL and returns a **file pointer resource** to use with:

- `fread()`  
- `fwrite()`  
- `fgets()`  
- `fclose()`

### Syntax

```php
fopen(filename, mode);
```

- `filename` → Path of file  
- `mode` → How to open (read, write, append, etc.)

---

## 🔹 File Modes

| Mode | Meaning |
|------|---------|
| r    | Read only (file must exist) |
| r+   | Read + write |
| w    | Write only (overwrite file) |
| w+   | Read + write (overwrite) |
| a    | Append (add data at end) |
| a+   | Read + append |
| x    | Create new file (error if exists) |

---

## 🔹 Example 1 – Read File

```php
$file = fopen("data.txt", "r");
$content = fread($file, filesize("data.txt"));
echo $content;
fclose($file);
```

---

## 🔹 Example 2 – Write File

```php
$file = fopen("data.txt", "w");
fwrite($file, "Hello Krishna!");
fclose($file);
```

⚠ Overwrites if file exists, creates if missing  

---

## 🔹 Example 3 – Append Data

```php
$file = fopen("data.txt", "a");
fwrite($file, "\nNew Line Added");
fclose($file);
```

---

## 🔹 Important Points

- Always use `fclose()` to free memory  
- Handle errors properly:

```php
$file = fopen("data.txt", "r");
if (!$file) {
    die("Unable to open file.");
}
```

- `fopen()` can open URLs if `allow_url_fopen` enabled  

---

## 🎯 Interview Short Answer

> `fopen()` opens a file and returns a file resource for reading, writing, or appending.  
> Must be used with `fread()`, `fwrite()`, and `fclose()` for proper file handling.

---

# 🔥1️⃣4️⃣ Difference Between `fopen()` and `file_get_contents()`

## 1️⃣ `fopen()`

- Opens a file → returns **file resource**  
- Read/write line-by-line  
- Best for large files, streaming, appending  
- High control  

### Example

```php
$file = fopen("data.txt", "r");
while(!feof($file)) {
    echo fgets($file);
}
fclose($file);
```

---

## 2️⃣ `file_get_contents()`

- Reads entire file at once → returns **string**  
- Best for small files, JSON, API responses  
- Simple and short  

### Example

```php
$content = file_get_contents("data.txt");
echo $content;
```

---

## 🚀 Key Differences

| Feature | fopen() | file_get_contents() |
|---------|---------|-------------------|
| Returns | File resource | String |
| Reads   | Step-by-step | Entire file at once |
| Writing support | ✅ Yes | ❌ No |
| Large file handling | ✅ Better | ❌ Not recommended |
| Control level | High | Low |
| Code complexity | More | Simple |

---

# 🔥 When to Use

✅ Use `fopen()`:

- Large files  
- Write/append  
- Line-by-line reading  
- Better memory control  

✅ Use `file_get_contents()`:

- Small files  
- Quick read  
- Fetching JSON / API response  

---

## 🎯 Interview Smart Answer

> `fopen()` opens a file and returns a file resource for reading/writing/large files.  
> `file_get_contents()` reads the entire file at once as a string, simpler but less memory-efficient for large files.

---
## 7️⃣ What Are Magic Methods?

Special methods that start with `__`.

### Examples:

- `__construct()`  
- `__destruct()`  
- `__call()`  
- `__get()`  
- `__set()`  

---

# 🔥1️⃣5️⃣ What Is Late Static Binding?

Allows static methods to refer to the **called class** instead of the parent class using `static::`.

---

# 🔥 OOP Concepts

# 🔥 9️⃣ What Are the 4 Pillars of OOP?

- Encapsulation  
- Abstraction  
- Inheritance  
- Polymorphism  

---

## 🔥 🔟 Difference Between Abstract Class and Interface

| Abstract Class | Interface |
|---------------|------------|
| Can have method body | Only method declarations |
| Can have properties | No properties |
| Supports single inheritance | Multiple interfaces allowed |

---

## 🔥 1️⃣1️⃣ What Is a Trait?

A Trait is used to reuse methods in multiple classes.

It helps solve the **multiple inheritance problem** in PHP.

---

## 🔥 1️⃣2️⃣ What Is Dependency Injection?

Dependency Injection means passing dependencies from outside instead of creating them inside the class.

### Benefits:

- Improves testability  
- Reduces tight coupling  
- Follows SOLID principles  

---

# 🔥 1️⃣ What Happens Internally When You Type a URL in Laravel?

## ✅ Strong Answer (Request Lifecycle)

1️⃣ Request hits `public/index.php`  
2️⃣ Composer autoload loads the framework  
3️⃣ HTTP Kernel handles the request  
4️⃣ Global middleware runs  
5️⃣ Route is matched  
6️⃣ Controller action is executed  
7️⃣ Response passes back through middleware  
8️⃣ Response is sent back to the browser  

🎯 If you clearly explain the Laravel request lifecycle, it creates a very strong impression.

---

# 🔥 2️⃣ What Is the N+1 Query Problem?

## ✅ Problem Example

If you fetch 100 users and for each user fetch posts separately:

- 1 query for users  
- 100 queries for posts  
- Total = **101 queries**

This is called the **N+1 problem**.

---

## ✅ Solution: Eager Loading

```php
User::with('posts')->get();
```

✔ Loads relationships in a single optimized query  
✔ Improves performance  
✔ Shows strong ORM understanding  

---

# 🔥 3️⃣ Difference Between `static`, `self`, and `$this`

- `$this` → Refers to the current object instance  
- `self::` → Refers to the current class (compile-time binding)  
- `static::` → Refers to called class (late static binding, runtime binding)  

🎯 This is a commonly asked and important OOP question.

---

# 🔥 4️⃣ How Does PHP Handle Sessions Internally?

## ✅ Strong Answer

- Session ID is stored in a browser cookie  
- Session data is stored on the server (file by default)  
- On each request, PHP reads the session file using the session ID  
- Data is loaded into `$_SESSION`  

## ✅ Session Drivers (Laravel)

- `file`  
- `redis`  
- `database`  

Mentioning session drivers gives extra points.

---

# 🔥 5️⃣ What Is OPcache?

OPcache stores compiled PHP bytecode in memory.

## Without OPcache:

- PHP compiles script on every request  

## With OPcache:

- Cached bytecode is reused  
- Performance improves significantly  

---

# 🔥 6️⃣ How Do You Prevent Race Conditions?

## ✅ Example

Two users updating the same wallet balance simultaneously.

## ✅ Solutions

- Database transactions  
- Row-level locking (`SELECT ... FOR UPDATE`)  
- Optimistic locking  
- Atomic operations  

🎯 Mentioning transactions and locking shows strong backend knowledge.

---

# 🔥 7️⃣ Explain SOLID Principles Briefly

- **S** → Single Responsibility  
- **O** → Open/Closed  
- **L** → Liskov Substitution  
- **I** → Interface Segregation  
- **D** → Dependency Inversion  

## 🎯 Real Example

A Controller should not handle business logic directly.  
Instead, move business logic into a Service class.

Interviewers prefer practical examples over theory.

---

# 🔥 8️⃣ Difference Between `isset()` and `empty()`

- `isset()` → Checks if variable exists and is not null  
- `empty()` → Checks if variable is empty (`0`, `""`, `null`, `false`, `[]`)  

⚠ Tricky Part:

```php
$value = 0;

isset($value);  // true
empty($value);  // true
```

`0` is considered empty, but it is set.

---

# 🔥 9️⃣ How Do You Secure a Laravel API?

## ✅ Best Practices

- Use Sanctum or JWT authentication  
- Apply authentication middleware  
- Use rate limiting  
- Validate all inputs  
- Use CSRF protection (for web routes)  
- Hash passwords  
- Always use HTTPS  

🎯 Mentioning rate limiting and HTTPS sounds senior-level.

---

# 🔥 🔟 Difference Between Process and Thread

- A **process** is an independent program instance  
- A **thread** is a smaller execution unit inside a process  

## In PHP:

- PHP is mostly single-threaded per request  
- Each request runs separately  
- Laravel queue workers can run multiple processes  

---

# 🔥 4️⃣ Explain PHP Namespaces and Their Benefits

## ✅ What is a Namespace?

A **namespace** in PHP is used to organize classes and avoid naming conflicts.

If two classes have the same name in different files, a namespace prevents collision.

```php
namespace App\Services;

class PaymentService {}
```

### Use it like:

```php
use App\Services\PaymentService;

$payment = new PaymentService();
```

## ✅ Benefits of Namespaces

- Avoid class name conflicts  
- Better project structure  
- Supports PSR-4 autoloading  
- Cleaner large applications  

👉 In Laravel, almost everything is namespaced (Controllers, Models, Services).

---

# 🔥 7️⃣ What is Output Buffering in PHP?

## ✅ Definition

Output buffering means PHP stores output in memory before sending it to the browser.

### Normally:

```php
echo "Hello";
```

It directly prints.

### With buffering:

```php
ob_start();
echo "Hello";
$content = ob_get_clean();
```

Now output is stored in `$content`.

## ✅ Why is Output Buffering Used?

- Modify headers after output  
- Capture view content  
- Improve performance  
- Compress output  

👉 Laravel internally uses output buffering when rendering views.

---

# 🔥 8️⃣ How Does PHP Handle Sessions Internally?

## ✅ Step-by-Step Process

1. `session_start()` is called.  
2. PHP generates a session ID.  
3. Session ID is stored in a browser cookie (`PHPSESSID`).  
4. Session data is stored on the server (file/database/Redis).  

### On the next request:

- The cookie sends the session ID.  
- PHP loads the session data.  

### Example:

```php
$_SESSION['user_id'] = 1;
```

## ✅ Laravel Session Drivers

Laravel supports:

- `file`  
- `database`  
- `redis`  
- `memcached`  

👉 In Laravel, the session driver is configured in the `.env` file.

---

# 🔟 What is Late Static Binding?

Late Static Binding allows a child class to override static methods properly.

## Example:

```php
class ParentClass {
    public static function test() {
        static::who();
    }

    public static function who() {
        echo "Parent";
    }
}

class ChildClass extends ParentClass {
    public static function who() {
        echo "Child";
    }
}

ChildClass::test();
```

### ✅ Output:

```
Child
```

If we used `self::who()` → the output would be:

```
Parent
```

## ✅ Why Is It Important?

- Used in frameworks like Laravel for dynamic static resolution.  
- Enables proper polymorphism with static methods.  

---

# 🔥 1️⃣1️⃣ Explain All SOLID Principles

Very important for 4+ years experience 🔥

---

## S — Single Responsibility Principle (SRP)

One class should have only **one responsibility**.

### ❌ Bad:
`User` class handles DB + email.

### ✅ Good:
- `UserRepository` handles DB  
- `EmailService` handles email  

---

## O — Open/Closed Principle (OCP)

Software entities should be:

> Open for extension, closed for modification.

Use interfaces to extend functionality without changing existing code.

---

## L — Liskov Substitution Principle (LSP)

A child class must work wherever the parent class is expected.

If replacing a parent with a child breaks functionality → LSP is violated.

---

## I — Interface Segregation Principle (ISP)

Do not force a class to implement unused methods.

### ✅ Better Approach:
Create small, specific interfaces instead of one large interface.

---

## D — Dependency Inversion Principle (DIP)

Depend on **abstractions**, not concrete classes.

```php
class Order {
    public function __construct(PaymentInterface $payment) {}
}
```

High-level modules should not depend on low-level modules.  
Both should depend on abstractions.

---

# 🔥 6️⃣ What is Dependency Injection?

## ✅ Definition

Dependency Injection (DI) means providing dependencies from outside instead of creating them inside the class.

---

## ❌ Without DI:

```php
class Order {
    protected $payment;

    public function __construct() {
        $this->payment = new PaymentService();
    }
}
```

### ❗ Problems:

- Tight coupling  
- Hard to test  
- Not flexible  

---

## ✅ With DI:

```php
class Order {
    protected $payment;

    public function __construct(PaymentService $payment) {
        $this->payment = $payment;
    }
}
```

Now Laravel’s Service Container automatically injects it.

---

## ✅ Benefits of Dependency Injection

- Loose coupling  
- Easy testing (mocking)  
- Better maintainability  
- Follows SOLID principles  

---

# 🔥 7️⃣ Constructor Injection vs Method Injection

## 🔹 Constructor Injection

Dependency is injected when the object is created.

```php
public function __construct(PaymentService $payment)
```

✔ Best for required dependencies  
✔ Used in Controllers and Services  

---

## 🔹 Method Injection

Dependency is injected in a specific method.

```php
public function store(Request $request)
```

✔ Used when dependency is not needed everywhere  
✔ More flexible  

---

## 🎯 Interview Line

> Constructor injection is preferred for mandatory dependencies, while method injection is better for optional or specific-use dependencies.

---

# 🔥 8️⃣ Difference Between Composition and Inheritance

## 🔹 Inheritance → "is-a" Relationship

```php
class Vehicle {}
class Car extends Vehicle {}
```

Car **is a** Vehicle.

### ❗ Problems:

- Tight coupling  
- Hard to modify parent  

---

## 🔹 Composition → "has-a" Relationship

```php
class Engine {}

class Car {
    protected $engine;

    public function __construct(Engine $engine) {
        $this->engine = $engine;
    }
}
```

Car **has an** Engine.

✔ More flexible  
✔ Preferred in modern design  
✔ Follows SOLID principles  

---

## 🎯 Interview Line

> Composition is better than inheritance because it reduces tight coupling and increases flexibility.

---

# 🔥 9️⃣ What Design Patterns Have You Used in PHP Projects?

For 4+ years Laravel experience, you can say:

## ✅ Repository Pattern
Separates business logic from database logic.

## ✅ Service Pattern
Business logic is placed in service classes.

## ✅ Singleton Pattern
Ensures only one instance (e.g., database connection).

## ✅ Factory Pattern
Used in Laravel model factories.

## ✅ Observer Pattern
Used in Laravel Events & Listeners.

## ✅ MVC Pattern
Laravel follows MVC architecture.

---

## 🎯 Strong Interview Answer

> “In my projects, I have mainly used Repository, Service, Factory, and Observer patterns to maintain clean architecture and proper separation of concerns.”

---

# 🔥 🔟 Difference Between Jobs, Events, and Queues

## 🔹 Job

A task that needs to be executed (e.g., sending an email).

```php
SendEmailJob::dispatch();
```

---

## 🔹 Event

Represents something that happened.

Example:  
`UserRegistered` event.

---

## 🔹 Queue

A system that processes jobs asynchronously.

Example:  
Redis queue worker.

---

## 🎯 Simple Difference

| Jobs        | Events               | Queues                          |
|------------|----------------------|----------------------------------|
| Work to do | Something happened   | System that processes jobs       |
| Can be sync/async | Triggers listeners | Runs in background |

---

# 🔥 1️⃣1️⃣ How Does Laravel Queue Work Internally?

## ✅ Step-by-Step Internal Flow

### 1️⃣ Dispatch a Job

```php
SendEmailJob::dispatch($user);
```

### 2️⃣ Laravel Serializes the Job

The job object is serialized before being stored.

### 3️⃣ Job is Stored in a Queue Driver

It can be stored in:

- `database`
- `redis`
- `sqs`
- `beanstalkd`

### 4️⃣ Worker Runs

```bash
php artisan queue:work
```

### 5️⃣ Worker Process

The worker:

- Pulls the job from the queue  
- Unserializes the job  
- Calls the `handle()` method  

### 6️⃣ After Execution

- ✅ If successful → Job is deleted  
- ❌ If failed → Stored in `failed_jobs` table  

---

## 🎯 Important Concepts

Laravel uses:

- Service Container  
- Queue Drivers  
- Worker daemon process  

Queues make heavy tasks asynchronous and improve performance.

---

# 🔥 1️⃣2️⃣ How to Handle Failed Jobs?

## ✅ 1. Check Failed Jobs

```bash
php artisan queue:failed
```

## ✅ 2. Retry a Job

```bash
php artisan queue:retry 1
```

## ✅ 3. Set Retry Attempts in Job

```php
public $tries = 3;
```

## ✅ 4. Handle Failure Method

```php
public function failed(Exception $e) {
    Log::error($e->getMessage());
}
```

---

## 🎯 Best Practices

- Use proper timeout  
- Set retry attempts  
- Log errors properly  
- Monitor queues in production  

---

# 🔥 1️⃣3️⃣ What is Route Model Binding?

Laravel automatically injects a model instance from a route parameter.

---

## ❌ Without Binding

```php
Route::get('/user/{id}', function($id) {
    return User::find($id);
});
```

---

## ✅ With Route Model Binding

```php
Route::get('/user/{user}', function(User $user) {
    return $user;
});
```

Laravel automatically runs:

```sql
select * from users where id = ?
```

---

## ✅ Benefits

- Clean code  
- Less boilerplate  
- Automatic 404 if not found  

---

# 🔥 1️⃣4️⃣ How to Process Millions of Records in Laravel?

## ❌ Never Do This

```php
User::all();
```

This loads everything into memory and can cause memory overflow.

---

## ✅ Use `chunk()`

```php
User::chunk(1000, function($users) {
    foreach ($users as $user) {
        // process
    }
});
```

- Loads 1000 records at a time  
- Good for batch processing  

---

## ✅ Use `cursor()`

```php
foreach (User::cursor() as $user) {
    // process
}
```

- Loads one record at a time  
- Very memory efficient  

---

## ✅ Use Queues for Heavy Processing

- Dispatch jobs  
- Process in the background  

---

## 🎯 Interview Line

> For millions of records, I use `chunk()`, `cursor()`, and queues to prevent memory overflow and improve performance.

---

# 🔥 1️⃣5️⃣ Difference Between `chunk()` and `cursor()`

| chunk() | cursor() |
|----------|----------|
| Loads data in batches | Loads one record at a time |
| Faster for moderate data | Best for huge datasets |
| Uses multiple queries | Uses generator |
| Slightly higher memory usage | Very low memory usage |

---

## 🎯 Simple Answer

- Use `chunk()` for batch processing.  
- Use `cursor()` when memory efficiency is critical.

---

# 🔥 1️⃣6️⃣ How to Prevent Deadlocks in MySQL?

## ✅ What is a Deadlock?

A deadlock happens when:

- Transaction A locks Row 1 and waits for Row 2  
- Transaction B locks Row 2 and waits for Row 1  

Both transactions wait forever → **deadlock occurs**.

---

## ✅ How to Prevent Deadlocks?

### 1️⃣ Always Lock Rows/Tables in the Same Order

If updating multiple tables, follow the same sequence everywhere in your application.

---

### 2️⃣ Keep Transactions Short

Do not write long business logic inside transactions.

```php
DB::transaction(function() {
    // only database queries
});
```

✔ Keep only DB queries inside  
✔ Avoid API calls or heavy processing inside transactions  

---

### 3️⃣ Use Proper Indexing

Without indexes:

- Full table scans occur  
- More rows get locked  
- Deadlock risk increases  

---

### 4️⃣ Retry the Transaction

If a deadlock occurs, retry the transaction.

```php
try {
    DB::transaction(function() {
        // DB operations
    });
} catch (\Exception $e) {
    // retry logic
}
```

---

## 🎯 Interview Line

> To prevent deadlocks, I keep transactions small, maintain consistent locking order, use proper indexing, and implement retry logic.

---

# 🔥 1️⃣7️⃣ Explain Database Transactions & Isolation Levels

## ✅ What is a Transaction?

A transaction ensures:

- Either all queries succeed  
- Or everything rolls back  

```php
DB::transaction(function() {
    Order::create();
    Payment::create();
});
```

---

## ✅ ACID Properties

- **Atomicity** – All or nothing  
- **Consistency** – Data remains valid  
- **Isolation** – Transactions don’t interfere  
- **Durability** – Data persists after commit  

---

## ✅ Isolation Levels

### 1️⃣ Read Uncommitted
- Can read uncommitted data  
- Allows dirty reads  

### 2️⃣ Read Committed
- Can only read committed data  

### 3️⃣ Repeatable Read (MySQL Default)
- Same row returns same result within a transaction  

### 4️⃣ Serializable
- Highest isolation level  
- Slowest but safest  

---

## 🎯 Interview Line

> MySQL’s default isolation level is **Repeatable Read**.

---

# 1️⃣8️⃣ What is CSRF? How Laravel Prevents It?

## ✅ What is CSRF?

CSRF (Cross-Site Request Forgery) is an attack where a malicious site sends a request 
using a logged-in user’s session.

---

## ✅ How Laravel Prevents CSRF

1️⃣ Generates a CSRF token  
2️⃣ Stores it in the session  
3️⃣ Adds hidden field in forms:

```blade
@csrf
```

4️⃣ Middleware verifies the token  

If the token is invalid → **419 error**.
# 🔥 Strong Interview Line

> Laravel automatically validates the CSRF token using the `VerifyCsrfToken` middleware.

---

## 🎯 Important

All `POST`, `PUT`, `DELETE` routes are protected by CSRF middleware by default.

---

# 🔥 1️⃣9️⃣ What is XSS? How to Prevent in PHP/Laravel?

## ✅ What is XSS?

XSS (Cross-Site Scripting) is when an attacker injects malicious JavaScript into your website.

Example:

```html
<script>alert('Hacked')</script>
```

---

## ✅ How Laravel Prevents XSS

By default, Blade escapes output:

```blade
{{ $name }}
```

This converts `<script>` into safe HTML entities.

---

## ❌ Dangerous:

```blade
{!! $name !!}
```

This prints raw HTML (not escaped).

---

## ✅ Prevention Techniques

- Escape output  
- Validate input  
- Use Content Security Policy (CSP) headers  
- Avoid rendering raw HTML  
- Use prepared statements (prevents SQL injection)  

---

# 🎯 Now You Have Covered

- PHP Core  
- Advanced OOP  
- SOLID Principles  
- Design Patterns  
- Laravel Internals  
- Queue System  
- Database Concepts  
- Security Best Practices  

---

# 🔥 1️⃣ Wallet Balance Bug (Race Condition)

## ✅ Scenario

Two users try to withdraw money at the same time.  
Wallet balance becomes negative.

## ❓ How Will You Fix It?

### ✅ Strong Answer

- Use a database transaction  
- Use `SELECT ... FOR UPDATE`  
- Lock the row before updating  
- Commit only after successful deduction  

```php
DB::transaction(function () use ($userId, $amount) {
    $wallet = DB::table('wallets')
        ->where('user_id', $userId)
        ->lockForUpdate()
        ->first();

    if ($wallet->balance >= $amount) {
        DB::table('wallets')
            ->where('user_id', $userId)
            ->update([
                'balance' => $wallet->balance - $amount
            ]);
    } else {
        throw new Exception("Insufficient balance");
    }
});
```

🎯 If you say “I will check balance before updating” → weak answer.  
Race condition still possible without locking.

---

# 🔥 2️⃣ Website Suddenly Slow in Production

## ✅ Scenario

Site was working fine. Now response time is 6–7 seconds.

## ❓ What Will You Check First?

### ✅ Debugging Approach

- Check slow queries (enable query log)  
- Check N+1 problem  
- Check missing indexes  
- Check server CPU / RAM usage  
- Check external API delays  
- Check queue backlog  

🎯 They want structured debugging thinking, not guessing.

---

# 🔥 3️⃣ Users Getting Logged Out Randomly

## ✅ Possible Reasons

- Session lifetime too low  
- Session driver misconfigured  
- Redis/file storage issue  
- Load balancer without sticky sessions  

🎯 Mentioning load balancer session issue = very impressive.

---

# 🔥 4️⃣ File Upload Works Locally but Fails on Server

## ✅ What to Check?

- `php.ini` → `upload_max_filesize`  
- `post_max_size`  
- File permissions  
- Storage link:

```bash
php artisan storage:link
```

- Nginx / Apache configuration  

🎯 Production awareness shows senior-level maturity.

---

# 🔥 5️⃣ Emails Not Sending in Production

## ✅ Steps to Debug

- Is queue worker running?  
- Check `.env` mail configuration  
- Check `failed_jobs` table  
- Verify SMTP credentials  
- Check firewall blocking mail port  

🎯 Mentioning queue workers = senior-level answer.

---

# 🔥 6️⃣ API Suddenly Returning 500 Error

## ✅ How to Debug in Production

- Check logs (`storage/logs`)  
- Check recent deployment changes  
- Verify environment variables  
- Enable error logging (not display errors)  
- Rollback deployment if needed  

🎯 Mentioning rollback strategy = impressive.

---

# 🔥 7️⃣ Payment Deducted but Order Not Created

## ✅ Critical Scenario

## ❓ What Should You Do?

- Use DB transaction for order + payment update  
- If one fails → rollback everything  
- Use idempotency key for payment API  
- Log every payment response  

🎯 This checks architectural thinking and data consistency handling.

---

# 🔥 8️⃣ High CPU Usage on Server

## ✅ What to Check?

- Infinite loops?  
- Heavy cron jobs?  
- Queue workers stuck?  
- Too many requests (DDoS)?  
- Missing caching?  

🎯 Mentioning rate limiting and caching = smart answer.

---

# 🔥 9️⃣ Data Mismatch Between Two Servers

## ✅ Scenario

Production has 2 servers behind load balancer.  
Some users see different data.

## ✅ Possible Causes

- Session not centralized  
- Cache not shared  
- Database replication lag  

🎯 Mention centralized Redis/session storage = advanced answer.

---

# 🔥 🔟 Memory Leak in PHP Script

## ✅ Scenario

Long-running script keeps increasing memory usage.

## ✅ What to Check?

- Unset large variables  
- Chunk large queries  
- Avoid loading huge collections  
- Use `cursor()` instead of `get()`  

### Example:

```php
User::chunk(100, function ($users) {
    foreach ($users as $user) {
        // process user
    }
});
```

✔ Shows real optimization knowledge  
✔ Prevents memory overflow  
✔ Suitable for large datasets  