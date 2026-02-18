# Usage Guide

## Main Flow

1. Open training hub: `go run ./cmd/mircogpt-tui`
2. Configure variables in `Train` tab (BPE + validation defaults are pre-set)
3. Start training (`s`)
4. Watch runtime + metrics in `Monitor`
5. Review artifacts in `Runs` and `Models`
6. Test checkpoints in `Chat`

## Train Tab Controls

- `j/k` or arrows: move selection
- `e` or `enter`: edit selected value
- `space`: cycle bool/choice values
- `f` (on `DATASET_PATH`): open dataset picker/search
- `1/2/3`: apply presets (`fast`, `balanced`, `max`)
- `s`: start training
- `x`: stop training

Dataset picker:
- Type to filter `.jsonl` paths
- `j/k` move
- `enter` select
- `esc` cancel

## Chat Tab Controls

- `enter`: start typing, then send
- `esc`: exit typing mode
- `pgup` / `pgdown`: scroll conversation
- `home` / `end`: jump to top/bottom
- `p`: toggle checkpoint path edit
- `L`: load latest checkpoint path
- `[` `]`: temperature down/up
- `-` `=`: max new tokens down/up

## Commands

- Train: `go run .`
- Validate dataset: `go run . validate-dataset`
- GPU readiness check: `go run . gpu-check`
- One-shot chat: `go run . chat-once models/latest_checkpoint.json "prompt"`
- Full checks: `./checks.sh`

## Recommended Defaults

- `TOKENIZER=bpe`
- `BPE_ENCODING=cl100k_base`
- `DATASET_PATH=datasets/raw/databricks-dolly-15k.jsonl`
- `TOKEN_VOCAB_SIZE=2048`
- `N_LAYER=1`, `N_EMBD=48`, `N_HEAD=4`, `BLOCK_SIZE=96`
- `NUM_STEPS=800`, `LEARNING_RATE=0.004`
- `VAL_SPLIT=0.10`, `EVAL_INTERVAL=50`, `EARLY_STOP_PATIENCE=8`
- `TRAIN_DEVICE=cpu` (default)

## Monitor Controls

- `left/right`: switch metric category (`All`, `Core`, `Eval`, `System`)
- `up/down`: select metric
- `enter`: focus selected metric graph
- `esc`: exit focus mode
- `pgup/pgdown/home/end`: scroll monitor
- `Metric Explorer` panel explains each selected metric in plain language

## Logs Tab Controls

- `pgup/pgdown/home/end`: scroll live logs
- `c`: clear logs
