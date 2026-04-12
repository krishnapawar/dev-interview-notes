### Q:🔹 What is Temporal Dead Zone (TDZ)?
🔹 Interview-Ready Answer

> The Temporal Dead Zone is the time between the declaration and initialization of variables declared with let and const, during which accessing them results in a ReferenceError. It ensures variables are not used before initialization.
### Applies to:
- let ✅
- const ✅

- ❌ Not for var

### Example :
```javascript
console.log(a); // ❌ ReferenceError

let a = 10;
```
### 👉 Here:

- a is hoisted
- But it's in TDZ until initialized

### Example :
```javascript
let a = 5;

{
  console.log(a); // ❌ TDZ
  let a = 10;
}

```
- 👉 Inner a creates a new scope → TDZ starts again

### Example :
```javascript
console.log(a); // undefined
var a = 10;
```
- 👉 var is initialized with undefined, so no TDZ

### 🔹 Why TDZ Exists?
- Prevents using variables before declaration
- Makes code safer and predictable
- Avoids bugs

---
### Q:🔹 What are call, apply, and bind?
- These are methods used to control the value of this inside a function.
-👉 They belong to Function prototype.
>call, apply, and bind are JavaScript methods used to set the value of this. call and apply execute the function immediately, while bind returns a new function. The main difference is that call takes arguments individually, apply takes them as an array, and bind is used for function reuse.

### 🔹 Why we use them?
- In JavaScript, this depends on how a function is called.
- These methods help us manually set this.

### 🔹 Example Setup
```javascript
const user = {
  name: "Krishna"
};

function greet(age) {
  console.log(`Hello ${this.name}, Age: ${age}`);
}
```

### 🔹 1️⃣ call()
✅ Definition

- Calls the function immediately and passes arguments one by one.
```javascript
greet.call(user, 25);
```
Output:
Hello Krishna, Age: 25
### 🔹 2️⃣ apply()
✅ Definition

- Calls the function immediately but arguments are passed as an array.
```javascript
greet.apply(user, [25]);
```
Output:
Hello Krishna, Age: 25
### 🔹 3️⃣ bind()
✅ Definition

- Does NOT call the function immediately.
- It returns a new function with this fixed.
```javascript
const newFunc = greet.bind(user, 25);
newFunc();
```
Output:
Hello Krishna, Age: 25

---


### Q: How does React reconciliation work?

**Answer:**

React keeps a Virtual DOM (a JS object representation of UI).

When state/props change → React creates a new Virtual DOM.

React diffs old vs new Virtual DOM using the diffing algorithm:

- If the element type is same → it updates props.
- If different → it destroys old and creates new.

For lists, React uses keys to identify items efficiently.

After diffing → minimal changes are applied to the real DOM.

---

### Q: How do you optimize re-renders in React?

**Answer:**

- Use React.memo for pure components
- Use useCallback and useMemo to memoize expensive calculations
- Split large components into smaller ones
- Use virtualization for long lists (react-window, react-virtualized)
- Lazy load components (React.lazy, Suspense)
- Avoid inline functions and objects inside JSX where possible

---

### Q: Difference between useEffect and useLayoutEffect?

**Answer:**

- **useEffect** runs asynchronously after paint → doesn’t block UI → best for data fetching, subscriptions.
- **useLayoutEffect** runs synchronously before paint → blocks UI → best for DOM measurements, animations.

---

### Q: Why keys in React lists?

**Answer:**

Keys help React identify which items changed, added, or removed.

Without keys → React may re-render entire list instead of reusing DOM nodes.

Keys must be unique and stable (avoid array index as key).

---

🔹 **Node.js**

### Q: How does Node.js handle 10,000 concurrent requests with a single thread?

**Answer:**

Node.js uses an event loop + non-blocking I/O.

Requests that involve I/O (DB, file, network) → delegated to OS/kernel → Node listens for callback when done.

CPU isn’t blocked → event loop continues processing other requests.

That’s why Node.js can handle thousands of concurrent connections with a single thread.

---

### Q: How does middleware chaining work in Express?

**Answer:**

Middleware are functions with `(req, res, next)`.

Each middleware can:

- Modify req or res.
- Call `next()` → to pass control to next middleware.
- Or end response using `res.send()`.

Order matters → they run top to bottom.

**Example:**

