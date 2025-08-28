# Essential Git Commands for DevOps

A focused guide to the Git commands most critical for DevOps professionals, emphasizing automation, collaboration, and release management.

## Table of Contents
- [Initial Setup](#initial-setup)
- [Core Workflow](#core-workflow)
- [Branching & Merging](#branching--merging)
- [Remote Repositories](#remote-repositories)
- [Inspecting History](#inspecting-history)
- [Undoing Changes](#undoing-changes)
- [Tagging for Releases](#tagging-for-releases)
- [DevOps Best Practices](#devops-best-practices)

---

## Initial Setup

First-time configuration to identify you as an author and set essential defaults.

### `git config`
Sets your name and email, which will be embedded in every commit you make.

**Example: Set global user information**
```bash
# Set your name to be used in all your commits
git config --global user.name "Your Full Name"

# Set your email to be used in all your commits
git config --global user.email "your.email@example.com"

# Recommended: Set the default branch name for new repositories to "main"
git config --global init.defaultBranch main

# Enable colored output for better readability
git config --global color.ui auto
```

### `git init` & `git clone`
Creating or copying a repository.

**`git init`**: Initializes a new Git repository in the current directory.
```bash
# Turn the current directory into a Git repository
git init
```

**`git clone`**: Creates a local copy of a remote repository. This is the most common way to start working on a project.
```bash
# Clone a repository from a URL
git clone https://github.com/user/repo.git

# For CI/CD pipelines, a shallow clone is much faster as it fetches limited history
git clone --depth 1 https://github.com/user/repo.git
```

---

## Core Workflow

The fundamental day-to-day commands.

### `git status`
Shows the current state of your working directory and staging area. It lets you see which changes have been staged, which haven't, and which files aren't being tracked by Git.
```bash
# See a full status of your repository
git status

# Get a shorter, more compact status view
git status -s
```

### `git add`
Adds file changes to the staging area, preparing them for the next commit.
```bash
# Stage a specific file
git add src/main.js

# Stage all new, modified, and deleted files in the current directory
git add .
```

### `git commit`
Records the staged changes into the repository's history.
```bash
# Commit staged changes with a short message
git commit -m "feat: Add user authentication endpoint"

# A good commit message is crucial for DevOps and team collaboration.
# It should explain the "what" and "why" of the change.
git commit -m "fix: Correct database connection leak" -m "The previous implementation failed to close connections in the connection pool, leading to resource exhaustion. This fix ensures connections are returned after each query."

# Amend the last commit (e.g., to fix a typo in the message)
git commit --amend -m "A better commit message"
```

### `git diff`
Shows the differences between your files and what has been committed.
```bash
# Show unstaged changes (what you've changed but not staged)
git diff

# Show staged changes (what you've staged but not committed)
git diff --staged
```

---

## Branching & Merging

Branching allows you to isolate work, and merging combines it back. This is the core of collaborative development and managing different environments (dev, staging, prod).

### `git branch`
Manages your branches.
```bash
# List all local branches
git branch

# Create a new branch
git branch feature/new-api-endpoint

# Delete a branch (only if it has been merged)
git branch -d feature/old-feature

# Force delete a branch (use with caution)
git branch -D feature/abandoned-work
```

### `git switch` / `git checkout`
Switch between branches. `git switch` is the modern, safer command.
```bash
# Switch to an existing branch
git switch main

# Create a new branch and switch to it immediately
git switch -c hotfix/critical-bug-fix
```

### `git merge`
Joins a branch's history into the current branch.
```bash
# First, switch to the branch you want to merge into
git switch main

# Then, merge the feature branch into it
git merge feature/new-api-endpoint

# If there are conflicts, Git will stop and ask you to resolve them.
# After resolving, add the files and commit.
git add .
git commit -m "Merge feature/new-api-endpoint"

# In DevOps, a --no-ff merge is often preferred for features
# as it creates a merge commit, preserving the history of the feature branch.
git merge --no-ff feature/new-api-endpoint
```

### `git rebase`
Re-applies commits from your branch on top of another branch. This creates a cleaner, linear history. **Use with caution on shared branches.**
```bash
# Switch to your feature branch
git switch feature/new-api-endpoint

# Update your branch with the latest changes from main
git rebase main

# After rebasing, you will likely need to force-push your branch.
# This is a common workflow for feature branches before merging.
```

---

## Remote Repositories

The heart of collaboration and CI/CD. Remotes are versions of your repository that live on other servers.

### `git remote`
Manages the set of remotes you are tracking.
```bash
# List your remote connections
git remote -v

# Add a new remote repository (e.g., an upstream for a forked project)
git remote add upstream https://github.com/original/repo.git
```

### `git fetch`
Downloads history from the remote repository but doesn't change your local working files. It's a safe way to see what others have done.
```bash
# Fetch all changes from the 'origin' remote
git fetch origin

# A key command in CI/CD is to prune branches that no longer exist on the remote
git fetch --prune
```

### `git pull`
Fetches *and* merges the changes into your current branch. It's a shortcut for `git fetch` followed by `git merge`.
```bash
# Fetch and merge changes from the 'origin' remote's corresponding branch
git pull origin main
```

### `git push`
Uploads your local commits to the remote repository. This is how you share your work.
```bash
# Push your current branch to the 'origin' remote
git push origin main

# If you created a new branch, you need to set its upstream on the remote
git push --set-upstream origin feature/new-api-endpoint

# Force push (DANGEROUS: overwrites remote history. Required after a rebase)
# Use --force-with-lease as a safer alternative that won't overwrite work if someone else has pushed.
git push --force-with-lease
```

---

## Inspecting History

Understanding the history of your project is key to debugging and auditing.

### `git log`
Displays the commit history.
```bash
# Show the full commit history
git log

# Show a compact, one-line history
git log --oneline

# Show a graph of the branch structure, which is great for visualizing merges
git log --graph --oneline --all

# Show commits that affect a specific file
git log --follow -- my-file.js
```

### `git show`
Shows detailed information about a specific commit.
```bash
# Show the most recent commit
git show

# Show a specific commit by its hash
git show a1b2c3d4
```

---

## Undoing Changes

Mistakes happen. Here’s how to fix them.

### `git revert`
Creates a *new* commit that undoes the changes of a previous commit. This is the safest way to undo changes on a shared, public branch because it doesn't rewrite history.
```bash
# Revert the changes made in a specific commit
git revert a1b2c3d4

# This will create a new commit saying "Revert 'feat: Add user authentication'"
# You can then push this new commit to the remote.
```

### `git reset`
Moves the current branch pointer back to a previous commit, effectively erasing commits. **This rewrites history and should not be used on shared branches.**
```bash
# Unstage a file you accidentally added
git reset my-file.js

# Go back to the previous commit, keeping your changes as unstaged files
git reset --soft HEAD~1

# DANGEROUS: Go back to the previous commit, DISCARDING all your changes since then
git reset --hard HEAD~1
```

### `git clean`
Removes untracked files from your working directory. Useful for ensuring a clean build environment.
```bash
# First, do a dry run to see what would be deleted
git clean -n -d

# Forcefully remove all untracked files and directories
git clean -f -d
```

---

## Tagging for Releases

Tagging is critical for DevOps. It creates a snapshot of a commit, typically used to mark release versions (e.g., `v1.0.2`).

### `git tag`
Manages your tags.
```bash
# Create an annotated tag for a release (recommended)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Push the tag to the remote repository (git push does not do this by default)
git push origin v1.0.0

# To push all your local tags at once
git push origin --tags

# Deleting a remote tag requires a specific syntax
git push --delete origin v1.0.0
```
CI/CD systems are often configured to trigger a deployment pipeline when a new tag is pushed.

---

## DevOps Best Practices

### `.gitignore`
A crucial file that tells Git which files or folders to ignore. This prevents temporary files, build artifacts, and sensitive data from being committed.

**Example `.gitignore`:**
```
# Dependencies
node_modules/
vendor/

# Build output
dist/
build/

# Log files
*.log

# Environment variables
.env
```

### CI/CD Workflow Example
A typical DevOps workflow for a feature branch looks like this:
1.  `git switch -c feature/my-new-feature` - Create a branch.
2.  *...write code...*
3.  `git add .` & `git commit -m "feat: Implement my new feature"` - Commit work.
4.  `git push -u origin feature/my-new-feature` - Push to remote. This often triggers automated tests in a CI system.
5.  Create a Pull Request (PR) in GitHub/GitLab.
6.  After PR is approved, merge it into `main`.
7.  `git tag -a v1.1.0 -m "Release v1.1.0"` - Tag the new `main` commit.
8.  `git push origin v1.1.0` - Push the tag. This triggers the deployment pipeline.
