# local API  ·  ⚪ not started  ·  phase 3: **Agents**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-3-agents.md) · layer: **Surfaces**

**What it does:** JSON-RPC on a Unix socket in the app's container, owner-only permissions, used by the CLI and MCP; launches the app in the background when needed.

| | |
|---|---|
| product | whispora |
| layer | Surfaces |
| language | Rust |
| code | `crates/local-api` (not started) |
| phase | 3 · Agents |
| runs today | ⚪ not started |

## Connections

- uses: [meetings](whispora.meetings.md), [file transcription](whispora.file_transcription.md), [store](whispora.store.md)
- used by: [CLI](whispora.cli.md), [MCP server](whispora.mcp.md)

## What we can absorb

- BAMA agent-cli pattern: the app binary is the CLI, commands run in the app

## Status

- phase: 3 · owner: (none yet) · done when: CLI and MCP reach the running app; a minimal socket exists from phase 0 for the CLI
