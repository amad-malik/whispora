# Phase 1: Meetings

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md)

**Goal:** Record a real team call without a bot, see each speaker separately, name them, get a summary

**Done when:** A 3-6 person call: speaker count right, speakers kept apart, names typed in under a minute, summary produced; idle and recording cost within budget

## New modules

- [audio](../modules/whispora.audio.md) — Microphone capture, system-audio capture through Core Audio process taps (excluding Whispora's own sounds), resampling to 16 kHz per track, level meters, ring buffers, archive encoding (Opus/Ogg) and echo handling.
- [platform macOS](../modules/whispora.platform_macos.md) — macOS services behind traits: permission status and System Settings links, global hotkeys (push-to-talk, toggle, modifier-only and Fn), call detection signals (apps holding the mic, known call apps, call windows), Accessibility reads, login item, notifications..
- [calendar](../modules/whispora.calendar.md) — Reads macOS Calendar through EventKit: current and next event, title, attendees (name, email), organizer and call link.
- [local LLM engine](../modules/whispora.llm_engine_local.md) — Rust helper running llama.cpp on Metal with small GGUF models (Qwen3/Qwen3.5, Gemma 4) for cleanup and summaries, fully offline..
- [providers](../modules/whispora.providers.md) — One interface over every way to reach a model.
- [speakers](../modules/whispora.speakers.md) — Tells people apart reliably beyond two: diarizes the them track (and the me track when several people share the mic in a room), counts speakers, keeps a short sample per speaker, and lets the user type a name per speaker for this call with calendar attendees as suggestions; merge and split fixes.
- [text](../modules/whispora.text.md) — Everything after recognition: a deterministic formatter (numbers, dates, punctuation, fillers, spoken punctuation, casing), vocabulary replacements, mode-based LLM transforms for dictation, and meeting outputs (summary, decisions, action items, templates).
- [meetings](../modules/whispora.meetings.md) — The meeting lifecycle: detect a call, notify (default) or auto-start (opt-in), record two tracks, stop manually or when the call ends, run the resumable post-processing job (transcription, speakers, summary), publish the result.

## Earlier modules that grow in this phase

- [speech engine (Mac)](../modules/whispora.speech_engine_mac.md)
- [transcription](../modules/whispora.transcription.md)
- [file transcription](../modules/whispora.file_transcription.md)
- [Mac app](../modules/whispora.app.md)
- [eval](../modules/whispora.eval.md)
- [distribution](../modules/whispora.distribution.md)

## Status

- ⚪ not started
