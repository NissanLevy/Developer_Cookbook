# How to Open a Jira Ticket

A step-by-step walkthrough for creating a ticket — from picking the project through hitting Create. For *which type* to pick and *how to write* a good summary/description, see [Jira.md](Jira.md); this guide focuses on the mechanical, field-by-field process.

## Table of Contents

1. [Before You Open a Ticket](#1-before-you-open-a-ticket)
2. [General Steps (Any Ticket Type)](#2-general-steps-any-ticket-type)
3. [Type-Specific Checklists](#3-type-specific-checklists)
4. [Fields Reference](#4-fields-reference)
5. [Do's and Don'ts](#5-dos-and-donts)

---

## 1. Before You Open a Ticket

- **Search first.** Check if a ticket already exists for this — duplicates fragment discussion and history across two places instead of one.
- **Confirm it needs to be a ticket at all.** Something answerable in a two-minute Slack message doesn't need a permanent record.
- **Know which type you're creating.** If you're not sure Story vs. Task vs. Bug, use the [decision guide in Jira.md](Jira.md#3-choosing-a-type-quick-decision-guide) first — it's faster to decide before opening the create dialog than to relabel it after.

---

## 2. General Steps (Any Ticket Type)

1. Click **Create** in Jira.
2. Select the correct **Project** — the one matching the codebase/product the work affects.
3. Select the **Issue Type** (see [Jira.md](Jira.md) if unsure which one fits).
4. Write a clear **Summary** — short, specific, action-oriented (see [Jira.md § Writing Good Tickets](Jira.md#4-writing-good-tickets)).
5. Fill in the **Description** with enough context that someone outside the conversation that spawned the ticket can pick it up cold. See [§3](#3-type-specific-checklists) below for what belongs in the description per type.
6. Set **Priority** (P0–P4) — see [Prioritization.md](Prioritization.md) for what each level means.
7. Add **Labels/Components** relevant to the area of the codebase or team — use existing ones where they already exist rather than inventing new variants.
8. Link to the **parent Epic**, if this piece of work belongs to one.
9. Add **acceptance criteria** (Stories/Tasks) or **reproduction steps** (Bugs) — see [§3](#3-type-specific-checklists).
10. Leave **Assignee** blank unless you already know who's picking this up — unassigned tickets go through normal backlog triage.
11. Leave the **Sprint** field empty unless this is being pulled into active work right now — don't pre-assign unrefined backlog items to a sprint.
12. Submit, then double-check formatting/links rendered correctly.

---

## 3. Type-Specific Checklists

Each type needs slightly different content in the description beyond the general steps above.

### Epic
- Framed as an initiative, not a single task ("Add SSO support," not "Implement OAuth token exchange").
- Target release or timeframe (quarter, version), if known.
- A rough scope description — child Stories/Tasks/Bugs will attach as they're created; you don't need to enumerate them all up front.

**Example:**
```
Project:     PLATFORM
Issue Type:  Epic
Summary:     Add SSO support
Priority:    P2
Target:      Q4 2026 (v3.0)
Labels:      auth, security

Description:
Enterprise customers are asking for SSO (SAML/OIDC) so their employees
can log in with the company identity provider instead of a separate
username/password. Scope includes: login UI changes, IdP integration,
session handling, and admin configuration for connecting an IdP.

Out of scope for this epic: SCIM user provisioning (tracked separately
in PLATFORM-512 once this ships).
```

### Story
- "As a [user], I want [capability], so that [benefit]" framing.
- Acceptance criteria as a checklist — what "done" looks like.
- Linked to its parent Epic, if one exists.
- Estimate is usually added later during [Backlog Refinement](AgileMeetings/BacklogRefinement.md), not required at creation.

**Example:**
```
Project:     PLATFORM
Issue Type:  Story
Summary:     Add support for password reset via email
Priority:    P2
Epic Link:   PLATFORM-480 (Add SSO support)   ← or its own epic if unrelated
Labels:      auth

Description:
As a user, I want to reset my password via email, so that I can
regain access to my account without contacting support.

Acceptance Criteria:
- [ ] "Forgot password?" link on the login page sends a reset email
      to the account's registered address.
- [ ] The reset link expires after 1 hour.
- [ ] Following an expired or already-used link shows a clear error
      with an option to request a new one.
- [ ] Successfully resetting the password invalidates all existing
      sessions for that account.
```

### Task
- Clear technical description of what needs to be done and *why* it's needed.
- Linked to a parent Epic if it supports one; standalone is fine if it doesn't.

### Bug

**Mandatory — a bug report without these gets bounced back, not triaged:**
- **Steps to reproduce** — numbered, specific enough that someone who never saw the issue can follow them exactly.
- **Expected behavior** — what should have happened.
- **Actual behavior** — what happened instead.
- **Environment** — which of develop/test/stage/prod, plus browser/OS/app version if relevant.
- **Severity/priority** — set per [Prioritization.md](Prioritization.md); this is what determines whether it gets fixed today or next sprint.

**Strongly recommended, include whenever available:**
- Screenshots, screen recordings, or logs — a stack trace or console error saves the assignee from having to reproduce it just to see what you saw.
- Link to the affected Story/Epic if this is a regression in existing functionality.
- Frequency — does it happen every time, or intermittently? Intermittent bugs need this noted explicitly so they aren't closed as "couldn't reproduce."
- Who's impacted and how many — one internal user hitting an edge case is a different bug than "all customers on checkout."

**Example:**
```
Project:     PLATFORM
Issue Type:  Bug
Summary:     Login redirect loops when session token is expired
Priority:    P1
Environment: prod, Chrome 126 (also confirmed on Safari 17)
Labels:      auth

Description:
Steps to reproduce:
1. Log in and stay idle for longer than the session timeout (30 min).
2. Click any link within the app.

Expected behavior:
Redirected once to the login page with a "session expired" message.

Actual behavior:
Page redirects to /login, which redirects back to the original page,
which redirects to /login again — infinite loop, page never loads.

Frequency: Happens every time once the session has expired.
Impact: Affects any user whose session expires mid-use — high traffic,
several support tickets already filed today.

Logs: attached (auth-service redirect trace, timestamps 14:02–14:03 UTC)
```

### Sub-task
- Linked to exactly **one** parent Story/Task/Bug — sub-tasks don't stand alone.
- Scoped narrowly enough for a single owner to pick up without further breakdown.

### Spike
- The specific question(s) to be answered, stated explicitly.
- An explicit **time-box** ("no more than 2 days").
- Expected output stated (a decision, a doc, a recommendation) — not shippable code.

### Improvement
- Current state vs. desired state.
- A measurable target where possible ("reduce dashboard load time from 4s to under 1s") rather than a vague "make it better."

---

## 4. Fields Reference

| Field | Required? | Guidance |
|---|---|---|
| Project | Always | Matches the codebase/product affected |
| Issue Type | Always | See [Jira.md](Jira.md#3-choosing-a-type-quick-decision-guide) |
| Summary | Always | Short, specific, searchable later |
| Description | Always | Type-specific content per [§3](#3-type-specific-checklists) |
| Priority | Always | P0–P4, see [Prioritization.md](Prioritization.md) |
| Epic Link | When applicable | Stories/Tasks/Bugs only — Epics don't link to a parent |
| Labels/Components | Recommended | Use existing values for consistency |
| Assignee | Optional at creation | Leave blank for backlog triage unless already known |
| Sprint | Only if actively planned | Don't assign to unrefined backlog items |
| Fix Version | At release-planning time | Usually set during refinement/planning, not at creation |
| Attachments | As needed | Screenshots/logs — especially important for Bugs |

---

## 5. Do's and Don'ts

**Do**
- Search before creating, to avoid duplicate tickets splitting the same discussion across two places.
- Write the summary as if a future teammate will be searching for it, not just describing it for yourself right now.
- Pick the correct type up front using [Jira.md's decision guide](Jira.md#3-choosing-a-type-quick-decision-guide).

**Don't**
- Don't leave the description blank with "I'll fill in details later" — an empty ticket just gets bounced back during refinement, costing more time overall.
- Don't set a Sprint or Fix Version on something that hasn't been refined yet.
- Don't open a ticket for something that's genuinely answerable in a quick message instead.
