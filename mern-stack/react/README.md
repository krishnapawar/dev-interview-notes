# ⚛️ React — Interview Questions & Answers

## 📋 Table of Contents
1. [What is ReactDOM in React?](#q-what-is-reactdom-in-react)
2. [What is a Pure Component in React? / Normal Component vs Pure Component](#what-is-a-pure-component-in-react)
3. [Difference Between Virtual DOM and Real DOM](#q-difference-between-virtual-dom-and-real-dom)
4. [How does React reconciliation work?](#q3-how-does-react-reconciliation-work)
5. [How do you optimize re-renders in React?](#q4-how-do-you-optimize-re-renders-in-react)
6. [Difference between useEffect and useLayoutEffect?](#q5-difference-between-useeffect-and-uselayouteffect)
7. [Why keys in React lists?](#q6-why-keys-in-react-lists)

---

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

---

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
