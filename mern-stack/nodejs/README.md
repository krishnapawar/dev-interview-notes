# Node JS — MERN Interview Questions & Answers

A prep sheet covering common Node.js interview questions (Accenture 1st Round style), with explanations and code examples.

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

## 18. How to secure an API on the server?

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

## 19. How to deploy a Node application on the server using EC2

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

## 20. What is a Production Outage?

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

## 21. Session management using Redis

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
| 18 | API Security | HTTPS, auth, validation, rate limiting, Helmet, CORS |
| 19 | EC2 Deployment | Node + PM2 + Nginx reverse proxy + SSL |
| 20 | Production Outage | Live app downtime; needs monitoring & rollback strategy |
| 21 | Session Mgmt (Redis) | Shared, fast session store for scaled apps |
