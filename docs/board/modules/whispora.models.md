# models  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Engines and models**

**What it does:** Model catalog and manager: signed manifest (Ed25519) with source, pinned revision, files, sha256, size, license, gated flag and engine; downloads from models.kaidera.ai or Hugging Face, resumable and verified; install and remove; license policy; generates the acknowledgements page.

| | |
|---|---|
| product | whispora |
| layer | Engines and models |
| language | Rust |
| code | `crates/models` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md), [config](whispora.config.md), [store](whispora.store.md)
- used by: [distribution](whispora.distribution.md), [engine supervisor](whispora.engine_supervisor.md), [FFI bridge](whispora.ffi.md), [local LLM engine](whispora.llm_engine_local.md), [speech engine (Mac)](whispora.speech_engine_mac.md)

## What we can absorb

- hf-hub crate
- superwhisper-style signed model list with sha256 (behaviour only)

## Status

- phase: 0 · owner: (none yet) · done when: Phase 0: models install from Hugging Face with verification. Phase 4: from models.kaidera.ai
