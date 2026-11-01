# engine supervisor  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Engines and models**

**What it does:** Starts, health-checks and stops the helper processes; framed IPC over a Unix socket (control messages plus PCM frames); version handshake; model load and unload with a keep-alive timer; restart on crash; backpressure.

| | |
|---|---|
| product | whispora |
| layer | Engines and models |
| language | Rust |
| code | `crates/engine-supervisor` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md), [models](whispora.models.md), [trace](whispora.trace.md)
- used by: [speakers](whispora.speakers.md), [transcription](whispora.transcription.md)

## What we can absorb

- Process-per-engine design because ggml symbols clash in one binary

## Status

- phase: 0 · owner: (none yet) · done when: Helpers start on demand, unload when idle and recover from a forced crash
