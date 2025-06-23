# 🧠 Git Advanced Commands (Not in Your Previous List)

## 🔧 Repository Setup & Project Management

```
git init --bare
```
---Create a bare Git repository (for use as a remote repo only, without a working directory)

```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
---Set your global Git username and email (used for commits)

```
git config --list
```
---View all Git configuration values

```
git clean -fd
```
---Remove all untracked files and directories (dangerous: use with care)

---

## 📦 Staging, Committing, & Diff Tools

```
git add -u
```
---Stage updated and deleted files (but not new files)

```
git diff
```
---View unstaged changes between working directory and index

```
git diff --cached
```
---View changes between staged files and last commit

```
git commit --amend
```
---Modify or update the most recent commit message or contents

---

## 🌲 Advanced Branching & History

```
git reflog
```
---View history of HEAD movements (recover deleted branches or commits)

```
git cherry-pick <commit-hash>
```
---Apply a specific commit from another branch onto your current branch

```
git bisect start
git bisect bad
git bisect good <commit>
```
---Start a binary search to find which commit introduced a bug

```
git blame <file>
```
---Show who last modified each line of a file (with commit info)

---

## 🌍 Remote Repository & Collaboration

```
git push --force
```
---Force push changes (overwrite remote history – be very careful)

```
git pull --rebase
```
---Rebase your local commits on top of fetched commits instead of merging

```
git fetch --prune
```
---Remove deleted branches from your remote-tracking references

```
git remote rename origin upstream
```
---Rename a remote name (e.g., from `origin` to `upstream`)

---

## 🔖 Tagging & Releases

```
git tag -d <tag-name>
```
---Delete a local tag

```
git push origin :refs/tags/<tag-name>
```
---Delete a tag on the remote repository

---

## 🛠️ Miscellaneous & Tools

```
git archive -o latest.zip HEAD
```
---Create a zip archive of the current repo at HEAD

```
git shortlog
```
---Summarize `git log` by author and commits

```
git describe --tags
```
---Describe the current commit using the most recent tag

```
git grep <pattern>
```
---Search for a string or pattern inside tracked files
