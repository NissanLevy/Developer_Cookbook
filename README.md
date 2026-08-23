# Developer Cookbook

A living reference of guidelines, best practices, and common recipes for developers on the team — branching, releases, Jira, and Agile process. Start here, then follow the links below into whichever area you need.

## Git & GitHub

- [Git Flow](GitFlow.md) — **default/recommended branching strategy.** Versioned releases with a deployment-committee approval gate: `main`/`develop`, feature/release/hotfix branches, environments, commit/PR conventions, and troubleshooting.
- [GitHub Flow](GithubFlow.md) — **exception, not the default.** Trunk-based strategy for projects with no distinct "released version" (e.g. a purely internal, always-current dashboard).
- [Branch Protection Rules](BranchProtectionRules.md) — implementation spec for DevOps: exact GitHub branch protection / merge / tag settings that enforce Git Flow, plus `gh` CLI snippets to script it across repos.
- [Deployments Log](DEPLOYMENTS.md) — manual record of what version is currently live in each environment (develop/test/stage/prod).
- [Jira Release Management](JiraReleaseManagement.md) — how to use Jira Fix Versions so they stay aligned with Git Flow's release branches and tags, including naming convention, partial releases, and hotfix versions.

**Which branching strategy do I use?** Start with **Git Flow** unless your project deploys straight to a single, always-current runtime you control, ships continuously with no committee/batch approval step, and never needs to patch an old released version independently — if all three hold, use GitHub Flow instead. Most projects here should default to Git Flow.

## Jira & Tickets

- [Jira Ticket Types](Jira.md) — what each issue type (Epic, Story, Task, Bug, Sub-task, Spike, Improvement) means and when to use it, plus a quick decision guide.
- [How to Open a Ticket](HowToOpenATicket.md) — the field-by-field process for creating a ticket, with a checklist of what belongs in the description per issue type.
- [Prioritization & Sprint Change Guide](Prioritization.md) — what P0–P4 mean, plus a weighted decision matrix for deciding whether something urgent justifies changing a sprint already in progress.

## Agile Process

- [Agile Meetings](AgileMeetings/Overview.md) — which Scrum ceremony happens when during a sprint, with a detail page per meeting: [Sprint Planning](AgileMeetings/SprintPlanning.md), [Daily Standup](AgileMeetings/DailyStandup.md), [Backlog Refinement](AgileMeetings/BacklogRefinement.md), [Sprint Review](AgileMeetings/SprintReview.md), [Sprint Retrospective](AgileMeetings/SprintRetrospective.md).
- [Daily Update Email Template](AgileMeetings/DailyUpdateEmailTemplate.md) ([Hebrew](AgileMeetings/DailyUpdateEmailTemplate.he.md)) — fill-in-the-blanks status email for keeping managers/stakeholders in the loop.
- [Quick Estimation Guide](Estimation.md) — story points vs. t-shirt sizes, fast techniques (Planning Poker, Bucket System, Affinity Mapping), and a step-by-step process for sizing work quickly.
- [Definition of Done](DefinitionOfDone.md) — the non-negotiable completion checklist for a User Story, an Epic, and a Version/Release, and how each level builds on the one below it.

## Adding a New Guide

- **A standalone topic** (e.g. code style, testing) → new root-level `TopicName.md`, linked from the relevant section above.
- **A family of related docs** (e.g. the five Agile ceremonies) → its own subfolder with an `Overview.md` indexing the rest, linked from README as one entry.
- Always add a link back here — an undiscoverable doc doesn't help anyone.
- Also add it to `_sidebar.md` (site navigation) and to the manifest list in `.github/workflows/build-pdf.yml` (PDF export) — neither is generated automatically from this file.

More topics will be added over time (e.g. code review, testing, CI/CD, code style).

## Other Formats

- **Live site:** published via GitHub Pages — see repo Settings → Pages for the URL.
- **PDF:** auto-rebuilt on every push to `master`, published to the [`latest-pdf` release](../../releases/tag/latest-pdf).
