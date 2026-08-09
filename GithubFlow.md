# Git Guidelines

A practical reference for how we use Git day to day: branching, commits, pull requests, and the common commands/situations you'll run into.

## Table of Contents

1. [Branching Strategy](#1-branching-strategy)
2. [Commit Messages](#2-commit-messages)
3. [Daily Workflow](#3-daily-workflow)
4. [Pull Requests](#4-pull-requests)
5. [Common Commands Cheat Sheet](#5-common-commands-cheat-sheet)
6. [Handling Merge Conflicts](#6-handling-merge-conflicts)
7. [Undoing Mistakes](#7-undoing-mistakes)
8. [.gitignore Best Practices](#8-gitignore-best-practices)
9. [Security](#9-security)
10. [Do's and Don'ts](#10-dos-and-donts)

---

## 1. Branching Strategy

We use **GitHub Flow** — simple, trunk-based, and optimized for shipping continuously.

- `main` is always deployable. Nothing broken ever sits on `main`.
- All work happens on short-lived **feature branches** created off `main`.
- Branches are merged back into `main` via **Pull Request**, then deleted.
- No long-lived `develop`, `release`, or `staging` branches. If a release process is needed, it's handled with tags, not branches.

### Branch naming

Use a consistent, descriptive pattern:

```
<type>/<short-description>
```

| Type       | Use for                                  | Example                        |
|------------|-------------------------------------------|---------------------------------|
| `feature/` | New functionality                         | `feature/user-avatar-upload`   |
| `fix/`     | Bug fixes                                  | `fix/login-redirect-loop`      |
| `chore/`   | Tooling, deps, config, non-code maintenance | `chore/upgrade-node-20`      |
| `docs/`    | Documentation only                         | `docs/api-auth-examples`       |
| `refactor/`| Code change with no behavior change        | `refactor/extract-auth-service`|
| `hotfix/`  | Urgent production fix                      | `hotfix/payment-timeout`       |

- Keep names short, lowercase, hyphen-separated.
- Include a ticket number if you use an issue tracker: `feature/JIRA-123-avatar-upload`.

### Branch lifetime

- Keep branches short-lived (ideally < 2-3 days). Long-lived branches drift from `main` and cause painful merges.
- If a feature is large, break it into smaller PRs behind a feature flag rather than keeping one giant branch alive for weeks.
- Delete branches after merging (GitHub can do this automatically on merge).

---

## 2. Commit Messages

We follow **[Conventional Commits](https://www.conventionalcommits.org/)**. It keeps history readable and enables automated changelogs.

### Format

```
<type>(<optional scope>): <short summary>

<optional body>

<optional footer>
```

### Types

| Type       | Meaning                                      |
|------------|-----------------------------------------------|
| `feat`     | A new feature                                 |
| `fix`      | A bug fix                                     |
| `docs`     | Documentation changes only                    |
| `style`    | Formatting, whitespace, no code meaning change|
| `refactor` | Code change that's neither a fix nor a feature|
| `perf`     | Performance improvement                       |
| `test`     | Adding or fixing tests                        |
| `build`    | Build system or dependency changes            |
| `ci`       | CI/CD configuration changes                   |
| `chore`    | Everything else (tooling, config, etc.)       |

### Examples

```
feat(auth): add support for magic-link login

fix(cart): prevent duplicate items when checkout retries

refactor(api): extract validation into middleware

chore: bump eslint to v9
```

### Rules of thumb

- Subject line: imperative mood, no period, ideally ≤ 72 chars — "add" not "added" or "adds".
- One logical change per commit. If your commit message needs "and", consider splitting it.
- Use the body to explain **why**, not what (the diff already shows what).
- Reference issues/tickets in the footer: `Closes #123` or `Refs JIRA-456`.
- Squash noisy WIP commits before merging (see [Pull Requests](#4-pull-requests)).

---

## 3. Daily Workflow

Standard loop for shipping a change under GitHub Flow:

```bash
# 1. Start from an up-to-date main
git checkout main
git pull origin main

# 2. Create a feature branch
git checkout -b feature/short-description

# 3. Work, committing in small logical chunks
git add <files>
git commit -m "feat(scope): describe the change"

# 4. Keep your branch current with main (do this often, not just at the end)
git fetch origin
git rebase origin/main

# 5. Push and open a PR
git push -u origin feature/short-description

# 6. After review + approval, merge via the PR (squash merge preferred)
#    Then clean up locally:
git checkout main
git pull origin main
git branch -d feature/short-description
```

### Keeping branches in sync: rebase vs merge

- **Prefer `rebase`** for updating your own feature branch with `main` — it keeps history linear and avoids noisy merge commits.
- **Never rebase a shared/public branch** that others are also working on — it rewrites history and breaks their clones.
- Use `git merge` when integrating `main` into a branch that others are actively collaborating on with you.

```bash
# Update your feature branch (safe: only you use this branch)
git fetch origin
git rebase origin/main

# If you've already pushed the branch and need to update the remote after a rebase:
git push --force-with-lease
```

> Always use `--force-with-lease`, never bare `--force`. It fails safely if someone else pushed to the branch since you last fetched.

---

## 4. Pull Requests

- Keep PRs **small and focused** — one concern per PR. Aim for something reviewable in 15-30 minutes.
- Write a clear description: what changed, why, and how to test it. Link the related issue/ticket.
- Ensure CI is green before requesting review.
- Use **draft PRs** for work-in-progress you want early feedback on.
- Squash-merge into `main` by default so `main` history stays one commit per PR. Keep merge commits only when preserving a meaningful multi-commit history matters.
- Respond to review comments with either a fix or a reasoned reply — don't silently dismiss feedback.
- Re-request review after pushing changes so reviewers know to look again.

### PR description template

```markdown
## What
Brief summary of the change.

## Why
Context / problem this solves. Link ticket: Closes #123

## How to test
Steps to verify locally.

## Screenshots (if UI)
```

---

## 5. Common Commands Cheat Sheet

### Status & history

```bash
git status                     # what's staged/unstaged/untracked
git log --oneline --graph -20  # compact visual history
git diff                       # unstaged changes
git diff --staged              # staged changes
git blame <file>                # who last changed each line
```

### Staging & committing

```bash
git add <file>                 # stage a specific file
git add -p                     # stage interactively, hunk by hunk
git commit -m "message"        # commit staged changes
git commit --amend             # edit the last commit (message and/or content)
```

### Branching

```bash
git branch                     # list local branches
git branch -a                  # list all branches (incl. remote)
git checkout -b <name>          # create + switch to new branch
git switch <name>               # switch branches (modern alternative)
git branch -d <name>            # delete a merged branch
git branch -D <name>            # force-delete an unmerged branch
```

### Syncing

```bash
git fetch origin               # download remote changes, don't merge
git pull origin main            # fetch + merge main into current branch
git pull --rebase origin main   # fetch + rebase instead of merge
git push                        # push current branch
git push -u origin <branch>     # push and set upstream tracking
```

### Stashing (save work without committing)

```bash
git stash                      # shelve current changes
git stash pop                  # reapply the most recent stash
git stash list                 # see all stashes
git stash drop                 # discard a stash
```

### Inspecting

```bash
git show <commit>              # view a specific commit's changes
git log --author="name"        # commits by a specific person
git log -- <file>               # history of a specific file
```

---

## 6. Handling Merge Conflicts

1. Pull/rebase and Git will flag conflicted files:
   ```bash
   git status   # shows files "both modified"
   ```
2. Open each conflicted file and resolve the markers:
   ```
   <<<<<<< HEAD
   your current branch's version
   =======
   incoming version
   >>>>>>> branch-name
   ```
3. Remove the markers, keep the correct combined result, then:
   ```bash
   git add <resolved-file>
   git rebase --continue   # if rebasing
   # or
   git commit               # if merging
   ```
4. If a rebase goes sideways, you can always bail out safely:
   ```bash
   git rebase --abort
   ```
5. Run tests locally after resolving conflicts before pushing — conflict resolution is a common source of silent bugs.

---

## 7. Undoing Mistakes

Git rarely loses work permanently — know these before you panic.

| Situation | Command |
|---|---|
| Undo uncommitted changes to a file | `git restore <file>` |
| Unstage a file (keep changes) | `git restore --staged <file>` |
| Change the last commit message | `git commit --amend` |
| Undo the last commit, keep changes staged | `git reset --soft HEAD~1` |
| Undo the last commit, keep changes unstaged | `git reset HEAD~1` |
| Discard the last commit entirely (dangerous) | `git reset --hard HEAD~1` |
| Undo a commit that's already pushed/public | `git revert <commit>` |
| Recover a "lost" commit or branch | `git reflog` then `git checkout <sha>` |

**Rule of thumb:** if a commit has already been pushed and others might have pulled it, use `revert` (adds a new commit undoing the change) instead of `reset`/`rebase` (rewrites history).

`git reflog` is your safety net — it records every place `HEAD` has pointed, even after a `reset --hard`. Almost nothing is truly gone until garbage collection runs, and even then rarely soon.

---

## 8. .gitignore Best Practices

- Every repo should have a `.gitignore` from the first commit — retrofitting it after secrets or build artifacts are committed doesn't remove them from history.
- Never commit: dependency folders (`node_modules/`, `vendor/`), build output (`dist/`, `build/`), local env files (`.env`, `.env.local`), IDE settings (`.vscode/`, `.idea/` — unless intentionally shared), OS cruft (`.DS_Store`, `Thumbs.db`).
- If something was committed by mistake and needs to be scrubbed from history entirely (e.g. a secret), don't just delete it in a new commit — see [Security](#9-security).

---

## 9. Security

- **Never commit secrets**: API keys, passwords, tokens, private keys. Use environment variables or a secrets manager instead.
- If a secret is committed:
  1. Rotate/revoke the secret immediately — treat it as compromised regardless of whether the repo is private.
  2. Remove it from history with a tool built for it (`git filter-repo` is preferred over the older `BFG Repo-Cleaner`/`filter-branch`).
  3. Force-push the cleaned history and have all collaborators re-clone (this is disruptive — coordinate with the team first).
- Sign commits with GPG/SSH signing (`git config commit.gpgsign true`) if your org requires verified commits.
- Enable branch protection on `main`: require PR review, require status checks to pass, disallow force-push and direct pushes.

---

## 10. Do's and Don'ts

**Do**
- Commit early and often in small, logical chunks.
- Pull/rebase from `main` frequently to avoid painful conflicts later.
- Write commit messages for the next person reading the history (including future you).
- Use `--force-with-lease` instead of `--force`.
- Open small PRs.

**Don't**
- Don't rebase or force-push a branch other people are actively working on.
- Don't commit directly to `main` — always go through a PR.
- Don't commit generated files, dependencies, or secrets.
- Don't use `git reset --hard` or `git clean -fd` without checking `git status` first.
- Don't let feature branches live for weeks — split the work instead.
