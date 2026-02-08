# Git Collaboration (Pull Requests, Code Reviews, Branch Protection)
# -----------------------------------------------------------------
# This lesson covers collaborative Git workflows:
#   - pull requests (PRs)
#   - code reviews
#   - branch protection rules
#   - resolving conflicts in teams
#   - fork workflows
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. Pull Requests (PRs)
# ---------------------------------------------------------------
# A Pull Request is a request to merge changes from one branch into another.
# Common workflow:
#   1. Create feature branch
#   2. Commit changes
#   3. Push branch to GitHub
#   4. Open PR
#   5. Request review
#   6. Merge after approval

## Create feature branch
git checkout -b feature-login

## Push branch
git push -u origin feature-login

## Then open GitHub → "New Pull Request"

# ---------------------------------------------------------------
# 2. PR Structure (Best Practices)
# ---------------------------------------------------------------

## A good PR includes:
# - clear title
# - description of changes
# - screenshots (if UI)
# - testing steps
# - linked issue number (#123)
# - small, focused changes

## Example PR description:
# Title: Add login validation
# Description:
#   - Added email format validation
#   - Added password strength check
# Testing:
#   - Run npm test
#   - Try invalid email

# ---------------------------------------------------------------
# 3. Code Reviews
# ---------------------------------------------------------------
# Code reviews ensure:
#   - quality
#   - consistency
#   - security
#   - maintainability

## Reviewer responsibilities:
# - check logic
# - check style
# - check security issues
# - check performance
# - suggest improvements

## Author responsibilities:
# - respond to comments
# - update code
# - keep PR small

# ---------------------------------------------------------------
# 4. Approving and Merging PRs
# ---------------------------------------------------------------

## Approve PR (GitHub UI)
# Click "Approve" → "Merge Pull Request"

## Merge strategies:
# - Merge commit (default)
# - Squash and merge (clean history)
# - Rebase and merge (linear history)

## Delete branch after merge
git push origin --delete feature-login

# ---------------------------------------------------------------
# 5. Branch Protection Rules
# ---------------------------------------------------------------
# Protect main branch from accidental changes.

## Common rules:
# - require PR before merging
# - require code review approval
# - require passing CI tests
# - prevent force pushes
# - require signed commits

## Set up in GitHub:
# Settings → Branches → Add rule → Protect main

# ---------------------------------------------------------------
# 6. Forking Workflow (Open Source)
# ---------------------------------------------------------------
# Used when you cannot push directly to a repo.

## Steps:
# 1. Fork repository
# 2. Clone your fork
git clone https://github.com/worku/project.git

# 3. Add upstream remote
git remote add upstream https://github.com/original/project.git

# 4. Sync fork
git fetch upstream
git merge upstream/main

# 5. Create feature branch
git checkout -b fix-typo

# 6. Push to your fork
git push origin fix-typo

# 7. Open PR to upstream repo

# ---------------------------------------------------------------
# 7. Resolving Conflicts in Collaboration
# ---------------------------------------------------------------

## When pulling changes:
git pull --rebase

## If conflicts occur:
# Edit conflicting files
git add .
git rebase --continue

## If too messy:
git rebase --abort

# ---------------------------------------------------------------
# 8. Reviewing Commit History
# ---------------------------------------------------------------

## Show who changed what
git blame file.txt

## Show PR-related commits
git log --oneline --decorate --graph

# ---------------------------------------------------------------
# 9. Continuous Integration (CI) in PRs
# ---------------------------------------------------------------
# CI runs automated tests on every PR.

## Common CI checks:
# - unit tests
# - linting
# - security scans
# - build checks

## If CI fails:
# Fix code → commit → push → CI reruns automatically

# ---------------------------------------------------------------
# 10. Practical Collaboration Workflow
# ---------------------------------------------------------------

## 1. Update local main
git checkout main
git pull

## 2. Create feature branch
git checkout -b feature-api

## 3. Work → commit → push
git add .
git commit -m "Add API endpoint"
git push -u origin feature-api

## 4. Open PR
# Add reviewers, description, tests

## 5. Fix review comments
git add .
git commit -m "Fix review comments"
git push

## 6. Merge PR
# Delete branch

# ---------------------------------------------------------------
# 11. Summary
# ---------------------------------------------------------------
# - Pull Requests → propose changes
# - Code reviews → improve quality
# - Branch protection → prevent mistakes
# - Fork workflow → open-source contributions
# - CI → automated testing
# - Rebase → keep history clean
#
# Next lesson:
# → Linux Networking (Advanced): routing, bonding, VLANs
