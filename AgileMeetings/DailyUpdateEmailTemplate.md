# Daily Update Email Template

> Hebrew version: [DailyUpdateEmailTemplate.he.md](DailyUpdateEmailTemplate.he.md)

A short status email to send to managers/stakeholders right after [Daily Standup](DailyStandup.md), so people who don't attend standup still have daily visibility into sprint progress — without needing a meeting invite.

## How to Use It

- Fill it out **during or immediately after standup**, while the info is fresh — it should take a couple of minutes, not a rewrite of the meeting.
- Translate standup notes into **stakeholder language**: they care about progress toward the Sprint Goal and anything that affects timeline, not which function got refactored.
- **Report risk honestly and early.** A stakeholder surprised at Sprint Review by something you already knew was at risk on day 3 is the failure mode this template exists to prevent.
- Keep it to one screen. If it's longer than that, it's becoming a report instead of an update.
- If there's truly nothing new (rare, e.g. day 1), it's fine to say so briefly rather than padding it.

---

## Template

```
Subject: Daily Update – [Project/Team Name] – Sprint [#] – Day [N of 10] – [Date]

Sprint Goal: [one-sentence reminder of what this sprint is delivering]

Status: [On track / At risk / Behind]

Completed since last update:
- [Item] – [one-line plain-language description]
- [Item] – [one-line plain-language description]

In progress:
- [Item] – [brief status] – expected [date/day]

Blockers / need help with:
- [Blocker] – blocking [what] – need [specific ask] from [who] by [when]
(or: "No blockers today.")

Risks to sprint goal or timeline:
- [Risk] – potential impact – mitigation plan
(or: "No new risks to report.")

Sprint snapshot: [X of Y points complete] ([Z%])
```

---

## Example (filled out)

```
Subject: Daily Update – Checkout Team – Sprint 14 – Day 6 – 2026-07-20

Sprint Goal: Ship password reset via email end to end.

Status: On track

Completed since last update:
- Reset-token API endpoint is done and deployed to the develop environment.
- Email template for the reset link is finalized and approved by design.

In progress:
- Reset-password UI screen – ~70% done – expected complete by day 8.

Blockers / need help with:
- Need confirmation from Security on token expiry duration (15 min vs 1 hour) to finalize the endpoint — need an answer from @security-lead by tomorrow to stay on track.

Risks to sprint goal or timeline:
- No new risks to report.

Sprint snapshot: 13 of 21 points complete (62%)
```

---

## Do's and Don'ts

**Do**
- Lead with status (on track / at risk / behind) so the reader gets the headline immediately.
- Name a specific person and a specific deadline for any ask — "need X from Y by Z" gets resolved; "blocked on security" doesn't.
- Flag a risk the moment it's known, even if it might resolve itself later.

**Don't**
- Don't paste raw ticket IDs or commit messages without translating them into what they mean for the sprint goal.
- Don't bury a blocker in the middle of a long update — if something needs action, make it easy to spot.
- Don't wait until Sprint Review to mention something that's been at risk for days.
