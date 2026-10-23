# ⚛️ React — Interview Questions & Answers

## 📋 Table of Contents
1. [What is ReactDOM in React?](#1-what-is-reactdom-in-react)
2. [What is a Pure Component in React? (Normal Component vs Pure Component)](#2-what-is-a-pure-component-in-react-normal-component-vs-pure-component)
3. [Difference Between Virtual DOM and Real DOM](#3-difference-between-virtual-dom-and-real-dom)
4. [How does React reconciliation work?](#4-how-does-react-reconciliation-work)
5. [How do you optimize re-renders in React?](#5-how-do-you-optimize-re-renders-in-react)
6. [Difference between useEffect and useLayoutEffect?](#6-difference-between-useeffect-and-uselayouteffect)
7. [Why keys in React lists?](#7-why-keys-in-react-lists)

---

## 1. What is ReactDOM in React?

> **Interview-Ready Answer:**  
> ReactDOM is the package that provides DOM-specific methods, serving as the bridge between React's in-memory Virtual DOM and the browser's Real DOM.

**Definition:**

ReactDOM is a package that provides methods to interact with the DOM (Document Object Model) in a web browser.

In short:

ReactDOM is the bridge between React components and the actual HTML page (the browser DOM).

---

**Why It’s Needed:**

React builds a **virtual DOM** in memory — a lightweight copy of the real DOM.

When something changes, React compares the virtual DOM with the real DOM and updates only what’s necessary (for better performance).

But React itself cannot touch the real browser DOM directly.

That’s where **ReactDOM** comes in.

---

**Example:**

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

**Example HTML element:**

```html
<div id="root"></div>
```

Your React UI becomes visible in the browser.

---

**Before React 18 (Old Way):**

```javascript
ReactDOM.render(<App />, document.getElementById("root"));
```

---

**After React 18 (New Way):**

```javascript
const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(<App />);
```

---


---

## 2. What is a Pure Component in React? (Normal Component vs Pure Component)

> **Interview-Ready Answer:**  
> A Pure Component (or `React.memo` for function components) automatically implements `shouldComponentUpdate` with a shallow comparison of props and state, skipping unnecessary re-renders when inputs remain unchanged.

A Pure Component in React is a component that **only re-renders when its props or state actually change**.

It automatically performs a **shallow comparison of props and state** to decide whether the component should update or not.

This helps **improve performance by preventing unnecessary re-renders**.

---

**🔄 Normal Component vs Pure Component:**

**Normal Component:**

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

**Pure Component:**

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

**What is Shallow Comparison?**

**PureComponent** checks only top-level values, not deep objects.

**Example:**
```javascript
const obj1 = { name: "Krishna" };
const obj2 = { name: "Krishna" };
console.log(obj1 === obj2); // false
```

Because objects are compared by **reference**, not value.

---

**Functional Component Alternative:**

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

**When to Use Pure Components:**

**Use when:**

- Component receives same props frequently
- You want to optimize performance
- Avoid unnecessary re-renders

**Example:**

- Dashboard widgets
- Large lists
- Tables

---


---

## 3. Difference Between Virtual DOM and Real DOM

> **Interview-Ready Answer:**  
> The Real DOM represents the browser's actual UI tree, where direct updates trigger costly reflow and repaint cycles; the Virtual DOM is a lightweight JavaScript representation in memory that React batches and diffs to update the Real DOM with minimum mutations.

**1️⃣ Real DOM:**

The **Real DOM** is the actual Document Object Model of the browser that represents the HTML structure of a webpage.

**Example HTML:**
```html
<div>
  <h1>Hello</h1>
</div>
```

Browser creates a **DOM tree** from this HTML.

**⚠️ Problem with Real DOM:**

When something changes:

- Browser recalculates layout
- Repaints the page
- Updates the entire DOM tree

> This process is **slow** if many elements exist.

---

**2️⃣ Virtual DOM:**

The **Virtual DOM** is a lightweight copy of the Real DOM stored in memory.

React uses it to **optimize updates**.

Instead of updating the browser DOM directly, React:

1. Creates a **Virtual DOM** copy
2. When state changes → creates **new Virtual DOM**
3. Compares old Virtual DOM vs new Virtual DOM (**Diffing**)
4. Updates **only the changed parts** in the Real DOM

---


---

## 4. How does React reconciliation work?

> **Interview-Ready Answer:**  
> React reconciliation is the diffing algorithm by which React compares the new Virtual DOM tree with the previous snapshot, calculates the minimal set of changes (diff), and patches the real browser DOM in the commit phase.

React keeps a Virtual DOM (a JS object representation of UI).

When state/props change → React creates a new Virtual DOM.

React diffs old vs new Virtual DOM using the diffing algorithm:

- If the element type is same → it updates props.
- If different → it destroys old and creates new.

For lists, React uses keys to identify items efficiently.

After diffing → minimal changes are applied to the real DOM.

---


---

## 5. How do you optimize re-renders in React?

> **Interview-Ready Answer:**  
> Optimize React performance by preventing unnecessary render cycles, keeping component state localized, using `React.memo`, caching calculations with `useMemo`, and memoizing callbacks with `useCallback`.

- Use React.memo for pure components
- Use useCallback and useMemo to memoize expensive calculations
- Split large components into smaller ones
- Use virtualization for long lists (react-window, react-virtualized)
- Lazy load components (React.lazy, Suspense)
- Avoid inline functions and objects inside JSX where possible

---


---

## 6. Difference between useEffect and useLayoutEffect?

> **Interview-Ready Answer:**  
> `useEffect` runs asynchronously after the browser paints the screen without blocking the UI; `useLayoutEffect` runs synchronously after DOM mutations but before the paint, blocking screen updates to perform DOM measurements or prevent flicker.

- **useEffect** runs asynchronously after paint → doesn’t block UI → best for data fetching, subscriptions.
- **useLayoutEffect** runs synchronously before paint → blocks UI → best for DOM measurements, animations.

---


---

## 7. Why keys in React lists?

> **Interview-Ready Answer:**  
> Keys provide stable identity to elements across renders, enabling React's reconciliation engine to match items in a list, track additions, removals, and reorders, and avoid re-rendering entire lists or corrupting internal state.

Keys help React identify which items changed, added, or removed.

Without keys → React may re-render entire list instead of reusing DOM nodes.

Keys must be unique and stable (avoid array index as key).

