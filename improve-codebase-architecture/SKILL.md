---
name: improve-codebase-architecture
description: Find and fix architectural problems in a codebase by creating deep modules (simple interfaces hiding a lot of functionality) and clean dependencies. Use when the user asks to improve, refactor, restructure, or untangle a codebase, reduce coupling or complexity, make code easier to test or for agents to work in, or review architecture.
---

# Improve Codebase Architecture

Make the codebase easier to understand, change, and test by turning shallow, tangled code into **deep modules** with **clean dependencies**.

## Core ideas

**Deep module:** a small, simple interface that hides a large amount of functionality (John Ousterhout, *A Philosophy of Software Design*). Callers need to know little to use it; complexity lives inside, once.

**Shallow module:** the interface is nearly as complex as what it does. Signs:
- Pass-through functions or classes that only forward calls to another layer.
- Many tiny functions or files a caller must combine in the right order to get anything done.
- Callers must know internal details (call order, flags, data formats, which config to set) to use it correctly.
- The same knowledge (a format, a rule, a protocol) repeated across several modules: information leakage.
- Classes split by technical step ("Reader", "Parser", "Validator") instead of by what they know.

**Clean dependencies:**
- Point one way: from high-level policy (business rules) toward low-level detail (database, HTTP, filesystem), with details behind interfaces the core owns.
- No cycles between modules.
- Each module depends on as few others as possible, and only through their public interfaces.
- External systems (databases, APIs, queues, clocks) sit at the edges behind adapters, so the core can be tested without them.

Deep modules make testing easier: test through the module's small interface, and the tests survive refactoring inside it.

## Workflow

1. **Map the codebase.** Explore before asking anything. List the main modules, what each one knows, and how they depend on each other. Where possible use tooling to get real data: import graphs and cycle detection (e.g. `madge` for JS/TS, `pydeps` or `import-linter` for Python, `go mod graph`, `jdeps`), plus churn (`git log --format= --name-only | sort | uniq -c | sort -rn | head`) to see which files change most.
2. **Find friction.** Look for where understanding or changing the code is hardest: shallow modules and leakage (signs above), dependency cycles, modules with very high fan-in or fan-out, core logic that imports the database or HTTP client directly, files that always change together, and code that's hard to test without heavy mocking.
3. **Propose candidates.** For each problem, write a short candidate:
   - **Problem:** what's shallow or tangled, with file paths.
   - **Proposed module:** its name, what it hides, and its interface (the few functions or types callers would use).
   - **Dependency change:** what it depends on after, and what depends on it.
   - **Benefit:** what gets simpler for callers and for tests.
   - **Cost and risk:** size of change, callers affected.
   Rank by benefit against cost; high-churn, hard-to-test areas usually come first.
4. **Decide with the user.** Present the candidates as a table, then go through the top ones one at a time, giving your recommendation for each (use the `grill-me` style). Don't refactor anything the user hasn't agreed to.
5. **Write the plan** for each agreed change as a local markdown file, `docs/architecture/<NNNN>-<short-name>.md` (template below). Record significant decisions as an ADR (see `sdlc-best-practices`).
6. **Refactor safely.**
   - First make sure tests cover the current behavior at the new module's boundary; add them if missing.
   - Introduce the new interface, move callers over in small steps, then delete the old shallow layers.
   - Run tests, type check, and lint after every step (see the `tdd` skill). Behavior must not change during a refactor; don't mix in features.
   - Keep each change to one PR where possible; for bigger moves, break the plan into issues with `prd-to-issues`.
7. **Lock it in.** Where tooling allows, add a rule that enforces the new dependency direction in CI (e.g. `import-linter` contracts, `dependency-cruiser`, ESLint `no-restricted-imports`, ArchUnit), so the architecture doesn't drift back.

## Rules

- Design interfaces around what callers need, not around how the code works inside.
- Prefer fewer, deeper modules over many shallow ones. Merging two shallow modules is often the fix.
- Hide decisions likely to change (storage, formats, third-party APIs) inside one module each.
- Pull complexity down: a module should handle its own edge cases and defaults rather than pushing them onto callers.
- Depend on interfaces at system boundaries only; don't add abstractions with a single implementation inside the core "for flexibility".
- Never change behavior and structure in the same step.

## Candidate plan template

````markdown
# <NNNN>: <Deepen / Extract / Merge> <module name>

**Status:** Proposed | Agreed | In progress | Done

## Problem
What is shallow or tangled now, with file paths and an example of the friction.

## Proposed module
- **Name and location:**
- **Hides:** the knowledge or complexity callers no longer need
- **Interface:**
  ```
  <the few functions or types callers will use>
  ```

## Dependencies
| | Before | After |
|-|--------|-------|
| Depends on | | |
| Used by | | |

## Testing
Tests at the new interface; what can now be tested without mocks.

## Migration steps
1. Add tests for current behavior at the boundary
2. Introduce the interface
3. Move callers (list them)
4. Delete old layers
5. Add a CI dependency rule

## Risks
````

## Changelog
- 2026-10-02: Initial version.
