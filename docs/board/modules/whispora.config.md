# config  ·  ⚪ not started  ·  phase 0: **Foundations**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-0-foundations.md) · layer: **Foundation**

**What it does:** Settings, modes and presets, meeting policy, per-app rules and provider choices as files under Application Support; secrets (API keys, OAuth tokens, Hugging Face token) only in the macOS Keychain.

| | |
|---|---|
| product | whispora |
| layer | Foundation |
| language | Rust |
| code | `crates/config` (not started) |
| phase | 0 · Foundations |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md)
- used by: [FFI bridge](whispora.ffi.md), [models](whispora.models.md), [providers](whispora.providers.md), [store](whispora.store.md), [text](whispora.text.md)

## What we can absorb

- Modes as plain files (superwhisper keeps modes as JSON)

## Status

- phase: 0 · owner: (none yet) · done when: Settings round-trip through files; secrets never touch disk outside the Keychain
