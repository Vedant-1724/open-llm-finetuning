# Dataset Specification

## Project goal

Create a focused instruction dataset for evaluating parameter-efficient fine-tuning of an open-weight language model to produce clear, technically accurate AI engineering explanations and coding guidance.

The desired behavior is consistent, useful assistance across six technical areas—not memorization of a fixed list of questions.

## Planned scope

Target: **up to 25,000 accepted examples** across six areas. This is a target, not a requirement to accept irrelevant, duplicated, unlicensed, or unverified records. The final accepted count may be lower if source quality or licensing does not support 25,000 suitable examples.

| Area | Target examples |
|---|---:|
| Python and software fundamentals | 4,000 |
| Machine learning fundamentals | 4,000 |
| LLMs and Transformers | 4,500 |
| Retrieval-Augmented Generation (RAG) | 4,500 |
| Embeddings and vector databases | 4,000 |
| Evaluation, deployment, and MLOps | 4,000 |
| **Total target** | **25,000** |

Planned split: 90% training, 5% validation, 5% test (22,500 / 1,250 / 1,250 if all 25,000 records pass review). Split by source/concept family where possible to reduce near-duplicate leakage. Keep the test set untouched until final evaluation.

The existing 120-record pilot is **only for testing the data and training pipeline**. It is not the final training corpus and must not be described as a 25,000-record dataset.

## Record format

Use JSON Lines (one JSON object per line), following the conversational `messages` format used by Hugging Face SFT workflows.

Example:

```json
{
  "id": "rag-000001",
  "topic": "rag",
  "source": "author_curated",
  "source_notes": "Reviewed against primary documentation",
  "messages": [
    {
      "role": "user",
      "content": "Explain Retrieval-Augmented Generation (RAG) and give one practical example."
    },
    {
      "role": "assistant",
      "content": "RAG retrieves relevant external information and supplies it to a language model as context. A typical pipeline retrieves relevant document chunks, places them in the prompt, and asks the model to answer using that context. For example, an internal support assistant might retrieve current policy documents before answering an employee's question. RAG can improve access to external or changing knowledge, but retrieval quality and groundedness still need to be evaluated."
    }
  ]
}
```

The example is illustrative and must be reviewed before acceptance into the final training set.

## Source and licensing policy

- Record the dataset name, version/revision when available, source URL, declared license, transformation, and filtering applied to every source.
- Prefer sources with clear redistribution/training terms (for example, Apache-2.0, MIT, or CC BY) and retain required attribution.
- A dataset's declared license does not automatically resolve rights in its upstream sources. Inspect upstream provenance before reusing aggregated datasets.
- Do not use datasets with missing or incompatible terms merely to reach the count target.
- Do not blindly mix code-generation, general-domain QA, and AI-engineering conceptual QA. Filter to the intended task and record the retained count.
- If a topic is under-represented, report the shortfall instead of filling it with unrelated examples.

## Quality rules

1. Each example must have one clear learning objective.
2. The assistant answer must be technically correct, self-contained, and directly relevant.
3. Prefer source-grounded and carefully reviewed examples; synthetic examples must be sampled and checked for errors.
4. Include definitions, comparisons, practical scenarios, debugging, trade-offs, and application questions.
5. Remove exact duplicates and investigate near-duplicates.
6. Track sources for factual claims; prefer official documentation or primary technical references.
7. Exclude private data and material whose license does not permit the intended use.
8. Keep validation and test examples separate from training examples.
9. Report actual counts, source composition, filtering decisions, and any limitations transparently.
10. Do not claim that dataset size alone demonstrates quality or model improvement.

## Suggested metadata

- `id`: stable unique identifier
- `topic`: one of the six approved area slugs
- `source`: source dataset or author-curated identifier
- `source_notes`: concise provenance/review notes; keep metadata out of the model's conversational messages
- `messages`: user and assistant turns

Before training, validate JSONL syntax, roles, non-empty text, approved topics, unique IDs, exact and near duplicates, source attribution, licensing notes, and token-length distribution using the chosen base model's tokenizer.

## Workflow

1. Keep the 120-record pilot separate as a smoke-test dataset.
2. Inventory candidate open datasets and inspect their actual schemas, topic fit, and licenses.
3. Normalize suitable records into the conversational schema.
4. Filter for the six topic areas, remove duplicates/low-quality and overly long records, and document source attribution.
5. Review representative records from each source and topic before accepting them.
6. Split by source/concept family to reduce leakage and validate all JSONL files.
7. Only then inspect token lengths, load the model, and run a small training smoke test.
8. Scale training only after the full dataset and runtime configuration are validated.

## Evaluation principle

Run identical held-out prompts through the base and fine-tuned models with comparable generation settings. Save actual outputs and assess correctness, relevance, clarity, and adherence to the target answer style. A lower training loss alone does not prove a better model.
