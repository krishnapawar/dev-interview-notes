## 🔥 When to use git fetch vs git pull (with examples)

✅ 1. Use git fetch (SAFE way)
👉 Situation:

You are working on a project and your teammate pushed new code.

But you want to:

First check what changes they made
Avoid breaking your current code

✅ What you do:
```bash
git fetch origin
```

Now check changes:
```bash
git log origin/main
```
or
```bash
git diff origin/main
```

💡 Example:

You are building a restaurant app (your project)
Your teammate updated:

- payment logic
- order calculation

👉 You don't want to break your running system
👉 So you fetch first, review, then merge manually
```bash
git merge origin/main
```

---

🚀 2. Use git pull (FAST way)
👉 Situation:
You trust the code
You just want the latest version quickly
No need to review

✅ What you do:
```bash
git pull origin main
```

💡 Example:

You just started your day and want latest code:
👉 Simply pull and start working

---

⚠️ 3. When NOT to use git pull
❌ Situation:

You have uncommitted changes
```bash
git pull
```

👉 This may cause:

- merge conflicts
- overwritten code

✅ Better approach:
```bash
git fetch
git stash
git merge origin/main
git stash pop
```

---

🧠 Best Practice (Very Important)

👉 Professional developers mostly do:
```bash
git fetch
git merge origin/main
```

✔ More control
✔ Less risk
✔ Cleaner history

---

## 🎯 Final Interview Answer (Short)

> "I use git fetch when I want to safely review changes before applying them.
> I use git pull when I want to quickly update my local branch with remote changes."

---

## 🔹 git merge vs git rebase

🟢 1. git merge (Combine history)
👉 What it does:
- Joins two branches
- Keeps both histories
- Creates a merge commit

📌 Example:

You have:

You have:
main:    A --- B --- C
feature:       D --- E

Run:
```bash
git checkout main
git merge feature
```

👉 Result:
main: A --- B --- C ------- M
\           /
feature:        D --- E
👉 M = merge commit
👉 History is not clean but safe

---

🔵 2. git rebase (Rewrite history)
👉 What it does:
- Moves your branch on top of another
- Creates linear (clean) history
- No merge commit

📌 Example:

Same starting point:

main:    A --- B --- C
feature:       D --- E

Run:
```bash
git checkout feature
git rebase main
```

👉 Result:
main:    A --- B --- C
feature:             D' --- E'

👉 D' E' = rewritten commits
👉 Looks like feature was created from latest main

---

## ⚡ Key Differences

| Feature       | Merge 🟢             | Rebase 🔵            |
| ------------- | -------------------- | -------------------- |
| History       | Keeps all            | Rewrites history     |
| Commit        | Creates merge commit | No extra commit      |
| Safe          | ✅ Very safe          | ⚠️ Risky if misused  |
| Clean history | ❌ Messy              | ✅ Clean              |

---

## 🎯 Best Interview Answer

> "Merge preserves history and creates a merge commit, while rebase rewrites history to create a clean linear timeline. I use merge for shared branches and rebase for my local feature branches."