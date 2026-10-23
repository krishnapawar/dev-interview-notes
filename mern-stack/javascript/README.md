# ⚡ JavaScript — Interview Questions & Answers

## 📋 Table of Contents
1. [What is Temporal Dead Zone (TDZ)?](#1-what-is-temporal-dead-zone-tdz)
2. [What are call, apply, and bind?](#2-what-are-call-apply-and-bind)
3. [What is async/await?](#3-what-is-asyncawait)
4. [What is Event Delegation?](#4-what-is-event-delegation)
5. [Difference Between slice() and splice()](#5-difference-between-slice-and-splice)
6. [slice vs substring vs substr](#6-slice-vs-substring-vs-substr)
7. [What is Shallow Copy?](#7-what-is-shallow-copy)
8. [What is Deep Copy?](#8-what-is-deep-copy)
9. [How Do You Add Text to a Div in jQuery?](#9-how-do-you-add-text-to-a-div-in-jquery)

---

## 1. What is Temporal Dead Zone (TDZ)?

> **Interview-Ready Answer:**  
> The Temporal Dead Zone (TDZ) is the time period between variable binding (hoisting) and its initialization with `let` or `const`, during which accessing the variable throws a `ReferenceError`.

**Applies to:**
- let ✅
- const ✅
- ❌ Not for var

**Example:**
```javascript
console.log(a); // ❌ ReferenceError

let a = 10;
```

**Here:**

- a is hoisted
- But it's in TDZ until initialized

**Example:**
```javascript
let a = 5;

{
  console.log(a); // ❌ TDZ
  let a = 10;
}
```
- 👉 Inner a creates a new scope → TDZ starts again

**Example:**
```javascript
console.log(a); // undefined
var a = 10;
```
- 👉 var is initialized with undefined, so no TDZ

**Why TDZ Exists?**
- Prevents using variables before declaration
- Makes code safer and predictable
- Avoids bugs

---


---

## 2. What are call, apply, and bind?

> **Interview-Ready Answer:**  
> `call`, `apply`, and `bind` are `Function.prototype` methods used to set the `this` context: `call()` invokes the function immediately with comma-separated arguments, `apply()` invokes it immediately with arguments as an array, and `bind()` returns a new copy of the function with bound `this` for later execution.

- These are methods used to control the value of this inside a function.
- 👉 They belong to Function prototype.

> call, apply, and bind are JavaScript methods used to set the value of this. call and apply execute the function immediately, while bind returns a new function. The main difference is that call takes arguments individually, apply takes them as an array, and bind is used for function reuse.

**Why we use them?**
- In JavaScript, this depends on how a function is called.
- These methods help us manually set this.

**Example Setup:**
```javascript
const user = {
  name: "Krishna"
};

function greet(age) {
  console.log(`Hello ${this.name}, Age: ${age}`);
}
```

**1️⃣ call():**
**Definition:**

- Calls the function immediately and passes arguments one by one.

```javascript
greet.call(user, 25);
```

**Output:**
```text
Hello Krishna, Age: 25
```

**2️⃣ apply():**
**Definition:**

- Calls the function immediately but arguments are passed as an array.

```javascript
greet.apply(user, [25]);
```

**Output:**
```text
Hello Krishna, Age: 25
```

**3️⃣ bind():**
**Definition:**

- Does NOT call the function immediately.
- It returns a new function with this fixed.

```javascript
const newFunc = greet.bind(user, 25);
newFunc();
```

**Output:**
```text
Hello Krishna, Age: 25
```

---


---

## 3. What is async/await?

> **Interview-Ready Answer:**  
> `async/await` is syntactic sugar built on top of Promises in ES2017. An `async` function always returns a Promise, and `await` pauses execution of the async function until the Promise resolves or rejects, making asynchronous code read synchronously without blocking the Event Loop.

async/await is syntactic sugar built on top of Promises in JavaScript.

It makes asynchronous code look and behave like synchronous code, which is easier to read and maintain.

---

**Rules:**

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


---

## 4. What is Event Delegation?

> **Interview-Ready Answer:**  
> Event Delegation is a pattern where a single event listener is attached to a common parent element rather than multiple child nodes, leveraging DOM event bubbling to catch events on `event.target` for better memory efficiency and dynamic element support.

Event delegation is a technique in JavaScript where you attach a single event listener to a parent element instead of attaching separate listeners to each child element.

The event bubbles up from the child to the parent, and you can detect which child triggered it using `event.target`.

---

**Why use it?**

- Improves performance (fewer event listeners).
- Handles dynamically added elements (new children don’t need extra listeners).
- Cleaner, maintainable code.

---

**Example: Without Event Delegation**

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

**Example: With Event Delegation ✅**

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

**Event Delegation in React:**

In React, we don’t usually add event listeners directly to DOM elements. React already uses a **synthetic event system** where events bubble up to the root (`document`).

That means **event delegation is built-in to React.**

But we can still apply the idea of event delegation for efficiency.

---

**Example: Without Event Delegation (React)**

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

**Example: With Event Delegation (React)**

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

**Interview-ready React answer:**

In React, event delegation is already part of its **synthetic event system** — React attaches a single listener at the root and manages bubbling.

But we can still implement delegation manually: instead of assigning `onClick` to every child, we assign it to a parent (like `<ul>`) and detect which child triggered the event using `event.target`.

This reduces the number of listeners and handles dynamically added children efficiently.

---


---

## 5. Difference Between slice() and splice()

> **Interview-Ready Answer:**  
> `slice()` returns a shallow copy of a portion of an array without modifying the original array, whereas `splice()` modifies the original array in place by adding, removing, or replacing elements.

| Feature | slice() | splice() |
|------|------|------|
| Definition | Returns a shallow copy of a portion of an array into a new array | Changes the original array by adding/removing/replacing elements |
| Mutates array? | ❌ No | ✅ Yes |
| Return value | Returns a new array | Returns an array of removed elements |
| Arguments | slice(start, end) | splice(start, deleteCount, item1, item2, ...) |
| Use case | To copy part of an array | To insert, remove, or replace items in array |

---

**Examples:**

**1. slice()**

```javascript
let arr = [1, 2, 3, 4, 5];

let sliced = arr.slice(1, 4); // from index 1 to 3

console.log(sliced); // [2, 3, 4]
console.log(arr);    // [1, 2, 3, 4, 5] (original not changed)
```

---

**2. splice()**

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

**Interview-Ready Short Answer:**

`slice()` returns a shallow copy of part of an array into a new array and does **not modify the original array**.

`splice()` **changes the original array** by adding, removing, or replacing elements and returns the removed elements.

---


---

## 6. slice vs substring vs substr

> **Interview-Ready Answer:**  
> `slice(start, end)` extracts a substring and accepts negative indices from the end; `substring(start, end)` treats negative values as 0 and swaps arguments if start > end; `substr(start, length)` extracts characters by count and is deprecated in modern ECMAScript.

| Method | Parameters | Negative Index | Notes |
|------|------|------|------|
| slice() | (start, end) | ✅ Yes | End is exclusive |
| substring() | (start, end) | ❌ No | Swaps if start > end |
| substr() | (start, length) | ✅ Yes | Deprecated in modern JS |

---

**Interview-Ready One-Liner:**

- `slice(start, end)` extracts using indexes (supports negatives)
- `substring(start, end)` similar but does not support negatives and swaps indexes if start > end
- `substr(start, length)` extracts substring of given length, supports negatives but is **deprecated**

---


---

## 7. What is Shallow Copy?

> **Interview-Ready Answer:**  
> A shallow copy duplicates top-level primitive properties into a new object, but any nested objects or arrays are copied by reference—meaning mutating a nested property in the clone also mutates the original.

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


---

## 8. What is Deep Copy?

> **Interview-Ready Answer:**  
> A deep copy duplicates all properties and recursively copies all nested objects and arrays, creating a completely isolated clone in memory with zero shared references using `structuredClone()` or `JSON.parse(JSON.stringify())`.

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

---


---

## 9. How Do You Add Text to a Div in jQuery?

> **Interview-Ready Answer:**  
> In jQuery, use `$('#myDiv').text('New Text')` to set or replace the text content, `$('#myDiv').append(' Extra Text')` to insert text at the end, or `$('#myDiv').html('<b>Bold</b>')` if inserting formatted HTML markup.

In jQuery, you can add (append, prepend, insert) text to an existing `<div>` in a few ways.

---

**1. Using `.append()`**

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

**2. Using `.prepend()`**

Adds text at the beginning.

```javascript
$("#myDiv").prepend("Hi, ");

// Result:
// <div id="myDiv">Hi, Hello World!</div>
```

---

**3. Using `.html()`**

Replaces the whole content.

```javascript
$("#myDiv").html($("#myDiv").html() + " Again!");

// Result:
// <div id="myDiv">Hi, Hello World! Again!</div>
```

---

**4. Using `.text()`**

For plain text only.

```javascript
$("#myDiv").text($("#myDiv").text() + " !!!");

// Result:
// Hello World! !!!
```

---

**Which one to use?**

- Use `.append()` / `.prepend()` to add content inside the div
- Use `.html()` if you need HTML tags
- Use `.text()` for plain text only

---

**Example with HTML:**

```javascript
$("#myDiv").append("<strong> Bold Text</strong>");
```

**Result:**

```html
<div id="myDiv">Hello <strong> Bold Text</strong></div>
```

