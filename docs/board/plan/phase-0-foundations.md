# Phase 0: Foundations

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md)

**Goal:** Transcribe a file on-device and see it in the app

**Done when:** A 30-minute file transcribes on the Neural Engine; WER and speed measured on fixtures; diarizer chosen with numbers

## New modules

- [contracts](../modules/whispora.contracts.md) — The shared language of the system: domain types (Recording, Track, Segment, Word, Speaker, Meeting, Mode, ModelRef, Trace) and the traits every swappable part implements (AudioSource, Vad, AsrEngine, Diarizer, SpeakerEmbedder, LlmProvider, SttProvider, CalendarSource, TextSink, Store), plus the event bus and error types.
- [config](../modules/whispora.config.md) — Settings, modes and presets, meeting policy, per-app rules and provider choices as files under Application Support; secrets (API keys, OAuth tokens, Hugging Face token) only in the macOS Keychain..
- [store](../modules/whispora.store.md) — Recording folders as the source of truth, a SQLite index with FTS5 search (rusqlite), retention rules, export (Markdown, JSON, SRT/VTT) and migrations.
- [trace](../modules/whispora.trace.md) — Per-recording pipeline trace (stage, engine, model, timings, prompt used) and local structured logs.
- [engine supervisor](../modules/whispora.engine_supervisor.md) — Starts, health-checks and stops the helper processes; framed IPC over a Unix socket (control messages plus PCM frames); version handshake; model load and unload with a keep-alive timer; restart on crash; backpressure..
- [speech engine (Mac)](../modules/whispora.speech_engine_mac.md) — Swift helper running FluidAudio on Core ML and the Neural Engine: Parakeet TDT v2 (English) batch and streaming ASR, Silero VAD, offline and live diarization, per-call speaker embeddings, vocabulary boosting.
- [models](../modules/whispora.models.md) — Model catalog and manager: signed manifest (Ed25519) with source, pinned revision, files, sha256, size, license, gated flag and engine; downloads from models.kaidera.ai or Hugging Face, resumable and verified; install and remove; license policy; generates the acknowledgements page..
- [transcription](../modules/whispora.transcription.md) — Turns a track into timestamped text: VAD silence trimming, chunking, batch or streaming, local agreement for streaming (committed vs provisional words), language selection, hallucination filtering, per-track results merged on one timeline.
- [file transcription](../modules/whispora.file_transcription.md) — Import audio or video files by drag and drop or CLI, transcribe, optionally diarize and summarize, and store like a meeting..
- [FFI bridge](../modules/whispora.ffi.md) — The UniFFI bridge: exposes the core API, events and async calls to Swift; generates Swift bindings; builds the core as an XCFramework..
- [Mac app](../modules/whispora.app.md) — The SwiftUI/AppKit app: menu bar item, main window (Home, Meetings, Library, Models, Settings), meeting view (transcript with speakers, per-segment audio, speaker naming panel, summary), recording indicator, onboarding and permissions, About with acknowledgements; dictation overlay in phase 2..
- [CLI](../modules/whispora.cli.md) — The whispora command: status, transcribe a file, list, search and show meetings, export, start and stop recording, model management..
- [eval](../modules/whispora.eval.md) — Benchmarks and regression gates: word error rate, diarization error rate, speaker count, latency, idle and peak memory and CPU, on fixture audio and consented team recordings (private).
- [distribution](../modules/whispora.distribution.md) — Developer ID signing, notarization, DMG, updates through Sparkle from GitHub Releases, the landing page, the acknowledgements page, release notes..

## Spikes

- Diarizer bake-off: FluidAudio vs sherpa-onnx (pyannote + embeddings) vs speakrs on recorded team calls.
- Start collecting consented team recordings with reference transcripts and speaker labels (stored privately).

## Status

- ⚪ not started
