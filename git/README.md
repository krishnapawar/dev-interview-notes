# 🌿 Git Interview Q&A — Complete Guide

> A practical reference for the most commonly asked Git interview questions, with real examples and short interview-ready answers.

---

## 📑 Table of Contents

1. [git fetch vs git pull](#1-git-fetch-vs-git-pull)
2. [git merge vs git rebase](#2-git-merge-vs-git-rebase)
3. [What is git stash and when do you use it?](#3-what-is-git-stash-and-when-do-you-use-it)
4. [What is a git conflict and how do you resolve it?](#4-what-is-a-git-conflict-and-how-do-you-resolve-it)
5. [git reset vs git revert vs git checkout](#5-git-reset-vs-git-revert-vs-git-checkout)
6. [What is git cherry-pick?](#6-what-is-git-cherry-pick)
7. [What is .gitignore and how does it work?](#7-what-is-gitignore-and-how-does-it-work)
8. [What is a git tag?](#8-what-is-a-git-tag)
9. [What is git bisect?](#9-what-is-git-bisect)
10. [Branching Strategy — Git Flow](#10-branching-strategy--git-flow)
11. [How do you squash commits?](#11-how-do-you-squash-commits)
12. [What is the difference between git clone, fork, and clone?](#12-fork-vs-clone)
13. [How do you undo the last commit?](#13-how-do-you-undo-the-last-commit)
14. [What is HEAD in Git?](#14-what-is-head-in-git)
15. [git log tips — find anything fast](#15-git-log-tips--find-anything-fast)

---

## 1. git fetch vs git pull

### ✅ Use `git fetch` — the SAFE way

**Situation:** Your teammate pushed new code, but you want to check what changed before applying it.

```bash
git fetch origin
```

Now review what changed:
```bash
git log origin/main        # see new commits
git diff origin/main       # see line-by-line diff
```

Then merge manually when you're ready:
```bash
git merge origin/main
```

**💡 Real Example:**
You're building a restaurant app. Your teammate updated payment logic and order calculation. You don't want to break your running system — so you fetch, review, then merge.

---

### 🚀 Use `git pull` — the FAST way

**Situation:** You trust the team's code, you just want the latest version quickly.

```bash
git pull origin main
```

`git pull` = `git fetch` + `git merge` in one command.

**💡 Real Example:** You just started your workday and want the latest code before beginning. Simply pull and start working.

---

### ⚠️ When NOT to use `git pull`

If you have **uncommitted changes**, a blind pull can cause:
- Merge conflicts
- Overwritten local changes

✅ Safer approach:
```bash
git fetch
git stash          # save your uncommitted work
git merge origin/main
git stash pop      # restore your work
```

---

### 🧠 What Professional Developers Do

```bash
git fetch
git merge origin/main
```

✔ More control &nbsp;&nbsp; ✔ Less risk &nbsp;&nbsp; ✔ Cleaner history

---

### 🎯 Interview Answer

> "I use `git fetch` when I want to safely review changes before applying them. I use `git pull` when I want to quickly sync my local branch. In a team environment I prefer fetch + merge so I can review diffs first."

---

## 2. git merge vs git rebase

### 🟢 git merge — Combine History

What it does:
- Joins two branches together
- Keeps the full history of both
- Creates an extra **merge commit**

**📌 Example:**
```
Before:
main:    A --- B --- C
feature:       D --- E

After: git merge feature
main:    A --- B --- C ------- M
                  \           /
                   D ------- E
```

`M` = merge commit. History is preserved but can look messy.

```bash
git checkout main
git merge feature
```

---

### 🔵 git rebase — Rewrite History

What it does:
- Moves your branch commits **on top of** another branch
- Creates a **clean, linear** history
- No merge commit

**📌 Example:**
```
Before:
main:    A --- B --- C
feature:       D --- E

After: git rebase main
main:    A --- B --- C
feature:             D' --- E'
```

`D'` `E'` = rewritten commits. Looks like feature was always built on top of latest main.

```bash
git checkout feature
git rebase main
```

---

### ⚡ Key Differences

| Feature | Merge 🟢 | Rebase 🔵 |
|---|---|---|
| History | Keeps all | Rewrites history |
| Extra commit | ✅ Creates merge commit | ❌ No extra commit |
| Safety | ✅ Very safe | ⚠️ Risky on shared branches |
| Clean history | ❌ Non-linear | ✅ Linear |
| Best for | Shared / main branches | Local feature branches |

---

### ⚠️ Golden Rule of Rebase

> **Never rebase a branch that others are working on.** It rewrites commit history and will break their local copies.

✅ Rebase your **own local** feature branch before merging.
❌ Never rebase `main`, `develop`, or any shared branch.

---

### 🎯 Interview Answer

> "Merge preserves history and creates a merge commit — safe for shared branches. Rebase rewrites history into a clean linear timeline — I use it on my own local feature branches before opening a PR to keep the history readable."

---

## 3. What is git stash and when do you use it?

**Situation:** You're in the middle of changes and need to switch branches urgently — but you're not ready to commit yet.

`git stash` temporarily saves your uncommitted work and **cleans your working directory**.

---

### 📦 Basic Commands

```bash
git stash              # save current changes
git stash pop          # restore the latest stash + remove it
git stash apply        # restore but keep the stash saved
git stash list         # see all stashes
git stash drop         # delete the latest stash
git stash clear        # delete all stashes
```

---

### 💡 Real Example

```bash
# You're mid-feature when a hotfix is needed
git stash                    # save your work-in-progress
git checkout main
git checkout -b hotfix/login-bug
# ... fix the bug, commit, merge ...
git checkout feature/my-feature
git stash pop                # restore your work exactly where you left off
```

---

### 🎯 Interview Answer

> "I use `git stash` when I need to switch context quickly without committing half-done work. It's like a clipboard for your working directory — save, switch, come back, restore."

---

## 4. What is a git conflict and how do you resolve it?

A conflict happens when **two branches modify the same line** of the same file and Git can't decide which version to keep.

---

### 🔴 When Conflicts Happen

- `git merge`
- `git rebase`
- `git pull`
- `git cherry-pick`

---

### 🔍 What a Conflict Looks Like

```
<<<<<<< HEAD (your branch)
const price = 100;
=======
const price = 150;
>>>>>>> feature/pricing
```

- Everything between `<<<<<<< HEAD` and `=======` is **your version**
- Everything between `=======` and `>>>>>>>` is the **incoming version**

---

### ✅ How to Resolve

**Step 1** — Open the conflicted file and decide what to keep:
```javascript
// Keep your version, their version, or combine:
const price = 150; // chose their value, or merge logic
```

**Step 2** — Remove all conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)

**Step 3** — Stage and commit:
```bash
git add src/pricing.js
git commit -m "resolve merge conflict in pricing"
```

---

### 🛠️ Using a Merge Tool

```bash
git mergetool          # opens visual diff tool (VS Code, vimdiff, etc.)
```

Configure VS Code as merge tool:
```bash
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

---

### 🎯 Interview Answer

> "A conflict happens when two branches change the same lines. Git marks them with `<<<<<<<` and `>>>>>>>` markers. I open the file, decide which change to keep (or combine them), remove the markers, then `git add` and commit. For complex conflicts I use VS Code's built-in merge editor."

---

## 5. git reset vs git revert vs git checkout

These three are the most commonly confused Git commands.

---

### 🔴 git reset — Undo commits (rewrites history)

```bash
git reset --soft HEAD~1    # undo last commit, keep changes staged
git reset --mixed HEAD~1   # undo last commit, keep changes unstaged (default)
git reset --hard HEAD~1    # undo last commit, DELETE all changes ⚠️
```

```
Before: A --- B --- C  (HEAD)
After:  A --- B  (HEAD)     C is gone
```

> ⚠️ Never `reset` commits that have been **pushed to a shared branch** — it rewrites history and breaks others.

---

### 🟢 git revert — Undo safely (safe for shared branches)

Creates a **new commit** that undoes the changes of a previous commit. History is preserved.

```bash
git revert HEAD         # revert last commit
git revert abc1234      # revert a specific commit by hash
```

```
Before: A --- B --- C
After:  A --- B --- C --- C'   (C' undoes C's changes)
```

✅ Safe to use on `main` or shared branches.

---

### 🔵 git checkout — Switch or restore

```bash
git checkout main              # switch branch
git checkout -b feature/new    # create and switch to new branch
git checkout -- file.js        # discard changes to a specific file ⚠️
```

> In modern Git (2.23+), `git switch` and `git restore` are preferred over `git checkout` for clarity.

---

### 📊 Comparison

| Command | Rewrites History | Safe for Shared | Use Case |
|---|---|---|---|
| `reset --hard` | ✅ Yes | ❌ No | Local undo, clean up before push |
| `reset --soft` | ✅ Yes | ❌ No | Undo commit, keep changes staged |
| `revert` | ❌ No | ✅ Yes | Undo pushed commit safely |
| `checkout` | ❌ No | ✅ Yes | Switch branch / discard file |

---

### 🎯 Interview Answer

> "`reset` moves the HEAD pointer and can delete commits — only safe locally. `revert` creates a new undo commit and is safe on shared branches. `checkout` switches branches or restores files. In short: use `revert` in production, `reset` only locally."

---

## 6. What is git cherry-pick?

Cherry-pick lets you **copy a specific commit** from one branch and apply it to another — without merging the whole branch.

---

### 💡 When to Use

- A bug fix is on a feature branch but you need it on `main` **now**
- You want one commit from a colleague's branch, not all of it
- Backporting a fix to an older release branch

---

### 📌 Example

```bash
# Find the commit hash you want
git log feature/payments --oneline

# abc1234 fix: correct tax calculation

# Apply it to main
git checkout main
git cherry-pick abc1234
```

The commit is now on `main` with a new hash but the same changes.

---

### Cherry-pick multiple commits:
```bash
git cherry-pick abc1234 def5678        # two specific commits
git cherry-pick abc1234..def5678       # range of commits
```

---

### 🎯 Interview Answer

> "Cherry-pick copies a specific commit onto another branch without a full merge. I use it when a fix exists on a feature branch but needs to go to `main` or a release branch immediately."

---

## 7. What is .gitignore and how does it work?

`.gitignore` tells Git which files and folders to **never track**.

---

### 📄 Common .gitignore Patterns

```gitignore
# Dependencies
node_modules/
vendor/

# Environment files (NEVER commit these)
.env
.env.local
.env.production

# Build output
/dist
/build
*.min.js

# Logs
*.log
logs/

# OS files
.DS_Store
Thumbs.db

# IDE
.idea/
.vscode/
```

---

### ⚠️ Already Tracked Files

`.gitignore` only works for **untracked** files. If a file is already committed, you must untrack it:

```bash
git rm --cached .env           # stop tracking but keep file locally
git rm --cached -r node_modules/
git commit -m "remove .env from tracking"
```

---

### Global .gitignore (for all your projects)

```bash
git config --global core.excludesfile ~/.gitignore_global
```

---

### 🎯 Interview Answer

> "`.gitignore` tells Git which files to never track — like `node_modules`, `.env`, build folders, and OS files. If a file is already tracked, I use `git rm --cached` to untrack it without deleting it locally."

---

## 8. What is a git tag?

Tags mark a **specific point in history** — typically used for releases.

---

### 📌 Two Types

**Lightweight tag** — just a pointer to a commit:
```bash
git tag v1.0.0
```

**Annotated tag** — stores extra metadata (author, date, message):
```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

---

### Common Commands

```bash
git tag                        # list all tags
git tag -a v1.2.0 -m "msg"    # create annotated tag
git push origin v1.2.0         # push a specific tag
git push origin --tags         # push all tags
git checkout v1.0.0            # check out a specific release
git tag -d v1.0.0              # delete tag locally
git push origin --delete v1.0.0  # delete tag remotely
```

---

### 🎯 Interview Answer

> "Tags mark specific commits — usually release versions like `v1.0.0`. Unlike branches, tags don't move. I prefer annotated tags for releases because they store the author and message. Push tags separately with `git push origin --tags`."

---

## 9. What is git bisect?

`git bisect` uses **binary search** to find the exact commit that introduced a bug.

---

### 🐛 When to Use

You know the app worked at commit `v1.0` but is broken now. Instead of manually checking every commit, `bisect` finds the culprit in `log N` steps.

---

### 📌 How to Use

```bash
git bisect start
git bisect bad                  # current commit is broken
git bisect good v1.0.0          # this tag/commit was working

# Git checks out a middle commit — you test it
# If broken:
git bisect bad
# If working:
git bisect good

# Git keeps narrowing until it finds the first bad commit
# When done:
git bisect reset                # return to original HEAD
```

---

### 🎯 Interview Answer

> "Git bisect does a binary search through commit history to find exactly which commit introduced a bug. You mark a known-good and known-bad commit, then test each midpoint Git checks out — it finds the culprit in log N steps instead of checking every commit manually."

---

## 10. Branching Strategy — Git Flow

**Q: What branching strategy do you follow?**

---

### 🌿 Git Flow (most common in teams)

```
main        ← production code only, always stable
develop     ← integration branch, latest development
feature/*   ← new features, branch off develop
release/*   ← prep for release, branch off develop
hotfix/*    ← urgent production fixes, branch off main
```

---

### 🔄 Typical Workflow

```bash
# Start a feature
git checkout develop
git checkout -b feature/user-auth

# Work, commit, done
git checkout develop
git merge feature/user-auth
git branch -d feature/user-auth

# Hotfix in production
git checkout main
git checkout -b hotfix/payment-crash
# fix it
git checkout main
git merge hotfix/payment-crash
git checkout develop
git merge hotfix/payment-crash   # keep develop in sync too
```

---

### 🚀 GitHub Flow (simpler, for CI/CD teams)

```
main        ← always deployable
feature/*   ← branch off main, PR back to main
```

Simpler than Git Flow — good for teams that deploy frequently.

---

### 🎯 Interview Answer

> "I follow Git Flow for most projects — `main` is production, `develop` is integration, and all work happens in `feature/`, `hotfix/`, or `release/` branches. For smaller projects or CI/CD pipelines that deploy on every merge, GitHub Flow is cleaner."

---

## 11. How do you squash commits?

Squashing combines multiple commits into one — useful for keeping history clean before merging a PR.

---

### 📌 Using Interactive Rebase

```bash
# Squash last 3 commits into one
git rebase -i HEAD~3
```

This opens an editor:
```
pick abc1234 add login page
pick def5678 fix typo
pick ghi9012 fix another typo
```

Change `pick` to `squash` (or `s`) on commits you want to merge:
```
pick abc1234 add login page
squash def5678 fix typo
squash ghi9012 fix another typo
```

Save → Git opens another editor to write the combined commit message.

---

### 📌 Squash on Merge (GitHub/GitLab PR option)

When merging a PR, choose **"Squash and merge"** — all commits become one clean commit on `main`.

---

### 🎯 Interview Answer

> "I use `git rebase -i` to squash multiple WIP or fix commits into a single meaningful commit before opening a PR. It keeps the main branch history clean and easy to read."

---

## 12. Fork vs Clone

| | Fork | Clone |
|---|---|---|
| What | Copy of repo **on GitHub/GitLab** | Copy of repo **on your local machine** |
| Connection | Linked to original (can PR back) | Linked to the remote you cloned from |
| Used for | Contributing to others' projects | Working on your own / team projects |
| Where it lives | Remote (GitHub account) | Local machine |

```bash
# Clone a repo locally
git clone https://github.com/user/repo.git

# After forking on GitHub, clone YOUR fork
git clone https://github.com/YOUR-USERNAME/repo.git

# Keep fork in sync with original
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main
```

---

### 🎯 Interview Answer

> "Fork creates your own copy of a repo on GitHub — used for contributing to open source. Clone copies a repo to your local machine. When contributing to open source, I fork first, then clone my fork, and add the original as `upstream` to stay in sync."

---

## 13. How do you undo the last commit?

Depends on what you want to keep:

```bash
# Keep changes, unstage them (most common)
git reset --soft HEAD~1

# Keep changes, but unstaged (default mixed)
git reset HEAD~1

# Delete everything — nuclear option ⚠️
git reset --hard HEAD~1

# Already pushed? Use revert instead
git revert HEAD
git push
```

---

### 🎯 Interview Answer

> "If the commit is local I use `git reset --soft HEAD~1` to undo it while keeping my changes. If it's already pushed to a shared branch I use `git revert HEAD` which creates a safe undo commit without rewriting history."

---

## 14. What is HEAD in Git?

`HEAD` is a **pointer** that tells Git which commit you're currently working on. It usually points to the tip of your current branch.

---

```
main:  A --- B --- C  ← HEAD (you are here)
```

```bash
HEAD        # current commit
HEAD~1      # one commit before HEAD
HEAD~2      # two commits before HEAD
HEAD^       # same as HEAD~1 (parent)
```

**Detached HEAD** — when HEAD points to a specific commit (not a branch):
```bash
git checkout abc1234    # detached HEAD state
```
> In this state, new commits won't belong to any branch. Create a branch if you want to keep them: `git checkout -b new-branch`

---

### 🎯 Interview Answer

> "HEAD is Git's pointer to the current commit. Normally it points to the tip of the active branch. A detached HEAD means you've checked out a specific commit directly — any changes made there need to be saved to a new branch or they'll be lost."

---

## 15. git log Tips — Find Anything Fast

```bash
# Standard log
git log --oneline                          # compact, one line per commit
git log --oneline --graph --all            # visual branch tree
git log --oneline -10                      # last 10 commits

# Filter by author
git log --author="Krishna"

# Filter by date
git log --after="2024-01-01"
git log --before="2024-12-31"

# Filter by message
git log --grep="fix"

# Filter by file
git log -- src/payment.js

# See what changed in each commit
git log -p                                 # full diff per commit
git log --stat                             # files changed summary

# Find when a line was introduced
git log -S "const price = 100"            # which commit added this string

# Pretty format
git log --pretty=format:"%h %an %ar %s"   # hash, author, time, message
```

---

### 🎯 Interview Answer

> "I use `git log --oneline --graph --all` to get a visual overview of all branches. For debugging I use `git log -S 'searchTerm'` to find exactly which commit introduced a specific piece of code."

---

*Last updated: 2026 · Git interview preparation reference*