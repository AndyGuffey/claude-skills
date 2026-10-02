---
name: tdd
description: Test-driven development with a tight feedback loop. Use when implementing a feature, issue, or bug fix in code, when the user mentions TDD, red-green-refactor, or test-first, or when picking up an issue file written by the prd-to-issues skill.
---

# Test-Driven Development

Build in small red-green-refactor cycles, and let the project's own checks (tests, types, lint) tell you when you're done instead of judging it yourself.

## Before you start

1. **Find the feedback loop.** Work out how to run the tests, type checker, and linter in this project (README, `package.json`, `Makefile`, `pyproject.toml`, CI config). Run them once to confirm they pass before you change anything. If they already fail, tell the user before going further.
2. **Find the fastest check.** Learn how to run a single test file or test name. The inner loop should take seconds, not minutes.
3. **List the behaviors to build.** Turn each acceptance criterion (from the issue or the user) into one or more behaviors, each small enough for one test. Order them simplest first.

## The cycle

Repeat for one behavior at a time:

1. **Red.** Write one test for the next behavior. Run it and watch it fail, and check it fails for the reason you expect (not a typo or import error). A test you never saw fail proves nothing.
2. **Green.** Write the simplest code that makes that test pass. Don't add behavior no test asks for yet.
3. **Refactor.** With the tests green, clean up names, duplication, and structure in both code and tests. Rerun the tests after each change.
4. **Run the full loop** (all tests, type check, lint) before moving to the next behavior. Fix anything it reports now, while the change is small.

## Feedback loop rules

- Run the checks after every change; never write several steps of code before running anything.
- Read the full error output and fix the root cause. Never weaken an assertion, skip or delete a test, or add `any` / `# type: ignore` / lint disables to get green.
- If the same failure survives three attempts, stop and rethink: reread the error, check your assumptions about the code, or ask the user.
- Keep the build green between cycles, so you could stop at any point and commit.
- Commit after each green cycle or small group of cycles, so a bad step is easy to undo.

## What makes a good test

- Tests behavior through the public interface, not internal details, so refactoring doesn't break it.
- One reason to fail; the name says what behavior it checks.
- Fast and deterministic: no real network, clock, or random values without control.
- Mock only at system boundaries (external APIs, time, filesystem), not your own code.

## Bug fixes

Write a test that reproduces the bug and watch it fail first. Then fix it. The test stays in the suite to stop the bug coming back.

## Done means

- Every acceptance criterion has at least one test that was seen failing and now passes.
- The full test suite, type check, and lint pass.
- If working from an issue file, its acceptance criteria are ticked and its status updated.

## Changelog
- 2026-10-02: Initial version.
