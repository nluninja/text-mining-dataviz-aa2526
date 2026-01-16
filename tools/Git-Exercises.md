# Git Exercises

Practice exercises to reinforce your Git skills. Complete them in order as they build on each other.

---

## Exercise 1: Repository Setup and First Commit

**Objective:** Create a new repository and make your first commit.

**Tasks:**
1. Create a new folder called `git-practice`
2. Initialize it as a Git repository
3. Create a file called `notes.txt` with some text
4. Stage and commit the file with the message "Add notes file"
5. Check the log to verify your commit

---

## Exercise 2: Branching and Merging

**Objective:** Create a feature branch, make changes, and merge back to main.

**Tasks:**
1. Create and switch to a new branch called `feature-readme`
2. Create a `README.md` file with a project title and description
3. Commit your changes
4. Switch back to `main` branch
5. Merge the `feature-readme` branch into `main`
6. Delete the feature branch

---

## Exercise 3: Handling Conflicts

**Objective:** Simulate and resolve a merge conflict.

**Tasks:**
1. Create a new branch called `edit-notes`
2. On the `edit-notes` branch, modify the first line of `notes.txt`
3. Commit the change
4. Switch to `main` and modify the same first line of `notes.txt` differently
5. Commit this change on `main`
6. Try to merge `edit-notes` into `main`
7. Resolve the conflict by keeping both changes
8. Complete the merge

---

## Exercise 4: Using Stash

**Objective:** Learn to temporarily save work in progress.

**Tasks:**
1. Make some changes to `README.md` but do NOT commit
2. Stash your changes with a descriptive message
3. Verify your working directory is clean with `git status`
4. Create a new file `urgent-fix.txt` and commit it (simulating urgent work)
5. Apply your stashed changes back
6. Commit the README changes

---

## Exercise 5: Interactive Rebase (Advanced)

**Objective:** Clean up commit history before sharing your work.

**Tasks:**
1. Create three separate commits:
   - Add a file `file1.txt` with commit message "Add file1"
   - Add a file `file2.txt` with commit message "Add file2"
   - Add a file `file3.txt` with commit message "Add flie3" (note the typo!)
2. Use interactive rebase to:
   - Fix the typo in the third commit message ("flie3" → "file3")
   - Squash the first two commits into one with message "Add file1 and file2"
3. Verify your cleaned history with `git log`

---

## Bonus Challenge

Clone this course repository to your local machine, create a branch, and practice pulling updates from `origin/main` while keeping your local changes.

```bash
git clone <repository-url>
cd text-mining-dataviz-aa2526
git checkout -b my-notes
# Add your own notes to the notebooks
# When new content is available:
git fetch origin
git rebase origin/main
```

---

## Tips for Success

- Always check `git status` before and after operations
- Use `git log --oneline --graph` to visualize branch history
- When in doubt, create a backup branch before risky operations
- Practice these exercises multiple times until they feel natural
