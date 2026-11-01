# text  ·  ⚪ not started  ·  phase 1: **Meetings**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-1-meetings.md) · layer: **Pipelines**

**What it does:** Everything after recognition: a deterministic formatter (numbers, dates, punctuation, fillers, spoken punctuation, casing), vocabulary replacements, mode-based LLM transforms for dictation, and meeting outputs (summary, decisions, action items, templates). Prompts are our own.

| | |
|---|---|
| product | whispora |
| layer | Pipelines |
| language | Rust |
| code | `crates/text` (not started) |
| phase | 1 · Meetings |
| runs today | ⚪ not started |

## Connections

- uses: [providers](whispora.providers.md), [config](whispora.config.md), [contracts](whispora.contracts.md)
- used by: [dictation](whispora.dictation.md), [file transcription](whispora.file_transcription.md), [meetings](whispora.meetings.md)

## What we can absorb

- text-processing-rs (Apache-2.0) for inverse text normalization
- Rule-based cleanup before any LLM (seen in superwhisper)

## Status

- phase: 1 · owner: (none yet) · done when: Meeting summary with decisions and action items from every provider
