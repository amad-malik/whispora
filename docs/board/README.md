# Whispora on the board

*Talk naturally. Keep what matters.*

Local-first meeting recording, transcription and dictation for macOS. Rust core, native SwiftUI app, no subscription.

- Design: [2026-11-01-whispora-design.md](../superpowers/specs/2026-11-01-whispora-design.md)
- Architecture map: [map.excalidraw](map.excalidraw) · [map.svg](map.svg) · [map.mmd](map.mmd)
- Status: [status.yaml](status.yaml)

## Phases

0. [Foundations](plan/phase-0-foundations.md): Transcribe a file on-device and see it in the app
1. [Meetings](plan/phase-1-meetings.md): Record a real team call without a bot, see each speaker separately, name them, get a summary
2. [Dictation](plan/phase-2-dictation.md): Daily-driver dictation into any app
3. [Agents](plan/phase-3-agents.md): Claude Code and Codex use Whispora
4. [Own models](plan/phase-4-own-models.md): Team-tuned, smaller models served from models.kaidera.ai
5. [Languages](plan/phase-5-languages.md): Norwegian, Spanish, Dutch and Arabic
6. [Ship and extend](plan/phase-6-ship.md): Public project, landing page and extras

## Modules by layer

### Surfaces

- [FFI bridge](modules/whispora.ffi.md) · phase 0 · Rust — The UniFFI bridge: exposes the core API, events and async calls to Swift; generates Swift bindings; builds the core as an XCFramework..
- [Mac app](modules/whispora.app.md) · phase 0 · Swift — The SwiftUI/AppKit app: menu bar item, main window (Home, Meetings, Library, Models, Settings), meeting view (transcript with speakers, per-segment audio, speaker naming panel, summary), recording indicator, onboarding and permissions, About with acknowledgements; dictation overlay in phase 2..
- [local API](modules/whispora.local_api.md) · phase 3 · Rust — JSON-RPC on a Unix socket in the app's container, owner-only permissions, used by the CLI and MCP; launches the app in the background when needed..
- [CLI](modules/whispora.cli.md) · phase 0 · Rust — The whispora command: status, transcribe a file, list, search and show meetings, export, start and stop recording, model management..
- [MCP server](modules/whispora.mcp.md) · phase 3 · Rust — whispora mcp over stdio for Claude Code, Codex and other MCP clients: search transcripts, list and read meetings, summaries and action items, transcribe a file, read and edit vocabulary.
- [agent plugins](modules/whispora.agent_plugins.md) · phase 3 · Markdown/Shell — A Claude Code plugin (skills and hooks) and Codex hooks/config that register the MCP server; later voice replies to agent questions and done notifications..

### Features

- [meetings](modules/whispora.meetings.md) · phase 1 · Rust — The meeting lifecycle: detect a call, notify (default) or auto-start (opt-in), record two tracks, stop manually or when the call ends, run the resumable post-processing job (transcription, speakers, summary), publish the result.
- [file transcription](modules/whispora.file_transcription.md) · phase 0 · Rust — Import audio or video files by drag and drop or CLI, transcribe, optionally diarize and summarize, and store like a meeting..
- [dictation](modules/whispora.dictation.md) · phase 2 · Rust — Push-to-talk or toggle, overlay states (recording, processing, result), streaming partial text, per-app mode selection, vocabulary, cleanup, then hand-off to insertion..
- [insertion](modules/whispora.insertion.md) · phase 2 · Rust — Gets text into the focused app reliably: paste with clipboard save and restore, verify it landed through Accessibility, fall back to Accessibility insert or simulated typing, per-app quirks (Chrome, Electron, terminals); never loses text..

### Pipelines

- [transcription](modules/whispora.transcription.md) · phase 0 · Rust — Turns a track into timestamped text: VAD silence trimming, chunking, batch or streaming, local agreement for streaming (committed vs provisional words), language selection, hallucination filtering, per-track results merged on one timeline.
- [speakers](modules/whispora.speakers.md) · phase 1 · Rust — Tells people apart reliably beyond two: diarizes the them track (and the me track when several people share the mic in a room), counts speakers, keeps a short sample per speaker, and lets the user type a name per speaker for this call with calendar attendees as suggestions; merge and split fixes.
- [text](modules/whispora.text.md) · phase 1 · Rust — Everything after recognition: a deterministic formatter (numbers, dates, punctuation, fillers, spoken punctuation, casing), vocabulary replacements, mode-based LLM transforms for dictation, and meeting outputs (summary, decisions, action items, templates).

