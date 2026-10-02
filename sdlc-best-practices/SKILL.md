---
name: sdlc-best-practices
description: Software development lifecycle practices from planning through release. Use when starting a project or feature, writing requirements, choosing a branching strategy, writing commits, opening or reviewing pull requests, setting up tests or CI/CD, versioning, releasing, or deploying, and when asked how a project "should" be set up.
---

# SDLC Best Practices

A default lifecycle for projects of any size. Scale it down for solo prototypes (skip formal reviews, keep tests and CI) and up for teams (add required reviewers, environments, change approval).

## Lifecycle at a glance

| Phase | Output | Done when |
|-------|--------|-----------|
| 1. Plan | Issue with problem, acceptance criteria, scope | Someone else could build it from the issue |
| 2. Design | Short design note or ADR for non-trivial changes | Tradeoffs and risks written down; security considered |
| 3. Build | Small commits on a short-lived branch | Code, tests, and docs change together |
| 4. Verify | Green CI, reviewed PR | All checks pass; reviewer approved |
| 5. Release | Versioned, tagged artifact with changelog | Deployed and monitored; rollback path known |
| 6. Operate | Dashboards, alerts, incident notes | Issues feed back into Plan |

## 1. Plan
- Write each unit of work as an issue: **problem**, **acceptance criteria** (testable statements), **out of scope**.
- Slice work so each piece can merge in a day or two.
- Label by type (feature, bug, chore) and priority.

## 2. Design
- For changes that affect architecture, data models, public APIs, or security, write an Architecture Decision Record in `docs/adr/NNNN-title.md` (template: `references/adr-template.md`).
- Run the `secure-development` skill's threat model for anything handling sensitive data.
- Decide how the change will be tested and rolled back before building it.

## 3. Build
- **Branching:** trunk-based development. `main` is always releasable; work on short-lived branches named `type/short-description` (e.g. `feat/user-export`, `fix/login-timeout`). Merge within days, not weeks.
- **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`; `!` or `BREAKING CHANGE:` for breaking changes). One logical change per commit; the message says *why*.
- **Feature flags** for incomplete work that must merge before it's ready.
- **Code style:** enforce with a formatter and linter in a pre-commit hook and CI (e.g. Prettier/ESLint, Ruff/Black, gofmt), not by review comments.
- Update docs, the changelog entry, and tests in the same PR as the code.

## 4. Verify

### Testing
Follow the test pyramid: many fast unit tests, fewer integration tests, a handful of end-to-end tests on critical paths.
- Build features test-first in red-green-refactor cycles, running tests, type check, and lint after every change (see the `tdd` skill).
- Every bug fix starts with a failing test that reproduces it.
- Tests are deterministic: no reliance on wall-clock time, network, or ordering without control.
- Track coverage as a signal, not a target; aim for meaningful coverage of business logic.

### Pull requests
- Keep PRs small (under ~400 changed lines where possible).
- Use the template in `references/pr-checklist.md`.
- At least one reviewer for shared code; the author resolves every comment (fix or explain).
- Reviewers check correctness, tests, security, readability, and whether the change matches the issue, in that order.

### CI pipeline (minimum)
Runs on every PR and on `main`:
1. Install with lockfile
2. Format and lint
3. Type check (where the language supports it)
4. Unit and integration tests
5. Security scans: secrets, SAST, dependencies (see `secure-development`)
6. Build the release artifact

Protect `main`: require passing CI and review, disallow force-push.

## 5. Release
- **Versioning:** Semantic Versioning (`MAJOR.MINOR.PATCH`). Conventional Commits let tools (release-please, semantic-release, changesets) bump versions and write the changelog.
- **Changelog:** `CHANGELOG.md` in Keep a Changelog format.
- **Deploy:** automated from CI, same artifact promoted through environments (dev, staging, prod). Configuration comes from the environment, not the build.
- Prefer progressive rollouts (canary, percentage flags) and always know the rollback command.
- Follow `references/release-checklist.md`.

## 6. Operate
- Structured logs, metrics for the four golden signals (latency, traffic, errors, saturation), and alerts tied to user impact.
- After incidents, write a blameless postmortem: timeline, root cause, what went well, action items with owners.
- Feed postmortem actions and user feedback back into the backlog.

## Repository essentials
Every project repo should have: `README.md` (what, setup, run, test), `CONTRIBUTING.md`, `CHANGELOG.md`, `LICENSE`, `.gitignore`, `.editorconfig`, lockfile, CI config, PR template, `CODEOWNERS` (teams), `SECURITY.md` (how to report vulnerabilities), and `CLAUDE.md` or `AGENTS.md` pointing agents at these skills.

## References
- `references/pr-checklist.md`: PR description template and reviewer checklist.
- `references/release-checklist.md`: pre-release, release, and post-release steps.
- `references/adr-template.md`: Architecture Decision Record template.

## Changelog
- 2026-10-02: Point to the `tdd` skill for test-first development.
- 2026-09-28: Initial version.
