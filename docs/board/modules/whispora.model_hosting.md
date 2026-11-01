# model hosting  ·  ⚪ not started  ·  phase 4: **Own models**

[← overview](../README.md) · [design](../../superpowers/specs/2026-11-01-whispora-design.md) · [plan](../plan/phase-4-own-models.md) · layer: **Tooling and operations**

**What it does:** models.kaidera.ai: the signed manifest on the website VM, model files in Azure Blob (content-addressed, resumable), key rotation, mirror of upstream files we ship.

| | |
|---|---|
| product | whispora |
| layer | Tooling and operations |
| language | Infra |
| code | `infra/models` (not started) |
| phase | 4 · Own models |
| runs today | ⚪ not started |

## Connections

- uses: [model lab](whispora.model_lab.md)
- used by: (nothing yet)

## What we can absorb

- Azure Blob + signed manifest design from inference-engines research

## Status

- phase: 4 · owner: (none yet) · done when: The app installs a model from models.kaidera.ai and verifies it
