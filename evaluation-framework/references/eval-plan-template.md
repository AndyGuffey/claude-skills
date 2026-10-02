# Eval Plan: <feature or system>

## Purpose
- **User task:** what the user is trying to accomplish.
- **Decision this eval informs:** e.g. ship v2 prompt, switch models, launch to 100%.

## Success criteria
| Criterion | Metric | Threshold to ship | Current baseline |
|-----------|--------|-------------------|------------------|
| Correctness | e.g. accuracy vs. reference | ≥ 90% | |
| Safety | e.g. harmful or policy-violating outputs | 0 in adversarial set | |
| Format | e.g. valid JSON rate | ≥ 99.5% | |
| Latency | p95 end-to-end | ≤ 3 s | |
| Cost | per request | ≤ $0.01 | |

## Dataset
- **Sources:** hand-written, production samples (anonymized), synthetic, past bug reports.
- **Size and split:** dev set N = __, held-out set N = __.
- **Coverage tags:** typical / edge / adversarial / regression.
- **Location and version:** `evals/datasets/<name>-vN.jsonl`

## Graders
| Criterion | Grader type | Details |
|-----------|-------------|---------|
| | code / LLM judge / human | rubric file, judge model, calibration result |

## Run configuration
- System version / commit:
- Model and parameters:
- Trials per case:

## Results
| Variant | Metric 1 | Metric 2 | ... | Notes |
|---------|----------|----------|-----|-------|
| Baseline | | | | |

## Failure analysis
| Category | Count | Example case ID | Proposed fix |
|----------|-------|-----------------|--------------|

## Recommendation
Ship / don't ship / iterate, and why.