```javascript
app.use((req, res, next) => { console.log("A"); next(); });
app.use((req, res, next) => { console.log("B"); next(); });
app.get("/", (req, res) => res.send("C"));
```

Output: `A → B → C`

---

### Q: Difference between process.nextTick() and setImmediate()?

**Answer:**

- **process.nextTick()** → runs before next event loop phase (higher priority).
- **setImmediate()** → runs at the check phase of the event loop (after I/O events).

---

🔹 **MongoDB**

### Q: When would you use $lookup?

**Answer:**

`$lookup` is used to join collections (similar to SQL joins).

Example: Getting orders with customer details.

```javascript
db.orders.aggregate([
  { 
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer"
    }
  }
]);
```

---

### Q: Difference between findOne() and aggregate().match().limit(1)?

**Answer:**

- **findOne()** → quick, optimized, returns first matching doc.
- **aggregate().match().limit(1)** → heavier, goes through pipeline, more flexible for complex queries (e.g., transformations).

---

### Q: How would you design a schema for an e-commerce cart system?

**Answer:**

**Users collection:**

```json
{ name, email, password }
```

**Products collection:**

```json
{ name, price, stock }
```

**Cart collection:**

```json
{
  userId,
  items: [
    { productId, quantity, price }
  ],
  totalAmount,
  status: "active" | "ordered"
}
```

Cart embedded items for fast access.

---

🔹 **System Design**

### Q: Design a scalable chat app with typing indicators.

**Answer:**

**Frontend:** React + Socket.IO  

**Backend:** Node.js + Express + Socket.IO  

**Database:** MongoDB for storing messages, Redis for caching online users

**Flow:**

- User connects via WebSocket → stored in Redis
- Messages sent → saved in MongoDB, emitted to recipient
- Typing indicators → socket events (`typing`, `stopTyping`)
- For scaling → use Redis pub/sub with multiple Node instances

---

🔹 **SQL**

### Q: SQL vs NoSQL?

**Answer:**

- **SQL** → structured, relational, ACID, best for banking, inventory.
- **NoSQL (MongoDB)** → flexible schema, horizontal scaling, best for fast-changing data, chat apps, analytics.

---

🔹 **Behavioral**

### Q: Biggest challenge in your project?

**Answer (example you can adapt):**

In my last project, we faced performance issues with MongoDB queries as data grew. I analyzed slow queries with `explain()`, created proper indexes, and implemented Redis caching. This reduced API response time from ~1.2s to ~150ms.

🔹 **What is async/await?**

async/await is syntactic sugar built on top of Promises in JavaScript.

It makes asynchronous code look and behave like synchronous code, which is easier to read and maintain.

---

🔹 **Rules**

- A function declared with `async` always returns a Promise.
- Inside an async function, you can use `await` before a Promise.
- `await` makes JavaScript wait until the Promise resolves or rejects.

**Why use async/await?**

- Makes code cleaner than `.then()` chains.
- Easier error handling with `try/catch`.
- Looks like synchronous code but still non-blocking.

👉 **In an interview, a good short answer would be:**

> "async/await is a way to handle asynchronous code in JavaScript. async functions return a Promise, and await pauses execution inside the function until the Promise resolves or rejects. It makes async code look synchronous and is easier to read than `.then()` chains."

---

### Q15: What is Event Delegation?

Event delegation is a technique in JavaScript where you attach a single event listener to a parent element instead of attaching separate listeners to each child element.

The event bubbles up from the child to the parent, and you can detect which child triggered it using `event.target`.

---

🔹 **Why use it?**

- Improves performance (fewer event listeners).
- Handles dynamically added elements (new children don’t need extra listeners).
- Cleaner, maintainable code.

---

### Example: Without Event Delegation

Suppose you have 100 list items, and you want to handle clicks on each:

```html
<ul id="menu">
  <li>Home</li>
  <li>About</li>
  <li>Services</li>
  <li>Contact</li>
</ul>
```

```javascript
// ❌ Bad way: adding listener to each item
document.querySelectorAll("#menu li").forEach(li => {
  li.addEventListener("click", () => {
    console.log("You clicked:", li.innerText);
  });
});
```

👉 **Problem:** If you add new `<li>`, you must reattach listeners.

---

### Example: With Event Delegation ✅

```javascript
document.getElementById("menu").addEventListener("click", function(event) {
  if (event.target.tagName === "LI") {
    console.log("You clicked:", event.target.innerText);
  }
});
```

