# eval  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Tooling and operations**

**What it does:** Benchmarks and regression gates: word error rate, diarization error rate, speaker count, latency, idle and peak memory and CPU, on fixture audio and consented team recordings (private). Neural Engine evals run on a team Mac as a self-hosted runner.

| | |
|---|---|
| product | whispora |
| layer | Tooling and operations |
| language | Rust/Python |
| code | `eval` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [transcription](whispora.transcription.md), [speakers](whispora.speakers.md), [trace](whispora.trace.md)
- used by: [model lab](whispora.model_lab.md)

## What we can absorb

- anarlog DER harness (MIT)
- Open ASR Leaderboard method

## Status

- phase: 0 · owner: (none yet) · done when: Reports for every phase; private dataset of consented team calls
