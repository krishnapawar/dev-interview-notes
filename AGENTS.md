# 🤖 Agent Guidelines & Coding Instructions

# 📖 GitHub Markdown (.md) Format Guide for DevPrep Hub

This guide explains **how to write and format GitHub Markdown (`.md`) files** so that **DevPrep Interview Hub** can properly parse and display questions and answers across all features:

- 📖 **Technical Reader** (Continuous document & Q&A Card mode)
- 🃏 **3D Flip Flashcards** (Front question, back answer)
- ⏱️ **Mock Interview Simulator** (Question queue & scoring)
- 🎙️ **Speech-to-Text Voice Practice** (Spoken response analysis)
- 🧠 **Daily SRS Revision Queue** (Spaced repetition engine)
- 🔍 **Question Explorer** (Search, topic filter, bookmarking & mastering)

---

## 📁 1. File Naming & Folder Organization

The app categorizes notes automatically based on their folder path via `getTopicMetadata(path)` in `src/utils/markdownParser.js`. 

Organize your `.md` files in your repository using standard folder paths:

| File Path in Repo | Auto-Detected Category | Color Accent | Tag |
| :--- | :--- | :--- | :--- |
| `mern-stack/react/README.md` | **Frontend** | `#00d8ff` (Cyan) | `UI Library` |
| `mern-stack/javascript/README.md` | **Frontend** | `#facc15` (Yellow) | `Core Language` |
| `mern-stack/nodejs/README.md` | **Backend** | `#22c55e` (Green) | `Runtime & APIs` |
| `mern-stack/mongodb/README.md` | **Databases** | `#10b981` (Emerald) | `NoSQL Database` |
| `sql/README.md` | **Databases** | `#3b82f6` (Blue) | `Relational DB` |
| `git/README.md` | **DevOps & Tools** | `#f97316` (Orange) | `VCS & Workflow` |
| `websocket/README.md` | **Backend** | `#a855f7` (Purple) | `Real-Time Protocol` |
| `laravel/README.md` | **Backend** | `#ef4444` (Red) | `PHP Framework` |
| `php/README.md` | **Backend** | `#818cf8` (Indigo) | `Server Language` |
| `mern-stack/roadmap/README.md` | **Fullstack** | `#ec4899` (Pink) | `Roadmap & Specs` |

> 💡 *Note: Any folder path not listed above will safely fallback to the `General` category.*

---

## 🏗️ 2. Overall Structure of a Markdown File

Every `.md` file in this repository follows this clean structure:

```markdown
# ⚛️ Topic Name — Interview Questions & Answers

## 📋 Table of Contents
1. [Question Title 1](#anchor-slug-1)
2. [Question Title 2](#anchor-slug-2)

---

### Q1: Question Title 1
Answer content goes here...

---

### Q2: Question Title 2
Answer content goes here...
```

---

## 🎯 3. How the Parser Detects Questions

The parser in `src/utils/markdownParser.js` scans the Markdown file line-by-line.

### Rule 1: Heading Levels
Your question heading **MUST** be level 2, 3, or 4:
- `## Question Heading`
- `### Question Heading` (Recommended)
- `#### Question Heading`

> ⚠️ **Do NOT use Level 1 (`# ...`) for questions!** `#` is reserved for the file title only.

### Rule 2: Question Trigger Keywords
A heading is identified as a question if it contains **at least one** of the following keywords or patterns:

| Pattern / Keyword | Examples |
| :--- | :--- |
| **`?`** (Question mark) | `### Difference between useEffect and useLayoutEffect?` |
| **`What is` / `What are`** | `### What is ReactDOM in React?` |
| **`Difference` / `vs`** | `### Difference Between Virtual DOM and Real DOM` |
| **`How` / `How to`** | `### How does React reconciliation work?` |
| **`Explain`** | `### Explain the Event Loop in Node.js` |
| **`Why`** | `### Why keys are required in React lists?` |
| **`When to`** | `### When to use PureComponent vs React.memo?` |
| **`Which`** | `### Which caching strategy should you use?` |
| **`Definition`** | `### Definition of Closures in JavaScript` |
| **`Overview`** | `### Architecture Overview of Node.js` |
| **Numbering / Emojis** | `## 1. What is Node.js`, `### Q1: ...`, `### 1️⃣ ...`, `### Q: ...` |

### Rule 3: Automatic Prefix Cleaning
The parser automatically removes prefixes so the flashcards and question lists stay clean and readable:
- `### Q: What is React?` ➔ parsed as `"What is React?"`
- `### Q1: What is React?` ➔ parsed as `"What is React?"`
- `### Question 2: What is React?` ➔ parsed as `"What is React?"`
- `### 1️⃣ What is React?` ➔ parsed as `"What is React?"`
- `## 5. What is React?` ➔ parsed as `"What is React?"`
- `### 🔹 What is React?` ➔ parsed as `"What is React?"`