👉 **Now:**

- Only **1 listener** on `#menu`.
- Works even if you dynamically add new `<li>`.

---

### Event Delegation in React

In React, we don’t usually add event listeners directly to DOM elements. React already uses a **synthetic event system** where events bubble up to the root (`document`).

That means **event delegation is built-in to React.**

But we can still apply the idea of event delegation for efficiency.

---

### Example: Without Event Delegation (React)

```javascript
import React from "react";

export default function Menu() {
  const items = ["Home", "About", "Services", "Contact"];

  return (
    <ul>
      {items.map((item, i) => (
        <li key={i} onClick={() => alert(`Clicked: ${item}`)}>
          {item}
        </li>
      ))}
    </ul>
  );
}
```

👉 **Problem:** Every `<li>` gets its own `onClick` function.  
Not a big deal for small lists, but for **1000 items this is inefficient.**

---

### Example: With Event Delegation (React)

We attach one click handler to the parent `<ul>` and check the clicked child via `event.target`.

```javascript
import React from "react";

export default function Menu() {
  const items = ["Home", "About", "Services", "Contact"];

  const handleClick = (e: React.MouseEvent<HTMLUListElement>) => {
    const target = e.target as HTMLElement;
    if (target.tagName === "LI") {
      alert(`Clicked: ${target.innerText}`);
    }
  };

  return (
    <ul onClick={handleClick}>
      {items.map((item, i) => (
        <li key={i}>{item}</li>
      ))}
    </ul>
  );
}
```

✅ Now only **one handler** is attached.  
✅ Works even if **new items are added dynamically.**

---

### Interview-ready React answer

In React, event delegation is already part of its **synthetic event system** — React attaches a single listener at the root and manages bubbling.

But we can still implement delegation manually: instead of assigning `onClick` to every child, we assign it to a parent (like `<ul>`) and detect which child triggered the event using `event.target`.

This reduces the number of listeners and handles dynamically added children efficiently.

### Q: Common Default Modules in Node.js

#### 1. File System (fs)

Used to work with files (read, write, update, delete).

```javascript
const fs = require("fs");
fs.writeFileSync("test.txt", "Hello Node!");
```

---

#### 2. Path (path)

Provides utilities for working with file and directory paths.

```javascript
const path = require("path");
console.log(path.basename("/home/user/file.txt")); // file.txt
```

---

#### 3. HTTP (http)

Used to create HTTP servers and clients.

```javascript
const http = require("http");

http.createServer((req, res) => {
  res.end("Hello, World!");
}).listen(3000);
```

---

#### 4. HTTPS (https)

For creating secure servers (with SSL certificates).

```javascript
const https = require("https");
```

---

#### 5. URL (url)

Helps parse and format URLs.

```javascript
const url = require("url");

const parsed = url.parse("https://example.com/page?name=krishna");

console.log(parsed.query); // name=krishna
```

---

#### 6. Query String (querystring)

Work with query strings (older, now replaced by URLSearchParams).

```javascript
const qs = require("querystring");

console.log(qs.parse("name=krishna&age=24"));
// { name: 'krishna', age: '24' }
```

---

#### 7. Events (events)

Event-driven programming.

```javascript
const EventEmitter = require("events");

const emitter = new EventEmitter();

emitter.on("greet", () => console.log("Hello Krishna!"));

emitter.emit("greet");
```

---

#### 8. OS (os)

System info (CPU, memory, platform, etc).

```javascript
const os = require("os");

console.log(os.platform(), os.totalmem());
```

---

#### 9. Crypto (crypto)

Encryption, hashing, security.

```javascript
const crypto = require("crypto");

console.log(
  crypto.createHash("sha256").update("hello").digest("hex")
);
```

---

#### 10. Stream (stream)

Handling streams of data (files, network).

```javascript
const fs = require("fs");

const read = fs.createReadStream("file.txt");

read.on("data", chunk => console.log(chunk.toString()));
```

---

#### 11. Buffer (buffer)

Work with binary data.

```javascript
const buf = Buffer.from("Hello");

console.log(buf.toString()); // Hello
```

---

#### 12. Child Process (child_process)

Run other system commands/programs.

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

#### 14. Assert (assert)

Used for testing (throw error if condition fails).

```javascript
const assert = require("assert");

assert.strictEqual(2 + 2, 4);
```

---

✅ **Interview-ready short answer**

