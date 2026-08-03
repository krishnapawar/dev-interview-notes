
### Q: jQuery: Add Text to a Div

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
