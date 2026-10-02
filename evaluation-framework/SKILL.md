---
name: evaluation-framework
description: Framework for designing and running evaluations that decide whether software, an AI/LLM feature, a prompt, a model, or an agent is good enough to ship. Use when asked to evaluate, benchmark, compare models or prompts, build an eval set or test harness, pick metrics, use an LLM-as-judge, set quality gates in CI, or detect regressions.
---

# Evaluation Framework

An evaluation answers one question with evidence: **"Is this good enough for its purpose, and is it better or worse than before?"** Deterministic software is mostly covered by tests (see `sdlc-best-practices`); this skill matters most where output is variable or subjective: LLM features, agents, search and ranking, recommendations, and performance.

## Workflow

1. **Define success before building the eval.** Write down the user task, what "good" looks like, and the ship threshold. Use `references/eval-plan-template.md`.
2. **Build the dataset.**
   - Start with 20 to 50 realistic cases; grow to hundreds as failures surface.
   - Cover: typical cases, edge cases, adversarial cases (prompt injection, malformed input), and known past failures.
   - Record expected output or grading criteria for each case. Store as versioned JSONL or CSV in the repo (`evals/datasets/`).
   - Keep a held-out set you don't tune against.
3. **Choose graders**, cheapest reliable one first:
   1. **Code-based:** exact match, regex, JSON schema validity, unit tests on generated code, numeric tolerance. Fast and deterministic; use whenever possible.
   2. **LLM-as-judge:** a model scores output against a rubric. Use for open-ended quality. Follow the rules below.
   3. **Human review:** for calibrating judges, high-stakes decisions, and a periodic sample of production traffic.
4. **Pick metrics** that map to the success definition (see `references/metrics.md`). Always include at least one quality metric, one safety or failure metric, and cost and latency.
5. **Run a baseline** on the current system before changing anything.
6. **Change one thing at a time** (prompt, model, retrieval, code) and rerun. Compare against the baseline on the same dataset.
7. **Analyze failures, not just scores.** Read the failing outputs, group them into categories, and fix the largest category first. Add new failure types to the dataset.
8. **Gate and monitor.** Run a fast eval subset in CI on every change that touches the system under test; block merges that drop below threshold. Sample production traffic into the eval set regularly.

## LLM-as-judge rules
- Give the judge a specific rubric with a small scale (pass/fail or 1 to 5) and a written definition for each level.
- Ask for the reasoning before the score.
- Grade one criterion per judge call rather than "overall quality."
- For comparisons, use pairwise judging and swap the order of A and B to cancel position bias.
- Calibrate: have a human grade 30 to 50 cases and check agreement with the judge (target 80% or higher, or Cohen's kappa of 0.6 or higher) before trusting it.
- Use a different or stronger model than the one being judged where possible, and pin the judge model version.

## Rigor
- Run nondeterministic systems several times per case (3 to 5) and report the mean and spread, or pass@k and pass^k for agents.
- Report confidence intervals; with small datasets a few points of difference is often noise. Use a paired test (e.g. bootstrap or McNemar) when comparing two variants on the same cases.
- Pin everything that affects results: model version, temperature, prompts, dataset version, code commit. Log it with each run.
- Watch for leakage: eval cases must not appear in prompts, few-shot examples, or fine-tuning data.

## Evaluating agents
- Grade the **outcome** (was the task completed, checked against the final state) separately from the **trajectory** (tool calls, steps, cost).
- Track: task success rate, steps and tokens per task, tool-error rate, and unsafe or out-of-scope actions (should be zero).
- Run in a sandbox with reset state per trial.

## Suggested layout in a project

```
evals/
├── README.md             # how to run, current baseline scores
├── datasets/             # versioned JSONL: {"id", "input", "expected" | "criteria", "tags"}
├── graders/              # code graders and judge rubrics
├── run_eval.py           # runs system over dataset, applies graders, writes results
└── results/              # timestamped run outputs (or send to a tracking tool)
```

Tools that can host this: promptfoo, OpenAI Evals, Inspect (UK AISI), DeepEval, Ragas (RAG), LangSmith, Braintrust. A small script plus JSONL is often enough to start.

## Reporting
Present results as a table of variants against metrics, with the baseline row first, then the top failure categories with one example each, then a ship or don't-ship recommendation tied to the thresholds.

## References
- `references/eval-plan-template.md`: plan to fill in before building an eval.
- `references/metrics.md`: metric catalog by system type.

## Changelog
- 2026-09-28: Initial version.
