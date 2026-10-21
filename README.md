# Open LLM Fine-Tuning Lab

A reproducible experiment in parameter-efficient fine-tuning of an open-weight language model using QLoRA, PEFT, and TRL. The goal is to evaluate whether fine-tuning improves consistency and usefulness on AI engineering instruction tasks.

> **Project status:** Foundation and dataset specification prepared. Training has not yet run; model behavior, loss curves, and before/after results will be added only after real experiments.

## Planned experiment

- **Candidate base model:** [Qwen3-4B-Instruct-2507](https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507) (subject to GPU and package compatibility checks)
- **Training method:** QLoRA with PEFT and TRL supervised fine-tuning
- **Dataset target:** 10,000 reviewed examples across six AI engineering areas; a small pilot comes first
- **Compute:** Google Colab GPU; the actual assigned GPU and available VRAM will be recorded before training
- **Evaluation:** identical prompts for the base and adapter-loaded models, plus held-out test examples

## Technical areas

1. Python and software fundamentals
2. Machine learning fundamentals
3. LLMs and Transformers
4. Retrieval-Augmented Generation (RAG)
5. Embeddings and vector databases
6. Evaluation, deployment, and MLOps

## Repository layout

- `docs/dataset_specification.md` — dataset scope, conversational schema, quality criteria, and split strategy
- `data/` — curated JSONL splits and data handling notes
- `configs/` — versioned training configuration
- `results/` — actual metrics, comparison outputs, and plots once available

## Workflow

1. Finalize data provenance and schema; create a manually reviewed pilot.
2. Validate records, IDs, duplicates, topic balance, and token lengths.
3. Inspect the Colab GPU and test compatible model/library loading.
4. Run a short fine-tuning smoke test and verify saving/reloading the adapter.
5. Scale the dataset and training only if quality and runtime are reasonable.
6. Compare base and fine-tuned responses on fixed, held-out prompts.
7. Document configuration, loss curve, real outputs, limitations, and reproducibility steps.

## Dataset quality and leakage

The dataset will use one JSON object per line with user/assistant messages. Examples must be technically accurate, relevant, and appropriately sourced. Exact duplicates and near-duplicates will be examined. Validation and test examples must remain outside training, with splitting performed by concept/source family when feasible to reduce leakage. See [Dataset Specification](docs/dataset_specification.md).

## Reproducibility and honesty

No training results are claimed until a run has completed. Training loss alone is not proof of improved answers. The final report will include the real environment, base model identifier, training configuration, data counts, comparison prompts and outputs, and known failure cases. Do not commit credentials, private data, large caches, or raw checkpoints unnecessarily.

## License

The base model has its own license and terms. Dataset and code licensing will be documented separately after their sources and redistribution permissions are finalized.
