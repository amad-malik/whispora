# insertion  ·  ⚪ not started  ·  phase 2: **Dictation**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-2-dictation.md) · layer: **Features**

**What it does:** Gets text into the focused app reliably: paste with clipboard save and restore, verify it landed through Accessibility, fall back to Accessibility insert or simulated typing, per-app quirks (Chrome, Electron, terminals); never loses text.

| | |
|---|---|
| product | whispora |
| layer | Features |
| language | Rust |
| code | `crates/insertion` (not started) |
| phase | 2 · Dictation |
| runs today | ⚪ not started |

## Connections

- uses: [platform macOS](whispora.platform_macos.md)
- used by: [dictation](whispora.dictation.md)

## What we can absorb

- VoiceInk issues #1000 and #1024 (what breaks)
- Clipboard restore and simulated typing (seen in superwhisper)

## Status

- phase: 2 · owner: (none yet) · done when: Insertion verified in Chrome, Electron apps and terminals with the clipboard intact
