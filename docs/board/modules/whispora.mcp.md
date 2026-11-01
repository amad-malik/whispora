# MCP server  ·  ⚪ not started  ·  phase 3: **Agents**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-3-agents.md) · layer: **Surfaces**

**What it does:** whispora mcp over stdio for Claude Code, Codex and other MCP clients: search transcripts, list and read meetings, summaries and action items, transcribe a file, read and edit vocabulary. Read-mostly; recording asks the user in the app.

| | |
|---|---|
| product | whispora |
| layer | Surfaces |
| language | Rust |
| code | `crates/mcp` (not started) |
| phase | 3 · Agents |
| runs today | ⚪ not started |

## Connections

- uses: [local API](whispora.local_api.md)
- used by: [agent plugins](whispora.agent_plugins.md)

## What we can absorb

- rmcp (official Rust MCP SDK, Apache-2.0)
- anarlog MCP server (MIT)

## Status

- phase: 3 · owner: (none yet) · done when: Claude Code and Codex answer questions about our meetings
