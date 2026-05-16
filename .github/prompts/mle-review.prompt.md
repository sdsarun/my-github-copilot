---
mode: agent
description: ML engineering review — data leakage, training reproducibility, evaluation correctness, serving safety, and monitoring
---

# ML Engineering Review

Review the selected ML pipeline code. Report findings only.

## Scope

Covers the full ML production pipeline: data → training → evaluation → serving → monitoring.

## Review Priorities

### CRITICAL — Data Leakage

- Features computed using future data — check timestamps and data splits
- Preprocessing (scaling, encoding, imputation) fitted on the full dataset before splitting — fit ONLY on training data, transform validation/test
- Target-correlated features included in training — label encoding, ID columns, columns derived from the target
- Validation or test samples appearing in the training set

### CRITICAL — Evaluation Correctness

- Metric not appropriate for the task: accuracy on imbalanced classification — use F1, AUC, precision-recall
- Evaluating on training data — always report on held-out set
- Test set used during hyperparameter tuning — creates an optimistic bias; use a validation set for tuning

### HIGH — Training Reproducibility

- Missing random seeds: `torch.manual_seed`, `np.random.seed`, `random.seed`, environment variable `PYTHONHASHSEED`
- Non-deterministic data loader without `worker_init_fn` setting seeds per worker
- Model architecture or hyperparameters not logged with the run — use MLflow, W&B, or similar

### HIGH — Serving Safety

- Model loaded at import time in a web process — memory waste and slow startup; load lazily or in a worker
- Missing input validation before inference — shape, dtype, range checks
- No timeout or circuit breaker on inference calls
- Single point of failure — no fallback if model is unavailable

### HIGH — Monitoring

- No prediction distribution monitoring — silent model degradation
- No data drift detection on incoming features
- No alerting on inference latency or error rate

### MEDIUM — Code Quality

- Long notebooks with no modularized functions — extract `train.py`, `evaluate.py`, `predict.py`
- Hardcoded file paths — use config or environment variables
- Missing requirements pinning (`requirements.txt` without exact versions) — breaks reproducibility

## Output Format

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [File:Line if known]
Issue: [What is wrong]
Fix: [Concrete suggestion]
```

End with:

```
## Summary
- Critical: N
- High: N
- Medium: N
- Production-ready: yes / no / partial
```
