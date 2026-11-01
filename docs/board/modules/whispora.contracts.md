# contracts  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Foundation**

**What it does:** The shared language of the system: domain types (Recording, Track, Segment, Word, Speaker, Meeting, Mode, ModelRef, Trace) and the traits every swappable part implements (AudioSource, Vad, AsrEngine, Diarizer, SpeakerEmbedder, LlmProvider, SttProvider, CalendarSource, TextSink, Store), plus the event bus and error types. No platform code, no I/O.

| | |
|---|---|
| product | whispora |
| layer | Foundation |
| language | Rust |
| code | `crates/contracts` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: (nothing)
- used by: [audio](whispora.audio.md), [calendar](whispora.calendar.md), [config](whispora.config.md), [engine supervisor](whispora.engine_supervisor.md), [models](whispora.models.md), [platform macOS](whispora.platform_macos.md), [providers](whispora.providers.md), [speakers](whispora.speakers.md), [store](whispora.store.md), [text](whispora.text.md), [trace](whispora.trace.md), [transcription](whispora.transcription.md)

## What we can absorb

- Trait seams like superwhisper's engine/runner split (behaviour only)

## Status

- phase: 0 · owner: (none yet) · done when: Every other crate compiles against these types; traits have mock implementations for tests
