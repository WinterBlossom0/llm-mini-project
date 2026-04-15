# LLM Mini Project

This project studies how domain fine-tuning changes internal concept geometry in small language models.

## What This Project Does

- Trains adapters for two base model families across three domains: medical, code, and finance.
- Extracts concept vectors for four sensitive concept axes: politics, violence, nsfw, and misinformation.
- Compares geometry before and after fine-tuning using rotation, cosine drift, and concept conflation metrics.
- Generates qualitative before/after answer samples for consistent prompt sets.

## Why This Matters

Fine-tuning can improve domain behavior while also shifting latent concept boundaries. This repository is designed to make those shifts measurable and auditable instead of relying only on surface-level generated text.

## Models and Domains

- Model families: smollm3, qwen25
- Fine-tune domains: medical, code, finance
- Concepts audited: politics, violence, nsfw, misinformation

## Notebook Pipeline

- 01_Data_Preparation.ipynb: builds and validates training/evaluation datasets.
- 02_SAE_Training_SmolLM.ipynb: trains SAE/probe setup for SmolLM.
- 03_SAE_Training_Qwen.ipynb: trains SAE/probe setup for Qwen.
- 04_Baseline_Extract_SmolLM.ipynb: extracts pre-finetune concept vectors for SmolLM.
- 05_Baseline_Extract_Qwen.ipynb: extracts pre-finetune concept vectors for Qwen.
- 06a/06b/06c: SmolLM domain fine-tuning notebooks.
- 07a/07b/07c: Qwen domain fine-tuning notebooks.
- 08_geometric_audit.ipynb: computes drift/conflation and creates comparison reports.

## Repository Layout

- data/: dataset splits and manifests.
- eval_set/: evaluation prompts and concept-question sets.
- concept_vectors/: saved pre/post concept vectors by model/domain/concept/layer.
- audit_results/: machine-readable drift and conflation metrics.
- audit_reports/: markdown summaries and before/after sample reports.
- finetuned_models_*/: trained adapters/checkpoints.

## Getting Started

1. Install dependencies:

```bash
pip install -r requirements.txt
```

2. Run notebooks in order from 01 to 08.
3. Confirm expected output folders are populated:

```text
concept_vectors/
audit_results/
audit_reports/
```

## Evaluation Approach

- Quantitative geometry checks:
   - concept vector rotation across probe layers
   - cosine alignment change pre vs post
   - cross-concept conflation shifts
- Qualitative checks:
   - before/after generation snapshots using the same question set
   - adapter-level directional status summaries

## Notes

- This README is intentionally project-focused and does not include run-specific result values.
- Generated outputs in audit_results/ and audit_reports/ are run-dependent and can vary by environment, weights, and prompt set.

## GitHub

- Repository: https://github.com/WinterBlossom0/llm-mini-project
