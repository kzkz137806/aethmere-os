# Aethmere · 识海

> Public distribution repository — **this is not an open-source repository**.

Aethmere is a continuity layer for AI-assisted work. It helps supported AI clients
carry approved context across tasks, devices, and models while keeping user control
and answer boundaries visible.

[Website](https://aethmere.com) ·
[Web app](https://app.aethmere.com) ·
[Latest release](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Report an issue](https://github.com/kzkz137806/aethmere-os/issues)

## What is in this repository

This public repository is the official home for:

- release downloads and checksums;
- installation and update instructions;
- public changelogs;
- issue tracking and security reporting.

The proprietary Aethmere core, private knowledge systems, evaluation material,
service implementation, and internal development history are **not included**.

## Install Aethmere CLI

Requirements: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Expected version:

```text
Aethmere CLI 0.7.0
```

`aethmere connect` creates a user-level connection for supported AI clients. You
do not need to reconnect whenever you change project folders. Local use does not
require a web invitation. Cloud login and synchronization are optional, and
desktop upload remains off until the user enables it.

For a step-by-step Chinese guide, visit
[aethmere.com](https://aethmere.com/#install).

## Verify the download

SHA-256 for `aethmere-cli-0.7.0.tgz`:

```text
964903d1f5787e6fb58dfe37a762d29c966971abd20e06a2b22cdcfe9954a2a6
```

PowerShell:

```powershell
Get-FileHash .\aethmere-cli-0.7.0.tgz -Algorithm SHA256
```

macOS/Linux:

```bash
shasum -a 256 aethmere-cli-0.7.0.tgz
```

The CLI also verifies signed update metadata, package size, and SHA-256 before an
update is installed. Updates are never installed without confirmation.

## Product model

Aethmere uses a public-client/private-core model:

- public distribution and integration entry points;
- proprietary hosted core services;
- downloadable consumer client;
- no public disclosure of the core source code.

The contents of this repository and its release assets are proprietary unless a
file explicitly states otherwise. No open-source license is granted. See
[NOTICE.md](NOTICE.md).

## Hallucination-risk governance

Aethmere treats hallucination risk as a context-lifecycle problem, not only as
a retrieval problem. Its governance layer helps supported AI clients determine
whether context is source-supported, stale, superseded, conflicting, or too
weakly evidenced to reuse safely in an answer.

The governance model includes:

- source-linked context with visible provenance;
- explicit updates, retractions, and temporal-conflict handling;
- checks for false-memory carryover and omitted constraints;
- calibrated abstention when available support is insufficient or conflicting.

These controls are designed to reduce hallucination risk; they do not make any
model infallible. Aethmere publishes quantitative performance claims only when
each result is tied to a named protocol, fixed evaluation set, model, resource
budgets, judge, evaluation date, and reproducible evidence.

The governance engine remains part of Aethmere's proprietary core. Users receive
the signed client and verifiable release artifacts through this repository while
the core source code and private evaluation material remain unpublished.

## Support

Use [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) for public
bug reports and feature requests. Do not include passwords, API keys, private
memories, personal data, or confidential project content.

For security issues, follow [SECURITY.md](SECURITY.md).
