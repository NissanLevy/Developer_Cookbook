# Quick Estimation Guide

What estimation is for, what unit to estimate in, and techniques for sizing work fast without sacrificing too much accuracy. Estimation happens mainly during [Backlog Refinement](AgileMeetings/BacklogRefinement.md) and gets confirmed during [Sprint Planning](AgileMeetings/SprintPlanning.md).

## Table of Contents

1. [Why We Estimate](#1-why-we-estimate)
2. [Choosing a Unit](#2-choosing-a-unit)
3. [Quick Estimation Techniques](#3-quick-estimation-techniques)
4. [The Estimation Process, Step by Step](#4-the-estimation-process-step-by-step)
5. [Reference Table](#5-reference-table)
6. [Do's and Don'ts](#6-dos-and-donts)

---

## 1. Why We Estimate

Estimates exist to answer planning questions — how much can we commit to this sprint, roughly when will this Epic be done, is this worth doing relative to something else — **not** to produce a precise time promise.

- An estimate is a **forecasting input**, not a contract with management.
- It measures **relative size**: effort, complexity, and uncertainty combined — not "how many hours will this take."
- Getting it roughly right, fast, beats getting it precisely right, slowly. A team that spends 20 minutes debating whether something is a 5 or an 8 has already lost more time than the estimate could ever save.

---

## 2. Choosing a Unit

| Unit | What it is | Use it for |
|---|---|---|
| **Story points** (Fibonacci-like: 1, 2, 3, 5, 8, 13...) | Relative size, unitless | Stories/Tasks/Bugs going into a sprint — the standard unit for day-to-day estimation |
| **T-shirt sizes** (XS, S, M, L, XL) | Coarse relative grouping | Epics, or a first-pass triage of a large, unrefined backlog before deeper estimation |
| **Ideal days/hours** | Absolute time, assuming zero interruptions | Rarely — mainly for well-understood technical Sub-tasks where relative sizing doesn't add value |

**Default to story points.** They force relative comparison ("is this bigger or smaller than the login-fix we called a 3?") instead of a guess at hours, which is both harder to get right and easy to mistake for a deadline.

---

## 3. Quick Estimation Techniques

### Planning Poker (default choice)

Each estimator privately picks a card from a Fibonacci-like deck (0, 1, 2, 3, 5, 8, 13, 20, 40, 100, `?`) and everyone reveals **simultaneously**. Simultaneous reveal is the entire point — it stops the first person's guess from anchoring everyone else's.

### T-Shirt Sizing

Sort items into XS–XL buckets with no numbers attached. Faster and coarser than Planning Poker — good for triaging a large batch of Epics or a backlog that hasn't been touched in a while, before doing real point estimation on what rises to the top.

### Bucket System

Lay out several reference items representing each size bucket, then rapid-fire place new items into the bucket they resemble most, with minimal discussion per item. Good for estimating many items in one sitting (e.g. clearing a backlog backlog after a big planning session).

### Affinity Mapping (Silent Grouping)

Put all tickets on a board; everyone silently moves them into size groups without talking, then the team discusses only the outliers where people disagreed. Very fast for large batches, and the silence avoids groupthink.

---

## 4. The Estimation Process, Step by Step

Using Planning Poker as the walkthrough (the same shape applies to the other techniques):

1. **Read the ticket aloud**, including acceptance criteria — don't assume everyone already read it.
2. **Timebox clarifying questions** — 2–3 minutes per item. If the team still doesn't understand it after that, the ticket isn't ready to estimate; park it and send it back for refinement (see [BacklogRefinement.md](AgileMeetings/BacklogRefinement.md)).
3. **Anchor against a reference item** — "is this bigger or smaller than the password-reset story we called a 3?" Relative comparison is faster and more consistent than sizing from scratch every time.
4. **Everyone estimates privately**, then reveals at the same time.
5. **If estimates are the same or adjacent** (e.g. 5 and 8), take the higher value and move on — don't argue over one increment of precision.
6. **If there's a wide spread** (e.g. 2 and 13), the two outliers briefly explain their reasoning — this almost always surfaces a missed requirement or a misunderstanding, not a real disagreement about difficulty. Re-vote once after the discussion.
7. **Cap it at two rounds.** If the team still hasn't converged, don't burn the meeting on it — park the item, split it, or take it offline, and move to the next ticket.

---

## 5. Reference Table

### Story points

| Points | Meaning | Guidance |
|---|---|---|
| 1 | Trivial | Well understood, fits in an hour or two |
| 2 | Small | Clear, straightforward, half a day or less |
| 3 | Small-medium | Clear scope, a few moving parts |
| 5 | Medium | Some unknowns or multiple components involved |
| 8 | Large | Significant complexity or uncertainty — worth double-checking it can't be split |
| 13 | Very large | Should usually be split before entering a sprint |
| 13+ | Too big | Don't pull into a sprint as-is — split it during refinement |

### T-shirt to points (rough mapping)

| Size | Points |
|---|---|
| XS | 1 |
| S | 2–3 |
| M | 5 |
| L | 8 |
| XL | 13+ (split before committing) |

### Keep reference stories pinned

Maintain 2–3 previously completed tickets per point value as calibration anchors (e.g. "a 3 looks like the login-redirect fix"). Without this, "what does a 5 mean" drifts over time and between people. Refresh these periodically — what counted as a 5 a year ago on unfamiliar code may be a 2 now that the team knows the codebase better.

---

## 6. Do's and Don'ts

**Do**
- Estimate relative to past reference items, not from a blank slate.
- Timebox discussion aggressively — speed is the point of "quick" estimation.
- Split anything landing at 13+ before it enters a sprint.
- Re-calibrate reference stories occasionally as the team and codebase evolve.
- Park items the team doesn't understand well enough to size, rather than forcing a guess.

**Don't**
- Don't convert points to hours ("1 point = 4 hours"). It defeats the purpose of relative sizing and turns an estimate into a deadline the moment someone does that math.
- Don't reveal estimates one at a time — it anchors everyone to the first number spoken.
- Don't let the most senior or loudest voice's estimate become the default; simultaneous reveal exists specifically to prevent this.
- Don't treat estimates as commitments to stakeholders — they're a planning input for the team, not a promise.
- Don't over-invest in precision — "roughly an 8, let's move on" beats a 15-minute debate over 8 vs. 13.
