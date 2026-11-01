# speakers  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Pipelines**

**What it does:** Tells people apart reliably beyond two: diarizes the them track (and the me track when several people share the mic in a room), counts speakers, keeps a short sample per speaker, and lets the user type a name per speaker for this call with calendar attendees as suggestions; merge and split fixes. Nothing is remembered between meetings. Later: names and active speaker read from the call window.

| | |
|---|---|
| product | whispora |
| layer | Pipelines |
| language | Rust |
| code | `crates/speakers` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [engine supervisor](whispora.engine_supervisor.md), [calendar](whispora.calendar.md), [store](whispora.store.md), [contracts](whispora.contracts.md)
- used by: [eval](whispora.eval.md), [file transcription](whispora.file_transcription.md), [meetings](whispora.meetings.md)

## What we can absorb

- anarlog's diarization pipeline and DER harness (MIT)
- FluidAudio diarizer; sherpa-onnx and speakrs as bake-off candidates
- Meetily issue #230: why pyannote-rs fell short

## Status

- phase: 1 · owner: (none yet) · done when: Speaker count right on 9 calls in 10; DER under 15% on 3-6 person calls
