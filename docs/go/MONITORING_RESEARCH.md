# Monitoring Research Notes

This page documents why the training dashboard tracks each metric category.

## Sources

- Charm Harmonica (animation/springs for TUI motion):
  - https://github.com/charmbracelet/harmonica
- Hugging Face: Perplexity of fixed-length models:
  - https://huggingface.co/docs/transformers/en/perplexity
- NVIDIA Nsight Systems Analysis Guide (system/perf metric framing):
  - https://docs.nvidia.com/nsight-systems/AnalysisGuide/index.html

## Metric Rationale

## Core Training Quality

- Train loss:
  - Optimization target. Should trend down.
- Validation loss:
  - Generalization signal on held-out data.
- Generalization gap:
  - `val_loss - train_loss`; detects overfitting drift early.
- Validation perplexity:
  - Human-friendly transform of val loss (`exp(val_loss)`).

## Throughput and Stability

- Steps/sec:
  - Fast feedback on runtime speed.
- Tokens/sec:
  - Better speed comparison when sequence length changes.
- Learning rate:
  - Context needed to explain instability or stalled learning.

## System Health

- CPU %:
  - Confirms compute saturation.
- RAM used:
  - Detects memory pressure before OOM/swap events.
- Process RSS:
  - Tracks trainer memory footprint directly.
- Go heap + Go runtime sys memory:
  - Distinguishes allocator/runtime memory behavior.
- Goroutines + GC count:
  - Detects runtime churn that can reduce throughput.

## Design Notes

- Graphs use larger history buffers to preserve full run context.
- Harmonica spring smoothing improves readability of realtime motion.
- Metric Explorer provides plain-language `what`, `why`, and `how to read`.
