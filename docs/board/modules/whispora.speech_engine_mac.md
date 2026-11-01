# speech engine (Mac)  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Engines and models**

**What it does:** Swift helper running FluidAudio on Core ML and the Neural Engine: Parakeet TDT v2 (English) batch and streaming ASR, Silero VAD, offline and live diarization, per-call speaker embeddings, vocabulary boosting. Apple SpeechAnalyzer as a no-download fallback.

| | |
|---|---|
| product | whispora |
| layer | Engines and models |
| language | Swift |
| code | `engines/speech-mac` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [models](whispora.models.md)
- used by: (nothing yet)

## What we can absorb

- FluidAudio (Apache-2.0): Parakeet, VAD, diarization on the Neural Engine
- Parakeet TDT v2/v3 weights (CC-BY-4.0, credit NVIDIA)
- Silero VAD (MIT)
- pyannote community-1 (CC-BY-4.0)
- Apple SpeechAnalyzer (macOS 26)

## Status

- phase: 0 · owner: (none yet) · done when: Phase 0: batch ASR + VAD over IPC. Phase 1: diarization and embeddings
