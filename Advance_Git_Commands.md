# Advanced Git Commands & Concepts (Cheat Sheet)

Curated for: Harikalatheeswaran (Bangalore)

---

## 🔎 Inspect & Navigate History

### Powerful `log` queries
```bash
git log --oneline --graph --decorate --all
git log --author="Harikalatheeswaran" --since="2025-01-01" -- ./src
git log -S"featureFlag"
git log -G"regex"
git log --follow -- path/to/file
git log --merges
```

### View differences between ranges
```bash
git log --oneline main..HEAD
git log --oneline main...feature
git diff v1.2.0..v1.3.0
```
---

## 🧭 Recover & Time Travel

### reflog
```bash
git reflog
git checkout <reflog-sha>
git reset --hard <reflog-sha>
```
### restore/checkout
```bash
git restore --source=origin/main -- path/to/file
git checkout <commit> -- path/to/file
```

---

## ✍️ Rewriting History (Use with care)

### Interactive rebase
```bash
git rebase -i origin/main
```

### Amend
```bash
git commit --amend
git commit --amend --no-edit
```

### Split a commit
```bash
git rebase -i HEAD~3
git reset HEAD^
git add -p
git commit -m "Part 1"
git add -p
git commit -m "Part 2"
git rebase --continue
```

### Filter repo
```bash
git filter-repo --invert-paths --path secrets.txt
```

---

## 🧪 Bisection
```bash
git bisect start
git bisect bad
git bisect good v1.3.0
git bisect good | bad
git bisect reset
```
---

## 🔀 Merge Strategies

### merge options
```bash
git merge --no-ff feature
git merge --squash feature
```
### rerere
```bash
git config --global rerere.enabled true
```
---

## 🧩 Patch Workflow
```bash
git format-patch origin/main..HEAD
git am < 0001-some-change.patch>
git format-patch -1 <sha>
git cherry-pick <sha>
```
---

## 🌲 Multi-Repo

### submodule
```bash
git submodule add https://... libs/awesome
git submodule update --init --recursive
git submodule foreach git pull origin main
```
### subtree
```bash
git subtree add --prefix=vendor/awesome https://... main --squash
git subtree pull --prefix=vendor/awesome https://... main --squash
```

---

## 🧱 Multi-Working Directories & Sparse

### worktree
```bash
git worktree add ../repo-fix bugfix/ref-issue
```

### sparse-checkout
```bash
git sparse-checkout init --cone
git sparse-checkout set src/ docs/
```
### partial clone
```bash
git clone --filter=blob:none --no-checkout <url>
git sparse-checkout init --cone
git sparse-checkout set src/
git checkout main
```
---

## 🧰 Index Mastery
```bash
git add -p
git add -N path/to/newfile
git commit -v

git diff
git diff --staged
git diff HEAD

git cherry-pick <oldest>^..<newest>
git cherry -v main
```
---

## 📝 Metadata

### notes
```bash
git notes add -m "Perf tested on prod; safe."
git notes show <sha>
git log --show-notes
```
### blame
```bash
git blame -L 100,160 -- path/file.py
git blame --reverse <old>..<new> -- path/file.py
```
---

## 🧹 Maintenance

```bash
git gc --aggressive --prune=now
git remote prune origin
git clean -fdx     # destructive
git clean -fdxn    # preview

git fsck --full
git repack -Ad
```
---

## 🔐 Security
```bash
git log -p -S"BEGIN RSA PRIVATE KEY"
```
---

## ⚙️ Config Tips
```bash
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global fetch.prune true
git config --global rerere.enabled true
git config --global merge.conflictStyle zdiff3
git config --global core.autocrlf input
```
---

## 🧪 Aliases
```bash
git config --global alias.lg "log --oneline --decorate --graph --all"
git config --global alias.st "status -sb"
git config --global alias.co "checkout"
git config --global alias.br "branch -vv"
git config --global alias.unstage "reset HEAD --"
git config --global alias.wip "!git add -A && git commit -m 'wip' --no-verify"
```
---

## 🍒 Deep Dive: git cherry

### What it does
`git cherry` compares diff *content* between branches, not SHAs.

### Usage
```bash
git cherry main
git cherry main feature
```
### Output
+ abc123    # commit exists in feature but not in main
- fef456    # commit already present in main (content match)

### When it's useful
- after rebasing
- before PR
- checking cherry-picks
- cleaning old branches

---

## Quick Scenarios

“I messed up a rebase.” → git reflog + git reset --hard <sha>
“I need one file from main.” → git restore --source=origin/main FILE
“Who changed this function?” → git blame -L <start>,<end>
“Vendor dependency without submodule pain.” → git subtree
