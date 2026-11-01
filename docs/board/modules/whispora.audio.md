# audio  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Capture and platform**

**What it does:** Microphone capture, system-audio capture through Core Audio process taps (excluding Whispora's own sounds), resampling to 16 kHz per track, level meters, ring buffers, archive encoding (Opus/Ogg) and echo handling. Two tracks, me and them, never mixed.

| | |
|---|---|
| product | whispora |
| layer | Capture and platform |
| language | Rust |
| code | `crates/audio` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md), [platform macOS](whispora.platform_macos.md)
- used by: [dictation](whispora.dictation.md), [file transcription](whispora.file_transcription.md), [meetings](whispora.meetings.md)

## What we can absorb

- cpal (Apache-2.0) for the microphone
- objc2-core-audio for process taps; cidre usage in Meetily and anarlog (MIT)
- rubato for resampling; opus + ogg crates
- Lesson from Meetily: mixing to mono makes speaker separation impossible

## Status

- phase: 1 · owner: (none yet) · done when: Two clean tracks recorded from a real call; own sounds excluded; echo handled on speakers
