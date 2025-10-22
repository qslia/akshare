Good — your remote setup looks correct. To keep your local `main` branch synced with the **upstream** repository (`https://github.com/akfamily/akshare.git`), follow these steps carefully:

---

### 🧭 Step-by-Step Guide

1. **Make sure you’re on your local main branch**

   ```bash
   git checkout main
   ```

2. **Fetch the latest changes from upstream**

   ```bash
   git fetch upstream
   ```

   This downloads all branches and commits from the upstream repo without modifying your files yet.

3. **Merge or rebase upstream/main into your local main**

   You have two options:

   #### Option A (recommended): Rebase — keep a clean history

   ```bash
   git rebase upstream/main
   ```

   #### Option B: Merge — simpler, but adds merge commits

   ```bash
   git merge upstream/main
   ```

4. **Push the updated main to your fork on GitHub**

   ```bash
   git push origin main
   ```

   Now your fork (`origin/main`) is aligned with the upstream repository.

---

### ✅ Quick Summary

```bash
git checkout main
git fetch upstream
git rebase upstream/main   # or git merge upstream/main
git push origin main
```

---

Would you like me to also show how to **sync automatically** in the future (e.g., via a single `git pull` command or a shell alias)?
<br><br><br>

Excellent question — understanding `git rebase` is key to becoming fluent in Git.

Let’s break it down clearly and visually 👇

---

## 🧩 What `git rebase` Does

**`git rebase`** means:

> “Move or replay my commits on top of another branch (or commit).”

In simpler words:

* It **takes the changes (commits)** from your current branch,
* **temporarily removes** them,
* **updates** your branch to match the target branch (e.g., `upstream/main`),
* then **reapplies** your commits *on top of* the latest version of that target.

---

## ⚙️ Example

### Suppose you have this situation:

```
A---B---C  (upstream/main)
         \
          D---E  (your main)
```

You run:

```bash
git rebase upstream/main
```

Git will:

1. Move commits `D` and `E` aside temporarily.
2. Make your branch match the latest `upstream/main` (A–B–C).
3. Replay `D` and `E` on top.

Result:

```
A---B---C---D'---E'  (your main)
```

Now your branch’s history is **clean and linear** — as if you had started from the latest upstream main all along.

---

## 🧠 Why Use Rebase?

✅ **Keeps history clean and linear** — no messy merge commits.
✅ **Makes reviewing and debugging easier**.
✅ **Ideal when syncing a fork** (like your case).

---

## ⚠️ When *Not* to Use Rebase

❌ Never rebase **shared branches** (e.g., ones others are using or that exist on GitHub and others pull from).
Because rebasing *rewrites history* — it changes commit IDs.

Use it safely only for your **local** work or for **syncing your fork**.

---

## 💡 Shortcut for Your Case

Since you’re syncing your forked `main`, rebasing is perfect:

```bash
git checkout main
git fetch upstream
git rebase upstream/main
git push origin main --force
```

*(You need `--force` because rebasing changes commit history.)*

---

Would you like me to give a **visual diagram** of how `merge` and `rebase` differ? It makes the difference crystal clear.

<br><br><br>
Great question — this is one that confuses **many Git users** at first!
Let’s break it down clearly and practically 👇

---

## 🧭 1. Background

Originally, **`git checkout`** did *many different things*:

* switch branches,
* restore files,
* create branches, etc.

That made it **powerful**, but also **confusing and risky** (easy to overwrite files accidentally).

So Git introduced **two safer, clearer commands** in Git 2.23 (2019):

* `git switch` → for **switching branches**
* `git restore` → for **restoring files**

---

## 🔀 2. `git checkout` vs `git switch`

| Action                                   | `git checkout`                    | `git switch`                           | Notes                                             |
| ---------------------------------------- | --------------------------------- | -------------------------------------- | ------------------------------------------------- |
| **Switch to another branch**             | `git checkout main`               | `git switch main`                      | Both work. `git switch` is clearer.               |
| **Create a new branch and switch to it** | `git checkout -b my-branch`       | `git switch -c my-branch`              | Both same idea.                                   |
| **Restore file from last commit**        | `git checkout -- file.txt`        | `git restore file.txt`                 | `git restore` is the new, safer version.          |
| **Reset a file to a specific commit**    | `git checkout 123abc -- file.txt` | `git restore --source 123abc file.txt` | `restore` is explicit; `checkout` is older style. |

---

## 💬 3. Summary (Simple Rule)

* 🧩 Use **`git switch`** when you’re dealing with **branches**.
* 🧩 Use **`git restore`** when you’re dealing with **files**.
* 🧩 Avoid `git checkout` unless you’re using older Git or need compatibility.

---

## ⚙️ 4. Examples

**Switch branch:**

```bash
git switch main
```

**Create a new branch:**

