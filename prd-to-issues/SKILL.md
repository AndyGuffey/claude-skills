---
name: prd-to-issues
description: Break a product requirements document (PRD) into independently-grabbable issues using vertical slices (tracer bullets), written as local markdown files. Use when the user asks to turn a PRD, spec, or feature plan into issues, tickets, tasks, a backlog, or an implementation plan.
---

# PRD to Issues

Break a PRD into independently-grabbable issues using vertical slices (tracer bullets), written as local markdown files.

- **Vertical slice (tracer bullet):** each issue cuts through every layer it needs (data, API, UI, tests) and leaves something working end to end, rather than "build the database" then "build the API". The first slice is the thinnest path that proves the whole system connects; later slices thicken it.
- **Independently-grabbable:** a developer or agent can pick up any unblocked issue on its own, with no conversation needed. The issue file carries all the context required to build it.

## Workflow

1. **Read the PRD** (usually `docs/prd/<feature-name>.md`, written with the `write-a-prd` skill). If requirements aren't numbered or testable, fix that with the user first.
2. **Explore the codebase** to see where each requirement lands: modules, data models, APIs, and UI touched. Don't ask the user what the code can tell you.
3. **Cut the tracer bullet first.** Define the thinnest end-to-end slice, then the slices that build on it.
4. **Minimize dependencies.** Prefer slices that can be built in parallel. Where one slice must come first, record it as `Blocked by`.
5. **Check coverage.** Every Must requirement maps to at least one issue; every issue maps to at least one requirement. List any gaps.
6. **Review with the user.** Show the summary table (title, requirements covered, blocked by, size) and revise until they agree.
7. **Write the issues as local markdown files**, one file per issue, plus an index:
   ```
   docs/prd/<feature-name>/issues/
   ├── README.md               # summary table, linking each file
   ├── 01-<short-slug>.md
   ├── 02-<short-slug>.md
   └── ...
   ```
   Number files in build order. Reference other issues by file name (`Blocked by: 01-<short-slug>.md`). Only copy them into an external tracker (e.g. `gh issue create`) if the user asks.

## Rules

- One issue fits in one PR, about a day or two of work. Split anything larger.
- Each issue stands alone: restate the context it needs from the PRD instead of saying "see PRD", and name the relevant files or modules found in the codebase.
- Each issue has testable acceptance criteria and references the PRD requirement IDs (R1, R2, ...).
- Write titles as outcomes, verb first: "Let users export reports as CSV", not "CSV stuff".
- Include setup work (migrations, feature flags, config) inside the slice that needs it, unless several slices share it.
- Put security, privacy, and accessibility requirements in the issues they apply to, not in a separate "hardening" issue at the end.
- Leave out implementation detail the developer should decide; include constraints they must follow.

## Issue template

```markdown
# <NN>: <Verb-first outcome title>

**PRD:** docs/prd/<feature-name>.md
**Requirements:** R1, R3
**Size:** S | M | L
**Status:** Todo | In progress | Done
**Blocked by:** <NN-slug>.md or none
**Blocks:** <NN-slug>.md or none

## What
What the user or system can do once this is done, in one or two sentences.

## Why
The problem from the PRD this addresses, restated so this file stands alone.

## Context
Relevant files, modules, data models, and constraints found in the codebase.

## Acceptance criteria
- [ ] Given <context>, when <action>, then <result>
- [ ] Tests cover the criteria above
- [ ] Docs updated if behavior changed

## Open questions
Anything the builder must confirm before or while building.
```

## Index template (`issues/README.md`)

```markdown
# Issues: <Feature name>

PRD: ../../<feature-name>.md

| # | Issue | Requirements | Blocked by | Size | Status |
|---|-------|--------------|------------|------|--------|
| 01 | [Thinnest end-to-end path](01-<short-slug>.md) | R1 | none | M | Todo |
```

## Changelog
- 2026-10-02: Issues are independently-grabbable tracer-bullet slices, written as one local markdown file each with an index.
- 2026-10-02: Initial version.
