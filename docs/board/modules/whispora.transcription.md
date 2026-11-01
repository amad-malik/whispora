# transcription  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Pipelines**

**What it does:** Turns a track into timestamped text: VAD silence trimming, chunking, batch or streaming, local agreement for streaming (committed vs provisional words), language selection, hallucination filtering, per-track results merged on one timeline. Routes to the local engine or a cloud STT provider.

| | |
|---|---|
| product | whispora |
| layer | Pipelines |
| language | Rust |
| code | `crates/transcription` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [engine supervisor](whispora.engine_supervisor.md), [providers](whispora.providers.md), [contracts](whispora.contracts.md), [trace](whispora.trace.md)
- used by: [dictation](whispora.dictation.md), [eval](whispora.eval.md), [file transcription](whispora.file_transcription.md), [meetings](whispora.meetings.md)

## What we can absorb

- Local agreement streaming policy
- Silence removal before ASR (reduces hallucinations)
- transcribe-rs / Handy patterns (MIT)

## Status

- phase: 0 · owner: (none yet) · done when: Fixture files transcribe with measured WER and speed
