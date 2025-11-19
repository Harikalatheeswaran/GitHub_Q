# *__GitHub_Q__*
This repo contains all knowledge on GitHub &amp; the related automation scripts &amp; procedures.

---

### The complete Work Flow : 
`Workflow: Pull → Edit → Remote Update → Merge → Push`

```
          ┌───────────────────────────────┐
          │        Remote Repo            │
          │   A (initial) → B (new)       │
          └───────────────────────────────┘
                     ↑
                     │
        ┌───────────────────────────────┐
        │        Local Repo             │
        │   A (initial)                 │
        └───────────────────────────────┘

Step 1: Pull Script
────────────────────────────────────────────
```
```bash
#!/bin/bash
echo "📥 Pulling latest changes..."
git pull || { echo "Pull failed"; exit 1; }
git status -sb
echo "✅ Up to date – ready to edit!"
read -p "Press Enter to close..."
```
```
Result:
Local Repo: A → B (synced with remote)

────────────────────────────────────────────
Step 2: User edits → Creates Commit C
Local Repo: A → B → C

────────────────────────────────────────────
Step 3: Meanwhile, Remote gets Commit B (already there)
Remote Repo: A → B

────────────────────────────────────────────
Step 4: Push Script
────────────────────────────────────────────
```
```bash
#!/bin/bash
echo "📤 Staging all changes..."
git add .
read -p "📝 Commit message: " message
if [ -z "$message" ]; then 
    git commit 
else 
    git commit -m "$message"
fi

echo "🔄 Final safety pull..."
git pull --no-rebase || { echo "⚠️ Conflict! Fix manually"; exit 1; }

echo "🚀 Pushing..."
git push || exit 1
git status -sb
echo "🎉 All synced perfectly!"
read -p "Press Enter to close..."
```
```
────────────────────────────────────────────
Result after merge:
Local Repo: A → B → C (merged)
Remote Repo: A → B → C
```

---

### Here’s the timeline of what happens when you commit locally and then pull before pushing:

```
Remote Repo (GitHub)       Local Repo (Your PC)
------------------------------------------------
| Commit A | Commit B |    | Commit A |
------------------------------------------------
You make changes → Commit C locally
------------------------------------------------
Now:
Remote: A → B
Local:  A → C
------------------------------------------------
If you push now → Conflict risk because B exists remotely.
------------------------------------------------
Solution:
Pull first → Merge B into your local branch:
Local becomes: A → B → C (or merged)
------------------------------------------------
Then push → Everyone is in sync.
```
So pulling ensures you integrate others’ changes before pushing yours.

---

`Note`:
1. Why the second pull matters
```
If someone pushed B while you were editing, your local commit C needs to integrate B before pushing.
The second pull ensures merge happens locally, not on the remote, avoiding broken history.
```
2. Rebase vs Merge in this context
```
* Merge: Adds a merge commit → history shows both branches combined.
* Rebase: Moves your commit C on top of B → cleaner history, but rewrites commits.
```
