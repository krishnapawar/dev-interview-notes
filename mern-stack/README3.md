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