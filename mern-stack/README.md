# Full Stack Interview Notes (React, Node.js, MongoDB, System Design, SQL)

A quick reference for common backend and frontend interview questions.

---

# 🔹 React

## Q2: How does React reconciliation work?

**Answer:**

React keeps a **Virtual DOM** (a JavaScript object representation of the UI).

When **state or props change**, React:

1. Creates a **new Virtual DOM**
2. Compares the **old Virtual DOM vs new Virtual DOM** using a **diffing algorithm**

Rules used during diffing:

- If the **element type is the same** → React updates the props
- If the **element type is different** → React destroys the old element and creates a new one
- For **lists**, React uses **keys** to identify items efficiently

After diffing → **only minimal changes are applied to the real DOM**.

---

## Q3: How do you optimize re-renders in React?

**Answer:**

Techniques to reduce unnecessary re-renders:

- Use **React.memo** for pure components
- Use **useCallback** and **useMemo** to memoize expensive calculations
- Split large components into **smaller components**
- Use **virtualization** for long lists  
  - `react-window`
  - `react-virtualized`
- Lazy load components using:
  - `React.lazy`
  - `Suspense`
- Avoid **inline functions and objects inside JSX** when possible

---

## Q4: Difference between `useEffect` and `useLayoutEffect`?

| Hook | Execution | Use Case |
|-----|-----|-----|
| **useEffect** | Runs **asynchronously after paint** | Data fetching, subscriptions |
| **useLayoutEffect** | Runs **synchronously before paint** | DOM measurements, animations |

Key idea:

- `useEffect` → **does not block UI**
- `useLayoutEffect` → **blocks UI rendering**

---

## Q5: Why keys in React lists?

Keys help React identify:

- Which items **changed**
- Which items **were added**
- Which items **were removed**

Benefits:

- Prevents unnecessary re-renders
- Allows React to **reuse existing DOM nodes**

Best practice:

- Keys must be **unique**
- Keys must be **stable**
- Avoid using **array index as key**

---

# 🔹 Node.js

## Q6: How does Node.js handle 10,000 concurrent requests with a single thread?

Node.js uses:

- **Event Loop**
- **Non-blocking I/O**

Flow:

1. Requests that involve **I/O operations** (DB, file system, network)
2. These tasks are **delegated to the OS/kernel**
3. Node registers a **callback**
4. When the operation finishes → callback is pushed to the **event loop queue**

Result:

- CPU is **not blocked**
- Node can process **thousands of concurrent connections**

---

## Q7: How does middleware chaining work in Express?

Middleware functions follow this structure:

```js
(req, res, next)