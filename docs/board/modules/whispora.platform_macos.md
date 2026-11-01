# platform macOS  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Capture and platform**

**What it does:** macOS services behind traits: permission status and System Settings links, global hotkeys (push-to-talk, toggle, modifier-only and Fn), call detection signals (apps holding the mic, known call apps, call windows), Accessibility reads, login item, notifications.

| | |
|---|---|
| product | whispora |
| layer | Capture and platform |
| language | Rust |
| code | `crates/platform-macos` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md)
- used by: [audio](whispora.audio.md), [calendar](whispora.calendar.md), [dictation](whispora.dictation.md), [insertion](whispora.insertion.md), [meetings](whispora.meetings.md)

## What we can absorb

- objc2 + objc2-* crates (MIT)
- handy-keys (Fn and modifier-only hotkeys)
- Handy (MIT) for hotkey and paste patterns

## Status

- phase: 1 · owner: (none yet) · done when: Permissions shown and deep-linked; call start detected for Zoom, Teams, Meet, Slack, FaceTime
