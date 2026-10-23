# 🔴 Laravel — Interview Questions & Answers

## 📋 Table of Contents
1. [What Is Service Container?](#1-what-is-service-container)
2. [What Is Middleware?](#2-what-is-middleware)
3. [Difference Between Eager and Lazy Loading](#3-difference-between-eager-and-lazy-loading)
4. [What Is CSRF?](#4-what-is-csrf)
5. [What Is Queue in Laravel?](#5-what-is-queue-in-laravel)

---

## 1. What Is Service Container?

> **Interview-Ready Answer:**  
> The Laravel Service Container is an Inversion of Control (IoC) dependency injection container that automatically binds, resolves, and injects class dependencies throughout the application via PHP type-hinting or registered service providers.

#### Core Concept
Instead of instantiating classes manually using `new`, you register them into the container. When a controller or method needs a class, Laravel's reflection API inspects the constructor and injects the resolved instance automatically.

#### Code Example:
```php
namespace App\Http\Controllers;

use App\Services\PaymentService;

class OrderController extends Controller
{
    // PaymentService is automatically resolved and injected by the Service Container
    public function __construct(protected PaymentService $paymentService) {}

    public function checkout()
    {
        return $this->paymentService->process();
    }
}
```

---

## 2. What Is Middleware?

> **Interview-Ready Answer:**  
> Middleware acts as an HTTP request pipeline filter sitting between the incoming client request and the route controller, executing tasks such as authentication (`auth`), CSRF verification, rate limiting, and CORS handling.

#### Common Use Cases:
- **Authentication:** Ensuring the user is logged in before allowing access (`auth`).
- **Role Verification:** Restricting admin routes to users with `role === 'admin'`.
- **Request Modification:** Sanitizing input data or trimming strings.

#### Code Example:
```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

class EnsureUserIsAdmin
{
    public function handle(Request $request, Closure $next): Response
    {
        if (!$request->user() || !$request->user()->is_admin) {
            abort(403, 'Unauthorized access.');
        }

        return $next($request);
    }
}
```

---

## 3. Difference Between Eager and Lazy Loading

> **Interview-Ready Answer:**  
> Lazy loading loads related Eloquent models on-demand when accessed via property, leading to the N+1 query problem; Eager loading loads all related records upfront in a single secondary SQL query using `with()`, reducing database queries from N+1 to 2.

| Feature | Lazy Loading | Eager Loading |
| :--- | :--- | :--- |
| **How it loads** | Queries database on-demand per model | Preloads relationships using `with()` |
| **Database Queries** | `N + 1` queries (1 for parent + N for children) | Exactly `2` queries regardless of item count |
| **Performance** | ⚠️ Can crash databases on large datasets | ✅ Highly optimized for high-traffic APIs |
| **Syntax** | `$posts = Post::all();` then `$post->author` | `$posts = Post::with('author')->get();` |

#### Comparison Example:
```php
// ❌ Lazy Loading (triggers 101 queries for 100 books)
$books = Book::all();
foreach ($books as $book) {
    echo $book->author->name; // Separate query per loop iteration!
}

// ✅ Eager Loading (triggers exactly 2 queries)
$books = Book::with('author')->get();
foreach ($books as $book) {
    echo $book->author->name; // Zero additional queries
}
```

---

## 4. What Is CSRF?

> **Interview-Ready Answer:**  
> Cross-Site Request Forgery (CSRF) is an attack that forces authenticated users to execute unwanted actions. Laravel protects against CSRF automatically by generating a unique session-bound token embedded in POST forms with `@csrf` and verified via `VerifyCsrfToken` middleware.

**How Laravel Prevents CSRF:**
1. For every active user session, Laravel generates a cryptographically secure token.
2. Blade forms include this token using the `@csrf` directive.
3. When the form submits, the `ValidateCsrfToken` middleware verifies the submitted `_token` matches the session token.

#### Form Example:
```blade
<form action="/profile/update" method="POST">
    @csrf
    <input type="text" name="name" value="{{ auth()->user()->name }}" />
    <button type="submit">Update Profile</button>
</form>
```

---

## 5. What Is Queue in Laravel?

> **Interview-Ready Answer:**  
> Laravel Queues allow time-consuming tasks (sending emails, processing image uploads, third-party API webhooks) to be dispatched asynchronously to background worker processes (backed by Redis, database, or Amazon SQS), keeping HTTP responses fast.

#### Request Flow with Queues:
1. User clicks "Register".
2. Controller creates user record in DB.
3. Controller dispatches `SendWelcomeEmailJob::dispatch($user)`.
4. Controller immediately returns HTTP 201 JSON response to the user (~50ms).
5. Background worker (`php artisan queue:work`) picks up the job and sends the email via SMTP in the background.

#### Code Example:
```php
namespace App\Jobs;

use App\Models\User;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;
use Illuminate\Support\Facades\Mail;

class SendWelcomeEmail implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public function __construct(public User $user) {}

    public function handle(): void
    {
        Mail::to($this->user->email)->send(new \App\Mail\WelcomeMail($this->user));
    }
}
```
