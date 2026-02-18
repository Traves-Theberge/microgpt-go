# GPU Readiness Guide

## Current Status

- Your current Go trainer runs CPU kernels.
- `TRAIN_DEVICE=gpu` is accepted in config, but runtime falls back to CPU.
- Use `go run . gpu-check` to inspect:
  - NVIDIA visibility (`nvidia-smi`)
  - CUDA toolkit visibility (`nvcc --version`)

## For Your Laptop (GTX 1650 Ti)

- Hardware is suitable for small CUDA workloads.
- For this project, GPU acceleration requires a GPU math backend in the trainer code (not just GPU detection).

## Practical Next Step

1. Keep current training on CPU for now.
2. Use `gpu-check` each environment update to confirm driver/toolkit health.
3. Add a dedicated GPU backend phase (separate from current trainer), then wire it behind `TRAIN_DEVICE=gpu`.

## Why This Matters

- A GPU can significantly improve throughput once kernels are implemented.
- Without GPU kernels, selecting `gpu` only changes intent, not execution.
