
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
---
#### 🔹 What is Shallow Copy?

A shallow copy creates a new object, but nested objects/arrays are still shared by reference.

**Example:**
```javascript
const user1 = {
  name: "Krishna",
  address: {
    city: "Bhopal"
  }
};

const user2 = { ...user1 }; // Shallow Copy

user2.name = "Rahul";
user2.address.city = "Indore";

console.log(user1);
console.log(user2);

// output
{
  name: "Krishna",
  address: {
    city: "Indore"
  }
}

{
  name: "Rahul",
  address: {
    city: "Indore"
  }
}

const copy = Object.assign({}, obj);
//in array
const copy = [...arr];

const copy = arr.slice();

```

---

### What is Deep Copy?
A deep copy creates a completely independent copy, including all nested objects and arrays.

```javascript
const user1 = {
  name: "Krishna",
  address: {
    city: "Bhopal"
  }
};

const user2 = structuredClone(user1);

user2.address.city = "Indore";

console.log(user1);
console.log(user2);

//Older Method
const copy = JSON.parse(JSON.stringify(obj));


//output
{
  name: "Krishna",
  address: {
    city: "Bhopal"
  }
}

{
  name: "Krishna",
  address: {
    city: "Indore"
  }
}

```
