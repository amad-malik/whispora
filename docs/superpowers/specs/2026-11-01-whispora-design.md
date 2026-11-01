# Whispora: design

*Talk naturally. Keep what matters.*

Status: draft for review · 2026-11-01 · author: Claude with Amad Malik
Repo: github.com/amad-malik/whispora (public, Apache-2.0)
Research behind this design (private workspace unless noted):
`misc/stt/docs/research/competitors.md`, `misc/docs/rust-stack.md`,
`misc/docs/inference-engines.md`, `misc/docs/llm-providers.md`,
`misc/stt/recon/superwhisper/` (private, never published).

---

## 1. Intent

Whispora is a local-first speech product for Amad and his team: meeting
recording with named speakers, transcription, dictation into any app, and
access for coding agents (Claude Code, Codex). It does everything the
commercial dictation and meeting apps do, with **no subscription and no
server of ours in the loop**, and it does the things they do badly (speaker
separation beyond two people, naming speakers, staying light) well.

**Why now.** The team uses tools like superwhisper, Meetily and Krisp and hits
their limits: subscriptions, cloud dependence, Meetily distinguishing only one
person, Krisp being heavy on hardware. The open models (Parakeet, pyannote,
Silero, small LLMs) and Apple's Neural Engine now make all of it possible on a
Mac without a cloud.

**Who.** Internal use by the team first. Published as an open-source project
with a landing page that credits every model and library used. Selling may
happen later; nothing is built for it now.

## 2. Goals, non-goals, anti-goals

**Goals**
1. Record any meeting (Zoom, Teams, Meet, Slack, FaceTime, in person) **without
   joining it or integrating with it**: capture the microphone and the system
   audio on the Mac.
2. Tell **how many people spoke and keep them apart** (Speaker 1, 2, 3…), so the
   user can type who each one is for that call. Nice to have, not required:
   read participant names and the active speaker from the call app's window.
3. Accurate on-device transcription by default; cloud only when the user picks a
   provider.
4. Summaries, decisions and action items from the user's own LLM access:
   Claude or ChatGPT/Codex subscriptions, or API keys (Anthropic, OpenAI,
   OpenRouter, Fireworks AI), or local models.
5. Dictation into any app with reliable insertion (phase 2).
6. A local MCP server and plugins so Claude Code and Codex can use transcripts,
   meetings and dictation (phase 3).
7. Our own model builds (quantized, converted, tuned for the team) served from
   models.kaidera.ai (phase 4).
8. Modular: every engine, provider and platform service sits behind a trait, so
   parts can be swapped and the Rust core can later serve Windows and Linux.

**Non-goals (for now)**
- Anything for selling: accounts, billing, licensing, usage metering.
- Meeting bots; CRM sync; team workspaces or shared cloud storage.
- Hosting, proxying or routing model requests for anyone.
- Phone apps; Windows and Linux builds (keep the core portable only).
- Telemetry or analytics of any kind.
- Remembering voices across meetings: no voiceprints are stored. Names are
  typed per call.
- Languages other than English, until a later phase (see roadmap).

**Anti-goals (failure even if it works)**
- Heavy like Krisp: always-on per-frame neural processing, high idle memory.
- Mixing mic and system audio into one stream before transcription (the reason
  Meetily cannot separate people).
- Sending audio, transcripts or screen context to a cloud the user did not pick.
- Breaking the user's clipboard or losing dictated text.
- Publishing the superwhisper reverse-engineering notes or copying its code,
  prompts or assets.

## 3. Constraints and decided choices

Decided by Amad:
- **macOS only**, Apple Silicon (M-series) only, every Mac has more than 8 GB of
  memory. Native **SwiftUI/AppKit** app, built, signed and packaged with Apple
  tools (Apple Developer account, own certificates).
- **English only** for now. Later: Norwegian, Spanish, Dutch, Arabic.
- Speakers are told apart per call and named by the user; nothing remembers
  voices.
- **Rust** for the core. Keep it platform-neutral so Windows/Linux can reuse it.
- **No paid SDKs.** Any free model may be used while Whispora is non-commercial;
  every source is credited. Non-commercial models come out if it is ever sold.
- Subscriptions: **Claude**, **Codex/ChatGPT** (more later). API keys:
  **Anthropic, OpenAI, OpenRouter, Fireworks AI**. No routing service.
- Own model hosting at **models.kaidera.ai** (Azure).
- Accounts available: Apple Developer, Hugging Face, NVIDIA Developer.
- First priority: **transcription and meeting recording**.

