### Q: Event Loop in Node.js

### Definition

The **Event Loop** is the core mechanism that allows Node.js to handle non-blocking asynchronous operations on a **single thread**.

It constantly checks for callbacks (events, timers, promises, I/O results) and executes them.

---

### Restaurant Analogy

Think of it like a **restaurant manager**:

- Customers place orders (async tasks)
- The manager doesn’t cook
- The manager coordinates tasks
- When food is ready, the manager delivers it

---

### Event Loop Phases

### 1. Timers Phase
Executes callbacks from:

- `setTimeout()`
- `setInterval()`

---

### 2. Pending Callbacks
Executes deferred I/O callbacks.

---

### 3. Idle / Prepare
Internal Node.js operations.

---

### 4. Poll Phase

- Waits for new I/O events
- Executes I/O callbacks

Example:

- `fs.readFile`
- Network requests

---

### 5. Check Phase

Executes:

```javascript
setImmediate()
```

---

### 6. Close Callbacks

Handles cleanup:

```javascript
socket.on("close")
```

---

📌 **Microtasks**

- `Promises`
- `process.nextTick`

These run **between phases** and have **higher priority**.

---

🔹 **Short Interview-Ready Answer**

The Event Loop in Node.js allows non-blocking execution by continuously checking for pending callbacks, I/O operations, timers, and promises. It processes them in phases while prioritizing microtasks like `process.nextTick()` and Promises. This architecture allows Node.js to efficiently handle thousands of concurrent requests on a single thread.

---

### Q: How does Node.js handle 10,000 concurrent requests with a single thread?

### Short Answer

Node.js handles thousands of concurrent requests using **non-blocking I/O**, the **event loop**, and the **libuv thread pool**, instead of creating a new thread for each request.

---

### Step-by-Step Explanation

### 1️⃣ Single Thread for JavaScript

Node.js runs JavaScript code on **one main thread**.

But it does not execute long operations directly (like file reading, DB queries, network calls).

---

### 2️⃣ Non-Blocking I/O

When Node.js encounters an I/O operation such as:

- Database query
- API call
- File read

It **delegates the task** instead of waiting.

### Example

```javascript
const fs = require("fs");

fs.readFile("data.txt", (err, data) => {
  console.log(data.toString());
});
```

Node.js **does not wait** for the file to finish reading.

---

### 3️⃣ Task Goes to libuv / OS

The operation is handled by:

- **Operating System** → network I/O
- **libuv thread pool** → heavy tasks like file system operations and crypto

Meanwhile, Node.js continues processing other requests.

---

### 4️⃣ Callback Queue

When the operation finishes:

The result is placed into the **callback queue**.

---

### 5️⃣ Event Loop Executes Callback

The **Event Loop** continuously checks:

- Is the main thread free?
- Are there callbacks waiting?

If yes → it executes them.

---

### Example Flow (10,000 Requests)

1. 10,000 users send HTTP requests
2. Node.js receives them
3. If a request needs DB or file data:
   - Task goes to OS or thread pool
4. Node.js immediately processes other requests
5. When data is ready → callback is queued
6. Event Loop executes the callback

Result:

High concurrency without creating multiple threads.

### Q: Difference between Cluster, Worker Threads, and Child Process in Node.js

All three are used to run tasks outside the main thread, but they work differently.

---

### 1️⃣ Cluster Module

### Purpose

Used to scale Node.js applications across multiple CPU cores.

Node.js normally runs on one CPU core, but **cluster allows multiple Node.js processes to run**.

Each process shares the **same server port**.

---

### Example

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

### Key Points

- Creates multiple Node.js processes
- Used for high traffic servers
- Each process has separate memory

---

### 2️⃣ Worker Threads

### Purpose

Used for **CPU-intensive JavaScript tasks**.

Runs JavaScript code in **parallel threads inside the same process**.

---

### Example

```javascript
const { Worker } = require("worker_threads");

new Worker(`
  const { parentPort } = require("worker_threads");
  parentPort.postMessage("Task completed");
`, { eval: true })
.on("message", msg => console.log(msg));
```

---

### Key Points

- Runs parallel JavaScript code
- Used for heavy calculations
- Shares memory using `SharedArrayBuffer`

### Example Tasks

- Image processing
- Video encoding
- AI computations

---

### 3️⃣ Child Process

### Purpose

Used to run **external programs or commands**.

Node creates a **separate process** and communicates via messaging.

---

### Example

```javascript
const { exec } = require("child_process");

exec("ls", (err, stdout) => {
  console.log(stdout);
});
```

---

### Key Points

- Runs system commands
- Separate process
- Communication via IPC (Inter-Process Communication)

### Example Tasks

- Running Python scripts
- Running shell commands
- Running other programs

---

### Comparison Table

| Feature | Cluster | Worker Threads | Child Process |
|------|------|------|------|
| Type | Multiple Node processes | Multiple threads | Separate process |
| Use Case | Scaling servers | CPU-heavy tasks | Running external programs |
| Memory | Separate memory | Shared memory possible | Separate memory |
| Performance | Good for load balancing | Good for parallel computation | Slower communication |
| Example | Web servers | Image processing | Shell commands |

---

### Simple Way to Remember

| Problem | Solution |
|------|------|
| High traffic API | Cluster |
| Heavy CPU calculation | Worker Threads |
| Run external program | Child Process |

---

### Interview Ready Answer

Cluster is used to scale Node.js applications across multiple CPU cores by creating multiple processes.

Worker threads are used for CPU-intensive JavaScript tasks that run in parallel threads within the same process.

Child processes are used to run external programs or commands in separate processes.