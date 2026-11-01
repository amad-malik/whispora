# file transcription  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Features**

**What it does:** Import audio or video files by drag and drop or CLI, transcribe, optionally diarize and summarize, and store like a meeting.

| | |
|---|---|
| product | whispora |
| layer | Features |
| language | Rust |
| code | `crates/file-transcription` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [audio](whispora.audio.md), [transcription](whispora.transcription.md), [speakers](whispora.speakers.md), [text](whispora.text.md), [store](whispora.store.md)
- used by: [FFI bridge](whispora.ffi.md), [local API](whispora.local_api.md)

## What we can absorb

- Meetily/MacWhisper file import flows

## Status

- phase: 0 · owner: (none yet) · done when: Phase 0: a file becomes a stored transcript. Phase 1: with speakers and summary
