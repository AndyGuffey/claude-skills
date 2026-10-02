# Skills Library

Reusable skills for AI coding tools and agents (Claude Code, the Claude Agent SDK, and any tool that reads the Agent Skills format). Each skill packages a practice (security, SDLC, evaluation) so every new project starts from the same standards instead of re-explaining them.

## Skills

| Area | Skill | Use it when |
|------|-------|-------------|
| Security | [`secure-development`](secure-development/SKILL.md) | Designing, writing, or reviewing code that handles input, auth, secrets, data, or dependencies |
| SDLC | [`sdlc-best-practices`](sdlc-best-practices/SKILL.md) | Planning work, branching, committing, opening PRs, setting up CI/CD, or cutting a release |
| Evaluation | [`evaluation-framework`](evaluation-framework/SKILL.md) | Deciding whether a feature, model, prompt, or agent is good enough to ship, and measuring it over time |
| Planning | [`grill-me`](grill-me/SKILL.md) | Stress-testing a plan or design by being interviewed one question at a time |
| Planning | [`write-a-prd`](write-a-prd/SKILL.md) | Turning an idea into a product requirements document someone can build from |
| Planning | [`prd-to-issues`](prd-to-issues/SKILL.md) | Breaking a PRD into small, ordered issues that each ship a working slice |
| SDLC | [`tdd`](tdd/SKILL.md) | Implementing an issue or bug fix test-first, with tests, types, and lint as the feedback loop |
| Architecture | [`improve-codebase-architecture`](improve-codebase-architecture/SKILL.md) | Turning shallow, tangled code into deep modules with clean, one-way dependencies |

## Layout

Every skill follows the Agent Skills format: one directory per skill, with a `SKILL.md` at its root.

```
skills/
├── README.md                     # this index
└── <skill-name>/
    ├── SKILL.md                  # required: frontmatter + core instructions
    ├── references/               # optional: detail loaded only when needed
    ├── templates/                # optional: files the agent copies into a project
    └── scripts/                  # optional: helper scripts the agent can run
```

`SKILL.md` starts with YAML frontmatter:

```yaml
---
name: skill-name            # lowercase, hyphens, matches the directory name
description: One or two sentences saying what the skill does and when to use it.
---
```

The `description` is what an agent reads to decide whether to load the skill, so it names the triggers (the tasks and words that should activate it). The body stays short (under ~500 lines) and points to `references/` files for depth, so the agent only pulls in detail when the task needs it.

## Using these skills in a project

Pick whichever fits the project:

1. **Copy per project (simplest).** Copy the skill directories you want into the project's `.claude/skills/`:
   ```bash
   mkdir -p .claude/skills
   cp -r /path/to/skills/secure-development .claude/skills/
   ```
   Claude Code discovers skills in `.claude/skills/` automatically. Commit them so every contributor and CI agent gets the same standards.
2. **Install once for all projects.** Copy or symlink them into `~/.claude/skills/` on your machine so they apply everywhere you work.
3. **Git submodule (keeps projects in sync).** Once this folder lives in its own repository:
   ```bash
   git submodule add <repo-url> .claude/skills-library
   ln -s ../skills-library/secure-development .claude/skills/secure-development
   ```
   Run `git submodule update --remote` to pull improvements into a project.
4. **Point to them from CLAUDE.md or AGENTS.md.** For tools that don't load skills natively, add a line such as: "Before any security-sensitive change, read `.claude/skills/secure-development/SKILL.md` and follow it."

## Adding a new skill

1. Create `skills/<new-skill>/SKILL.md` with `name` and `description` frontmatter.
2. Keep `SKILL.md` to the workflow and the rules that apply every time; move checklists, long examples, and background into `references/`.
3. Write the description around triggers: what the user or task will say when the skill is needed.
4. Add a row to the table above.
5. Test it: ask an agent to do a task that should trigger it and confirm it loads and follows it.

Ideas for next skills: `api-design`, `code-review`, `testing-strategy`, `observability`, `dependency-management`, `incident-response`, `accessibility`, `llm-app-security` (prompt injection, tool permissions), `documentation`.

## Versioning

Record notable changes to a skill at the bottom of its `SKILL.md` under a `## Changelog` heading, so projects pinned to an older copy can see what changed.
