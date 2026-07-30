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

## Hallucination governance

Aethmere treats hallucination risk as a lifecycle governance problem. The goal
is not merely to produce a plausible answer; it is to release an answer whose
claims, evidence, time context, and memory effects remain accountable.

| Failure surface | Aethmere's governance target |
|---|---|
| Unsupported claims | Require usable evidence or abstain instead of filling gaps |
| Citation mismatch | Keep claims attached to inspectable supporting sources |
| Stale or conflicting facts | Preserve replacement and time context instead of silently reusing an old fact |
| Overstated or incomplete answers | Make missing support visible; do not let confidence hide an evidence gap |
| Memory contamination | Gate durable memory changes by provenance, review, and user control |
| Unsafe external release | Apply a final disclosure and evidence boundary before content leaves the system |

### Evidence standard

Aethmere reports deterministic guard verification separately from end-to-end
model outcomes. Every published result must identify its dataset, model, budget,
judge, scope, and known limitations. A single aggregate score does not replace
failure analysis.

The evaluation scorecard covers answer accuracy, unsupported assertions,
omissions, false-memory resistance, update hallucination, temporal-conflict
handling, abstention calibration, cost, and latency. Cross-system claims require
the same frozen inputs and constraints for every system under test.

Current evidence supports scoped governance controls. A reproducible
cross-system end-to-end evaluation is in progress; results will be published
with the protocol and failure analysis when they satisfy the release gate.

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

## Support

Use [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) for public
bug reports and feature requests. Do not include passwords, API keys, private
memories, personal data, or confidential project content.

For security issues, follow [SECURITY.md](SECURITY.md).
