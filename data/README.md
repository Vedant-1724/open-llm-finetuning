# Dataset directory

This directory will contain the curated instruction dataset after the schema and sourcing pipeline are validated.

Planned files (created as the dataset matures):

- `train.jsonl` — training split
- `validation.jsonl` — validation split
- `test.jsonl` — held-out final evaluation split

Do not commit generated data until it has been checked for correctness, duplicates, source/license constraints, and train/test leakage. Do not place credentials or private information here.

The expected conversational schema and quality rules are documented in [the dataset specification](../docs/dataset_specification.md).
