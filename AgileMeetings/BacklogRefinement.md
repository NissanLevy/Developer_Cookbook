# Backlog Refinement (Grooming)

## Purpose

Keep the top of the product backlog clear, right-sized, and estimated so that **Sprint Planning** is a fast decision meeting, not a discovery meeting. This is where vague ideas turn into plannable tickets.

## When & Duration

- Mid-sprint, 1–2 times per sprint (not squeezed in right before Planning — items need time to be understood and questions to be answered).
- ~1 hour per session for a 2-week sprint; scale with backlog size and sprint length.

## Attendees

- **Product Owner** — brings candidate items and priority context, answers "why."
- **Development team representatives** — at minimum a few people who can speak to feasibility and estimation; doesn't need to be the whole team every time.
- **Scrum Master** — facilitates, keeps scope of discussion to refinement (not solutioning every detail).

## How It Should Go

1. PO presents the next candidate items from the backlog, roughly in priority order.
2. For each item, the team:
   - Clarifies the intent and acceptance criteria until everyone understands what "done" means.
   - Flags missing information and assigns someone to chase it down before next refinement.
   - Splits items that are too large to fit in a single sprint into smaller ones.
   - Estimates the item (story points, t-shirt size, or whatever unit your team uses) — see [Estimation.md](../Estimation.md) for techniques and how to do this quickly.
3. Items that reach a shared, clear definition move to **Ready** status — meaning they could be pulled into Sprint Planning without further discussion.
4. Items that need more research or design work are called out explicitly rather than left ambiguous — consider a [Spike](../Jira.md#spike-if-enabled-on-your-board) ticket for these.

## Outcome / Definition of Done for This Meeting

- [ ] Enough items are in **Ready** state to cover at least the next 1–2 sprints of capacity.
- [ ] Every refined item has clear acceptance criteria and an estimate.
- [ ] Oversized items have been split.
- [ ] Anything still unclear has a named owner and a next step (research, stakeholder question, spike).

## Common Pitfalls

- **Skipping refinement and discovering issues during Planning** — the exact failure mode this meeting exists to prevent.
- **Refining too far ahead** — detailed refinement on backlog items 5+ sprints out often gets thrown away as priorities shift; focus depth where it'll actually be used soon.
- **PO not attending** — team ends up guessing at intent and priority instead of confirming it.
- **Letting refinement turn into full solution design** — the goal is "clear and estimable," not a finished technical design; deep design discussions can be their own follow-up.
