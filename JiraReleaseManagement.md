# Jira Release Management & Git Flow Alignment

How to use Jira's Release (Fix Version) feature so it stays a truthful mirror of what's actually happening in [Git Flow](GitFlow.md) — instead of two systems that quietly drift apart.

## Table of Contents

1. [What Is a Jira Release (Fix Version)?](#1-what-is-a-jira-release-fix-version)
2. [Mapping Jira Releases to Git Flow](#2-mapping-jira-releases-to-git-flow)
3. [The Release Lifecycle, Step by Step](#3-the-release-lifecycle-step-by-step)
4. [Naming Convention](#4-naming-convention)
5. [Handling Tickets That Don't Make the Cut](#5-handling-tickets-that-dont-make-the-cut)
6. [Hotfix Releases](#6-hotfix-releases)
7. [Release Notes](#7-release-notes)
8. [Do's and Don'ts](#8-dos-and-donts)

---

## 1. What Is a Jira Release (Fix Version)?

A **Fix Version** (Jira's release object) is a container that groups tickets intended to ship together — independent of any single sprint. It gives you a release burndown, auto-generated release notes, and one record answering "what shipped in 1.5.0."

**Releases and sprints are orthogonal, not the same thing.** A release can span several sprints, and a single sprint's work can split across two releases (e.g. most of it lands in the next planned release, but one P0 fix ships immediately as a hotfix — see [§6](#6-hotfix-releases)). Don't assume "the sprint" and "the release" are the same container.

---

## 2. Mapping Jira Releases to Git Flow

| Jira concept | Git Flow equivalent |
|---|---|
| Fix Version created, status "Unreleased" | `release/x.y.z` branch not yet cut, or currently being stabilized |
| A ticket's Fix Version field is set | The ticket's code is confirmed merged into that `release/x.y.z` branch |
| Fix Version marked **Released**, release date set | `vX.Y.Z` tag merged to `main` **and** actually deployed to prod (see [DEPLOYMENTS.md](DEPLOYMENTS.md)) |
| Release Notes (auto-generated from the Fix Version) | The changelog for that git tag |

```
Jira                              Git Flow
────────────────────────────────  ──────────────────────────────
Fix Version "1.5.0" created   ──▶ (planning: scope is firming up)
  │
  ▼ tickets get Fix Version set
Tickets scoped into 1.5.0     ──▶ release/1.5.0 branch cut from main
  │                                       │
  ▼                                       ▼ test → stage → committee approval
Release burndown tracked      ──▶ release/1.5.0 stabilizing
  │                                       │
  ▼                                       ▼ merge to main, tag v1.5.0
Fix Version marked "Released" ──▶ v1.5.0 deployed to prod,
  (release date = prod date)             DEPLOYMENTS.md updated
  │
  ▼
Release Notes generated
```

---

## 3. The Release Lifecycle, Step by Step

1. **Create the Fix Version in Jira** as soon as release scope starts firming up — typically around [Backlog Refinement](AgileMeetings/BacklogRefinement.md) or [Sprint Planning](AgileMeetings/SprintPlanning.md). Name it to exactly match the intended version number (see [§4](#4-naming-convention)).
2. **Set the Fix Version field** on Stories/Tasks/Bugs as they're confirmed to be in scope. Don't set it on unrefined backlog items just to look organized — same reasoning as not pre-assigning a Sprint to unready work (see [Jira.md](Jira.md#4-writing-good-tickets)).
3. **Cut `release/x.y.z`** from `main` once scope is firm, per [GitFlow.md § Releases](GitFlow.md#4-releases).
4. **Track progress via the Fix Version's release burndown** — but treat it as aspirational until reconciled against reality: a ticket's Fix Version field being set doesn't mean its code actually landed in the release branch. Reconcile the two before calling the release ready.
5. Once the release branch clears test, stage, and [deployment committee approval](Prioritization.md#5-who-decides), merge to `main` and tag `vX.Y.Z`.
6. **Deploy to prod**, update [DEPLOYMENTS.md](DEPLOYMENTS.md).
7. **Back in Jira:** set the Fix Version's release date to the actual prod deployment date — not the tag date, not the stage deployment date — then mark it **Released**. Generate release notes (see [§7](#7-release-notes)).
8. Create the **next** Fix Version for whatever's coming after.

This lifecycle is also the version-level bar from [DefinitionOfDone.md § Version/Release](DefinitionOfDone.md#4-definition-of-done-version--release) — a Fix Version shouldn't be marked Released until that checklist is satisfied.

---

## 4. Naming Convention

**The Jira Fix Version name must exactly match the git tag** (the `v` prefix aside): Fix Version `1.5.0` ↔ git tag `v1.5.0` ↔ branch `release/1.5.0`.

This isn't cosmetic — the entire mapping in [§2](#2-mapping-jira-releases-to-git-flow) only works if the names line up. A Fix Version called "July Release" next to a tag called `v1.5.0` means nobody can look at either system and know they're the same thing.

---

## 5. Handling Tickets That Don't Make the Cut

Sometimes a ticket carries a Fix Version but ends up not actually shipping in it — see [GitFlow.md § Selective / Partial Releases](GitFlow.md#selective--partial-releases) for the git side of this (cherry-picking one feature while deferring another).

**When this happens, move the ticket's Fix Version to whichever release it will actually ship in** before marking the original version Released. Jira's "bulk change" / release-closing tool offers exactly this ("move all unresolved issues to another version") — use it as the last step before closing a release, not as an afterthought.

**Never leave a ticket pointing at a Fix Version it didn't actually ship in.** That's what makes release notes and later audits ("wait, did 1.5.0 include this fix or not?") unreliable.

---

## 6. Hotfix Releases

A [hotfix](GitFlow.md#5-hotfixes) gets its **own** Fix Version (e.g. `1.5.1`), separate from whatever the next planned release is (`1.6.0`), even though the fix will also be present in `1.6.0` once it merges into `develop`.

The hotfix ticket's Fix Version should be set to `1.5.1` **only** — not also `1.6.0`. The field records *which release first shipped this fix*, not everywhere its commit eventually exists. If it's genuinely useful to know a fix is present in both, use a comment or link rather than double-assigning Fix Version.

---

## 7. Release Notes

Jira can auto-generate release notes from every ticket attached to a **Released** Fix Version, typically grouped by issue type.

- Write **Story** summaries with this in mind for anything customer-facing — "Add password reset via email" reads fine in release notes handed to a customer; "Fix thing PM asked about" does not. See [Jira.md § Writing Good Tickets](Jira.md#4-writing-good-tickets).
- Consider excluding internal-only Tasks/tech-debt from customer-facing notes — most Jira release-notes views let you filter by issue type or label.

---

## 8. Do's and Don'ts

**Do**
- Create the Fix Version before cutting the release branch, and name them identically.
- Reconcile Fix Version ticket assignments against what's actually in the release branch before marking it Released.
- Set the release date to the actual prod deployment date, not the tag or stage date.
- Move deferred tickets to their real Fix Version before closing out the old one.

**Don't**
- Don't assign a Fix Version to unrefined backlog items.
- Don't leave a ticket's Fix Version pointing at a release it didn't ship in.
- Don't let Jira's "Released" status drift out of sync with [DEPLOYMENTS.md](DEPLOYMENTS.md) — they should always agree on what's actually live.
- Don't double-assign a hotfix's Fix Version to both the hotfix version and the next planned release.