### Engines and models

- [engine supervisor](modules/whispora.engine_supervisor.md) · phase 0 · Rust — Starts, health-checks and stops the helper processes; framed IPC over a Unix socket (control messages plus PCM frames); version handshake; model load and unload with a keep-alive timer; restart on crash; backpressure..
- [speech engine (Mac)](modules/whispora.speech_engine_mac.md) · phase 0 · Swift — Swift helper running FluidAudio on Core ML and the Neural Engine: Parakeet TDT v2 (English) batch and streaming ASR, Silero VAD, offline and live diarization, per-call speaker embeddings, vocabulary boosting.
- [local LLM engine](modules/whispora.llm_engine_local.md) · phase 1 · Rust — Rust helper running llama.cpp on Metal with small GGUF models (Qwen3/Qwen3.5, Gemma 4) for cleanup and summaries, fully offline..
- [providers](modules/whispora.providers.md) · phase 1 · Rust — One interface over every way to reach a model.
- [models](modules/whispora.models.md) · phase 0 · Rust — Model catalog and manager: signed manifest (Ed25519) with source, pinned revision, files, sha256, size, license, gated flag and engine; downloads from models.kaidera.ai or Hugging Face, resumable and verified; install and remove; license policy; generates the acknowledgements page..

### Capture and platform

- [audio](modules/whispora.audio.md) · phase 1 · Rust — Microphone capture, system-audio capture through Core Audio process taps (excluding Whispora's own sounds), resampling to 16 kHz per track, level meters, ring buffers, archive encoding (Opus/Ogg) and echo handling.
- [platform macOS](modules/whispora.platform_macos.md) · phase 1 · Rust — macOS services behind traits: permission status and System Settings links, global hotkeys (push-to-talk, toggle, modifier-only and Fn), call detection signals (apps holding the mic, known call apps, call windows), Accessibility reads, login item, notifications..
- [calendar](modules/whispora.calendar.md) · phase 1 · Rust — Reads macOS Calendar through EventKit: current and next event, title, attendees (name, email), organizer and call link.

### Foundation

- [contracts](modules/whispora.contracts.md) · phase 0 · Rust — The shared language of the system: domain types (Recording, Track, Segment, Word, Speaker, Meeting, Mode, ModelRef, Trace) and the traits every swappable part implements (AudioSource, Vad, AsrEngine, Diarizer, SpeakerEmbedder, LlmProvider, SttProvider, CalendarSource, TextSink, Store), plus the event bus and error types.
- [config](modules/whispora.config.md) · phase 0 · Rust — Settings, modes and presets, meeting policy, per-app rules and provider choices as files under Application Support; secrets (API keys, OAuth tokens, Hugging Face token) only in the macOS Keychain..
- [store](modules/whispora.store.md) · phase 0 · Rust — Recording folders as the source of truth, a SQLite index with FTS5 search (rusqlite), retention rules, export (Markdown, JSON, SRT/VTT) and migrations.
- [trace](modules/whispora.trace.md) · phase 0 · Rust — Per-recording pipeline trace (stage, engine, model, timings, prompt used) and local structured logs.

### Tooling and operations

- [eval](modules/whispora.eval.md) · phase 0 · Rust/Python — Benchmarks and regression gates: word error rate, diarization error rate, speaker count, latency, idle and peak memory and CPU, on fixture audio and consented team recordings (private).
- [model lab](modules/whispora.model_lab.md) · phase 4 · Python — Offline pipeline: convert models (Core ML, GGUF), quantize (int8, 4-bit, palettization), fine-tune on team vocabulary and accents (Whisper LoRA, NeMo for Parakeet) on an Azure GPU VM, evaluate, publish builds..
- [model hosting](modules/whispora.model_hosting.md) · phase 4 · Infra — models.kaidera.ai: the signed manifest on the website VM, model files in Azure Blob (content-addressed, resumable), key rotation, mirror of upstream files we ship..
- [distribution](modules/whispora.distribution.md) · phase 0 · Shell/Swift — Developer ID signing, notarization, DMG, updates through Sparkle from GitHub Releases, the landing page, the acknowledgements page, release notes..

This folder is the source of truth for Whispora on board.kaidera.ai. It is synced to the board automatically from GitHub.
