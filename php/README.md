# 🐘 PHP & Backend Architecture — Interview Questions & Answers

A comprehensive, production-grade interview and revision guide covering Core PHP internals, OOP patterns, modern Laravel architecture, concurrency, database locking, and real-world system debugging.

---

## 📋 Table of Contents

1. [Difference Between == and ===](#1-difference-between-and-)
2. [Difference Between Session and Cookies](#2-difference-between-session-and-cookies)
3. [How Does PHP Handle Sessions Internally?](#3-how-does-php-handle-sessions-internally)
4. [What Is OPcache in PHP?](#4-what-is-opcache-in-php)
5. [Difference Between include and require](#5-difference-between-include-and-require)
6. [Difference Between include and include_once](#6-difference-between-include-and-includeonce)
7. [Difference Between require and require_once](#7-difference-between-require-and-requireonce)
8. [Difference Between echo and print_r()](#8-difference-between-echo-and-printr)
9. [How to Prevent SQL Injection in PHP?](#9-how-to-prevent-sql-injection-in-php)
10. [What Are PHP Superglobals?](#10-what-are-php-superglobals)
11. [Difference Between GET and POST](#11-difference-between-get-and-post)
12. [What Is Rate Limiting in Web Applications?](#12-what-is-rate-limiting-in-web-applications)
13. [What Is Throttling in Web Applications?](#13-what-is-throttling-in-web-applications)
14. [Difference Between Rate Limiting and Throttling](#14-difference-between-rate-limiting-and-throttling)
15. [Difference Between Laravel 8, 9, 10, 11 and 12](#15-difference-between-laravel-8-9-10-11-and-12)
16. [What is fopen() in PHP and How Does It Work?](#16-what-is-fopen-in-php-and-how-does-it-work)
17. [Difference Between fopen() and file_get_contents()](#17-difference-between-fopen-and-filegetcontents)
18. [What Are Magic Methods in PHP?](#18-what-are-magic-methods-in-php)
19. [What Are the 4 Pillars of Object-Oriented Programming (OOP)?](#19-what-are-the-4-pillars-of-object-oriented-programming-oop)
20. [Difference Between Abstract Class and Interface in PHP](#20-difference-between-abstract-class-and-interface-in-php)
21. [What Is a Trait in PHP?](#21-what-is-a-trait-in-php)
22. [What Happens Internally When You Type a URL in Laravel?](#22-what-happens-internally-when-you-type-a-url-in-laravel)
23. [What Is the N+1 Query Problem in Eloquent and How Do You Fix It?](#23-what-is-the-n1-query-problem-in-eloquent-and-how-do-you-fix-it)
24. [Difference Between static, self, and $this in PHP](#24-difference-between-static-self-and-this-in-php)
25. [How Do You Prevent Race Conditions in PHP and Databases?](#25-how-do-you-prevent-race-conditions-in-php-and-databases)
26. [Difference Between isset() and empty() in PHP](#26-difference-between-isset-and-empty-in-php)
27. [How Do You Secure a Laravel API?](#27-how-do-you-secure-a-laravel-api)
28. [Difference Between Process and Thread](#28-difference-between-process-and-thread)
29. [Explain PHP Namespaces and Their Benefits](#29-explain-php-namespaces-and-their-benefits)
30. [What is Output Buffering in PHP?](#30-what-is-output-buffering-in-php)
31. [What is Late Static Binding in PHP?](#31-what-is-late-static-binding-in-php)
32. [Explain All SOLID Principles with PHP Examples](#32-explain-all-solid-principles-with-php-examples)
33. [What is Dependency Injection and Why Is It Used?](#33-what-is-dependency-injection-and-why-is-it-used)
34. [Difference Between Constructor Injection and Method Injection](#34-difference-between-constructor-injection-and-method-injection)
35. [Difference Between Composition and Inheritance](#35-difference-between-composition-and-inheritance)
36. [What Design Patterns Have You Used in PHP Projects?](#36-what-design-patterns-have-you-used-in-php-projects)
37. [Difference Between Jobs, Events, and Queues in Laravel](#37-difference-between-jobs-events-and-queues-in-laravel)
38. [How Does Laravel Queue Work Internally?](#38-how-does-laravel-queue-work-internally)
39. [How Do You Handle Failed Jobs in Laravel?](#39-how-do-you-handle-failed-jobs-in-laravel)
40. [What is Route Model Binding in Laravel?](#40-what-is-route-model-binding-in-laravel)
41. [How to Process Millions of Records in Laravel Without Crashing Memory?](#41-how-to-process-millions-of-records-in-laravel-without-crashing-memory)
42. [Difference Between chunk() and cursor() in Laravel](#42-difference-between-chunk-and-cursor-in-laravel)
43. [How to Prevent Deadlocks in MySQL?](#43-how-to-prevent-deadlocks-in-mysql)
44. [Explain Database Transactions and Isolation Levels](#44-explain-database-transactions-and-isolation-levels)
45. [What is CSRF and How Does Laravel Prevent It?](#45-what-is-csrf-and-how-does-laravel-prevent-it)
46. [What is XSS and How Do You Prevent It in PHP and Laravel?](#46-what-is-xss-and-how-do-you-prevent-it-in-php-and-laravel)
47. [How to Debug and Fix a Wallet Balance Race Condition Bug?](#47-how-to-debug-and-fix-a-wallet-balance-race-condition-bug)
48. [How to Diagnose and Fix a Website That Is Suddenly Slow in Production?](#48-how-to-diagnose-and-fix-a-website-that-is-suddenly-slow-in-production)
49. [Why Are Users Getting Logged Out Randomly and How Do You Fix It?](#49-why-are-users-getting-logged-out-randomly-and-how-do-you-fix-it)
50. [Why Does File Upload Work Locally but Fail on Server?](#50-why-does-file-upload-work-locally-but-fail-on-server)
51. [Why Are Emails Not Sending in Production and How Do You Fix It?](#51-why-are-emails-not-sending-in-production-and-how-do-you-fix-it)
52. [How to Troubleshoot an API Suddenly Returning 500 Error?](#52-how-to-troubleshoot-an-api-suddenly-returning-500-error)
53. [How to Handle: Payment Deducted but Order Not Created?](#53-how-to-handle-payment-deducted-but-order-not-created)
54. [How to Troubleshoot High CPU Usage on Server?](#54-how-to-troubleshoot-high-cpu-usage-on-server)
55. [How to Resolve Data Mismatch Between Two Servers?](#55-how-to-resolve-data-mismatch-between-two-servers)
56. [How to Detect and Fix a Memory Leak in a PHP Script?](#56-how-to-detect-and-fix-a-memory-leak-in-a-php-script)

---

## 1. Difference Between == and ===

> **Interview-Ready Answer:**  
> In PHP, `==` (loose comparison) compares values after type coercion, while `===` (strict comparison) checks both value and data type without any type conversion.

- `==` → Compares value only (type conversion allowed)  
- `===` → Compares value + type (strict comparison)  

**Example::**

```php
var_dump(5 == "5");   // true
var_dump(5 === "5");  // false
```

---

## 2. Difference Between Session and Cookies

> **Interview-Ready Answer:**  
> Cookies store data on the client browser and are sent with every HTTP request; Sessions store user data securely on the server, saving only a unique session identifier cookie (`PHPSESSID`) on the browser.

| Session | Cookies |
|----------|----------|
| Stored on server | Stored in browser |
| More secure | Less secure |
| Accessed via `$_SESSION` | Accessed via `$_COOKIE` |
| Expires when session ends (by default) | Can have custom expiry time |
| Used for login/authentication | Used for small data like "remember me" |

---

**🎯 Strong Interview Line:**

> Session stores data on the server, and only the session ID is stored in the cookie.

---

## 3. How Does PHP Handle Sessions Internally?

> **Interview-Ready Answer:**  
> When `session_start()` executes, PHP checks for a `PHPSESSID` cookie. If not found, it generates a unique cryptographic ID and sets a cookie; session data is serialized and stored on the server file system or in-memory stores like Redis.

Sessions store user data on the **server side**.

Used for:

- Login authentication  
- Shopping cart data  
- User preferences  

**✅ Step-by-Step Process:**

1. `session_start()` is called.  
2. PHP generates a session ID.  
3. Session ID is stored in a browser cookie (`PHPSESSID`).  
4. Session data is stored on the server (file/database/Redis).  

**On the next request::**

- The cookie sends the session ID.  
- PHP loads the session data.  

**Example::**

```php
$_SESSION['user_id'] = 1;
```

**✅ Laravel Session Drivers:**

Laravel supports:

- `file`  
- `database`  
- `redis`  
- `memcached`  

👉 In Laravel, the session driver is configured in the `.env` file.

---

## 4. What Is OPcache in PHP?

> **Interview-Ready Answer:**  
> OPcache is an opcode caching engine built into PHP that compiles human-readable PHP scripts into bytecode (opcode) once and stores them in shared memory, eliminating compilation overhead on subsequent requests.

OPcache stores compiled PHP bytecode in memory.

**Without OPcache::**

- PHP compiles script on every request  

**With OPcache::**

- Cached bytecode is reused  
- Performance improves significantly

---

## 5. Difference Between include and require

> **Interview-Ready Answer:**  
> `include` generates a PHP Warning and continues script execution if the file is missing; `require` generates a Fatal Error and immediately halts script execution.

- `include` → Warning if file not found, script continues  
- `require` → Fatal error if file not found, script stops

---

## 6. Difference Between include and include_once

> **Interview-Ready Answer:**  
> `include` loads and executes the specified file every time it is called, whereas `include_once` checks if the file has already been loaded, preventing duplicate function definitions or variable re-declarations.

**✅ `include`:**

- Includes a file  
- If file not found → **Warning**   
- Script continues execution  
- Can include the same file multiple times  
- Redeclaration errors (functions/classes) 

---

**✅ `include_once`:**

- Includes a file only one time  
- If the same file is included again → ignored  
- Prevents redeclaration errors (functions/classes)  

📌 Use `include_once` when the file contains functions or classes.

---

## 7. Difference Between require and require_once

> **Interview-Ready Answer:**  
> `require` halts script execution with a Fatal Error if the file is missing; `require_once` also throws a Fatal Error if missing, but guarantees the file is included at most once per request.

**✅ `require`:**

- Includes a file  
- If file not found → **Fatal Error**  
- Script stops execution  
- Can include the same file multiple times  

---

**✅ `require_once`:**

- Includes file only once  
- Stops script if file is missing  
- Prevents duplicate class/function declaration  

📌 In real projects, we usually use `require_once` for config files and autoload files.

---

## 8. Difference Between echo and print_r()

> **Interview-Ready Answer:**  
> `echo` is a language construct that outputs one or more scalar strings with high performance; `print_r()` is a built-in function that prints human-readable representations of complex variables like arrays and objects.

**✅ `echo`:**

- Used to print string output  
- Faster  
- Can print multiple strings  
- No return value  

**Example::**

```php
echo "Hello";
```

---

**✅ `print_r()`:**

- Used to print arrays and objects (mainly for debugging)  
- Shows structured, human-readable output  
- Mostly used during development  

**Example::**

```php
print_r($array);
```

---

📌 For debugging arrays → use `print_r()`  
📌 For normal output → use `echo`

---

## 9. How to Prevent SQL Injection in PHP?

> **Interview-Ready Answer:**  
> Prevent SQL injection by using PDO or MySQLi Prepared Statements with parameterized queries, separating SQL code from user-supplied inputs so data cannot be executed as SQL commands.

- Use prepared statements  
- Use parameter binding (PDO / MySQLi)  
- Never concatenate user input directly in queries

---

## 10. What Are PHP Superglobals?

> **Interview-Ready Answer:**  
> Superglobals are built-in global associative arrays available in all scopes without the `global` keyword, providing access to request inputs, server environments, sessions, and cookies (`$_GET`, `$_POST`, `$_SERVER`, `$_SESSION`, etc.).

Predefined global arrays:

- `$_GET`  
- `$_POST`  
- `$_SESSION`  
- `$_COOKIE`  
- `$_SERVER`  
- `$_FILES`  
- `$_REQUEST`

---

## 11. Difference Between GET and POST

> **Interview-Ready Answer:**  
> `GET` appends form data to the URL query string (visible, bookmarkable, cached, length-limited); `POST` sends data inside the HTTP request body (invisible in URL, not cached, suitable for sensitive data and file uploads).

**✅ GET:**

- Data sent in URL  
- Limited length  
- Less secure  
- Used for fetching data  

---

**✅ POST:**

- Data sent in request body  
- No practical size limit  
- More secure than GET  
- Used for form submission and sensitive data

---

## 12. What Is Rate Limiting in Web Applications?

> **Interview-Ready Answer:**  
> Rate limiting restricts the maximum number of requests a user or IP address can submit to an API within a defined window of time (e.g., 60 requests per minute) to prevent brute-force attacks and resource exhaustion.

**✅ Definition:**

Rate limiting restricts the number of requests a user/client can make to a server within a specific time period.

**Example:**

- Max 100 requests per minute per user  
- 101st request → server blocks or returns **429 Too Many Requests**

---

**✅ Why Rate Limiting Is Important:**

- Prevent API abuse  
- Stop brute-force attacks  
- Protect server from overload  
- Control resource usage  
- Ensure fair usage for all users  

---

**✅ Real-Life Examples:**

- ATM machine → limited withdrawals per day  
- OTP requests → max 3 per 10 minutes  

---

**✅ Example in Laravel:**

```php
Route::middleware(['throttle:60,1'])->group(function () {
    Route::get('/api/data', function () {
        return response()->json(['data' => 'Limited']);
    });
});
```

- 60 requests per 1 minute  

---

**✅ Example in Node.js (Express):**

```javascript
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP
});

app.use(limiter);
```

---

## 13. What Is Throttling in Web Applications?

> **Interview-Ready Answer:**  
> Throttling manages and controls the velocity or throughput of incoming requests, delaying or pacing execution rather than outright dropping requests, ensuring downstream services avoid spikes.

**✅ Definition:**

Throttling controls the **speed of requests**, slowing them down instead of completely blocking.

- Rate limiting → **block requests**  
- Throttling → **slow down requests**

**Example:**

- First 50 requests → fast  
- After that → 1-second delay per request  

---

**✅ Where It’s Used:**

- API gateways  
- Network bandwidth control  
- File uploads  
- Search APIs

---

## 14. Difference Between Rate Limiting and Throttling

> **Interview-Ready Answer:**  
> Rate limiting enforces a hard cap and rejects excess requests with HTTP 429 Too Many Requests; Throttling regulates traffic speed and delays processing to flatten bursty traffic over time.

| Rate Limiting | Throttling |
|---------------|------------|
| Blocks requests after limit | Slows down requests |
| Returns 429 error | Adds delay |
| Strict control | Soft control |
| Used for security | Used for performance |

---

**🎯 Interview Smart Answer:**

> Rate limiting restricts the number of API requests a client can make, usually returning a 429 error when exceeded.  
> Throttling controls the speed of requests, slowing them down instead of blocking.  
> Rate limiting → security/abuse prevention  
> Throttling → performance management

---

## 15. Difference Between Laravel 8, 9, 10, 11 and 12

> **Interview-Ready Answer:**  
> Laravel evolution moved from verbose configs to streamlined defaults: Laravel 9 required PHP 8 and Symfony 6; Laravel 10 added native type declarations; Laravel 11 introduced a streamlined directory structure without kernel files; Laravel 12 adds enhanced concurrency and performance optimizations.

🔹 Laravel 8 (2020)
**Key Features::**
> Introduced Job Batching.
> Improved Model Factory (class-based).
> Laravel Jetstream (auth system).

👉 Example:
```php
Bus::batch([...])->dispatch();
```
**Summary::**

➡️ Major update for structure and testing

🔹 Laravel 9 (2022)
Key Features:
> Uses Symfony 6.
> Minimum PHP 8 required.
> Anonymous stub migrations.
> Improved route:list UI.
Example:
```php
return new class extends Migration {};
```
**Summary::**

➡️ Modern PHP support + performance improvements

🔹 Laravel 10 (2023)
**Key Features::**
> Better type declarations.
> Improved validation rules.
> Faster performance.
> Native support for latest PHP versions.
Example:
> public function rules(): array.
**Summary::**

➡️ Stability + cleaner code + better typing

🔹 Laravel 11 (2024)

🔥 Big change version

Key Features:
> New slim folder structure.
> Removed unnecessary files (clean project).
> No kernel files (simplified).
> Config reduced.
> Built-in health check route.
Example:
```php
    Route::get('/up', fn() => 'OK');

```
**Summary::**

➡️ Clean, fast, minimal Laravel

🔹 Laravel 12 (2025 - Latest Trend)

(Based on evolution, small improvements)

Expected / New:
> Performance improvements
> Better async support.
> Improved queue handling.
> More developer-friendly defaults.
**Summary::**

➡️ More optimized and modern

🔐 Security Updates in Laravel 11 & 12
🔹 Laravel 11 Security Improvements
✅ 1. Better Password Hashing
> Uses strong hashing (bcrypt/argon2)
```php
    Hash::make('password');

```
✅ 2. Improved CSRF Protection
> Automatic CSRF token validation.
> Middleware more optimized.

✅ 3. Safer File Uploads
> Better validation for file types.
> Prevents malicious uploads.

✅ 4. Secure Headers (by default)
> XSS protection headers.
> Content Security improvements.

✅ 5. Improved Rate Limiting
```php
RateLimiter::for('api', function () {});

```
> Prevents brute force attacks

🔹 Laravel 12 Security (Latest Enhancements)
✅ 1. Better Authentication Security
> Stronger session handling.
> Improved token security.

✅ 2. Improved XSS Protection
> Blade escaping improved.
> Safer rendering.

✅ 3. Queue Security
> Safer job serialization.
> Prevents data tampering.

✅ 4. API Security Improvements
> Better token validation.
> Improved middleware.

**🎯 Final Short Interview Answer:**

👉 “Laravel 8 introduced job batching and modern factories. Laravel 9 moved to PHP 8 and Symfony 6. Laravel 10 focused on stability and type safety. Laravel 11 simplified the structure and improved performance. Laravel 12 continues with optimizations and better security.”

👉 “Laravel 11 and 12 improved security with better CSRF protection, secure headers, stronger authentication, and improved rate limiting.”

---

## 16. What is fopen() in PHP and How Does It Work?

> **Interview-Ready Answer:**  
> `fopen()` opens a file or URL stream resource in specified modes (`r`, `w`, `a`), returning a pointer resource that allows memory-efficient, chunk-by-chunk reading and writing via `fgets()` or `fwrite()`.

**✅ Definition:**

`fopen()` opens a file or URL and returns a **file pointer resource** to use with:

- `fread()`  
- `fwrite()`  
- `fgets()`  
- `fclose()`

**Syntax:**

```php
fopen(filename, mode);
```

- `filename` → Path of file  
- `mode` → How to open (read, write, append, etc.)

---

**🔹 File Modes:**

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

**🔹 Example 1 – Read File:**

```php
$file = fopen("data.txt", "r");
$content = fread($file, filesize("data.txt"));
echo $content;
fclose($file);
```

---

**🔹 Example 2 – Write File:**

```php
$file = fopen("data.txt", "w");
fwrite($file, "Hello Krishna!");
fclose($file);
```

⚠ Overwrites if file exists, creates if missing  

---

**🔹 Example 3 – Append Data:**

```php
$file = fopen("data.txt", "a");
fwrite($file, "\nNew Line Added");
fclose($file);
```

---

**🔹 Important Points:**

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

**🎯 Interview Short Answer:**

> `fopen()` opens a file and returns a file resource for reading, writing, or appending.  
> Must be used with `fread()`, `fwrite()`, and `fclose()` for proper file handling.

---

## 17. Difference Between fopen() and file_get_contents()

> **Interview-Ready Answer:**  
> `file_get_contents()` reads an entire file into a string in memory at once (best for small files); `fopen()` reads the file as a stream line-by-line (best for huge files to prevent PHP memory exhaustion).

**1️⃣ `fopen()`:**

- Opens a file → returns **file resource**  
- Read/write line-by-line  
- Best for large files, streaming, appending  
- High control  

**Example:**

```php
$file = fopen("data.txt", "r");
while(!feof($file)) {
    echo fgets($file);
}
fclose($file);
```

---

**2️⃣ `file_get_contents()`:**

- Reads entire file at once → returns **string**  
- Best for small files, JSON, API responses  
- Simple and short  

**Example:**

```php
$content = file_get_contents("data.txt");
echo $content;
```

---

**🚀 Key Differences:**

| Feature | fopen() | file_get_contents() |
|---------|---------|-------------------|
| Returns | File resource | String |
| Reads   | Step-by-step | Entire file at once |
| Writing support | ✅ Yes | ❌ No |
| Large file handling | ✅ Better | ❌ Not recommended |
| Control level | High | Low |
| Code complexity | More | Simple |

---

**💡 When to Use::**

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

**🎯 Interview Smart Answer:**

> `fopen()` opens a file and returns a file resource for reading/writing/large files.  
> `file_get_contents()` reads the entire file at once as a string, simpler but less memory-efficient for large files.

---

## 18. What Are Magic Methods in PHP?

> **Interview-Ready Answer:**  
> Magic methods are special reserved methods starting with a double underscore (like `__construct`, `__get`, `__set`, `__call`, `__toString`) that PHP invokes automatically in response to specific lifecycle actions or property accesses.

Special methods that start with `__`.

**Examples::**

- `__construct()`  
- `__destruct()`  
- `__call()`  
- `__get()`  
- `__set()`

---

## 19. What Are the 4 Pillars of Object-Oriented Programming (OOP)?

> **Interview-Ready Answer:**  
> The 4 pillars of OOP are **Encapsulation** (bundling data and methods with access modifiers), **Abstraction** (hiding internal implementation details), **Inheritance** (reusing code across parent-child classes), and **Polymorphism** (allowing different classes to implement common interfaces).

- Encapsulation  
- Abstraction  
- Inheritance  
- Polymorphism

---

## 20. Difference Between Abstract Class and Interface in PHP

> **Interview-Ready Answer:**  
> An Abstract Class can contain both abstract methods and concrete implemented methods with member variables, and supports single inheritance; an Interface defines only method signatures and constants, and a class can implement multiple interfaces.

| Abstract Class | Interface |
|---------------|------------|
| Can have method body | Only method declarations |
| Can have properties | No properties |
| Supports single inheritance | Multiple interfaces allowed |

---

## 21. What Is a Trait in PHP?

> **Interview-Ready Answer:**  
> A Trait is a mechanism for code reuse in single-inheritance languages like PHP, allowing developers to reuse sets of methods across independent classes without requiring class inheritance (`use MyTrait;`).

A Trait is used to reuse methods in multiple classes.

It helps solve the **multiple inheritance problem** in PHP.

---

**🔥 1️⃣2️⃣ What Is Dependency Injection?:**

Dependency Injection means passing dependencies from outside instead of creating them inside the class.

**Benefits::**

- Improves testability  
- Reduces tight coupling  
- Follows SOLID principles

---

## 22. What Happens Internally When You Type a URL in Laravel?

> **Interview-Ready Answer:**  
> The web server routes the request to `public/index.php`, which loads Composer autoloader, initializes the HTTP Kernel via the Service Container, runs global and route middleware pipelines, matches the route, executes the controller, and emits an HTTP Response.

**✅ Strong Answer (Request Lifecycle):**

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

## 23. What Is the N+1 Query Problem in Eloquent and How Do You Fix It?

> **Interview-Ready Answer:**  
> The N+1 problem occurs when fetching N parent records executes 1 initial query plus N additional queries for related records during loop iterations; it is fixed using eager loading (`with()`), reducing total queries to 2.

**✅ Problem Example:**

If you fetch 100 users and for each user fetch posts separately:

- 1 query for users  
- 100 queries for posts  
- Total = **101 queries**

This is called the **N+1 problem**.

---

**✅ Solution: Eager Loading:**

```php
User::with('posts')->get();
```

✔ Loads relationships in a single optimized query  
✔ Improves performance  
✔ Shows strong ORM understanding

---

## 24. Difference Between static, self, and $this in PHP

> **Interview-Ready Answer:**  
> `$this` refers to the current object instance; `self` refers to the exact class where it was defined (early bound at compile time); `static` refers to the class called at runtime (Late Static Binding).

- `$this` → Refers to the current object instance  
- `self::` → Refers to the current class (compile-time binding)  
- `static::` → Refers to called class (late static binding, runtime binding)  

🎯 This is a commonly asked and important OOP question.

---

## 25. How Do You Prevent Race Conditions in PHP and Databases?

> **Interview-Ready Answer:**  
> Prevent race conditions using atomic database queries (`UPDATE accounts SET balance = balance - 50 WHERE id = 1 AND balance >= 50`), database pessimistic row locks (`SELECT ... FOR UPDATE`), or distributed Redis locks.

**✅ Example:**

Two users updating the same wallet balance simultaneously.

**✅ Solutions:**

- Database transactions  
- Row-level locking (`SELECT ... FOR UPDATE`)  
- Optimistic locking  
- Atomic operations  

🎯 Mentioning transactions and locking shows strong backend knowledge.

---

## 26. Difference Between isset() and empty() in PHP

> **Interview-Ready Answer:**  
> `isset()` returns true if the variable exists and is not `null`; `empty()` returns true if the variable does not exist or evaluates to falsy (`""`, `0`, `null`, `false`, `[]`).

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

## 27. How Do You Secure a Laravel API?

> **Interview-Ready Answer:**  
> Secure a Laravel API by using HTTPS, stateless token authentication (Sanctum/Passport), route rate limiting (`throttle:api`), request payload validation, CORS policies, and SQL/XSS prevention via prepared statements and output escaping.

**✅ Best Practices:**

- Use Sanctum or JWT authentication  
- Apply authentication middleware  
- Use rate limiting  
- Validate all inputs  
- Use CSRF protection (for web routes)  
- Hash passwords  
- Always use HTTPS  

🎯 Mentioning rate limiting and HTTPS sounds senior-level.

---

## 28. Difference Between Process and Thread

> **Interview-Ready Answer:**  
> A process is an independent execution unit with its own dedicated memory space allocated by the OS; a thread is a lightweight execution path within a process that shares memory and resources with other threads of that process.

- A **process** is an independent program instance  
- A **thread** is a smaller execution unit inside a process  

**In PHP::**

- PHP is mostly single-threaded per request  
- Each request runs separately  
- Laravel queue workers can run multiple processes

---

## 29. Explain PHP Namespaces and Their Benefits

> **Interview-Ready Answer:**  
> Namespaces solve class naming collisions across libraries and map directly to PSR-4 autoloading directory structures, allowing distinct packages to define identically named classes (like `Controller`) without conflict.

**✅ What is a Namespace?:**

A **namespace** in PHP is used to organize classes and avoid naming conflicts.

If two classes have the same name in different files, a namespace prevents collision.

```php
namespace App\Services;

class PaymentService {}
```

**Use it like::**

```php
use App\Services\PaymentService;

$payment = new PaymentService();
```

**✅ Benefits of Namespaces:**

- Avoid class name conflicts  
- Better project structure  
- Supports PSR-4 autoloading  
- Cleaner large applications  

👉 In Laravel, almost everything is namespaced (Controllers, Models, Services).

---

## 30. What is Output Buffering in PHP?

> **Interview-Ready Answer:**  
> Output buffering (`ob_start()`) instructs PHP to hold generated output in an internal memory buffer rather than immediately sending it to the client, allowing developers to modify HTTP headers or manipulate output before sending.

**✅ Definition:**

Output buffering stores output in memory before sending it to the browser.

**Normally::**

```php
echo "Hello";
```

It directly prints.

**With buffering::**

```php
ob_start();
echo "Hello";
$content = ob_get_clean();
```

Now output is stored in `$content`.

**✅ Why is Output Buffering Used?:**

- Modifying headers after output  
- Improving performance
- Compress output 
- Capture view content  

👉 Laravel internally uses output buffering when rendering views.

---

## 31. What is Late Static Binding in PHP?

> **Interview-Ready Answer:**  
> Late Static Binding allows static method calls in an inherited hierarchy to resolve to the runtime calling class using the `static::` keyword instead of the compile-time declaring class referenced by `self::`.

Allows static methods to refer to the **called class** instead of the parent class using `static::`.
Late Static Binding allows a child class to override static methods properly.

**Example::**

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

**✅ Output::**

```text
Child
```

If we used `self::who()` → the output would be:

```text
Parent
```

**✅ Why Is It Important?:**

- Used in frameworks like Laravel for dynamic static resolution.  
- Enables proper polymorphism with static methods.

---

## 32. Explain All SOLID Principles with PHP Examples

> **Interview-Ready Answer:**  
> SOLID represents 5 architectural design principles: **Single Responsibility**, **Open/Closed** (extend without modifying), **Liskov Substitution** (subtypes substitutable for base types), **Interface Segregation** (focused interfaces), and **Dependency Inversion** (depend on abstractions, not concretions).

Very important for 4+ years experience 🔥

- **S** → Single Responsibility  
- **O** → Open/Closed  
- **L** → Liskov Substitution  
- **I** → Interface Segregation  
- **D** → Dependency Inversion  

**🎯 Real Example:**

A Controller should not handle business logic directly.  
Instead, move business logic into a Service class.

Interviewers prefer practical examples over theory.

---

**S — Single Responsibility Principle (SRP):**

One class should have only **one responsibility**.

**❌ Bad::**
`User` class handles DB + email.

**✅ Good::**
- `UserRepository` handles DB  
- `EmailService` handles email  

---

**O — Open/Closed Principle (OCP):**

Software entities should be:

> Open for extension, closed for modification.

Use interfaces to extend functionality without changing existing code.

---

**L — Liskov Substitution Principle (LSP):**

A child class must work wherever the parent class is expected.

If replacing a parent with a child breaks functionality → LSP is violated.

---

**I — Interface Segregation Principle (ISP):**

Do not force a class to implement unused methods.

**✅ Better Approach::**
Create small, specific interfaces instead of one large interface.

---

**D — Dependency Inversion Principle (DIP):**

Depend on **abstractions**, not concrete classes.

```php
class Order {
    public function __construct(PaymentInterface $payment) {}
}
```

High-level modules should not depend on low-level modules.  
Both should depend on abstractions.

---

## 33. What is Dependency Injection and Why Is It Used?

> **Interview-Ready Answer:**  
> Dependency Injection is a design pattern where an object receives its dependencies from an external source (like a container) rather than instantiating them internally with `new`, decoupling code and enabling easy unit testing.

**✅ Definition:**

Dependency Injection (DI) means providing dependencies from outside instead of creating them inside the class.

---

**❌ Without DI::**

```php
class Order {
    protected $payment;

    public function __construct() {
        $this->payment = new PaymentService();
    }
}
```

**❗ Problems::**

- Tight coupling  
- Hard to test  
- Not flexible  

---

**✅ With DI::**

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

**✅ Benefits of Dependency Injection:**

- Loose coupling  
- Easy testing (mocking)  
- Better maintainability  
- Follows SOLID principles

---

## 34. Difference Between Constructor Injection and Method Injection

> **Interview-Ready Answer:**  
> Constructor injection injects mandatory dependencies required for the entire object lifecycle upon instantiation; Method injection injects dependencies needed only for a specific method execution.

**🔹 Constructor Injection:**

Dependency is injected when the object is created.

```php
public function __construct(PaymentService $payment)
```

✔ Best for required dependencies  
✔ Used in Controllers and Services  

---

**🔹 Method Injection:**

Dependency is injected in a specific method.

```php
public function store(Request $request)
```

✔ Used when dependency is not needed everywhere  
✔ More flexible  

---

**🎯 Interview Line:**

> Constructor injection is preferred for mandatory dependencies, while method injection is better for optional or specific-use dependencies.

---

## 35. Difference Between Composition and Inheritance

> **Interview-Ready Answer:**  
> Inheritance establishes an "is-a" relationship by subclassing, which tightly couples components; Composition establishes a "has-a" relationship by injecting functionality via interfaces, which provides flexibility and loose coupling.

**🔹 Inheritance → "is-a" Relationship:**

```php
class Vehicle {}
class Car extends Vehicle {}
```

Car **is a** Vehicle.

**❗ Problems::**

- Tight coupling  
- Hard to modify parent  

---

**🔹 Composition → "has-a" Relationship:**

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

**🎯 Interview Line:**

> Composition is better than inheritance because it reduces tight coupling and increases flexibility.

---

## 36. What Design Patterns Have You Used in PHP Projects?

> **Interview-Ready Answer:**  
> Commonly used patterns include Repository & Service Pattern (separating business logic from data access), Singleton Pattern (single instance), Factory Pattern (object creation), and Observer Pattern (Laravel event listeners).

For 4+ years Laravel experience, you can say:

**✅ Repository Pattern:**
Separates business logic from database logic.

**✅ Service Pattern:**
Business logic is placed in service classes.

**✅ Singleton Pattern:**
Ensures only one instance (e.g., database connection).

**✅ Factory Pattern:**
Used in Laravel model factories.

**✅ Observer Pattern:**
Used in Laravel Events & Listeners.

**✅ MVC Pattern:**
Laravel follows MVC architecture.

---

**🎯 Strong Interview Answer:**

> “In my projects, I have mainly used Repository, Service, Factory, and Observer patterns to maintain clean architecture and proper separation of concerns.”

---

## 37. Difference Between Jobs, Events, and Queues in Laravel

> **Interview-Ready Answer:**  
> An Event announces that an action took place; a Listener handles the event; a Job is a self-contained unit of work; and a Queue is the message broker pipeline where asynchronous jobs wait to be executed by workers.

**🔹 Job:**

A task that needs to be executed (e.g., sending an email).

```php
SendEmailJob::dispatch();
```

---

**🔹 Event:**

Represents something that happened.

Example:  
`UserRegistered` event.

---

**🔹 Queue:**

A system that processes jobs asynchronously.

Example:  
Redis queue worker.

---

**🎯 Simple Difference:**

| Jobs        | Events               | Queues                          |
|------------|----------------------|----------------------------------|
| Work to do | Something happened   | System that processes jobs       |
| Can be sync/async | Triggers listeners | Runs in background |

---

## 38. How Does Laravel Queue Work Internally?

> **Interview-Ready Answer:**  
> When a job is dispatched with `ShouldQueue`, Laravel serializes the job payload and pushes it into a store (Redis/Database); a worker daemon running `queue:work` continuously polls the store, deserializes the job, and executes its `handle()` method.

**✅ Step-by-Step Internal Flow:**

**1️⃣ Dispatch a Job:**

```php
SendEmailJob::dispatch($user);
```

**2️⃣ Laravel Serializes the Job:**

The job object is serialized before being stored.

**3️⃣ Job is Stored in a Queue Driver:**

It can be stored in:

- `database`
- `redis`
- `sqs`
- `beanstalkd`

**4️⃣ Worker Runs:**

```bash
php artisan queue:work
```

**5️⃣ Worker Process:**

The worker:

- Pulls the job from the queue  
- Unserializes the job  
- Calls the `handle()` method  

**6️⃣ After Execution:**

- ✅ If successful → Job is deleted  
- ❌ If failed → Stored in `failed_jobs` table  

---

**🎯 Important Concepts:**

Laravel uses:

- Service Container  
- Queue Drivers  
- Worker daemon process  

Queues make heavy tasks asynchronous and improve performance.

---

## 39. How Do You Handle Failed Jobs in Laravel?

> **Interview-Ready Answer:**  
> Handle failed jobs by setting automatic retries (`$tries = 3`), configuring backoff delays, logging errors in the `failed_jobs` database table via `failed()`, and running `php artisan queue:retry` to reprocess jobs after resolving the issue.

**✅ 1. Check Failed Jobs:**

```bash
php artisan queue:failed
```

**✅ 2. Retry a Job:**

```bash
php artisan queue:retry 1
```

**✅ 3. Set Retry Attempts in Job:**

```php
public $tries = 3;
```

**✅ 4. Handle Failure Method:**

```php
public function failed(Exception $e) {
    Log::error($e->getMessage());
}
```

---

**🎯 Best Practices:**

- Use proper timeout  
- Set retry attempts  
- Log errors properly  
- Monitor queues in production

---

## 40. What is Route Model Binding in Laravel?

> **Interview-Ready Answer:**  
> Route Model Binding automatically resolves Eloquent model instances directly into route handler signatures based on matching URI parameter identifiers (e.g. `{user}` auto-fetches `User::findOrFail($id)`).

Laravel automatically injects a model instance from a route parameter.

---

**❌ Without Binding:**

```php
Route::get('/user/{id}', function($id) {
    return User::find($id);
});
```

---

**✅ With Route Model Binding:**

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

**✅ Benefits:**

- Clean code  
- Less boilerplate  
- Automatic 404 if not found

---

## 41. How to Process Millions of Records in Laravel Without Crashing Memory?

> **Interview-Ready Answer:**  
> Never use `all()` or `get()` for massive datasets. Process records in batches using `chunk()`, stream records one-by-one with PHP generators using `cursor()`, or dispatch chunked asynchronous background queue jobs.

**❌ Never Do This:**

```php
User::all();
```

This loads everything into memory and can cause memory overflow.

---

**✅ Use `chunk()`:**

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

**✅ Use `cursor()`:**

```php
foreach (User::cursor() as $user) {
    // process
}
```

- Loads one record at a time  
- Very memory efficient  

---

**✅ Use Queues for Heavy Processing:**

- Dispatch jobs  
- Process in the background  

---

**🎯 Interview Line:**

> For millions of records, I use `chunk()`, `cursor()`, and queues to prevent memory overflow and improve performance.

---

## 42. Difference Between chunk() and cursor() in Laravel

> **Interview-Ready Answer:**  
> `chunk(1000)` executes separate paginated SQL queries with `LIMIT` and `OFFSET` into arrays; `cursor()` executes a single SQL query and streams records one by one using a PHP generator, reducing PHP memory to near zero.

| chunk() | cursor() |
|----------|----------|
| Loads data in batches | Loads one record at a time |
| Faster for moderate data | Best for huge datasets |
| Uses multiple queries | Uses generator |
| Slightly higher memory usage | Very low memory usage |

---

**🎯 Simple Answer:**

- Use `chunk()` for batch processing.  
- Use `cursor()` when memory efficiency is critical.

---

## 43. How to Prevent Deadlocks in MySQL?

> **Interview-Ready Answer:**  
> Prevent deadlocks by accessing tables and rows in consistent order across all transactions, keeping transactions short, using row-level indexing for `UPDATE`/`DELETE`, and acquiring only necessary row locks.

**✅ What is a Deadlock?:**

A deadlock happens when:

- Transaction A locks Row 1 and waits for Row 2  
- Transaction B locks Row 2 and waits for Row 1  

Both transactions wait forever → **deadlock occurs**.

---

**✅ How to Prevent Deadlocks?:**

**1️⃣ Always Lock Rows/Tables in the Same Order:**

If updating multiple tables, follow the same sequence everywhere in your application.

---

**2️⃣ Keep Transactions Short:**

Do not write long business logic inside transactions.

```php
DB::transaction(function() {
    // only database queries
});
```

✔ Keep only DB queries inside  
✔ Avoid API calls or heavy processing inside transactions  

---

**3️⃣ Use Proper Indexing:**

Without indexes:

- Full table scans occur  
- More rows get locked  
- Deadlock risk increases  

---

**4️⃣ Retry the Transaction:**

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

**🎯 Interview Line:**

> To prevent deadlocks, I keep transactions small, maintain consistent locking order, use proper indexing, and implement retry logic.

---

## 44. Explain Database Transactions and Isolation Levels

> **Interview-Ready Answer:**  
> Database transactions group queries into an atomic commit/rollback unit. The 4 ANSI SQL isolation levels are **Read Uncommitted** (dirty reads), **Read Committed** (non-repeatable reads), **Repeatable Read** (MySQL InnoDB default), and **Serializable** (strict row locking).

**✅ What is a Transaction?:**

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

**✅ ACID Properties:**

- **Atomicity** – All or nothing  
- **Consistency** – Data remains valid  
- **Isolation** – Transactions don’t interfere  
- **Durability** – Data persists after commit  

---

**✅ Isolation Levels:**

**1️⃣ Read Uncommitted:**
- Can read uncommitted data  
- Allows dirty reads  

**2️⃣ Read Committed:**
- Can only read committed data  

**3️⃣ Repeatable Read (MySQL Default):**
- Same row returns same result within a transaction  

**4️⃣ Serializable:**
- Highest isolation level  
- Slowest but safest  

---

**🎯 Interview Line:**

> MySQL’s default isolation level is **Repeatable Read**.

---

## 45. What is CSRF and How Does Laravel Prevent It?

> **Interview-Ready Answer:**  
> Cross-Site Request Forgery tricks authenticated users into submitting unwanted commands; Laravel prevents it automatically by verifying an encrypted `_token` in session data against every incoming POST, PUT, and DELETE request.

**✅ What is CSRF?:**

CSRF (Cross-Site Request Forgery) is an attack where a malicious site sends a request 
using a logged-in user’s session.

---

**✅ How Laravel Prevents CSRF:**

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

**🎯 Important:**

All `POST`, `PUT`, `DELETE` routes are protected by CSRF middleware by default.

---

## 46. What is XSS and How Do You Prevent It in PHP and Laravel?

> **Interview-Ready Answer:**  
> Cross-Site Scripting (XSS) injects malicious client-side JavaScript into web pages viewed by other users. Prevent it in PHP using `htmlspecialchars()` and in Laravel by rendering dynamic data with Blade double curly braces `{{ $data }}` which auto-escapes HTML entities.

**✅ What is XSS?:**

XSS (Cross-Site Scripting) is when an attacker injects malicious JavaScript into your website.

Example:

```html
<script>alert('Hacked')</script>
```

---

**✅ How Laravel Prevents XSS:**

By default, Blade escapes output:

```blade
{{ $name }}
```

This converts `<script>` into safe HTML entities.

---

**❌ Dangerous::**

```blade
{!! $name !!}
```

This prints raw HTML (not escaped).

---

**✅ Prevention Techniques:**

- Escape output  
- Validate input  
- Use Content Security Policy (CSP) headers  
- Avoid rendering raw HTML  
- Use prepared statements (prevents SQL injection)  

---



- PHP Core  
- Advanced OOP  
- SOLID Principles  
- Design Patterns  
- Laravel Internals  
- Queue System  
- Database Concepts  
- Security Best Practices

---

## 47. How to Debug and Fix a Wallet Balance Race Condition Bug?

> **Interview-Ready Answer:**  
> Prevent concurrent withdrawal double-spending by wrapping the balance deduction inside a database transaction with a pessimistic row lock (`SELECT * FROM wallets WHERE user_id = 1 FOR UPDATE`) or atomic SQL decrement (`UPDATE wallets SET balance = balance - 100 WHERE user_id = 1 AND balance >= 100`).

**✅ Scenario:**

Two users try to withdraw money at the same time.  
Wallet balance becomes negative.

**❓ How Will You Fix It?:**

**✅ Strong Answer:**

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

## 48. How to Diagnose and Fix a Website That Is Suddenly Slow in Production?

> **Interview-Ready Answer:**  
> Diagnose production slowness by checking server CPU/RAM with `htop`, enabling MySQL slow query log, inspecting PHP-FPM status, analyzing APM traces, and adding indexes or Redis caching for slow endpoints.

**✅ Scenario:**

Site was working fine. Now response time is 6–7 seconds.

**❓ What Will You Check First?:**

**✅ Debugging Approach:**

- Check slow queries (enable query log)  
- Check N+1 problem  
- Check missing indexes  
- Check server CPU / RAM usage  
- Check external API delays  
- Check queue backlog  

🎯 They want structured debugging thinking, not guessing.

---

## 49. Why Are Users Getting Logged Out Randomly and How Do You Fix It?

> **Interview-Ready Answer:**  
> Random logouts usually occur due to session file garbage collection on distributed load-balanced servers without sticky sessions, incorrect session lifetime configurations, or session cookie domain mismatches. Fix by centralizing sessions in Redis.

**✅ Possible Reasons:**

- Session lifetime too low  
- Session driver misconfigured  
- Redis/file storage issue  
- Load balancer without sticky sessions  

🎯 Mentioning load balancer session issue = very impressive.

---

## 50. Why Does File Upload Work Locally but Fail on Server?

> **Interview-Ready Answer:**  
> This issue is commonly caused by `php.ini` directives (`upload_max_filesize`, `post_max_size`, `memory_limit`) being too low, missing directory write permissions (`chmod 775 storage`), or Nginx `client_max_body_size` blocking large payloads.

**✅ What to Check?:**

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

## 51. Why Are Emails Not Sending in Production and How Do You Fix It?

> **Interview-Ready Answer:**  
> Troubleshoot by checking if queue workers are running (`queue:work`), verifying SMTP credentials and port 587/465 firewall rules, and reviewing the `failed_jobs` table and `laravel.log` for provider API errors.

**✅ Steps to Debug:**

- Is queue worker running?  
- Check `.env` mail configuration  
- Check `failed_jobs` table  
- Verify SMTP credentials  
- Check firewall blocking mail port  

🎯 Mentioning queue workers = senior-level answer.

---

## 52. How to Troubleshoot an API Suddenly Returning 500 Error?

> **Interview-Ready Answer:**  
> Check `storage/logs/laravel.log` or Nginx error logs immediately to capture the fatal stack trace, verify database connection limits and disk space, and inspect recent deployment git commits.

**✅ How to Debug in Production:**

- Check logs (`storage/logs`)  
- Check recent deployment changes  
- Verify environment variables  
- Enable error logging (not display errors)  
- Rollback deployment if needed  

🎯 Mentioning rollback strategy = impressive.

---

## 53. How to Handle: Payment Deducted but Order Not Created?

> **Interview-Ready Answer:**  
> Handle gateway disconnects by implementing idempotent webhook handlers, storing payment transactions in a ledger with unique gateway transaction IDs, and providing an automated reconciliation cron job to create missing orders.

**✅ Critical Scenario:**

**❓ What Should You Do?:**

- Use DB transaction for order + payment update  
- If one fails → rollback everything  
- Use idempotency key for payment API  
- Log every payment response  

🎯 This checks architectural thinking and data consistency handling.

---

## 54. How to Troubleshoot High CPU Usage on Server?

> **Interview-Ready Answer:**  
> Identify offending processes using `top -c` or `pidstat`, analyze slow database queries via `SHOW FULL PROCESSLIST`, and use `strace` or profiling tools (Blackfire/Xdebug) to detect infinite loops or unindexed full table scans.

**✅ What to Check?:**

- Infinite loops?  
- Heavy cron jobs?  
- Queue workers stuck?  
- Too many requests (DDoS)?  
- Missing caching?  

🎯 Mentioning rate limiting and caching = smart answer.

---

## 55. How to Resolve Data Mismatch Between Two Servers?

> **Interview-Ready Answer:**  
> Check MySQL replication lag with `SHOW SLAVE STATUS`, verify server clock synchronization with NTP, ensure proper database isolation levels, and use checksum tools like `pt-table-checksum` to repair drift.

**✅ Scenario:**

Production has 2 servers behind load balancer.  
Some users see different data.

**✅ Possible Causes:**

- Session not centralized  
- Cache not shared  
- Database replication lag  

🎯 Mention centralized Redis/session storage = advanced answer.

---

## 56. How to Detect and Fix a Memory Leak in a PHP Script?

> **Interview-Ready Answer:**  
> Detect leaks using `memory_get_usage()` inside long-running scripts, clear unbounded arrays and caches, disable Eloquent query logging (`DB::disableQueryLog()`), and process large collections using `cursor()` or chunking.

**✅ Scenario:**

Long-running script keeps increasing memory usage.

**✅ What to Check?:**

- Unset large variables  
- Chunk large queries  
- Avoid loading huge collections  
- Use `cursor()` instead of `get()`  

**Example::**

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