Node.js comes with built-in core modules like **fs (file system), http (server), path (file paths), events (event handling), os (system info), crypto (security), stream (data streams), and child_process (running system commands)**.  
These modules don’t require installation — you can require them directly.

---

### Q: Difference Between slice() and splice()

| Feature | slice() | splice() |
|------|------|------|
| Definition | Returns a shallow copy of a portion of an array into a new array | Changes the original array by adding/removing/replacing elements |
| Mutates array? | ❌ No | ✅ Yes |
| Return value | Returns a new array | Returns an array of removed elements |
| Arguments | slice(start, end) | splice(start, deleteCount, item1, item2, ...) |
| Use case | To copy part of an array | To insert, remove, or replace items in array |

---

## Examples

### 1. slice()

```javascript
let arr = [1, 2, 3, 4, 5];

let sliced = arr.slice(1, 4); // from index 1 to 3

console.log(sliced); // [2, 3, 4]
console.log(arr);    // [1, 2, 3, 4, 5] (original not changed)
```

---

### 2. splice()

```javascript
let arr = [1, 2, 3, 4, 5];

// Remove 2 elements starting from index 1
let spliced = arr.splice(1, 2);

console.log(spliced); // [2, 3]
console.log(arr);     // [1, 4, 5] (original changed)

// Insert elements at index 1
arr.splice(1, 0, "a", "b");

console.log(arr); // [1, "a", "b", 4, 5]
```

---

🔹 **Interview-Ready Short Answer**

`slice()` returns a shallow copy of part of an array into a new array and does **not modify the original array**.

`splice()` **changes the original array** by adding, removing, or replacing elements and returns the removed elements.

---

### slice vs substring vs substr

| Method | Parameters | Negative Index | Notes |
|------|------|------|------|
| slice() | (start, end) | ✅ Yes | End is exclusive |
| substring() | (start, end) | ❌ No | Swaps if start > end |
| substr() | (start, length) | ✅ Yes | Deprecated in modern JS |

---

🔹 **Interview-Ready One-Liner**

- `slice(start, end)` extracts using indexes (supports negatives)
- `substring(start, end)` similar but does not support negatives and swaps indexes if start > end
- `substr(start, length)` extracts substring of given length, supports negatives but is **deprecated**

---

### jQuery: Add Text to a Div

In jQuery, you can add (append, prepend, insert) text to an existing `<div>` in a few ways.

---

### 1. Using `.append()`

Adds text at the end of the existing content.

```html
<div id="myDiv">Hello</div>
```

```javascript
$("#myDiv").append(" World!");

// Result:
// <div id="myDiv">Hello World!</div>
```

---

### 2. Using `.prepend()`

Adds text at the beginning.

```javascript
$("#myDiv").prepend("Hi, ");

// Result:
// <div id="myDiv">Hi, Hello World!</div>
```

---

### 3. Using `.html()`

Replaces the whole content.

```javascript
$("#myDiv").html($("#myDiv").html() + " Again!");

// Result:
// <div id="myDiv">Hi, Hello World! Again!</div>
```

---

### 4. Using `.text()`

For plain text only.

```javascript
$("#myDiv").text($("#myDiv").text() + " !!!");

// Result:
// Hello World! !!!
```

---

🔹 **Which one to use?**

- Use `.append()` / `.prepend()` to add content inside the div
- Use `.html()` if you need HTML tags
- Use `.text()` for plain text only

---

👉 **Example with HTML**

```javascript
$("#myDiv").append("<strong> Bold Text</strong>");
```

Result:

```html
<div id="myDiv">Hello <strong> Bold Text</strong></div>
```

---

### Q: Event-Driven Architecture in Node.js

### Definition

Event-driven architecture (EDA) in Node.js means the flow of the program is determined by **events** and **listeners** that react when those events occur.

It’s like saying:

> “Don’t wait, tell me when it’s ready, and I’ll act then.”

---

### How It Works

1. Event Emitter generates an event
2. Event Listener (callback) is registered
3. When the event happens, the listener executes

Node.js uses the **EventEmitter class from the events module**.

Many Node.js components follow this pattern:
- HTTP requests
- Streams
- File system operations
- Database queries

---

### Example

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

🔹 **Short Interview-Ready Answer**

Event-driven architecture in Node.js is a design pattern where the program flow is controlled by events and listeners. Node.js uses the `EventEmitter` class to emit and listen for events, making it highly scalable and efficient for handling asynchronous, non-blocking I/O operations.

