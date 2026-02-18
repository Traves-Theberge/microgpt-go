# Dataset Guide

## Active Dataset

- Training file: `go/datasets/raw/databricks-dolly-15k.jsonl`
- Source snapshot: `go/datasets/raw/databricks-dolly-15k.jsonl`

## Required JSONL Schema

Each line must be a valid JSON object with:
- `id` (required, unique)
- `record_type` (required)

Supported `record_type` values:
- `knowledge` with `text`
- `memory` with `text`
- `qa` with `question` and `answer`
- `chat` with `input` and `output`
- `trajectory` with `task`, `actions[]`, and `result`
- `preference` with `prompt`, `chosen`, `rejected`

## Current Dataset Strategy

For `0.0.3`, we use one primary conversational SFT dataset (Dolly 15k) mapped to:
- `record_type: "chat"`
- `input` from instruction (+ optional context)
- `output` from response

## Validation

```bash
cd go
go run . validate-dataset
```

`./checks.sh` also validates schema and JSON correctness.

## Conversion Example (Optional)

You do not need conversion for current defaults (raw Dolly rows are auto-mapped at load time).
If you want a fully normalized dataset file:

```bash
jq -cs '
  to_entries[] |
  .value as $r |
  select(($r.instruction|type=="string") and ($r.response|type=="string")) |
  {
    id: ("dolly-" + ((.key+1)|tostring)),
    record_type: "chat",
    input: (if (($r.context // "")|length)>0
      then ($r.instruction + "\nContext: " + $r.context)
      else $r.instruction end),
    output: $r.response
  }' datasets/raw/databricks-dolly-15k.jsonl > datasets/generated/dolly-normalized.jsonl
```

## Quality Notes

- Ensure IDs stay unique after any filtering/merging.
- Keep one active training dataset to reduce drift across runs.
- Keep raw source snapshots in `datasets/raw/` for reproducibility.
