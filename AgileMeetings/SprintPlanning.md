# Sprint Planning

## Purpose

Decide **what** will be delivered in the upcoming sprint and **how** the team plans to deliver it. Ends with the team committing to a Sprint Goal and a Sprint Backlog.

## When & Duration

- First day of the sprint, before any other sprint work begins.
- ~4 hours for a 2-week sprint (roughly 2 hours per week of sprint — scale with sprint length).

## Attendees

- **Product Owner** — brings the prioritized, refined backlog.
- **Scrum Master** — facilitates, keeps time, keeps the conversation structured.
- **Development team** — assesses what's achievable and how to build it.

## Prerequisites (walk in with this already true)

- Backlog items being considered are already **refined**: clear acceptance criteria, estimated (see [Estimation.md](../Estimation.md)), right-sized (see [BacklogRefinement.md](BacklogRefinement.md)). Planning is not the time to discover a ticket is vague.
- Team capacity for the sprint is known (accounting for PTO, holidays, on-call, other commitments).
- Previous sprint's velocity/throughput is available as a reference point.

## How It Should Go

### Part 1 — What (Product Owner–led)

1. PO presents the sprint objective / theme and the top of the prioritized backlog.
2. Team asks clarifying questions on each candidate item.
3. Team and PO agree on a **Sprint Goal** — a single sentence describing the purpose of the sprint, not just a list of tickets. ("Ship password-reset end to end" not "Do tickets 101, 104, 110.")
4. Items are pulled into the sprint based on the goal and team capacity — not just "whatever's next on the backlog."

### Part 2 — How (Team-led)

5. Team breaks selected Stories/Tasks into technical Sub-tasks (see [Jira.md](../Jira.md) for ticket-type guidance).
6. Team confirms the selected scope is actually achievable within capacity — trim scope now if it isn't, don't discover this on day 8.
7. Any cross-team dependencies or risks are called out explicitly and assigned an owner to track.

## Outcome / Definition of Done for This Meeting

- [ ] A single, clear **Sprint Goal** is written down and visible to the team.
- [ ] The **Sprint Backlog** is finalized — every item pulled in is a firm commitment, not a stretch guess.
- [ ] Every Story/Task in scope has been broken into Sub-tasks where needed.
- [ ] Known risks/dependencies are logged with an owner.
- [ ] Every team member could, if asked right after the meeting, state the Sprint Goal in their own words.

## Common Pitfalls

- **No Sprint Goal** — the sprint becomes an arbitrary bag of tickets with no shared purpose, making trade-off decisions mid-sprint harder.
- **Overcommitting** — pulling in more than capacity supports "to be safe," which just guarantees a incomplete sprint.
- **Planning unrefined work** — if the team is discovering acceptance criteria *during* planning, refinement failed upstream; don't let planning become ad-hoc refinement.
- **PO absent or disengaged** — the team ends up guessing at priority and intent instead of confirming it.
