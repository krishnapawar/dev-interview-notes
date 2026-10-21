# 🤖 Agent Guidelines & Coding Instructions

Welcome to the **Developer Interview Notes** repository (`dev-interview-notes`). This repository serves as a centralized, high-quality, practical revision guide for technical interviews across MERN Stack, WebSocket, Git, SQL, Laravel, and System Design.

All AI coding assistants and contributors must strictly follow these instructions and formatting standards when creating, modifying, or refactoring documentation.

---

## 📌 Core Principles

1. **Clarity & Brevity**: Provide direct, interview-ready answers with real-world examples.
2. **Visual Hierarchy**: Maintain consistent heading sizes (`#`, `##`, `###`, `####`) and formatting across all files.
3. **Structured Q&A Format**: Always keep question headings and answer headings clearly separated.
4. **Reliable Navigation**: Ensure Table of Contents links match the exact headings and GitHub markdown anchor rules.

---

## 📝 Document & Question Formatting Standard

Every technology section or README file must adhere to the following structure:

### 1. Document Title
- Use a single top-level `H1` (`#`) with a relevant technology emoji.
  ```markdown
  # 🟢 Node.js & Express — Interview Questions & Answers
  ```

### 2. Table of Contents (TOC)
- Located near the top under `## 📋 Table of Contents`.
- Organized logically by sub-topics if the list is long.
- Numbering must be sequential (`1.`, `2.`, `3.`) with **no skipped numbers**.
- TOC links must use exact GitHub markdown slugified anchors:
  ```markdown
  1. [What is Node.js & explain the architecture](#1-what-is-nodejs-explain-the-architecture)
  ```

### 3. Separate Question Heading & Answer Heading (MANDATORY)
Every interview question **must** have its Question Heading separated from its Answer Heading:

```markdown
## <Number>. <Question Title>

### 💡 Answer

<Core explanation / direct answer>

<Supporting subsections, analogies, or step-by-step points>

**Example:**
```<language>
<Clean, commented code snippet>
```

### 🎯 Quick Interview Answer (or 🔹 Short Interview-Ready Answer)
<1-2 punchy sentences summarizing the concept for an interviewer>
```

#### Heading Level Hierarchy:
| Element | Markdown Level | Example |
| :--- | :--- | :--- |
| **Document Title** | `H1 (#)` | `# 🟢 Node.js & Express — Interview Questions & Answers` |
| **Major Sections & TOC** | `H2 (##)` | `## 📋 Table of Contents`, `## Quick Reference Table` |
| **Question Heading** | `H2 (##)` | `## 1. What is Node.js & explain the architecture` |
| **Answer Heading** | `H3 (###)` | `### 💡 Answer` |
| **Subsections inside Answer** | `#### (####)` | `#### 1️⃣ setTimeout()`, `#### 🔄 Event Loop Phases` |
| **Inline Metadata / Labels** | **Bold Text** | `**Definition:**`, `**Example:**`, `**Output:**`, `**Key Points:**` |

> ⚠️ **Important:** Never use `###` (H3) or `####` (H4) for single-sentence labels like `### Definition` or `### Example`. Use bold inline labels (`**Definition:**`, `**Example:**`) instead to avoid broken text sizes and visual clutter.

---

## 🔢 Numbering & Consistency Rules

- **Continuous Numbering**: Never skip numbers (e.g., jumping from `16` directly to `18`).
- **No Ambiguous Tags**: Avoid unnumbered or disjointed prefixes like `### Q:`, `### Q8:`, or `### Q17:`. All questions must follow the standard sequential numbering `## <Number>. <Question Title>`.
- **Reference Tables**: If a file contains a summary table (e.g., Quick Reference Table), its topic numbers must exactly match the question numbers in the body.
- **TOC Sync**: Whenever questions are added, removed, or reordered, immediately re-verify and synchronize the Table of Contents numbers and anchors.

---

## 💻 Code Snippet Standards

1. Always specify the language identifier for code fences (`javascript`, `bash`, `sql`, `json`, etc.).
2. Write production-ready, readable code with minimal unnecessary boilerplate.
3. Always include expected outputs where helpful:
   ```markdown
   **Output:**
   ```text
   Hello World
   ```
   ```

---

## 📂 Repository Layout

```
├── AGENTS.md                  # Workspace guidelines & rules (this file)
├── README.md                  # Global index & technology selector
├── hr/
│   └── README.md              # HR round & behavioral interview Q&A
├── mern-stack/
│   ├── README.md              # MERN stack overview & index
│   ├── javascript/README.md   # JavaScript interview Q&A
│   ├── react/README.md        # React interview Q&A
│   ├── nodejs/README.md       # Node.js & Express interview Q&A
│   └── mongodb/README.md      # MongoDB & System Design Q&A
├── websocket/README.md        # WebSockets & Real-time Q&A
├── docker/README.md           # Docker & containerization interview Q&A
├── git/README.md              # Git workflow & interview Q&A
├── sql/README.md              # SQL queries & DB interview Q&A
└── laravel/README.md          # Laravel / PHP interview Q&A
```
