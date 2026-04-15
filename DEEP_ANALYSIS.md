# LLM Mini Project Deep Analysis

Date: 2026-04-15

## Scope

This report summarizes the current geometric audit and adapter-directional outcomes for:
- Models: smollm3, qwen25
- Datasets: medical, code, finance
- Concepts: politics, violence, nsfw, misinformation

## What Was Connected

The local workspace is connected to:
- https://github.com/WinterBlossom0/llm-mini-project.git

Current main branch contains:
- Notebook pipeline files (01 through 08)
- Evaluation questions
- Requirements
- Updated geometric audit notebook with adapter-based evaluation and directional scoring

## High-Level Directional Outcomes (All 6 Adapters)

Source: audit_reports/adapter_directional_summary.md

### qwen25
- medical: Not Improved (4/9 improved, avg delta +1.5348)
- code: Improved (6/9 improved, avg delta -0.7363)
- finance: Improved (6/9 improved, avg delta -0.0782)

### smollm3
- medical: Improved (3/9 improved, avg delta -0.0667)
- code: Improved (5/9 improved, avg delta -0.3690)
- finance: Not Improved (3/9 improved, avg delta +0.3173)

Interpretation:
- qwen25 medical is the weakest configuration currently.
- smollm3 finance remains risky.
- code finetunes are strongest for both model families by directional trend.

## Drift Severity Highlights

From drift_metrics.json summaries:

### Most severe rotations
- smollm3/medical politics: max rotation 157.54 deg
- qwen25/code misinformation: max rotation 151.46 deg

### Most stable concept regions
- qwen25/medical nsfw: avg cosine 0.9054, max rotation 27.95 deg
- qwen25/finance nsfw: avg cosine 0.9073, max rotation 26.95 deg

### Potentially unstable concept groups
- misinformation and politics are repeatedly high-rotation concepts across multiple adapters.
- magnitude deltas are near zero in current metrics because concept vectors are unit-normalized before storage; directional and angular metrics carry most signal.

## Conflation Risk Highlights

From conflation_metrics.json max absolute pair deltas:

- smollm3/code: politics|nsfw delta -0.760 (very large shift)
- smollm3/medical: politics|nsfw delta -0.724 (very large shift)
- smollm3/finance: politics|nsfw delta -0.416
- qwen25/medical: politics|misinformation delta +0.489

Interpretation:
- smollm3 has repeated strong politics-nsfw geometry coupling shifts.
- qwen25 medical has high politics-misinformation coupling increase.

## Practical Risk Note

In qualitative samples, both base and adapted models can produce unsafe completions on explicitly harmful prompts. The directional score indicates movement quality but does not by itself guarantee policy-safe outputs. Both geometry and generation behavior should be treated as first-class signals.

## Recommended Next Steps

1. Add a hard safety refusal layer for evaluation prompts known to be harmful before comparing content quality.
2. Track per-concept directional scores separately for:
   - question text embedding
   - answer embedding
   to isolate instruction-following drift from generation-style drift.
3. Add aggregate confidence intervals across sampled questions (bootstrap on delta).
4. Add a concept-wise pass/fail threshold policy, for example:
   - avg delta <= -0.05 and improved_count >= 6/9 for pass.
5. Re-run qwen25/medical and smollm3/finance finetuning with stronger safety regularization and compare to this baseline report.

## Reproducibility Notes

The geometric audit notebook now:
- resolves model layers dynamically across architectures
- evaluates all 6 adapters explicitly (2 models x 3 datasets)
- reports directional status as Improved or Not Improved
- uses max_new_tokens = 128 for controlled comparison length
- writes per-adapter summaries and a combined adapter summary table
