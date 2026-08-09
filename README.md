# Developer Cookbook

A living reference of guidelines, best practices, and common recipes for developers on the team.

## Contents

- [Git Flow](GitFlow.md) — **default/recommended branching strategy.** Versioned releases with a deployment-committee approval gate: `main`/`develop`, feature/release/hotfix branches, and the same commit/PR/troubleshooting conventions. Use this for anything versioned and distributed — Windows services, console apps, DLLs, and most APIs/services in our portfolio.
- [GitHub Flow](GithubFlow.md) — **exception, not the default.** Trunk-based strategy for projects that are continuously deployed with no distinct "released version" (e.g. a purely internal, always-current web dashboard). Only use this if a project genuinely has no batch/versioned release process — when in doubt, use Git Flow.
- [Deployments Log](DEPLOYMENTS.md) — manual record of what version is currently live in each environment (develop/test/stage/prod). Update it as part of every deploy; see [GitFlow.md § Environments & Deployment Tracking](GitFlow.md#6-environments--deployment-tracking) for how it fits the release process.
- [Branch Protection Rules](BranchProtectionRules.md) — implementation spec for DevOps: exact GitHub branch protection / merge / tag settings that enforce Git Flow, plus `gh` CLI snippets to script it across repos.
- [Jira Ticket Types](Jira.md) — what each Jira issue type (Epic, Story, Task, Bug, Sub-task, Spike, Improvement) means and when to use it, plus a quick decision guide.
- [How to Open a Ticket](HowToOpenATicket.md) — the field-by-field process for actually creating a ticket, with a checklist of what belongs in the description for each issue type.
- [Agile Meetings](AgileMeetings/Overview.md) — which Scrum ceremony happens when during a sprint. Each meeting also has its own detail page: [Sprint Planning](AgileMeetings/SprintPlanning.md), [Daily Standup](AgileMeetings/DailyStandup.md), [Backlog Refinement](AgileMeetings/BacklogRefinement.md), [Sprint Review](AgileMeetings/SprintReview.md), [Sprint Retrospective](AgileMeetings/SprintRetrospective.md). Plus a [Daily Update Email Template](AgileMeetings/DailyUpdateEmailTemplate.md) ([Hebrew](AgileMeetings/DailyUpdateEmailTemplate.he.md)) for keeping managers/stakeholders in the loop.
- [Quick Estimation Guide](Estimation.md) — what estimation is for, story points vs. t-shirt sizes, fast techniques (Planning Poker, Bucket System, Affinity Mapping), and a step-by-step process for sizing work quickly.
- [Prioritization & Sprint Change Guide](Prioritization.md) — what P0–P4 mean and when to use each, plus a weighted decision matrix for deciding whether something urgent enough justifies changing a sprint already in progress.
- [Definition of Done](DefinitionOfDone.md) — the non-negotiable completion checklist for a User Story, an Epic, and a Version/Release, and how each level builds on the one below it.

## Which branching strategy do I use?

Start with **Git Flow** unless your project meets *all* of these:
- Deploys straight to a single, always-current runtime you control (no installed/distributed artifact)
- Ships continuously, with no committee/batch approval step
- Never needs to patch an old released version independently of ongoing development

If any of those don't hold — which is most projects here — use Git Flow.

More topics will be added over time (e.g. code review, testing, CI/CD, code style).
