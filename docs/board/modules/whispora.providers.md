# providers  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Engines and models**

**What it does:** One interface over every way to reach a model. Subscriptions: Claude via the user's own unmodified claude CLI (opt-in, personal use), ChatGPT/Codex via Sign in with ChatGPT (codex app-server fallback). API keys: Anthropic, OpenAI, OpenRouter, Fireworks AI. Local runtimes: Ollama, LM Studio, the local engine. Cloud STT where offered. Streaming, retries, cost estimate in the trace. No proxy.

| | |
|---|---|
| product | whispora |
| layer | Engines and models |
| language | Rust |
| code | `crates/providers` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [contracts](whispora.contracts.md), [config](whispora.config.md), [trace](whispora.trace.md)
- used by: [text](whispora.text.md), [transcription](whispora.transcription.md)

## What we can absorb

- reqwest + SSE parsing for Anthropic Messages
- async-openai for OpenAI-compatible APIs
- OpenRouter OAuth (user-owned key)
- claude -p headless flags; codex app-server protocol

## Status

- phase: 1 · owner: (none yet) · done when: Each provider passes the same cleanup and summary tests
