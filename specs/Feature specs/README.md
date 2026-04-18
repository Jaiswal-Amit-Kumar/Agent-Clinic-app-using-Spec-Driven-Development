# Feature Specs

This folder contains **spec-driven development** documentation for individual features. Each feature gets its own Dated Feature Folder containing three spec files that guide implementation from planning through validation.

## The Three Spec Files

Every feature folder contains these three documents:

| File | Purpose |
|------|---------|
| **plan.md** | Task breakdown with numbered steps, organized into logical groups. Lists what needs to be built. |
| **requirements.md** | Defines the target audience (who benefits), the problem solved, and concrete outcome/acceptance criteria. Explains *why* and *when* it's done. |
| **validation.md** | How to verify the feature works end-to-end. Includes manual test steps and/or automated test specs. |

## Dated Feature Folder Naming

Folder names follow the format: `YYYY-MM-DD-feature-name-slug`

- **Date**: When the feature spec was created (creates chronological ordering)
- **Slug**: Lowercase, hyphenated, descriptive name
- **Example**: `2026-04-18-agent-profiles`

Dating each folder makes the roadmap chronological and shows when each feature was spec'd.

## How to Use These Specs

1. **Read requirements.md first** — understand the goal and success criteria
2. **Follow plan.md** — execute the numbered steps in order
3. **Use validation.md** — verify completeness before marking the feature done

## Referenced Feature Specs

- [2026-04-18-agent-profiles](2026-04-18-agent-profiles/) — Agent Profile Management
  - `plan.md` — Database schema, API routes, pages, and seed tasks
  - `requirements.md` — Target audience and acceptance criteria
  - `validation.md` — Test steps for agents, ailments, and API