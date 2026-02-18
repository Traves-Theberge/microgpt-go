# microgpt-go

Version: `0.0.4`

`microgpt-go` is a compact Go training project for building a conversational assistant model from JSONL data, with a unified TUI (`MircoGPT-tui`) for configuration, training, monitoring, artifacts, and chat testing.

## Highlights

- BPE-first tokenization (`cl100k_base`) with offline loader
- Validation split + early stopping + best checkpoint saving
- Improved decoding controls (`top-k`, `top-p`, repetition penalty, min new tokens)
- Unified training dashboard with live metrics, eval tracking, and run artifacts
- Chat testing panel with better wrapping and scroll controls
- Structured run folders (`train`, `system`, `eval`, `runs`) + manifest files
- Auto checkpoint naming by run tag (`ckpt_<run>_step####_valloss#.json`)

## Quick Start

```bash
cd /home/traves/Development/6.GroupProjects/microgpt/go
go mod tidy
./checks.sh
go run ./cmd/mircogpt-tui
```

## Current Defaults (0.0.4)

- `TOKENIZER=bpe`
- `BPE_ENCODING=cl100k_base`
- `DATASET_PATH=datasets/raw/databricks-dolly-15k.jsonl`
- `TOKEN_VOCAB_SIZE=2048`
- `N_LAYER=1`, `N_EMBD=48`, `N_HEAD=4`, `BLOCK_SIZE=96`
- `NUM_STEPS=800`, `LEARNING_RATE=0.004`
- `VAL_SPLIT=0.10`, `EVAL_INTERVAL=50`, `EARLY_STOP_PATIENCE=8`
- `TRAIN_DEVICE=cpu`

## Dataset

Active training dataset:
- `datasets/raw/databricks-dolly-15k.jsonl`

Source snapshot:
- `datasets/raw/databricks-dolly-15k.jsonl`

Validate dataset:

```bash
go run . validate-dataset
```

## TUI Controls

Global:
- `tab` / `shift+tab` (`l` / `h`) switch tabs
- `s` start training
- `x` stop training
- `r` refresh lists
- `c` clear current logs/chat
- `q` quit
- In `Train`, select `DATASET_PATH` and press `f` to open dataset picker/search
- Bottom command bar is context-aware by tab (global + tab-specific hints)

Chat:
- `enter` start typing, then send
- `esc` exit typing
- `pgup` / `pgdown` scroll history
- `home` / `end` jump top/bottom
- `p` edit checkpoint path
- `L` load `models/latest_checkpoint.json`
- `[` `]` temperature down/up
- `-` `=` max tokens down/up

Dataset picker:
- Type to filter dataset files
- `j/k` move selection
- `enter` apply selected path
- `esc` cancel

Monitor:
- `left` / `right` switch metric category (`All`, `Core`, `Eval`, `System`)
- `up` / `down` focus a metric in the explorer panel
- `pgup` / `pgdown` / `home` / `end` scroll monitor content
- `enter` toggle full graph focus for selected metric
- `esc` exit focused graph view
- Metric Explorer explains each metric in plain language:
  - what it is
  - why it matters
  - how to interpret it

Logs:
- `Logs` tab shows live streaming logs
- `pgup` / `pgdown` / `home` / `end` scroll logs
- `c` clears the live log panel

## CLI

Train:

```bash
go run .
```

Validate dataset:

```bash
go run . validate-dataset
```

GPU readiness check:

```bash
go run . gpu-check
```

One-shot inference:

```bash
go run . chat-once models/latest_checkpoint.json "Help me prioritize my day"
```

## Artifacts

Logs:
- `logs/train/tui_train_<...>.log`
- `logs/system/tui_system_metrics_<...>.csv`
- `logs/eval/tui_eval_metrics_<...>.csv`
- `logs/runs/run_<...>.txt`
- `logs/train_latest.log`

Models:
- `models/ckpt_<run-tag>_step<steps>_valloss<loss>.json`
- `models/latest_checkpoint.json`
- `models/best_checkpoint.json`

## Monitor Coverage

The `Monitor` tab now tracks and visualizes:
- Train loss history
- Validation loss history
- Generalization gap (`val_loss - train_loss`)
- Throughput (steps/sec and tokens/sec)
- CPU usage
- RAM used (system)
- RSS used (trainer process)
- Learning rate
- Validation perplexity

Each graph keeps full in-run history (not just a short window), and eval snapshots are written to `logs/eval/` for post-run analysis.
Graph motion smoothing uses `harmonica` springs in the TUI runtime.

## Notes

- `TOKEN_VOCAB_SIZE=2048` is the laptop-safe default.
- Training remains CPU-first. `TRAIN_DEVICE=gpu` is accepted but currently falls back to CPU kernels.

## Docs

- `../docs/go/README.md`
- `../docs/go/TRAINING_HUB_GUIDE.md`
- `../docs/go/GPU_READINESS.md`
- `../docs/go/MONITORING_RESEARCH.md`
- `CHANGELOG.md`
