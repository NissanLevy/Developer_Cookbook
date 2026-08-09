# GitHub Branch Protection — Implementation Spec

Implementation instructions for DevOps to configure GitHub repository settings that enforce the [Git Flow](GitFlow.md) model. This is meant to be handed off and implemented as-is (via the UI, `gh` CLI, or Terraform) — see [Verification Checklist](#9-verification-checklist) to confirm it's wired up correctly afterward.

## Table of Contents

1. [Branches to Protect](#1-branches-to-protect)
2. [Protection Rules: `main`](#2-protection-rules-main)
3. [Protection Rules: `develop`](#3-protection-rules-develop)
4. [Repository-Level Merge Settings](#4-repository-level-merge-settings)
5. [Tag Protection](#5-tag-protection)
6. [Other Repository Settings](#6-other-repository-settings)
7. [Implementation via GitHub UI](#7-implementation-via-github-ui)
8. [Implementation via GitHub CLI / API](#8-implementation-via-github-cli--api)
9. [Verification Checklist](#9-verification-checklist)

---

## 1. Branches to Protect

Only **`main`** and **`develop`** need standing branch protection rules. `feature/*`, `release/*`, and `hotfix/*` are short-lived (see [GitFlow.md § Branching Strategy](GitFlow.md#1-branching-strategy)) — they don't need their own protection, because the enforcement point is the PR merge *into* `main`/`develop`, not the working branch itself.

---

## 2. Protection Rules: `main`

Branch name pattern: **`main`**

| Setting | Value |
|---|---|
| Require a pull request before merging | ✅ Enabled |
| Required approving reviews | **2** |
| Dismiss stale reviews on new commits | ✅ Enabled |
| Require review from Code Owners | Optional — enable only if a `CODEOWNERS` file exists |
| Require status checks to pass before merging | ✅ Enabled |
| Require branches to be up to date before merging | ✅ Enabled |
| Required status checks | Your CI build + test job names (e.g. `ci/build`, `ci/test`) |
| Require conversation resolution before merging | ✅ Enabled |
| Require signed commits | Optional — enable if org policy mandates it (see [GitFlow.md § Security](GitFlow.md#12-security)) |
| Require linear history | ❌ **Disabled** — Git Flow relies on `--no-ff` merge commits from `release/*`/`hotfix/*`, which a linear-history requirement would block |
| Do not allow bypassing the above settings | ✅ Enabled — applies to administrators too, no exceptions |
| Restrict who can push to matching branches | Nobody — all changes land via PR merge only |
| Allow force pushes | ❌ Disabled |
| Allow deletions | ❌ Disabled |

## 3. Protection Rules: `develop`

Branch name pattern: **`develop`** — mirrors `main`, slightly relaxed on review count since this is the day-to-day integration branch, not production.

| Setting | Value |
|---|---|
| Require a pull request before merging | ✅ Enabled |
| Required approving reviews | **1** |
| Dismiss stale reviews on new commits | ✅ Enabled |
| Require status checks to pass before merging | ✅ Enabled |
| Require branches to be up to date before merging | ✅ Enabled |
| Required status checks | Same CI checks as `main` |
| Require conversation resolution before merging | ✅ Enabled |
| Require linear history | ❌ Disabled (same reasoning as `main`) |
| Do not allow bypassing the above settings | ✅ Enabled |
| Restrict who can push to matching branches | Nobody |
| Allow force pushes | ❌ Disabled |
| Allow deletions | ❌ Disabled |

---

## 4. Repository-Level Merge Settings

Under **Settings → General → Pull Requests**:

| Setting | Value | Why |
|---|---|---|
| Allow merge commits | ✅ Enabled | Required for `release/*`/`hotfix/*` → `main`/`develop` merges, to preserve `--no-ff` history per [GitFlow.md § Pull Requests](GitFlow.md#7-pull-requests) |
| Allow squash merging | ✅ Enabled | Used for `feature/*` → `develop` merges |
| Allow rebase merging | ❌ Disabled | Rebasing should happen locally by the branch owner before opening/updating a PR, not via the merge button |
| Automatically delete head branches | ✅ Enabled | Matches the branch-lifetime rules in [GitFlow.md § Branching Strategy](GitFlow.md#1-branching-strategy) |
| Always suggest updating pull request branches | ✅ Enabled | Keeps PRs current with their base before merge |

**Convention to document in your PR template:**
- `feature/*` → `develop`: use **Squash and merge**
- `release/*` or `hotfix/*` → `main`/`develop`: use **Create a merge commit**

---

## 5. Tag Protection

Under **Settings → Tags → New rule**, pattern: **`v*`**

Restrict who can create or delete matching tags to release managers (or whatever team/role owns cutting releases). This prevents version tags from being accidentally overwritten or deleted by anyone with general write access — tags are how [GitFlow.md § Releases](GitFlow.md#4-releases) marks what's actually in production.

---

## 6. Other Repository Settings

- **Default branch: `develop`.** Most day-to-day PRs target `develop`, so this reduces the chance of someone accidentally opening a PR against `main`. `main` remains fully protected regardless of which branch is set as default.
- **Require commit signing org-wide:** optional, per organization policy.

---

## 7. Implementation via GitHub UI

1. Go to **Settings → Branches → Branch protection rules → Add rule**.
2. Enter `main` as the branch name pattern, apply all settings from [Section 2](#2-protection-rules-main), save.
3. Repeat with `develop` and the settings from [Section 3](#3-protection-rules-develop).
4. Go to **Settings → General → Pull Requests**, apply the merge settings from [Section 4](#4-repository-level-merge-settings).
5. Go to **Settings → Tags → New rule**, add the `v*` pattern from [Section 5](#5-tag-protection).
6. Go to **Settings → General → Default branch**, set it to `develop`.

> **Note:** GitHub's newer **Repository Rulesets** (Settings → Rules → Rulesets) are the modern replacement for classic branch protection, and support glob patterns (e.g. protecting `release/*` and `hotfix/*` too) plus tag rules in one unified, exportable JSON config. If available on your plan, prefer Rulesets over classic branch protection — the settings above map directly, just organized differently.

---

## 8. Implementation via GitHub CLI / API

For applying this consistently across multiple repos, script it instead of clicking through the UI per repo.

**`main-protection.json`:**
```json
{
  "required_status_checks": {
    "strict": true,
    "contexts": ["ci/build", "ci/test"]
  },
  "enforce_admins": true,
  "required_pull_request_reviews": {
    "required_approving_review_count": 2,
    "dismiss_stale_reviews": true,
    "require_code_owner_reviews": false
  },
  "restrictions": null,
  "required_linear_history": false,
  "allow_force_pushes": false,
  "allow_deletions": false,
  "required_conversation_resolution": true
}
```

**`develop-protection.json`:** same as above, with `"required_approving_review_count": 1`.

Apply with the GitHub CLI:
```bash
gh api --method PUT repos/OWNER/REPO/branches/main/protection --input main-protection.json
gh api --method PUT repos/OWNER/REPO/branches/develop/protection --input develop-protection.json
```

Replace `ci/build` / `ci/test` with your actual required status check context names — these must exactly match the check names your CI reports, or GitHub won't recognize them as satisfiable.

To roll this out across many repos, loop over a repo list:
```bash
for repo in repo-a repo-b repo-c; do
  gh api --method PUT "repos/OWNER/$repo/branches/main/protection" --input main-protection.json
  gh api --method PUT "repos/OWNER/$repo/branches/develop/protection" --input develop-protection.json
done
```

---

## 9. Verification Checklist

After implementing, confirm each of these (a throwaway test repo/branch is the safest way to check the destructive ones):

- [ ] Direct push to `main` is rejected, including for repo admins.
- [ ] Direct push to `develop` is rejected, including for repo admins.
- [ ] A PR into `main` requires 2 approvals and passing CI before the merge button is enabled.
- [ ] A PR into `develop` requires 1 approval and passing CI before the merge button is enabled.
- [ ] Both **Squash and merge** and **Create a merge commit** are available as merge options.
- [ ] Force-push to `main` or `develop` is rejected.
- [ ] Deleting `main` or `develop` is rejected.
- [ ] A `v*` tag cannot be deleted or overwritten by a non-release-manager.
- [ ] Feature branches auto-delete after their PR merges.
- [ ] Default branch is `develop`.
