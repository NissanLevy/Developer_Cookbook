# Definition of Done

Definition of Done (DoD) is the **shared, non-negotiable checklist** that decides whether a piece of work is actually finished — distinct from **acceptance criteria**, which are specific to one ticket ("what does done mean for *this* story"). DoD applies to *every* item at that level, every time.

A Story can meet 100% of its own acceptance criteria and still not be "Done" if it skipped code review or has no tests — that's what DoD catches.

## Table of Contents

1. [How the Levels Relate](#1-how-the-levels-relate)
2. [Definition of Done: User Story](#2-definition-of-done-user-story)
3. [Definition of Done: Epic](#3-definition-of-done-epic)
4. [Definition of Done: Version / Release](#4-definition-of-done-version--release)
5. [Do's and Don'ts](#5-dos-and-donts)

---

## 1. How the Levels Relate

```
Version (release)
  └── Epic
        └── Story / Task / Bug
```

Each level's DoD **builds on** the level below — it doesn't replace it:

- A **Story** can't be Done unless it individually passes the Story-level checklist.
- An **Epic** can't be Done unless every child Story/Task/Bug is Done *and* the epic-level integration/stakeholder checks also pass.
- A **Version** can't be Done unless every included Epic is Done *and* the release-level checks (tagging, deployment approval, rollback plan) also pass.

If a lower level isn't Done, nothing above it can be either — there's no such thing as a "Done" Epic with an incomplete Story inside it.

---

## 2. Definition of Done: User Story

Applies to Stories, Tasks, and Bugs (see [Jira.md](Jira.md) for the distinction between them) — the unit of work planned into a sprint.

- [ ] Code is complete and satisfies the ticket's acceptance criteria.
- [ ] Code has been reviewed and approved via PR, per [GitFlow.md](GitFlow.md) branching rules.
- [ ] Automated tests (unit/integration as appropriate) are written and passing.
- [ ] The full automated test suite is green — no regressions introduced.
- [ ] No known critical/high-severity bugs introduced by the change.
- [ ] Relevant documentation updated (README, API docs, inline comments where non-obvious).
- [ ] Merged into `develop` and verified working in the develop/test environment (see [GitFlow.md § Environments](GitFlow.md#6-environments--deployment-tracking)).
- [ ] Acceptance criteria demonstrated — to the PO, QA, or via an automated check, depending on your process.

**A Story is not Done if:** it's "code complete" but unreviewed, un-tested, or unmerged. "Works on my machine" is not Done.

---

## 3. Definition of Done: Epic

Applies on top of the Story-level checklist — an Epic inherits everything above, plus:

- [ ] **Every** child Story/Task/Bug meets the [Story-level Definition of Done](#2-definition-of-done-user-story).
- [ ] The epic's functionality has been validated **end to end**, as a whole — not just as isolated stories. Individually-Done stories can still combine into a broken feature if no one tested the seams.
- [ ] Demoed to stakeholders (at [Sprint Review](AgileMeetings/SprintReview.md) or a dedicated epic demo) and feedback has been addressed or explicitly deferred.
- [ ] No open P0/P1 bugs related to the epic (see [Prioritization.md](Prioritization.md) for what P0/P1 mean).
- [ ] User-facing documentation and release-notes content for the feature is drafted.
- [ ] Monitoring/metrics/alerting are in place for significant new functionality, if applicable.
- [ ] The Product Owner has explicitly accepted the epic as complete against its original goal — not just "the tickets are closed."

**An Epic is not Done if:** all its stories are closed but no one has verified the feature actually works as a cohesive whole.

---

## 4. Definition of Done: Version / Release

Applies on top of the Epic-level checklist — a Version inherits everything above, plus:

- [ ] Every Epic/Story planned for this version meets its own [Definition of Done](#3-definition-of-done-epic).
- [ ] The release branch has been validated in the **test** environment (see [GitFlow.md § Releases](GitFlow.md#4-releases)).
- [ ] The release has been validated in **stage** and approved by the deployment committee (see [GitFlow.md § Environments & Deployment Tracking](GitFlow.md#6-environments--deployment-tracking)).
- [ ] No open P0 bugs anywhere in the release scope; any open P1s have been explicitly reviewed and accepted (not silently ignored) by the PO — see [Prioritization.md](Prioritization.md).
- [ ] Release notes/changelog are written.
- [ ] The release commit is tagged on `main` per [GitFlow.md § Releases](GitFlow.md#4-releases) (e.g. `v1.5.0`).
- [ ] A rollback plan exists and is understood by whoever is deploying.
- [ ] [DEPLOYMENTS.md](DEPLOYMENTS.md) is updated once deployed to each environment, including prod.
- [ ] Security/compliance checks relevant to the release have passed, if applicable (see [GitFlow.md § Security](GitFlow.md#12-security)).

**A Version is not Done if:** it's tagged and merged to `main` but hasn't actually been deployed and verified live — tagging marks *what* a version is, not that it shipped.

---

## 5. Do's and Don'ts

**Do**
- Treat DoD as a hard gate — an incomplete checklist means the work isn't Done, regardless of deadline pressure.
- Review and update your team's DoD occasionally as your process matures (e.g. adding accessibility checks, performance budgets) — but change it deliberately, not by quietly skipping items under pressure.
- Distinguish DoD (applies to everything at that level) from acceptance criteria (specific to one ticket) — don't conflate the two.

**Don't**
- Don't let "almost done" get reported as Done — partial completion belongs in status updates (see [Daily Update Email Template](AgileMeetings/DailyUpdateEmailTemplate.md)), not in the Done column.
- Don't skip DoD items to hit a deadline without an explicit, visible decision by the PO/team to accept the risk — silent skipping is how untested code reaches production.
- Don't apply a Story's DoD to an Epic or Version as if it were sufficient — each level has checks the ones below it can't catch.
