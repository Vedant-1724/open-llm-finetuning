# Dataset Specification

## Project goal

Create a high-quality instruction dataset for evaluating parameter-efficient fine-tuning of an open-weight language model to produce clear, technically accurate AI engineering explanations.

The desired behavior is a consistent, useful answer style—not memorization of a fixed list of questions.

## Planned scope

Target: 10,000 accepted examples across six areas. This is a target, not a requirement to accept low-quality or unverified records.

| Area | Target examples |
|---|---:|
| Python and software fundamentals | 1,700 |
| Machine learning fundamentals | 1,700 |
| LLMs and Transformers | 1,700 |
| Retrieval-Augmented Generation (RAG) | 1,700 |
| Embeddings and vector databases | 1,600 |
| Evaluation, deployment, and MLOps | 1,600 |
| **Total** | **10,000** |

Initial split target: 90% training, 5% validation, 5% test. Split by source/concept family where possible to reduce near-duplicate leakage. The test set must remain held out until final evaluation.

## Record format

Use JSON Lines (one JSON object per line), following the conversational messages format used by Hugging Face SFT workflows.

Example:

```json
{
  "id": "rag-000001",
  "topic": "rag",
  "source_notes": ["Author-written; reviewed against primary documentation"],
  "messages": [
    {"role": "user", "content": "Explain Retrieval-Augmented Generation (RAG) and give one practical example."},
    {"role": "assistant", "content": "RAG retrieves relevant external information and supplies it to a language model as context. A typical pipeline retrieves relevant document chunks, places them in the prompt, and asks the model to answer using that context. For example, an internal support assistant might retrieve current policy documents before answering an employee's question. RAG can improve access to external or changing knowledge, but retrieval quality and groundedness still need to be evaluated."}
  ]
}
```

The example is illustrative and must be reviewed before acceptance into the final training set.

## Quality rules

1. Each example must have one clear learning objective.
2. The assistant answer must be technically correct, self-contained, and directly relevant.
3. Prefer original, carefully reviewed examples; avoid bulk generation without validation.
4. Include definitions, comparisons, practical scenarios, debugging, trade-offs, and application questions.
5. Remove exact duplicates and investigate near-duplicates.
6. Track sources for factual claims; prefer official documentation or primary technical references.
7. Exclude private data and material whose license does not permit the intended use.
8. Keep validation and test examples separate from training examples.
9. Report dataset counts and filtering decisions transparently.
10. Do not claim that dataset size alone demonstrates quality or model improvement.

## Suggested metadata

- `id`: stable unique identifier
- `topic`: one of the six approved area slugs
- `messages`: user and assistant turns
- `source_notes`: concise provenance/review notes; do not insert this metadata into conversational messages

Before training, validate JSONL syntax, roles, non-empty text, approved topics, unique IDs, duplicates, and token-length distribution using the chosen base model's tokenizer.

## Pilot-first workflow

1. Prepare and manually review a small pilot (approximately 100–200 records).
2. Run schema, duplication, topic-balance, and token-length checks.
3. Confirm a compatible training stack and actual available GPU in Colab.
4. Run a short smoke-test training job and verify adapter saving and reload.
5. Expand the curated dataset toward the 10,000-example target only after the pipeline works.
6. Preserve a held-out test set for final before/after evaluation.

## Evaluation principle

Run identical held-out prompts through the base and fine-tuned models with comparable generation settings. Save actual outputs and assess correctness, relevance, clarity, and adherence to the target answer style. A lower training loss alone does not prove a better model.