---

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

### Q: What is ReactDOM in React?

### Definition

ReactDOM is a package that provides methods to interact with the DOM (Document Object Model) in a web browser.

In short:

ReactDOM is the bridge between React components and the actual HTML page (the browser DOM).

---

### Why It’s Needed

React builds a **virtual DOM** in memory — a lightweight copy of the real DOM.

When something changes, React compares the virtual DOM with the real DOM and updates only what’s necessary (for better performance).

But React itself cannot touch the real browser DOM directly.

That’s where **ReactDOM** comes in.

---

### Example

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(<App />);
```

Here:

- `ReactDOM.createRoot()` → Creates a root container for the React app
- `.render(<App />)` → Tells ReactDOM to render your React component inside the real HTML element

Example HTML element:

```html
<div id="root"></div>
```

Your React UI becomes visible in the browser.

---

### Before React 18 (Old Way)

```javascript
ReactDOM.render(<App />, document.getElementById("root"));
```

---

### After React 18 (New Way)

```javascript
const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(<App />);
```

---

### Q: Difference between setTimeout() and setImmediate() in Node.js

Both are used to execute code asynchronously, but they run in **different phases of the Event Loop**.

---

### 1️⃣ setTimeout()

### Definition

`setTimeout()` schedules a callback to run after a specified delay (in milliseconds).

### Example

```javascript
setTimeout(() => {
  console.log("Timeout executed");
}, 0);
```

Even with `0ms`, it **does not run immediately**.

It runs in the **Timers Phase** of the event loop.

---

### 2️⃣ setImmediate()

### Definition

`setImmediate()` schedules a callback to run in the **Check Phase** of the event loop, after I/O operations.

### Example

```javascript
setImmediate(() => {
  console.log("Immediate executed");
});
```

---

### Short Interview Answer

`setTimeout()` schedules a callback in the **timers phase** after a specified delay, while `setImmediate()` schedules a callback in the **check phase** of the event loop. When used inside an I/O callback, `setImmediate()` usually executes before `setTimeout()`.

---

### Q: Difference between process.nextTick() and setImmediate()

Both schedule asynchronous callbacks, but they run at **different times in the event loop**.

---

### 1️⃣ process.nextTick()

### Definition

`process.nextTick()` executes the callback **immediately after the current operation finishes**, before the event loop continues.

It runs in the **microtask queue** and has higher priority.

### Example

```javascript
console.log("Start");

process.nextTick(() => {
  console.log("NextTick");
});

console.log("End");
```

### Output

```text
Start
End
NextTick
```

---

### 2️⃣ setImmediate()

### Definition

`setImmediate()` executes the callback in the **Check Phase** of the event loop.

### Example

```javascript
console.log("Start");

setImmediate(() => {
  console.log("Immediate");
});

console.log("End");
```

### Output

```text
Start
End
Immediate
```

---

### Example Showing Both

```javascript
console.log("Start");

setImmediate(() => console.log("Immediate"));

process.nextTick(() => console.log("NextTick"));

console.log("End");
```

### Output

```text
Start
End
NextTick
Immediate
```

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

### 🔹 What is a Pure Component in React?

A Pure Component in React is a component that **only re-renders when its props or state actually change**.

It automatically performs a **shallow comparison of props and state** to decide whether the component should update or not.

This helps **improve performance by preventing unnecessary re-renders**.

---

### Q: ✅ Normal Component vs Pure Component

### Normal Component

A normal React component **re-renders every time its parent re-renders**, even if props are the same.

```javascript
import React, { Component } from "react";

class MyComponent extends Component {
  render() {
    console.log("Component Rendered");
    return <h1>{this.props.name}</h1>;
  }
}

```
# Pure Component

A **PureComponent** automatically checks if props/state changed.
```jsx
import React, { PureComponent } from "react";

