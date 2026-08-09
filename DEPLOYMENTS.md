# Deployments Log

Manual record of what version is currently deployed to each environment. Update this by hand as part of every deploy — a branch/tag name only tells you *what version something is*, not *where it's currently running*, so this file (plus the `env/*` git tags described in [GitFlow.md § Environments & Deployment Tracking](GitFlow.md#6-environments--deployment-tracking)) is the source of truth for "what's live right now."

## Current state

| Environment | Version | Deployed by | Date       | Notes |
|-------------|---------|-------------|------------|-------|
| develop     |         |             |            |       |
| test        |         |             |            |       |
| stage       |         |             |            |       |
| prod        |         |             |            |       |

## History

Append a row here each time an environment's version changes — don't overwrite the past, so there's an audit trail of when each environment changed and who approved/deployed it.

| Date | Environment | Version | Deployed by | Approved by (if applicable) | Notes |
|------|-------------|---------|-------------|------------------------------|-------|
|      |             |         |             |                              |       |
