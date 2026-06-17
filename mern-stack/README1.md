### Q1:🔹 What is Temporal Dead Zone (TDZ)?
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
### Q2:🔹 What are call, apply, and bind?
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


### Q3: How does React reconciliation work?

**Answer:**

React keeps a Virtual DOM (a JS object representation of UI).

When state/props change → React creates a new Virtual DOM.

React diffs old vs new Virtual DOM using the diffing algorithm:

- If the element type is same → it updates props.
- If different → it destroys old and creates new.

For lists, React uses keys to identify items efficiently.

After diffing → minimal changes are applied to the real DOM.

---

### Q4: How do you optimize re-renders in React?

**Answer:**

- Use React.memo for pure components
- Use useCallback and useMemo to memoize expensive calculations
- Split large components into smaller ones
- Use virtualization for long lists (react-window, react-virtualized)
- Lazy load components (React.lazy, Suspense)
- Avoid inline functions and objects inside JSX where possible

---

### Q5: Difference between useEffect and useLayoutEffect?

**Answer:**

- **useEffect** runs asynchronously after paint → doesn’t block UI → best for data fetching, subscriptions.
- **useLayoutEffect** runs synchronously before paint → blocks UI → best for DOM measurements, animations.

---

### Q6: Why keys in React lists?

**Answer:**

Keys help React identify which items changed, added, or removed.

Without keys → React may re-render entire list instead of reusing DOM nodes.

Keys must be unique and stable (avoid array index as key).

---

🔹 **Node.js**

### Q7: How does Node.js handle 10,000 concurrent requests with a single thread?

**Answer:**

Node.js uses an event loop + non-blocking I/O.

Requests that involve I/O (DB, file, network) → delegated to OS/kernel → Node listens for callback when done.

CPU isn’t blocked → event loop continues processing other requests.

That’s why Node.js can handle thousands of concurrent connections with a single thread.

---

### Q8: How does middleware chaining work in Express?

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

### Q9: Difference between process.nextTick() and setImmediate()?

**Answer:**

- **process.nextTick()** → runs before next event loop phase (higher priority).
- **setImmediate()** → runs at the check phase of the event loop (after I/O events).

---

🔹 **MongoDB**

### Q10: When would you use $lookup?

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

### Q11: Difference between findOne() and aggregate().match().limit(1)?

**Answer:**

- **findOne()** → quick, optimized, returns first matching doc.
- **aggregate().match().limit(1)** → heavier, goes through pipeline, more flexible for complex queries (e.g., transformations).

---

### Q12: How would you design a schema for an e-commerce cart system?

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

### Q13: Design a scalable chat app with typing indicators.

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

### Q14: SQL vs NoSQL?

**Answer:**

- **SQL** → structured, relational, ACID, best for banking, inventory.
- **NoSQL (MongoDB)** → flexible schema, horizontal scaling, best for fast-changing data, chat apps, analytics.

---

🔹 **Behavioral**

### Q15: Biggest challenge in your project?

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

### Q16: What is Event Delegation?

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

### Q17: Common Default Modules in Node.js

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

### Q18: Difference Between slice() and splice()

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

### O19: slice vs substring vs substr

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