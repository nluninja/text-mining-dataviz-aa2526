# Git Quick Tutorial
## Essential Commands for Version Control

---

# What is Git?

- **Distributed version control** system
- Tracks changes to your code
- Enables collaboration between developers
- Every developer has a complete copy of the repository

---

# Initial Setup

```bash
# Configure your name and email (once only)
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Verify configuration
git config --list
```

---

# 1. CLONE
## Copy a remote repository

```bash
# Syntax
git clone <repository-url>

# Example
git clone https://github.com/user/project.git

# Clone into a specific folder
git clone https://github.com/user/project.git my-folder
```

**What it does:** Downloads the entire repository (code + history) to your computer

---

# 2. STATUS and LOG
## Check the repository state

```bash
# See modified/added/removed files
git status

# See commit history
git log

# Compact log (one line per commit)
git log --oneline

# Log with branch graph
git log --oneline --graph --all
```

---

# 3. COMMIT
## Save your changes

```bash
# Step 1: Add files to the "staging area"
git add filename.py           # A specific file
git add .                     # All modified files
git add *.py                  # All .py files

# Step 2: Create the commit
git commit -m "Description of changes"

# Shortcut: add + commit together (only tracked files)
git commit -am "Description of changes"
```

---

# COMMIT - Visual Workflow

```
Working Directory    Staging Area    Repository
      |                   |              |
      |   git add         |              |
      |------------------>|              |
      |                   |  git commit  |
      |                   |------------->|
      |                   |              |
```

---

# COMMIT - Best Practices

```bash
# Effective commit messages:
git commit -m "Add user authentication feature"
git commit -m "Fix bug in payment processing"
git commit -m "Update README with installation instructions"

# Avoid vague messages like:
# "fix", "update", "changes", "stuff"
```

---

# 4. PUSH
## Send commits to remote server

```bash
# Syntax
git push <remote> <branch>

# Typical example
git push origin main

# First push of a new branch
git push -u origin new-branch

# After -u, just use:
git push
```

**What it does:** Uploads your local commits to the remote repository (GitHub, GitLab, etc.)

---

# 5. PULL
## Download changes from server

```bash
# Syntax
git pull <remote> <branch>

# Typical example
git pull origin main

# If branch is already configured:
git pull
```

**What it does:** Downloads and integrates changes from the remote repository

`git pull` = `git fetch` + `git merge`

---

# 6. CHECKOUT
## Switch branches or restore files

```bash
# Switch branch
git checkout branch-name

# Create and switch to a new branch
git checkout -b new-branch

# Restore a file to the last commit version
git checkout -- filename.py

# Modern commands (Git 2.23+)
git switch branch-name           # Switch branch
git switch -c new-branch         # Create new branch
git restore filename.py          # Restore file
```

---

# BRANCH
## Work on parallel versions

```bash
# See existing branches
git branch              # Local branches
git branch -a           # All branches (including remote)

# Create a new branch
git branch branch-name

# Delete a branch
git branch -d branch-name        # Safe delete
git branch -D branch-name        # Force delete
```

---

# 7. MERGE
## Combine two branches

```bash
# Step 1: Go to the destination branch
git checkout main

# Step 2: Merge the other branch
git merge feature-branch
```

```
        A---B---C  feature
       /         \
  D---E-----------F  main (after merge)
```

---

# MERGE - Handling Conflicts

```bash
# If there are conflicts, Git warns you:
# CONFLICT (content): Merge conflict in file.py

# 1. Open the file and look for markers:
<<<<<<< HEAD
code from current branch
=======
code from branch being merged
>>>>>>> feature-branch

# 2. Manually resolve by choosing the correct code

# 3. Add and commit
git add file.py
git commit -m "Resolve merge conflict"
```

---

# 8. REBASE
## Rewrite history

```bash
# Syntax
git rebase <base-branch>

# Example: update feature with latest changes from main
git checkout feature
git rebase main
```

---

# REBASE vs MERGE

```
MERGE:
      A---B---C  feature
     /         \
D---E---F-------G  main

REBASE:
D---E---F---A'--B'--C'  feature (rebased)
        |
       main
```

- **Merge**: Keeps complete history, creates merge commits
- **Rebase**: Linear and clean history, rewrites commits

---

# REBASE - When to Use It

```bash
# Before pushing, update your branch:
git checkout feature
git pull --rebase origin main

# Or:
git fetch origin
git rebase origin/main
```

**Golden rule:** Never rebase commits that have already been pushed!

---

# Typical Workflow

```bash
# 1. Update your repository
git pull origin main

# 2. Create a branch for the new feature
git checkout -b feature/new-function

# 3. Work and commit
git add .
git commit -m "Add new feature"

# 4. Update with latest changes from main
git pull --rebase origin main

# 5. Push your branch
git push -u origin feature/new-function

# 6. Create a Pull Request on GitHub
```

---

# Useful Commands

```bash
# See differences
git diff                    # Unstaged changes
git diff --staged           # Staged changes

# Undo last commit (keeping changes)
git reset --soft HEAD~1

# Discard all local changes
git checkout -- .

# See configured remotes
git remote -v

# Stash: temporarily save changes
git stash                   # Save
git stash pop               # Restore
```

---

# Command Summary

| Command | Description |
|---------|-------------|
| `clone` | Copy remote repository |
| `status` | Show file status |
| `add` | Add files to stage |
| `commit` | Save changes |
| `push` | Send to server |
| `pull` | Download from server |
| `checkout` | Switch branch/restore |
| `merge` | Combine branches |
| `rebase` | Reapply commits |

---

# Resources

- **Official documentation:** https://git-scm.com/doc
- **Git Cheat Sheet:** https://education.github.com/git-cheat-sheet-education.pdf
- **Learn Git Branching (interactive):** https://learngitbranching.js.org/
- **Pro Git Book (free):** https://git-scm.com/book/en/v2

---

# Questions?

```
  ______ _ _
 / ______(_) |
| |  __ _| |_
| | |_ | | __|
| |__| | | |_
 \_____|_|\__|
```
