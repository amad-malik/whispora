# calendar  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Capture and platform**

**What it does:** Reads macOS Calendar through EventKit: current and next event, title, attendees (name, email), organizer and call link. Used for the meeting title, call detection and speaker-name suggestions. No Zoom/Teams/Meet or calendar APIs.

| | |
|---|---|
| product | whispora |
| layer | Capture and platform |
| language | Rust |
| code | `crates/calendar` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md), [platform macOS](whispora.platform_macos.md)
- used by: [meetings](whispora.meetings.md), [speakers](whispora.speakers.md)

## What we can absorb

- objc2-event-kit
- anarlog's Apple Calendar link (MIT)

## Status

- phase: 1 · owner: (none yet) · done when: Meeting title and attendee suggestions appear for a calendar-backed call