class MyComponent extends PureComponent {
  render() {
    console.log("Pure Component Rendered");
    return <h1>{this.props.name}</h1>;
  }
}
```

If `name` stays the same → React skips re-rendering.

---

#### 🔹 What is Shallow Comparison?

**PureComponent** checks only top-level values, not deep objects.

**Example:**
```js
const obj1 = { name: "Krishna" }
const obj2 = { name: "Krishna" }
console.log(obj1 === obj2) // false
```

Because objects are compared by **reference**, not value.

---

#### 🔹 Functional Component Alternative

In functional components, we use:

- `React.memo()`

**Example:**
```jsx
const MyComponent = React.memo(({ name }) => {
  console.log("Rendered");
  return <h1>{name}</h1>;
});
```

`React.memo()` works like `PureComponent` for functional components.

---

## 🔹 When to Use Pure Components

**Use when:**

- Component receives same props frequently
- You want to optimize performance
- Avoid unnecessary re-renders

**Example:**

- Dashboard widgets
- Large lists
- Tables

### Q: Difference Between Virtual DOM and Real DOM

---

#### 1️⃣ Real DOM

The **Real DOM** is the actual Document Object Model of the browser that represents the HTML structure of a webpage.

**Example HTML:**
```html
<div>
  <h1>Hello</h1>
</div>
```

Browser creates a **DOM tree** from this HTML.

### ⚠️ Problem with Real DOM

When something changes:

- Browser recalculates layout
- Repaints the page
- Updates the entire DOM tree

> This process is **slow** if many elements exist.

---

## 2️⃣ Virtual DOM

The **Virtual DOM** is a lightweight copy of the Real DOM stored in memory.

React uses it to **optimize updates**.

Instead of updating the browser DOM directly, React:

1. Creates a **Virtual DOM** copy
2. When state changes → creates **new Virtual DOM**
3. Compares old Virtual DOM vs new Virtual DOM (**Diffing**)
4. Updates **only the changed parts** in the Real DOM

### Q: What types of API functions are used in Node.js?

**Answer:**

In Node.js, APIs are usually created using **HTTP methods**.

#### Common API Methods

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

### Q: What is a Module in Node.js?

**Answer:**

A module is a **reusable block of code** that can be exported and imported into other files.

Node.js uses the **CommonJS module system**.

**`math.js`:**
```javascript
exports.add = (a, b) => {
  return a + b;
}
```

**`app.js`:**
```javascript
const math = require("./math");

console.log(math.add(2, 3));
```

**Output:**
```
5
```

---

### Q: Main Disadvantages of Node.js?

**Answer:**

1. **Not good for CPU-heavy tasks** — Node.js is single-threaded, so heavy computation can block the event loop.
   - Examples: Video processing, large data calculation

2. **Callback complexity** — Too many callbacks can create **callback hell**.

3. **Immature libraries** *(earlier issue)* — Some npm packages may be unstable.

4. **Debugging async code** can be difficult.

---

### Q: How to print "Hello" in Node.js without Express?

**Answer:**
```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.write("Hello");
  res.end();
});

server.listen(3000);
```

Open in browser:
```
http://localhost:3000
```

**Output:**
```
Hello
```

---

### Q: What is Buffer in Node.js?

**Answer:**

A **Buffer** is used to handle **binary data** directly in memory.

Node.js uses buffers for:

- File system operations
- Streams
- Network packets

**Example:**
```javascript
const buffer = Buffer.from("Hello");

console.log(buffer);
```

**Output:**
```
<Buffer 48 65 6c 6c 6f>
```

**Convert back to string:**
```javascript
console.log(buffer.toString());
```

**Output:**
```
Hello
```

---

### Q: Difference Between Query Params and Request Params?

**Answer:**

#### Query Params

Values sent in the URL **after `?`**

**Example URL:**
```
/users?id=10&name=krishna
```

**Example:**
```javascript
app.get("/users", (req, res) => {
  console.log(req.query.id);
});
```

**Output:**
```
10
```

---

#### Request Params (Route Params)

Values embedded **inside the URL path**.

**Example URL:**
```
/users/10
```

**Example:**
```javascript
app.get("/users/:id", (req, res) => {
  console.log(req.params.id);
});
```

**Output:**
```
10
```

### Q: Synchronous vs Asynchronous Process in Node.js?

**Answer:**

---

#### 1️⃣ Synchronous Process

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
```
Start
Task 1
Task 2
End
```

Each line waits for the previous one.

---

#### 2️⃣ Asynchronous Process

Asynchronous means tasks run **without blocking** the next task.

Node.js can start a task and move to the next one without waiting. When the task finishes, the **callback function** runs.

**Example:**
```javascript
console.log("Start");

setTimeout(() => {
  console.log("Async Task");
}, 2000);

console.log("End");
```

**Output:**
```
Start
End
Async Task
```
