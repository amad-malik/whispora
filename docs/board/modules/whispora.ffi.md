# FFI bridge  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Surfaces**

**What it does:** The UniFFI bridge: exposes the core API, events and async calls to Swift; generates Swift bindings; builds the core as an XCFramework.

| | |
|---|---|
| product | whispora |
| layer | Surfaces |
| language | Rust |
| code | `crates/ffi` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [meetings](whispora.meetings.md), [file transcription](whispora.file_transcription.md), [dictation](whispora.dictation.md), [store](whispora.store.md), [config](whispora.config.md), [models](whispora.models.md)
- used by: [Mac app](whispora.app.md)

## What we can absorb

- UniFFI (MPL-2.0)

## Status

- phase: 0 · owner: (none yet) · done when: The Swift app calls the core and receives events
