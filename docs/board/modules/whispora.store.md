# store  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Foundation**

**What it does:** Recording folders as the source of truth, a SQLite index with FTS5 search (rusqlite), retention rules, export (Markdown, JSON, SRT/VTT) and migrations. The index can always be rebuilt from the folders.

| | |
|---|---|
| product | whispora |
| layer | Foundation |
| language | Rust |
| code | `crates/store` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md), [config](whispora.config.md)
- used by: [dictation](whispora.dictation.md), [FFI bridge](whispora.ffi.md), [file transcription](whispora.file_transcription.md), [local API](whispora.local_api.md), [meetings](whispora.meetings.md), [models](whispora.models.md), [speakers](whispora.speakers.md)

## What we can absorb

- rusqlite (MIT) with bundled FTS5
- Per-recording folders + index pattern seen in superwhisper and anarlog

## Status

- phase: 0 · owner: (none yet) · done when: Create, search, export and rebuild-index work on fixture recordings
