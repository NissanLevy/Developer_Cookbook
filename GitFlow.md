# Git Flow

A practical reference for teams using **Git Flow**: branching, commits, releases, hotfixes, and the common commands/situations you'll run into.

> Git Flow fits projects with **versioned, scheduled releases** — installed software, libraries with supported versions, or infrequent big-bang deploys. For continuously-deployed services, prefer [GitHub Flow](GithubFlow.md) instead — don't run both models on the same repo.

## Table of Contents

1. [Branching Strategy](#1-branching-strategy)
2. [Commit Messages](#2-commit-messages)
3. [Daily Workflow](#3-daily-workflow)
4. [Releases](#4-releases)
5. [Hotfixes](#5-hotfixes)
6. [Environments & Deployment Tracking](#6-environments--deployment-tracking)
7. [Pull Requests](#7-pull-requests)
8. [Common Commands Cheat Sheet](#8-common-commands-cheat-sheet)
9. [Handling Merge Conflicts](#9-handling-merge-conflicts)
10. [Undoing Mistakes](#10-undoing-mistakes)
11. [.gitignore Best Practices](#11-gitignore-best-practices)
12. [Security](#12-security)
13. [Do's and Don'ts](#13-dos-and-donts)

---

## 1. Branching Strategy

Git Flow uses **two permanent branches** and several **supporting branch types**, each with a specific purpose and lifetime.

### Permanent branches

| Branch | Purpose |
|---|---|
| `main` | Always reflects the **currently released** production code. Every commit on `main` is tagged with a version. |
| `develop` | Integration branch. Latest delivered development changes for the next release live here. |

### Supporting branches

| Type | Branches from | Merges into | Purpose |
|---|---|---|---|
| `feature/*` | `develop` | `develop` | New functionality, in progress work. |
| `release/*` | `develop` | `main` **and** `develop` | Stabilize and prepare a release (version bump, release-only fixes, docs). |
| `hotfix/*` | `main` | `main` **and** `develop` | Urgent fix to production that can't wait for the next release. |
| `support/*` (optional) | `main` (at an old tag) | — | Long-term maintenance of an old released version. |

### Branch naming

```
feature/<short-description>       e.g. feature/user-avatar-upload
release/<version>                 e.g. release/1.4.0
hotfix/<short-description>        e.g. hotfix/payment-timeout
support/<version>                 e.g. support/1.x
```

### Lifetime

- `main` and `develop` live forever.
- `feature/*` branches live until merged into `develop`, then are deleted.
- `release/*` and `hotfix/*` branches live only for the duration of stabilizing/fixing, then are deleted after merging to both `main` and `develop`.
- Keep `feature/*` branches as short-lived as practical — the longer they live, the more they drift from `develop`.

---

## 2. Commit Messages

Same convention regardless of branching model — we use **[Conventional Commits](https://www.conventionalcommits.org/)**.

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
chore(release): bump version to 1.4.0
```

### Rules of thumb

- Subject line: imperative mood, no period, ideally ≤ 72 chars.
- One logical change per commit.
- Use the body to explain **why**, not what.
- Reference issues/tickets in the footer: `Closes #123`.

---

## 3. Daily Workflow

Standard loop for a **feature**:

```bash
# 1. Start from an up-to-date develop
git checkout develop
git pull origin develop

# 2. Create a feature branch
git checkout -b feature/short-description

# 3. Work, committing in small logical chunks
git add <files>
git commit -m "feat(scope): describe the change"

# 4. Keep your branch current with develop (do this often)
git fetch origin
git rebase origin/develop

# 5. Push and open a PR into develop
git push -u origin feature/short-description

# 6. After review + approval, merge via PR, then clean up locally
git checkout develop
git pull origin develop
git branch -d feature/short-description
```

### Using the `git-flow` CLI extension (optional)

Many teams use the [git-flow](https://github.com/nvie/gitflow) extension to script these conventions instead of doing them by hand:

```bash
git flow init                          # one-time repo setup

git flow feature start short-description   # = branch off develop
git flow feature finish short-description  # = merge back into develop, delete branch

git flow release start 1.4.0
git flow release finish 1.4.0          # merges to main + develop, tags main

git flow hotfix start payment-timeout
git flow hotfix finish payment-timeout # merges to main + develop, tags main
```

This is optional — the underlying Git operations are identical either way, and many teams do it manually via PRs so the merges go through code review.

### Rebase vs merge

- **Prefer `rebase`** for updating your own `feature/*` branch with `develop`.
- **Never rebase** `main`, `develop`, or any `release/*`/`hotfix/*` branch others are collaborating on.
- Use `--force-with-lease` (never bare `--force`) if you need to update a pushed branch after a rebase.

---

## 4. Releases

> If you track releases in Jira via Fix Versions, see [JiraReleaseManagement.md](JiraReleaseManagement.md) for how to keep them aligned with the branch/tag steps below.

1. When `develop` has accumulated enough for a release, cut a release branch:
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b release/1.4.0
   ```
2. Only **release-related** changes happen here: version bumps, changelog, release-only bug fixes. No new features.
3. Any fixes made on `release/*` should also be merged back into `develop` (or cherry-picked) so they aren't lost.
4. When stable, merge into `main` **and** tag it:
   ```bash
   git checkout main
   git pull origin main
   git merge --no-ff release/1.4.0
   git tag -a v1.4.0 -m "Release 1.4.0"
   git push origin main --tags
   ```
5. Merge the same release branch back into `develop`:
   ```bash
   git checkout develop
   git pull origin develop
   git merge --no-ff release/1.4.0
   git push origin develop
   ```
6. Delete the release branch.

### Selective / Partial Releases

Sometimes `develop` accumulates multiple merged features, but the PO only wants to ship one of them — e.g. feature #1 and feature #2 are both merged into `develop`, but only feature #2 is approved to deploy right now.

**Don't cut the release branch from `develop`** in this case — it contains both features. Instead, cut it from `main` (the last known-good, currently-deployed state) and cherry-pick only the approved feature onto it:

```bash
# 1. Branch from main, not develop
git checkout main
git pull origin main
git checkout -b release/1.5.0

# 2. Cherry-pick only the approved feature's commit(s)
# If it was squash-merged into develop, it's a single clean commit:
git cherry-pick <feature-2-commit-sha>
# If it was merged with --no-ff (a merge commit), use -m 1 to take the feature side:
git cherry-pick -m 1 <feature-2-merge-commit-sha>

# 3. Resolve conflicts if any, test, get approval, then finish as normal:
git checkout main
git merge --no-ff release/1.5.0
git tag -a v1.5.0 -m "Release 1.5.0 (feature #2 only)"
git push origin main --tags

git checkout develop
git merge --no-ff release/1.5.0
git push origin develop
```

The merge back into `develop` is usually a no-op for the cherry-picked feature (it's already there) and doesn't touch the deferred feature — it simply stays in `develop`, untouched, waiting for a future release.

**Watch out for:** if the deferred feature and the approved feature touch the same files/logic, the cherry-pick can get messy or even impossible to cleanly separate — check for that dependency before committing to this approach.

**If this happens often**, it's a sign to reach for **feature flags** instead: merge freely into `develop`/`main`, keep the feature dark until the PO says go, and avoid the cherry-pick dance entirely by making "shipped" and "enabled" two separate decisions.

---

## 5. Hotfixes

For urgent production issues that can't wait for the next scheduled release:

1. Branch from `main` (not `develop`) at the tag currently in production:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b hotfix/payment-timeout
   ```
2. Fix the issue, commit, bump the patch version.
3. Merge into `main` and tag:
   ```bash
   git checkout main
   git merge --no-ff hotfix/payment-timeout
   git tag -a v1.4.1 -m "Hotfix 1.4.1"
   git push origin main --tags
   ```
4. Merge the same fix into `develop` (and into any active `release/*` branch, if one exists) so it isn't lost in the next release:
   ```bash
   git checkout develop
   git merge --no-ff hotfix/payment-timeout
   git push origin develop
   ```
5. Delete the hotfix branch.

---

## 6. Environments & Deployment Tracking

If your team deploys through a pipeline like `develop → test → stage → prod`, note that these are **deployment environments**, not additional Git branches. Don't create branches called `test`, `stage`, or `prod` — you only ever need `main`, `develop`, and the short-lived supporting branches from [Section 1](#1-branching-strategy). Environments are just deploy *targets* that receive builds of those branches at different points:

```
feature/*  ──merge──▶  develop (branch)
                          │
                          ▼ deploy
                     "develop" environment   ← continuous, every merge
                          │
                (enough features ready)
                          ▼
                  release/1.4.0 (branch)
                          │
                          ▼ build once, deploy
                      "test" environment      ← QA tests this build
                          │
                     (test passes)
                          ▼ same artifact, no rebuild
                     "stage" environment      ← committee reviews this exact build
                          │
                  (committee approves)
                          ▼
              merge release/1.4.0 → main, tag v1.4.0
                          │
                          ▼ deploy that tagged artifact
                      "prod" environment
```

- **`develop` branch → "develop" environment**: deploy on every merge, continuous, no approval gate.
- **`release/*` branch → "test" then "stage"**: build the release branch **once**, and promote that same artifact forward through both environments as each signs off — don't rebuild between them.
- **`main` (tagged) → "prod"**: only after the release branch has cleared test, stage, and committee approval.
- **`hotfix/*`**: same idea, fast-tracked through as many of these stages as urgency allows.

### Knowing what's deployed where

A release/tag name (`v1.4.0`) tells you **what version it is** — it says nothing about **where it's currently deployed**. Since deploys here are manual, track that separately using either or both of:

**A movable tag per environment**, updated at each deploy:
```bash
# after deploying v1.4.0 to stage
git tag -f env/stage v1.4.0
git push origin env/stage --force

# after deploying v1.3.2 to prod
git tag -f env/prod v1.3.2
git push origin env/prod --force
```
This also gives you a quick diff of what hasn't been promoted yet:
```bash
git log env/prod..env/stage --oneline   # what's in stage but not yet prod
```

**A deployment log**, [`DEPLOYMENTS.md`](DEPLOYMENTS.md), updated by hand at each promotion step — see that file for the template. It's the human-readable audit trail (who deployed what, when) that the `env/*` tags don't capture on their own.

---

## 7. Pull Requests

- `feature/*` → PR into `develop`.
- `release/*` → PR into `main`, then a second PR (or direct merge) into `develop`.
- `hotfix/*` → PR into `main`, then a second PR (or direct merge) into `develop`.
- Keep PRs small and focused — one concern per PR.
- Ensure CI is green before requesting review.
- Use `--no-ff` (no fast-forward) merges for `release/*` and `hotfix/*` into `main`/`develop` so the merge point stays visible in history — this matters more in Git Flow than in GitHub Flow, since you need to trace which release/hotfix introduced what.
- Squash-merge is fine for `feature/*` → `develop`; avoid squashing `release/*`/`hotfix/*` merges since you want the merge commit preserved.

---

## 8. Common Commands Cheat Sheet

### Status & history

```bash
git status
git log --oneline --graph --all -20    # visualize all branches at once — helpful in Git Flow
git diff
git diff --staged
```

### Staging & committing

```bash
git add <file>
git add -p
git commit -m "message"
git commit --amend
```

### Branching

```bash
git branch -a                          # list all branches
git checkout -b feature/name develop   # branch explicitly off develop
git branch -d feature/name             # delete a merged branch
```

### Tags (releases live here)

```bash
git tag                                # list tags
git tag -a v1.4.0 -m "Release 1.4.0"   # annotated tag
git push origin --tags                 # push tags to remote
git checkout v1.4.0                    # inspect a specific release
```

### Syncing

```bash
git fetch origin
git pull origin develop
git push
git push -u origin <branch>
```

### Stashing

```bash
git stash
git stash pop
git stash list
```

---

## 9. Handling Merge Conflicts

1. Pull/rebase/merge and Git will flag conflicted files:
   ```bash
   git status
   ```
2. Resolve the markers in each conflicted file:
   ```
   <<<<<<< HEAD
   your current branch's version
   =======
   incoming version
   >>>>>>> branch-name
   ```
3. Stage the resolved files and continue:
   ```bash
   git add <resolved-file>
   git rebase --continue   # if rebasing
   # or
   git commit               # if merging
   ```
4. Bail out safely if needed:
   ```bash
   git rebase --abort
   git merge --abort
   ```
5. Conflicts are most common when merging `release/*`/`hotfix/*` back into `develop` after `develop` has moved on — run the test suite after resolving, not just after CI passes.

---

## 10. Undoing Mistakes

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
| Remove a bad tag that was already pushed | `git tag -d v1.4.0 && git push origin :refs/tags/v1.4.0` |

**Rule of thumb:** never `reset --hard` or force-push on `main` or `develop` — they're shared, permanent branches. Use `revert` instead.

---

## 11. .gitignore Best Practices

- Every repo should have a `.gitignore` from the first commit.
- Never commit: dependency folders (`node_modules/`, `vendor/`), build output (`dist/`, `build/`), local env files (`.env`), IDE settings, OS cruft (`.DS_Store`, `Thumbs.db`).
- If a secret or unwanted file is committed, don't just delete it in a new commit — see [Security](#12-security).

---

## 12. Security

- **Never commit secrets**: API keys, passwords, tokens, private keys.
- If a secret is committed:
  1. Rotate/revoke it immediately.
  2. Remove it from history with `git filter-repo`.
  3. Force-push the cleaned history and have collaborators re-clone — coordinate first, this affects `main`/`develop` for everyone.
- Sign release tags: `git tag -s v1.4.0 -m "Release 1.4.0"` so consumers can verify authenticity.
- Enable branch protection on `main` and `develop`: require PR review, require status checks, disallow force-push and direct pushes. See [BranchProtectionRules.md](BranchProtectionRules.md) for the exact settings to hand off to DevOps.

---

## 13. Do's and Don'ts

**Do**
- Branch features off `develop`, never off `main`.
- Branch hotfixes off `main`, never off `develop`.
- Merge every `release/*` and `hotfix/*` into **both** `main` and `develop`.
- Tag every commit that lands on `main`.
- Use `--no-ff` merges for release/hotfix branches so history shows where they landed.

**Don't**
- Don't commit directly to `main` or `develop`.
- Don't let a release branch pick up new feature work — only stabilization fixes.
- Don't forget to merge a hotfix back into `develop` — it's the most common way regressions creep back in.
- Don't force-push `main` or `develop`.
- Don't run Git Flow and GitHub Flow simultaneously on the same repo — pick one per project.