```bash
git switch -c feature-x
```

**Restore file:**

```bash
git restore src/app.py
```

**Older style (still works):**

```bash
git checkout main
git checkout -b feature-x
git checkout -- src/app.py
```

---

## 🧠 5. TL;DR

| Task          | Modern Command         | Old Command                |
| ------------- | ---------------------- | -------------------------- |
| Switch branch | `git switch main`      | `git checkout main`        |
| Create branch | `git switch -c dev`    | `git checkout -b dev`      |
| Restore file  | `git restore file.txt` | `git checkout -- file.txt` |

---

Would you like me to show a short visual diagram of **how `switch` and `checkout` behave differently with branches and files**? It helps you see why `switch` is safer.
<br><br><br>
Excellent — you’re asking exactly the right kind of question for mastering Git. 👍

Let’s unpack what you’re seeing in:

```bash
git branch -a
```

---

## 🧭 1. Local vs Remote Branches

* **Local branches** are the ones you have on your computer.
  → They appear like:

  ```
  main
  my-work
  ```
* **Remote-tracking branches** are branches that track a branch on a remote (like `origin` or `upstream`).
  → They appear like:

  ```
  remotes/origin/main
  remotes/upstream/dev
  remotes/upstream/main
  ```

---

## 🌐 2. How to Tell Which Remote They Belong To

Everything **after `remotes/`** tells you the remote name:

| Remote Branch           | Remote     | Actual Remote URL                         |
| ----------------------- | ---------- | ----------------------------------------- |
| `remotes/origin/main`   | `origin`   | `https://github.com/qslia/akshare.git`    |
| `remotes/upstream/main` | `upstream` | `https://github.com/akfamily/akshare.git` |
| `remotes/upstream/dev`  | `upstream` | `https://github.com/akfamily/akshare.git` |

So:

* Branches starting with `remotes/origin/…` come from **your fork**.
* Branches starting with `remotes/upstream/…` come from the **official repository**.

---

## 🧩 3. How to See What Each Remote Points To

Run:

```bash
git remote -v
```

You’ll see:

```
origin   https://github.com/qslia/akshare.git (fetch)
upstream https://github.com/akfamily/akshare.git (fetch)
```

So:

* **origin** = your GitHub fork
* **upstream** = the main Akshare project

---

## 🔍 4. Check What a Local Branch Tracks

If you want to see what remote branch a **local branch** follows, run:

```bash
git branch -vv
```

Example output:

```
* main     9f1d23f [origin/main]  latest commit message
  my-work  7a3b9af [origin/my-work]  working on something
```

This tells you:

* `main` tracks `origin/main`
* `my-work` tracks `origin/my-work`

If your `main` instead tracks `upstream/main`, it would show `[upstream/main]`.

---

## 🧠 Summary

| Branch Type       | Example                 | Remote Source         | Purpose             |
| ----------------- | ----------------------- | --------------------- | ------------------- |
| Local             | `main`                  | your computer         | your working copy   |
| Remote (origin)   | `remotes/origin/main`   | your fork             | your GitHub repo    |
| Remote (upstream) | `remotes/upstream/main` | official Akshare repo | main project source |

---

Would you like me to show how to **set your local `main` branch** to track `upstream/main` instead of `origin/main` (so syncing is easier)?
<br><br><br>
Perfect 👍 — you’re almost there. Right now your local `main` branch is **tracking `origin/main`** (your fork).
You want it to **track `upstream/main`** (the official Akshare repo).

Let’s fix that step by step 👇

---

## 🧭 Step 1: Make sure you’re on `main`

```bash
git switch main
```

*(You can also use `git checkout main` if your Git is older.)*

---

## 🧭 Step 2: Set the new upstream branch

Run this command:

```bash
git branch --set-upstream-to=upstream/main
```

This tells Git:

> “From now on, my local `main` branch should track `upstream/main` instead of `origin/main`.”

---

## 🧭 Step 3: Verify the change

```bash
git branch -vv
```

Now you should see something like:

```
* main    ab5dc5b [upstream/main] feat: add version 1.17.71
  my-work c98a3b2 git notes
```

✅ That confirms your local `main` is attached to `upstream/main`.

---

## 🧭 Step 4 (optional but recommended): Sync your fork after rebasing

After syncing from upstream, you may still want to update your fork (`origin/main`) to match.

```bash
git fetch upstream
git rebase upstream/main
git push origin main --force
```

This keeps:

* `main` → same as `upstream/main`
* `origin/main` → your fork stays current

---

## ⚠️ Important

If you later want to **switch back** to tracking your fork’s main branch:

```bash
git branch --set-upstream-to=origin/main
```

---

Would you like me to also show you a **simple sync alias** (e.g. a one-line `git sync` command) so you can update your fork from upstream with a single command in the future?
