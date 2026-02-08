# Git Advanced (rebase, stash, cherry-pick, workflows)
# ----------------------------------------------------
# This lesson covers advanced Git techniques:
#   - rebasing
#   - stashing
#   - cherry-picking
#   - resolving conflicts
#   - advanced branching workflows
#   - interactive rebase
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Git Rebase (Rewrite History)
# ---------------------------------------------------------------
# Rebase moves commits to a new base, creating a cleaner history.

## Rebase your branch onto main
git checkout feature-login
git rebase main

## If conflicts occur:
# Fix files → then:
git add .
git rebase --continue

## Abort rebase
git rebase --abort

# ---------------------------------------------------------------
# 2. Interactive Rebase (edit history)
# ---------------------------------------------------------------

## Rewrite last 5 commits
git rebase -i HEAD~5

## Options inside editor:
# pick     → keep commit
# reword   → change commit message
# edit     → modify commit
# squash   → combine commits
# drop     → remove commit

# ---------------------------------------------------------------
# 3. Git Stash (Save Work Without Committing)
# ---------------------------------------------------------------

## Save uncommitted changes
git stash

## Save with message
git stash push -m "WIP: fixing login"

## List stashes
git stash list

## Apply latest stash
git stash apply

## Apply and remove stash
git stash pop

## Drop a stash
git stash drop stash@{1}

## Clear all stashes
git stash clear

# ---------------------------------------------------------------
# 4. Cherry-Pick (Copy Specific Commits)
# ---------------------------------------------------------------

## Copy a commit to current branch
git cherry-pick <commit-hash>

## Cherry-pick a range
git cherry-pick A..B

## Resolve conflicts if needed
git add .
git cherry-pick --continue

# ---------------------------------------------------------------
# 5. Git Reset (Soft, Mixed, Hard)
# ---------------------------------------------------------------

## Soft reset (keep changes staged)
git reset --soft HEAD~1

## Mixed reset (default, keep changes unstaged)
git reset HEAD~1

## Hard reset (discard changes)
git reset --hard HEAD~1

# ---------------------------------------------------------------
# 6. Git Revert (Undo Without Rewriting History)
# ---------------------------------------------------------------

## Create a new commit that undoes a previous one
git revert <commit-hash>

## Revert a merge commit
git revert -m 1 <merge-hash>

# ---------------------------------------------------------------
# 7. Git Clean (Remove Untracked Files)
# ---------------------------------------------------------------

## Preview what will be deleted
git clean -n

## Delete untracked files
git clean -f

## Delete untracked directories
git clean -fd

# ---------------------------------------------------------------
# 8. Git Workflows (Professional)
# ---------------------------------------------------------------

## Git Flow (common in teams)
# main        → production-ready code
# develop     → integration branch
# feature/*   → new features
# release/*   → release preparation
# hotfix/*    → urgent fixes

## GitHub Flow (simpler)
# main        → always deployable
# feature branches → PR → merge

## Trunk-Based Development
# small, frequent commits to main
# feature flags for incomplete work

# ---------------------------------------------------------------
# 9. Pull With Rebase (Cleaner History)
# ---------------------------------------------------------------

## Instead of:
git pull

## Use:
git pull --rebase

## Set as default:
git config --global pull.rebase true

# ---------------------------------------------------------------
# 10. Bisect (Find Buggy Commit)
# ---------------------------------------------------------------

## Start bisect
git bisect start

## Mark current commit as bad
git bisect bad

## Mark last known good commit
git bisect good <commit-hash>

## Git will guide you through testing commits
# After finding the bad commit:
git bisect reset

# ---------------------------------------------------------------
# 11. Submodules (External Repos Inside Repo)
# ---------------------------------------------------------------

## Add submodule
git submodule add https://github.com/user/lib.git libs/lib

## Update submodules
git submodule update --init --recursive

## Pull latest changes
git submodule update --remote

# ---------------------------------------------------------------
# 12. Practical Advanced Examples
# ---------------------------------------------------------------

## 1. Combine multiple commits into one
git rebase -i HEAD~4
# mark commits as "squash"

## 2. Move a feature branch onto updated main
git checkout feature-api
git fetch origin
git rebase origin/main

## 3. Apply a fix from another branch
git cherry-pick <commit-hash>

## 4. Save work before switching branches
git stash push -m "WIP"
git checkout main

# ---------------------------------------------------------------
# 13. Summary
# ---------------------------------------------------------------
# - rebase → rewrite history, cleaner commits
# - interactive rebase → squash, edit, reorder commits
# - stash → save work temporarily
# - cherry-pick → copy specific commits
# - reset → move HEAD (soft/mixed/hard)
# - revert → undo safely
# - bisect → find buggy commit
# - workflows → Git Flow, GitHub Flow, trunk-based
#
# Next lesson:
# → Git Collaboration (Pull Requests, Code Reviews, Branch Protection)
