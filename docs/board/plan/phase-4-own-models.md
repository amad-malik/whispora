# Phase 4: Own models

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md)

**Goal:** Team-tuned, smaller models served from models.kaidera.ai

**Done when:** A tuned, quantized build beats the stock model on team audio and ships from models.kaidera.ai

## New modules

- [model lab](../modules/whispora.model_lab.md) — Offline pipeline: convert models (Core ML, GGUF), quantize (int8, 4-bit, palettization), fine-tune on team vocabulary and accents (Whisper LoRA, NeMo for Parakeet) on an Azure GPU VM, evaluate, publish builds..
- [model hosting](../modules/whispora.model_hosting.md) — models.kaidera.ai: the signed manifest on the website VM, model files in Azure Blob (content-addressed, resumable), key rotation, mirror of upstream files we ship..

## Status

- ⚪ not started