---

## ✍️ 4. How to Write the Answer Content

Everything between one question heading and the next question heading (or end of file) is treated as the **Answer**.

DevPrep Hub supports full **GitHub Flavored Markdown (GFM)**:

### A. Interview-Ready One-Liner (Blockquote)
Always start with a 1-2 sentence high-impact summary inside a blockquote `>`. This gives candidates an instant answer to recite in an interview:

```markdown
> ReactDOM is the package that provides DOM-specific methods, serving as the bridge between React's in-memory Virtual DOM and the real browser DOM.
```

### B. Code Blocks with Language Syntax Highlighting
Always add the language tag (e.g. `javascript`, `jsx`, `html`, `css`, `sql`, `bash`, `json`). Prism.js highlights the code and enables the **one-click copy** button:

````markdown
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
````

### C. Comparison Tables
For "Difference between" questions, use standard Markdown tables:

```markdown
| Feature | `slice()` | `splice()` |
| :--- | :--- | :--- |
| **Mutation** | ❌ Does not mutate original array | ✅ Mutates original array in-place |
| **Return Value** | Returns a new array shallow copy | Returns array of removed elements |
| **Use Case** | Extracting elements safely | Inserting, replacing, or deleting items |
```

### D. Subheadings Inside the Answer
You can use `####` or bold titles inside the answer to organize definitions, examples, and rules:

```markdown
#### 🔹 Why it happens:
1. First reason
2. Second reason

#### 🔹 How to fix it:
- Fix step 1
- Fix step 2
```

### E. Emojis and Visual Bullet Points
Use intuitive emojis for quick scannability:
- `✅` Best practice / do this
- `❌` Anti-pattern / avoid this
- `⚠️` Warning / pitfall
- `👉` Key takeaway
- `🔹` Note or subsection

### F. Separator Line
Add `---` before starting the next question to keep the document visually organized.

---

## 📋 5. Complete Copy-Paste File Template

Save this template as `README.md` inside your topic folder (e.g., `mern-stack/react/README.md`):

````markdown
# ⚛️ React — Interview Questions & Answers

