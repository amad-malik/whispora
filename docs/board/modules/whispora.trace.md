# trace  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Foundation**

**What it does:** Per-recording pipeline trace (stage, engine, model, timings, prompt used) and local structured logs. Powers the info view and benchmarks. No telemetry; nothing leaves the Mac.

| | |
|---|---|
| product | whispora |
| layer | Foundation |
| language | Rust |
| code | `crates/trace` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md)
- used by: [engine supervisor](whispora.engine_supervisor.md), [eval](whispora.eval.md), [meetings](whispora.meetings.md), [providers](whispora.providers.md), [transcription](whispora.transcription.md)

## What we can absorb

- tracing crate
- Per-recording stage timings (seen in superwhisper's info panel)

## Status

- phase: 0 · owner: (none yet) · done when: Every pipeline stage writes a trace entry visible in the app
