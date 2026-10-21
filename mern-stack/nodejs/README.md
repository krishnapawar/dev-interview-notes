# 🟢 Node.js & Express — Interview Questions & Answers

## 📋 Table of Contents

### Core Architecture & Production Concepts
1. [What is Node.js & explain the architecture](#1-what-is-nodejs-explain-the-architecture)
2. [What is blocking & non-blocking](#2-what-is-blocking-non-blocking)
3. [How to manage concurrency (multiple requests) in Node.js](#3-how-to-manage-concurrency-multiple-requests-in-nodejs)
4. [What is a Stream?](#4-what-is-a-stream)
5. [What is the Buffer?](#5-what-is-the-buffer)
6. [What is the Node Cluster?](#6-what-is-the-node-cluster)
7. [What is a Race Condition?](#7-what-is-a-race-condition)
8. [What is Rate Limiting?](#8-what-is-rate-limiting)
9. [What is Dependency Injection?](#9-what-is-dependency-injection)
10. [Write code using a stream to read a file and print the value](#10-write-code-using-a-stream-to-read-a-file-and-print-the-value)
11. [Write code for rate limiting](#11-write-code-for-rate-limiting)
12. [If memory usage increases on a production server, what are the possible causes and how can we fix it?](#12-if-memory-usage-increases-on-a-production-server-what-are-the-possible-causes-and-how-can-we-fix-it)
13. [What is Jenkins?](#13-what-is-jenkins)
14. [What is an S3 Bucket?](#14-what-is-an-s3-bucket)
15. [What is Redis and where do you use it?](#15-what-is-redis-and-where-do-you-use-it)
16. [Explain the JWT authentication process](#16-explain-the-jwt-authentication-process)
17. [How to secure an API on the server?](#17-how-to-secure-an-api-on-the-server)
18. [How to deploy a Node application on the server using EC2](#18-how-to-deploy-a-node-application-on-the-server-using-ec2)
19. [What is a Production Outage?](#19-what-is-a-production-outage)
20. [Session management using Redis](#20-session-management-using-redis)
- [Quick Reference Table](#quick-reference-table)

### In-Depth Event Loop, Concurrency & Core Modules
21. [Difference between setTimeout() and setImmediate() in Node.js](#21-difference-between-settimeout-and-setimmediate-in-nodejs)
22. [Difference between process.nextTick() and setImmediate()](#22-difference-between-processnexttick-and-setimmediate)
23. [Event Loop in Node.js](#23-event-loop-in-nodejs)
24. [How does Node.js handle 10,000 concurrent requests with a single thread?](#24-how-does-nodejs-handle-10000-concurrent-requests-with-a-single-thread)
25. [Difference between Cluster, Worker Threads, and Child Process in Node.js](#25-difference-between-cluster-worker-threads-and-child-process-in-nodejs)
26. [Event-Driven Architecture in Node.js](#26-event-driven-architecture-in-nodejs)
27. [How does middleware chaining work in Express?](#27-how-does-middleware-chaining-work-in-express)
28. [Common Default Modules in Node.js](#28-common-default-modules-in-nodejs)
29. [What types of API functions are used in Node.js?](#29-what-types-of-api-functions-are-used-in-nodejs)
30. [What is a Module in Node.js?](#30-what-is-a-module-in-nodejs)
31. [Main Disadvantages of Node.js?](#31-main-disadvantages-of-nodejs)
32. [How to print "Hello" in Node.js without Express?](#32-how-to-print-hello-in-nodejs-without-express)
33. [Difference Between Query Params and Request Params?](#33-difference-between-query-params-and-request-params)
34. [Synchronous vs Asynchronous Process in Node.js?](#34-synchronous-vs-asynchronous-process-in-nodejs)

---

## 1. What is Node.js & explain the architecture

**Node.js** is an open-source, cross-platform JavaScript runtime built on Google Chrome's **V8 engine**. It lets you run JavaScript outside the browser (on servers), and is designed for building fast, scalable network applications.

**Architecture:**
- **Single-threaded event loop**: Node.js runs your JS code on a single main thread.
- **libuv**: A C++ library that provides the event loop and a thread pool for handling asynchronous, non-blocking I/O operations (file system, DNS, some crypto operations, etc.).
- **V8 Engine**: Compiles and executes JavaScript code.
- **Event Loop**: Continuously checks the call stack and callback/task queues, delegating I/O work to the OS/thread pool and executing callbacks once operations complete.
- **Non-blocking I/O**: Instead of waiting for I/O to finish, Node registers a callback and moves on, making it highly efficient for I/O-heavy workloads.

Flow: `Client Request → Event Queue → Event Loop → (if I/O) Thread Pool / OS → Callback pushed back → Response`

---

## 2. What is blocking & non-blocking

- **Blocking**: Code execution waits (blocks) until an operation (e.g., reading a file synchronously) completes before moving to the next line. This wastes CPU time when I/O is slow.

```js
const fs = require('fs');
const data = fs.readFileSync('file.txt', 'utf8'); // blocks here
console.log(data);
console.log('This runs after file is read');
```

- **Non-blocking**: The operation is initiated, and execution continues immediately. A callback/promise resolves later when the operation finishes.

```js
const fs = require('fs');
fs.readFile('file.txt', 'utf8', (err, data) => {
  console.log(data); // runs later
});
console.log('This runs immediately, before file read completes');
```

Node.js is built around non-blocking, asynchronous I/O to maximize throughput on a single thread.

---

## 3. How to manage concurrency (multiple requests) in Node.js

Even though Node.js is single-threaded for JS execution, it handles concurrency via:

1. **Event Loop + Non-blocking I/O** — I/O operations are offloaded to libuv's thread pool or the OS kernel, so the main thread isn't blocked while waiting.
2. **Asynchronous programming** — callbacks, Promises, `async/await` allow many operations to be "in flight" at once.
3. **Clustering** (`cluster` module) — spawns multiple Node processes (one per CPU core) that share the same server port, effectively using multi-core systems.
4. **Worker Threads** — for CPU-intensive tasks, run JS in parallel threads without blocking the event loop.
5. **Load Balancers / Reverse Proxies** (Nginx, PM2) — distribute incoming requests across multiple Node instances.
6. **Queueing systems** (RabbitMQ, Bull with Redis) — for handling heavy background workloads asynchronously.

---

## 4. What is a Stream?

A **Stream** is an abstract interface in Node.js for working with streaming data — reading or writing data piece by piece (chunks) instead of loading it all into memory at once. Useful for large files, network communication, etc.

**Types of Streams:**
- **Readable** — read data from a source (e.g., `fs.createReadStream`)
- **Writable** — write data to a destination (e.g., `fs.createWriteStream`)
- **Duplex** — both readable and writable (e.g., TCP sockets)
- **Transform** — a duplex stream that can modify data as it passes through (e.g., `zlib.createGzip()`)

Benefits: low memory usage, faster processing (data processed as it arrives), composability via `.pipe()`.

---

## 5. What is the Buffer?

A **Buffer** is a temporary storage area used to handle raw binary data in Node.js. Since JavaScript originally had no way to handle binary data directly, Node introduced the `Buffer` class.

- Buffers represent fixed-length chunks of memory allocated outside the V8 heap.
- Commonly used when dealing with streams, file I/O, TCP streams, and binary protocols.

```js
const buf = Buffer.from('Hello');
console.log(buf);           // <Buffer 48 65 6c 6c 6f>
console.log(buf.toString()); // Hello

const buf2 = Buffer.alloc(10); // creates a 10-byte buffer filled with zeros
```

---

## 6. What is the Node Cluster?

The **`cluster` module** allows you to create multiple child processes (workers) that all share the same server port, letting a Node.js application take advantage of multi-core CPU systems (since a single Node process runs on one core only).

- The **master process** forks multiple **worker processes**.
- Each worker is a separate instance of the Node app with its own event loop.
- Incoming connections are distributed across workers (round-robin on most platforms).

```js
const cluster = require('cluster');
const http = require('http');
const os = require('os');

if (cluster.isMaster) {
  const numCPUs = os.cpus().length;
  for (let i = 0; i < numCPUs; i++) cluster.fork();

  cluster.on('exit', (worker) => {
    console.log(`Worker ${worker.process.pid} died. Restarting...`);
    cluster.fork();
  });
} else {
  http.createServer((req, res) => {
    res.end('Handled by worker ' + process.pid);
  }).listen(3000);
}
```

---

## 7. What is a Race Condition?

A **race condition** occurs when two or more operations access/modify shared data concurrently, and the final result depends on the unpredictable timing/order of execution — leading to inconsistent or incorrect results.

**Example scenario:** Two requests simultaneously read a bank balance, both calculate a new balance based on the same stale value, and both write back — one update overwrites the other, losing data.

```js
// Two concurrent requests both read balance = 100 before either writes back
let balance = 100;

async function withdraw(amount) {
  const current = await getBalanceFromDB(); // both reads happen before either write
  await updateBalanceInDB(current - amount);
}
```

**Prevention:**
- Use database transactions with proper isolation levels
- Use atomic operations (`findOneAndUpdate`, `$inc` in MongoDB)
- Use locks/mutexes (e.g., Redis distributed locks)
- Use queues to serialize critical operations

---

## 8. What is Rate Limiting?

**Rate limiting** restricts how many requests a client (user/IP/API key) can make to a server within a given time window, to prevent abuse, DDoS attacks, and to protect backend resources.

Common algorithms:
- **Fixed Window** — count requests in fixed time blocks (e.g., 100 req/min)
- **Sliding Window** — smoother, tracks requests over a rolling time window
- **Token Bucket** — tokens refill at a fixed rate; each request consumes a token
- **Leaky Bucket** — requests processed at a fixed constant rate, queued otherwise

Typically implemented using Redis (for distributed systems) or in-memory stores (`express-rate-limit`).

---

## 9. What is Dependency Injection?

**Dependency Injection (DI)** is a design pattern where an object's dependencies (services, configs, other objects) are provided ("injected") from the outside rather than being created internally by the object itself.

**Benefits:** loose coupling, easier testing (mock dependencies), better maintainability.

```js
// Without DI — tightly coupled
class UserService {
  constructor() {
    this.db = new MongoDB(); // hard dependency
  }
}

// With DI — loosely coupled
class UserService {
  constructor(db) {
    this.db = db; // injected dependency
  }
}

const db = new MongoDB();
const userService = new UserService(db); // easy to swap/mock db in tests
```

In Node.js, this pattern is used heavily in frameworks like **NestJS**, which has built-in DI containers.

---

## 10. Write code using a stream to read a file and print the value

```js
const fs = require('fs');

const readStream = fs.createReadStream('example.txt', { encoding: 'utf8' });

readStream.on('data', (chunk) => {
  console.log('Chunk received:', chunk);
});

readStream.on('end', () => {
  console.log('File reading completed.');
});

readStream.on('error', (err) => {
  console.error('Error reading file:', err);
});
```

---

## 11. Write code for rate limiting

**Using `express-rate-limit` (simple, in-memory):**

```js
const express = require('express');
const rateLimit = require('express-rate-limit');

const app = express();

const limiter = rateLimit({
  windowMs: 60 * 1000, // 1 minute window
  max: 5,              // limit each IP to 5 requests per window
  message: 'Too many requests, please try again later.'
});

app.use(limiter);

app.get('/', (req, res) => {
  res.send('Welcome!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

**Custom Token Bucket implementation (no library):**

```js
class TokenBucket {
  constructor(capacity, refillRate) {
    this.capacity = capacity;
    this.tokens = capacity;
    this.refillRate = refillRate; // tokens per second
    this.lastRefill = Date.now();
  }

  allowRequest() {
    const now = Date.now();
    const elapsed = (now - this.lastRefill) / 1000;
    this.tokens = Math.min(this.capacity, this.tokens + elapsed * this.refillRate);
    this.lastRefill = now;

    if (this.tokens >= 1) {
      this.tokens -= 1;
      return true;
    }
    return false;
  }
}

const bucket = new TokenBucket(5, 1); // 5 tokens max, refill 1/sec
console.log(bucket.allowRequest()); // true/false
```

---

## 12. If memory usage increases on a production server, what are the possible causes and how can we fix it?

**Possible causes (memory leaks):**
1. **Global variables** unintentionally holding references to large objects.
2. **Unclosed event listeners** — adding listeners repeatedly without removing them.
3. **Closures** retaining references to large objects longer than needed.
4. **Uncleared timers/intervals** (`setInterval` never cleared).
5. **Caching without limits** — unbounded in-memory caches/maps growing indefinitely.
6. **Large unbounded arrays/buffers** accumulating data (e.g., logs, queues).
7. **Detached objects** in closures kept alive by callbacks.
8. **Too many open DB connections / unclosed streams or sockets**.

**How to diagnose:**
- Use `process.memoryUsage()` to monitor heap usage.
- Use tools like **Node --inspect + Chrome DevTools**, **clinic.js**, **heapdump**, or **0x** to profile and take heap snapshots.
- Compare heap snapshots over time to spot growing objects.
- Use APM tools (New Relic, Datadog) for production monitoring.

**How to fix:**
- Remove unused event listeners (`removeListener` / `once` instead of `on`).
- Clear timers (`clearInterval`, `clearTimeout`) when no longer needed.
- Use LRU caches with size limits (e.g., `lru-cache` package) instead of plain objects/Maps.
- Paginate or stream large datasets instead of loading everything in memory.
- Set proper `--max-old-space-size` flags and monitor with process managers (PM2) that can auto-restart on high memory.
- Fix circular references that prevent garbage collection.

---

## 13. What is Jenkins?

**Jenkins** is an open-source automation server used for **CI/CD (Continuous Integration / Continuous Deployment)**. It automates building, testing, and deploying applications.

- Uses **pipelines** (defined via `Jenkinsfile`, either declarative or scripted) to define build/test/deploy stages.
- Supports plugins for integrating with Git, Docker, AWS, Slack, etc.
- Typical Node.js pipeline: pull code from Git → install dependencies (`npm install`) → run tests (`npm test`) → build → deploy (e.g., to EC2, S3, or a container registry).
- Enables automatic triggers on code push, PR merge, or scheduled builds.

---

## 14. What is an S3 Bucket?

**Amazon S3 (Simple Storage Service)** is a scalable object storage service from AWS. An **S3 bucket** is a container (like a top-level folder) used to store objects (files) such as images, videos, backups, static website files, or logs.

**Key features:**
- Virtually unlimited storage, high durability (99.999999999%).
- Access control via IAM policies and bucket policies.
- Can be used to host static websites.
- Commonly used in Node.js apps for storing user uploads (profile pictures, documents) using the AWS SDK (`@aws-sdk/client-s3`).

```js
const { S3Client, PutObjectCommand } = require('@aws-sdk/client-s3');
const s3 = new S3Client({ region: 'us-east-1' });

await s3.send(new PutObjectCommand({
  Bucket: 'my-bucket',
  Key: 'uploads/photo.jpg',
  Body: fileBuffer
}));
```

---

## 15. What is Redis and where do you use it?

**Redis** (Remote Dictionary Server) is an open-source, in-memory key-value data store, known for extreme speed since it operates in RAM.

**Common use cases:**
- **Caching** — cache frequent DB query results to reduce load and latency.
- **Session storage** — store user sessions for fast lookup (especially in distributed systems).
- **Rate limiting** — track request counts per user/IP.
- **Pub/Sub messaging** — real-time messaging between services.
- **Queues** — job queues (e.g., with Bull/BullMQ) for background processing.
- **Leaderboards / counters** — using sorted sets for rankings.
- **Distributed locks** — to prevent race conditions across multiple server instances.

```js
const redis = require('redis');
const client = redis.createClient();

await client.set('user:1:name', 'John');
const name = await client.get('user:1:name');
```

---

## 16. Explain the JWT authentication process

**JWT (JSON Web Token)** is a compact, self-contained token format used for securely transmitting information between parties, commonly used for stateless authentication.

**Structure:** `Header.Payload.Signature`
- **Header** — algorithm & token type (e.g., `HS256`, `JWT`)
- **Payload** — claims/data (e.g., user id, role, expiry)
- **Signature** — created by signing header + payload with a secret key, ensures the token hasn't been tampered with

**Flow:**
1. User logs in with credentials (email/password).
2. Server verifies credentials, then generates a JWT signed with a secret key, containing user info (e.g., `userId`, `role`) and an expiry.
3. Server sends the JWT back to the client.
4. Client stores it (localStorage, cookie) and sends it in the `Authorization: Bearer <token>` header on subsequent requests.
5. Server verifies the token's signature and expiry on each request (middleware) — no need to query the DB or maintain session state (stateless).
6. If valid, the request proceeds; if invalid/expired, a 401 Unauthorized is returned.

```js
const jwt = require('jsonwebtoken');

// Generate token
const token = jwt.sign({ userId: user._id, role: user.role }, process.env.JWT_SECRET, { expiresIn: '1h' });

// Verify token (middleware)
function authMiddleware(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ message: 'No token provided' });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ message: 'Invalid or expired token' });
  }
}
```

---

## 17. How to secure an API on the server?

1. **Use HTTPS** — encrypt data in transit.
2. **Authentication & Authorization** — JWT/OAuth, role-based access control (RBAC).
3. **Input validation & sanitization** — prevent SQL/NoSQL injection, XSS (use libraries like `joi`, `express-validator`).
4. **Rate limiting** — prevent brute force/DDoS (`express-rate-limit`).
5. **Helmet.js** — sets secure HTTP headers automatically.
6. **CORS configuration** — restrict which origins can access the API.
7. **Environment variables** — never hardcode secrets/API keys; use `.env` + secret managers (AWS Secrets Manager).
8. **Data encryption** — hash passwords with `bcrypt`, encrypt sensitive data at rest.
9. **Avoid verbose error messages** — don't leak stack traces/internal details to clients.
10. **API versioning & logging/monitoring** — track suspicious activity.
11. **Use CSRF protection** for cookie-based auth.
12. **Keep dependencies updated** — run `npm audit` regularly to catch vulnerabilities.

---

## 18. How to deploy a Node application on the server using EC2

**High-level steps:**

1. **Launch an EC2 instance** on AWS (choose an AMI like Ubuntu, select instance type, configure security group to allow ports 22, 80, 443, and your app port).
2. **Connect via SSH:**
   ```bash
   ssh -i my-key.pem ubuntu@<EC2-public-ip>
   ```
3. **Install Node.js** (and npm) on the server:
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```
4. **Clone your project** (via Git) or upload files (SCP):
   ```bash
   git clone https://github.com/your-repo/app.git
   cd app
   npm install
   ```
5. **Set environment variables** (`.env` file or export directly).
6. **Use a process manager (PM2)** to keep the app running and auto-restart on crash:
   ```bash
   npm install -g pm2
   pm2 start index.js --name my-app
   pm2 startup
   pm2 save
   ```
7. **Set up Nginx as a reverse proxy** (to serve on port 80/443 and forward to Node's internal port, e.g., 3000):
   ```nginx
   server {
     listen 80;
     server_name yourdomain.com;
     location / {
       proxy_pass http://localhost:3000;
       proxy_http_version 1.1;
       proxy_set_header Upgrade $http_upgrade;
       proxy_set_header Connection 'upgrade';
       proxy_set_header Host $host;
       proxy_cache_bypass $http_upgrade;
     }
   }
   ```
8. **Set up SSL** using Let's Encrypt (`certbot`) for HTTPS.
9. **(Optional) Set up CI/CD** (Jenkins/GitHub Actions) for automated deployment on push.

---

## 19. What is a Production Outage?

A **production outage** is a period during which a live (production) application or service is unavailable or not functioning correctly for its users — resulting in downtime.

**Common causes:**
- Server crashes (unhandled exceptions, memory leaks, high CPU)
- Database failures or connection pool exhaustion
- Deployment errors / bad releases
- Network issues, DNS failures
- Traffic spikes exceeding capacity (no auto-scaling / rate limiting)
- Third-party service failures (payment gateway, cloud provider outage)
- Misconfigurations (wrong env variables, expired SSL certificates)

**Incident response best practices:**
- Monitoring & alerting (Datadog, New Relic, CloudWatch) to detect outages quickly
- Rollback strategy (revert to last stable deployment)
- Post-mortem / Root Cause Analysis (RCA) after resolution
- Health checks & auto-restart (PM2, Kubernetes liveness probes)
- Redundancy & failover (multiple instances, load balancers, multi-AZ deployments)

---

## 20. Session management using Redis

Since Node.js apps are often stateless and horizontally scaled (multiple instances/servers), storing sessions in memory on a single server doesn't work well. **Redis** is commonly used as a fast, shared session store accessible by all server instances.

**Why Redis for sessions:**
- In-memory = very fast reads/writes
- Shared across multiple app instances (works well with load balancers)
- Supports TTL (auto-expiry) for session timeout
- Persistent enough (with AOF/RDB) to survive restarts if configured

**Implementation with Express + `express-session` + `connect-redis`:**

```js
const express = require('express');
const session = require('express-session');
const RedisStore = require('connect-redis').default;
const { createClient } = require('redis');

const app = express();

const redisClient = createClient({ url: 'redis://localhost:6379' });
redisClient.connect();

app.use(session({
  store: new RedisStore({ client: redisClient }),
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    maxAge: 1000 * 60 * 60, // 1 hour
    secure: process.env.NODE_ENV === 'production', // HTTPS only in prod
    httpOnly: true
  }
}));

app.get('/login', (req, res) => {
  req.session.userId = 'user123'; // stored in Redis
  res.send('Logged in');
});

app.get('/profile', (req, res) => {
  if (!req.session.userId) return res.status(401).send('Not logged in');
  res.send(`Welcome user ${req.session.userId}`);
});
```

Each session is stored as a key in Redis (e.g., `sess:<sessionId>`), and the client only holds a signed session ID in a cookie — the actual session data stays server-side in Redis.

---

## Quick Reference Table

| # | Topic | One-line Summary |
|---|-------|-------------------|
| 1 | Node.js Architecture | Single-threaded event loop + libuv thread pool for async I/O |
| 2 | Blocking vs Non-blocking | Sync waits vs async continues immediately |
| 3 | Concurrency | Event loop, clustering, worker threads, load balancing |
| 4 | Stream | Process data in chunks instead of all at once |
| 5 | Buffer | Handles raw binary data outside V8 heap |
| 6 | Cluster | Multiple processes share one port for multi-core use |
| 7 | Race Condition | Unpredictable results from concurrent shared-data access |
| 8 | Rate Limit | Restrict requests per client per time window |
| 9 | Dependency Injection | Pass dependencies in instead of creating them internally |
| 13 | Jenkins | CI/CD automation server |
| 14 | S3 Bucket | AWS object storage container |
| 15 | Redis | In-memory store for caching, sessions, queues, etc. |
| 16 | JWT Auth | Stateless token-based authentication |
| 17 | API Security | HTTPS, auth, validation, rate limiting, Helmet, CORS |
| 18 | EC2 Deployment | Node + PM2 + Nginx reverse proxy + SSL |
| 19 | Production Outage | Live app downtime; needs monitoring & rollback strategy |
| 20 | Session Mgmt (Redis) | Shared, fast session store for scaled apps |

---

## 21. Difference between setTimeout() and setImmediate() in Node.js

Both are used to execute code asynchronously, but they run in **different phases of the Event Loop**.

---

### 1️⃣ setTimeout()

- **Definition:** `setTimeout()` schedules a callback to run after a specified delay (in milliseconds).
- Even with `0ms`, it **does not run immediately**.
- It runs in the **Timers Phase** of the event loop.

**Example:**
```javascript
setTimeout(() => {
  console.log("Timeout executed");
}, 0);
```

---

### 2️⃣ setImmediate()

- **Definition:** `setImmediate()` schedules a callback to run in the **Check Phase** of the event loop, after I/O operations.

**Example:**
```javascript
setImmediate(() => {
  console.log("Immediate executed");
});
```

---

### 🔹 Short Interview Answer

`setTimeout()` schedules a callback in the **timers phase** after a specified delay, while `setImmediate()` schedules a callback in the **check phase** of the event loop. When used inside an I/O callback, `setImmediate()` usually executes before `setTimeout()`.

---

## 22. Difference between process.nextTick() and setImmediate()

Both schedule asynchronous callbacks, but they run at **different times in the event loop**.

---

### 1️⃣ process.nextTick()

- **Definition:** `process.nextTick()` executes the callback **immediately after the current operation finishes**, before the event loop continues.
- It runs in the **microtask queue** and has higher priority.

**Example:**
```javascript
console.log("Start");

process.nextTick(() => {
  console.log("NextTick");
});

console.log("End");
```

**Output:**
```text
Start
End
NextTick
```

---

### 2️⃣ setImmediate()

- **Definition:** `setImmediate()` executes the callback in the **Check Phase** of the event loop.

**Example:**
```javascript
console.log("Start");

setImmediate(() => {
  console.log("Immediate");
});

console.log("End");
```

**Output:**
```text
Start
End
Immediate
```

---

### 3️⃣ Comparison Example (Showing Both)

**Example:**
```javascript
console.log("Start");

setImmediate(() => console.log("Immediate"));

process.nextTick(() => console.log("NextTick"));

console.log("End");
```

**Output:**
```text
Start
End
NextTick
Immediate
```

---

## 23. Event Loop in Node.js

The **Event Loop** is the core mechanism that allows Node.js to handle non-blocking asynchronous operations on a **single thread**.

It constantly checks for callbacks (events, timers, promises, I/O results) and executes them.

---

### 🍽️ Restaurant Analogy

Think of it like a **restaurant manager**:

- Customers place orders (async tasks)
- The manager doesn’t cook
- The manager coordinates tasks
- When food is ready, the manager delivers it

---

### 🔄 Event Loop Phases

#### 1. Timers Phase
Executes callbacks from:
- `setTimeout()`
- `setInterval()`

#### 2. Pending Callbacks
Executes deferred I/O callbacks.

#### 3. Idle / Prepare
Internal Node.js operations.

#### 4. Poll Phase
- Waits for new I/O events
- Executes I/O callbacks (e.g., `fs.readFile`, network requests)

#### 5. Check Phase
Executes:
```javascript
setImmediate()
```

#### 6. Close Callbacks
Handles cleanup:
```javascript
socket.on("close")
```

#### 📌 Microtasks
- `Promises`
- `process.nextTick`

These run **between phases** and have **higher priority**.

---

### 🔹 Short Interview-Ready Answer

The Event Loop in Node.js allows non-blocking execution by continuously checking for pending callbacks, I/O operations, timers, and promises. It processes them in phases while prioritizing microtasks like `process.nextTick()` and Promises. This architecture allows Node.js to efficiently handle thousands of concurrent requests on a single thread.

---

## 24. How does Node.js handle 10,000 concurrent requests with a single thread?

**Short Answer:**  
Node.js handles thousands of concurrent requests using **non-blocking I/O**, the **event loop**, and the **libuv thread pool**, instead of creating a new thread for each request.

---

### 🔍 Step-by-Step Explanation

#### 1️⃣ Single Thread for JavaScript
Node.js runs JavaScript code on **one main thread**.

But it does not execute long operations directly (like file reading, DB queries, network calls).

#### 2️⃣ Non-Blocking I/O
When Node.js encounters an I/O operation such as:
- Database query
- API call
- File read

It **delegates the task** instead of waiting.

**Example:**
```javascript
const fs = require("fs");

fs.readFile("data.txt", (err, data) => {
  console.log(data.toString());
});
```

Node.js **does not wait** for the file to finish reading.

#### 3️⃣ Task Goes to libuv / OS
The operation is handled by:
- **Operating System** → network I/O
- **libuv thread pool** → heavy tasks like file system operations and crypto

Meanwhile, Node.js continues processing other requests.

#### 4️⃣ Callback Queue
When the operation finishes:
The result is placed into the **callback queue**.

#### 5️⃣ Event Loop Executes Callback
The **Event Loop** continuously checks:
- Is the main thread free?
- Are there callbacks waiting?

If yes → it executes them.

---

### 🌊 Example Flow (10,000 Requests)

1. 10,000 users send HTTP requests
2. Node.js receives them
3. If a request needs DB or file data:
   - Task goes to OS or thread pool
4. Node.js immediately processes other requests
5. When data is ready → callback is queued
6. Event Loop executes the callback

**Result:** High concurrency without creating multiple threads.

---

## 25. Difference between Cluster, Worker Threads, and Child Process in Node.js

All three are used to run tasks outside the main thread, but they work differently.

---

### 1️⃣ Cluster Module

- **Purpose:** Used to scale Node.js applications across multiple CPU cores. Node.js normally runs on one CPU core, but **cluster allows multiple Node.js processes to run**. Each process shares the **same server port**.
- **Key Points:**
  - Creates multiple Node.js processes
  - Used for high traffic servers
  - Each process has separate memory

**Example:**
```javascript
const cluster = require("cluster");
const os = require("os");

if (cluster.isMaster) {
  const cpuCount = os.cpus().length;

  for (let i = 0; i < cpuCount; i++) {
    cluster.fork();
  }
} else {
  require("./server");
}
```

---

### 2️⃣ Worker Threads

- **Purpose:** Used for **CPU-intensive JavaScript tasks**. Runs JavaScript code in **parallel threads inside the same process**.
- **Key Points:**
  - Runs parallel JavaScript code
  - Used for heavy calculations
  - Shares memory using `SharedArrayBuffer`
- **Example Tasks:** Image processing, video encoding, AI computations

**Example:**
```javascript
const { Worker } = require("worker_threads");

new Worker(`
  const { parentPort } = require("worker_threads");
  parentPort.postMessage("Task completed");
`, { eval: true })
.on("message", msg => console.log(msg));
```

---

### 3️⃣ Child Process

- **Purpose:** Used to run **external programs or commands**. Node creates a **separate process** and communicates via messaging.
- **Key Points:**
  - Runs system commands
  - Separate process
  - Communication via IPC (Inter-Process Communication)
- **Example Tasks:** Running Python scripts, shell commands, other external programs

**Example:**
```javascript
const { exec } = require("child_process");

exec("ls", (err, stdout) => {
  console.log(stdout);
});
```

---

### 📊 Comparison Table

| Feature | Cluster | Worker Threads | Child Process |
|---|---|---|---|
| **Type** | Multiple Node processes | Multiple threads | Separate process |
| **Use Case** | Scaling servers | CPU-heavy tasks | Running external programs |
| **Memory** | Separate memory | Shared memory possible | Separate memory |
| **Performance** | Good for load balancing | Good for parallel computation | Slower communication |
| **Example** | Web servers | Image processing | Shell commands |

---

### 💡 Simple Way to Remember

| Problem | Solution |
|---|---|
| High traffic API | Cluster |
| Heavy CPU calculation | Worker Threads |
| Run external program | Child Process |

---

### 🔹 Interview-Ready Answer

- **Cluster** is used to scale Node.js applications across multiple CPU cores by creating multiple processes.
- **Worker threads** are used for CPU-intensive JavaScript tasks that run in parallel threads within the same process.
- **Child processes** are used to run external programs or commands in separate processes.

---

## 26. Event-Driven Architecture in Node.js

Event-driven architecture (EDA) in Node.js means the flow of the program is determined by **events** and **listeners** that react when those events occur.

> “Don’t wait, tell me when it’s ready, and I’ll act then.”

---

### ⚙️ How It Works

1. **Event Emitter** generates an event
2. **Event Listener (callback)** is registered
3. When the event happens, the listener executes

Node.js uses the **`EventEmitter` class from the `events` module**.

Many Node.js components follow this pattern:
- HTTP requests
- Streams
- File system operations
- Database queries

**Example:**
```javascript
const EventEmitter = require("events");

// Create event emitter
const myEmitter = new EventEmitter();

// Register listener
myEmitter.on("greet", (name) => {
  console.log(`Hello, ${name}!`);
});

// Emit event
myEmitter.emit("greet", "Krishna");
```

---

### 🔹 Short Interview-Ready Answer

Event-driven architecture in Node.js is a design pattern where the program flow is controlled by events and listeners. Node.js uses the `EventEmitter` class to emit and listen for events, making it highly scalable and efficient for handling asynchronous, non-blocking I/O operations.

---

## 27. How does middleware chaining work in Express?

Middleware are functions with signature `(req, res, next)`.

Each middleware can:
- Modify `req` or `res`.
- Call `next()` to pass control to the next middleware.
- Or end the response cycle using `res.send()`.

Order matters — they execute sequentially from top to bottom.

**Example:**
```javascript
app.use((req, res, next) => { console.log("A"); next(); });
app.use((req, res, next) => { console.log("B"); next(); });
app.get("/", (req, res) => res.send("C"));
```

**Output:** `A → B → C`

---

## 28. Common Default Modules in Node.js

Node.js comes with built-in core modules that do not require separate installation via npm.

### 📦 Core Modules

#### 1. File System (`fs`)

Used to work with files (read, write, update, delete).

```javascript
const fs = require("fs");
fs.writeFileSync("test.txt", "Hello Node!");
```

---

#### 2. Path (`path`)

Provides utilities for working with file and directory paths.

```javascript
const path = require("path");
console.log(path.basename("/home/user/file.txt")); // file.txt
```

---

#### 3. HTTP (`http`)

Used to create HTTP servers and clients.

```javascript
const http = require("http");

http.createServer((req, res) => {
  res.end("Hello, World!");
}).listen(3000);
```

---

#### 4. HTTPS (`https`)

For creating secure servers (with SSL certificates).

```javascript
const https = require("https");
```

---

#### 5. URL (`url`)

Helps parse and format URLs.

```javascript
const url = require("url");

const parsed = url.parse("https://example.com/page?name=krishna");

console.log(parsed.query); // name=krishna
```

---

#### 6. Query String (`querystring`)

Work with query strings (legacy, now largely replaced by `URLSearchParams`).

```javascript
const qs = require("querystring");

console.log(qs.parse("name=krishna&age=24"));
// { name: 'krishna', age: '24' }
```

---

#### 7. Events (`events`)

Event-driven programming using EventEmitter.

```javascript
const EventEmitter = require("events");

const emitter = new EventEmitter();

emitter.on("greet", () => console.log("Hello Krishna!"));

emitter.emit("greet");
```

---

#### 8. OS (`os`)

System info (CPU, memory, platform, uptime).

```javascript
const os = require("os");

console.log(os.platform(), os.totalmem());
```

---

#### 9. Crypto (`crypto`)

Encryption, hashing, security.

```javascript
const crypto = require("crypto");

console.log(
  crypto.createHash("sha256").update("hello").digest("hex")
);
```

---

#### 10. Stream (`stream`)

Handling streams of data (files, network).

```javascript
const fs = require("fs");

const read = fs.createReadStream("file.txt");

read.on("data", chunk => console.log(chunk.toString()));
```

---

#### 11. Buffer (`buffer`)

Work with raw binary data.

```javascript
const buf = Buffer.from("Hello");

console.log(buf.toString()); // Hello
```

---

#### 12. Child Process (`child_process`)

Run external system commands and scripts.

```javascript
const { exec } = require("child_process");

exec("ls", (err, stdout) => console.log(stdout));
```

---

#### 13. Timers

- `setTimeout`
- `setInterval`
- `setImmediate`
- `process.nextTick`

---

#### 14. Assert (`assert`)

Used for testing (throws error if condition fails).

```javascript
const assert = require("assert");

assert.strictEqual(2 + 2, 4);
```

---

### 🔹 Interview-Ready Short Answer

Node.js comes with built-in core modules like **fs (file system), http (server), path (file paths), events (event handling), os (system info), crypto (security), stream (data streams), and child_process (running system commands)**.  
These modules don’t require installation — you can require them directly.

---

## 29. What types of API functions are used in Node.js?

In Node.js, APIs are usually created using standard **HTTP methods**:

### 📋 Common API Methods

| Method | Purpose | Example |
|---|---|---|
| `GET` | Fetch data | Get users |
| `POST` | Create data | Add new user |
| `PUT` | Update full data | Update user |
| `PATCH` | Update partial data | Update user email |
| `DELETE` | Remove data | Delete user |

**Example (Node.js):**
```javascript
const http = require("http");

http.createServer((req, res) => {
  if (req.method === "GET") {
    res.end("Get API Called");
  }
}).listen(3000);
```

---

## 30. What is a Module in Node.js?

A module is a **reusable block of code** that can be exported and imported into other files.

Node.js uses the **CommonJS module system** (`require` / `module.exports`) by default, and also supports **ES Modules** (`import` / `export`).

**`math.js`:**
```javascript
exports.add = (a, b) => {
  return a + b;
};
```

**`app.js`:**
```javascript
const math = require("./math");

console.log(math.add(2, 3));
```

**Output:**
```text
5
```

---

## 31. Main Disadvantages of Node.js?

1. **Not good for CPU-heavy tasks** — Node.js is single-threaded for JS execution, so heavy computation can block the event loop.
   - Examples: Video processing, large data calculation, machine learning

2. **Callback complexity** — Too many nested callbacks can create **callback hell** (mitigated with Promises and `async/await`).

3. **Immature / unmaintained libraries** — Some npm packages may lack proper maintenance, types, or stability.

4. **Debugging async code** — Tracing call stacks across asynchronous operations and event loops can be difficult.

---

## 32. How to print "Hello" in Node.js without Express?

Using Node.js's built-in **`http` module**:

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.write("Hello");
  res.end();
});

server.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

**Open in browser:**
```
http://localhost:3000
```

**Output:**
```text
Hello
```

---

## 33. Difference Between Query Params and Request Params?

### 1️⃣ Query Params

Values sent in the URL **after `?`**, commonly used for filtering, searching, or pagination.

**Example URL:**
```
/users?id=10&name=krishna
```

**Example:**
```javascript
app.get("/users", (req, res) => {
  console.log(req.query.id);   // 10
  console.log(req.query.name); // krishna
});
```

**Output:**
```text
10
```

---

### 2️⃣ Request Params (Route Params)

Values embedded **inside the URL path**, specified with `:paramName` in route patterns.

**Example URL:**
```
/users/10
```

**Example:**
```javascript
app.get("/users/:id", (req, res) => {
  console.log(req.params.id); // 10
});
```

**Output:**
```text
10
```

---

## 34. Synchronous vs Asynchronous Process in Node.js?

---

### 1️⃣ Synchronous Process

Synchronous means tasks are executed **one by one**. The next task waits until the previous task finishes.

> Execution is **blocking**.

**Example:**
```javascript
console.log("Start");
console.log("Task 1");
console.log("Task 2");
console.log("End");
```

**Output:**
```text
Start
Task 1
Task 2
End
```

Each line waits for the previous one.

---

### 2️⃣ Asynchronous Process

Asynchronous means tasks run **without blocking** the next task.

Node.js can start a task and move to the next one without waiting. When the task finishes, the **callback function** runs.

> Execution is **non-blocking**.

**Example:**
```javascript
console.log("Start");

setTimeout(() => {
  console.log("Async Task");
}, 2000);

console.log("End");
```

**Output:**
```text
Start
End
Async Task
```
---
