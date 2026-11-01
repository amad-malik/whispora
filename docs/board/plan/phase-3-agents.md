# Phase 3: Agents

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md)

**Goal:** Claude Code and Codex use Whispora

**Done when:** Agents search and read meetings through MCP; plugin installs in one step

## New modules

- [local API](../modules/whispora.local_api.md) — JSON-RPC on a Unix socket in the app's container, owner-only permissions, used by the CLI and MCP; launches the app in the background when needed..
- [MCP server](../modules/whispora.mcp.md) — whispora mcp over stdio for Claude Code, Codex and other MCP clients: search transcripts, list and read meetings, summaries and action items, transcribe a file, read and edit vocabulary.
- [agent plugins](../modules/whispora.agent_plugins.md) — A Claude Code plugin (skills and hooks) and Codex hooks/config that register the MCP server; later voice replies to agent questions and done notifications..

## Status

- ⚪ not started