## 📋 Table of Contents
1. [What is ReactDOM in React?](#q1-what-is-reactdom-in-react)
2. [Difference between useEffect and useLayoutEffect?](#q2-difference-between-useeffect-and-uselayouteffect)
3. [How do you optimize re-renders in React?](#q3-how-do-you-optimize-re-renders-in-react)

---

### Q1: What is ReactDOM in React?

> **Interview-Ready Answer:**  
> ReactDOM is the package that provides DOM-specific methods, serving as the bridge between React's in-memory Virtual DOM and the browser's Real DOM.

#### Why It's Needed
React itself is platform-agnostic and only manages the component tree and state. It cannot touch the real browser DOM directly. ReactDOM handles rendering and updating the actual browser DOM elements.

#### Example (React 18+)
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

// Creates a root container and renders the React app into <div id="root"></div>
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

---

### Q2: Difference between useEffect and useLayoutEffect?

> **Interview-Ready Answer:**  
> `useEffect` runs asynchronously **after** the browser paints the screen; `useLayoutEffect` runs synchronously **after** DOM mutations but **before** the browser paints.

| Feature | `useEffect` | `useLayoutEffect` |
| :--- | :--- | :--- |
| **Execution Timing** | Asynchronous, after browser paint | Synchronous, before browser paint |
| **Blocks UI Rendering?** | ❌ No, non-blocking | ⚠️ Yes, blocks painting until finished |
| **Primary Use Cases** | API calls, subscriptions, timers | Measuring DOM size, positioning tooltips |

#### Code Example
```jsx
import { useState, useLayoutEffect, useRef } from 'react';

function Tooltip() {
  const ref = useRef(null);
  const [height, setHeight] = useState(0);

  // Synchronously measure height before the user sees the frame
  useLayoutEffect(() => {
    if (ref.current) {
      setHeight(ref.current.getBoundingClientRect().height);
    }
  }, []);

  return <div ref={ref}>Tooltip Height: {height}px</div>;
}
```

---

### Q3: How do you optimize re-renders in React?

> **Interview-Ready Answer:**  
> Optimize React performance by preventing unnecessary render cycles, keeping component state localized, and memoizing expensive computations.

#### Key Optimization Techniques:
1. **`React.memo`**: Skips re-rendering a child component if its props have not changed (shallow comparison).
2. **`useMemo`**: Caches the **result** of an expensive computation across renders.
3. **`useCallback`**: Caches a **function instance** so child components receiving it as a prop don't re-render.
4. **List Virtualization**: Use libraries like `react-window` for lists with 1,000+ items to only render visible nodes.
5. **State Colocation**: Move state down as close as possible to where it is used so parent re-renders don't trigger subtree re-renders.

#### Example of `React.memo`:
```jsx
import React from 'react';

const UserBadge = React.memo(function UserBadge({ username }) {
  console.log('Renders only when username changes');
  return <span className="badge">{username}</span>;
});
```

---
````

---

## 🌟 6. Four Concrete Question Styles Supported

### Style A: "What is" / Definition Style
```markdown
### Q: What is Temporal Dead Zone (TDZ) in JavaScript?

> The Temporal Dead Zone (TDZ) is the time period between variable binding (hoisting) and its declaration/initialization with `let` or `const`, during which accessing the variable throws a `ReferenceError`.

```javascript
console.log(a); // ❌ ReferenceError: Cannot access 'a' before initialization
let a = 10;
```

- **Applies to:** `let`, `const`
- **Does not apply to:** `var` (initialized with `undefined`)
```

---

### Style B: "Difference between A and B" Style (Table)
```markdown
### Q: Difference Between slice() and splice() in JavaScript

| Feature | `slice()` | `splice()` |
| :--- | :--- | :--- |
| **Definition** | Extracts a section of an array into a new array | Adds, removes, or replaces elements in the original array |
| **Mutates Original?** | ❌ No | ✅ Yes |
| **Return Value** | New array containing extracted elements | Array containing removed elements |

```javascript
const numbers = [1, 2, 3, 4, 5];

// slice: does NOT mutate
const sliced = numbers.slice(1, 3); // [2, 3]
console.log(numbers); // [1, 2, 3, 4, 5]

// splice: DOES mutate
const removed = numbers.splice(1, 2); // [2, 3]
console.log(numbers); // [1, 4, 5]
```
```

---

### Style C: "How does" / Architectural Deep Dive Style
```markdown
### Q: How does Node.js handle 10,000 concurrent requests with a single thread?

> Node.js executes JavaScript on a single thread using the V8 engine, but offloads blocking I/O operations (file system, network, DNS, crypto) to the operating system kernel or the Libuv C++ thread pool.

#### Request Processing Flow:
1. Incoming request arrives at the **Event Queue**.
2. **Event Loop** picks up the request on the main thread.
3. If the request involves I/O (e.g. database query, file read):
   - Offloaded to **Libuv worker pool** or kernel non-blocking socket.
   - The main thread immediately continues handling the next incoming client request.
4. When I/O finishes, Libuv pushes the callback to the **Callback Queue**.
5. Event Loop executes the callback and returns the response to the client.
```

---

### Style D: "Explain" / Practical Scenario Style
```markdown
### Q: Explain how to prevent Memory Leaks in Node.js

> Memory leaks in Node.js occur when objects are referenced unintentionally and cannot be collected by the V8 garbage collector.

#### Common Causes & Fixes:
1. **Unclosed Event Listeners**: Always remove listeners with `.removeListener()` or use `.once()`.
2. **Uncleared Timers**: Always call `clearTimeout()` or `clearInterval()` when tasks finish.
3. **Unbounded Caching**: Plain JavaScript objects or Maps grow indefinitely. Use an LRU cache with an item count limit (e.g. `lru-cache`).
4. **Global Variables**: Accidental global variables stay in memory for the process lifetime. Enable `'use strict';`.
```

---

## 🚫 7. Common Formatting Mistakes to Avoid

| ❌ Mistake | ⚠️ What Breaks | ✅ Correct Fix |
| :--- | :--- | :--- |
| `# What is React?` | `#` (H1) is treated as a document title, not a question | Use `### What is React?` or `## What is React?` |
| `### React Reconciliation` (No question word or `?`) | Parser may skip it if it has no question keywords | Use `### How does React Reconciliation work?` or `### Q: React Reconciliation` |
| Omitting language tag on code block (```` ``` ````) | Syntax highlighting and one-click copy won't work | Always specify language: ```` ```javascript ````, ```` ```sql ```` |
| Missing `---` separator | Questions might visually blend together in reader mode | Add `---` between questions |
| Creating questions with fewer than 4 characters | Parser skips headings with clean text ≤ 3 characters | Ensure question title is descriptive |

---

## 🔄 8. How to Test Your Markdown in the App

1. Place your `.md` file in the repo (or update `src/fallbackNotes.json` if testing offline).
2. Start the dev server:
   ```bash
   npm run dev
   ```
3. Open `http://localhost:5173/dev-interview-hub/`.
4. Check:
   - **Reader Mode**: Does the Table of Contents pick up all questions?
   - **Flashcard Deck**: Do the front (question) and back (answer) render properly?
   - **Question Explorer**: Does search and category filtering find your new question?
