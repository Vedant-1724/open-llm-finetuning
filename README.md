# Open LLM Fine-Tuning Lab

A reproducible experiment in parameter-efficient fine-tuning of an open-weight language model using QLoRA, PEFT, and TRL. The goal is to evaluate whether fine-tuning improves consistency and usefulness on AI engineering instruction tasks.

> **Project status:** Repository foundation and dataset specification prepared. Training has not yet run; model behavior, loss curves, and before/after results will be added only after real experiments.

## Planned experiment

- **Candidate base model:** [Qwen3-4B-Instruct-2507](https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507), subject to GPU and package compatibility checks
- **Training method:** QLoRA with PEFT and TRL supervised fine-tuning
- **Dataset target:** Up to 25,000 accepted, source-tracked examples across six AI engineering areas
- **Pilot:** The existing 120-record pilot is for pipeline smoke testing only; it is not the final training dataset
- **Compute:** Google Colab GPU; actual GPU and VRAM are recorded before training
- **Evaluation:** Identical prompts for the base and adapter-loaded models, plus held-out test examples

## Technical areas

1. Python and software fundamentals — target 4,000
2. Machine learning fundamentals — target 4,000
3. LLMs and Transformers — target 4,500
4. Retrieval-Augmented Generation (RAG) — target 4,500
5. Embeddings and vector databases — target 4,000
6. Evaluation, deployment, and MLOps — target 4,000

These are targets, not claimed final counts. We will not fill a topic quota with irrelevant or unlicensed records.

## Repository layout

- `docs/dataset_specification.md` — scope, schema, source and licensing policy, and quality rules
- `data/` — curated JSONL splits and data handling notes
- `configs/` — versioned training configuration
- `results/` — actual metrics, comparison outputs, and plots once available

## Workflow

1. Inventory candidate sources and verify topic fit and licensing.
2. Normalize suitable records to conversational JSONL.
3. Filter, deduplicate, inspect topic balance, and document source provenance.
4. Create final train/validation/test splits without source/concept leakage.
5. Verify token lengths and test loading/training on the actual Colab GPU.
6. Compare base and fine-tuned responses on fixed held-out prompts.
7. Document configuration, loss curve, real outputs, limitations, and reproducibility steps.

## Dataset quality and leakage

The dataset uses one JSON object per line with user/assistant messages. Examples must be technically accurate, relevant, and appropriately sourced. Dataset declarations alone do not settle the licenses of upstream material in aggregated datasets. Exact duplicates and near-duplicates will be examined. Validation and test examples remain outside training, with splitting by source/concept family where feasible. See [Dataset Specification](docs/dataset_specification.md).

## Reproducibility and honesty

No training results are claimed until a run has completed. Training loss alone is not proof of improved answers. The final report will include the real environment, base model identifier, training configuration, accepted data counts and sources, comparison prompts and outputs, and known failure cases. Do not commit credentials, private data, large caches, or raw checkpoints unnecessarily.

## License

The base model has its own license and terms. Dataset and code licensing will be documented separately after source review and redistribution permissions are finalized.
