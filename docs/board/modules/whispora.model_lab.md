# model lab  ·  ⚪ not started  ·  phase 4: **Own models**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-4-own-models.md) · layer: **Tooling and operations**

**What it does:** Offline pipeline: convert models (Core ML, GGUF), quantize (int8, 4-bit, palettization), fine-tune on team vocabulary and accents (Whisper LoRA, NeMo for Parakeet) on an Azure GPU VM, evaluate, publish builds.

| | |
|---|---|
| product | whispora |
| layer | Tooling and operations |
| language | Python |
| code | `model-lab` (not started) |
| phase | 4 · Own models |
| runs today | ⚪ not started |

## Connections

- uses: [eval](whispora.eval.md)
- used by: [model hosting](whispora.model_hosting.md)

## What we can absorb

- coremltools, NeMo, PEFT
- FluidAudio's conversion scripts

## Status

- phase: 4 · owner: (none yet) · done when: A tuned, quantized build beats the stock model on team audio
