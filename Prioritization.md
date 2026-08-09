# Prioritization & Sprint Change Guide

How to assign a priority (P0–P4) to work, and how to decide — with a repeatable decision matrix rather than gut feel — whether something urgent enough to justify changing a sprint already in progress.

## Table of Contents

1. [Priority Levels](#1-priority-levels)
2. [Priority vs. Sprint Order](#2-priority-vs-sprint-order)
3. [Mid-Sprint Change Decision Matrix](#3-mid-sprint-change-decision-matrix)
4. [The Swap Rule](#4-the-swap-rule)
5. [Who Decides](#5-who-decides)
6. [Do's and Don'ts](#6-dos-and-donts)

---

## 1. Priority Levels

Priority is a separate field from [Jira ticket type](Jira.md) — a Bug, Story, or Task can each be any priority. It answers "how soon must this be addressed," not "what kind of work is this."

| Priority | Meaning | Response expectation | Example |
|---|---|---|---|
| **P0 — Critical** | Production is down, data loss/corruption, security breach, or a complete blocker with **no workaround**. | Immediate — drop everything, right now, regardless of sprint. | Payment processing is completely down. Customer data exposed. |
| **P1 — High** | Major functionality broken or significantly degraded for many users; a workaround exists but is painful, or business impact is large. | Same day / next business day. Usually justifies a sprint change — run it through the [decision matrix](#3-mid-sprint-change-decision-matrix). | Checkout fails for one payment method; users on Safari can't log in. |
| **P2 — Medium** | Real impact, but limited in scope (subset of users, non-critical flow) or a reasonable workaround exists. | Scheduled into the next sprint via normal [refinement](AgileMeetings/BacklogRefinement.md) and planning — not a sprint interruption. | A secondary report shows incorrect totals in an edge case. |
| **P3 — Low** | Minor issue: cosmetic, small edge case, low-traffic feature. | Backlog, addressed when it naturally comes up in priority order. | Icon misaligned on a rarely-used settings page. |
| **P4 — Trivial** | Nice-to-have polish with negligible impact. | Backlog; may never get scheduled, and that's fine. | Typo in a tooltip. |

### Setting priority well

- Base it on **impact and urgency**, not on who's asking or how loudly. A VP asking for a P3 doesn't make it a P0 — if it genuinely needs to jump the queue, that's what the [decision matrix](#3-mid-sprint-change-decision-matrix) is for, applied consistently.
- Re-evaluate priority if new information changes impact/urgency — priority isn't fixed at creation time.
- P0 should be rare. If everything is P0, nothing is — reserve it for things that are genuinely "stop other work now."

---

## 2. Priority vs. Sprint Order

A common confusion: **priority is not the same as "interrupt the current sprint."**

- P2–P4 items, however important long-term, go through normal backlog refinement and get pulled into a *future* sprint in priority order. They don't justify disrupting a sprint already underway.
- P0 always interrupts the sprint immediately — no matrix needed, no debate. Production is down; someone drops what they're doing.
- **P1 is the genuinely ambiguous case** — important enough that "just wait for next sprint" feels wrong, but not so severe it's an automatic all-hands interrupt. This is exactly what the decision matrix below is for: a repeatable way to decide, instead of re-litigating it emotionally every time someone asks "can we just squeeze this in?"

---

## 3. Mid-Sprint Change Decision Matrix

Score the request on four criteria, 1–5 each, then apply the weights below. This turns "how urgent does this feel" into a number the team can agree on quickly and consistently.

| Criteria | Weight | 1 (low) | 5 (high) |
|---|---|---|---|
| **Business/customer impact** | ×3 | Affects almost no one | Affects most users / major revenue or reputational risk |
| **Urgency** (cost of waiting until next sprint) | ×2 | No real cost to waiting | Damage compounds daily/hourly the longer it waits |
| **Risk of not acting now** | ×2 | Fully reversible later, no side effects | Irreversible or actively worsening (data loss, security exposure) |
| **Effort to address now** (inverse — smaller effort scores higher) | ×1 | Large effort, would consume most of remaining sprint | Small, contained fix |

**Score = (Impact × 3) + (Urgency × 2) + (Risk × 2) + (Effort × 1)**, max possible = 60.

| Total score | Recommended action |
|---|---|
| 45–60 | **Interrupt the sprint now.** Apply the [Swap Rule](#4-the-swap-rule) immediately. |
| 30–44 | **Discuss with PO + Scrum Master** before deciding — genuinely borderline, worth a real conversation rather than either a knee-jerk yes or no. |
| 15–29 | **Fast-track to the top of the backlog** for the *next* Sprint Planning — don't interrupt the current sprint. |
| Below 15 | **Normal backlog.** Refine and prioritize through the usual process. |

### Worked example

*"A customer reports that CSV export produces a corrupted file for accounts with non-English characters in the name."*

| Criteria | Score | Weighted |
|---|---|---|
| Impact | 3 (affects a subset of accounts, not most users) | 9 |
| Urgency | 3 (annoying, but a manual workaround exists) | 6 |
| Risk of waiting | 2 (no data loss, fully fixable later) | 4 |
| Effort now | 4 (small, contained fix) | 4 |
| **Total** | | **23 → Fast-track to next planning, don't interrupt this sprint** |

---

## 4. The Swap Rule

If a scored item earns a sprint interruption (45–60, or a PO/SM decision to proceed on a borderline 30–44 score), **don't just add it on top of the existing sprint backlog.** Capacity didn't increase just because urgency did.

1. Identify a not-yet-started item of roughly equal size already in the sprint.
2. Remove it from the sprint (back to the backlog, re-prioritized for later — not deleted).
3. Add the urgent item in its place.
4. Re-confirm the Sprint Goal still makes sense with the swap — if the removed item was load-bearing for the goal, say so out loud rather than quietly letting the goal slip.

This keeps the sprint commitment honest: the team isn't silently expected to do more just because something urgent came up.

---

## 5. Who Decides

- **P0** — anyone can and should act immediately; this is not a group decision, it's a "stop, drop, fix" situation. Inform the PO/SM as soon as the immediate fire is out.
- **Score 45–60** — Scrum Master facilitates, but the Product Owner has final say on the trade-off (it's their backlog and their Sprint Goal being affected).
- **Score 30–44** — a real conversation between PO and Scrum Master, ideally with input from whoever would do the work, before committing either way.
- **Score below 30** — Product Owner alone can decide backlog placement; no need to convene anyone.

---

## 6. Do's and Don'ts

**Do**
- Score requests consistently using the same matrix every time — this is what prevents priority from becoming "whoever asks most persistently wins."
- Apply the Swap Rule whenever something is added mid-sprint — capacity is fixed, not elastic.
- Reserve P0 for things that truly cannot wait, so it retains meaning.
- Revisit and re-score if new information comes in mid-discussion.

**Don't**
- Don't let priority be set by seniority or volume of the person asking rather than actual impact/urgency.
- Don't add urgent work to a sprint without removing something of equivalent size.
- Don't skip the matrix for "obviously urgent" P1 requests — the matrix exists precisely because "obviously urgent" is where disagreement happens most.
- Don't treat every P1 as a mandatory interruption — plenty of P1s land at 30-something and are better handled via fast-tracked next-sprint planning.
