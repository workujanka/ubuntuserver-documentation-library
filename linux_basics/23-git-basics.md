# Git Basics (init, clone, commit, push, branches)
# ------------------------------------------------
# This lesson introduces Git version control:
#   - creating repositories
#   - staging and committing changes
#   - pushing to GitHub
#   - branching and merging
#   - viewing history
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. What Is Git?
# ---------------------------------------------------------------
# Git is a distributed version control system used to track changes in files.
# It helps you:
#   - collaborate with others
#   - track history
#   - revert mistakes
#   - manage multiple versions of a project

## Check Git version
git --version

# ---------------------------------------------------------------
# 2. Configure Git (First Time Only)
# ---------------------------------------------------------------

git config --global user.name "Worku"
git config --global user.email "your-email@example.com"

## View config
git config --list

# ---------------------------------------------------------------
# 3. Creating a New Repository
# ---------------------------------------------------------------

## Initialize Git in a folder
git init

## Check repository status
git status

## Add files to staging
git add file.txt

## Add all files
git add .

## Commit changes
git commit -m "Initial commit"

# ---------------------------------------------------------------
# 4. Cloning a Repository
# ---------------------------------------------------------------

## Clone from GitHub
git clone https://github.com/user/repo.git

## Clone into a specific folder
git clone https://github.com/user/repo.git myproject

# ---------------------------------------------------------------
# 5. Staging and Committing
# ---------------------------------------------------------------

## Stage a single file
git add script.sh

## Stage everything
git add .

## Commit with message
git commit -m "Added new script"

## Commit with detailed message
git commit

# ---------------------------------------------------------------
# 6. Viewing Changes
# ---------------------------------------------------------------

## Show modified files
git status

## Show unstaged changes
git diff

## Show staged changes
git diff --cached

## Show commit history
git log

## Compact history
git log --oneline --graph --decorate

# ---------------------------------------------------------------
# 7. Branching
# ---------------------------------------------------------------

## List branches
git branch

## Create a branch
git branch feature-login

## Switch to a branch
git checkout feature-login

## Create + switch
git checkout -b feature-api

## Delete a branch
git branch -d feature-login

# ---------------------------------------------------------------
# 8. Merging Branches
# ---------------------------------------------------------------

## Switch to main branch
git checkout main

## Merge another branch into main
git merge feature-api

## Resolve merge conflicts manually
# Edit files → mark conflict resolved → then:
git add .
git commit -m "Resolved merge conflict"

# ---------------------------------------------------------------
# 9. Working With Remote Repositories
# ---------------------------------------------------------------

## Add remote origin
git remote add origin https://github.com/user/repo.git

## View remotes
git remote -v

## Push to GitHub (first time)
git push -u origin main

## Push changes
git push

## Pull latest changes
git pull

# ---------------------------------------------------------------
# 10. Undoing Changes
# ---------------------------------------------------------------

## Unstage a file
git reset file.txt

## Undo local changes (dangerous)
git checkout -- file.txt

## Undo last commit (keep changes)
git reset --soft HEAD~1

## Undo last commit (discard changes)
git reset --hard HEAD~1

# ---------------------------------------------------------------
# 11. .gitignore
# ---------------------------------------------------------------

## Create .gitignore
nano .gitignore

## Example:
# node_modules/
# *.log
# .env

## Apply .gitignore
git add .gitignore
git commit -m "Added .gitignore"

# ---------------------------------------------------------------
# 12. Tags (Releases)
# ---------------------------------------------------------------

## Create a tag
git tag v1.0

## Push tags
git push --tags

## List tags
git tag

# ---------------------------------------------------------------
# 13. Practical Workflow Example
# ---------------------------------------------------------------

## 1. Clone repo
git clone https://github.com/user/app.git

## 2. Create feature branch
git checkout -b feature-auth

## 3. Make changes → stage → commit
git add .
git commit -m "Added authentication module"

## 4. Push branch
git push -u origin feature-auth

## 5. Create Pull Request on GitHub

# ---------------------------------------------------------------
# 14. Summary
# ---------------------------------------------------------------
# - git init → create repo
# - git clone → copy repo
# - git add / commit → save changes
# - git push / pull → sync with remote
# - git branch / merge → manage features
# - git log → view history
# - .gitignore → exclude files
#
# Next lesson:
# → Git Advanced (rebasing, stash, cherry-pick, tags, workflows)
