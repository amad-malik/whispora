# Phase 2: Dictation

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md)

**Goal:** Daily-driver dictation into any app

**Done when:** Key-up to text under about 1 s; insertion verified in Chrome, Electron apps and terminals

## New modules

- [dictation](../modules/whispora.dictation.md) — Push-to-talk or toggle, overlay states (recording, processing, result), streaming partial text, per-app mode selection, vocabulary, cleanup, then hand-off to insertion..
- [insertion](../modules/whispora.insertion.md) — Gets text into the focused app reliably: paste with clipboard save and restore, verify it landed through Accessibility, fall back to Accessibility insert or simulated typing, per-app quirks (Chrome, Electron, terminals); never loses text..

## Earlier modules that grow in this phase

- [speech engine (Mac)](../modules/whispora.speech_engine_mac.md)
- [transcription](../modules/whispora.transcription.md)
- [text](../modules/whispora.text.md)
- [file transcription](../modules/whispora.file_transcription.md)
- [Mac app](../modules/whispora.app.md)
- [eval](../modules/whispora.eval.md)
- [distribution](../modules/whispora.distribution.md)

## Status

- ⚪ not started
