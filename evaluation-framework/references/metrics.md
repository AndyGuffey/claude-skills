# Metric Catalog

Pick the few that map to your success criteria. Always pair a quality metric with cost, latency, and a failure or safety metric.

## Classification and extraction
- Accuracy (balanced classes only), precision, recall, F1 (per class and macro).
- Confusion matrix to see which classes get mixed up.
- Field-level exact match for structured extraction; schema-validity rate.

## Generation (summaries, answers, writing)
- Rubric scores from a calibrated LLM judge: correctness, completeness, relevance, tone, conciseness (one criterion per judge call).
- Pairwise win rate against the baseline.
- Reference-based (BLEU, ROUGE, BERTScore) only as rough signals; they correlate weakly with human judgment on open-ended text.

## Retrieval and RAG
- Retrieval: recall@k, precision@k, MRR, nDCG.
- Generation grounded in context: faithfulness (claims supported by retrieved text), answer relevance, citation accuracy.
- "I don't know" rate on questions with no answer in the corpus (should be high).

## Code generation
- pass@k on unit tests; compile or lint success rate.
- Security findings in generated code (run SAST on outputs).

## Agents
- Task success rate (verified by final state), pass@k and pass^k (succeeds on all k tries).
- Steps, tool calls, and tokens per successful task.
- Tool-error rate; unsafe or out-of-scope action count (target 0).

## Safety and robustness
- Harmful output rate on a red-team set.
- Prompt-injection success rate.
- Refusal rate on benign requests (over-refusal).
- Consistency: same answer across paraphrased inputs.

## Operational (every system)
- Latency p50, p95, p99; time to first token for streaming.
- Cost per request and per successful task.
- Error and timeout rate.

## Product (online, after launch)
- Task completion, user ratings (thumbs up/down), edit or regenerate rate, retention.
- Compare against offline metrics to confirm the offline eval predicts real outcomes.
- A/B test with a pre-registered primary metric and sample size.
