# local LLM engine  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Engines and models**

**What it does:** Rust helper running llama.cpp on Metal with small GGUF models (Qwen3/Qwen3.5, Gemma 4) for cleanup and summaries, fully offline.

| | |
|---|---|
| product | whispora |
| layer | Engines and models |
| language | Rust |
| code | `engines/llm-local` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [models](whispora.models.md)
- used by: (nothing yet)

## What we can absorb

- llama-cpp-2 crate (MIT/Apache)
- Qwen3 / Gemma 4 small models (Apache-2.0)

## Status

- phase: 1 · owner: (none yet) · done when: A meeting summary is produced offline with a local model
