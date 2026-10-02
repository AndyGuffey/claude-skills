---
name: write-a-prd
description: Write a product requirements document (PRD) for a new feature or product. Use when the user asks for a PRD, product spec, requirements doc, or feature spec, or wants to turn an idea into something a team or agent can build from.
---

# Write a PRD

A PRD says **what** to build and **why**, clearly enough that someone else (a developer or an agent) could build it and know when it's done. It leaves **how** to the design and implementation.

## Workflow

1. **Understand the problem first.** Ask the user what problem this solves, for whom, and how they'll know it worked. Ask the questions one at a time, and give your recommended answer with each. If the plan is vague, use the `grill-me` skill before writing.
2. **Explore the codebase** when one exists, instead of asking questions it can answer: current behavior, related features, data models, constraints.
3. **Draft the PRD** using the template below. Mark anything you assumed as an assumption and anything unresolved under Open questions; never invent requirements silently.
4. **Review it with the user.** Walk through the requirements and acceptance criteria; revise until they agree.
5. **Save it** as `docs/prd/<feature-name>.md` in the project (or where the user says), then offer to break it into issues with the `prd-to-issues` skill.

## Rules

- Lead with the problem, not the solution.
- Every requirement is testable. "Fast" is not a requirement; "search returns results in under 500 ms at p95" is.
- Number requirements (R1, R2, ...) so issues, tests, and PRs can reference them.
- Prioritize: **Must** (launch blocker), **Should**, **Could**. Keep the Must list short.
- Write non-goals explicitly; they prevent scope creep.
- Include security, privacy, and accessibility requirements whenever the feature touches user data or UI (see `secure-development`).
- Keep it as short as the feature allows. A small feature can be one page.

## Template

```markdown
# PRD: <Feature name>

- **Status:** Draft | In review | Approved
- **Owner:** <name>
- **Last updated:** YYYY-MM-DD

## Problem
What problem are we solving, for whom, and what evidence shows it matters?

## Goals
- Outcome 1, with how it will be measured
- Outcome 2

## Non-goals
- What this explicitly will not do

## Users and use cases
| User | Wants to | So that |
|------|----------|---------|

## Requirements
| ID | Requirement | Priority |
|----|-------------|----------|
| R1 | The system shall ... | Must |

## User experience
Key flows, states (empty, loading, error), and links to mockups.

## Acceptance criteria
- [ ] Given <context>, when <action>, then <result> (R1)

## Success metrics
| Metric | Baseline | Target | How measured |
|--------|----------|--------|--------------|

## Security, privacy, and compliance
Data collected, who can access it, retention, threats considered.

## Dependencies and risks
| Item | Impact | Mitigation |
|------|--------|------------|

## Rollout
Feature flag, phased rollout, migration, rollback plan.

## Open questions
- [ ] Question (owner, due date)

## Assumptions
- Assumption made while writing, to confirm
```

## Changelog
- 2026-10-02: Initial version.
