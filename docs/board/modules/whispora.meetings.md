# meetings  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Features**

**What it does:** The meeting lifecycle: detect a call, notify (default) or auto-start (opt-in), record two tracks, stop manually or when the call ends, run the resumable post-processing job (transcription, speakers, summary), publish the result. Also meetings without a calendar event and in-person meetings.

| | |
|---|---|
| product | whispora |
| layer | Features |
| language | Rust |
| code | `crates/meetings` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [audio](whispora.audio.md), [platform macOS](whispora.platform_macos.md), [calendar](whispora.calendar.md), [transcription](whispora.transcription.md), [speakers](whispora.speakers.md), [text](whispora.text.md), [store](whispora.store.md), [trace](whispora.trace.md)
- used by: [FFI bridge](whispora.ffi.md), [local API](whispora.local_api.md)

## What we can absorb

- Bot-free capture as in Granola/anarlog, but fully local
- Separate mic and system channels (superwhisper, anarlog)

## Status

- phase: 1 · owner: (none yet) · done when: A real team call recorded, processed and reviewable without a bot
