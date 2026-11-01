# dictation  ·  ⚪ not started  ·  phase 2: **Dictation**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-2-dictation.md) · layer: **Features**

**What it does:** Push-to-talk or toggle, overlay states (recording, processing, result), streaming partial text, per-app mode selection, vocabulary, cleanup, then hand-off to insertion.

| | |
|---|---|
| product | whispora |
| layer | Features |
| language | Rust |
| code | `crates/dictation` (not started) |
| phase | 2 · Dictation |
| runs today | ⚪ not started |

## Connections

- uses: [audio](whispora.audio.md), [platform macOS](whispora.platform_macos.md), [transcription](whispora.transcription.md), [text](whispora.text.md), [insertion](whispora.insertion.md), [store](whispora.store.md)
- used by: [FFI bridge](whispora.ffi.md)

## What we can absorb

- Streaming partial text (top user request against Wispr Flow)
- Handy (MIT) dictation flow

## Status

- phase: 2 · owner: (none yet) · done when: Key-up to text under about 1 s with Parakeet
