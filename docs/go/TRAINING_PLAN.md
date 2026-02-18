# Training Plan

## Goal

Run stable conversational training with reproducible checkpoints and clear quality gates.

## Standard Run

1. `cd go`
2. `./checks.sh`
3. `go run ./cmd/mircogpt-tui`
4. Select preset:
   - `1` fast
   - `2` balanced
   - `3` max (overnight)
5. Start training (`s`)
6. Monitor:
   - training loss
   - validation loss
   - early-stop behavior
   - CPU/RAM stability
7. Validate in `Chat` against `models/latest_checkpoint.json` and `models/best_checkpoint.json`

## Overnight Plan

- Use preset `max`.
- Keep `TOKENIZER=bpe`.
- Increase `TOKEN_VOCAB_SIZE` cautiously from `2048` only if memory remains stable.
- Keep validation enabled (`VAL_SPLIT=0.10`).
- Keep early stopping enabled.

## Quality Gates

- Dataset validation passes.
- Validation loss improves over baseline.
- `best_checkpoint.json` is saved.
- Chat outputs are coherent on fixed prompts.

## Prompt Set (Quick Eval)

Use the same prompts every run:
1. "What can you do for me today?"
2. "Help me plan a focused work block."
3. "Summarize next actions from this context: ..."
4. "Ask me 3 clarifying questions before you propose a plan."

## Artifacts To Keep

- `logs/train/*`
- `logs/system/*`
- `logs/eval/*`
- `logs/runs/*`
- `models/ckpt_*.json`
- `models/latest_checkpoint.json`
- `models/best_checkpoint.json`