Chosen in this design (Claude's calls, open to change):
- Two inference runtimes on the Mac: **FluidAudio** (Apache-2.0, Core ML on the
  Neural Engine) in a Swift helper for speech, VAD, diarization and per-call
  speaker embeddings;
  **llama.cpp** in a Rust helper for local LLMs. Apple SpeechAnalyzer as a
  no-download fallback. transcribe.cpp later for long-tail languages and
  Windows/Linux. MLX and ONNX Runtime are not used on the Mac.
- Engines run as **helper processes**, never inside the app process (native
  libraries clash; killing a helper reliably frees model memory; a crash cannot
  take the app down).
- The Rust core runs **inside the app process** as a library through UniFFI, so
  macOS permissions (microphone, system audio, Accessibility, calendar) belong
  to the signed app.
- The CLI and MCP server never record or run models themselves; they ask the
  running app over a local socket.
- Model tuning is a separate offline **Python** pipeline on a GPU machine; the
  app stays Rust and Swift.
- **Minimum macOS 26** (all team Macs are recent Apple Silicon; gives Apple
  SpeechAnalyzer and the current SwiftUI). English model: Parakeet TDT v2.
- Meeting recording starts only with the user's action or a click on Whispora's
  notification ("Notify" is the default); "Auto" exists as an opt-in setting.
- Calendar comes from macOS Calendar (EventKit) only: the meeting title and the
  attendees as suggestions while typing speaker names. If an account is not in
  macOS Calendar, meetings simply have no suggestions.
- **No App Sandbox.** Distributed with Developer ID and notarization, because
  Whispora needs Accessibility, runs helper processes and launches external
  CLIs (`claude`, `codex`).
- The **Claude subscription** provider ships **opt-in, off by default**, labelled
  "personal use of your own Claude login", and never touches Claude tokens. An
  Anthropic API key is the recommended Claude path, and the only one for shared
  or team-wide use.
- A **diarizer bake-off** runs before phase 1 commits to an engine: FluidAudio
  vs sherpa-onnx (pyannote + embeddings) vs speakrs, on recorded team calls. If
  FluidAudio misses the target, a second runtime is allowed on the Mac for
  diarization.

## 4. How the parts fit

### 4.1 Processes

```text
Whispora.app (signed, one bundle)
├── Swift UI shell  ── UniFFI ──►  Rust core (library, in-process)
│   menu bar, windows,             pipelines, capture, store, providers,
│   overlay, onboarding            engine supervisor, local API socket
│                                        │ framed IPC (Unix socket)
│                    ┌───────────────────┼────────────────────┐
│                    ▼                   ▼                    ▼
│   whispora-speech (Swift helper)  whispora-llm (Rust helper)  [later] whispora-speech-ggml
│   FluidAudio on Neural Engine:    llama.cpp on Metal:          transcribe.cpp (Rust)
│   ASR, VAD, diarization,          cleanup, summaries           long-tail languages,
│   speaker embeddings, vocab boost                              Windows/Linux
└── Contents/MacOS/whispora  (CLI + `whispora mcp`) ── local socket ──► Rust core
```

### 4.2 Module map

```text
SURFACES   app(Swift) · ffi · local_api · cli · mcp · agent_plugins
FEATURES   meetings · file_transcription · dictation* · insertion*
PIPELINES  transcription · speakers · text
ENGINES    engine_supervisor · speech_engine_mac · llm_engine_local · providers · models
CAPTURE    audio · platform_macos · calendar
FOUNDATION contracts · config · store · trace
TOOLING    eval · model_lab · model_hosting · distribution
(* phase 2)
```

Dependencies only point down the stack. Features use pipelines; pipelines use
engines through `contracts` traits; nothing below a layer knows the layer above.

### 4.3 Meeting flow (phase 1)

1. `platform_macos` sees a call start (mic in use by a known call app, or a
   calendar event with a call link starting now). `calendar` supplies title,
   attendees and link.
2. `meetings` asks the user (Off / Notify / Auto, set per user) and starts.
3. `audio` records **two tracks**: "me" = microphone, "them" = system audio
   tap (all call apps, excluding Whispora's own sounds). Both are saved to the
   recording folder; nothing is mixed.
4. Optional live transcript (off by default to stay light).
5. On stop, `meetings` runs the post-processing job:
   `transcription` (VAD trim → ASR per track, timestamps) →
   `speakers` (diarize "them"; also diarize "me" when several voices share the
   mic in a room; count speakers; label them Speaker 1…N) → merged timeline →
   `text` (format, then summary, decisions, action items through the chosen
   `providers` LLM) → `store` with a `trace`.
6. The app shows the meeting: transcript with speakers, audio per segment, a
   speaker panel where the user types who each speaker is (calendar attendees
   offered as suggestions, a short audio sample per speaker), and the summary,
   which updates with the names. Agents read it later through `mcp`.

### 4.4 Dictation flow (phase 2)

Hotkey (`platform_macos`) → `dictation` opens the overlay and records the mic
(`audio`) → streaming ASR through `transcription` (committed and provisional
words shown live) → stop → `text` (formatter, vocabulary, optional LLM cleanup by
mode) → `insertion` (paste with clipboard restore, verify it landed, fall back to
Accessibility insert or simulated typing) → `store` + `trace`.

### 4.5 Data

Per recording, one folder: `audio/me.opus`, `audio/them.opus`,
`transcript.json` (segments with track, speaker id, start/end, words,
confidence), `speakers.json`, `summary.md`, `trace.json` (each stage: engine,
model, duration, prompt used), `meta.json`. SQLite holds the index and FTS5
search; files are the source of truth. Speaker names are stored per recording
only; no voiceprints are kept between meetings.

## 5. Modules

Each module: what it does · owns · uses → used by · phase · notes.
IDs match the board (`whispora.<id>`).

### Foundation

**contracts** — the shared language of the system: domain types (Recording,
Track, Segment, Word, Speaker, Person, Meeting, Mode, ModelRef, Trace) and the
traits every swappable part implements: `AudioSource`, `Vad`, `AsrEngine`,
`Diarizer`, `SpeakerEmbedder`, `LlmProvider`, `SttProvider`, `CalendarSource`,
`TextSink`, `Store`. Also the internal event bus and error types. No platform
code, no I/O.
Uses: nothing → used by: every Rust module. Phase 0.

**config** — settings, modes/presets, meeting policy, per-app rules, provider
choices; stored as files (TOML/JSON) under Application Support; secrets (API
keys, OAuth tokens, HF token) only in the macOS Keychain.
Uses: contracts → used by: all features, providers, app. Phase 0.

**store** — recording folders, SQLite index (rusqlite, FTS5 search), retention
rules, export (Markdown, JSON, SRT/VTT), migrations. Files are the source of
truth; the index can be rebuilt.
Uses: contracts, config → used by: features, mcp, cli, app. Phase 0.

**trace** — per-recording pipeline trace (stage, engine, model, timings, prompt
used) plus local structured logs (`tracing`). Powers the "info" view and
benchmarks. No telemetry, nothing leaves the Mac.
Uses: contracts → used by: everything that runs a stage. Phase 0.

### Capture and platform

**audio** — microphone capture (cpal), system-audio capture through Core Audio
process taps (objc2-core-audio, macOS 14.2+; excludes Whispora's own output),
optional per-app tap, resampling to 16 kHz per track (rubato), level meters,
ring buffers, archive encoding (Opus/Ogg), and echo handling when the user is on
speakers (the "them" track is the reference). Two tracks, never mixed.
Uses: contracts, platform_macos (permissions) → used by: meetings, dictation,
file_transcription (decode). Phase 1 (mic-only decode in phase 0).

**platform_macos** — macOS services behind `contracts` traits: permission status
and deep links to System Settings; global hotkeys (push-to-talk, toggle,
modifier-only/Fn); call detection signals (which apps hold the mic, known call
apps, call windows); Accessibility reads (focused field, selection, call
participant names on screen, later); login item; notifications. objc2 bindings.
Uses: contracts → used by: audio, meetings, dictation, insertion, app. Phase 1.

**calendar** — reads the macOS Calendar (EventKit) so any account the user
already syncs there works (Google, Exchange, iCloud): current/next event, title,
attendees (name, email), organizer, call link. Used for the meeting title,
call detection and name suggestions. No Zoom/Teams/Meet or calendar APIs.
Uses: contracts, platform_macos → used by: meetings, speakers. Phase 1.

### Engines and models

**engine_supervisor** — starts, health-checks and stops helper processes;
framed IPC over a Unix socket (control messages + PCM frames); version
handshake; model load/unload with a keep-alive timer (models leave memory when
idle); restart on crash; backpressure.
Uses: contracts, models, trace → used by: transcription, speakers, text. Phase 0.

**speech_engine_mac** (Swift helper `whispora-speech`) — runs FluidAudio on
Core ML / Neural Engine: Parakeet TDT v3 (multilingual) and v2 (English) batch
and streaming ASR, Silero VAD, offline diarization (pyannote community-1) and
live diarization (LS-EEND/Sortformer), speaker embeddings, vocabulary boosting.
Apple SpeechAnalyzer as a no-download fallback. Speaks the engine IPC protocol.
Uses: models (files on disk) → used by: engine_supervisor. Phase 0 (ASR+VAD),
phase 1 (diarization, embeddings).

**llm_engine_local** (Rust helper `whispora-llm`) — llama.cpp (Metal) running
small GGUF models (Qwen3/Qwen3.5, Gemma 4) for cleanup and summaries, fully
offline. Same IPC protocol.
Uses: models → used by: providers (as the "local" provider). Phase 1.

**providers** — one `LlmProvider` / `SttProvider` interface over every way to
reach a model:
- Subscriptions: **Claude** by running the user's own unmodified `claude` CLI
  in headless mode (no tools, empty MCP config, scratch directory; never reads
  or stores Claude tokens; personal use only, API key recommended for shared
  use); **ChatGPT/Codex** through OpenAI's "Sign in with ChatGPT" (PKCE,
  loopback callback) calling the Responses API directly, with a warm
  `codex app-server` as the fallback for Business/Enterprise plans.
- API keys: **Anthropic** (Messages API), **OpenAI**, **OpenRouter** (OAuth that
  returns a user-owned key), **Fireworks AI** (OpenAI-compatible).
- Local runtimes: Ollama, LM Studio (OpenAI-compatible), and `llm_engine_local`.
- Cloud STT where offered (OpenAI, OpenRouter) as an opt-in alternative to
  local ASR.
Streaming, retries, timeouts, per-call cost estimate in the trace. No proxy.
Uses: contracts, config (Keychain), trace → used by: text, transcription.
Phase 1.

**models** — the model catalog and manager: a signed manifest (Ed25519) listing
each model's source, pinned revision, files, sha256, size, license, gated flag
and which engine runs it; downloads from models.kaidera.ai or directly from
Hugging Face (token only for gated repos), resumable, verified; disk usage;
install/remove; license policy (flags non-commercial models); generates the
acknowledgements page.
Uses: contracts, config, store → used by: engine_supervisor, app, distribution.
Phase 0 (Hugging Face direct), phase 4 (own hosting).

### Pipelines

**transcription** — turns a track into timestamped text: VAD silence trimming,
chunking, batch or streaming mode, "local agreement" for streaming (committed vs
provisional words), language selection, hallucination filtering, per-track
results merged on one timeline. Routes to the local engine or a cloud
`SttProvider`.
Uses: engine_supervisor, providers, contracts, trace → used by: meetings,
file_transcription, dictation. Phase 0.

**speakers** — the differentiator: tell people apart reliably, beyond two.
Diarizes each track that needs it and counts speakers:
1. "me" track = the user, unless several voices share the mic (in-room
   meeting), then it is diarized too;
2. "them" track is diarized into Speaker 1…N, with the count shown up front;
3. keeps a short clean audio sample per speaker so the user can hear who it is;
4. the user types a name per speaker for this call (calendar attendees offered
   as suggestions); merge and split fixes when the diarizer got it wrong;
5. nice to have, later: participant names and the active-speaker highlight read
   from the call app's window (Accessibility) as extra hints.
Never forces a name; nothing is remembered between meetings.
Uses: engine_supervisor, calendar, store, contracts → used by: meetings,
file_transcription. Phase 1.

**text** — everything after recognition: a deterministic formatter (numbers,
dates, punctuation, filler words, spoken punctuation, casing), vocabulary
replacements, mode-based LLM transforms for dictation, and meeting outputs
(summary, decisions, action items, templates). Prompts are our own.
Uses: providers, config, contracts → used by: meetings, dictation,
file_transcription. Phase 1 (meeting outputs), phase 2 (dictation transforms).

### Features

**meetings** — the meeting lifecycle: detect, ask or auto-start (Off / Notify /
Auto), record two tracks, stop (manually or when the call ends), run the
post-processing job, keep it resumable if the app quits, and publish the result.
Also handles meetings without a calendar event and in-person meetings.
Uses: audio, platform_macos, calendar, transcription, speakers, text, store,
trace → used by: app, local_api. Phase 1.

**file_transcription** — import audio or video files (drag and drop, CLI),
transcribe, optionally diarize and summarize, store like a meeting.
Uses: audio (decode), transcription, speakers, text, store → used by: app, cli,
mcp. Phase 0 (basic), phase 1 (speakers, summary).

**dictation** — push-to-talk / toggle, overlay states (recording, processing,
result), streaming partial text, per-app mode selection, vocabulary, cleanup,
then hand-off to insertion.
Uses: audio, platform_macos, transcription, text, insertion, store → used by:
app. Phase 2.

**insertion** — gets text into the focused app reliably: paste with clipboard
save and restore, verify it landed through Accessibility, fall back to
Accessibility insert or simulated typing, per-app quirks list (Chrome,
Electron, terminals), never loses the text (kept in history).
Uses: platform_macos → used by: dictation. Phase 2.

### Surfaces

**ffi** — the UniFFI bridge: exposes the Rust core's API, events and async calls
to Swift; generates the Swift bindings; builds the core as an XCFramework.
Uses: core modules → used by: app. Phase 0.

**app** (Swift) — the Mac app: menu bar item, main window (Home, Meetings,
Library/History, Models, Settings), meeting view (transcript with speakers,
per-segment audio, speaker naming panel, summary), recording indicator,
onboarding and permissions, About with acknowledgements; dictation overlay in
phase 2. Built with BAMA conventions (snapshot mode, self-test, design tokens).
Uses: ffi → used by: the user. Phase 0 shell, grows every phase.

**local_api** — a JSON-RPC server on a Unix socket in the app's container,
owner-only permissions, used by the CLI and MCP; launches the app in the
background if it is not running.
Uses: features, store → used by: cli, mcp. Phase 3 (a minimal one in phase 0
for the CLI).

**cli** — the `whispora` command: status, transcribe a file, list/search/show
meetings, export, start/stop recording, model management.
Uses: local_api → used by: people, scripts, agent_plugins. Phase 0 (transcribe),
phase 3 (full).

**mcp** — `whispora mcp` (rmcp, stdio) for Claude Code, Codex and other MCP
clients: search transcripts, list and read meetings, summaries and action
items, transcribe a file, read and edit vocabulary. Read-mostly; anything that
records asks the user in the app.
Uses: local_api → used by: coding agents. Phase 3.

**agent_plugins** — a Claude Code plugin (skills + hooks) and Codex hooks/config
that register the MCP server, and later voice replies to agent questions and
"done" notifications.
Uses: mcp, cli → used by: coding agents. Phase 3.

### Tooling and operations

**eval** — benchmarks and regression gates: word error rate, diarization error
rate, speaker-naming accuracy, latency, idle and peak memory/CPU; fixture audio
plus consented team recordings; runs in CI and locally.
Uses: transcription, speakers, trace → used by: every phase's "done" check.
Phase 0 (WER, latency), phase 1 (DER, naming).

**model_lab** (Python, offline) — convert models (Core ML, GGUF), quantize
(int8, 4-bit, palettization), fine-tune on team vocabulary and accents (Whisper
LoRA, NeMo for Parakeet) on an Azure GPU VM, evaluate with `eval` metrics,
publish builds.
Uses: eval datasets → used by: model_hosting. Phase 4.

**model_hosting** — models.kaidera.ai: the signed manifest on the website VM,
model files in Azure Blob (content-addressed, resumable), key rotation, mirror
of upstream files we ship.
Uses: model_lab → used by: models. Phase 4.

**distribution** — Developer ID signing, notarization, DMG, updates (Sparkle,
free and open source) from GitHub Releases, the landing page, the
acknowledgements page, release notes.
Uses: models (licenses), app → used by: the team. Phase 0 (signed dev builds),
phase 1 (team installs), phase 5 (landing page).

## 6. Roadmap

| Phase | Goal | Modules | Done when |
|---|---|---|---|
| 0 Foundations | Transcribe a file on-device and see it in the app | contracts, config, store, trace, ffi, engine_supervisor, speech_engine_mac (ASR+VAD), models (HF direct), transcription (batch), file_transcription (basic), cli (transcribe), app (shell), eval (WER, latency), distribution (signed dev builds); spike: diarizer bake-off; start collecting consented team recordings | A 30-min file transcribes on the Neural Engine; WER and speed measured on fixtures; diarizer chosen with numbers |
| 1 Meetings | Record a real team call without a bot, see each speaker separately, name them, get a summary | audio, platform_macos, calendar, speakers, meetings, text (meeting outputs), providers, llm_engine_local, speech_engine_mac (diarization), app (meetings, naming panel), eval (DER, speaker count), distribution (team installs) | A 3–6 person call: speaker count right, speakers kept apart, names typed in under a minute, summary produced; idle and recording cost within budget |
| 2 Dictation | Daily-driver dictation into any app | dictation, insertion, text (dictation transforms), transcription (streaming), app (overlay) | Key-up to text under ~1 s; insertion verified in Chrome, Electron apps, terminals |
| 3 Agents | Claude Code and Codex use Whispora | local_api, cli (full), mcp, agent_plugins | Agents search and read meetings through MCP; plugin installs in one step |
| 4 Own models | Team-tuned, smaller models served by us | model_lab, model_hosting, models (own manifest) | A tuned, quantized build beats the stock model on team audio and ships from models.kaidera.ai |
| 5 Languages | Norwegian, Spanish, Dutch, Arabic | speech_engine_mac (Parakeet v3 covers Spanish and Dutch), a second engine for Norwegian and Arabic (transcribe.cpp with Whisper, or Qwen3-ASR), text (per-language formatting, right-to-left display) | Each language measured with `eval` on team audio |
| 6 Ship and extend | Public project and extras | distribution (landing page), on-device context awareness, command mode, snippets, call-app participant names, Windows/Linux groundwork (transcribe.cpp engine) | Landing page live with full credits |

## 7. Success measures (proposed)

- Diarization error rate under 15% on our 3–6 person calls; speaker count right
  on at least 9 calls in 10.
- On-device word error rate on team audio under 8% (improves in phase 4).
- Post-meeting processing under 10% of the meeting length.
- Idle memory under 150 MB with models unloaded; recording without live
  transcript under 5% CPU.
- No audio, transcript or context leaves the Mac unless a cloud provider is
  chosen for that job.

These are targets that `eval` reports every phase, not hard gates: a miss
triggers a decision (tune, swap a part, or accept), not a stop. Measuring them
needs consented team recordings with reference transcripts and speaker labels,
collected during phases 0–1 (a plan task). They are stored privately, never in
the public repo. Evals that need the Neural Engine run on a team Mac acting as
a self-hosted CI runner; public CI runs only unit and fixture tests.

## 8. Risks

- **Diarization quality locally** for 4+ speakers is unproven everywhere;
  measured with `eval` before we promise it.
- **Echo** when the user is on speakers puts "them" into the mic track; needs
  echo cancellation tested early.
- **System audio permission** cannot be checked in advance; onboarding must
  handle a silent denial.
- **Subscription terms**: Claude subscription use is a grey area beyond
  personal use; API key stays the recommended path for shared use. OpenAI's
  "Sign in with ChatGPT" is designed for local apps.
- **Binding maturity**: some Rust bindings have one maintainer (objc2 crates,
  whisper-rs); pinned versions and thin wrappers limit the blast radius.
- **Model licenses**: CC-BY attribution, NVIDIA and OpenMDW terms, gated
  pyannote repos; tracked per model in the manifest.

## 9. Left to the builder

- Exact crate boundaries and names inside the Cargo workspace.
- IPC encoding (length-prefixed JSON or CBOR control messages plus raw PCM).
- Opus vs FLAC for archived tracks; segment JSON schema details.
- Default models per language and per Mac; keep-alive defaults.
- Echo cancellation method (Apple voice processing vs a dedicated canceller).
- UI layout details, following BAMA's mac-design and ux-design skills.

## 10. Repository layout (proposed)

```text
whispora/
  Cargo.toml                 Rust workspace
  crates/                    contracts, config, store, trace, audio,
                             platform-macos, calendar, engine-supervisor,
                             providers, models, transcription, speakers, text,
                             meetings, file-transcription, dictation, insertion,
                             local-api, ffi, cli (bin: whispora), mcp
  engines/speech-mac/        Swift package, helper executable (FluidAudio)
  engines/llm-local/         Rust helper (llama.cpp)
  app/                       Swift app (BAMA template), links the core XCFramework
  plugins/claude-code/, plugins/codex/
  model-lab/                 Python (offline): convert, quantize, fine-tune, eval
  eval/                      fixtures, harness, reports
  docs/                      design, decisions, acknowledgements
```
