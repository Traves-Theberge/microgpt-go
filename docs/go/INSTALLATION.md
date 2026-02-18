# Installation

## Requirements

- Go `1.25+`
- Linux/macOS terminal
- `jq` (used by `./checks.sh`)

## Setup

```bash
cd /home/traves/Development/6.GroupProjects/microgpt/go
go mod tidy
./checks.sh
```

## First Run

```bash
go run ./cmd/mircogpt-tui
```

## Included Dataset

- Active dataset: `datasets/raw/databricks-dolly-15k.jsonl`
- Source snapshot: `datasets/raw/databricks-dolly-15k.jsonl`

## Optional GPU Check

Run:

```bash
go run . gpu-check
```

This verifies NVIDIA visibility (`nvidia-smi`) and CUDA toolkit presence (`nvcc`), then reports current trainer status (CPU kernels).

## Version

- Current project version: `0.0.4`
- Changelog: `../../go/CHANGELOG.md`
