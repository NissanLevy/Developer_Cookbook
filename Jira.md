# Jira Ticket Types

A practical guide to the standard Jira issue types, what each one means, and when to use it. Picking the right type keeps backlogs reportable (velocity, burndown, and release tracking all rely on consistent typing) and makes it obvious to anyone browsing the board what kind of work a ticket represents.

> Looking for the step-by-step process of actually creating a ticket — fields, checklists per type? See [HowToOpenATicket.md](HowToOpenATicket.md).

## Table of Contents

1. [Ticket Hierarchy](#1-ticket-hierarchy)
2. [Ticket Types](#2-ticket-types)
3. [Choosing a Type: Quick Decision Guide](#3-choosing-a-type-quick-decision-guide)
4. [Writing Good Tickets](#4-writing-good-tickets)
5. [Do's and Don'ts](#5-dos-and-donts)

---

## 1. Ticket Hierarchy

```
Epic
 └── Story / Task / Bug
       └── Sub-task
```

- **Epic** — a large body of work, spanning multiple sprints/releases, made up of several Stories/Tasks/Bugs.
- **Story / Task / Bug** — the unit of work planned into a sprint. This is the level velocity is measured at.
- **Sub-task** — a breakdown of a Story/Task/Bug into smaller technical steps, owned by one person, not independently estimated against velocity.

A ticket at the Story/Task/Bug level should always link up to an Epic where one exists. Sub-tasks always belong to exactly one parent — they don't exist standalone.

---

## 2. Ticket Types

### Epic

**What it is:** A large initiative or feature area too big to deliver in a single sprint.

**When to use it:**
- The work will span multiple sprints or releases.
- It's made up of several distinct, independently shippable pieces of work.
- You want a single place to track overall progress toward a bigger goal.

**Example:** *"Add SSO support"* — an epic containing stories for the login UI, identity-provider integration, session handling, and admin configuration.

**Don't use it for:** A single piece of work that fits in one sprint — that's a Story or Task with no epic needed, or a small epic is just unnecessary overhead.

---

### Story

**What it is:** A user-facing piece of functionality, described from the perspective of the end user (or another system consuming the feature).

**When to use it:**
- The work delivers observable value to a user, customer, or another team.
- It can typically be described as "As a [user], I want [capability], so that [benefit]."
- It fits within a single sprint.

**Example:** *"As a user, I want to reset my password via email, so that I can regain access without contacting support."*

**Don't use it for:** Pure internal/technical work with no user-facing behavior change — use Task instead.

---

### Task

**What it is:** A piece of work that needs to get done but isn't naturally framed as user-facing functionality — internal, technical, or operational work.

**When to use it:**
- Refactoring, tooling changes, dependency upgrades, infrastructure work, documentation, research that doesn't fit the "Spike" definition, etc.
- The work is well-understood and doesn't need a user-story framing.

**Example:** *"Upgrade the CI pipeline to use Node 20"*, *"Add integration test coverage for the payment service"*.

**Don't use it for:** Anything with a clear user-facing benefit — frame that as a Story so it's visible in user-value reporting.

---

### Bug

**What it is:** A defect — something that isn't working as intended, found in already-delivered functionality.

**When to use it:**
- Something that used to work (or was specified to work) is broken.
- Reported by QA, support, monitoring/alerting, or a user.

**Example:** *"Login redirect loops when session token is expired."*

**Include in the description:**
- Steps to reproduce
- Expected vs. actual behavior
- Environment (which of develop/test/stage/prod, browser/OS/version if relevant)
- Severity/priority (see [Prioritization.md](Prioritization.md) for what P0–P4 mean and how to set them) and any relevant logs or screenshots

**Don't use it for:** A feature that was never built — that's a Story or Task, not a Bug, even if a user is unhappy about its absence.

---

### Sub-task

**What it is:** A concrete step within a Story, Task, or Bug — the actual technical breakdown of how the parent gets done.

**When to use it:**
- The parent ticket is big enough that splitting it across multiple people or multiple distinct steps clarifies ownership.
- Common split: "backend change," "frontend change," "write tests," "update docs" as separate sub-tasks of one Story.

**Example:** Parent Story *"Add password reset via email"* → Sub-tasks: *"Add reset-token endpoint," "Build reset-password UI," "Send reset email via notification service."*

**Don't use it for:** Work that could stand alone and be prioritized independently — that should be its own Story/Task, not buried as a sub-task.

---

### Spike (if enabled on your board)

**What it is:** A time-boxed research/investigation task where the *output* is knowledge or a decision, not shippable code.

**When to use it:**
- You need to answer a technical question or evaluate an approach before real implementation work can be estimated.
- The effort needs to be time-boxed (e.g. "spend no more than 2 days") rather than open-ended.

**Example:** *"Evaluate whether we can migrate the queue from RabbitMQ to SQS without a breaking change."*

**Don't use it for:** Work where the implementation approach is already known — that's just a Task.

---

### Improvement (if enabled on your board)

**What it is:** A change to something that already works, making it better rather than fixing a defect or adding new capability — performance, usability, code quality.

**When to use it:**
- The current behavior is "working as intended" but not as good as it could be.
- Distinguishes "this is broken" (Bug) from "this works but should be better" (Improvement).

**Example:** *"Reduce dashboard load time from 4s to under 1s."*

---

## 3. Choosing a Type: Quick Decision Guide

Ask these in order:

1. **Is this a big initiative spanning multiple sprints, made up of smaller pieces?** → **Epic**
2. **Is something broken that used to work / was specified to work?** → **Bug**
3. **Do you need to research/investigate before you can even scope the real work?** → **Spike**
4. **Does it deliver user-facing value, describable as "as a user, I want..."?** → **Story**
5. **Is it well-understood internal/technical work with no user framing?** → **Task**
6. **Is it a technical step that only makes sense as part of a bigger Story/Task/Bug?** → **Sub-task**
7. **Is it making already-working functionality better rather than new or broken?** → **Improvement**

---

## 4. Writing Good Tickets

Regardless of type:

- **Summary**: short, specific, action-oriented. "Fix login redirect loop" not "Login issue."
- **Description**: enough context that someone outside the conversation that spawned the ticket can pick it up cold.
- **Acceptance criteria**: for Stories/Tasks, state what "done" looks like — this is what a reviewer/QA checks against.
- **Link related work**: parent Epic, related Bugs, blocking/blocked-by relationships.
- **Estimate before committing to a sprint**: Stories/Tasks/Bugs should carry an estimate (points or time) before being pulled into a sprint — Sub-tasks and Epics typically aren't estimated the same way.
- **Set the right priority/severity**: don't mark everything Highest — reserve it for things that are genuinely blocking.

---

## 5. Do's and Don'ts

**Do**
- Keep Stories/Tasks/Bugs small enough to finish within a sprint — split large ones into Sub-tasks or separate tickets.
- Link every Story/Task/Bug to its Epic when one exists.
- Use Bug strictly for regressions/defects, not for "things we forgot to build."
- Write acceptance criteria before work starts, not after.

**Don't**
- Don't create an Epic for something that fits in one sprint — that's unnecessary overhead.
- Don't use Sub-tasks for work that should be independently prioritized — it hides that work from backlog grooming.
- Don't leave tickets untyped or mistyped just to get something onto the board quickly — it breaks velocity and reporting later.
- Don't retroactively relabel a missed feature as a Bug to make it sound more urgent.
